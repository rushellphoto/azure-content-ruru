<properties
   pageTitle="Приступая к работе с модулем PowerShell пакетной службы Azure | Microsoft Azure"
   description="Краткое описание командлетов Azure PowerShell, используемых для управления пакетной службой Azure."
   services="batch"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="07/28/2016"
   ms.author="danlep"/>

# Приступая к работе с командлетами Azure PowerShell
С помощью командлетов PowerShell пакетной службы Azure можно выполнять те же задачи, которые выполняются с помощью API-интерфейсов пакетной службы, портала Azure и интерфейса командной строки (CLI). Эта статья содержит краткие сведения о командлетах, используемых для управления учетными записями пакетной службы и работы с ее ресурсами, включая пулы, задания и задачи. В этой статье описаны командлеты Azure PowerShell 1.6.0.

Полный список командлетов пакетной службы и их синтаксис см. в статье [Azure Batch Cmdlets](https://msdn.microsoft.com/library/azure/mt125957.aspx) (Командлеты пакетной службы Azure).


## Предварительные требования

* **Azure PowerShell**. Инструкции по скачиванию и установке Azure PowerShell см. в статье [Установка и настройка Azure PowerShell](../powershell-install-configure.md).
   
    * Так как командлеты пакетной службы Azure входят в состав модуля Azure Resource Manager, вам потребуется выполнить командлет **Login-AzureRmAccount**, чтобы подключиться к своей подписке.
    
    * Рекомендуется часто обновлять Azure PowerShell, чтобы пользоваться всеми преимуществами службы.
    
* **Регистрация пространства имен поставщика пакетной службы (одноразовая операция)**. Чтобы начать работу с учетными записями пакетной службы, зарегистрируйте пространство имен поставщика пакетной службы. Эту операцию нужно выполнять один раз для каждой подписки. Выполните следующий командлет:

        Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch


## Управление учетными записями пакетной службы и ключами

### Создание учетной записи Пакетной службы

Командлет **New-AzureRmBatchAccount** создает новую учетную запись пакетной службы в указанной группе ресурсов. Если у вас еще нет группы ресурсов, создайте ее. Для этого выполните командлет [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx), указав один из регионов Azure, например «Центральная часть США», в параметре **Расположение**. Например:


    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"


Затем создайте новую учетную запись пакетной службы в группе ресурсов, указав имя учетной записи (элемент <*account\_name*>), а также расположение и имя группы ресурсов. Создание учетной записи пакетной службы может занять некоторое время. Например:


    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName MyBatchResourceGroup

> [AZURE.NOTE] Имя учетной записи пакетной службы должно быть уникальным для региона Azure группы ресурсов, содержать от 3 до 24 символов и состоять только из строчных букв и цифр.

### Получение ключей доступа к учетной записи
**Get-AzureRmBatchAccountKeys** выводит клавиши доступа, связанные с учетной записью пакетной службы Azure. Например, выполните следующую команду, чтобы получить первичные и вторичные ключи созданной учетной записи.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <accountname>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey


### Создание нового ключа доступа
**New-AzureRmBatchAccountKey** создает новый первичный или вторичный ключ учетной записи пакетной службы Azure. Например, чтобы создать новый первичный ключ для учетной записи Пакетной службы, введите:


    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary


> [AZURE.NOTE] Чтобы создать новый вторичный ключ, введите "Вторичный" в параметре **KeyType**. Повторно создавать первичный и вторичный ключи нужно отдельно.

### Удаление учетной записи Пакетной службы
**Remove-AzureRmBatchAccount** удаляет учетную запись пакетной службы. Например:


    Remove-AzureRmBatchAccount -AccountName <account_name>

При появлении запроса на удаление учетной записи подтвердите удаление. Удаление учетной записи может занять некоторое время.

## Создание объекта BatchAccountContext

Чтобы выполнить проверки подлинности с помощью командлетов PowerShell пакетной службы во время создания пулов, заданий, задач и других ресурсов, а также управления ими, вам необходимо сначала создать объект BatchAccountContext для хранения ключей и имени учетной записи.

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Передайте объект BatchAccountContext в командлеты, которые используют параметр **BatchContext**.

> [AZURE.NOTE] По умолчанию первичный ключ учетной записи используется для проверки подлинности, но вы можете явно выбрать ключ, который нужно использовать, изменив свойство **KeyInUse** объекта BatchAccountContext: `$context.KeyInUse = "Secondary"`.



## Создание и изменение ресурсов пакетной службы
Чтобы создать ресурсы в учетной записи пакетной службы, используйте командлеты **New-AzureBatchPool**, **New-AzureBatchJob** и **New-AzureBatchTask**. Чтобы обновить свойства ресурсов в учетной записи пакетной службы, используйте соответствующие командлеты **Get-** и **Set-**, а чтобы удалить ресурсы — командлет **Remove-**.

При использовании этих командлетов вам нужно не только передать объект BatchContext, но также создать или передать объекты, которые содержат параметры с подробными настройками ресурсов, как показано в следующем примере. Дополнительные примеры доступны в справочных материалах по каждому командлету.

### Создание пула пакетной службы

При создании или обновлении пула пакетной службы вам нужно определить конфигурацию облачной службы или виртуальной машины для операционной системы на вычислительных узлах (см. [обзор функций пакетной службы](batch-api-basics.md#pool)). По сути вы выберете, будет ли создан образ вычислительных узлов с одним из [выпусков гостевой ОС Azure](../cloud-services/cloud-services-guestos-update-matrix.md#releases) или с одним из поддерживаемых образов виртуальной машины Windows или Linux в Azure Marketplace.

При запуске командлета **New-AzureBatchPool** передайте параметры операционной системы в объекте PSCloudServiceConfiguration или PSVirtualMachineConfiguration. Например, следующий командлет создает новый пул пакетной службы с вычислительными узлами малого размера в конфигурации облачной службы. При этом образ создан с использованием последней версии операционной системы семейства 3 (Windows Server 2012). Здесь параметр **CloudServiceConfiguration** определяет переменную *$configuration* как объект PSCloudServiceConfiguration. Параметр **BatchContext** определяет ранее определенную переменную *$context* как объект BatchAccountContext.


    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(3,"*")
    
    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Целевое число вычислительных узлов в новом пуле определяется по формуле автоматического масштабирования. В этом случае формула выглядит так: **$TargetDedicated=4**. Это значит, что число вычислительных узлов в пуле не будет превышать 4.

## Запрос на получение сведений о пулах, заданиях, задачах и другой информации

Чтобы отправить запрос о сущностях, созданных в учетной записи пакетной службы, используйте командлеты **Get-AzureBatchPool**, **Get-AzureBatchJob** и **Get-AzureBatchTask**.


### Запрос данных

Например, для поиска пулов используйте**Get AzureBatchPools**. По умолчанию этот командлет опрашивает все пулы вашей учетной записи при условии, что вы уже сохранили объект BatchAccountContext в *$context*.


    Get-AzureBatchPool -BatchContext $context

### Использование фильтра OData

Чтобы найти объекты, которые вас интересуют, установите фильтр OData в параметре **Фильтр**. Например, можно найти все пулы с идентификаторами, начинающимися с myPool:


    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context


Этот способ не такой гибкий, как использование "Where-Object" в локальном конвейере. Но запрос отправляется непосредственно Пакетной службе, чтобы вся фильтрация выполнялась на стороне сервера, сохраняя пропускную способность Интернета.

### Используйте параметр Id

Альтернативой использования фильтра OData является использование параметра **Id**. Для запроса конкретного пула с идентификатором myPool сделайте следующее.


    Get-AzureBatchPool -Id "myPool" -BatchContext $context


Параметр **Id** поддерживает поиск только полного идентификатора без подстановочных знаков или фильтров в стиле OData.



### Использование параметра MaxCount

По умолчанию каждый командлет возвращает максимум 1000 объектов. Если этот предел достигнут, уточните параметры фильтра, чтобы он возвращал меньшее количество объектов, или явно задайте максимальное значение с помощью параметра **MaxCount**. Например:


    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Чтобы удалить верхнюю границу, задайте **MaxCount** значение "0" или меньше.

### Использование конвейера

Командлеты Пакетной службы могут использовать конвейер PowerShell для передачи данных между командлетами. Это имеет тот же эффект, что и указание параметра, но упрощает вывод нескольких сущностей. Например, этот командлет находит все задачи в вашей учетной записи:


    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context


## Дальнейшие действия
* Сведения о синтаксисе командлетов и их примеры см. в [справке по командлетам пакетной службы Azure](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Дополнительные сведения об уменьшении числа элементов и типах сведений, возвращаемых в ответ на запросы к пакетной службе, см. в статье [Эффективные запросы к пакетной службе Azure](batch-efficient-list-queries.md).

<!---HONumber=AcomDC_0803_2016-->