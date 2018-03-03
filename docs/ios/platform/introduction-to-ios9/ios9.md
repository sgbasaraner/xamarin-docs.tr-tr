---
title: iOS 9 uyumluluk
description: "Hemen uygulamanıza iOS 9 özellikleri eklemeyi planladığınız yok olsa bile, uygulamalarınızı en son sürümünü Xamarin ile yeniden oluşturmalısınız."
ms.topic: article
ms.prod: xamarin
ms.assetid: 69A05B0E-8A0A-489F-8165-B10AC46FAF3C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: bfdf0c73226eec472eb671d5543f5ce124919ab8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-9-compatibility"></a>iOS 9 uyumluluk

_Hemen uygulamanıza iOS 9 özellikleri eklemeyi planladığınız yok olsa bile, uygulamalarınızı en son sürümünü Xamarin ile yeniden oluşturmalısınız._

> [!IMPORTANT]
> **Not:** bu bilgileri, bu sayfada zaten iOS 9 uyumluluk güncelleştirmeleri göndermedi uygulamaları iOS 8 veya daha önceki hedefleme uygulama Mağazası'nda zaten sahip müşteriler içindir. En son sürümleri - zaten kullanıyorsanız, Xcode 7 ve Xamarin.iOS 9 - uygulama geliştirme için lütfen ziyaret [iOS 9 giriş](~/ios/platform/introduction-to-ios9/index.md).

İlk iOS 9 Beta görüntülendiğinde iki sorunları eski uygulamaları iOS 9 başlatmak erişememe bildirilen Xamarin daha eski sürümleriyle belirledik.

Bu iki sorunları (olarak [forumlarımıza üzerinde ayrıntılı](http://forums.xamarin.com/discussion/comment/131529/#Comment_131529)) olan:

- Uygulamalar oluşturmak için iOS 8 veya daha önceki 32-bit aygıtlarda başlatmak yazdıramama (ile oluşturulmuş uygulamalara dahil olmak üzere [Unified API](~/cross-platform/macios/unified/index.md)).
- P/Invoke tam yoluna sahip başarısız belirtilmedi.

Xamarin yüklemenizi en son kararlı kanal yayın ve ardından yeniden oluşturma ve uygulamalarınızı dağıtarak için güncelleştirme bu iki sorunu giderir.

_Xamarin en son sürümü ile yeniden oluşturun ve uygulama Mağazası'na yeniden göndermek uygulamanızı iOS 9 özelliklerle hemen güncelleştirmek planlama olmayan olsa bile, öneririz_.



Bu, müşterilerinizin yükselttikten sonra uygulamanızı iOS 9 çalışacağını garanti eder.
İOS 8 - desteklemeye devam edebilirsiniz en son sürümüyle yeniden oluşturma uygulama hedef sürümü etkilemez.

İOS 9 mevcut uygulamalarınızı test ederken başka ilgili sorununuz olursa, okuma [artırma Uyumluluk](#compat) bölümüne bakın.


### <a name="updating-with-visual-studio"></a>Visual Studio ile güncelleştirme

Visual Studio için en yeni kararlı sürüm güncelleştirilir açıkça denetlemesi önerilir.

## <a name="what-about-components-nugets-and-other-libraries"></a>Bileşenleri, Nugets ve diğer kitaplıkları nasıldır?

Bunu **değil** tüm bileşenleri veya yukarıdaki iki sorunlarını gidermek üzere kullandığınız Nugets yeni sürümlerini beklemeniz gerekir.
Yalnızca uygulamanızı Xamarin.iOS en son kararlı sürümü ile yeniden oluşturarak bu sorunlar giderildi.

Benzer şekilde, bileşen satıcılar ve Nuget yazarlar olan **değil** yalnızca yukarıda belirtilen iki sorunlarını gidermek için yeni yapılar göndermek için gerekli. Ancak, varsa bir bileşeni ya da Nuget kullanır `UICollectionView` veya yük görünümleri **Xib** dosyaları, bir güncelleştirme *olabilir* aşağıda belirtilen iOS 9 uyumluluk sorunlarını gidermek üzere gerekli olabilir.


<a name="compat" />

## <a name="improving-compatibility-in-your-code"></a>Kodunuzda uyumluluğunu artırma

Olduğu bazı durumlar kod düzenleri *kullanılan* iOS 9 çiğnemekten iOS eski sürümleri çalışmaya. Bazı olası sorunları (ve bunların çözümleri) İşte iOS 9 sınarken ortaya:

### <a name="uicollectionviewcellcontentview-is-null-in-constructors"></a>UICollectionViewCell.ContentView is null in constructors

**Neden:** iOS 9 içinde `initWithFrame:` Oluşturucusu olan artık iOS 9 davranış değişiklikleri nedeniyle gerekli [UICollectionView belgelerine durumları](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UICollectionView_class/#//apple_ref/occ/instm/UICollectionView/dequeueReusableCellWithReuseIdentifier:forIndexPath). Belirtilen tanımlayıcı için bir sınıf kayıtlı ve yeni bir hücreye oluşturulmalıdır, hücre şimdi çağrılarak başlatılır, `initWithFrame:` yöntemi.

**Düzeltme:** Ekle `initWithFrame:` Oluşturucusu şuna benzer:

```csharp
[Export ("initWithFrame:")]
public YourCellClassName (CGRect frame) : base (frame)
{
    Initialize (); // refactor initialize code into a method
}
```

İlgili örnekleri: [MotionGraph](https://github.com/xamarin/monotouch-samples/commit/3c1b7a4170c001e7290db9babb2b7a6dddeb8bcb), [TextKitDemo](https://github.com/xamarin/monotouch-samples/commit/23ea01b37326963b5ebf68bbcc1edd51c66a28d6)



### <a name="uiview-fails-to-init-with-coder-when-loading-a-view-from-a-xibnib"></a>Bir görünüm Xib/Nib yüklerken Kodlayıcı ile başlatma için UIView başarısız

**Neden:** `initWithCoder:` Oluşturucusu olan bir görünümü bir arabirim Oluşturucu Xib dosyasından yüklenirken adlı bir. Bu oluşturucu değil verildiyse, yönetilmeyen kod yönetilen bizim sürümü çağrılamaz. Daha önce (ör.) iOS 8 içinde) `IntPtr` Oluşturucusu görünüm başlatmak için çağrıldığı.

**Düzeltme:** oluşturma ve verme `initWithCoder:` Oluşturucusu şuna benzer:

```csharp
[Export ("initWithCoder:")]
public YourClassName (NSCoder coder) : base (coder)
{
    Initialize (); // refactor initialize code into a method
}
```

İlgili örnek: [sohbet](https://github.com/xamarin/monotouch-samples/commit/7b81138d52e5f3f1aa3769fcb08f46122e9b6a88)


### <a name="dyld-message-no-cache-image-with-name"></a>Dyld iletisi: hiçbir önbellek ada sahip bir görüntü...

Günlükteki şu bilgileri içeren bir kilitlenme karşılaşabilirsiniz:

```csharp
Dyld Error Message:
Dyld Message: no cache image with name (/System/Library/PrivateFrameworks/JavaScriptCore.framework/JavaScriptCore)
```

**Neden:** bu özel framework ortak yaptıklarında gerçekleşen Apple'nın yerel bağlayıcı, bir hata olduğunu (JavaScriptCore yapıldı iOS 7, ortak önce özel bir çerçeve olan), ve uygulamanın dağıtım hedefi için bir iOS sürümü olduğunda Framework'te özel. Bu durumda Apple'nın bağlayıcı çerçevesi ortak sürümü yerine özel sürümü ile bağlantı içerir.

**Düzeltme:** bu iOS 9 ele alınacaktır ancak uygulayabileceğiniz kendiniz zarfında kolay bir geçici çözüm yoktur: yalnızca (deneyebilirsiniz iOS 7 bu durumda) sonraki bir iOS sürümünü projenizdeki hedefleyin. Diğer çerçeveler benzer sorunlar sergileyen, örneğin da WebKit framework iOS 8 genel yapıldı (ve iOS 7 hedefleme bu hata; neden olacak şekilde iOS 8 uygulamanızda WebKit kullanmasını hedeflemelidir).



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 uyumluluk sürüm bilgisi](https://releases.xamarin.com/ios-hotfix-for-ios-9-preview-xcode-6/)
- [İOS 9 giriş](~/ios/platform/introduction-to-ios9/index.md)
- [Xamarin.iOS uygulamaları iOS9 (video) güncelleştiriliyor](https://university.xamarin.com/lightninglectures/Updating-your-XamariniOS-apps-to-iOS9)
