---
title: Bölüm 3 özeti. Daha derin bir metne
description: 'Xamarin.Forms ile mobil uygulamaları oluşturma: Bölüm 3 özeti. Daha derin bir metne'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 2E5581A6-4D3E-4BD5-9FDB-ACBA0F0FC734
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: f0e6063b6ce6038a6f6def67c27347ca024e72f6
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241493"
---
# <a name="summary-of-chapter-3-deeper-into-text"></a>Bölüm 3 özeti. Daha derin bir metne

Bu bölümde ele [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) biçimlendirme ve renk, yazı tipleri dahil olmak üzere daha fazla ayrıntılı görünümü.

## <a name="wrapping-paragraphs"></a>Paragrafları sarmalama

Zaman [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) özelliği `Label` uzun metin içeren `Label` otomatik olarak birden çok satıra tarafından gösterildiği gibi sarmalar [ **Baskervilles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/Baskervilles) örnek. Tire veya 'yeni bir satır sonu \r' gibi karakterler C# '\u2014' gibi Unicode kodlarını eklenebilir.

Zaman [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özelliklerini bir `Label` ayarlanır `LayoutOptions.Fill`, genel boyutu `Label` alanı tarafından yönetilen, kapsayıcısı kullanılabilir hale getirir. `Label` Olarak kabul edilir *kısıtlı*. Boyutunu `Label` kapsayıcısı boyutudur.

Zaman `HorizontalOptions` ve `VerticalOptions` özellikler ayarlanır değerlere dışında `LayoutOptions.Fill`, boyutu `Label` kapsayıcısı için kullanılabilir hale getirir boyutu kadar metni işlemek için gereken alanı tabidir `Label`. `Label` Olarak kabul edilir *Kısıtlanmamış* ve kendi boyutunu belirler.

(Not: koşulları *kısıtlı* ve *Kısıtlanmamış* Kısıtlanmamış görünüm genellikle kısıtlanmış bir görünümü küçük olduğundan counter-intuitive, olabilir. Ayrıca, bu koşulları tutarlı bir şekilde kitap erken bölümlerde kullanılmaz.)

Bir görünümü gibi bir `Label` bir boyuttaki kısıtlı ve diğer Kısıtlanmamış. A `Label` yatay kısıtlı yalnızca metin birden çok satırda kaydırılır.

Varsa bir `Label` olan kısıtlı, bu metin için gereken daha önemli ölçüde daha fazla alan kaplar. Metnin genel alanını yerleştirilebilir `Label`. Ayarlama [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) üyesi özelliğine [ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) numaralandırması ([`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/), veya [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)) paragrafın tüm satırları hizalamasını denetlemek için. Varsayılan değer `Start` ve metni sola hizalar.

Ayarlama [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) üyesi özelliğine `TextAlignment` üstünde, merkezi veya tarafından kapladığı alanı altındaki metni konumlandırmak için numaralandırma `Label`.

Ayarlama [ `LineBreakMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.LineBreakMode/) üyesi özelliğine [ `LineBreakMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) numaralandırması ([`WordWrap`](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.WordWrap/), [ `CharacterWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.CharacterWrap/), [ `NoWrap` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.NoWrap/), [ `HeadTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.HeadTruncation/), [ `MiddleTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.MiddleTruncation/), veya [ `TailTruncation` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LineBreakMode.TailTruncation/)) için birden fazla paragraf aranın satırları veya kesiliyor nasıl denetim.

## <a name="text-and-background-colors"></a>Metin ve arka plan renkleri

Ayarlama [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) ve [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) özelliklerini `Label` için [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) metin ve arka plan rengini denetlemek için değerler.

`BackgroundColor` Tarafından kullanılan tüm alanı arka planının uygulandığı `Label`. Bağlı olarak `HorizontalOptions` ve `VerticalOptions` özellikleri, boyutu metni görüntülemek için gereken alandan daha büyük olabilir. Renk çeşitli değerleriyle denemek için kullanabileceğiniz `HorizontalOptions`, `VerticalOptions`, `HorizontalExeAlignment`, ve `VerticalTextAlignment` boyutunu ve konumunu nasıl etkilediklerini görmek için `Label`, boyutu ve içindeki metnin konumunu `Label`.

## <a name="the-color-structure"></a>Renk yapısı

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Yapısı kırmızı yeşil mavi (RGB) değerler veya Ton Doygunluk parlaklığını (HSL) değerler olarak ya da bir renk adıyla renkleri belirtmenize olanak sağlar. Alfa kanal saydamlık belirtmek de kullanılabilir.

Kullanım bir `Color` Oluşturucusu belirtin:

- bir [gri gölge](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/)
- bir [RGB değeri](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/)
- bir [saydamlık RGB değerle](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/)

Bağımsız değişkenler `double` 0 ile 1 arasında değişen değerler.

Oluşturmak için çeşitli statik yöntemler kullanabilirsiniz `Color` değerler:

- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/) için `double` RGB değerleri 0 ile 1
- [`Color.FromRgb`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Int32/System.Int32/System.Int32/) tamsayı RGB değerleri 0 ile 255
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Double/System.Double/System.Double/System.Double/) için `double` saydamlık RGB değerleri
- [`Color.FromRgba`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) saydam olarak tamsayı RGB değerleri
- [`Color.FromHsla`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) için `double` saydamlık HSL değerleri
- [`Color.FromUint`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromUint/p/System.UInt32/) için bir `uint` değerinin hesaplanması olarak (B + 256 * (G + 256 * (R + 256 * A)))
- [`Color.FromHex`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) için bir `string` biçiminde onaltılık basamak biçiminin "#AARRGGBB" veya "#RRGGBB" veya "#ARGB" veya "#RGB", burada her harfi karşılık gelen bir onaltılık basamak alfa, kırmızı, için yeşil ve mavi kanallar. Bu yöntem anlatıldığı gibi XAML renk dönüştürmeleri için kullanılan birincil [Bölüm 7, kod ve XAML](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md).

Bir kez oluşturulduktan sonra bir `Color` değer değişmez. Renk özelliklerini aşağıdaki özelliklerinden edinilebilir:

- [`R`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/)
- [`G`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/)
- [`B`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/)
- [`A`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/)
- [`Hue`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Hue/)
- [`Saturation`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Saturation/)
- [`Luminosity`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Luminosity/)

Tüm bunlar `double` 0 ile 1 arasında değişen değerler.

`Color` Ayrıca 240 ortak statik salt okunur alanlar için ortak renkleri tanımlar. Kitap yazıldı aynı anda yalnızca 17 ortak renkleri kullanılabilir.

Başka bir genel statik salt okunur alan, sıfır olarak ayarlanması tüm renk kanallar ile bir renk tanımlar:

- [`Color.Transparent`](https://developer.xamarin.com/api/field/Xamarin.Forms.Color.Transparent/)

Birkaç örnek yöntemleri, yeni bir renk oluşturmak için varolan bir rengi değiştirme izin ver:

- [`AddLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.AddLuminosity/p/System.Double/)
- [`MultiplyAlpha`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.MultiplyAlpha/p/System.Double/)
- [`WithHue`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithHue/p/System.Double/)
- [`WithLuminosity`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithLuminosity/p/System.Double/)
- [`WithSaturation`](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.WithSaturation/p/System.Double/)

Son olarak, iki statik salt okunur özellikler özel renk değeri tanımlayın:

- [`Color.Default`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), tüm kanalları kümesine &ndash;1
- [`Color.Accent`](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Accent/)

`Color.Default` platformun renk düzenini zorlamak için tasarlanmıştır ve sonuç olarak farklı platformlarda farklı bağlamlarda farklı bir anlamı yoktur. Varsayılan olarak, platform renk düzenleri şunlardır:

- iOS: açık bir arka plan üzerinde koyu metin
- Android: Açık metin (kitaptaki) koyu arka plan üzerinde veya açık bir arka plan üzerinde koyu metin (malzeme tasarım uygulama aracılığıyla **ana** örnek kod deponun dalı)
- UWP: Açık bir arka plan koyu metin
- Windows 8.1: Koyu arka plan üzerinde açık metin
- Windows Phone 8.1: Koyu arka plan üzerinde açık metin

`Color.Accent` Değeri ya da bir koyu veya açık arka plan üzerinde görünürdür platforma özgü (ve bazen kullanıcı tarafından seçilebilen) renk sonuçlanır.

## <a name="changing-the-application-color-scheme"></a>Uygulama renk düzenini değiştirme

Çeşitli platformlar yukarıdaki listede gösterildiği gibi bir varsayılan renk düzenini vardır.

Android hedeflerken Android.Manifest.xml dosyasında veya tarafından açık bir tema belirterek bir açık üzerinde koyu düzenine geçmek olası [uygulama ekleme ve malzeme tasarım](~/xamarin-forms/platform/android/appcompat.md).

Windows platformları için renk temasını normalde kullanıcı tarafından seçilir, ancak ekleyebileceğiniz bir `RequestedTheme` özniteliğini ayarlamak için iki `Light` veya `Dark` platformun App.xaml dosyasında. Varsayılan olarak, UWP projesini App.xaml dosyasında içeren bir `RequestedTheme` özniteliği kümesine `Light`.

## <a name="font-sizes-and-attributes"></a>Yazı tipi boyutlarını ve öznitelikleri

Ayarlama [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontFamily/) özelliği `Label` "Kez bir yazı tipi ailesi seçmek için Latin" gibi bir dizeye. Ancak, belirli platformda desteklenen bir yazı tipi ailesi belirtmeniz gerekir ve platformları bu bağlamda tutarsız.

Ayarlama [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontSize/) özelliği `Label` için bir `double` yazı tipi yaklaşık yüksekliğini belirtmek için. Bkz: [boyutlarıyla ilgilenme bölüm 5](chapter05.md), yazı tipi boyutlarını akıllıca seçme hakkında daha fazla bilgi.

Alternatif olarak birkaç hazır platforma bağımlı yazı tipi boyutlarını birini elde edebilirsiniz. Statik [ `Device.GetNamedSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/System.Type/) yöntemi ve [aşırı](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.GetNamedSize/p/Xamarin.Forms.NamedSize/Xamarin.Forms.Element/) ikisi de bir `double` yazı tipi boyutu değeri platforma uygun dayalı üyelerinde [ `NamedSize` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NamedSize/)numaralandırması ([`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Default/), [ `Micro` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Micro/), [ `Small` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Small/), [ `Medium` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Medium/),  ve [ `Large` ](https://developer.xamarin.com/api/field/Xamarin.Forms.NamedSize.Large/)). Döndürülen değerin `Medium` üyesi olmayan mutlaka aynı `Default`. [ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) örnek boyutları adlı bunlarla metin görüntüler.

Ayarlama [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FontAttributes/) özelliği `Label` bu üye için [ `FontAttributes` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FontAttributes/) numaralandırma, [ `Bold` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Bold/), [ `Italic` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.Italic/), veya [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FontAttributes.None/). Birleştirebilirsiniz `Bold` ve `Italic` C# bit düzeyinde OR işleci üyeleriyle.

## <a name="formatted-text"></a>Biçimlendirilmiş metin

Tüm örnekler kadarki tüm metin tarafından görüntülenen `Label` aynı şekilde biçimlendirilmiş. Bir metin dizesi içinde biçimlendirme değiştirecek şekilde ayarlamazsanız `Text` özelliği `Label`. Bunun yerine, ayarlamak [ `FormattedText` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.FormattedText/) türünde bir nesne için özellik [ `FormattedString` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/).

`FormattedString` sahip bir [ `Spans` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FormattedString.Spans/) koleksiyonudur özelliği [ `Span` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Span/) nesneleri. Her `Span` nesne sahip kendi [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.Text/), [ `FontFamily` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontFamily/), [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontSize/), [ `FontAttributes` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.FontAttributes/), [ `ForegroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.ForegroundColor/), ve [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Span.BackgroundColor/) özellikleri.

[ **VariableFormattedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormText) örnek gösterilmektedir kullanarak `FormattedText` tek satırlık metin, özellik ve [ **VariableFormattedParagraph** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/VarFormPara) tekniği tüm paragrafı için aşağıda gösterildiği gibi gösterir:

[![Üçlü ekran değişkenin biçimlendirilmiş paragraf](images/ch03fg06-small.png "değişkeni biçimlendirilmiş etiket metnini")](images/ch03fg06-large.png#lightbox "değişkeni biçimlendirilmiş etiket metni")

[ **NamedFontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/NamedFontSizes) program kullanan tek bir `Label` ve `FormattedString` her platform için adlandırılmış yazı tipi boyutlarını görüntülenecek nesne.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 3 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Bölüm 3 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Bölüm 3 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Etiket](~/xamarin-forms/user-interface/text/label.md)
- [Renklerle çalışma](~/xamarin-forms/user-interface/colors.md)
