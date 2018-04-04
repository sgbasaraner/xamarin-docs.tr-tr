---
title: Bölüm 21 özeti. Dönüşümler
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d9c135191fc8143ca932fec5129f1b35af4b29b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-21-transforms"></a>Bölüm 21 özeti. Dönüşümler

Xamarin.Forms görünüm konumu ve boyutu genellikle üst tarafından belirlenen ekranında görünen bir `Layout` veya `Layout<View>` türevi. *Dönüştürme* Xamarin.Forms özelliğini, bu konum, boyut veya hatta yönlendirmesini değiştirebilirsiniz.

Xamarin.Forms dönüşümler üç temel türlerini destekler:

- *Çeviri* &mdash; öğenin yatay veya dikey kaydırma
- *Ölçek* &mdash; öğenin boyutunu değiştirme
- *Döndürme* &mdash; bir nokta veya ekseni etrafında bir öğeyi aç

Xamarin.Forms içinde ölçeklendirme isotropic; Bu genişlik ve yükseklik hep etkiler. Döndürme hem ekranın ve iki boyutlu yüzeyinde 3B uzaydaki desteklenir. Hiçbir eğme (veya çalıştırmaları) dönüştürme ve hiçbir genelleştirilmiş matris dönüştürme yok.

Dönüşümler türü sekiz özelliklerle desteklenen `double` tarafından tanımlanan `VisualElement` sınıfı:

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

Bu özellikleri bağlanabilir özellikleri tarafından desteklenir. Bunlar veri bağlamanın hedefleri olabilir ve stili. [**Bölüm 22. Animasyon** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) nasıl bu özellikleri animasyon uygulanabilir, gösterir ancak bu bölümde bazı örnekleri Xamarin.Forms kullanarak bunları animasyon nasıl Göster [Zamanlayıcı](~/xamarin-forms/platform/device.md#Device_StartTimer).

Yalnızca öğe işlenir ve nasıl yapmak özellikleri etkileyen dönüştürme *değil* öğesi düzeninde nasıl algılanan etkiler.

## <a name="the-translation-transform"></a>Çeviri Dönüştürme

Sıfır olmayan değerler, [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) ve [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) özellikleri shift öğenin yatay veya dikey olarak.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) program iki Bu özelliklerle deneme olanak tanır `Slider` denetim öğeleri `TranslationX` ve `TranslationY` bir özelliklerini`Frame`. Dönüştürme, tüm alt de etkiler `Frame`.

### <a name="text-effects"></a>Metin etkileri

Bir ortak çevirisi özellikleri biraz metin işleme dengelemek için kullanılır. Bu, gösterilmiştir [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) örnek:

[![Üçlü ekran görüntüsü metin kaydırır](images/ch21fg03-small.png "metin kaydırır")](images/ch21fg03-large.png#lightbox "metin kaydırır")

Birden çok kopyasını oluşturmak için başka bir etkin olduğu bir `Label` örnekte gösterildiği gibi bir 3B blok benzeyecek şekilde [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) örnek.

### <a name="jumps-and-animations"></a>Atlar ve animasyonları

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) örnek taşımak için çeviri kullanan bir `Button` dokunduğunuz, ancak birincil amacı, göstermektir her `Button` konumda kullanıcı girişini alır nerede Düğme işlenir.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) örnek benzer ancak animasyon uygulamak için bir zamanlayıcı kullanan `Button` için başka bir noktadan.

## <a name="the-scale-transform"></a>Ölçek dönüştürme

[ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Dönüştürme artırabilir veya öğe işlenmiş boyutunu azaltabilirsiniz. Varsayılan değer 1’dir. 0 değeri öğenin görünmemesini neden olur. Negatif değerler Döndürülmüş 180 derece olarak görünür öğenin neden olur. `Scale` Özelliğini etkilemez `Width` veya `Height` öğesinin özelliklerini. Bu değerleri aynı kalır.

Deneme yapabileceğiniz `Scale` özelliğini kullanarak [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) örnek.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) örnek gösterilmektedir animasyon arasındaki farkı `Scale` özelliği bir `Button` ve animasyon `FontSize` özelliği. `FontSize` Özelliği etkiler nasıl `Button` düzende; algılanan `Scale` özelliğini desteklemez.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) örnek hesaplar bir `Scale` uygulanan özelliği bir `Label` page içinde hala sığdırma sırasında olabildiğince büyük yapmak için öğesi.

### <a name="anchoring-the-scale"></a>Ölçek sabitleme

Önceki üç örneklerinde ölçeklendirilmiş öğeleri tüm artırılabilir veya öğenin merkezi göre boyutunda azaltılabilir. Diğer bir deyişle, öğe artırır veya aynı tüm yönde boyutunu azaltır. Yalnızca noktası Center'da öğesi, ölçekleme sırasında aynı konumda kalır.

Ayarlayarak ölçeklendirme veya merkezi değiştirebileceğiniz [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) ve [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) özellikleri. Bu öğe göre özellikleridir. İçin `AnchorX`, öğenin sol tarafındaki için 0 değeri başvuruyor ve 1 için sağ tarafı gösterir. Benzer şekilde `AnchorY`, 0, üst ve alt 1. Her iki özellik merkezi olduğu 0,5, varsayılan değerlerine sahiptir.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) örnek deneme olanak tanır `AnchorX` ve `AnchorY` özellikleri yanı sıra `Scale` özelliği.

İos'ta, varsayılan olmayan değerleri kullanılarak `AnchorX` ve `AnchorY` özellikleri, telefon yönlendirmesini değişikliklerle genellikle uyumlu değil.

## <a name="the-rotation-transform"></a>Döndürme dönüştürme

[ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Özelliği derece cinsinden belirtilir ve bir nokta tarafından tanımlanan öğesinin etrafında saat yönünde bir döndürme belirtir `AnchorX` ve `AnchorY`. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) bu üç özellik ile denemeler olanak tanır.

### <a name="rotated-text-effects"></a>Döndürülen metin etkileri

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) örnek gösterilmektedir 64 küçük Döndürülmüş kullanarak bir daire çizmek gerekli matematik `BoxView` öğeleri.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) örnek görüntüler birden çok `Label` aynı metin dizesine öğeleriyle Döndürülmüş bağlı bileşen gibi görünmesi.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) örnek bir daire içinde kaydırmak için görüntülenen metin dizesini görüntüler.

### <a name="an-analog-clock"></a>Bir analog saat

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığını içeren bir [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) bir saat ellerini açıları hesaplar sınıfı. Sınıf kullandığı ViewModel Platform bağımlılıkları önlemek için `Task.Delay` yeni bir bulma için Zamanlayıcı yerine `DateTime` değeri.

De dahil **Xamarin.FormsBook.Toolkit** olan bir [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) uygulayan sınıf `IValueConverter` ve ikinci açı yakın ikinci yuvarlanacak sunar.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) üç döndürme kullanan `BoxView` bir analog saat çizmek için öğeleri.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) kullanan `BoxView` daha kapsamlı grafikler için değer de dahil olmak üzere işaretler saatin yüzünü ve bunların uçları bir az uzaklığı bu döndürme aktarır:

[![Üçlü ekran BoxView saatinin](images/ch21fg17-small.png "Analog Saat yüzünü")](images/ch21fg17-large.png#lightbox "Analog Saat yüzü")

Ayrıca bir [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) sınıfını **Xamarin.FormsBook.Toolkit** saniyeyi geri biraz şimdi, atlama önce çekme görünmesine neden olur ve ardından doğru konumuna geri dönün.

### <a name="vertical-sliders"></a>Dikey kaydırıcılar?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) örnek gösterilmektedir `Slider` öğeleri 90 derece döndürülmüş ve hala işlevi. Ancak, bunlar Döndürülmüş konumlandırmak zor olduğu `Slider` öğeleri düzende bunlar hala çünkü görünüyor yatay.

## <a name="3d-ish-rotations"></a>3B ngilizce döndürme

[ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) Doğrultusunda veya Görüntüleyici çıktığınızda taşımak için üst ve alt öğenin göründüğü şekilde bir öğeyi 3B x ekseni etrafında döndürme özelliği görünür. Benzer şekilde, [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) bir öğe ve sol tarafında öğesinin doğru veya Görüntüleyici çıktığınızda taşıma göründüğü yapmak için y ekseni etrafında döndürme gibi görünüyor.

`AnchorX` Özelliği etkiler `RotationY` ama `RotationX`. `AnchorY` Özelliği etkiler `RotationX` ama `RotationY`. Deneme yapabileceğiniz [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) bu özelliklerin etkileşimleri keşfetmek için örnek.

Xamarin.Forms tarafından kapsanan 3B koordinat sistemi sol ' dir. Noktası varsa, sol taraftaki artan X yönünde erişebildiğinizden (sağa) düzenler ve (aşağı), ardından Flash noktalarınızı, Z koordinatları (dışında ekran) artan yönde artan Y yönünde, Orta parmak düzenler.

Sol kaydırma, değerleri, artan yönde noktası ise ayrıca herhangi üç eksen için sonra parmakları eğrisini pozitif döndürme açısı için birer kez denemeyi yönünü gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 21 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Bölüm 21 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
