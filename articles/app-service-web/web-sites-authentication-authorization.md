<properties 
	pageTitle="Использование Active Directory для проверки подлинности в службе приложений Azure" 
	description="Сведения о различных параметрах проверки подлинности и авторизации для бизнес-приложений, развертываемых в веб-приложениях службы приложений Azure" 
	services="app-service" 
	documentationCenter="" 
	authors="cephalin" 
	manager="wpickett" 
	editor="jimbe"/>

<tags 
	ms.service="app-service" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="web" 
	ms.date="02/26/2016" 
	ms.author="cephalin"/>

# Использование Active Directory для проверки подлинности в службе приложений Azure #

[Веб-приложения службы приложений Azure](http://go.microsoft.com/fwlink/?LinkId=529714) реализуют сценарии корпоративных бизнес-приложений благодаря поддержке единого входа (SSO) пользователей, независимо от способа доступа к приложению (из локальной среды или через общедоступный Интернет). Их можно интегрировать с [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) или локальной службой токенов безопасности (STS), например со службами федерации Active Directory (AD FS), чтобы проверять подлинность внутренних пользователей Active Directory (AD) и правильно авторизовать их.

## Аутентификация и авторизация без лишних усилий ##

Вы можете включить проверку подлинности и авторизацию для своего веб-приложения несколькими щелчками. Настройка параметров с помощью флажков в каждом веб-приложении Azure обеспечивает базовое управление доступом к веб-приложениям для бизнеса. Это достигается за счет использования протокола HTTPS и проверки подлинности выбранного клиента Azure AD перед предоставлением пользователям доступа ко всему содержимому веб-приложения. Дополнительные сведения см. в статье [Проверка подлинности и авторизация веб-приложений](https://azure.microsoft.com/blog/2014/11/13/azure-websites-authentication-authorization/).

>[AZURE.NOTE] Эта функция в настоящее время находится на стадии предварительной версии.

## Реализация аутентификации и авторизации вручную ##

Во многих сценариях необходимо настраивать поведение приложения при аутентификации и авторизации, например страницы входа и выхода, логику настраиваемой авторизации, поведение многоклиентского приложения и т. д. В таких случаях лучше настраивать аутентификацию и авторизацию вручную, что обеспечит более гибкие возможности для использования этих функций. Ниже приведены два основных варианта реализации.

-	[Azure AD](web-sites-dotnet-lob-application-azure-ad.md). Вы можете осуществлять проверку подлинности и авторизацию для своего веб-приложения с помощью Azure AD. Использование Azure AD в качестве поставщика удостоверений имеет ряд особенностей.
	-	Поддерживаются такие популярные протоколы проверки подлинности, как [OAuth 2.0](http://oauth.net/2/), [OpenID Connect](http://openid.net/connect/) и [SAML 2.0](http://en.wikipedia.org/wiki/SAML_2.0) Полный список поддерживаемых протоколов см. в статье [Протоколы проверки подлинности Azure Active Directory](http://msdn.microsoft.com/library/azure/dn151124.aspx).
	-	Можно использовать специализированного поставщика удостоверений Azure без локальной инфраструктуры.
	-	Можно настроить синхронизацию каталогов с локальной (управляемой локально) службой AD.
	-	Azure AD позволяет использовать единый вход в веб-приложение при доступе пользователей AD из интрасети и Интернета. Это возможно благодаря синхронизации каталогов из локального домена AD. Из интрасети пользователи AD с помощью средств встроенной проверки подлинности могут автоматически войти в веб-приложение. Из Интернета пользователи AD могут войти в веб-приложение, используя свои учетные данные Windows.
	-	Предоставляет единый вход для [всех приложений, поддерживаемых Azure AD](/marketplace/active-directory/), в том числе Azure, Office 365, Dynamics CRM Online, Microsoft Intune и тысячи сторонних облачных приложений. 
	-	Azure AD делегирует управление приложениями [проверяющей стороны](http://en.wikipedia.org/wiki/Relying_party) ролям без прав администратора, но настраивать доступ приложений к конфиденциальным данным каталога должны глобальные администраторы.
	-	Всем приложениям проверяющей стороны отправляется общий набор типов утверждений. Список типов утверждений см. в статье [Поддерживаемые типы токенов и утверждений](http://msdn.microsoft.com/library/azure/dn195587.aspx). Утверждения не настраиваются.
	-	[API Graph Azure AD](http://msdn.microsoft.com/library/azure/hh974476.aspx) обеспечивает доступ приложения к данным каталога в Azure AD.
-	[Локальная служба токенов безопасности (STS), например AD FS](web-sites-dotnet-lob-application-adfs.md). Вы можете выполнить проверку подлинности и авторизацию для своего веб-приложения с помощью локальной службы токенов безопасности, к примеру AD FS. Использование локальной службы AD FS имеет ряд особенностей.
	-	Топологию AD FS необходимо разворачивать локально с учетом затрат и расходов на управление.
	-	Лучше всего, если политика компании требует, чтобы данные AD хранились локально.
	-	Только администраторы AD FS могут настроить [отношения доверия с проверяющей стороной и правила утверждений](http://technet.microsoft.com/library/dd807108.aspx).
	-	Можно управлять [утверждениями](http://technet.microsoft.com/library/ee913571.aspx) для каждого отдельного приложения.
	-	Для доступа к локальным данным AD через корпоративный брандмауэр необходимо использовать отдельное решение.

>[AZURE.NOTE] Чтобы приступить к работе со службой приложений Azure до создания учетной записи Azure, перейдите к разделу [Пробное использование службы приложений](http://go.microsoft.com/fwlink/?LinkId=523751), где вы можете быстро создать кратковременное веб-приложение начального уровня в службе приложений. Никаких кредитных карт и обязательств.

## Изменения
* Указания по изменениям при переходе от веб-сайтов к службе приложений см. в разделе [Служба приложений Azure и ее влияние на существующие службы Azure](http://go.microsoft.com/fwlink/?LinkId=529714).
 

<!---HONumber=AcomDC_0504_2016-->