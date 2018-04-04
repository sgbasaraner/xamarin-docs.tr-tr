---
title: Etiketler
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 8a5b2c12cfbca18280a8044d3d8c599d848de91d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="labels"></a>Etiketler

`UILabel` Okuma yalnızca metin denetimi, tek veya çok satırlı görüntülemek için kullanılır. 

## <a name="implementing-a-label"></a>Bir etiket uygulama

Yeni bir etiket örneği tarafından oluşturulan bir [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Etiketleri ve film şeritleri

UI Tasarımcısı iOS kullanırken bir etiket ekleyebilirsiniz. Arama **etiket** içinde **araç** ve görünümünüze sürükleyin:

![Araç kutusunda Etiket](labels-images/image3.png)

Aşağıdaki özellikler özellikleri defterinde ayarlanabilir:

![Etiket özelliği paneli](labels-images/image2.png)

- **Metin bağlamı** – düz veya öznitelikli. Düz metin ayarlamanıza olanak tanır [biçimlendirme özniteliklerini](#Formatting_Text_and_Label) tüm dizeyi üzerinde. Öznitelikli metinleri farklı karakterler veya sözcükler dizesindeki biçimlendirme ayarlamanıza olanak tanır.
- **Renk, yazı tipi hizalama** – etiketi uygulanabilir biçimlendirme öznitelikleri.
- **Satırları** – etiketi yayılabilir satır sayısını ayarlar. Bunu ayarlamak için gereken sayıda satır kullanılacak etiket izin vermek için 0.
- **Davranış** – etkin veya Highlighted için ayarlanabilir. Etkin olan varsayılan olarak ayarlanırsa, devre dışı bırakılmış metin açık bir gri renkte görüntülenir. Vurgulanan varsayılan olarak devre dışıdır ve bir kullanıcı tarafından seçildiğinde vurgulanan durumuyla çizilmesi etiket sağlar.
- **Baselane ve satır sonu** – 
    - Basline belirler nasıl olması metnin yazı tipi boyutlarını belirtilen farklı ise konumlandırıldı.
    - Satır sonları nasıl bir dize Sarmalanan veya kaldırılacak tek bir çizgi uzunsa kesilmiş belirler.
- **Daralma** – boyutta yazı tipi etiketi içinde nasıl küçültülmüş gerekirse belirler.
- **Vurgulanmış, gölge uzaklığı** – Hightlighted ve gölge rengini ayarlamanıza olanak tanır ve gölge uzaklığı.

## <a name="truncating-and-wrapping"></a>Kesilmesiyle ve kaydırma

İOS satırını kullanarak bilgi sonları için başvurmak [kesmek ve Kaydır](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/) tarif.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Metin ve etiket biçimlendirme

Bir etiket kullandığınız dize biçimlendirmek için tüm dizeyi özniteliklerinde biçimlendirmeyi ayarlayın ya da olabilir veya öznitelikli dizeleri kullanabilirsiniz. Aşağıdaki örnekler, bu uygulama gösterir:

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Stil metin kullanma hakkında daha fazla bilgi için `NSAttributedString` başvurmak [stili metin](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/) tarif.

Varsayılan etiketleri sahip `Enabled` true ayarlanmış, ancak için ayarlamak için olası bir belirli denetim devre dışı bırakıldı ipucu kullanıcıya vermek için devre dışı bırakılır:

```csharp
label.Enabled = false;
```

Bu etiket açık gri renge iOS kısıtlamaları ekran aşağıdaki örnek görüntüde gösterildiği gibi ayarlar:

![İOS devre dışı düğmesi](labels-images/image1.png)

Ek efektler için etiket metni için vurgu ve gölge metin renkleri de ayarlayabilirsiniz:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Bu gibi metin görüntüleyen:

![Metni vurgulama ve gölge ayarlayın](labels-images/image4.png)

Bir UILabel yazı tipini değiştirme hakkında daha fazla bilgi için bkz [yazı tipini değiştir](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/) tarif.





