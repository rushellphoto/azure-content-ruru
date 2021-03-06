<properties
	pageTitle="Службы синхронизации Azure AD Connect: рекомендации по изменению конфигурации по умолчанию | Microsoft Azure"
	description="В этой статье приведены рекомендации по изменению стандартной конфигурации служб синхронизации Azure AD Connect."
	services="active-directory"
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
	ms.author="markvi;andkjell"/>


# Службы синхронизации Azure AD Connect: рекомендации по изменению конфигурации по умолчанию
В этой статье описываются поддерживаемые и неподдерживаемые изменения в службах синхронизации Azure AD Connect.

Конфигурация, созданная при помощи Azure AD Connect, подходит для большинства сред, в которых локальные каталоги Active Directory синхронизируются с Azure AD. Однако в некоторых случаях, чтобы выполнить определенную задачу или соблюсти нужные требования, в конфигурацию приходится вносить изменения.

## Изменения в учетной записи службы
Службы синхронизации Azure AD Connect работают под учетной записью службы, созданной мастером установки. Эта учетная запись содержит ключи шифрования для базы данных, используемой в процессе синхронизации. Она создается с паролем длиной 127 символов, срок действия которого не ограничен.

- Изменение или сброс пароля учетной записи службы **не поддерживается**. Это приведет к уничтожению ключей шифрования, и служба не сможет получить доступ к базе данных и запуститься.

## Изменения в планировщике
Начиная со сборки 1.1 (февраль 2016 года) можно настроить в [планировщике](active-directory-aadconnectsync-feature-scheduler.md) другой цикл синхронизации по сравнению с 30 минутами по умолчанию.

## Изменения в правилах синхронизации
Мастер установки предлагает конфигурацию, которая должна работать для наиболее распространенных сценариев. Если необходимо внести изменения в конфигурацию, чтобы она по-прежнему была поддерживаемой, необходимо следовать этим правилам.

- Вы можете [изменить потоки атрибутов](#change-attribute-flows), если прямые потоки атрибутов по умолчанию не подходят для вашей организации.
- Если вы не хотите [отправлять атрибут в потоке](#do-not-flow-an-attribute) и вам требуется удалить существующие значения атрибута в Azure AD, для этого необходимо создать правило.
- [Отключайте ненужные правила синхронизации](#disable-an-unwanted-sync-rule), вместо того чтобы их удалять. Удаленное правило будет снова создано во время обновления.
- Если вы хотите [внести изменения в стандартное правило](#change-an-out-of-box-rule), сделайте его копию и отключите стандартное правило. Редактор правил синхронизации поможет вам в этом.
- Экспортируйте настраиваемые правила синхронизации с помощью редактора правил синхронизации. Так вы получите скрипт PowerShell, который позволяет легко создать их заново для сценария аварийного восстановления.

>[AZURE.WARNING] Стандартные правила синхронизации имеют отпечаток. При внесении изменений в эти правила соответствие отпечатка нарушится, и в будущем при попытке применить новый выпуск Azure AD Connect могут возникнуть проблемы. Выполняйте изменения только так, как описано в этой статье.

### Изменение потоков атрибутов
В некоторых случаях потоки атрибутов по умолчанию не подходят для организации.

Необходимо соблюдать следующие правила.

- Создайте новое правило синхронизации с потоками атрибутов. Путем указания более высокого приоритета (меньшего порядкового номера) правила переопределяют любые стандартные потоки атрибутов.
- Не добавляйте дополнительные потоки в стандартное правило. Эти изменения будут потеряны при обновлении.

В компании Fabrikam есть лес, в котором локальные алфавиты используются для указания имени, фамилии и отображаемого имени. Представления этих атрибутов хранятся в атрибутах расширения, отображенные символами латиницы. При формировании глобального списка адресов в Azure AD и Office 365 организации требуется использовать эти представления.

В конфигурации по умолчанию объект из локального леса выглядит следующим образом:

![Поток атрибутов 1](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/attributeflowjp1.png)

Чтобы создать правило с другими потоками атрибутов, выполните следующие действия.

- Запустите **редактор правил синхронизации** из меню "Пуск".
- Не снимая флажок **Входящие** слева, нажмите кнопку **Добавить новое правило**.
- Присвойте правилу имя и описание. Выберите локальный каталог Active Directory и соответствующие типы объектов. В поле **Тип связи** выберите **Присоединение**. Для задания приоритета выберите число, которое не используется другим правилом. Приоритет стандартных правил начинается с 100, поэтому в этом примере используется значение 50. ![Поток атрибутов 2](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/attributeflowjp2.png)
- Оставьте значение области пустым (т. е. правило должно применяться для всех объектов-пользователей в лесу).
- Оставьте поле правил присоединения пустым (т. е. стандартные правила могут обрабатывать любые присоединения).
- В разделе "Преобразования" создайте следующие потоки. ![Поток атрибутов 3](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/attributeflowjp3.png)
- Нажмите кнопку **Добавить**, чтобы сохранить правило.
- Перейдите в **Диспетчер службы синхронизации**. В разделе **Соединители** выберите соединитель, в который мы добавили правило. Выберите **Выполнить**, а затем **Полная синхронизация**. Функция полной синхронизации повторно вычислит все объекты, используя текущие правила.

Это конечный результат для того же объекта с применением данного настраиваемого правила:

![Поток атрибутов 4](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/attributeflowjp4.png)

### Отключение потока для атрибута
Существует два способа отключения потока для атрибута. Первый доступен в мастере установки и позволяет [удалить выбранные атрибуты](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Им можно воспользоваться, если атрибут никогда не синхронизировался. Однако если вы начали синхронизацию этого атрибута и позже удалили его с помощью этой функции, то модуль синхронизации прекратит управление атрибутом, и в Azure AD будут оставлены существующие значения.

Если вы хотите удалить значение атрибута и гарантировать, что в будущем для него не будет включен поток, нужно создать настраиваемое правило.

В компании Fabrikam мы обнаружили, что некоторые атрибуты, которые мы синхронизируем с облаком, лишние. Мы хотим удалить эти атрибуты из Azure AD.

![Атрибуты расширения](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/badextensionattribute.png)

- Создайте новое входящее правило синхронизации и укажите описание ![Описания](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/syncruledescription.png).
- Создайте потоки атрибута типа **Выражение** с источником **AuthoritativeNull**. Литеральный тип **AuthoritativeNull** указывает, что значение должно быть пустым в MV, даже если правило синхронизации с более низким приоритетом пытается заполнить значение. ![Атрибуты расширения](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/syncruletransformations.png)
- Сохраните правило синхронизации. Запустите **службу синхронизации**, найдите соединитель, выберите **Запустить** и **Полная синхронизация**. Это приведет к пересчету всех потоков атрибутов.
- Убедитесь, что требуемые изменения будут экспортированы, с помощью поиска в пространстве соединителя. ![Промежуточное удаление](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/deletetobeexported.png)

### Отключение ненужного правила синхронизации
Не удаляйте стандартное правило синхронизации; оно будет создано заново при следующем обновлении.

В некоторых случаях мастер установки может создать конфигурацию, которая не будет работать для вашей топологии. Например, если у вас топология леса ресурсов учетной записи, но вы расширили схему в лесу учетной записи со схемой Exchange, то правила для Exchange будут создаваться для леса учетных записей и для леса ресурсов. В этом случае необходимо отключить правило синхронизации для Exchange.

![Отключенное правило синхронизации](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/exchangedisabledrule.png)

На рисунке выше установки мастер обнаружил старую схему Exchange 2003 в лесу учетных записей. Она была добавлена до того, как лес ресурсов был введен в среду компании Fabrikam. Чтобы убедиться, что атрибуты из старой реализации Exchange не синхронизируются, необходимо отключить правило синхронизации, как описано в этой статье.

### Изменение стандартного правила
Если вы хотите внести изменения в стандартное правило, сделайте его копию и отключите исходное правило. Затем внесите необходимые изменения в клонированное правило. Редактор правил синхронизации поможет вам в этом. При открытии стандартного правила отображается это диалоговое окно:

![Предупреждение стандартного правила](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/warningoutofboxrule.png)

Выберите **Да** для создания копии правила. Затем будет открыто клонированное правило.

![Клонированное правило](./media/active-directory-aadconnectsync-best-practices-changing-default-configuration/clonedrule.png)

В этом клонированном правиле выполните необходимые изменения в области, объединении и преобразованиях.

## Дальнейшие действия
Узнайте больше о настройке [службы синхронизации Azure AD Connect](active-directory-aadconnectsync-whatis.md).

Узнайте больше об [интеграции локальных удостоверений с Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0629_2016-->