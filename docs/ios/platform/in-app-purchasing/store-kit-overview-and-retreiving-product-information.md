---
title: StoreKit genel bakış ve Xamarin.iOS ürün bilgilerini alma
description: Bu belge StoreKit genel bir bakış sağlar. StoreKit etkileşimleri sınama, ürünleri satış için görüntüleme, geçersiz ürünleri işleme ve yerelleştirilmiş fiyatlar görüntüleme StoreKit ile kullanılan sınıflar açıklanmaktadır.
ms.prod: xamarin
ms.assetid: FC21192E-6325-4389-C060-E92DBB5EBD87
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 964b97e82db8e79cb32598d0c955fac3ab122314
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787230"
---
# <a name="storekit-overview-and-retrieving-product-info-in-xamarinios"></a>StoreKit genel bakış ve Xamarin.iOS ürün bilgilerini alma

Bir uygulama içi satın alma için kullanıcı arabirimi, aşağıdaki ekran gösterilir.
Herhangi bir işlem gerçekleşmeden önce uygulamayı ürünün fiyatını ve açıklamasını görüntülemek için almanız gerekir. Ardından kullanıcı bastığında **satın**, uygulama Apple kimliği oturum açma ve onay iletişim kutusu yöneten StoreKit isteğinde bulunur. Uygulama kodu StoreKit bildirir işlem sonra başarılı olduğu varsayılarak, işlem sonucu depolamak ve kullanıcı kendi satın alma erişim sağlamak gerekir.   

   
 [![](store-kit-overview-and-retreiving-product-information-images/image14.png "İşlem sonucu depolamak ve kullanıcı kendi satın alma erişim sağlamak gerekir uygulama kodu StoreKit bildirir")](store-kit-overview-and-retreiving-product-information-images/image14.png#lightbox)

## <a name="classes"></a>Sınıflar

Uygulama içi satın almalara uygulama StoreKit framework aşağıdaki sınıflardan gerektirir:   
   
 **SKProductsRequest** – StoreKit (uygulama mağazası) satmak için onaylanan ürünleri için bir istek. Ürün kimlikleri sayısı ile yapılandırılabilir.

-   **SKProductsRequestDelegate** – ürün istekleri ve yanıtları işlemek için yöntemleri bildirir. 
-   **SKProductsResponse** – StoreKit (uygulama mağazası) geri temsilciye gönderilir. Kimlikleri istekle birlikte gönderilen ürün eşleşen SKProducts içerir. 
-   **SKProduct** – bir ürün (yapılandırdığınız iTunes Bağlan) StoreKit öğesinden alınan. Ürün Kimliği, başlık, açıklama ve fiyat gibi ürün hakkında bilgiler içerir. 
-   **SKPayment** – bir ürün kimliği ile oluşturulan ve satın alma gerçekleştirmek için ödeme kuyruğuna eklendi. 
-   **SKPaymentQueue** – Apple'a gönderilmek üzere sıraya alınan ödeme istekleri. Bildirimleri işlenmekte olan her bir ödeme sonucu olarak tetiklenir. 
-   **SKPaymentTransaction** – Tamamlanan işlem (App Store tarafından işlenen ve geri uygulamanıza StoreKit üzerinden gönderilen bir satın alma isteği) temsil eder. İşlem, satın alınan, geri yüklenen veya başarısız olabilir. 
-   **SKPaymentTransactionObserver** – StoreKit ödeme sıranın tarafından oluşturulan olaylara yanıt özel alt sınıf. 
-   **StoreKit işlemlerini zaman uyumsuz** – sonra bir SKProductRequest başlatıldığında veya bir SKPayment denetim kodunuzu döndürülen kuyruğa eklenir. Apple'nın sunucularından veri aldığında StoreKit SKProductsRequestDelegate veya SKPaymentTransactionObserver alt üzerinde yöntemleri çağırır. 


Aşağıdaki diyagramda (soyut sınıflar, uygulamanızda uygulanmalı) çeşitli StoreKit sınıfları arasındaki ilişkiler gösterilmektedir:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image15.png "Çeşitli StoreKit sınıflarının soyut sınıflar arasındaki ilişkileri uygulamada uygulanmalı")](store-kit-overview-and-retreiving-product-information-images/image15.png#lightbox)   
   
   
   
 Bu sınıfları, bu belgenin ilerleyen bölümlerinde daha ayrıntılı açıklanmıştır.

## <a name="testing"></a>Sınama

StoreKit işlemlerinin çoğu, test etmek için gerçek bir aygıt gerektirir. (Örn. ürün bilgileri alınıyor Fiyat &amp; açıklama) simulator ancak satın alma çalışır ve geri yükleme işlemleri, bir hata döndürecektir (FailedTransaction kodu gibi 5002 = bilinmeyen bir hata oluştu).

Not: İOS Simulator'da StoreKit çalışmaz. Uygulamanızın ödeme sıraya almaya çalışırsa, uygulamanızı iOS Simulator'da çalıştırırken StoreKit bir uyarı kaydeder. Mağaza sınama gerçek cihazlarda yapılması gerekir.   
   
   
   
 Önemli: test hesabınız ayarları uygulamada oturum yok. Oturum ayarları uygulamaya var olan tüm Apple ID hesabı dışında kullanabilirsiniz ve ardından sorulması için beklemeniz gerekir *, uygulama içi satın alma dizisi içindeki* test Apple kimliğini kullanarak oturum açmak için   
   
   
   

Oturum sınama hesabı gerçek depoya açmak çalışırsanız, bu otomatik olarak gerçek bir Apple kimliği dönüştürülür. Bu hesap artık test etmek için kullanışlı olur.

StoreKit kodu test etmek için test deposuna bağlı (iTunes Bağlan oluşturulan) bir özel test hesabıyla normal iTunes test hesabınız ve oturum açma, oturum kapatma gerekir. Geçerli hesap ziyaret dışında imzalamak için **ayarlar > iTunes ve uygulama mağazası** aşağıda gösterildiği gibi:

 [![](store-kit-overview-and-retreiving-product-information-images/image16.png "Geçerli hesap ziyaret ayarları iTunes oturumunu kapatın ve uygulama Mağazası")](store-kit-overview-and-retreiving-product-information-images/image16.png#lightbox)
 
bir test hesabı ile oturum *StoreKit tarafından uygulamanızda istendiğinde*:



Oluşturmak için test kullanıcılar iTunes Bağlan tıklayın **kullanıcılar ve roller** ana sayfada.

 [![](store-kit-overview-and-retreiving-product-information-images/image17.png "İTunes test kullanıcılarını oluşturmak için Bağlan'ı tıklatın kullanıcılar ve roller ana sayfasında")](store-kit-overview-and-retreiving-product-information-images/image17.png#lightbox)

Seçin **Sandbox sınayıcılar**

 [![](store-kit-overview-and-retreiving-product-information-images/image18.png "Korumalı alan sınayıcılar seçme")](store-kit-overview-and-retreiving-product-information-images/image18.png#lightbox)

Varolan kullanıcılar listesi görüntülenir. Yeni bir kullanıcı eklemek veya var olan kaydını silin. Portal desteklemez (şu anda) görüntüle veya Düzenle (özellikle atadığınız parola) oluşturulan her bir test kullanıcı iyi kaydını tutmanız önerilir böylece kullanıcılar, test varolan sağlar. Test kullanıcısı sildikten sonra e-posta adresi için başka bir test hesap yeniden kullanılamaz.  
   
 [![](store-kit-overview-and-retreiving-product-information-images/image19.png "Varolan kullanıcılar listesi görüntülenir")](store-kit-overview-and-retreiving-product-information-images/image19.png#lightbox)   
   
 Yeni test kullanıcıları (örneğin, adı, parola, gizli soru ve yanıt) gerçek bir Apple kimliği benzer özelliklere sahip. Buraya girilen tüm ayrıntılar kaydını tutun. **Seçin iTunes mağazası** alan para birimini belirler ve uygulama içi satın dil bilgisi kullanacak oturum açan kullanıcı olarak.

 [![](store-kit-overview-and-retreiving-product-information-images/image20.png "Select iTunes mağazası alan kullanıcının para birimi ve kendi uygulama içi satın almalara dilini belirler")](store-kit-overview-and-retreiving-product-information-images/image20.png#lightbox)

## <a name="retrieving-product-information"></a>Ürün bilgileri alınıyor

Bir uygulama içi satın alma ürün satış ilk adımı görüntüleme: görüntü için uygulama Mağazası'ndan geçerli fiyat ve açıklama alınıyor.   
   
   
   
 Ürünler ne tür bir uygulama (kaynaklarda, olmayan kaynaklarda veya abonelik türü) sattığı bağımsız olarak, ürün bilgilerini görüntülemek için alma işlemi aynıdır. Bu makalede eşlik InAppPurchaseSample kodunu adlı projesi içerir *sarf* , üretim bilgilerini görüntülemek için almayı gösterir. Bunu gösterir nasıl yapılır:

-  Uygulaması oluşturma `SKProductsRequestDelegate` ve uygulamanıza `ReceivedResponse` soyut yöntemi. Bu kod örneği çağırır `InAppPurchaseManager` sınıfı. 
-  Ödemeler izin verilip verilmediğini görmek için StoreKit ile denetleyin (kullanarak `SKPaymentQueue.CanMakePayments` ). 
-  Örneği bir `SKProductsRequest` iTunes Bağlan tanımlanan ürün kimlikleri. Bu örnekte 's yapılır `InAppPurchaseManager.RequestProductData` yöntemi. 
-  Start yöntemi çağırmak `SKProductsRequest` . Bu uygulama mağazası sunucularına zaman uyumsuz bir çağrının tetikler. Temsilci ( `InAppPurchaseManager` ) olarak adlandırılan geri sonuçlarıyla olacaktır. 
-  Temsilcinin ( `InAppPurchaseManager` ) `ReceivedResponse` yöntemi (ürün fiyatları & açıklamaları veya geçersiz ürünlerle ilgili iletileri) uygulama mağazasından döndürülen veriler ile kullanıcı arabirimini güncelleştirir. 

Genel etkileşim şöyle ( **StoreKit** iOS, yerleşiktir ve **App Store** Apple'nın sunucularından temsil eder):

 [![](store-kit-overview-and-retreiving-product-information-images/image21.png "Ürün bilgisi grafik alınıyor")](store-kit-overview-and-retreiving-product-information-images/image21.png#lightbox)

### <a name="displaying-product-information-example"></a>Ürün bilgisi örnek görüntüleme

[InAppPurchaseSample](https://developer.xamarin.com/samples/monotouch/StoreKit/) *sarf* örnek kod, ürün bilgilerini nasıl alınabilir göstermektedir. Örnek ana ekranında uygulama Mağazası'ndan alınan iki ürün bilgilerini görüntüler:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image23.png "Ana ekran uygulama Mağazası'ndan alınan bilgiler ürünleri görüntüler")](store-kit-overview-and-retreiving-product-information-images/image23.png#lightbox)   
   
   
   
 Alabilir ve ürün bilgilerini görüntülemek için örnek kod aşağıdaki daha ayrıntılı olarak açıklanmıştır.


#### <a name="viewcontroller-methods"></a>ViewController yöntemleri

`ConsumableViewController` Sınıfı, Fiyatlar, ürün kimlikleri sınıfında sabit kodlanmış olan iki ürün için görünen yöneteceğiniz.

```csharp
public static string Buy5ProductId = "com.xamarin.storekit.testing.consume5credits",
   Buy10ProductId = "com.xamarin.storekit.testing.consume10credits";
List<string> products;
InAppPurchaseManager iap;
public ConsumableViewController () : base()
{
   // two products for sale on this page
   products = new List<string>() {Buy5ProductId, Buy10ProductId};
   iap = new InAppPurchaseManager();
}
```

Sınıfta ayrıca olması gerektiğini bir NSObject bildirilen düzeyi kurulumu için kullanılacak bir `NSNotificationCenter` gözlemci:

```csharp
NSObject priceObserver;
```

Gözlemci ViewWillAppear yönteminde oluşturulur ve varsayılan bildirim merkezi aracılığıyla atanan:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
}
```

   
   
 Sonunda `ViewWillAppear` yöntemi, çağrı `RequestProductData` StoreKit isteği başlatmak için yöntem. Bu istek gerçekleştirildikten sonra StoreKit zaman uyumsuz olarak bilgi almak ve uygulamanıza geri akış için Apple'nın sunucularıyla iletişim kurar. Bu tarafından sağlanır `SKProductsRequestDelegate` bir alt kümesi ( `InAppPurchaseManager`) sonraki bölümde açıklanmıştır.

```csharp
iap.RequestProductData(products);
```

Fiyat ve açıklamasını görüntülemek için kod sadece SKProduct bilgileri alır ve Uıkit denetimlere atar (biz görüntülemek bildirimi `LocalizedTitle` ve `LocalizedDescription` – StoreKit otomatik olarak çözer temel alarak fiyatlar ve doğru metni kullanıcının hesap ayarlarını). Aşağıdaki kod, yukarıda oluşturduğumuz bildirim aittir:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (
  InAppPurchaseManager.InAppPurchaseManagerProductsFetchedNotification,
(notification) => {
   // display code goes here, to handle the response from the App Store
   var info = notification.UserInfo;
   if (info.ContainsKey(NSBuy5ProductId)) {
       var product = (SKProduct) info.ObjectForKey(NSBuy5ProductId);
       buy5Button.Enabled = true;
       buy5Title.Text = product.LocalizedTitle;
       buy5Description.Text = product.LocalizedDescription;
       buy5Button.SetTitle("Buy " + product.Price, UIControlState.Normal); // price display should be localized
   }
}
```

Son olarak, `ViewWillDisappear` yöntemi olun gözlemci kaldırılır:

```csharp
NSNotificationCenter.DefaultCenter.RemoveObserver (priceObserver);
```

#### <a name="skproductrequestdelegate-inapppurchasemanager-methods"></a>SKProductRequestDelegate (InAppPurchaseManager) yöntemleri

`RequestProductData` Uygulama ürün fiyatları ve diğer bilgileri almak istediğinde yöntemi çağrılır. Doğru veri türünde ürün kimlikleri koleksiyona ayrıştırır ardından oluşturur bir `SKProductsRequest` bu bilgilerle. Start yöntemi çağırmak için Apple'nın sunucularından yapılacak ağ isteği neden olur. İstek zaman uyumsuz olarak çalıştırın ve arama `ReceivedResponse` başarıyla tamamlandığında temsilci yöntemi.

```csharp
public void RequestProductData (List<string> productIds)
{
   var array = new NSString[productIds.Count];
   for (var i = 0; i < productIds.Count; i++) {
       array[i] = new NSString(productIds[i]);
   }
   NSSet productIdentifiers = NSSet.MakeNSObjectSet<NSString>(array);
   productsRequest = new SKProductsRequest(productIdentifiers);
   productsRequest.Delegate = this; // for SKProductsRequestDelegate.ReceivedResponse
   productsRequest.Start();
}
```

Geliştirme veya uygulamanızı test etme isteği her ürüne erişebilecek şekilde iOS otomatik olarak istek uygulama ile – çalıştığı hangi sağlama profili bağlı olarak uygulama Mağazası 'korumalı alan' veya 'üretim' sürümüne yönlendirir. iTunes Bağlan (olanlar henüz gönderilen veya Apple tarafından onaylanan) yapılandırılmış. Uygulamanızı üretim olduğunda StoreKit istekleri yalnızca bilgi için geri **Onaylandı** ürünler.   
   
   
   
 `ReceivedResponse` Geçersiz kılınan yöntemi, Apple'nın sunucularından verilerle yanıtladığını sonra çağrılır. Bu arka planda çağrıldığı için kod geçerli verileri ayrıştırılamıyor ve ürün bilgileri 'Bu bildirim için dinleme' ViewControllers göndermek için bir bildirim kullanmanız gerekir. Geçerli bir ürün bilgileri toplamak ve bildirim göndermek için kod aşağıda verilmiştir:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   SKProduct[] products = response.Products;
   NSDictionary userInfo = null;
   if (products.Length > 0) {
       NSObject[] productIdsArray = new NSObject[response.Products.Length];
       NSObject[] productsArray = new NSObject[response.Products.Length];
       for (int i = 0; i < response.Products.Length; i++) {
           productIdsArray[i] = new NSString(response.Products[i].ProductIdentifier);
           productsArray[i] = response.Products[i];
       }
       userInfo = NSDictionary.FromObjectsAndKeys (productsArray, productIdsArray);
   }
   NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerProductsFetchedNotification, this, userInfo);
}
```

Aşağıdaki çizimde gösterilmese `RequestFailed` yöntem de geçersiz böylece uygulama mağazası sunucuları erişilebilir değilse (veya başka bir hata oluştuğunda durumda) kullanıcıya bazı geri bildirim sağlayabilirsiniz. Kod örneği yalnızca konsola yazar, ancak gerçek bir uygulama için sorgu tercih edebileceğiniz `error.Code` (örneğin, bir uyarı kullanıcıya) özelliği ve uygulama özel davranışı.

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   Console.WriteLine (" ** InAppPurchaseManager RequestFailed() " + error.LocalizedDescription);
}
```

Bu ekran (hiçbir ürün bilgisi kullanılabilir olduğunda) hemen sonra yükleme örnek uygulaması gösterir:

 [![](store-kit-overview-and-retreiving-product-information-images/image24.png "Ürün bilgisi kullanılabilir olduğunda hemen sonra yükleme örnek uygulaması")](store-kit-overview-and-retreiving-product-information-images/image24.png#lightbox)

## <a name="invalid-products"></a>Geçersiz ürünleri

Bir `SKProductsRequest` geçersiz ürün kimlikleri listesi de döndürebilir. Geçersiz ürünler genellikle aşağıdakilerden biri nedeniyle döndürülür:   
   
   
   
 **Ürün Kimliği yanlış** – yalnızca geçerli ürün kimlikleri kabul edilir.   
   
 **Ürün değil Onaylandı** – test ederken satışa temizlenir tüm ürünleri tarafından döndürülmesi gereken bir `SKProductsRequest`; ancak, üretim yalnızca onaylanmış ürünlerini döndürülür.   
   
 **Uygulama Kimliği açık değil** – joker uygulama kimlikleri (yıldız) izin verme uygulama içi satın alma.   
   
 **Yanlış sağlama profili** – yeniden oluşturup uygulamayı oluştururken doğru bir sağlama profili kullanmalısınız unutmayın (örneğin, uygulama içi satın almalara etkinleştirme), sağlama Portalı'nda uygulama yapılandırmasında değişiklik yaparsanız.   
   
 **iOS uygulamaları Ücretli sözleşme yerinde değil** – StoreKit özellikler çalışmaz hiç olmadığı sürece, Apple Geliştirici hesabınız için geçerli bir sözleşme.   
   
 **İkili reddedildi durumda** – varsa daha önce gönderilen bir ikili reddedildi durumda (veya uygulama mağazası ekibi tarafından geliştirici tarafından) StoreKit özellikler çalışmaz.

`ReceivedResponse` Örnek kodda yöntemi geçersiz ürünleri konsoluna çıkarır:

```csharp
public override void ReceivedResponse (SKProductsRequest request, SKProductsResponse response)
{
   // code removed for clarity
   foreach (string invalidProductId in response.InvalidProducts) {
       Console.WriteLine("Invalid product id: " + invalidProductId );
   }
}
```

## <a name="displaying-localized-prices"></a>Yerelleştirilmiş fiyatlar görüntüleme

Fiyat katmanları her ürün için özel bir fiyat tüm uluslararası uygulama mağazaları belirtin. Fiyatlar doğru her para birimi için görüntülenen emin olmak için aşağıdaki uzantı yöntemi kullanın (tanımlanan `SKProductExtension.cs`) her fiyat özelliği yerine `SKProduct`:

```csharp
public static class SKProductExtension {
   public static string LocalizedPrice (this SKProduct product)
   {
       var formatter = new NSNumberFormatter ();
       formatter.FormatterBehavior = NSNumberFormatterBehavior.Version_10_4;  
       formatter.NumberStyle = NSNumberFormatterStyle.Currency;
       formatter.Locale = product.PriceLocale;
       var formattedString = formatter.StringFromNumber(product.Price);
       return formattedString;
   }
}
```

Düğmenin başlığını ayarlar kod şöyle genişletme yöntemi kullanır:

```csharp
string Buy = "Buy {0}"; // or a localizable string
buy5Button.SetTitle(String.Format(Buy, product.LocalizedPrice()), UIControlState.Normal);
```

Aşağıdaki ekran görüntülerinde sonuçları (Amerikan deposu için bir tane) ve bir Japonca deposu için iki farklı iTunes test hesapları kullanma:   
   
   
   
 [![](store-kit-overview-and-retreiving-product-information-images/image25.png "İki farklı iTunes dil belirli sonuçları gösteren hesapları test etme")](store-kit-overview-and-retreiving-product-information-images/image25.png#lightbox)   
   
   
   
 Mağaza aygıtın dil ayarı etiketleri ve diğer yerelleştirilmiş içeriği etkiler, ürün bilgileri ve fiyat para birimi için kullanılan dili etkiler dikkat edin.   
   
   
   
 Farklı bir depolama kullanmak için test yapmanız gerekenler hesabı geri çağırma **oturum kapatma** içinde **ayarlar > iTunes ve uygulama mağazası** ve farklı bir hesap ile oturum açma uygulamayı yeniden başlatın. Cihazın dilini değiştirmek için Git **ayarlar > Genel > Uluslararası > dil**.

