<properties
	pageTitle="Назначение ролей администратора в Azure Active Directory | Microsoft Azure"
	description="Поясняет, какие роли администратора доступны в Azure Active Directory и как их назначить."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/29/2016"
	ms.author="curtand"/>

# Назначение ролей администратора в Azure Active Directory

С помощью Azure Active Directory (Azure AD) можно назначить несколько администраторов, которые будут выполнять различные функции. У них будет доступ к различным возможностям на портале Azure (обновленном или классическом) и, в зависимости от их роли, они смогут создавать или изменять пользователей, назначать административные роли другим пользователям, сбрасывать пароли пользователей, управлять лицензиями и доменами. У пользователя, которому назначена роль администратора, будут одинаковые разрешения во всех облачных службах, на которые подписана ваша организация, независимо от того, назначена ли роль на портале Office 365, на классическом портале Azure или с помощью модуля Azure AD для Windows PowerShell.

Доступны следующие роли администратора.

- **Администратор выставления счетов**: совершает покупки, управляет подписками и обращениями в службу поддержки, а также отслеживает работоспособность службы.

- **Глобальный администратор**: имеет доступ ко всем административным функциям. Пользователь, зарегистрировавший учетную запись Azure, становится глобальным администратором. Только глобальные администраторы могут назначать другие административные роли. В компании может быть несколько глобальных администраторов.

	> [AZURE.NOTE] В API Microsoft Graph, API Azure AD Graph и Azure AD PowerShell эта роль определяется как "Администратор организации".

- **Администратор паролей**: сбрасывает пароли, управляет запросами на обслуживание и отслеживает работоспособность службы. Администраторы паролей могут сбрасывать пароли только для пользователей и других администраторов паролей.

	> [AZURE.NOTE] В API Microsoft Graph, API Azure AD Graph и Azure AD PowerShell эта роль определяется как "Администратор службы поддержки".

- **Администратор службы**: управляет запросами на обслуживание и отслеживает работоспособность службы.

	> [AZURE.NOTE] Чтобы назначить пользователю роль администратора службы, глобальный администратор наделяет его правами администратора в такой службе, как Exchange Online, и затем назначает ему роль администратора службы на классическом портале Azure.

- **Администратор пользователей**: сбрасывает пароли, отслеживает работоспособность службы и управляет учетными записями пользователей, групп пользователей и запросами на обслуживание. К разрешениям администраторов пользователей применяются некоторые ограничения. Например, они не могут удалить глобального администратора или создать других администраторов. Кроме того, они не могут сбрасывать пароли для глобальных администраторов, администраторов выставления счетов и администраторов служб.

- **Чтение данных безопасности**: доступ только для чтения к ряду функций безопасности центра защиты идентификации, управления привилегированными пользователями, отслеживания работоспособности служб Office 365 и центра защиты Office 365.

- **Администратор безопасности**: все разрешения только для чтения, доступные роли **Чтение данных безопасности**, а также ряд дополнительных разрешений администратора для тех же служб: центр защиты идентификации, управление привилегированными пользователями, отслеживание работоспособности служб Office 365 и центр защиты Office 365.

## Разрешения администратора

### Администратор выставления счетов

Может | Не может
------------- | -------------
<p>Просматривать сведения о компании и пользователях</p><p>Управлять запросами на обслуживание Office</p><p>Выполнять операции выставления счетов и совершать покупки продуктов Office</p> | <p>Сбрасывать пароли пользователей</p><p>Создавать представления пользователей и управлять ими</p><p>Создавать, изменять, удалять пользователей и группы, а также управлять пользовательскими лицензиями</p><p>Управлять доменами</p><p>Управлять данными компании</p><p>Делегировать административные роли другим пользователям</p><p>Использовать синхронизацию каталогов</p>

### Глобальный администратор

Может | Не может
------------- | -------------
<p>Просматривать сведения о компании и пользователях</p><p>Управлять запросами на обслуживание Office</p><p>Выполнять операции выставления счетов и совершать покупки продуктов Office</p> <p>Сбрасывать пароли пользователей</p><p>Создавать представления пользователей и управлять ими</p><p>Создавать, изменять, удалять пользователей и группы, а также управлять пользовательскими лицензиями</p><p>Управлять доменами</p><p>Управлять данными компании</p><p>Делегировать административные роли другим пользователям</p><p>Использовать синхронизацию каталогов</p><p>Включать и отключать многофакторную проверку подлинности</p> | Недоступно

### Администратор паролей

Может | Не может
------------- | -------------
<p>Просматривать сведения о компании и пользователях</p><p>Управлять запросами на обслуживание Office</p><p>Сбрасывать пароли пользователей</p> | <p>Выполнять операции выставления счетов и совершать покупки продуктов Office</p><p>Создавать представления пользователей и управлять ими</p><p>Создавать, изменять, удалять пользователей и группы, а также управлять пользовательскими лицензиями</p><p>Управлять доменами</p><p>Управлять данными компании</p><p>Делегировать административные роли другим пользователям</p><p>Использовать синхронизацию каталогов</p>

### Администратор служб

Может | Не может
------------- | -------------
<p>Просматривать сведения о компании и пользователях</p><p>Управлять запросами на обслуживание Office</p> | <p>Сбрасывать пароли пользователей</p><p>Выполнять операции выставления счетов и совершать покупки продуктов Office</p><p>Создавать представления пользователей и управлять ими</p><p>Создавать, изменять, удалять пользователей и группы, а также управлять пользовательскими лицензиями</p><p>Управлять доменами</p><p>Управлять данными компании</p><p>Делегировать административные роли другим пользователям</p><p>Использовать синхронизацию каталогов</p>

### Администратор пользователей

Может | Не может
------------- | -------------
<p>Просматривать сведения о компании и пользователях</p><p>Управлять запросами на обслуживание Office</p><p>Сбрасывать пароли пользователей без ограничений. Администраторы не могут сбрасывать пароли для администраторов выставления счетов, глобальных администраторов и администраторов службы.</p><p>Создавать представления пользователей и управлять ими</p><p>Создавать, изменять, удалять пользователей и группы, а также управлять пользовательскими лицензиями без ограничений. Администраторы не могут удалить глобального администратора или создать других администраторов.</p> | <p>Выполнять операции выставления счетов и совершать покупки продуктов Office</p><p>Управлять доменами</p><p>Управлять данными компании</p><p>Делегировать административные роли другим пользователям</p><p>Использовать синхронизацию каталогов</p><p>Включать и отключать многофакторную проверку подлинности</p>

### Чтение данных безопасности

В | Может
------------- | -------------
Центр защиты идентификации | Чтение всех отчетов о безопасности и сведений о параметрах функций безопасности<ul><li>Защита от нежелательной почты<li>Шифрование<li>Защита от потери данных<li>Защита от вредоносных программ<li>Дополнительная защита от угроз<li>Защита от фишинга<li>Правила обработки почты
управление привилегированными пользователями; | <p>Имеет доступ только для чтения ко всем сведениям, отображаемым в службе управления привилегированными пользователями (PIM) Azure AD: политики и отчеты для назначения ролей Azure AD, проверки безопасности, а в будущем также доступ на чтение для данных политик и отчетов, относящихся к сценариям за пределами назначения ролей Azure AD.<p>**Не может** регистрироваться в службе управления привилегированными пользователями Azure AD или вносить в нее какие-либо изменения. Используя портал службы управления привилегированными пользователями или PowerShell, кто-то в этой роли может активировать дополнительные роли (например, глобальный администратор или администратор привилегированных ролей), если пользователь является кандидатом на них.
<p>Отслеживание работоспособности служб Office 365</p><p>Центр защиты Office 365</p> | <ul><li>Чтение оповещений и управление ими<li>Чтение политик безопасности<li>Чтение аналитики угроз, Cloud App Discovery и карантин в поиске и расследовании<li>Чтение всех отчетов

### Администратор безопасности

В | Может
------------- | -------------
Центр защиты идентификации | <ul><li>Все разрешения роли "Чтение данных безопасности".<li>А также возможность выполнять все операции центра защиты идентификации кроме сброса паролей.
управление привилегированными пользователями; | <ul><li>Все разрешения роли "Чтение данных безопасности".<li>**Не может** управлять принадлежностью к ролям или параметрами ролей Azure AD.
<p>Отслеживание работоспособности служб Office 365</p><p>Центр защиты Office 365 | <ul><li>Все разрешения роли "Чтение данных безопасности".<li>Может настраивать все параметры функции "Дополнительная защита от угроз" (защита от вредоносных программ и вирусов, защита от вредоносных URL-адресов, трассировка URL-адресов и т. д.).

## Сведения о роли глобального администратора

Глобальный администратор имеет доступ ко всем административным функциям. По умолчанию роль глобального администратора каталога назначается пользователю, зарегистрировавшему подписку Azure. Только глобальные администраторы могут назначать другие административные роли.

## Назначение и удаление ролей администратора

1. На классическом портале Azure щелкните **Active Directory**, а затем выберите имя каталога своей организации.

2. На странице **Пользователи** щелкните отображаемое имя пользователя, которое требуется изменить.

3. Из списка **Роль в организации** выберите роль администратора, которую требуется назначить этому пользователю, или выберите **Пользователь**, чтобы удалить существующую роль администратора.

4. В поле **запасной адрес электронной почты** введите электронный адрес. Он используется для важных уведомлений, в том числе для самостоятельного сброса пароля, поэтому у пользователя должен быть доступ к учетной записи электронной почты независимо от доступа к Azure.

5. Выберите **Разрешить** или **Блокировать**, чтобы указать, следует ли разрешить пользователю вход и доступ к службам.

6. Выберите расположение из раскрывающегося списка **Место использования**.

7. После завершения нажмите кнопку **Сохранить**.

## Дальнейшие действия

- Дополнительные сведения об изменении администраторов для подписки Azure см. в статье [Добавление или изменение ролей администратора Azure](../billing-add-change-azure-subscription-administrator.md).

- Дополнительные сведения о том, как осуществляется доступ к ресурсам в Microsoft Azure, см. в статье [Основные сведения о доступе к ресурсам в Azure](active-directory-understanding-resource-access.md).

- Дополнительные сведения о связи Azure Active Directory с подпиской Azure см. в статье [Связь между подписками Azure и Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).

- [Управление пользователями](active-directory-create-users.md)

- [Управление паролями](active-directory-manage-passwords.md)

- [Управление группами](active-directory-manage-groups.md)

<!---HONumber=AcomDC_0706_2016-->