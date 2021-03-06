<properties
    pageTitle="Добавление соединителя базы данных SQL Azure в приложения логики | Microsoft Azure"
    description="Обзор соединителя базы данных SQL Azure с параметрами интерфейса REST API"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/25/2016"
   ms.author="mandia"/>


# Начало работы с соединителем базы данных SQL Azure
С помощью соединителя базы данных SQL Azure можно создавать рабочие процессы для управления данными в таблицах в вашей организации.

С помощью базы данных SQL вы можете:

- создавать рабочие процессы путем добавления нового клиента в базу данных клиентов или обновления заказа в базе данных заказов;
- использовать действия для получения строки данных, вставки новой строки и даже ее удаления. Например, при создании записи в Dynamics CRM Online (триггер) может вставляться строка в базу данных SQL Azure (действие).

В этой статье содержатся сведения об использовании соединителя базы данных SQL в приложении логики, а также перечислены предоставляемые им действия.

>[AZURE.NOTE] Эта версия статьи предназначена для общедоступного выпуска приложений логики.

Дополнительные сведения о приложениях логики см. в статье, посвященной [приложениям логики](../app-service-logic/app-service-logic-what-are-logic-apps.md), и [руководстве по созданию приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Подключение к базе данных SQL Azure

Чтобы обеспечить доступ приложения логики к какой-либо службе, сначала необходимо создать соответствующее *подключение*. Таким образом вы установите соединение между приложением логики и другой службой. Например, чтобы подключиться к базе данных SQL, сначала необходимо создать соответствующее *подключение*. Чтобы создать подключение, нужно ввести учетные данные, которые используются для доступа к определенной службе. Таким образом, чтобы создать подключение к базе данных SQL, введите учетные данные базы данных SQL.

#### Создание подключения

>[AZURE.INCLUDE [Создание подключения к SQL Azure](../../includes/connectors-create-api-sqlazure.md)]

## Использование триггера

Этот соединитель не содержит триггеров. Чтобы запустить приложение логики, используйте другие триггеры, например триггер повторения, триггер веб-перехватчика HTTP, триггеры, доступные для других соединителей, и другие. Пример см. в статье о [создании приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Использование действий
	
Действие — это операция, выполняемая рабочим процессом, определенным в приложении логики. Дополнительные сведения о действиях см. [здесь](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Щелкните знак "плюс". Отобразятся следующие параметры: **Добавить действие**, **Добавить условие** или **Еще**.

	![](./media/connectors-create-api-sqlazure/add-action.png)

2. Выберите **Добавить действие**.

3. Чтобы открыть список всех доступных действий, в текстовом поле введите sql.

	![](./media/connectors-create-api-sqlazure/sql-1.png)

4. В нашем примере мы выберем действие для SQL Server **Get row** (Получение строки). Если подключение уже существует, выберите **имя таблицы** в раскрывающемся списке и введите **идентификатор строки**, который необходимо вернуть.

	![](./media/connectors-create-api-sqlazure/sample-table.png)

	Если появится запрос на предоставление сведений о подключении, введите их, чтобы создать подключение. Эти свойства описаны в разделе [Создание подключения](connectors-create-api-sqlazure.md#create-the-connection) этой статьи.

	> [AZURE.NOTE] В этом примере строка возвращается из таблицы. Чтобы просмотреть данные в этой строке, добавьте другое действие, которое создает файл с помощью полей из таблицы. Например, можно добавить действие OneDrive, использующее поля FirstName и LastName, чтобы создать файл в учетной записи облачного хранилища.

5. **Сохраните** изменения, нажав соответствующую кнопку в левом верхнем углу панели инструментов. Приложение логики сохранено и теперь может быть включено автоматически.


## Технические сведения

## Действия базы данных SQL
Действие — это операция, выполняемая рабочим процессом, определенным в приложении логики. Соединитель базы данных SQL позволяет выполнять следующие действия.

|Действие|Описание|
|--- | ---|
|[ExecuteProcedure](connectors-create-api-sqlazure.md#execute-stored-procedure)|Выполняет хранимую процедуру в SQL|
|[GetRow](connectors-create-api-sqlazure.md#get-row)|Получает одну строку из таблицы SQL|
|[GetRows](connectors-create-api-sqlazure.md#get-rows)|Извлекает строки из таблицы SQL|
|[InsertRow](connectors-create-api-sqlazure.md#insert-row)|Вставляет новую строку в таблицу SQL|
|[DeleteRow](connectors-create-api-sqlazure.md#delete-row)|Удаляет строку из таблицы SQL|
|[GetTables](connectors-create-api-sqlazure.md#get-tables)|Получает таблицы из базы данных SQL|
|[UpdateRow](connectors-create-api-sqlazure.md#update-row)|Изменяет существующую строку в таблице SQL|

### Сведения о действиях

В этом разделе приведены сведения о каждом действии, включая обязательные и необязательные входные свойства, а также соответствующие выходные данные, связанные с соединителем.


#### Выполнение хранимой процедуры
Выполняет хранимую процедуру в SQL.

| Имя свойства| Отображаемое имя |Описание|
| ---|---|---|
|procedure* | Имя процедуры | Имя хранимой процедуры, которую необходимо выполнить |
|parameters* | Входные параметры | Параметры динамичны и зависят от выбранной хранимой процедуры. <br/><br/> Например, если используется образец базы данных Adventure Works, необходимо выбрать хранимую процедуру *ufnGetCustomerInformation*. Отобразится входной параметр **Идентификатор клиента**. Введите "6" или один из других идентификаторов клиента |

Звездочка (*) означает, что свойство является обязательным.

##### Сведения о выходных данных
ProcedureResult: содержит результат выполнения хранимой процедуры.

| Имя свойства | Тип данных | Описание |
|---|---|---|
|OutputParameters|object|Значения выходных параметров |
|ReturnCode|целое число|Код возврата процедуры |
|ResultSets|object| Результирующие наборы|


#### Получение строки 
Получает одну строку из таблицы SQL.

| Имя свойства| Отображаемое имя |Описание|
| ---|---|---|
|table* | Имя таблицы |Имя таблицы SQL|
|id* | Идентификатор строки |Уникальный идентификатор извлекаемой строки|

Звездочка (*) означает, что свойство является обязательным.

##### Сведения о выходных данных
Элемент

| Имя свойства | Тип данных |
|---|---|
|ItemInternalId|string|


#### Получение строк 
Извлекает строки из таблицы SQL.

|Имя свойства| Отображаемое имя|Описание|
| ---|---|---|
|table*|Имя таблицы|Имя таблицы SQL|
|$skip|Число пропусков|Количество пропускаемых записей (значение по умолчанию — 0)|
|$top|Максимальное число записей|Максимальное количество извлекаемых записей (значение по умолчанию — 256)|
|$filter|Запрос фильтра|Запрос фильтра ODATA для ограничения количества записей|
|$orderby|Упорядочить по|Запрос orderBy ODATA для указания порядка записей|

Звездочка (*) означает, что свойство является обязательным.

##### Сведения о выходных данных
ItemsList

| Имя свойства | Тип данных |
|---|---|
|value|array|


#### Вставка строки 
Вставляет новую строку в таблицу SQL.

|Имя свойства| Отображаемое имя|Описание|
| ---|---|---|
|table*|Имя таблицы|Имя таблицы SQL|
|item*|Строка|Строка, вставляемая в указанную таблицу в SQL|

Звездочка (*) означает, что свойство является обязательным.

##### Сведения о выходных данных
Элемент

| Имя свойства | Тип данных |
|---|---|
|ItemInternalId|string|


#### Удаление строки 
Удаляет строку из таблицы SQL.

|Имя свойства| Отображаемое имя|Описание|
| ---|---|---|
|table*|Имя таблицы|Имя таблицы SQL|
|id*|Идентификатор строки|Уникальный идентификатор удаляемой строки|

Звездочка (*) означает, что свойство является обязательным.

##### Сведения о выходных данных
Нет.

#### Получение таблиц 
Получает таблицы из базы данных SQL.

Для этого вызова параметры отсутствуют.

##### Сведения о выходных данных 
TablesList

| Имя свойства | Тип данных |
|---|---|
|value|array|

#### Обновление строки 
Обновляет имеющуюся строку в таблице SQL.

|Имя свойства| Отображаемое имя|Описание|
| ---|---|---|
|table*|Имя таблицы|Имя таблицы SQL|
|id*|Идентификатор строки|Уникальный идентификатор обновляемой строки|
|item*|Строка|Строка с обновленными значениями|

Звездочка (*) означает, что свойство является обязательным.

##### Сведения о выходных данных  
Элемент

| Имя свойства | Тип данных |
|---|---|
|ItemInternalId|string|


### Ответы HTTP

В результате вызова различных действий могут возвращаться определенные ответы. В следующей таблице приведены ответы, а также их описания.

|Имя|Описание|
|---|---|
|200|ОК|
|202|Принято|
|400|Ошибка запроса|
|401|Не авторизовано|
|403|Запрещено|
|404|Не найдено|
|500|Внутренняя ошибка сервера. Произошла неизвестная ошибка|
|по умолчанию|Операция завершилась ошибкой.|


## Дальнейшие действия

См. статью о [создании приложения логики](../app-service-logic/app-service-logic-create-a-logic-app.md). Чтобы узнать, какие еще соединители доступны в Logic Apps, см. [список API-интерфейсов](apis-list.md).

<!---HONumber=AcomDC_0727_2016-->