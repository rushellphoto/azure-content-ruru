<properties
	pageTitle="Профили оценки (REST API службы Поиска Azure, версия 2015-02-28-Preview) | Microsoft Azure | API Поиска Azure (предварительная версия)"
	description="Поиск Azure — это размещенная облачная служба поиска, которая поддерживает настройку ранжированных результатов на основе определяемых пользователем профилей повышения."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="mblythe"
	editor=""/>

<tags
	ms.service="search"
	ms.devlang="rest-api"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.author="heidist"
	ms.date="05/18/2016" />

# Профили оценки (API REST службы "Поиск Azure", версия 2015-02-28-Preview)

> [AZURE.NOTE] В этой статье описываются профили оценки, доступные в версии [2015-02-28-Preview](search-api-2015-02-28-preview.md). Сейчас нет различий между версией `2015-02-28`, описанной на сайте [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx), и версией `2015-02-28-Preview`, описанной в этой статье. Однако мы предлагаем этот документ, чтобы обеспечить полную документацию по API.

## Обзор

Оценка — это вычисление оценки поиска для всех элементов, возвращенных в результатах поиска. Это показатель релевантности элемента в контексте текущей операции поиска. Чем выше оценка, тем более релевантен элемент. Элементы в результатах поиска упорядочиваются по рангу (от наивысшего до наименьшего) на основе оценок поиска, вычисленных для каждого элемента.

В службе "Поиск Azure" используется метод начальной оценки по умолчанию, но вы можете настроить вычисление с помощью профиля оценки. Профили оценки позволяют управлять приоритетом элементов в результатах поиска. Например, может потребоваться повысить приоритет элементов на основе потенциального дохода, повысить уровень более новых элементов или, возможно, элементов, которые слишком долго находились на складе.

Профиль оценки — часть определения индекса, состоящая из полей, функций и параметров.

В следующем примере показан простой профиль geo, который поможет вам получить представление о профиле оценки. Он повышает приоритет элементов, содержащих условие поиска в поле `hotelName`. Он также использует функцию `distance` для выбора элементов, которые находятся в пределах десяти километров от текущего расположения. Если кто-то ищет слово inn, которое входит в название отеля, документы, содержащие слово inn, будут показаны в результатах поиска выше.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

Чтобы использовать этот профиль оценки, в строке запроса необходимо указать профиль. В следующем запросе обратите внимание на параметр `scoringProfile=geo`.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Этот запрос выполняет поиск слова inn и передает текущее расположение. Обратите внимание, что этот запрос содержит другие параметры, такие как `scoringParameter`. Параметры запроса описаны в [разделе, в котором рассматривается поиск документов (API службы "Поиск Azure")](search-api-2015-02-28-preview/#SearchDocs).

Щелкните [Пример](#example), чтобы просмотреть более подробный пример профиля оценки.

## Что такое оценка по умолчанию?

В процессе оценки вычисляется оценка поиска для каждого элемента в упорядоченном по рангу результирующем наборе. Каждому элементу в результирующем наборе поиска назначается оценка поиска (от наивысшей до наименьшей). Элементы с более высокой оценкой возвращаются приложению. По умолчанию возвращаются первые 50 элементов, но можно использовать параметр `$top`, чтобы вернуть меньше или больше элементов (до 1000 в одном ответе).

По умолчанию оценка поиска вычисляется на основе статистических свойств данных и запроса. Поиск Azure находит документы, содержащие условия поиска в строке запроса (некоторые или все в зависимости от `searchMode`), отдавая предпочтение документам, которые содержат несколько экземпляров условия поиска. Оценка поиска возрастает, если условие поиска редко встречается в совокупности данных, но часто — внутри документа. Основу для такого подхода к вычислению релевантности называют TF-IDF (частота условия — инверсная частота в документе).

Если предположить, что пользовательская сортировка не применяется, результаты ранжируются по оценке поиска, прежде чем они будут возвращены вызывающему приложению. Если параметр `$top` не указан, возвращаются 50 элементов с наивысшей оценкой поиска.

Значения оценки поиска в результирующем наборе могут повторяться. Например, может быть 10 элементов с оценкой 1,2, 20 элементов с оценкой 1,0 и 20 элементов с оценкой 0,5. Если у нескольких совпадений одинаковая оценка поиска, порядок сортировки элементов не определен и не является стабильным. Выполните запрос еще раз, и, возможно, позиции элементов изменятся. Если есть два элемента с одинаковой оценкой, невозможно определить, какой из них отобразится первым.

## Когда следует использовать пользовательские оценки

Вам следует создать один или несколько профилей оценки, если ранжирование по умолчанию не соответствует вашим бизнес-целям. Например, вы можете решить, что при поиске следует предпочитать вновь добавленные элементы. Аналогично, у вас может быть поле, содержащее удельную прибыль, или другое поле, указывающее потенциальный доход. Повышение приоритета элементов, которые дают преимущества для бизнеса, может быть важным фактором при решении об использовании профилей оценки.

Упорядочивание на основе релевантности также реализуется с помощью профилей оценки. Рассмотрим страницы результатов поиска, которые вы использовали ранее, позволяющие сортировать данные по цене, дате, рейтингу или релевантности. В службе "Поиск Azure" профили оценки предоставляют параметр "релевантности". Определение релевантности контролируете вы в соответствии с бизнес-целями и типом результатов поиска, которые хотите предоставить.

<a name="example"></a>
## Пример

Как отмечалось ранее, пользовательская оценка реализуется с профилей оценки, определенных в схеме индекса.

В этом примере показана схема индекса с двумя профилями оценки (`boostGenre`, `newAndHighlyRated`). Любой запрос к этому индексу, включающий профиль в качестве параметра запроса, будет использовать профиль для оценки результирующего набора.

[Ознакомьтесь с этим примером](search-get-started-scoring-profiles.md).

    {
      "name": "musicstoreindex",
	  "fields": [
	    { "name": "key", "type": "Edm.String", "key": true },
	    { "name": "albumTitle", "type": "Edm.String" },
	    { "name": "albumUrl", "type": "Edm.String", "filterable": false },
	    { "name": "genre", "type": "Edm.String" },
	    { "name": "genreDescription", "type": "Edm.String", "filterable": false },
	    { "name": "artistName", "type": "Edm.String" },
	    { "name": "orderableOnline", "type": "Edm.Boolean" },
	    { "name": "rating", "type": "Edm.Int32" },
	    { "name": "tags", "type": "Collection(Edm.String)" },
	    { "name": "price", "type": "Edm.Double", "filterable": false },
	    { "name": "margin", "type": "Edm.Int32", "retrievable": false },
	    { "name": "inventory", "type": "Edm.Int32" },
	    { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
	  ],
      "scoringProfiles": [
        {
	      "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
	    },
        {
	      "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## Рабочий процесс

Чтобы реализовать пользовательское поведение оценки, добавьте профиль оценки в схему, которая определяет индекс. В индексе может быть несколько профилей оценки, но в одном запросе можно указать только один профиль.

Начните с [шаблона](#bkmk_template), приведенного в этой статье.

Введите имя. Профили оценки не являются обязательными, но если вы добавите профиль, необходимо указать имя. Следуйте соглашениям об именовании полей (начинается с буквы, не содержит специальных символов и зарезервированных слов). Правила именования приведены в статье [Правила именования](http://msdn.microsoft.com/library/azure/dn857353.aspx).

Текст профиля оценки состоит из взвешенных полей и функций.

### Weights ###

Свойство `weights` профиля повышения содержит пары "имя–значение", которые назначают полю относительный вес. В [примере](#example) приоритет полей albumTitle, genre и artistName повышается на 1,5, 5 и 2 позиции соответственно. Почему для поля genre приоритет повышен намного больше, чем для других полей? Если поиск выполняется в однородных данных (как в случае с genre в `musicstoreindex`), вам может потребоваться большая вариативность в относительных весах. Например, в `musicstoreindex` «рок» появляется как жанр и в идентично сформулированных описаниях жанра. Если жанр должен обладать большим весом, чем описание жанра, полю genre потребуется назначить гораздо больший относительный вес.

### Functions ###

Функции используются, когда необходимы дополнительные вычисления для определенных контекстов. Допустимые типы функций: `freshness`, `magnitude`, `distance` и `tag`. Каждая функция имеет свои уникальные параметры.

  - `freshness` следует использовать, если вам необходимо повысить приоритет нового или старого элемента в зависимости от его "возраста". Эта функция может использоваться только с полями типа "дата и время" (`Edm.DataTimeOffset`). Обратите внимание, что атрибут `boostingDuration` используется только с функцией freshness.
  - `magnitude` следует применять, когда требуется повысить приоритет элементов на основе числового значения. К сценариям, в которых может вызваться эта функция, относится повышение приоритета на основе прибыли, наибольшей цены, наименьшей цены или числа загрузок. Можно задать обратный диапазон (от высоких оценок до низких), если результаты нужно отобразить в обратном порядке (например, повысить приоритет для элементов с более низкими ценами по сравнению с элементами с более высокими ценами). Если у вас есть диапазон цен от 100 до 1 доллара США, для параметров `boostingRangeStart` и `boostingRangeEnd` следует установить значения 100 и 1 соответственно, чтобы повысить приоритет для элементов с более низкими ценами. Эта функция может использоваться только с полями типа double и integer.
  - `distance` следует применять, чтобы повысить приоритет по близости или географическому расположению. Эта функция может использоваться только с полями типа `Edm.GeographyPoint`.
  - `tag` следует применять, чтобы повысить приоритет по тегам, которые одновременно используются в документах и поисковых запросах. Эта функция может использоваться только с полями `Edm.String` и `Collection(Edm.String)`.
  
#### Правила использования функции ####

  - Тип функции (freshness, magnitude, distance, tag) необходимо указывать в нижнем регистре.
  - Функции не могут содержать значения NULL или пустые значения. В частности, если добавить поле fieldname, для него необходимо указать значение.
  - Функции можно применять только к фильтруемым полям. Подробнее о фильтруемых полях см. в разделе [Создание индекса](search-api-2015-02-28/#createindex).
  - Функции могут применяться только к полям, определенным в коллекции полей индекса.

После определения индекса выполните построение индекса, загрузив схему индекса и документы. Инструкции по этим операциям приведены в разделах [Создание индекса](search-api-2015-02-28-preview/#createindex) и [Добавление или обновление документов](search-api-2015-02-28-preview/#AddOrUpdateDocuments). После построения индекса вы получите функциональный профиль оценки, который работает с данными поиска.

<a name="bkmk_template"></a>
## Шаблон
В этом разделе показан синтаксис и шаблон для профилей оценки. Описание атрибутов см. в подразделе [Справочник по атрибутам индекса](#bkmk_indexref) в следующем разделе.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies to searchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)
              "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>
## Справочник по свойствам профиля повышения

**Примечание**. Функция оценки может применяться только к фильтруемым полям.

| Свойство | Описание |
|----------|-------------|
| `name` | обязательный параметр. Это имя профиля оценки. К нему применяются те же соглашения об именовании, что и к полям. Оно должно начинаться с буквы, не может содержать точки, двоеточия и символы "@", а также не может начинаться с фразы azureSearch (с учетом регистра). |
| `text` | Содержит свойство Weights (Весовые коэффициенты). |
| `weights` | необязательный параметр. Пара "имя-значение", указывающая имя поля и относительный вес. Относительный вес должен быть положительным целым числом или числом с плавающей запятой. Можно указать имя поля без соответствующего веса. Весовые коэффициенты используются для указания важности одного поля относительно другого. |
| `functions` | необязательный параметр. Учтите, что функция оценки может применяться только к фильтруемым полям. |
| `type` | Требуется для функций оценки. Указывает тип используемой функции. Допустимые значения: `magnitude`, `freshness`, `distance` и `tag`. В каждый профиль оценки можно включить несколько функций. Имя функции должно состоять из строчных букв. |
| `boost` | Требуется для функций оценки. Положительное число, используемое в качестве множителя для необработанной оценки. Не может быть равно 1. |
| `fieldName` | Требуется для функций оценки. Функция оценки может применяться только к фильтруемым полям, входящим в коллекцию полей индекса. Кроме того, каждый тип функции накладывает дополнительные ограничения (freshness используется с полями типа datetime, magnitude — с полями типа integer или double, distance — с полями типа location, а tag — с полями типа string или string collection). В определении функции можно указать только одно поле. Например, чтобы использовать функцию magnitude дважды в одном профиле, необходимо добавить два определения magnitude (по одному для каждого поля). |
| `interpolation` | Требуется для функций оценки. Определяет наклон для повышения приоритета оценки от начала до конца диапазона. Допустимые значения: `linear` (по умолчанию), `constant`, `quadratic` и `logarithmic`. Подробные сведения см. в разделе [Настройка интерполяций](#bkmk_interpolation). |
| `magnitude` | Функция оценки magnitude используется для изменения рейтинга на основе диапазона значений для числового поля. Ниже приведены некоторые наиболее распространенные примеры.<ul><li>Оценка по звездам: изменение оценки на основе значения в поле "Оценка по звездам". Если релевантны два элемента, первым отобразится элемент с более высокой оценкой.</li><li>Маржа: если релевантны два документа, продавец может повышать приоритет документов с более высокой маржой.</li><li>Число нажатий: для приложений, которые отслеживают переходы к продуктам или страницам, можно использовать функцию magnitude для повышения приоритета элементов, которые могут привлекать больше всего трафика.</li><li>Число загрузок: для приложений, которые отслеживают загрузки, функция magnitude позволяет повысить приоритет элементов с наибольшим числом загрузок.</li></ul> |
| `magnitude:boostingRangeStart` | Задает начальное значение диапазона, на основе которого вычисляется результат функции magnitude. Значение должно быть целым числом или числом с плавающей запятой. Для оценок по звездам от 1 до 4 — 1. Для маржи более 50 % — 50. |
| `magnitude:boostingRangeEnd` | Задает конечное значение диапазона, на основе которого вычисляется результат функции magnitude. Значение должно быть целым числом или числом с плавающей запятой. Для оценок по звездам от 1 до 4 — 4. |
| `magnitude:constantBoostBeyondRange` | Допустимые значения: true или false (по умолчанию). Если задано значение true, к документам, содержащим значение целевого поля, превышающее верхнее ограничение диапазона, будет применяться максимальное повышение приоритета. Если задано значение false, эта функция не будет применяться к документам, содержащим значение целевого поля, которое не входит в диапазон. |
| `freshness` | Функция оценки freshness используется для оценки ранжирования элементов на основе значений в полях DateTimeOffset. Например, элемент с более близкой датой может классифицироваться выше, чем старые элементы. (Обратите внимание, что ранжировать можно и такие элементы, как события календаря с будущими датами, в случае чего события, более близкие к текущей дате, получат более высокий ранг, чем события в будущем.) В текущем выпуске службы один конец диапазона фиксируется на текущее время. Другой конец находится в прошлом и определяется параметром `boostingDuration`. Чтобы повысить приоритет диапазона будущего времени, используйте отрицательное значение параметра `boostingDuration`. Скорость повышения приоритета от максимального и минимального значений диапазона определяется интерполяцией, которая применяется к профилю оценки (см. рисунок ниже). Чтобы изменить направление коэффициента повышения приоритета, выберите коэффициент повышения приоритета меньше 1. |
| `freshness:boostingDuration` | Задает срок действия, после которого повышение приоритета перестает применяться для определенного документа. Синтаксис и примеры приведены далее в подразделе [Настройка boostingDuration][#bkmk\_boostdur]. |
| `distance` | Функция оценки distance используется, чтобы изменить оценку документов на основе их близости к эталонному географическому расположению. Эталонное расположение предоставляется как часть запроса в параметре (с помощью параметра запроса `scoringParameter`) в аргументе lon,lat. |
| `distance:referencePointParameter` | Параметр, который передается в запросах и используется в качестве эталонного расположения. scoringParameter — это параметр запроса. Описание параметров запроса см. в разделе о [поиске документов](search-api-2015-02-28-preview/#SearchDocs). |
| `distance:boostingDistance` | Число, которое задает расстояние в километрах от эталонного расположения, где заканчивается диапазон повышения приоритета. |
| `tag` | Функция оценки tag определяет оценку документов на основе тегов, имеющихся в документах и поисковых запросах. Документы, в которых имеются общие с поисковыми запросами теги, получают более высокий приоритет. Теги для каждого поискового запроса указываются в качестве параметра оценки (с помощью параметра запроса `scoringParameter`). |
| `tag:tagsParameter` | Параметр, который передается в запросах для указания тегов определенного запроса. `scoringParameter` — это параметр запроса. Описание параметров запроса см. в разделе о [поиске документов](search-api-2015-02-28-preview/#SearchDocs). |
| `functionAggregation` | необязательный параметр. Применяется, только если указаны функции. Допустимые значения: `sum` (по умолчанию), `average`, `minimum`, `maximum` и `firstMatching`. Оценка поиска — это одиночное значение, которое вычисляется на основе нескольких переменных, включая несколько функций. Эти атрибуты показывают, как значения повышения приоритета всех функций объединяются в единое агрегированное повышение приоритета, которое затем применяется к базовой оценке документа. Базовая оценка основана на значении tf-idf, которое вычисляется на основе документа и поискового запроса. |
| `defaultScoringProfile` | Если при выполнении поискового запроса профиль оценки не указан, используется оценка по умолчанию (только tf-idf). Здесь можно указать имя профиля оценки по умолчанию, после чего служба "Поиск Azure" будет использовать его, если в поисковом запросе на задан другой профиль. |

<a name="bkmk_interpolation"></a>
## Настройка интерполяций

Интерполяции позволяют определить наклон для повышения приоритета оценки от начала до конца диапазона. Можно использовать следующие типы интерполяции.

  - `Linear`: для элементов, которые находятся в диапазоне от минимального до максимального значения, применяется постоянно убывающая величина. Линейная интерполяция — это интерполяция для оценки профиля по умолчанию.
  - `Constant`: для элементов, которые находятся между начальным и конечным диапазонами, к результатам ранжирования применяется постоянное повышение приоритета.
  - `Quadratic`: по сравнению с линейной интерполяцией с постоянно убывающим повышением приоритета квадратичная интерполяционная функция сначала уменьшается не так быстро, а по мере приближения к конечному диапазону уменьшается на гораздо большую величину. Этот тип интерполяции невозможно использовать в функциях оценки tag.
  - `Logarithmic`: по сравнению с линейной интерполяцией с постоянно убывающим повышением приоритета логарифмическая интерполяционная функция сначала уменьшается быстрее, а по мере приближения к конечному диапазону уменьшается на гораздо меньшую величину. Этот тип интерполяции невозможно использовать в функциях оценки tag.

<a name="Figure1"></a> ![][1]

<a name="bkmk_boostdur"></a>
## Настройка boostingDuration

`boostingDuration` — это атрибут функции freshness. Он задает срок действия, после которого повышение приоритета перестает применяться для определенного документа. Например, для повышения приоритета линейки продуктов или торговой марки в течение 10-дневного периода следует указать для соответствующих документов 10-дневный период как "P10D". А чтобы повысить приоритет событий, предстоящих на следующей неделе, укажите значение -P7D.

`boostingDuration` необходимо отформатировать как XSD-значение dayTimeDuration (ограниченное подмножество значения длительности ISO 8601). Используется следующий шаблон: `][-]P\[nD]\[T\[nH]\[nM]\[nS]\]]`.

Следующая таблица содержит несколько примеров.

| Длительность | boostingDuration |
|----------|------------------|
| 1 день | "P1D" |
| 2 дня и 12 часов | "P2DT12H" |
| 15 минут | "PT15M" |
| 30 дней, 5 часов, 10 минут и 6,334 секунды | "P30DT5H10M6.334S" |

Дополнительные примеры см. в [документе, в котором рассматриваются типы данных в схеме XML (веб-сайт W3.org)](http://www.w3.org/TR/xmlschema11-2/).

**Дополнительные материалы** [API REST службы поиска Azure](http://msdn.microsoft.com/library/azure/dn798935.aspx) на сайте MSDN <br/> [Создание индекса (API REST службы поиска Azure)](http://msdn.microsoft.com/library/azure/dn798941.aspx) на сайте MSDN<br/> [Добавление профилей оценки в индекс поиска](http://msdn.microsoft.com/library/azure/dn798928.aspx) на сайте MSDN<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png

<!---HONumber=AcomDC_0525_2016-->