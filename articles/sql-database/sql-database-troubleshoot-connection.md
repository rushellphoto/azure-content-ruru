<properties
	pageTitle="Ошибка при подключении к базе данных SQL: ";База данных на сервере сейчас недоступна"; | Microsoft Azure"
	description="Устранение ошибки ";База данных на сервере сейчас недоступна"; при подключении приложения к базе данных SQL."
	services="sql-database"
	documentationCenter=""
	authors="dalechen"
	manager="felixwu"
	editor=""
	keywords="ошибка при подключении к базе данных SQL, база данных на сервере сейчас недоступна"/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="daleche"/>

# Ошибка при подключении к базе данных SQL: "База данных на сервере сейчас недоступна".
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Когда приложение подключается к базе данных SQL Azure, появляется следующее сообщение об ошибке:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Это сообщение об ошибке обычно временное (кратковременное).

Эта ошибка возникает, когда база данных Azure перемещается (или перенастраивается) и приложение потеряло подключение к базе данных SQL. События перенастройки базы данных SQL обычно происходят в запланированном случае (например при обновлении программного обеспечения) или в незапланированном случае (например при сбое процесса или балансировке нагрузки). Большей частью события перенастройки кратковременные и должны завершаться в течение 60 секунд максимум. Тем не менее завершение этих событий иногда может занимать больше времени, например когда большая транзакция приводит к длительному восстановлению.

## Порядок устранения временных проблем подключения
1.	Проверьте [панель мониторинга служб Microsoft Azure](https://azure.microsoft.com/status) на наличие каких-либо известных сбоев, произошедших в то время, когда приложение сообщало об ошибках.
2. Приложения, подключающиеся к облачной службе, такой как база данных SQL Azure, должны ожидать периодические события перенастройки и реализовывать логику повторов для обработки этих ошибок, а не отображать их как ошибки приложения для пользователей. Дополнительные сведения и общие стратегии повторов см. в разделе [Временные ошибки](sql-database-connectivity-issues.md) и в статье [Общие сведения о разработке базы данных SQL](sql-database-develop-overview.md). Затем ознакомьтесь с примерами кода в [библиотеке подключений для базы данных SQL и SQL Server](sql-database-libraries.md).
3.	Если база данных близка к исчерпанию доступных ресурсов, может возникать временная проблема подключения. См. раздел [Устранение неполадок производительности](sql-database-troubleshoot-performance.md).
4.	Если проблемы подключения остаются или интервал, во время которого приложение обнаруживает ошибку, превышает 60 секунд, а также если в определенный день такая ошибка возникает многократно, зарегистрируйте запрос на поддержку Azure, нажав **Получить поддержку** на сайте [Поддержка Azure](https://azure.microsoft.com/support/options).

## Дальнейшие действия
- При появлении другой ошибки проанализируйте [сообщение об ошибке](sql-database-develop-error-messages.md), чтобы разобраться в вызвавших ее причинах.
- Если проблема сохраняется, см. указания в статье [Устранение неполадок подключения к базе данных SQL Azure](sql-database-troubleshoot-common-connection-issues.md).

<!---HONumber=AcomDC_0713_2016-->