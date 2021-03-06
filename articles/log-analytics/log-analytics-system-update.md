<properties
	pageTitle="Решение ";Оценка системных обновлений"; в Log Analytics | Microsoft Azure"
	description="Решение для обновлений системы в службе Log Analytics позволяет применять отсутствующие обновления к серверам в инфраструктуре."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/06/2016"
	ms.author="banders"/>

# Решение "Оценка системных обновлений" в Log Analytics


Решение для обновлений системы в службе Log Analytics позволяет применять отсутствующие обновления к серверам в инфраструктуре. После установки решения можно просмотреть обновления, отсутствующие на отслеживаемых серверах, с помощью элемента **Оценка системных обновлений** на странице **Обзор** в OMS.

Сведения об отсутствующих обновлениях отображаются на панели мониторинга **Обновления**. На панели мониторинга **Обновления** можно работать с пропущенными обновлениями и разработать план для всех серверов, которым требуются обновления.

## Установка и настройка решения
Для установки и настройки решений используйте указанные ниже данные.

- Решение "Оценка системных обновлений" необходимо добавить в рабочую область OMS, как описано в статье [Добавление решений Log Analytics из каталога решений](log-analytics-add-solutions.md). Дополнительная настройка не требуется.

## Сведения о сборе данных об обновлениях системы

В следующей таблице представлены методы сбора данных и другие сведения о сборе данных для решения "Оценка системных обновлений".

| платформа | Direct Agent | Агент SCOM | Хранилище Azure | Нужен ли SCOM? | Отправка данных агента SCOM через группу управления | частота сбора |
|---|---|---|---|---|---|---|
|Windows|![Да](./media/log-analytics-system-update/oms-bullet-green.png)|![Да](./media/log-analytics-system-update/oms-bullet-green.png)|![Нет](./media/log-analytics-system-update/oms-bullet-red.png)| ![Нет](./media/log-analytics-system-update/oms-bullet-red.png)|![Да](./media/log-analytics-system-update/oms-bullet-green.png)| Не меньше 2 раз в день и через 15 минут после установки обновления|


### Работа с обновлениями

1. На странице **Обзор** щелкните плитку **Оценка системных обновлений**.![image of the Overview page](./media/log-analytics-system-update/oms-updates01.png)
2. На панели мониторинга **Обновления** просмотрите категории обновлений.![image of the Updates page](./media/log-analytics-system-update/oms-updates02.png)
3. Прокрутите страницу вправо для просмотра колонки **Тип пропущенного обновления**, а затем щелкните **Обновления для системы безопасности**.![image of the Updates page](./media/log-analytics-system-update/oms-updates03.png)
4. На странице «Поиск» отображается список обновлений для системы безопасности, которые были пропущены для серверов инфраструктуры. Щелкните идентификатор статьи базы знаний (KBID), чтобы просмотреть дополнительные сведения о пропущенном обновлении. В данном примере: *KBID 3032323*. ![image of the Updates page](./media/log-analytics-system-update/oms-updates04.png)
5. Статья с описанием обновления откроется в веб-браузере. ![изображение статьи базы знаний](./media/log-analytics-system-update/oms-updates05.png)
6. Используя найденную информацию, можно создать план для установки пропущенных обновлений.

## Дальнейшие действия

- [Выполните поиск в журналах](log-analytics-log-searches.md), чтобы просмотреть подробные сведения об обновлениях системы.

<!---HONumber=AcomDC_0518_2016-->