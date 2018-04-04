---
title: Kaynaklarda ürünleri satın alma
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5c2c84c044ff41cced2c97e414502faff45341ec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="purchasing-consumable-products"></a>Kaynaklarda ürünleri satın alma

Kaynaklarda ürün basit 'geri' bir gereksinim olduğundan uygulamak değil. Bunlar, oyun para birimi veya tek kullanımlık bir işlevsellik gibi ürünler için kullanışlıdır. Kullanıcıların tüketilebilir ürünleri üzerinden-ve-üzerinden yeniden yeniden satın alabilirsiniz.

## <a name="built-in-product-delivery"></a>Yerleşik ürün teslim

Bu belge ile birlikte gelen örnek kod, yerleşik ürünleri göstermektedir – ürün kimlikleri bunlar sıkı şekilde 'özelliği, sonra ödeme kilidini açarak' koduna bağlı kodlanmış uygulamaya olduğu. Satın alma işlemi şu şekilde canlandırılabilir:   
   
[![Satın alma işlemi Görselleştirme](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 Temel iş akışı şöyledir:   
   
   
   
 1. Uygulama ekler bir `SKPayment` kuyruğa. Kullanıcı kendi Apple kimliği istenir ve ödeme onaylaması istenir gerekiyorsa.   
   
 2. StoreKit işleme için sunucu isteği gönderir.   
   
 3. İşlem tamamlandığında, sunucu işlem girişiyle yanıt verir.   
   
 4. `SKPaymentTransactionObserver` Bir alt giriş alır ve işler.   
   
 5. Uygulama ürün sağlar (güncelleştirerek `NSUserDefaults` veya başka bir mekanizma) ve ardından StoreKit'ın çağırır `FinishTransaction`.

İş akışı – başka bir tür *Server-Delivered ürünleri* – bu belgenin sonraki bölümlerinde ele alınan (bölümüne bakın *giriş doğrulama ve Server-Delivered ürünleri*).

## <a name="consumable-products-example"></a>Kaynaklarda ürünleri örneği

[InAppPurchaseSample kod](https://developer.xamarin.com/samples/monotouch/StoreKit/) adlı bir proje içeren *sarf* basic 'oyun para birimi ("krediler monkey" olarak adlandırılır)' uygular. Örnek, birçok "monkey krediler" istedikleri gibi – gerçek bir uygulamada ayrıca olurdu gibi bunları harcama bazı şekilde satın almak izin vermek için bu iki uygulama içi satın alma ürün uygulamak gösterilmiştir!   
   
   
   
 Uygulama bu ekran görüntülerinde gösterilen – kullanıcının dengelemek için daha fazla "monkey krediler" her satın alma ekler:   
   
   
   
 [![Her satın alma daha fazla monkey krediler kullanıcılar dengelemek için ekler.](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 StoreKit ve uygulama mağazası özel sınıflar arasındaki etkileşimler şöyle görünür:   
   
   
   
 [![StoreKit ve uygulama mağazası özel sınıflar arasındaki etkileşimler](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController yöntemleri

Özellikleri ve yöntemleri ürün bilgisi almak için gereken ek olarak, görünüm denetleyicisini satın alma ile ilgili bildirimler için dinlemek ek bildirim gözlemcilerin gerektirir. Yalnızca bunlar `NSObjects` , kaydedilecek ve kaldırılabileceği `ViewWillAppear` ve `ViewWillDisappear` sırasıyla.

```csharp
NSObject succeededObserver, failedObserver;
```

Oluşturucusu ayrıca oluşturacak `SKProductsRequestDelegate` bir alt kümesi ( `InAppPurchaseManager`), sırayla oluşturur ve kaydettirir `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 Kullanıcı bir şey, satın örnek uygulamadan aşağıdaki kodda gösterildiği gibi istediğinde Düğme basma işlemek için bir uygulama içi satın alma işlemi işlenirken ilk bölümünü oluşturur:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 Kullanıcı arabirimini ikinci bölümü bildirimini işleme işlem, bu durumda görüntülenen bakiyenin güncelleştirerek başarılı olduğunu:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

Bazı nedenlerden dolayı bir işlem iptal edilirse kullanıcı arabiriminin son bölümü bir ileti görüntülüyor. Örnek kodda bir ileti sadece çıkış penceresine yazılır:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Görünüm denetleyicisini bu yöntemlere ek olarak, bir tüketilebilir ürün satın alma işlemi ayrıca kod gerektiriyorsa `SKProductsRequestDelegate` ve `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>InAppPurchaseManager Methods

Örnek kod uygulayan bir dizi satın InAppPurchaseManager sınıfında ilgili yöntemleri de dahil olmak üzere `PurchaseProduct` oluşturan yöntemi bir `SKPayment` örneği ve işleme için kuyruğa ekler:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Ödeme sıraya ekleme zaman uyumsuz bir işlemdir. StoreKit işlem işler ve Apple'nın sunucularına gönderir sırada uygulama denetim yeniden kazanabilmesi. Bu noktada, iOS kullanıcı uygulama mağazasında günlüğe kaydedilir ve her bir Apple kimliği ve parola için gerekli sıfırlanırsa doğrular değil.   
   
   
   
 Kullanıcı başarıyla varsayılarak App Store ile doğrular ve bir işleme için kabul `SKPaymentTransactionObserver` StoreKit'ın yanıtını alma ve işlem karşılamak ve onu sonlandırmak için aşağıdaki yöntemi çağırın.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

Son adım, başarılı bir şekilde hareket çağırarak karşılamış olduğunu StoreKit bildir emin olmaktır `FinishTransaction`:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

Ürün teslim sonra `SKPaymentQueue.DefaultQueue.FinishTransaction` ödeme sıradan işlem kaldırmak için çağrılmalıdır.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>SKPaymentTransactionObserver (CustomPaymentObserver) yöntemleri

StoreKit çağrıları `UpdatedTransactions` Apple'nın sunucularından bir yanıt alır ve bir dizi geçirir yöntemi `SKPaymentTransaction` kodunuz incelemek için nesne. Yöntemi, her işlem üzerinden döngüye girer ve işlem durumuna (aşağıda gösterildiği gibi) göre farklı bir işlevi gerçekleştirir:

```csharp
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
           default:
               break;
       }
   }
}
```

`CompleteTransaction` Yöntemi kapsanan daha önce bu bölümdeki – satın alma ayrıntıları kaydeder `NSUserDefaults`, StoreKit işlemle sonlandırır ve son olarak güncelleştirmek için kullanıcı Arabirimi bildirir.

### <a name="purchasing-multiple-products"></a>Birden çok ürün satın alma

Birden çok ürün satın almak için uygulamanızda mantıklıdır kullanırsanız `SKMutablePayment` sınıfı ve Miktar alanını ayarlayın:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Tamamlanan işlem işleme kodu aynı zamanda doğru satın karşılamak üzere miktar özelliği sorgu gerekir:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Kullanıcı birden çok miktarda satın aldığında StoreKit onay uyarı miktarı, birim fiyat ve bunlar ücret, toplam fiyat aşağıdaki ekran görüntüsünde gösterildiği gibi yansıtır:

[![Satın alma onayı](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>İşleme ağ kesintileri

Uygulama içi satın almalara Apple'nın sunucularıyla iletişim kurmak StoreKit için çalışan bir ağ bağlantısı gerektirir. Bir ağ bağlantısı kullanılabilir durumda değilse, ardından uygulama içi satın alma kullanılamaz.

### <a name="product-requests"></a>Ürün istekleri

Ağ yapma çalışırken kullanılamıyorsa bir `SKProductRequest`, `RequestFailed` yöntemi `SKProductsRequestDelegate` alt ( `InAppPurchaseManager`) aşağıda gösterildiği gibi çağrılır:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

ViewController ardından bildirim için dinler ve satın alma düğmeleri bir ileti görüntülenir:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Bir ağ bağlantısı mobil cihazlarda geçici olabileceğinden, uygulamaları SystemConfiguration framework kullanarak ağ durumunu izlemek isterseniz ve bir ağ bağlantısı kullanılabilir olmadığında yeniden deneyin. Apple için bakın veya bunu kullanır.

### <a name="purchase-transactions"></a>Satın alma işlemleri

StoreKit ödeme sıranın depolar ve satın alma işlemi sırasında ağ başarısız olduğunda bir ağ kesintisi etkisini bağlı olarak farklılık gösterir iletme satın alma Mümkünse, ister.   
   
   
   
 Bir işlem sırasında bir hata oluşursa `SKPaymentTransactionObserver` alt ( `CustomPaymentObserver`) sahip olacaktır `UpdatedTransactions` yöntemi çağrılır ve `SKPaymentTransaction` sınıfı başarısız durumunda olacaktır.

```csharp
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
           default:
               break;
       }
   }
}
```

`FailedTransaction` Yöntemi algılar hata kullanıcı-iptal nedeniyle olup aşağıda gösterildiği gibi:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

Bir işlem başarısız olsa bile, `FinishTransaction` yöntemi çağrılır, ödeme sıradan işlem kaldırmak için:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

ViewController bir ileti görüntüleyebilmesi örnek kodu daha sonra bir bildirim gönderir. Uygulamaları gösterme ek bir kullanıcı işlemi iptal ederseniz iletisi. Oluşabilir ve diğer hata kodları şunlardır:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>İşleme kısıtlamaları

**Ayarlar > Genel > kısıtlamaları** iOS özelliği kullanıcıların cihazlarını belirli özelliklerini kilitlemenizi sağlar.   
   
   
   
 Kullanıcı aracılığıyla uygulama içi satın alımlar yapmasına izin verilip verilmediğini sorgu `SKPaymentQueue.CanMakePayments` yöntemi. False değeri döndürülürse kullanıcı uygulama içi satın alma erişemez. Satın alma denenirse StoreKit bir hata iletisi kullanıcıya otomatik olarak görüntüler. Bu değer denetleyerek uygulamanızı yerine satın alma düğmeleri gizleyebilir veya kullanıcının yardımcı olmak için diğer bazı işlemler yapması.   
   
   
   
 İçinde `InAppPurchaseManager.cs` dosya `CanMakePayments` yöntemi şöyle StoreKit işlevi sarmalar:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Bu yöntem sınamak için kullanın **kısıtlamaları** özelliğini devre dışı bırakmak için iOS **uygulama içi satın almalara**:   
   
   
   
 [![Uygulama içi satın almalara devre dışı bırakmak için iOS kısıtlamaları özelliğini kullanın](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Bu örnek koddan `ConsumableViewController` için tepki verdiğini `CanMakePayments` false değerinin döndürülmesi görüntüleyerek **AppStore devre dışı** devre dışı düğmelerin üzerindeki metin.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

Uygulama gibi durumlarda arar **uygulama içi satın almalara** özelliktir kısıtlı – satın alma düğmeleri devre dışı bırakıldı.   
   
   
   
 [![Düğmeleri devre dışı satın alma In uygulama özelliği satın alma işlemleri sınırlı olduğunda uygulama şöyle görünür](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Ürün bilgisi hala olabilir ne zaman istenen `CanMakePayments` uygulama hala alabilir ve fiyatları görüntülemek için yanlış. Biz kaldırdığını yani `CanMakePayments` bir satın alma çalışırken kullanıcı bir ileti görür ancak onay satın alma düğmeleri hala kodundan etkin, **uygulama içi satın almalara izin verilmez** (StoreKit tarafından oluşturulan Ödeme sıranın erişildiğinde):   
   
   
   
 [![Uygulama içi satın almalara izin verilmiyor](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Gerçek uygulamalar düğmeleri tamamen gizleme ve belki de otomatik olarak StoreKit gösteren uyarı daha ayrıntılı bir ileti sunan gibi kısıtlama, işleme için farklı bir yaklaşım sürebilir.

