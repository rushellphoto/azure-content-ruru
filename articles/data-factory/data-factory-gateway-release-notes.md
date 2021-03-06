<properties 
	pageTitle="Заметки о выпуске шлюза управления данными | Фабрика данных Azure" 
	description="Заметки о выпуске шлюза управления данными" 
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/17/2016" 
	ms.author="spelluru"/>

# Заметки о выпуске шлюза управления данными

Одной из проблем интеграции данных является незаметное для пользователя перемещение данных между локальной средой и облаком. Для решения этой проблемы в фабрике данных используется шлюз управления данными. Это агент, который можно установить локально для переноса гибридных данных.

Дополнительные сведения см. в статье [Перемещение данных между локальным и облачным хранилищами с помощью фабрики данных Azure](data-factory-move-data-between-onprem-and-cloud.md) и [Data Management Gateway](data-factory-data-management-gateway.md) (Шлюз управления данными).

## ТЕКУЩАЯ ВЕРСИЯ (2.1.6040.1)

- Драйвер DB2 теперь включен в пакет установки шлюза. Вам не нужно устанавливать его отдельно.
- Наряду с уже поддерживаемыми платформами (Linux, Unix и Windows) драйвер DB2 теперь поддерживает z/OS и DB2 для i (AS/400).
- Поддержка использования DocumentDB в качестве источника или назначения для локальных хранилищ данных.
- Поддержка копирования данных в горячее и холодное хранилище BLOB-объектов наряду с уже поддерживаемой учетной записью общего назначения.
- Возможность подключения к локальному серверу SQL Server через шлюз с правами для удаленного входа.

## Более ранние версии

## 2\.0.6013.1

- Можно выбрать язык и региональные параметры, используемые при ручной установке шлюза.
- Если шлюз не работает, как ожидалось, то можно отправить в корпорацию Майкрософт журналы шлюза за последние 7 дней, чтобы упростить устранение проблемы. Если шлюз не подключен к облачной службе, то можно выбрать сохранение и архивирование журналов шлюза.
- Улучшения пользовательского интерфейса диспетчера конфигурации шлюза:
	- Улучшено отображение состояния шлюза на вкладке "Главная".
	- Элементы управления реорганизованы и упрощены.
- Из хранилища, отличного от хранилища BLOB-объектов Azure, можно скопировать данные в хранилище данных SQL Azure с помощью PolyBase и промежуточного большого двоичного объекта, используя [инструмент копирования с предварительным просмотром, не требующий программирования](data-factory-copy-data-wizard-tutorial.md). В разделе [Промежуточное копирование](data-factory-copy-activity-performance.md#staged-copy) приведены общие сведения об этой функции.
- Шлюз управления данными можно использовать для передачи данных непосредственно из локальной базы данных SQL Server в Машинное обучение Azure.
- Повышение производительности.
	- Повысьте производительность при просмотре схемы или предварительном просмотре данных SQL Server с помощью инструмента копирования с предварительным просмотром, не требующего программирования.



## 1\.12.5953.1
- Исправления ошибок

## 1\.11.5918.1

- Максимальный размер журнала событий шлюза был увеличен с 1 МБ до 40 МБ.
- Если во время автоматического обновления шлюза требуется перезагрузка, отображается диалоговое окно с предупреждением. Перезагрузку можно запустить сразу или отложить.
- При сбое автоматического обновления установщик шлюза пытается выполнить автоматическое обновление не больше трех раз.
- Повышение производительности.
	- Повышена эффективность загрузки больших таблиц из локального сервера в сценарий копирования без кода.
- Исправления ошибок

## 1\.10.5892.1

- Повышение производительности.
- Исправления ошибок

## 1\.9.5865.2

- Полностью автоматическое обновление.
- Новый значок на панели задач, позволяющий просматривать индикаторы состояния шлюза.
- Возможность немедленного выполнения обновления из клиента.
- Настройка времени обновления.
- Включение и выключение автоматического обновления с использованием сценария PowerShell.
- Поддержка формата JSON
- Повышение производительности.
- Исправления ошибок

## 1\.8.5822.1

- Улучшение возможностей устранения неполадок.
- Повышение производительности.
- Исправления ошибок

### 1\.7.5795.1

- Повышение производительности.
- Исправления ошибок

### 1\.7.5764.1

- Повышение производительности.
- Исправления ошибок

### 1\.6.5735.1

- Поддержка локальной системы HDFS для источника и приемника.
- Повышение производительности.
- Исправления ошибок

### 1\.6.5696.1

- Повышение производительности.
- Исправления ошибок

### 1\.6.5676.1

- Поддержка средств диагностики в диспетчере конфигурации.
- Поддержка столбцов источников табличных данных для фабрики данных Azure.
- Поддержка хранилища данных SQL для фабрики данных Azure.
- Поддержка свойства reclusive в BlobSource и FileSource для фабрики данных Azure.
- Поддержка значений MergeFiles, PreserveHierarchy и FlattenHierarchy свойства copyBehavior в BlobSink и FileSink для двоичного копирования в фабрике данных Azure.
- Поддержка сообщения о ходе выполнения действия копирования в фабрике данных Azure.
- Поддержка проверки подключения к источнику данных в фабрике данных Azure.
- Исправления ошибок


### 1\.6.5672.1

- Поддержка задания имени таблицы для источника данных ODBC в фабрике данных Azure
- Повышение производительности.
- Исправления ошибок

### 1\.6.5658.1

- Поддержка FileSink для фабрики данных Azure.
- Поддержка сохранения иерархии при двоичном копировании в фабрике данных Azure.
- Поддержка идемпотентности действия копирования в фабрике данных Azure.
- Исправления ошибок

### 1\.6.5640.1

- Поддержка 3 дополнительных источников данных (ODBC, OData, HDFS) для фабрики данных Azure.
- Поддержка символа кавычек в средстве синтаксического анализа CSV-файлов для фабрики данных Azure.
- Поддержка сжатия (в формат BZIP2).
- Исправления ошибок

### 1\.5.5612.1

- Поддержка 5 реляционных баз данных (MySQL, PostgreSQL, DB2, Teradata и Sybase) для фабрики данных Azure.
- Поддержка сжатия (в форматы GZIP и DEFLATE).
- Повышение производительности.
- Исправления ошибок


### 1\.4.5549.1

- Поддержка источника данных Oracle для фабрики данных Azure.
- Повышение производительности.
- Исправления ошибок

### 1\.4.5492.1

- Внедрение единого двоичного файла, который поддерживает фабрику данных Microsoft Azure и службы Power BI для Office 365.
- Детализация пользовательского интерфейса настройки и процесса регистрации.
- Поддержка входящих и исходящих данных Azure для источника данных SQL Server в фабрике данных Azure.

### 1\.2.5303.1

- 	Устранение проблемы времени ожидания для поддержки подключений к источникам данных, требующих еще больших затрат времени.
 	
### 1\.1.5526.8

- Добавление необходимого компонента при установке – платформы .NET Framework 4.5.1.

### 1\.0.5144.2

- Нет изменений, которые влияют на сценарии фабрики данных Azure.

## Вопросы и ответы

### Почему диспетчер источников данных пытается установить подключение к шлюзу?
Это связано с обеспечением безопасности. Вы можете настроить только локальные источники данных для доступа к облаку в корпоративной сети, а ваши учетные данные недоступны за пределами брандмауэра организации. Убедитесь, что с вашего компьютера можно подключиться к компьютеру, на котором установлен шлюз.

<!---HONumber=AcomDC_0803_2016-->