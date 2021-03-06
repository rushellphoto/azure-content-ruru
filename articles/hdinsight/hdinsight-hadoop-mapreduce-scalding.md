<properties
 pageTitle="Разработка заданий Scalding MapReduce с помощью Maven | Microsoft Azure"
 description="Узнайте, как использовать Maven для создания задания Scalding MapReduce, а затем развернуть и запустить задание на Hadoop в кластере HDInsight."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="paulettm"
 editor="cgronlun"
	tags="azure-portal"/>
<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="08/02/2016"
 ms.author="larryfr"/>

# Разработка заданий Scalding MapReduce с помощью Apache Hadoop в HDInsight

Scalding – это библиотека Scala, которая позволяет легко создавать задания Hadoop MapReduce. Она предлагает сокращенный синтаксис, а также тесную интеграцию со Scala.

В этом документе рассматривается использование Maven для создания задания MapReduce по базовому подсчету слов, написанного на Scalding. Затем вы узнаете, как развернуть и выполнить это задание в кластере HDInsight.

## Предварительные требования

- **Подписка Azure.**. См. [Бесплатная пробная версия Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Hadoop на основе Windows или Linux в кластере HDInsight**. Дополнительные сведения см. в разделе [Подготовка Hadoop на основе Linux в HDInsight](hdinsight-hadoop-provision-linux-clusters.md) или [Подготовка Hadoop на основе Windows в HDInsight](hdinsight-provision-clusters.md).

* **[Maven](http://maven.apache.org/)**

* **[Пакет JDK для платформы Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html) версии 7 или более поздней**

## Создание и сборка проекта

1. Используйте следующую команду для создания нового проекта Maven:

        mvn archetype:generate -DgroupId=com.microsoft.example -DartifactId=scaldingwordcount -DarchetypeGroupId=org.scala-tools.archetypes -DarchetypeArtifactId=scala-archetype-simple -DinteractiveMode=false

    Эта команда создаст новый каталог с именем **scaldingwordcount**, а также создаст оболочку для приложения Scala.

2. В каталоге **scaldingwordcount** откройте файл **pom.xml** и замените содержимое следующим:

        <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
            <modelVersion>4.0.0</modelVersion>
            <groupId>com.microsoft.example</groupId>
            <artifactId>scaldingwordcount</artifactId>
            <version>1.0-SNAPSHOT</version>
            <name>${project.artifactId}</name>
            <properties>
            <maven.compiler.source>1.6</maven.compiler.source>
            <maven.compiler.target>1.6</maven.compiler.target>
            <encoding>UTF-8</encoding>
            </properties>
            <repositories>
            <repository>
                <id>conjars</id>
                <url>http://conjars.org/repo</url>
            </repository>
            <repository>
                <id>maven-central</id>
                <url>http://repo1.maven.org/maven2</url>
            </repository>
            </repositories>
            <dependencies>
            <dependency>
                <groupId>com.twitter</groupId>
                <artifactId>scalding-core_2.11</artifactId>
                <version>0.13.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-core</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
            </dependencies>
            <build>
            <sourceDirectory>src/main/scala</sourceDirectory>
            <plugins>
                <plugin>
                <groupId>org.scala-tools</groupId>
                <artifactId>maven-scala-plugin</artifactId>
                <version>2.15.2</version>
                <executions>
                    <execution>
                    <id>scala-compile-first</id>
                    <phase>process-resources</phase>
                    <goals>
                        <goal>add-source</goal>
                        <goal>compile</goal>
                    </goals>
                    </execution>
                </executions>
                </plugin>
                <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                    </transformer>
                    </transformers>
                    <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                        <exclude>META-INF/*.SF</exclude>
                        <exclude>META-INF/*.DSA</exclude>
                        <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>com.twitter.scalding.Tool</mainClass>
                        </transformer>
                        </transformers>
                    </configuration>
                    </execution>
                </executions>
                </plugin>
            </plugins>
            </build>
        </project>

    Этот файл описывает проект, зависимости и подключаемые модули. Ниже перечислены важные записи.

    * **maven.compiler.source** и **maven.compiler.target**: задает версию Java для этого проекта

    * **repositories**: репозитории, которые содержат файлы зависимостей, используемые данным проектом

    * **scalding-core\_2.11** и **hadoop-core**: этот проект зависит от основных пакетов Scalding и Hadoop

    * **maven-scala-plugin**: подключаемый модуль для компиляции приложений Scala

    * **maven-shade-plugin**: подключаемый модуль для создания затененных JAR-файлов (fat). Этот подключаемый модуль применяет фильтры и преобразования; а именно:

        * **Фильтры**: примененные фильтры изменяют метаданные, включенные в состав jar-файла. Чтобы предотвратить исключения подписи во время выполнения, обеспечивает исключение различных файлов подписей, которые могли быть включены с зависимостями.

        * **Выполнения**: конфигурация выполнения этапа пакета задает класс **com.twitter.scalding.Tool** как основной класс для пакета. Без этого потребовалось бы указать com.twitter.scalding.Tool, а также класс, содержащий логику приложения, при запуске задания с помощью команды hadoop.

3. Удалите каталог **src/test**, так как вы не будете создавать тесты в этом примере.

4. Откройте файл **src/main/scala/com/microsoft/example/app.scala** и замените содержимое следующим:

        package com.microsoft.example

        import com.twitter.scalding._

        class WordCount(args : Args) extends Job(args) {
            // 1. Read lines from the specified input location
            // 2. Extract individual words from each line
            // 3. Group words and count them
            // 4. Write output to the specified output location
            TextLine(args("input"))
            .flatMap('line -> 'word) { line : String => tokenize(line) }
            .groupBy('word) { _.size }
            .write(Tsv(args("output")))

            //Tokenizer to split sentance into words
            def tokenize(text : String) : Array[String] = {
            text.toLowerCase.replaceAll("[^a-zA-Z0-9\\s]", "").split("\\s+")
            }
        }

    Это реализует задание по базовому подсчету слов.

5. Сохраните и закройте файлы.

6. Используйте следующую команду из каталога **scaldingwordcount** для сборки и упаковки приложения:

        mvn package

    По завершении этого задания можно найти пакет, содержащий приложение WordCount, в файле **target/scaldingwordcount-1.0-SNAPSHOT.jar**.

## Запуск задания в кластере под управлением Linux

> [AZURE.NOTE] В следующих действиях используются SSH и команда Hadoop. Другие способы выполнения заданий MapReduce см. в разделе [Использование MapReduce в Hadoop в HDInsight](hdinsight-use-mapreduce.md).

1. Воспользуйтесь следующей командой, чтобы загрузить пакет в ваш кластер HDInsight:

        scp target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:

    В результате файлы будут скопированы из локальной системы на головной узел.

    > [AZURE.NOTE] Если для защиты учетной записи SSH использовался пароль, будет предложено ввести пароль. Если использовался ключ SSH, возможно, нужно будет использовать параметр `-i` и указать путь к закрытому ключу. Например, `scp -i /path/to/private/key target/scaldingwordcount-1.0-SNAPSHOT.jar username@clustername-ssh.azurehdinsight.net:.`

2. Введите следующую команду для подключения к головному узлу кластера:

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Если для защиты учетной записи SSH использовался пароль, будет предложено ввести пароль. Если использовался ключ SSH, возможно, нужно будет использовать параметр `-i` и указать путь к закрытому ключу. Например, `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`

3. После подключения к головному узлу используйте следующую команду для запуска задания подсчета слов.

        yarn jar scaldingwordcount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount --hdfs --input wasbs:///example/data/gutenberg/davinci.txt --output wasbs:///example/wordcountout

    Эта команда запускает класс WordCount, реализованный вами ранее. `--hdfs` указывает заданию использовать HDFS. `--input` задает входной текстовый файл, а `--output` указывает местоположение выходных файлов.

4. После завершения задания просмотрите выходные данные с помощью следующей команды.

        hdfs dfs -text wasbs:///example/wordcountout/*

    Будут отображены сведения, подобные следующим:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## Запуск задания в кластере под управлением Windows

В следующих действиях используется Windows PowerShell. Другие способы выполнения заданий MapReduce см. в разделе [Использование MapReduce в Hadoop в HDInsight](hdinsight-use-mapreduce.md).

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

2. Запустите Azure PowerShell и войдите в учетную запись Azure. После предоставления учетных данных команда возвращает информацию о вашей учетной записи.

		Add-AzureRMAccount

		Id                             Type       ...
		--                             ----
		someone@example.com            User       ...

3. Если у вас несколько подписок, укажите идентификатор подписки, которую вы хотите использовать для развертывания.

		Select-AzureRMSubscription -SubscriptionID <YourSubscriptionId>

    > [AZURE.NOTE] `Get-AzureRMSubscription` можно использовать для получения списка всех подписок, связанных с вашей учетной записью, в котором указан идентификатор каждой подписки.

4. Используйте следующий сценарий, чтобы отправить и запустить задание WordCount. Замените `CLUSTERNAME` на имя вашего кластера HDInsight и убедитесь, что `$fileToUpload` содержит правильный путь к файлу __scaldingwordcount-1.0-SNAPSHOT.jar__.

        #Cluster name, file to be uploaded, and where to upload it
        $clustername = "CLUSTERNAME"
        $fileToUpload = "scaldingwordcount-1.0-SNAPSHOT.jar"
        $blobPath = "example/jars/scaldingwordcount-1.0-SNAPSHOT.jar"
        
        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup)[0].Value
        
        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        Set-AzureStorageBlobContent `
            -File $fileToUpload `
            -Blob $blobPath `
            -Container $container `
            -Context $context
            
        #Create a job definition and start the job
        $jobDef=New-AzureRmHDInsightMapReduceJobDefinition `
            -JobName ScaldingWordCount `
            -JarFile wasbs:///example/jars/scaldingwordcount-1.0-SNAPSHOT.jar `
            -ClassName com.microsoft.example.WordCount `
            -arguments "--hdfs", `
                       "--input", `
                       "wasbs:///example/data/gutenberg/davinci.txt", `
                       "--output", `
                       "wasbs:///example/wordcountout"
        $job = Start-AzureRmHDInsightJob `
            -clustername $clusterName `
            -jobdefinition $jobDef `
            -HttpCredential $creds
        Write-Output "Job ID is: $job.JobId"
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $job.JobId `
            -HttpCredential $creds
        #Download the output of the job
        Get-AzureStorageBlobContent `
            -Blob example/wordcountout/part-00000 `
            -Container $container `
            -Destination output.txt `
            -Context $context
        #Log any errors that occured
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

     При запуске сценария вам будет предложено ввести имя пользователя с правами администратора и пароль для кластера HDInsight. Все ошибки, возникающие во время выполнения задания, будут регистрироваться в консоли.
     
6. После завершения задания выходные данные будут скачаны в файл __output.txt__ в текущем каталоге. Используйте следующую команду, чтобы отобразить результаты.

        cat output.txt

    Файл должен содержать значения следующего вида:

        writers 9
        writes  18
        writhed 1
        writing 51
        writings        24
        written 208
        writtenthese    1
        wrong   11
        wrongly 2
        wrongplace      1
        wrote   34
        wrotefootnote   1
        wrought 7

## Дальнейшие действия

Теперь, когда вы узнали, как использовать Scalding для создания заданий MapReduce для HDInsight, воспользуйтесь следующими ссылками, чтобы изучить другие способы работы с Azure HDInsight.

* [Использование Hive с HDInsight](hdinsight-use-hive.md)

* [Использование Pig с HDInsight](hdinsight-use-pig.md)

* [Использование заданий MapReduce с HDInsight](hdinsight-use-mapreduce.md)

<!---HONumber=AcomDC_0803_2016-->