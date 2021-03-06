Балансировщик нагрузки позволяет обеспечить высокую степень доступности для рабочих нагрузок в Azure. Балансировщик нагрузки Azure на уровне 4 (TCP, UDP) распределяет входящий трафик между работоспособными экземплярами службы в облачных службах или виртуальных машинах, определенных в наборе балансировщика нагрузки.
 
Балансировщика нагрузки можно настроить для выполнения следующих задач.

- Балансировка нагрузки входящего интернет-трафика виртуальных машин. В этом сценарии балансировщика нагрузки называют [балансировщиком нагрузки сети Интернет](../articles/load-balancer/load-balancer-internet-overview.md).
- Балансировка нагрузки трафика между виртуальными машинами в виртуальной сети, между виртуальными машинами в облачных службах или между локальными компьютерами и виртуальными машинами в распределенной виртуальной сети. В этом сценарии балансировщика нагрузки называют [внутренним балансировщиком нагрузки](../articles/load-balancer/load-balancer-internal-overview.md).
- 	Перенаправление внешнего трафика к определенному экземпляру виртуальной машины.

<!---HONumber=AcomDC_0224_2016-->