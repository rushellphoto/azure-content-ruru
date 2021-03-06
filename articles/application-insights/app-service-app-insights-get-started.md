<properties 
	pageTitle="Application Insights в Microsoft Azure" 
	description="Обнаружение, сортировка и диагностика проблем в активном веб-приложении или приложении на устройстве. Непрерывно отслеживайте приложение и повышайте оптимальность его работы для пользователей." 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/31/2016" 
	ms.author="awills"/>
 
# Visual Studio Application Insights

Application Insights — это расширяемая служба аналитики, которая отслеживает ваше живое приложение. Она помогает обнаруживать и диагностировать проблемы производительности, а также понять, что пользователи действительно делают с вашим приложением. Она предназначена для разработчиков и помогает постоянно улучшать производительность и удобство использования приложения.

![Составляйте диаграммы по действиям пользователей или детализируйте конкретные события.](./media/app-service-app-insights-get-started/00-sample.png)

Служба работает как с автономными, так и с веб-приложениями на самых разнообразных платформах, включая приложения .NET и J2EE с локальным или облачным размещением.

Служба Application Insights предназначена для команды разработчиков. С ее помощью можно выполнять следующие действия.

* [Анализировать шаблоны использования][knowUsers] для более глубокого понимания пользователей и постоянного усовершенствования приложений.
 * Формировать статистику по количеству просмотров, новым и повторным пользователям, географическому распределению, платформам и другим важным данным использования.
 * Трассировать пути использования для оценки успешности каждой функции.
* [Обнаруживайте, рассматривайте и диагностируйте][detect] проблемы производительности и устраняйте их, прежде чем большинство пользователей выявит их.
 *  Предупреждения об изменениях производительности или сбоях.
 *  Метрики, которые помогут диагностировать проблемы производительности, например время отклика, использование ЦП, отслеживание зависимостей.
 *  Тесты доступности для веб-приложений.
 *  Отчеты и уведомления о сбоях и исключениях.
 *  Эффективный поиск журналов диагностики (включая трассировки журналов на предпочитаемой платформе ведения журналов).

Пакет SDK для каждой платформы включает ряд модулей, приступающих к мониторингу приложения сразу после установки. Для получения более подробной, специализированной аналитики можно написать код для собственной телеметрии.

Полученные из приложения данные телеметрии хранятся и анализируются на портале Azure с интуитивно-понятными представлениями и эффективными средствами для быстрой диагностики и анализа.



Необходим еще более подробный анализ? [Экспортируйте](app-insights-export-telemetry.md) данные [в SQL](app-insights-code-sample-export-telemetry-sql-database.md), [Power BI](app-insights-export-power-bi.md) или в собственные инструменты.

.![Просмотр данных в Power BI](./media/app-service-app-insights-get-started/210.png)

## Платформы и языки

Количество платформ, для которых выпущены пакеты SDK, постоянно растет. В настоящее время в этот список входят следующие.

 * [Серверы ASP.NET][greenbrown] на сервере Azure или вашем сервере IIS
 * [Облачные службы Azure](app-insights-cloudservices.md)
 * [Серверы J2EE][java]
 * [веб-страницы][client] — HTML и JavaScript.
 * [Службы Windows, рабочие роли и настольные приложения][desktop]
 * [Другие платформы][platforms]\: Node.js, PHP, Python, Ruby, Joomla, SharePoint, WordPress

Application Insights может также получать данные телеметрии из существующих веб-приложений ASP.NET на IIS, не требуя их изменения.

Если у вашего приложения есть клиент, сервер и другие компоненты, можно инструментировать их все. Данные будут интегрированы с порталом Application Insights, чтобы вы могли, например, сопоставлять события на стороне клиента с событиями на сервере.


## Принцип работы

Вы устанавливаете небольшой пакет SDK в приложение и настраиваете учетную запись на портале Application Insights. Пакет SDK отслеживает приложение и отправляет данные телеметрии на портал. Портал отображает статистические диаграммы и предоставляет мощные средства, чтобы помочь вам диагностировать любые проблемы.

![Пакет SDK Application Insights в приложении отправляет телеметрию в ресурс Application Insights на портале Azure.](./media/app-service-app-insights-get-started/01-scheme.png)

Пакет SDK содержит несколько модулей, собирающих телеметрию, например для подсчета пользователей, сеансов и производительности. Вы также можете написать собственный код для отправки данных телеметрии на портал. Пользовательская телеметрия особенно полезна для трассировки пользовательской Истории. Вы можете вести учет событий, таких как число нажатий кнопки, достижение конкретных целей или ошибки пользователя.

Для серверов ASP.NET и веб-приложений Azure можно также установить [монитор состояний][redfield], который используется в двух целях. Он позволяет:

* выполнять мониторинг веб-приложения без необходимости повторного создания или повторной установки;
* отслеживать вызовы зависимых модулей.



### Увеличение нагрузки

Влияние на производительность очень мало. Отслеживание вызовов не ведет к блокировке, выполняется в пакетном режиме и отправляется в отдельном потоке.



## Начало работы

1. Вам потребуется подписка на [Microsoft Azure](http://azure.com). Вы можете бесплатно зарегистрироваться и выбрать бесплатную [ценовую категорию](https://azure.microsoft.com/pricing/details/application-insights/) службы Application Insights.

2. Если используется Visual Studio:

 * Новый проект: установите флажок **Добавить Application Insights**.
 * Существующий проект: щелкните проект правой кнопкой мыши и выберите пункт **Добавить Application Insights**.

Если вы не используете Visual Studio или эти параметры недоступны для вашего проекта, найдите свою платформу [здесь](app-insights-platforms.md).

3. Запустите приложение в режиме разработки (или опубликуйте его) и посмотрите, как накапливаются данные на [портале Azure](https://portal.azure.com).

## Код


[Примеры и пошаговые руководства](app-insights-code-samples.md)

[Лабораторные работы SDK](https://www.myget.org/gallery/applicationinsights-sdk-labs) — это пакеты NuGet, которые можно установить (и удалить) в качестве дополнений к пакету SDK Application Insights. Попробуйте и напишите свои отзывы!


## Поддержка и обратная связь

* Вопросы и проблемы
 * [Устранение неполадок][qna]
 * [Форум MSDN](https://social.msdn.microsoft.com/Forums/vstudio/ru-RU/home?forum=ApplicationInsights)
 * [Stackoverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Ошибки
 * [Подключение](https://connect.microsoft.com/VisualStudio/Feedback/LoadSubmitFeedbackForm?FormID=6076)
* Предложения
 * [UserVoice](https://visualstudio.uservoice.com/forums/357324)


## Видеоролики


> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]


<!--Link references-->

[android]: https://github.com/Microsoft/ApplicationInsights-Android
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[desktop]: app-insights-windows-desktop.md
[detect]: app-insights-detect-triage-diagnose.md
[greenbrown]: app-insights-asp-net.md
[ios]: https://github.com/Microsoft/ApplicationInsights-iOS
[java]: app-insights-java-get-started.md
[knowUsers]: app-insights-overview-usage.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[windows]: app-insights-windows-get-started.md

 

<!---HONumber=AcomDC_0810_2016---->