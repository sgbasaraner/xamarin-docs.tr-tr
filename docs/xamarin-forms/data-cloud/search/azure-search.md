---
title: Azure arama ile veri arama
description: Azure Search dizin oluşturma ve karşıya yüklenen veriler için özellikleri sorgulama sağlayan bir bulut hizmetidir. Bu, geleneksel bir uygulamada arama işlevini uygulama ile ilişkili arama algoritması karmaşıklık ve altyapı gereksinimleri kaldırır. Bu makalede, Microsoft Azure arama kitaplığı bir Xamarin.Forms uygulamasına Azure Search tümleştirmek için nasıl kullanılacağı gösterilmektedir.
ms.topic: article
ms.prod: xamarin
ms.assetid: A4AEF233-3672-4174-9DBA-15BEE3030C0B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2016
ms.openlocfilehash: 24db1404e218eea86356f9bbc004e7d5850c2e7a
ms.sourcegitcommit: 7b76c3d761b3ffb49541e2e2bcf292de6587c4e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="searching-data-with-azure-search"></a>Azure arama ile veri arama

_Azure Search dizin oluşturma ve karşıya yüklenen veriler için özellikleri sorgulama sağlayan bir bulut hizmetidir. Bu, geleneksel bir uygulamada arama işlevini uygulama ile ilişkili arama algoritması karmaşıklık ve altyapı gereksinimleri kaldırır. Bu makalede, Microsoft Azure arama kitaplığı bir Xamarin.Forms uygulamasına Azure Search tümleştirmek için nasıl kullanılacağı gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Verileri Azure Search'te dizinler ve belgeler depolanır. Bir *dizin* Azure Search hizmeti tarafından aranabilecek veri deposudur ve bir veritabanı tablosuna kavramsal olarak benzerdir. A *belge* dizin aranabilir verileri tek bir birimidir ve kavramsal olarak veritabanı satırına benzer. Ne zaman belgeleri karşıya yüklemek ve Azure Search'e arama sorguları gönderme istekleri belirli bir arama hizmeti dizininde yapılır.

Azure Search yapılan her isteği hizmeti ve bir API anahtarı adını içermelidir. API anahtarı iki tür vardır:

- *Yönetici anahtarları* tüm işlemleri tam hakları. Bu hizmeti yönetme, oluşturma ve dizinler ve veri kaynaklarını silme içerir.
- *Sorgu anahtarları* dizinler ve belgeler için salt okunur erişim vermek ve arama istekleri gönderen uygulamalar tarafından kullanılmalıdır.

En yaygın Azure arama için bir sorguyu yürütmek için isteğidir. Gönderilebilir sorgu iki tür vardır:

- A *arama* sorgu bir dizindeki tüm aranabilir alanları bir veya daha fazla öğe arar. Arama sorguları, Basitleştirilmiş söz dizimi ya da Lucene sorgu söz dizimi kullanılarak oluşturulur. Daha fazla bilgi için bkz: [Basit Sorgu söz dizimi Azure Search'te](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search/), ve [Lucene sorgu söz dizimi Azure Search'te](/rest/api/searchservice/Lucene-query-syntax-in-Azure-Search/).
- A *filtre* sorgu bir dizindeki tüm filtrelenebilir alanları üzerinden bir Boole ifadesini değerlendirir. Filtre sorgularını OData filtre dilinin bir alt kullanılarak oluşturulur. Daha fazla bilgi için bkz: [Azure arama için OData ifade sözdizimini](/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search/).

Arama sorguları ve filtre sorguları ayrı olarak veya birlikte kullanılabilir. Filtre sorgusu birlikte kullanıldığında, önce tüm dizine uygulanır ve ardından arama sorgusu filtre sorgusu sonuçlarını gerçekleştirilir.

Azure Search'te arama girişinize göre alınırken önerileri de destekler. Daha fazla bilgi için bkz: [öneri sorguları](#suggestions).

## <a name="setup"></a>Kurulum

Azure Search bir Xamarin.Forms uygulamayla tümleştirmek için işlem aşağıdaki gibidir:

1. Azure Search Hizmeti oluşturun. Daha fazla bilgi için bkz: [Azure Portalı'nı kullanarak bir Azure Search hizmeti oluşturma](/azure/search/search-create-service-portal/).
1. Silverlight hedef çerçeve olarak Xamarin.Forms çözümünü taşınabilir sınıf kitaplığı (PCL) kaldırın. Bu, platformlar arası geliştirme destekler, ancak Silverlight, 151 veya profili 92 gibi desteklemeyen herhangi bir profili PCL profili değiştirerek gerçekleştirilebilir.
1. Ekleme [Microsoft Azure arama Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Search) Xamarin.Forms çözümünü PCL projede NuGet paketi.

Bu adımları gerçekleştirdikten sonra Microsoft Search kitaplığı API arama dizinleri ve veri kaynaklarını yönet, karşıya yükleme ve belgeleri yönetmek ve sorguları yürütmek için kullanılabilir.

## <a name="creating-the-azure-search-index"></a>Azure Search dizini oluşturma

Bir dizin şemasını Aranacak veri yapısını eşleyen tanımlanmalıdır. Azure Portalı'nda başarılı ya da program aracılığıyla kullanarak bu `SearchServiceClient` sınıfı. Bu sınıf, Azure Search bağlantıları yönetir ve dizin oluşturmak için kullanılabilir. Aşağıdaki kod örneği, bu sınıfın bir örneğinin nasıl oluşturulacağını gösterir:

```csharp
var searchClient =
  new SearchServiceClient(Constants.SearchServiceName, new SearchCredentials(Constants.AdminApiKey));
```

`SearchServiceClient` Oluşturucusu aşırı bir arama hizmeti adı alır ve bir `SearchCredentials` ile bağımsız değişken olarak nesne `SearchCredentials` nesne kaydırma *yönetici anahtarını* Azure Search hizmeti için. *Yönetici anahtarını* dizin oluşturmak için gereklidir.

> [!NOTE]
>  Tek bir `SearchServiceClient` örneği Azure arama için çok fazla bağlantı açmayı önlemek için bir uygulamada kullanılmalıdır.

Bir dizini tarafından tanımlanan `Index` , aşağıdaki kod örneğinde gösterildiği şekilde nesnesi:

```csharp
static void CreateSearchIndex()
{
  var index = new Index()
  {
    Name = Constants.Index,
    Fields = new[]
    {
      new Field("id", DataType.String) { IsKey = true, IsRetrievable = true },
      new Field("name", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("location", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSortable = true, IsSearchable = true },
      new Field("details", DataType.String) { IsRetrievable = true, IsFilterable = true, IsSearchable = true },
      new Field("imageUrl", DataType.String) { IsRetrievable = true }
    },
    Suggesters = new[]
    {
      new Suggester("nameSuggester", SuggesterSearchMode.AnalyzingInfixMatching, new[] { "name" })
    }
  };

  searchClient.Indexes.Create(index);
}
```

`Index.Name` Özelliği dizin adına ayarlanmalıdır ve `Index.Fields` özelliği için bir dizi ayarlanmalıdır `Field` nesneleri. Her `Field` örneği bir adı, türü ve alanın nasıl kullanıldığını belirtmek tüm özellikleri belirtir. Bu özellikler şunları içerir:

- `IsKey` – alan dizin anahtarı olup olmadığını gösterir. Yalnızca bir alan türü dizindeki `DataType.String`, anahtar alan olarak işaretlenmesi gerekir.
- `IsFacetable` – Bu alanı modellenmiş bir gezinmede gerçekleştirmeniz mümkün olup olmadığını gösterir. Varsayılan değer `false` şeklindedir.
- `IsFilterable` – alanın filtre sorgularında kullanılıp kullanılamayacağını belirtir. Varsayılan değer `false` şeklindedir.
- `IsRetrievable` – alan arama sonuçlarında alınıp alınamayacağını belirtir. Varsayılan değer `true` şeklindedir.
- `IsSearchable` – alanın tam metin araması eklenip eklenmeyeceğini gösterir. Varsayılan değer `false` şeklindedir.
- `IsSortable` – alanın kullanılabilir olup olmadığını gösteren `OrderBy` ifadeler. Varsayılan değer `false` şeklindedir.

> [!NOTE]
> Dağıtıldıktan sonra bir dizini yeniden oluşturma ve verileri yeniden yükleme değiştirilmesidir.

Bir `Index` nesne isteğe bağlı olarak belirtebileceğiniz bir `Suggesters` alanları otomatik tamamlama desteklemek veya öneri sorguları aramak için kullanılacak dizini tanımlayan özelliği. `Suggesters` Özelliği için bir dizi ayarlanmalıdır `Suggester` arama önerisi sonuçları oluşturmak için kullanılan alanları tanımlayın nesneleri.

Oluşturduktan sonra `Index` nesnesi, dizini oluşturulan çağırarak `Indexes.Create` üzerinde `SearchServiceClient` örneği.

> [!NOTE]
> Bir uygulamadan bir dizin oluşturma esnek tutulması gereken durumlarda kullanın `Indexes.CreateAsync` yöntemi.

Daha fazla bilgi için bkz: [.NET SDK kullanarak Azure Search dizini oluşturma](/azure/search/search-create-index-dotnet/).

## <a name="deleting-the-azure-search-index"></a>Azure Search dizini silme

Bir dizin çağırarak silinebilir `Indexes.Delete` üzerinde `SearchServiceClient` örneği:

```csharp
searchClient.Indexes.Delete(Constants.Index);
```

## <a name="uploading-data-to-the-azure-search-index"></a>Azure Search dizinine verileri yükleniyor

Dizin tanımladıktan sonra verileri iki modelden birini kullanarak karşıya yüklenebilir:

- **Model çekme** – verileri düzenli aralıklarla alınan Azure Cosmos DB, Azure SQL Database, Azure Blob Depolama veya SQL Server barındırılan bir Azure sanal makinesine.
- **Model anında** – veri dizine program aracılığıyla gönderilir. Bu, bu makaledeki benimsenen modelidir.

A `SearchIndexClient` örneği dizinine veri aktarmak için oluşturulması gerekir. Bu çağırarak gerçekleştirilebilir `SearchServiceClient.Indexes.GetClient` aşağıdaki kod örneğinde gösterildiği şekilde yöntemi:

```csharp
static void UploadDataToSearchIndex()
{
  var indexClient = searchClient.Indexes.GetClient(Constants.Index);

  var monkeyList = MonkeyData.Monkeys.Select(m => new
  {
    id = Guid.NewGuid().ToString(),
    name = m.Name,
    location = m.Location,
    details = m.Details,
    imageUrl = m.ImageUrl
  });

  var batch = IndexBatch.New(monkeyList.Select(IndexAction.Upload));
  try
  {
    indexClient.Documents.Index(batch);
  }
  catch (IndexBatchException ex)
  {
    // Sometimes when the Search service is under load, indexing will fail for some
    // documents in the batch. Compensating actions like delaying and retrying should be taken.
    // Here, the failed document keys are logged.
    Console.WriteLine("Failed to index some documents: {0}",
      string.Join(", ", ex.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
  }
}
```

Olarak dizine alınacak veri paketlenmiş bir `IndexBatch` koleksiyonunu yalıtır nesne `IndexAction` nesneleri. Her `IndexAction` örnek bir belge ve Azure Search belge üzerinde gerçekleştirilecek eylemi söyleyen bir özellik içerir. Yukarıdaki kod örneğinde `IndexAction.Upload` eylemi, belgenin yeni olması durumunda dizine eklenmekte hangi sonuçlarında belirtilen veya zaten varsa değiştirildi. `IndexBatch` Nesne sonra gönderilir dizine çağırarak `Documents.Index` yöntemi `SearchIndexClient` nesnesi. Diğer dizin oluşturma eylemleri hakkında daha fazla bilgi için bkz: [kullanmak için hangi dizin oluşturma eyleminin karar](/azure/search/search-import-data-dotnet#ii-decide-which-indexing-action-to-use).

> [!NOTE]
> Tek bir dizin oluşturma isteğine yalnızca 1000 belge dahil edilebilir.

Yukarıdaki kod örneğinde unutmayın `monkeyList` koleksiyonu koleksiyonundan anonim bir nesne olarak oluşturulur `Monkey` nesneleri. Bu verileri oluşturur `id` alan ve Pascal durumda eşleme çözümler `Monkey` özellik adlarını ortası büyük için arama dizini alan adları. Alternatif olarak, bu eşlemeyi da ekleyerek gerçekleştirilebilir `[SerializePropertyNamesAsCamelCase]` özniteliğini `Monkey` sınıfı.

Daha fazla bilgi için bkz: [.NET SDK kullanarak Azure Search'te karşıya veri](/azure/search/search-import-data-dotnet/).

## <a name="querying-the-azure-search-index"></a>Azure Search dizini sorgulama

A `SearchIndexClient` örneği oluşturulan, dizin sorgulanamıyor. Bir uygulama sorguları yürütüldüğünde, en az ayrıcalık ilkesini izleyin ve oluşturmak için önerilir; bir `SearchIndexClient` doğrudan geçirme *sorgu anahtarını* bağımsız değişken olarak. Bu, kullanıcıların dizinler ve belgeler için salt okunur erişimi olmasını sağlar. Bu yaklaşım, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
SearchIndexClient indexClient =
  new SearchIndexClient(Constants.SearchServiceName, Constants.Index, new SearchCredentials(Constants.QueryApiKey));
```

`SearchIndexClient` Oluşturucusu aşırı bir arama hizmeti adınızı, dizin adı alır ve bir `SearchCredentials` ile bağımsız değişken olarak nesne `SearchCredentials` nesne kaydırma *sorgu anahtarını* Azure Search hizmeti için.

### <a name="search-queries"></a>Arama sorguları

Dizin çağırarak sorgulanabilir `Documents.SearchAsync` yöntemi `SearchIndexClient` , aşağıdaki kod örneğinde gösterildiği şekilde örneği:

```csharp
async Task AzureSearch(string text)
{
  Monkeys.Clear();

  var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text);
  foreach (SearchResult<Monkey> result in searchResults.Results)
  {
    Monkeys.Add(new Monkey
    {
      Name = result.Document.Name,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SearchAsync` Yöntemi alır arama metni bağımsız değişkeni ve isteğe bağlı bir `SearchParameters` daha fazla sorguyu daraltmak için kullanılan nesne. Filtre sorgusunun ayarlayarak belirtilebilir sırada bir arama sorgusu arama metni bağımsız değişkeni belirtilen `Filter` özelliği `SearchParameters` bağımsız değişkeni. Aşağıdaki kod örneğinde, her ikisi de sorgu türleri gösterilmektedir:

```csharp
var parameters = new SearchParameters
{
  Filter = "location ne 'China' and location ne 'Vietnam'"
};
var searchResults = await indexClient.Documents.SearchAsync<Monkey>(text, parameters);
```

Bu filtre sorgusu tüm dizine uygulanır ve belgeleri sonuçlarından kaldırır nerede `location` alan Çin eşit ve Vietnam eşit değil. Filtreleme sonrasında, arama sorgusu filtre sorgusu sonuçlarını gerçekleştirilir.

> [!NOTE]
> Arama yapmadan filtrelemek üzere geçirmek `*` arama metni bağımsız değişkeni olarak.

`SearchAsync` Yöntemi döndürür bir `DocumentSearchResult` sorgu sonuçlarını içeren nesne. Bu nesne, her biri numaralandırılır `Document` olarak oluşturulan nesne bir `Monkey` nesne ve eklenen `Monkeys` `ObservableCollection` görüntülemek için. Azure aramadan döndürülen aşağıdaki ekran görüntüleri Göster arama sorgusu sonuçlarını:

![](azure-search-images/search.png "Arama sonuçları")

Arama ve filtreleme hakkında daha fazla bilgi için bkz: [.NET SDK kullanarak Azure Search dizininizi sorgulama](/azure/search/search-query-dotnet/).

<a name="suggestions" />

### <a name="suggestion-queries"></a>Öneri sorguları

Azure arama özelliğine imkan tanıyan istenmesi için öneriler çağırarak bir arama sorgusuna dayalı `Documents.SuggestAsync` yöntemi `SearchIndexClient` örneği. Bu, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
async Task AzureSuggestions(string text)
{
  Suggestions.Clear();

  var parameters = new SuggestParameters()
  {
    UseFuzzyMatching = true,
    HighlightPreTag = "[",
    HighlightPostTag = "]",
    MinimumCoverage = 100,
    Top = 10
  };

  var suggestionResults =
    await indexClient.Documents.SuggestAsync<Monkey>(text, "nameSuggester", parameters);

  foreach (var result in suggestionResults.Results)
  {
    Suggestions.Add(new Monkey
    {
      Name = result.Text,
      Location = result.Document.Location,
      Details = result.Document.Details,
      ImageUrl = result.Document.ImageUrl
    });
  }
}
```

`SuggestAsync` Yöntemi (yani dizinde tanımlanır), bir arama metni bağımsız değişken kullanmak için öneri aracı adını alır ve isteğe bağlı bir `SuggestParameters` daha fazla sorguyu daraltmak için kullanılan nesne. `SuggestParameters` Örneği aşağıdaki özellikleri ayarlar:

- `UseFuzzyMatching` – ayarlandığında `true`, Azure Search, arama metni eksik veya yedek karakter olsa öneriler bulacaksınız.
- `HighlightPreTag` – öneri İsabetli Okuma $a etiketi.
- `HighlightPostTag` – öneri İsabetli Okuma eklenir etiketi.
- `MinimumCoverage` – temsil eden başarılı olması bir sorgu için bir öneri sorgu tarafından ele alınması gereken dizin yüzdesi bildirdi. Varsayılan 80'dir.
- `Top` – almak için öneriler sayısı. 1 ile 5 varsayılan değeri ile 100 arasında bir tamsayı olmalıdır.

En iyi 10 sonuç dizinden ile İsabet vurgulama ve sonuçları benzer şekilde arama terimleri yazılmış içeren belgeleri içerecektir döndürüleceğini genel etkisidir.

`SuggestAsync` Yöntemi döndürür bir `DocumentSuggestResult` sorgu sonuçlarını içeren nesne. Bu nesne, her biri numaralandırılır `Document` olarak oluşturulan nesne bir `Monkey` nesne ve eklenen `Monkeys` `ObservableCollection` görüntülemek için. Aşağıdaki ekran görüntüleri Azure aramadan döndürülen öneri sonuçları göster:

![](azure-search-images/suggest.png "Öneri sonuçları")

Örnek uygulama unutmayın `SuggestAsync` yöntemi yalnızca kullanıcı bir arama terimi giriş yapma tamamlandığında çağrılır. Ancak, bu da her keypress üzerinde yürüterek otomatik tamamlama arama sorguları desteklemek için kullanılabilir.

## <a name="summary"></a>Özet

Bu makalede, Microsoft Azure arama kitaplığı bir Xamarin.Forms uygulamasına Azure Search tümleştirmek için nasıl kullanılacağı gösterilmektedir. Azure Search dizin oluşturma ve karşıya yüklenen veriler için özellikleri sorgulama sağlayan bir bulut hizmetidir. Bu, geleneksel bir uygulamada arama işlevini uygulama ile ilişkili arama algoritması karmaşıklık ve altyapı gereksinimleri kaldırır.


## <a name="related-links"></a>İlgili bağlantılar

- [Azure arama (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureSearch/)
- [Azure Search belgeleri](/azure/search/)
- [Microsoft Azure Search Library](https://www.nuget.org/packages/Microsoft.Azure.Search/)
