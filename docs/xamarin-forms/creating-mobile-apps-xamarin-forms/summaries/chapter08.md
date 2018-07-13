---
title: Bölüm 8 özeti. Kod ve XAML uyum içinde
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 8 özeti. Kod ve XAML uyum içinde'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b08355db6cc90381b16f51ce7bf23be8e8bd4e14
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994539"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Bölüm 8 özeti. Kod ve XAML uyum içinde

Bu bölümde daha derin bir şekilde XAML keşfediyor ve özellikle kod ve XAML nasıl etkileşim.

## <a name="passing-arguments"></a>Bağımsız değişkenleri geçirme

Genel durumda XAML içinde örneklenmiş bir sınıfı genel parametresiz oluşturucusu olmalıdır; Sonuçta elde edilen nesnenin özellik ayarları'nda başlatılır. Ancak, nesne örneği başlatıldı ve iki yol vardır.

Bunlar genel amaçlı teknikleri olsa da, bunlar çoğunlukla MVVM görünüm modelleri bağlantılı olarak kullanılır.

### <a name="constructors-with-arguments"></a>Oluşturucu bağımsız değişken

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) örnek nasıl kullanılacağını gösterir `x:Arguments` oluşturucu bağımsız değişkenlerini belirtmek için etiket. Bu bağımsız değişken bağımsız değişken türünü gösteren öğesi etiketleri ile sınırlanması gerekir. Temel .NET veri türleri için aşağıdaki etiketleri kullanılabilir:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>XAML yöntemleri çağırabilir miyim?

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) örnek nasıl kullanılacağını gösterir `x:FactoryMethod` bir nesne oluşturmak için çağrılan bir Üreteç yöntemi belirtmek için öğesi. Böyle bir Üreteç yöntemi genel ve statik olmalıdır ve içinde tanımlandığı türünde bir nesne oluşturmanız gerekir. (Örneğin [ `Color.FromRgb` ](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) yöntemi niteleyen genel ve statik olduğundan ve türünde bir değer döndürür `Color`.) Bağımsız değişkenler için Üreteç yöntemi içinde belirtilen `x:Arguments` etiketler.

## <a name="the-xname-attribute"></a>X: Name özniteliği

`x:Name` Özniteliği sağlayan bir ad vermeniz için XAML içinde oluşturulan bir nesne. Bu adlar için kuralları, C# değişken adları ile aynıdır. Dönüşü aşağıdaki `InitializeComponent` oluşturucuda LogEvent() çağrısını, arka plan kod dosyasına karşılık gelen XAML öğesine erişmek için bu adlar başvurabilir. Adları oluşturulan kısmi sınıftaki özel alanlara XAML ayrıştırıcı tarafından gerçekten dönüştürülür.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) örnek, kullanımını gösterir `x:Name` iki korumak arka plan kod dosyasına izin vermek için `Label` geçerli tarih ve saat ile güncelleştirilmiş XAML içinde tanımlanan öğe.

Aynı adlı birden çok öğe aynı sayfada için kullanılamaz. Kullanıyorsanız belirli bir sorun olduğunu `OnPlatform` paralel adlı oluşturmak için nesneleri her platform için. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) örnek gibi bir şey daha iyi bir yolunu gösterir.

## <a name="custom-xaml-based-views"></a>XAML tabanlı özel görünümler

XAML, biçimlendirme tekrarından kaçınmanın birkaç yolu vardır. Bir sık kullanılan bir yöntemdir, türetilen yeni XAML tabanlı sınıf oluşturmak için [ `ContentView` ](xref:Xamarin.Forms.ContentView). Bu teknik gösterilmiştir [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) örnek. `ColorView` Sınıf türetilir `ContentView` belirli bir renk ve adını görüntülemek için while `ColorViewListPage` sınıf türetilir `ContentPage` zamanki ve açıkça 17 örneklerini oluşturur `ColorView`.

Erişim `ColorView` XAML sınıfında sık adlı başka bir XML ad alanı bildirimi gerektirir `local` aynı derleme içerisindeki sınıflar için.

## <a name="events-and-handlers"></a>Olayları ve işleyicilerini

Olayları olay işleyicileri XAML ile atanabilir, ancak arka plan kod dosyasında olay işleyicisi uygulanmalıdır. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) XAML tuş kullanıcı arabiriminde oluşturma ve nasıl uygulanacağını gösterir `Clicked` arka plan kod dosyasında işleyicileri.

## <a name="tap-gestures"></a>Hareket dokunun

Tüm `View` nesne dokunma girişi alabilir ve bu giriş olayları oluşturmak. `View` Sınıfı tanımlayan bir [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers) bir veya daha fazla türetilen sınıfların örneklerini içeren bir koleksiyon özelliği [ `GestureRecognizer` ](xref:Xamarin.Forms.GestureRecognizer).

[ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer) Oluşturur [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) olayları. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) program nasıl ekleneceği gösterilmektedir `TapGestureRecognizer` dört nesnelere `BoxView` taklit bir oyun oluşturmak için öğeleri:

[![Üç ekran görüntüsü monkey dokunun](images/ch08fg07-small.png "kopya oyun")](images/ch08fg07-large.png#lightbox "kopya oyunu")

Ancak **MonkeyTap** programın gerçekten ses gerekiyor. (Bkz [sonraki bölümde](chapter09.md).)



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 8 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Bölüm 8 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Bölüm 8'de F # örnek](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
