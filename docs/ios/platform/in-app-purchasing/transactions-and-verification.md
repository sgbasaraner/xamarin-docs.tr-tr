---
title: İşlemleri ve doğrulama
ms.prod: xamarin
ms.assetid: 84EDD2B9-3FAA-B3C7-F5E8-C1E5645B7C77
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: c8d86d0ce3119b3e104a65a170ab141484af44a7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="transactions-and-verification"></a>İşlemleri ve doğrulama

## <a name="restoring-past-transactions"></a>İşlemler geri yükleme

Uygulamanızı geri yüklenebilen ürün türünü destekliyorsa, bu satın alma işlemleri geri yapmalarına izin vermek için bazı kullanıcı arabirimi öğeleri eklemeniz gerekir.
Bu işlevsellik ürün için yeni cihaz ekleyemez veya ürün temiz silinmesinden sonra aynı cihaza geri yüklemek için bir müşteri sağlar veya kaldırarak ve uygulamayı yeniden yükleme. Aşağıdaki ürün türleri geri yüklenebilen şunlardır:

-  Olmayan tüketilebilir ürünleri
-  Otomatik yenilenebilir abonelikleri
-  Ücretsiz abonelikler


Geri yükleme işlemi kayıtlarını güncelleştirmelisiniz ürünlerinizi karşılamak için cihazda tutun. Müşteri, istediğiniz zaman, herhangi bir geri yüklemek seçebilirsiniz. Geri yükleme işlemi o kullanıcı için tüm önceki işlemlerin yeniden gönderir; uygulama kodu daha sonra bu bilgileri (örneğin, zaten varsa bu satın alma cihazda kayıt ve satın alma bir kayıt oluşturma değil, denetleme ve kullanıcı için ürün etkinleştirme) ile hangi eylemin belirlemeniz gerekir.

### <a name="implementing-restore"></a>Uygulama geri yükleme

Kullanıcı arabirimi **geri** düğmesi üzerinde RestoreCompletedTransactions tetikler aşağıdaki yöntemini çağırır `SKPaymentQueue`.

```csharp
public void Restore()
{
   // theObserver will be notified of when the restored transactions start arriving <- AppStore
   SKPaymentQueue.DefaultQueue.RestoreCompletedTransactions();
}
```

StoreKit geri yükleme isteği için Apple'nın sunucularından zaman uyumsuz olarak gönderir.   
   
   
   
 Çünkü `CustomPaymentObserver` kayıtlı bir işlem gözlemci, Apple'nın sunucularından yanıtladığında iletileri alacak. Bu kullanıcı herhangi bir zamanda (tüm cihazlarıyla) Bu uygulamada gerçekleştirdiği tüm işlemler yanıt içerir. Her bir işlem aracılığıyla kodu döngüler algılar geri yüklenen durumu ve çağrıları `UpdatedTransactions` yöntemi aşağıda gösterildiği gibi işlemek için:

```csharp
// called when the transaction status is updated
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
       case SKPaymentTransactionState.Purchased:
          theManager.CompleteTransaction(transaction);
           break;
       case SKPaymentTransactionState.Failed:
          theManager.FailedTransaction(transaction);
           break;
       case SKPaymentTransactionState.Restored:
           theManager.RestoreTransaction(transaction);
           break;
default:
           break;
       }
   }
}
```

Kullanıcı için geri yüklenebilen hiç ürün varsa `UpdatedTransactions` çağrılmaz.   
   
   
   
 Örnek belirtilen bir işlemde geri yüklemek için en basit olası kod bir satın alma ne zaman, dışında yer aldığı olarak aynı eylemleri gerçekleştirir `OriginalTransaction` özelliği ürün kimliği erişmek için kullanılır:

```csharp
public void RestoreTransaction (SKPaymentTransaction transaction)
{
   // Restored Transactions always have an 'original transaction' attached
   var productId = transaction.OriginalTransaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId); // it's as though it was purchased again
   FinishTransaction(transaction, true);
}
```

Daha karmaşık bir uygulama diğer denetleyebilirsiniz `transaction.OriginalTransaction` özgün tarih ve giriş sayısı gibi özellikleri. Bu bilgiler bazı ürün türleri (örneğin, abonelikler) için yararlı olacaktır.

#### <a name="restore-completion"></a>Tamamlama geri yükleme

`CustomPaymentObserver` (Başarıyla veya bir hata ile), geri yükleme işlemi tamamlandığında, StoreKit tarafından adlı iki ek yöntemleri sahip aşağıda gösterilmektedir:

```csharp
public override void PaymentQueueRestoreCompletedTransactionsFinished (SKPaymentQueue queue)
{
   Console.WriteLine(" ** RESTORE Finished ");
}
public override void RestoreCompletedTransactionsFailedWithError (SKPaymentQueue queue, NSError error)
{
   Console.WriteLine(" ** RESTORE FailedWithError " + error.LocalizedDescription);
}
```

Gerçek bir uygulamada bir ileti kullanıcının ya da diğer bazı işlevler uygulamayı seçebilir ancak örnekte nothing, bu yöntemleri yapın.

## <a name="securing-purchases"></a>Satın alma işlemleri güvenli hale getirme

Bu iki örnek belge kullanımı `NSUserDefaults` satın alma işlemleri izlemek için:   
   
 **Sarf** – basit bir kredi satın alma işlemleri 'Bakiye' olan `NSUserDefaults` her satın alma ile artırılır tamsayı değeri.   
   
 **Olmayan sarf** – her bir fotoğraf filtre satınalma bir anahtar-değer çifti olarak depolanan `NSUserDefaults`.

Kullanarak `NSUserDefaults` kod örneği basit korur, ancak kullanıcılar için teknik minded (ödeme mekanizmasını atlama) ayarlarını güncelleştirmek olası olabileceğinden çok güvenli bir çözüm sunmaz.   
   
Not: Gerçek uygulamalar kullanıcı oynama tabi olmayan içerik satın depolamak için güvenli bir mekanizma benimsemeyi. Bu, şifreleme ve/veya uzak sunucu kimlik doğrulaması gibi başka teknikler gerektirebilir.   
   
 Mekanizması, iOS, iTunes ve iCloud yerleşik yedekleme ve kurtarma özelliklerden yararlanmak için aynı zamanda tasarlanmalıdır. Bu, bir kullanıcı bir yedeği geri yükledikten sonra önceki aldıklarını hemen kullanılabilir olacağını garanti eder.   
   
   
 Güvenli kodlama Kılavuzu iOS özgü konusunda daha fazla yönerge için Apple'nın ' na bakın.

## <a name="receipt-verification-and-server-delivered-products"></a>Giriş doğrulama ve sunucu teslim ürünleri

Şu ana kadar bu belgedeki örneklerde, özellikleri veya yetenekleri zaten uygulamaya kodlanmış kilidini satın alma işlemleri yürütmek için doğrudan uygulama mağazasına sunucularıyla iletişim kurarken yalnızca uygulamanın oluşmuştur.   
   
   
   
 Apple bağımsız olarak bir satın alma bir parçası olarak dijital içerik teslim etmeden önce bir isteği doğrulamak yararlı olabilecek başka bir sunucu tarafından doğrulanması satın alma Alındılar vererek ek bir satın alma güvenlik düzeyini sağlar (dijital kitap gibi veya dergisi).   
   
   
   
 **Yerleşik ürünleri** – bu belgedeki örneklerde gibi uygulama ile birlikte gelen işlevi satın ürün bulunmaktadır. Bir uygulama içi satın alma işlevselliği erişmek kullanıcı sağlar.
Ürün, sabit kodlanmış Kimlikleridir.   
   
 **Ürünler sunucu teslim** – ürün yüklenmek üzere içerik başarılı bir işlem neden olana kadar uzak bir sunucuda depolanan indirilebilir içeriğini oluşur.
Örnekler, kitap veya dergi sorunları içerebilir. Ürün kimlikleri genellikle bir dış sunucudan (ürün içeriği de barındırıldığı) elde edilir. Uygulamalar başarısız içeriği yüklerseniz, bu kullanıcının kafa karıştırıcı olmadan yeniden denenen olabilir bir zaman bir işlem tamamlandı, kaydetme, sağlam bir şekilde uygulamalıdır.

### <a name="server-delivered-products"></a>Sunucu teslim ürünleri

Books ve dergi (veya hatta bir oyun düzeyi) satın alma işlemi sırasında uzak bir sunucudan yüklenmesine gerek gibi bazı ürün içeriğinin. Başka bir deyişle, depolamak ve onu satın sonra ürün içerik sağlamak için ek bir sunucu gereklidir.

#### <a name="getting-prices-for-server-delivered-products"></a>Fiyatlar ürünler için sunucu teslim alma

Ürünleri uzaktan teslim edildiğinden, aynı zamanda daha fazla ürün Zamanla (uygulama kodu güncelleştirmeden), daha fazla kitap veya yeni bir dergi sorunları ekleme gibi eklemek mümkündür. Uygulama bu haber ürünler bulmak ve kullanıcıya gösterilecek böylece ek sunucu depolamak ve bu bilgileri teslim gerekir.   
   
   
   
 [![](transactions-and-verification-images/image38.png "Fiyatlar ürünler için sunucu teslim alma")](transactions-and-verification-images/image38.png#lightbox)   
   
   
   
 1. Birden fazla yerde ürün bilgisi depolanan: sunucunuzda ve iTunes Bağlan. Ayrıca, her ürünün ilişkili içerik dosyalarını sahip olur. Bu dosyalar sonra başarılı bir satın alma teslim edilecek.   
   
 2. Kullanıcı bir ürünü satın istediğinde, uygulamanın hangi ürünleri kullanılabilir belirlemeniz gerekir. Bu bilgileri önbelleğe alınmış olabilir, ancak ana ürünlerin listesini depolandığı bir Uzak sunucudan teslim.   
   
 3. Sunucu uygulamanın ayrıştırmak ürün kimlikleri listesi döndürür.   
   
 4. Uygulama, fiyatları ve açıklamaları almak için StoreKit göndermek için bu ürün kimlikleri belirler.   
   
 5. StoreKit Apple'nın sunucularına ürün kimlikleri listesi gönderir.   
   
 6. İTunes sunucuları (açıklama ve geçerli fiyat) geçerli bir ürün bilgilerle yanıt verir.   
   
 7. Uygulamanın `SKProductsRequestDelegate` ürün bilgilerini görüntülemek için kullanıcıya geçirilir.

#### <a name="purchasing-server-delivered-products"></a>Sunucu teslim ürünleri satın alma

Uzak sunucu bir içerik isteği geçerli olduğunu doğrulama bazı şekilde gerektirdiğinden (IE. için Ücretli), okundu bilgisi boyunca kimlik doğrulaması için geçirilir. Uzak sunucu iTunes doğrulama için bu verileri iletir ve başarılı olursa, ürün içeriği uygulama yanıtı içerir.   
   
 [![](transactions-and-verification-images/image39.png "Sunucu teslim ürünleri satın alma")](transactions-and-verification-images/image39.png#lightbox)   
   
 1. Uygulama ekler bir `SKPayment` kuyruğa. Kullanıcı kendi Apple kimliği istenir ve ödeme onaylaması istenir gerekiyorsa.   
   
 2. StoreKit işleme için sunucu isteği gönderir.   
   
 3. İşlem tamamlandığında, sunucu işlem girişiyle yanıt verir.   
   
 4. `SKPaymentTransactionObserver` Bir alt giriş alır ve işler. Ürün bir sunucudan yüklenmesi gerektiğinden uygulama uzak sunucu ağ isteği başlatır.   
   
 5. Uzak sunucu içeriğe erişmeye yetkilendirilmiş olduğunuzu doğrulayabilmeniz indirme isteği tarafından giriş verileri vardır. Bu isteğe yanıt uygulamanın ağ istemcisi bekler.   
   
 6. Sunucu içerik için bir istek aldığında, giriş verilerini ayrıştırır ve alındığını doğrulamak için bir doğrudan iTunes sunucuları için geçerli bir işlem taleptir gönderir. Sunucu, isteği üretim veya korumalı alan URL'sine gönderilip gönderilmeyeceğini belirlemek için bazı mantığı kullanmanız gerekir. Apple öneren her zaman üretim URL'yi kullanarak ve korumalı alanda durumunda geçiş durumu: 21007 (korumalı alan giriş üretim sunucusuna gönderilir) – başvurmak için [Teknik Not 2259 SSS #16](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html).   
   
 7. iTunes alınmasını denetleyin ve geçerli ise sıfır durumunu döndürür.   
   
 8. Sunucu için iTunes yanıt bekler. Geçerli bir yanıt alırsa, kod uygulama yanıta dahil edilecek ilişkili ürün içerik dosyasını bulun.   
   
 9. Uygulama alır ve cihazın dosya sistemi için ürün içeriği kaydetme yanıt ayrıştırır.   
   
 10. Uygulama ürünü etkinleştirir ve StoreKit'ın çağıran `FinishTransaction`. Ardından, uygulamayı isteğe bağlı olarak (örneğin, satın alınan kitap veya dergi sorun gösterilecek ilk sayfa) satın alınan içerik gösteriyor olabilir.

Çok büyük ürün içerik dosyaları için alternatif bir uygulama, böylece işlem hızlı bir şekilde tamamlanabilmesi için işlem giriş #9. adımda yalnızca depolama ve kullanıcının gerçek ürün içerik indirmek bir kullanıcı arabirimi sağlayan ilgili olabilir Bazı daha sonra. Sonraki indirme isteği gerekli ürün içerik dosyaya erişmek için saklı giriş yeniden gönderebilirsiniz.

### <a name="writing-server-side-receipt-verification-code"></a>Sunucu tarafı giriş doğrulama kodu yazma

Sunucu tarafı kodu bir giriş doğrulama basit HTTP POST isteği / #8'de iş akışı diyagramı aracılığıyla #5 adımlarını kapsayan yanıt ile yapılabilir.   
   
   
   
 Ayıklama `SKPaymentTansaction.TransactionReceipt` uygulama özelliği. Bu doğrulama (#5. adım) iTunes gönderilmesi gereken verilerdir.

Base64-hareket giriş verilerini (ya da #5 veya #6. adım) kodlayın.

Bu gibi basit bir JSON yükü oluşturun:

```csharp
{
   "receipt-data" : "(base-64 encoded receipt here)"
}
```

HTTP POST için JSON [ https://buy.itunes.apple.com/verifyReceipt ](https://buy.itunes.apple.com/verifyReceipt) üretim için veya [ https://sandbox.itunes.apple.com/verifyReceipt ](https://sandbox.itunes.apple.com/verifyReceipt) test etmek için.   
   
 JSON yanıt aşağıdaki anahtarlarını içerir:

```csharp
{
   "status" : 0,
   "receipt" : { (receipt repeated here) }
}
```

Geçerli bir giriş sıfır durumunu gösterir. Satın alınan ürünün içeriği karşılamak üzere sunucunuzu geçebilirsiniz. Giriş anahtarı aynı özellikleri içeren bir JSON sözlük içeren `SKPaymentTransaction` sunucu kodu product_id ve satın alma miktarı gibi bilgileri almak için bu sözlük sorgulayabilir böylece uygulama tarafından alınan nesne.

Apple'nın bkz [doğrulama deposu Alındılar](https://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/StoreKitGuide/VerifyingStoreReceipts/VerifyingStoreReceipts.html#//apple_ref/doc/uid/TP40008267-CH104-SW1) ek bilgi için.

#### <a name="using-aspnet"></a>ASP.NET kullanma

C# geliştiricileri için adlı yararlı bir açık kaynak projesi yok [APNS-Sharp](https://github.com/Redth/APNS-Sharp) ASP.NET çalışır giriş doğrulama kodu içerir. Özellikle, `Receipt.cs` ve `ReceiptVerification.cs` dosyalar [ `Jdsoft.Apple.AppStore` ](https://github.com/Redth/APNS-Sharp/tree/master/JdSoft.Apple.AppStore) kolayca Server-Delivered ürünleri iş akışı diyagramı #6'dan #8 adımları uygulamak için bir .NET Web sitesi için dizin eklenebilir.   
   
   
   
 İTunes sunucuları ile iletişim C# dilinde işlemek kolay JSON, kullanır.   
   
   
   
 Apple'nın uygulama içi satın almaya yönlendirin [giriş doğrulama](https://developer.apple.com/library/ios/#releasenotes/StoreKit/IAP_ReceiptValidation/_index.html) belgelerine yönelik bir giriş geçerli olduğunu doğrulamak ayrıntılı bilgi.

### <a name="3rd-party-receipt-verification"></a>3 parti giriş doğrulama

Giriş Doğrulaması (ve başka şeyler), kendi sunucu oluşturma yerine kullanabileceğiniz platformları teklif şirketler bulunmaktadır. Xamarin hizmetlerin desteklemez; Bunlar yalnızca burada başvurusunu belirtilen.

#### <a name="urban-airship"></a>Kentsel Airship

Kentsel Airship bir giriş doğrulama ve anında iletme bildirimleri de dahil olmak üzere iOS uygulamaları için farklı arka uç hizmetlerini sağlar.   
   
 [http://urbanairship.com/products/in-app-purchase/](http://urbanairship.com/products/in-app-purchase/)



