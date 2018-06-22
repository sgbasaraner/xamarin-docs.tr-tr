---
title: Android ile ADO.NET kullanarak
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 29e81afdf2c46cdefc68e2c2fae4e6e47999a346
ms.sourcegitcommit: 797597d902330652195931dec9ac3e0cc00792c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/20/2018
ms.locfileid: "31646787"
---
# <a name="using-adonet-with-android"></a>Android ile ADO.NET kullanarak

Xamarin Android üzerinde kullanılabilir ve tanıdık ADO.NET benzeri sözdizimi kullanarak verilebilen SQLite veritabanı için yerleşik desteğe sahiptir. Bu API'leri kullanarak, gerektirir SQLite tarafından gibi işlenir SQL deyimleri yazmanıza `CREATE TABLE`, `INSERT` ve `SELECT` deyimleri.

## <a name="assembly-references"></a>Derleme başvuruları

Erişim SQLite eklemelisiniz ADO.NET aracılığıyla kullanılacak `System.Data` ve `Mono.Data.Sqlite` aşağıda gösterildiği gibi Android projenize başvuruyor:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin) 

![Visual Studio'da Android başvuruları](using-adonet-images/image7.png "Android başvuran Visual Studio'da") 

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac) 

![Mac için Visual Studio'da Android başvuruları](using-adonet-images/image5.png "Android, Mac için Visual Studio'da başvurur") 

-----


Sağ **başvuruları > başvurular Düzenle...**  sonra gerekli derlemeleri seçmek için tıklatın.

## <a name="about-monodatasqlite"></a>Mono.Data.Sqlite hakkında

Kullanacağız `Mono.Data.Sqlite.SqliteConnection` bir boş veritabanı dosyası oluşturmak için sınıf ve ardından örneği için `SqliteCommand` nesneleri biz veritabanında SQL yönergeleri yürütmek için kullanabilirsiniz.

**Boş bir veritabanı oluşturma** &ndash; çağrısı `CreateFile` geçerli bir yöntemle (IE. yazılabilir) dosya yolu. Bu yöntemi çağırmadan önce dosya zaten var, aksi halde yeni (boş) veritabanı eskisinin üst oluşturulur ve eski dosyasındaki veriler kaybolacak olup olmadığını denetlemelisiniz.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` `dbPath` Değişkeni bu belgenin önceki bölümlerinde açıklanan kuralları göre belirlenemedi.

**Veritabanı bağlantısı oluşturma** &ndash; SQLite veritabanı dosyası oluşturulduktan sonra verilere erişmek için bir bağlantı nesnesi oluşturabilirsiniz. Bağlantı biçimini alır sahip bir bağlantı dizesi oluşturulan `Data Source=file_path`, aşağıda gösterildiği gibi:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Daha önce belirtildiği gibi bir bağlantı hiçbir zaman farklı iş parçacıkları arasında yeniden kullanılan olmalıdır. Emin değilseniz, gerekli olarak bağlantı oluşturun ve tamamladığınızda kapatın; Ancak bu fazla genellikle çok gerekli yapma dikkatli olun.

**Oluşturma ve bir veritabanı komutu yürütülürken** &ndash; bağlantı sahibiz sonra biz bunu karşı rasgele SQL komutları çalıştırabilirsiniz. Aşağıdaki kodu gösterir bir `CREATE TABLE` yürütülmekte olan ifade.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

SQL veritabanına karşı doğrudan yürütülürken zaten bir tablo oluşturma girişimi gibi geçersiz istek yapmamak için normal önlemler almanız gerekir. Veritabanınızın yapısını neden olmayan böylece izlemek bir `SqliteException` gibi **SQLite hata tablosundaki [öğeleri] zaten**.

## <a name="basic-data-access"></a>Temel veri erişimi

*DataAccess_Basic* bu belge için örnek kod şu şekilde görünür Android üzerinde çalışırken:

![Android ADO.NET örnek](using-adonet-images/image8.png "Android ADO.NET örnek")

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

Veri karşı çalıştırmak için rasgele SQL komutlarını SQLite izin verdiğinden, ne olursa olsun gerçekleştirebilirsiniz `CREATE`, `INSERT`, `UPDATE`, `DELETE`, veya `SELECT` istediğiniz deyimleri. Sqlite Web sitesinde SQLite tarafından desteklenen SQL komutları hakkında bilgi edinebilirsiniz. SQL deyimleri üç yöntemden birini kullanarak çalışan bir `SqliteCommand` nesnesi:

-   **ExecuteNonQuery** &ndash; genellikle tablo oluşturma veya veri ekleme için kullanılır. Bazı işlemler için dönüş değerini etkilenen satırların sayısı, aksi takdirde, -1'dir.

-   **ExecuteReader** &ndash; satır koleksiyonu olarak döndürülmelidir çağrılırken bir `SqlDataReader`.

-   **ExecuteScalar** &ndash; (örneğin bir toplama) tek bir değer alır.


### <a name="executenonquery"></a>EXECUTENONQUERY

`INSERT`, `UPDATE`, ve `DELETE` deyimleri etkilenen satırların sayısını döndürür. Diğer tüm SQL deyimlerini -1 döndürür.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

Aşağıdaki yöntem gösterildiği bir `WHERE` yan tümcesinde `SELECT` deyimi.
Kodu tam bir SQL deyimi oluşturmak için bu ayrılmış karakterleri dizeleri etrafında tırnak işareti (') gibi kaçınmak için dikkatli olun gerekir.

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

`ExecuteReader` Yöntemi döndürür bir `SqliteDataReader` nesnesi. Ek olarak `Read` yöntemi gösterilen örnekte, diğer kullanışlı özellikler şunlardır:

-   **RowsAffected** &ndash; sorgu tarafından etkilenen satırların sayısı.

-   **HasRows** &ndash; herhangi bir satır olup döndürülmedi.


### <a name="executescalar"></a>EXECUTESCALAR

Bu iş için `SELECT` (örneğin, bir toplama) tek bir değer döndürmesi deyimleri.

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

`ExecuteScalar` Yöntemin dönüş türü `object` &ndash; veritabanı sorgusu bağlı olarak sonuç atamalısınız. Sonucu arasında bir tamsayı olabilir bir `COUNT` sorgu veya tek bir sütun dizeden `SELECT` sorgu. Bu diğer farklı olduğuna dikkat edin `Execute` Okuyucu nesnesi veya etkilenen satırların sayısını döndüren yöntemler.



## <a name="related-links"></a>İlgili bağlantılar

- [ADO'da Basic (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [ADO'da Gelişmiş (örnek)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android veri tarif](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms veri erişimi](~/xamarin-forms/app-fundamentals/databases.md)
