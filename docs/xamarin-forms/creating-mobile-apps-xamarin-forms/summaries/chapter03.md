---
title: Bölüm 3 özeti. Metnin ayrıntılı incelemesi
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 3 özeti. Metnin ayrıntılı incelemesi'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 3ef8f14bd60cf612408bb9e3885ef319d3efc8c5
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998341"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Bölüm 3 özeti. Metnin ayrıntılı incelemesi

Bu bölümde ele [ `Label` ](xref:Xamarin.Forms.Label) biçimlendirme ve renk, yazı tiplerini dahil olmak üzere daha ayrıntılı görünümde.

## <a name="wrapping-paragraphs"></a>Paragrafları sarmalama

Zaman [ `Text` ](xref:Xamarin.Forms.Label.Text) özelliği `Label` uzun metin içeriyor `Label` otomatik olarak birden çok satıra tarafından gösterildiği şekilde sarmalar [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) örnek. Tire veya yeni bir satıra ayırmak için '\r' gibi karakterler C# '\u2014' gibi Unicode kodlar ekleyebilirsiniz.

Zaman [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özelliklerini bir `Label` ayarlandığından `LayoutOptions.Fill`, genel boyutu `Label` boşluk ile yönetilir, kapsayıcısı kullanılabilir hale getirir. `Label` Olduğu söylenir *kısıtlı*. Boyutu `Label` kapsayıcısı boyutudur.

Zaman `HorizontalOptions` ve `VerticalOptions` özellikleri ayarlanır değerlere dışında `LayoutOptions.Fill`, boyutu `Label` kapsayıcısı için kullanılabilmesini yuvarlanarak metin işlemek için gereken alanı tabidir `Label`. `Label` Olduğu söylenir *sınırlandırılmamış* ve kendi boyutunu belirler.

(Not: koşulları *kısıtlı* ve *sınırlandırılmamış* sınırlandırılmamış bir görünüm kısıtlı bir görünüm genellikle daha küçük olduğundan counter-intuitive, olabilir. Ayrıca, bu kullanım koşullarını tutarlı bir şekilde kitabın erken olarak bölümlerde kullanılmaz.)

Bir görünümü gibi bir `Label` kısıtlı bir boyut ve diğerinde sınırlandırılmamış. A `Label` yatay kısıtlı yalnızca metin birden çok satırda kaydırılır.

Varsa bir `Label` olan kısıtlanmış, bu metin için gerekenden çok daha fazla alan kaplar. Metin genel alanını konumlandırılmalıdır `Label`. Ayarlama [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) üyesi özelliğini [ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) numaralandırması ([`Start`](xref:Xamarin.Forms.TextAlignment.Start), [ `Center` ](xref:Xamarin.Forms.TextAlignment.Center), veya [ `End` ](xref:Xamarin.Forms.TextAlignment.Center)) tüm satırlar Paragraf hizalamasını denetlemek için. Varsayılan `Start` ve metni sola hizalar.

Ayarlama [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) üyesi özelliğini `TextAlignment` üstünde, Orta veya tarafından kapladığı alan alt kısmındaki metni konumlandırmak için numaralandırma `Label`.

Ayarlama [ `LineBreakMode` ](xref:Xamarin.Forms.Label.LineBreakMode) üyesi özelliğini [ `LineBreakMode` ](xref:Xamarin.Forms.LineBreakMode) numaralandırması ([`WordWrap`](xref:Xamarin.Forms.LineBreakMode.WordWrap), [ `CharacterWrap` ](xref:Xamarin.Forms.LineBreakMode.CharacterWrap), [ `NoWrap` ](xref:Xamarin.Forms.LineBreakMode.NoWrap), [ `HeadTruncation` ](xref:Xamarin.Forms.LineBreakMode.HeadTruncation), [ `MiddleTruncation` ](xref:Xamarin.Forms.LineBreakMode.MiddleTruncation), veya [ `TailTruncation` ](xref:Xamarin.Forms.LineBreakMode.TailTruncation)) için Denetim nasıl birden çok bir paragraf aranın satırları veya kesilir.

## <a name="text-and-background-colors"></a>Metin ve arka plan renkleri

Ayarlama [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) ve [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) özelliklerini `Label` için [ `Color` ](xref:Xamarin.Forms.Color) metin ve arka plan rengini denetlemek için değerler.

`BackgroundColor` Tarafından kullanılan tüm alan arka planını uygulandığı `Label`. Yapılandırmanıza bağlı olarak `HorizontalOptions` ve `VerticalOptions` özellikleri, boyutu metni görüntülemek için gerekli alandan daha önemli ölçüde daha büyük olabilir. Renk çeşitli değerleriyle denemek için kullanabileceğiniz `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, ve `VerticalTextAlignment` boyutunu ve konumunu nasıl etkilediklerini görmek için `Label`, boyutu ve içindeki metnin konumunu `Label`.

## <a name="the-color-structure"></a>Renk yapısı

[ `Color` ](xref:Xamarin.Forms.Color) Yapısı renkleri kırmızı yeşil mavi (RGB) değerleri veya Hue doygunluk parlaklık (HSL) değerleri olarak veya bir renk adı belirtmenize olanak sağlar. Alfa kanalı saydamlık belirtmek de kullanılabilir.

Kullanım bir `Color` Oluşturucusu belirtmek için:

- bir [gri tonu](xref:Xamarin.Forms.Color.%23ctor(System.Double))
- bir [RGB değeri](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double))
- bir [saydamlık RGB değeri](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double))

Bağımsız değişkenler `double` değerleri 0 ile 1 arasında.

Oluşturmak için çeşitli statik yöntemler kullanabilirsiniz `Color` değerleri:

- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) için `double` RGB değerleri 0'dan 1
- [`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Int32,System.Int32,System.Int32)) RGB için tamsayı değerleri 0 ile 255
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Double,System.Double,System.Double,System.Double)) için `double` saydamlığına RGB değerleri
- [`Color.FromRgba`](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) saydam olarak tamsayı RGB değerleri
- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) için `double` saydamlığına HSL değerleri
- [`Color.FromUint`](xref:Xamarin.Forms.Color.FromUint(System.UInt32)) için bir `uint` olarak hesaplanan değer (B + 256 * (G + 256 * (R + 256 * Y)))
- [`Color.FromHex`](xref:Xamarin.Forms.Color.FromHex(System.String)) için bir `string` biçiminde bir onaltılık basamak biçiminin "#AARRGGBB" veya "#RRGGBB" veya "#ARGB" veya "#RGB", burada her harfe karşılık gelen bir onaltılık basamak alfa, kırmızı, yeşil ve mavi kanal. Bu bölümünde açıklandığı gibi XAML renk dönüştürmeler için kullanılan birincil yöntemdir [Bölüm 7, XAML ve kod karşılaştırması](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Oluşturulduktan sonra bir `Color` değer değişmez. Renk özelliklerini aşağıdaki özelliklerinden elde edilebilir:

- [`R`](xref:Xamarin.Forms.Color.R)
- [`G`](xref:Xamarin.Forms.Color.G)
- [`B`](xref:Xamarin.Forms.Color.B)
- [`A`](xref:Xamarin.Forms.Color.A)
- [`Hue`](xref:Xamarin.Forms.Color.Hue)
- [`Saturation`](xref:Xamarin.Forms.Color.Saturation)
- [`Luminosity`](xref:Xamarin.Forms.Color.Luminosity)

Tüm bunlar `double` değerleri 0 ile 1 arasında.

`Color` Ayrıca 240 ortak statik salt okunur alanları genel renklerin tanımlar. Kitap yazıldığı sırada, 17'yalnızca genel renklerin kullanılabilir.

Başka bir ortak statik salt okunur alan, tüm renk kanallarını sıfır ile bir renk tanımlar:

- [`Color.Transparent`](xref:Xamarin.Forms.Color.Transparent)

Birkaç örnek yöntemleri, yeni bir renk oluşturmak için varolan bir rengi değiştirme izin ver:

- [`AddLuminosity`](xref:Xamarin.Forms.Color.AddLuminosity(System.Double))
- [`MultiplyAlpha`](xref:Xamarin.Forms.Color.MultiplyAlpha(System.Double))
- [`WithHue`](xref:Xamarin.Forms.Color.WithHue(System.Double))
- [`WithLuminosity`](xref:Xamarin.Forms.Color.WithLuminosity(System.Double))
- [`WithSaturation`](xref:Xamarin.Forms.Color.WithSaturation(System.Double))

Son olarak, iki statik salt okunur özellikler özel renk değeri tanımlayın:

- [`Color.Default`](xref:Xamarin.Forms.Color.Default), tüm kanallar kümesine &ndash;1
- [`Color.Accent`](xref:Xamarin.Forms.Color.Accent)

`Color.Default` platformun renk düzenini uygulamak için tasarlanmıştır ve sonuç olarak farklı platformlarda farklı bağlamlarda farklı bir anlama sahiptir. Varsayılan olarak, platform renk düzenleri şunlardır:

- iOS: açık bir arka plan üzerinde koyu metin
- Android: Açık metin (kitaptaki) koyu renkli arka plan üzerinde ya da açık bir arka plan üzerinde koyu metin (AppCompat içinde aracılığıyla malzeme tasarımı için **ana** örnek kodu deponun dalı)
- UWP: Açık arka plan koyu metin
- Windows 8.1: Koyu renkli arka plan üzerinde açık metin
- Windows Phone 8.1: Koyu renkli arka plan üzerinde açık metin

`Color.Accent` Değeri ya da bir koyu ya da açık arka plan üzerinde görünür bir platforma özgü (ve bazı durumlarda kullanıcı tarafından seçilebilen) renk sonuçlanır.

## <a name="changing-the-application-color-scheme"></a>Uygulama renk düzenini değiştirme

Çeşitli platformları, yukarıdaki listede gösterildiği gibi bir varsayılan renk şeması vardır.

Android'i hedefleyen, hafif bir tema Android.Manifest.xml dosyasında ya da tarafından belirterek bir ışık üzerinde koyu düzenine geçmek olası [AppCompat ekleme ve malzeme tasarımı](~/xamarin-forms/platform/android/appcompat.md).

Windows platformları için renk teması genellikle kullanıcı tarafından seçilir, ancak ekleyebileceğiniz bir `RequestedTheme` özniteliğini ayarlayın ya da çok `Light` veya `Dark` platformun App.xaml dosyasında. Varsayılan olarak App.xaml dosyasında UWP projesi içeren bir `RequestedTheme` özniteliğini `Light`.

## <a name="font-sizes-and-attributes"></a>Yazı tipi boyutu ve öznitelikler

Ayarlama [ `FontFamily` ](xref:Xamarin.Forms.Label.FontFamily) özelliği `Label` "Times yazı tipi ailesi seçilecek Roman" gibi bir dize. Ancak, belirli bir platformda desteklenen bir yazı tipi ailesi belirtmeniz gerekir ve bu bağlamda platformları tutarsız.

Ayarlama [ `FontSize` ](xref:Xamarin.Forms.Label.FontSize) özelliği `Label` için bir `double` yazı yaklaşık yüksekliğini belirtmek için. Bkz: [boyutlarla ilgilenme bölüm 5](chapter05.md), yazı tipi boyutlarını akıllıca seçme hakkında daha fazla ayrıntı için.

Alternatif olarak birkaç hazır platforma bağımlı yazı tipi boyutlarını birini elde edebilirsiniz. Statik [ `Device.GetNamedSize` ](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,System.Type)) yöntemi ve [aşırı](xref:Xamarin.Forms.Device.GetNamedSize(Xamarin.Forms.NamedSize,Xamarin.Forms.Element)) ikisi de bir `double` yazı tipi boyutu değeri platforma uygun tabanlı üyelerinde [ `NamedSize` ](xref:Xamarin.Forms.NamedSize)numaralandırması ([`Default`](xref:Xamarin.Forms.NamedSize.Default), [ `Micro` ](xref:Xamarin.Forms.NamedSize.Micro), [ `Small` ](xref:Xamarin.Forms.NamedSize.Small), [ `Medium` ](xref:Xamarin.Forms.NamedSize.Medium),  ve [ `Large` ](xref:Xamarin.Forms.NamedSize.Large)). Döndürülen değer `Medium` üyesi değildir aynı `Default`. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) örnek boyutları adlı bu metni görüntüler.

Ayarlama [ `FontAttributes` ](xref:Xamarin.Forms.Label.FontAttributes) özelliği `Label` bu üye için [ `FontAttributes` ](xref:Xamarin.Forms.FontAttributes) numaralandırma [ `Bold` ](xref:Xamarin.Forms.FontAttributes.Bold), [ `Italic` ](xref:Xamarin.Forms.FontAttributes.Italic), veya [ `None` ](xref:Xamarin.Forms.FontAttributes.None). Birleştirebilirsiniz `Bold` ve `Italic` üyeleri ile C# bit düzeyinde OR işleci.

## <a name="formatted-text"></a>Biçimlendirilmiş metin

Tüm örnekler kadarki tüm metin görüntülenen `Label` aynı şekilde biçimlendirilmiş. Bir metin dizesi içinde biçimlendirme değiştirmek için ayarlamamanız `Text` özelliği `Label`. Bunun yerine, [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) türünde bir nesne özelliğini [ `FormattedString` ](xref:Xamarin.Forms.FormattedString).

`FormattedString` sahip bir [ `Spans` ](xref:Xamarin.Forms.FormattedString.Spans) koleksiyonudur özelliği [ `Span` ](xref:Xamarin.Forms.Span) nesneleri. Her `Span` nenesindeki kendi [ `Text` ](xref:Xamarin.Forms.Span.Text), [ `FontFamily` ](xref:Xamarin.Forms.Span.FontFamily), [ `FontSize` ](xref:Xamarin.Forms.Span.FontSize), [ `FontAttributes` ](xref:Xamarin.Forms.Span.FontAttributes), [ `ForegroundColor` ](xref:Xamarin.Forms.Span.ForegroundColor), ve [ `BackgroundColor` ](xref:Xamarin.Forms.Span.BackgroundColor) özellikleri.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) örnek gösterir kullanarak `FormattedText` tek satırlık bir metin özelliği ve [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) burada gösterildiği gibi tüm bir paragraf teknik gösterilmektedir:

[![Üç ekran değişkeninin biçimlendirilmiş paragraf](images/ch03fg06-small.png "değişkeni biçimlendirilmiş etiket metni")](images/ch03fg06-large.png#lightbox "değişkeni biçimlendirilmiş etiket metni")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) program kullanan tek bir `Label` ve `FormattedString` tüm her platform için adlandırılmış yazı tipi boyutunu görüntülemek için nesne.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 3 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Bölüm 3 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Bölüm 3'de F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Etiket](~/xamarin-forms/user-interface/text/label.md)
- [Renklerle çalışma](~/xamarin-forms/user-interface/colors.md)
