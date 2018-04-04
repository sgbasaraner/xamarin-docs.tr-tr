---
title: Veritabanları
description: Bu makalede, anahtar-değer kodlama ve anahtar-değer SQLite veritabanları ve Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğeleri arasında veri bağlamayı izin vermek üzere Gözlemleme kullanmayı ele alır. Ayrıca, SQLite veri erişim sağlamak için SQLite.NET ORM kullanmayı ele alır.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 33c1ab7092669bb1dbd4e7bfae628b58a0bf3726
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="databases"></a>Veritabanları

_Bu makalede, anahtar-değer kodlama ve anahtar-değer SQLite veritabanları ve Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğeleri arasında veri bağlamayı izin vermek üzere Gözlemleme kullanmayı ele alır. Ayrıca, SQLite veri erişim sağlamak için SQLite.NET ORM kullanmayı ele alır._

## <a name="overview"></a>Genel Bakış

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, bir Xamarin.iOS veya Xamarin.Android uygulamasına erişmek için aynı SQLite veritabanlarını da erişiminiz vardır.

Bu makalede biz SQLite verilere erişmek için iki yol kapsayan:

1. **Doğrudan erişim** - bir SQLite veritabanına doğrudan erişerek biz verileri veritabanından anahtar-değer kodlama için kullanabilir ve kullanıcı Arabirimi öğeleri ile veri bağlaması Xcode'nın arabirimi Oluşturucusu'nda oluşturulan. Anahtar-değer kodlama ve veri Xamarin.Mac uygulamanızda teknikleri bağlama kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız için sahip kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma faydası vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), başında bakımı kolay, daha esnek uygulama için Tasarım.
2. **SQLite.NET ORM** - açık kaynak kullanarak [SQLite.NET](http://www.sqlite.org) biz büyük ölçüde azaltabilir okumak ve bir SQLite veritabanından veri yazmak için gereken kod miktarını nesne ilişki Yöneticisi (ORM). Bu veriler, bir kullanıcı arabirimi öğesi gibi bir tablo görünümü doldurmak için daha sonra kullanılabilir.

[![Çalışan uygulama örneği](databases-images/intro01.png "çalışan uygulama örneği")](databases-images/intro01-large.png#lightbox)

Bu makalede, biz anahtar-değer kodlama ve Xamarin.Mac uygulamasında SQLite veritabanlarıyla veri bağlama ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Biz anahtar-değer kodlama ve veri bağlama kullanacağınız, lütfen aracılığıyla çalışmaya [verileri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md) ilk olarak çekirdek teknikleri ve kavramlarını ele alınan kullanılacak bu belgeleri ve onun örnek uygulama.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve kullanıcı Arabirimi öğeleri, C# sınıfları wire için kullanılır.

## <a name="direct-sqlite-access"></a>Doğrudan SQLite erişim

Kullanıcı Arabirimi öğeleri Xcode'nın arabirimi Oluşturucu bağlı olmasını giderek SQLite verileri için yüksek oranda önerilir doğrudan (aksine bir ORM gibi bir yöntemle) SQLite veritabanı erişim, yol toplam denetime sahip olduğundan veri yazılmış okuyun ve  veritabanından kaldırılıyor.

İçinde anlatıldığı gibi [verileri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md) belgeleri, anahtar-değer kodlama ve veri Xamarin.Mac uygulamanızda teknikleri bağlama kullanarak, önemli ölçüde düşebilir yazmak zorunda kod miktarını ve doldurma ve kullanıcı Arabirimi öğeleri ile çalışmak için korur. Bir SQLite veritabanı doğrudan erişim ile birlikte kullanıldığında, okumak ve bu veritabanına veri yazmak için gereken kod miktarını büyük ölçüde azaltabilir.

Bu makalede, sizi veri bağlama ve anahtar-değer kodlama belge bir SQLite veritabanı yedekleme kaynağı olarak bağlaması için kullanılacak örnek uygulamadan değiştirme.

### <a name="including-sqlite-database-support"></a>SQLite veritabanı desteği de dahil olmak üzere

Devam etmeden önce biz birkaç başvuruları dahil ederek uygulamamız için SQLite veritabanı desteği eklemeniz gerekir. DLL dosyaları.

Aşağıdakileri yapın:

1. İçinde **çözüm paneli**, sağ tıklayın **başvuruları** klasörü ve Seç **Düzenle başvurular**.
2. Her ikisini de seçin **Mono.Data.Sqlite** ve **System.Data** derlemeler: 

    [![Gerekli başvuruları ekleme](databases-images/reference01.png "gerekli başvuruları ekleme")](databases-images/reference01-large.png#lightbox)
3. Tıklatın **Tamam** düğmesine yaptığınız değişiklikleri kaydetmek ve başvurular ekleyin.

### <a name="modifying-the-data-model"></a>Veri modeli değiştirme

Doğrudan uygulamamız SQLite veritabanına erişmek için destek ekledik, biz bizim veri modeli okuyun ve veritabanından veri yazma (aynı zamanda anahtar-değer kodlama ve veri bağlama sağlar) nesnesine değiştirmeniz gerekir. Örnek uygulamamız söz konusu olduğunda, biz düzenleyeceksiniz **PersonModel.cs** sınıfı ve şu şekilde görünür yapın:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _ID = "";
        private string _managerID = "";
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        private SqliteConnection _conn = null;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        [Export("ID")]
        public string ID {
            get { return _ID; }
            set {
                WillChangeValue ("ID");
                _ID = value;
                DidChangeValue ("ID");
            }
        }

        [Export("ManagerID")]
        public string ManagerID {
            get { return _managerID; }
            set {
                WillChangeValue ("ManagerID");
                _managerID = value;
                DidChangeValue ("ManagerID");
            }
        }

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }

        public PersonModel (string id, string name, string occupation)
        {
            // Initialize
            this.ID = id;
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (SqliteConnection conn, string id)
        {
            // Load from database
            Load (conn, id);
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion

        #region SQLite Routines
        public void Create(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Create new record ID?
            if (ID == "") {
                ID = Guid.NewGuid ().ToString();
            }

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Save manager ID and create the sub record
                Person.ManagerID = ID;
                Person.Create (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Update(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Update sub record
                Person.Update (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Load(SqliteConnection conn, string id) {
            bool shouldClose = false;

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Is the database already open?
            if (conn.State != ConnectionState.Open) {
                shouldClose = true;
                conn.Open ();
            }

            // Execute query
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", id);

                using (var reader = command.ExecuteReader ()) {
                    while (reader.Read ()) {
                        // Pull values back into class
                        ID = (string)reader [0];
                        Name = (string)reader [1];
                        Occupation = (string)reader [2];
                        isManager = (bool)reader [3];
                        ManagerID = (string)reader [4];
                    }
                }
            }

            // Is this a manager?
            if (isManager) {
                // Yes, load children
                using (var command = conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

                    // Populate with data from the record
                    command.Parameters.AddWithValue ("@COL1", id);

                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Load child and add to collection
                            var childID = (string)reader [0];
                            var person = new PersonModel (conn, childID);
                            _people.Add (person);
                        }
                    }
                }
            }

            // Should we close the connection to the database
            if (shouldClose) {
                conn.Close ();
            }

            // Save last connection
            _conn = conn;
        }

        public void Delete(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Empty class
            ID = "";
            ManagerID = "";
            Name = "";
            Occupation = "";
            isManager = false;
            _people = new NSMutableArray();

            // Save last connection
            _conn = conn;
        }
        #endregion
    }
}
```

Bir değişiklik aşağıda ayrıntılı olarak bakalım.

Birkaç ilk olarak, ekledik SQLite kullanmak için gereken deyimleri ve son bizim bağlantı SQLite veritabanına kaydetmek için bir değişken ekledik:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Kullanıcı veri bağlama aracılığıyla kullanıcı arabiriminde içeriği değiştirdiğinde otomatik olarak herhangi bir değişiklik kayıt veritabanına kaydetmek için bu kaydedilmiş bir bağlantıyı kullanacağız:

```csharp
[Export("Name")]
public string Name {
    get { return _name; }
    set {
        WillChangeValue ("Name");
        _name = value;
        DidChangeValue ("Name");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("Occupation")]
public string Occupation {
    get { return _occupation; }
    set {
        WillChangeValue ("Occupation");
        _occupation = value;
        DidChangeValue ("Occupation");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}
```

Yapılan değişiklikler **adı**, **Mesleği** veya **isManager** özellikler veri var. önce kaydedilmişse veritabanına gönderilir (örneğin, `_conn` değişken değil `null`). Ardından, için ekledik yöntemleri bakalım **oluşturma**, **güncelleştirme**, **yük** ve **silmek** veritabanı kişilerden.

#### <a name="create-a-new-record"></a>Yeni bir kayıt oluşturun

Aşağıdaki kod, SQLite veritabanında yeni bir kayıt oluşturmak için eklendi:

```csharp
public void Create(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Create new record ID?
    if (ID == "") {
        ID = Guid.NewGuid ().ToString();
    }

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Save manager ID and create the sub record
        Person.ManagerID = ID;
        Person.Create (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Kullanıyoruz bir `SQLiteCommand` veritabanında yeni kaydı oluşturun. Size yeni bir komut almak `SQLiteConnection` (biz çağırarak yönteme geçirilen conn) `CreateCommand`. Ardından, yeni bir kayıt gerçekten yazmaya SQL yönerge parametreleri için gerçek değerleri sağlayarak ayarlarız:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Daha sonra kullanarak parametreler için değerleri ayarlar `Parameters.AddWithValue` yöntemi `SQLiteCommand`. Parametreleri kullanarak, biz değerleri (örneğin, tek tırnak işareti) düzgün SQLite için gönderilmeden önce kodlanmış emin olun. Örnek:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Bir kişinin bir yönetici olması ve bunların altındaki çalışanlar oluşan bir koleksiyona sahip olduğundan, son olarak, yinelemeli olarak duyuyoruz çağırma `Create` veritabanına kaydetmek için bu kişilerin yöntemi:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Save manager ID and create the sub record
    Person.ManagerID = ID;
    Person.Create (conn);
}
```

#### <a name="updating-a-record"></a>Kayıt güncelleştirme

Aşağıdaki kod, SQLite veritabanındaki var olan bir kaydı güncelleştirmeye eklendi:

```csharp
public void Update(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Update sub record
        Person.Update (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Gibi **oluşturma** yukarıdaki biz alma bir `SQLiteCommand` geçirilen gelen içinde `SQLiteConnection`ve (parametreleri sağlayarak) bizim kaydını güncelleştirmek üzere bizim SQL ayarlayın:

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Biz parametre değerleri doldurun (örnek: `command.Parameters.AddWithValue ("@COL1", ID);`) ve yeniden, yinelemeli olarak çağırın güncelleştirmesini tüm alt kaydeder:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Bir kayıt yükleniyor

Aşağıdaki kod, varolan bir kaydı SQLite veritabanından yüklemek için eklendi:

```csharp
public void Load(SqliteConnection conn, string id) {
    bool shouldClose = false;

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Is the database already open?
    if (conn.State != ConnectionState.Open) {
        shouldClose = true;
        conn.Open ();
    }

    // Execute query
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Pull values back into class
                ID = (string)reader [0];
                Name = (string)reader [1];
                Occupation = (string)reader [2];
                isManager = (bool)reader [3];
                ManagerID = (string)reader [4];
            }
        }
    }

    // Is this a manager?
    if (isManager) {
        // Yes, load children
        using (var command = conn.CreateCommand ()) {
            // Create new command
            command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

            // Populate with data from the record
            command.Parameters.AddWithValue ("@COL1", id);

            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Load child and add to collection
                    var childID = (string)reader [0];
                    var person = new PersonModel (conn, childID);
                    _people.Add (person);
                }
            }
        }
    }

    // Should we close the connection to the database
    if (shouldClose) {
        conn.Close ();
    }

    // Save last connection
    _conn = conn;
}
```

Yordam, yinelemeli olarak (örneğin, kendi çalışanlar nesne Yükleme Yöneticisi nesnesi) üst nesneden çağrılabilir olduğundan, özel kodu açma ve veritabanı bağlantısı kapatma işlemek için eklendi:

```csharp
bool shouldClose = false;
...

// Is the database already open?
if (conn.State != ConnectionState.Open) {
    shouldClose = true;
    conn.Open ();
}
...

// Should we close the connection to the database
if (shouldClose) {
    conn.Close ();
}

```

Her zaman olduğu gibi kayıt almak ve parametrelerini kullanmak için bizim SQL ayarlayın:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Son olarak, bir veri okuyucu sorguyu yürütmek ve kayıt alanlarını dönmek için kullanıyoruz (hangi biz örneğini kopyalamak `PersonModel` sınıfı):

```csharp
using (var reader = command.ExecuteReader ()) {
    while (reader.Read ()) {
        // Pull values back into class
        ID = (string)reader [0];
        Name = (string)reader [1];
        Occupation = (string)reader [2];
        isManager = (bool)reader [3];
        ManagerID = (string)reader [4];
    }
}
```

Bu kişi bir yöneticisi ise, ayrıca tüm çalışanlar yüklemek ihtiyacımız (yinelemeli arama tarafından yeniden kendi `Load` yöntemi):

```csharp
// Is this a manager?
if (isManager) {
    // Yes, load children
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Load child and add to collection
                var childID = (string)reader [0];
                var person = new PersonModel (conn, childID);
                _people.Add (person);
            }
        }
    }
}
```

#### <a name="deleting-a-record"></a>Kayıt silme

Aşağıdaki kod, varolan bir kaydı SQLite veritabanından silmek için eklendi:

```csharp
public void Delete(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Empty class
    ID = "";
    ManagerID = "";
    Name = "";
    Occupation = "";
    isManager = false;
    _people = new NSMutableArray();

    // Save last connection
    _conn = conn;
}
```

Burada yöneticileri kaydı ve (parametrelerini kullanarak) bu Yöneticisi altında herhangi bir çalışan kayıtları silmek için SQL sağlar:

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Kaydı kaldırıldıktan sonra şu geçerli örneği temizleyin `PersonModel` sınıfı:

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>Veritabanı başlatılıyor

Yapılan değişikliklerle birlikte veri Modelimizi yerinde okuma ve veritabanına yazılması desteklemek için kimliğinizi veritabanına bir bağlantı açmak ve ilk çalıştırılmasında başlatma gerekiyor. Aşağıdaki kodu ekleyelim bizim **MainWindow.cs** dosyası:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection DatabaseConnection = null;
...

private SqliteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "People.db3");

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);
    if (!exists)
        SqliteConnection.CreateFile (db);

    // Create connection to the database
    var conn = new SqliteConnection("Data Source=" + db);

    // Set the structure of the database
    if (!exists) {
        var commands = new[] {
            "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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

        // Build list of employees
        var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
        Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
        Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
        Craig.Create (conn);

        var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
        Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
        Larry.Create (conn);
    }

    // Return new connection
    return conn;
}
```

Yukarıdaki kod daha yakın bir göz atalım. İlk olarak, yeni veritabanı (Bu örnekte, kullanıcının Masaüstü), veritabanının var olup olmadığını görmek için bakın ve seçili değilse, oluşturmak için size bir konum seçin:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Ardından, yukarıda oluşturduğumuz yolu kullanarak veritabanına bağlan kurar:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Ardından tüm SQL tablolarını biz gerektiren veritabanında oluşturuyoruz:

```csharp
var commands = new[] {
    "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
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
```

Son olarak, biz veri Modelimizi de kullanabilirsiniz (`PersonModel`) uygulama veritabanı ilk çalıştırıldığında veya veritabanı eksik olup olmadığını için varsayılan bir kayıt kümesi oluşturmak için:

```csharp
// Build list of employees
var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
Craig.Create (conn);

var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
Larry.Create (conn);
``` 

Uygulamayı başlatır ve ana penceresi açar, yukarıda eklediğimiz kod kullanarak veritabanına bir bağlantı vermiyoruz:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Yükleme veri bağlama

Doğrudan bağlı yerinde SQLite veritabanındaki verilere için tüm bileşenlerle, biz uygulamamız sağlar ve otomatik olarak bizim kullanıcı Arabiriminde görüntülenecek farklı görünümleri verileri yükleyebilir.

#### <a name="loading-a-single-record"></a>Tek bir kaydını yükleniyor

Tek bir kayıt kimliği olduğu bilmeniz yüklemek için biz aşağıdaki kodu kullanabilirsiniz:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Tüm kayıtları yükleniyor

Bir yönetici veya yok olması durumunda tüm kişilerin, bakılmaksızın yüklemek için aşağıdaki kodu kullanın:

```csharp
// Load all employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People]";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();

```

Burada, bir aşırı yüklemesini Oluşturucusu kullanıyoruz `PersonModel` sınıfı kişi belleğe yüklemek için:

```csharp
var person = new PersonModel (_conn, childID);
```

Biz de kişi kişilerin bizim koleksiyona eklemek için veri bağlama sınıfı çağırma `AddPerson (person)`, bu kullanıcı arabirimimizi değişikliği algılar ve görüntüler sağlar:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Yalnızca üst düzey kayıtları yükleniyor

Yalnızca Yöneticiler (örneğin, bir ana hattı verileri görüntülemek için) yüklemek için aşağıdaki kodu kullanırız:

```csharp
// Load only managers employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();
```

İçinde SQL deyiminde yalnızca gerçek fark (yalnızca Yöneticiler yükler `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), ancak aynı aksi yukarıdaki bölümde çalışır.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Veritabanları ve comboboxes

MacOS (örneğin, birleşik giriş kutusu) kullanımına menü denetimleri listeden (arabirimi Oluşturucusu'nda önceden tanımlanamıyor veya kodu aracılığıyla doldurulmuş) bir iç ya da aşağı açılan listeyi doldurmak için ayarlayabilirsiniz veya kendi özel, dış veri kaynağı sağlayarak. Bkz: [menü denetim verileri sağlayan](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) daha fazla ayrıntı için.

Örnek olarak, düzenleme basit bağlama Yukarıdaki örnek arabirimi oluşturucu ekleyin birleşik giriş kutusu ve adlı prizine kullanarak kullanıma `EmployeeSelector`:

[![Birleşik giriş kutusu çıkışı gösterme](databases-images/combo01.png "birleşik giriş kutusu çıkışı gösterme")](databases-images/combo01-large.png#lightbox)

İçinde **öznitelikleri denetçisi**, denetleme **Autocompletes** ve **veri kaynağını kullanan** özellikleri:

![Birleşik giriş kutusu öznitelikleri yapılandırma](databases-images/combo02.png "birleşik giriş kutusu öznitelikleri yapılandırma")

Değişikliklerinizi kaydetmek ve Visual Studio eşitlemek için Mac için geri dönün.

#### <a name="providing-combobox-data"></a>ComboBox verileri sağlayan

Ardından, yeni bir sınıf adlı projeye ekleyin `ComboBoxDataSource` ve şu şekilde görünür yapın:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public class ComboBoxDataSource : NSComboBoxDataSource
    {
        #region Private Variables
        private SqliteConnection _conn = null;
        private string _tableName = "";
        private string _IDField = "ID";
        private string _displayField = "";
        private nint _recordCount = 0;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        public string TableName {
            get { return _tableName; }
            set { 
                _tableName = value;
                _recordCount = GetRecordCount ();
            }
        }

        public string IDField {
            get { return _IDField; }
            set {
                _IDField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public string DisplayField {
            get { return _displayField; }
            set { 
                _displayField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public nint RecordCount {
            get { return _recordCount; }
        }
        #endregion

        #region Constructors
        public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.DisplayField = displayField;
        }

        public ComboBoxDataSource (SqliteConnection conn, string tableName, string idField, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.IDField = idField;
            this.DisplayField = displayField;
        }
        #endregion

        #region Private Methods
        private nint GetRecordCount ()
        {
            bool shouldClose = false;
            nint count = 0;

            // Has a Table, ID and display field been specified?
            if (TableName !="" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read count from query
                            var result = (long)reader [0];
                            count = (nint)result;
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return the number of records
            return count;
        }
        #endregion

        #region Public Methods
        public string IDForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string ValueForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string IDForValue (string value)
        {
            NSString result = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", value);

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            result = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return result;
        }
        #endregion 

        #region Override Methods
        public override nint ItemCount (NSComboBox comboBox)
        {
            return RecordCount;
        }

        public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public override nint IndexOfItem (NSComboBox comboBox, string value)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";
            nint index = NSRange.NotFound;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read () && !found) {
                            // Read the display field from the query
                            field = (string)reader [0];
                            ++index;

                            // Is this the value we are searching for?
                            if (value == field) {
                                // Yes, exit loop
                                found = true;
                            }
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return index;
        }

        public override string CompletedString (NSComboBox comboBox, string uncompletedString)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Escape search string
                uncompletedString = uncompletedString.Replace ("'", "");

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            field = (string)reader [0];
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return field;
        }
        #endregion
    }
}
```

Bu örnekte, yeni bir oluşturuyoruz `NSComboBoxDataSource` birleşik giriş kutusu öğeleri herhangi SQLite veri kaynağından sunabileceği. İlk olarak, aşağıdaki özellikleri tanımlayın:

- `Conn` -Alır veya SQLite veritabanına bir bağlantı ayarlar.
- `TableName` -Alır veya tablo adını ayarlar.
- `IDField` -Alanını alır veya belirtilen tablo için benzersiz Kimliğini sağlayan ayarlar. Varsayılan değer `ID` şeklindedir.
- `DisplayField` -Alanını alır veya açılır listede görüntülenen ayarlar.
- `RecordCount` -Belirtilen tabloda kayıt sayısını alır.

Nesne yeni bir örneğini oluşturuyoruz olduğunda, bağlantı, tablo adı, isteğe bağlı olarak ID alanı ve görüntü alanını Geçir:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

`GetRecordCount` Yöntemi belirtilen tabloda kayıt sayısını döndürür:

```csharp
private nint GetRecordCount ()
{
    bool shouldClose = false;
    nint count = 0;

    // Has a Table, ID and display field been specified?
    if (TableName !="" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read count from query
                    var result = (long)reader [0];
                    count = (nint)result;
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return the number of records
    return count;
}
```

İstediğiniz zaman adlı `TableName`, `IDField` veya `DisplayField` özellikleri değeri değiştirildi.

`IDForIndex` Yöntemi benzersiz kimliği döndürür (`IDField`) verilen açılır liste öğesi dizinindeki kaydı için: 

```csharp
public string IDForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

`ValueForIndex` Yöntemi değeri döndürür (`DisplayField`) verilen açılır liste dizindeki öğe için:

```csharp
public string ValueForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

`IDForValue` Yöntemi benzersiz kimliği döndürür (`IDField`) için belirtilen değer (`DisplayField`):

```csharp
public string IDForValue (string value)
{
    NSString result = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", value);

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    result = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return result;
}
```

`ItemCount` Ne zaman hesaplanan şekilde listedeki önceden hesaplanan öğe sayısını döndürür `TableName`, `IDField` veya `DisplayField` özellikleri değiştirilir:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

`ObjectValueForItem` Yöntemi değeri sağlar (`DisplayField`) verilen açılır liste öğesi dizini için:

```csharp
public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Kullanıyoruz bildirimi `LIMIT` ve `OFFSET` deyimleri size gereken tek bir kayıtta sınırlamak için bizim SQLite komutu.

`IndexOfItem` Yöntemi açılır öğe dizini değeri döndürür (`DisplayField`) verilen:

```csharp
public override nint IndexOfItem (NSComboBox comboBox, string value)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";
    nint index = NSRange.NotFound;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read () && !found) {
                    // Read the display field from the query
                    field = (string)reader [0];
                    ++index;

                    // Is this the value we are searching for?
                    if (value == field) {
                        // Yes, exit loop
                        found = true;
                    }
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return index;
}
```

Değeri bulunamazsa `NSRange.NotFound` değer dönüş ve tüm öğeleri açılır liste kutusunda seçili.

`CompletedString` Yöntemi ilk eşleşen değeri döndürür (`DisplayField`) kısmen yazılan girişi için:

```csharp
public override string CompletedString (NSComboBox comboBox, string uncompletedString)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Escape search string
        uncompletedString = uncompletedString.Replace ("'", "");

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    field = (string)reader [0];
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return field;
}
```

#### <a name="displaying-data-and-responding-to-events"></a>Verileri görüntüleme ve olaylara yanıt verme

Tüm parçaları bir araya getirmek için düzenleme `SubviewSimpleBindingController` ve şu şekilde görünür yapın:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public partial class SubviewSimpleBindingController : AppKit.NSViewController
    {
        #region Private Variables
        private PersonModel _person = new PersonModel();
        private SqliteConnection Conn;
        #endregion

        #region Computed Properties
        //strongly typed view accessor
        public new SubviewSimpleBinding View {
            get {
                return (SubviewSimpleBinding)base.View;
            }
        }

        [Export("Person")]
        public PersonModel Person {
            get {return _person; }
            set {
                WillChangeValue ("Person");
                _person = value;
                DidChangeValue ("Person");
            }
        }

        public ComboBoxDataSource DataSource {
            get { return EmployeeSelector.DataSource as ComboBoxDataSource; }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public SubviewSimpleBindingController (IntPtr handle) : base (handle)
        {
            Initialize ();
        }

        // Called when created directly from a XIB file
        [Export ("initWithCoder:")]
        public SubviewSimpleBindingController (NSCoder coder) : base (coder)
        {
            Initialize ();
        }

        // Call to load from the XIB/NIB file
        public SubviewSimpleBindingController (SqliteConnection conn) : base ("SubviewSimpleBinding", NSBundle.MainBundle)
        {
            // Initialize
            this.Conn = conn;
            Initialize ();
        }

        // Shared initialization code
        void Initialize ()
        {
        }
        #endregion

        #region Private Methods
        private void LoadSelectedPerson (string id)
        {

            // Found?
            if (id != "") {
                // Yes, load requested record
                Person = new PersonModel (Conn, id);
            }
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Configure Employee selector dropdown
            EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");

            // Wireup events
            EmployeeSelector.Changed += (sender, e) => {
                // Get ID
                var id = DataSource.IDForValue (EmployeeSelector.StringValue);
                LoadSelectedPerson (id);
            };

            EmployeeSelector.SelectionChanged += (sender, e) => {
                // Get ID
                var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
                LoadSelectedPerson (id);
            };

            // Auto select the first person
            EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
            Person = new PersonModel (Conn, DataSource.IDForIndex(0));
    
        }
        #endregion
    }
}
```

`DataSource` Özelliği sağlar kısayol `ComboBoxDataSource` (yukarıda açılan kutu bağlı oluşturulur).

`LoadSelectedPerson` Yöntemi veritabanından kişi verilen benzersiz kimliği yükler:

```csharp
private void LoadSelectedPerson (string id)
{

    // Found?
    if (id != "") {
        // Yes, load requested record
        Person = new PersonModel (Conn, id);
    }
}
```

İçinde `AwakeFromNib` yöntemi geçersiz kılma, ilk biz bizim Özel birleşik giriş kutusunu veri kaynağı örneği ekleyin:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Ardından, açılan kutu metin değerini ilişkili benzersiz kimliği bularak düzenleme kullanıcıya yanıt vermemiz (`IDField`) sunan ve belirli kişi, yükleme verilerini bulundu:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Kullanıcı açılır listeden yeni bir öğe seçerse biz de yeni bir kişiye yük:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Son olarak, biz otomatik-birleşik giriş kutusu ve listedeki ilk öğe ile görüntülenen kişi doldurun:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Açık kaynak kullanarak, yukarıda belirtildiği gibi [SQLite.NET](http://www.sqlite.org) biz büyük ölçüde azaltabilir okumak ve bir SQLite veritabanından veri yazmak için gereken kod miktarını nesne ilişki Yöneticisi (ORM). Bu, birkaç anahtar-değer kodlama ve veri bağlama bir nesne üzerinde yerleştirmek gereksinimleri nedeniyle veri bağlama sırasında olabilmesi için en iyi yolu olmayabilir.

SQLite.Net Web sitesi göre _"SQLite olduğu müstakil sunucusuz, sıfır yapılandırmalı, işlemsel bir SQL veritabanı altyapısı uygulayan bir yazılım kitaplığı. SQLite dünyada en yaygın olarak dağıtılan veritabanı altyapısıdır. Kaynak SQLite ortak etki alanında kodudur."_

Aşağıdaki bölümlerde, SQLite.Net için bir tablo görünümü verilerini sağlamak için nasıl kullanılacağını göstereceğiz.

### <a name="including-the-sqlitenet-nuget"></a>Including the SQLite.net NuGet

SQLite.NET uygulamanıza dahil bir NuGet paketi olarak sunulur. Veritabanı desteği SQLite.NET kullanarak ekleyebilmeniz için önce bu pakete dahil gerekir.

Paketi eklemek için aşağıdakileri yapın:

1. İçinde **çözüm paneli**, sağ tıklatın **paketleri** klasörü ve select **paketleri Ekle...**
2. Girin `SQLite.net` içinde **arama kutusu** seçip **sqlite net** girişi:

    [![SQLite NuGet paketi ekleme](databases-images/nuget01.png "SQLite NuGet paketi ekleme")](databases-images/nuget01-large.png#lightbox)
3. Tıklatın **Paketi Ekle** tamamlanması düğmesi.

### <a name="creating-the-data-model"></a>Veri modeli oluşturma

Şimdi projeye yeni bir sınıf ekleyin ve arama `OccupationModel`. Ardından, düzenleyelim **OccupationModel.cs** dosya ve şu şekilde görünür yapın:

```csharp
using System;
using SQLite;

namespace MacDatabase
{
    public class OccupationModel
    {
        #region Computed Properties
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set;}
        public string Description { get; set;}
        #endregion

        #region Constructors
        public OccupationModel ()
        {
        }

        public OccupationModel (string name, string description)
        {

            // Initialize
            this.Name = name;
            this.Description = description;

        }
        #endregion
    }
}
```

SQLite.NET ilk olarak, eklediğimiz (`using Sqlite`), ardından biz çeşitli özellikler kullanıma, her biri yazılır veritabanına bu kaydı kaydedildiğinde. İlk özelliği biz birincil anahtarı olarak yapın ve otomatik artış gibi ayarlayın:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Veritabanı başlatılıyor

Yapılan değişikliklerle birlikte veri Modelimizi yerinde okuma ve veritabanına yazılması desteklemek için kimliğinizi veritabanına bir bağlantı açmak ve ilk çalıştırılmasında başlatma gerekiyor. Aşağıdaki kod ekleyelim:

```csharp
using SQLite;
...

public SQLiteConnection Conn { get; set; }
...

private SQLiteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "Occupation.db3");
    OccupationModel Occupation;

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);

    // Create connection to database
    var conn = new SQLiteConnection (db);

    // Initially populate table?
    if (!exists) {
        // Yes, build table
        conn.CreateTable<OccupationModel> ();

        // Add occupations
        Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
        conn.Insert (Occupation);
    }

    return conn;
}
```

İlk olarak, size bir yol (Bu durumda kullanıcının Masaüstü) veritabanına alın ve veritabanı zaten bulunup bulunmadığına bakın:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Ardından, yukarıda oluşturduğumuz yolundaki veritabanıyla bağlantı kurun:

```csharp
var conn = new SQLiteConnection (db);
```

Son olarak, biz tablo oluşturun ve bazı varsayılan kayıtları ekleyin:

```csharp
// Yes, build table
conn.CreateTable<OccupationModel> ();

// Add occupations
Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
conn.Insert (Occupation);
```

### <a name="adding-a-table-view"></a>Bir tablo görünümü ekleme

Bir örnek kullanım bizim UI Xcode'nın arabirimi Oluşturucu için bir tablo görünümü ekleyeceğiz. Bu tablo görünüm prizine aracılığıyla kullanıma (`OccupationTable`) biz C# kodu aracılığıyla erişebilmesi için:

[![Bir tablo görünümü çıkışı gösterme](databases-images/table01.png "bir tablo görünümü çıkışı gösterme")](databases-images/table01-large.png#lightbox)

Ardından, bu tabloyu SQLite.NET veritabanından verilerle doldurmak için özel sınıflar ekleyeceğiz.

### <a name="creating-the-table-data-source"></a>Tablo veri kaynağı oluşturma

Bizim tablosu için veri sağlamak için özel bir veri kaynağını oluşturalım. İlk olarak, adlı yeni bir sınıf ekleyin `TableORMDatasource` ve şu şekilde görünür yapın:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDatasource : NSTableViewDataSource
    {
        #region Computed Properties
        public List<OccupationModel> Occupations { get; set;} = new List<OccupationModel>();
        public SQLiteConnection Conn { get; set; }
        #endregion

        #region Constructors
        public TableORMDatasource (SQLiteConnection conn)
        {
            // Initialize
            this.Conn = conn;
            LoadOccupations ();
        }
        #endregion

        #region Public Methods
        public void LoadOccupations() {

            // Get occupations from database
            var query = Conn.Table<OccupationModel> ();

            // Copy into table collection
            Occupations.Clear ();
            foreach (OccupationModel occupation in query) {
                Occupations.Add (occupation);
            }

        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Occupations.Count;
        }
        #endregion
    }
}
```

Biz daha sonra bu sınıfının bir örneği oluşturduğunuzda, biz bizim açık SQLite.NET veritabanı bağlantısı geçmesi. `LoadOccupations` Yöntemi veritabanını sorgular ve belleğe bulunan kayıtları kopyalar (kullanarak bizim `OccupationModel` veri modeli).

### <a name="creating-the-table-delegate"></a>Tablo temsilci oluşturma

İhtiyacımız son SQLite.NET veritabanından yüklemiş olduğunuz bilgileri görüntülemek için özel bir tablo temsilci sınıftır. Yeni bir ekleyelim `TableORMDelegate` bizim projeye ve şu şekilde görünür yapın:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDelegate : NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "OccCell";
        #endregion

        #region Private Variables
        private TableORMDatasource DataSource;
        #endregion

        #region Constructors
        public TableORMDelegate (TableORMDatasource dataSource)
        {
            // Initialize
            this.DataSource = dataSource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Occupation":
                view.StringValue = DataSource.Occupations [(int)row].Name;
                break;
            case "Description":
                view.StringValue = DataSource.Occupations [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Veri kaynağının burada kullanırız `Occupations` (biz SQLite.NET veritabanından yüklenen) koleksiyonu bizim tablosunun sütunları doldurmak için `GetViewForItem` yöntemi geçersiz kılma.

### <a name="populating-the-table"></a>Tabloyu doldurma

.Xib dosyasından kılarak şişirileceğini zaman tüm parçaları yerinde, şimdi bizim tabloyu doldurmak `AwakeFromNib` yöntemi ve Ara kolaylaştırarak aşağıdaki gibi:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get database connection
    Conn = GetDatabaseConnection ();

    // Create the Occupation Table Data Source and populate it
    var DataSource = new TableORMDatasource (Conn);

    // Populate the Product Table
    OccupationTable.DataSource = DataSource;
    OccupationTable.Delegate = new TableORMDelegate (DataSource);
}
```

İlk olarak, biz SQLite.NET Veritabanımıza oluşturma ve zaten yoksa doldurma erişin. Ardından, yeni bir örneğini oluşturuyoruz bizim özel tablo veri kaynağının bizim veritabanı bağlantısı geçirmek ve biz tabloya ekleyin. Son olarak, biz bizim özel tablo temsilci yeni bir örneğini oluşturma bizim veri kaynağında geçirmek ve tabloya ekleyin.

## <a name="summary"></a>Özet

Bu makalede, veri bağlama ve anahtar-değer Xamarin.Mac uygulama SQLite veritabanlarında kodlama ile çalışan bir ayrıntılı bakış duruma getirdi. İlk olarak, bir C# sınıfına Objective-C (KVC) anahtar-değer kodlama kullanılarak ve anahtar-değer (KVO) Gözlemleme gösterme Aranan. Ardından, KVO uyumlu sınıfının nasıl kullanılacağı gösterilmiştir ve veri Xcode'nın arabirimi Oluşturucu kullanıcı Arabirimi öğelerine bağlayın. Makaleyi SQLite.NET ORM aracılığıyla SQLite verilerle çalışma ve bu verileri Tablo görünümünde görüntüleyerek de kapsar.



## <a name="related-links"></a>İlgili bağlantılar

- [MacDatabase (örnek)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Veri bağlama ve anahtar-değer kodlama](~/mac/app-fundamentals/databinding.md)
- [Standart denetimler](~/mac/user-interface/standard-controls.md)
- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Anahat görünümleri](~/mac/user-interface/outline-view.md)
- [Koleksiyon görünümleri](~/mac/user-interface/collection-view.md)
- [Anahtar-değer programlama kılavuzu kodlama](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Programlama konularına Cocoa bağlamaları giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Giriş Cocoa bağlamaları başvurusu](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
