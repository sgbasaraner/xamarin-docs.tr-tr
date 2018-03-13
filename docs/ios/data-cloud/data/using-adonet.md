---
title: ADO.NET kullanarak
ms.topic: article
ms.prod: xamarin
ms.assetid: 79078A4D-2D24-44F3-9543-B50418A7A000
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b53f98206c100ed76f601937844bf182a6dc146c
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="using-adonet"></a>ADO.NET kullanarak

Xamarin iOS tanıdık ADO.NET benzeri bir sözdizimi kullanılarak kullanıma sunulan, kullanılabilir SQLite veritabanı için yerleşik desteğe sahiptir. Bu API'leri kullanarak, gerektirir SQLite tarafından gibi işlenir SQL deyimleri yazmanıza `CREATE TABLE`, `INSERT` ve `SELECT` deyimleri.

## <a name="assembly-references"></a>Derleme başvuruları

Erişim SQLite eklemelisiniz ADO.NET aracılığıyla kullanılacak `System.Data` ve `Mono.Data.Sqlite` (için Mac ve Visual Studio için Visual Studio Örnekleri) aşağıda gösterildiği gibi iOS projenize başvuruyor:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

 ![](using-adonet-images/image4.png "Mac için Visual Studio'da derleme başvuruları")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  ![](using-adonet-images/image6.png "Visual Studio'da derleme başvuruları")

-----

Sağ **başvuruları > başvurular Düzenle...**  sonra gerekli derlemeleri seçmek için tıklatın.

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite hakkında

Kullanacağız `Mono.Data.Sqlite.SqliteConnection` bir boş veritabanı dosyası oluşturmak için sınıf ve ardından örneği için `SqliteCommand` nesneleri biz veritabanında SQL yönergeleri yürütmek için kullanabilirsiniz.


1. **Boş bir veritabanı oluşturma** -çağrı `CreateFile` geçerli bir yöntemle (IE. yazılabilir) dosya yolu. Bu yöntemi çağırmadan önce dosya zaten var, aksi halde yeni (boş) veritabanı eskisinin üst oluşturulur ve eski dosyasındaki veriler kaybolacak olup olmadığını kontrol edin:

    `Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);`

    > [!NOTE]
> **Not:** dbPath değişkeni bu belgenin önceki bölümlerinde açıklanan kuralları göre belirlenmesi.

2. **Veritabanı bağlantısı oluşturma** - SQLite veritabanı dosyası oluşturulduktan sonra verilere erişmek için bir bağlantı nesnesi oluşturabilirsiniz. Bağlantı biçimini alır sahip bir bağlantı dizesi oluşturulan `Data Source=file_path`, aşağıda gösterildiği gibi:

    ```csharp
    var connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open();
    // do stuff
    connection.Close();
    ```

    Daha önce belirtildiği gibi bir bağlantı hiçbir zaman farklı iş parçacıkları arasında yeniden kullanılan olmalıdır. Emin değilseniz, gerekli olarak bağlantı oluşturun ve tamamladığınızda kapatın; Ancak bu fazla genellikle çok gerekli yapma dikkatli olun.
    
3. **Oluşturma ve bir veritabanı komutu yürütülürken** - biz bunu rasgele SQL komutunu yürütebilirsiniz bağlantı sahibiz sonra. Aşağıdaki kod, yürütülmekte olan bir CREATE TABLE deyimi gösterir.

    ```csharp
    using (var command = connection.CreateCommand ()) {
        command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
        var rowcount = command.ExecuteNonQuery ();
    }
    ```

SQL veritabanına karşı doğrudan yürütülürken zaten bir tablo oluşturma girişimi gibi geçersiz istek yapmamak için normal önlemler almanız gerekir. Veritabanınızın yapısını "SQLite hata tablosundaki [öğeleri] zaten var."gibi bir SqliteException neden olmayan böylece takip edin.

## <a name="basic-data-access"></a>Temel veri erişimi

*DataAccess_Basic* bu belge için örnek kod şu şekilde görünür İos'ta çalışırken:

 ![](using-adonet-images/image9.png "iOS ADO.NET örnek")

Aşağıdaki kodu basit SQLite işlemleri gerçekleştirmek nasıl gösterir ve sonuçları, uygulamanın ana penceresinde metin olarak gösterilir.

Bu ad alanlarını dahil yapmanız gerekir:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

Aşağıdaki kod örneği, tüm veritabanı etkileşim gösterir:

1.  Veritabanı dosyası oluşturma
2.  Bazı veri ekleme
3.  Veriyi sorgulama

Bu işlem genellikle, kodunuzu boyunca birden çok yerde Örneğin, uygulamanızı ilk başladığında tablo ve veritabanı dosyası oluşturabilir ve veri okuma ve yazma işlemleri ayrı ayrı ekranları uygulamanızda gerçekleştirmek görüntülenir. Aşağıdaki örnekte bu örnek için tek bir yöntem halinde gruplandırılır:

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

Veri karşı çalıştırmak için rasgele SQL komutlarını SQLite izin verdiğinden, ne olursa olsun oluşturmak, eklemek, GÜNCELLEŞTİRMEK, silmek veya SELECT istediğiniz deyimleri gerçekleştirebilirsiniz. Sqlite Web sitesinde SQLite tarafından desteklenen SQL komutları hakkında bilgi edinebilirsiniz. SQL deyimleri üç yöntemden birini kullanarak SqliteCommand nesne üzerinde çalışan:

-  **ExecuteNonQuery** – genellikle tablo oluşturma veya veri eklemek için kullanılır. Bazı işlemler için dönüş değerini etkilenen satırların sayısı, aksi takdirde, -1'dir.
-  **ExecuteReader** – satır koleksiyonu olarak döndürülmelidir çağrılırken bir `SqlDataReader` .
-  **ExecuteScalar** – (örneğin bir toplama) tek bir değer alır.


### <a name="executenonquery"></a>EXECUTENONQUERY

INSERT, UPDATE ve DELETE deyimleri etkilenen satırların sayısını döndürür. Diğer tüm SQL deyimlerini -1 döndürür.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Aşağıdaki yöntem WHERE yan tümcesi SELECT deyiminde gösterir. Kodu tam bir SQL deyimi oluşturmak için bu ayrılmış karakterleri dizeleri etrafında tırnak işareti (') gibi kaçınmak için dikkatli olun gerekir.

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

ExecuteReader yöntemi bir SqliteDataReader nesnesi döndürür. Örnekte gösterilen Read yöntemi yanı sıra diğer yararlı özellikleri şunlardır:

-  **RowsAffected** – sorgu tarafından etkilenen satırların sayısı.
-  **HasRows** – herhangi bir satır döndürülmedi olup olmadığını.


### <a name="executescalar"></a>EXECUTESCALAR

(Örneğin, bir toplama) tek bir değer döndürmesi SELECT deyimleri için bunu kullanın.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Yöntemin dönüş türü `object` – veritabanı sorgusu bağlı olarak sonuç atamalısınız. Sonuç sayısı sorgudan bir tamsayı ya da tek sütun seçme sorgusunun bir dize olabilir. Okuyucu nesnesi veya etkilenen satırların sayısını veren diğer Execute yöntemleri için farklı olduğunu unutmayın.


## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS veri tarif](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
