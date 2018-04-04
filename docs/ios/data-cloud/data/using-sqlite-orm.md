---
title: SQLite.NET kullanma
ms.prod: xamarin
ms.assetid: 79813B09-42D7-47DD-AE71-A605E6B9EF24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2018
ms.openlocfilehash: 8d68df2c29afe828482da7c5747b30dc5d30a5de
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="using-sqlitenet"></a>SQLite.NET kullanma

Xamarin önerdiği SQLite.NET bir iOS cihazında yerel SQLite veritabanındaki nesneler depolanıp olanak sağlayan bir temel ORM kitaplığıdır.
Nesne İlişkisel eşleme – kaydedin ve "nesneler" SQL deyimleri yazmak zorunda kalmadan veritabanından olanak sağlayan bir API ORM gösterir.

<a name="Usage"/>

## <a name="usage"></a>Kullanım

Ekleme [SQLite.net PCL NuGet paketi](https://www.nuget.org/packages/sqlite-net-pcl/), isteğe bağlı olarak, projenize - bir gibi çeşitli platformlardan iOS, Android ve Windows destekler.

  [![](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet paketi")](using-sqlite-orm-images/image1a.png#lightbox)

SQLite.NET kitaplığının kullanılabilir olduktan sonra bir veritabanına erişmek için kullanmak için aşağıdaki üç adımı izleyin:


1. **Kullanarak bir ekleme deyimi** -veri erişimi olduğu gerekli C# dosyalarını için aşağıdaki ifadeyi ekleyin:

        using SQLite;

1. **Boş bir veritabanı oluşturun** -SQLiteConnection sınıfı oluşturucusu dosya yolunu geçirerek bir veritabanı başvurusu oluşturulabilir. Dosya zaten var. – otomatik olarak oluşturulur, gerekli, aksi halde var olan veritabanı dosyasını açılacak varsa denetlemek gerekmez.

        var db = new SQLiteConnection (dbPath);

    Bu belgede açıklanan kuralları göre dbPath değişkeni belirlenmesi.

1. **Verileri Kaydet** - SQLiteConnection nesne, komutları CreateTable ve bu gibi ekleme gibi kendi yöntemler çağrılarak çalıştırılır veritabanı oluşturduktan sonra:

        db.CreateTable<Stock> ();
        db.Insert (newStock); // after creating the newStock object

1. **Verileri** - almak için bir nesne (veya nesnelerin bir listesini) aşağıdaki sözdizimini kullanın:

        var stock = db.Get<Stock>(5); // primary key id of 5
        var stockList = db.Table<Stock>();

## <a name="basic-data-access-sample"></a>Temel veri erişim örneği

*DataAccess_Basic* bu belge için örnek kod şu şekilde görünür İos'ta çalıştırırken. Kod basit SQLite.NET işlemleri gerçekleştirmek nasıl gösterir ve sonuçları, uygulamanın ana penceresinde metin olarak gösterilir.

**iOS**

 ![](using-sqlite-orm-images/image2.png "iOS SQLite.NET örnek")

Aşağıdaki kod örneği, temel alınan veritabanı erişimi kapsülleyen SQLite.NET kitaplığı kullanarak tüm veritabanını etkileşim gösterir. Bunu gösterir:

1.  Veritabanı dosyası oluşturma
1.  Bazı veri nesnesi oluşturma ve bunları kaydederek ekleme
1.  Veriyi sorgulama


Bu ad alanlarını dahil yapmanız gerekir:

```csharp
using SQLite; // from the github SQLite.cs class
```

Bu vurgulanan projenize SQLite eklediğiniz gerektirir [burada](#Usage). SQLite veritabanı tablosu için bir sınıf öznitelikleri ekleyerek tanımlanır unutmayın ( `Stock` sınıfı) yerine bir CREATE TABLE komutu.

```csharp
[Table("Items")]
public class Stock {
    [PrimaryKey, AutoIncrement, Column("_id")]
    public int Id { get; set; }
    [MaxLength(8)]
    public string Symbol { get; set; }
}
public static void DoSomeDataAccess () {
       Console.WriteLine ("Creating database, if it doesn't already exist");
   string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "ormdemo.db3");
   var db = new SQLiteConnection (dbPath);
   db.CreateTable<Stock> ();
   if (db.Table<Stock> ().Count() == 0) {
        // only insert the data if it doesn't already exist
        var newStock = new Stock ();
        newStock.Symbol = "AAPL";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "GOOG";
        db.Insert (newStock);
        newStock = new Stock ();
        newStock.Symbol = "MSFT";
        db.Insert (newStock);
    }
    Console.WriteLine("Reading data");
    var table = db.Table<Stock> ();
    foreach (var s in table) {
        Console.WriteLine (s.Id + " " + s.Symbol);
    }
}
```

Kullanarak `[Table]` özniteliği hiçbir belirtmeden bir tablo adı parametresi (Bu durumda "Stok") sınıf ile aynı ada sahip temel alınan veritabanı tablosu neden olur. SQL sorguları doğrudan veritabanına yazmak yerine, ORM veri erişimi yöntemleri kullanmak gerçek tablo adı önemlidir. Benzer şekilde `[Column("_id")]` özniteliği isteğe bağlıdır ve bir sütun sınıfı bir özellik aynı ada sahip tabloya eklenen durumunda.

## <a name="sqlite-attributes"></a>SQLite öznitelikleri

Sınıflarınızı nasıl temel veritabanında depolanan denetlemek için uygulayabileceğiniz ortak öznitelikleri şunlardır:

-  **[PrimaryKey]**  – Temel tablonun birincil anahtarı olmasını zorunlu kılmak için bir tamsayı özelliği bu özniteliği uygulanabilir. Birleşik birincil anahtarlar desteklenmez.
-  **[AutoIncrement]**  – Bu öznitelik bir tamsayı özelliğin değeri veritabanına eklenen her yeni nesne için otomatik artım olacak şekilde neden olur
-  **[Column(name)]**  – İsteğe bağlı sağladığını `name` parametresi geçersiz kılma özelliği aynıdır) temel alınan veritabanı sütunun adı (varsayılan değeri.
-  **[Table(name)]**  – Sınıfı temel bir SQLite tablo içinde saklanan yapamamasına olarak işaretler. İsteğe bağlı adı parametresini belirterek sınıf adı aynı olan) temel alınan veritabanı tablosunun adı (varsayılan değerini geçersiz kılar.
-  **[MaxLength(value)]**  -Veritabanı Ekle çalışırken bir metin özelliğini uzunluğunu kısıtla. Kod kullanan bu bu özniteliği yalnızca 'veritabanı Ekle denetlenir' veya güncelleştirme işlemi deneyen gibi nesne eklemeden önce doğrulamalıdır.
-  **[Yoksay]**  – Bu özellik yoksaymak için SQLite.NET neden olur. Veritabanında depolanan bir türe sahip özellikleri veya özellikleri otomatik olarak çözümlenemiyor modeli koleksiyonları SQLite olması için bu özellikle yararlıdır.
-  **[Benzersiz]**  – Temel alınan veritabanı sütunundaki değerler benzersiz olmasını sağlar.


Bu öznitelikler çoğunu isteğe bağlıdır, SQLite tablo ve sütun adları için varsayılan değerleri kullanır. Seçim ve silme sorguları verilerinizde verimli bir şekilde gerçekleştirilebilir böylece her zaman bir tamsayı birincil anahtar belirtmeniz gerekir.

## <a name="more-complex-queries"></a>Daha karmaşık sorgular

Aşağıdaki yöntemlerden `SQLiteConnection` diğer veri işlemleri gerçekleştirmek için kullanılabilir:

-  **INSERT** – yeni bir nesne veritabanına ekler.
-  **Alma<T>**  – birincil anahtarı kullanan bir nesne almaya çalışır.
-  **Tablo<T>**  – tablodaki tüm nesneleri döndürür.
-  **Silme** – birincil anahtarı kullanarak nesneyi siler.
-  **Sorgu<T>**  -(nesneler) olarak satır sayısı döndüren bir SQL sorgusunu uygulayın.
-  **Yürütme** – bu yöntemi kullanın (ve `Query` ) ne zaman yok beklediğiniz satırları geri SQL (örneğin, INSERT, UPDATE ve DELETE yönergeleri).


### <a name="getting-an-object-by-the-primary-key"></a>Bir nesne birincil anahtar ile Başlarken

SQLite.Net birincil anahtarı üzerinde göre tek bir nesne almak için Get yöntemi sağlar.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>LINQ kullanarak bir nesne seçme

Koleksiyonları döndüren yöntemler destek IEnumerable<T> LINQ Sorgu veya bir tablo içeriğini sıralamak için kullanabilirsiniz. Aşağıdaki kod, "A" harfiyle başlayan tüm girişleri filtrelemek için LINQ kullanarak bir örnek gösterilmektedir:

```csharp
var apple = from s in db.Table<Stock>()
    where s.Symbol.StartsWith ("A")
    select s;
Console.WriteLine ("-> " + apple.FirstOrDefault ().Symbol);
```

### <a name="selecting-an-object-using-sql"></a>SQL kullanarak bir nesne seçme

Bazen SQLite.Net verilerinize nesne tabanlı erişim sağlayabilir olsa bile, LINQ izin verir (veya daha hızlı performans gerekebilir daha) daha karmaşık bir sorgu yapmanız gerekebilir. Aşağıda gösterildiği gibi sorgu yöntemiyle SQL komutlarını kullanabilirsiniz:

```csharp
var stocksStartingWithA = db.Query<Stock>("SELECT * FROM Items WHERE Symbol = ?", "A");
foreach (var s in stocksStartingWithA) {
    Console.WriteLine ("a " + s.Symbol);
}
```

> [!IMPORTANT]
> SQL deyimleri doğrudan yazılırken tabloların adlarını, sınıflar ve onların öznitelikleri oluşturulan sütunları, veritabanınızdaki üzerinde bir bağımlılık oluşturun. Kodunuzda bu adlar değiştirirseniz, tüm el ile yazılmış SQL deyimlerini güncelleştirmek unutmamanız gerekir.

### <a name="deleting-an-object"></a>Bir nesne silme

Birincil anahtar, satır silme için aşağıda gösterildiği gibi kullanılır:

```csharp
var rowcount = db.Delete<Stock>(someStock.Id); // Id is the primary key
```

Kontrol edebilirsiniz `rowcount` satır sayısını (Bu durumda silinir) etkilendi onaylamak için.

## <a name="using-sqlitenet-with-multiple-threads"></a>Birden çok iş parçacığı ile SQLite.NET kullanma

SQLite üç farklı iş parçacığı modlarını destekler: *tek iş parçacığı*, *çoklu iş parçacığı*, ve *seri hale getirilmiş*. Birden çok iş parçacığından herhangi bir kısıtlamanın olmadığı veritabanına erişmek istiyorsanız, kullanılacak SQLite yapılandırabilirsiniz **seri hale getirilmiş** modu iş parçacığı oluşturma. Bu mod, uygulamanızda erken ayarlamak önemlidir (örneğin, başında `OnCreate` yöntemi).

İş parçacığı modunu değiştirmek için çağrı `SqliteConnection.SetConfig`. Örneğin, bu kod satırı için SQLite yapılandırır **seri hale getirilmiş** modu:

```csharp
SqliteConnection.SetConfig(SQLiteConfig.Serialized);
```


## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarif](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
