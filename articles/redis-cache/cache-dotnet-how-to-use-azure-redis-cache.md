<properties 
	pageTitle="Как использовать кэш Redis для Azure | Microsoft Azure" 
	description="Узнайте, как повысить производительность приложений Azure с помощью кэша Redis для Azure." 
	services="redis-cache,app-service" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags 
	ms.service="cache" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="cache-redis" 
	ms.devlang="dotnet" 
	ms.topic="hero-article" 
	ms.date="06/09/2016" 
	ms.author="sdanie"/>

# Как использовать кэш Redis для Azure

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

В этом руководстве объясняется, как приступить к использованию **кэша Redis для Azure**. Кэш Redis для Microsoft Azure основывается на популярном продукте с открытым кодом — кэше Redis. Он дает доступ к защищенному выделенному кэшу Redis, управляемому Майкрософт. Кэш, созданный с использованием кэша Azure Redis, доступен из любого приложения в Microsoft Azure.

Доступны указанные ниже уровни кэша Redis для Microsoft Azure.

-	**Basic** — один узел. Множество размеров вплоть до 53 ГБ.
-	**Standard** — два узла: основной и реплика. Множество размеров вплоть до 53 ГБ. СОГЛАШЕНИЕ ОБ УРОВНЕ ОБСЛУЖИВАНИЯ 99,9 %.
-	**Премиум** — два узла (основной и реплика), включающие до 10 сегментов. Несколько размеров от 6 до 530 ГБ (если вам необходим больший размер, свяжитесь с нами). Все функции уровня «Стандартный», а также дополнительные функции, включая поддержку [кластера Redis](cache-how-to-premium-clustering.md), [сохраняемости Redis](cache-how-to-premium-persistence.md) и [виртуальной сети Azure](cache-how-to-premium-vnet.md). СОГЛАШЕНИЕ ОБ УРОВНЕ ОБСЛУЖИВАНИЯ 99,9 %.

Каждый уровень отличается доступными возможностями и ценой. Дополнительные сведения о ценах см. на странице [Кэш Redis. Цены][].

В этом руководстве показано, как использовать клиент [StackExchange.Redis][] с помощью кода C#. Рассматриваемые сценарии включают **создание и настройку кэша**, **настройку клиентов кэша**, а также **добавление объектов в кэш и их удаление**. Дополнительные сведения об использовании кэша Redis для Azure см. в разделе [Дальнейшие действия][]. Пошаговое руководство по созданию веб-приложения MVC ASP.NET с помощью кэша Redis см. в статье [Как создать веб-приложение с использованием кэша Redis](cache-web-app-howto.md).

<a name="getting-started-cache-service"></a>
## Начало работы с кэшем Redis для Azure

Начать работу с кэшем Azure Redis просто. Чтобы приступить к работе, подготовьте и настройте кэш. Затем следует настроить клиенты кэша, чтобы они могли обращаться к кэшу. Когда клиенты кэша настроены, с ними можно работать.

-	[Создание кэша][]
-	[Настройка клиентов кэша][]

<a name="create-cache"></a>
## Создание кэша

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### Доступ к кэшу после его создания

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Дополнительные сведения о настройке кэша см. в статье [Настройка кэша Redis для Azure](cache-configure.md).

<a name="NuGet"></a>
## Настройка клиентов кэша

[AZURE.INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

После настройки проекта клиента для кэширования можно использовать методы, описанные в следующих разделах, для работы с кэшем.

<a name="working-with-caches"></a>
## Работа с кэшами

Действия, приведенные в этом разделе, описывают способы выполнения типовых задач при работе с кэшем.

-	[Подключение к кэшу][]
-   [Добавление в кэш объектов и их извлечение][]
-   [Работа с объектами .NET в кэше](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>
## Подключение к кэшу

Чтобы программно работать с кэшем, требуется ссылка на кэш. Добавьте следующий код вверху любого файла, из которого будет использоваться клиент StackExchange.Redis для доступа к кэшу Redis для Azure.

    using StackExchange.Redis;

>[AZURE.NOTE] Клиенту StackExchange.Redis необходима среда .NET Framework 4 или выше.

Подключение к кэшу Redis для Azure выполняется с помощью класса `ConnectionMultiplexer`. Этот класс предназначен для общего использования и повторного использования клиентским приложением и не нуждается в создании нового экземпляра для каждой отдельной операции.

Для подключения к кэшу Redis для Azure и получения экземпляра подключенного класса `ConnectionMultiplexer` вызовите статический метод `Connect` и передайте ему конечную точку кэша и ключ, как показано в примере ниже. Используйте в качестве параметра пароля ключ, созданный на портале Azure.

	ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

>[AZURE.IMPORTANT] Внимание! Никогда не храните учетные данные в исходном коде. Для сохранения простоты примера они показаны в исходном коде. Сведения о том, как хранить учетные данные, см. в статье [Принципы работы строк приложений и подключения][].

Если вам не требуется SSL, задайте значение `ssl=false` или пропустите параметр `ssl`.

>[AZURE.NOTE] Для новых кэшей не SSL порт по умолчанию запрещен. Инструкции по включению портов, не использующих протокол SSL, см. в разделе [Порты доступа](cache-configure.md#access-ports).

Один из способов совместного использования экземпляра `ConnectionMultiplexer` в приложении таков: нужно иметь статическое свойство, которое возвращает подключенный экземпляр (как в приведенном ниже примере). Это потокобезопасный способ инициализации только одного подключенного экземпляра `ConnectionMultiplexer`. В этих примерах для параметра `abortConnect` задано значение False. Это означает, что вызов завершается успешно, даже если подключение к кэшу Redis для Azure не установлено. Одна из ключевых особенностей `ConnectionMultiplexer` заключается в том, что этот параметр автоматически восстанавливает подключение к кэшу, когда устраняется проблема с сетью или другие проблемы.

	private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
	{
	    return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
	});
	
	public static ConnectionMultiplexer Connection
	{
	    get
	    {
	        return lazyConnection.Value;
	    }
	}

За дополнительной информацией по вариантам расширенных настроек соединения обратитесь к раз делу [Модель конфигурации StackExchange.Redis][].

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

Создав подключение, верните ссылку на базу данных кэша Redis с помощью метода `ConnectionMultiplexer.GetDatabase`. Объект, возвращенный из метода `GetDatabase`, – это упрощенный передаваемый объект, не требующий сохранения.

	// Connection refers to a property that returns a ConnectionMultiplexer
	// as shown in the previous example.
	IDatabase cache = Connection.GetDatabase();

	// Perform cache operations using the cache object...
	// Simple put of integral data types into the cache
	cache.StringSet("key1", "value");
	cache.StringSet("key2", 25);

	// Simple get of data types from the cache
	string key1 = cache.StringGet("key1");
	int key2 = (int)cache.StringGet("key2");

Теперь нам известно, как соединяться с экземпляром кэша Azure Redis и возвращать ссылку на базу данных кэша, поэтому давайте посмотрим, как работать с кэшем.

<a name="add-object"></a>
## Добавление и извлечение объектов из кэша

Элементы можно хранить в кэше и извлекать из него с помощью методов `StringSet` и `StringGet`.

	// If key1 exists, it is overwritten.
	cache.StringSet("key1", "value1");

	string value = cache.StringGet("key1");

Redis хранит большинство данных в строках Redis, но эти строки могут содержать данные многих типов, включая сериализованные двоичные данные, которые можно использовать при сохранении объектов .NET в кэше.

Если объект существует, вызов метода `StringGet` возвращает его. В противном случае возвращается значение `null`. В этом случае можно извлечь значение из желаемого источника данных и сохранить его в кэше для последующего использования. Это называется шаблоном "кэш на стороне".

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // The item keyed by "key1" is not in the cache. Obtain
        // it from the desired data source and add it to the cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

Чтобы указать срок действия объекта в кэше, используйте параметр `TimeSpan` метода `StringSet`.

	cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## Работа с объектами .NET в кэше

Кэш Azure Redis может кэшировать объекты .NET, а также несложные типы данных, но прежде чем объект .NET можно будет сохранить, он должен быть сериализован. Это ответственность разработчика приложения, что дает разработчику гибкость в выборе сериализатора.

Простой способ сериализации объектов — использовать методы сериализации `JsonConvert` в [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) и выполнять сериализацию в нотацию JSON и из нее. В следующем примере показано получение и задание экземпляра объекта `Employee`.


	class Employee
	{
	    public int Id { get; set; }
	    public string Name { get; set; }
	
	    public Employee(int EmployeeId, string Name)
	    {
	        this.Id = EmployeeId;
	        this.Name = Name;
	    }
	}

    // Store to cache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>
## Дальнейшие действия

Теперь, когда вы ознакомились с основными сведениями, используйте следующие ссылки для получения дополнительных сведений о кэше Redis для Azure.

-	Ознакомьтесь с информацией о поставщиках ASP.NET для кэша Redis для Azure.
	-	[Поставщик состояний сеансов Redis для Azure](cache-aspnet-session-state-provider.md)
	-	[Поставщик кэша вывода ASP.NET для кэша Redis для Azure](cache-aspnet-output-cache-provider.md)
-	[Включите диагностику кэша](cache-how-to-monitor.md#enable-cache-diagnostics), чтобы можно было [наблюдать](cache-how-to-monitor.md) за работоспособностью кэша. Метрики можно просмотреть на портале Azure. Кроме того, их можно [скачать и просмотреть](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) с помощью привычных вам инструментов.
-	Ознакомьтесь с [документацией по клиенту кэша StackExchange.Redis][].
	-	Доступ к кэшу Redis для Azure можно получить из многих клиентов Redis и языков разработки. Дополнительные сведения см. на странице [http://redis.io/clients][].
-	Кэш Redis для Azure можно также использовать со сторонними службами и средствами, например с Redsmin и Redis Desktop Manager.
	-	Дополнительные сведения о Redsmin см. в статье [Получение строки подключения Redis для Azure и ее использование с Redsmin][].
	-	Вы можете открыть и проверить свои данные в кэше Redis для Azure с помощью графического пользовательского интерфейса [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).
-	См. документацию по [redis][]\: прочитайте [статью о типах данных redis][] и [пятнадцатиминутное введение в типы данных Redis][].



<!-- INTRA-TOPIC LINKS -->
[Дальнейшие действия]: #next-steps
[Introduction to Azure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project to Use Azure Caching]: #prepare-vs
[Configure Your Application to Use Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Создание кэша]: #create-cache
[Configure the cache]: #enable-caching
[Настройка клиентов кэша]: #NuGet
[Working with Caches]: #working-with-caches
[Подключение к кэшу]: #connect-to-cache
[Добавление в кэш объектов и их извлечение]: #add-object
[Specify the expiration of an object in the cache]: #specify-expiration
[Store ASP.NET session state in the cache]: #store-session

  
<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png






   
<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[Получение строки подключения Redis для Azure и ее использование с Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How to Configure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set the Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[Модель конфигурации StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md

[Work with .NET objects in the cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Кэш Redis. Цены]: http://www.windowsazure.com/pricing/details/cache/
[Azure Portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate to Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups to manage your Azure resources]: http://azure.microsoft.com/documentation/articles/resource-group-overview/

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[документацией по клиенту кэша StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[статью о типах данных redis]: http://redis.io/topics/data-types
[пятнадцатиминутное введение в типы данных Redis]: http://redis.io/topics/data-types-intro

[Принципы работы строк приложений и подключения]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/

<!---HONumber=AcomDC_0727_2016-->