---
title: Xamarin.iOS API tasarımı
description: Bu belgede Xamarin.iOS API'leri ve bunları Objective-c ilişkileri oluşturmak için kullanılan temel ilkeler bazıları açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 322D2724-AF27-6FFE-BD21-AA1CFE8C0545
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 275db96435639a60be89e0e3ddb7fa120a30de1c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996418"
---
# <a name="xamarinios-api-design"></a>Xamarin.iOS API tasarımı

' % S'core, Mono bir parçası olan bir temel sınıf kitaplıkları yanı sıra [Xamarin.iOS](http://www.xamarin.com/iOS) çeşitli iOS API'leri geliştiriciler, Mono ile yerel iOS uygulamaları oluşturmak izin vermek için bağlamaları ile birlikte gelir.

Xamarin.iOS setinin için C tabanlı API'ler CoreGraphics gibi iOS bağlamaları yanı sıra Objective-C dünya ile C# dünya arasında köprü görevi gören bir birlikte çalışma altyapısı yoktur ve [OpenGL ES](#OpenGLES).

Objective-C kodu ile iletişim kurmak için düşük düzeyli çalışma zamanı bulunduğu [MonoTouch.ObjCRuntime](#MonoTouch.ObjCRuntime). Bu, bağlamalarda üzerine [Foundation](#MonoTouch.Foundation), CoreFoundation, ve [Uıkit](#MonoTouch.UIKit) sağlanır.

## <a name="design-principles"></a>Tasarım ilkeleri

(Aynı zamanda Xamarin.Mac, macOS üzerinde Objective-C için Mono bağlamaları uygulandıkları) tasarım ilkelerimizde Xamarin.iOS bağlamaları için bazıları şunlardır:

- İzleyin [çerçeve tasarım yönergeleri](https://docs.microsoft.com/dotnet/standard/design-guidelines)
- Objective-C alt sınıflara geliştiriciler izin ver:

  - Varolan bir sınıftan türetilen
  - Zinciri için temel oluşturucu çağrısı
  - C# ' nin geçersiz kılma sistemiyle yöntemleri geçersiz yapılmalıdır
  - Sınıflara C# Standart yapıları ile çalışması gerekir

- Objective-C seçicileri geliştiricilerine açığa çıkarmayın
- Rastgele Objective-C kitaplıklarını çağırmak için bir mekanizma sağlar
- Kolay ve sabit Objective-C görevleri olası genel Objective-C görevleri olun
- Objective-C özellikleri C# özellikleri olarak kullanıma sunma
- Türü kesin belirlenmiş bir API kullanıma sunar:

  - Tür güvenliği artırın
  - Çalışma zamanı hataları en aza indirin
  - IDE IntelliSense dönüş türlerinde Al
  - IDE açılan belgelerine sağlar

- IDE içinde araştırma API'leri önerilir:

  - Örneğin, şunun gibi zayıf yazılmış bir dizi gösterme yerine:
    
    ```objc
    NSArray *getViews
    ```
    Bu gibi güçlü bir tür kullanır:
    
    ```csharp
    NSView [] Views { get; set; }
    ```
    
    Visual Studio Mac için API atarken otomatik tamamlama yapabilmenizi sağlar. Bu, tüm yapar `System.Array` döndürülen değeri, kullanılabilir işlemleri ve LINQ to katılmak dönüş değeri sağlar.

- Yerel C# türleri:

  - [`NSString` olur `string`](~/ios/internals/api-design/nsstring.md)
  - Kapatma `int` ve `uint` numaralandırmalar C# sabit listeleri ve C# ile numaralandırmalar olmalıydı parametreleri `[Flags]` öznitelikleri
  - Tür-nötr yerine `NSArray` nesneleri dizileri kesin olarak belirlenmiş diziler olarak kullanıma sunar.
  - Olayları ve bildirimleri için kullanıcılar arasında seçim olanağı sunun:

    - Varsayılan olarak kesin tür belirtilmiş bir sürümü
    - Gelişmiş kullanım örnekleri için zayıf yazılmış bir sürümü

- Objective-C temsilci desenini destekler:

    - C# olay sistemi
    - C# temsilciler kullanıma sunma (lambda ifadeleri, anonim yöntemler ve `System.Delegate`) bloklar olarak Objective-C API'leri

### <a name="assemblies"></a>Derlemeleri

Xamarin.iOS oluşturan derlemeleri içeren *Xamarin.iOS profili*. [Derlemeleri](~/cross-platform/internals/available-assemblies.md) sayfası, daha fazla bilgi içerir.

### <a name="major-namespaces"></a>Birincil ad alanları 

<a name="MonoTouch.ObjCRuntime" />

#### <a name="objcruntime"></a>ObjCRuntime

[ObjCRuntime](https://developer.xamarin.com/api/namespace/ObjCRuntime/) ad alanı, geliştiriciler C# ve Objective-c arasındaki ortamlarınız arasında köprü sağlar
Cocoa # ve Gtk # deneyiminden göre özel olarak iOS için tasarlanan yeni bir bağlama budur.

<a name="MonoTouch.Foundation" />

#### <a name="foundation"></a>Foundation

[Foundation](https://developer.xamarin.com/api/namespace/Foundation/) ad alanı, temel veri türlerini iOS parçası olan Objective-C Foundation framework ile birlikte çalışmak üzere tasarlanan ve nesne yönelimli Objective-c programlama için tabanıdır sağlar

Xamarin.iOS C# dilinde Objective-c sınıflardan hiyerarşisini yansıtır Örneğin, Objective-C temel sınıf [NSObject](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSObject_Class/Reference/Reference.html) C# kullanılabilir [Foundation.NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/).

Birkaç durumda Biz bu ad alanı temel alınan Objective-C Foundation türleri için bağlamaları sağlasa da, temel alınan türler için .NET türlerini eşlediğiniz. Örneğin:

- Uğraşmanızı yerine [NSString](http://developer.apple.com/iphone/library/documentation/Cocoa/Reference/Foundation/Classes/NSString_Class/Reference/NSString.html) ve [NSArray](https://developer.apple.com/library/ios/#documentation/Cocoa/Reference/Foundation/Classes/NSArray_Class/NSArray.html), çalışma zamanı bu C# olarak sunan [dize](xref:System.String)s ve kesin türü belirtilmiş [dizi](xref:System.Array)boyunca s API.

- Çeşitli yardımcı API'leri sunulur burada geliştiriciler üçüncü taraf Objective-C API'leri, diğer iOS API'leri veya Xamarin.iOS tarafından şu anda bağlı değil API'larını bağlamak izin vermek için.

API'leri bağlama hakkında ayrıntılı bilgi için bkz: [Xamarin.iOS bağlaması Oluşturucu](~/cross-platform/macios/binding/binding-types-reference.md) bölümü.


##### <a name="nsobject"></a>NSObject

[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) Objective-C bağlamaları için temel türdür. Xamarin.iOS türleri yansıtma CocoaTouch API'leri iOS türlerinden iki sınıf: C türleri (genellikle başvurulan CoreFoundation türleri gibi) ve Objective-C türleri (bunların tümü NSObject sınıfından türetilir).

Nespravovaným typem yansıtır her tür için aracılığıyla yerel nesne elde etmek üzere mümkün olduğu [işlemek](https://developer.xamarin.com/api/property/Foundation.NSObject.Handle/) özelliği.

Çöp toplama, nesnelerin tümü için Mono sağlar ancak `Foundation.NSObject` uygulayan [System.IDisposable](xref:System.IDisposable) arabirimi. Bu, açıkça herhangi belirli nsobject'in kaynakları çöp toplayıcısının clark'i için beklemek zorunda kalmadan serbest bırakabilirsiniz, anlamına gelir. Ağır NSObjects, örneğin, büyük veri blokları için işaretçiler tutabilir UIImages kullanılırken bu önemlidir.

Türünüz sonlandırma gerçekleştirmeniz gerekiyorsa, geçersiz kılma [NSObject.Dispose(bool) yöntemi](https://developer.xamarin.com/api/type/Foundation.NSObject/%2fM%2fDispose) Dispose parametresi olan "bool disposing", ve kümesi bunu true olarak nedeni kendi Dispose yöntemini çağrılışıysa anlamına gelir, kullanıcı nesne üzerinde açıkça çağrılan Dispose (). Değer false ise, bu, Dispose (bool disposing) yöntemini Sonlandırıcı iş parçacığında Sonlandırıcı çağrılışıysa anlamına gelir. []()


##### <a name="categories"></a>Kategoriler

Xamarin.iOS 8.10 ile başlatmayı Objective-C kategorisi C# ' tan oluşturmak mümkündür.

Bu yapılır kullanarak `Category` öznitelik, öznitelik bağımsız değişkeni olarak genişletmek için türünü belirleme. Aşağıdaki örnek, örneğin NSString genişletilir.

    [Category (typeof (NSString))]

Her kategori yöntemi yöntemleri Objective-C kullanarak dışarı aktarmak için normal bir mekanizma kullanarak `Export` özniteliği:

    [Export ("today")]
    public static string Today ()
    {
        return "Today";
    }

Tüm yönetilen uzantı yöntemleri statik olmalıdır, ancak uzantı yöntemlerini C# için standart sözdizimini kullanarak Objective-C örnek yöntemler oluşturmak mümkündür:

    [Export ("toUpper")]
    public static string ToUpper (this NSString self)
    {
        return self.ToString ().ToUpper ();
    }

ve genişletme yöntemi için ilk bağımsız değişken yöntemi çağrıldı örnek olacaktır.

Tam bir örnek:

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

Bu örnek yerel toUpper örnek yöntemi Objective-c çağrılabilir NSString sınıfı ekler

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

Bunun olduğu kullanışlı bir senaryo için temelinizin sınıflarda kümesinin tamamını bir yöntem eklemektir, örneğin, bu tüm yapacağınız `UIViewController` örnekleri rapor bunlar döndürebilirsiniz:

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

PreserveAttribute bir türü veya bir tür üyesi korumak için – Xamarin.iOS dağıtım aracı – mtouch bildirmek için kullanılan özel bir öznitelik boyutunu küçültmek için uygulamanın ne zaman işlenir aşamasında ' dir.

Uygulama tarafından statik olarak bağlı olmayan her üye tabi olduğu kaldırılır. Bu nedenle, bu öznitelik değil, statik olarak başvurulur, ancak uygulamanız tarafından hala gerekli olduğunu üyeleri işaretlemek için kullanılır.

Örneğin, türleri dinamik olarak oluşturmak, varsayılan oluşturucu, türlerinin korumak isteyebilirsiniz. XML serileştirme kullanırsanız, türlerinizi özelliklerini korumak isteyebilirsiniz.

Bu öznitelik her üyesine bir türün ya da türü uygulayabilirsiniz. Tüm tür korumak istiyorsanız, sözdizimini kullanabilirsiniz. [korumak (AllMembers = true)] türü.

<a name="MonoTouch.UIKit" />

#### <a name="uikit"></a>Uıkit

[Uıkit](https://developer.xamarin.com/api/namespace/UIKit/) ad alanı, bire bir eşleme için C# sınıfları biçiminde CocoaTouch oluşturan tüm kullanıcı Arabirimi bileşenleri içerir. API C# dilinde kullanılan kurallara uymayı değiştirildi.

C# temsilciler ortak işlemler için sağlanır. Bkz: [Temsilciler](#Delegates) bölümünde daha fazla bilgi için.

<a name="OpenGLES" />

#### <a name="opengles"></a>OpenGLES

OpenGLES için biz dağıtmak bir [sürümünde değişiklik](https://developer.xamarin.com/api/namespace/OpenTK/) , [OpenTK](http://www.opentk.com/) API CoreGraphics veri türleri ve yapıları kullanmak için değiştirilmiş olan OpenGL nesne yönelimli bir bağlamaya ek olarak yalnızca gösterme İos'ta kullanılabilir işlevselliği.

OpenGLES 1.1 işlevselliği belgelenen ES11.GL türü aracılığıyla kullanılabilir [burada](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES11.GL/) türü.

OpenGLES 2.0 işlevselliği belgelenen ES20.GL türü aracılığıyla kullanılabilir [burada](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES20.GL/) türü.

OpenGLES 3.0 işlevselliği belgelenen ES30.GL türü aracılığıyla kullanılabilir [burada](https://developer.xamarin.com/api/type/OpenTK.Graphics.ES30.GL/) türü.


### <a name="binding-design"></a>Tasarım bağlama

Xamarin.iOS, yalnızca temel alınan platforma Objective-C bağlama değil. .NET türü ve daha iyi blend C# ve Objective-c dağıtım sistemi genişletir

Yalnızca P/Invoke Windows ve Linux üzerinde yerel kitaplıkları çağırmak için kullanışlı bir araçtır veya IJW desteği üzerinde Windows COM birlikte çalışma için kullanılabilir olarak Xamarin.iOS bağlaması C# nesneleri Objective-C nesnelere desteklemek için çalışma zamanı genişletir.

Bazı bölümleri Xamarin.iOS uygulamalarının profilini oluşturma, ancak geliştiricilerin yardımcı olacak kullanıcılar için gerekli değildir sonraki tartışma şeyler nasıl yapılır ve bunları daha karmaşık uygulamalar oluştururken yardımcı olacak anlayın.



#### <a name="types"></a>Türler

Burada mantıklı yapılan C# türleri, C# evreni için alt düzey Foundation türler yerine sunulur.  Diğer bir deyişle [NSString yerine C# "dize" türü için API kullanan](~/ios/internals/api-design/nsstring.md) ve kesin türü belirtilmiş C# dizilerinin NSArray gösterme yerine kullanır.

Temel alınan genel olarak, Xamarin.iOS ve Xamarin.Mac tasarımında, `NSArray` nesne gösterilmez. Bunun yerine, çalışma zamanı otomatik olarak dönüştürür `NSArray`kesin olarak belirlenmiş diziler bazı s `NSObject` sınıfı. Dolayısıyla, Xamarin.iOS bir NSArray döndürülecek GetViews gibi zayıf yazılmış bir yöntemini kullanıma sunmuyor:

```csharp
NSArray GetViews ();
```

Bunun yerine, bağlama gibi bu türü kesin belirlenmiş bir dönüş değeri kullanıma sunar:

```csharp
UIView [] GetViews ();
```

Birkaç içinde kullanıma sunulan yöntemleri `NSArray`, isteyebileceğiniz kullanmak için köşe durumlarda bir `NSArray` doğrudan, ancak API bağlamasında bunların kullanılması önerilmez.

Buna ek olarak **Klasik API** gösterme yerine `CGRect`, `CGPoint` ve `CGSize` CoreGraphics API'SİNDEN biz olanlar yerini `System.Drawing` uygulamaları `RectangleF`, `PointF`ve `SizeF` yardımcı olan olarak geliştiriciler OpenTK kullanan var olan OpenGL kod korumak. Yeni 64 bit kullanırken **birleşik API**, CoreGraphics API kullanılmalıdır.

<a name="Inheritance" />

#### <a name="inheritance"></a>Devralma

Xamarin.iOS API tasarımı, geliştiricilerin "temel" C# anahtar sözcüğünü kullanarak temel uygulama zincirlemeyi yanı sıra bir türetilmiş sınıfta "override" anahtar sözcüğünü kullanarak bir C# türü, genişletmek şekilde yerel Objective-C türleri genişleten olanak tanır.

Objective-C sistemin zaten Xamarin.iOS kitaplıkları içinde sarmalanmış çünkü bu tasarım, geliştiricilerin ilgilenen Objective-C seçicileri ile kendi geliştirme işleminin bir parçası önlemek olanak tanır.


#### <a name="types-and-interface-builder"></a>Türleri ve arabirim Oluşturucu

Arabirimi Oluşturucu tarafından oluşturulan tür örnekleri .NET sınıfları oluşturduğunuzda, tek bir alan bir oluşturucu sağlamanız gereken `IntPtr` parametresi.
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

Objective-C ve C# word temsilci için farklı anlamları her dilde vardır.

Objective-C dünyanın ve CocoaTouch hakkında çevrimiçi bulduğunuz belgeleri, bir temsilci genellikle bir dizi yöntem yanıtlar bir sınıf örneğidir. Yöntem her zaman zorunlu değildir, olmasına çok benzer bir C# ile arabirim oluşturmak için fark budur.

Bu temsilciler Uıkit ve diğer CocoaTouch API'leri önemli bir rol oynar. Bunlar, çeşitli görevleri gerçekleştirmek için kullanılır:

-  Kodunuzda (C# veya Gtk + olay teslimi benzer) bildirimleri sağlamak için.
-  Veri görselleştirme denetimleri için modeller uygulamak için.
-  Bir denetimin davranışı oluşturmak için.


Programlama düzeni, denetimin davranışını değiştirmek için türetilmiş sınıfları oluşturulmasını en aza indirmek için tasarlanmıştır. Bu çözüm Ruhu benzer ne diğer GUI araç Setleri yıllar içinde yaptığınız için: Gtk'ın sinyalleri, Qt yuvaları, olayları Winforms, WPF/Silverlight etkinlikler ve benzeri. Yüzlerce arabirimleri (her eylem için bir tane) sahip veya çok fazla yöntemleri ihtiyaç uygulama geliştiricilerin gerektirmeden önlemek için isteğe bağlı yöntemi tanımları Objective-C destekler. Bu, tüm yönteminin uygulanmasını gerektiren C# arabirimleri farklıdır.

Objective-C sınıflarda programlama bu düzeni kullanın sınıflar genellikle adlı bir özelliği kullanıma görürsünüz `delegate`, zorunlu arabirimi bölümlerini ve sıfır veya daha fazla isteğe bağlı, bölümleri uygulamak için gerekli olan.

Xamarin.iOS bu temsilcileri bağlamak için üç birbirini dışlayan mekanizmaları sunulur:

1.  [Olayları aracılığıyla](#Via_Events).
2.  [Aracılığıyla kesin türü belirtilmiş bir `Delegate` özelliği](#StrongDelegate)
3.  [Aracılığıyla gevşek yazılmış bir `WeakDelegate` özelliği](#WeakDelegate)

Örneğin, düşünün [UIWebView](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html) sınıfı. Bu şekilde dağıtır bir [UIWebViewDelegate](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html) öğesine atanan örneği [temsilci](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebView_Class/Reference/Reference.html#//apple_ref/occ/instp/UIWebView/delegate) özelliği.

<a name="Via_Events" />

##### <a name="via-events"></a>Olayları

Birçok türü için Xamarin.iOS otomatik olarak yönlendiren uygun bir temsilci oluşturur `UIWebViewDelegate` C# olayları üzerine çağrıları. İçin `UIWebView`:

-  [WebViewDidStartLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidStartLoad:) yöntemi eşlenmiş durumda [UIWebView.LoadStarted](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadStarted/) olay.
-  [WebViewDidFinishLoad](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webViewDidFinishLoad:) yöntemi eşlenmiş durumda [UIWebView.LoadFinished](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadFinished/) olay.
-  [WebView:didFailLoadWithError](http://developer.apple.com/iphone/library/documentation/UIKit/Reference/UIWebViewDelegate_Protocol/Reference/Reference.html#//apple_ref/occ/intfm/UIWebViewDelegate/webView:didFailLoadWithError:) yöntemi eşlenmiş durumda [UIWebView.LoadError](https://developer.xamarin.com/api/event/UIKit.UIWebView.LoadError/) olay.

Örneğin, bu basit bir program web yüklenirken görüntülediğinizde başlangıç ve bitiş zamanlarını kaydeder:

```csharp
DateTime startTime, endTime;
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += (o, e) => startTime = DateTime.Now;
web.LoadFinished += (o, e) => endTime = DateTime.Now;
```


##### <a name="via-properties"></a>Özellikleri aracılığıyla

Olabilir, bu olayın birden fazla aboneye olayları yararlı olur. Ayrıca, olayları çalışmalarına sınırlıdır koddan dönüş değeri olduğu.

Burada bir değer döndürmek için kod beklendiği durumlarda, biz bunun yerine özellikleri için kabul edildi. Başka bir deyişle, bir nesne belirli bir zamanda yalnızca bir yöntem ayarlanabilir.

Örneğin, işleyici için ekranda klavye kapatmak için bu mekanizma kullanabilirsiniz bir `UITextField`:

```csharp
void SetupTextField (UITextField tf)
{
    tf.ShouldReturn = delegate (textfield) {
        textfield.ResignFirstResponder ();
        return true;
    }
}
```

`UITextField`'S `ShouldReturn` özelliği bir Boole değeri döndürür ve TextField basıldığında dönüş düğmeyle şeyler olup olmadığını belirleyen bir temsilci bu durumda bir bağımsız değişken olarak alır. Biz bizim yöntemde dönüş *true* çağırana, ancak biz de klavye ekrandan kaldırmak (textfield çağırdığında böyle `ResignFirstResponder`).

<a name="StrongDelegate"/>

##### <a name="strongly-typed-via-a-delegate-property"></a>Bir temsilci özelliği aracılığıyla kesin

Olayları kullanmayı tercih ederseniz, kendi sağlayabilirsiniz [UIWebViewDelegate](https://developer.xamarin.com/api/type/UIKit.UIWebViewDelegate/) alt atayabilirsiniz [UIWebView.Delegate](https://developer.xamarin.com/api/property/UIKit.UIWebView.Delegate/) özelliği. UIWebView.Delegate atandıktan sonra UIWebView olay gönderim mekanizması artık çalışmaz ve ilgili olaylar meydana geldiğinde UIWebViewDelegate yöntemi çağrılır.

Örneğin, basit bu tür bir web görünümü yüklemek için gereken süreyi kaydeder:

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

Yukarıdaki kod şöyle kullanılır:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.Delegate = new Notifier ();
```

Yukarıdaki bir UIWebViewer oluşturur ve bu bildirim, iletileri cevaplayin oluşturduğumuz bir sınıf örneği iletileri göndermek için yenilemelerini ister.

Bu düzen ayrıca örneğin UIWebView durumda belirli denetimler için davranışını denetlemesini kullanılır [UIWebView.ShouldStartLoad](https://developer.xamarin.com/api/property/UIKit.UIWebView.ShouldStartLoad/) özelliği sağlar `UIWebView` denetim örneği olup olmadığını `UIWebView` yükleyecek bir veya sayfa.

Desen, isteğe bağlı olarak bazı denetimler için veri sağlamak için de kullanılır. Örneğin, [UITableView](https://developer.xamarin.com/api/type/UIKit.UITableView/) denetimin güçlü bir tablo oluşturma denetimi – ve Ara hem içeriği örneği tarafından yönlendirilen bir [UITableViewDataSource](https://developer.xamarin.com/api/type/UIKit.UITableView/DataSource)

<a name="WeakDelegate"/>

### <a name="loosely-typed-via-the-weakdelegate-property"></a>WeakDelegate özelliği aracılığıyla gevşek yazılmış

Kesin türü belirtilmiş özelliği yanı sıra, ayrıca farklı şeyler bağlamak Geliştirici veren zayıf yazılmış temsilci yoktur.
Türü kesin belirlenmiş her yerde `Delegate` özellik sunulmuştur Xamarin.iOS'ın bağlamada, karşılık gelen `WeakDelegate` özelliği de gösterilir.

Kullanırken `WeakDelegate`, düzgün bir şekilde sınıfını kullanarak dekorasyon için sorumlu olduğunuz [dışarı](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) Seçici belirtmek için özniteliği. Örneğin:

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

Bu kez Not `WeakDelegate` özelliği atanmış, `Delegate` özellik kullanılmayacak. Ayrıca, [Excel'e] istediğiniz devralınan bir temel sınıf yöntemi uygularsanız, genel bir yöntem yapmalısınız.


## <a name="mapping-of-the-objective-c-delegate-pattern-to-c35"></a>C Objective-C temsilci desen eşleştirme&#35;

Şuna Objective-C örnekleri görürsünüz:

```csharp
foo.delegate = [[SomethingDelegate] alloc] init]
```

Bu, oluşturmak ve "SomethingDelegate" sınıfının bir örneğini oluşturur ve temsilci özelliği foo değişkeni değer atamak için dili bildirir. Bu mekanizma Xamarin.iOS tarafından desteklenir ve C# sözdizimi:

```csharp
foo.Delegate = new SomethingDelegate ();
```

Objective-C eşleme türü kesin belirlenmiş sınıfları sınıfları temsilci sağlanan Xamarin.ios'ta sahibiz. Bunları kullanmak için sınıflara ve Xamarin.iOS'ın uygulama tarafından tanımlanan yöntemleri geçersiz. Bölüm "" aşağıdaki modelleri nasıl çalıştığı hakkında daha fazla bilgi için bkz.


##### <a name="mapping-delegates-to-c35"></a>C için eşleme temsilciler&#35;

Uıkit Objective-C temsilciler iki biçimde genel kullanır.

İlk form, bileşen modeli için bir arabirim sağlar. Örneğin, bir liste için veri depolama tesisi gibi bir görünüm için isteğe bağlı veri sağlamak için bir mekanizma görüntüleyin.  Bu durumlarda, her zaman uygun sınıfının bir örneğini oluşturmak ve değişkenine atayın.

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

İkinci form, olaylar için bildirim sağlamaktır. Yine de kullanıma sunuyoruz, yukarıda özetlenen biçiminde API olsa da bu gibi durumlarda da hızlı işlemleri için daha basit ve anonim Temsilciler ve lambda ifadelerine C# ile tümleşik olması gereken C# olayları, sunuyoruz.

Örneğin, abone olabileceğiniz `UIAccelerometer` olayları:

```csharp
UIAccelerometer.SharedAccelerometer.Acceleration += (sender, args) => {
   UIAcceleration acc = args.Acceleration;
   Console.WriteLine ("Time={0} at {1},{2},{3}", acc.Time, acc.X, acc.Y, acc.Z);
}
```

İki seçenek, nerede oldukları anlamlı ancak bir programcısı birini seçmelisiniz. C# olayları, kendi türü kesin belirlenmiş bir Yanıtlayıcı/temsilci örneği oluşturun ve atayın, işlevsel olmayacaktır. C# olayları kullanırsanız, Yanıtlayıcı/temsilci sınıfınızdaki yöntemi asla çağrılmaz.

Önceki örnekte kullanılan `UIWebView` böyle C# 3.0 lambdaları kullanarak yazılabilir:

```csharp
var web = new UIWebView (new CGRect (0, 0, 200, 200));
web.LoadStarted += () => { startTime = DateTime.Now; }
web.LoadFinished += () => { endTime = DateTime.Now; }
```


#### <a name="responding-to-events"></a>Olaylarına yanıt verme

Objective-C kodunda bazen birden çok denetimleri ve sağlayıcıları birden fazla denetim için daha fazla bilgi için olay işleyicileri barındırılacak aynı sınıfta. Sınıfları iletilere yanıt verir çünkü ve sınıflar iletileri cevaplayin sürece bu mümkündür, nesneler birbirine bağlamak mümkündür.

Daha önce ayrıntılı olarak Xamarin.iOS hem C# olay tabanlı programlama modelini destekler ve yeni bir sınıf, oluşturabileceğiniz Objective-C temsilci düzeni temsilci uygulayan ve istenen yöntemlerini geçersiz kılar.

Ayrıca birden çok farklı işlemler için Yanıtlayıcılar tüm bir sınıf içinde aynı örneği barındırıldığı Objective-C'ın desenin desteklenmesi mümkündür. Ancak bunun için alt düzey Xamarin.iOS bağlaması özelliklerinin kullanılabilmesi gerekir.

Örneğin, hem de yanıt sınıfınıza istediyseniz `UITextFieldDelegate.textFieldShouldClear`: ileti ve `UIWebViewDelegate.webViewDidStartLoad`: [dışarı aktarma] özniteliği bildirimi kullanılacak olurdu bir sınıfın aynı örneği:

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

C# adları yöntemleri için önemli değildir; önemli [dışarı aktarma] özniteliğine geçirilen dizelerdir.

Bu stil programlama kullanırken, C# parametreleri çalışma zamanı altyapısı geçer gerçek türler eşleştiğinden emin olun.

<a name="Models" />

#### <a name="models"></a>Modeller

Uıkit depolama tesislerdeki veya yardımcı sınıflarını kullanarak uygulanan Yanıtlayıcıları, bunlar genellikle temsilci olarak Objective-C kodu adlandırılır ve protokoller olarak uygulanabilir.

Objective-C protokolleri arabirimdir gibi ancak isteğe bağlı yöntemler destekledikleri: diğer bir deyişle, tüm yöntemleri Protokolü çalışmaya uygulanması gerekiyor.

Bir model kullanmanın iki yolu vardır. El ile uygulayın veya var olan türü kesin belirlenmiş tanımları kullanır.


Xamarin.iOS ile ilişkili olmayan bir sınıf uygulamak çalıştığınızda el ile mekanizması gereklidir. Yapmak oldukça kolaydır:

-  Sınıfınız için çalışma zamanı ile kayıt bayrağı
-  Geçersiz kılmak istediğiniz her metodunda gerçek Seçici adıyla [dışarı aktarma] özniteliğini uygulayın
-  Sınıf örneği oluşturun ve geçirin.

Örneğin, aşağıdaki isteğe bağlı yöntemlerden yalnızca biri UIApplicationDelegate Protokolü tanımında uygulayın:

```csharp
public class MyAppController : NSObject {
        [Export ("applicationDidFinishLaunching:")]
        public void FinishedLaunching (UIApplication app)
        {
                SetupWindow ();
        }
}
```

Objective-C Seçici adına ("applicationDidFinishLaunching:") dışarı aktarma öznitelik ile bildirilmediği ve sınıfa kayıtlı `[Register]` özniteliği.

Xamarin.iOS el ile bağlama gerektirmeyen kesin türü belirtilmiş bildirimleri, kullanıma hazır sağlar. Xamarin.iOS çalışma zamanı bu programlama modelini desteklemek için bir sınıf bildiriminde [Model] özniteliğini destekler. Bu yöntemleri olmadığı sürece bu sınıftaki tüm yöntemlerin'kurmak wire değil çalışma zamanının açıkça gerçekleştirilen bildirir.

Uıkit içinde buna, bir protokol olan isteğe bağlı yöntemleri temsil eden sınıfları aşağıdaki gibi yazılır:

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

Yalnızca bazı yöntemlerini uygulayan bir modeli uygulamak istiyorsanız, yapmanız gereken tek şey ilgilendiğiniz ve diğer yöntemleri yoksay yöntemleri geçersiz kılmak için. Çalışma zamanı üzerine yöntemleri, özgün yöntemleri Objective-C dünya çapında değil yalnızca denetime.

El ile önceki örneğe eşdeğerdir:

```csharp
public class AppController : UIApplicationDelegate {
    public override void FinishedLaunching (UIApplication uia)
    {
     ...
    }
}
```

Objective-C üstbilgi dosyalarına Seçici, bağımsız değişkenler veya C# eşleme türleri bulmak için incelemek gerek yoktur ve IntelliSense Visual Studio'dan Mac için güçlü türleri ile birlikte alırsınız, avantajı


#### <a name="xib-outlets-and-c35"></a>XIB çıkışlar ve C&#35;

> [!IMPORTANT]
> Bu bölümde, çıkışlar IDE tümleştirmesiyle XIB dosyaları kullanırken açıklanmaktadır. İOS için Xamarin Tasarımcısı'nı kullanarak, bu tüm altında bir ad girerek değiştirilir **kimlik > adı** aşağıda gösterildiği gibi IDE özellikleri bölümünde:
>
> [![](images/designeroutlet.png "İOS Designer'daki bir öğe adı girerek")](images/designeroutlet.png#lightbox)
>
>İOS Designer hakkında daha fazla bilgi için lütfen inceleyin [iOS Designer giriş](~/ios/user-interface/designer/introduction.md#how-it-works) belge.

Bu alt düzey açıklaması çıkışlar C# ile tümleştirmek ve Xamarin.iOS İleri düzey kullanıcılar için sağlanır. Ne zaman eşleştirme Mac için Visual Studio kullanarak otomatik olarak kullanarak arka planda gerçekleştirilir uçuş kod sizin için oluşturulur.

Arabirim Oluşturucu, kullanıcı arabirimiyle tasarlarken, uygulamanın görünümünü tasarlama yalnızca ve bazı varsayılan bağlantı kurar. Program aracılığıyla bilgileri getirmek, çalışma zamanında davranışını değiştirmek veya çalışma zamanında denetim değiştirmek istiyorsanız, bazı denetimleri için yönetilen kodunuzun bağlamak gereklidir.

Bu, birkaç adımda gerçekleştirilir:

1.  Ekleme **çıkış bildirimi** için **dosya sahibi**.
1.  Denetiminize bağlanma **dosya sahibi**.
1.  Kullanıcı arabirimini artı XIB/NIB dosyanızı bağlantıları Store.
1.  Çalışma zamanında NIB dosya yükleyin.
1.  Çıkış değişkeni erişin.


(3) adımları (1) arabirim Oluşturucu ile arabirim oluşturmak için Apple belgelerinde ele alınmaktadır.

Xamarin.iOS kullanırken, uygulamanızın Uıviewcontroller türeyen bir sınıf oluşturmanız gerekir. Uygulandığı onu şöyle:

```csharp
public class MyViewController : UIViewController {
    public MyViewController (string nibName, NSBundle bundle) : base (nibName, bundle)
    {
        // You can have as many arguments as you want, but you need to call
        // the base constructor with the provided nibName and bundle.
    }
}
```

Ardından, ViewController NIB dosya yüklemek için bunu yapabilirsiniz:

```csharp
var controller = new MyViewController ("HelloWorld", NSBundle.MainBundle, this);
```

Bu kullanıcı arabirimini NIB yükler. Artık, kuruluşlarının erişmek için bunlara erişmek için istediğimiz çalışma zamanı bildirmek gereklidir. Bunu yapmak için `UIViewController` özellikleri bildirme ve bunları [Connect] özniteliğiyle ek açıklama için gereksinim duyduğu alt sınıfı. Böyle:

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

Özellik gerçekten getirir ve gerçek yerel türü için değer depolayan bir uygulamasıdır.

Visual Studio Mac ve InterfaceBuilder için kullanırken bu konuda endişelenmeniz gerekmez. Mac için Visual Studio, projenizin bir parçası olarak derlenmiş bir kısmi sınıftaki kod ile bildirilen tüm çıkışlar otomatik olarak yansıtır.

#### <a name="selectors"></a>Seçici

Bir çekirdek Objective-C programlama Seçici kavramıdır. Genellikle, bir seçici geçirmek ihtiyaç duyduğunuz veya kodunuz için bir seçici yanıt bekliyor API'leri üzerinden gelir.

C# ' de yeni Seçici oluşturma çok kolay – yeni bir örneğini oluşturmanız yeterlidir `ObjCRuntime.Selector` sınıfı ve bunu gerektiren API'sindeki herhangi bir yerde sonucu kullanın. Örneğin:

```csharp
var selector_add = new Selector ("add:plus:");
```

Bir C# yöntemi yanıt için bir seçici çağrı, öğesinden devralmalıdır `NSObject` türüne ve C# yöntemi gerekir düzenlenmiş seçiciyi kullanarak adı `[Export]` özniteliği. Örneğin:

```csharp
public class MyMath : NSObject {
    [Export ("add:plus:")]
    int Add (int first, int second)
    {
         return first + second;
    }
}
```

Seçicinin Not adları **gerekir** tüm ara ve sondaki iki nokta üst üste dahil olmak üzere tam olarak eşleşen (":"), varsa.

#### <a name="nsobject-constructors"></a>NSObject oluşturucular

Öğesinden türetilen Xamarin.iOS çoğu sınıflarda `NSObject` oluşturucular belirli nesne işlevselliğini açığa çıkarır ancak hemen göze olmayan çeşitli oluşturucular de açığa çıkarır.

Oluşturucular şu şekilde kullanılır:

```csharp
public Foo (IntPtr handle)
```

Bu oluşturucu, çalışma zamanı yönetilmeyen bir sınıfa sınıfınıza eşleme gerektiğinde, sınıfı örneğini oluşturmak için kullanılır. XIB/NIB dosya yüklediğinizde bu gerçekleşir.  Bu noktada, yönetilmeyen dünyanın Objective-C çalışma zamanı nesne oluşturmuş olacaksınız ve yönetilen yan başlatmak için bu oluşturucu çağrılır.

Genellikle, yapmanız gereken tek şey tutamacını parametreli ve gövdesinde temel oluşturucu çağrısı, gerekli olan herhangi bir başlangıç yapın.

```csharp
public Foo ()
```

Bu sınıf için varsayılan oluşturucu ve Xamarin.iOS sağlanmıştır, bu Foundation.NSObject sınıfı ve tüm sınıflar arasında başlatır ve sonunda, bu Objective-C zincir `init` sınıfı yöntemi.

```csharp
public Foo (NSObjectFlag x)
```

Bu oluşturucu örneği başlatmak, ancak kod sonunda Objective-C "başlatma" yöntemini çağırmasını engellemek için kullanılır. Başlatma için zaten kayıtlı olduğunda, genellikle bunu kullanın (kullandığınızda `[Export]` , oluşturucuda) veya bunu zaten yaptığınızda, başlatma başka bir mean aracılığıyla.

```csharp
public Foo (NSCoder coder)
```

Bu oluşturucu, nesne NSCoding örneği burada Başlatılmakta olan durumlar için sağlanır. Daha fazla bilgi için bkz. Apple'nın [arşivler ve Serileştirme programlama kılavuzu.](http://developer.apple.com/mac/library/documentation/Cocoa/Conceptual/Archiving/index.html#//apple_ref/doc/uid/10000047i)

#### <a name="exceptions"></a>Özel Durumlar

Xamarin.iOS API tasarımı, C# özel durumlar olarak Objective-C özel durumları oluşturmaz. Objective-C dünya ilk başta hiçbir çöp gönderilmesini tasarım zorlar ve geçersiz veriler hiç olmadığı kadar önce üretilmesi gereken özel durumları bağlama tarafından üretilen Objective-C dünya geçirildi.

#### <a name="notifications"></a>Bildirimler

İOS ve OS X, geliştiriciler tarafından temel platform yayın Bildirimlere abone olabilirsiniz. Bu kullanılarak yapılır `NSNotificationCenter.DefaultCenter.AddObserver` yöntemi. `AddObserver` Yöntem iki parametre alır, abone olmak istediğiniz bildirim biridir; diğer bildirimi oluştuğunda çağrılacak yöntem budur.

Xamarin.iOS ve Xamarin.Mac çeşitli bildirimler için anahtarları bildirimleri tetikleyen sınıf üzerinde barındırılır. Örneğin, bildirimler harekete geçirilen `UIMenuController` olarak barındırılan `static NSString` özelliklerinde `UIMenuController` adı "Bildirim" ile biten sınıfları.

### <a name="memory-management"></a>Bellek Yönetimi

Xamarin.iOS ele artık kullanımda olduğunda kaynaklar sizin için serbest ilgilendiğiniz atık Toplayıcıya sahiptir. Çöp toplayıcı ek olarak tüm nesneler, türetilen `NSObject` uygulamak `System.IDisposable` arabirimi.

#### <a name="nsobject-and-idisposable"></a>NSObject ve IDisposable

Gösterme `IDisposable` arabirimidir geliştiriciler, büyük boyutlu bellek bloklarını kapsülleyen nesneleri serbest Yardım için kullanışlı bir yol (örneğin, bir `UIImage` yalnızca zararsız bir işaretçi gibi görünebilir, ancak 2 megabayt görüntüye işaret eden ) ve diğer önemli ve sınırlı kaynaklar (örneğin, bir video kod çözme arabellek).

NSObject IDisposable arayüzünü uygular ve ayrıca [.NET Dispose deseni](http://msdn.microsoft.com/library/fs2xkftw.aspx). Bu, geliştiricilerin bu alt NSObject Dispose davranışı geçersiz kılabilir ve isteğe bağlı olarak kendi kaynaklarını serbest bırakmak için sağlar. Örneğin, görüntüleri bir sürü tutar bu görünüm denetleyicisi göz önünde bulundurun:

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

Artık bir yönetilen nesne silindiğinde, kullanışlıdır. Nesnelere başvuru yine de sahip olabilir, ancak nesne için tüm intents ve purposes bu noktada geçersiz. Bazı .NET API'lerini bırakılmış bir nesne üzerinde herhangi bir yöntem örneğin erişmeyi denerseniz bir ObjectDisposedException atma emin olun:

```csharp
var image = UIImage.FromFile ("demo.png");
image.Dispose ();
image.XXX = false;  // this at this point is an invalid operation
```

' % S'değişkeni "image" erişmeye devam edebilirsiniz bile bu gerçekten geçersiz bir başvuru ve artık görüntü tutulan Objective-C nesnesi noktalarına olur.

Ancak, bir nesne C# disposing nesne mutlaka edileceği anlamına gelmez. Bunu olan C# olan nesneye başvuru bırakın. Cocoa ortamı kendi kullanımı için geçici olarak bir başvuru tutmuş mümkündür. Örneğin, görüntüye UIImageView'ın görüntü özelliğini ayarlama ve görüntü dispose sonra temel alınan UIImageView kendi başvurusunu geçen ve işlemi tamamlanana kadar bu nesneye bir başvuru tutar kullanmadan.

#### <a name="when-to-call-dispose"></a>Dispose çağrısı yapıldığında

Mono, nesnenin RID gerektiğinde atmayı çağırmalıdır. Mono, NSObject gerçekten bellek ya da bir bilgi havuzu gibi önemli bir kaynağı bir başvuru tutan bilgisi varsa olası kullanım durumdur. Bu gibi durumlarda, çöp toplama döngüsü gerçekleştirmek Mono için beklemek yerine bellek başvuru hemen yayımlamayı atmayı çağırmalıdır.

Dahili olarak, Mono oluşturduğunda [NSString başvuran C# dizelerden](~/ios/internals/api-design/nsstring.md), hemen çöp toplayıcı yapmaları gereken iş miktarını azaltmak için bunları dispose. Yaklaşık hızlı GC uğraşmanız daha az nesne çalışır.

#### <a name="when-to-keep-references-to-objects"></a>Nesnelere başvurular tutmak ne zaman

Bir yan otomatik bellek yönetimi olan etkisi, bunları başvuru var olduğu sürece GC kullanılmayan nesnelerin kurtulun, ' dir. Bu bazen şaşırtıcı yan etkileri, örneğin, üst düzey görünüm denetleyicinize tutmak için yerel bir değişken oluşturun ya da üst düzey pencerenizin ve bu zorunda arkanızı kaybolur olabilir.

Başvuru, statik veya örnek değişkenleri nesnelerinizi tutma, Mono sonsuza dek Dispose() yöntemini üzerlerinde çağırır ve nesnesine başvuru serbest bırakır. Bu yalnızca bekleyen başvuru olabileceğinden, Objective-C çalışma zamanı, nesne silecektir.

## <a name="related-links"></a>İlgili bağlantılar

- [Alanların bağlama](~/cross-platform/macios/binding/objective-c-libraries.md#Binding_Fields)
