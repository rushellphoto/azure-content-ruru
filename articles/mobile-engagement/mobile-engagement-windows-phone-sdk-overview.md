<properties 
	pageTitle="Общие сведения о пакете SDK для Windows Phone Silverlight" 
	description="Общие сведения о пакете SDK для Windows Phone Silverlight для Azure Mobile Engagement" 					
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
	ms.date="05/03/2016" 
	ms.author="piyushjo" />

#Общие сведения о пакете SDK для Windows Phone Silverlight для Azure Mobile Engagement

Начните с этой статьи, чтобы получить подробную информацию об интеграции Azure Mobile Engagement в приложение Windows Phone Silverlight. Если вы хотите потренироваться, обязательно пройдите наш [15-минутный учебник](mobile-engagement-windows-phone-get-started.md).

Щелкните, чтобы просмотреть [содержимое пакета SDK](mobile-engagement-windows-phone-sdk-content.md).

##Процедуры по интеграции

1. Начните с этого раздела: [Как интегрировать Mobile Engagement в приложение Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)

2. Сведения об уведомлениях: [Как интегрировать Reach (Notifications) в приложение Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Реализация плана добавления тегов: [Как использовать API для расширенного добавления тегов Mobile Engagement в приложении Windows Phone Silverlight](mobile-engagement-windows-phone-use-engagement-api.md)

##Заметки о выпуске

###3\.3.0 (19.04.2016)
Входит в пакет NuGet *MicrosoftAzure.MobileEngagement* **версии 3.4.0**

-   Добавлен API "TestLogLevel" для включения, отключения или фильтрации журналов консоли, созданных с помощью пакета SDK.

Информацию о предыдущих версиях см. в [полных заметках о выпуске](mobile-engagement-windows-phone-release-notes.md).

##Процедуры обновления

Если вы уже интегрировали в приложение старую версию пакета SDK, при обновлении пакета SDK необходимо учитывать следующее.

Если вы пропустили несколько версий пакета SDK, вам понадобиться выполнить несколько процедур. Посмотрите полные [процедуры обновления](mobile-engagement-windows-phone-upgrade-procedure.md). Например, при миграции с версии 0.10.1 в версию 0.11.0 необходимо сначала выполнить процедуру миграции «с 0.9.0 в 0.10.1», а затем процедуру миграции «с 0.10.1 в 0.11.0».

###С 2.0.0 в 3.3.0

####Журналы тестирования

Теперь журналы консоли, созданные с помощью пакета SDK, можно включать, отключать или фильтровать. Чтобы настроить это действие, обновите свойство `EngagementAgent.Instance.TestLogEnabled` до одного из значений, доступных в перечислении `EngagementTestLogLevel`, например:

			EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
			EngagementAgent.Instance.Init();

### Обновление предыдущих версий

См. статью [Процедуры обновления](mobile-engagement-windows-phone-upgrade-procedure/).
 

<!---HONumber=AcomDC_0504_2016-->