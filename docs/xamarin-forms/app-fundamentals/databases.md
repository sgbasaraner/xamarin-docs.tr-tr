---
title: "Yerel veritabanı"
description: "Xamarin.Forms veritabanı güdümlü uygulamaları yüklemek ve paylaşılan kodda nesneleri kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak destekler. Bu makalede nasıl Xamarin.Forms uygulamaları okuma ve SQLite.Net kullanarak yerel bir SQLite veritabanı için veri yazma açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2017
ms.openlocfilehash: 29686b29a18fe409a1f778d54266cbeedea40eda
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="local-databases"></a>Yerel veritabanı

_Xamarin.Forms veritabanı güdümlü uygulamaları yüklemek ve paylaşılan kodda nesneleri kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak destekler. Bu makalede nasıl Xamarin.Forms uygulamaları okuma ve SQLite.Net kullanarak yerel bir SQLite veritabanı için veri yazma açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms uygulamalar kullanabilir [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) veritabanı işlemlere eklemenizi paket başvurarak kod paylaşılan `SQLite` NuGet sevk sınıfları. Veritabanı işlemleri veritabanı depolanacağı için bir yol döndürme platforma özgü projeleri Xamarin.Forms çözümün taşınabilir sınıf kitaplığı (PCL) projesiyle tanımlanabilir.

Eşlik eden [örnek uygulama](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) basit bir Yapılacaklar listesi uygulamasıdır. Aşağıdaki ekran görüntüleri örnek her platformda nasıl göründüğünü gösterir:

[![Xamarin.Forms veritabanı örnek ekran görüntüleri](databases-images/todo-list-sml.png "TodoList ilk sayfa ekran görüntüleri")](databases-images/todo-list.png#lightbox "TodoList ilk sayfa ekran görüntüleri") [ ![ Xamarin.Forms veritabanı örnek ekran görüntüleri](databases-images/todo-list-sml.png "TodoList ilk sayfa ekran görüntüleri")](databases-images/todo-list.png#lightbox "TodoList ilk sayfa ekran görüntüleri")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite kullanarak

Bu bölümde SQLite.Net NuGet paketleri bir Xamarin.Forms çözüme eklemek için veritabanı işlemleri ve kullanmak için yöntemleri yazma gösterilmiştir [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) her platformda veritabanını depolamak için bir konum belirlemek için.

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-pcl-project"></a>Xamarins.Forms PCL proje

Xamarin.Forms PCL projeye SQLite desteği eklemek için bulmak için NuGet arama işlevini kullanın **sqlite net pcl** paketini ve yükleyin:

![](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL Paketi Ekle")

NuGet paketlerini benzer adlara sahip bir dizi vardır, doğru paket bu özniteliklere sahiptir:

- **Tarafından oluşturulan:** Frank A. Krueger
- **Kimliği:** sqlite net pcl
- **NuGet bağlantı:** [sqlite net pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

Başvuru eklendikten sonra veritabanı dosyasının konumunu belirlemektir platforma özgü işlevselliği soyut bir arabirim yazma. Aşağıdaki örnekte kullanılan arabirimi tek bir yöntem tanımlar:

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

Arabirim tanımlandıktan sonra kullanmak [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) uygulaması elde edilir ve bir yerel dosya yolu (Bu arabirim henüz uygulanmadı olduğunu unutmayın). Aşağıdaki kod uygulaması alır `App.Database` özelliği:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` Oluşturucusu aşağıda gösterilmektedir:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Bu yaklaşım, uygulama, bu nedenle, açma ve veritabanı dosyası bir veritabanı işlemi gerçekleştirilen her zaman kapatılırken gider önleme çalışırken, açık tutulur tek veritabanı bağlantısı oluşturur.

Geri kalan `TodoItemDatabase` sınıfı, platformlar arası çalışması SQLite sorguları içerir. Örnek sorgu kod aşağıda gösterilen (sözdizimi hakkında daha fazla ayrıntı bulunabilir [kullanarak SQLite.NET](~/cross-platform/app-fundamentals/index.md) makale):

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> Zaman uyumsuz SQLite.Net API kullanmanın avantajı, işlemleri arka plan iş parçacıkları taşınır bu veritabanıdır. Ayrıca, ek eşzamanlılık API bunu mvc'deki çünkü kod işleme yazmaya gerek yoktur.

Tüm veri erişim kodu tüm platformlarda paylaşılması için PCL projede yazılır. Yalnızca veritabanı için bir yerel dosya yolu alma platforma özgü kod, aşağıdaki bölümlerde özetlenen gerektirir.

<a name="PCL_iOS" />

### <a name="ios-project"></a>iOS Project

İOS uygulama yapılandırmak için iOS kullanarak projesi aynı NuGet paketi ekleme *NuGet* penceresi:

![](databases-images/vsmac-sqlite-nuget.png "NuGet SQLite.NET PCL Paketi Ekle")

Gerekli yalnızca kodu `IFileHelper` uygulamasında, veri dosyası yolu belirler. Aşağıdaki kod SQLite veritabanı dosyasına yerleştirir **kitaplık/veritabanları** uygulamanın Korumalı alan klasördeki. Bkz: [iOS dosya sistemi ile çalışma](~/ios/app-fundamentals/file-system.md) depolaması için kullanılabilecek farklı dizinleri hakkında daha fazla bilgi için.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

            if (!Directory.Exists(libFolder))
            {
                Directory.CreateDirectory(libFolder);
            }

            return Path.Combine(libFolder, filename);
        }
    }
}
```

Kod içeren Not `assembly:Dependency` bu uygulama tarafından bulunabilmesini böylece özniteliği `DependencyService`.

<a name="PCL_Android" />

### <a name="android-project"></a>Android Project

Android uygulaması yapılandırmak için aynı NuGet paketi kullanarak Android projesi ekleme *NuGet* penceresi:

![](databases-images/vsmac-sqlite-nuget.png "NuGet SQLite.NET PCL Paketi Ekle")

Bu başvuru eklendikten sonra gerekli yalnızca kodudur `IFileHelper` uygulamasında, veri dosyası yolu belirler.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
            return Path.Combine(path, filename);
        }
    }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Windows 10 Evrensel Windows Platformu (UWP)

UWP uygulaması yapılandırmak için UWP projesi kullanarak aynı NuGet paketi ekleme *NuGet* penceresi:

![](databases-images/vs2017-sqlite-uwp-nuget.png "NuGet SQLite.NET PCL Paketi Ekle")

Başvuru eklendikten sonra uygulama `IFileHelper` arabirimi platforma özgü kullanılarak `Windows.Storage` veri dosya yolu belirlemek için API.

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
    public class FileHelper : IFileHelper
    {
        public string GetLocalFilePath(string filename)
        {
            return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
        }
    }
}

```

## <a name="summary"></a>Özet

Xamarin.Forms veritabanı güdümlü uygulamaları yüklemek ve paylaşılan kodda nesneleri kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak destekler.

Bu makalede odaklanan **erişme** Xamarin.Forms kullanarak bir SQLite veritabanı. SQLite.Net kendisi ile çalışma hakkında daha fazla bilgi için başvurmak [veri erişimi: kullanarak SQLite.NET](~/cross-platform/app-fundamentals/index.md) belgeleri. Çoğu SQLite.Net kodu tüm platformlar arasında paylaşılabilir.; yalnızca SQLite veritabanı dosyasının konumu yapılandırma platforma özgü işlevselliği gerektirir.


## <a name="related-links"></a>İlgili bağlantılar

- [Yapılacaklar örnek](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Veritabanı çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
