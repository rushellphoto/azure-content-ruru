<properties
 pageTitle="Управление сроком действия содержимого BLOB-объекта в сети доставки содержимого (CDN) Azure | Microsoft Azure"
 description="Сведения о возможностях контроля времени жизни BLOB-объектов в кэшировании Azure CDN."
 services="cdn"
 documentationCenter=".NET"
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="07/28/2016"
 ms.author="casoper"/>


#Управление сроком действия содержимого BLOB-объекта в сети доставки содержимого (CDN) Azure  

Наибольшие преимущества от кэширования Azure CDN получают BLOB-объекты, которые часто запрашиваются в течение их срока жизни (TTL). BLOB-объект остается в кэше в течение срока жизни, а затем обновляется службой BLOB-объектов по истечении этого времени. Затем процесс повторяется.

Имеется два варианта управления сроком жизни.

1.	Не задавайте значения кэша, чтобы использовать срок жизни по умолчанию длиной в 7 дней.
2.	Явно задайте свойство *x-ms-blob-cache-control* в запросе **Put Blob**, **Put Block List** или **Set Blob Properties** или используйте управляемую библиотеку Azure для задания свойства [BlobProperties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx). При установке этого свойства задается значение заголовка *Cache-Control* BLOB-объекта. В заголовке или свойстве должно быть указано соответствующее значение в секундах. Например, чтобы задать максимальный период кэширования, равный одному году, можно указать заголовок запроса `x-ms-blob-cache-control: public, max-age=31556926`. Дополнительные сведения о задании заголовков кэширования см. в [спецификации HTTP/1.1](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

Любое содержимое, которое вы хотите кэшировать через CDN, должно храниться в вашей учетной записи хранения Azure в виде общедоступного BLOB-объекта. Дополнительные сведения о службе BLOB-объектов Azure см. в разделе [Основные понятия службы BLOB-объектов](https://msdn.microsoft.com/library/dd179376.aspx).

Существует несколько разных способов работы с содержимым в службе BLOB-объектов.

-	С помощью управляемых API, предоставленных в [клиентской библиотеке службы хранилища Azure для .NET](https://msdn.microsoft.com/library/azure/mt347887.aspx).
-	С помощью стороннего средства управления хранилищем.
-	С помощью [REST API служб хранилища Azure](https://msdn.microsoft.com/library/azure/dd179355.aspx).

В следующем примере кода приведено консольное приложение, которое использует клиентскую библиотеку службы хранилища Azure для создания контейнера, задания его разрешений для общего доступа и создания большого двоичного объекта в этом контейнере. Оно также явно определяет желаемый интервал обновления посредством задания заголовка Cache-Control этого BLOB-объекта.

Если вы включили CDN, как показано выше, созданный BLOB-объект будет кэширован сетью CDN. Обязательно укажите учетные данные учетной записи, используя свою учетную запись хранения и свой ключ доступа:

```csharp
using System;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.StorageClient;

namespace BlobsInCDN
{
	class Program
	{
		static void Main(string[] args)
		{
			//Specify storage credentials.
			StorageCredentialsAccountAndKey credentials = new StorageCredentialsAccountAndKey("storagesample",
				"<your storage account key>");

			//Create a reference to your storage account, passing in your credentials.
			CloudStorageAccount storageAccount = new CloudStorageAccount(credentials, true);

			//Create a new client object, which will provide access to Blob service resources.
			CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

			//Create a new container.
			CloudBlobContainer container = blobClient.GetContainerReference("cdncontent");
			container.CreateIfNotExist();

			//Specify that the container is publicly accessible.
			BlobContainerPermissions containerAccess = new BlobContainerPermissions();
			containerAccess.PublicAccess = BlobContainerPublicAccessType.Container;
			container.SetPermissions(containerAccess);

			//Create a new blob and write some text to it.
			CloudBlob blob = blobClient.GetBlobReference("cdncontent/testblob.txt");
			blob.UploadText("This is a test blob.");

			//Set the Cache-Control header on the blob to specify your desired refresh interval.
			blob.SetCacheControl("public, max-age=31536000");
		}
	}

	public static class BlobExtensions
	{
		//A convenience method to set the Cache-Control header.
		public static void SetCacheControl(this CloudBlob blob, string value)
		{
			blob.Properties.CacheControl = value;
			blob.SetProperties();
		}
	}
}
```

Проверьте доступность BLOB-объекта через URL-адрес сети CDN. Для приведенного выше BLOB-объекта URL-адрес будет аналогичен следующему:

	http://<endpoint>.azureedge.net/cdncontent/testblob.txt  

При желании можно использовать такие средства, как **wget** или Fiddler, для просмотра сведений о запросе и ответе.

##См. также

[Управление сроком действия содержимого облачных служб в сети доставки содержимого (CDN Azure)](./cdn-manage-expiration-of-cloud-service-content.md)

<!---HONumber=AcomDC_0803_2016-->