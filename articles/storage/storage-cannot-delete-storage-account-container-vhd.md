<properties
	pageTitle="Устранение неполадок при удалении учетных записей хранения Azure, контейнеров или виртуальных жестких дисков | Microsoft Azure"
	description="Устранение неполадок при удалении учетных записей хранения Azure, контейнеров или виртуальных жестких дисков"
	services="storage"
	documentationCenter=""
	authors="genlin"
	manager="felixwu"
	editor=""
	tags="storage"/>

<tags
	ms.service="storage"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/20/2016"
	ms.author="genli"/>

# Устранение неполадок при удалении учетных записей хранения Azure, контейнеров или виртуальных жестких дисков

## Сводка
При попытке удалить учетную запись хранения Azure, контейнер или виртуальный жесткий диск на [портале Azure](https://portal.azure.com/) или на [классическом портале Azure](https://manage.windowsazure.com/) могут возникать ошибки. Проблемы могут возникать в следующих обстоятельствах:

-	При удалении виртуальной машины диск и виртуальный жесткий диск не удаляются автоматически. Это может быть причиной сбоя удаления учетной записи хранения. Диск не удаляется, чтобы вы могли использовать его для подключения другой виртуальной машины.

-	Диск или большой двоичный объект, связанный с диском, по-прежнему находится в аренде.

Если в этой статье нет решения вашей проблемы с Azure, посетите форумы по Azure [на сайтах MSDN и Stack Overflow](https://azure.microsoft.com/support/forums/). Проблему можно разместить на этих форумах или отправить по адресу @AzureSupport в Твиттере. Кроме того, можно отправить запрос в службу поддержки Azure, выбрав **Получить поддержку** на сайте [службы поддержки Azure](https://azure.microsoft.com/support/options/).

## Способы устранения:
Для устранения наиболее распространенных проблем попробуйте следующий метод:

1. Переключитесь на [классический портал Azure](https://manage.windowsazure.com/).
2. Выберите **ВИРТУАЛЬНАЯ МАШИНА** > **ДИСКИ**.

	![Изображение дисков на виртуальных машинах на классическом портале Azure.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Найдите диски, связанные с учетной записью хранения, контейнером или виртуальным жестким диском, которые нужно удалить. Проверив расположение диска, вы найдете связанную учетную запись хранения, контейнер или виртуальный жесткий диск.

	![Рисунок, демонстрирующий сведения о расположении для дисков на классическом портале Azure](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Убедитесь, что в поле дисков **Подключено к** нет виртуальных машин, а затем удалите диски.

 	> [AZURE.NOTE] Если диск подключен к виртуальной машине, вы не сможете его удалить. Диски отключаются от удаленной виртуальной машины асинхронно. После удаления виртуальной машины может пройти несколько минут, прежде чем это поле будет очищено.

5. Выберите **ВИРТУАЛЬНАЯ МАШИНА** > **ОБРАЗЫ**, а затем удалите образы, которые связаны с учетной записью хранения, контейнером или виртуальным жестким диском.

	После этого попробуйте удалить учетную запись хранения, контейнер или виртуальный жесткий диск снова.

> [AZURE.WARNING] Создайте резервные копии нужных данных, прежде чем удалять учетную запись. Восстановить удаленную учетную запись хранения или ее содержимое невозможно. Это также касается любых ресурсов в учетной записи. Восстановить удаленный виртуальный жесткий диск, большой двоичный объект, таблицу, очередь или файл невозможно. Убедитесь, что ресурс не используется.

## Симптом

В следующем разделе приводится список наиболее распространенных ошибок, которые могут возникать при попытке удаления учетных записей хранения Azure, контейнеров или виртуальных жестких дисков.

### Сценарий 1. Невозможно удалить учетную запись хранения

Когда вы переходите в учетную запись хранения на [портале Azure](https://portal.azure.com/) или на [классическом портале Azure](https://manage.windowsazure.com/) и выбираете **Удалить**, может появиться следующее сообщение об ошибке:

**На портале Azure**:

*Не удалось удалить учетную запись хранения <vm-storage-account-name>. Невозможно удалить учетную запись хранения <vm-storage-account-name>: "Учетная запись хранения <vm-storage-account-name> содержит активные образы или диски. Убедитесь, что эти образы или диски удалены, перед тем как удалять эту учетную запись хранения".*

**На классическом портале Azure**:

*Учетная запись хранения <vm-storage-account-name> содержит активные образы или диски, например xxxxxxxxx- xxxxxxxxx-O-209490240936090599. Убедитесь, что эти образы или диски удалены, перед тем как удалять эту учетную запись хранения.*

Также может появиться следующее сообщение об ошибке:

**На портале Azure**:

*Учетная запись хранения <vm-storage-account-name> содержит 1 контейнер, в котором есть активный образ или артефакты диска. Убедитесь, что эти артефакты удалены из репозитория образов, перед удалением этой учетной записи хранения*.

**На классическом портале Azure**:

*Ошибка отправки. Учетная запись хранения <vm-storage-account-name> содержит 1 контейнер, в котором есть активный образ или артефакты диска. Убедитесь, что эти артефакты удалены из репозитория образов, перед удалением этой учетной записи хранения. Когда вы пытаетесь удалить учетную запись хранения, у которой есть связанные активные диски, появится сообщение о том, что существуют активные диски, которые нужно удалить*.

### Сценарий 2. Невозможно удалить контейнер

При попытке удалить контейнер хранилища может появиться следующая ошибка:

*Не удалось удалить контейнер хранилища <container name>. Ошибка: "В настоящий момент контейнер находится в аренде, и в запросе не указан идентификатор аренды*".

### Сценарий 3. Невозможно удалить виртуальный жесткий диск

После удаления виртуальной машины при попытке удаления больших двоичных объектов для связанных виртуальных жестких дисков может появиться следующее сообщение:

*Не удалось удалить BLOB-объект 'path/XXXXXX-XXXXXX-os-1447379084699.vhd'. Ошибка: "В настоящий момент BLOB-объект находится в аренде, и в запросе не указан идентификатор аренды".*

## Дополнительные сведения

Виртуальные машины, созданные в классической модели развертывания и сохраненные, будут иметь состояние **Остановлено (освобождено)** на [портале Azure](https://portal.azure.com/) или на [классическом портале Azure](https://manage.windowsazure.com/).

**Классический портал Azure**:

![Состояние "Остановлено (освобождено)" виртуальных машин на классическом портале Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

**Портал Azure**:

![Состояние "Остановлено (освобождено)" виртуальных машин на портале Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

Состояние "Остановлено (освобождено)" освобождает ресурсы этого компьютера, такие как ЦП, память и сеть. Однако диски по-прежнему сохраняются, чтобы пользователь при необходимости мог быстро повторно создать виртуальную машину. Эти диски создаются на основе виртуальных жестких дисков, поддерживаемых службой хранилища Azure. Учетная запись хранения имеет эти виртуальные жесткие диски, а диски арендуют их.

## Ссылки

- [Удаление учетной записи хранения](storage-create-storage-account.md#delete-a-storage-account)
- [Приостановка заблокированной аренды хранилища BLOB-объектов в Microsoft Azure (PowerShell)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)

<!---HONumber=AcomDC_0330_2016-->