<properties
   pageTitle="Поддержка клиентов прежних версий хранилища данных SQL для аудита данных | Microsoft Azure"
   description="Сведения о поддержке клиентов прежних версий хранилища данных SQL для аудита данных"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="ronortloff"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/31/2016" 
   ms.author="rortloff;barbkess;sonyama"/>

# Хранилище данных SQL — поддержка клиентов прежних версий для аудита и динамического маскирования данных

> [AZURE.SELECTOR]
- [Обзор безопасности](sql-data-warehouse-overview-manage-security.md)
- [Обнаружение угроз](sql-data-warehouse-security-threat-detection.md)
- [Шифрование (портал)](sql-data-warehouse-encryption-tde.md)
- [Шифрование (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
- [Общие сведения об аудите](sql-data-warehouse-auditing-overview.md)
- [Поддержка клиентов нижнего уровня](sql-data-warehouse-auditing-downlevel-clients.md)


Функции [аудита](sql-data-warehouse-auditing-overview.md) работают в клиентах SQL, которые поддерживают перенаправление TDS.

Любой клиент, который реализует TDS 7.4, также должен поддерживать перенаправление. К исключениям относятся JDBC 4.0, где функция перенаправления поддерживается не полностью, и Tedious для Node.JS, где перенаправление не реализовано.

Для клиентов прежних версий, т. е. которые поддерживают TDS 7.3 и ниже, в строке подключения необходимо изменить полное доменное имя сервера:

Полное доменное имя исходного сервера в строке подключения: <*имя сервера*>.database.windows.net

Измененное полное доменное имя исходного сервера в строке подключения: <*имя сервера*>.database.**secure**.windows.net

Частичный список клиентов прежних версий включает:

- .NET 4.0 и ниже
- ODBC 10.0 и ниже
- JDBC (хотя JDBC поддерживает TDS 7.4, но функция перенаправления TDS поддерживается не полностью)
- Tedious (для Node.JS)

**Примечание.** Описанное выше изменение полного доменного имени сервера можно использовать также для применения политики аудита уровня SQL Server без необходимости настройки в каждой базе данных (временное устранение рисков).

<!---HONumber=AcomDC_0706_2016-->