<properties
	pageTitle="Использование R в HDInsight для настройки кластеров | Microsoft Azure"
	description="Сведения об установке R с помощью действия сценария и использовании R в кластерах HDInsight."
	services="hdinsight"
	documentationCenter=""
	tags="azure-portal"
	authors="mumian"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
    ms.date="06/28/2016"
	ms.author="jgao"/>

# Установка и использование R на кластерах HDInsight Hadoop

Научитесь настраивать кластер HDInsight на основе Windows с R с помощью сценария действия и использовать R в кластерах HDInsight. Предложение [уровня "Премиум"](https://azure.microsoft.com/pricing/details/hdinsight/) для HDInsight включает в себя R Server в составе кластера HDInsight. Это позволяет сценариям R использовать MapReduce и Spark для выполнения распределенных вычислений. Дополнительные сведения см. в статье [Приступая к работе с R Server в HDInsight](hdinsight-hadoop-r-server-get-started.md). Сведения об использовании R с кластером под управлением Linux см. в разделе [Установка и использование R в кластерах HDInsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md).
 
R можно установить в кластере любого типа (Hadoop, Storm, HBase, Spark) в HDInsight в Azure, воспользовавшись *Действием сценария*. Пример скрипта для установки R на кластере HDInsight доступен в большом двоичном объекте хранилища Azure (доступ только для чтения): [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

**Связанные статьи**

- [Установка и использование R на кластерах HDInsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Создание кластеров Hadoop в HDInsight](hdinsight-provision-clusters.md). Общие сведения о создании кластеров HDInsight
- [Настройка кластеров HDInsight с помощью действия сценария][hdinsight-cluster-customize]. Общая информация о настройке кластеров HDInsight с помощью действия сценария
- [Разработка скриптов действия сценария для HDInsight](hdinsight-hadoop-script-actions.md)

## Что такое R

<a href="http://www.r-project.org/" target="_blank">Проект R для статистических вычислений</a> — это язык программирования с открытым исходным кодом и программная среда для статистических вычислений. R предоставляет сотни встраиваемых статистических функций и собственный язык программирования, который сочетает аспекты функционального и объектно-ориентированного программирования. Этот проект также обеспечивает обширные графические возможности. Большинство профессиональных статистиков и ученых, работающих в целом ряде областей, отдает предпочтение программной среде R.

R совместим с хранилищем больших двоичных объектов Azure (WASB). Это дает возможность обрабатывать находящиеся там данные, используя R в HDInsight.

## Установка R

[Пример сценария](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) для установки R в кластере HDInsight доступен в большом двоичном объекте только для чтения в службе хранилища Azure. Этот раздел содержит инструкции по использованию примера скрипта при создании кластера с помощью портала Azure.

> [AZURE.NOTE] Пример сценария был представлен в кластере HDInsight версии 3.1. Дополнительную информацию о версиях кластера HDInsight см. в статье [Новые возможности версий кластеров Hadoop, предоставляемых HDInsight](hdinsight-component-versioning.md).

1. При создании кластера HDInsight на портале щелкните **Необязательная конфигурация**, а затем щелкните **Действия скрипта**.
2. На странице **Действия скрипта** введите следующие значения.

	![Использование действия сценария для настройки кластера](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Использование действия сценария для настройки кластера")

	<table border='1'>
		<tr><th>Свойство</th><th>Значение</th></tr>
		<tr><td>Имя</td>
			<td>Укажите имя для действия сценария, например <b>Установить R</b>.</td></tr>
		<tr><td>URI-адрес сценария</td>
			<td>Укажите универсальный код ресурса для сценария, который вызывается для настройки кластера, например <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i>.</td></tr>
		<tr><td>Тип узла</td>
			<td>Укажите узлы, на которых выполняется сценарий настройки. Можно выбрать одно из трех значений: <b>Все узлы</b>, <b>Только головные узлы</b> или <b>Только рабочие узлы</b>.
		<tr><td>Параметры</td>
			<td>Укажите параметры, если они требуются для сценария. Но сценарий для установки R не требует никаких параметров, поэтому можно оставить это поле пустым.</td></tr>
	</table>

	Можно добавить несколько действий сценария для установки нескольких компонентов в кластере. После добавления скриптов щелкните флажок, чтобы начать создание кластера.

Сценарий также позволяет установить R в HDInsight с помощью Azure PowerShell или пакета SDK для HDInsight .NET. Указания по выполнению этих процедур приведены ниже в этой статье.

## Запуск сценариев R
В этом разделе содержится описание действий для выполнения скрипта R в кластере Hadoop на базе HDInsight.

1. **Установите подключение между удаленным рабочим столом и кластером** — на портале Azure включите удаленный рабочий стол для созданного кластера с установленным R, а затем подключитесь к кластеру. Указания см. в разделе [Подключение к кластерам HDInsight с помощью RDP](hdinsight-administer-use-management-portal.md#rdp).

2. **Откройте консоль R** — установочный файл R размещает на рабочем столе головного узла ссылку на консоль R. Щелкните ее, чтобы открыть консоль R.

3. **Запустите сценарий R** — сценарий R можно запустить непосредственно из консоли R. Для этого необходимо вставить его в консоль, выбрать его и нажать клавишу ВВОД. Ниже приведен пример простого скрипта, который генерирует числа от 1 до 100 и умножает их на 2.

		library(rmr2)
		library(rhdfs)
		ints = to.dfs(1:100)
		calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
		from.dfs(calc)

Первые две строки вызывают библиотеки RHadoop, установленные с R. Последняя строка выводит результаты на консоль. Выходные данные должны выглядеть так:

	[1,]  1 2
	[2,]  2 4
	.
	.
	.
	[98,]  98 196
	[99,]  99 198
	[100,] 100 200


## Установка R с помощью Azure PowerShell

Обратитесь к статье [Настройка кластеров HDInsight с помощью действия сценария](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell). В этом примере показано, как установить Spark с помощью Azure PowerShell. Необходимо изменить сценарий для использования [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## Установка R с помощью пакета SDK для .NET

Обратитесь к статье [Настройка кластеров HDInsight с помощью действия сценария](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell). В этом примере показано, как установить Spark с помощью пакета SDK для .NET. Необходимо изменить сценарий для использования [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).


## См. также

- [Установка и использование R на кластерах HDInsight Hadoop (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [Создание кластеров Hadoop в HDInsight](hdinsight-provision-clusters.md). Общие сведения о создании кластеров HDInsight
- [Настройка кластеров HDInsight с помощью действия сценария][hdinsight-cluster-customize]. Общая информация о настройке кластеров HDInsight с помощью действия сценария
- [Разработка скриптов действия сценария для HDInsight](hdinsight-hadoop-script-actions.md)
- [Установка и использование Spark в HDInsight][hdinsight-install-spark]. Пример действия сценария для установки Spark
- [Установка Giraph в кластерах HDInsight](hdinsight-hadoop-giraph-install.md). Пример действия сценария для установки Giraph
- [Установка Solr в кластерах HDInsight](hdinsight-hadoop-solr-install-linux.md). Пример действия сценария для установки Solr

[powershell-install-configure]: powershell-install-configure.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-apache-spark-jupyter-spark-sql.md

<!---HONumber=AcomDC_0629_2016-->