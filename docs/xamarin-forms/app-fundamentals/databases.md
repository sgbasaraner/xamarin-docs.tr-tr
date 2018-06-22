---
title: Xamarin.Forms yerel veritabanları
description: Xamarin.Forms veritabanı güdümlü uygulamaları yüklemek ve paylaşılan kodda nesneleri kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak destekler. Bu makalede nasıl Xamarin.Forms uygulamaları okuma ve SQLite.Net kullanarak yerel bir SQLite veritabanı için veri yazma açıklanmaktadır.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: feec4993a0719a083d713e084552b18aead8ee42
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310146"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms yerel veritabanları

_Xamarin.Forms veritabanı güdümlü uygulamaları yüklemek ve paylaşılan kodda nesneleri kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak destekler. Bu makalede nasıl Xamarin.Forms uygulamaları okuma ve SQLite.Net kullanarak yerel bir SQLite veritabanı için veri yazma açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms uygulamalar kullanabilir [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) veritabanı işlemlere eklemenizi paket başvurarak kod paylaşılan `SQLite` NuGet sevk sınıfları. Veritabanı işlemleri Xamarin.Forms çözümünü .NET standart kitaplığı projesinde tanımlanabilir.

Eşlik eden [örnek uygulama](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) basit bir Yapılacaklar listesi uygulamasıdır. Aşağıdaki ekran görüntüleri örnek her platformda nasıl göründüğünü gösterir:

[![Xamarin.Forms veritabanı örnek ekran görüntüleri](databases-images/todo-list-sml.png "TodoList ilk sayfa ekran görüntüleri")](databases-images/todo-list.png#lightbox "TodoList ilk sayfa ekran görüntüleri") [ ![ Xamarin.Forms veritabanı örnek ekran görüntüleri](databases-images/todo-list-sml.png "TodoList ilk sayfa ekran görüntüleri")](databases-images/todo-list.png#lightbox "TodoList ilk sayfa ekran görüntüleri")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite kullanarak

Xamarin.Forms .NET standart kitaplığına SQLite desteği eklemek için bulmak için NuGet arama işlevini kullanın **sqlite net pcl** ve en son paketini yükleyin:

![NuGet SQLite.NET PCL paket ekleme](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL Paketi Ekle")

NuGet paketlerini benzer adlara sahip bir dizi vardır, doğru paket bu özniteliklere sahiptir:

- **Tarafından oluşturulan:** Frank A. Krueger
- **Kimliği:** sqlite net pcl
- **NuGet bağlantı:** [sqlite net pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Paket adı rağmen kullanmak **sqlite net pcl** bile .NET standart projelerinde NuGet paketi.

Başvuru eklendikten sonra yeni özellik eklemek `App` veritabanını depolamak için bir yerel dosya yolu döndürür sınıfı:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

`TodoItemDatabase` Bağımsız değişken olarak veritabanı dosyasının yolunu alır, Oluşturucusu aşağıda gösterilmektedir:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Uygulama çalışırken açık tutulan tek tek veritabanı bağlantısı oluşturduğunuz olduğundan veritabanı gösterme avantajı çalışır, bu nedenle veritabanı işlemi açma ve veritabanı dosyası her zaman kapatılırken gider önleme gerçekleştirilir.

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

## <a name="summary"></a>Özet

Xamarin.Forms veritabanı güdümlü uygulamaları yüklemek ve paylaşılan kodda nesneleri kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak destekler.

Bu makalede odaklanan **erişme** Xamarin.Forms kullanarak bir SQLite veritabanı. SQLite.Net kendisi ile çalışma hakkında daha fazla bilgi için başvurmak [android'de SQLite.NET](~/android/data-cloud/data-access/using-sqlite-orm.md) veya [iOS SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md) belgeleri.

## <a name="related-links"></a>İlgili bağlantılar

- [Yapılacaklar örnek](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Veritabanı çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
