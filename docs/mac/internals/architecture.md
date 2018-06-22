---
title: Xamarin.Mac mimarisi
description: Bu kılavuz, Objective-C en düşük düzeyde için Xamarin.Mac ve ilişkisini araştırır. Derleme, seçiciler, kaydedicilerin, uygulama başlatma ve oluşturucunun gibi kavramlarını açıklar.
ms.prod: xamarin
ms.assetid: 74D1FF57-4F2A-4646-8669-003DE99671D4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: d6d7557fed5ea0ca0719dcbddbda316340645320
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30785562"
---
# <a name="xamarinmac-architecture"></a>Xamarin.Mac mimarisi

_Bu kılavuz, Objective-C en düşük düzeyde için Xamarin.Mac ve ilişkisini araştırır. Derleme, seçiciler, kaydedicilerin, uygulama başlatma ve oluşturucunun gibi kavramlarını açıklar._

## <a name="overview"></a>Genel Bakış

Xamarin.Mac uygulamaları Mono yürütme ortamında çalıştırın ve ardından sadece çalışma zamanında yerel koda derlenmiş zamanında (JIT) olduğu Ara dile (IL) aşağıya doğru derlemek için Xamarin'ın derleyici kullanın. Bu yan yana çalışır Objective-C çalışma zamanı. Her iki çalışma zamanı ortamları UNIX benzeri çekirdek en üstünde, özellikle XNU çalıştırın ve arka plandaki yerel veya yönetilen sisteme erişmek geliştiricilere izin vererek kullanıcı kodu için çeşitli API'leri.

Aşağıdaki diyagramda Bu mimarinin temel bir bakış gösterilir:

[![Mimariyi temel bir bakış gösteren diyagram](architecture-images/mac-arch.png "mimariyi temel bir bakış gösteren diyagram")](architecture-images/mac-arch-large.png#lightbox)

### <a name="native-and-managed-code"></a>Yerel ve yönetilen kod

İçin Xamarin, geliştirirken koşulları *yerel* ve *yönetilen* kodu genellikle kullanılır. Yönetilen kod .NET Framework ortak dil çalışma zamanı tarafından yönetildiği veya Xamarin'ın durumda yürütülmesinin sahip kodudur: Mono çalışma zamanı.

Yerel kod yerel olarak (örneğin, Objective-C veya bir KOL yonga üzerinde bile uygulama nesne AĞACI derlenmiş kod) belirli platformda çalışacak kodudur. Bu kılavuz, ayrıca erişmesini sırasında tam kullanımını bağlamaları kullanımı ile Apple'nın Mac API'leri yapmadan nasıl yönetilen kodunuzu yerel koda derlenmiş ve nasıl Xamarin.Mac uygulama çalışır açıklanmaktadır araştırır. NET'in BCL ve Gelişmiş dil C# gibi.

## <a name="requirements"></a>Gereksinimler

Aşağıdaki Xamarin.Mac macOS uygulamayla geliştirmek için gereklidir:

- Mac çalışan macOS Sierra (10,12) veya daha büyük.
- Xcode en son sürümünü (yüklenen [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12))
- En son sürümünü Xamarin.Mac ve Mac için Visual Studio

Xamarin.Mac ile oluşturulan çalışan Mac uygulamaları aşağıdaki sistem gereksinimleri vardır:

- Mac OS 10.7 veya büyük X çalıştıran bir Mac.

## <a name="compilation"></a>Derleme

Herhangi bir Xamarin platform uygulamasını derlerken Mono C# (veya F #) derleyici çalışacak ve C# ve F # kodunuzu Microsoft Ara dile (MSIL veya IL) derlenir. Xamarin.Mac sonra kullanan bir *sadece anında (JIT)* derleyici gerektiği gibi doğru mimariyi yürütülmesine izin yerel kodu derleme zamanında.

Uygulama Nesne AĞACI derleme kullanan Xamarin.iOS aksine budur. Uygulama Nesne AĞACI derleyici kullanırken, tüm derlemeler ve bunların içindeki tüm yöntemleri derleme zamanında derlenir. JIT ile derleme yürütülen yöntemleri için isteğe bağlı olur.

Xamarin.Mac uygulamalarla Mono genellikle uygulama paketi katıştırılır (ve olarak adlandırılan **katıştırılmış Mono**). Klasik Xamarin.Mac API kullanırken, uygulama yerine kullanarak **sistem Mono**, ancak bu birleşik API'desteklenmiyor. İşletim sisteminde yüklü Mono sistem Mono başvuruyor. Uygulama başlatma üzerinde Xamarin.Mac uygulama bu kullanır.

## <a name="selectors"></a>Seçici

.NET ve getirmek için ihtiyacımız Apple, olmak üzere iki ayrı ekosistemlerini sahibiz Xamarin ile birlikte olabildiğince, son hedef kesintisiz kullanıcı deneyimi olduğundan emin olmak kolaylaştırılmış göründüğü için. İki çalışma zamanları nasıl iletişim kuracağını yukarıda bölümünde anlatıldığı ve, çok iyi Xamarin kullanılacak yerel Mac API'ler sağlar 'bağlamaları' koşulunun duymuş olabilirsiniz. Bağlamaları ayrıntılı olarak açıklanmıştır [Objective-C bağlama belgelerine](~/mac/platform/binding.md), bu nedenle, şimdilik Xamarin.Mac başlık altında işleyişi inceleyelim.

İlk olarak, var. sahip Objective-C seçiciler yapılır C# kullanıma sunmak için bir yol olmalıdır. Bir seçici bir nesne veya sınıf gönderilen bir iletidir. Objective-C ile bu aracılığıyla yapılır [objc_msgSend](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ObjCRuntimeRef/index.html) işlevleri. Seçici kullanma hakkında daha fazla bilgi için iOS başvuran [Objective-C seçiciler](~/ios/internals/objective-c-selectors.md) Kılavuzu. Orada da sahiptir Objective-C yönetilen kodu hakkında herhangi bir şey bilmiyor due için daha karmaşık olabilir Objective-C, yönetilen koda kullanıma sunmak için bir yol olmalıdır. Bu sorunu almak için kullanırız bir [kayıt](~/mac/internals/registrar.md). Bu, sonraki bölümde daha ayrıntılı açıklanmıştır.

## <a name="registrar"></a>Kaydedici

Yukarıda belirtildiği gibi kayıt şirketi olan kod bu çıkarır yönetilen kod Objective-c Bunu NSObject türetilen her yönetilen sınıf listesi oluşturarak yapar:

- Varolan bir Objective-C sınıfı sarmalama değil tüm sınıflar için yeni bir Objective-C sınıf sahip tüm yönetilen Üyeler yansıtma Objective-C üyeleriyle oluşturur bir `[Export]` özniteliği.
- Her Objective-C üyesi için uygulamalarında kod yansıtılmış yönetilen üye çağırmak için otomatik olarak eklenir.

Aşağıdaki sözde kod bunun nasıl yapıldığına dair bir örnek göstermektedir:

**C# (yönetilen kod):**

```csharp
class MyViewController : UIViewController{
    [Export ("myFunc")]
    public void MyFunc ()
    {
    }
 }
 ```

**Objective-C (yerel kod):**

```objc
@interface MyViewController : UIViewController
 - (void)myFunc;
@end 

@implementation MyViewController
- (void)myFunc {
    // Code to call the managed C# MyFunc method in MyViewController
}
@end
```

Yönetilen kod özniteliklerini içerebilir `[Register]` ve `[Export]`, nesne Objective-c açığa çıkarılması gerektiğini bilmek kayıt şirketi kullanan [Kayıt] özniteliği oluşturulan varsayılan ad uygun olmayan olasılığına oluşturulan Objective-C sınıf adını belirtmek için kullanılır. Tüm sınıflar NSObject türetilen Objective-c ile otomatik olarak kaydedilir Gerekli [verme] özniteliği oluşturulan Objective-C sınıfta kullanılan seçici bir dize içeriyor.

Kaydedicilerin Xamarin.Mac – dinamik ve statik kullanılan iki tür vardır:

- Dinamik kaydedicilerin – tüm Xamarin.Mac yapılar için varsayılan Kaydedici budur. Dinamik kayıt derlemenizi çalışma zamanında içindeki tüm türler kayıt yapar. API Objective-C'ın çalışma zamanı tarafından sağlanan işlevleri kullanarak bunu yapar. Dinamik kayıt şirketi, bu nedenle derleme süresi bir yavaş başlatma, ancak daha hızlı sahiptir. Trampolines, çağrılan yerel işlevler (genellikle C'de), dinamik kaydedicilerin kullanırken yöntemi uygulamaları kullanılır. Bunlar, farklı mimari arasında farklılık gösterir.
- Statik kaydedicilerin – statik kayıt şirketi sonra bir statik kitaplık içine derlenmiş ve yürütülebilir bağlanmış derleme sırasında Objective-C kodunu oluşturur. Bu daha hızlı bir başlangıç için sağlar, ancak derleme sırasında daha uzun sürer.

## <a name="application-launch"></a>Uygulama başlatma

Sistem Mono kullanılan veya Xamarin.Mac başlangıç mantığı bağlı olarak ekli olup olmadığını farklılık gösterir. Kod ve Xamarin.Mac uygulama başlatma için adımları görüntülemek için başvurmak [başlatma üstbilgi](https://github.com/xamarin/xamarin-macios/blob/master/runtime/xamarin/launch.h) xamarin macios ortak depodaki dosyasına.

## <a name="generator"></a>Oluşturucu

Xamarin.Mac her Mac API tanımlarını içerir. Bunlardan birine üzerinde gözatabilirsiniz [MaciOS github deposuna](https://github.com/xamarin/xamarin-macios/tree/master/src). Bu tanımları öznitelikleri arabirimleriyle yanı sıra tüm gerekli yöntemleri ve özellikleri içerir. Örneğin, aşağıdaki kodu: bir NSBox tanımlamak için kullanılan [AppKit ad alanı](https://github.com/xamarin/xamarin-macios/blob/master/src/appkit.cs#L1465-L1526). Bir dizi yöntem ve özellikleri arabirimiyle olduğuna dikkat edin:

```csharp
[BaseType (typeof (NSView))]
public interface NSBox {

        …

        [Export ("borderRect")]
        CGRect BorderRect { get; }

        [Export ("titleRect")]
        CGRect TitleRect { get; }

        [Export ("titleCell")]
        NSObject TitleCell { get; }

        [Export ("sizeToFit")]
        void SizeToFit ();

        [Export ("contentViewMargins")]
        CGSize ContentViewMargins { get; set; }

        [Export ("setFrameFromContentFrame:")]
        void SetFrameFromContentFrame (CGRect contentFrame);

        …

}
```

Adlı Oluşturucu `bmac` Xamarin.Mac içinde bu tanım dosyalarını alır ve geçici bir derlemeye derlemek için .NET araçları kullanır. Ancak, bu geçici derleme Objective-C kodunu çağırmak için kullanışlı değildir. Oluşturucunun geçici derleme okur ve çalışma zamanında kullanılan C# kod oluşturur. Bu, rastgele bir öznitelik tanımı .cs dosyanıza ekleyin, neden, örneğin, bunu outputted kodda görünmez olur. Oluşturucunun bu konuda bilmiyor ve bu nedenle `bmac` Geçici derlemede çıktısını aramanız için bilmiyor.

Paketleyici Xamarin.Mac.dll oluşturulduktan sonra `mmp`, tüm bileşenler birlikte pakete ekler.

Yüksek bir düzeyde, bunu aşağıdaki görevleri yürüterek uygular:

- Bir uygulama paket yapısı oluşturun.
- Yönetilen derlemeleriniz kopyalayın.
- Bağlantı etkinse, kullanılmayan bölümleri kaldırarak derlemeleriniz iyileştirmek için yönetilen bağlayıcı çalıştırın.
- Statik modundaysa registar kodunda birlikte hakkında açıklandı Başlatıcısı kodda bağlama bir Başlatıcısı uygulaması oluşturun.

Kullanıcı bir parçası olarak çalıştır yapı ardından kullanıcı kodu bir bütünleştirilmiş koda derler işlemi bu referansının Xamarin.Mac.dll ve çalışır budur `mmp` bir paket yapmak için

Bağlayıcı ve nasıl kullanıldığı hakkında daha ayrıntılı bilgi için iOS başvuran [bağlayıcı](~/ios/deploy-test/linker.md) Kılavuzu.

## <a name="summary"></a>Özet

Bu kılavuz Xamarin.Mac uygulamalar, keşfedilen Xamarin.Mac ve Objective-c ilişkisini derleme sırasında arama
