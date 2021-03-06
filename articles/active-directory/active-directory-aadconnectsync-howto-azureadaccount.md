<properties
	pageTitle="Синхронизация Azure AD Connect: управление учетной записью службы Azure AD | Microsoft Azure"
	description="В этом разделе описывается процедура восстановления учетной записи службы Azure AD."
	services="active-directory"
    keywords="AADSTS70002, AADSTS50054 Как сбросить пароль для учетной записи службы соединителя синхронизации Azure AD Connect"
	documentationCenter=""
	authors="andkjell"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/27/2016"
	ms.author="andkjell"/>

# Синхронизация Azure AD Connect: управление учетной записью службы Azure AD
Учетная запись службы, используемая соединителем Azure AD, как правило, не нуждается в обслуживании. Но если требуется сбросить ее учетные данные, то в этом разделе вы найдете необходимые сведения. Например, это может произойти, если глобальный администратор по ошибке сбросит пароль учетной записи службы с помощью PowerShell.

## Сброс учетных данных
Если учетная запись службы, определенная в соединителе Azure AD, не может связаться с Azure AD из-за проблем с проверкой подлинности, можно сбросить пароль.

1. Войдите на сервер синхронизации Azure AD Connect и запустите PowerShell.
2. Запустите `Add-ADSyncAADServiceAccount`. ![Командлет PowerShell addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Введите учетные данные глобального администратора Azure AD.

Этот командлет сбросит пароль для учетной записи службы и обновит его в Azure AD и модуле синхронизации.

## Известные проблемы, которые можно решить с помощью описанных действий
Ниже представлен список обнаруженных пользователями ошибок, которые можно исправить, выполнив следующие действия.

-----------
Событие 6900 Сервер обнаружил непредвиденную ошибку при обработке уведомления об изменении пароля. AADSTS70002: ошибка при проверке учетных данных. AADSTS50054: старый пароль используется для проверки подлинности.

----------
Событие 659 Произошла ошибка при получении конфигурации синхронизации политики паролей. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException. AADSTS70002: ошибка при проверке учетных данных. AADSTS50054: старый пароль используется для проверки подлинности.

## Дальнейшие действия
Узнайте больше об [интеграции локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0629_2016-->