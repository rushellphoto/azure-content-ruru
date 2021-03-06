<properties 
	pageTitle="Руководство по устранению неполадок потоковой передачи в реальном времени" 
	description="В этом разделе приводятся предложения по решению проблем потоковой передачи в реальном времени." 
	services="media-services" 
	documentationCenter="" 
	authors="juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/22/2016"  
	ms.author="juliako"/>

#Руководство по устранению неполадок потоковой передачи в реальном времени

В этом разделе приводятся предложения по решению некоторых проблем потоковой передачи в реальном времени.

## Проблемы, связанные с локальными кодировщиками 

Данный раздел содержит рекомендации по устранению неполадок, связанных с локальными кодировщиками, настроенными для отправки односкоростного потока в каналы AMS, которые выполняют кодирование в реальном времени.

###Проблема: необходимо просмотреть журналы 

- **Потенциальная проблема**: не удается найти журналы кодировщика, которые могут помочь в отладке.
	
	- **Telestream Wirecast**: обычно журналы можно найти в папке C:\\Users\\{имя\_пользователя}\\AppData\\Roaming\\Wirecast\\.
	- **Elemental Live**: ссылки на журналы можно найти на портале управления. Щелкните **Статистика**, затем — **Журналы**. На странице **Файлы журналов** вы увидите список журналов для всех элементов LiveEvent. Выберите журнал текущего сеанса.
	- **Flash Media Live Encoder**: каталог **Log Directory...** можно найти, перейдя на вкладку **Encoding Log** (Журнал кодирования).
	
###Проблема. Невозможно вывести поток с прогрессивной разверткой

- **Потенциальная причина**. Используемый кодировщик автоматически не выполняет устранение чересстрочной развертки.

	**Шаги по устранению неполадок**. Проверьте параметр устранения чересстрочной развертки в интерфейсе кодировщика. После включения устранения чересстрочной развертки еще раз проверьте настройки вывода потока с прогрессивной разверткой.
 
###Проблема. Я попробовал несколько вариантов выходных кодировщика и по-прежнему не могу подключить кодировщик. 

- **Потенциальная причина**. Канал кодирования Azure не был сброшен должным образом.

	**Шаги по устранению неполадок**. Убедитесь, что кодировщик больше не отправляет поток в AMS, остановите и сбросьте канал. После повторного запуска попробуйте подключить кодировщик с новыми параметрами. Если это еще не решило проблему, попробуйте создать полностью новый канал. Иногда работа каналов нарушается после нескольких неудачных попыток подключения.

- **Потенциальная причина**. Размер GOP или ключевого кадра не оптимален.

	**Шаги по устранению неполадок**. Рекомендуемое значение размера GOP или интервала опорного кадра — 2 секунды. В некоторых кодировщиках этот параметр задается в количестве кадров, в других — в количестве секунд. Например: при выводе с частотой кадров 30 кадров/с размер GOP составит 60 кадров, что соответствует 2 секундам.
	 
- **Потенциальная причина**. Поток блокируется из-за закрытых портов.

	**Шаги по устранению неполадок**. При потоковой передаче через RTMP проверьте параметры брандмауэра или прокси-сервера, чтобы убедиться, что исходящие порты 1935 и 1936 открыты. При потоковой передаче через RTP убедитесь, что открыт исходящий порт 2010.


###Проблема. При настройке кодировщика для отправки потока по протоколу RTP негде ввести имя узла. 

- **Потенциальная причина**. Многие кодировщики RTP не разрешают ввода имен узлов, и для них необходимо получить IP-адрес.

	**Шаги по устранению неполадок**. Чтобы получить IP-адрес, откройте командную строку на любом компьютере. В Windows откройте окно «Выполнить» (WIN + R) и введите cmd, чтобы открыть командную строку.

	После открытия командной строки введите «Ping [имя\_узла\_AMS]».

	Имя узла можно получить, опустив номер порта в URL-адресе приема Azure, как показано в следующем примере:

	rtp://test2-amstest009.rtp.channel.mediaservices.windows.net:2010/

	![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###Проблема. Не удается воспроизвести опубликованный поток.
 
- **Потенциальная причина**. Не запущена конечная точка потоковой передачи или не выделены единицы потоковой передачи (единицы масштабирования).

	**Шаги по устранению неполадок**. Перейдите на вкладку «Конечная точка потоковой передачи» в средстве AMSE и убедитесь, что запущена одна конечная точка потоковой передачи с одной единицей потоковой передачи.
	


>[AZURE.NOTE] Если после выполнения этих действий воспроизвести поток по-прежнему не получается, отправьте запрос в службу поддержки с помощью классического портала Azure.

##Схемы обучения работе со службами мультимедиа

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Отзывы

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0629_2016-->