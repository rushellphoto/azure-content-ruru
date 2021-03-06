<properties 
   pageTitle="Уведомление пользователей о данных, полученных с датчиков или из других систем | Microsoft Azure"
   description="Описывает использование концентраторов событий для уведомления пользователей о данных датчиков."
   services="event-hubs"
   documentationCenter="na"
   authors="spyrossak"
   manager="timlt"
   editor="" />
<tags 
   ms.service="event-hubs"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/31/2016"
   ms.author="spyros;sethm" />

# Уведомление пользователей о данных, полученных с датчиков или из других систем

Предположим, у вас есть приложение, которое отслеживает данные в режиме реального времени или регулярно формирует отчеты. Открыв веб-сайт, где в режиме реального времени отображаются графики и отчеты, вы можете увидеть сообщения, требующие каких-либо действий. Как настроить оповещения о таких ситуациях и избавить себя от необходимости помнить о регулярной проверке веб-сайта? Представьте, что у вас есть теплица с лампами и вы хотите мгновенно узнавать об отключениях электричества. Один из способов решения этой задачи — установить в теплице датчик освещенности и настроить отправку уведомлений об отключении электричества по электронной почте.

![][1]

Другой сценарий. Допустим, вы содержите гостиницу для животных и хотите получать оповещения о недостаточных материальных запасах. Например, вы можете настроить отправку текстовых сообщений из системы ERP в случае, если объем собачьей еды на складе достигнет критически низкого уровня.

![][2]

Проблема в том, как получать критически важную информацию при наступлении определенных условий не в процессе проверки статических отчетов. Если для получения данных с устройств или корпоративных приложений, таких как [Dynamics AX][], используются [концентратор событий Azure][] или [центр IoT Azure][], вы можете использовать различные способы их обработки. Вы можете просматривать данные на веб-сайте, анализировать их, сохранять и использовать для активации определенных команд. Для этого вы можете использовать такие мощные средства как, например, [веб-сайты Azure][], [SQL Azure][], [HDInsight][], [Cortana Intelligence Suite][], [IoT Suite][], [приложения логики][] или [центры уведомлений Azure][]. Однако в некоторых случаях требуется только отправка данных с минимальной дополнительной нагрузкой на систему. Чтобы показать вам, как это сделать, используя лишь небольшой фрагмент кода, мы возьмем новый образец — [AppToNotifyUsers][]. В число доступных параметров входя электронная почта (SMTP), SMS и телефон.

## Структура приложений

Приложение создается на языке C#, а файл Readme в образце содержит все сведения, необходимые для его изменения, создания и публикации. Общие сведения о том, как работает приложение, см. в следующих разделах.

Для начала предположим, что в концентратор событий Azure или центр IoT передаются критические события. Подойдет любой концентратор, если у вас есть к нему доступ и вам известная строка подключения.

Если у вас еще нет концентратора событий или центра IoT, вы легко можете собрать испытательный стенд с платами Arduino Shield и Raspberry Pi, выполнив инструкции в проекте [Connect The Dots](https://github.com/Azure/connectthedots). Датчик освещения с платы Arduino Shield через Pi отправляет данные об уровне освещенности в [концентратор событий Azure][] \(**ehdevices**), а служба [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) передает оповещения во второй концентратор событий (**ehalerts**), если полученный уровень освещенности упадет ниже определенного уровня.

Запущенная служба **AppToNotify** считывает файл конфигурации (App.config), извлекая из него URL-адрес и учетные данные для концентратора событий, получившего оповещения. Затем она расширяет работу, включая в нее постоянный мониторинг концентратора событий на предмет любых поступающих в него сообщений, если у вас есть доступ к URL-адресу этого концентратора событий или центра IoT и действительные учетные данные, код считывателя этого концентратора будет постоянно считывать все поступающие данные. Во время запуска приложение считывает URL-адрес и учетные данные службы обмена сообщениями (электронная почта, SMS, телефон), которую вы хотите использовать, имя и адрес отправителя и список получателей.

Обнаружив сообщение, монитор концентратора событий запускает процесс, отправляющий сообщение способом, указанным в файле конфигурации. Обратите внимание на то, что монитор отправляет все обнаруженные сообщения. Если монитор настроен на концентратор событий, получающий десять сообщений в секунду, отправитель будет передавать десять сообщений в секунду — десять электронных писем в секунду, десять SMS в секунду или десять телефонных звонков в секунду. В связи с этим убедитесь, что мониторингу подвергается концентратор событий, который получает только те оповещения, которые необходимо отправлять, а не все необработанные данные с ваших датчиков или приложений.

## Применимость

Код, представленный в этом примере, демонстрирует только мониторинг концентраторов событий и порядок вызова внешних служб обмена сообщениями в случае, если вы хотите реализовать эту функцию в своем приложении. Обратите внимание на то, что это решение представляет собой пример, который создается с нуля и предназначен только для разработчиков. Оно не соответствует таким корпоративным требованиям, как избыточность, отказоустойчивость, перезапуск после отказа и т. д. В качестве более комплексных производственных решений можно привести следующие.

- Использование соединителей и push-уведомлений с помощью [приложений логики Azure](../app-service-logic/app-service-logic-connectors-list.md).
- Использование [концентраторов уведомлений Azure](https://msdn.microsoft.com/library/azure/jj927170.aspx), как описано в блоге [Передача push-уведомлений на миллионы мобильных устройств с помощью центров уведомлений Azure](http://weblogs.asp.net/scottgu/broadcast-push-notifications-to-millions-of-mobile-devices-using-windows-azure-notification-hubs). 

## Дальнейшие действия

Оптимальный вариант — создать простую службу уведомлений, которая будет отправлять адресам электронные письма или текстовые сообщения либо звонить им по телефону и ретранслировать данные, полученные концентратором событий или центром IoT. Инструкции по развертыванию решения для уведомления пользователей на основе данных, полученных из этих концентраторов, см. в разделе [AppToNotifyUsers][].

Дополнительные сведения об этих концентраторах см. в следующих статьях:

- [Концентраторы событий Azure]
- [Центр Azure IoT]
- Начните работу с [учебника по концентраторам событий].
- Полный [пример приложения, использующего концентраторы событий].
- [Решение для обмена сообщениями в очереди] при помощи очередей служебной шины.

Инструкции по развертыванию решения для уведомления пользователей на основе данных, полученных из этих концентраторов, см. здесь:

- [AppToNotifyUsers][]

[учебника по концентраторам событий]: event-hubs-csharp-ephcs-getstarted.md
[Центр Azure IoT]: https://azure.microsoft.com/services/iot-hub/
[центр IoT Azure]: https://azure.microsoft.com/services/iot-hub/
[Концентраторы событий Azure]: https://azure.microsoft.com/services/event-hubs/
[концентратор событий Azure]: https://azure.microsoft.com/services/event-hubs/
[пример приложения, использующего концентраторы событий]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Решение для обмена сообщениями в очереди]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
[AppToNotifyUsers]: https://github.com/Azure-Samples/event-hubs-dotnet-user-notifications
[Dynamics AX]: http://www.microsoft.com/dynamics/erp-ax-overview.aspx
[веб-сайты Azure]: https://azure.microsoft.com/services/app-service/web/
[SQL Azure]: https://azure.microsoft.com/services/sql-database/
[HDInsight]: https://azure.microsoft.com/services/hdinsight/
[Cortana Intelligence Suite]: http://www.microsoft.com/server-cloud/cortana-analytics-suite/Overview.aspx?WT.srch=1&WT.mc_ID=SEM_lLFwOJm3&bknode=BlueKai
[IoT Suite]: https://azure.microsoft.com/solutions/iot-suite/
[приложения логики]: https://azure.microsoft.com/services/app-service/logic/
[центры уведомлений Azure]: https://azure.microsoft.com/services/notification-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/services/stream-analytics/
 
[1]: ./media/event-hubs-sensors-notify-users/event-hubs-sensor-alert.png
[2]: ./media/event-hubs-sensors-notify-users/event-hubs-erp-alert.png

<!---HONumber=AcomDC_0601_2016-->