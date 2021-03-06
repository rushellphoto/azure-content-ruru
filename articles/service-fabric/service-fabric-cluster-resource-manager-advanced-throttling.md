<properties
   pageTitle="Регулирование в диспетчере кластерных ресурсов Service Fabric | Microsoft Azure"
   description="Узнайте, как настраивать регулирование в диспетчере кластерных ресурсов Service Fabric."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/20/2016"
   ms.author="masnider"/>


# Регулирование поведения диспетчера кластерных ресурсов Service Fabric
В некоторых случаях, даже если диспетчер ресурсов настроен правильно, работа кластера может быть нарушена (отказы нескольких узлов или доменов сбоя во время установки новой версии, при применении обновлений и т. д.). Resource Manager будет пытаться все исправить, но в таких случаях вы задумаетесь об поддержке, позволяющей кластеру самостоятельно стабилизировать работу (чтобы восстанавливаемые узлы возвращались в строй, сети сами восстанавливали свою работоспособность, а исправленные биты развертывались). Чтобы помочь в подобных ситуациях, Resource Manager включает в себя несколько регулировок. Обратите внимание, что это достаточно "большие пушки", которые не стоит использовать, пока вы тщательно не просчитали объем параллельных работ, которые фактически можно выполнить в кластере, и не определили, насколько часто приходится реагировать на подобные события незапланированной колоссальной перенастройки (т. н. "очень неудачные дни").

Обычно мы рекомендуем избегать очень неудачных дней иными средствами (в первую очередь, регулярно обновляя код и не перегружая расписание кластера), а не регулировать кластер, чтобы он не пытался использования ресурсы, одновременно пытаясь восстановить свою работоспособность. С другой стороны, вы можете выяснить, что существуют случаи, когда (пока вы не устраните проблему) необходимо иметь несколько регулировок, даже если это означает, что стабилизация кластера будет занимать больше времени.

##Настройка регулирования
По умолчанию включены следующие регулировки:

-	GlobalMovementThrottleThreshold — управляет общим количеством перемещений в кластере за некоторое время (определяется в секундах как GlobalMovementThrottleCountingInterval).
-	MovementPerPartitionThrottleThreshold — управляет общим количеством перемещений для любой секции службы за некоторое время (MovementPerPartitionThrottleCountingInterval, значение задается в секундах).

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Имейте в виду, что чаще всего клиенты используют эти регулировки, так как в их среде уже были ограничены ресурсы (например, ограниченная пропускная способность подключений к отдельным узлам), то есть такие операции в любом случае не выполнялись бы или выполнялись бы медленно. И клиентов устраивало, что они потенциально увеличивают время стабилизации состояния кластера и могут снизить общую надежность при использовании регулирования.

## Дальнейшие действия
- Чтобы узнать, как диспетчер кластерных ресурсов управляет нагрузкой кластера и балансирует ее, ознакомьтесь со статьей о [балансировке нагрузки](service-fabric-cluster-resource-manager-balancing.md).
- В диспетчере кластерных ресурсов много параметров для описания кластера. Чтобы узнать о них больше, ознакомьтесь с этой статьей об [описании кластера Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md).

<!---HONumber=AcomDC_0525_2016-->