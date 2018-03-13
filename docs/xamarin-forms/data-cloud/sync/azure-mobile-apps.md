---
title: "Azure Mobile Apps ile çevrimdışı veri eşitleme"
description: "Çevrimdışı eşitleme sağlayan bir mobil uygulama, görüntüleme, ekleme veya verileri değiştirme ile etkileşim kullanıcılar bile bulunmadığı durumlarda bir ağ bağlantısı. Değişiklikler yerel bir veritabanında depolanır ve cihaz çevrimiçi olduktan sonra Azure Mobile Apps örneğiyle değişiklikleri eşitlenen. Bu makalede, bir Xamarin.Forms uygulaması çevrimdışı eşitleme işlevselliği eklemek açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: DBB343B0-2709-4C20-A669-5522B9956D9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2017
ms.openlocfilehash: 965d4987c154acc5a2f95d4ca622266ebdc2a1c2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="synchronizing-offline-data-with-azure-mobile-apps"></a>Azure Mobile Apps ile çevrimdışı veri eşitleme

_Çevrimdışı eşitleme sağlayan bir mobil uygulama, görüntüleme, ekleme veya verileri değiştirme ile etkileşim kullanıcılar bile bulunmadığı durumlarda bir ağ bağlantısı. Değişiklikler yerel bir veritabanında depolanır ve cihaz çevrimiçi olduktan sonra Azure Mobile Apps örneğiyle değişiklikleri eşitlenen. Bu makalede, bir Xamarin.Forms uygulaması çevrimdışı eşitleme işlevselliği eklemek açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

[Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/) sağlar `IMobileServiceTable` Azure Mobile Apps örneğinde depolanan tablolar üzerinde gerçekleştirilen işlemler temsil eden arabirim. Bu işlemleri doğrudan Azure Mobile Apps örneğine bağlanın ve mobil cihazda bir ağ bağlantısı yoksa başarısız olur.

Çevrimdışı eşitleme desteklemek için Azure Mobile istemci SDK'sı destekler *eşitleme tabloları*, tarafından sağlanan `IMobileServiceSyncTable` arabirimi. Bu arabirim sağlayan aynı oluşturma, okuma, güncelleştirme, Sil (CRUD) işlemleridir olarak `IMobileServiceTable` arabirimi ancak işlem okuma veya yerel depoya yazma. Çağrı kadar yerel deposunu Azure Mobile Apps örneğinden yeni verilerle doldurulur değil *çekme* veri. Çağrı kadar benzer şekilde, Azure Mobile Apps örneğine gönderilen veri değil *itme* yerel değişiklikler.

Çevrimdışı eşitleme hem yerel depodaki ve aynı kaydı değiştiğinde çakışmaları Azure Mobile Apps örneği ve özel çakışma çözümü saptamak için destek de içerir. Çakışmaları ya da yerel depodaki ya da Azure Mobile Apps örneğinde işlenebilir.

Çevrimdışı eşitleme hakkında daha fazla bilgi için bkz: [Azure Mobile Apps çevrimdışı veri eşitleme](/azure/app-service-mobile/app-service-mobile-offline-data-sync/) ve [Xamarin.Forms mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-offline-data/).

## <a name="setup"></a>Kurulum

Çevrimdışı eşitleme Xamarin.Forms uygulaması ile Azure Mobile Apps örneği arasında tümleştirme işlemi aşağıdaki gibidir:

1. Azure Mobile Apps örneği oluşturun. Daha fazla bilgi için bkz: [Azure mobil uygulaması tüketen](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Ekleme [Microsoft.Azure.Mobile.Client.SQLiteStore](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/) Xamarin.Forms Çözümdeki tüm projeleri için NuGet paketi.
1. (İsteğe bağlı) Azure Mobile Apps örneği ve Xamarin.Forms uygulaması kimlik doğrulamasını etkinleştirin. Daha fazla bilgi için bkz: [Azure Mobile Apps ile kimlik doğrulaması yapan kullanıcıların](~/xamarin-forms/data-cloud/authentication/azure.md).

Aşağıdaki bölümde Microsoft.Azure.Mobile.Client.SQLiteStore NuGet paketini kullanmak için evrensel Windows Platformu (UWP) projeleri yapılandırma için ek kurulum yönergeleri sağlar. İOS ve Android Microsoft.Azure.Mobile.Client.SQLiteStore NuGet paketini kullanmak için hiçbir ek ayar gereklidir.

### <a name="universal-windows-platform"></a>Evrensel Windows Platformu

Evrensel Windows Platformu (UWP) üzerinde SQLite kullanmak için aşağıdaki adımları izleyin:

1. Yükleme [SQLite Evrensel Windows platformu için](http://sqlite.org/2016/sqlite-uwp-3120200.vsix) geliştirme ortamınızı Visual Studio uzantı.
1. Visual Studio'da UWP projesi sağ tıklayın **başvuruları > Başvuru Ekle**, gitmek **uzantıları** ve ekleme **SQLite Evrensel Windows platformu için** ve  **Evrensel Windows platformu uygulamaları için Visual C++ 2015 çalışma zamanı** UWP projesini uzantıları.

## <a name="initializing-the-local-store"></a>Yerel deposu başlatılıyor

Yerel depodan eşitleme tablo işlemleri gerçekleştirilmeden önce başlatılması gerekir. Bu, Xamarin.Forms çözümünü taşınabilir sınıf kitaplığı (PCL) projesinde sağlanır:

```csharp
using Microsoft.WindowsAzure.MobileServices;
using Microsoft.WindowsAzure.MobileServices.SQLiteStore;
using Microsoft.WindowsAzure.MobileServices.Sync;

namespace TodoAzure
{
    public partial class TodoItemManager
    {
        static TodoItemManager defaultInstance = new TodoItemManager();
        IMobileServiceClient client;
        IMobileServiceSyncTable<TodoItem> todoTable;

        private TodoItemManager()
        {
            this.client = new MobileServiceClient(Constants.ApplicationURL);
            var store = new MobileServiceSQLiteStore("localstore.db");
            store.DefineTable<TodoItem>();
            this.client.SyncContext.InitializeAsync(store);
            this.todoTable = client.GetSyncTable<TodoItem>();
        }
        ...
  }
}
```

Yeni bir yerel SQLite veritabanı tarafından oluşturulan `MobileServiceSQLiteStore` sınıf koşuluyla zaten mevcut değil. Ardından, `DefineTable<T>` yöntemi alanları eşleşen yerel deposunda bir tablo oluşturur `TodoItem` koşuluyla, henüz yoksa yazın.

A *eşitleme bağlamı* ile ilişkili bir `MobileServiceClient` örneği ve eşitleme tablolarla yapılan değişiklikleri izler. Eşitleme bağlamı oluşturma, güncelleştirme ve silme (CUD) işlemleri sıralı bir listesini tutar bir kuyruk için Azure Mobile Apps örnek daha sonra gönderilecek tutar. `IMobileServiceSyncContext.InitializeAsync()` Yöntemi yerel deposu eşitleme bağlamı ile ilişkilendirmek için kullanılır.

`todoTable` Alan bir `IMobileServiceSyncTable`, ve yerel depo tüm CRUD işlemleri kullanın.

## <a name="performing-synchronization"></a>Eşitleme gerçekleştirme

Yerel depodan Azure Mobile Apps ile eşitlenen zaman örnek `SyncAsync` yöntemi çağrılır:

```csharp
public async Task SyncAsync()
{
  ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

  try
  {
    await this.client.SyncContext.PushAsync();

    // The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
    // Use a different query name for each unique query in your program.
    await this.todoTable.PullAsync("allTodoItems", this.todoTable.CreateQuery());
  }
  catch (MobileServicePushFailedException exc)
  {
    if (exc.PushResult != null)
    {
      syncErrors = exc.PushResult.Errors;
    }
  }

  // Simple error/conflict handling.
  if (syncErrors != null)
  {
    foreach (var error in syncErrors)
    {
      if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
      {
        // Update failed, revert to server's copy
        await error.CancelAndUpdateItemAsync(error.Result);
      }
      else
      {
        // Discard local change
        await error.CancelAndDiscardItemAsync();
      }

      Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
    }
  }
}
```

`IMobileServiceSyncTable.PushAsync` Yöntemi belirli bir tablo yerine eşitleme bağlamı üzerinde çalışır ve bu yana son zorlama tüm CUD değişiklikleri gönderir.

Çekme tarafından gerçekleştirilir `IMobileServiceSyncTable.PullAsync` tek bir tabloda yöntemi. İlk parametre olarak `PullAsync` yalnızca mobil aygıtta kullanılan bir sorgu adı bir yöntemdir. Azure Mobile istemci SDK'sı işlemlerinde adı sonuçları bir null olmayan sorgu sağlayarak bir *Artımlı eşitleme*, her seferinde bir çekme işlemi sonuçları, en son döndüğü `updatedAt` sonuçlarından zaman damgası, yerel depolanır Sistem tabloları. Sonraki çekme işlemleri sonra yalnızca kayıtları sonra zaman damgası alın. Alternatif olarak, *tam eşitleme* geçirerek sağlanabilir `null` sorgu adı hangi sonuçlanıyor her çekme işleminde alınan tüm kayıtları. Herhangi bir eşitleme işlemi alınan veriler yerel depolama alanına eklenir.

> [!NOTE]
> Bir çekme bekleyen yerel güncelleştirmeleri içeren bir tablo karşı yürütülürse, çekme push eşitleme bağlamda ilk yürütün. Bu zaten kuyruğa alınmış değişiklikleri ve yeni verileri Azure Mobile Apps örneğinden arasındaki çakışmaları en aza indirir.

`SyncAsync` Yöntemi de aynı kayıt hem yerel depodaki ve Azure Mobile Apps örneğinde değiştiğinde çakışmalarını işleme için temel bir uygulama içerir. Çakışma Yerel Depodaki ve Azure Mobile Apps örneğindeki veri güncelleştirildi olduğunda `SyncAsync` yöntemi verileri yerel depodaki Azure Mobile Apps örneğinde depolanan verileri güncelleştirir. Diğer bir çakışma meydana geldiğinde `SyncAsync` yöntemi yerel değişiklik atar. Bu senaryo Azure Mobile Apps örnekten silinmiş veriler için yerel bir değişiklik bulunduğu işler.

Bir üretim uygulamasında, geliştiriciler özel yazması gerektiğini `IMobileServiceSyncHandler` çakışma işleme uygulamasında, bunların kullanım durumu için uygundur. Daha fazla bilgi için bkz: [kullanım iyimser eşzamanlılık çakışması çözümlemesi için](https://azure.microsoft.com/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/#optimisticconcurrency) Azure portalındaki ve [derin dalın yönetilen istemci SDK çevrimdışı desteği](https://blogs.msdn.microsoft.com/azuremobile/2014/04/07/deep-dive-on-the-offline-support-in-the-managed-client-sdk/) MSDN bloglarında üzerinde.

## <a name="purging-data"></a>Veri temizleme

Tablolar Yerel Depodaki temizlenmiş veri `IMobileServiceSyncTable.PurgeAsync` yöntemi. Bu yöntem artık bir uygulama gerektiren eski verileri kaldırılıyor gibi senaryoları destekler. Örneğin, örnek uygulamayı yalnızca görüntüler `TodoItem` tam olmayan örnekleri. Bu nedenle, tamamlanmış öğeleri artık yerel olarak depolanan gerekir. Yerel depolama alanından tamamlanmış öğeleri temizleme gibi gerçekleştirilebilir:

```csharp
await todoTable.PurgeAsync(todoTable.Where(item => item.Done));
```

Çağrı `PurgeAsync` da anında iletme işlemi tetikler. Bu nedenle, yerel depodan kaldırılmadan önce yerel olarak tamamlandı olarak işaretlenmiş tüm öğeleri, Azure Mobile Apps örneğine'e gönderilir. Ancak, Azure Mobile Apps örneğiyle eşitleme bekleyen işlemler varsa, temizleme özel durum oluşturacak bir `InvalidOperationException` sürece `force` parametrenin ayarlanmış `true`. Alternatif bir strateji incelemektir `IMobileServiceSyncContext.PendingOperations` Özelliği Azure Mobile Apps örneğine gönderilen henüz ve özelliği sıfırsa temizleme yalnızca gerçekleştirmek işlemlerinin bekleyen sayısını döndürür.

> [!NOTE]
> Çağırma `PurgeAsync` ile `force` parametre kümesine `true` bekleyen tüm değişiklikleri kaybedeceksiniz.

## <a name="initiating-synchronization"></a>Eşitleme başlatılıyor

Örnek uygulamasında `SyncAsync` yöntemi çağrılır aracılığıyla `TodoList.OnAppearing` yöntemi:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  // Set syncItems to true to synchronize the data on startup when running in offline mode
  await RefreshItems(true, syncItems: true);
}
```

Bu, uygulama başlatıldığında Azure Mobile Apps örneğiyle eşitleme dener anlamına gelir.

Ayrıca, eşitleme iOS ve Android kullanarak veri listesi ve Windows platformları yenilemek için çekme kullanarak başlatılabilir **eşitleme** kullanıcı arabiriminde düğmesi. Daha fazla bilgi için bkz: [çekme yenilemeye](~/xamarin-forms/user-interface/listview/interactivity.md#Pull_to_Refresh).

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.Forms uygulaması çevrimdışı eşitleme işlevselliği ekleme açıklanmıştır. Çevrimdışı eşitleme sağlayan bir mobil uygulama, görüntüleme, ekleme veya verileri değiştirme ile etkileşim kullanıcılar bile bulunmadığı durumlarda bir ağ bağlantısı. Değişiklikler yerel bir veritabanında depolanır ve cihaz çevrimiçi olduktan sonra Azure Mobile Apps örneğiyle değişiklikleri eşitlenen.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoAzureAuthOfflineSync (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzureAuthOfflineSync/)
- [Bir Azure mobil uygulaması kullanma](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Mobile Apps kullanıcıların kimlik doğrulaması](~/xamarin-forms/data-cloud/authentication/azure.md)
- [Azure mobil istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [MobileServiceClient](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx)
