<properties
	pageTitle="Создание службы поиска Azure с помощью портала Azure | Microsoft Azure | Размещенная облачная служба поиска"
	description="Научитесь подготавливать службу поиска Azure с помощью портала Azure."
	services="search"
	authors="ashmaka"
	documentationCenter=""/>

<tags
	ms.service="search"
	ms.devlang="NA"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="07/13/2016"
	ms.author="ashmaka"/>

# Создание службы поиска Azure с помощью портала Azure

Эта статья поможет вам создать (или подготовить) индекс службы поиска Azure с помощью [портала Azure](https://portal.azure.com/).

В этом руководстве предполагается, что у вас уже есть подписка Azure и вы можете войти на портал Azure.

## Найдите службу поиска Azure на портале Azure
1. Перейдите на [портал Azure](https://portal.azure.com/) и войдите в систему.
1. Нажмите на значок "плюс" ("+") в верхнем левом углу.
2. Выберите **Данные+хранилище**.
3. Выберите **Поиск Azure**.

![](./media/search-create-service-portal/find-search.png)

## Выберите имя службы и URL-адрес конечной точки для службы
1. Имя службы будет частью URL-адреса конечной точки службы поиска Azure. К этой конечной точке будут выполняться вызовы API для использования службы поиска и управления ею.
2. Введите имя службы в поле **URL-адрес**. Имя службы:
  * должно содержать только строчные буквы, цифры или дефисы ("-");
  * не может содержать дефис ("-") в первых двух символах или в последнем символе;
  * не может содержать последовательные дефисы ("--");
  * может иметь длину от 2 до 60 символов.


## Выберите подписку, где будет храниться служба
Если у вас несколько подписок, вы можете выбрать ту, которая будет включать эту службу поиска Azure.

## Выберите группу ресурсов для своей службы
Создайте новую группу ресурсов или выберите существующую. Группы ресурсов — это коллекция служб и ресурсов Azure, которые используются совместно. Например, если вы используете службу поиска Azure для индексации базы данных SQL, то обе службы должны входить в одну группу ресурсов.

## Выберите расположение, в котором будут размещаться службы
Как и служба Azure, служба поиска Azure может размещаться в центрах обработки данных по всему миру. Обратите внимание, что [цены могут отличаться](https://azure.microsoft.com/pricing/details/search/) в зависимости от региона.

## Выберите ценовую категорию
[Служба поиска Azure сейчас предлагается в нескольких ценовых категориях](https://azure.microsoft.com/pricing/details/search/): "Бесплатный", "Базовый" и "Стандартный". Каждая категория отличается собственным [объемом и ограничениями](search-limits-quotas-capacity.md). Подробные сведения см. в статье [Выбор SKU или уровня для службы поиска Azure](search-sku-tier.md).

В данном случае мы выбрали уровень Стандартный.

## Нажмите кнопку "Создать", чтобы подготовить службу

![](./media/search-create-service-portal/create-service.png)

## Выполните масштабирование службы

Подготовив службу, вы можете выполнить ее масштабирование в соответствии со своими потребностями. При выборе уровня Стандартный для службы поиска Azure вы сможете масштабировать свою службу в двух измерениях: репликах и разделах. Если вы выбрали уровень Базовый, то сможете добавлять только реплики.

*__Разделы__* позволяют службе хранить данные и осуществлять поиск в большем количестве документов.

*__Реплики__* позволяют службе обрабатывать более высокую нагрузку запросов поиска — [службе необходимы две реплики для достижения соглашения об уровне обслуживания только для чтения и три реплики для соглашения об уровне обслуживания для чтения и записи](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

1. Перейдите в колонку управления службы поиска Azure на портале Azure.
2. В колонке **Параметры** выберите **Масштабирование**.
3. Службу можно масштабировать, добавив реплики или разделы.
  * Службу нельзя масштабировать более чем на 36 единиц поиска. Общее количество единиц поиска — это произведение количества реплик и количество разделов (количество реплик * количество разделов = общее количество единиц поиска).
  * Если вы выбрали уровень Базовый, то сможете выполнить масштабирование не более чем на 3 реплики. Службы уровня Базовый привязаны к одному разделу.

![](./media/search-create-service-portal/scale-service.png)

## Далее
Подготовив службу поиска Azure, вы сможете [определить индекс поиска Azure](search-what-is-an-index.md), чтобы отправить свои данные и осуществлять поиск в них.

Краткие инструкции см. в статье [Начало работы со службой поиска Azure на портале](search-get-started-portal.md).

<!---HONumber=AcomDC_0713_2016-->