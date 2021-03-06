<properties 
	pageTitle="Автоматическое масштабирование и среда службы приложений" 
	description="Автоматическое масштабирование и среда службы приложений" 
	services="app-service"
	documentationCenter=""
	authors="btardif" 
	manager="wpickett" 
	editor="" 
/>

<tags 
	ms.service="app-service" 
	ms.workload="web" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/07/2016" 
	ms.author="byvinyal"
/>
	
# Автоматическое масштабирование и среда службы приложений

## Введение

**Среды службы приложений** поддерживают автоматическое масштабирование. Это достигается путем разрешения автоматического масштабирования отдельных пулов исполнителей на основе метрик или расписания.
 
![][intro]
 
Автоматическое масштабирование позволяет оптимизировать использование ресурсов, автоматически увеличивая и уменьшая **среду службы приложений** в зависимости от размера бюджета и/или профиля нагрузки.

## Настройка автоматического масштабирования пула исполнителей 

Доступ к функции автоматического масштабирования можно получить на вкладке «Параметры» **пула исполнителей**.
 
![][settings-scale]

Вы наверняка знакомы с этим интерфейсом, поскольку он аналогичен масштабированию **плана службы приложений**. Вы можете вручную ввести значение масштаба:
 
![][scale-manual]
 
Или настроить профиль автоматического масштабирования:
 
![][scale-profile]
 
Профили автоматического масштабирования полезны для установки ограничений масштаба. Таким образом можно обеспечить как согласованную производительность, установив нижнюю границу значения масштаба (1), так и прогнозируемый максимум затрат, установив верхнюю границу (2).
 
![][scale-profile2]
 
После определения профиля можно добавить правила автоматического масштабирования на основе метрик для увеличения или уменьшения количества экземпляров в **пуле исполнителей** в пределах границ, определенных профилем.

![][scale-rule]

 Для определения правил автомасштабирования можно использовать любую из метрик **пула исполнителей** или **внешнего интерфейса**. Это те же метрики, которые можно отслеживать на графиках в колонке ресурсов или устанавливать для них оповещения.
 
## Пример автоматического масштабирования

Автомасштабирование **среды службы приложений** лучше всего проиллюстрировать пошаговым сценарием.

В этой статье я рассмотрю все факторы, которые необходимо учитывать во время настройки автоматического масштабирования, а также все взаимосвязи, которые вступят в силу, если мы будем учитывать автоматическое масштабирование **сред службы приложений**, размещенных в ASE.

### Введение в сценарий

Федор — системный администратор предприятия, который перенес часть управляемых им рабочих нагрузок в **среду службы приложений**.

**Среда службы приложений** настроена для масштабирования вручную следующим образом:

* **внешние интерфейсы**: 3;
* **рабочий пул 1**: 10;
* **рабочий пул 2**: 5;
* **рабочий пул 3**: 5.

**Пул исполнителей 1** используется для производственных рабочих нагрузок, а **пул исполнителей 2** и **пул исполнителей 3** — для рабочих нагрузок в целях контроля качества и разработки.

**Планы службы приложений**, используемые для контроля качества и разработки, настроены на **ручное масштабирование**, однако для производственного **плана службы приложений** установлено **автоматическое масштабирование**, позволяющее справляться с вариациями нагрузки и трафика.

Федор хорошо знаком с приложением и знает, что часы пиковой нагрузки длятся с 09:00 до 18:00, поскольку это *бизнес-приложение* используется сотрудниками, когда они находятся в офисе. После окончания рабочего дня использование снижается. Однако часть нагрузки остается, так как пользователи могут получать удаленный доступ к приложению с мобильных устройств или домашних компьютеров. Производственный **план службы приложений** уже настроен на **автоматическое масштабирование** на основе использования ЦП со следующими правилами:

.![][asp-scale]

|	**Профиль автоматического масштабирования — рабочие дни — план службы приложений** |	**Профиль автоматического масштабирования — выходные дни — план службы приложений** |
|	----------------------------------------------------	|	----------------------------------------------------	|
|	**Имя:** профиль рабочего дня |	**Имя:** профиль выходного дня |
|	**Масштабирование:** расписание и правила производительности |	**Масштабирование:** расписание и правила производительности |
|	**Профиль:** рабочие дни |	**Профиль:** выходные дни |
|	**Тип:** повторение |	**Тип:** повторение |
|	**Целевой диапазон:** 5–20 экземпляров |	**Целевой диапазон:** 3–10 экземпляров |
|	**Дни:** понедельник, вторник, среда, четверг, пятница |	**Дни:** суббота, воскресенье |
|	**Время начала:** 09:00 |	**Время начала:** 09:00 |
|	**Часовой пояс:** UTC-08 |	**Часовой пояс:** UTC-08 |
| | |
|	**Правило автоматического масштабирования (увеличения)** |	**Правило автоматического масштабирования (увеличения)** |
|	**Ресурс:** производство (среда службы приложений) |	**Ресурс:** производство (среда службы приложений) |
|	**Метрика:** % ЦП |	**Метрика:** % ЦП |
|	**Операции:** более 60 % |	**Операции:** более 80 % |
|	**Длительность:** 5 минут |	**Длительность:** 10 минут |
|	**Агрегация времени:** среднее |	**Агрегация времени:** среднее |
|	**Действие:** увеличить количество на 2 |	**Действие:** увеличить количество на 1 |
|	**Охлаждение (в минутах):** 15 |	**Охлаждение (в минутах):** 20 |
| | |
|	**Правило автоматического масштабирования (уменьшения)** |	**Правило автоматического масштабирования (уменьшения)** |
|	**Ресурс:** производство (среда службы приложений) |	**Ресурс:** производство (среда службы приложений) |
|	**Метрика:** % ЦП |	**Метрика:** % ЦП |
|	**Операции:** менее 30 % |	**Операции:** менее 20 % |
|	**Длительность:** 10 минут |	**Длительность:** 15 минут |
|	**Агрегация времени:** среднее |	**Агрегация времени:** среднее |
|	**Действие:** уменьшить количество на 1 |	**Действие:** уменьшить количество на 1 |
|	**Охлаждение (в минутах):** 20 |	**Охлаждение (в минутах):** 10 |

### Скорость роста плана службы приложений

**Планы службы приложений**, настроенные на автоматическое масштабирование, выполняют его с максимальной скоростью. Эту скорость можно вычислить на основе значений, указанных в правиле автоматического масштабирования.

Понимание и вычисление **скорости роста плана службы приложений** очень важно для автоматического масштабирования **среды службы приложений**, так как изменения масштаба **рабочего пула** осуществляются не мгновенно, и для их применения требуется некоторое время.

Скорость роста **плана службы приложений** вычисляется следующим образом:

![][ASP-Inflation]

При применении правила *Автоматическое масштабирование — увеличение* для профиля *Рабочие дни* производственного **плана службы приложений** это будет выглядеть следующим образом:

.![][Equation1]

Если применяется правило *Автоматическое масштабирование — увеличение* для профиля *Выходные дни* производственного **плана службы приложений**, формула будет рассчитана следующим образом:

.![][Equation2]

Это значение также можно вычислить для операций масштабирования в сторону уменьшения:

При применении правила *Автоматическое масштабирование — уменьшение* для профиля *Рабочие дни* производственного **плана службы приложений** это будет выглядеть следующим образом:

.![][Equation3]

Если применяется правило *Автоматическое масштабирование — уменьшение* для профиля *Выходные дни* производственного **плана службы приложений**, формула будет рассчитана следующим образом:

.![][Equation4]

Это означает, что производственный **план службы приложений** может увеличиваться с максимальной скоростью **8** экземпляров в час в течение рабочей недели и **4** экземпляра в час в выходные дни. Экземпляры могут высвобождаться с максимальной скоростью **4** экземпляра в час в течение рабочей недели и **6** экземпляров в час в выходные дни.

Если в **рабочем пуле** размещено несколько **планов службы приложений**, то необходимо рассчитать **общую скорость роста**, которую можно выразить как *сумму* скорости роста для всех **планов службы приложений**, размещаемых в данном **рабочем пуле**.

![][ASP-Total-Inflation]

### Использование скорости роста плана службы приложений для определения правил автоматического масштабирования пула исполнителей

**Пулам исполнителей**, в которых размещаются **планы службы приложений**, настроенные для автоматического масштабирования, необходимо будет выделить буфер емкости, позволяющий операциям автоматического масштабирования увеличивать или уменьшать **план службы приложений** при необходимости. Минимальным буфером будет вычисленная **общая скорость роста плана службы приложений**.

Поскольку применение операций масштабирования **среды службы приложений** занимает некоторое время, любое изменение должно учитывать дальнейшие изменения спроса, которые могут произойти во время выполнения операции масштабирования. Для этого рекомендуется использовать вычисляемую **общую скорость роста плана службы приложений** как минимальное количество экземпляров, добавляемых для каждой операции автоматического масштабирования.

С помощью этой информации Федор может определить следующий профиль и правила автоматического масштабирования:

.![][Worker-Pool-Scale]

|	**Профиль автоматического масштабирования — рабочие дни** |	**Профиль автоматического масштабирования — выходные дни** |
|	----------------------------------------------------	|	--------------------------------------------	|
|	**Имя:** профиль рабочего дня |	**Имя:** профиль выходного дня |
|	**Масштабирование:** расписание и правила производительности |	**Масштабирование:** расписание и правила производительности |
|	**Профиль:** рабочие дни |	**Профиль:** выходные дни |
|	**Тип:** повторение |	**Тип:** повторение |
|	**Целевой диапазон:** 13–25 экземпляров |	**Целевой диапазон:** 6–15 экземпляров |
|	**Дни:** понедельник, вторник, среда, четверг, пятница |	**Дни:** суббота, воскресенье |
|	**Время начала:** 07:00 |	**Время начала:** 09:00 |
|	**Часовой пояс:** UTC-08 |	**Часовой пояс:** UTC-08 |
| | |
|	**Правило автоматического масштабирования (увеличения)** |	**Правило автоматического масштабирования (увеличения)** |
|	**Ресурс:** пул исполнителей 1 |	**Ресурс:** пул исполнителей 1 |
|	**Метрика:** WorkersAvailable |	**Метрика:** WorkersAvailable |
|	**Операции:** менее 8 |	**Операции:** менее 3 |
|	**Длительность:** 20 минут |	**Длительность:** 30 минут |
|	**Агрегация времени:** среднее |	**Агрегация времени:** среднее |
|	**Действие:** увеличить количество на 8 |	**Действие:** увеличить количество на 3 |
|	**Охлаждение (в минутах):** 180 |	**Охлаждение (в минутах):** 180 |
| | |
|	**Правило автоматического масштабирования (уменьшения)** |	**Правило автоматического масштабирования (уменьшения)** |
|	**Ресурс:** пул исполнителей 1 |	**Ресурс:** пул исполнителей 1 |
|	**Метрика:** WorkersAvailable |	**Метрика:** WorkersAvailable |
|	**Операции:** более 8 |	**Операции:** более 3 |
|	**Длительность:** 20 минут |	**Длительность:** 15 минут |
|	**Агрегация времени:** среднее |	**Агрегация времени:** среднее |
|	**Действие:** уменьшить количество на 2 |	**Действие:** уменьшить количество на 3 |
|	**Охлаждение (в минутах):** 120 |	**Охлаждение (в минутах):** 120 |

Целевой диапазон, определенный в профиле, вычисляется по минимальному числу экземпляров, определенных в профиле для **плана службы приложений** + буфер.

Максимальный диапазон будет суммой всех максимальных диапазонов для всех **планов службы приложений**, размещенных в **пуле исполнителей**.

Количество увеличения для правил увеличения масштаба должно быть задано в размере как минимум одной **скорости роста плана службы приложений** для увеличения масштаба.

Количество уменьшения можно отрегулировать между 1/2X и 1X от **скорости роста плана службы приложений** для масштабирования с уменьшением.

### Автоматическое масштабирование пула внешних интерфейсов

Правила автоматического масштабирования **внешних интерфейсов** проще, чем для **пулов исполнителей**. Основные моменты, которые следует учитывать: длительность измерения и таймер охлаждения должны учитывать, что операции масштабирования в **планах службы приложений** выполняются не мгновенно.

Федор знает, что частота ошибок увеличивается после достижения внешним интерфейсом 80 % загрузки ЦП. Чтобы предотвратить это, он устанавливает правило автоматического масштабирования для увеличения экземпляров следующим образом:
 
.![][Front-End-Scale]
  
|	**Профиль автоматического масштабирования — внешние интерфейсы** |
|	--------------------------------------------	|
|	**Имя:** автоматическое масштабирование — внешние интерфейсы |
|	**Масштабирование:** расписание и правила производительности |
|	**Профиль:** ежедневно |
|	**Тип:** повторение |
|	**Целевой диапазон:** 3–10 экземпляров |
|	**Дни:** ежедневно |
|	**Время начала:** 09:00 |
|	**Часовой пояс:** UTC-08 |
| |
|	**Правило автоматического масштабирования (увеличения)** |
|	**Ресурс:** пул внешних интерфейсов |
|	**Метрика:** % ЦП |
|	**Операции:** более 60 % |
|	**Длительность:** 20 минут |
|	**Агрегация времени:** среднее |
|	**Действие:** увеличить количество на 3 |
|	**Охлаждение (в минутах):** 120 |
| |
|	**Правило автоматического масштабирования (уменьшения)** |
|	**Ресурс:** пул исполнителей 1 |
|	**Метрика:** % ЦП |
|	**Операции:** менее 30 % |
|	**Длительность:** 20 минут |
|	**Агрегация времени:** среднее |
|	**Действие:** уменьшить количество на 3 |
|	**Охлаждение (в минутах):** 120 |

<!-- IMAGES -->
[intro]: ./media/app-service-environment-auto-scale/introduction.png
[settings-scale]: ./media/app-service-environment-auto-scale/settings-scale.png
[scale-manual]: ./media/app-service-environment-auto-scale/scale-manual.png
[scale-profile]: ./media/app-service-environment-auto-scale/scale-profile.png
[scale-profile2]: ./media/app-service-environment-auto-scale/scale-profile-2.png
[scale-rule]: ./media/app-service-environment-auto-scale/scale-rule.png
[asp-scale]: ./media/app-service-environment-auto-scale/asp-scale.png
[ASP-Inflation]: ./media/app-service-environment-auto-scale/asp-inflation-rate.png
[Equation1]: ./media/app-service-environment-auto-scale/equation1.png
[Equation2]: ./media/app-service-environment-auto-scale/equation2.png
[Equation3]: ./media/app-service-environment-auto-scale/equation3.png
[Equation4]: ./media/app-service-environment-auto-scale/equation4.png
[ASP-Total-Inflation]: ./media/app-service-environment-auto-scale/asp-total-inflation-rate.png
[Worker-Pool-Scale]: ./media/app-service-environment-auto-scale/wp-scale.png
[Front-End-Scale]: ./media/app-service-environment-auto-scale/fe-scale.png

<!---HONumber=AcomDC_0810_2016-->