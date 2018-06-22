---
title: Intent Services in Xamarin.Android
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 80849213649707615f8bd8e941e1a51c6b54e76e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763348"
---
# <a name="intent-services-in-xamarinandroid"></a>Intent Services in Xamarin.Android

## <a name="intent-services-overview"></a>Hedefi Hizmetleri'ne Genel Bakış

Her ikisi de başlatıldı ve bağlı Hizmetleri performans kesintisiz tutmak için bir hizmetin iş zaman uyumsuz olarak gerçekleştirmek ihtiyaç duyduğu anlamına gelir ana iş parçacığı üzerinde çalıştırın. Bu sorunu çözmenin en basit yolu, biri ile bir _çalışan sıra işlemci düzeni_, tek bir iş parçacığı tarafından hizmet verilen bir kuyrukta yapılacak işleri nereye yerleştirilir. 

[ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/) Sınıfıdır `Service` bu deseni Android belirli uygulaması sağlayan sınıf. Kuyruk hizmeti için iş parçacığı başlatılırken çağrısı iş yönetecek ve çekme istekleri çalışan iş parçacığı üzerinde çalıştırılacak sıranın kapalı. Bir `IntentService` sessizce kendisini durduracak ve daha fazla iş kuyruğundaki olduğunda çalışan iş parçacığı kaldırın.
 
İş gönderildi sıraya oluşturarak bir `Intent` ve, geçirme `Intent` için `StartService` yöntemi.

Kesme veya durdurmak mümkün değildir `OnHandleIntent` yöntemi `IntentService` onu çalışırken. Bu tasarım nedeniyle bir `IntentService` durum bilgisiz tutulmalıdır &ndash; etkin bir bağlantı veya bir uygulamanın geri kalanından iletişimi güvenmemelisiniz. Bir `IntentService` statelessly iş isteklerini işlemek için tasarlanmıştır.

Sınıflara için iki gereksinim vardır `IntentService`:

1. Yeni türe (sınıflara tarafından oluşturulan `IntentService`) yalnızca geçersiz kılar `OnHandleIntent` yöntemi.
2. Yeni türünün Oluşturucusu istekleri işleyen çalışan iş parçacığı adı için kullanılan bir dize gerektirir. Bu iş parçacığı adı öncelikle uygulamayı hata ayıklama sırasında kullanılır.

Aşağıdaki kodda gösterildiği bir `IntentService` geçersiz kılınan uygulamasıyla `OnHandleIntent` yöntemi:

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }
    
    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

İş gönderildiği bir `IntentService` örneği tarafından bir `Intent` ve ardından çağırma [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/) bir parametre olarak bu amacıyla yöntemi. Amaç hizmete parametre olarak geçirilen `OnHandleIntent` yöntemi. Bu kod parçacığını bir hedefi için bir iş isteği gönderilirken bir örnektir: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` Değerleri, bu kod parçacığında gösterildiği gibi amaçtan ayıklamak:  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");
    
    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```


## <a name="related-links"></a>İlgili bağlantılar

- [IntentService](https://developer.xamarin.com/api/type/Android.App.IntentService/)
- [StartService](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)
