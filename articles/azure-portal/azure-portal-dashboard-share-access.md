<properties
   pageTitle="Доступ к панели мониторинга на портале Azure | Microsoft Azure"
   description="В этой статье описывается, как предоставить общий доступ к панели мониторинга на портале Azure."
   services="azure-portal"
   documentationCenter=""
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="multiple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="tomfitz"/>

# Совместное использование панелей мониторинга Azure

После настройки панели мониторинга ее можно опубликовать и использовать совместно с другими пользователями в организации. Предоставить другим пользователям доступ к панели мониторинга Azure можно с помощью [управления доступом на основе ролей](../active-directory/role-based-access-control-configure.md). Роль назначается пользователю или группе пользователей. Она определяет, можно ли пользователям просматривать или изменять опубликованную панель мониторинга.

Все опубликованные панели мониторинга реализуются как ресурсы Azure, т. е. они представлены в качестве управляемых элементов в рамках подписки и находятся в группе ресурсов. С точки зрения контроля доступа панели мониторинга ничем не отличаются от других ресурсов, например виртуальной машины и учетной записи хранения.

> [AZURE.TIP] Отдельные элементы на панели мониторинга принудительно применяют собственные требования к контролю доступа (в зависимости от отображаемых ими ресурсов). Таким образом, можно создать панель мониторинга с более широкими возможностями совместного использования, по-прежнему обеспечивая защиту данных в отдельных элементах.

## Общие сведения о контроле доступа для панелей мониторинга

С помощью управления доступом на основе ролей пользователям можно назначать роли на трех разных уровнях области:

- подписку
- resource group
- resource

Разрешения, которые вы назначаете, наследуются от подписки до ресурса. Опубликованная панель мониторинга — это ресурс. Значит, пользователям уже могут быть назначены роли для подписки, которая также относится к опубликованной панели мониторинга.

Ниже приведен пример. Допустим, у вас есть подписка Azure, а некоторым участникам рабочей группы назначены роли **владельца**, **участника** или **читателя** в подписке. Пользователи, являющиеся владельцами или участниками, могут просматривать, создавать, изменять и удалять панели мониторинга в подписке, а также получать их списки. Пользователи, являющиеся читателями, могут просматривать панели мониторинга и получать их списки, но не могут изменять или удалять их. Пользователи с доступом для чтения могут вносить локальные изменения в опубликованную панель мониторинга (например, при устранении неполадок), но не могут публиковать эти изменения на сервере. Они получат возможность создать закрытую копию панели мониторинга для собственных потребностей.

Однако можно назначать разрешения для группы ресурсов, содержащей несколько панелей мониторинга, или для отдельной панели мониторинга. Например, вы можете решить, что группа пользователей должна располагать ограниченными разрешениями в подписке, но иметь доступ более высокого уровня к определенной панели мониторинга. Таким образом, пользователям необходимо назначить роль для этой панели мониторинга.

## Публикация панели мониторинга

Предположим, что вы завершили настройку панели мониторинга, которую необходимо использовать совместно с группой пользователей в вашей подписке. В следующих шагах описывается настроенная группа, которая называется "Менеджеры хранилищ" (вы можете присвоить группе любое другое имя). Сведения о создании группы Active Directory и добавлении в нее пользователей см. в статье [Управление группами в Azure Active Directory](../active-directory/active-directory-accessmanagement-manage-groups.md).

1. На панели мониторинга выберите **Общий доступ**.

     ![выбрать "Общий доступ"](./media/azure-portal-dashboard-share-access/select-share.png)

2. Перед назначением доступа необходимо опубликовать панель мониторинга. По умолчанию панель мониторинга будет опубликована в группе ресурсов с именем **Панели мониторинга**. Нажмите кнопку **Опубликовать**.

     ![publish](./media/azure-portal-dashboard-share-access/publish.png)

Панель мониторинга опубликована. Если разрешения, унаследованные от подписки, подходят, больше ничего не нужно делать. Другие пользователи в организации смогут получать доступ к панели мониторинга и изменять ее на основе роли уровня подписки. Однако в целях этого руководства давайте назначим группе пользователей роль для этой панели мониторинга.

## Назначение доступа к панели мониторинга

1. После публикации панели мониторинга щелкните **Управление пользователями**.

     ![управлять пользователями.](./media/azure-portal-dashboard-share-access/manage-users.png)

2. Появится список имеющихся пользователей, которым уже назначена роль для этой панели мониторинга. Ваш список имеющихся пользователей будет отличаться от приведенного на рисунке ниже. Скорее всего, назначения будут унаследованы от подписки. Щелкните **Добавить**, чтобы добавить нового пользователя или группу.

     ![добавить пользователя](./media/azure-portal-dashboard-share-access/existing-users.png)

3. Выберите роль, соответствующую разрешениям, которые нужно предоставить. В этом примере щелкните **Участник**.

     ![выбрать роль](./media/azure-portal-dashboard-share-access/select-role.png)

4. Выберите пользователя или группу, которым нужно назначить роль. Если необходимых пользователя или группы нет в списке, используйте поле поиска. В списке доступных групп будут отображаться группы, созданные в Active Directory.

     ![выбрать пользователя](./media/azure-portal-dashboard-share-access/select-user.png)

5. По завершении добавления пользователей или групп нажмите кнопку **ОК**.

6. Новое назначение будет добавлено в список пользователей. Обратите внимание, что для **доступа** задано состояние **Назначенный**, а не **Унаследованный**.

     ![назначенные роли](./media/azure-portal-dashboard-share-access/assigned-roles.png)

## Дальнейшие действия

- Сведения о списке ролей см. в статье [RBAC: встроенные роли](../active-directory/role-based-access-built-in-roles.md).
- Сведения об управлении ресурсами см. в статье [Управление ресурсами Azure через портал](resource-group-portal.md).

<!---HONumber=AcomDC_0803_2016-->