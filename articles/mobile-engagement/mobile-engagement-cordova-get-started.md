<properties
	pageTitle="Начало работы с Azure Mobile Engagement для Cordova (Phonegap)"
	description="Узнайте, как использовать Azure Mobile Engagement для аналитики и отправки push-уведомлений в приложения Cordova (Phonegap)."
	services="mobile-engagement"
	documentationCenter="Mobile"
	authors="piyushjo"
	manager="dwrede"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-phonegap"
	ms.devlang="js"
	ms.topic="hero-article" 
	ms.date="04/04/2016"
	ms.author="piyushjo" />

# Начало работы с Azure Mobile Engagement для Cordova (Phonegap)

[AZURE.INCLUDE [Выбор других руководств](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

В этом разделе показано, как использовать Azure Mobile Engagement для анализа использования приложений и отправки push-уведомлений определенным группам пользователей мобильного приложения, разработанного на платформе Cordova.

В этом учебнике вы создадим пустое приложение Cordova на компьютере Mac и интегрируем пакет Mobile Engagement SDK. Приложение будет собирать базовые данные аналитики и получать push-уведомления с помощью системы push-уведомлений Apple (APNS) для iOS и Google Cloud Messaging (GCM) для Android. Для тестирования мы развернем его на устройствах под управлением iOS или Android.

> [AZURE.NOTE] Для работы с этим учебником необходима активная учетная запись Azure. Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fru-RU%2Fdocumentation%2Farticles%2Fmobile-engagement-cordova-get-started).

Для работы с данным учебником требуется следующее:

+ Xcode, который можно установить из Mac App Store (для развертывания на iOS)
+ [Android SDK и эмулятор](http://developer.android.com/sdk/installing/index.html) (для развертывания на Android)
+ Сертификат push-уведомлений (P12), который можно получить в центре разработки Apple (для APNS)
+ Номер проекта GCM, который можно найти в консоли разработчиков Google (для GCM)
+ [Подключаемый модуль Cordova для Mobile Engagement](https://www.npmjs.com/package/cordova-plugin-ms-azure-mobile-engagement)

> [AZURE.NOTE] Исходный код и файл сведений о подключаемом модуле Cordova можно найти на сайте [Github](https://github.com/Azure/azure-mobile-engagement-cordova).

##<a id="setup-azme"></a>Настройка Mobile Engagement для приложения Cordova

[AZURE.INCLUDE [Создание приложения Mobile Engagement на портале](../../includes/mobile-engagement-create-app-in-portal.md)]

##<a id="connecting-app"></a>Подключение приложения к серверной части Mobile Engagement

В этом учебнике описаны действия по базовой интеграции, т. е. минимум, требуемый для сбора данных и отправки push-уведомлений.

Чтобы продемонстрировать интеграцию, мы создадим простое приложение при помощи Сordova.

###Создание нового проекта Cordova

1. Чтобы создать проект Cordova на основе шаблона по умолчанию, откройте окно *терминала* на компьютере Mac и введите указанную ниже команду. Убедитесь, что в профиле публикации, который вы впоследствии будете использовать для развертывания приложения iOS, используется идентификатор приложения "com.mycompany.myapp". 

		$ cordova create azme-cordova com.mycompany.myapp
		$ cd azme-cordova

2. Выполните указанные ниже команды, чтобы настроить проект для **iOS** и запустить его в iOS Simulator.

		$ cordova platform add ios 
		$ cordova run ios

3. Выполните указанные ниже команды, чтобы настроить проект для **Android** и запустить его в эмуляторе Android. Убедитесь, что для параметров эмулятора Android SDK в качестве цели установлены API-интерфейсы Google (Google Inc.), а в качестве ЦП/ABI используется ARM API-интерфейсов Google.

		$ cordova platform add android
		$ cordova run android

4. 	Добавьте консольный подключаемый модуль Cordova.

		$ cordova plugin add cordova-plugin-console 

###Подключение приложения к серверной части Mobile Engagement

1. Установите подключаемый модуль Cordova для Azure Mobile Engagement и укажите значения переменных для настройки подключаемого модуля:

		cordova plugin add cordova-plugin-ms-azure-mobile-engagement    
     		--variable AZME_IOS_CONNECTION_STRING=<iOS Connection String> 
	        --variable AZME_IOS_REACH_ICON=... (icon name WITH extension) 
	        --variable AZME_ANDROID_CONNECTION_STRING=<Android Connection String> 
			--variable AZME_ANDROID_REACH_ICON=... (icon name WITHOUT extension)       
	        --variable AZME_ANDROID_GOOGLE_PROJECT_NUMBER=... (From your Google Cloud console for sending push notifications) 
	        --variable AZME_ACTION_URL =... (URL scheme which triggers the app for deep linking)
	        --variable AZME_ENABLE_NATIVE_LOG=true|false
			--variable AZME_ENABLE_PLUGIN_LOG=true|false

*Значок Android Reach* — это должно быть имя ресурса без расширения или префикса (например, mynotificationicon), а значок файла необходимо скопировать в приложение Android (platforms/android/res/drawable).

*Значок iOS Reach* — это должно быть имя ресурса с расширением (например, mynotificationicon.png), а файл значка должен быть добавлен в проект iOS с XCode (с помощью меню «Добавить файлы»).

##<a id="monitor"></a>Включение мониторинга в реальном времени

1. В проекте Cordova добавьте в файл **www/js/index.js** вызов Mobile Engagement, чтобы объявить новое действие после получения события *deviceReady*.

		 onDeviceReady: function() {
		        Engagement.startActivity("myPage",{});
		    }

2. Запустите приложение:
		
	- **Для iOS:**
	
		В окне `Terminal` запустите ваше приложение в новом экземпляре iOS Simulator, выполнив указанную ниже команду.

			cordova run ios

	- **Для Android:**
		
		В окне `Terminal` запустите ваше приложение в новом экземпляре эмулятора, выполнив указанную ниже команду.

			cordova run android

3. В журнале консоли можно увидеть следующее:

		[Engagement] Agent: Session started
		[Engagement] Agent: Activity 'myPage' started
		[Engagement] Connection: Established
		[Engagement] Connection: Sent: appInfo
		[Engagement] Connection: Sent: startSession
		[Engagement] Connection: Sent: activity name='myPage'

##<a id="monitor"></a>Подключение приложения с возможностью его отслеживания в режиме реального времени

[AZURE.INCLUDE [Подключение приложения с возможностью его отслеживания в режиме реального времени](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Включение функции отправки и приема push-уведомлений и обмена сообщениями в приложении

Mobile Engagement позволяет взаимодействовать с пользователями с помощью push-уведомлений и сообщений в приложении в контексте кампаний. На портале Mobile Engagement этот модуль называется МОДУЛЕМ ОБРАБОТКИ РЕКЛАМНЫХ КАМПАНИЙ. В следующих разделах описаны действия по настройке приложения для приема уведомлений и сообщений.

###Настройка учетных данных для отправки push-уведомлений из Mobile Engagement

Чтобы разрешить службе Mobile Engagement отправлять push-уведомления от вашего имени, необходимо предоставить ей доступ к сертификату Apple iOS или ключу API сервера GCM.
	
1. Перейдите на портал Mobile Engagement. Убедитесь, что у вас открыто приложение, используемое для этого проекта, и нажмите кнопку **Использовать** внизу.
	
	![][1]
	
2. Вы перейдете на страницу "Параметры на портале Mobile Engagement. Здесь выберите раздел **Собственные push-уведомления**.
	
	![][2]

3. Настройте сертификат Apple iOS или ключ API сервера GCM.

	**[iOS]**

	а. Выберите сертификат P12, загрузите его и введите пароль:
	
	![][3]

	**[Android]**

	а. Выберите значок правки перед **ключом API** в разделе параметров GCM, а затем в открывшемся всплывающем окне вставьте ключ сервера GCM и нажмите кнопку **ОК**.
		
	![][4]

###Включение push-уведомлений в приложении Cordova

Добавьте в файл **www/js/index.js** вызов Mobile Engagement для запроса push-уведомлений и объявления обработчика.

	 onDeviceReady: function() {
           Engagement.initializeReach(  
	 			// on OpenUrl  
	 			function(_url) {   
	 			alert(_url);   
	 			});  
			Engagement.startActivity("myPage",{});  
	    }

###Запуск приложения

**[iOS]**

1. При тестировании push-уведомлений для сборки и развертывания приложения на устройстве мы будем использовать XCode, поскольку iOS разрешает вывод push-уведомлений только на реальных устройствах. Откройте каталог, в котором создан проект Cordova, и перейдите в папку **...\\platforms\\ios**. Откройте XCODEPROJ-файл в XCode. 
	
2. Скомпилируйте и разверните приложение Cordova на устройстве iOS, используя учетную запись с профилем подготовки, содержащим сертификат, который вы только что загрузили на портал Mobile Engagement, и идентификатором приложения, введенным при создании приложения Cordova. Вы можете найти и проверить значение *идентификатора набора* в файле **Resources*-info.plist** в XCode.

3. На устройстве iOS появится стандартное всплывающее окно с уведомлением о том, что приложение запрашивает разрешение на отправку уведомлений. Предоставьте это разрешение.

**[Android]**

Для запуска приложения на Android можно просто использовать эмулятор Android, так как он поддерживает уведомления GCM.

	cordova run android

##<a id="send"></a>Отправка уведомления в приложение

Теперь мы создадим простую кампанию push-уведомлений, которая будет отправлять push-уведомления в приложение, запущенное на устройстве.

1. Перейдите на вкладку **Reach** на портале Mobile Engagement.

2. Щелкните **Создать объявление**, чтобы создать кампанию push-уведомлений.

	![][6]

3. Введите данные для новой кампании **[Android]**
	
	- Задайте **имя** кампании. 
	- Для параметра **Тип доставки** выберите значение *Системное уведомление* *Простое*.
	- Для параметра *Время доставки* выберите значение **Любое время**.
	- Укажите **название** уведомления, которое будет отображаться в первой строке push-уведомления.
	- Укажите **сообщение** для уведомления, которое будет использоваться в качестве текста сообщения. 

	![][11]

4. Введите данные для новой кампании **[iOS]**

	- Задайте **имя** кампании. 
	- Для параметра *Время доставки* выберите значение **Только вне приложения**.
	- Укажите **название** уведомления, которое будет отображаться в первой строке push-уведомления.
	- Укажите **сообщение** для уведомления, которое будет использоваться в качестве текста сообщения. 
 
	![][12]

5. Прокрутите окно вниз и в разделе содержимого выберите пункт **Только уведомления**.

	![][8]

6. [Необязательно] Можно также указать URL-адрес действия. Убедитесь, что используется та же схема URL-адресов, что и для переменной **AZME\_REDIRECT\_URL** подключаемого модуля, например **myapp://test*.

7. Вы настроили простейшую базовую кампанию. Еще раз прокрутите окно вниз и нажмите кнопку **Создать**, чтобы сохранить кампанию.

8. И, наконец, **активируйте** кампанию.
	
	![][10]

9. Теперь push-уведомления в рамках этой кампании будут отображаться на устройстве или в эмуляторе.

##<a id="next-steps"></a>Дальнейшие действия
[Обзор методов, доступных в пакете Cordova SDK для Mobile Engagement](https://github.com/Azure/azure-mobile-engagement-cordova)

<!-- Images. -->

[1]: ./media/mobile-engagement-cordova-get-started/engage-button.png
[2]: ./media/mobile-engagement-cordova-get-started/engagement-portal.png
[3]: ./media/mobile-engagement-cordova-get-started/native-push-settings.png
[4]: ./media/mobile-engagement-cordova-get-started/api-key.png
[6]: ./media/mobile-engagement-cordova-get-started/new-announcement.png
[8]: ./media/mobile-engagement-cordova-get-started/campaign-content.png
[10]: ./media/mobile-engagement-cordova-get-started/campaign-activate.png
[11]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-android.png
[12]: ./media/mobile-engagement-cordova-get-started/campaign-first-params-ios.png

<!---HONumber=AcomDC_0406_2016---->