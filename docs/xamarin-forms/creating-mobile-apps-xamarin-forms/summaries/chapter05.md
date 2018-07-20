---
title: Bölüm 5 özeti. Boyutlarla ilgilenme
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 5 özeti. Boyutlarla ilgilenme'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 486800E9-C09F-4B95-9AC2-C0F8FE563BCF
author: charlespetzold
ms.author: chape
ms.date: 07/19/2018
ms.openlocfilehash: c82e222fd47f3a3f13043c076c488b4769659352
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156502"
---
# <a name="summary-of-chapter-5-dealing-with-sizes"></a>Bölüm 5 özeti. Boyutlarla ilgilenme

> [!NOTE] 
> Bu sayfadaki notları kitapta tanıtılan malzeme gelen Xamarin.Forms nerede ayrıldığını alanları gösterir.

Xamarin.Forms içinde çeşitli boyutlarda şimdiye karşılaşıldı:

- İOS durum çubuğunun yüksekliği 20'dir
- `BoxView` Varsayılan genişlik ve yükseklik 40 sahip
- Varsayılan `Padding` içinde bir `Frame` 20'dir
- Varsayılan `Spacing` üzerinde `StackLayout` 6
- `Device.GetNamedSize` Yöntemi bir sayısal yazı tipi boyutunu döndürür

Bu boyutları piksel değildir. Bunun yerine, bunlar bağımsız olarak her bir platform tarafından tanınan CİHAZDAN bağımsız birimler.

## <a name="pixels-points-dps-dips-and-dius"></a>Piksel, noktaları, dağıtım noktaları, Dıp'lerin ve DIUs

Erken geçmişleri Apple Mac ve Microsoft Windows içinde programcıları piksel birimlerinde çalışmıştır. Ancak, daha yüksek çözünürlükte görüntüler gelişinden ekran koordinatları daha sanallaştırılmış ve soyut bir yaklaşım gerekir. Mac dünyasında, programcılar birimleri cinsinden çalışılan *noktaları*, kullanılan Windows geliştiricileri sırasında 1/72 inç geleneksel *CİHAZDAN bağımsız birimler* (DIUs) tabanlı 1/96 inç üzerinde.

Mobil cihazlar, ancak genellikle yüz için çok daha yakın tutulan ve bir masaüstü daha yüksek çözünürlüğe sahip ekranlar, büyük bir piksel yoğunluğu izin anlamına gelir.

Apple iPhone ve iPad cihazları hedefleyen programcılar birimleri cinsinden çalışmaya devam *noktaları*, vardır, ancak bu noktalarının inç 160. Cihaza bağlı olarak olabilir 1, 2 veya 3 piksel noktasına.

Android benzerdir. Programcıların iş birimleri cinsinden *yoğunluklu bağımsız piksel* yu (dps) ve dağıtım noktaları ve piksel arasında ilişki 160 dps inç için temel alır.

Windows telefonlar ve mobil cihazları da inç 160 CİHAZDAN bağımsız birimler yakın bir şeyler gelmez ölçekleme faktörü oluşturulur.

> [!NOTE]
> Xamarin.Forms, artık Windows tabanlı bir telefon ya da mobil cihaz destekler.

Özet olarak, bir Xamarin.Forms Programcı telefonlar ve tabletler hedefleyen tüm ölçü birimi aşağıdaki ölçütü temel alan kabul edilebilir:

- 160 birimlerine denktir inç
- santimetre 64 birimler

Salt okunur [ `Width` ](xref:Xamarin.Forms.VisualElement.Width) ve [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) tarafından tanımlanan özellikler `VisualElement` "değerlerini sahte" varsayılan &ndash;1. Bu özellikler, yalnızca bir öğe olduğundan boyutlandırıldığından ve düzende paylaşabiliyor sırasında CİHAZDAN bağımsız birimler öğesinde gerçek boyutuna ücreti yansıtılır. Bu boyut herhangi içerir `Padding` lze nastavit ama `Margin`.

Bir görsel öğe harekete [ `SizeChanged` ](xref:Xamarin.Forms.VisualElement.SizeChanged) olay olduğunda kendi `Width` veya `Height` değişti. [ **WhatSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/WhatSize) örnek, programın ekran boyutunu görüntülemek için bu olay kullanır.

## <a name="metrical-sizes"></a>Metrical boyutları

[ **MetricalBoxView** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/MetricalBoxView) kullanan [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) ve [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) görüntülemek için bir `BoxView` bir inç uzun ve biri santimetre geniş.

## <a name="estimated-font-sizes"></a>Tahmini yazı tipi boyutu

[ **FontSizes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FontSizes) örnek nasıl noktaları birimlerinde yazı tipi boyutunu belirlemek için 160 birimleri-için--inç kural kullanılacağını göstermektedir. Bu tekniği kullanarak platformlar arasında visual tutarlılık göre daha iyidir `Device.GetNamedSize`.

## <a name="fitting-text-to-available-size"></a>Metni kullanılabilir boyutuna sığdırma

Belirli bir dikdörtgen içindeki metin bloğu hesaplayarak uyacak şekilde mümkündür bir `FontSize` , `Label` aşağıdaki ölçütleri kullanarak:

- Satır aralığı %120 yazı tipi boyutu (% Windows platformları 130) ' dir.
- Ortalama karakter genişliği yazı tipi boyutunun yüzdesi 50 ' dir.

[ **EstimatedFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EstimatedFontSize) örnek, bu teknik gösterir. Bu program, önce yazılmış [ `Margin` ](xref:Xamarin.Forms.View.Margin) , kullanması için özelliği kullanılabilir bir [ `ContentView` ](xref:Xamarin.Forms.ContentView) ile bir [ `Padding` ](xref:Xamarin.Forms.Layout.Padding) ayarı benzetimini yapmak için bir kenar boşluğu.

[![Tahmini yazı tipi boyutu üç ekran](images/ch05fg07-small.png "metin sığdırmak için kullanılabilen boyut")](images/ch05fg07-large.png#lightbox "metin sığdırmak için kullanılabilen boyut")

## <a name="a-fit-to-size-clock"></a>Uygun boyutu saati

[ **FitToSizeClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FitToSizeClock) örnek gösterir kullanarak [ `Device.StartTimer` ](xref:Xamarin.Forms.Device.StartTimer(System.TimeSpan,System.Func{System.Boolean})) saati güncelleştirme zamanı olduğunda, düzenli aralıklarla uygulama bildiren bir zamanlayıcıyı başlatmak için. Yazı tipi boyutunu-altıda için sayfa genişliği görünen mümkün olduğu kadar büyük hale getirmek için ayarlanır.

## <a name="accessibility-issues"></a>Erişilebilirlik sorunları

**EstimatedFontSize** program ve **FitToSizeClock** programı hem ince bir kusur içerir: kullanıcı, Android veya Windows 10 Mobile, program artık telefonun erişilebilirlik seçeneklerini değişip değişmediğini ne kadar büyük işlenen metnin yazı tipi boyutunu temel alan tahmin edebilirsiniz. [ **AccessibilityTest** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/AccessibilityTest) örnek bu sorunu gösterir.

## <a name="empirically-fitting-text"></a>Türü sığdırma metin

Bir dikdörtgene metin sığdırmak için başka bir türü işlenen metin boyutunu hesaplamak ve yukarı veya aşağı ayarlamak için yoludur. Kitap çağrıları programdaki [ `GetSizeRequest` ](xref:Xamarin.Forms.VisualElement.GetSizeRequest(System.Double,System.Double)) öğenin istenen boyut almak için bir görsel öğe üzerinde. Yöntem kullanımdan kaldırıldı ve programlar yerine çağırmalıdır [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)).

İçin bir `Label`ilk bağımsız değişken (kaydırma izin vermek için) kapsayıcının genişliği olmalıdır, ikinci bağımsız değişkeni ayarlanması için `Double.PositiveInfinity` sınırlandırılmamış yüksekliğe Getir için. [ **EmpiricalFontSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/EmpiricalFontSize) örnek, bu teknik gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 5 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch05-Apr2016.pdf)
- [Bölüm 5 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05)
- [Bölüm 5 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter05/FS)
