---
title: Xamarin.iOS etiketler
description: Bu belge Xamarin.ios'ta etiketleri kullanmayı açıklar. Bu program aracılığıyla ve iOS Designer ile etiketler oluşturma işlemini açıklar.
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: b52bdbd41eaafbc5e6c78e1f8514b701fd78bd6b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241919"
---
# <a name="labels-in-xamarinios"></a>Xamarin.iOS etiketler

`UILabel` Okuma yalnızca metin denetimi, tek ve çok satırlı görüntülemek için kullanılır. 

## <a name="implementing-a-label"></a>Bir etiket uygulama

Yeni bir etiket örneği tarafından oluşturulan bir [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Etiketleri ve görsel Taslaklar

UI iOS Designer kullanırken bir etiket ekleyebilirsiniz. Arama **etiket** içinde **araç kutusu** görünümünüzü sürükleyin:

![Araç kutusunda Etiket](labels-images/image3.png)

Aşağıdaki özellikler, Özellikler panelinde ayarlanabilir:

![Etiketi özellik bölmesi](labels-images/image2.png)

- **Metin içeriği** – düz veya öznitelikli. Düz metin ayarlamanıza olanak tanır [biçimlendirme özniteliklerini](#Formatting_Text_and_Label) üzerindeki tüm dize. Öznitelikli metinleri farklı karakter veya dize sözcükleri biçimlendirme ayarlamanıza olanak sağlar.
- **Renk, yazı tipi, hizalama** – biçimlendirme etiketi uygulanabilir öznitelikleri.
- **Satırları** – etiketi yayılabilir satır sayısını ayarlar. Bunu ayarlamak için gerekli sayıda satır kullanılacak etiketi izin vermek için 0.
- **Davranış** – etkin veya vurgulanan için ayarlanabilir. Etkin olan varsayılan olarak ayarlanırsa, devre dışı bırakılmış metinlerin açık gri renkte görüntülenir. Vurgulanan etiket bir kullanıcı tarafından seçildiğinde vurgulanan durumuyla çizilmesini sağlar ve varsayılan olarak devre dışıdır.
- **Baselane ve satır sonu** – 
    - Basline belirler nasıl olması metnin yazı tipi boyutlarını belirtilen farklı ise konumlandırılmış.
    - Satır sonlarını nasıl bir dize sarmalanmış veya kaldırılacak tek satırdan uzun olduğunda kesilmiş belirleyin.
- **Otomatik olarak küçültme** – gerekirse belirler, yazı tipi boyutu bir etiketi içinde nasıl simge.
- **Gölge uzaklığı vurgulanmış** – vurgulanan ve gölge rengini ayarlamanıza olanak tanır ve gölge uzaklığı.

## <a name="truncating-and-wrapping"></a>Kırpmadan ve sarmalama

İOS satırını kullanarak bilgi sonu için başvurmak [Kes ve Kaydır](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text) tarif.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Biçimlendirme metni ve etiket

Etikette kullandığınız dizeyi biçimlendirmek için tüm dizeyi özniteliklerinde biçimlendirmeyi ayarlayın ya da olabilir veya öznitelikli dizelerini kullanabilirsiniz. Aşağıdaki örnekler, bu uygulama işlemini gösterir:

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

Metin stil kullanma hakkında daha fazla bilgi için `NSAttributedString` başvurmak [stili metin](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/text_field/style_text) tarif.

Varsayılan olarak etiketlere sahip `Enabled` true, ancak bunu ayarlamak için olası kullanıcıya belirli bir denetimin devre dışı bir ipucu vermek için devre dışı bırakıldı:

```csharp
label.Enabled = false;
```

Bu etiket açık gri renge iOS kısıtlamaları ekran aşağıdaki örnek görüntüde gösterildiği gibi ayarlar:

![İOS düğmesini devre dışı](labels-images/image1.png)

Ayrıca, ek etkileri için etiket metni için vurgulama ve gölge metin rengi ayarlayabilirsiniz:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Bu gibi metin görüntüleyen:

![Vurgulama ve metin kümesinde gölge](labels-images/image4.png)

Yazı tipini bir UILabel değiştirme ile ilgili daha fazla bilgi için [yazı tipini değiştir](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/labels/change_the_font) tarif.





