<properties
	pageTitle="Использование пакета SDK для устройств Azure IoT для C | Microsoft Azure"
	description="Основные понятия и начало работы с примерами кода из пакета SDK для устройств Azure IoT для C."
	services="iot-hub"
	documentationCenter=""
	authors="olivierbloch"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="03/29/2016"
     ms.author="obloch"/>

# Знакомство с пакетом SDK для устройств Azure IoT для C

**Пакет SDK для устройств Azure IoT** — это набор библиотек, предназначенных для упрощения процесса отправки событий и получения сообщений из **центра Azure IoT**. Существуют различные варианты пакетов SDK, предназначенные для разных платформ. В этой статье описывается **пакет SDK для устройств Azure IoT для C**.

Пакет SDK для устройств Azure IoT для C написан на языке ANSI C (C99), что обеспечивает максимальную переносимость. Таким образом, он подходит для использования с разными платформами и устройствами, особенно в условиях ограниченных ресурсов (дискового пространства и оперативной памяти).

Этот пакет SDK протестирован на множестве платформ (подробные сведения см. в [статье о платформах и совместимости](iot-hub-tested-configurations.md)). В этой статье приведены пошаговые инструкции с примерами кода, выполняемого на платформе Windows. Для других поддерживаемых платформ код будет аналогичным.

Из этой статьи вы узнаете об архитектуре пакета SDK для устройств Azure IoT для C. Мы покажем, как инициализировать библиотеку, отправлять события в центр IoT и получать сообщения из него. Приведенной здесь информации достаточно, чтобы вы могли начать работу с пакетом SDK. Также вы найдете здесь ссылки для получения дополнительных сведений о библиотеках.

>> [AZURE.NOTE] В этой статье нет сведений об использовании возможностей *управления устройствами* библиотек C из пакета SDK. Чтобы узнать, как использовать возможности управления устройствами, см. раздел [Знакомство с клиентской библиотекой управления устройствами для центра Azure IoT](iot-hub-device-management-library.md).

## Архитектура пакета SDK

**Пакет SDK для устройств Azure IoT для C** доступен в репозитории [пакетов SDK Microsoft Azure IoT](https://github.com/Azure/azure-iot-sdks) на сайте GitHub, а информацию об API см. в [документации по API для C](http://azure.github.io/azure-iot-sdks/c/api_reference/index.html).

Последнюю версию библиотеки можно найти в ветке **master** этого репозитория:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

Этот репозиторий содержит все семейство пакетов SDK для устройств Azure IoT. Но эта статья описывает пакет SDK для устройств Azure IoT *для C*, который можно найти в папке **c**.

  ![](media/iot-hub-device-sdk-c-intro/02-CFolder.PNG)

* Реализация ядра пакета SDK находится в папке **iothub\_client**, где содержится реализация самого нижнего уровня API в пакете SDK — библиотека **IoTHubClient**. Библиотека **IoTHubClient** содержит API-интерфейсы, реализующие обмен необработанными сообщениями для отправки сообщений в центр IoT, а также получения сообщений из него. Если вы используете эту библиотеку, вам нужно самостоятельно реализовать сериализацию сообщений (используя пример сериализатора, описанный ниже). Другие составляющие процесса взаимодействия с центром IoT реализуются автоматически.
* Папка **serializer** содержит вспомогательные функции и примеры, демонстрирующие сериализацию данных перед их отправкой в центр IoT Azure с помощью клиентской библиотеки. Обратите внимание, что сериализатор не обязателен для использования и предоставляется только для удобства. Если вы используете библиотеку **serializer**, начните с определения модели, которая описывает события, отправляемые в центр IoT, а также сообщения, получаемые из него. После определения модели пакет SDK предоставит поверхность API, которая упрощает работу с событиями и сообщениями и избавляет вас от необходимости выполнять задачи, связанные с сериализацией. Библиотека основана на других библиотеках с открытым исходным кодом, в которых реализована транспортировка данных по ряду протоколов (AMQP, MQTT).
* Библиотека **IoTHubClient** зависит от других библиотек с открытым кодом:
   * [общей библиотеки Azure C](https://github.com/Azure/azure-c-shared-utility), которая предоставляет основные функции для решения базовых задач (включая манипуляции со строками и списками, ввод и вывод данных и т. д.), требуемых в различных связанных с Azure пакетах SDK для C;
   * библиотеки [Azure uAMQP](https://github.com/Azure/azure-uamqp-c), которая представляет собой реализацию AMQP на стороне клиента, оптимизированную для устройств с ограниченными ресурсами;
   * [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) — библиотеки общего назначения, реализующей протокол MQTT и оптимизированной для устройств с ограниченными ресурсами.

Все это проще понять, взглянув на примеры кода. Следующие разделы содержат разбор нескольких примеров приложений, включенных в пакет SDK. Ознакомившись с ними, вы получите представление о разных возможностях архитектурных уровней пакета и о работе с API.

## Перед запуском примеров

Прежде чем выполнять примеры кода в пакете SDK устройств Azure IoT для C, необходимо создать экземпляр службы в подписке Azure (если это еще не сделано) и выполнить 2 задачи:
* подготовить среду разработки;
* получить учетные данные устройства.

Инструкции по созданию экземпляра центра Azure IoT в подписке Azure см. [здесь](https://github.com/Azure/azure-iot-sdks/blob/master/doc/setup_iothub.md).

В пакет SDK входит [файл сведений](https://github.com/Azure/azure-iot-sdks/tree/master/c), который содержит инструкции по подготовке среды разработки и получению учетных данных устройства. В следующих разделах приведены дополнительные комментарии к этим инструкциям.

### Подготовка среды разработки

Несмотря на то, что для некоторых платформ предоставляются пакеты (например NuGet для Windows или apt\_get для Debian и Ubuntu) и по возможности в примерах используются такие пакеты, ниже приводятся инструкции, описывающие сборку библиотеки и примеров непосредственно из кода.

Во-первых, вам нужно загрузить копию пакета SDK из GitHub, а затем выполнить сборку исходного кода. Копию исходного кода следует извлечь из ветви **master** [репозитория GitHub](https://github.com/Azure/azure-iot-sdks).

Скачав копию исходного кода, необходимо выполнить действия, описанные в статье [Prepare your development environment](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) (Подготовка среды разработки), посвященной пакету SDK.


Вы можете воспользоваться следующими советами, которые помогут выполнить процедуру, описанную в руководстве по подготовке.

-   Устанавливая служебную программу **CMake**, выберите параметр для добавления **CMake** в системную переменную PATH для **всех пользователей** (можно также добавить только для **текущего пользователя**).

  ![](media/iot-hub-device-sdk-c-intro/08-CMake.PNG)


-   Прежде чем открыть **командную строку разработчика для VS2015**, установите программы командной строки Git. Чтобы установить эти средства, выполните следующие действия:

	1. Запустите программу установки **Visual Studio 2015** (или откройте на панели управления раздел **Программы и компоненты**, выберите пункт **Microsoft Visual Studio 2015** и нажмите кнопку **Изменить**).
	
	2. Убедитесь, что в установщике выбран компонент **Git для Windows**. Для обеспечения интеграции IDE также необходимо установить флажок **Расширение GitHub для Visual Studio**.

  		![](media/iot-hub-device-sdk-c-intro/10-GitTools.PNG)

	3. Завершите работу мастера установки, чтобы установить инструменты.

	4. Добавьте каталог **bin** средств Git в системную переменную среды **PATH**. В ОС Windows это будет выглядеть следующим образом:

  		![](media/iot-hub-device-sdk-c-intro/11-GitToolsPath.PNG)


Выполнив все действия, описанные на странице [Prepare your development environment](https://github.com/Azure/azure-iot-sdks/blob/master/c/doc/devbox_setup.md) (Подготовка среды разработки), можно приступать к компиляции примеров приложений.

### Получение учетных данных устройства

После настройки среды разработки вам нужно получить набор учетных данных устройства. Чтобы обеспечить устройству доступ к центру IoT, добавьте его в реестр устройств центра IoT. Когда устройство будет добавлено, вы получите набор учетных данных, которые нужны для подключения этого устройства к центру IoT. Примеры приложений, которые мы рассмотрим в следующем разделе, предполагают использование этих учетных данных в виде **строки подключения устройства**.

В репозиторий SDK с открытым кодом входят несколько инструментов для управления центром IoT. Одно из них — приложение Windows под названием обозреватель устройств, а второе — кроссплатформенный интерфейс командной строки на базе node-js под названием iothub-explorer. Дополнительные сведения об этих инструментах см. [здесь](https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md).

В примерах для Windows, приведенных в этой статье, будет использоваться обозреватель устройств. Если вы предпочитаете инструменты для работы с командной строкой, используйте iothub-explorer.

[Обозреватель устройств](https://github.com/Azure/azure-iot-sdks/tree/master/tools/DeviceExplorer) использует библиотеки службы Azure IoT для выполнения различных операций в центре IoT (включая добавление устройств). Если для добавления устройств вы используете обозреватель устройств, соответствующая строка подключения будет создана автоматически. Эта строка подключения понадобится вам для запуска примеров приложений.

Если вы еще не знакомы с этим процессом, изучите следующую процедуру, в которой описывается добавление устройства и получение строки подключения устройства с помощью диспетчера устройств.

Установщик Windows для обозревателя устройств можно найти на [странице выпуска пакетов SDK](https://github.com/Azure/azure-iot-sdks/releases). Кроме того, его можно запустить прямо из кода, открыв **[DeviceExplorer.sln](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/DeviceExplorer.sln)** в **Visual Studio 2015** и выполнив сборку решения.

При запуске программы вы увидите следующий интерфейс:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Введите в первое поле **строку подключения центра IoT** и щелкните **Обновить**. Теперь средство настроено для обмена данными с центром IoT.

Настроив строку подключения центра IoT, перейдите на вкладку **Управление**.

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Здесь вы можете управлять устройствами, зарегистрированными в центре IoT.

Чтобы создать устройство, нажмите кнопку **Создать**. Откроется диалоговое окно с заполненным набором ключей (первичных и вторичных). Осталось ввести **идентификатор устройства** и щелкнуть **Создать**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Когда устройство будет создано, список зарегистрированных устройств обновится. Он будет включать только что созданное устройство. Если щелкнуть новое устройство правой кнопкой мыши, появится следующее меню:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Если выбрать пункт **Copy connection string for selected device** (Копировать строку подключения для выбранного устройства), строка подключения выбранного устройства будет скопирована в буфер обмена. Сохраните копию строки подключения. Она потребуется вам при запуске примеров приложений, описанных в последующих разделах.

Выполнив описанные выше шаги, вы будете готовы к запуску кода. В обоих примерах в верхней части основного исходного файла есть константа для ввода строки подключения. Например, соответствующая строка из приложения **iothub\_client\_sample\_amqp** выглядит следующим образом.

```
static const char* connectionString = "[device connection string]";
```

Чтобы перейти к дальнейшим действиям, введите строку подключения устройства и перекомпилируйте решение. После этого вы сможете запустить пример.

## IoTHubClient

В папке **iothub\_client** репозитория azure-iot-sdks есть папка **samples**. Она содержит приложение **iothub\_client\_sample\_amqp**.

Версия приложения **iothub\_client\_sample\_ampq** для Windows включает следующее решение Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-amqp.PNG)

Это решение содержит один проект: Однако следует отметить, что в этом решении установлено четыре пакета NuGet.

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.uamqp

Для работы с пакетом SDK всегда необходим пакет **Microsoft.Azure.C.SharedUtility**. Так как этот пример использует AMQP, вам также потребуются пакеты **Microsoft.Azure.uamqp** и **Microsoft.Azure.IoTHub.AmqpTransport** (для HTTP и MQTT есть эквивалентные пакеты). Так как в рассматриваемом примере используется библиотека **IoTHubClient**, вам также нужно включить в решение пакет **Microsoft.Azure.IoTHub.IoTHubClient**.

Реализацию примера приложения вы можете найти в исходном файле **iothub\_client\_sample\_amqp.c**.

На примере этого приложения мы покажем, что необходимо сделать для использования библиотеки **IoTHubClient**.

### Инициализация библиотеки

> [AZURE.NOTE] Перед началом работы с библиотеками вам может потребоваться выполнить определенные действия по инициализации для конкретной платформы. Например, если вы планируете использовать AMQPS в Linux, вам необходимо инициализировать библиотеку OpenSSL. Примеры в [репозитории GitHub](https://github.com/Azure/azure-iot-sdks) вызывают служебную функцию **platform\_init**, когда клиент запускает и вызывает функцию **platform\_deinit** перед выходом. Эти функции объявлены в файле заголовка platform.h. Изучите определения этих функций для целевой платформы в [репозитории](https://github.com/Azure/azure-iot-sdks), чтобы определить, необходимо ли включить код для инициализации платформы в клиент.

Чтобы начать работу с библиотеками, сначала вам нужно выделить дескриптор клиента центра IoT:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Обратите внимание, что мы передаем этой функции копию строки подключения устройства (полученную из диспетчера устройств). Также мы указываем протокол передачи данных. В этом примере используется AMQP, но вы также можете использовать MQTT или HTTP.

Получив обработчик **IOTHUB\_CLIENT\_HANDLE**, вы можете начинать вызывать API-интерфейсы для отправки событий и получения сообщений из центра IoT. Эти операции мы рассмотрим далее.

### Отправка событий

Чтобы отправить события в центр IoT, выполните следующие действия:

Сначала создайте сообщение:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_Over_AMQP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText);
```

А затем отправьте его:

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Во время каждой отправки сообщения вам нужно указывать ссылку на функцию обратного вызова, которая будет вызываться при отправке данных:

```
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %d with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Обратите внимание на вызов функции **IoTHubMessage\_Destroy** после завершения операции с сообщением. Этот вызов необходим для освобождения ресурсов, выделенных во время создания сообщения.

### Получение сообщений

Получение сообщения является асинхронной операцией. Сначала вам необходимо зарегистрировать функцию обратного вызова, выполняемую при получении устройством сообщения:

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Последний параметр — это указатель void, который может указывать на что угодно. Хотя в нашем примере он указывает на целое число, в других случаях он может указывать и на более сложную структуру данных. В результате функция обратного вызова может работать в общем состоянии с объектом, вызывающим эту функцию.

Когда устройство получает сообщение, вызывается зарегистрированная функция обратного вызова:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) == IOTHUB_MESSAGE_OK)
    {
        (void)printf("Received Message [%d] with Data: <<<%.*s>>> & Size=%d\r\n", *counter, (int)size, buffer, (int)size);
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Обратите внимание на использование функции **IoTHubMessage\_GetByteArray** при получении сообщения, которое в этом примере представлено строкой.

### Отмена инициализации библиотеки

После завершения отправки событий и получения сообщений вы можете отменить инициализацию библиотеки IoT. Это можно сделать с помощью вызова следующей функции:

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Это действие освобождает ресурсы, выделенные функцией **IoTHubClient\_CreateFromConnectionString**.

Очевидно, что с помощью библиотеки **IoTHubClient** вы можете легко отправлять события и получать сообщения. Библиотека обеспечивает обмен данными с центром IoT, включая выбор используемого протокола (что с точки зрения разработчика является простым параметром конфигурации).

Библиотека **IoTHubClient** также позволяет полностью управлять способом сериализации событий, которые ваше устройство отправляет в центр IoT. В некоторых случаях это является преимуществом, но иногда подобная функция может и не требоваться в ходе реализации. В таком случае целесообразно использовать библиотеку **serializer**, о которой будет рассказано в следующем разделе.

## Serializer

На концептуальном уровне библиотека **serializer** управляет библиотекой **IoTHubClient**, входящей в пакет SDK. Она использует библиотеку **IoTHubClient** в качестве базового канала связи с центром IoT, а также предоставляет возможности моделирования, которые помогают решать задачи, связанные с сериализацией сообщений. Работу этой библиотеки лучше всего продемонстрировать на примере.

Папка **serializer** репозитория azure-iot-sdks включает папку **samples**. В ней находится приложение **simplesample\_amqp**. Версия этого примера для Windows включает следующее решение Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_amqp.PNG)

Этот пример, как и предыдущий, включает несколько пакетов NuGet:

- Microsoft.Azure.C.SharedUtility
- Microsoft.Azure.IoTHub.AmqpTransport
- Microsoft.Azure.IoTHub.IoTHubClient
- Microsoft.Azure.IoTHub.Serializer
- Microsoft.Azure.uamqp

Большинство из них мы видели в предыдущем примере, но **Microsoft.Azure.IoTHub.Serializer** — это новый пакет. Наличие этого пакета обязательно, если используется библиотека **serializer**.

Реализацию примера приложения вы можете найти в файле **simplesample\_amqp.c**.

Следующие разделы содержат описание основных частей этого примера.

### Инициализация библиотеки

Чтобы приступить к работе с библиотекой **serializer**, вам нужно вызвать API-интерфейсы инициализации:

```
serializer_init(NULL);

IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);

ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
```

Функция **serializer\_init** вызывается один раз. Она инициализирует базовую библиотеку. Затем вызывается функция **IoTHubClient\_CreateFromConnectionString**. Это тот же API, что и в примере **IoTHubClient**. Вызов задает строку подключения устройства (на этом этапе вы также можете выбрать используемый протокол). Обратите внимание, что в этом примере для транспорта используется протокол AMQP, но также можно использовать и HTTP.

Наконец, вызывается функция **CREATE\_MODEL\_INSTANCE**. Обратите внимание, что **WeatherStation** — это пространство имен модели, а **ContosoAnemometer** — имя модели. Создав экземпляр модели, вы сможете использовать его для отправки событий и получения сообщений. Однако сначала нужно понять, что такое модель.

### Определение модели

Модель в библиотеке **serializer** определяет события, которые ваше устройство может отправлять в центр IoT, а также сообщения, которые оно может получать (на языке моделирования называются *действия*). Модель определяется с помощью набора макросов C. Образец можно увидеть в примере приложения **simplesample\_amqp**:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Макросы **BEGIN\_NAMESPACE** и **END\_NAMESPACE** принимают имя пространства имен модели в качестве аргумента. Предполагается, что весь код, находящийся между этими макросами, и является определением моделей и структур данных, используемых в моделях.

В этом примере приведена одна модель с именем **ContosoAnemometer**. Эта модель определяет два события, которые ваше устройство может отправить в центр IoT: **DeviceId** и **WindSpeed**. Она также определяет три действия (сообщения), которые ваше устройство может получить: **TurnFanOn**, **TurnFanOff** и **SetAirResistance**. Каждое событие имеет тип, и каждое действие имеет имя (и, при необходимости, набор параметров).

События и действия, определенные в модели, определяют поверхность API, с помощью которой можно отправлять события в центр IoT и отвечать на сообщения, отправляемые устройству. Лучше всего это показать на примере.

### Отправка событий

Модель определяет события, которые вы можете отправить в центр IoT. В нашем примере это одно из двух событий, определенных с помощью макроса **WITH\_DATA**. Например, если вы хотите отправить событие **WindSpeed** в центр IoT, для этого потребуется выполнить несколько шагов. Первый шаг — определение данных для отправки:

```
myWeather->WindSpeed = 15;
```

Определенная ранее модель позволяет сделать это, задав элемент **struct**. Далее мы сериализуем событие, которое нужно отправить:

```
unsigned char* destination;
size_t destinationSize;

SERIALIZE(&destination, &destinationSize, myWeather->WindSpeed);
```

Этот код выполняет сериализацию события в буфер (на который указывает **destination**). В заключение мы отправим событие в центр IoT, используя следующий код:

```
sendMessage(iotHubClientHandle, destination, destinationSize);
```

Это вспомогательная функция примера приложения, которая отправляет сериализованное событие в центр IoT:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        if (IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
    messageTrackingId++;
}
```

Этот код очень похож на код из приложения **iothub\_client\_sample\_amqp**. В этом примере из массива байтов создается сообщение, которое затем отправляется в центр IoT с помощью **IoTHubClient\_SendEventAsync**. После этого нам нужно освободить дескриптор сообщения и буфер сериализованных данных (выделено ранее).

Предпоследний параметр **IoTHubClient\_SendEventAsync** — это ссылка на функцию обратного вызова, вызываемую при успешной отправке данных. Пример функции обратного вызова:

```
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    int messageTrackingId = (intptr_t)userContextCallback;

    (void)printf("Message Id: %d Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

Второй параметр — это указатель на контекст пользователя. Такой же указатель мы передавали в **IoTHubClient\_SendEventAsync**. В данном случае контекстом является простой счетчик, но это может быть любой другой объект.

Это все, что касается отправки событий. Единственное, о чем осталось рассказать, — это получение сообщений.

### Получение сообщений

Получение сообщений выполняется аналогично обработке сообщений в библиотеке **IoTHubClient**. Сначала вам нужно зарегистрировать функцию обратного вызова сообщения:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Затем вы создаете функцию обратного вызова, которая вызывается при получении сообщения:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Данный код является стандартным и может использоваться с любым решением. Эта функция получает сообщение и перенаправляет его соответствующей функции через вызов **EXECUTE\_COMMAND**. Функция, которая будет вызвана в этот момент, зависит от определения действий в нашей модели.

Когда вы определяете действие в модели, вам нужно реализовать функцию, которая вызывается, когда устройство получает соответствующее сообщение. Например, если модель определяет следующее действие:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Вам нужно определить функцию со следующей сигнатурой:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Обратите внимание, что имя функции соответствует имени действия в модели, а параметры функции — параметрам, указанным для действия. Первый параметр всегда является обязательным и содержит указатель, который указывает на экземпляр нашей модели.

Когда устройство получает сообщение, которое соответствует сигнатуре, вызывается соответствующая функция. Таким образом, кроме включения стереотипного кода из **IoTHubMessage**, для получения сообщений нужно просто определить простую функцию для каждого действия, заданного в модели.

### Отмена инициализации библиотеки

После завершения отправки и получения сообщений, вы можете отменить инициализацию библиотеки IoT:

```
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Каждая из этих трех функций соответствуют трем функциям инициализации, описанным ранее. Вызов этих API позволит освободить выделенные ранее ресурсы.

## Дальнейшие действия

В этой статье описываются основы использования библиотек, включенных в **пакет SDK для устройств Azure IoT для C**. В ней содержатся сведения, которых будет достаточно для понимания содержимого пакета SDK и его архитектуры, а также для начала работы с примерами для Windows. В следующей статье также рассказывается о пакете SDK и приводятся [дополнительные сведения о библиотеке IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).

Чтобы узнать, как использовать возможности управления устройствами в **пакете SDK для устройств Azure IoT для C**, см. статью [Знакомство с клиентской библиотекой управления устройствами для центра Azure IoT](iot-hub-device-management-library.md).

Дополнительные сведения о разработке для центра IoT см. в статье [Пакеты SDK для центра IoT][lnk-sdks].

Для дальнейшего изучения возможностей центра IoT см. следующие статьи:

- [Разработка решения][lnk-design]
- [Обзор управления устройствами центра IoT с помощью примера пользовательского интерфейса][lnk-dmui]
- [Пакет SDK для шлюза IoT (бета-версия): отправка сообщений с устройства в облако через виртуальное устройство с помощью Linux][lnk-gateway]
- [Управление центрами IoT через портал Azure][lnk-portal]

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-sdks-summary.md

[lnk-design]: iot-hub-guidance.md
[lnk-dmui]: iot-hub-device-management-ui-sample.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-portal]: iot-hub-manage-through-portal.md

<!---HONumber=AcomDC_0713_2016-->