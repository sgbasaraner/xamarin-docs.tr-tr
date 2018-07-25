---
title: Xamarin.iOS ile ADO.NET kullanma
description: Bu belge, SQLite bir Xamarin.iOS uygulamasına erişmek için bir yöntem olarak ADO.NET kullanmayı açıklar. Bütünleştirilmiş kod başvuruları ve Mono.Data.Sqlite BasicDataAccess örnek ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 83f6059c405b2156270f4359cbba33177861af02
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241244"
---
# <a name="using-adonet-with-xamarinios"></a>Xamarin.iOS ile ADO.NET kullanma

Xamarin iOS, alışık olduğunuz ADO.NET benzeri sözdizimi kullanarak açık kullanılabilir SQLite veritabanı için yerleşik destek sunmaktadır. Bu API'leri kullanarak, gerektirir SQLite tarafından gibi işlenir SQL deyimleri yazmanıza `CREATE TABLE`, `INSERT` ve `SELECT` deyimleri.

## <a name="assembly-references"></a>Derleme başvuruları

SQLite eklemelisiniz ADO.NET aracılığıyla erişimi kullanmak için `System.Data` ve `Mono.Data.Sqlite` (Mac ve Visual Studio için Visual Studio örnekleri için) burada gösterildiği gibi iOS projenizi başvuruyor:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 ![](using-adonet-images/image4.png "Mac için Visual Studio'da derleme başvuruları")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Visual Studio'da derleme başvuruları")

-----

Sağ **başvuruları > başvuruları Düzenle...**  sonra gerekli derlemeleri seçmek için tıklayın.

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite hakkında

Kullanacağız `Mono.Data.Sqlite.SqliteConnection` bir boş veritabanı oluşturmak için sınıf örneği oluşturmak için sonra da `SqliteCommand` nesneleri biz veritabanına karşı SQL yönergeleri yürütmek için kullanabilirsiniz.


1. **Boş veritabanı oluşturma** -çağrı `CreateFile` geçerli bir yöntemle (IE. yazılabilir) dosya yolu. Dosya zaten bu yöntemi çağırmadan önce Aksi takdirde eski üst yeni (boş) bir veritabanı oluşturulur ve eski dosyasındaki veriler kaybolur olup olmadığını incelemeniz gerekir:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
    > `dbPath` Değişkeni bu belgenin önceki bölümlerinde açıklanan kurallara göre belirlenemedi.

2. **Veritabanı bağlantısı oluşturma** - SQLite veritabanı dosyası oluşturulduktan sonra verilere erişmek için bir bağlantı nesnesi oluşturabilirsiniz. Bağlantı biçiminde alan bir bağlantı dizesi ile oluşturulmuş olan `Data Source=file_path`, burada gösterildiği gibi:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Daha önce bahsedildiği gibi bir bağlantı hiçbir zaman farklı iş parçacıkları arasında yeniden kullanılan olmamalıdır. Bu konuda şüpheleriniz varsa gerektiği gibi bir bağlantı oluşturun ve işiniz bittiğinde kapatın; Ancak, bu çok genellikle çok gerekli yapmanın oluşturduğunu unutmayın.
    
3. **Oluşturma ve bir veritabanı komutu yürütülürken** - sonra biz bunu rastgele SQL komutları yürütebilir bağlantı sahibiz. Aşağıdaki kod yürütülmekte olan bir CREATE TABLE deyimi göstermektedir.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

Doğrudan veritabanına karşı SQL yürütürken gibi zaten var olan bir tablo oluşturulmaya çalışılırken geçersiz istekleri yapmamak için normal önlemleri almalıdır. Veritabanınızı yapısı gibi "SQLite hata tablosundaki [öğeleri] zaten var" bir SqliteException neden olmaz, takip edin.

## <a name="basic-data-access"></a>Temel veri erişimi

*DataAccess_Basic* bu belge için örnek kod şuna benzer iOS üzerinde çalışırken:

 ![](using-adonet-images/image9.png "iOS ADO.NET örneği")

Aşağıdaki kod, basit SQLite işlemlerini nasıl gerçekleştireceğinizi gösterir ve uygulamanın ana pencere metin olarak sonuçları gösterir.

Bu ad alanlarını içerecek şekilde gerekir:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Aşağıdaki kod örneği, veritabanının tamamını etkileşim gösterilmektedir:

1.  Veritabanı dosyası oluşturma
2.  Bazı veri ekleme
3.  Veriyi sorgulama

Bu işlemlerin genellikle kodunuz genelinde birden çok yerde Örneğin, uygulamanızın ilk kez başlatıldığında tablo ve veritabanı dosyası oluşturabilir ve veri okuma ve yazma işlemleri, uygulamanızda bireysel ekranlara gerçekleştirmek görünür. Aşağıdaki örnekte bu örneğin tek bir yöntem içinde gruplandırılır:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}
```

## <a name="more-complex-queries"></a>Daha karmaşık sorgular

SQLite karşı verileri çalıştırmayı rastgele SQL komutlarını izin verdiğinden, her şeyi oluşturmak, eklemek, GÜNCELLEŞTİRMEK, silmek veya SELECT deyimleri istediğiniz gerçekleştirebilirsiniz. Sqlite Web sitesinde SQLite tarafından desteklenen SQL komutlar hakkında bilgi edinebilirsiniz. SQL deyimlerini üç yöntemden birini kullanarak bir SqliteCommand nesne üzerinde çalıştırılır:

-  **ExecuteNonQuery** – genellikle, tablo oluşturma veya veri ekleme için kullanılır. Bazı işlemler için dönüş değeri etkilenen satır sayısı, aksi takdirde, -1'dir.
-  **ExecuteReader** – satır koleksiyonu olarak döndürülmesi kullanılan bir `SqlDataReader` .
-  **ExecuteScalar** – (örneğin bir toplama) tek bir değer alır.


### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT, UPDATE ve DELETE deyimleri etkilenen satır sayısını döndürür. Diğer tüm SQL deyimleri -1 döndürür.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Aşağıdaki yöntem bir WHERE yan tümcesi SELECT deyiminde gösterir. Kod, tam bir SQL deyimi kaynaklı olduğundan, dizeleri etrafında tırnak işareti (')'gibi ayrılmış karakterleri kaçış ilgileniriz gerekir.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

ExecuteReader metodunu SqliteDataReader nesnesi döndürür. Read yöntemi örnekte gösterilen yanı sıra diğer yararlı özellikler şunlardır:

-  **RowsAffected** – sorgu tarafından etkilenen satırların sayısı.
-  **HasRows** : herhangi bir satır döndürülmedi.


### <a name="executescalar"></a>EXECUTESCALAR

(Örneğin, bir toplama) tek bir değer döndüren SELECT deyimleri için bunu kullanın.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Yöntemin dönüş türü `object` – sonucu veritabanı sorguya bağlı olarak atamanız gerekir. Sonuç sayısı sorgudan bir tamsayı ya da bir dizeden tek sütun seçme sorgusu olabilir. Bu Okuyucu nesnesi veya etkilenen satır sayısını döndüren diğer yürütme yöntemler farklı olduğunu unutmayın.


## <a name="related-links"></a>İlgili bağlantılar

- [DataAccess Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarifleri](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
