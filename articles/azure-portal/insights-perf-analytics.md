<properties
	pageTitle="Мониторинг производительности веб-приложения Azure | Microsoft Azure"
	description="Диаграммы времени загрузки и ответа, информация о зависимостях и настройка оповещений о производительности."
	services="azure-portal"
    documentationCenter="na"
	authors="alancameronwills"
	manager="douge"/>

<tags
	ms.service="azure-portal"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/28/2016"
	ms.author="awills"/>

# Мониторинг производительности веб-приложения Azure

На [портале Azure](https://portal.azure.com) можно настроить мониторинг для сбора статистических данных и информации о зависимостях приложений в [веб-приложениях Azure](../app-service-web/app-service-web-overview.md) или [виртуальных машинах](../virtual-machines/virtual-machines-linux-about.md).

Благодаря использованию расширений Azure поддерживает мониторинг производительности приложений. Эти расширения устанавливаются в приложение. Они позволяют собирать данные и отправлять отчеты в службы мониторинга.

**Application Insights** и **New Relic** — это два доступных расширения для мониторинга производительности. Чтобы использовать их, необходимо установить агента в среде выполнения. Кроме того, благодаря Application Insights можно создать свой код с помощью пакета SDK. Пакет SDK позволяет писать код для более подробного отслеживания использования и производительности приложения.

## Application Insights

### (Необязательно). Перестроить приложение с помощью пакета SDK…

Установите пакет SDK в свое приложение, чтобы воспользоваться более подробными сведениями телеметрии Application Insights.

В проект в Visual Studio (2013 с обновлением 2 или более поздней версии) добавьте пакет SDK для Application Insights.

![Щелкните правой кнопкой мыши веб-проект и выберите пункт "Добавить Application Insights".](./media/insights-perf-analytics/03-add.png)

Если отобразится предложение войти, воспользуйтесь данными вашей учетной записи Azure.

Операция выполняет два действия:

1. Создает ресурс Application Insights, в котором выполняется отображение, хранение и анализ данных телеметрии.
2. Добавляет пакет NuGet Application Insights в код и настраивает его для отправки данных телеметрии в ресурс Azure.

Телеметрию можно проверить путем запуска приложения на компьютере разработчика (F5) или можно сразу повторно опубликовать приложение.

Пакет SDK предоставляет API-интерфейс, чтобы пользователь мог [написать пользовательские телеметрии](../application-insights/app-insights-api-custom-events-metrics.md) для отслеживания использования.

### Настройка ресурса вручную

Если пакет SDK не добавлен в Visual Studio, в Azure необходимо настроить ресурс Application Insights, где будут храниться, анализироваться и отображаться данные телеметрии.

![Последовательно выберите "Добавить", "Службы для разработчиков", "Application Insights". Выберите тип приложения ASP.NET.](./media/insights-perf-analytics/01-new.png)


## Включить расширение

1. Перейдите к колонке управления веб-приложения или виртуальной машины, которую нужно инструментировать.

2. Добавьте расширение Application Insights или New Relic.

    Если вы инструментируете веб-приложение:

![Параметры, расширения, добавление, Application Insights](./media/insights-perf-analytics/05-extend.png)

Или, если вы используете виртуальную машину:

![Щелкните плитку аналитики](./media/insights-perf-analytics/10-vm1.png)



## Просмотр данных

1. Откройте ресурс Application Insights (непосредственно с использованием кнопки "Обзор" или с помощью средства наблюдения за производительностью в веб-приложении).

2. Щелкните любую диаграмму, чтобы получить более подробные сведения.

    ![В колонке "Обзор" Application Insights щелкните диаграмму](./media/insights-perf-analytics/07-dependency.png)

    Вы можете [настроить колонки метрик](../application-insights/app-insights-metrics-explorer.md).

3. Вы можете щелкнуть отдельные события, чтобы просмотреть сведения о них и их свойства.

    ![Щелкните тип события, по которому нужно отфильтровать результаты поиска](./media/insights-perf-analytics/08-requests.png)

    Если щелкнуть знак многоточия, откроются все свойства.

    Вы можете [настроить поисковые запросы](../application-insights/app-insights-diagnostic-search.md).

Для более эффективного поиска по данным телеметрии используйте [язык запросов аналитики](../application-insights/app-insights-analytics-tour.md).


## Вопросы и ответы

Как настроить отправку данных на другой ресурс Application Insights?

* *При добавлении Application Insights в код в Visual Studio*: щелкните проект правой кнопкой мыши, выберите **Application Insights > Настройка**, а затем — нужный ресурс. После этого вы сможете создать, заново собрать или повторно развернуть ресурс.
* *В противном случае*: в Azure откройте колонку управления веб-приложением и выберите **Сервис > Расширения**. Удалите расширение Application Insights. Затем откройте **Сервис > Производительность**, перейдите по ссылке "щелкните здесь", выберите пункт Application Insights, а затем — нужный ресурс. (Если нужно создать ресурс Application Insights, сделайте это в первую очередь.)


## Дальнейшие действия

* [Отслеживайте метрики состояния службы](insights-how-to-customize-monitoring.md), чтобы убедиться, что служба доступна и отвечает на запросы.
* [Включите отслеживание и диагностику](insights-how-to-use-diagnostics.md), чтобы собирать подробные метрики о службе с высокой частотой.
* [Получайте уведомления](insights-receive-alert-notifications.md) при возникновении операционных событий или превышении пороговых значений метрик.
* Используйте [приложения Application Insights для JavaScript и веб-страниц](../application-insights/app-insights-web-track-usage.md), чтобы получать телеметрию клиентов из браузеров, которые используются для открытия веб-страницы.
* [Настройте веб-тесты доступности](../application-insights/app-insights-monitor-web-app-availability.md), чтобы получать оповещение, если сайт не работает.

<!---HONumber=AcomDC_0803_2016-->