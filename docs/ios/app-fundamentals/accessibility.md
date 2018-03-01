---
title: "İOS erişilebilirlik"
ms.topic: article
ms.prod: xamarin
ms.assetid: 06FD3940-D666-4C9E-BC3E-BBE481EF8012
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/18/2016
ms.openlocfilehash: 28701daca18852be93724ddfe444e1e620788619
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="accessibility-on-ios"></a>İOS erişilebilirlik

Bu sayfayı göre uygulamalar oluşturmak için erişilebilirlik API'leri iOS kullanmayı açıklar [erişilebilirlik denetim listesi](~/cross-platform/app-fundamentals/accessibility.md).
Başvurmak [Android erişilebilirlik](~/android/app-fundamentals/accessibility.md) ve [OS X erişilebilirlik](~/mac/app-fundamentals/accessibility.md) diğer platform API'leri için sayfa.

## <a name="describing-ui-elements"></a>Kullanıcı Arabirimi öğeleri açıklayan

iOS sağlar `AccessibilityLabel` ve `AccessibilityHint` VoiceOver tarafından kullanılabilecek açıklayıcı metin eklemek geliştiricilere yönelik özellikler ekran okuyucusu denetimleri daha erişilebilir hale getirmek için. Denetimler de erişilebilir modda ek bağlam sağlayan bir veya daha fazla nitelikler ile etiketlenebilir.

Bazı denetimler (örneğin, bir metin girişi veya tamamen dekoratif bir resim üzerinde bir etiket için) – erişilebilir olmasını gerekmeyebilir `IsAccessibilityElement` bu durumlarda erişilebilirlik devre dışı bırakmak için sağlanır.

**UI Tasarımcısı**

**Özellikleri paneli** iOS UI Tasarımcısı bir Denetim seçildiğinde düzenlenmesi için bu ayarları sağlayan bir erişilebilirlik bölümü içerir:

![](accessibility-images/ios-designer-sml.png "Erişilebilirlik ayarları")

**C#**

Bu özellikleri de doğrudan kodunda ayarlayabilirsiniz:

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>AccessibilityIdentifier nedir?

`AccessibilityIdentifier` Kullanıcı arabirimi öğeleri UIAutomation API aracılığıyla başvurmak için kullanılan benzersiz bir anahtar ayarlamak için kullanılır.

Değeri `AccessibilityIdentifier` hiçbir zaman konuşulan veya kullanıcıya gösterilir.

<a name="postnotification" />

## <a name="postnotification"></a>PostNotification

`UIAccessibility.PostNotification` Yöntemi (örneğin, belirli bir denetim ile etkileşim sırasında) doğrudan etkileşim dışında kullanıcıya çıkarılmasına olayları sağlar.

### <a name="announcement"></a>Duyuru

Duyuru koddan (bir arka plan işlemi tamamlandı gibi), bazı durumu değişti kullanıcıya bildirmek için gönderilebilir. Bu görsel bir gösterge kullanıcı arabiriminde tarafından eşlik:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>LayoutChanged

`LayoutChanged` Duyuru kullanılır, ekranı düzeni:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```


## <a name="accessibility-and-localization"></a>Erişilebilirlik ve yerelleştirme

Etiket ve ipucu yalnızca yerelleştirilebilir gibi erişilebilirlik özellikleri kullanıcı arabiriminde diğer metin ister.

**MainStoryboard.strings**

Kullanıcı arabirimi bir film şeridi düzenlendiğini, diğer özellikleri aynı şekilde çevirileri erişilebilirlik özellikleri sağlar. Aşağıdaki örnekte bir `UITextField` sahip bir **yerelleştirme kimliği** , `Pqa-aa-ury` ve İspanyolca olarak ayarlanan iki erişilebilirlik özellikleri:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Bu dosya olarak yerleştirilir **es.lproj** İspanyolca içerik için dizin.

**Localizable.strings**

Çeviriler alternatif olarak, eklenebilir **Localizable.strings** dosya yerelleştirilmiş içerik dizininde (ör.) **Es.lproj** İspanyolca için):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

C# bu çevirileri kullanılabilir `LocalizedString` yöntemi:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Başvurmak [iOS yerelleştirme Kılavuzu](~/ios/app-fundamentals/localization/index.md) içerik yerelleştirme hakkında daha fazla bilgi.

<a name="testing" />

## <a name="testing-accessibility"></a>Erişilebilirlik test etme

VoiceOver etkinleşir **ayarları** giderek uygulama **genel > Erişilebilirlik > VoiceOver**:

![](accessibility-images/settings-sml.png "Konuşma hızını ayarlama")

**Erişilebilirlik** ekran ayarlarını yakınlaştırma, metin boyutu, renk & karşıtlık seçenekleri, Konuşma ayarları ve diğer yapılandırma seçeneklerini de sağlar.

Aşağıdaki adımları [VoiceOver yönergeleri](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) erişilebilirlik iOS cihazlarda test etmek için.


## <a name="simulator-testing"></a>Simulator test etme

Benzetici, test edilirken **erişilebilirlik denetçisi** erişilebilirlik özellikleri ve olayları doğru şekilde yapılandırıp yardımcı olmak kullanılabilir. Denetçisi'nde Aç **ayarları** giderek uygulama **genel > Erişilebilirlik > Erişilebilirlik denetçisi**:

![](accessibility-images/settings-inspector-sml.png "Erişilebilirlik Inspector'ı etkinleştirmek")

Bir kez etkinleştirildikten sonra Inspector penceresini her zaman iOS ekran üzerinde gezinen.
İşte bir örnek çıktısını bir tablo görünümü satır seçildiğinde – fark **etiket** satır ve, ayrıca, "yapılır" içerik sunan bir cümle içerir (IE. değer çizgilerinin görülebilir):

![](accessibility-images/tableview-a11y-sml.png "Erişilebilirlik Inspector'ı kullanarak")

Inspector görünür olsa da, geçici olarak göstermek ve katmana gizleme ve erişilebilirlik ayarlarını etkinleştir/devre dışı bırak için üst sol "X" simgesini kullanın.



## <a name="related-links"></a>İlgili bağlantılar

- [Platformlar arası erişilebilirlik](~/cross-platform/app-fundamentals/accessibility.md)
- [iOS erişilebilirlik (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [iOS VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)
