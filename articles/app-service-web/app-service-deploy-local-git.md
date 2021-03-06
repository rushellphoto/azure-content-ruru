<properties
	pageTitle="Развертывание локального репозитория Git в службе приложений Azure"
	description="Узнайте, как включить локальное развертывание репозитория Git в службе приложений Azure."
	services="app-service"
	documentationCenter=""
	authors="dariagrigoriu"
	manager="wpickett"
	editor="mollybos"/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/13/2016"
	ms.author="dariagrigoriu"/>
    
# Развертывание локального репозитория Git в службе приложений Azure

В этом учебнике содержатся сведения о развертывании приложения в [службе приложений Azure] из репозитория Git на локальном компьютере. Служба приложений поддерживает такой подход, если на [портале Azure] выбран вариант развертывания **Локальный репозиторий Git**. Многие команды Git, описанные в этой статье, автоматически выполняются при создании приложения службы приложений с помощью [интерфейса командной строки Azure], как описано [здесь](app-service-web-get-started.md).

## Предварительные требования

Для работы с этим учебником необходимы указанные ниже компоненты.

- Git. Двоичный файл установки можно скачать [отсюда](http://www.git-scm.com/downloads).
- Базовые знания о Git.
- Учетная запись Microsoft Azure. Если у вас нет учетной записи, можно [подписаться на бесплатную пробную версию](https://azure.microsoft.com/pricing/free-trial) или [активировать преимущества для подписчиков Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

>[AZURE.NOTE] Если требуется приступить к работе со службой приложений Azure до создания учетной записи Azure, перейдите к разделу [Пробное использование службы приложений](http://go.microsoft.com/fwlink/?LinkId=523751), который поможет быстро создать кратковременное приложение начального уровня в службе приложений. Никаких кредитных карт и обязательств.

## <a id="Step1"></a>Шаг 1. Создание локального репозитория

Чтобы создать новый репозиторий Git, выполните следующие задачи.

1. Запустите программу командной строки, например **GitBash** (Windows) или **Bash** (оболочка Unix). На компьютерах с OS X доступ к командной строке можно получить через приложение **Terminal**.

2. Перейдите в каталог, в котором будет находиться содержимое для развертывания.

3. Используйте следующую команду, чтобы инициализировать новый репозиторий Git:

		git init

## <a id="Step2"></a>Шаг 2. Фиксация содержимого

Служба приложений поддерживает приложения, созданные с использованием различных языков программирования.

1. Если в репозитории уже имеется содержимое, пропустите этот пункт и перейдите к пункту 2 ниже. Если репозиторий пуст, заполните его статическим HTML-файлом, как показано ниже.

    - С помощью текстового редактора создайте файл с именем **index.html** в корневой папке репозитория Git.
    - Добавьте следующий текст в качестве содержимого файла index.html и сохраните его: *Hello Git!*
        
2. Из командной строки проверьте свое нахождение в корневой папке репозитория Git. Затем используйте следующие команды для добавления файлов в репозиторий:

		git add -A 

4. Затем примените эти изменения к репозиторию с помощью следующей команды:

		git commit -m "Hello Azure App Service"

## <a id="Step3"></a>Шаг 3. Включение репозитория приложения службы приложений

Чтобы включить репозиторий Git для приложения службы приложений, выполните приведенные далее действия.

1. Войдите на [портал Azure].

2. В колонке приложения службы приложений выберите **Параметры > Источник развертывания**. Щелкните **Выбрать источник**, затем **Локальный репозиторий Git** и нажмите кнопку **OK**.

	![Локальный репозиторий Git](./media/app-service-deploy-local-git/local_git_selection.png)

3. Если репозиторий в Azure настраивается вами впервые, потребуется создать для него учетную запись. Вы будете использовать данные этой учетной записи для входа в репозиторий Azure и отправки изменений из локального репозитория Git. В колонке приложения щелкните **Параметры > Учетные данные развертывания**, а затем настройте имя пользователя и пароль развертывания. Закончив, нажмите кнопку **Сохранить**.

	![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a id="Step4"></a>Шаг 4. Развертывание проекта

Чтобы опубликовать приложение в службу приложений с помощью локального репозитория Git, выполните указанные далее действия.

1. В колонке приложения на портале Azure выберите **Параметры > Свойства** для **URL-адреса Git**.

	![](./media/app-service-deploy-local-git/git_url.png)

	**URL-адрес Git** содержит внешнюю ссылку для развертывания из вашего локального репозитория. Этот URL-адрес будет использоваться на следующих шагах.

2. С помощью командной строки проверьте свое нахождение в корневой папке локального репозитория Git.

3. Используйте `git remote` для добавления внешней ссылки, указанной в пункте **URL-адрес Git** на шаге 1. Команда будет выглядеть примерно следующим образом:

		git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
    > [AZURE.NOTE] Команда **remote** добавляет именованную ссылку на удаленный репозиторий. В этом примере создается ссылка с именем azure для репозитория вашего веб-приложения.

4. Опубликуйте содержимое в службу приложений с помощью созданной команды remote **azure**.

		git push azure master

	Вам предложат ввести пароль, созданный ранее при сбросе учетных данных развертывания на портале Azure. Введите пароль (обратите внимание, что при вводе пароля Gitbash не выводит звездочки на консоль).
       
5. Перейдите к приложению на портале Azure. В колонке **Развертывание** должна отображаться запись журнала с последним push-уведомлением.

	![](./media/app-service-deploy-local-git/deployment_history.png)

6. В верхней части колонки приложения нажмите кнопку **Обзор**, чтобы проверить развертывание содержимого.
    
## <a id="Step5"></a>Устранение неполадок

Ниже перечислены ошибки или проблемы, обычно возникающие при использовании Git для публикации в приложение службы приложений в Azure.

****

**Симптом**: Не удалось подключиться к [scmAddress]: Нет доступа к [siteURL]:

**Причина**: эта ошибка может возникнуть, если приложение не работает.

**Устранение**: запустите приложение на портале Azure. Развертывание Git не будет работать, пока приложение не запущено.


****

**Симптом**: не удается разрешить hostname узла.

**Причина**: эта ошибка может возникнуть, если при создании удаленного репозитория azure был введен неправильный адрес.

**Устранение**: используйте команду `git remote -v`, чтобы вывести список всех удаленных репозиториев с соответствующими URL-адресами. Проверьте правильность URL-адреса удаленного репозитория "azure". При необходимости удалите и повторно создайте этот удаленный репозиторий, используя правильный URL-адрес.

****

**Симптом**: нет ссылок и ничего не указано; ничего не выполняется. Возможно, следует указать ветвь, например master.

**Причина**: эта ошибка может возникать, если не указана ветвь при выполнении операции принудительной отправки git и не задано значение push.default, используемое Git.

**Устранение**: снова выполните операцию принудительной отправки, указав главную ветвь. Например:

	git push azure master

****

**Симптом**: src refspec [имя\_ветви] не соответствует никакой ветви.

**Причина**: эта ошибка может возникнуть при попытке принудительно отправить данные в ветвь, отличную от главной, в удаленном репозитории azure.

**Устранение**: снова выполните операцию принудительной отправки, указав главную ветвь. Например:

	git push azure master

****

**Симптом**: ошибка — изменения в удаленный репозиторий внесены, но веб-приложение не обновилось.

**Причина**: эта ошибка может возникнуть при развертывании приложения Node.js, содержащего файл package.json, в котором указаны дополнительные необходимые модули.

**Устранение**: до этой ошибки должны быть зарегистрированы дополнительные сообщения, содержащие «npm ERR!»; они могут предоставить дополнительный контекст для данного сбоя. Ниже перечислены известные причины этой ошибки и соответствующее сообщение npm ERR!.

* **Malformed package.json file**: npm ERR! Couldn't read dependencies (не удалось прочитать зависимости).

* **Собственный модуль, не имеющий двоичного дистрибутива для Windows**:

	* npm ERR! `cmd "/c" "node-gyp rebuild"` failed with 1

		ИЛИ

	* npm ERR! [имя\_модуля@версия] preinstall: `make || gmake`


## Дополнительные ресурсы

* [Документация по Git](http://git-scm.com/documentation)
* [Документация по проекту Kudu](https://github.com/projectkudu/kudu/wiki)
* [Непрерывное развертывание в службе приложений Azure](app-service-continuous-deployment.md)
* [Использование PowerShell для Azure](../powershell-install-configure.md)
* [Использование интерфейса командной строки Azure](../xplat-cli-install.md)

[службе приложений Azure]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://azure.microsoft.com/develop/overview/
[портал Azure]: https://portal.azure.com
[портале Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[интерфейса командной строки Azure]: https://azure.microsoft.com/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart

<!---HONumber=AcomDC_0803_2016-->