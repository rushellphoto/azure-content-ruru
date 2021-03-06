<properties
	pageTitle="Командлеты предварительной версии Azure Active Directory PowerShell для управления группами в Azure AD | Microsoft Azure"
	description="На этой странице представлены примеры командлетов PowerShell, которые помогут вам управлять группами в Azure Active Directory."
    keywords="Azure AD, Azure Active Directory, PowerShell, группы, управление группами"
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
	ms.date="07/11/2016"
	ms.author="curtand"/>

# Командлеты предварительной версии Azure Active Directory для управления группами

В следующем документе представлены примеры использования PowerShell для управления группами в Azure Active Directory (Azure AD). Также в нем содержатся сведения о том, как выполнить настройки предварительной версии модуля Azure AD PowerShell. Во-первых, необходимо [скачать модуль Azure AD PowerShell](http://go.microsoft.com/fwlink/p/?linkid=236297).

## Установка модуля Azure AD PowerShell

Чтобы установить предварительную версию модуля AzureAD PowerShell , воспользуйтесь следующими командами:

	PS C:\Windows\system32> install-module azureadpreview

Чтобы проверить, установлена ли предварительная версия модуля, воспользуйтесь следующей командой:

	PS C:\Windows\system32> get-module azureadpreview

	ModuleType Version    Name                                ExportedCommands
	---------- -------    ----                                ----------------
	Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Теперь можно начать использование командлетов в модуле. Полное описание командлетов в предварительной версии модуля AzureAD см. в [электронной справочной документации](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## Подключение к каталогу
Прежде чем приступить к управлению группами с помощью командлетов предварительной версии Azure AD PowerShell, необходимо подключить сеанс PowerShell к каталогу, которым вы хотите управлять. Для этого воспользуйтесь следующей командой:

	PS C:\Windows\system32> Connect-AzureAD -Force

Командлет запросит ввести учетные данные, которые вы хотите использовать для доступа к каталогу. В этом примере для доступа к демонстрационному каталогу используется karen@drumkit.onmicrosoft.com. Командлет возвращает подтверждение, показывающее, что сеанс был успешно подключен к каталогу:

	Account                       Environment Tenant
	-------                       ----------- ------
	Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Теперь можно начать использование командлетов предварительной версии AzureAD для управления группами в каталоге.

## Получение сведений о группах
Для получения сведений о существующих группах из каталога можно воспользоваться командлетом Get-AzureADGroups. Чтобы получить сведения о всех группах в каталоге, используйте командлет без параметров:

	PS C:\Windows\system32> get-azureadgroup

Командлет возвращает сведения о всех группах в подключенном каталоге.

Для получения сведений о конкретной группе, для которой указывается идентификатор объекта, можно использовать параметр -objectID:

	PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Командлет возвращает сведения о группе, идентификатор objectID которой совпадает со значением введенного параметра:

	DeletionTimeStamp            :
	ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
	ObjectType                   : Group
	Description                  :
	DirSyncEnabled               :
	DisplayName                  : Pacific NW Support
	LastDirSyncTime              :
	Mail                         :
	MailEnabled                  : False
	MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
	OnPremisesSecurityIdentifier :
	ProvisioningErrors           : {}
	ProxyAddresses               : {}
	SecurityEnabled              : True

Для поиска конкретной группы можно использовать параметр -filter. Этот параметр принимает предложение фильтра ODATA и возвращает сведения о всех группах, соответствующих фильтру, как показано в следующем примере:

	PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


	DeletionTimeStamp            :
	ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
	ObjectType                   : Group
	Description                  : Intune Administrators
	DirSyncEnabled               :
	DisplayName                  : Intune Administrators
	LastDirSyncTime              :
	Mail                         :
	MailEnabled                  : False
	MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
	OnPremisesSecurityIdentifier :
	ProvisioningErrors           : {}
	ProxyAddresses               : {}
	SecurityEnabled              : True

Обратите внимание, что командлеты AzureAD PowerShell используют стандарт запросов OData; дополнительные сведения можно найти [здесь](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## Создание групп
Чтобы создать новую группу в каталоге, используйте командлет New-AzureADGroup. Этот командлет создает новую группу безопасности с именем Marketing:

	PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## Обновление групп
Чтобы обновить существующую группу, используйте командлет Set-AzureADGroup. В этом примере изменяется свойство DisplayName группы Intune Administrators. Сначала с помощью командлета Get-AzureADGroup мы находим группу и фильтруем, используя атрибут DisplayName:

	PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


	DeletionTimeStamp            :
	ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
	ObjectType                   : Group
	Description                  : Intune Administrators
	DirSyncEnabled               :
	DisplayName                  : Intune Administrators
	LastDirSyncTime              :
	Mail                         :
	MailEnabled                  : False
	MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
	OnPremisesSecurityIdentifier :
	ProvisioningErrors           : {}
	ProxyAddresses               : {}
	SecurityEnabled              : True

Затем мы меняем свойство Description на новое значение — Intune Device Administrators:

	PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Если теперь снова выполнить поиск группы, то мы увидим, что свойство Description обновилось и отражает новое значение:

	PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


	DeletionTimeStamp            :
	ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
	ObjectType                   : Group
	Description                  : Intune Device Administrators
	DirSyncEnabled               :
	DisplayName                  : Intune Administrators
	LastDirSyncTime              :
	Mail                         :
	MailEnabled                  : False
	MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
	OnPremisesSecurityIdentifier :
	ProvisioningErrors           : {}
	ProxyAddresses               : {}
	SecurityEnabled              : True

## Удаление групп
Для удаления групп из каталога используйте командлет Remove-AzureADGroup, как показано ниже:

	PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## Управление членами групп
Для добавления в группу новых членов используйте командлет Add-AzureADGroupMember. Эта команда добавляет члена в группу Intune Administrators, которую мы использовали в предыдущем примере:

	PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Параметр -ObjectId — это идентификатор объекта группы, в которую требуется добавить члена, а -RefObjectId — это идентификатор объекта пользователя, которого требуется добавить в качестве члена группы.

Чтобы получить сведения о существующих членах группы, используйте командлет Get-AzureADGroupMember, как показано в этом примере:

	PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

	DeletionTimeStamp ObjectId                             ObjectType
	----------------- --------                             ----------
                  		72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                  		8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Чтобы удалить члена, ранее добавленного в группу, используйте командлет Remove-AzureADGroupMember, как показано здесь:

	PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Чтобы проверить, является ли пользователь членом какой-либо группы, используйте командлет Select-AzureADGroupIdsUserIsMemberOf. В качестве параметров этот командлет принимает идентификатор объекта пользователя, для которого выполняется проверка членства в группах, и список групп, в которых проверяется членство. Список групп необходимо указать в форме сложной переменной типа Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck, поэтому сначала необходимо создать переменную с этим типом:

	PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Затем необходимо указать значения идентификаторов группы для регистрации атрибута GroupIds этой сложной переменной:

	PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Теперь, если требуется проверить членство пользователя с идентификатором объекта 72cd4bbd-2594-40a2-935c-016f3cfeeeea в группах в $g, используйте следующую команду:

	PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

	OdataMetadata 																								Value
	-------------      																							-----
	https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String) 			{31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Возвращаемое значение — это список групп, членом которых является данный пользователь. Этот метод также можно применять для проверки членства в контактах, группах или субъектах-службах для определенного списка групп. Для этого используйте командлеты Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf и Select-AzureADGroupIdsServicePrincipalIsMemberOf.

## Управление владельцами групп
Чтобы добавить в группу новых владельцев, воспользуйтесь командлетом Add-AzureADGroupOwner:

	PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Параметр -ObjectId — это идентификатор объекта группы, в которую требуется добавить владельца, а -RefObjectId — это идентификатор объекта пользователя, которого требуется добавить в качестве владельца группы.

Для получения сведений о владельцах группы используйте командлет Get-AzureADGroupOwner:

	PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Командлет возвращает список владельцев для указанной группы:

	DeletionTimeStamp ObjectId                             ObjectType
	----------------- --------                             ----------
                  		e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Для удаления владельца из группы, используйте командлет Remove-AzureADGroupOwner:

	PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## Дальнейшие действия

Дополнительную документацию по PowerShell Azure Active Directory см. в разделе [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260) (Командлеты Azure Active Directory).

* [Управление доступом к ресурсам с помощью групп Azure Active Directory](active-directory-manage-groups.md)

* [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0803_2016-->