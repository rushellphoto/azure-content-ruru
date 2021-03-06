<properties
   pageTitle="Руководство по созданию службы данных для Marketplace | Microsoft Azure"
   description="Подробные инструкции по созданию, сертификации и развертыванию службы данных для продажи в Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="06/28/2016"
      ms.author="hascipio; avikova" />

# Руководство по публикации службы данных для Azure Marketplace
После выполнения шага 1, [Создание и регистрация учетной записи](marketplace-publishing-accounts-creation-registration.md), мы рассказали об [Общих нетехнических требованиях](marketplace-publishing-pre-requisites.md) и [Технических требованиях](marketplace-publishing-data-service-creation-prerequisites.md) для службы данных в Azure Marketplace. Рассмотрим процедуру создания предложения службы данных на [портале публикации][link-pubportal] для Azure Marketplace.

## 1\. Войдите на портал публикации.

Откройте страницу [https://publish.windowsazure.com](https://publish.windowsazure.com.).

**При первом входе в портал публикации укажите учетную запись, под которой профиль продавца для вашей компании зарегистрирован в центре разработчиков.** (Впоследствии вы сможете добавить в качестве соадминистратора на портале публикации любого сотрудника своей компании.)

Щелкните плитку **Публикация служб данных**, если это первый вход на портал публикации.

## 2\. Выберите **Службы данных** в меню навигации с левой стороны.

  ![рисунок](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## 3\. Создание новой службы данных

Введите название нового предложения службы данных и щелкните на знак плюса ("+") справа.

  ![рисунок](media/marketplace-publishing-data-service-creation/step-3.png)

## 4\. Просмотрите подменю только что созданной службы данных в меню навигации.

Откройте вкладку **Пошаговое руководство** и ознакомьтесь с действиями, необходимыми для правильной публикации службы данных в Azure Marketplace.

> [AZURE.TIP] В любое время можно перейти по ссылкам на странице "Пошаговое руководство" или воспользоваться вкладками в подменю предложения службы данных слева.

## 5\. Создание нового плана

### Предложения, планы, транзакции.

Каждое предложение может иметь несколько планов, но не менее одного (1). Когда конечные пользователи подписываются на ваше предложение, они подписываются на один из планов предложения. Каждый план определяет порядок применения службы конечными пользователями.

В настоящее время Azure Marketplace поддерживает для служб данных только ежемесячную подписку на основе транзакций, то есть конечные пользователи вносят ежемесячную плату за выбранный план и каждый месяц могут использовать транзакции в количестве, определенном этим планом.

Транзакция обычно определяется как ряд записей, возвращаемых службой данных по запросу. Количество по умолчанию — 100. Число транзакций, выполняемых по запросу, равно числу записей, деленному на 100 и округленному до ближайшего целого числа.

Отслеживанием (измерением) числа транзакций по каждому запросу занимается уровень службы Azure Marketplace.

> [AZURE.IMPORTANT] Конечные пользователи, которые достигли максимального числа транзакций за месяц, блокируются и не смогут работать со службой до окончания месячного цикла подписки.

> План или один из планов может (но не обязательно) включать неограниченное количество транзакций.

### Создание плана.
1. Щелкните **"+"** рядом с командой "Добавить план".

2. Выберите один из вариантов: **Неограниченный** или **Ограниченный**. Если выбран вариант "Ограниченный", укажите число транзакций, доступных по плану в месяц.

    ![рисунок](media/marketplace-publishing-data-service-creation/step-5.1.png)

    Портал публикации также предложит "Идентификатор плана", который будет использоваться в интерфейсе, а также в службе Marketplace для идентификации плана. При желании "Идентификатор плана" можно изменить.

    > [AZURE.NOTE] "Идентификатор плана" должен быть уникальным в рамках каждого предложения. Как и многие другие идентификаторы, используемые на портале публикации, "Идентификатор плана" будет заблокирован после первой публикации в рабочей среде, и вы больше не сможете изменить его.

3. Щелкните, чтобы подтвердить выбор.

4. Затем вам будет предложено ответить на несколько дополнительных вопросов о новом плане.

    ![рисунок](media/marketplace-publishing-data-service-creation/step-5.2.png)


|Вопрос|Значимость|
|----|----|
|**Этот план бесплатный и доступен во всем мире?**|Можно создать полностью бесплатный план. Если это единственный план для данного предложения, значит, вы публикуете в Marketplace "Бесплатное предложение". Если плата не взимается только в одном (из нескольких) плане, вы можете дать своим конечным пользователям возможность познакомиться со своей службой, предоставив бесплатно относительно небольшой объем транзакций в месяц. Если вы ответите "Да", дополнительных вопросов не будет.|

> [AZURE.NOTE] Конечные пользователи всегда могут перейти на платные планы.

|Вопрос|Значимость|
|----|----|
|**Предусмотрена ли бесплатная пробная версия?**|Можно выбрать ответ "Без пробной версии" или указать возможность пробного использования плана в течение "Одного месяца". Издатели часто используют этот вариант, чтобы предоставить конечным пользователям возможность узнать о преимуществах их предложений в течение месяца бесплатной подписки.|

> [AZURE.IMPORTANT] Конечные пользователи могут воспользоваться бесплатной пробной версией, только если они задали платежный инструмент, например, кредитную карту, соглашению Enterprise.

> Через месяц пробного использования Azure Marketplace начнет выставлять счета с даты оформления подписки, если только клиент не начал процедуру отмены подписки. Никаких специальных уведомлений конечным пользователям не отправляется.

|Вопрос|Значимость|
|----|----|
|**Требуется ли промокод для приобретения этого плана?**| Издатель может ограничить доступ к своим планам обслуживания, предоставляя специальный код ("промокод") некоторым клиентам. В этом случае подписаться на план смогут только пользователи, имеющие этот промокод. Если вы выберете "Нет", то вы подтверждаете, что любой человек из региона, где доступно предложение (дополнительные сведения см. в [руководстве по маркетинговому содержимому Marketplace](marketplace-publishing-push-to-staging.md)), сможет подписаться на этот план. Дополнительных вопросов больше не будет.|
|**Скрыть этот план от всех, у кого нет действующего промокода?**|Если на предыдущий вопрос был дан ответ "Да", издатель может полностью удалить этот план из интерфейса Marketplace. Это означает, что клиенты не будут видеть этот план на странице сведений о предложении. Конечные пользователи, получившие промокод для покупки этого плана, смогут подписаться на него с помощью этого промокода.|

## 6\. Создание маркетинговых материалов Marketplace
Сведения об указании данных на вкладках **"Маркетинг", "Цены, "Поддержка" и "Категории"** см. в [руководстве по маркетинговому содержимому Marketplace](marketplace-publishing-push-to-staging.md), которое применяется ко всем артефактам, опубликованным в Azure Marketplace.

## 7\. Подключение вашего предложения к службе (на основе SQL Azure или веб-службы)

Откройте подменю **Службы данных**.

В верхней половине страницы вам будет предложено указать **Пространство имен** предложения.

  ![рисунок](media/marketplace-publishing-data-service-creation/step-7.png)

Ответ на вопрос ниже определяет, как издатель будет выставлять новое предложение в Azure Marketplace. (Дополнительные сведения см. в статье [Руководство по техническим условиям для служб данных](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![рисунок](media/marketplace-publishing-data-service-creation/step-7.2.png)

**Публикация службы на основе базы данных**

Щелкните **База данных**. Откроется следующая страница.

  ![рисунок](media/marketplace-publishing-data-service-creation/step-7.3.png)

Чтобы создать сопоставление CSDL для набора данных, основанного на базе данных SQL Azure, укажите следующие параметры.

  ![рисунок](media/marketplace-publishing-data-service-creation/step-7.4.png)

А затем для каждой таблицы

  ![рисунок](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![рисунок](media/marketplace-publishing-data-service-creation/step-7.6.png)

Для веб-службы укажите следующее.

  ![рисунок](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [AZURE.IMPORTANT] Подробные инструкции и примеры для создания веб-службы CSDL см. в статье [Сопоставление существующей веб-службы с OData через CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) .

## Дальнейшие действия
После создания предложения службы данных выполните инструкции в [руководстве по маркетинговому содержимому Marketplace](marketplace-publishing-push-to-staging.md) перед тем, как перейти к [Тестированию службы данных в промежуточной среде](marketplace-publishing-data-service-test-in-staging.md).

## См. также
- [Приступая к работе: как опубликовать предложение в Azure Marketplace](marketplace-publishing-getting-started.md)
- Если вы хотите понять процесс и смысл сопоставления OData, см. статью [Сопоставление OData для службы данных](marketplace-publishing-data-service-creation-odata-mapping.md), где приводятся определения, структуры и инструкции.
- Если вы заинтересованы в изучении и понимании конкретных узлов и их параметров, в статье [Узлы сопоставления службы данных OData](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) вы найдете определения и объяснения, примеры и контекст вариантов использования.
- Если вы хотите поработать с примерами, см. статью [Примеры сопоставления OData для службы данных](marketplace-publishing-data-service-creation-odata-mapping-examples.md), где приводятся примеры кода вместе с объяснением синтаксиса и контекста.


[link-pubportal]: https://publish.windowsazure.com

<!---HONumber=AcomDC_0706_2016-->