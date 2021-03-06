<properties
	pageTitle="Подписка Microsoft Azure, ограничения служб, квоты и ограничения"
	description="В данном разделе приводится перечень наиболее распространенных ограничений, относящихся к подписке Azure и различным службам, квот и ограничений. Сюда входит информация о том, как увеличить лимиты и максимальные значения."
	services=""
	documentationCenter=""
	authors="rothja"
	manager="jeffreyg"
	editor=""
	tags="billing"
	/>

<tags
	ms.service="multiple"
	ms.workload="multiple"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/03/2016"
	ms.author="btardif"/>

# Подписка Azure, границы, квоты и ограничения службы

## Обзор

Настоящий документ описывает некоторые из наиболее известных ограничений Microsoft Azure. Учтите, что на текущий момент он охватывает не все службы Azure. Со временам эти ограничения будут расширены.

> [AZURE.NOTE] Если требуется сделать ограничение выше значения **ограничения по умолчанию**, то можно [бесплатно отправить запрос в службу поддержки](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Ограничения не могут быть увеличены выше значения **Максимальное ограничение** в таблицах ниже. Если столбца **Максимальное ограничение** нет, то указанный ресурс не имеет настраиваемого ограничения.

## Ограничения и диспетчер ресурсов Azure

Теперь несколько ресурсов можно объединить в одной группе ресурсов Azure. При использовании групп ресурсов ограничения, которые были глобальными, становятся управляемыми на региональном уровне благодаря диспетчеру ресурсов Azure. Дополнительные сведения о группах ресурсов Azure см. в статье [Общие сведения об Azure Resource Manager](resource-group-overview.md).

В следующие ограничения добавлена новая таблица, в которой отражены все различия ограничений при использовании диспетчера ресурсов Azure. Например, существует таблица **Ограничения подписки** и таблица **Ограничения подписки — диспетчер ресурсов Azure**. Если ограничение применяется в обоих случаях, оно показано только в первой таблице. Если не указано иное, ограничения глобальны во всех областях.

> [AZURE.NOTE] Важно подчеркнуть, что квоты для ресурсов в группах ресурсов Azure доступны для каждого региона по подписке, а не для каждой подписки, как квоты управления службами. В качестве примера используем квоты ядер ЦП. Если вам необходимо запросить увеличение квоты с поддержкой ядер, следует решить, сколько ядер будут использоваться и в каких регионах, а затем отправить запрос на квоты ядер группы ресурсов Azure core с нужными регионами и количеством. Поэтому, если необходимо использовать 30 ядер в Западной Европе для запуска приложения, следует запросить 30 ядер в Западной Европе. Но других регионах квота ядер не увеличится не будет — только в Западной Европе будет 30 ядер.
<!-- -->
В результате может быть полезно проанализировать, какие квоты группы ресурсов Azure потребуется для вашей рабочей нагрузки в каждом регионе, и запросить эти ресурсы в регионах, где выполняется развертывание. Подробные сведения об определении текущих квот для конкретных регионов см. в разделе [Устранение неполадок развертывания](./resource-manager-common-deployment-errors.md).


## Ограничения определенных служб

- [Active Directory](#active-directory-limits)
- [Управление API](#api-management-limits)
- [Служба приложений](#app-service-limits)
- [Application Insights](#application-insights-limits)
- [Автоматизация](#automation-limits)
- [кэш Azure Redis](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Архивация](#backup-limits)
- [Пакетная служба](#batch-limits)
- [Службы BizTalk](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Облачные службы](#cloud-services-limits)
- [Фабрика данных](#data-factory-limits)
- [Аналитика озера данных](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Концентраторы событий](#event-hubs-limits)
- [Центр IoT](#iot-hub-limits)
- [хранилище ключей;](#key-vault-limits)
- [Службы мультимедиа](#media-services-limits)
- [Mobile Engagement;](#mobile-engagement-limits)
- [Мобильные службы](#mobile-services-limits)
- [Multi-Factor Authentication](#multi-factor-authentication)
- [Сеть](#networking-limits)
- [Служба концентратора уведомлений](#notification-hub-service-limits)
- [Operational Insights;](#operational-insights-limits)
- [Группа ресурсов](#resource-group-limits)
- [Планировщик](#scheduler-limits)
- [Поиск](#search-limits)
- [Служебная шина](#service-bus-limits)
- [Site Recovery](#site-recovery-limits)
- [База данных SQL](#sql-database-limits)
- [Хранилище](#storage-limits)
- [StorSimple System](#storsimple-system-limits)
- [Анализ потока](#stream-analytics-limits)
- [Подписка](#subscription-limits)
- [Диспетчер трафика](#traffic-manager-limits)
- [Виртуальные машины](#virtual-machines-limits)
- [Наборы для масштабирования виртуальных машин](#virtual-machine-scale-sets-limits)


### Ограничения подписки
#### Ограничения подписки
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### Ограничения подписки — диспетчер ресурсов Azure

При использовании диспетчера ресурсов Azure и групп ресурсов Azure применяются следующие ограничения. Ограничения, которые не были изменены после выпуска диспетчера ресурсов Azure, ниже не перечислены. Их можно просмотреть в предыдущей таблице.

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### Ограничения группы ресурсов

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### Ограничения виртуальных машин
#### Ограничения виртуальной машины
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### Ограничения виртуальных машин — диспетчер ресурсов Azure

При использовании диспетчера ресурсов Azure и групп ресурсов Azure применяются следующие ограничения. Ограничения, которые не были изменены после выпуска диспетчера ресурсов Azure, ниже не перечислены. Их можно просмотреть в предыдущей таблице.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### Ограничения для масштабируемых наборов виртуальных машин

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### Ограничения сети

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### Ограничения сети
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### Ограничения диспетчера трафика

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### Ограничения DNS

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### Ограничения хранилища

Дополнительные сведения об ограничениях учетных записей хранения см. в статье [Целевые показатели по производительности и масштабируемости для хранилища Azure](../articles/storage/storage-scalability-targets.md).

#### Ограничения службы хранения

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### Ограничения для дисков виртуальной машины

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Дополнительные сведения см. в статье [Размеры виртуальных машин](../articles/virtual-machines/virtual-machines-linux-sizes.md).

**Учетные записи хранения Standard**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Учетные записи хранения Premium**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### Ограничения поставщика ресурсов хранилища

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### Ограничения облачных служб

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### Ограничения службы приложений
Следующие ограничения службы приложения включают ограничения для веб-приложений, мобильных приложений, приложений API и приложений логики.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### Ограничения планировщика

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### Ограничения пакета

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###Ограничения служб BizTalk
В следующей таблице показаны ограничения для служб BizTalk Azure.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### Ограничения DocumentDB

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Квоты, перечисленные со звездочкой (*), [могут быть изменены при обращении в службу поддержки Azure](./documentdb/documentdb-increase-limits.md).

### Ограничения привлечения пользователей мобильных устройств

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### Ограничения поиска

Ценовые категории определяют емкость и ограничения службы поиска. Существуют следующие категории:

- Категория *Бесплатный*: мультитенантная служба, используемая совместно с другими подписчиками Azure и предназначенная для оценки и разработки небольших проектов.
- Категория *Базовый* предоставляет выделенные вычислительные ресурсы для небольших рабочих нагрузок в рабочей среде, поддерживая до 3 реплик для обработки запросов с высоким уровнем доступности.
- Категория *Стандартный (S1, S2, S3, S3 с высокой плотностью)* предназначена для больших рабочих нагрузок в рабочей среде. Существует несколько уровней в рамках уровня "Стандартный", поэтому для конкретных сценариев можно выбирать конфигурацию ресурсов.

**Ограничения на одну подписку**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Ограничения на одну службу поиска**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Подробные сведения о других ограничениях, включая размер документов, количество запросов в секунду, ключи, запросы и ответы, см. в статье [Ограничения поиска Azure](search/search-limits-quotas-capacity.md).

### Ограничения служб мультимедиа

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### Ограничения CDN

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### Ограничения мобильных служб

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### Ограничения служб концентратора уведомлений

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### Ограничения концентраторов событий

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### Ограничения служебной шины

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### Ограничения центра IoT

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### Ограничения фабрики данных

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### Ограничения аналитики озера данных
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### Ограничения Stream Analytics
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### Ограничения Active Directory

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### Ограничения Azure RemoteApp

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### Ограничения системы StorSimple

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### Ограничения Operational Insights

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### Ограничения резервного копирования

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### Ограничения Site Recovery

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### Ограничения Application Insights

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### Ограничения управления API

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### Ограничения кэша Redis для Azure

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### Ограничения хранилища ключей

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### Multi-Factor Authentication
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### Ограничения автоматизации
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### Ограничения базы данных SQL

Ограничения базы данных SQL см. в разделе [Ограничения ресурсов базы данных SQL](sql-database/sql-database-resource-limits.md).

## См. также

[Основные сведения о лимитах Azure и их увеличении](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Размеры виртуальных машин и облачных служб для Azure](virtual-machines/virtual-machines-linux-sizes.md)

[Размеры для облачных служб](cloud-services/cloud-services-sizes-specs.md)

<!----HONumber=AcomDC_0803_2016-->
