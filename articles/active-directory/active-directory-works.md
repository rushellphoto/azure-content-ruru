<properties
	pageTitle="Как работает Azure Active Directory? | Microsoft Azure"
	description="Azure Active Directory создает ваш личный идентификационный ландшафт в облаке. Его можно подключить к локальной идентификационной системе или использовать отдельно."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="curtand"/>



# Как работает Azure Active Directory?

###Другие статьи на эту тему
[Что такое Azure AD?](active-directory-whatis.md)<br> [Как это работает?](active-directory-works.md)<br> [Приступая к работе](active-directory-get-started.md)<br> [Дальнейшие действия](active-directory-next-steps.md)<br> [Подробнее](active-directory-learn-map.md)


Azure Active Directory (Azure AD) создает ваш личный идентификационный ландшафт в облаке. Его можно подключить к локальной идентификационной системе или использовать отдельно.

Учетная запись в Azure AD в облаке работает по принципу водительского удостоверения — это уникальный идентификатор для доступа к веб-службам. В этом случае Azure AD работает в качестве личного регистратора этих водительских удостоверений. Она обеспечивает использование идентификаторов по всему облаку и повышает уровень мобильности для пользователей, у который есть доступ к локальным ресурсам.

> [AZURE.NOTE] Чтобы использовать Azure Active Directory, необходима учетная запись Azure. Если у вас нет учетной записи, вы можете [зарегистрироваться для получения бесплатной учетной записи Azure](https://azure.microsoft.com/pricing/free-trial/).

## Как Azure AD поддерживает Office 365, Microsoft Intune и другие службы Azure?
Сначала происходит чтение данных портала Azure, Центра администрирования Office 365, портала учетных записей Microsoft Intune и командлетов из модуля Azure AD PowerShell, а затем выполняется запись в один общий экземпляр Azure AD, который связан с вашим каталогом. Порталы (или командлеты) работают в качестве внешнего интерфейса, который запрашивает или изменяет данные каталога. [Подробнее о поддержке других служб](active-directory-administer.md#what-is-an-azure-ad-tenant)

## Как Azure AD поддерживает приложения сторонних производителей?
Azure AD упрощает аутентификацию для разработчиков, обеспечивая идентификацию в рамках службы, а также предоставляя библиотеки с открытым исходным кодом для различных платформ, которые могут помочь быстро начать кодирование. [Подробнее о сценариях аутентификации для Azure AD](active-directory-authentication-scenarios.md).


## Как Azure AD расширяет локальный каталог?
Azure AD поддерживает несколько широко используемых протоколов проверки подлинности и авторизации. [Подробнее о протоколах проверки подлинности Azure Active Directory](active-directory-authentication-scenarios.md).

## Как Azure помогает управлять идентификацией?
Хотите узнать больше о том, как управлять Azure AD? Как получить каталог? Как удалить каталог? Как управлять данными каталога? Подробнее об администрировании каталога Azure AD: [Подробнее о том, как администрировать Azure AD](active-directory-administer.md).

## Дополнительные ресурсы

* [Регистрация организации в Azure](sign-up-organization.md)
* [Удостоверение Azure](fundamentals-identity.md)

<!---HONumber=AcomDC_0427_2016-->