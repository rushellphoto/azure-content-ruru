<properties 
   pageTitle="Анализ производительности запросов базы данных Azure SQL" 
   description="Мониторинг производительности запросов определяет запросы, максимально использующие ресурсы процессора, в базе данных SQL Azure." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/09/2016"
   ms.author="sstein"/>

# Анализ производительности запросов базы данных Azure SQL


Управление производительностью реляционных баз данных и ее настройка являются сложной задачей, требующей значительного опыта и времени. Анализ производительности запросов позволяет тратить меньше времени на устранение неполадок с производительностью базы данных, предоставляя следующие возможности:

- Более глубокое понимание потребления ресурсов базы данных (DTU).
- Определение запросов, максимально использующих ресурсы процессора. Этот показатель можно настроить и улучшить производительность.
- Возможность получить подробные сведения о запросе.

## Предварительные требования

- Анализ производительности запросов доступен только с базой данных SQL Azure версии 12.
- Для анализа производительности запросов в базе данных должно быть запущено [хранилище запросов](https://msdn.microsoft.com/library/dn817826.aspx). Если хранилище запросов не запущено, портал подаст запрос на его включение.

 
## Разрешения

Для использования анализа производительности запросов необходимо [управление доступом на основе ролей](../active-directory/role-based-access-control-configure.md) с такими разрешениями:

- разрешения **читатель**, **владелец**, **участник**, **участник базы данных SQL** или **участник сервера SQL Server** для просмотра основных ресурсов, использующих запросы и диаграммы;
- разрешения **владелец**, **участник**, **участник базы данных SQL** или **участник сервера SQL Server** для просмотра текста запросов.



## Использование анализа производительности запросов

Анализ производительности запросов легко использовать:

- Просмотрите список самых ресурсоемких запросов.
- Чтобы просмотреть сведения о нем, выберите отдельный запрос.
- Откройте [помощник по базам данных SQL](sql-database-advisor.md) и проверьте наличие рекомендаций.
- Для просмотра дополнительных сведений увеличьте масштаб.

    ![панель мониторинга производительности](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Хранилище запросов должно сохранить несколько часов данных, чтобы база данных SQL могла предоставить анализ производительности запросов. Если в базе данных не выполняются действия или хранилище запросов было неактивно в течение определенного времени, диаграммы не содержат данные за этот период. Если хранилище запросов не запущено, можно включить его в любой момент.



## Просмотр запросов, максимально использующих ресурсы процессора

На [портале](http://portal.azure.com) выполните следующие действия.

1. Перейдите к базе данных SQL и выберите пункты **Параметры** > **Производительность** > **Запросы**.

    .![Анализ производительности запросов][1]

    Откроется представление популярных запросов и список запросов, наиболее активно использующих ресурсы процессора.

1. Щелкните диаграмму для получения сведений.<br>В верхней строке отображается общий процент DTU для базы данных. Столбцы отображают процент ресурсов процессора, используемых выбранными запросами за определенный период (например, если выбрана **прошлая неделя**, каждый столбец соответствует одному дню).

    .![наиболее ресурсоемкие запросы][2]

    В приведенной ниже таблице представлены сводные данные по видимым запросам.

    -	Идентификатор запроса — уникальный идентификатор запроса внутри базы данных.
    -	Загрузка ЦП на запрос за период наблюдения (зависит от статистической функции).
    -	Длительность запроса (зависит от статистической функции).
    -	Общее число выполнений определенного запроса.


	Выделите отдельные запросы или отмените их выделение, чтобы включить их в диаграмму или исключить из нее.


1. Если данные устарели, нажмите кнопку **Обновить**.
1. Либо щелкните **Параметры**, чтобы настроить отображение данных об использовании ресурсов процессора или отобразить другой период времени.

    ![Параметры](./media/sql-database-query-performance/settings.png)

## Просмотр подробных сведений для отдельного запроса

Чтобы просмотреть сведения о запросе, выполните следующие действия.

1. Щелкните любой запрос в списке популярных запросов.

    ![сведения](./media/sql-database-query-performance/details.png)

4. Откроется представление сведений, в котором использование ресурсов процессора запросами разбито по времени.
3. Щелкните диаграмму для получения сведений.<br>Верхняя строка показывает общий процент DTU, а полосы процент ресурсов процессора, используемых выбранным запросом.
4. Изучите данные, чтобы увидеть подробные показатели, в том числе длительность, количество выполнений, процент использования ресурсов для каждого интервала, в течение которого выполнялся запрос.
    
    ![сведения о запросе][3]

1. Либо щелкните **Параметры**, чтобы настроить отображение данных об использовании ресурсов процессора или отобразить другой период времени.


## 	Оптимизация конфигурации хранилища запросов для анализа производительности запросов

При работы с анализом производительности запросов хранилище запросов может выдавать следующие сообщения:

- "Хранилище запросов достигло максимальной емкости и не собирает новые данные".
- "Хранилище запросов для этой базы данных находится в режиме только для чтения и не собирает данные о производительности".
- "Параметры хранилища запросов не оптимальны для анализа производительности запросов".

Обычно эти сообщения отображаются, когда хранилище запросов не может собирать новые данные. Есть несколько вариантов устранения этих проблем:

-	изменить политику хранения и отслеживания хранилища запросов;
-	увеличить размер хранилища запросов;
-	очистить хранилище запросов.

### Рекомендуемая политика хранения и отслеживания

Политики хранения бывают двух видов.

- На основе размера: если указано значение AUTO, по достижении максимального размера хранилища данные автоматически удаляются.
- На основе времени: если в хранилище запросов не хватает места, удаляются данные запросов, поступивших ранее, чем 30 дней назад (значение по умолчанию).

Для политики отслеживания доступны следующие параметры.

- **All**: фиксируются все запросы. **All** — вариант по умолчанию.
- **Auto**: редкие запросы и запросы с незначительным временем компиляции и выполнения игнорируются. Пороговые значения счетчика, а также времени компиляции и выполнения установлены внутри хранилища.
- **None**: хранилище запросов не отслеживает новые запросы.
	
Рекомендуем выбрать для всех политик параметр AUTO и настроить удаление через 30 дней:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
    	
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Увеличьте размер хранилища запросов. Для этого подключитесь к базе данных и отправьте следующий запрос:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

При очистке хранилища запросов Удаляет все текущие данные в хранилище запросов:

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## Сводка

Анализ производительности запросов помогает понять влияние рабочей нагрузки запросов и его связь с потреблением ресурсов базы данных. Благодаря этой функции можно узнать о наиболее ресурсоемких запросах и легко определить запросы, которые нужно исправить, прежде чем они станут проблемными.




## Дальнейшие действия

Для получения дополнительных рекомендаций по повышению производительности базы данных SQL щелкните [Помощник по базам данных](sql-database-advisor.md) в колонке **Анализ производительности запросов**.

.![Помощник по производительности](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png

<!---HONumber=AcomDC_0810_2016-->