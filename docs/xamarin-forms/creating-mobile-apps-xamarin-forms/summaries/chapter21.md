---
title: Bölüm 21 özeti. Dönüşümler
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 21 özeti. Dönüşümler'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 16fcdb345fd9aeb9201a00a0bb98d143e6468f01
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156775"
---
# <a name="summary-of-chapter-21-transforms"></a>Bölüm 21 özeti. Dönüşümler

Ekranda bir konum ve boyut genellikle üst tarafından belirlenen bir Xamarin.Forms görünümü açılır bir `Layout` veya `Layout<View>` Türev. *Dönüştürme* bu konum, boyut veya hatta yönünü değiştirebilir, bir Xamarin.Forms özelliktir.

Xamarin.Forms, üç temel dönüşümleri türlerini destekler:

- *Çeviri* &mdash; öğenin yatay veya dikey kaydırma
- *Ölçek* &mdash; öğenin boyutunu değiştirme
- *Döndürme* &mdash; bir nokta veya ekseni etrafında bir öğeyi aç

Xamarin.Forms içinde ölçeklendirme isotropic; Bu genişliği ve yüksekliği eşit etkiler. Hem ekranın iki boyutlu yüzeyinde 3B alanda döndürme desteklenir. Eğriltme (veya çalıştırmaları) hiçbir dönüştürme ve genelleştirilmiş matris hiçbir dönüştürme yoktur.

Dönüşümler türü sekiz özelliklerle desteklenen `double` tarafından tanımlanan `VisualElement` sınıfı:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Tüm bu özellikler tarafından bağlanabilir özellikler desteklenir. Bunlar, veri bağlama hedefleri olabilir ve stili. [**Bölüm 22. Animasyon** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) nasıl bu özellikleri animasyonu oluşturulabilen, gösterir ancak bu bölümde bazı örnekleri Xamarin.Forms kullanarak bunları nasıl canlandırabilirsiniz Göster [Zamanlayıcı](~/xamarin-forms/platform/device.md#Device_StartTimer).

Yalnızca öğe işlenir ve nasıl yapmak özelliklerini etkileyen dönüşüm *değil* öğe düzeninde nasıl algılanan etkiler.

## <a name="the-translation-transform"></a>Çeviri dönüşümü

Sıfır olmayan değerleri [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) ve [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) özellikleri öğenin yatay veya dikey kaydırma.

[ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) program iki ile bu özellikleri denemek verir `Slider` denetim öğeleri `TranslationX` ve `TranslationY` bir özelliklerini`Frame`. Dönüştürme tüm alt de etkiler `Frame`.

### <a name="text-effects"></a>Yazı efektleri

Bir ortak çevirisi özellikleri biraz metin işleme dengelemek için kullanılır. Bu gösterilmiştir [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) örnek:

[![Metni kaydırır Üçlü görüntüsü](images/ch21fg03-small.png "metni kaydırır")](images/ch21fg03-large.png#lightbox "metni kaydırır")

Birden çok kopyasını oluşturmak için başka bir etkin olduğu bir `Label` örnekte gösterildiği gibi bir 3B blok benzeyecek şekilde [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) örnek.

### <a name="jumps-and-animations"></a>Atlar ve animasyonları

[ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) örnek çeviri taşımak için kullandığı bir `Button` dokunulduğunda, ancak birincil amacı, göstermektir her `Button` konumda kullanıcı girişini alır burada Düğme işlenir.

[ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) örnek benzer, ancak animasyon uygulamak için bir zamanlayıcı kullanır `Button` diğerine bir noktadan.

## <a name="the-scale-transform"></a>Ölçekleme dönüşümü

[ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Dönüştürme artırabilir veya öğenin işlenmiş boyutunu azaltabilirsiniz. Varsayılan değer 1’dir. 0 değeri, öğe görünmez neden olur. Negatif değerler öğeyi 180 derece dönmüş olabilir görünmesine neden olur. `Scale` Özelliğini etkilemez `Width` veya `Height` öğesinin özellikleri. Bu değerler aynı kalır.

Deneme yapabileceğiniz `Scale` özelliğini kullanarak [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) örnek.

[ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) örnek hareketlendirme arasındaki farkı gösterir `Scale` özelliği bir `Button` ve animasyon `FontSize` özelliği. `FontSize` Özelliğini etkiler nasıl `Button` düzende; algılanan `Scale` özelliği yok.

[ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) örnek hesaplar bir `Scale` uygulanan özellik bir `Label` sayfasında sığdırma hala çalışırken mümkün olduğu kadar büyük hale getirmeyi öğesi.

### <a name="anchoring-the-scale"></a>Ölçek sabitleme

Önceki üç örneklerinde ölçeği öğeleri tüm artırabilir veya merkezini öğenin göreli boyutu azaltılabilir. Diğer bir deyişle, öğe artırır veya tüm yönleri aynı boyutunu azaltır. Yalnızca merkezi noktada öğe ölçeklendirme sırasında aynı konumda kalır.

Ayarlayarak ölçeklendirmenin merkezi değiştirebilirsiniz [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) ve [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) özellikleri. Bu özellikler, öğe göre olur. İçin `AnchorX`, öğenin sol tarafındaki için 0 değerini ifade eder ve 1 sağ ifade eder. Benzer şekilde `AnchorY`, 0'dır, üst ve alt 1'dir. Her iki özellik, merkezi olduğu 0,5, varsayılan değerlerine sahiptir.

[ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) örnek denemeler olanak tanır `AnchorX` ve `AnchorY` özellikleri yanı sıra `Scale` özelliği.

İOS, varsayılan olmayan değerleri kullanarak `AnchorX` ve `AnchorY` özellikleri, telefon yönlendirme değişikliklerle genellikle uyumlu değil.

## <a name="the-rotation-transform"></a>Döndürme dönüşümü

[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Özelliği derece cinsinden belirtilir ve öğe tarafından tanımlanan bir nokta etrafında saat yönünde bir döndürme belirtir `AnchorX` ve `AnchorY`. [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) bu üç özellik ile deneme yapmanızı sağlar.

### <a name="rotated-text-effects"></a>Döndürülmüş metin etkileri

[ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) örnek gösterir 64 küçük Döndürülmüş kullanarak bir daire çizin gerekli matematik `BoxView` öğeleri.

[ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) birden çok örnek görüntüler `Label` aynı metin dizesine öğelerle Döndürülmüş uçlar gibi görünecek.

[ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) örnek bir daire içinde sarmalamak için görüntülenen metin dizesini görüntüler.

### <a name="an-analog-clock"></a>Bir analog saati

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı içeren bir [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) açıları bire bir saat için hesaplar sınıfı. ViewModel sınıfı kullanır, Platform bağımlılıkları önlemek için `Task.Delay` yerine yeni bir bulma için Zamanlayıcı `DateTime` değeri.

Ayrıca dahil **Xamarin.FormsBook.Toolkit** olduğu bir [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) uygulayan sınıf `IValueConverter` en yakın saniye için ikinci bir açı yuvarlanacak derler ve sunar.

[ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) üç döndürme kullanan `BoxView` bir analog saat çizmek için öğeleri.

[ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) kullanan `BoxView` daha kapsamlı grafikler, değer çizgisi dahil olmak üzere işaretler saatin yüzünü ve bunların uçları bir az uzaklığı, döndürme uygulamalı:

[![Üç ekran görüntüsü BoxView saat](images/ch21fg17-small.png "Analog saati yüz")](images/ch21fg17-large.png#lightbox "Analog saati yüz tanıma")

Ayrıca bir [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) sınıfını **Xamarin.FormsBook.Toolkit** saniyeyi geri biraz atlamak isterseniz, önce çekme gibi görünmesine neden olur ve ardından doğru konumunu geri taşımak için.

### <a name="vertical-sliders"></a>Dikey kaydırma çubuklarını?

[ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) örnek gösterir `Slider` öğeleri 90 derece döndürülmüş ve hala işlevi. Ancak, bunlar Döndürülmüş konumlandırmak zor olan `Slider` düzende bunlar hala çünkü öğeleri görünür yatay olarak.

## <a name="3d-ish-rotations"></a>Ish 3B Döndürme

[ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) Doğrultusunda veya uzaklaşmasına taşımak için üst ve alt öğe görünen şekilde öğenin 3B bir x ekseni etrafında döndürme özelliği görünür. Benzer şekilde, [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) öğenin doğrultusunda veya uzaklaşmasına taşımak gibi görünüyor sol ve sağ tarafında öğe hale getirmek için y ekseni etrafında döndürme gibi görünüyor.

`AnchorX` Özelliğini etkiler `RotationY` ama `RotationX`. `AnchorY` Özelliğini etkiler `RotationX` ama `RotationY`. Deneme yapabileceğiniz [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) bu özelliklerin etkileşimler keşfetmek için örnek.

Xamarin.Forms tarafından kapsanan 3B koordinat sistemi sol. İşaret, (sağda) erişebildiğinizden, sol taraftaki artan X yönünde, düzenler ve orta parmağınızı artan Y yönünde (basılı), ardından thumb noktalarınızı (dışında olan ekran) Z koordinatları artırma yönünde düzenler.

Sol taraftaki işaret, değerleri, artan bir yöne işaret Ayrıca, herhangi üç eksenin ardından eğrinin parmaklarınızın döndürme pozitif döndürme açısı için yönünü gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 21 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Bölüm 21 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
