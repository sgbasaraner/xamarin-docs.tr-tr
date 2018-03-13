---
title: "Programsal düzeni kısıtlamaları"
description: "Bu kılavuz iOS çalışmak iOS Tasarımcısı oluşturmak yerine C# kodunda Otomatik Yerleşim kısıtlamaları gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 119C8365-B470-4CD4-85F7-086F0A46DCBB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 7819201e76e268ea84bf2cc5d49a5a07b20a04e3
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="programmatic-layout-constraints"></a>Programsal düzeni kısıtlamaları

_Bu kılavuz iOS çalışmak iOS Tasarımcısı oluşturmak yerine C# kodunda Otomatik Yerleşim kısıtlamaları gösterir._

Otomatik Düzen ("Uyarlamalı düzeni" olarak da bilinir) bir esnek tasarım yaklaşımdır. Her öğenin konumu olduğu sabit kodlanmış bir noktasına ekranında, geçici düzeni sisteminden farklı olarak, Otomatik Yerleşim hakkındadır *ilişkileri* -öğe diğer öğeleri tasarım yüzeyine göreli konumlarını. Otomatik düzeni Kalp kısıtlamaları veya diğer öğeleri ekranında bağlamında, bir öğenin yerleştirme veya öğelerin tanımlayan kurallar olur. Öğeleri ekranında belirli bir konuma bağlı olmak zorunda değildir çünkü kısıtlamalar farklı ekran boyutlarına ve cihaz yönler iyi görünür Uyarlamalı bir düzen oluşturmak yardımcı olur.

Genellikle otomatik düzeniyle iOS çalışırken, grafik kullanıcı Arabirimi öğeleri düzeni kısıtlamalar yerleştirmek için iOS Tasarımcısı kullanacaksınız. Ancak, oluşturmak ve C# kodunda kısıtlamaları uygulamak için gereken zaman zamanlar olabilir. Örneğin, dinamik olarak kullanarak oluşturduğunuzda eklenen kullanıcı Arabirimi öğeleri bir `UIView`.

Bu kılavuz oluşturmak ve bunları grafik Tasarımcı iOS oluşturmak yerine C# kod kullanarak kısıtlamaları ile çalışmak nasıl yapacağınızı gösterir.

<a name="Creating-Constraints-Programmatically" />

## <a name="creating-constraints-programmatically"></a>Kısıtlamaları program aracılığıyla oluşturma

Yukarıda belirtildiği gibi genellikle, Otomatik Yerleşim kısıtlamalarıyla iOS Tasarımcısı çalışıyor olmanız. Kısıtlamaları program aracılığıyla oluşturmak zorunda kaldıkları süreleri seçmek için üç seçeneğiniz vardır:

* [Düzen bağlayıcılarını](#Layout-Anchors) -bu API bağlantı özelliklere erişim sağlar (gibi `TopAnchor`, `BottomAnchor` veya `HeightAnchor`) kısıtlanmasını kullanıcı Arabirimi öğelerinin.
* [Düzen kısıtlamaları](#Layout-Constraints) -kullanarak doğrudan kısıtlamaları oluşturabilirsiniz `NSLayoutConstraint` sınıfı.
* [Görsel biçimlendirme dili](#Visual-Format-Language) -kısıtlamaları tanımlamak için yöntemi gibi bir ASCII art sağlar.

Aşağıdaki bölümlerde, her seçenek ayrıntılı üzerinden geçer.

<a name="Layout-Anchors" />

### <a name="layout-anchors"></a>Düzen tutturucular

Kullanarak `NSLayoutAnchor` sınıfı, kısıtlanmasını kullanıcı Arabirimi öğeleri bağlantı özelliklerini temel kısıtlamalar oluşturmak için bir fluent arabirimi içeriyor. Örneğin, bir görünüm denetleyicisinin üst ve alt düzeni açığa çıkarır kılavuzları `TopAnchor`, `BottomAnchor` ve `HeightAnchor` kenar, merkezi, boyutu ve temel özellikleri bir görünüm sunar ancak bağlantı özellikleri.

> [!IMPORTANT]
> **Not:** standart bağlantı özellikler kümesini yanı sıra iOS görünümleri de dahil `LayoutMarginsGuides` ve `ReadableContentGuide` özellikleri. Bu özellikleri kullanıma `UILayoutGuide` nesneleri görünümün kenar boşlukları ve okunabilir çalışmak için kılavuzları sırasıyla içerik.

Düzen bağlayıcılarını bir kolay okunur, sıkıştırılmış biçimde kısıtlamalar oluşturmak için birkaç yöntem sağlar:

- **ConstraintEqualTo** -ilişkiyi tanımlar burada `first attribute = second attribute + [constant]` bir isteğe bağlı olarak sağlanan ile `constant` uzaklık değeri.
- **ConstraintGreaterThanOrEqualTo** -ilişkiyi tanımlar burada `first attribute >= second attribute + [constant]` bir isteğe bağlı olarak sağlanan ile `constant` uzaklık değeri.
- **ConstraintLessThanOrEqualTo** -ilişkiyi tanımlar burada `first attribute <= second attribute + [constant]` bir isteğe bağlı olarak sağlanan ile `constant` uzaklık değeri.

Örneğin:

```csharp
// Get the parent view's layout
var margins = View.LayoutMarginsGuide;

// Pin the leading edge of the view to the margin
OrangeView.LeadingAnchor.ConstraintEqualTo (margins.LeadingAnchor).Active = true;

// Pin the trailing edge of the view to the margin
OrangeView.TrailingAnchor.ConstraintEqualTo (margins.TrailingAnchor).Active = true;

// Give the view a 1:2 aspect ratio
OrangeView.HeightAnchor.ConstraintEqualTo (OrangeView.WidthAnchor, 2.0f);
```

Tipik bir düzen kısıtlaması yalnızca doğrusal bir ifade olarak ifade edilebilir. Aşağıdaki örnek alın:

[![](programmatic-layout-constraints-images/graph01.png "Doğrusal bir ifade olarak ifade edilen bir yerleşim kısıtlaması")](programmatic-layout-constraints-images/graph01.png#lightbox)

Hangi düzeni bağlayıcılarını kullanarak C# kod aşağıdaki satırı dönüştürülmesi:

```csharp
PurpleView.LeadingAnchor.ConstraintEqualTo (OrangeView.TrailingAnchor, 10).Active = true; 
```

Burada C# kod parçalarını denklemi belirli bölümlerine karşılık gelen aşağıdaki gibidir:

<table width="100%" border="1">
<tr>
<td width="50%"><b>Eşitlik</b></td><td><b>Kod</b></td>
</tr>
<tr>
<td width="50%">Madde 1</td><td>PurpleView</td>
</tr>
<tr>
<td width="50%">1 özniteliği</td><td>LeadingAnchor</td>
</tr>
<tr>
<td width="50%">İlişki</td><td>ConstraintEqualTo</td>
</tr>
<tr>
<td width="50%">Çarpan</td><td>Bu nedenle belirtilmemiş 1.0 varsayılanlara</td>
</tr>
<tr>
<td width="50%">2 öğesi</td><td>OrangeView</td>
</tr>
<tr>
<td width="50%">Öznitelik 2</td><td>TrailingAnchor</td>
</tr>
<tr>
<td width="50%">Sabit</td><td>10.0</td>
</tr>
</table>

Verilen yerleşim kısıtlaması denklemi çözmek için gerekli olan parametreleri sağlamanın yanı sıra, düzen bağlantı yöntemlerin her biri zorunlu kendisine geçirilen parametrelerden biri tür güvenliği. Bu nedenle yatay kısıtlaması tutturur gibi `LeadingAnchor` veya `TrailingAnchor` yalnızca kullanılabilir ile yatay diğer bağlantı türleri ve çarpanları yalnızca boyutu sınırlaması sağlanır.

<a name="Layout-Constraints" />

### <a name="layout-constraints"></a>Düzen kısıtlamaları

Doğrudan oluşturarak Otomatik Yerleşim kısıtlamaları el ile ekleyebilirsiniz bir `NSLayoutConstraint` C# kod. Düzen bağlayıcılarını kullanarak aksine tanımlanmakta kısıtlaması üzerinde hiçbir etkisi olmaz olsa bile her parametresi için bir değer belirtmeniz gerekir. Sonuç olarak, okumak Demirbaş kod sabit önemli miktarda oluşturan yukarı sona erer. Örneğin:

```csharp
//// Pin the leading edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Leading, NSLayoutRelation.Equal, View, NSLayoutAttribute.LeadingMargin, 1.0f, 0.0f).Active = true;

//// Pin the trailing edge of the view to the margin
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Trailing, NSLayoutRelation.Equal, View, NSLayoutAttribute.TrailingMargin, 1.0f, 0.0f).Active = true;

//// Give the view a 1:2 aspect ratio
NSLayoutConstraint.Create (OrangeView, NSLayoutAttribute.Height, NSLayoutRelation.Equal, OrangeView, NSLayoutAttribute.Width, 2.0f, 0.0f).Active = true;
```

Burada `NSLayoutAttribute` enum değeri görünümün kenar boşlukları için tanımlar ve karşılık gelen `LayoutMarginsGuide` gibi özellikler `Left`, `Right`, `Top` ve `Bottom` ve `NSLayoutRelation` enum ilişkiyi tanımlar, Belirtilen öznitelik olarak arasında oluşturulan `Equal`, `LessThanOrEqual` veya `GreaterThanOrEqual`.

Aksine düzeni bağlantı API ile `NSLayoutConstraint` oluşturma yöntemleri belirli bir kısıtlama önemli yönleri vurgulayın değil ve hiçbir derleme vardır zaman kısıtlaması gerçekleştirilen denetler. Sonuç olarak, çalışma zamanında bir özel durum atar geçersiz bir kısıtlama oluşturmak kolaydır.

<a name="Visual-Format-Language" />

### <a name="visual-format-language"></a>Görsel biçimi dili

Görsel biçim diliyle oluşturulan kısıtlaması görsel gösterimi sağlamak dizeler gibi ASCII resim kullanarak sınırlamaları tanımlamanızı sağlar. Bu, aşağıdaki avantajları ve dezavantajları vardır:

- Görsel biçim diliyle yalnızca geçerli kısıtlamaları oluşturulmasını zorlar.
 - Otomatik Yerleşim kısıtlaması oluşturmak için kullanılan kod hata ayıklama iletilerini benzer şekilde Visual biçimi dili kullanılarak konsola kısıtlamaları çıkarır.
 - Visual biçimi dili çok kısa bir ifade ile aynı anda birden çok kısıtlama oluşturmanıza olanak sağlar.
 - Bir derleme tarafında doğrulama Visual biçimi dil dizeleri olduğundan sorunları yalnızca çalışma zamanında bulunabilir.
 - Bazı kısıtlama türlerine Visual biçim diliyle görselleştirme bütünlük vurgular beri ile (oranları gibi) oluşturulamıyor.

Bir kısıtlama oluşturmak için Visual biçim diliyle kullanırken, aşağıdaki adımları uygulayın:

1. Oluşturma bir `NSDictionary` nesneleri görüntüle ve Düzen kılavuzları ve biçimleri tanımlarken kullanılacak bir dize anahtarı içerir.
2. İsteğe bağlı olarak oluşturma bir `NSDictionary` anahtarları ve değerleri kümesini tanımlar (`NSNumber`) kısıtlaması sabit değeri olarak kullanılır.
3. Düzen biçim dizesine, tek bir sütun veya satır öğelerinin oluşturun.
4. Çağrı `FromVisualFormat` yöntemi `NSLayoutConstraint` kısıtlamalar oluşturmak için sınıfı.
5. Çağrı `ActivateConstraints` yöntemi `NSLayoutConstraint` etkinleştirmek ve kısıtlamalar uygulamak için sınıf.

Örneğin, başında ve sonunda bir kısıtlama Visual biçimi dilde oluşturmak için aşağıdakileri kullanabilirsiniz:

```csharp
// Get views being constrained
var views = new NSMutableDictionary (); 
views.Add (new NSString ("orangeView"), OrangeView);

// Define format and assemble constraints
var format = "|-[orangeView]-|";
var constraints = NSLayoutConstraint.FromVisualFormat (format, NSLayoutFormatOptions.AlignAllTop, null, views);

// Apply constraints
NSLayoutConstraint.ActivateConstraints (constraints);
```

Visual biçimi dili her zaman varsayılan aralık kullanırken, üst görünüm öğenin kenar boşluklarına bağlı sıfır noktası kısıtlamaları oluşturduğundan, bu kod yukarıda sunulan örnekler için aynı sonucu verir.

Tek bir satıra birden çok alt görünüm gibi daha karmaşık UI tasarımları için görsel biçim diliyle yatay boşluğu ve dikey hizalamasını belirtir. Burada belirtir yukarıdaki örnekte olduğu gibi `AlignAllTop` `NSLayoutFormatOptions` tüm satır veya sütun kendi dows masaüstleri için görünümleri hizalar.

Apple'nın bkz [Visual biçimi dil ek](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1) genel kısıtlamalar ve görsel biçim dizesi dilbilgisi belirtme bazı örnekler.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu kılavuz, oluşturma ve iOS Tasarımcısı grafik oluşturarak aksine C# Otomatik Yerleşim kısıtlamaları çalışma sunulmuştur. İlk olarak, Düzen bağlayıcılarını kullanarak Aranan (`NSLayoutAnchor`) otomatik yerleşim işlemek için. Ardından, Düzen kısıtlamaları ile çalışmaya nasıl oluşturulacağını gösterir (`NSLayoutConstraint`). Son olarak, otomatik yerleşim için görsel biçimi dilini kullanarak sunulmuştur.

## <a name="related-links"></a>İlgili bağlantılar

- [Görsel Taslaklara Giriş](~/ios/user-interface/storyboards/index.md)
- [iOS Designable denetimleri gözden geçirme](~/ios/user-interface/designer/ios-designable-controls-walkthrough.md)
- [İOS için Xamarin Tasarımcısı ile otomatik Düzenle](~/ios/user-interface/designer/designer-auto-layout.md#modifying-in-code)
- [Apple - kısıtlamaları program aracılığıyla oluşturma](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ProgrammaticallyCreatingConstraints.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)
- [Apple - görsel biçimi dil ek](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH27-SW1)
