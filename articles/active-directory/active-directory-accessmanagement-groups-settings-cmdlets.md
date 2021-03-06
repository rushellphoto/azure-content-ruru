<properties
	pageTitle="Настройка параметров групп с помощью командлетов Azure Active Directory | Microsoft Azure"
	description="Управление параметрами групп с помощью командлетов Azure Active Directory."
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
	ms.date="06/21/2016"
	ms.author="curtand"/>


# Настройка параметров групп с помощью командлетов Azure Active Directory

Можно настроить следующие параметры для объединенных групп в каталоге.

1.  Классификации: разделенный запятыми список классификаций, которые пользователи могут присваивать группе. Например, Classified (Конфиденциально), Secret (Секретно) и Top Secret (Совершенно секретно).

2.  URL-адрес правил использования: URL-адрес, указывающий пользователям на условия использования объединенных групп, определенные вашей организацией. Этот URL-адрес будет отображаться в пользовательском интерфейсе, в котором пользователи используют группы.

3.  Создание групп включено: никто из пользователей, некоторые из них или все пользователи могут создавать объединенные группы. Когда этот параметр включен, все пользователи могут создавать группы. Когда этот параметр отключен, никто из пользователей не может создавать группы. Когда параметр отключен, можно также указать группу безопасности, члены которой все же смогут создавать группы.

Эти параметры настраиваются с помощью объектов Settings и SettingsTemplate. Изначально в каталоге не отображаются объекты Settings. Это означает, что каталог настроен с параметрами по умолчанию. Чтобы изменить параметры по умолчанию, необходимо с помощью шаблона параметров создать объект параметров. Шаблоны параметров определены корпорацией Майкрософт.

С [сайта Microsoft Connect](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185) можно скачать модуль, содержащий командлеты, которые используются для этих операций.

## Создание параметров на уровне каталога

Далее приведены действия, необходимые для создания параметров на уровне каталога, применимые ко всем группам Office в каталоге.

1. Если вы не знаете, какой объект SettingTemplate использовать, этот командлет возвращает список шаблонов параметров:

	`Get-MsolAllSettingTemplate`

	![Список шаблонов параметров](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Чтобы добавить URL-адрес правил использования, сначала необходимо получить объект SettingsTemplate, который определяет значение URL-адреса правил использования; это — шаблон Group.Unified:

	`$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Затем создайте новый объект параметров на основе этого шаблона:

	`$setting = $template.CreateSettingsObject()`

4. Затем обновите значение правил использования:

	`$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. И, наконец, примените параметры:

	`New-MsolSettings –SettingsObject $setting`

	![Добавление URL-адреса правил использования](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Ниже приведены параметры, определенные в объекте SettingsTemplate шаблона Group.Unified.

 **Параметр** | **Описание**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Type: String<li>Default: “” | Разделенный запятыми список допустимых значений классификации, которые можно применять к объединенным группам.                
 <ul><li>EnableGroupCreation<li>Type: Boolean<li>Default: True | Флаг, указывающий, разрешено ли создание объединенных групп в каталоге.                               
 <ul><li>GroupCreationAllowedGroupId<li>Type: String<li>Default: “” | Идентификатор GUID группы безопасности, которой разрешено создавать объединенные группы, даже когда EnableGroupCreation == false.
 <ul><li>UsageGuidelinesUrl<li>Type: String<li>Default: “” | Ссылка на правила использования группы.                                                                       

## Чтение параметров на уровне каталога

Далее приведены действия, необходимые для чтения параметров на уровне каталога, применимые ко всем группам Office в каталоге.

1. Чтение всех существующих параметров каталога:

	`Get-MsolAllSettings`

2. Чтение всех параметров определенной группы:

	`Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Чтение определенных параметров каталога, использующих идентификатор GUID SettingId:

	`Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

	![Идентификатор GUID параметров](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## Обновление параметров на уровне каталога

Далее приведены действия, необходимые для обновления параметров на уровне каталога, применимые ко всем группам Office в каталоге.

1. Получение существующего объекта Settings:

	`$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Получение значения, которое требуется обновить:

	`$value = $Setting.GetSettingsValue()`

3. Обновление значения:

	`$value["AllowToAddGuests"] = "false"`

4. Обновление параметра:

	`Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## Удаление параметров на уровне каталога

Далее приведено действие, необходимое для удаления параметров на уровне каталога, применимое ко всем группам Office в каталоге.

	`Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## Справочник по синтаксису командлетов

Дополнительную документацию по PowerShell Azure Active Directory см. в разделе [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260) (Командлеты Azure Active Directory).

## Ссылка на объект SettingsTemplate (объект SettingsTemplate шаблона Group.Unified)

- "name": "EnableGroupCreation", "type": "System.Boolean", "defaultValue": "true", "description": "Логический флаг, указывающий, включена ли функция создания объединенных групп."

- "name": "GroupCreationAllowedGroupId", "type": "System.Guid", "defaultValue": "", "description": "Идентификатор GUID группы безопасности, внесенной в список расширений и имеющей право на создание объединенных групп."

- "name": "ClassificationList", "type": "System.String", "defaultValue": "", "description": "Разделенный запятыми список допустимых значений классификации, которые можно применять к объединенным группам."

- "name": "UsageGuidelinesUrl", "type": "System.String", "defaultValue": "", "description": "Ссылка на правила использования группы."

name | type | defaultValue | description
----------  | ----------  | ---------  | ----------
"EnableGroupCreation" | "System.Boolean" | True | "Логический флаг, указывающий, включена ли функция создания объединенных групп."
"GroupCreationAllowedGroupId" | "System.Guid" | "" | "Идентификатор GUID группы безопасности, внесенной в список расширений и имеющей право на создание объединенных групп."
"ClassificationList" | "System.String" | "" | "Разделенный запятыми список допустимых значений классификации, которые можно применять к объединенным группам."
"UsageGuidelinesUrl" | "System.String" | "" | "Ссылка на правила использования группы."

## Дальнейшие действия

Дополнительную документацию по PowerShell Azure Active Directory см. в разделе [Azure Active Directory Cmdlets](http://go.microsoft.com/fwlink/p/?LinkId=808260) (Командлеты Azure Active Directory).

Дополнительные инструкции от руководителя программы Майкрософт Роба де Йонга (Rob de Jong) см. в [групповом блоге Роба](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Управление доступом к ресурсам с помощью групп Azure Active Directory](active-directory-manage-groups.md)

* [Интеграция локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0622_2016-->