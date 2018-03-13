---
title: "Bölüm 5 özeti. Boyutları postalarla"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 1df1751c55c6a031bf9f26d774b739f4ca83fa91
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Bölüm 5 özeti. Boyutları postalarla

Xamarin.Forms çeşitli boyutlarda kadarki karşılaşıldı:

- İOS durum çubuğu yüksekliğini 20'dir
- `BoxView` Varsayılan genişlik ve yükseklik 40 sahip
- Varsayılan `Padding` içinde bir `Frame` 20'dir
- Varsayılan `Spacing` üzerinde `StackLayout` 6'dır
- `Device.GetNamedSize` Yöntemi bir sayısal yazı tipi boyutu döndürür

Bu boyutlar piksel değildir. Bunun yerine, bunlar bağımsız olarak her platformu tarafından tanınan aygıttan bağımsız birimleridir.

## <a name="pixels-points-dps-dips-and-dius"></a>Piksel, noktaları, dağıtım noktaları, Dıps ve DIUs

Erken geçmişine Apple Mac ve Microsoft Windows içinde programcıların piksel birimlerinde çalışmıştır. Ancak, daha yüksek çözünürlüklü ekranlar geliştirilirken daha sanallaştırılmış ve soyut bir yaklaşım ekran koordinatları için gereklidir. Mac dünyada programcıları çalışılan, biriminde *noktaları*, kullanılan Windows geliştiricileri çalışırken 1/72 inç geleneksel *CİHAZDAN bağımsız birimler* (DIUs) tabanlı 1/96 inç üzerinde.

Mobil aygıtlar, ancak genellikle yüze çok daha yakın tutulur ve Masaüstü daha yüksek çözünürlüğe sahip büyük bir piksel yoğunluğu izin gelir ekranlar.

Apple iPhone ve iPad cihazları hedefleme programcıları, biriminde çalışmaya devam *noktaları*, ancak bu noktalarına inç 160 vardır. Cihaza bağlı olarak olabilir 1, 2 veya 3 piksel noktasına.

Android benzer. Programcıları iş birimleri içinde *yoğunluğu bağımsız piksel* (dps) ve dağıtım noktaları ve piksel arasındaki ilişkiyi 160 dps inç için temel alır.

Windows çalışma zamanı da inç 160 CİHAZDAN bağımsız birimler yakın bir şey kapsıyor ölçeklendirme etkenleri oluşturmuştur.

Özet olarak, tüm ölçü aşağıdaki ölçütü temel alan telefonlar ve tabletler hedefleme Xamarin.Forms Programcı kabul edebilirsiniz:

- eşdeğer inç 160 birimler
- santimetre 64 birimler

Salt okunur [ `Width` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Width/) ve [ `Height` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Height/) tarafından tanımlanan özellikler `VisualElement` varsayılan "değerlerini & #x 2013; mock" 1. Yalnızca bir öğe olduğundan boyuta sahip ve düzende içerilen olduğunda bu özellikleri CİHAZDAN bağımsız birimler öğesinde gerçek boyutuna yansıtır. Bu boyut herhangi bir içerip `Padding` öğesinde ayarlanan ama `Margin`.

Bir görsel öğe harekete [ `SizeChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.SizeChanged/) olay olduğunda kendi `Width` veya `Height` değişti. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) örnek programın ekran boyutunu görüntülemek için bu olay kullanır.

## <a name="metrical-sizes"></a>Metrical boyutları

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) kullanan [ `WidthRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/) ve [ `HeightRequest` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/) görüntülemek için bir `BoxView` bir inç uzun ve diğeri santimetre geniş.

## <a name="estimated-font-sizes"></a>Tahmini yazı tipi boyutlarını

[ **FontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) örnek 160-birimler--inç arası kural noktaları birimlerinde yazı tipi boyutunu belirlemek için nasıl kullanılacağını gösterir. Bu teknik kullanılarak platformları arasında visual tutarlılık daha iyi `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Kullanılabilir boyut metne sığdırma

Hesaplayarak blok belirli dikdörtgenin içindeki metnin sığması olası bir `FontSize` , `Label` şu ölçütleri kullanarak:

- Satır aralığını %120 yazı tipi boyutu (%130 Windows platformlarında) olur.
- Ortalama karakter genişliği yazı tipi boyutunun % 50'dir.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) örneği, bu teknik gösterir. Bu programı önce yazıldı [ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) onu kullanması için özelliği kullanılabilir bir [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/) ile bir [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) benzetimini yapmak için ayarı bir kenar boşluğu.

[![Tahmini yazı tipi boyutu Üçlü ekran](images/ch05fg07-small.png "metnin sığması için kullanılabilen boyut")](images/ch05fg07-large.png#lightbox "metnin sığması için kullanılabilen boyut")

## <a name="a-fit-to-size-clock"></a>Uygun boyutu saati

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) örnek gösterilmektedir kullanarak [ `Device.StartTimer` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.StartTimer/p/System.TimeSpan/System.Func%7BSystem.Boolean%7D/) saati güncelleştirmek için zamanı geldiğinde bildiren düzenli aralıklarla uygulama süreölçer başlatmak için. Yazı tipi boyutunu görüntü mümkün olduğu kadar büyük yapmak için bir altıncı için sayfa genişliği ayarlanır.

## <a name="accessibility-issues"></a>Erişilebilirlik sorunları

**EstimatedFontSize** program ve **FitToSizeClock** programı hem zarif bir kusur içerir: kullanıcı Android veya Windows 10 Mobile, programın artık telefonun erişilebilirlik ayarlarını değişip değişmediğini ne kadar büyük işlenen metin yazı tipi boyutuna göre tahmin edebilirsiniz. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) örneği, bu sorunu gösterir.

## <a name="empirically-fitting-text"></a>Empirically sığdırma metin

Dikdörtgene metin sığması için başka bir empirically İşlenmiş metin boyutu hesaplamak ve yukarı veya aşağı ayarlamak için yoludur. Kitap çağrıları programa [ `GetSizeRequest` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.GetSizeRequest/p/System.Double/System.Double/) öğenin istenen boyuta elde etmek için bir görsel öğe üzerinde. Yöntem kullanım dışı bırakıldı ve programlar yerine çağırın [`Measure`] (/ api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/).

İçin bir `Label`, ilk bağımsız değişken (kaydırma izin vermek için) kapsayıcının genişliği olmalıdır, ikinci bağımsız değişken ayarlanmalıdır için `Double.PositiveInfinity` Kısıtlanmamış yükseklik yapma. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) örneği, bu teknik gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 5 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Bölüm 5 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Bölüm 5 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
