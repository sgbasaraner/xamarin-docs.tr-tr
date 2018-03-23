---
title: Bir Azure Cosmos DB belge veritabanı kullanma
description: Bir Azure Cosmos DB belge veritabanı sorunsuz ölçeklendirme ve genel çoğaltma gerektiren uygulamalar için hızlı, yüksek oranda kullanılabilir, ölçeklenebilir veritabanı hizmet sunumu, JSON belgelerini düşük gecikmeli erişim sağlayan bir NoSQL veritabanıdır. Bu makalede Azure Cosmos DB standart .NET istemci kitaplığı Azure Cosmos DB belge veritabanı bir Xamarin.Forms uygulamasına tümleştirmek için nasıl kullanılacağı açıklanmaktadır.
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: e2fa5ae12531069e1ad1bc19e110e4dcffe23a02
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="consuming-an-azure-cosmos-db-document-database"></a>Bir Azure Cosmos DB belge veritabanı kullanma

_Bir Azure Cosmos DB belge veritabanı sorunsuz ölçeklendirme ve genel çoğaltma gerektiren uygulamalar için hızlı, yüksek oranda kullanılabilir, ölçeklenebilir veritabanı hizmet sunumu, JSON belgelerini düşük gecikmeli erişim sağlayan bir NoSQL veritabanıdır. Bu makalede Azure Cosmos DB standart .NET istemci kitaplığı Azure Cosmos DB belge veritabanı bir Xamarin.Forms uygulamasına tümleştirmek için nasıl kullanılacağı açıklanmaktadır._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB, göre [Xamarin Üniversitesi](https://university.xamarin.com/)**

Bir Azure Cosmos DB belge veritabanı hesabı, bir Azure aboneliği kullanılarak sağlanabilir. Her veritabanı hesabı sıfır veya daha fazla veritabanı bulunabilir. Azure Cosmos DB belge veritabanında, belge koleksiyonları ve kullanıcılar için mantıksal bir kapsayıcısıdır.

Bir Azure Cosmos DB belge veritabanı sıfır veya daha fazla belge koleksiyonları içerebilir. Her bir belge koleksiyonu sık erişilen koleksiyonlar için belirtilmesi için daha fazla verimlilik ve seyrek erişilen koleksiyonlar için daha az işleme izin vererek bir farklı performans düzeyine sahip olabilir.

Her bir belge koleksiyonu sıfır veya daha fazla JSON belgeleri oluşur. Bir koleksiyon belgelerde şemasız ve bu nedenle aynı yapısını veya alanlar paylaşmak gerekmez. Belgeler için belge koleksiyonunu eklendikçe Cosmos DB otomatik olarak dizinler bunları ve bunların Sorgulanacak kullanılabilir hale gelir.

Geliştirme amaçlı bir belge veritabanı bir öykünücü tüketilebilir. Öykünücüsü kullanarak, uygulamaları geliştirilen ve bir Azure aboneliği oluşturmak veya herhangi bir maliyet olmadan yerel olarak test. Öykünücü hakkında daha fazla bilgi için bkz: [yerel olarak Azure Cosmos DB öykünücü ile geliştirme](/azure/cosmos-db/local-emulator/).

Bu makalede ve örnek uygulama ile birlikte gelen görevleri bir Azure Cosmos DB belge veritabanında depolandığı bir Yapılacaklar listesi uygulaması gösterir. Örnek uygulama hakkında daha fazla bilgi için bkz: [örnek anlama](~/xamarin-forms/data-cloud/walkthrough.md).

Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB belgeleri](/azure/cosmos-db/).

## <a name="setup"></a>Kurulum

Bir Azure Cosmos DB belge veritabanı bir Xamarin.Forms uygulamayla tümleştirmek için işlem aşağıdaki gibidir:

1. Cosmos DB hesabı oluşturun. Daha fazla bilgi için bkz: [Azure Cosmos DB hesap oluşturmak](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Ekleme [Azure Cosmos DB standart .NET İstemci Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) Xamarin.Forms çözüm platformu projelerin için NuGet paketi.
1. Ekleme `using` yönergeleri için `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, ve `Microsoft.Azure.Documents.Linq` Cosmos DB hesap erişecek sınıfları için ad alanları.

Bu adımları gerçekleştirdikten sonra Azure Cosmos DB standart .NET istemci kitaplığı yapılandırmak ve belge veritabanında isteklerini yürütmek için kullanılabilir.

> [!NOTE]
> Azure Cosmos DB standart .NET istemci kitaplığı yalnızca platform projelerine ve bir taşınabilir sınıf kitaplığı (PCL) projesine yüklenebilir. Bu nedenle, örnek bir paylaşılan erişim proje (kod yinelemesinden kaçınmak için SAP) uygulamasıdır. Ancak, `DependencyService` sınıfı platforma özgü projelerinde bulunan Azure Cosmos DB standart .NET istemci kitaplığı kodu çağrılacak PCL projede kullanılabilir.

## <a name="consuming-the-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı kullanma

`DocumentClient` Türü uç nokta, kimlik bilgileri ve Azure Cosmos DB hesabına erişmek için kullanılan bağlantı İlkesi yalıtır ve yapılandırmak ve hesap isteklerini yürütmek için kullanılır. Aşağıdaki kod örneği, bu sınıfın bir örneğinin nasıl oluşturulacağını gösterir:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

Cosmos DB URI ve birincil anahtar için sağlanmalıdır `DocumentClient` Oluşturucusu. Bu, Azure Portalı'ndan elde edilebilir. Daha fazla bilgi için bkz: [Azure Cosmos DB hesabına Bağlan](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### <a name="creating-a-database"></a>Veritabanı oluşturma

Bir belge veritabanı belge koleksiyonları ve kullanıcılar için mantıksal bir kapsayıcısıdır ve Azure Portal'da oluşturulan ya da program aracılığıyla kullanarak `DocumentClient.CreateDatabaseIfNotExistsAsync` yöntemi:

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

`CreateDatabaseIfNotExistsAsync` Yöntemi belirtir. bir `Database` ile bir bağımsız değişken olarak nesne `Database` nesnesi olarak veritabanı adını belirterek kendi `Id` özelliği. `CreateDatabaseIfNotExistsAsync` Yöntemi oluşturur veritabanı yok veya veritabanı zaten varsa döndürür. Ancak, örnek uygulama tarafından döndürülen veri yoksayar `CreateDatabaseIfNotExistsAsync` yöntemi.

> [!NOTE]
> `CreateDatabaseIfNotExistsAsync` Yöntemi döndürür bir `Task<ResourceResponse<Database>>` nesne ve yanıtın durum kodu kontrol edileceği bir veritabanı oluşturuldu veya varolan bir veritabanını döndürüldü olup olmadığını belirlemek için.

### <a name="creating-a-document-collection"></a>Bir belge koleksiyonu oluşturma

Bir belge koleksiyonu JSON belgeleri için bir kapsayıcıdır ve Azure Portal'da oluşturulan ya da program aracılığıyla kullanarak `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` yöntemi:

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

`CreateDocumentCollectionIfNotExistsAsync` Yöntemi gerektiren iki zorunlu bağımsız değişkeni – bir veritabanı adı olarak belirtilen bir `Uri`ve bir `DocumentCollection` nesnesi. `DocumentCollection` Nesne adı ile belirtilmişse belge koleksiyonunu temsil eder `Id` özelliği. `CreateDocumentCollectionIfNotExistsAsync` Yöntemi yok veya zaten varsa belge koleksiyonunu döndürür belge koleksiyonu oluşturur. Ancak, örnek uygulama tarafından döndürülen veri yoksayar `CreateDocumentCollectionIfNotExistsAsync` yöntemi.

> [!NOTE]
> `CreateDocumentCollectionIfNotExistsAsync` Yöntemi döndürür bir `Task<ResourceResponse<DocumentCollection>>` nesne ve yanıtın durum kodu kontrol edileceği bir belge koleksiyonu oluşturduğunuzda veya mevcut bir belge koleksiyonu döndürüldü olup olmadığını belirlemek için.

İsteğe bağlı olarak, `CreateDocumentCollectionIfNotExistsAsync` yöntemi de belirtebilirsiniz bir `RequestOptions` Cosmos DB hesabına verilen istekleri için belirtilen seçenekler yalıtan nesne. `RequestOptions.OfferThroughput` Özelliği belge koleksiyonunu performans düzeyini tanımlamak için kullanılır ve örnek uygulama, saniye başına 400 istek birimlerine ayarlanır. Bu değer artırılabilir veya olup koleksiyonu sık veya seyrek erişileceği bağlı olarak azaltılabilir.

> [!IMPORTANT]
> Unutmayın `CreateDocumentCollectionIfNotExistsAsync` yöntemi fiyatlandırmaya sahip bir ayrılmış işleme ile yeni bir koleksiyon oluşturur.

<a name="document_query" />

### <a name="retrieving-document-collection-documents"></a>Belge koleksiyonu belgeleri alınıyor

Bir belge koleksiyonu içeriğini oluşturma ve bir belge sorgu yürütülürken alınabilir. Bir belge sorgusu ile oluşturulan `DocumentClient.CreateDocumentQuery` yöntemi:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

Bu sorgu zaman uyumsuz olarak belirtilen koleksiyondan tüm belgeleri alır ve belgelerde yerleştirir bir `List<TodoItem>` görüntü için koleksiyonu.

`CreateDocumentQuery<T>` Yöntemi belirtir. bir `Uri` belgeler için Sorgulanacak koleksiyonunu temsil eder bağımsız değişkeni. Bu örnekte, `collectionLink` değişkeni belirten bir alandır sınıf düzeyi `Uri` belgelerden almak için belge koleksiyonunu temsil eder:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

`CreateDocumentQuery<T>` Yöntemi, zaman uyumlu olarak yürütülür ve döndüren bir sorgu oluşturur bir `IQueryable<T>` nesnesi. Ancak, `AsDocumentQuery` yöntemi dönüştürür `IQueryable<T>` nesnesine bir `IDocumentQuery<T>` zaman uyumsuz olarak çalıştırılabilir bir nesne. Zaman uyumsuz sorgu ile çalıştırılır `IDocumentQuery<T>.ExecuteNextAsync` ile belge veritabanından sonuçlar sonraki sayfasını alır yöntemi `IDocumentQuery<T>.HasMoreResults` sorgudan döndürülecek ek sonuçlar olup olmadığını gösteren özellik.

Belgeler olabilir ekleyerek sunucu tarafı filtrelenmiş bir `Where` belge koleksiyonunu sorgusu bir filtre koşulu uygulandığı sorgu yan tümcesinde:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

Bu sorgu tüm alır koleksiyondan özelliği belgeleri `Done` özelliği eşittir `false`.

<a name="inserting_document" />

### <a name="inserting-a-document-into-a-document-collection"></a>Bir belgenin belge koleksiyona ekleme

Belgeler, kullanıcı tanımlı JSON içeriği olan ve bir belge koleksiyonu ile eklenen `DocumentClient.CreateDocumentAsync` yöntemi:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

`CreateDocumentAsync` Yöntemi belirtir. bir `Uri` belge eklenmiş olabilir, koleksiyonunu temsil eder bağımsız değişkeni ve bir `object` eklenecek belgeyi temsil eden bağımsız değişkeni.

### <a name="replacing-a-document-in-a-document-collection"></a>Bir belge koleksiyonundaki bir belgeyi değiştirme

Belgeler, bir belge koleksiyonunda ile değiştirilebilir `DocumentClient.ReplaceDocumentAsync` yöntemi:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

`ReplaceDocumentAsync` Yöntemi belirtir. bir `Uri` değiştirilmelidir, koleksiyondaki belgeyi temsil eden değişken ve bir `object` güncelleştirilmiş belge verileri temsil eden bağımsız değişkeni.

<a name="deleting_document" />

### <a name="deleting-a-document-from-a-document-collection"></a>Bir belgenin belge koleksiyonundan silme

Bir belge ile bir belge koleksiyonundan silinebilir `DocumentClient.DeleteDocumentAsync` yöntemi:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

`DeleteDocumentAsync` Yöntemi belirtir. bir `Uri` silinmeli koleksiyonunda belgeyi temsil eden bağımsız değişkeni.

### <a name="deleting-a-document-collection"></a>Bir belge koleksiyonu siliniyor

Bir belge koleksiyonu ile veritabanından silinmiş `DocumentClient.DeleteDocumentCollectionAsync` yöntemi:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

`DeleteDocumentCollectionAsync` Yöntemi belirtir. bir `Uri` silinecek belge koleksiyonunu temsil eder bağımsız değişkeni. Bu yöntem çağırma ayrıca bir koleksiyonda depolanan belgeler silineceğini unutmayın.

### <a name="deleting-a-database"></a>Veritabanını silme

Bir veritabanı ile Cosmos DB veritabanı hesabından silinebilir `DocumentClient.DeleteDatabaesAsync` yöntemi:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

`DeleteDatabaseAsync` Yöntemi belirtir. bir `Uri` silinecek veritabanını temsil eden bağımsız değişkeni. Bu yöntem çağırma ayrıca veritabanında depolanan belge koleksiyonlar ve belge koleksiyonu içinde depolanan belgeler silineceğini unutmayın.

## <a name="summary"></a>Özet

Bu makalede Azure Cosmos DB standart .NET istemci kitaplığı Azure Cosmos DB belge veritabanı bir Xamarin.Forms uygulamasına tümleştirmek için nasıl kullanılacağı açıklanmıştır. Bir Azure Cosmos DB belge veritabanı sorunsuz ölçeklendirme ve genel çoğaltma gerektiren uygulamalar için hızlı, yüksek oranda kullanılabilir, ölçeklenebilir veritabanı hizmet sunumu, JSON belgelerini düşük gecikmeli erişim sağlayan bir NoSQL veritabanıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [Yapılacaklar Azure Cosmos DB (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDB/)
- [Azure Cosmos DB belgeleri](/azure/cosmos-db/)
- [Azure Cosmos DB .NET standart istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://docs.microsoft.com/en-us/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)
