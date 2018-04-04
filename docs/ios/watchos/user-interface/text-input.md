---
title: Metin girişi ile çalışma
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9dec6f754590abf6db8829f555376b423b7a7da7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-input"></a>Metin girişi ile çalışma

Ancak, bazı izleme kolay alternatifleri desteklemiyor Apple Watch kullanıcıların metin, Giriş bir klavye sağlamaz:

- Metin seçenekleri önceden tanımlanmış bir listeden seçerek,
- Siri yazdırma
- Bir emoji seçme,
- Harf harfli el yazısı tanıma (watchOS 3 sunulan) karalama.

Simulator dikte şu anda desteklemiyor, ancak hala metin girişi denetleyicinin diğer gibi seçenekler karalama, aşağıda gösterildiği gibi test edebilirsiniz:

![](text-input-images/textinput-sml.png "Karalama seçeneği test etme")

Bir izleme uygulaması metin girişi kabul etmek için:

1. Önceden tanımlanmış seçenekler dize dizisi oluşturun.
2. Çağrı `PresentTextInputController` diziye sahip veya değil, emoji izin verilip verilmeyeceğini ve bir `Action` kullanıcı bitirdikten sonra çağrılır.
3. Tamamlama eylemi için giriş sonucunu sınayın ve (büyük olasılıkla bir etiketin metin değerini ayarlama) uygulamasında uygun eylemi gerçekleştirin.

Aşağıdaki kod parçacığını kullanıcıya üç önceden tanımlanmış seçenekleri sunar:

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

`WKTextInputMode` Numaralandırması üç değeri vardır:

- Düz
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>Düz

Düz modu ayarlandığında, kullanıcının birini seçebilirsiniz:

- Yazdırma,
- Karalama, veya
- Uygulama sağlayan önceden tanımlanmış listeden.

[![](text-input-images/plain-scribble-sml.png "Yazdırma, karalama, veya uygulama sağlayan önceden tanımlanmış bir listeden")](text-input-images/plain-scribble.png#lightbox)

Sonuç olarak her zaman döndürülür bir `NSObject` , dönüştürülür için bir `string`.

## <a name="emoji"></a>Emoji

Emoji iki tür vardır:

- Normal Unicode emoji
- Animasyonlu görüntüleri

Kullanıcı bir Unicode emoji seçtiğinde, dize olarak döndürülür.

Animasyonlu resmi emoji seçtiyseniz `result` tamamlanmasında işleyici içerecek bir `NSData` emoji içeren nesne `UIImage`.

## <a name="accepting-dictation-only"></a>Dikte yalnızca kabul etme

Tüm öneriler (veya karalama seçeneği) göstermeden dikte ekran kullanıcıya doğrudan almak için:

- öneriler listesi için boş bir dizi geçirin ve
- Ayarlama `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Kullanıcı konuşurken izleme ekran ("Bu bir testtir." Örneğin) anlaşılan gibi metni içeren aşağıdaki ekran görüntüler:

![](text-input-images/dictation.png "Kullanıcı konuşurken anladım gibi izleme ekran metni gösterir")

Bunlar basın sonra **Bitti** düğmesi metni döndürülecek.



## <a name="related-links"></a>İlgili bağlantılar

- [Apple'nın metin ve etiketleri belge](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [watchOS 3’e Giriş](~/ios/watchos/platform/introduction-to-watchos3/index.md)
