<properties
	pageTitle="Проверка подлинности пользователей для приложений API в службе приложений Azure | Microsoft Azure"
	description="Узнайте, как защитить приложение API в службе приложений Azure путем предоставления доступа только тем пользователям, которые прошли проверку подлинности."
	services="app-service\api"
	documentationCenter=".net"
	authors="tdykstra"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-api"
	ms.workload="na"
	ms.tgt_pltfrm="dotnet"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/30/2016"
	ms.author="rachelap"/>

# Проверка подлинности пользователя для приложений API в службе приложений Azure

## Обзор

В этой статье показано, как защитить приложение API Azure, чтобы к нему могли обращаться только пользователи, прошедшие проверку подлинности. В статье предполагается, что вы уже ознакомились со статьей [Проверка подлинности и авторизация в службе приложений Azure](../app-service/app-service-authentication-overview.md).

Вы узнаете следующее:

* Как настроить поставщик проверки подлинности с помощью сведений для Azure Active Directory (Azure AD).
* Как использовать защищенные приложения API с помощью [библиотеки проверки подлинности Active Directory (ADAL) для JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Статья содержит два раздела.

* В разделе [Настройка проверки подлинности пользователей в службе приложений Azure](#authconfig) содержатся общие инструкции по настройке проверки подлинности пользователя для любого приложения API. Они одинаково применимы ко всем платформам, которые поддерживаются службой приложений, включая .NET, Node.js и Java.

* В этой статье, которая начинается с раздела [Продолжение руководств по началу работы с .NET](#tutorialstart), содержатся инструкции по настройке примера приложения с серверной частью .NET и интерфейсной частью AngularJS для использования Azure Active Directory для проверки подлинности пользователя.

## <a id="authconfig"></a> Настройка проверки подлинности пользователей в службе приложений Azure

Этот раздел содержит общие инструкции, которые применимы к любому приложению API. Конкретные инструкции для примера приложения .NET To Do List см. в разделе [Продолжение учебников по началу работы с .NET](#tutorialstart).

1. На [портале Azure](https://portal.azure.com/) перейдите к колонке **Параметры** приложения API, которое требуется защитить, найдите раздел **Функции** и щелкните **Проверка подлинности и авторизация**.

	![Проверка подлинности и авторизация на портале Azure](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. В колонке **Проверка подлинности и авторизация** щелкните **Вкл.**

4. Выберите один из вариантов в раскрывающемся списке **Предпринимаемое действие, если проверка подлинности для запроса не выполнена**.

	* Если требуется, чтобы приложения API достигали только прошедшие проверку вызовы, выберите один из вариантов **входа в систему**. Этот параметр позволяет защитить приложение API без написания кода, который выполняется в нем.

	* Если требуется, чтобы все вызовы достигали приложения API, выберите **Разрешить запрос (нет действия)**. С помощью этого параметра можно направлять непроверенные вызывающие стороны к различным поставщикам проверки подлинности. В этом случае необходимо написать код для обработки авторизации.

	Дополнительные сведения см. в статье [Проверка подлинности и авторизация для приложений API в службе приложений Azure](app-service-api-authentication.md#multiple-protection-options).

5. Выберите один или несколько **поставщиков проверки подлинности**.

	На изображении показаны параметры, при выборе которых все вызывающие стороны должны проходить проверку подлинности в Azure AD.
 
	![Колонка "Проверка подлинности и авторизация" на портале Azure](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

	Если выбрать поставщик проверки подлинности, на портале отобразится колонка конфигурации для данного поставщика.

	Подробные инструкции, в которых объясняется, как настроить колонки конфигурации для поставщика проверки подлинности, см. в статье [Настройка приложения службы приложений для использования службы входа Azure Active Directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Ссылка указывает на статью об Azure AD, но сама статья содержит вкладки, ведущие на статьи, посвященные другим поставщикам проверки подлинности).

7. Закончив настройку колонки поставщика проверки подлинности, нажмите кнопку **ОК**.

7. В колонке **Проверка подлинности и авторизация** нажмите кнопку **Сохранить**.

После этого служба приложений начнет проверять подлинность всех вызовов API, прежде чем они достигнут приложения API. Службы проверки подлинности работают одинаково для всех языков, которые поддерживает служба приложений, в том числе .NET, Node.js и Java.

Чтобы выполнять проверенные на подлинность вызовы API, вызывающая сторона должна включать токен носителя OAuth 2.0 поставщика проверки подлинности в заголовок авторизации HTTP-запросов. Этот токен можно получить с помощью пакета SDK поставщика проверки подлинности.

## <a id="tutorialstart"></a> Продолжение руководств по началу работы с .NET

Если вы изучаете серию учебников для приложений API по работе с Node.js или Java, перейдите к статье [Проверка подлинности с использованием субъекта-службы для приложений API](app-service-api-dotnet-service-principal-auth.md).

Если вы изучаете серию учебников для приложений API по работе с .NET и уже развернули пример приложения в соответствии с [первым](app-service-api-dotnet-get-started.md) и [вторым](app-service-api-cors-consume-javascript.md) учебниками, перейдите к разделу [Настройка проверки подлинности в службе приложений и Azure AD](#azureauth).

Если вы хотите применить инструкции этого учебника, не изучая первый и второй учебники, выполните следующие действия, в которых объясняется, как приступить к работе с помощью автоматизированного процесса для развертывания примера приложения.

>[AZURE.NOTE] Выполнив эти шаги, вы сделаете все, что предусмотрено в первых двух учебниках, с одним исключением: Visual Studio не будет знать, в какое именно веб-приложение или приложение API будет развертываться каждый проект. Это означает, что в этом учебнике не будет точных инструкций, объясняющих как выполнять развертывание в определенные целевые объекты. Если вам неудобно самостоятельно определять шаги развертывания, рекомендуется следовать пошаговым инструкциям из [первого учебника](app-service-api-dotnet-get-started.md), а не начинать выполнение автоматического процесса развертывания.

1. Выполните все предварительные требования, перечисленные в [первом учебнике](app-service-api-dotnet-get-started.md). Помимо указанных предварительных требований для работы с этими учебниками требуется опыт работы с веб-приложениями службы приложений и приложениями API в Visual Studio и на портале Azure.

2. Нажмите кнопку **Развертывание в Azure** в [файле readme примера репозитория To Do List](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md), чтобы развернуть приложения API и веб-приложение. Запомните созданную группу ресурсов Azure — впоследствии ее можно будет использовать для поиска имен веб-приложения и приложения API.
 
3. Загрузите или клонируйте [пример репозитория To Do List](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list), чтобы получить код для локального использования в Visual Studio.

## <a id="azureauth"></a> Настройка проверки подлинности в службе приложений и Azure AD

На данный момент ваше приложение работает в службе приложений Azure, и проверка подлинности пользователей не требуется. В этом разделе мы добавим проверку подлинности, выполнив следующие задачи.

* Настроим в службе приложений требование проверки подлинности в Azure Active Directory (Azure AD) для вызова приложения API среднего уровня.
* Создадим приложение Azure AD.
* Настроим приложение Azure AD для отправки токена носителя после входа во внешнее приложение AngularJS.

Если при выполнении указаний в учебнике у вас возникли проблемы, см. раздел [Устранение неполадок](#troubleshooting) в конце этого учебника.
 
### Настройка проверки подлинности для приложения API среднего уровня

1. На [портале Azure](https://portal.azure.com/) перейдите к колонке **Параметры** приложения API, созданного для проекта ToDoListAPI, найдите раздел **Функции** и щелкните **Проверка подлинности и авторизация**.

	![Проверка подлинности и авторизация на портале Azure](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. В колонке **Проверка подлинности и авторизация** щелкните **Вкл.**

4. В раскрывающемся списке **Действие, выполняемое, если запрос не прошел проверку подлинности** выберите **Войти с помощью Azure Active Directory**.

	Этот параметр гарантирует, что запросы, не прошедшие проверку подлинности, не достигнут приложения API.

5. В разделе **Поставщики проверки подлинности** щелкните **Azure Active Directory**.

	![Колонка "Проверка подлинности и авторизация" на портале Azure](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. В колонке **Параметры Azure Active Directory** щелкните **Автоматическое**.

	![Параметр "Экспресс" для проверки подлинности и авторизации на портале Azure](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

	Если выбран параметр **Экспресс**, служба приложений может автоматически создать приложение Azure AD в [клиенте](https://msdn.microsoft.com/ru-RU/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) Azure AD.

	Вам не нужно создавать клиент, так как каждая учетная запись Azure автоматически содержит клиент.

7. В разделе **Режим управления** нажмите кнопку **Создать приложение AD** (если она еще не нажата) и обратите внимание на значение, которое находится в поле **Создание приложения** — вы будете искать это приложение AAD на классическом портале Azure позднее.

	![Параметры Azure AD на портале Azure](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

	Azure автоматически создаст приложение Azure AD с указанным именем в клиенте Azure AD. По умолчанию приложение Azure AD называется так же, как приложение API. Вы можете ввести другое имя.
 
7. Нажмите кнопку **ОК**.

7. В колонке **Проверка подлинности и авторизация** нажмите кнопку **Сохранить**.

	![Щелкните Сохранить](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Теперь приложение API смогут вызвать только пользователи в клиенте Azure AD.

### Тестирование приложения API (необязательно)

1. В браузере перейдите по URL-адресу приложения API. Для этого в колонке **Приложение API** на портале Azure щелкните ссылку в разделе **URL-адрес**.

	Вы будете перенаправлены на экран входа, так как запросы, не прошедшие проверку подлинности, не могут получать доступ к приложению API.

	Если браузер перенаправляет вас на страницу с сообщением "Успешно создано", возможно, он уже вошел в приложение. В таком случае откройте окно InPrivate или анонимное окно и перейдите по URL-адресу приложения API.

2. Войдите с помощью учетных данных, используемых в клиенте Azure AD.

	Когда вы войдете, в браузере откроется страница с сообщением "Успешно создано".

9. Закройте браузер.

### Настройка приложения Azure AD

При настройке проверки подлинности в Azure AD служба приложений автоматически создала приложение Azure AD. По умолчанию новое приложение Azure AD было настроено для предоставления токена носителя URL-адресу приложения API. В этом разделе мы настроим приложение Azure AD таким образом, чтобы токен носителя предоставлялся внешнему приложению AngularJS, а не непосредственно приложению API среднего уровня. Внешнее приложение AngularJS отправит токен приложению API при вызове приложения API.

>[AZURE.NOTE] Чтобы процесс оставался максимально простым, в этом учебнике используется одно приложение Azure AD для интерфейсного приложения API и приложения API среднего уровня. Другим вариантом является использование двух приложений Azure AD. В этом случае потребуется предоставить приложению Azure AD внешнего интерфейса разрешение на вызов приложения Azure AD среднего уровня. Такой подход, в котором задействовано несколько приложений, обеспечивает более точный контроль разрешений для каждого уровня.

11. На [классическом портале Azure](https://manage.windowsazure.com/) перейдите в раздел **Azure Active Directory**.

	Нужно использовать именно классический портал, так как в текущей версии портала Azure еще не доступны некоторые параметры Azure Active Directory, которые нам нужны.

12. На вкладке **Каталог** щелкните свой клиент AAD.

	![Azure AD на классическом портале](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Щелкните **Приложения > Приложения, принадлежащие моей компании**, а затем установите флажок.

	Кроме того, может потребоваться обновить страницу, чтобы увидеть новое приложение.

15. В списке приложений щелкните имя приложения, созданного в Azure после того, как вы активировали проверку подлинности для приложения API.

	![Вкладка "Приложения Azure AD"](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Нажмите **Настроить**.

	![Вкладка "Настройка Azure AD"](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. Для параметра **URL-адрес входа** укажите URL-адрес веб-приложения AngularJS без косой черты в конце.

	Например: https://todolistangular.azurewebsites.net

	![URL-адрес входа](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. Для параметра **URL-адрес ответа** укажите URL-адрес веб-приложения без косой черты в конце.

	Например: https://todolistsangular.azurewebsites.net

16. Щелкните **Сохранить**.

	![URL-адрес ответа](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. В нижней части страницы щелкните **Управление манифестом > Загрузить манифест**.

	![Загрузить манифест](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Загрузите файл в папку, где его можно будет отредактировать.

16. В скачанном файле манифеста найдите свойство `oauth2AllowImplicitFlow`. Измените значение этого свойства с `false` на `true`, а затем сохраните файл.

	Это нужно для того, чтобы был возможен доступ из одностраничного приложения JavaScript. При таком значении параметра токен носителя Oauth 2.0 возвращается в виде фрагмента URL-адреса.

16. Щелкните **Управление манифестом > Отправить манифест** и отправьте файл, который был обновлен на предыдущем шаге.

	![Отправить манифест](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Скопируйте значение в поле **Идентификатор клиента** и сохраните его, чтобы использовать позже.

## Настройка проекта ToDoListAngular для использования проверки подлинности

В этом разделе мы изменим внешнее приложение AngularJS таким образом, чтобы токен носителя для вошедшего пользователя из Azure AD можно было получить с помощью библиотеки проверки подлинности Active Directory (ADAL) для JS. Код будет включать токен в HTTP-запросы, отправляемые приложению среднего уровня, как показано на следующей схеме.

![Схема проверки подлинности пользователей](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Внесите следующие изменения в файлы проекта ToDoListAngular.

1. Откройте файл *index.html*.

2. Раскомментируйте строки, которые ссылаются на библиотеку проверки подлинности Active Directory (ADAL) для скриптов JS.

		<script src="app/scripts/adal.js"></script>
		<script src="app/scripts/adal-angular.js"></script>

1. Откройте файл *app/scripts/app.js*.

2. Закомментируйте блок кода с пометкой "без проверки подлинности" и раскомментируйте блок кода с пометкой "с проверкой подлинности".

	Это изменение ссылается на поставщик проверки подлинности ADAL JS и предоставляет ему значения конфигурации. В следующих шагах мы зададим значения конфигурации для приложения API и приложения Azure AD.

8. В коде, который задает переменную `endpoints`, в качестве URL-адреса API укажите URL-адрес приложения API, созданного для проекта ToDoListAPI, а в качестве идентификатора приложения Azure AD укажите идентификатор клиента, скопированный на классическом портале Azure.

	Код будет выглядеть примерно следующим образом:

		var endpoints = {
		    "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
		};

9. В вызове `adalProvider.init` для параметра `tenant` укажите имя клиента, а для параметра `clientId` — значение, которое мы использовали на предыдущем шаге.

	Код будет выглядеть примерно следующим образом:

		adalProvider.init(
		    {
		        instance: 'https://login.microsoftonline.com/', 
		        tenant: 'contoso.onmicrosoft.com',
		        clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
		        extraQueryParameter: 'nux=1',
		        endpoints: endpoints
		    },
		    $httpProvider
		    );

	Эти изменения в файле `app.js` указывают, что вызывающий код и вызываемый API находятся в одном приложении Azure AD.

1. Откройте файл *app/scripts/homeCtrl.js*.

2. Закомментируйте блок кода с пометкой "без проверки подлинности" и раскомментируйте блок кода с пометкой "с проверкой подлинности".

1. Откройте файл *app/scripts/indexCtrl.js*.

2. Закомментируйте блок кода с пометкой "без проверки подлинности" и раскомментируйте блок кода с пометкой "с проверкой подлинности".

### Развертывание проекта ToDoListAngular в Azure

8. В **обозревателе решений** щелкните правой кнопкой мыши проект ToDoListAngular и выберите пункт **Опубликовать**.

9. Щелкните **Опубликовать**.

	Visual Studio развернет проект и откроет в браузере базовый URL-адрес веб-приложения. Откроется страница с ошибкой 403. Это обычная ситуация при попытке перехода к базовому URL-адресу веб-API из браузера.

	Тем не менее перед тестированием приложения вам придется внести некоторые изменения в приложение API среднего уровня.

10. Закройте браузер.

## Настройка проекта ToDoListAPI для использования проверки подлинности

В настоящее время проект ToDoListAPI отправляет "*" в проект ToDoListDataAPI как значение параметра `owner`. В этом разделе мы изменим код для отправки идентификатора вошедшего пользователя.

Внесите следующие изменения в проект ToDoListAPI.

1. Откройте файл *Controllers/ToDoListController.cs* и раскомментируйте строку в каждом методе действия, который задает для параметра `owner` значение утверждения `NameIdentifier` в Azure AD. Например:

		owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

	**Важно**. Не выполняйте расскомментирование кода в методе `ToDoListDataAPI`. Вы выполните это действие позже в учебнике по проверке подлинности субъекта-службы.

### Развертывание проекта ToDoListAPI в Azure

8. В **обозревателе решений** щелкните правой кнопкой мыши проект ToDoListAPI и выберите пункт **Опубликовать**.

9. Щелкните **Опубликовать**.

	Visual Studio развернет проект и откроет в браузере базовый URL-адрес приложения API.

10. Закройте браузер.

### Тестирование приложения

9. Перейдите к URL-адресу веб-приложения **с помощью протокола HTTPS, а не HTTP**.

8. Откройте вкладку **Список дел**.

	Вам будет предложено войти.

9. Войдите с помощью учетных данных, используемых в клиенте AAD.

10. Откроется страница **Список дел**.

	![Страница "Список дел"](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

	Элементы списка дел не отображаются, так как до этого момента все они принадлежали владельцу "*". Теперь приложение среднего уровня запрашивает элементы для вошедшего пользователя, но они пока не созданы.

11. Добавьте новые элементы списка дел, чтобы убедиться, что приложение работает.

12. В другом окне браузера откройте URL-адрес пользовательского интерфейса Swagger для приложения API ToDoListDataAPI и щелкните **ToDoList > Получить**. Введите звездочку как значение параметра `owner`, а затем щелкните **Попробовать**.

	Ответ показывает, что в новых элементах списка дел в свойстве Owner указывается фактический идентификатор пользователя Azure AD.

	![Идентификатор владельца в ответе JSON](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## Построение проектов с нуля

Эти два проекта веб-API созданы с помощью шаблона проекта **Приложение API Azure** и путем замены контроллера значений по умолчанию контроллером ToDoList.

Сведения о создании одностраничного приложения AngularJS с помощью внутреннего приложения веб-API 2 см. в статье [Практикум. Создание одностраничного приложения (SPA) с помощью веб-API ASP.NET и Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Дополнительные сведения о добавлении кода для проверки подлинности в Azure AD см. в следующих ресурсах.

* [Защита одностраничных приложений AngularJS с помощью Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [Introducing ADAL JS v1 (Введение в ADAL JS, версия 1)](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## Устранение неполадок

[AZURE.INCLUDE [устранение неполадок](../../includes/app-service-api-auth-troubleshooting.md)]

* Убедитесь, что вы не путаете ToDoListAPI (средний уровень) и ToDoListDataAPI (уровень данных). Например, проверьте, что вы добавили проверку подлинности в приложение среднего уровня API, а не в приложение уровня данных.
* Убедитесь, что исходный код AngularJS ссылается на URL-адрес приложения API среднего уровня (ToDoListAPI, не ToDoListDataAPI) и правильный идентификатор клиента Azure AD.

## Дальнейшие действия

Из этого руководства вы узнали, как использовать проверку подлинности в службе приложений для приложения API и как вызывать приложение API с помощью библиотеки ADAL JS. В следующем руководстве вы узнаете, как [обеспечить безопасный доступ к приложению API в сценариях взаимодействия между службами](app-service-api-dotnet-service-principal-auth.md).

<!---HONumber=AcomDC_0713_2016-->