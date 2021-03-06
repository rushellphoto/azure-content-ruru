<properties
pageTitle="Индексирование хранилища BLOB-объектов Azure с помощью службы поиска Azure"
description="Узнайте, как индексировать хранилище BLOB-объектов Azure и извлекать текст из документов с помощью службы поиска Azure."
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="08/08/2016"
ms.author="eugenesh" />

# Индексирование документов в хранилище BLOB-объектов Azure с помощью службы поиска Azure

В этой статье показано, как использовать поиск Azure для индексации документов (например, файлов PDF, Microsoft Office и некоторых других распространенных форматов), которые хранятся в хранилище BLOB-объектов Azure. Новый индексатор больших двоичных объектов поиска Azure позволяет сделать этот процесс быстрым и эффективным.

> [AZURE.IMPORTANT] Сейчас эта функция доступна в режиме предварительной версии. Она доступна при использовании REST API версии **2015-02-28-Preview**. Помните, что предварительные версии API предназначены для тестирования и ознакомления. Они не должны использоваться в рабочей среде.

## Поддерживаемые форматы документов

Индексатор больших двоичных объектов может извлечь текст из документов следующих форматов:

- PDF;
- форматы Microsoft Office: DOCX/DOC, XLSX/XLS, PPTX/PPT, MSG (сообщения электронной почты Outlook);
- HTML
- XML
- ZIP;
- EML
- Обычные текстовые файлы
- JSON (подробные сведения см. в статье [Индексирование BLOB-объектов JSON с помощью индексатора BLOB-объектов службы поиска Azure](search-howto-index-json-blobs.md));
- CSV (подробные сведения см. в статье [Индексирование BLOB-объектов в формате CSV с помощью индексатора BLOB-объектов службы поиска Azure](search-howto-index-csv-blobs.md)).

## Настройка индексирования больших двоичных объектов

Установить и настроить индексатор хранилища BLOB-объектов Azure можно с помощью REST API службы поиска Azure. Вы сможете создать **индексаторы** и **источники данных**, а также управлять ими, как описано в [этой статье](https://msdn.microsoft.com/library/azure/dn946891.aspx). В будущем поддержка индексирования больших двоичных объектов будет добавлена в пакет Azure SDK для .NET и на портал Azure.

Чтобы настроить индексатор, выполните три шага: создайте источник данных, создайте индекс, настройте индексатор.

### Шаг 1. Создание источника данных

Источник данных определяет следующее: какие данные нужно индексировать, какие учетные данные требуются для доступа к этим данным и какие политики нужны, чтобы служба поиска Azure могла эффективно определять изменения в данных (новые, измененные или удаленные строки). Источник данных могут использовать несколько индексаторов в одной подписке.

Чтобы индексировать большие двоичные объекты, источник данных должен иметь следующие необходимые свойства.

- **Имя** — уникальное имя источника данных в службе поиска.

- **Тип** должен быть `azureblob`.

- **Учетные данные** — предоставляют строку подключения к учетной записи как параметр `credentials.connectionString`. Строку подключения можно получить на портале Azure. Перейдите к колонке > **Настройки** > **Ключи** нужной учетной записи хранения и используйте значение "Первичная строка подключения" или "Вторичная строка подключения". Так как строка подключения привязана к учетной записи хранения, указав строку подключения, вы неявно определяете учетную запись хранения, предоставляющую данные.

- **Контейнер** определяет контейнер в вашей учетной записи хранения. По умолчанию все большие двоичные объекты в контейнере доступны для извлечения. Если требуется индексирование больших двоичных объектов из определенного виртуального каталога, можно указать этот каталог с помощью дополнительного параметра **запрос**.

В следующем примере проиллюстрировано определение источника данных:

	POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	    "name" : "blob-datasource",
	    "type" : "azureblob",
	    "credentials" : { "connectionString" : "<my storage connection string>" },
	    "container" : { "name" : "my-container", "query" : "<optional-virtual-directory-name>" }
	}   

Дополнительные сведения об API создания источника данных см. в разделе [Создание источника данных](search-api-indexers-2015-02-28-preview.md#create-data-source).

### Шаг 2. Создание индекса 

Индекс задает поля в документе, атрибуты, и другие компоненты, которые определяют процедуру поиска.

Чтобы индексировать большие двоичные объекты, убедитесь, что в индексе есть поддерживающее поиск поле `content` для хранения большого двоичного объекта.

	POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
	Content-Type: application/json
	api-key: [admin key]

	{
  		"name" : "my-target-index",
  		"fields": [
    		{ "name": "id", "type": "Edm.String", "key": true, "searchable": false },
    		{ "name": "content", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
  		]
	}

Дополнительные сведения об API создания индекса см. в статье, посвященной [созданию индекса](https://msdn.microsoft.com/library/dn798941.aspx)

### Шаг 3. Создание индексатора 

Индексатор соединяет источники данных с целевыми индексами поиска и предоставляет сведения о планировании, чтобы вы могли автоматизировать обновление данных. Создав индекс и источник данных, вы можете легко создать индексатор, который ссылается на источник данных и целевой индекс. Например:

	POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  "name" : "blob-indexer",
	  "dataSourceName" : "blob-datasource",
	  "targetIndexName" : "my-target-index",
	  "schedule" : { "interval" : "PT2H" }
	}

Этот индексатор будет выполняться каждые два часа (интервал расписания имеет значение "PT2H"). Чтобы запускать индексатор каждые 30 минут, задайте интервал "PT30M". Самый короткий интервал, который можно задать, составляет 5 минут. Расписание является необязательным. Если оно не указано, то индексатор выполняется только один раз при его создании. Однако индексатор можно запустить по запросу в любое время.

Дополнительные сведения об API для создания индексатора см. в разделе [Создание индексатора](search-api-indexers-2015-02-28-preview.md#create-indexer).


## Процесс извлечения документа

Служба поиска Azure индексирует каждый документ (большой двоичный объект) указанным ниже образом.

- Все текстовое содержимое документа извлекается в поле строки с именем `content`. Обратите внимание, что сейчас извлечение нескольких документов из одного большого двоичного объекта не поддерживается.
	- Например, CSV-файл индексируется как один документ. Если вам необходимо обрабатывать каждую строку в CSV-файле как отдельный документ, проголосуйте за [это предложение UserVoice](https://feedback.azure.com/forums/263029-azure-search/suggestions/13865325-please-treat-each-line-in-a-csv-file-as-a-separate).
	- Составной или внедренный документ (например, ZIP-архив или документ Word с внедренным сообщением электронной почты Outlook и вложением в формате PDF) также индексируется как один документ.

- В большом двоичном объекте определяемые пользователем свойства метаданных извлекаются без изменений. Свойства метаданных также можно использовать для управления некоторыми аспектами процесса извлечения документа. Дополнительные сведения см. в разделе [Использование пользовательских метаданных для управления извлечением документа](#CustomMetadataControl).

- Стандартные свойства метаданных большого двоичного объекта извлекаются в указанные ниже поля.

	- **metadata\_storage\_name** (Edm.String) — имя файла большого двоичного объекта. Например, для большого двоичного объекта /my-container/my-folder/subfolder/resume.pdf значение этого поля — `resume.pdf`.

	- **metadata\_storage\_path** (Edm.String) — полный универсальный код ресурса большого двоичного объекта, включая учетную запись хранения. Например, `https://myaccount.blob.core.windows.net/my-container/my-folder/subfolder/resume.pdf`

	- **metadata\_storage\_content\_type** (Edm.String) — тип содержимого, указанный в коде для отправки большого двоичного объекта. Например, `application/octet-stream`.

	- **metadata\_storage\_last\_modified** (Edm.DateTimeOffset) — последняя измененная метка времени для большого двоичного объекта. Служба поиска Azure использует эту метку времени для определения измененных больших двоичных объектов, чтобы не выполнять повторное полное индексирование.

	- **metadata\_storage\_size** (Edm.Int64) — размер большого двоичного объекта в байтах.

	- **metadata\_storage\_content\_md5** (Edm.String) — хэш MD5 содержимого большого двоичного объекта, если он доступен.

- Определенные для каждого формата документа свойства метаданных извлекаются в поля, список которых приводится [здесь](#ContentSpecificMetadata).

Вам не нужно определять поля для всех перечисленных свойств в индексе поиска — достаточно записать свойства, необходимые для приложения.

> [AZURE.NOTE] Как правило, имена полей в существующем индексе отличаются от имен полей, созданных при извлечении документа. Можно использовать **сопоставления полей** для сопоставления имен свойств, предоставленных службой поиска Azure, с именами полей в индексе поиска. Ниже вы увидите пример использования сопоставления полей.

## Выбор поля ключа документа и работа с разными именами полей

В службе поиска Azure ключ документа однозначно определяет документ. Каждый индекс поиска должен содержать только одно поле ключа типа Edm.String. Поле ключа является обязательным для каждого документа, который добавляется к индексу (собственно, это единственное обязательное для заполнения поле).
   
Следует определить, какие извлеченные поля должны сопоставляться с полем ключа индекса. Основные варианты представлены ниже.

- **metadata\_storage\_name** — удобный вариант, но следует учесть следующее. 1) Имена могут не быть уникальными, так как возможно наличие больших двоичных объектов с одинаковыми именами в разных папках. 2) Имя может содержать символы, недопустимые для использования в ключах документов, например дефисы. Можно работать с недопустимыми символами, активировав параметр `base64EncodeKeys` в свойствах индексатора. После этого обязательно закодируйте ключи документов для передачи во время вызовов API, таких как подстановка. (Например, в .NET для этого можно использовать [метод UrlTokenEncode](https://msdn.microsoft.com/library/system.web.httpserverutility.urltokenencode.aspx)).

- **metadata\_storage\_path** — использование полного пути гарантирует уникальность, но такой путь определенно содержит символы `/`, [недопустимые для использования в ключе документа](https://msdn.microsoft.com/library/azure/dn857353.aspx). Как упоминалось выше, вы можете закодировать ключи с помощью параметра `base64EncodeKeys`.

- Если ни один вариант вам не подходит, вы можете просто добавить свойства пользовательских метаданных в большие двоичные объекты. Однако для этого варианта требуется, чтобы при передаче эти свойства метаданных добавлялись ко всем большим двоичным объектам. Поскольку ключ является обязательным свойством, все большие двоичные объекты без этого свойства индексироваться не будут.

> [AZURE.IMPORTANT] Если в индексе отсутствует явное сопоставление поля ключа, служба поиска Azure будет автоматически использовать `metadata_storage_path` (второй параметр из указанных выше) в качестве ключа, активировав кодировку ключей base-64.

В этом примере мы выберем поле `metadata_storage_name` в качестве ключа документа. Предположим, что индекс содержит поле ключа с именем `key` и поле `fileSize` для хранения данных о размере документа. Чтобы правильно связать все компоненты при создании или обновлении индексатора, укажите следующие сопоставления полей:

	"fieldMappings" : [
	  { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key" },
	  { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
	]

Чтобы объединить все компоненты, можно добавить сопоставления полей и активировать кодировку ключей base-64 для существующего индексатора:

	PUT https://[service name].search.windows.net/indexers/blob-indexer?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  "dataSourceName" : " blob-datasource ",
	  "targetIndexName" : "my-target-index",
	  "schedule" : { "interval" : "PT2H" },
	  "fieldMappings" : [
	    { "sourceFieldName" : "metadata_storage_name", "targetFieldName" : "key" },
	    { "sourceFieldName" : "metadata_storage_size", "targetFieldName" : "fileSize" }
	  ],
	  "parameters" : { "base64EncodeKeys": true }
	}

> [AZURE.NOTE] Подробнее о сопоставления полей см. в [этой статье](search-indexer-field-mappings.md).

## Добавочное индексирование и обнаружение удаления

Когда для индексатора больших двоичных объектов вы настраиваете запуск по расписанию, повторно индексируются только измененные большие двоичные объекты. Они определяются по метке времени большого двоичного объекта `LastModified`.

> [AZURE.NOTE] Нет необходимости указывать политику обнаружения изменений — добавочное индексирование будет активировано автоматически.

Выбрать определенные документы, которые будут удалены из индекса, можно с помощью стратегии обратимого удаления. Вместо удаления соответствующих больших двоичных объектов добавьте свойство пользовательских метаданных, указывающее, что они удалены. Затем настройте политику обнаружения обратимого удаления в источнике данных.

> [AZURE.WARNING] Если вы просто удалите большие двоичные объекты (не используя политику обнаружения удаления), соответствующие документы не будут удалены из индекса поиска.

Например, в соответствии с приведенной ниже политикой большой двоичный объект удаляется, если у него есть свойство метаданных `IsDeleted` со значением `true`.

	PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	    "name" : "blob-datasource",
	    "type" : "azureblob",
	    "credentials" : { "connectionString" : "<your storage connection string>" },
		"container" : { "name" : "my-container", "query" : "my-folder" },
		"dataDeletionDetectionPolicy" : {
			"@odata.type" :"#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", 	
			"softDeleteColumnName" : "IsDeleted",
			"softDeleteMarkerValue" : "true"
		}
	}   

<a name="ContentSpecificMetadata"></a>
## Свойства метаданных определенного типа содержимого

В таблице ниже содержатся сводные данные обработки для каждого формата документа, а также описание свойств метаданных, извлеченных службой поиска Azure.

Формат документа или тип содержимого | Свойства метаданных определенного типа содержимого | Сведения об обработке
-------------------------------|-------------------------------------------|-------------------
HTML (`text/html`) | `metadata_content_encoding`<br/>`metadata_content_type`<br/>`metadata_language`<br/>`metadata_description`<br/>`metadata_keywords`<br/>`metadata_title` | Удаление разметки HTML и извлечение текста
PDF (`application/pdf`) | `metadata_content_type`<br/>`metadata_language`<br/>`metadata_author`<br/>`metadata_title`| Извлечение текста, включая внедренные документы (кроме изображений)
DOCX (application/vnd.openxmlformats-officedocument.wordprocessingml.document) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Извлечение текста, включая внедренные документы
DOC (application/msword) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_character_count`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_page_count`<br/>`metadata_word_count` | Извлечение текста, включая внедренные документы
XLSX (application/vnd.openxmlformats-officedocument.spreadsheetml.sheet) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Извлечение текста, включая внедренные документы
XLS (application/vnd.ms-excel) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified` | Извлечение текста, включая внедренные документы
PPTX (application/vnd.openxmlformats-officedocument.presentationml.presentation) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Извлечение текста, включая внедренные документы
PPT (application/vnd.ms-powerpoint) | `metadata_content_type`<br/>`metadata_author`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_slide_count`<br/>`metadata_title` | Извлечение текста, включая внедренные документы
MSG (application/vnd.ms-outlook) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_message_bcc`<br/>`metadata_creation_date`<br/>`metadata_last_modified`<br/>`metadata_subject` | Извлечение текста, включая вложения
ZIP (application/zip) | .`metadata_content_type` | Извлечение текста из всех документов в архиве
XML (application/xml) | `metadata_content_type`</br>`metadata_content_encoding`</br> | Удаление разметки XML и извлечение текста
JSON (application/json) | `metadata_content_type`</br>`metadata_content_encoding` | Извлечение текста <br/> Примечание. Инструкции по извлечению нескольких полей документа из большого двоичного объекта JSON см. в статье [Индексирование BLOB-объектов JSON с помощью индексатора BLOB-объектов службы поиска Azure](search-howto-index-json-blobs.md)
EML (message/rfc822) | `metadata_content_type`<br/>`metadata_message_from`<br/>`metadata_message_to`<br/>`metadata_message_cc`<br/>`metadata_creation_date`<br/>`metadata_subject` | Извлечение текста, включая вложения
Обычный текст (text/plain) | `metadata_content_type`</br>`metadata_content_encoding`</br> | 

<a name="CustomMetadataControl"></a>
## Использование пользовательских метаданных для управления извлечением документов

Можно добавить свойства метаданных в большой двоичный объект, чтобы управлять некоторыми аспектами индексирования больших двоичных объектов и извлечения документов. В настоящее время поддерживаются следующие свойства.

Имя свойства | Значение свойства | Пояснение
--------------|----------------|------------
AzureSearch\_Skip | True | Указывает, что индексатор должен полностью пропустить большой двоичный объект. Не будут извлекаться ни метаданные, ни содержимое. Это полезно, если требуется пропустить определенные типы содержимого или когда обработка определенного большого двоичного объекта постоянно завершается сбоем и индексирование прерывается.
AzureSearch\_SkipContent | True | Указывает, что индексатор больших двоичных объектов должен индексировать только метаданные и не должен извлекать содержимое большого двоичного объекта. Это полезно в том случае, если содержимое больших двоичных объектов не слишком интересно, но метаданные, присоединенные к большому двоичном объекту, индексировать по-прежнему необходимо.

<a name="IndexerParametersConfigurationControl"></a>
## Использование параметров индексатора для управления извлечением документов

Имеется несколько параметров конфигурации индексатора, которые позволяют контролировать, какие большие двоичные объекты и какие части содержимого большого двоичного объекта и метаданных будут индексироваться.

### Индексация только BLOB-объектов с определенными расширениями файлов

Параметр конфигурации индексатора `indexedFileNameExtensions` позволяет индексировать большие двоичные объекты только с указанными расширениями имен файлов. Значение представлено строкой, содержащей разделенный запятыми список расширений файлов (с предшествующей точкой). Например, чтобы индексировать только большие двоичные объекты PDF и DOCX, выполните следующую команду:

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "indexedFileNameExtensions" : ".pdf,.docx" } }
	}

### Исключение из индексации BLOB-объектов с определенными расширениями файлов

Большие двоичные объекты с определенными расширениями файлов можно исключить из индексации с помощью параметра конфигурации `excludedFileNameExtensions`. Значение представлено строкой, содержащей разделенный запятыми список расширений файлов (с предшествующей точкой). Например, для индексации всех больших двоичных объектов, кроме объектов с расширениями PNG и JPEG, выполните следующую команду:

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "excludedFileNameExtensions" : ".png,.jpeg" } }
	}

Если присутствуют оба параметра `indexedFileNameExtensions` и `excludedFileNameExtensions`, то Azure сначала выполняет поиск по `indexedFileNameExtensions`, а затем по `excludedFileNameExtensions`. Это означает, что если одно расширение файла присутствует в обоих списках, объект будет исключен из индексации.

### Индексация только метаданных хранилища

Вы можете настроить индексацию только метаданных хранилища и полностью пропустить процесс извлечения документа, используя свойство конфигурации `indexStorageMetadataOnly`. Это удобно в тех случаях, когда содержимое документа, а также какие-либо свойства метаданных содержимого определенного типа не требуются. Для этого задайте для свойства `indexStorageMetadataOnly` значение `true`.

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "indexStorageMetadataOnly" : true } }
	}

### Индексация метаданных хранилища и типа содержимого с пропуском извлечения содержимого

Если необходимо извлечь все метаданные, но пропустить извлечение содержимого всех больших двоичных объектов, это можно сделать с помощью конфигурации индексатора, вместо того чтобы добавлять метаданные `AzureSearch_SkipContent` в каждый большой двоичный объект по отдельности. Для этого задайте для свойства конфигурации индексатора `skipContent` значение `true`.

	PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2015-02-28-Preview
	Content-Type: application/json
	api-key: [admin key]

	{
	  ... other parts of indexer definition
	  "parameters" : { "configuration" : { "skipContent" : true } }
	}

## Помогите нам усовершенствовать службу поиска Azure

Если вам нужна какая-либо функция или у вас есть идеи, которые можно было бы реализовать, сообщите об этом на [сайте UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

<!---HONumber=AcomDC_0810_2016-->