<properties
   pageTitle="Защита данных в центре безопасности Azure | Microsoft Azure"
   description="В этой статье объясняется, каким образом обеспечивается управление данными и их защита в центре безопасности Azure."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="yurid"/>

# Защита данных в центре безопасности Azure
Чтобы помочь клиентам предотвращать и выявлять угрозы, а также реагировать на них, центр безопасности Azure собирает и обрабатывает данные о ресурсах Azure, в том числе сведения о конфигурации, метаданные, журналы событий, файлы аварийных дампов и многое другое. Корпорация Майкрософт берет на себя ответственность по защите конфиденциальности и безопасности этих данных. Корпорация Майкрософт следует строгим нормативным требованиям и указаниям по безопасности — от создания кода до эксплуатации служб.

В этой статье объясняется, каким образом обеспечивается управление данными и их защита в центре безопасности Azure.

## Источники данных
Центр безопасности Azure анализирует данные из следующих источников:

- Службы Azure. Из развернутых служб Azure считываются сведения о конфигурации путем взаимодействия с поставщиком ресурсов соответствующей службы.
- Сетевой трафик. Из инфраструктуры Майкрософт считывается выборка метаданных сетевого трафика, например IP-адрес и порт источника и назначения, размер пакета и сетевой протокол.
- Решения партнеров. Из интегрированных решений партнеров, например брандмауэров и решений по защите от вредоносных программ, собираются оповещения безопасности. Эти данные хранятся в хранилище центра безопасности Azure, которое в настоящее время расположено в США.
- Виртуальные машины. Центр безопасности Azure может собирать сведения о конфигурации и событиях безопасности, например события Windows, данные журналов аудита и журналов IIS, а также сообщения системного журнала и файлы аварийного дампа, из виртуальных машин с помощью агентов сбора данных. Дополнительные сведения см. в приведенном ниже разделе об управлении сбором данных.

Кроме того, в хранилище центра безопасности Azure, которое в настоящее время расположено в США, хранятся сведения об оповещениях безопасности, состоянии работоспособности системы безопасности и рекомендации. Эти сведения могут включать в себя связанные данные конфигурации и события безопасности, собранные из виртуальных машин для предоставления оповещений безопасности, состояния работоспособности системы безопасности и рекомендаций.

## Защита данных
**Разделение данных**. Данные логическим образом отделяются для каждого компонента службы. Все данные отмечаются тегами по организациям. Эти теги существуют в течение всего жизненного цикла данных и используются на каждом уровне службы. Кроме того, данные, собранные из виртуальных машин, хранятся в учетных записях хранения.

**Доступ к данным**. Чтобы предоставлять рекомендации по обеспечению безопасности и исследовать потенциальные угрозы безопасности, сотрудники корпорации Майкрософт могут получить доступ к информации, собранной или проанализированной службами Azure, включая файлы аварийного дампа. В файлы аварийного дампа и сведения о событиях создания процесса могут случайно попасть данные клиента или персональные данные из виртуальных машин. Мы соблюдаем [условия использования](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) и [заявление о конфиденциальности Microsoft Online Services](https://www.microsoft.com/privacystatement/ru-RU/OnlineServices/Default.aspx), которые гласят, что корпорация Майкрософт не будет использовать данные клиента или извлекать из них сведения для рекламных или аналогичных коммерческих целей. Мы используем данные клиента только в рамках предоставления служб Azure, а также для совместимых с этим целей. Вы сохраняете все права на данные клиента.

**Использование данных**. Корпорация Майкрософт использует шаблоны и данные анализа угроз, которые наблюдались у нескольких клиентов, чтобы улучшить возможности предотвращения и обнаружения. При этом соблюдаются обязательства по безопасности, описанные в [заявлении о конфиденциальности](https://www.microsoft.com/privacystatement/ru-RU/OnlineServices/Default.aspx).

**Расположение данных**. Учетная запись хранения указывается для каждого региона, в котором работают виртуальные машины. Таким образом, данные можно хранить в том же регионе, где расположена виртуальная машина, из которой выполняется сбор данных. Эти данные, включая файлы аварийного дампа, будут постоянно храниться в учетной записи хранения. Служба также сохраняет сведения об оповещениях безопасности, в том числе из интегрированных решений конкурентов, и состоянии работоспособности системы безопасности, а также рекомендации в хранилище центра безопасности Azure, которое в настоящее время расположено в США.

## Управление сбором данных из виртуальных машин

При включении центра безопасности Azure сбор данных включается для всех подписок пользователя. Сбор данных можно отключить в разделе "Политика безопасности" панели мониторинга центра безопасности Azure. При включении сбора данных центр безопасности Azure подготавливает к работе агент мониторинга Azure на всех имеющихся и создаваемых поддерживаемых виртуальных машинах. Расширение "Мониторинг безопасности Azure" сканирует систему на наличие конфигураций, связанных с безопасностью, и передает их в [службу трассировки событий Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Кроме того, операционная система вызывает события журнала событий во время работы виртуальной машины. Примеры таких данных — тип и версия операционной системы, журналы операционной системы (журналы событий Windows), выполняющиеся процессы, имя компьютера, IP-адреса, имя пользователя, выполнившего вход, и идентификатор клиента. Агент мониторинга Azure считывает записи журнала событий и трассировки ETW и копирует их в вашу учетную запись хранения для анализа.

Учетную запись хранения нужно указать для каждого региона, в котором выполняются виртуальные машины. В них будут храниться данные, собираемые из виртуальных машин в определенных регионах. Это позволяет хранить данные в одной географической области, чтобы обеспечить их конфиденциальность и независимость. Настроить учетные записи хранения для каждого региона можно в разделе "Политика безопасности" панели мониторинга центра безопасности Azure.

Агент мониторинга Azure также копирует файлы аварийных дампов в учетную запись хранения. Центр безопасности Azure собирает временные копии файлов аварийного дампа и анализирует их на признаки попыток компрометации и удачной компрометации. Этот анализ выполняется в том географическом регионе, к которому привязана учетная запись хранения. После завершения анализа временные копии удаляются.

Вы можете отключить сбор данных из виртуальных машин в любое время. Это приведет к удалению всех агентов мониторинга, ранее установленных центром безопасности Azure.


## Дальнейшие действия

Из этой статьи вы узнали, каким образом обеспечивается управление данными и их защита в центре безопасности Azure. Дополнительные сведения о центре безопасности Azure см. в следующих статьях:

- [Руководство по планированию использования центра безопасности Azure и работе в нем](security-center-planning-and-operations-guide.md). Узнайте, как спланировать работу с центром безопасности Azure, и получите рекомендации по переходу к его использованию.
- [Наблюдение за работоспособностью системы безопасности в Центре безопасности Azure](security-center-monitoring.md). Узнайте, как отслеживать работоспособность ресурсов Azure.
- [Управление оповещениями безопасности в центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md). Узнайте, как управлять оповещениями системы безопасности и реагировать на них.
- [Мониторинг решений партнеров с помощью центра безопасности Azure](security-center-partner-solutions.md). Узнайте, как отслеживать работоспособность партнерских решений.
- [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md). Часто задаваемые вопросы об использовании этой службы.
- [Блог по безопасности Azure](http://blogs.msdn.com/b/azuresecurity/). Записи блога, посвященные безопасности и соответствию требованиям в Azure.

<!---HONumber=AcomDC_0817_2016-->