<properties
 pageTitle="Исходящая проверка подлинности планировщика"
 description="Исходящая проверка подлинности планировщика"
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="06/30/2016"
 ms.author="krisragh"/>

# Исходящая проверка подлинности планировщика

Задания планировщика могут вызывать службы, требующие проверки подлинности. В этом случае вызванная служба определяет, может ли задание планировщика получить доступ к ее ресурсам. В число таких служб входят другие службы Azure, Salesforce.com, Facebook и защищенные пользовательские веб-сайты.

## Добавление и удаление проверки подлинности

Добавить в задание планировщика проверку подлинности очень просто: при создании или обновлении задания добавьте в элемент `request` дочерний элемент JSON `authentication`. Секретные данные, передаваемые службе планировщика запросами PUT, POST или PATCH в составе объекта `authentication`, в ответах не отображаются. В ответах секретная информация обнуляется или получает открытый маркер, который представляет сущность, прошедшую проверку подлинности.

Чтобы удалить проверку подлинности, примените непосредственно к заданию запрос PUT или PATCH, установив для объекта `authentication` нулевое значение. Никакие свойства проверки подлинности в ответе показаны не будут.

В настоящее время поддерживаются только следующие типы проверки подлинности: модель `ClientCertificate` (для использования клиентских SSL/TLS-сертификатов), модель `Basic` (для обычной проверки подлинности) и модель `ActiveDirectoryOAuth` (для проверки подлинности Active Directory OAuth.)

## Текст запроса для проверки подлинности ClientCertificate

При добавлении проверки подлинности с использованием модели `ClientCertificate` укажите в тексте запроса следующие дополнительные элементы.

|Элемент|Описание|
|:---|:---|
|_authentication (родительский элемент)_|Объект проверки подлинности для использования клиентского SSL-сертификата.|
|_type_|обязательный параметр. Тип проверки подлинности. Для клиентских SSL-сертификатов используйте значение `ClientCertificate`.|
|_pfx_|обязательный параметр. Содержимое PFX-файла с кодировкой Base64.|
|_password_|обязательный параметр. Пароль для доступа к PFX-файлу.|


## Текст ответа при проверке подлинности ClientCertificate

При отправке запроса со сведениями о проверке подлинности в ответ включаются следующие элементы.

|Элемент |Описание |
|:--|:--|
|_authentication (родительский элемент)_ |Объект проверки подлинности для использования клиентского SSL-сертификата.|
|_type_ |Тип проверки подлинности. Для клиентских SSL-сертификатов это значение равно `ClientCertificate`.|
|_certificateThumbprint_ |Отпечаток сертификата.|
|_certificateSubjectName_ |Различающееся имя субъекта сертификата.|
|_certificateExpiration_ |Срок действия сертификата.|

## Текст запроса для обычной проверки подлинности

При добавлении проверки подлинности с использованием модели `Basic` укажите в тексте запроса следующие дополнительные элементы.

|Элемент|Описание|
|:--|:--|
|_authentication (родительский элемент)_ |Объект проверки подлинности для использования обычной проверки подлинности.|
|_type_ |обязательный параметр. Тип проверки подлинности. Для обычной проверки подлинности используйте значение `Basic`.|
|_username_ |обязательный параметр. Имя пользователя для проверки подлинности.|
|_password_ |обязательный параметр. Пароль для проверки подлинности.|

## Текст ответа при обычной проверке подлинности

При отправке запроса со сведениями о проверке подлинности в ответ включаются следующие элементы.

|Элемент|Описание|
|:--|:--|
|_authentication (родительский элемент)_ |Объект проверки подлинности для использования обычной проверки подлинности.|
|_type_ |Тип проверки подлинности. Для обычной проверки подлинности это значение равно `Basic`.|
|_username_ |Имя пользователя, прошедшего проверку подлинности.|

## Текст запроса для проверки подлинности ActiveDirectoryOAuth

При добавлении проверки подлинности с использованием модели `ActiveDirectoryOAuth` укажите в тексте запроса следующие дополнительные элементы.

|Элемент |Описание |
|:--|:--|
|_authentication (родительский элемент)_ |Объект проверки подлинности для использования проверки подлинности ActiveDirectoryOAuth.|
|_type_ |обязательный параметр. Тип проверки подлинности. Для проверки подлинности ActiveDirectoryOAuth используйте значение `ActiveDirectoryOAuth`.|
|_tenant_ |обязательный параметр. Идентификатор клиента Azure AD.|
|_audience_ |обязательный параметр. Это свойство имеет значение https://management.core.windows.net/.|.
|_clientid_ |обязательный параметр. Укажите идентификатор клиента для приложения Azure AD.|
|_secret_ |обязательный параметр. Секретные данные клиента, запрашивающего маркер.|

### Определение идентификатора клиента

Идентификатор клиента Azure AD можно узнать, выполнив команду `Get-AzureAccount` в Azure PowerShell.

## Текст ответа при проверке подлинности ActiveDirectoryOAuth

При отправке запроса со сведениями о проверке подлинности в ответ включаются следующие элементы.

|Элемент |Описание |
|:--|:--|
|_authentication (родительский элемент)_ |Объект проверки подлинности для использования проверки подлинности ActiveDirectoryOAuth.|
|_type_ |Тип проверки подлинности. Для аутентификации ActiveDirectoryOAuth это значение равно `ActiveDirectoryOAuth`.|
|_tenant_ |Идентификатор клиента Azure AD. |
|_audience_ |Имеет значение https://management.core.windows.net/.|.
|_clientid_ |Идентификатор клиента для приложения Azure AD.|

## См. также


 [Что такое планировщик?](scheduler-intro.md)

 [Основные понятия, терминология и иерархия сущностей планировщика Azure](scheduler-concepts-terms.md)

 [Приступая к работе с планировщиком Azure на портале Azure](scheduler-get-started-portal.md)

 [Планы и выставление счетов в планировщике Azure](scheduler-plans-billing.md)

 [Справочник по API REST планировщика Azure](https://msdn.microsoft.com/library/mt629143)

 [Справочник по командлетам PowerShell планировщика Azure](scheduler-powershell-reference.md)

 [Высокая доступность и надежность планировщика Azure](scheduler-high-availability-reliability.md)

 [Ограничения, значения по умолчанию и коды ошибок планировщика Azure](scheduler-limits-defaults-errors.md)

<!---HONumber=AcomDC_0706_2016-->