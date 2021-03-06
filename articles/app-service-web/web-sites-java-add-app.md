<properties 
	pageTitle="Добавление приложения Java в веб-приложения службы приложений Azure" 
	description="В этом учебнике показано, как добавить страницу или приложение в экземпляр веб-приложений службы приложений Azure, который уже настроен на использование Java." 
	services="app-service\web" 
	documentationCenter="java" 
	authors="rmcmurray" 
	manager="wpickett" 
	editor=""/>

<tags 
	ms.service="app-service-web" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="Java" 
	ms.topic="article" 
	ms.date="06/24/2016" 
	ms.author="robmcm"/>

# Добавление приложения Java в веб-приложения службы приложений Azure

После инициализации веб-приложения в [службе приложений Azure][], как описано в разделе [Создание веб-приложения Java в службе приложений Azure](web-sites-java-get-started.md), приложение можно передать, поместив WAR-файл в папку **webapps**.

Путь к папке **webapps** зависит от настроек экземпляра веб-приложений.

- Если веб-приложение настроено с использованием Магазина Azure, то папка **webapps** находится по пути **d:\\home\\site\\wwwroot\\bin\\application\_server\\webapps**, где **application\_server** — имя сервера приложений в экземпляре веб-приложений.
- После настройки веб-приложения в пользовательском интерфейсе Azure путь к папке **webapps** будет иметь вид **d:\\home\\site\\wwwroot\\webapps**.

Обратите внимание, что для отправки веб-страниц или приложения можно использовать систему управления версиями, в том числе в [сценариях непрерывной интеграции](app-service-continuous-deployment.md). Загружать приложения или веб-страницы можно также с помощью FTP.

Примечание в отношении веб-приложений Tomcat. Сразу после загрузки WAR-файла в папку **webapps** сервер приложений Tomcat определит это и автоматически загрузит файл. Обратите внимание, что при копировании файлов (кроме WAR-файлов) в КОРНЕВОЙ каталог необходимо перезапустить сервер приложений, прежде чем использовать эти файлы. Работа функции автозагрузки для запущенных в Azure веб-приложений Java Tomcat основана на добавлении нового WAR-файла либо новых файлов или каталогов в папку **webapps**.

## Дальнейшие действия

Дополнительную информацию см. в [Центре разработчика Java](/develop/java/).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- External Links -->
[службе приложений Azure]: http://go.microsoft.com/fwlink/?LinkId=529714

<!---HONumber=AcomDC_0803_2016-->