<properties 
	pageTitle="Процедуры обновления пакета SDK для Windows Phone Silverlight" 
	description="Процедуры обновления пакета SDK для Windows Phone Silverlight для Azure Mobile Engagement" 					
	services="mobile-engagement" 
	documentationCenter="mobile" 
	authors="piyushjo" 
	manager="dwrede"
	editor="" />

<tags 
	ms.service="mobile-engagement" 
	ms.workload="mobile" 
	ms.tgt_pltfrm="mobile-windows-phone" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/29/2016" 
	ms.author="piyushjo" />

#Процедуры обновления пакета SDK для Windows Phone Silverlight

Если вы уже интегрировали в приложение старую версию пакета SDK, при обновлении пакета SDK необходимо учитывать следующее.

Если вы пропустили несколько версий пакета SDK, вам понадобиться выполнить несколько процедур. Например, при миграции с версии 0.10.1 в версию 0.11.0 необходимо сначала выполнить процедуру миграции «с 0.9.0 в 0.10.1», а затем процедуру миграции «с 0.10.1 в 0.11.0».

##С 2.0.0 в 3.3.0

### Журналы тестирования

Теперь журналы консоли, созданные с помощью пакета SDK, можно включать, отключать или фильтровать. Чтобы настроить это действие, обновите свойство `EngagementAgent.Instance.TestLogEnabled` до одного из значений, доступных в перечислении `EngagementTestLogLevel`, например:

			EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
			EngagementAgent.Instance.Init();

##От версии 1.1.1 до версии 2.0.0

Ниже описан процесс переноса интеграции пакета SDK из службы Capptain от Capptain SAS в приложение на платформе Azure Mobile Engagement.

> [Azure.IMPORTANT] Службы Capptain и Azure Mobile Engagement отличаются между собой. В представленных ниже процедурах описывается способ переноса только для клиентского приложения. При переносе пакета SDK в приложение данные НЕ будут перенесены с серверов Capptain на серверы Mobile Engagement.

При переносе с использованием более ранней версии сначала ознакомьтесь с информацией о переносе в версии 1.1.1 на веб-сайте Capptain, а затем примените следующую процедуру.

### Пакет NuGet

Замените **Capptain.WindowsPhone** на пакет Nuget **MicrosoftAzure.MobileEngagement**.

### Применение Mobile Engagement

В пакете SDK используется термин `Engagement`, поэтому необходимо обновить проект с учетом этого изменения.

Необходимо удалить текущий пакет NuGet Capptain. Советуем удалить все, что было изменено в папке ресурсов Capptain. Если хотите сохранить эти файлы, скопируйте их в другое место.

После этого установите в проекте новый пакет NuGet для Microsoft Azure Engagement. Его можно найти непосредственно на веб-сайте [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). В результате выполнения этого действия все файлы ресурсов, используемые Engagement, будут заменены, а в ссылки проекта будет добавлена новая библиотека DLL для Engagement.

Ссылки проекта необходимо очистить, удалив ссылки на библиотеку DLL Capptain. Если этого не сделать, возникнет конфликт с версией Capptain и будут происходить ошибки.

После настройки ресурсов Capptain скопируйте содержимое старых файлов и вставьте его в новые файлы Engagement. Обратите внимание, что XAML- и CS-файлы нужно обновить.

После выполнения этих шагов необходимо заменить старые ссылки Capptain на новые ссылки Engagement.

1. Все пространства имен Capptain должны быть обновлены.

	Перед миграцией:
	
		using Capptain.Agent;
		using Capptain.Reach;
	
	После миграции:
	
		using Microsoft.Azure.Engagement;

2. Все классы Capptain, содержащие Capptain должны содержать Engagement.

	Перед миграцией:
	
		public sealed partial class MainPage : CapptainPage
		{
		  protected override string GetCapptainPageName()
		  {
		    return "Capptain Demo";
		  }
		  ...
		}
	
	После миграции:
	
		public sealed partial class MainPage : EngagementPage
		{
		  protected override string GetEngagementPageName()
		  {
		    return "Engagement Demo";
		  }
		  ...
		}

3. Для XAML-файлов изменятся также пространство имен и атрибуты Capptain.

	Перед миграцией:
	
		<capptain:CapptainPage
		...
		xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
		...
		</capptain:CapptainPage>
	
	После миграции:
	
		<engagement:EngagementPage
		...
		xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
		...
		</engagement:EngagementPage>

4. Обратите внимание, что для использования Engagement были переименованы и другие ресурсы, такие как изображения Capptain.

### Идентификатор приложения и ключ SDK

Engagement использует строку подключения. При использовании Mobile Engagement не нужно указывать идентификатор приложения и ключ SDK. Достаточно просто задать строку подключения. Ее можно настроить в файле EngagementConfiguration.

Конфигурацию Engagement можно определить в файле `Resources\EngagementConfiguration.xml` проекта.

Измените этот файл, чтобы указать:

-   строку подключения приложения между тегами `<connectionString>` и `<\connectionString>`.

Если вместо этого вам необходимо указать ее во время выполнения, можно вызвать следующий метод до инициализации агента Engagement:

		/* Engagement configuration. */
		EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
		engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
		
		/* Initialize Engagement angent with above configuration. */
		EngagementAgent.Instance.Init(engagementConfiguration);

Строка подключения для приложения отображается на классическом портале Azure.

### Изменение имени элементов

Каждый из элементов с именем *capptain* был переименован в *engagement*. Аналогичным образом имя *Capptain* изменилось на *Engagement*.

Примеры часто используемых элементов Capptain:

-   CapptainConfiguration теперь называется EngagementConfiguration.
-   CapptainAgent теперь называется EngagementAgent.
-   CapptainReach теперь называется EngagementReach.
-   CapptainHttpConfig теперь называется EngagementHttpConfig.
-   GetCapptainPageName теперь называется GetEngagementPageName.

Обратите внимание, что переименование также влияет на переопределенные методы.



 

<!---HONumber=AcomDC_0420_2016-->