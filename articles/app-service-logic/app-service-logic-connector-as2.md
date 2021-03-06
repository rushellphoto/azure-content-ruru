<properties 
   pageTitle="Использование соединителя AS2 в приложениях логики | Служба приложений Microsoft Azure" 
   description="Как создать и настроить соединитель AS2 или приложение API и использовать его в приложении логики в службе приложений Azure" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajeshramabathiran" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# Приступая к работе с соединителем AS2: добавление в приложение логики

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Соединитель AS2 позволяет получать и отправлять сообщения по транспортному протоколу AS2 (Applicability Statement 2) между компаниями. Данные безопасно и надежно транспортируются через Интернет. Безопасность обеспечивается с помощью цифровых сертификатов и шифрования.

Соединитель AS2 можно добавить в рабочий бизнес-процесс и обрабатывать данные в составе рабочего процесса «бизнес-бизнес» в приложении логики.

## Триггеры и действия
Триггер запускает новый экземпляр на основе определенного события, например при поступлении сообщения AS2 от партнера. Действие – это результат, например, после получения сообщения AS2 – отправка этого сообщения с помощью AS2.

Соединитель AS2 в приложении логики можно использовать как триггер или действие, он поддерживает данные в форматах JSON и XML. Соединитель AS2 предоставляет следующие триггеры и действия.

триггеры; | Действия
--- | ---
Получение и декодирование | Кодирование и отправка

## Требования для начала работы
Перед использованием соединителем AS2 необходимо создать следующие элементы:

Требование | Описание
--- | ---
Приложение TPM API | Перед созданием соединителя AS2 необходимо создать [соединитель BizTalk TPM][1]. <br/><br/>**Примечание.** Узнайте имя своего приложения API TPM. 
База данных SQL Azure | Хранит элементы B2B, в том числе партнеров, схемы, сертификаты и соглашения. Каждому приложению API B2B требуется своя собственная база данных SQL Azure. <br/><br/>**Примечание.** Скопируйте строку подключения к этой базе данных.<br/><br/>[ Создайте базу данных SQL Azure](../sql-database/sql-database-get-started.md)
Контейнер хранилища BLOB-объектов Azure | Хранит свойства сообщений, если включена архивация AS2. Если архивация сообщений AS2 не требуется, то контейнер хранилища не нужен. <br/><br/>**Примечание.** Если вы включаете архивацию, скопируйте строку подключения к этому хранилищу BLOB-объектов.<br/><br/>[Об учетных записях хранения Azure](../storage/storage-create-storage-account.md).

## Создание соединителя AS2

Соединитель можно создать в приложении логики или непосредственно в Azure Marketplace. Создание соединителя в Marketplace

1. На начальной панели Azure выберите **Marketplace**.
2. Выполните поиск по запросу «Соединитель AS2», выберите его, а затем нажмите **Создать**.
3. Введите имя, план службы приложений и другие свойства.
4. Введите следующие параметры пакета:

	Свойство | Описание
--- | --- 
Строка подключения к базе данных | Введите строку подключения ADO.NET к созданной базе данных SQL Azure. При копировании строки подключения пароль в эту строку подключения не добавляется. Перед вставкой не забудьте указать пароль в строке подключения.
Включить архивацию для входящих сообщений | необязательный параметр. Включите это свойство, чтобы сохранять свойства входящего сообщения AS2, полученного от партнера. 
Строка подключения к хранилищу BLOB-объектов Azure | Введите строку подключения к созданному контейнеру хранилища BLOB-объектов Azure. Если архивация включена, закодированные и декодированные сообщения хранятся в этом контейнере хранилища.
Имя экземпляра TPM | Введите имя приложения API **управления торговыми партнерами BizTalk**, созданного ранее. При создании соединителя AS2 этот соединитель выполняет соглашения AS2 только в этом конкретном экземпляре TPM.

5. Нажмите кнопку **Создать**.

Торговые партнеры — это сущности, участвующие во взаимодействиях между компаниями. Когда два партнера устанавливают связь, это называется соглашением. Определенное соглашение строится на основе взаимодействия, которого желают достичь эти два партнера, и зависит от конкретного протокола или транспорта.

См. действия [по созданию соглашения с торговыми партнерами][2].

## Использование соединителя в качестве триггера

1. Создавая или изменяя приложение логики, выберите на панели справа созданный соединитель AS2: ![Настройки триггера][3]

2. Щелкните стрелку вправо →: ![Параметры триггера][4]

3. Соединитель AS2 предоставляет один триггер. Выберите *Получение и декодирование*: ![Получение и декодирование входных данных][5]

4. Этот триггер не имеет входных данных. Щелкните стрелку вправо →: ![Получение и декодирование настроены][6]

В рамках вывода соединитель возвращает полезные данные AS2 и характерные для AS2 метаданные.

Триггер срабатывает, когда полезными данными AS2 является команда размещения публикации по адресу https://{Host URL}/decode. URL-адрес узла можно найти в параметрах приложения API. Кроме того, в параметрах приложения API вам, возможно, потребуется установить уровень доступа «Общий» (с проверкой подлинности или анонимно).

## Использование соединителя в качестве действия
1. После запуска триггера (или запуска логики вручную) добавьте из области справа созданный соединитель AS2: ![Настройки действия][7]

2. Щелкните стрелку вправо →: ![Список действий][8]

3. Соединитель AS2 поддерживает только одно действие. Выберите *Кодирование и отправка*: ![Кодирование и отправка входных данных][9]

4. Введите входные данные для действия и настройте его: ![Кодирование и отправка настроены][10]

	Параметры:

	Параметр | Тип | Описание
--- | --- | ---
Полезные данные | object| Содержимое полезных данных для кодирования и публикации в настроенной конечной точке. Полезные данные необходимо предоставить в виде объекта JSON.
AS2 из | string | Удостоверение AS2 отправителя сообщения AS2. Этот параметр используется для поиска соответствующего соглашения для отправки сообщения.
AS2 в | string | Удостоверение AS2 получателя сообщения AS2. Этот параметр используется для поиска соответствующего соглашения для отправки сообщения.
URL-адрес партнера | string | Конечная точка участника, которому требуется отправить сообщение.
Включить архивирование | Логическое | Определяет, следует ли архивировать исходящее сообщение.

Это действие возвращает код ответа HTTP 200 при успешном завершении.

## Дополнительные возможности соединителя
Вы можете [архивировать сообщения AS2](app-service-logic-archive-as2-messages.md).

Дополнительные сведения о приложениях логики см. в статье [Что такое приложения логики?](app-service-logic-what-are-logic-apps.md).

>[AZURE.NOTE] Если вы хотите опробовать приложения логики Azure, не регистрируя учетную запись Azure, перейдите на [эту страницу](https://tryappservice.azure.com/?appservice=logic). Там вы сможете быстро создать в службе приложений простое кратковременное приложение логики. Никаких кредитных карт и обязательств.

Справку по API REST Swagger см. в статье [Справочные материалы по соединителям и приложениям API](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Можно также просматривать статистику производительности и управлять безопасностью соединителя. См. статью [Управление встроенными приложениями API и соединителями, а также их мониторинг](app-service-logic-monitor-your-connectors.md).

<!--References -->
[1]: app-service-logic-connector-tpm.md
[2]: app-service-logic-create-a-trading-partner-agreement.md
[3]: ./media/app-service-logic-connector-as2/TriggerSettings.PNG
[4]: ./media/app-service-logic-connector-as2/TriggerOptions.PNG
[5]: ./media/app-service-logic-connector-as2/ReceiveAndDecodeInput.PNG
[6]: ./media/app-service-logic-connector-as2/ReceiveAndDecodeConfigured.PNG
[7]: ./media/app-service-logic-connector-as2/ActionSettings.PNG
[8]: ./media/app-service-logic-connector-as2/ListOfActions.PNG
[9]: ./media/app-service-logic-connector-as2/EncodeAndSendInput.PNG
[10]: ./media/app-service-logic-connector-as2/EncodeAndSendConfigured.PNG

<!---HONumber=AcomDC_0803_2016-->