---
title: SQLite.NET Android ile kullanma
description: SQLite.NET PCL NuGet kitaplığı Xamarin.Android uygulamaları için bir basit veri erişim mekanizma sağlar.
ms.prod: xamarin
ms.assetid: 3447B7EE-A320-489E-AF02-E5721097760A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/18/2018
ms.openlocfilehash: e8e6e98cb6ada8d8da494e408e8db66ad5038799
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
---
# <a name="using-sqlitenet-with-android"></a>SQLite.NET Android ile kullanma

Xamarin önerdiği SQLite.NET Android cihazda yerel SQLite veritabanındaki nesneler kolayca depolanıp olanak sağlayan çok basit bir ORM kitaplığıdır. ORM temsil eden nesne ilişkisel eşlemek için &ndash; kaydedin ve "nesneler" SQL deyimleri yazmak zorunda kalmadan veritabanından olanak sağlayan bir API.

Bir Xamarin uygulaması SQLite.NET kitaplığı eklemek için aşağıdaki NuGet paketini projenize ekleyin:

- **Paket adı:** sqlite net pcl
- **Yazar:** Frank A. Krueger
- **Kimliği:** sqlite net pcl
- **URL:** [nuget.org/packages/sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

[![SQLite.NET NuGet paketi](using-sqlite-orm-images/image1a-sml.png "SQLite.NET NuGet paketi")](using-sqlite-orm-images/image1a.png#lightbox)

> [!TIP]
> Kullanılabilir farklı SQLite paket sayısı – (Bu üst arama sonucu olmayabilir) doğru olanı seçtiğinizden emin olun.

SQLite.NET kitaplığının kullanılabilir olduktan sonra bir veritabanına erişmek için kullanmak için aşağıdaki üç adımı izleyin:

1.  **Kullanarak bir ekleme deyimi** &ndash; veri erişimi olduğu gerekli C# dosyalarını için aşağıdaki ifadeyi ekleyin:

    ```csharp
    using SQLite;
    ```

2.  **Boş bir veritabanı oluşturun** &ndash; SQLiteConnection sınıfı oluşturucusu dosya yolunu geçirerek bir veritabanı başvurusu oluşturulabilir. Dosya zaten var olup olmadığını denetlemek gerekmez &ndash; gerektiğinde otomatik olarak oluşturulur, aksi halde var olan veritabanı dosyasını açılacak. `dbPath` Değişkeni bu belgenin önceki bölümlerinde açıklanan kuralları göre belirlenemedi:

    ```csharp
    var db = new SQLiteConnection (dbPath);
    ```

3.  **Verileri Kaydet** &ndash; SQLiteConnection nesne oluşturduktan sonra veritabanı komutları CreateTable ve bu gibi ekleme gibi kendi yöntemler çağrılarak çalıştırılır:

    ```csharp
    db.CreateTable<Stock> ();
    db.Insert (newStock); // after creating the newStock object
    ```

4.  **Verileri** &ndash; almak için bir nesne (veya nesnelerin bir listesini) aşağıdaki sözdizimini kullanın:

    ```csharp
    var stock = db.Get<Stock>(5); // primary key id of 5
    var stockList = db.Table<Stock>();
    ```

## <a name="basic-data-access-sample"></a>Temel veri erişim örneği

*DataAccess_Basic* bu belge için örnek kod şu şekilde görünür Android çalıştırırken. Kod basit SQLite.NET işlemleri gerçekleştirmek nasıl gösterir ve sonuçları, uygulamanın ana penceresinde metin olarak gösterilir.


**Android**

![Android SQLite.NET örnek](using-sqlite-orm-images/image3.png "Android SQLite.NET örnek")

Aşağıdaki kod örneği, temel alınan veritabanı erişimi kapsülleyen SQLite.NET kitaplığı kullanarak tüm veritabanını etkileşim gösterir.
Bunu gösterir:

1.  Veritabanı dosyası oluşturma

2.  Bazı veri nesnesi oluşturma ve bunları kaydederek ekleme

3.  Veriyi sorgulama

Bu ad alanlarını dahil yapmanız gerekir:

```csharp
using SQLite; // from the github SQLite.cs class
```

Sonuncu projenize eklenen SQLite gerektirir. SQLite veritabanı tablosu için bir sınıf öznitelikleri ekleyerek tanımlanır unutmayın ( `Stock` sınıfı) yerine bir CREATE TABLE komutu.

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

-   **[PrimaryKey]**  &ndash; Bu özniteliği temel tablonun birincil anahtarı olmasını zorunlu kılmak için bir tamsayı özelliği uygulanabilir. Birleşik birincil anahtarlar desteklenmez.

-   **[AutoIncrement]**  &ndash; Bu öznitelik bir tamsayı özelliğin değeri veritabanına eklenen her yeni nesne için otomatik artım olacak şekilde neden olur

-   **[Column(name)]**  &ndash; İsteğe bağlı sağladığını `name` parametresi geçersiz kılma özelliği aynıdır) temel alınan veritabanı sütunun adı (varsayılan değeri.

-   **[Table(name)]**  &ndash; Sınıfı temel bir SQLite tablo içinde saklanan yapamamasına olarak işaretler. İsteğe bağlı adı parametresini belirterek sınıf adı aynı olan) temel alınan veritabanı tablosunun adı (varsayılan değerini geçersiz kılar.

-   **[MaxLength(value)]**  &ndash; Veritabanı Ekle çalışırken bir metin özelliğini uzunluğunu kısıtla. Kod kullanan bu bu özniteliği yalnızca 'veritabanı Ekle denetlenir' veya güncelleştirme işlemi deneyen gibi nesne eklemeden önce doğrulamalıdır.

-   **[Yoksay]**  &ndash; Bu özellik yoksaymak için SQLite.NET neden olur.
    Veritabanında depolanan bir türe sahip özellikleri veya özellikleri otomatik olarak çözümlenemiyor modeli koleksiyonları SQLite olması için bu özellikle yararlıdır.

-   **[Benzersiz]**  &ndash; Temel alınan veritabanı sütunundaki değerler benzersiz olmasını sağlar.


Bu öznitelikler çoğunu isteğe bağlıdır, SQLite tablo ve sütun adları için varsayılan değerleri kullanır. Seçim ve silme sorguları verilerinizde verimli bir şekilde gerçekleştirilebilir böylece her zaman bir tamsayı birincil anahtar belirtmeniz gerekir.

## <a name="more-complex-queries"></a>Daha karmaşık sorgular

Aşağıdaki yöntemlerden `SQLiteConnection` diğer veri işlemleri gerçekleştirmek için kullanılabilir:

-   **INSERT** &ndash; veritabanına yeni bir nesne ekler.

-   **Alma&lt;T&gt;**  &ndash; birincil anahtarı kullanan bir nesne almaya çalışır.

-   **Tablo&lt;T&gt;**  &ndash; tablodaki tüm nesneleri döndürür.

-   **Silme** &ndash; birincil anahtarı kullanarak nesneyi siler.

-   **Sorgu&lt;T&gt;**  &ndash; (nesneler) olarak satır sayısı döndüren bir SQL sorgusunu uygulayın.

-   **Yürütme** &ndash; bu yöntemi kullanın (ve `Query`) ne zaman yok beklediğiniz satırları geri SQL (örneğin, INSERT, UPDATE ve DELETE yönergeleri).


### <a name="getting-an-object-by-the-primary-key"></a>Bir nesne birincil anahtar ile Başlarken

SQLite.Net birincil anahtarı üzerinde göre tek bir nesne almak için Get yöntemi sağlar.

```csharp
var existingItem = db.Get<Stock>(3);
```

### <a name="selecting-an-object-using-linq"></a>LINQ kullanarak bir nesne seçme

Koleksiyonları döndüren yöntemler Destek `IEnumerable<T>` LINQ Sorgu veya bir tablo içeriğini sıralamak için kullanabilirsiniz. Aşağıdaki kod, "A" harfiyle başlayan tüm girişleri filtrelemek için LINQ kullanarak bir örnek gösterilmektedir:

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

> [!NOTE]
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

SQLite Android sürümü birkaç adım gerektiren bir kısıtlaması vardır. Varsa çağrısı `SqliteConnection.SetConfig` gibi bir SQLite özel durum üreten `library used incorrectly`, aşağıdaki geçici çözüm kullanmanız gerekir:

1.  Yerel bağlantı **libsqlite.so** kitaplığı böylece `sqlite3_shutdown` ve `sqlite3_initialize` API uygulamasına sunulur:

    ```csharp
    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_shutdown();

    [DllImport("libsqlite.so")]
    internal static extern int sqlite3_initialize();
    ```


2.  Çok başındaki `OnCreate` yöntemi, SQLite kapatmak için bu kodu ekleyin, için yapılandırma **seri hale getirilmiş** modu ve SQLite yeniden başlatın:

    ```csharp
    sqlite3_shutdown();
    SqliteConnection.SetConfig(SQLiteConfig.Serialized);
    sqlite3_initialize();
    ```

Bu geçici çözüm için de çalışır. `Mono.Data.Sqlite` kitaplığı. Ve çoklu iş parçacığı SQLite hakkında daha fazla bilgi için bkz: [SQLite ve birden çok iş parçacığı](https://www.sqlite.org/threadsafe.html). 

## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarif](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
