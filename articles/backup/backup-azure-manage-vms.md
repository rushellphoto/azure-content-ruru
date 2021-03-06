
<properties
	pageTitle="Управление резервными копиями виртуальных машин, развернутых с помощью Resource Manager, и их мониторинг | Microsoft Azure"
	description="Узнайте, как управлять резервными копиями виртуальных машин, развернутых с помощью Resource Manager, и осуществлять их мониторинг"
	services="backup"
	documentationCenter=""
	authors="trinadhk"
	manager="shreeshd"
	editor=""/>

<tags
	ms.service="backup"
	ms.workload="storage-backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/03/2016"
	ms.author="jimpark; markgal;"/>

# Управление резервными копиями виртуальных машин Azure и их мониторинг

> [AZURE.SELECTOR]
- [Управление резервными копиями виртуальных машин Azure](backup-azure-manage-vms.md)
- [Управление резервными копиями классических виртуальных машин Azure](backup-azure-manage-vms-classic.md)

В этой статье содержатся рекомендации по управлению резервными копиями виртуальных машин и рассмотрены сведения о резервном копировании, доступные на панели мониторинга портала. Приведенные здесь рекомендации применимы только к виртуальным машинам, для которых используются хранилища служб восстановления. В этой статье не рассматриваются вопросы создания и защиты виртуальных машин. Сведения о защите виртуальных машин в Azure, развернутых с помощью Azure Resource Manager, с использованием хранилища служб восстановления см. в статье [Общие сведения о резервном копировании виртуальных машин ARM в хранилище служб восстановления](backup-azure-vms-first-look-arm.md).

## Доступ к хранилищам и защищенным виртуальным машинам

На панели мониторинга хранилища служб восстановления на портале Azure предоставляются следующие сведения:

- сведения о последнем моментальном снимке резервной копии, который также является последней точкой восстановления; <br>
- политика архивации; <br>
- общий размер всех моментальных снимков резервной копии; <br>
- количество виртуальных машин, защищенных с помощью хранилища. <br>

Чтобы приступить к выполнению задач по управлению резервной копией виртуальной машины, в большинстве случаев необходимо открыть хранилище на панели мониторинга. Так как хранилища можно использовать для защиты нескольких элементов (или виртуальных машин), для просмотра сведений о конкретной виртуальной машине необходимо открыть панель мониторинга для этого элемента хранилища. Ниже приведены сведения о том, как открыть *панель мониторинга хранилища* и перейти к *панели мониторинга элемента хранилища*, а также советы по добавлению хранилища и элемента хранилища на панель мониторинга Azure с помощью команды "Закрепить на панели мониторинга". В результате выполнения этой команды создается ярлык для хранилища или элемента. Затем ярлык можно использовать для выполнения общих команд.

>[AZURE.TIP] Если открыто несколько панелей мониторинга или колонок, для перехода в панели мониторинга Azure можно использовать темно-синий ползунок в нижней части окна.

![Полное представление с ползунком](./media/backup-azure-manage-vms/bottom-slider.png)

### Открытие хранилища службы восстановления на панели мониторинга

1. Войдите на [портал Azure](https://portal.azure.com/).

2. В главном меню щелкните **Обзор**, а затем в списке ресурсов введите **Службы восстановления**. Как только вы начнете вводить, список отфильтруется соответствующим образом. Щелкните **Хранилище служб восстановления**.

    ![Создание хранилища служб восстановления — шаг 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    После этого отобразится список хранилищ служб восстановления.

    ![Список хранилищ служб восстановления](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

    >[AZURE.TIP] Если закрепить хранилище на панели мониторинга Azure, доступ к нему можно получить непосредственно при открытии портала Azure. Чтобы закрепить хранилище на панели мониторинга, в списке хранилищ щелкните правой кнопкой мыши необходимое хранилище и выберите команду **Закрепить на панели мониторинга**.

3. В списке выберите хранилище, чтобы открыть соответствующую панель мониторинга. Когда вы выберете хранилище, для него откроется панель мониторинга и колонка **Параметры**. На следующем рисунке панель мониторинга хранилища **Contoso-vault** выделена красным цветом.

    ![Открытые панели мониторинга хранилища и колонки "Параметры"](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### Открытие панели мониторинга элемента хранилища

При выполнении предыдущей процедуры вы открыли панель мониторинга хранилища. Чтобы открыть панель мониторинга элемента хранилища, сделайте следующее:

1. На панели мониторинга хранилища на плитке **Архивируемые элементы** щелкните **Виртуальные машины Azure**.

    ![Открытие элемента "Архивируемые элементы"](./media/backup-azure-manage-vms/contoso-vault.png)

    В колонке **Архивируемые элементы** отображаются последние выполненные задания резервного копирования для каждого элемента. В этом примере приведена одна виртуальная машина (demovm-markgal), защищенная с помощью этого хранилища.

    ![Элемент "Архивируемые элементы"](./media/backup-azure-manage-vms/backup-items-blade.png)

    >[AZURE.TIP] Для более удобного доступа элемент хранилища можно закрепить на панели мониторинга Azure. Для этого в списке элементов хранилища щелкните правой кнопкой мыши необходимый элемент и выберите команду **Закрепить на панели мониторинга**.

2. В колонке **Архивируемые элементы** щелкните элемент, для которого необходимо открыть панель мониторинга.

    ![Элемент "Архивируемые элементы"](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Откроется панель мониторинга элемента хранилища и колонка **Параметры**.

    ![Панель мониторинга архивируемого элемента с колонкой "Параметры"](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    На панели мониторинга элемента хранилища можно выполнять следующие основные задачи по управлению:

    - изменять и создавать политики архивации; <br>
	- просматривать точки восстановления и их состояние согласованности; <br>
	- выполнять резервное копирование виртуальной машины по запросу; <br>
	- отключать защиту виртуальных машин; <br>
	- возобновлять защиту виртуальных машин; <br>
	- удалять данные резервного копирования (или точки восстановления); <br>
	- [восстанавливать резервные копии (или точки восстановления)](./backup-azure-arm-restore-vms.md#restore-a-recovery-point). <br>

Приведенные ниже процедуры выполняются на панели мониторинга элемента хранилища.

## Изменение или создание политики архивации

1. На [панели мониторинга элемента хранилища](backup-azure-manage-vms.md#open-a-vault-item-dashboard) щелкните **Все параметры**, чтобы открыть колонку **Параметры**.

    ![Колонка "Политика архивации"](./media/backup-azure-manage-vms/all-settings-button.png)

2. В колонке **Параметры** щелкните **Политика архивации**, чтобы открыть соответствующую колонку.

    В ней отображаются сведения о частоте резервного копирования и диапазоне хранения.

    ![Колонка "Политика архивации"](./media/backup-azure-manage-vms/backup-policy-blade.png)

3. Используйте меню **Выбор политики архивации**, чтобы выполнить следующие действия.
    - Чтобы изменить политику, выберите необходимую политику и нажмите кнопку **Сохранить**. Новая политика будет немедленно применена к хранилищу. <br>
    - Чтобы создать политику, выберите **Создать**.

    ![Резервное копирование виртуальной машины](./media/backup-azure-manage-vms/backup-policy-create-new.png)

    Указания по созданию политики архивации см. в разделе [Определение политики архивации](backup-azure-manage-vms.md#defining-a-backup-policy).


## Резервное копирование виртуальной машины по запросу
Резервное копирование виртуальной машины по запросу можно выполнять, когда для нее настроена защита. Если процесс начального резервного копирования находится в состоянии ожидания для виртуальной машины, то процесс резервного копирования по запросу создаст полную копию виртуальной машины в хранилище служб восстановления. После завершения процесса начального резервного копирования при выполнении резервного копирования по запросу будут отправляться только изменения, произошедшие со времени создания предыдущего моментального снимка, в хранилище служб восстановления (т. е. оно всегда будет добавочным).

>[AZURE.NOTE] Для диапазона хранения резервных копий по запросу используется значение ежедневного сохранения, указанное в политике. Если значение ежедневного сохранения не выбрано, используется значение еженедельного сохранения.

Чтобы выполнить резервное копирование виртуальной машины по запросу, сделайте следующее:

1. На [панели мониторинга элемента хранилища](backup-azure-manage-vms.md#open-a-vault-item-dashboard) щелкните **Создать резервную копию**.

    ![Кнопка "Создать резервную копию"](./media/backup-azure-manage-vms/backup-now-button.png)

    После этого откроется окно с запросом на подтверждение выполнения этой операции. Чтобы приступить к выполнению, нажмите кнопку **Да**.

    ![Кнопка "Создать резервную копию"](./media/backup-azure-manage-vms/backup-now-check.png)

    При выполнении задания резервного копирования создается новая точка восстановления. Диапазон хранения созданной точки восстановления будет таким же, как и диапазон времени, указанный в политике, связанной с виртуальной машиной. Чтобы отследить ход выполнения задания, на панели мониторинга хранилища щелкните элемент **Задания архивации**.


## Отключение защиты виртуальных машин
При остановке защиты виртуальной машины появится запрос на сохранение точек восстановления. Чтобы остановить защиту виртуальной машины, можно использовать следующие варианты: остановить все будущие задания резервного копирования и удалить точки восстановления или остановить все будущие задания резервного копирования, но сохранить точки восстановления. Сохранение точек восстановления в хранилище сопровождается определенными затратами. Однако позже с помощью этих точек при необходимости можно восстановить виртуальную машину. Для получения дополнительных сведений о ценах для таких виртуальных машин щелкните [здесь](https://azure.microsoft.com/pricing/details/backup/). Если удалить все точки восстановления, восстановить виртуальную машину будет невозможно.

Чтобы остановить защиту виртуальной машины, сделайте следующее:

1. На [панели мониторинга элемента хранилища](backup-azure-manage-vms.md#open-a-vault-item-dashboard) щелкните **Прекратить резервное копирование**.

    ![Кнопка "Прекратить резервное копирование"](./media/backup-azure-manage-vms/stop-backup-button.png)

    Откроется колонка "Прекратить резервное копирование".

    ![Колонка "Прекратить резервное копирование"](./media/backup-azure-manage-vms/stop-backup-blade.png)

2. В колонке **Прекратить резервное копирование** выберите действие, которое необходимо выполнить с данными резервного копирования — сохранить или удалить. В поле сведений содержится информация о вариантах выбора и возможных последствиях.

    ![Остановить защиту](./media/backup-azure-manage-vms/retain-or-delete-option.png)

3. Если вы решили сохранить данные резервного копирования, перейдите к шагу 4. При удалении данных резервного копирования подтвердите остановку заданий резервного копирования и удалите точки восстановления (введите имя элемента).

    ![Остановить проверку](./media/backup-azure-manage-vms/item-verification-box.png)

    Если вы не помните имя элемента, чтобы просмотреть его, наведите указатель мыши на восклицательный знак. Имя элемента также отображается в верхней части колонки **Прекратить резервное копирование**.

4. При необходимости можно указать **причину** или добавить **комментарий**.

5. Чтобы остановить задание резервного копирования для текущего элемента, нажмите кнопку ![Кнопка "Прекратить резервное копирование"](./media/backup-azure-manage-vms/stop-backup-button-blue.png).

    После остановки отобразится сообщение с соответствующим уведомлением.

    ![Подтверждение остановки защиты](./media/backup-azure-manage-vms/stop-message.png)


## Возобновление защиты виртуальной машины
Если при остановке защиты **данные резервного копирования были сохранены**, их можно использовать для возобновления защиты виртуальной машины. Если **нет** — защиту виртуальной машины восстановить невозможно.

Чтобы восстановить защиту виртуальной машины, сделайте следующее:

1. На [панели мониторинга элемента хранилища](backup-azure-manage-vms.md#open-a-vault-item-dashboard) щелкните **Возобновить резервное копирование**.

    ![Возобновить защиту](./media/backup-azure-manage-vms/resume-backup-button.png)

    После этого откроется колонка "Политика архивации".

    >[AZURE.NOTE] При повторной защите виртуальной машины можно выбрать другую политику, отличную от политики, с помощью которой виртуальная машина была защищена изначально.

2. Чтобы назначить политику для виртуальной машины, выполните действия, описанные в разделе [Изменение или создание политики архивации](backup-azure-manage-vms.md#change-policies-or-create-a-new-backup-policy).

    После применения политики к виртуальной машине откроется следующее сообщение:

    ![Защищенные виртуальные машины](./media/backup-azure-manage-vms/success-message.png)

## Удаление данных резервных копий
Данные резервного копирования, связанные с виртуальной машиной, можно удалить во время **остановки резервного копирования** или в любое время после остановки. После остановки задания резервного копирования для виртуальной машины следует несколько дней или недель тщательно обдумать, следует ли удалять точки восстановления. В отличие от восстановления точек восстановления при удалении данных резервного копирования нельзя выбрать определенные точки для удаления. При удалении данных резервного копирования удаляются все точки восстановления, связанные с этим элементом.

В следующей процедуре подразумевается, что задание резервного копирования для виртуальной машины остановлено или отключено. Только после остановки этого задания на панели мониторинга элемента хранилища станут доступными параметры **Возобновить резервное копирование** и **Delete backup** (Удалить резервную копию).

![Кнопки возобновления и удаления](./media/backup-azure-manage-vms/resume-delete-buttons.png)

Чтобы удалить данные резервного копирования на виртуальной машине с *отключенным заданием резервного копирования*, сделайте следующее:

1. На [панели мониторинга элемента хранилища](backup-azure-manage-vms.md#open-a-vault-item-dashboard) щелкните **Delete backup** (Удалить резервную копию).

    ![Тип виртуальной машины](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    Откроется колонка **Удаление данных архивации**.

    ![Тип виртуальной машины](./media/backup-azure-manage-vms/delete-backup-blade.png)

2. Чтобы подтвердить удаление точек восстановления, необходимо ввести имя элемента.

    ![Остановить проверку](./media/backup-azure-manage-vms/item-verification-box.png)

    Если вы не помните имя элемента, чтобы просмотреть его, наведите указатель мыши на восклицательный знак. Имя элемента также отображается в верхней части колонки **Удаление данных архивации**.

3. При необходимости можно указать **причину** или добавить **комментарий**.

4. Чтобы удалить данные резервных копий для текущего элемента, нажмите кнопку ![Кнопка "Прекратить резервное копирование"](./media/backup-azure-manage-vms/delete-button.png).

    После удаления отобразится сообщение с соответствующим уведомлением.

## Аудит операций
Сведения об операциях управления, выполненных в хранилище служб восстановления, можно просмотреть в журналах операций и событий. Журналы операций позволяет проводить эффективный анализ и аудит операций резервного копирования. С помощью параметра "Журналы аудита" можно просматривать журналы всех операций, выполненных *в рамках подписки*. Дополнительные сведения о журналах событий, операций и аудита см. в статье [Просмотр журналов событий и аудита](../azure-portal/insights-debugging-with-events.md). Используйте параметр **Журналы аудита**, чтобы просматривать журналы событий и операций для конкретного хранилища служб восстановления или элемента хранилища.

В журналы аудита записываются следующие операции:

- Зарегистрировать
- отмена регистрации;
- настройка защиты;
- резервное копирование (как запланированное, так и запущенное по запросу);
- Восстановление
- остановка защиты;
- удаление резервных копий;
- добавление политики;
- удаление политики;
- изменение политики;
- Отмена задания

Чтобы просмотреть журналы событий для хранилища служб восстановления, сделайте следующее:

1. На [панели мониторинга хранилища](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard) найдите и щелкните элемент **Журналы аудита**, чтобы открыть колонку **События**.

    ![Журналы аудита](./media/backup-azure-manage-vms/audit-logs.png)

    После этого откроется колонка со сведениями об операциях только для текущего хранилища. В колонке отображаются критические события, события ошибок, предупреждения и информационные события за последнюю неделю. Значение интервала времени соответствует значению по умолчанию в параметрах **фильтрации**. В колонке **События** также отображается линейчатая диаграмма, которая позволяет отслеживать время возникновения событий. Если отображать линейчатую диаграмму не требуется, в меню **События** установите переключатель **Показать диаграмму** в положение "Выкл.".

    ![Фильтр "Журналы аудита"](./media/backup-azure-manage-vms/audit-logs-filter.png)

2. Чтобы открыть колонку с дополнительными сведениями об определенной операции, щелкните необходимую операцию в списке операций. В этой колонке содержатся детальные сведения об операции и список событий, произошедших в течение определенного интервала времени.

    ![Сведения об операции](./media/backup-azure-manage-vms/audit-logs-details-window.png)

3. Чтобы открыть колонку с подробными сведениями об определенном событии, в списке событий щелкните необходимую операцию.

    ![Сведения о событии](./media/backup-azure-manage-vms/audit-logs-details-window-deep.png)

    Информация о событиях зависит от количества полученных сведений. В оставшейся части этого раздела объясняются способы редактирования или изменения доступных сведений.

4. Чтобы изменить список доступных фильтров, в меню **События** выберите пункт **Фильтр**. После этого откроется соответствующая колонка.

    ![Открытие колонки "Фильтр"](./media/backup-azure-manage-vms/audi-logs-filter-button.png)

5. В колонке **Фильтр** можно настроить значения фильтров **Уровень**, **Интервал времени** и **Вызывающая сторона**. Другие фильтры недоступны, так как они используются для предоставления текущих сведений для хранилища служб восстановления.

    ![Сведения о запросе журнала аудита](./media/backup-azure-manage-vms/filter-blade.png)

    Вы можете указать следующие **уровни** события: "Критическое", "Ошибка", "Предупреждение" и "Информационное". Вы можете выбрать любую комбинацию типов событий. Необходимо выбрать по крайней мере один уровень. Кроме того, уровни событий можно включить или отключить. В фильтре **Интервал времени** можно указать время, в течение которого будут записываться события, а также время начала и окончания записи.

6. Чтобы отправить запрос к журналу операций, выбрав критерии фильтра, нажмите кнопку **Обновить**. Результаты отобразятся в колонке **События**.

    ![Сведения об операции](./media/backup-azure-manage-vms/edited-list-of-events.png)


## Оповещения
Вы можете получать настраиваемые оповещения о заданиях на портале. Для этого в PowerShell нужно определить правила оповещений для событий журналов операций. Мы рекомендуем использовать PowerShell версии *1.3.0 или более поздней*.

Чтобы настроить пользовательское оповещение об ошибках резервного копирования, используйте команду такого типа:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/RecoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/RecoveryServicesVault/trinadhVault -Actions $actionEmail
```

**ResourceId**. Значение этого параметра можно получить в журналах аудита. Он указан в столбце "Ресурс".

**OperationName**. Этот параметр будет иметь формат Microsoft.RecoveryServices/recoveryServicesVault/<EventName>, где EventName соответствует одному из следующих значений: Register, Unregister, ConfigureProtection, Backup, Restore, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy.

**Status**. Поддерживаемые значения: Started, Succeeded и Failed.

**ResourceGroup**. Группа ресурсов, к которой принадлежит ресурс. В созданные журналы можно добавить столбец "Группа ресурсов". Группа ресурсов — это один из доступных типов сведений о событиях.

**Name**. Имя правила оповещений.

**CustomEmail**. Это адрес электронной почты для отправки оповещений.

**SendToServiceOwners**. Этот параметр отправляет оповещения всем администраторам и соадминистраторам подписки. Кроме того, его можно использовать в командлете **New-AzureRmAlertRuleEmail**.

### Связанные с оповещениями ограничения
К оповещениям на основе событий применяются следующие ограничения:

1. Оповещения активируются на всех виртуальных машинах в хранилище служб восстановления. Оповещения нельзя настроить для определенного набора виртуальных машин в хранилище служб восстановления.
2. Эта функция предоставляется в предварительной версии. [Подробнее](../azure-portal/insights-powershell-samples.md#create-alert-rules)
3. Оповещения поступают от отправителя alerts-noreply@mail.windowsazure.com. В настоящее время невозможно изменить отправителя электронной почты.

[AZURE.INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## Дальнейшие действия

Сведения о восстановлении виртуальных машин из точки восстановления см. в статье [Восстановление виртуальных машин в Azure](backup-azure-restore-vms.md). Сведения о защите виртуальных машин см. в статье [Общие сведения о резервном копировании виртуальных машин ARM в хранилище служб восстановления](backup-azure-vms-first-look-arm.md).

<!---HONumber=AcomDC_0608_2016-->