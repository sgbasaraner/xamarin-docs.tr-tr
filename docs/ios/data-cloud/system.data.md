---
title: System.Data
ms.topic: article
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 145a0692ed9761944eec4c7ece1f098a584f2d54
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="systemdata"></a>System.Data

Xamarin.iOS 8.10 için destek ekler [System.Data](https://developer.xamarin.com/api/namespace/System.Data/)dahil `Mono.Data.Sqlite.dll` ADO.NET sağlayıcısı. Desteği içeren aşağıdaki eklenmesi [derlemeleri](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`


<a name="Example" />

## <a name="example"></a>Örnek

Bir veritabanında aşağıdaki programı oluşturur `Documents/mydb.db3`, ve veritabanı önceden yoksa, örnek verilerle doldurulur. Veritabanı sonra yazılan çıkış sorgulanır `stderr`.

### <a name="add-references"></a>Başvuruları ekleme

İlk olarak, sağ tıklayın **başvuruları** düğümü seçin **başvuruları Düzenle...**  seçip `System.Data` ve `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Yeni başvuru ekleme")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Örnek kod

Aşağıdaki kod bir tablo oluşturma ve katıştırılmış SQL komutlarını kullanarak satırlar ekleme basit bir örnek gösterilmektedir:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;

class Demo {
    static void Main (string [] args)
    {
        var connection = GetConnection ();
        using (var cmd = connection.CreateCommand ()) {
            connection.Open ();
            cmd.CommandText = "SELECT * FROM People";
            using (var reader = cmd.ExecuteReader ()) {
                while (reader.Read ()) {
                    Console.Error.Write ("(Row ");
                    Write (reader, 0);
                    for (nint i = 1; i < reader.FieldCount; ++i) {
                        Console.Error.Write(" ");
                        Write (reader, i);
                    }
                    Console.Error.WriteLine(")");
                }
            }
            connection.Close ();
        }
    }

    static SqliteConnection GetConnection()
    {
        var documents = Environment.GetFolderPath (
                Environment.SpecialFolder.Personal);
        string db = Path.Combine (documents, "mydb.db3");
        bool exists = File.Exists (db);
        if (!exists)
            SqliteConnection.CreateFile (db);
        var conn = new SqliteConnection("Data Source=" + db);
        if (!exists) {
            var commands = new[] {
            "CREATE TABLE People (PersonID INTEGER NOT NULL, FirstName ntext, LastName ntext)",
            // WARNING: never insert user-entered data with embedded parameter values
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (1, 'First', 'Last')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (2, 'Dewey', 'Cheatem')",
            "INSERT INTO People (PersonID, FirstName, LastName) VALUES (3, 'And', 'How')",
            };
            conn.Open ();
            foreach (var cmd in commands) {
                using (var c = conn.CreateCommand()) {
                    c.CommandText = cmd;
                    c.CommandType = CommandType.Text;
                    c.ExecuteNonQuery ();
                }
            }
            conn.Close ();
        }
        return conn;
    }

    static void Write(SqliteDataReader reader, int index)
    {
        Console.Error.Write("({0} '{1}')",
                reader.GetName(index),
                reader [index]);
    }
}
```

> [!IMPORTANT]
> Yukarıdaki kod örneğinde belirtildiği gibi kodunuzu karşı savunmasız yaptığından dizeleri SQL komutları katıştırmak için hatalı alıştırma gereklidir [SQL ekleme](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Komut parametreleri kullanma

Aşağıdaki kod komut parametreleri (metin tek kesme işareti gibi özel SQL karakterler içeriyorsa bile) kullanıcı tarafından girilen metin güvenli bir şekilde veritabanına eklemek için nasıl kullanılacağını gösterir:

```csharp
// user input from Textbox control
var fname = fnameTextbox.Text;
var lname = lnameTextbox.Text;
// use command parameters to safely insert into database
using (var addCmd = conn.CreateCommand ()) {
    addCmd.CommandText = "INSERT INTO [People] (PersonID, FirstName, LastName) VALUES (@COL1, @COL2, @COL3)";
    addCmd.CommandType = System.Data.CommandType.Text;
    addCmd.AddParameterWithValue ("@COL1", 1);
    addCmd.AddParameterWithValue ("@COL2", fname);
    addCmd.AddParameterWithValue ("@COL3", lname);
    addCmd.ExecuteNonQuery ();
}
```

<a name="Missing_Functionality" />

## <a name="missing-functionality"></a>Eksik işlev

Her ikisi de **System.Data** ve **Mono.Data.Sqlite** bazı işlevler eksik.

<a name="System.Data" />

### <a name="systemdata"></a>System.Data

İşlevsellik gelen eksik **System.Data.dll** oluşur:

-  Herhangi bir şey gerektiren [System.CodeDom](https://developer.xamarin.com/api/namespace/System.CodeDom/) (örn.  [System.Data.TypedDataSetGenerator](https://developer.xamarin.com/api/type/System.Data.TypedDataSetGenerator/) )
-  XML yapılandırma dosyası desteği (örn.  [System.Data.Common.DbProviderConfigurationHandler](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderConfigurationHandler/) )
-   [System.Data.Common.DbProviderFactories](https://developer.xamarin.com/api/type/System.Data.Common.DbProviderFactories/) (depends on XML config file support)
-   [System.Data.OleDb](https://developer.xamarin.com/api/namespace/System.Data.OleDb/)
-   [System.Data.Odbc](https://developer.xamarin.com/api/namespace/System.Data.Odbc/)
-  `System.EnterpriseServices.dll` Bağımlılık belirtildi *kaldırılan* gelen `System.Data.dll` , kaldırılmasını sonuçta elde edilen [SqlConnection.EnlistDistributedTransaction(ITransaction)](https://developer.xamarin.com/api/member/System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction/(System.EnterpriseServices.ITransaction)) yöntemi.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

Bu sırada, **Mono.Data.Sqlite.dll** hiçbir kaynak kod değişiklikleri başlayamıyorsa, ancak bunun yerine bir dizi ana bilgisayar olabilir *çalışma zamanı* bu yana sorunları `Mono.Data.Sqlite.dll` SQLite 3.5 bağlar. iOS 8'de, bu sırada, 3.8.5 SQLite ile birlikte gelir. Açmamayý, çok deyin, bazı şeyleri iki sürümü arasında değiştirilmiştir.

İOS eski sürümünü SQLite aşağıdaki sürümleri ile birlikte:

- **iOS 7** -sürüm 3.7.13.
- **iOS 6** -sürüm 3.7.13.
- **iOS 5** -sürüm 3.7.7.
- **iOS 4** -sürüm 3.6.22.

En yaygın sorunları şema veritabanını sorgulama için ilgili görünmesini örn, sütunlar gibi belirli bir tablo üzerinde mevcut çalışma zamanında belirleme `Mono.Data.Sqlite.SqliteConnection.GetSchema` (geçersiz kılma [DbConnection.GetSchema](https://developer.xamarin.com/api/member/System.Data.Common.DbConnection.GetSchema/)) ve `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` () geçersiz kılma [DbDataReader.GetSchemaTable](https://developer.xamarin.com/api/member/System.Data.Common.DbDataReader.GetSchemaTable/)). Kısacası, diğer herhangi bir şey kullanarak görünüyor [DataTable](https://developer.xamarin.com/api/type/System.Data.DataTable/) çalışmaya düşüktür.

<a name="Data_Binding" />

## <a name="data-binding"></a>Veri Bağlama

Veri bağlama şu anda desteklenmiyor.

