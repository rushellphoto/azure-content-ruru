<properties
pageTitle="Работа с определяемыми пользователем функциями Java с использованием Hive в HDInsight | Microsoft Azure"
description="Узнайте, как создать и использовать определяемые пользователем функции Java из Hive в HDInsight."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="paulettm"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="07/07/2016"
ms.author="larryfr"/>

#Работа с определяемыми пользователем функциями Java с использованием Hive в HDInsight

Hive отлично подходит для работы с данными в HDInsight, но в некоторых случаях требуется язык программирования более общего назначения. Hive позволяет создавать определяемые пользовательские функции (UDF) с использованием различных языков программирования. Здесь вы узнаете, как использовать определяемую пользователем функцию Java из Hive.

## Требования

* Подписка Azure

* Кластер HDInsight (под управлением Windows или Linux)

    > [AZURE.NOTE] Большинство шагов, описанных в этой статье, подходят для кластеров обоих типов. Тем не менее шаги по отправке скомпилированных определяемых пользователем функций в кластер и их выполнению относятся только к кластерам под управлением Linux. Здесь также приведены ссылки на дополнительные сведения, которые можно использовать при работе с кластерами под управлением Windows.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 7 или более поздней версии (или эквивалент, например OpenJDK).

* [Apache Maven](http://maven.apache.org/)

* Текстовый редактор или Java IDE.

    > [AZURE.IMPORTANT] Если вы используете сервер HDInsight под управлением Linux, но создаете файлы Python на клиенте Windows, следует использовать редактор, который использует символ LF в качестве конца строки. Если вы не уверены, использует ли редактор символы LF или CRLF, см. раздел [Устранение неполадок](#troubleshooting), в котором описано, как удалять знаки CR с помощью служебных программ в кластере HDInsight.

## Создание примера определяемой пользователем функции

1. Чтобы создать проект Maven, введите следующую команду в командной строке.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    > [AZURE.NOTE] При использовании PowerShell параметры необходимо заключить в кавычки. Пример: `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Будет создан каталог __exampleudf__, который будет содержать проект Maven.

2. После создания проекта удалите каталог __exampleudf/src/test__, который создан как часть проекта. Его не нужно использовать для этого примера.

3. Откройте файл __exampleudf/pom.xml__ и замените текущую запись `<dependencies>` на следующую:

        <dependencies>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-client</artifactId>
                <version>2.7.1</version>
                <scope>provided</scope>
            </dependency>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-exec</artifactId>
                <version>1.2.1</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

    Эти записи содержат версию Hadoop и Hive, используемую в кластерах HDInsight 3.3 и 3.4. Сведения о версиях Hadoop и Hive, включенных в HDInsight, можно найти в статье [Что представляют собой различные компоненты Hadoop, доступные в HDInsight?](hdinsight-component-versioning.md).

    После внесения этих изменений сохраните файл.

4. Переименуйте файл __exampleudf/src/main/java/com/microsoft/examples/App.java__ на __ExampleUDF.java__, а затем откройте его в редакторе.

5. Замените содержимое файла __ExampleUDF.java__ на следующее и сохраните файл.

        package com.microsoft.examples;

        import org.apache.hadoop.hive.ql.exec.Description;
        import org.apache.hadoop.hive.ql.exec.UDF;
        import org.apache.hadoop.io.*;

        // Description of the UDF
        @Description(
            name="ExampleUDF",
            value="returns a lower case version of the input string.",
            extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
        )
        public class ExampleUDF extends UDF {
            // Accept a string input
            public String evaluate(String input) {
                // If the value is null, return a null
                if(input == null)
                    return null;
                // Lowercase the input string and return it
                return input.toLowerCase();
            }
        }

    Этот код реализует определяемую пользователем функцию, которая принимает строковое значение и возвращает строку в нижнем регистре.

## Создание и установка определяемой пользователем функции

1. Выполните следующую команду, чтобы скомпилировать и упаковать определяемую пользователем функцию.

        mvn compile package

    В результате определяемая пользователем функция будет создана и упакована в файл __exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar__.

2. Чтобы скопировать файл в кластер HDInsight, воспользуйтесь командой `scp`.

        scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight

    Замените __myuser__ именем учетной записи пользователя SSH для кластера. Замените __mycluster__ именем кластера. Если для учетной записи SSH используется пароль, будет предложено его ввести. Если используется сертификат, может потребоваться использовать параметр `-i`, чтобы указать соответствующий файл закрытого ключа.

3. Подключитесь к кластеру с помощью SSH.

        ssh myuser@mycluster-ssh.azurehdinsight.net

    Дополнительные сведения об использовании SSH с HDInsight см. в приведенных ниже документах.

    * [Использование SSH с Hadoop под управлением Linux в HDInsight в Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Использование SSH с Hadoop под управлением Linux в HDInsight в Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

4. В сеансе SSH скопируйте JAR-файл в хранилище HDInsight.

        hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars

## Использование определяемой пользователем функции из Hive

1. В сеансе SSH используйте следующую команду, чтобы запустить клиент Beeline.

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

    В этой команде предполагается, что вы использовали для кластера учетную запись входа по умолчанию — __admin__.

2. После появления командной строки `jdbc:hive2://localhost:10001/>` введите следующую команду, чтобы добавить определяемую пользователем функцию в Hive и предоставить ее как функцию.

        ADD JAR wasbs:///example/jar/ExampleUDF-1.0-SNAPSHOT.jar;
        CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';

3. Используйте определяемую пользователем функцию, чтобы преобразовать значения, полученные из таблицы, в строки нижнего регистра.

        SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;

    В результате выполнения этой команды в таблице будет выбрана платформа устройства (Android, Windows, iOS и т. д.) и отображены строки, преобразованные в нижний регистр. Выходные данные будут выглядеть следующим образом:

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## Дальнейшие действия

Другие способы работы с Hive см. в статье об [использовании Hive в HDInsight](hdinsight-use-hive.md).

Дополнительные сведения об определяемых пользователем функциях Hive см. в разделе [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) (Операторы Hive и определяемые пользователем функции) в вики Hive на сайте apache.org.

<!---HONumber=AcomDC_0727_2016-->