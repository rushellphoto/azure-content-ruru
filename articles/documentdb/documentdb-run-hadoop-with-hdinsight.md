<properties 
	pageTitle="Выполнение заданий Hadoop с помощью DocumentDB и HDInsight | Microsoft Azure" 
	description="Узнайте, как выполнять простые задания Hive, Pig и MapReduce с помощью DocumentDB и Azure HDInsight."
	services="documentdb" 
	authors="AndrewHoh" 
	manager="jhubbard" 
	editor="mimig"
	documentationCenter=""/>


<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="java" 
	ms.topic="article" 
	ms.date="04/26/2016" 
	ms.author="anhoh"/>

#<a name="DocumentDB-HDInsight"></a>Запуск задания Hadoop с помощью DocumentDB и HDInsight

В этом учебнике содержатся сведения о выполнении заданий MapReduce [Apache Hive][apache-hive], [Apache Pig][apache-pig] и [Apache Hadoop][apache-hadoop] в Azure HDInsight с помощью соединителя Hadoop DocumentDB. Соединитель Hadoop позволяет DocumentDB функционировать в качестве источника и приемника для заданий Hive, Pig и MapReduce. В этом учебнике DocumentDB будет использоваться как источник данных и назначение для заданий Hadoop.

После изучения этого учебника вы сможете ответить на следующие вопросы.

- Как загрузить данные из DocumentDB с помощью задания MapReduce, Pig и Hive?
- Как сохранить данные в DocumentDB с помощью задания MapReduce, Pig и Hive?

Прежде чем приступить к работе, рекомендуется просмотреть следующий видеоролик о выполнении задания Hive с помощью DocumentDB и HDInsight.

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

После этого вернитесь к этой статье, содержащей исчерпывающие сведения о выполнении заданий аналитики с данными DocumentDB.

> [AZURE.TIP] В этом учебнике предполагается, что у вас есть опыт использования Apache Hadoop, Hive и Pig. Если вы не знакомы с Apache Hadoop, Hive и Pig, рекомендуем обратиться к [документации по Apache Hadoop][apache-hadoop-doc]. Кроме того, в этом учебнике предполагается, что у вас есть опыт использования DocumentDB и учетная запись DocumentDB. Если вы не знакомы с использованием DocumentDB или у вас нет учетной записи DocumentDB, ознакомьтесь со сведениями на странице [Приступая к работе][getting-started].

Нет времени на подробное изучение материала и просто хотите получить все примеры сценариев PowerShell для Hive, Pig и MapReduce? Не проблема. Они находятся [здесь][documentdb-hdinsight-samples]. В загружаемый пакет входят HQL-, PIG- и YAVA-файлы для этих примеров.

## <a name="NewestVersion"></a>Последняя версия

<table border='1'>
	<tr><th>Версия соединителя Hadoop</th>
		<td>1.2.0</td></tr>
	<tr><th>URI-адрес сценария</th>
		<td>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</td></tr>
	<tr><th>Дата изменения</th>
		<td>26.04.2016</td></tr>
	<tr><th>Поддерживаемые версии HDInsight</th>
		<td>3.1, 3.2</td></tr>
	<tr><th>Журнал изменений</th>
		<td>Обновление SDK Java DocumentDB до версии 1.6.0</br>
			Добавлена поддержка секционированных коллекций в качестве источника и приемника</br>
		</td></tr>
</table>

## <a name="Prerequisites"></a>Предварительные требования
Перед выполнением инструкций в этом учебнике убедитесь в наличии следующих ресурсов.

- Учетная запись DocumentDB, база данных и коллекция с документами внутри. Дополнительные сведения можно найти в статье [Начало работы с DocumentDB][getting-started]. Импортируйте демонстрационные данные в вашу учетную запись DocumentDB с помощью [средства импорта DocumentDB][documentdb-import-data].
- Пропускная способность. Операции чтения и записи из HDInsight будут входить в число выделенных единиц запроса для ваших коллекций. Дополнительные сведения можно найти в разделе [Подготовленная пропускная способность, единицы запросов и операции базы данных][documentdb-manage-throughput].
- Емкость для дополнительных хранимых процедур в каждой выходной коллекции. Хранимые процедуры используются для передачи результирующих документов. Дополнительные сведения можно найти в разделе [Коллекции и подготовленная пропускная способность][documentdb-manage-document-storage].
- Емкость для документов, являющихся результатами выполнения заданий MapReduce, Pig и Hive. Дополнительные сведения можно найти в статье [Управление емкостью и производительностью DocumentDB][documentdb-manage-collections].
- [*Необязательная*] емкость для дополнительной коллекции. Подробнее можно узнать в разделе [Подготовленное хранилище документов и накладные затраты на индексирование][documentdb-manage-document-storage].
	
> [AZURE.WARNING] Во избежание создания новой коллекции во время выполнения заданий можно распечатать результаты в stdout, сохранить результат в контейнере WASB или указать уже существующую коллекцию. Если указывается существующая коллекция, в ней будут созданы новые документы. При наличии конфликта в полях *id* затрагиваются только существующие документы. **Соединитель автоматически перезапишет существующие документы с конфликтами идентификаторов**. Эту функцию можно отключить, задав параметру upsert значение false. Если параметру upsert задано значение false и возникает конфликт, задание Hadoop завершится ошибкой и выводом сообщения о конфликте идентификаторов.

## <a name="CreateStorage"></a>Шаг 1. Создание учетной записи хранения Azure

> [AZURE.IMPORTANT] Если у вас **уже** есть учетная запись хранения Azure и вы хотите создать в ней новый контейнер больших двоичных объектов, перейдите к разделу [Шаг 2. Создание настраиваемого кластера HDInsight](#ProvisionHDInsight).

Для хранения данных Azure HDInsight использует хранилище больших двоичных объектов Azure. Оно называется *WASB* или *Хранилище Azure — большие двоичные объекты*. WASB — это реализация HDFS на базе хранилища больших двоичных объектов Azure от корпорации Майкрософт. Дополнительные сведения см. в разделе [Использование хранилища больших двоичных объектов Azure с HDInsight][hdinsight-storage].

При подготовке кластера HDInsight указывается учетная запись хранения Azure Storage. Конкретный контейнер хранилища больших двоичных объектов из этой учетной записи назначается в качестве файловой системы по умолчанию, так же как и в HDFS. По умолчанию подготовка кластера HDInsight выполняется в том же центре обработки данных, в котором находится указанная учетная запись хранения.

**Создание учетной записи хранения Azure**

1. Войдите на [классический портал Azure][azure-classic-portal].

2. Щелкните **+ СОЗДАТЬ** в левом нижнем углу, выберите **СЛУЖБЫ ДАННЫХ**, затем **ХРАНИЛИЩЕ**, а затем щелкните **БЫСТРОЕ СОЗДАНИЕ**. ![Классический портал Azure, на котором можно настроить новую учетную запись хранения с помощью функции быстрого создания.][image-storageaccount-quickcreate]

3. Введите **URL-адрес**, выберите значения для параметров **РАСПОЛОЖЕНИЕ** и **РЕПЛИКАЦИЯ**, а затем нажмите кнопку **СОЗДАТЬ УЧЕТНУЮ ЗАПИСЬ ХРАНЕНИЯ**. Территориальные группы не поддерживаются.
	
	Вы увидите новую учетную запись хранения в списке хранилищ.

	> [AZURE.IMPORTANT] В целях повышения производительности убедитесь, что учетная запись хранения, кластер HDInsight и учетная запись DocumentDB находятся в одном регионе Azure.

4. Дождитесь, пока **СОСТОЯНИЕ** новой учетной записи хранения не изменится на **В сети**.

## <a name="ProvisionHDInsight"></a>Шаг 2. Создание настраиваемого кластера HDInsight
В этом учебнике для настройки кластера HDInsight используется действие сценария на классическом портале Azure. С помощью этого портала создается настраиваемый кластер. Инструкции по использованию командлетов PowerShell или пакета HDInsight .NET SDK можно найти в статье [Настройка кластеров HDInsight с помощью действия скрипта][hdinsight-custom-provision].

1. Войдите на [классический портал Azure][azure-classic-portal]. Вы уже могли выполнить это действие в предыдущем шаге.

2. В нижней части страницы щелкните **+СОЗДАТЬ**, затем последовательно щелкните **СЛУЖБЫ ДАННЫХ**, **HDINSIGHT** и **НАСТРАИВАЕМОЕ СОЗДАНИЕ**.

3. На странице **Сведения о кластере** введите или выберите следующие значения:

	![Укажите подробную информацию об исходном кластере Hadoop HDInsight][image-customprovision-page1]

	<table border='1'>
		<tr><th>Свойство</th><th>Значение</th></tr>
		<tr><td>Имя кластера</td><td>Имя кластера.<br/>
			DNS-имя должно начинаться и заканчиваться буквенно-цифровым символом и может содержать дефисы.<br/>
			Поле должно представлять собой строку длиной от 3 до 63 символов.</td></tr>
		<tr><td>Название подписки</td>
			<td>Если имеется несколько подписок Azure, выберите подписку, соответствующую учетной записи хранения, из <strong>шага&#160;1</strong>. </td></tr>
		<tr><td>Тип кластера</td>
			<td>Для типа кластера выберите <strong>Hadoop</strong>.</td></tr>
		<tr><td>Операционная система</td>
			<td>В качестве операционной системы выберите <strong>Windows Server 2012 R2 Datacenter</strong>.</td></tr>
		<tr><td>Версия HDInsight</td>
			<td>Выберите версию. </br>Выберите <Strong>HDInsight версии 3.1</Strong>.</td></tr>
		</table>

	<p>Введите или выберите значения, указанные в таблице, а затем щелкните стрелку вправо.</p>

4. На странице **Настройка кластера** введите или выберите следующие значения:

	<table border="1">
	<tr><th>Имя</th><th>Значение</th></tr>
	<tr><td>Узлы данных</td><td>Число узлов данных, которые требуется развернуть. </br>Обратите внимание, что узлы данных HDInsight связаны с производительностью и ценами.</td></tr>
	<tr><td>Регион/виртуальная сеть</td><td>Выберите тот же регион, что и для созданной <strong>учетной записи хранения</strong> и <strong>учетной записи DocumentDB</strong>. </br> Для HDInsight требуется, чтобы учетная запись хранения находилась в том же регионе. При последующей настройке можно выбрать только учетную запись хранения, которая находится в том же регионе, который указан здесь.</td></tr>
	</table>
	
    Щелкните стрелку вправо.

5. На странице **Настройка пользователя кластера** введите следующие значения:

    <table border='1'>
		<tr><th>Свойство</th><th>Значение</th></tr>
		<tr><td>Имя пользователя</td>
			<td>Укажите имя пользователя кластера HDInsight.</td></tr>
		<tr><td>Пароль/подтверждение пароля</td>
			<td>Укажите пароль пользователя кластера HDInsight.</td></tr>
	</table>
	
    Щелкните стрелку вправо.
    
6. На странице **Учетная запись хранения** введите следующие значения:

	![Указание учетной записи хранения для кластера Hadoop HDInsight][image-customprovision-page4]

	<table border='1'>
		<tr><th>Свойство</th><th>Значение</th></tr>
		<tr><td>Учетная запись хранения</td>
			<td>Укажите учетную запись хранения Azure, которая будет использована в качестве файловой системы по умолчанию для кластера HDInsight. Можно выбрать один из трех параметров: использовать существующее хранилище, создать новое хранилище или использовать хранилище другой подписки.</br></br>
			Выберите <strong>Использовать существующее хранилище</strong>.
			</td>
			</td></tr>
		<tr><td>Имя учетной записи</td>
			<td>
			Для параметра <strong>Имя учетной записи</strong> выберите учетную запись, созданную на <strong>шаге&#160;1</strong>. В раскрывающемся списке приводятся только учетные записи хранения в одной подписке Azure, расположенные в центре обработки данных, выбранном для подготовки кластера.
			</td></tr>
		<tr><td>Контейнер по умолчанию</td>
			<td>Задает контейнер по умолчанию в учетной записи хранения, который используется в качестве файловой системы по умолчанию для кластера HDInsight. Если выбирается параметр <strong>Использовать существующее хранилище</strong> для поля <strong>Учетная запись хранения</strong> и в этой учетной записи нет существующих контейнеров, то контейнер создается по умолчанию с тем же именем, что и имя кластера. Если контейнер с именем кластера уже существует, к имени контейнера будет добавлено последовательное число.
	    </td></tr>
		<tr><td>Дополнительные учетные записи хранения</td>
			<td>HDInsight поддерживает несколько учетных записей хранения. Ограничения на дополнительную учетную запись хранения, которая может использоваться в кластере, отсутствуют. Тем не менее, при создании кластера с использованием классического портала Azure можно использовать не более семи учетных записей из-за ограничений пользовательского интерфейса. Каждая дополнительная учетная запись хранения, указанная в этом поле, добавляет дополнительную страницу учетной записи хранения к мастеру, где можно указать сведения об учетной записи.</td></tr>
	</table>

	Щелкните стрелку вправо.

7. На странице **Действия сценария** щелкните **Добавить действие сценария** для указания сведений о пользовательском сценарии, который необходимо выполнить для настройки кластера во время его создания. Сценарий PowerShell установит соединитель Hadoop DocumentDB в кластеры HDInsight во время их создания.
	
	![Настройка действия сценария для настройки кластера HDInsight][image-customprovision-page5]

	<table border='1'>
		<tr><th>Свойство</th><th>Значение</th></tr>
		<tr><td>Имя</td>
			<td>Укажите имя для действия сценария.</td></tr>
		<tr><td>URI-адрес сценария</td>
			<td>Укажите URI для сценария, который вызывается для настройки кластера.</br></br>
			Введите: </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v03.ps1</strong>.</td></tr>
		<tr><td>Тип узла</td>
			<td>Указывает узлы, на которых выполняется скрипт настройки. Можно выбрать одно из трех значений: <b>Все узлы</b>, <b>Только головные узлы</b> или <b>Только рабочие узлы</b>.</br></br>
			Выберите <strong>Все узлы</strong>.</td></tr>
		<tr><td>Параметры</td>
			<td>Укажите параметры, если они требуются для сценария.</br></br>
			<strong>Параметры не нужны</strong>.</td></tr>
	</table>

	Установите флажок, чтобы завершить создание кластера.

## <a name="InstallCmdlets"></a>Шаг 3. Установка и настройка Azure PowerShell

1. Установите Azure PowerShell. Инструкции можно найти [здесь][powershell-install-configure].

	> [AZURE.NOTE] Кроме того, только для запросов Hive можно использовать онлайн-редактор Hive в HDInsight. Для этого войдите на [классический портал Azure][azure-classic-portal] и щелкните **HDInsight** в левой области, чтобы просмотреть список кластеров HDInsight. Щелкните кластер, в котором будут выполняться запросы Hive, а затем щелкните **Консоль запросов**.

2. Откройте интегрированную среду сценариев Azure PowerShell.
	- На компьютере под управлением Windows 8 или Windows Server 2012 или более поздней версии можно использовать встроенную функцию поиска. На начальном экране введите **powershell ise** и нажмите клавишу **ВВОД**. 
	- На компьютере под управлением Windows более ранней версии, чем Windows 8 или Windows Server 2012, можно использовать меню «Пуск». В меню "Пуск" в поле поиска введите **командная строка**, а затем в списке результатов щелкните вариант **Командная строка**. В командной строке введите **powershell\_ise** и нажмите клавишу **ВВОД**.

3. Добавьте учетную запись Azure.
	1. В области консоли введите **Add-AzureAccount** и нажмите клавишу **ВВОД**. 
	2. Введите адрес электронной почты, связанный с подпиской Azure, и нажмите кнопку **Продолжить**. 
	3. Введите пароль для подписки Azure. 
	4. Щелкните **Войти**.

4. На следующей схеме показаны важные части среды сценариев PowerShell Azure.

	![Схема для Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Шаг 4. Выполнение задания Hive с помощью DocumentDB и HDInsight

> [AZURE.IMPORTANT] Все переменные, обозначенные символами < >, должны быть заполнены с помощью параметров конфигурации.

1. Задайте следующие переменные в области сценариев PowerShell.

		# Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
		$subscriptionName = "<SubscriptionName>"
		$storageAccountName = "<AzureStorageAccountName>"
		$containerName = "<AzureStorageContainerName>"

		# Provide the HDInsight cluster name where you want to run the Hive job.
		$clusterName = "<HDInsightClusterName>"

2. 
	<p>Начнем создавать строку запроса. Будет написан запрос Hive, который принимает все системные отметки времени документа(_ts)и уникальные идентификаторы (_rid) из коллекции DocumentDB, подсчитывает все документы поминутно, а затем сохраняет результаты в новую коллекцию DocumentDB. </p>

    <p>Сначала создадим таблицу Hive из коллекции DocumentDB. Добавьте следующий фрагмент кода в область сценариев PowerShell <strong>после</strong> фрагмента кода с шага&#160;1. Убедитесь, что включен дополнительный параметр DocumentDB.query для обрезки документов до вида _ts и _rid. </p>

    > [AZURE.NOTE] **Именование DocumentDB.inputCollections не было ошибочным.** Да, разрешается добавление нескольких коллекций в качестве входных данных: </br> 
	'*DocumentDB.inputCollections*' = '*<DocumentDB Input Collection Name 1>*,*<DocumentDB Input Collection Name 2>*' </br> Имена коллекций отделены одной запятой, без пробелов.


		# Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
		$queryStringPart1 = "drop table DocumentDB_timestamps; "  + 
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " + 
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
 
3.  Теперь создадим таблицу Hive для выходной коллекции. Выходными свойствами документа будут месяц, день, час, минута и общее число вхождений.

	> [AZURE.NOTE] **Повторим еще раз: именование DocumentDB.outputCollections не было ошибкой.** Да, разрешается добавление нескольких коллекций в качестве выходных данных: </br> 
	'*DocumentDB.outputCollections*' = '*\<DocumentDB Output Collection Name 1\>*,*\<DocumentDB Output Collection Name 2\>*' </br> Имена коллекций отделены одной запятой, без пробелов. </br></br>
	Документы будут распределяться по нескольким коллекциям согласно схеме циклического перебора. Пакет документов будет храниться в одной коллекции, второй пакет документов будет храниться в следующей коллекции и т. д.

		# Create a Hive table for the output data to DocumentDB.
	    $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " + 
                              "tblproperties ( " + 
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. Наконец, подсчитаем документы по месяцу, дню, часу и минуте и вставим результаты в выходную таблицу Hive.

		# GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. Добавьте следующий фрагмент сценария, чтобы создать определение задания Hive из предыдущего запроса.

		# Create a Hive job definition.
		$queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
		$hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

	Можно также использовать параметр -File, чтобы указать файл сценария HiveQL в HDFS.

6. Добавьте следующий фрагмент для экономии времени начала и отправки задания Hive.

		# Save the start time and submit the job to the cluster.
		$startTime = Get-Date
		Select-AzureSubscription $subscriptionName
		$hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. Добавьте следующий фрагмент, чтобы дождаться завершения задания Hive.

		# Wait for the Hive job to complete.
		Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. Добавьте следующий фрагмент печати стандартного вывода и время начала и окончания.

		# Print the standard error, the standard output of the Hive job, and the start and end time.
		$endTime = Get-Date
		Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
		Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **Выполните** новый сценарий. **Нажмите** зеленую кнопку выполнения.

10. Проверьте результаты. Войдите на [портал Azure][azure-portal]
	1. Щелкните кнопку <strong>Обзор</strong> на панели слева. </br>
	2. В правой верхней части панели обзора выберите <strong>Все ресурсы</strong>. </br>
	3. Найдите и щелкните пункт <strong>Учетные записи DocumentDB</strong>. </br>
	4. Затем найдите свою <strong>учетную запись DocumentDB</strong>, <strong>базу данных DocumentDB</strong> и <strong>коллекцию DocumentDB</strong>, связанную с выходной коллекцией, указанной в запросе Hive.</br>
	5. И, наконец, щелкните <strong>Обозреватель документов</strong> в разделе <strong>Средства разработчика</strong>.</br></p>

	Будут отображены результаты запроса Hive.

	![Результаты запроса Hive][image-hive-query-results]

## <a name="RunPig"></a>Шаг 5. Выполнение задания Pig с помощью DocumentDB и HDInsight

> [AZURE.IMPORTANT] Все переменные, обозначенные символами < >, должны быть заполнены с помощью параметров конфигурации.

1. Задайте следующие переменные в области сценариев PowerShell.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>Начнем создавать строку запроса. Напишем запрос Pig, который принимает все системные отметки времени документов (_ts) и уникальные идентификаторы (_rid) из коллекции DocumentDB, подсчитывает все документы поминутно и затем сохраняет результаты в новую коллекцию DocumentDB.</p>
    <p>Сначала загрузите документы из DocumentDB в HDInsight. Добавьте следующий фрагмент кода в область сценариев PowerShell <strong>после</strong> фрагмента кода с шага&#160;1. Добавьте запрос DocumentDB в дополнительный параметр запроса DocumentDB для обрезки документов до вида _ts и _rid.</p>

    > [AZURE.NOTE] Да, разрешается добавление нескольких коллекций в качестве входных данных: </br>
    '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*'</br>. Имена коллекций отделены одной запятой, без пробелов. </b>

	Документы будут распределяться по нескольким коллекциям согласно циклической схеме. Пакет документов будет храниться в одной коллекции, второй пакет документов будет храниться в следующей коллекции и т. д.

		# Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  Затем подсчитаем документы по месяцу, дню, часу, минуте и определим общее число вхождений.

		# GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. И, наконец, сохраним результаты в новой выходной коллекции.

    > [AZURE.NOTE] Да, разрешается добавление нескольких коллекций в качестве выходных данных: </br>
    '\<DocumentDB Output Collection Name 1\>,\<DocumentDB Output Collection Name 2\>'</br>. Имена коллекций отделены одной запятой, без пробелов.</br>
    Документы будут распределяться по нескольким коллекциям согласно циклической схеме. Пакет документов будет храниться в одной коллекции, второй пакет документов будет храниться в следующей коллекции и т. д.

		# Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. Добавьте следующий фрагмент сценария для создания определения запроса Pig из предыдущего запроса.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

	Можно также использовать параметр -File, чтобы указать сценарий Pig в HDFS.

6. Добавьте следующий фрагмент для экономии времени начала и отправки задания Pig.

		# Save the start time and submit the job to the cluster.
		$startTime = Get-Date
		Select-AzureSubscription $subscriptionName
		$pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. Добавьте следующий фрагмент, чтобы дождаться завершения задания Pig.

		# Wait for the Pig job to complete.
		Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. Добавьте следующий фрагмент печати стандартного вывода и время начала и окончания.

		# Print the standard error, the standard output of the Hive job, and the start and end time.
		$endTime = Get-Date
		Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
		Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
		
9. **Выполните** новый сценарий. **Нажмите** зеленую кнопку выполнения.

10. Проверьте результаты. Войдите на [портал Azure][azure-portal]
	1. Щелкните кнопку <strong>Обзор</strong> на панели слева. </br>
	2. В правой верхней части панели обзора выберите <strong>Все ресурсы</strong>. </br>
	3. Найдите и щелкните пункт <strong>Учетные записи DocumentDB</strong>. </br>
	4. Затем найдите свою <strong>учетную запись DocumentDB</strong>, <strong>базу данных DocumentDB</strong> и <strong>коллекцию DocumentDB</strong>, связанную с выходной коллекцией, указанной в запросе Pig.</br>
	5. И, наконец, щелкните <strong>Обозреватель документов</strong> в разделе <strong>Средства разработчика</strong>.</br></p>

	Будут отображены результаты выполнения запроса Pig.

	![Результаты запроса Pig][image-pig-query-results]

## <a name="RunMapReduce"></a>Шаг 6. Выполнение задания MapReduce с помощью DocumentDB и HDInsight

1. Задайте следующие переменные в области сценариев PowerShell.
		
		$subscriptionName = "<SubscriptionName>"   # Azure subscription name
		$clusterName = "<ClusterName>"             # HDInsight cluster name
		
2. Будет выполняться задание MapReduce, которое подсчитывает число вхождений для каждого свойства документа из коллекции DocumentDB. Добавьте этот фрагмент сценария **после** приведенного выше фрагмента.

		# Define the MapReduce job.
		$TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

	> [AZURE.NOTE] В состав TallyProperties-v01.jar входит выборочная установка соединителя Hadoop DocumentDB.

3. Добавьте следующую команду, чтобы отправить задание MapReduce.

		# Save the start time and submit the job.
		$startTime = Get-Date
		Select-AzureSubscription $subscriptionName
		$TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

	Помимо определения задания MapReduce вы также указываете имя кластера HDInsight, в котором нужно выполнить задание MapReduce, и учетные данные. Start-AzureHDInsightJob представляет собой асинхронизированный вызов. Для проверки выполнения задания используйте командлет *Wait-AzureHDInsightJob*.

4. Добавьте следующую команду, чтобы проверить наличие ошибок при выполнении задания MapReduce.
	
		# Get the job output and print the start and end time.
		$endTime = Get-Date
		Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
		Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green 

5. **Выполните** новый сценарий. **Нажмите** зеленую кнопку выполнения.

6. Проверьте результаты. Войдите на [портал Azure][azure-portal]
	1. Щелкните кнопку <strong>Просмотреть</strong> на панели слева.
	2. В правой верхней части панели обзора выберите <strong>Все ресурсы</strong>.
	3. Найдите и щелкните пункт <strong>Учетные записи DocumentDB</strong>.
	4. Затем найдите свою <strong>учетную запись DocumentDB</strong>, <strong>базу данных DocumentDB</strong> и <strong>коллекцию DocumentDB</strong>, связанную с выходной коллекцией, указанной в задании MapReduce.
	5. И, наконец, щелкните <strong>Обозреватель документов</strong> в разделе <strong>Средства разработчика</strong>.

	Будут отображены результаты выполнения задания MapReduce.

	![Результаты запроса MapReduce][image-mapreduce-query-results]

## <a name="NextSteps"></a>Дальнейшие действия

Поздравляем! Вы только что выполнили свои первые задания Hive, Pig и MapReduce с помощью Azure DocumentDB и HDInsight.

Теперь у нас есть соединитель Hadoop с открытым исходным кодом. Если вас заинтересовал этот процесс, вы можете продолжить на [GitHub][documentdb-github].

Для получения дополнительных сведений ознакомьтесь со следующими статьями:

- [Разработка приложений Java с помощью Documentdb][documentdb-java-application]
- [Разработка программ MapReduce на Java для Hadoop в HDInsight][hdinsight-develop-deploy-java-mapreduce]
- [Приступая к работе с Hadoop с использованием Hive в HDInsight для анализа данных об использовании мобильного телефона][hdinsight-get-started]
- [Использование MapReduce с HDInsight][hdinsight-use-mapreduce]
- [Использование Hive с HDInsight][hdinsight-use-hive]
- [Использование Pig с HDInsight][hdinsight-use-pig]
- [Настройка кластеров HDInsight с помощью действия сценария][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-classic-portal]: https://manage.windowsazure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png
[azure-portal]: https://portal.azure.com/

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-customprovision-page4]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page4.png
[image-customprovision-page5]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page5.png
[image-storageaccount-quickcreate]: ./media/documentdb-run-hadoop-with-hdinsight/storagequickcreate.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../powershell-install-configure.md
 

<!---HONumber=AcomDC_0518_2016-->