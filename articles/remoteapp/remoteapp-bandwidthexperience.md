<properties 
    pageTitle="Azure RemoteApp — как пропускная способность сети и качество взаимодействия связаны друг с другом? | Microsoft Azure"
	description="Сведения о том, как пропускная способность сети в Azure RemoteApp может влиять на качество взаимодействия с пользователем."
	services="remoteapp"
	documentationCenter="" 
	authors="lizap" 
	manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/27/2016" 
    ms.author="elizapo" />

# Azure RemoteApp — как пропускная способность сети и качество взаимодействия связаны друг с другом?

Если вы рассматриваете [общую пропускную способность сети](remoteapp-bandwidth.md), необходимую Azure RemoteApp, учитывайте следующие факторы, которые входят в состав динамической системы, влияющей на все аспекты взаимодействия с пользователем.

- **Доступная пропускная способность и текущие условия сети** — набор параметров (потери, задержка, дрожание) одной сети, который в каждый момент времени может негативно влиять на потоковую передачу приложений, снижая общие впечатления от использования. Доступная пропускная способность в сети зависит от перегрузки, случайных потерь, задержки, так как эти параметры влияют на механизм контроля перегрузки, который, в свою очередь, управляет скоростью передачи данных для предотвращения конфликтов. Например, сеть с высоким показателем потерь или задержек приводит к неудовлетворительному взаимодействию с пользователем даже в сети с пропускной способностью 1000 МБ. Потери и задержки зависят от числа и действий (например просмотр видеозаписей, скачивание или отправка больших файлов, печать) пользователей, находящихся в одной сети.
- **Сценарий использования** — взаимодействие зависит от того, что пользователи делают по отдельности и в составе группы в рамках одной сети. Например, для чтения одного слайда требуется обновление всего одного кадра. Если же пользователь бегло просматривает и прокручивает содержимое текстового документа, требуется обновлять большее число кадров в секунду. Взаимодействие с сервером в этом случае потребует больше пропускной способности сети. Рассмотрите и экстремальный случай, когда несколько пользователей смотрят видео высокой четкости (например с разрешением 4K), проводят конференции в высоком разрешении, играют в трехмерные игры или работают в системах автоматизированного проектирования. Все это может привести к перегрузке сети даже с очень высокой пропускной способностью.
- **Разрешение и число экранов** — для полного обновления экранов большого размера требуется больше пропускной способности. Базовые технологии помогают справиться с этим, кодируя и передавая только обновленные области экранов, однако периодически требуется обновить весь экран. При использовании экрана с высоким разрешением (например 4K) для его обновления требуется больше пропускной способности, чем для экрана с низким разрешением (например 1024x768). Это справедливо и при использовании нескольких экранов для перенаправления. Пропускная способность должна увеличиваться вместе с числом экранов.
- **Буфер обмена и перенаправление устройств** — это не вполне очевидная проблема, но во многих случаях, когда пользователь сохраняет большой блок данных в буфер обмена, передача этой информации с клиента удаленного рабочего стола на сервер занимает много времени. Качество работы в нисходящем сегменте может снизиться при восходящей отправке содержимого из буфера обмена. То же самое касается и перенаправления устройств — если сканер или веб-камера создают большой объем данных, который должен быть отправлен по восходящему каналу на сервер, или принтер должен принимать большой документ, или локальное хранилище должно быть доступно запущенному в облаке приложению для копирования большого файла, то пользователи могут наблюдать пропуск кадров или временное "зависание" видео, так как данные, необходимые для перенаправления устройств, повышают требования к пропускной способности сети.

При оценке потребностей в пропускной способности сети следует учитывать воздействие всех этих факторов в виде единой системы.

А сейчас вернитесь к [основной статье о пропускной способности сети](remoteapp-bandwidth.md) или перейдите к тестированию [пропускной способности](remoteapp-bandwidthtests.md).

## Подробнее
- [Оценка использования пропускной способности сети Azure RemoteApp](remoteapp-bandwidth.md)

- [Azure RemoteApp — тест использования пропускной способности сети в рамках распространенных сценариев](remoteapp-bandwidthtests.md)

- [Пропускная способность сети Azure RemoteApp — общие рекомендации (если невозможно провести свои тесты)](remoteapp-bandwidthguidelines.md)

<!---HONumber=AcomDC_0629_2016-->