<properties
    pageTitle="Мониторинг пула эластичных баз данных и управление им с помощью C# | Microsoft Azure"
    description="Используйте методы разработки баз данных на C# для управлением пулом эластичных баз данных в Базе данных SQL Azure."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="07/05/2016"
    ms.author="srinia"/>

# Мониторинг пула эластичных баз данных и управление им с помощью C&#x23; 

> [AZURE.SELECTOR]
- [Портал Azure](sql-database-elastic-pool-manage-portal.md)
- [PowerShell](sql-database-elastic-pool-manage-powershell.md)
- [C#](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL](sql-database-elastic-pool-manage-tsql.md)


Узнайте, как управлять [пулом эластичных баз данных](sql-database-elastic-pool.md) с помощью языка C&#x23;.

Стандартные коды ошибок см. в статье [Коды ошибок SQL для клиентских приложений базы данных SQL: ошибки подключения к базе данных и другие проблемы](sql-database-develop-error-messages.md).

В примерах ниже используется [библиотека базы данных SQL для .NET](https://msdn.microsoft.com/library/azure/mt349017.aspx), поэтому, прежде чем продолжить, установите эту библиотеку, если она еще не установлена. Чтобы установить библиотеку, в Visual Studio выполните следующую команду в [консоли диспетчера пакетов](http://docs.nuget.org/Consume/Package-Manager-Console) (**Средства** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**).

    PM> Install-Package Microsoft.Azure.Management.Sql –Pre


## Перемещение базы данных в пул эластичных БД

Автономную базу данных можно переместить в пул и из него.

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## Получение списка баз данных в пуле эластичных БД

Для получения всех баз данных в пуле вызовите метод [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases).

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## Изменение параметров производительности пула

Получите существующие свойства пула. Измените значения и выполните метод CreateOrUpdate.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## Задержка операций эластичного пула

- Изменение минимального или максимального числа eDTU для базы данных обычно завершается за 5 минут.
- Изменение числа eDTU на пул зависит от общей емкости, используемой всеми базами данных в пуле. Изменение занимает порядка 90 минут или меньше на каждые 100 ГБ. Например, если общее пространство, используемое всеми базами данных в пуле, равно 200 ГБ, то ожидаемая задержка при изменении числа eDTU для пула составит до 3 часов.


## Управление пулом: пример C&#x23;

Для выполнения этого примера требуются дополнительные библиотеки. Чтобы установить их, в Visual Studio выполните в [консоли диспетчера пакетов](http://docs.nuget.org/Consume/Package-Manager-Console) (**Средства** > **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**) следующие команды.

    PM> Install-Package Microsoft.Azure.Management.Sql –Pre
    PM> Install-Package Microsoft.Azure.Management.Resources –Pre
    PM> Install-Package Microsoft.Azure.Common.Authentication –Pre

Создайте консольное приложение и замените содержимое файла Program.cs приведенным ниже. Сведения о том, как получить идентификатор клиента и другие значения, см. в статье [Получение идентификатора клиента и ключа для доступа к базе данных SQL из кода](sql-database-client-id-keys.md).

    using Microsoft.Azure;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbElasticPoolsSample
    {
    class Program
    {
        // pool variables
        static string poolName = "elasticPool1";
        static string poolEdition = "Standard";
        static int poolDtus = 400;
        static int databaseMinDtus = 0;
        static int databaseMaxDtus = 100;

        // authentication variables
        static string subscriptionId = "<your Azure subscription id>";
        static string clientId = "<your client id>";
        static string redirectUri = "<your redirect URI>";
        static string domainName = "<i.e. microsoft.onmicrosoft.com>";
        static AuthenticationResult token;

        // resource group variables
        static string datacenterLocation = "Japan West";
        static string resourceGroupName = "<resource group name>";

        // server variables
        static string serverName = "<server name>";
        static string adminLogin = "<server admin>";
        static string adminPassword = "<server admin password>";
        static string serverVersion = "12.0";

        // database variables
        static string databaseName = "<database name>";


        static void Main(string[] args)
        {
            // Sign in to Azure
            token = GetAccessToken();
            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            // Create a resource group
            ResourceGroup rg = CreateResourceGroup();
            Console.WriteLine(rg.Name + " created.");

            // Create a server
            Console.WriteLine("Creating server... ");
            ServerGetResponse srvr = CreateServer();
            Console.WriteLine("Creation of server " + srvr.Server.Name + ": " + srvr.StatusCode.ToString());

            // Create a pool
            Console.WriteLine("Creating elastic database pool with 400 pool eDTUs... ");
            ElasticPoolCreateOrUpdateResponse epool = CreateElasticDatabasePool();
            Console.WriteLine("Creation of pool " + epool.ElasticPool.Name + ": " + epool.Status.ToString());

            // Open the portal so we can see our operations in action
            string portalPage = @"https://portal.azure.com/#resource/subscriptions/"
                + subscriptionId
                + @"/resourceGroups/"
                + resourceGroupName
                + @"/providers/Microsoft.Sql/servers/"
                + serverName
                + @"/elasticPools/"
                + poolName ;
            System.Diagnostics.Process.Start(portalPage);


            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to update the pool to 1200 pool eDTUs.");
            Console.ReadLine();

            // Update the pool
            Console.WriteLine("Updating elastic database pool to 1200 pool eDTUs...");
            ElasticPoolCreateOrUpdateResponse epool2 = UpdateElasticDatabasePool();
            Console.WriteLine("Update of pool " + epool2.ElasticPool.Name + ": " + epool2.Status.ToString());

            // Create a new database in the pool
            Console.WriteLine("Creating a new database in the pool... ");
            DatabaseCreateOrUpdateResponse db = CreateNewDatabaseInPool();
            Console.WriteLine("Creation of elastic database " + db.Database.Name + ": " + db.Status.ToString());

            // Move an existing database into pool
            Console.WriteLine("Creating a new database, stand-alone, not yet in the pool... ");
            DatabaseCreateOrUpdateResponse db2 = CreateStandAloneDatabase();
            Console.WriteLine("Creation of stand-alone database " + db2.Database.Name + ": " + db2.Status.ToString());

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to move our existing db into the pool.");
            Console.ReadLine();

            Console.WriteLine("Moving stand-alone database into the pool... ");
            DatabaseCreateOrUpdateResponse db3 = MoveExistingDatabaseIntoPool();
            Console.WriteLine("Moving stand-alone database " + db3.Database.Name + ": " + db3.Status.ToString());

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to list all databases in {0}.", poolName);
            Console.ReadLine();

            // List all databases in a pool
            DatabaseListResponse dbs = GetDbsInPool();
            Console.WriteLine("Databases in Elastic Pool {0}", poolName);
            foreach (Database db4 in dbs)
            {
                Console.WriteLine("- " + db4.Name);
            }

            // Pause the console until Enter is pressed.
            Console.WriteLine("Press Enter to exit.");
            Console.ReadLine();
        }


        static ElasticPoolCreateOrUpdateResponse CreateElasticDatabasePool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus, 
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }

        static ElasticPoolCreateOrUpdateResponse UpdateElasticDatabasePool()
        {
            int newPoolDtus = 1200;
            int newDatabaseMinDtus = 10;
            int newDatabaseMaxDtus = 50;

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve existing pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Configure create or update parameters with existing property values, override those to be changed.
            ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentPool.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = currentPool.Properties.Edition,
                    DatabaseDtuMax = newDatabaseMaxDtus,
                    DatabaseDtuMin = newDatabaseMinDtus,
                    Dtu = newPoolDtus
                }
            };
            var updatePoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, updatePoolParameters);
            return updatePoolResponse;
        }

        static DatabaseCreateOrUpdateResponse CreateNewDatabaseInPool()
        {
            databaseName = "newDbInThePool";

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve current pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Create a database
            DatabaseCreateOrUpdateParameters databaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentPool.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = "Default",
                    Edition = currentPool.Properties.Edition,
                    ElasticPoolName = poolName
                }
            };

            var dbResult = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, databaseParameters);
            return dbResult;
        }

        static DatabaseCreateOrUpdateResponse MoveExistingDatabaseIntoPool()
        {
            databaseName = "standaloneDB";

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Retrieve current database
            var currentDatabase = sqlClient.Databases.Get(resourceGroupName, serverName, databaseName).Database;

            // Retrieve current pool
            var currentPool = sqlClient.ElasticPools.Get(resourceGroupName, serverName, poolName).ElasticPool;

            // Configure create or update parameters with existing property values, override those to be changed.
            DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentDatabase.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = currentPool.Properties.Edition,
                    RequestedServiceObjectiveName = "ElasticPool",
                    ElasticPoolName = currentPool.Name,
                    MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
                    Collation = currentDatabase.Properties.Collation,
                }
            };
            // Update the database
            var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, updatePooledDbParameters);
            return dbUpdateResponse;
        }

        static DatabaseListResponse GetDbsInPool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            //List databases in the elastic pool
            DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases(resourceGroupName, serverName, poolName);
            return dbListInPool;
        }
     

        static ServerGetResponse CreateServer()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = adminLogin,
                    AdministratorLoginPassword = adminPassword,
                    Version = serverVersion
                }
            };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }

        static DatabaseCreateOrUpdateResponse CreateStandAloneDatabase()
        {
            databaseName = "standaloneDB";
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a database
            DatabaseCreateOrUpdateParameters databaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = "Standard",
                    RequestedServiceObjectiveName = "S0",
                    CreateMode = "Default"
                }
            };

            var dbResult = sqlClient.Databases.CreateOrUpdate(resourceGroupName,serverName, databaseName, databaseParameters);
            return dbResult;
        }

        static ResourceGroup CreateResourceGroup()
        {
            Microsoft.Rest.TokenCredentials creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);
            // Create a resource management client 
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = datacenterLocation,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }
    }
    }

## Дополнительные ресурсы

- [База данных SQL](https://azure.microsoft.com/documentation/services/sql-database/)
- [API управления ресурсами](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Создание нового пула эластичных баз данных с помощью C#](sql-database-elastic-pool-create-csharp.md)
- [Когда следует использовать пул эластичных баз данных?](sql-database-elastic-pool-guidance.md)
- Ознакомьтесь с разделом [Развертывание с помощью Базы данных SQL Azure](sql-database-elastic-scale-introduction.md). В нем описывается использование инструментов эластичной базы данных для развертывания, перемещения данных, выполнения запросов и создания транзакций.

<!---HONumber=AcomDC_0706_2016-->