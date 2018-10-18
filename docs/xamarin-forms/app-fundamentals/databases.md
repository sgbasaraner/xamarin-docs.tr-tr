---
title: Xamarin.Forms yerel veritabanları
description: Xamarin.Forms, yüklemek ve nesneleri paylaşılan kodu kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak veritabanı odaklı uygulamaları destekler. Bu makalede nasıl Xamarin.Forms uygulamalarını okuma ve SQLite.Net kullanarak yerel bir SQLite veritabanı için veri yazma açıklanır.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 05c77c4c47841a897d0d1de5c3ba004db524ea2d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "36310146"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms yerel veritabanları

_Xamarin.Forms, yüklemek ve nesneleri paylaşılan kodu kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak veritabanı odaklı uygulamaları destekler. Bu makalede nasıl Xamarin.Forms uygulamalarını okuma ve SQLite.Net kullanarak yerel bir SQLite veritabanı için veri yazma açıklanır._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms uygulamaları kullanabilir [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) veritabanı işlemi birleştirmek için paket paylaşılan kodu başvurarak `SQLite` Nuget'te sevk sınıfları. Veritabanı işlemleri Xamarin.Forms çözümü .NET Standard kitaplığı projesinde tanımlanabilir.

Eşlik eden [örnek uygulama](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) basit bir Yapılacaklar listesi uygulamasıdır. Aşağıdaki ekran görüntüleri örnek her platformda nasıl göründüğünü gösterir:

[![Xamarin.Forms veritabanı örnek ekran görüntüleri](databases-images/todo-list-sml.png "TodoList ilk sayfa ekran görüntüleri")](databases-images/todo-list.png#lightbox "TodoList ilk sayfa ekran görüntüleri") [ ![ Xamarin.Forms veritabanı örnek ekran görüntüleri](databases-images/todo-list-sml.png "TodoList ilk sayfa ekran görüntüleri")](databases-images/todo-list.png#lightbox "TodoList ilk sayfa ekran görüntüleri")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>SQLite kullanarak

Xamarin.Forms .NET Standard kitaplığı için SQLite desteği eklemek için bulmak için NuGet'ın arama işlevini kullanın **sqlite net pcl** ve en son paketini yükleyin:

![NuGet SQLite.NET PCL paketi ekleme](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL paketi ekleme")

Birçok NuGet paketlerinin benzer adlarla, doğru paketi bu özniteliklere sahiptir:

- **Oluşturan:** Frank A. Krueger
- **ID:** sqlite net pcl
- **NuGet bağlantı:** [sqlite net pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Paket adı rağmen **sqlite net pcl** NuGet paketini bile .NET Standard projelerine.

Başvuru eklendikten sonra özellik eklemek `App` veritabanını depolamak için bir yerel dosya yolu döndürür sınıfı:

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

`TodoItemDatabase` Yolun veritabanı dosyası bağımsız değişken olarak alan oluşturucu, aşağıda gösterilmektedir:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Uygulama çalışırken açık tutulan tek tek veritabanı bağlantısı oluşturulur olduğundan veritabanı gösterme avantajı çalışır ve bu nedenle açmak ve her veritabanı dosyası kapatılırken bir veritabanı işlemi önleme gerçekleştirilir.

Kalanı `TodoItemDatabase` sınıfı, platformlar arası çalışan SQLite sorguları içerir. Örnek sorgu kod aşağıda gösterilmektedir (söz dizimi hakkında daha fazla ayrıntı bulunabilir [kullanarak SQLite.NET Xamarin.iOS ile](~/ios/data-cloud/data/using-sqlite-orm.md).

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
> Zaman uyumsuz SQLite.Net API kullanmanın avantajı, işlemleri arka plan iş parçacığı taşınır, veritabanıdır. Ayrıca, ek eşzamanlılık API bunu üstlenir çünkü kod işleme yazmaya gerek yoktur.

## <a name="summary"></a>Özet

Xamarin.Forms, yüklemek ve nesneleri paylaşılan kodu kaydetmek mümkün kılar SQLite veritabanı altyapısı kullanılarak veritabanı odaklı uygulamaları destekler.

Bu makalede odaklanan **erişme** Xamarin.Forms kullanarak bir SQLite veritabanı. SQLite.Net kendisi ile çalışma hakkında daha fazla bilgi için [Android SQLite.NET](~/android/data-cloud/data-access/using-sqlite-orm.md) veya [ios'ta SQLite.NET](~/ios/data-cloud/data/using-sqlite-orm.md) belgeleri.

## <a name="related-links"></a>İlgili bağlantılar

- [Yapılacaklar örneği](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms Örnekleri](https://developer.xamarin.com/samples/xamarin-forms/all/)

