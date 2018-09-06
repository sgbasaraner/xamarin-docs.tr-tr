---
title: Xamarin.Mac veritabanları
description: Bu makale, anahtar-değer kodlaması ve anahtar-değer arasında SQLite veritabanları ve kullanıcı Arabirimi öğeleri Xcode'un arabirim oluşturucu içinde veri bağlamayı izin vermek için gözleme kullanarak kapsar. SQLite verilerine erişim sağlaması için SQLite.NET ORM kullanma kapsar.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: db418df0869d73e9f04982fb508fd261304240c0
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780657"
---
# <a name="databases-in-xamarinmac"></a>Xamarin.Mac veritabanları

_Bu makale, anahtar-değer kodlaması ve anahtar-değer arasında SQLite veritabanları ve kullanıcı Arabirimi öğeleri Xcode'un arabirim oluşturucu içinde veri bağlamayı izin vermek için gözleme kullanarak kapsar. SQLite verilerine erişim sağlaması için SQLite.NET ORM kullanma kapsar._

## <a name="overview"></a>Genel Bakış

C# ve .NET ile bir Xamarin.Mac uygulamasında çalışırken, bir Xamarin.iOS veya Xamarin.Android uygulamaya erişebilmesi için SQLite veritabanlarına erişebilirsiniz.

Bu makalede biz SQLite veri erişmenin iki yöntemi kapsayan:

1. **Doğrudan erişim** - bir SQLite veritabanından doğrudan erişerek, veritabanındaki verileri anahtar-değer kodlaması için kullanabiliriz ve veri bağlama UI öğesi Xcode'un arabirimi Oluşturucu'da oluşturuldu. Anahtar-değer kodlaması ve veri teknikleri Xamarin.Mac uygulamanızda bağlama kullanarak, yazma ve doldurmak ve kullanıcı Arabirimi öğeleri ile çalışmak için korumanız gereken kod miktarını önemli ölçüde düşürebilir. Ayrıca, yedekleme verilerinizi daha fazla ayırma avantajı vardır (_veri modeli_) kullanıcı arabirimi, Önden bitiş (_Model-View-Controller_), bakımı kolay, daha esnek bir uygulama için önde gelen Tasarım.
2. **SQLite.NET ORM** - açık kaynak kullanarak [SQLite.NET](http://www.sqlite.org) size önemli ölçüde azaltabilirsiniz bir SQLite veritabanından veri yazma ve okuma için gereken kod miktarını nesne ilişki Yöneticisi (ORM). Bu veriler daha sonra kullanıcı arabirimi öğesi gibi bir tablo görünümü doldurmak için kullanılabilir.

[![Çalışan uygulamaya örneği](databases-images/intro01.png "çalıştıran uygulama örneği")](databases-images/intro01-large.png#lightbox)

Bu makalede, şu anahtar-değer kodlaması ve bir Xamarin.Mac uygulamasını SQLite veritabanlarıyla veri bağlama ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Biz anahtar-değer kodlaması ve veri bağlama kullandıklarından, lütfen aracılığıyla iş [veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md) ilk teknikler çekirdek ve kavramları arasında kapsamdaki kullanılacak olan bu belgeleri ve kendi örnek uygulama.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve UI öğeleri, C# sınıfları kablo kullanılır.

## <a name="direct-sqlite-access"></a>Doğrudan SQLite erişim

Xcode'un arabirim Oluşturucu UI öğelerine bağlanacak geçiyor SQLite veriler için yüksek oranda önerilir (değil doğrudan bir ORM gibi bir yöntem kullanarak) SQLite veritabanı erişim, yolu üzerinde tam denetime sahip olduğundan veri yazılan okuyun ve  veritabanından kaldırılıyor.

İçinde anlatıldığı gibi [veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md) belgeleri, anahtar-değer kodlaması ve veri teknikleri Xamarin.Mac uygulamanızda bağlama kullanarak, büyük ölçüde düşebilir yazmanız gereken kod miktarını ve doldurma ve kullanıcı Arabirimi öğeleri ile çalışmak için korur. Bir SQLite veritabanından doğrudan erişimle birlikte kullanıldığında, okuma ve bu veritabanına veri yazmak için gereken kod miktarını büyük ölçüde azaltabilir.

Bu makalede, biz veri bağlama ve anahtar-değer kodlama belge bir SQLite veritabanı için bağlama yedekleme kaynağı olarak kullanmak için örnek uygulamayı değiştirme.

### <a name="including-sqlite-database-support"></a>SQLite veritabanı desteği dahil olmak üzere

Devam etmeden önce size birkaç başvuruları ekleyerek uygulamamız için SQLite veritabanı desteği eklemeniz gerekir. DLL dosyaları.

Aşağıdakileri yapın:

1. İçinde **çözüm bölmesi**, sağ **başvuruları** klasörü ve select **başvuruları Düzenle**.
2. Her ikisini de seçin **Mono.Data.Sqlite** ve **System.Data** derlemeler: 

    [![Gerekli başvuru ekleme](databases-images/reference01.png "gerekli başvuru ekleme")](databases-images/reference01-large.png#lightbox)
3. Tıklayın **Tamam** düğmesine yaptığınız değişiklikleri kaydedin ve başvurular ekleyin.

### <a name="modifying-the-data-model"></a>Veri modelini değiştirme

Uygulamamız için bir SQLite veritabanından doğrudan erişmek için destek ekledik, biz bizim veri modeli okumak ve veritabanından veri yazma (aynı zamanda anahtar-değer kodlaması ve veri bağlama sağlar) nesnesine değiştirmeniz gerekir. Örnek uygulamamızla söz konusu olduğunda, biz Düzenle **PersonModel.cs** sınıfı ve aşağıdaki gibi görünmesi:

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

Birkaç önce ekledik SQLite kullanmak için gerekli olan deyimleri ve bizim son bağlantı SQLite veritabanına kaydetmek için bir değişken ekledik:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Kullanıcı veri bağlama aracılığıyla kullanıcı arabiriminde içeriği değiştirdiğinde otomatik olarak herhangi bir değişiklik kaydı veritabanına kaydetmek için kaydedilmiş bu bağlantıyı kullanacağız:

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

Yapılan tüm değişiklikler **adı**, **meslek** veya **isManager** özellik verileri var. önce kaydedildiyse veritabanına gönderilir (örneğin, `_conn` değişken `null`). Ardından, ekledik yöntemleri göz atalım **Oluştur**, **güncelleştirme**, **yük** ve **Sil** veritabanından kişiler.

#### <a name="create-a-new-record"></a>Yeni bir kayıt oluşturun

SQLite veritabanı'nda yeni bir kayıt oluşturmak için aşağıdaki kodu eklendi:

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

Kullanmakta olduğunuz bir `SQLiteCommand` veritabanında yeni bir kayıt oluşturmak için. Yeni bir komut aldığımız `SQLiteConnection` (biz çağırarak metodun Metoda geçilen conn) `CreateCommand`. Ardından, biz aslında yeni bir kayıt yazmak için SQL yönerge parametreleri için gerçek değerleri sağlayarak ayarlayın:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Kullanarak parametreleri için değerleri daha sonra ayarladığımız `Parameters.AddWithValue` metodunda `SQLiteCommand`. Parametreleri kullanarak (örneğin, tek tırnak işareti) düzgün için SQLite gönderilmeden önce kodlanmış emin emin oluruz. Örnek:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Son olarak, biz, yinelemeli bir kişi bir yönetici olması ve bunları altında çalışan bir koleksiyonunuz olduğundan, çağırma `Create` bunları veritabanına kaydetmek için bu kişilere yöntemi:

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

#### <a name="updating-a-record"></a>Bir kaydı güncelleştirme

SQLite veritabanındaki varolan bir kaydı güncelleştirmek için aşağıdaki kodu eklendi:

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

Gibi **Oluştur** yukarıdaki aldığımız bir `SQLiteCommand` geçirilen gelen içinde `SQLiteConnection`, (parametreleri sağlayarak) bizim kaydı güncelleştirmek için sunduğumuz SQL ayarlayın:

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Şu parametre değerleri doldurun (örnek: `command.Parameters.AddWithValue ("@COL1", ID);`) ve yeniden yinelemeli olarak çağrı güncelleştirme tüm alt kayıtları:

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

Varolan bir kaydı SQLite veritabanından yüklemek için aşağıdaki kodu eklendi:

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

Yordam (örneğin, çalışanlar nesne yüklenirken bir Yöneticisi nesnesi) üst nesneden özyinelemeli olarak çağrılabilir olduğundan, açılış ve kapanış veritabanı bağlantısını işlemek için özel kod eklendi:

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

Her zaman olduğu gibi sunduğumuz SQL kayıt alma ve parametreleri ayarlayın:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Son olarak, bir veri okuyucu sorguyu ve kaydın alanları döndürmek için kullanırız (biz örneğini kopyalayın, `PersonModel` sınıfı):

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

Bu kişiyi bir yönetici ise, biz de tüm çalışanlarına yüklemeniz gerekir (yeniden yinelemeli olarak çağırarak kendi `Load` yöntemi):

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

Varolan bir kaydı SQLite veritabanından silmek için aşağıdaki kodu eklendi:

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

Burada hem yöneticileri kaydı hem de (parametrelerini kullanarak) bu Yöneticisi altında herhangi bir çalışan kayıtlarını silmek için SQL sağlıyoruz:

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Kaydı kaldırıldıktan sonra biz'ün geçerli örneğini Temizle `PersonModel` sınıfı:

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

Veri Modelimizi yerinde okuma ve yazma veritabanına desteklemek için değişiklik yapmadan, veritabanına bir bağlantı açmak ve ilk çalıştırılmasında başlatmak ihtiyacımız var. Aşağıdaki kodu ekleyelim bizim **MainWindow.cs** dosyası:

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

Yukarıdaki kod daha yakın bir göz atalım. İlk olarak, veritabanı bulunup bulunmadığına bakın ve bu gereksinimleri karşılamıyorsa oluşturun (Bu örnekte, kullanıcının Masaüstü), yeni veritabanı için size bir konum seçin:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

Ardından, yukarıda oluşturduğumuz yolu kullanarak veritabanına bağlanma kurar:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

Ardından tüm SQL tabloları kılarız veritabanında oluştururuz:

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

Son olarak, veri Modelimizi kullanırız (`PersonModel`) uygulama veritabanı ilk çalıştırıldığında veya veritabanı eksik olup olmadığını için varsayılan bir kayıt kümesi oluşturmak için:

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

Uygulama başlatır ve ana penceresi açılır, yukarıda ekledik kodu kullanarak veritabanına bir bağlantı vermiyoruz:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Yükleme, veri bağlama

Doğrudan bir yerde bir SQLite veritabanından ilişkili verilerine erişmek için tüm bileşenlerle, biz uygulamamızı sağlayan ve otomatik olarak bizim kullanıcı Arabiriminde görüntülenen farklı görünümlerdeki verileri yükleyebilir.

#### <a name="loading-a-single-record"></a>Tek bir kaydı yükleniyor

Tek bir kayıt kimliği olduğu bilmeniz yüklemek için aşağıdaki kod kullanabiliriz:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Tüm kayıtları yükleniyor

Bir yönetici veya yok olması durumunda tüm kişilerin, bağımsız olarak yüklemek için aşağıdaki kodu kullanın:

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

Burada bir aşırı yüklemesini Oluşturucusu kullanıyoruz `PersonModel` sınıfı kişi belleğe yüklemek için:

```csharp
var person = new PersonModel (_conn, childID);
```

Biz de kişi kişilerin bizim koleksiyona eklemek için veri bağlama sınıfı çağırma `AddPerson (person)`, bu kullanıcı Arabirimimizi değişikliği algılar ve görüntüler sağlar:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Yalnızca üst düzey kayıtlar yükleniyor

Yalnızca Yöneticiler (örneğin, bir ana görünümünde verileri görüntülemek için) yüklemek için aşağıdaki kodu kullanırız:

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

SQL deyimi içinde yalnızca gerçek farkı (yalnızca Yöneticiler yükler `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`) ancak Aksi halde aynı bölümde yukarıdaki çalışır.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Veritabanları ve comboboxes

MacOS (örneğin, birleşik giriş kutusu) kullanılabilir menü denetimleri (, arabirim oluşturucu içinde önceden tanımlanmış veya kod doldurulur) bir iç listesinden ya da açılan listeyi doldurmak için ayarlanabilir veya kendi özel, dış veri kaynağı sağlayarak. Bkz: [menü denetimini veri sağlayan](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) daha fazla ayrıntı için.

Örneğin, düzenleme, arabirim Oluşturucu basit bağlama örnek yukarıda bir birleşik giriş kutusu ekleyin ve adlı bir çıkış kullanarak üzerinden kullanıma sunacaksınız `EmployeeSelector`:

[![Birleşik giriş kutusu çıkışı gösterme](databases-images/combo01.png "birleşik giriş kutusu çıkışı gösterme")](databases-images/combo01-large.png#lightbox)

İçinde **öznitelikleri denetçisi**, kontrol **yanı** ve **veri kaynağını kullanan** özellikleri:

![Birleşik giriş kutusu öznitelikleri yapılandırma](databases-images/combo02.png "birleşik giriş kutusu öznitelikleri yapılandırma")

Değişikliklerinizi kaydetmek ve eşitlemek Mac için Visual Studio geri dönün.

#### <a name="providing-combobox-data"></a>ComboBox verileri sağlama

Ardından, yeni bir sınıf adlı projeye ekleyin `ComboBoxDataSource` ve aşağıdaki gibi görünmesi:

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

Bu örnekte, yeni bir oluşturuyoruz `NSComboBoxDataSource` birleşik giriş kutusu öğeleri herhangi bir SQLite veri kaynağından sunabileceği. İlk olarak, aşağıdaki özellikler tanımlayın:

- `Conn` -Alır veya SQLite veritabanına bir bağlantı ayarlar.
- `TableName` -Alır veya tablo adını ayarlar.
- `IDField` -Alanını alır veya verilen tablo için benzersiz Kimliğini sağlayan ayarlar. Varsayılan değer `ID` şeklindedir.
- `DisplayField` -Alır veya ayarlar alanın açılan listede görüntülenir.
- `RecordCount` -Belirli bir tablodaki kayıt sayısını alır.

Biz nesnesinin yeni bir örneğini oluşturduğunuzda, bağlantı, tablo adı, isteğe bağlı olarak kimlik alanı ve görüntüleme alanına geçirin:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

`GetRecordCount` Yöntemi, belirtilen tablodaki kayıt sayısını döndürür:

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

Dilediğiniz zaman çağrılır `TableName`, `IDField` veya `DisplayField` özellikleri değeri değiştirilir.

`IDForIndex` Yöntem benzersiz kimliği döndürür (`IDField`) belirli bir açılır liste öğesi dizini kaydı için: 

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

`ValueForIndex` Yöntemi değeri döndürür (`DisplayField`) için belirli bir açılır liste dizinindeki öğe:

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

`IDForValue` Yöntem benzersiz kimliği döndürür (`IDField`) için belirtilen değer (`DisplayField`):

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

`ItemCount` Ne zaman hesaplanan şekilde listedeki filtrelerinde öğe sayısını döndürür `TableName`, `IDField` veya `DisplayField` özellikleri değiştirilir:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

`ObjectValueForItem` Yöntemi değeri sağlar (`DisplayField`) için belirli bir açılır liste öğesi dizini:

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

Kullanıyoruz bildirimi `LIMIT` ve `OFFSET` deyimlerinde size gereken tek bir kayıtta sınırlamak için sunduğumuz SQLite komutu.

`IndexOfItem` Yöntem açılan öğe dizini değeri döndürür (`DisplayField`) verilen:

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

Değeri bulunamazsa `NSRange.NotFound` değer dönüş ve tüm öğeleri açılan listede seçili.

`CompletedString` Yöntemi ilk eşleşen değeri döndürür (`DisplayField`) kısmen yazılı girişi için:

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

#### <a name="displaying-data-and-responding-to-events"></a>Verileri görüntüleme ve olaylarına yanıt verme

Tüm parçaları bir araya getirip Düzenle `SubviewSimpleBindingController` ve aşağıdaki gibi görünmesi:

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

`DataSource` Özelliği için bir kısayol sağlar `ComboBoxDataSource` (Yukarıdaki açılan kutu bağlı oluşturulur).

`LoadSelectedPerson` Yöntemi için sağlanan benzersiz kimliği veritabanından kişi yükler:

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

İçinde `AwakeFromNib` yöntemi geçersiz kılma, ilk biz özel birleşik giriş kutusu veri Kaynağımıza örneğini ekleyin:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

Ardından, açılan kutunun metin değeri ilişkili benzersiz kimliği bularak düzenleme kullanıcı için yanıtlarız (`IDField`) verileri sunmak ve belirli kişi, yükleme bulundu:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

Kullanıcı açılır listeden yeni bir öğe seçtiğinde biz de yeni bir kişiye yükle:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Son olarak, biz otomatik olarak birleşik giriş kutusu ve listedeki ilk öğe ile görüntülenen kişi doldurmak:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Açık kaynak kullanarak, yukarıda belirtildiği gibi [SQLite.NET](http://www.sqlite.org) size önemli ölçüde azaltabilirsiniz bir SQLite veritabanından veri yazma ve okuma için gereken kod miktarını nesne ilişki Yöneticisi (ORM). Birkaç anahtar-değer kodlaması ve veri bağlama, bir nesne üzerinde yerleştirmek gereksinimleri nedeniyle, veri bağlama sırasında yapılacak en iyi yol bu olmayabilir.

SQLite.Net Web sitesi göre _"SQLite müstakil, sunucusuz, sıfır yapılandırmalı, işlem bir SQL veritabanı altyapısı uygulayan bir yazılım kitaplık sunulmaktadır. SQLite dünyanın en yaygın olarak dağıtılan bir veritabanı motorudur. SQLite için kaynak kodu ortak etki alanında var."_

Aşağıdaki bölümlerde, SQLite.Net için bir tablo görünümü verilerini sağlamak için nasıl kullanılacağını göstereceğiz.

### <a name="including-the-sqlitenet-nuget"></a>SQLite.net NuGet dahil

SQLite.NET uygulamanıza dahil bir NuGet paketi olarak sunulur. SQLite.NET kullanarak veritabanı desteği ekleriz önce Biz bu paket eklemeniz gerekir.

Paket eklemek için aşağıdakileri yapın:

1. İçinde **çözüm bölmesi**, sağ **paketleri** klasörü ve select **paketleri Ekle...**
2. Girin `SQLite.net` içinde **arama kutusuna** seçip **sqlite net** girişi:

    [![SQLite NuGet paketini ekleme](databases-images/nuget01.png "SQLite NuGet paketi ekleme")](databases-images/nuget01-large.png#lightbox)
3. Tıklayın **Paketi Ekle** düğmesini tamamlayın.

### <a name="creating-the-data-model"></a>Veri modeli oluşturma

Şimdi projeye yeni bir sınıf ekleyin ve arama `OccupationModel`. Ardından, düzenleyelim **OccupationModel.cs** dosyasını açıp aşağıdaki gibi görünmesi:

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

İlk olarak biz SQLite.NET içerir (`using Sqlite`), ardından kullanıma sunuyoruz çeşitli özellikleri, her biri yazılacak veritabanına bu kaydı kaydedildiğinde. İlk özellik biz birincil anahtar olarak yapın ve otomatik artırma için şu şekilde ayarlayın:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Veritabanı başlatılıyor

Veri Modelimizi yerinde okuma ve yazma veritabanına desteklemek için değişiklik yapmadan, veritabanına bir bağlantı açmak ve ilk çalıştırılmasında başlatmak ihtiyacımız var. Şimdi aşağıdaki kodu ekleyin:

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

İlk olarak biz bir yolu ' % s'veritabanı (Bu durumda kullanıcının Masaüstü) ve veritabanı zaten var olup olmadığını:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

Ardından, yukarıda oluşturduğumuz yolda veritabanına bir bağlantı kurar:

```csharp
var conn = new SQLiteConnection (db);
```

Son olarak, biz tablosu oluşturun ve bazı varsayılan kayıtları ekleyin:

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

### <a name="adding-a-table-view"></a>Tablo görünümü ekleme

Örnek kullanım, kullanıcı Arabirimimizi Xcode'un arabirimi Oluşturucu için bir tablo görünümü ekleyeceğiz. Bu tablo görünümü prizine aracılığıyla kullanıma sunuyoruz (`OccupationTable`) size C# kodu aracılığıyla erişebilmesi için:

[![Tablo görünümü çıkışı gösterme](databases-images/table01.png "bir tablo görünümü çıkışı gösterme")](databases-images/table01-large.png#lightbox)

Ardından, bu tabloyu SQLite.NET veritabanındaki verilerle doldurmak için özel sınıflar ekleyeceğiz.

### <a name="creating-the-table-data-source"></a>Tablosu veri kaynağı oluşturma

Tablomuza için veri sağlamak için özel bir veri kaynağı oluşturalım. İlk olarak, adlı yeni bir sınıf ekleyin `TableORMDatasource` ve aşağıdaki gibi görünmesi:

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

Daha sonra bu sınıfın bir örneği oluşturduğumuzda, biz de Aç bizim SQLite.NET veritabanı bağlantısını ileteceksiniz. `LoadOccupations` Yöntemi veritabanını sorgular ve belleğe bulunan kayıtları kopyalar (kullanarak bizim `OccupationModel` veri modeli).

### <a name="creating-the-table-delegate"></a>Tablo temsilci oluşturma

İhtiyacımız son SQLite.NET veritabanından yüklendik bilgileri görüntülemek için özel bir tablo temsilci sınıftır. Yeni bir ekleyelim `TableORMDelegate` Projemizin için ve aşağıdaki gibi görünmesi:

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

Veri kaynağının burada kullandığımız `Occupations` (SQLite.NET veritabanından yüklendik) koleksiyonu tablomuza sütunlarının doldurmak için `GetViewForItem` yöntemi geçersiz kılma.

### <a name="populating-the-table"></a>Tablo doldurma

.Xib dosyasından geçersiz kılarak şişirileceğini zaman tüm parçaları yerinde, şimdi tablomuza doldurmak `AwakeFromNib` yöntemi ve bu konum, aşağıdaki gibidir:

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

İlk olarak biz SQLite.NET veritabanımızdaki oluşturma ve önceden yoksa, doldurma erişin. Ardından, yeni bir örneğini oluşturacağız bizim Özel tablosu veri kaynağı bizim veritabanı bağlantısında geçirin ve size tablosuna ekleyin. Son olarak, biz bizim özel tablo temsilci yeni bir örneğini oluşturmak, veri Kaynağımıza geçirin ve tabloya ekleyin.

## <a name="summary"></a>Özet

Bu makalede, veri bağlama ve bir Xamarin.Mac uygulamasını SQLite veritabanlarında anahtar-değer kodlaması ile çalışmak üzere ayrıntılı bir bakış duruma getirdi. İlk olarak, C# sınıfı Objective-c (KVC) anahtar-değer kodlaması kullanarak ve anahtar-değer (KVO) gözleme gösterme görünüyordu. Ardından, KVO uyumlu sınıfının nasıl kullanılacağını gösterdi ve veri Xcode'un arabirim Oluşturucu UI öğelerine bağlar. Makaleyi SQLite.NET ORM aracılığıyla SQLite verilerle çalışmaya ve bu verileri Tablo görünümünde görüntüleyerek de kapsar.



## <a name="related-links"></a>İlgili bağlantılar

- [MacDatabase (örnek)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Veri bağlama ve anahtar-değer kodlaması](~/mac/app-fundamentals/databinding.md)
- [Standart denetimler](~/mac/user-interface/standard-controls.md)
- [Tablo görünümleri](~/mac/user-interface/table-view.md)
- [Anahat görünümleri](~/mac/user-interface/outline-view.md)
- [Koleksiyon görünümleri](~/mac/user-interface/collection-view.md)
- [Anahtar-değer kodlaması Programlama Kılavuzu](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Cocoa bağlamaları programlama konuları giriş](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Cocoa bağlamaları başvuru giriş](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [macOS İnsan Arabirimi yönergelerine](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
