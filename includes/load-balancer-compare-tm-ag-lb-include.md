## Различия балансировки нагрузки

Есть различные варианты для распределения сетевого трафика с помощью Microsoft Azure. Эти варианты работают по-разному, имеют разные наборы функций и поддерживают разные сценарии. Их можно использовать по отдельности или вместе.

- Azure Load Balancer работает на транспортном уровне (уровень 4 в эталонном стеке сети OSI). Она обеспечивает распределение трафика на уровне сети по экземплярам приложения, работающего в одном центре обработки данных Azure.

- Шлюз приложений работает на уровне приложения (уровень 7 в эталонном стеке OSI). Он выступает в качестве службы обратного прокси-сервера, завершая подключение клиента и перенаправляя запросы к конечным точкам серверной части.

- 	Диспетчер трафика работает на уровне DNS. Он использует ответы DNS для перенаправления трафика конечных пользователей к глобально распределенным конечным точкам. Затем клиенты подключаются к этим конечным точкам напрямую. В следующей таблице перечислены возможности, предоставляемые каждой службой:

| служба | Подсистема балансировщика нагрузки Azure | Шлюз приложений | Диспетчер трафика |
|---|---|---|---|
|Технология| Транспортный уровень (уровень 4) | Уровень приложения (уровень 7) | Уровень DNS |
| Поддерживаемые протоколы приложений |	Любой | HTTP и HTTPS | 	Любой (конечная точка HTTP или HTTPS обязательна для мониторинга конечных точек) |
| Конечные точки | Экземпляры роли ВМ и облачных служб Azure | Любой внутренний IP-адрес Azure или общедоступный IP-адрес в Интернете | Виртуальные машины Azure, облачные службы, веб-приложения Azure и внешние конечные точки |
| Поддержка виртуальной сети | Может использоваться для приложений с выходом в Интернет и внутренних приложений (в виртуальной сети) | Может использоваться для приложений с выходом в Интернет и внутренних приложений (в виртуальной сети) |	Поддерживает только приложения с выходом в Интернет |
Мониторинг конечных точек | Поддерживается посредством проб | Поддерживается посредством проб | Поддерживается через метод GET в HTTP или HTTPS | 
<BR> Балансировщик нагрузки Azure и шлюз приложений направляют трафик к конечным точкам, но их сценарии использования при обработке трафика отличаются. Таблица ниже помогает понять различие между этими подсистемами балансировки нагрузки:


| Тип | Подсистема балансировщика нагрузки Azure | Шлюз приложений |
|---|---|---|
| Протоколы | UDP и TCP | HTTP и HTTPS |
| Резервирование IP-адресов | Поддерживаются | Не поддерживается | 
| Режим балансировки нагрузки | 5 кортежей (исходный IP-адрес, исходный порт, IP-адрес назначения, порт назначения, тип протокола) | CookieBasedAffinity = false, rules = basic (циклический перебор) | 
| Режим балансировки нагрузки (исходный IP-адрес или прикрепленные сеансы) | 2 кортежа (исходный IP-адрес и IP-адрес назначения), 3 кортежа (исходный IP-адрес, конечный IP-адрес и порт). Можно масштабировать вверх или вниз в зависимости от числа виртуальных машин | CookieBasedAffinity = true, rules = basic (циклический перебор) для новых подключений. |
| Пробы работоспособности | По умолчанию: интервал выборки — 15 секунд. Изъятие из ротации: 2 последовательных сбоя. Поддерживает пользовательские пробы | Интервал простоя пробы — 30 секунд. Изъятие после 5 последовательных сбоев в интерактивном трафике или одного сбоя пробы в режиме простоя. Поддерживает пользовательские пробы | 
| Разгрузка SSL | Не поддерживается | Поддерживается | 


  

<!---HONumber=AcomDC_0713_2016-->