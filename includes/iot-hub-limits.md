В следующей таблице перечислены ограничения, связанные с разными уровнями служб (S1, S2, S3, F1). Сведения о стоимости каждой *единицы* на каждом уровне см. в разделе [Цены на центр IoT](https://azure.microsoft.com/pricing/details/iot-hub/).

| Ресурс | Стандартный S1 | Стандартный S2 | Стандартный S3 | Бесплатный F1 |
| -------- | ----------- | ----------- | ----------- | ------- |
| Сообщений в день | 400 000 | 6 000 000 | 300 000 000 | 8000 |
| Максимальное число единиц | 200 | 200 | 200 | 1 |

> [AZURE.NOTE] Если предполагается использование более 200 единиц с центром уровня S1, S2 или S3, обратитесь в службу технической поддержки Майкрософт.

В следующей таблице перечислены ограничения, которые применяются к ресурсам центра IoT:

| Ресурс | Ограничение |
| -------- | ----- |
| Максимальное число платных центров IoT на подписку Azure | 10 |
| Максимальное число бесплатных центров IoT на подписку Azure | 1 |
| Максимальное число удостоверений устройств,<br/> возвращаемых в одном вызове | 1000 |
| Максимальный срок хранения сообщения центра IoT | 7 дней |
| Максимальный размер сообщения, отправляемого с устройства в облако | 256 KB |
| Максимальный размер пакета, отправляемого с устройства в облако | 256 KB |
| Максимальное число сообщений в пакете, отправляемом с устройства в облако | 500 |
| Максимальный размер сообщения, отправляемого из облака на устройство | 64 КБ |
| Максимальный срок жизни для сообщений, отправляемых из облака на устройство | 2 дня |
| Максимальное число доставок для сообщений, <br/> отправляемых из облака на устройство | 100 |
| Максимальное число доставок для сообщений обратной связи <br/> в ответ на сообщение, отправляемое из облака на устройство | 100 |
| Максимальный срок жизни для сообщений обратной связи <br/> в ответ на сообщение, отправляемое из облака на устройство | 2 дня |

> [AZURE.NOTE] Если для вашей подписки Azure требуется более 10 платных центров IoT, обратитесь в службу технической поддержки Майкрософт.

Служба центра IoT регулирует запросы при превышении следующих квот:

| Регулирование | Значение концентратора |
| -------- | ------------- |
| Операции с реестром удостоверений <br/> (создание, извлечение, перечисление, обновление и удаление), импорт и экспорт отдельных элементов или пакетов <br/> | 5000 в минуту на единицу (для S3),<br/> 100 в минуту на единицу (для S1 и S2). |
| Подключение устройств | 6000 в секунду на единицу (для S3), 120 в секунду на единицу (S2), 12 в секунду на единицу (для S1). <br/>Минимальное значение — 100/с. |
| Передачи с устройства в облако | 6000 в секунду на единицу (для S3), 120 в секунду на единицу (S2), 12 в секунду на единицу (для S1). <br/>Минимальное значение — 100/с. |
| Передачи из облака на устройство | 5000 в минуту на единицу (для S3), 100 в минуту на единицу (для S1 и S2). |
| Получение из облака на устройство | 50000 в минуту на единицу (для S3), 1000 в минуту на единицу (для S1 и S2). |
| Операции отправки файлов | 5000 уведомлений о передаче файла в минуту на единицу (для S3), 100 уведомлений о передаче файла в минуту на единицу (для S1 и S2). <br/> Допускается одновременная выдача 10 000 универсальных кодов ресурса (URI) SAS для учетной записи хранения.<br/> Допускается одновременная выдача до 10 универсальных кодов ресурса (URI) SAS на устройство. |

<!---HONumber=AcomDC_0713_2016-->