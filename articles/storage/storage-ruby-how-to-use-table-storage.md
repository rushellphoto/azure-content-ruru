<properties
	pageTitle="Использование табличного хранилища Azure в Ruby | Microsoft Azure"
	description="Хранение структурированных данных в облаке в хранилище таблиц Azure (хранилище данных NoSQL)."
	services="storage"
	documentationCenter="ruby"
	authors="rmcmurray"
	manager="wpickett"
	editor=""/>
<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="ruby"
	ms.topic="article"
	ms.date="06/24/2016"
	ms.author="robmcm"/>


# Использование табличного хранилища Azure в Ruby

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## Обзор

В этом руководстве показано, как реализовать типичные сценарии с использованием службы таблиц Azure. Примеры написаны с помощью Ruby API. Здесь описаны такие сценарии, как **создание и удаление таблицы, вставка и запрос сущностей в таблице**.

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Создание приложения Ruby

Инструкции о том, как создать приложение Ruby, см. в разделе [Веб-приложение Ruby on Rails на виртуальной машине Azure](../virtual-machines/virtual-machines-linux-classic-ruby-rails-web-app.md).


## Настройка приложения для доступа к хранилищу

Чтобы использовать службу хранилища Azure, скачайте пакет Azure для Ruby, который содержит набор библиотек, взаимодействующих со службами REST хранилища.

### Использование RubyGems для получения пакета

1. Используйте интерфейс командной строки, например **PowerShell** (Windows), **Terminal** (Mac) или **Bash** (Unix).

2. Введите **gem install azure** в окне командной строки, чтобы установить пакеты и зависимости.

### Импорт пакета

С помощью любого текстового редактора добавьте указанный код в начало файла Ruby, где планируется использовать службу хранилища.

	require "azure"

## Настройка подключения к службе хранилища Azure

Модуль Azure считывает переменные среды **AZURE\_STORAGE\_ACCOUNT** и **AZURE\_STORAGE\_ACCESS\_KEY**, чтобы получить информацию, необходимую для подключения к учетной записи хранения Azure. Если эти переменные среды не заданы, необходимо указать сведения об учетной записи перед использованием **Azure::TableService** с помощью следующего кода:

	Azure.config.storage_account_name = "<your azure storage account>"
	Azure.config.storage_access_key = "<your azure storage access key>"

Вот как можно получить эти значения из классический учетной записи хранения или учетной записи хранения Resource Manager на портале Azure.

1. Войдите на [портал Azure](https://portal.azure.com).
2. Перейдите к учетной записи хранения, которая будет использоваться.
3. В колонке "Параметры" справа щелкните **Ключи доступа**.
4. В колонке "Ключи доступа" вы увидите ключи доступа 1 и 2. Можно использовать любой из них.
5. Щелкните значок копирования, чтобы скопировать ключ в буфер обмена.

Вот как можно получить эти значения из классический учетной записи хранения на классическом портале Azure.

1. Войдите на [классический портал Azure](https://manage.windowsazure.com).
2. Перейдите к учетной записи хранения, которая будет использоваться.
3. Щелкните **УПРАВЛЕНИЕ КЛЮЧАМИ ДОСТУПА** в нижней части области навигации.
4. В открывшемся диалоговом окне вы увидите имя учетной записи хранения, первичный ключ доступа и вторичный ключ доступа. В качестве ключа доступа можно использовать либо первичный, либо вторичный ключ.
5. Щелкните значок копирования, чтобы скопировать ключ в буфер обмена.

## Создание таблицы

Объект **Azure::TableService** позволяет работать с таблицами и сущностями. Чтобы создать таблицу, используйте метод **create\_table()**. В следующем примере создается таблица или выводится ошибка, если она возникла.

	azure_table_service = Azure::TableService.new
	begin
	  azure_table_service.create_table("testtable")
	rescue
	  puts $!
	end

## Добавление сущности в таблицу

Чтобы добавить сущность, сначала создайте хэш-объект, который определяет свойства сущности. Обратите внимание, что для каждой сущности необходимо указать **PartitionKey** и **RowKey**. Это уникальные идентификаторы сущностей, которые можно запросить гораздо быстрее, чем для других свойств. Служба хранилища Azure использует **PartitionKey**, чтобы автоматически распространять сущности таблицы на множество узлов хранилища. Сущности с одним значением **PartitionKey** хранятся на одном узле. **RowKey** — это уникальный идентификатор сущности в разделе, которому она принадлежит.

	entity = { "content" => "test entity",
	  :PartitionKey => "test-partition-key", :RowKey => "1" }
	azure_table_service.insert_entity("testtable", entity)

## Обновление сущности

Для обновления имеющейся сущности доступно несколько методов:

* **update\_entity():** обновление имеющейся сущности путем ее замены.
* **merge\_entity():** обновление сущности путем объединения новых значений свойств с имеющейся сущностью.
* **insert\_or\_merge\_entity():** обновление имеющейся сущности путем ее замены. Если сущность не существует, будет вставлена новая сущность.
* **insert\_or\_replace\_entity():** обновление сущности путем объединения новых значений свойств с имеющейся сущностью. Если сущность не существует, будет вставлена новая сущность.

В следующем примере показано обновление сущности с помощью метода **update\_entity()**:

	entity = { "content" => "test entity with updated content",
	  :PartitionKey => "test-partition-key", :RowKey => "1" }
	azure_table_service.update_entity("testtable", entity)

Если сущность, которая обновляется, не существует, при использовании **update\_entity()** и **merge\_entity()** операция обновления завершается ошибкой. Поэтому если вам необходимо сохранить сущность независимо от того, существует ли она, следует использовать **insert\_or\_replace\_entity()** или **insert\_or\_merge\_entity()**.

## Работа с группами сущностей

Иногда имеет смысл отправлять совместно несколько операций в пакете для атомарной обработки сервером. Для этого сначала требуется создать объект **Batch**, а затем использовать метод **execute\_batch()** для **TableService**. В следующем примере показана отправка двух сущностей с RowKey 2 и 3 в пакете. Обратите внимание, что пример работает только для сущностей с одинаковым значением PartitionKey.

	azure_table_service = Azure::TableService.new
	batch = Azure::Storage::Table::Batch.new("testtable",
	  "test-partition-key") do
	  insert "2", { "content" => "new content 2" }
	  insert "3", { "content" => "new content 3" }
	end
	results = azure_table_service.execute_batch(batch)

## Запрос сущности

Для запроса сущности в таблице используйте метод **get\_entity()**, передав имя таблицы, **PartitionKey** и **RowKey**.

	result = azure_table_service.get_entity("testtable", "test-partition-key",
	  "1")

## Запрос набора сущностей

Для запроса набора сущностей в таблице создайте хэш-объект запроса и используйте метод **query\_entities()**. В следующем примере показано, как получить все сущности с одним значением **PartitionKey**:

	query = { :filter => "PartitionKey eq 'test-partition-key'" }
	result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] Если полученный в результате набор слишком велик и не может быть возвращен в одном запросе, возвращается токен продолжения, который можно использовать для извлечения последующих страниц.

## Запрос подмножества свойств сущности

Запрос к таблице может получить лишь несколько свойств сущности. Этот метод, который называется "проекцией", снижает потребление пропускной способности и может повысить производительность запросов, особенно для крупных сущностей. Используйте предложение select и передайте имена свойств, которые необходимо передать клиенту.

	query = { :filter => "PartitionKey eq 'test-partition-key'",
	  :select => ["content"] }
	result, token = azure_table_service.query_entities("testtable", query)

## Удаление сущности

Для удаления сущности используйте метод **delete\_entity()**. Необходимо передать имя таблицы, содержащей сущность, а также свойства PartitionKey и RowKey сущности.

		azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## Удаление таблицы

Для удаления таблицы используйте метод **delete\_table()** и передайте имя таблицы, которую нужно удалить.

		azure_table_service.delete_table("testtable")

## Дальнейшие действия

Дополнительную информацию о выполнении более сложных задач хранения см. по указанным ниже ссылкам.

- [Блог рабочей группы службы хранилища Azure](http://blogs.msdn.com/b/windowsazurestorage/)
- Репозиторий [пакетов Azure SDK для Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) на веб-сайте GitHub

<!---HONumber=AcomDC_0629_2016-->