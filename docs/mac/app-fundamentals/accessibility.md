---
title: "MacOS erişilebilirliği"
description: "Bu kılavuzda erişilebilir Xamarin.Mac uygulama oluşturmaya yönelik özellikler açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: D7F4892B-501A-4271-A7E0-BDD1586B63AD
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 0117364f02302add1f8788de1a79e4c4210fd07b
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="accessibility-on-macos"></a>MacOS erişilebilirliği

Bu sayfayı göre uygulamalar oluşturmak için erişilebilirlik API'leri macOS kullanmayı açıklar [erişilebilirlik denetim listesi](~/cross-platform/app-fundamentals/accessibility.md).
Başvurmak [Android erişilebilirlik](~/android/app-fundamentals/accessibility.md) ve [iOS erişilebilirlik](~/ios/app-fundamentals/accessibility.md) diğer platform API'leri için sayfa.

Erişilebilirlik API'leri ilk gözden geçirme (OS X adıysa), macOS içinde nasıl anlamak için [OS X erişilebilirlik modeli](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXmodel.html).

## <a name="describing-ui-elements"></a>Kullanıcı Arabirimi öğeleri açıklayan

AppKit kullanan `NSAccessibility` kullanıcı arabirimi tarafından erişilebilir yardım API'leri için protokol. Bu düğmenin ayarlama gibi erişilebilirlik özellikleri için anlamlı değerleri ayarlamaya çalışır varsayılan davranışı içerir `AccessibilityLabel`. Genellikle bir tek sözcük veya tümcecik denetim veya Görünüm açıklayan etiketidir.

### <a name="storyboard-files"></a>Film şeridi dosyaları

Xamarin.Mac Xcode arabirimi Oluşturucu film şeridi dosyalarını düzenlemek için kullanır.
Erişilebilirlik bilgilerini düzenlenebilir **kimlik denetçisi** bir Denetim seçildiğinde tasarım yüzeyine (aşağıdaki ekran görüntüsünde gösterildiği gibi):

[![Xcode'nın arabirimi Oluşturucu'da erişilebilirlik ekleme](accessibility-images/xcode.png "Xcode'nın arabirimi Oluşturucu'da erişilebilirlik ekleme")](accessibility-images/xcode-large.png)

### <a name="code"></a>Kod

Xamarin.Mac şu anda uygulamaz olarak `AccessibilityLabel` ayarlama.  Erişilebilirlik etiket ayarlamak için aşağıdaki yardımcı yöntemi ekleyin:

```csharp
public static class AccessibilityHelper
{
    [System.Runtime.InteropServices.DllImport (ObjCRuntime.Constants.ObjectiveCLibrary)]
    extern static void objc_msgSend (IntPtr handle, IntPtr selector, IntPtr label);

    static public void SetAccessibilityLabel (this NSView view, string value)
    {
        objc_msgSend (view.Handle, new ObjCRuntime.Selector ("setAccessibilityLabel:").Handle, new NSString (value).Handle);
    }
}
```

Bu yöntem gösterildiği gibi kodda sonra kullanılabilir:

```csharp
AccessibilityHelper.SetAccessibilityLabel (someButton, "New Accessible Description");
```

`AccessibilityHelp` Denetimi veya Görünüm yaptığı, bir açıklama için bir özelliktir ve etiket yeterli bilgileri sağlamayabilir zaman eklenmelidir. Belge örnek "siler için" Yardım metni hala mümkün olduğu kadar kısa tutulmalıdır.

Bazı kullanıcı arabirimi öğeleri (örneğin, kendi erişilebilirlik etiket ve Yardım sahip bir giriş yanındaki etiketi) erişilebilir erişim için ilgili değildir.
Bu gibi durumlarda ayarlamak `AccessibilityElement = false` böylece bu denetimleri veya görünümler ekran okuyucular veya diğer erişilebilirlik araçları tarafından atlanacak.

Apple sağlar [erişilebilirlik yönergelerini](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/EnhancingtheAccessibilityofStandardAppKitControls.html) erişilebilirlik etiketleri ve Yardım metni için en iyi uygulamaları açıklar.

## <a name="custom-controls"></a>Özel denetimler

Apple için başvuruda [erişilebilir özel denetimler için yönergeleri](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/ImplementingAccessibilityforCustomControls.html) gerekli ek adımlar hakkında ayrıntılı bilgi için.

## <a name="testing-accessibility"></a>Erişilebilirlik test etme

macOS sağlayan bir **erişilebilirlik denetçisi** yardımcı erişilebilirlik işlevselliğini test etmek. Inspector Xcode ile dahil edilir.

Bunu başlatıldığında, ilk kez **erişilebilirlik denetçisi** erişilebilirlik aracılığıyla bilgisayarı denetlemek için izin gerektirir:

![Çalıştırma izni isteyen erişilebilirlik denetçisi](accessibility-images/accessibility-inspector-1.png "erişilebilirlik çalıştırma izni isteyen denetçisi")

Değer çizgilerinin ve (sol alt üzerinde gerekirse,) ayarları ekran kilidini açma **erişilebilirlik denetçisi**:

![Erişilebilirlik Inspector'ı etkinleştirmek için ayarları ekranı](accessibility-images/accessibility-inspector-2.png "erişilebilirlik Inspector'ı etkinleştirmek için ayarları ekranı")

Bir kez etkinleştirildikten sonra denetçisi ekran taşınabilir bir kayan bir pencere olarak görünür. Aşağıdaki ekran görüntüsünde yanındaki örnek bir Mac uygulaması çalıştıran denetçisi gösterir. İmleç penceresi taşındıkça denetçisi tüm erişilebilir her denetim özelliklerini görüntüler:

[![Erişilebilirlik denetçisi çalışan örneği](accessibility-images/accessibility-example.png "erişilebilirlik denetçisi örnek çalışıyor")](accessibility-images/accessibility-example-large.png)

Daha fazla bilgi için okuma [OS X Kılavuzu için erişilebilirlik sınama](https://developer.apple.com/library/mac/documentation/Accessibility/Conceptual/AccessibilityMacOSX/OSXAXTestingApps.html).



## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası erişilebilirlik](~/cross-platform/app-fundamentals/accessibility.md)
- [Mac erişilebilirlik](https://www.apple.com/accessibility/mac/)
