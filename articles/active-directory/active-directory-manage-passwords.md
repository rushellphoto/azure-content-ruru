<properties
	pageTitle="Управление паролями в Azure Active Directory | Microsoft Azure"
	description="Сведения об управлении паролями в Azure Active Directory."
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
	ms.date="05/16/2016"
	ms.author="curtand"/>

# Управление паролями в Azure Active Directory

Если вы являетесь администратором, вы можете сбросить пароль пользователя в Azure Active Directory (Azure AD) на классическом портале Azure. Щелкните имя каталога, на странице «Пользователи» щелкните имя пользователя, а в нижней части портала щелкните **Сбросить пароль**.

В остальной части этого раздела рассматривается весь набор возможностей управления паролями, которые поддерживает Azure AD, в том числе перечисленные ниже.

- **Самостоятельное изменение пароля** позволяет конечным пользователям или администраторам менять свои пароли с истекшим или не истекшим сроком действия без поддержки администратора или службы технической поддержки.
- **Самостоятельное изменение пароля** позволяет конечным пользователям или администраторам автоматически изменять свои пароли с истекшим или не истекшим сроком действия без вызова администратора или обращения в службу технической поддержки. Для самостоятельного сброса пароля требуется Azure AD Premium или Basic. Дополнительные сведения см. в статье [Выпуски Azure Active Directory](active-directory-editions.md).
- **Сброс пароля администратором**. Администратор может сбросить пароль пользователя или другого администратора на классическом портале Azure.
- **Отчеты по операциям управления паролями** позволяют администраторам лучше понять, как работают операции по сбросу и регистрации паролей, происходящие в организации.
- **Обратная запись паролей** позволяет управлять локальными паролями из облака, чтобы федеративный пользователь или пользователь с синхронизированным паролем смог выполнять все сценарии, указанные выше, или эти сценарии можно было выполнить от его лица. Для обратной записи паролей требуется Azure AD Premium. Дополнительную информацию см. в разделе [Приступая к работе с Azure Active Directory Premium](active-directory-get-started-premium.md).

> [AZURE.NOTE]
Китайским клиентам служба Azure AD Premium доступна в качестве веб-экземпляра Azure AD, доступного по всему миру. Служба Microsoft Azure, обслуживаемая компанией 21Vianet в Китае, сейчас не поддерживает AD Premium. Чтобы получить дополнительную информацию, свяжитесь с нами на [форуме Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

Используйте следующие ссылки, чтобы перейти к документации, в которой вы заинтересованы больше всего.

- [Общие сведения: управление паролями в Azure AD](active-directory-passwords-how-it-works.md)
- [Самостоятельный сброс пароля в Azure AD: порядок включения, настройки и тестирования самостоятельного сброса паролей](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords)
- [Самостоятельный сброс пароля в Azure AD: как настроить сброс пароля согласно своим потребностям](active-directory-passwords-customize.md)
- [Самостоятельный сброс пароля в Azure AD: рекомендации по развертыванию и управлению](active-directory-passwords-best-practices.md)
- [Отчеты об управлении паролями в Azure AD: как просматривать операции управления паролями в клиенте](active-directory-passwords-get-insights.md)
- [Обратная запись паролей: настройка Azure AD для управления локальными паролями](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
- [Устранение неполадок, связанных с управлением паролями Azure AD](active-directory-passwords-troubleshoot.md)
- [Часто задаваемые вопросы об управлении паролями Azure AD](active-directory-passwords-faq.md)


## Дальнейшие действия

- [Администрирование Azure AD](active-directory-administer.md)
- [Создание и изменение пользователей в Azure AD](active-directory-create-users.md)
- [Управление группами в Azure AD](active-directory-manage-groups.md)

<!---HONumber=AcomDC_0518_2016-->