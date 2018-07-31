---
title: iOS 9 uyumluluğu
description: Hemen, uygulamanızın iOS 9 özellikleri eklemeyi planladığınız yoksa bile uygulamalarınızın Xamarin en son sürümü ile yeniden oluşturmalısınız.
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 2f22fdeaad1b276bb94d2b1ee5af4a6c24d22cb7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350787"
---
# <a name="ios-9-compatibility"></a>iOS 9 uyumluluğu

_Hemen, uygulamanızın iOS 9 özellikleri eklemeyi planladığınız yoksa bile uygulamalarınızın Xamarin en son sürümü ile yeniden oluşturmalısınız._

> [!IMPORTANT]
> Bu sayfadaki bilgileri App Store uygulamalarında zaten sahip iOS 8'i hedefleyen müşteriler için ya da daha önce kimin zaten iOS 9 uyumluluğu güncelleştirmeleri göndermedi. Xcode 7 ve Xamarin.iOS 9 - uygulama geliştirme için en son sürümler - kullanıyorsanız Lütfen ziyaret [iOS 9 giriş](~/ios/platform/introduction-to-ios9/index.md).

İlk iOS 9 betalar özelliğiyken iki sorun eski uygulamaları iOS 9 başlatmak çözememesi bildirilen Xamarin uygulamalarının eski sürümleriyle belirledik.

Bu iki sorunları (olarak [forumlarımıza üzerinde ayrıntılı](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) olan:

- İOS 8 veya önceki 32-bit cihazlarda başlatmanız mümkün olmadığı için uygulamalar oluşturun (ile oluşturulan uygulamalar da dahil olmak üzere [birleşik API](~/cross-platform/macios/unified/index.md)).
- P/Invoke tam yoluna sahip başarısız belirtilmedi.

Xamarin yüklemenizin en son kararlı kanal sürümünden yeniden oluşturmayı ve ardından, uygulamalarınızı yeniden dağıtmak için güncelleştiriliyor. Bu iki sorunu giderir.

_Xamarin en son sürümü ile yeniden oluşturun ve App Store için yeniden gönderme uygulamanızın iOS 9 özelliklerle hemen güncelleştirmek planlama olmayan bile öneririz_.



Bu, müşterilerinizin yükselttikten sonra uygulamanızın iOS 9 çalışacağını garanti eder.
İOS 8 - desteklemeye devam edebilirsiniz en son sürümüyle yeniden uygulamanın hedef sürümü etkilemez.

Mevcut uygulamalarınızı iOS 9 test ederken daha fazla sorun varsa, okuma [geliştirme Uyumluluk](#compat) bölümüne bakın.


### <a name="updating-with-visual-studio"></a>Visual Studio ile güncelleştiriliyor

Visual Studio en son kararlı sürüme güncelleştirildiğinden emin açıkça denetlemesi önerilir.

## <a name="what-about-components-nugets-and-other-libraries"></a>Bileşenleri ve Nuget'lerde diğer kitaplıkları hakkında neler diyeceksiniz?

Bunu **değil** tüm bileşenleri veya yukarıdaki iki sorunları ele almak için kullandığınız Nuget'i yeni sürümleri için beklemeniz gerekir.
Yalnızca uygulamanızı Xamarin.iOS en son kararlı sürümü ile yeniden oluşturarak bu sorunlar düzeltildikten.

Benzer şekilde, Component satıcısı ve Nuget yazarlar olan **değil** yalnızca yukarıdaki iki sorun gidermek için yeni derlemeler göndermek için gerekli. Ancak, varsa bir bileşen veya Nuget kullanır `UICollectionView` veya yük görünümleri **Xıb** dosyaları, bir güncelleştirme *olabilir* aşağıda belirtilen iOS 9 uyumluluğu sorunları ele almak için gerekli.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Kodunuzda uyumluluğu artırma

Bazı durumlarda kod desenleri yoktur *kullanılan* iOS 9 bozucu iOS'ın eski sürümlerinde çalışması için. Burada, bazı olası sorunları (ve çözümlerine) iOS 9 test ederken ortaya:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>Oluşturucularda UICollectionViewCell.ContentView null

**Neden:** iOS 9 `initWithFrame:` oluşturucusu, artık iOS 9 davranış değişiklikleri nedeniyle, gerekli [UICollectionView belgeleri durumları](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Belirtilen tanımlayıcı için bir sınıf kayıtlı ve yeni bir hücre oluşturulmalıdır hücrenin artık çağrılarak başlatılır, `initWithFrame:` yöntemi.

**Düzeltme:** Ekle `initWithFrame:` Oluşturucusu şöyle:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

İlgili örnek: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Uıview init Kodlayıcı ile bir görünüm Xıb/Nib yüklenirken başarısız

**Neden:** `initWithCoder:` Oluşturucusu olan bir görünümü bir arabirim Oluşturucu Xıb dosyasından yüklenirken adlı bir tane. Bu oluşturucu değil dışarı aktardıysanız, yönetilmeyen kod yönetilen bizim sürümünü çağrılamıyor. Daha önce (örn.) iOS 8 içinde) `IntPtr` Oluşturucusu görünümü başlatmak için çağrıldı.

**Düzeltme:** oluşturma ve dışarı aktarma `initWithCoder:` Oluşturucusu şöyle:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

İlgili bir örnek: [sohbet edin](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld ileti: hiçbir önbellek ada sahip bir görüntü...

Bir kilitlenme günlüğünde aşağıdaki bilgilerle karşılaşabilirsiniz:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Neden:** bu özel bir çerçeve genel değişiklik yaptığınızda gerçekleşen Apple'nın yerel bağlayıcı, bir hata olduğunu (JavaScriptCore yapıldığını iOS 7'de, genel önce özel bir çerçeve olan), ve uygulamanın dağıtım hedefi için bir iOS sürümü olduğunda Framework özel. Bu durumda özel genel sürümü yerine framework sürümü ile Apple'nın bağlayıcı bağlayacaksınız.

**Düzeltme:** bu iOS 9 ele alınacaktır, ancak uygulayabileceğiniz kendiniz sırada bir kolayca geçici çözüm: yalnızca hedef (deneyebilirsiniz iOS 7 bu durumda) daha yeni bir iOS sürümü, projenizdeki. Diğer çerçeveler benzer sorunlar ortaya çıkabilir, örneğin da WebKit framework iOS 8'de genel yapıldı (ve iOS 8 WebKit uygulamanızda kullanmak için iOS 7 hedefleyen bu hataya neden olacak şekilde hedeflemesi gereken).



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 uyumluluğu sürüm bilgisi](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [iOS 9’a Giriş](~/ios/platform/introduction-to-ios9/index.md)
- [Xamarin.iOS uygulamalarınızı iOS9 (video) güncelleştiriliyor](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
