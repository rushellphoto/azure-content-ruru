<properties
pageTitle="GitHub | Microsoft Azure"
description="Создание приложений логики с помощью службы приложений Azure. GitHub — это веб-служба размещения репозиториев Git. Она предоставляет все возможности распределенного управления редакциями и исходным кодом (SCM) Git, а также собственные функции."
services="logic-apps"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="05/17/2016"
ms.author="deonhe"/>

# Начало работы с соединителем GitHub



Соединитель GitHub можно использовать из таких компонентов, как:

- [Приложения логики](../app-service-logic/app-service-logic-what-are-logic-apps.md)
- [PowerApps.](http://powerapps.microsoft.com)
- [Поток](http://flows.microsoft.com)

>[AZURE.NOTE] Эта версия статьи предназначена для приложений логики со схемой версии 2015-08-01-preview.

Для начала можно создать приложение логики, как указано в соответствующей [статье](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Триггеры и действия

Соединитель GitHub можно использовать как действие. Кроме того, он имеет триггеры. Все соединители поддерживают данные в форматах JSON и XML.

 Соединитель GitHub предоставляет следующие триггеры и действия:

### Действия GitHub
Вы можете выполнять перечисленные ниже действия:

|Действие|Описание|
|--- | ---|
|[CreateIssue](connectors-create-api-github.md#createissue)|Создает вопрос|
### Триггеры GitHub
Можно прослушивать указанные ниже события:

|Триггер | Описание|
|--- | ---|
|При открытии вопроса|Вопрос открыт|
|При закрытии вопроса|Вопрос закрыт|
|При назначении вопроса|Вопрос назначен|


## Создание подключения к GitHub
Для создания приложений логики с помощью GitHub необходимо создать **подключение**, а затем указать данные для следующих свойств:

|Свойство| Обязательно|Описание|
| ---|---|---|
|токен|Да|Укажите учетные данные GitHub|
Созданное подключение можно использовать для выполнения действий и прослушивания триггеров, описанных в этой статье.

>[AZURE.INCLUDE [Шаги по созданию подключения к GitHub](../../includes/connectors-create-api-github.md)]

>[AZURE.TIP] Это подключение можно использовать в других приложениях логики.

## Справочник по GitHub
Относится к версии 1.0.

## CreateIssue
Создание вопроса: создает вопрос

```POST: /repos/{repositoryOwner}/{repositoryName}/issues```

| Имя| Тип данных|Обязательно|Местонахождение|Значение по умолчанию|Описание|
| ---|---|---|---|---|---|
|repositoryOwner|строка|Да|path|Нет|Владелец репозитория|
|repositoryName|string|Да|path|Нет|Имя репозитория|
|issueBasicDetails| |Да|текст|Нет|Сведения о вопросе|

#### Ответ

|Имя|Описание|
|---|---|
|200|ОК|
|400|Ошибка запроса|
|401|Не авторизовано|
|403|Запрещено|
|404|Не найдено|
|500|Внутренняя ошибка сервера. Произошла неизвестная ошибка|
|по умолчанию|Операция завершилась ошибкой.|


## IssueOpened
При открытии вопроса: вопрос открыт

```GET: /trigger/issueOpened```

Для этого вызова параметры отсутствуют
#### Ответ

|Имя|Описание|
|---|---|
|200|ОК|
|400|Ошибка запроса|
|401|Не авторизовано|
|403|Запрещено|
|404|Не найдено|
|500|Внутренняя ошибка сервера. Произошла неизвестная ошибка|
|по умолчанию|Операция завершилась ошибкой.|


## IssueClosed
При закрытии вопроса: вопрос закрыт

```GET: /trigger/issueClosed```

Для этого вызова параметры отсутствуют
#### Ответ

|Имя|Описание|
|---|---|
|200|ОК|
|400|Ошибка запроса|
|401|Не авторизовано|
|403|Запрещено|
|404|Не найдено|
|500|Внутренняя ошибка сервера. Произошла неизвестная ошибка|
|по умолчанию|Операция завершилась ошибкой.|


## IssueAssigned
При назначении вопроса: вопрос назначен

```GET: /trigger/issueAssigned```

Для этого вызова параметры отсутствуют
#### Ответ

|Имя|Описание|
|---|---|
|200|ОК|
|400|Ошибка запроса|
|401|Не авторизовано|
|403|Запрещено|
|404|Не найдено|
|500|Внутренняя ошибка сервера. Произошла неизвестная ошибка|
|по умолчанию|Операция завершилась ошибкой.|


## Определения объектов 

### IssueBasicDetailsModel


| Имя свойства | Тип данных | Обязательное |
|---|---|---|
|title|строка|Да |
|body|строка|Да |
|assignee|string|Да |



### IssueDetailsModel


| Имя свойства | Тип данных | Обязательное |
|---|---|---|
|title|строка|Да |
|body|строка|Да |
|assignee|string|Да |
|number|строка|Нет |
|state|строка|Нет |
|created\_at|string|Нет |
|repository\_url|string|Нет |


## Дальнейшие действия
[Создайте приложение логики](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0803_2016-->