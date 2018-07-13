---
title: System.Data Xamarin.iOS içinde
description: Bu belge, bir Xamarin.iOS uygulaması SQLite verilere erişmek için System.Data ve Mono.Data.Sqlite.dll kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: F10C0C57-7BDE-A3F3-B011-9839949D15C8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 183079c150ad4df05424d4dbf2980a307a889352
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997204"
---
# <a name="systemdata-in-xamarinios"></a>System.Data Xamarin.iOS içinde

Xamarin.iOS 8.10 için destek ekler [System.Data](xref:System.Data)de dahil olmak üzere `Mono.Data.Sqlite.dll` ADO.NET sağlayıcısı. Desteği içeren aşağıdaki ek [derlemeleri](~/cross-platform/internals/available-assemblies.md):

-  `System.Data.dll`
-  `System.Data.Service.Client.dll`
-  `System.Transactions.dll`
-  `Mono.Data.Tds.dll`
-  `Mono.Data.Sqlite.dll`

<a name="Example" />

## <a name="example"></a>Örnek

Aşağıdaki program bir veritabanında oluşturur `Documents/mydb.db3`, ve veritabanı önceden yoksa, örnek verilerle doldurulur. Veritabanı ardından, yazılan çıktıyla sorgulanır `stderr`.

### <a name="add-references"></a>Başvuruları Ekle

İlk olarak, sağ **başvuruları** düğüm ve **başvuruları Düzenle...**  seçip `System.Data` ve `Mono.Data.Sqlite`:

[![](system.data-images/edit-references-sml.png "Yeni başvuru ekleme")](system.data-images/edit-references.png#lightbox)

### <a name="sample-code"></a>Örnek kod

Aşağıdaki kod, tablo oluşturma ve katıştırılmış SQL komutlarını kullanarak satır eklemek için basit bir örnek göstermektedir:

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
> Yukarıdaki kod örneğinde belirtildiği gibi kodunuzu savunmasız yaptığından dizeleri SQL komutları eklemek için hatalı bir uygulama olduğu [SQL ekleme](http://en.wikipedia.org/wiki/SQL_injection).


### <a name="using-command-parameters"></a>Komut parametreleri kullanma

Aşağıdaki kodu (metin tek kesme işareti gibi özel SQL karakterler içeriyorsa bile) metin kullanıcı tarafından girilen veritabanına güvenli bir şekilde eklemek için komut parametreleri kullanma işlemini gösterir:

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

-  Herhangi bir şey gerektiren [System.CodeDom](xref:System.CodeDom) (örn.)  [System.Data.TypedDataSetGenerator](xref:System.Data.TypedDataSetGenerator) )
-  XML yapılandırma dosyası desteği (örn.)  [System.Data.Common.DbProviderConfigurationHandler](xref:System.Data.Common.DbProviderConfigurationHandler) )
-   [System.Data.Common.DbProviderFactories](xref:System.Data.Common.DbProviderFactories) (XML yapılandırma dosyası desteğine bağlıdır)
-   [System.Data.OleDb](xref:System.Data.OleDb)
-   [System.Data.Odbc](xref:System.Data.Odbc)
-  `System.EnterpriseServices.dll` Bağımlılığı olan *kaldırıldı* gelen `System.Data.dll` , kaldırılmasını imzalanmayarak [SqlConnection.EnlistDistributedTransaction(ITransaction)](xref:System.Data.SqlClient.SqlConnection.EnlistDistributedTransaction*) yöntemi.


<a name="Mono.Data.Sqlite" />

### <a name="monodatasqlite"></a>Mono.Data.Sqlite

Bu arada **Mono.Data.Sqlite.dll** kaynak kod değişikliği olmadan çalışmaya başlayamıyorsa, ancak bunun yerine bir dizi konak olabilir *çalışma zamanı* beri sorunları `Mono.Data.Sqlite.dll` SQLite 3.5 bağlar. iOS 8'de, bu arada 3.8.5 SQLite ile birlikte gelir. İki sürümü açmamayý, çok varsayalım, yapılan değişiklikler.

Eski iOS sürümü SQLite'nın aşağıdaki sürümleriyle gönderin:

- **iOS 7** -3.7.13 sürümü.
- **iOS 6** -3.7.13 sürümü.
- **iOS 5** -3.7.7 sürümü.
- **iOS 4** -3.6.22 sürümü.

Yaygın sorunların çoğunu veritabanı şeması için sorgulama, ilgili olabilir görünmesine örn sütunlar gibi belirli bir tablodaki mevcut çalışma zamanında belirleyen `Mono.Data.Sqlite.SqliteConnection.GetSchema` (geçersiz kılma [DbConnection.GetSchema](xref:System.Data.Common.DbConnection.GetSchema) ve `Mono.Data.Sqlite.SqliteDataReader.GetSchemaTable` (geçersiz kılma [DbDataReader.GetSchemaTable](xref:System.Data.Common.DbDataReader.GetSchemaTable). Kısacası, başka herhangi bir şey kullanarak görünüyor [DataTable](xref:System.Data.DataTable) çalışacak şekilde düşüktür.

<a name="Data_Binding" />

## <a name="data-binding"></a>Veri Bağlama

Veri bağlama, şu anda desteklenmiyor.

