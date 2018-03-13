---
title: "API tasarım"
description: "Xamarin.iOS API tasarımına perspektifi"
ms.topic: article
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9bebc33affef4a1a25667039dfcdbe345dbd2cd6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="api-design"></a>API tasarım

Mono, parçası olan bir temel sınıf kitaplıkları çekirdek yanı sıra [Xamarin.iOS](http://www.xamarin.com/iOS) çeşitli iOS Mono ile yerel iOS uygulamaları oluşturmak geliştiriciler izin vermek için API için bağlamaları ile gelir.

Xamarin.iOS özünde C# dünyayla Objective-C world arasında köprü birlikte çalışma altyapının yoktur, iOS için bağlamaları hem de CoreGraphics C tabanlı API'ler ister ve [OpenGLES](#OpenGLES).

Objective-C kodunu ile iletişim kurmak için alt düzey çalışma zamanı bulunduğu [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Bu, bağlantılarında üstünde [Foundation](#MonoTouch.Foundation), CoreFoundation ve [Uıkit](#MonoTouch.UIKit) sağlanır.

## <a name="design-principles"></a>Tasarım ilkeleri

Bu bizim tasarım ilkeleri (bunlar da Xamarin.Mac, Mono bağlamaları Objective-C OS x için geçerli) Xamarin.iOS bağlama için bazıları şunlardır:

- Framework tasarım yönergeleri izleyin
- Alt Objective-C sınıfları geliştiricilerine izin ver:

  - Varolan bir sınıftan türetilen
  - Zincir temel oluşturucuya çağırın
  - Yöntemleri geçersiz kılma ile geçersiz kılma sistem C# ' ın yapılması gerekir

- Bir alt kümesi C# Standart yapıları ile çalışması gerekir
- Objective-C seçiciler geliştiricilerine gösterme
- Rastgele Objective-C kitaplıkları çağırmak için bir mekanizma sağlar
- Ortak Objective-C görevler kolay ve sabit Objective-C görevleri olası olun
- C# özellikleri olarak Objective-C özelliklerini ortaya
- Kesin türü belirtilmiş bir API ortaya çıkarır:
- Tür güvenliği artırın
- Çalışma zamanı hataları simge durumuna küçült
- Dönüş türleri üzerinde IDE IntelliSense Al
- IDE açılan belgelerine sağlar
- IDE araştırması API'leri teşvik edin:
- Yerel C# türleri:

    - Örnek: Bu gibi zayıf yazılmış dizi gösterme yerine:
        ```
        NSArray *getViews
        ```
        Biz bunları bu gibi güçlü türlerini kullanır:
    
        ```
        NSView [] Views { get; set; }
        ```
    
    Bu, Visual Studio Mac için API göz atarken otomatik tamamlama yapmasını sağlar ve ayrıca tüm sağlar `System.Array` döndürülen değerin üzerinde kullanılabilir olması için işlemler ve LINQ katılmayı dönüş değeri sağlar

- [Dize NSString olur](~/ios/internals/api-design/nsstring.md)
- Numaralandırmalar C# numaralandırmalar ve C# numaralandırmaları [bayraklar] özniteliklere sahip olması gereken int ve uint parametreleri Aç
- Tür bağımsız NSArray nesneleri yerine diziler kesin türü belirtilmiş bir dizi olarak kullanıma sunar.
- Olayları ve bildirimleri, kullanıcılar arasında bir seçim verin:

    - Kesin türü belirtilmiş varsayılan sürümdür
    - Gelişmiş kullanım durumları için zayıf yazılmış sürümü

- Objective-C temsilci düzenini destekler:

    - C# olay sistemi
    - C# temsilciler (Lambda'lar, anonim yöntemler ve System.Delegate) Objective-C API'lerini "Taşı" olarak kullanıma sunar.

### <a name="assemblies"></a>Derlemeler

Xamarin.iOS oluşturan derlemeler çeşitli içerir *Xamarin.iOS profil*. [Derlemeleri](~/cross-platform/internals/available-assemblies.md) sayfası, daha fazla bilgi içerir.

### <a name="major-namespaces"></a>Birincil ad alanları 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) ad alanının C# ve Objective-c arasındaki dünyaları birleştirmesi geliştiriciler sağlar
Cocoa # ve Gtk # deneyimlerden göre özellikle iOS için tasarlanan yeni bir bağlama budur.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) ad alanı, temel veri türlerini iOS parçası olan Objective-C Foundation framework ile birlikte çalışmak için tasarlanmış ve nesne yönelimli Objective-c programlama için tabanıdır sağlar

Objective-c sınıflardan hiyerarşisini Xamarin.iOS C# ' ta yansıtır Örneğin, Objective-C temel sınıf [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) C# üzerinden kullanılabilir [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Bazı durumlarda Biz bu ad alanı temel alınan Objective-C Foundation türleri için bağlamaları sağlasa da, temel türleri .NET türlerine eşledikten. Örneğin:

- Postalarla yerine [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) ve [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), çalışma zamanı olarak C# sunan [dize](https://developer.xamarin.com/api/type/System.String/)s ve kesin türü belirtilmiş [dizi](https://developer.xamarin.com/api/type/System.Array/)boyunca s API.

- Çeşitli Yardımcısı API'leri açığa burada üçüncü taraf Objective-C API'leri, diğer iOS API'leri veya Xamarin.iOS tarafından şu anda bağlı değil API'larını bağlamak geliştiricilere izin vermek için.

API bağlama ile ilgili daha fazla ayrıntı için bkz: [Xamarin.iOS bağlama Oluşturucu](~/cross-platform/macios/binding/binding-types-reference.md) bölümü.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) tüm Objective-C bağlamaları için temel türdür. Xamarin.iOS türleri yansıtma CocoaTouch API'leri iOS türlerinden birini iki sınıf: C türleri (genellikle refered için CoreFoundation türü olarak) ve Objective-C türleri (bunlar tüm NSObject sınıfından türetilen).

Türlerinin her zaman bir yönetilmeyen tür yansıtan aracılığıyla yerel nesnesini almak mümkün [işlemek](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) özelliği.

Mono atık toplama, nesnelerin tümü için sağlayacak sırada `Foundation.NSObject` uygulayan [System.IDisposable](https://developer.xamarin.com/api/type/System.IDisposable/) arabirimi. Bu, açıkça belirtilen tüm NSObject kaynakları atık toplayıcı başlangıç beklemek zorunda kalmadan serbest bırakabilirsiniz, anlamına gelir. Ağır NSObjects, örneğin, büyük veri blokları işaretçileri tutabilir UIImages kullanırken, bu önemlidir.

Türünüz sonlandırma gerçekleştirmeniz gerekirse, geçersiz kılma [NSObject.Dispose(bool) yöntemi](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) Dispose parametresi olan "bool atma", ve ayarlayın, true olarak Dispose yöntemini çünkü çağrıldığından emin anlamına gelir, kullanıcı nesne üzerinde açıkça çağrılan Dispose (). Değeri false ise, bu, (bool atma) Dispose yöntemini sonlandırıcıyı sonlandırıcıyı iş parçacığında çağrıldığından emin anlamına gelir. []()


##### <a name="categories"></a>Kategoriler

Xamarin.iOS 8.10 ile başlatmanızı Objective-C kategorileri C# ' dan oluşturmak mümkündür.

Bu yapılır kullanarak `Category` öznitelik, öznitelik bağımsız değişken olarak genişletmek için türünü belirtme. Aşağıdaki örnek, örneğin NSString uzatır.

    [Category (typeof (NSString))]

Her kategori yöntemi yöntemleri Objective-C kullanarak dışarı aktarmak için normal mekanizmasını kullanarak `Export` özniteliği:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Tüm yönetilen genişletme yöntemleri statik olmalıdır, ancak C# genişletme yöntemleri için standart söz dizimini kullanarak Objective-C örnek yöntemleri oluşturmak mümkündür:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

ve ilk bağımsız değişken genişletme yöntemi üzerinde yöntemi çağrıldı örneği olacaktır.

Tam örnek:

```csharp
[Category (typeof (NSString))]
public static class MyStringCategory
{
    [Export ("toUpper")]
    static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }
}
```

Bu örnek bir yerel toUpper örnek yöntemi hedefi C'den çağrılabilir NSString sınıfı ekler

```csharp
[Category (typeof (UIViewController))]
public static class MyViewControllerCategory
{
    [Export ("shouldAudoRotate")]
    static bool GlobalRotate ()
    {
        return true;
    }
}
```

Bu olduğu kullanışlı bir senaryo ekleme bir yöntem temelinizde sınıflarda kümesinin tamamını için örneğin, bu tüm yapacağı `UIViewController` örnekleri rapor bunlar döndürebilirsiniz:

```csharp
[Category (typeof (UINavigationController))]
class Rotation_IOS6 {
      [Export ("shouldAutorotate:")]
      static bool ShouldAutoRotate (this UINavigationController self)
      {
          return true;
      }
}
```


##### <a name="preserveattribute"></a>PreserveAttribute

PreserveAttribute bir tür ya da bir türünün bir üyesi korumak için – Xamarin.iOS dağıtım aracı – mtouch bildirmek için kullanılan özel uygulama boyutunu azaltmak için ne zaman işlenir aşamasında bir özniteliktir.

Uygulama tarafından statik olarak bağlantılı olmayan her üye tabidir kaldırılır. Bu nedenle, bu öznitelik değil, statik olarak başvurulur, ancak, uygulamanız tarafından hala gerekli olduğunu üyeleri işaretlemek için kullanılır.

Örneği için dinamik olarak türleri örneği varsa, türlerinin varsayılan oluşturucu korumak isteyebilirsiniz. XML serileştirme kullanırsanız, türlerinizi özelliklerini korumak isteyebilirsiniz.

Bu öznitelik her üye bir türde veya türü uygulayabilirsiniz. Tüm türü korumak istiyorsanız, söz dizimini kullanabilirsiniz [korumak (AllMembers = true)] türünde.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>Uıkit

[Uıkit](https://developer.xamarin.com/api/namespace/UIKit/) ad alanı, bire bir eşleme için C# sınıfları biçiminde CocoaTouch oluşturan UI bileşenlerinin tümünü içerir. API, C# dilinde kullanılan kurallarına uygun şekilde değiştirildi.

C# temsilciler ortak işlemler için sağlanır. Bkz: [Temsilciler](#Delegates) daha fazla bilgi için bölüm.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

Biz OpenGLES için dağıtmak bir [sürümünde değişiklik](https://developer.xamarin.com/api/namespace/OpenTK/) , [OpenTK](http://www.opentk.com/) API, CoreGraphics veri türleri kullanacak şekilde değiştirilmiş OpenGL nesne yönelimli bir bağlamaya yanı sıra yalnızca gösterme iOS kullanılabilir işlevselliği.

Belgelenen ES11.GL türü OpenGLES 1.1 işlevsellik mevcuttur [burada](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) türü.

OpenGLES 2.0 işlevselliği belgelenen ES20.GL türü kullanılabilir [burada](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) türü.

OpenGLES 3.0 işlevselliği belgelenen ES30.GL türü kullanılabilir [burada](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) türü.


### <a name="binding-design"></a>Tasarım bağlama

Xamarin.iOS yalnızca bir bağlama temel Objective-C platformu değil. .NET türü ve daha iyi karışım C# ve Objective-c gönderme sistemi genişletir

Yalnızca P/Invoke Windows ve Linux yerel kitaplıkları çağırmak için yararlı bir araçtır veya Windows COM birlikte çalışma için destek kullanılabilir IJW gibi Xamarin.iOS bağlama C# nesneleri Objective-C nesnelere desteklemek için çalışma zamanı genişletir.

Birkaç bölümleri Xamarin.iOS uygulamaları oluşturma, geliştiriciler yardımcı olacak kullanıcıları için gerekli değildir sonraki tartışma nasıl şeyler yapılır ve bunları daha karmaşık uygulamaları oluştururken yardımcı olacak anlayın.



#### <a name="types"></a>Türler

Burada anlamlı yapılan C# türleri C# universe için alt düzey Foundation türleri yerine sunulur.  Bunun anlamı [API NSString yerine C# "dize" türünü kullanır](~/ios/internals/api-design/nsstring.md) ve NSArray gösterme yerine kesin türü belirtilmiş C# diziler kullanır.

Arka plandaki genel olarak, Xamarin.iOS ve Xamarin.Mac tasarımında `NSArray` nesne sunulmaz. Bunun yerine, çalışma zamanı otomatik olarak dönüştürür `NSArray`bazı kesin türü belirtilmiş dizilerine s `NSObject` sınıfı. Bu nedenle, Xamarin.iOS zayıf yazılmış bir yöntem bir NSArray döndürülecek GetViews gibi göstermiyor:

```csharp
NSArray GetViews ();
```

Bunun yerine, bağlama şöyle kesin türü belirtilmiş bir dönüş değeri sunar:

```csharp
UIView [] GetViews ();
```

İçinde sunulan yöntemleri bazılarını vardır `NSArray`, burada kullanmak istediğiniz köşe durumlarda bir `NSArray` doğrudan, ancak bunların kullanılması API bağlamasında önerilmez.

Buna ek olarak **Klasik API** gösterme yerine `CGRect`, `CGPoint` ve `CGSize` CoreGraphics API'SİNDEN biz olanlar yerini `System.Drawing` uygulamaları `RectangleF`, `PointF`ve `SizeF` yardımcı olacak şekilde OpenTK kullanan mevcut OpenGL kod geliştiricilerin korumak. Yeni 64 bit kullanırken **Unified API**, CoreGraphics API'nin kullanılması gerekir.

<a name="Inheritance" />

#### <a name="inheritance"></a>Devralma

Xamarin.iOS API tasarım, "temel" C# anahtar sözcüğünü kullanarak temel uygulamayı zincirleme yanı sıra "geçersiz kılma" anahtar sözcüğü türetilmiş bir sınıf kullanarak C#, bir türü genişletmek aynı şekilde yerel Objective-C türleri genişletmek geliştiricilere sağlar.

Tüm Objective-C sistem zaten içinde Xamarin.iOS kitaplıkları Sarmalanan olduğundan bu tasarım geliştiricilerin kendi geliştirme sürecinin bir parçası olarak Objective-C Seçici ile ilgilenen kaçının olanak tanır.


#### <a name="types-and-interface-builder"></a>Türleri ve arabirim Oluşturucusu

Arabirim Oluşturucu tarafından oluşturulan türleri örnekleridir .NET sınıfları oluşturduğunuzda, tek bir alan bir oluşturucu sağlamanız gereken `IntPtr` parametresi.
Bu, yönetilmeyen nesne içeren yönetilen nesne örneği bağlamak için gereklidir.
Bu gibi tek bir satır kod oluşur:

```csharp
public partial class void MyView : UIView {
   // This is the constructor that you need to add.
   public MyView (IntPtr handle) : base (handle) {}
}
```

<a name="Delegates" />


#### <a name="delegates"></a>Temsilciler

Objective-C ve C# word temsilci için farklı anlamları her dilde sahiptir.

Objective-C dünyada ve CocoaTouch hakkında çevrimiçi bulduğunuz belgelerinde temsilci genellikle bir dizi yöntem yanıtlar bir sınıf örneğidir. Yöntem her zaman zorunlu olmayan olmasına, çok benzer bir C# ile arabirim oluşturmak için fark budur.

Bu temsilci Uıkit ve diğer CocoaTouch API'ları önemli bir rol oynar. Bunlar, çeşitli görevleri gerçekleştirmek için kullanılır:

-  Kodunuza (C# veya Gtk + olay teslimi benzer) bildirimleri sağlamak için.
-  Modelleri için veri görselleştirme denetimleri uygulamak için.
-  Sürücü için bir denetim davranışını.


Programlama modeli, bir denetim için davranışı değiştirmek için türetilen sınıflar oluşturulmasını en aza indirmek için tasarlanmıştır. Bu çözüm ne diğer GUI araç takımları yıllar içinde yaptığınızdan için Ruhu benzer: Gtk ait sinyalleri, Qt yuvaları, Winforms olayları, WPF/Silverlight etkinlikler ve benzeri. Arabirimler (her eylem için bir tane) yüzlerce olması veya değil gereken çok fazla yöntemleri uygulamak geliştiriciler gerektirmeden önlemek için isteğe bağlı yöntemi tanımları Objective-C destekler. Bu, uygulanacak tüm yöntemleri gerektiren C# arabirimleri farklıdır.

Objective-C sınıflarda bu programlama desenini kullanan sınıflar genellikle adlı bir özellik kullanıma görürsünüz `delegate`, arabiriminin zorunlu bölümleri ve sıfır veya daha fazla isteğe bağlı olarak bölümleri uygulamak için gerekli olduğu.

Xamarin.iOS içinde bu temsilciye bağlamak için üç birbirini dışlayan mekanizmaları sunulur:

1.  [Olayları aracılığıyla](#Via_Events).
2.  [Aracılığıyla kesin türü belirtilmiş bir `Delegate` özelliği](#StrongDelegate)
3.  [Gevşek aracılığıyla yazılan bir `WeakDelegate` özelliği](#WeakDelegate)

Örneğin, göz önünde bulundurun [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) sınıfı. Bunun için gönderir bir [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) atandığı örneği [temsilci](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) özelliği.

<a name="Via_Events" />

##### <a name="via-events"></a>Üzerinden olayları

Birçok türü için Xamarin.iOS otomatik olarak iletir uygun bir temsilci oluşturacak `UIWebViewDelegate` C# olayların üzerine çağrıları. İçin `UIWebView`:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) yöntemi eşleştirilir [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) olay.
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) yöntemi eşleştirilir [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) olay.
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) yöntemi eşleştirilir [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) olay.

Örneğin, bu basit bir program web yüklenirken görüntülediğinizde başlangıç ve bitiş zamanlarını kaydeder:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Özellikleri

Olay birden fazla abonelik olabilir, olayların kullanışlıdır. Ayrıca, olayları çalışmalarını sınırlı söz konusu olduğunda koddan dönüş değeri yok.

Kod bir değer döndürmek için beklenirken durumlarda, biz yerine özelliklerini seçti. Bu, yalnızca bir yöntem nesne içinde belirli bir zamanda ayarlanabilir anlamına gelir.

Örneğin, işleyicisi ekranda klavyede kapatmak için bu düzenek kullanabilirsiniz bir `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`'S `ShouldReturn` özelliği bu durumda TextField basılan dönüş düğmesi ile bir şeyler olup olmadığını belirler ve bir Boole değeri döndüren bir temsilci bağımsız değişken olarak alır. Bizim yönteminde döndürürüz *true* çağırana, ancak biz de klavye ekranından kaldırmak (textfield çağırdığında böyle `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Temsilci özelliği aracılığıyla kesin türü belirtilmiş

Olayları kullanmayı tercih ederseniz, kendi sağlayabilirsiniz [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) alt sınıf ve atayın [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) özelliği. UIWebView.Delegate atandıktan sonra UIWebView olay gönderme mekanizması artık çalışmaz ve ilgili olaylar meydana geldiğinde UIWebViewDelegate yöntemleri çağrılır.

Örneğin, bu basit tür bir web görünümü yükleme süresini kaydeder:

```csharp
class Notifier : UIWebViewDelegate  {
    DateTime startTime, endTime;

    public override LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    public override LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}
```

Yukarıdaki kod bu gibi kullanılır:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Bir UIWebViewer yukarıdaki oluşturacak ve bu bildirim, iletilere yanıt oluşturduğumuz bir sınıf örneği iletileri göndermek için ister.

Bu deseni da örnek durumda UIWebView, bazı denetimler için davranışını denetlemek için kullanılan [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) özelliği sağlar `UIWebView` denetim örneğine olup olmadığını `UIWebView` yükleyecek bir veya sayfa.

Desen isteğe bağlı olarak birkaç denetimleri için veri sağlamak için de kullanılır. Örneğin, [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) denetimi güçlü bir tablo işleme denetimi – ve Görünüm ve içeriği bir örneği tarafından yönlendirilen bir [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>Gevşek WeakDelegate özelliği aracılığıyla yazılan

Kesin türü belirtilmiş özelliği yanı sıra, aynı zamanda farklı şeyler bağlamak Geliştirici verir zayıf bir yazılı temsilci yoktur.
Kesin türü belirtilmiş her yerde `Delegate` özelliği Xamarin.iOS'ın bağlamada, karşılık gelen gösterilir `WeakDelegate` özelliği de gösterilir.

Kullanırken `WeakDelegate`, düzgün şekilde sınıfını kullanarak dekorasyon için sorumlu [verme](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) Seçici belirtmek için özniteliği. Örneğin:

```csharp
class Notifier : NSObject  {
    DateTime startTime, endTime;

    [Export ("webViewDidStartLoad:")]
    public void LoadStarted (UIWebView webview)
    {
        startTime = DateTime.Now;
    }

    [Export ("webViewDidFinishLoad:")]
    public void LoadingFinished (UIWebView webView)
    {
        endTime= DateTime.Now;
    }
}

[...]

var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.WeakDelegate = new Notifier ();
```

Bu kez Not `WeakDelegate` özelliği atanmış, `Delegate` özellik kullanılmayacak. Ayrıca, [dışarı aktarılacak] istediğiniz devralınan bir taban sınıf içinde yöntemi uygularsanız, genel bir yöntem olmalısınız.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>C Objective-C temsilci desen eşleştirme&#35;

Şuna Objective-C örnekleri gördüğünüzde:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Bu oluşturmak ve "SomethingDelegate" sınıfının bir örneği oluşturun ve foo değişkeni temsilci özelliği için değer atamak için dil bildirir. Bu mekanizma Xamarin.iOS tarafından desteklenir ve C# sözdizimi:

```csharp
foo.Delegate = new SomethingDelegate ();
```

Objective-C eşleme kesin türü belirtilmiş sınıfları sınıfları temsilci sağlanan Xamarin.iOS sunuyoruz. Bunları kullanmak için olmaları sınıflara ve Xamarin.iOS'ın uygulama tarafından tanımlanan yöntemleri geçersiz kılma. Bölüm "modelleri" aşağıdaki nasıl çalıştığını daha fazla bilgi için bkz.


##### <a name="mapping-delegates-to-c35"></a>C eşleme temsilciler&#35;

Uıkit genel iki biçimde Objective-C temsilcileri kullanır.

İlk form, bir bileşenin modeli için bir arabirim sağlar. Örneğin, bir listesi için veri depolama tesisi gibi isteğe bağlı bir görünüm için veri sağlamak için bir mekanizma görüntüleyin.  Bu durumlarda, her zaman uygun sınıfının bir örneğini oluşturmak ve değişkeni atayın.

Aşağıdaki örnekte, sağladığımız `UIPickerView` dizeleri kullanan bir model için bir uygulama ile:

```csharp
public class SampleTitleModel : UIPickerViewTitleModel {

    public override string TitleForRow (UIPickerView picker, nint row, nint component)
    {
        return String.Format ("At {0} {1}", row, component);
    }
}

[...]

pickerView.Model = new MyPickerModel ();
```

İkinci form olayları için bildirim sağlamaktır. Biz yine, yukarıda özetlenen biçimde API kullanıma rağmen bu gibi durumlarda da hızlı işlemleri için daha basit ve anonim Temsilciler ve C# ' deki lambda ifadeleri ile tümleşik olması gereken C# olayları, sunuyoruz.

Örneğin, için abone olabilirsiniz `UIAccelerometer` olayları:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

Bunlar anlamlı ancak Programcı olarak birini veya diğerini seçmelisiniz burada iki seçenek kullanılabilir. Kesin türü belirtilmiş bir Yanıtlayıcı/temsilci kendi örneğini oluşturmak ve atamak, C# olayları işlevsel olmayacak. C# olayları kullanırsanız, Yanıtlayıcı/temsilci sınıftaki yöntemlerin hiçbir zaman çağrılır.

Kullanılan önceki örnek `UIWebView` C# 3.0 Lambda'lar şöyle kullanılarak yazılabilir:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Olaylara yanıt verme

Objective-C kodda bazen birden çok denetimleri ve birden çok denetim bilgilerini sağlayıcıları için olay işleyicileri barındırılacağı aynı sınıfta. Sınıfları iletileri yanıtlayın olduğundan ve sınıfları iletileri yanıtlayın sürece bu mümkündür, nesneler birbirine bağlamak mümkündür.

Daha önce ayrıntılı olarak Xamarin.iOS hem C# olay tabanlı programlama modelini destekler ve yeni bir sınıf, oluşturabileceğiniz Objective-C temsilci düzeni temsilci uygular ve istenen yöntemlerini geçersiz kılar.

Ayrıca birden çok farklı işlemler için yanıtlayıcılarını tüm sınıfının aynı örneği barındırıldığı Objective-C'ın modeli desteklemek mümkündür. Ancak bunun için alt düzey özellikleri Xamarin.iOS bağlamanın kullanması gerekir.

Örneğin, her ikisi de yanıtlamasını sınıfınız istediyseniz `UITextFieldDelegate.textFieldShouldClear`: ileti ve `UIWebViewDelegate.webViewDidStartLoad`: sınıfının aynı örneği [verme] özniteliği bildirimi kullanması gerekir:

```csharp
public class MyCallbacks : NSObject {
    [Export ("textFieldShouldClear:"]
    public bool should_we_clear (UITextField tf)
    {
        return true;
    }

    [Export ("webViewDidStartLoad:")]
    public void OnWebViewStart (UIWebView view)
    {
        Console.WriteLine ("Loading started");
    }
}
```

C# adları yöntemleri için önemli değildir; önemli olan tüm [dışarı aktarma] özniteliği için geçirilen dizelerdir.

Bu stili programlama kullanırken, C# parametreleri çalışma zamanı altyapısı geçer gerçek türleri eşleştiğinden emin olun.


#### <a name="models"></a>Modeller

Uıkit Depolama tesisleri veya yardımcı sınıfları kullanılarak uygulanan yanıtlayıcılarını bunlar genellikle temsilci olarak Objective-C kodunu adlandırılır ve protokoller olarak uygulanır.

Objective-C protokolleri arabirimlerdir gibi ancak isteğe bağlı yöntemler destekledikleri – başka bir deyişle, tüm yöntemleri Protokolü çalışmak uygulanması gerekir.

Bir model kullanmanın iki yolu vardır. El ile uygulamak veya varolan kesin türü belirtilmiş tanımları kullanır.


Xamarin.iOS tarafından bağlı olmayan bir sınıf uygulama çalıştığınızda el ile mekanizması gereklidir. Yapmak çok kolaydır:

-  Sınıfınız kayıt zamanıyla için bayrak
-  Geçersiz kılmak istediğiniz her yöntem gerçek Seçici adla [verme] özniteliğini uygulayın
-  Sınıfının örneği ve onu geçirin.

Örneğin, aşağıdaki isteğe bağlı yöntemlerden sadece birini UIApplicationDelegate Protokolü tanımı'nda uygulayın:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective-C Seçici adı ("applicationDidFinishLaunching:") verme özniteliğiyle bildirilir ve sınıf kayıtlı `[Register]` özniteliği.

Xamarin.iOS el ile bağlama gerektirmeyen kesin türü belirtilmiş bildirimleri, kullanıma hazır sağlar. Bu programlama modeli desteklemek için Xamarin.iOS çalışma zamanı [Model] özniteliği bir sınıf bildirimi destekler. Bu yöntemleri olmadığı sürece, bu sınıftaki tüm yöntemleri wire değil çalışma zamanı açıkça gerçekleştirilen bildirir.

Uıkit içinde buna, bir iletişim kuralı isteğe bağlı yöntemleriyle temsil eden sınıflar şu şekilde yazılır:

```csharp
[Model]
public class SomeViewModel : NSObject {
    [Export ("someMethod:")]
    public virtual int SomeMethod (TheView view) {
       throw new ModelNotImplementedException ();
    }
    ...
}
```

Yalnızca bazı yöntemlere uygulayan bir modeli uygulamak istediğinizde, yapmanız gereken tek şey ilgilendiğiniz ve diğer yöntemleri yoksay yöntemleri geçersiz kılmak için. Çalışma zamanı yalnızca değil özgün yöntemleri Objective-C dünyaya üzerine yöntemleri bağlanacağını.

Önceki el ile örneğe eşdeğerdir:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Seçici, bağımsız değişken veya C# eşlemeye türlerini bulmak için Objective-C üstbilgi dosyalarına araştırma yapmak için gerek yoktur ve, IntelliSense Visual Studio'dan Mac için güçlü türleriyle birlikte aldığını yararları şunlardır


#### <a name="xib-outlets-and-c35"></a>XIB çıkışlar ve C&#35;

> [!IMPORTANT]
> Bu bölümde, XIB dosyaları kullanırken çıkışlar IDE tümleşme açıklanmaktadır. İOS için Xamarin Tasarımcısı'nı kullanarak, bu tüm bir ad altında girerek değiştirilir **kimliği > adı** aşağıda gösterildiği gibi IDE özellikler bölümünde:
>
> [![](images/designeroutlet.png "Bir öğe adı iOS Tasarımcısı girme")](images/designeroutlet.png#lightbox)
>
>İOS Tasarımcısı hakkında daha fazla bilgi için lütfen gözden [Tasarımcısı iOS giriş](~/ios/user-interface/designer/introduction.md#how-it-works) belge.

Bu alt düzey açıklaması nasıl çıkışlar C# ile tümleştirmek ve Xamarin.iOS İleri düzey kullanıcılar için sağlanır. Ne zaman eşleştirme Mac için Visual Studio kullanarak otomatik olarak kullanarak arka planda gerçekleştirilir uçuş kodu sizin için oluşturulur.

Arabirim Oluşturucu, kullanıcı arabirimiyle tasarlarken, uygulamanın görünümünü tasarlama yalnızca ve bazı varsayılan bağlantı. Program aracılığıyla bilgilerini getirmek, çalışma zamanında denetim davranışını değiştirmek veya denetiminin çalışma zamanında değiştirmek istiyorsanız, bazı denetimler yönetilen kodunuzu bağlamak gereklidir.

Bu, birkaç adımda gerçekleştirilir:

1.  Ekleme **çıkışı bildirimi** için **dosyanın sahibi**.
1.  Denetiminize bağlanmak **dosyanın sahibi**.
1.  UI artı bağlantıları XIB/NIB dosyasına depolar.
1.  Çalışma zamanında NIB dosyası yükleyin.
1.  Çıkış değişken erişin.


(3) (1) adımlara arabirimi Oluşturucu arabirimleriyle oluşturmak için Apple belgelerinde ele alınmıştır.

Xamarin.iOS kullanırken, uygulamanızın UIViewController türeyen bir sınıf oluşturmanız gerekir. Şu uygulanan onu şöyle:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Ardından, ViewController NIB dosyadan yüklemek için bunu:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Bu, NIB kullanıcı arabirimini yükler. Şimdi, çıkışlar erişmek için bunlara erişmek için istiyoruz çalışma zamanı hakkında bilgilendirmek gereklidir. Bunu yapmak için `UIViewController` alt sınıf gereken özellikleri bildirme ve bunları [Bağlan] özniteliğiyle ek açıklama eklemek. Böyle:

```csharp
[Connect]
UITextField UserName {
    get {
        return (UITextField) GetNativeField ("UserName");
    }
    set {
        SetNativeField ("UserName", value);
    }
}
```

Özelliği, aslında getirir ve gerçek yerel tür için değer depolayan bir uygulamasıdır.

Mac ve InterfaceBuilder için Visual Studio kullanarak bu hakkında endişelenmeniz gerekmez. Mac için Visual Studio, projenizin bir parçası olarak derlenmiş bir parçalı sınıf kodu ile bildirilen tüm çıkışlar otomatik olarak yansıtır.

#### <a name="selectors"></a>Seçici

Bir çekirdek Objective-C programlama seçiciler kavramdır. Genellikle, bir seçici geçirmeniz veya bir seçici yanıt kodunuzu bekliyor API'leri üzerinden gelecektir.

Yeni Seçicilerin C# ' ta oluşturma çok kolay – yalnızca yeni bir örneğini oluşturmanız `ObjCRuntime.Selector` sınıfı ve bunu gerektiren API'sindeki herhangi bir yerde sonucu kullanın. Örneğin:

```csharp
var selector_add = new Selector ("add:plus:");
```

Bir C# yöntemi yanıt için bir seçici çağrısına, öğesinden devralmalıdır `NSObject` türü ve C# yöntemi gerekir donatılmış Seçici kullanarak ad `[Export]` özniteliği. Örneğin:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Seçicinin Not adları **gerekir** tüm ara ve izleyen iki nokta üst üste dahil olmak üzere tam olarak eşleşen (":"), varsa.

#### <a name="nsobject-constructors"></a>NSObject oluşturucular

Öğesinden türetilen Xamarin.iOS çoğu sınıflarda `NSObject` oluşturucular nesnesinin işlevselliğini belirli açığa çıkarır, ancak hemen göze olmayan çeşitli oluşturucular de açığa çıkarır.

Oluşturucular aşağıdaki gibi kullanılır:

```csharp
public Foo (IntPtr handle)
```

Bu oluşturucu, çalışma zamanı yönetilmeyen bir sınıfa sınıfınıza eşleme gerektiğinde, sınıf örneği oluşturmak için kullanılır. Bu, bir XIB/NIB dosya yükleme ortaya çıkar.  Bu noktada, yönetilmeyen dünyada Objective-C çalışma zamanı nesne oluşturmuş olacaksınız ve bu oluşturucuyu yönetilen yan başlatmak için çağrılır.

Genellikle, yapmanız gereken tek şey temel oluşturucuyu işleme parametresi ile gövdesinde çağrısı, gerekli olan sıfırlamaları yapın.

```csharp
public Foo ()
```

Bu sınıf için varsayılan oluşturucu ve sınıfları sağlanan Xamarin.iOS bu Foundation.NSObject sınıfı ve tüm sınıflar arasında başlatır ve sonunda bu Objective-C zincir `init` sınıfında yöntemi.

```csharp
public Foo (NSObjectFlag x)
```

Bu oluşturucu örneği başlatmak, ancak sonunda Objective-C "Init" yöntemi çağırma kodu önlemek için kullanılır. Başlatma için zaten kaydettiğiniz zaman, genellikle bu kullanın (kullandığınızda `[Export]` , oluşturucu üzerinde) veya bunu önceden yaptığınızda, başlatma aracılığıyla başka bir ortalaması.

```csharp
public Foo (NSCoder coder)
```

Bu oluşturucu, nesne NSCoding örneğinden burada başlatılmış olduğu durumlarda sağlanır. Daha fazla bilgi için bkz. Apple'nın [arşivler ve seri hale getirme programlama kılavuzu.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Özel Durumlar

Xamarin.iOS API tasarım, C# özel durumlar olarak Objective-C özel durumları oluşturmaz. Tasarım hiçbir çöp Objective-C dünyasına ilk başta gönderilmesini zorlayan ve geçersiz veri hiç önce üretilmesi gereken özel durumlar bağlama tarafından üretilen Objective-C dünyasına geçirildi.

#### <a name="notifications"></a>Bildirimler

İOS ve OS X geliştiriciler temel platform tarafından yayınlanan Bildirimlere abone olabilirsiniz. Bu kullanılarak yapılır `NSNotificationCenter.DefaultCenter.AddObserver` yöntemi. `AddObserver` Yöntemi iki parametre alan bir abone olmak istediğiniz bildirim; diğer bildirim oluştuğunda çağrılacak yöntemidir.

Xamarin.iOS hem Xamarin.Mac çeşitli bildirimler tuşları bildirimleri tetikleyen sınıf üzerinde barındırılır. Örneğin, bildirimler yükseltilmiş tarafından `UIMenuController` olarak barındırılan `static NSString` özelliklerinde `UIMenuController` "Bildirim" adıyla bitiş sınıfları.

### <a name="memory-management"></a>Bellek Yönetimi

Xamarin.iOS ele artık kullanımda olduğunda kaynaklar sizin için serbest olarak önemli bir atık toplayıcı vardır. Çöp toplayıcı ek olarak, tüm nesneler, türetilen `NSObject` uygulamak `System.IDisposable` arabirimi.

#### <a name="nsobject-and-idisposable"></a>NSObject ve IDisposable

Gösterme `IDisposable` arabirimidir büyük bellek bloklarını kapsülleyen nesneleri serbest geliştiriciler Yardım için kolay bir yol (örneğin, bir `UIImage` yalnızca zararsız bir işaretçi gibi görünebilir, ancak 2 megabayt görüntüye işaret eden ) ve diğer önemli ve sınırlı kaynakları (örneğin, bir video kod çözme arabellek).

NSObject IDisposable arabirimini uygulayan ve ayrıca [.NET Dispose düzeni](http://msdn.microsoft.com/en-us/library/fs2xkftw.aspx). Bu, o alt sınıfın Dispose davranışını geçersiz kılmak ve isteğe bağlı kendi kaynakları serbest bırakmak için NSObject geliştiriciler sağlar. Örneğin, görüntüleri bir dizi tutar bu görünüm denetleyicisini göz önünde bulundurun:

```csharp
class MenuViewController : UIViewController {
    UIImage breakfast, lunch, dinner;
    [...]
    public override void Dispose (bool disposing)
    {
        if (disposing){
             if (breakfast != null) breakfast.Dispose (); breakfast = null;
             if (lunch != null) lunch.Dispose (); lunch = null;
             if (dinner != null) dinner.Dispose (); dinner = null;
        }
        base.Dispose (disposing)
    }
}
```

Yönetilen bir nesnenin silindiğinde, artık yararlıdır. Nesneleri başvuru hala olabilir, ancak nesnesi için tüm intents ve purposes bu noktada geçersiz. Bazı .NET API'lerini bırakılmış bir nesne üzerinde herhangi bir yöntem örneğin erişmeye çalıştığınızda bir ObjectDisposedException atma emin olun:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

"Görüntü" değişkeni erişmeye devam edebilirsiniz olsa bile bu gerçekten geçersiz başvuru ve artık görüntü tutulan Objective-C nesne noktalarına olur.

Ancak bir nesne C# atma nesne mutlaka yok, anlamına gelmez. Bunu şey C# için nesneniz başvuru bırakın. Cocoa ortamı kendi kullanımı için geçici bir başvuru kalmasını sağlamış mümkündür. Örneğin, bir görüntüye UIImageView'ın resim özelliği ayarlı ve sonra görüntüyü silmek, temel alınan UIImageView kendi başvuru gerçekleştirilecek ve işlemi tamamlanana kadar bu nesneye bir başvurusu tutar kullanıyor.

#### <a name="when-to-call-dispose"></a>Dispose çağrısı yapıldığında

Nesnenin RID Mono gerektiğinde Dispose öğesini çağırması gerekir. Mono, NSObject gerçekten bellek ya da bir bilgi havuzu gibi önemli bir kaynak başvuru bulunduran hiçbir bilgi olduğunda olası kullanım durumdur. Bu gibi durumlarda, hemen bir atık toplama döngüsü gerçekleştirmek Mono için beklemek yerine bellek referansı serbest bırakmak için Dispose öğesini çağırması gerekir.

Dahili olarak, Mono oluşturduğunda [NSString başvuruyor C# dizelerden](~/ios/internals/api-design/nsstring.md), bunları hemen yapmak için atık toplayıcı sahip iş miktarını azaltmak için dispose. Etrafında, hızlı GC uğraşmanız daha az nesne çalıştırın.

#### <a name="when-to-keep-references-to-objects"></a>Nesnelerin referansları tutmak ne zaman

Bir yan-otomatik bellek yönetimi sahip hiçbir başvuru onlara var olduğu sürece GC kullanılmayan nesnelerin kurtulun, etkisidir. Bu bazen şaşırtıcı yan etkileri, örneğin, üst düzey görünümü denetleyicinizi tutmak için yerel bir değişken oluşturun ya da üst düzey pencerenizi ve bu olması, geri kaybolur olabilir.

Başvuru, statik veya örnek değişkenleri nesnelerinizi tutma, Mono sonsuza dek Dispose() yöntemi üzerlerinde çağırır ve nesne başvurusunu serbest bırakır. Bu yalnızca bekleyen başvuru olabileceğinden, Objective-C çalışma zamanı sizin için nesne yok.

## <a name="related-links"></a>İlgili bağlantılar

- [Alanların bağlama](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
