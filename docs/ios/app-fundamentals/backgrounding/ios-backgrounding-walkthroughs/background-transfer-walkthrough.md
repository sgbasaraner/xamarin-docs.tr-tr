---
title: Arka plan aktarım ve Xamarin.iOS NSURLSession
description: Bu belge, arka plan aktarım ve NSUrlSession büyük bir görüntü yükleme devre dışı kazandırın ve uygulama arka planda yerleştirildiğinde, yükleme devam etmek için nasıl kullanılacağını gösteren bir kılavuz sağlar.
ms.prod: xamarin
ms.assetid: 6960E025-3D5C-457A-B893-25B734F8626D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 08a0ba1337c0d28d1f0d60d04394ccaf4a9ccfc7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783745"
---
# <a name="background-transfer-and-nsurlsession-in-xamarinios"></a>Arka plan aktarım ve Xamarin.iOS NSURLSession

Arka plan yapılandırarak arka plan aktarım başlatılan `NSURLSession` ve karşıya yükleme veya indirme görevleri nedeniyle. Uygulama backgrounded, askıya alındı veya sonlandırıldı sırada görevleri tamamlamak, iOS uygulamanın uygulamanın içinde tamamlama işleyici çağırarak bildirir *AppDelegate*. Aşağıdaki diyagramda bu eylemde gösterir:

 [![](background-transfer-walkthrough-images/transfer.png "Arka plan aktarım arka plan NSURLSession yapılandırarak başlatılır ve nedeniyle karşıya yükleme veya görevleri indirme")](background-transfer-walkthrough-images/transfer.png#lightbox)

Bu nasıl kodda göründüğünü görelim.

## <a name="configuring-a-background-session"></a>Bir arka plan oturum yapılandırma

Bir arka plan oturumu yapmak için yeni bir oluşturma `NSUrlSession` ve kullanarak yapılandırma bir `NSUrlSessionConfiguration` nesnesi.

Yapılandırma nesnesi oturum neler yapabileceğinizi belirler ve görevleri türlerini çalıştırılabilir.
Kullanılarak yapılandırılan oturumları `CreateBackgroundSessionConfiguration` yöntemi ayrı bir işlemde çalışacak ve veri ve pil ömrünü korumak için isteğe bağlı (WiFi) aktarımı gerçekleştirmek.
Aşağıdaki kod örneği, arka plan aktarım oturumu kullanarak uygun Kurulum gösterir `CreateBackgroundSessionConfiguration` yöntemi ve benzersiz bir dize tanımlayıcısı:

```csharp
public partial class SimpleBackgroundTransferViewController : UIViewController
{
  NSUrlSession session = null;

  NSUrlSessionConfiguration configuration =
      NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.SimpleBackgroundTransfer.BackgroundSession");
  session = NSUrlSession.FromConfiguration
      (configuration, (NSUrlSessionDelegate) new MySessionDelegate(), new NSOperationQueue());

}
```

Bir yapılandırma nesnesi dışında bir oturum oturum temsilci ve bir sıraya gerektirir.
Sıranın görevler tamamlanır sırasını belirler. Oturum temsilci aktarım işlemi ve tanıtıcıları kimlik doğrulaması, önbelleğe alma ve diğer oturum ile ilgili sorunları chaperones.

## <a name="working-with-tasks-and-delegates"></a>Görevleri ve temsilciler ile çalışma

Şimdi bir arka plan oturumu yapılandırmış olduğunuz, aktarım işlemek için görevleri tetiklersiniz. Biz kullanarak bu görevleri izlemek bir `NSUrlSessionDelegate` örneği oturum temsilci çağrılır. Oturum temsilci bir arka plan Sonlandırılan veya askıya alınmış uygulamada tanıtıcı kimlik doğrulaması, hatalar veya aktarımı tamamlama Uyandırma için sorumludur.

Bir `NSUrlSessionDelegate` aktarımı durumunu denetlemek için aşağıdaki temel yöntemleri sağlar:

-  *DidFinishEventsForBackgroundSession* -bu yöntem tüm görevleri tamamladıktan ve aktarımı tamamlanır uğradığında çağrılır.
-  *DidReceiveChallenge* - yetkilendirme gerekli olduğunda, isteği kimlik bilgilerine denir.
-  *DidBecomeInvalidWithError* -çağrılan IF `NSURLSession` geçersiz hale gelir.


Arka plan oturumları çalışan görev türlerine bağlı olarak daha özelleştirilmiş temsilciler gerektirir. Arka plan oturumları iki tür görev sınırlıdır:

-  *Görevleri karşıya* -görevleri türü `NSUrlSessionUploadTask` kullanmak `NSUrlSessionTaskDelegate` , devralan `NSUrlSessionDelegate` . Bu temsilci izlemek için ek yöntemleri ilerleme durumunu, tanıtıcı HTTP yeniden yönlendirmesi ve daha fazlasını karşıya sağlar.
-  *Görevler karşıdan* -görevleri türü `NSUrlSessionDownloadTask` kullanmak `NSUrlSessionDownloadDelegate` , devralan `NSUrlSessionTaskDelegate` . Bu temsilci, yükleme ilerleme durumunu izlemek ve indirme görevinin sürdürüldü veya tamamlandı belirlemek için yükleme özgü yöntemleri yanı sıra, görevleri tüm yöntemleri için karşıya yükleme sağlar.


Aşağıdaki kod, görüntü bir URL'den indirmek için kullanılan bir görev tanımlar. Biz çağırarak görevini Kapat kazandırın `CreateDownloadTask` bizim arka plan oturumda ve URL isteğinde geçirme:

```csharp
const string DownloadURLString = "http://cdn1.xamarin.com/webimages/images/xamarin.png";
public NSUrlSessionDownloadTask downloadTask;

NSUrl downloadURL = NSUrl.FromString (DownloadURLString);
NSUrlRequest request = NSUrlRequest.FromUrl (downloadURL);
downloadTask = session.CreateDownloadTask (request);
```

Ardından, tüm yükleme görevleri bu oturumda izlemek için yeni bir oturum indirme temsilci oluşturacağız:

```csharp
public class MySessionDelegate : NSUrlSessionDownloadDelegate
{
  public override void DidWriteData (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, long bytesWritten, long totalBytesWritten, long totalBytesExpectedToWrite)
  {
    Console.WriteLine (string.Format ("DownloadTask: {0}  progress: {1}", downloadTask, progress));
    InvokeOnMainThread( () => {
      // update UI with progress bar, if desired
    });
  }
  ...
}
```

Biz indirme görevinin ilerleme bulmak istiyorsanız, biz kılabilirsiniz `DidWriteData` UI güncelleştirme ilerleme durumunu izlemek ve hatta yöntemi. Uygulamanın ön planda veya bir kullanıcı, uygulamayı bir sonraki açışınızda bekleniyor kullanıcı Arabirimi güncelleştirmeleri hemen gösterilmesi gerekir.

Oturum temsilci API görevleri ile etkileşmek için geniş bir araç sağlar. Oturum tam listesi için temsilci yöntemlerini, başvurmak `NSUrlSessionDelegate` API belgeleri.

> [!IMPORTANT]
> Arka plan oturumları UI güncelleştirmek için tüm çağrıları açıkça UI iş parçacığında çağırarak çalıştırılması gereken şekilde bir arka plan iş parçacığı üzerinde başlatılacağı `InvokeOnMainThread` uygulama sonlandırma iOS önlemek için. 


## <a name="handling-transfer-completion"></a>İşleme aktarımı tamamlama

Oturumla ilişkili tüm görevleri tamamladığınızda, bilmeniz izin vermek amacıyla son adımdır ve yeni içerik tanıtıcı.

İçinde *AppDelegate*, abone `HandleEventsForBackgroundUrl` olay. Uygulama arka girer ve bir aktarım oturumu çalışıyorsa, bu yöntem çağrılır ve sistem bize tamamlama işleyici geçirir:

```csharp
public System.Action backgroundSessionCompletionHandler;

public override void HandleEventsForBackgroundUrl (UIApplication application, string sessionIdentifier, System.Action completionHandler)
{
  this.backgroundSessionCompletionHandler = completionHandler;
}
```

Tamamlama işleyici uygulamamız tamamlanınca bilmeniz iOS sağlamak için kullanacağız işleniyor.

Bir oturum bir aktarımı işlemek için bazı görevler oluşturabilir geri çağırma. Son görevi tamamlandığında bir askıya alındı veya işten çıkarılan arka plan içine yeniden başlatılan uygulamasıdır. Ardından, uygulamanın yeniden bağlandığı `NSURLSession` benzersiz bir oturum tanımlayıcısı ve çağrıları kullanarak `DidFinishEventsForBackgroundSession` oturum temsilci üzerinde. Bu yöntem yeni içerik aktarımı sonuçlarını yansıtacak şekilde UI güncelleştirme dahil olmak üzere, işlemek için uygulamanın fırsattır:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  // Handle new information, update UI, etc.
}
```

İşleme yeni içerik bitmek sonra uygulamayı bir anlık görüntüsünü ve uyku moduna geri dönmek güvenli olduğunu bilmesini sistem izin vermek için tamamlama işleyici diyoruz:

```csharp
public override void DidFinishEventsForBackgroundSession (NSUrlSession session) {
  var appDelegate = UIApplication.SharedApplication.Delegate as AppDelegate;

  // Handle new information, update UI, etc.

  // call completion handler when you're done
  if (appDelegate.backgroundSessionCompletionHandler != null) {
    NSAction handler = appDelegate.backgroundSessionCompletionHandler;
    appDelegate.backgroundSessionCompletionHandler = null;
    handler.Invoke ();
  }
}
```

Bu kılavuzda, iOS 7 arka plan Aktarım Hizmeti uygulamak için temel adımlar ele.



## <a name="related-links"></a>İlgili bağlantılar

- [Basit arka plan Aktarım (örnek)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
