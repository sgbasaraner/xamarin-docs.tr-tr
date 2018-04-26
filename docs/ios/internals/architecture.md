---
title: iOS mimarisi
description: En düşük düzeyde Xamarin.iOS keşfetme
ms.prod: xamarin
ms.assetid: F40F2275-17DA-4B4D-9678-618FF25C6803
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 85dc675a9b18b974f21532298e4d3028bdecd0b7
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2018
---
# <a name="ios-architecture"></a>iOS mimarisi

Xamarin.iOS uygulamaları Mono yürütme ortamında çalıştırın ve ARM assembler dili için C# kodu derlemek için tam şimdi, zaman Tıkların derleme kullanın. Bu yan yana çalışır ile [Objective-C çalışma zamanı](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Her iki çalışma zamanı ortamları UNIX benzeri çekirdek üstünde özellikle çalıştırmak [XNU](https://en.wikipedia.org/wiki/XNU)ve arka plandaki yerel veya yönetilen sisteme erişmek geliştiricilere izin vererek kullanıcı kodu için çeşitli API'leri.

Aşağıdaki diyagramda Bu mimarinin temel bir bakış gösterilir:

[ ![](architecture-images/ios-arch-small.png "Şimdi, zaman Tıkların derleme mimarisi temel bir bakış Bu diyagramda gösterilmektedir")](architecture-images/ios-arch.png#lightbox)

## <a name="native-and-managed-code-an-explanation"></a>Yerel ve yönetilen kod: bir açıklama

İçin Xamarin geliştirirken koşulları *yerel ve yönetilen* kodu genellikle kullanılır. [Yönetilen kod](https://blogs.msdn.microsoft.com/brada/2004/01/09/what-is-managed-code/) tarafından yönetilen yürütülmesinin sahip kodu [.NET Framework ortak dil çalışma zamanı](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx), veya Xamarin'ın büyük-küçük harf: Mono çalışma zamanı. Bir ara dile veririz budur.

Yerel kod yerel olarak (örneğin, Objective-C veya bir KOL yonga üzerinde bile uygulama nesne AĞACI derlenmiş kod) belirli platformda çalışacak kodudur. Bu kılavuz nasıl uygulama nesne AĞACI, yönetilen kodu yerel koda derlenen inceler ve bir Xamarin.iOS uygulaması nasıl çalışır, erişimi de sahip sırasında tam kullanımını bağlamaları kullanımı ile Apple'nın iOS API yapmadan açıklanmaktadır. NET'in BCL ve Gelişmiş dil C# gibi.


## <a name="aot"></a>UYGULAMA NESNE AĞACI

Herhangi bir Xamarin platform uygulamasını derlerken Mono C# (veya F #) derleyici çalışacak ve C# ve F # kodunuzu Microsoft Ara dili (MSIL içine) derlenir. Bir Xamarin.Android, Xamarin.Mac uygulama veya bile bir Xamarin.iOS uygulaması simulator'da çalıştırıyorsanız [.NET ortak dil çalışma zamanı (CLR)](https://msdn.microsoft.com/library/8bs2ecf4(v=vs.110).aspx) bir sadece anında (JIT) derleyicide kullanarak MSIL derler. Bu yerel koda derlenmiş çalışma zamanında, uygulamanız için doğru mimari çalıştırabilirsiniz.

Ancak, iOS, bir cihazda dinamik olarak üretilen kod yürütmeyi izin vermez Apple tarafından ayarlanan güvenlik sınırlaması yoktur.
Biz bu güvenlik protokolleri uyması emin olmak için Xamarin.iOS yerine şimdi, zaman Tıkların derleyici yönetilen kodu derlemek için kullanır. Bu, isteğe bağlı olarak LLVM ile Apple'nın ARM tabanlı işlemci üzerinde dağıtılabilir, cihazlar için en iyi duruma getirilmiş ikili, bir yerel iOS oluşturur. Bu birlikte nasıl uyduğunu kaba diyagramı aşağıda gösterilmiştir:

[![](architecture-images/aot.png "Bu birlikte nasıl uyduğunu kaba diyagramı")](architecture-images/aot-large.png#lightbox)

Uygulama Nesne AĞACI kullanarak sahip bir dizi ayrıntıları sınırlama [sınırlamalar](~/ios/internals/limitations.md) Kılavuzu. Ayrıca bazı geliştirmeler JIT başlangıç zamanı ve çeşitli performans iyileştirmelerini azalma aracılığıyla sağlar

Biz nasıl kaynaktan yerel koda derlenmiş kod denedikten, nasıl Xamarin.iOS tam olarak yerel iOS uygulamaları yazma kurmamızı sağlayan görmek için başlık altında göz atalım

## <a name="selectors"></a>Seçici

.NET ve getirmek için ihtiyacımız Apple, olmak üzere iki ayrı ekosistemlerini sahibiz Xamarin ile birlikte olabildiğince, son hedef kesintisiz kullanıcı deneyimi olduğundan emin olmak kolaylaştırılmış göründüğü için. İki çalışma zamanları nasıl iletişim kuracağını yukarıda bölümünde anlatıldığı ve, çok iyi yerel iOS Xamarin kullanılmak üzere API'ler sağlar 'bağlamaları' koşulunun duymuş olabilirsiniz. Bağlamaları ayrıntılı olarak açıklanmıştır bizim [Objective-C bağlama](~/cross-platform/macios/binding/overview.md) belgeleri, bu nedenle şu an için iOS başlık altında işleyişi inceleyelim.

İlk olarak, var. sahip Objective-C seçiciler yapılır C# kullanıma sunmak için bir yol olmalıdır. Bir seçici bir nesne veya sınıf gönderilen bir iletidir. Objective-C ile bu aracılığıyla yapılır [objc_msgSend](~/cross-platform/macios/binding/overview.md) işlevleri.
Seçici kullanma hakkında daha fazla bilgi için başvurmak [Objective-C seçiciler](~/ios/internals/objective-c-selectors.md) Kılavuzu. Orada da sahiptir Objective-C yönetilen kodu hakkında herhangi bir şey bilmiyor due için daha karmaşık olabilir Objective-C, yönetilen koda kullanıma sunmak için bir yol olmalıdır. Bu sorunu almak için kullanırız *kaydedicilerin*. Bu, sonraki bölümde daha ayrıntılı açıklanmıştır.

## <a name="registrars"></a>Kaydedicilerin

Yukarıda belirtildiği gibi kayıt şirketi olan kod bu çıkarır yönetilen kod Objective-c Bunu NSObject türetilen her yönetilen sınıf listesi oluşturarak yapar:

- Varolan bir Objective-C sınıfı sarmalama değil tüm sınıflar için yeni bir Objective-C sınıf sahip tüm yönetilen Üyeler yansıtma Objective-C üyeleriyle oluşturur bir [`Export`] özniteliği.

- Her Objective-C üyesi için uygulamalarında kod yansıtılmış yönetilen üye çağırmak için otomatik olarak eklenir.

Aşağıdaki sözde kod bunun nasıl yapıldığına dair bir örnek göstermektedir:

**C# (yönetilen kod)**

```csharp
 class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
```

**Hedefi C:**

```objectivec
@interface MyViewController : UIViewController { }

    -(void)myFunc;
@end

@implementation MyViewController {}

    -(void) myFunc
    {
        /* code to call the managed MyViewController.MyFunc method */
    }
@end

```

Yönetilen kod özniteliklerini içerebilir `[Register]` ve `[Export]`, nesne Objective-c açığa çıkarılması gerektiğini bilmek kayıt şirketi kullanan
`[Register]` Özniteliği oluşturulan varsayılan ad uygun olmayan olasılığına oluşturulan Objective-C sınıf adını belirtmek için kullanılır. Tüm sınıflar NSObject türetilen Objective-c ile otomatik olarak kaydedilir
Gerekli `[Export]` özniteliği oluşturulan Objective-C sınıfta kullanılan seçici bir dize içeriyor.

Kaydedicilerin Xamarin.iOS – dinamik ve statik kullanılan iki tür vardır:


- **Dinamik kaydedicilerin** – derlemenizi çalışma zamanında içindeki tüm türler kaydını dinamik kayıt yapar. Bunu tarafından sağlanan işlevleri kullanarak yapar [Objective-C'ın çalışma zamanı API](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/). Dinamik kayıt şirketi, bu nedenle derleme süresi bir yavaş başlatma, ancak daha hızlı sahiptir. Bu, iOS simülatörü'nü için varsayılan değerdir. Trampolines, çağrılan yerel işlevler (genellikle C'de), dinamik kaydedicilerin kullanırken yöntemi uygulamaları kullanılır. Bunlar, farklı mimari arasında farklılık gösterir.

- **Statik kaydedicilerin** – statik kayıt şirketi sonra bir statik kitaplık içine derlenmiş ve yürütülebilir bağlanmış derleme sırasında Objective-C kodunu oluşturur. Bu daha hızlı bir başlangıç için sağlar, ancak derleme sırasında daha uzun sürer. Cihaz derlemeler için bu varsayılan olarak kullanılır. Statik kayıt de ile iOS simülatörü geçirerek kullanılabilir `--registrar:static` olarak bir `mtouch` aşağıda gösterildiği gibi projenizin yapı Seçenekler'de, öznitelik:

    [![](architecture-images/image1.png "Ayar ek mtouch bağımsız değişkenleri")](architecture-images/image1.png#lightbox)

Xamarin.iOS tarafından kullanılan türü kayıt sistemi iOS özellikleri hakkında daha fazla bilgi için bkz [türü kayıt](~/ios/internals/registrar.md) Kılavuzu.

## <a name="application-launch"></a>Uygulama başlatma

Tüm Xamarin.iOS yürütülebilir dosyaları, giriş noktası olarak adlandırılan bir işlev tarafından sağlanan `xamarin_main`, mono başlatır.

Proje türüne bağlı olarak, aşağıdakiler meydana gelir:

- Normal iOS ve tvOS uygulamaları için Xamarin uygulama tarafından sağlanan yönetilen ana yöntemi çağrılır. Bu ana yöntemi yönetilen sonra çağırır `UIApplication.Main`, Objective-c için giriş noktası olduğu UIApplication.Main olan Objective-C'ın için bağlama `UIApplicationMain` yöntemi.
- Uzantılar, yerel işlevi – `NSExtensionMain` veya (`NSExtensionmain` WatchOS uzantıları için) – Apple'nın kitaplıkları çağrıldığında sağlanan. Bu projeleri sınıf kitaplıkları ve değil yürütülebilir projeleri olduğundan, yürütmek için yönetilen bir ana yöntem vardır.

Tüm bu başlatma sırası zemin alma, uygulamanızın bilmesi için son yürütülebilir dosyanın içine sonra bağlantılı bir statik kitaplık, derlenir.

Uygulamamıza bu noktada başlatıldı, Mono çalıştığından, yönetilen kodda duyuyoruz ve yerel kod çağırabilir ve geri çağrılması nasıl biliyoruz. Yapmamız gereken sonraki gerçekte denetimleri ekleme başlatmak ve uygulama etkileşimli yapmak için şeydir.


## <a name="generator"></a>Oluşturucu

Xamarin.iOS her tek iOS API için tanımları içerir. Bunlardan birine üzerinde gözatabilirsiniz [MaciOS github deposuna](https://github.com/xamarin/xamarin-macios/tree/master/src). Bu tanımları öznitelikleri arabirimleriyle yanı sıra tüm gerekli yöntemleri ve özellikleri içerir. Örneğin, aşağıdaki kodu: bir UIToolbar Uıkit tanımlamak için kullanılan [ad alanı](https://github.com/xamarin/xamarin-macios/blob/master/src/uikit.cs#L11277-L11327). Bir dizi yöntem ve özellikleri arabirimiyle olduğuna dikkat edin:

```csharp
[BaseType (typeof (UIView))]
public interface UIToolbar : UIBarPositioning {
    [Export ("initWithFrame:")]
    IntPtr Constructor (CGRect frame);

    [Export ("barStyle")]
    UIBarStyle BarStyle { get; set; }

    [Export ("items", ArgumentSemantic.Copy)][NullAllowed]
    UIBarButtonItem [] Items { get; set; }

    [Export ("translucent", ArgumentSemantic.Assign)]
    bool Translucent { [Bind ("isTranslucent")] get; set; }

    // done manually so we can keep this "in sync" with 'Items' property
    //[Export ("setItems:animated:")][PostGet ("Items")]
    //void SetItems (UIBarButtonItem [] items, bool animated);

    [Since (5,0)]
    [Export ("setBackgroundImage:forToolbarPosition:barMetrics:")]
    [Appearance]
    void SetBackgroundImage ([NullAllowed] UIImage backgroundImage, UIToolbarPosition position, UIBarMetrics barMetrics);

    [Since (5,0)]
    [Export ("backgroundImageForToolbarPosition:barMetrics:")]
    [Appearance]
    UIImage GetBackgroundImage (UIToolbarPosition position, UIBarMetrics barMetrics);

    ...
}
```

Adlı Oluşturucu [ `btouch` ](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs) Xamarin.iOS içinde bu tanım dosyalarını alır ve .NET araçları kullanır [geçici bir derlemeye derleme](https://github.com/xamarin/xamarin-macios/blob/master/src/btouch.cs#L318). Ancak, bu geçici derleme Objective-C kodunu çağırmak için kullanışlı değildir. Oluşturucunun geçici derleme okur ve çalışma zamanında kullanılan C# kod oluşturur.
Bu, rastgele bir öznitelik tanımı .cs dosyanıza ekleyin, neden, örneğin, bunu outputted kodda görünmez olur. Oluşturucunun bu konuda bilmiyor ve bu nedenle `btouch` Geçici derlemede çıktısını aramanız için bilmiyor.

Xamarin.iOS.dll oluşturulduktan sonra mtouch tüm bileşenleri birlikte paket.

Yüksek bir düzeyde, bunu aşağıdaki görevleri yürüterek uygular:

-   Bir uygulama paket yapısı oluşturun.
-   Yönetilen derlemeleriniz kopyalayın.
-   Bağlama etkinleştirilirse çıkışı kullanılmayan parçaları kopyalama tarafından derlemeleriniz iyileştirmek için yönetilen bağlayıcı çalıştırın.
-   Uygulama Nesne AĞACI derleme.
-   Yerel çalıştırılabilir dosyaya bağlı statik kitaplıklarından (her derleme için bir tane) bir dizi çıkarır ve böylece Başlatıcısı kod, kayıt kodu (statik varsa) ve AĞACI tüm çıkışlarından yerel yürütülebilir oluşan bir yerel yürütülebilir oluşturma Derleyici


Bağlayıcı ve nasıl kullanıldığı hakkında daha ayrıntılı bilgi için başvurmak [bağlayıcı](~/ios/deploy-test/linker.md) Kılavuzu.

## <a name="summary"></a>Özet

Xamarin.iOS uygulamaları, keşfedilen Xamarin.iOS ve Objective-C ilişkisini derinlemesine Uygulama Nesne AĞACI derleme, bu kılavuzda arama.

## <a name="related-links"></a>İlgili bağlantılar

- [Sınırlamalar](~/ios/internals/limitations.md)
- [Objective-C’yi Bağlama](~/cross-platform/macios/binding/overview.md)
- [Objective-C seçiciler](~/ios/internals/objective-c-selectors.md)
- [Kayıt türü](~/ios/internals/registrar.md)
- [Bağlayıcı](~/ios/deploy-test/linker.md)
