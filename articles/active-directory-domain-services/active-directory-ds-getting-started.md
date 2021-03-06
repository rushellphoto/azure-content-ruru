<properties
	pageTitle="Доменные службы Azure AD: создание группы администраторов контроллера домена AAD | Microsoft Azure"
	description="Приступая к работе с доменными службами Azure Active Directory (предварительная версия)"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="maheshu"/>

# Доменные службы Azure AD *(предварительная версия)*: создание группы администраторов контроллера домена AAD

В этой статье приводятся задачи конфигурации, необходимые для включения доменных служб Azure AD для вашего клиента Azure AD.

## Задача 1. Создание группы "Администраторы контроллера домена AAD"
Первой задачей является создание административной группы в клиенте Azure Active Directory. Эта специальная административная группа называется **Администраторы контроллера домена AAD**. Членам этой группы будут предоставлены административные права для компьютеров, которые будут присоединены к домену доменных служб Azure AD, который вы настроите. После присоединения к домену эта группа будет добавлена в группу "Администраторы" на компьютерах, которые были присоединены к домену. Кроме того, члены этой группы также смогут удаленно подключаться к компьютерам, присоединенным к домену.

> [AZURE.NOTE] Вы не получаете права администратора домена или администратора предприятия в управляемом домене, созданном с помощью доменных служб Azure AD. Так как это управляемый домен, эти привилегии зарезервированы службой и не становятся доступными для пользователей каталога. В то же время для выполнения некоторых привилегированных операций можно использовать специальную группу администраторов, созданную в этой задаче конфигурации. Эти операции включают присоединение компьютеров к домену, входящих в группу администраторов на компьютерах, присоединенных к домену, настройку групповой политики и т. д.

При выполнении этой задачи конфигурации вы создадите административную группу и добавите в нее одного или нескольких пользователей из своего каталога. Для создания административной группы для доменных служб Azure AD выполните следующие действия.

1. Перейдите на **классический портал Azure** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).

2. На панели слева выберите **Active Directory**.

3. Выберите клиента Azure AD (каталог), для которого требуется включить доменные службы Azure AD. Обратите внимание, что для каждого каталога Azure AD можно создать только один домен.

    ![Выбор каталога Azure AD](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Щелкните вкладку **Группы**.

5. Щелкните **Добавить группу** в области задач в нижней части страницы, чтобы добавить группу в каталог.

6. Создайте группу с именем **AAD DC Administrators**.

    > [AZURE.WARNING] Для доступа к доменным службам Azure AD необходимо создать группу именно с таким именем.

	![Создание группы администраторов](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Для этой группы можно добавить описание, которое позволит другим пользователям клиента Azure AD понять, что эта группа будет использоваться для предоставления прав администратора для доменных служб Azure AD.

8. После создания группы щелкните имя группы, чтобы просмотреть свойства этой группы. Нажмите кнопку **Добавить участников** на нижней панели, чтобы добавить пользователей в качестве участников этой группы.

9. В диалоговом окне **Добавление участников** выберите пользователей, которые должны стать членами этой группы, и в конце установите флажок.

    ![Добавление пользователей в группу администраторов](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## Задача 2. Создание или выбор виртуальной сети Azure.
Следующая задача конфигурации — [создать или выбрать виртуальную сеть Azure](active-directory-ds-getting-started-vnet.md).

<!---HONumber=AcomDC_0706_2016-->