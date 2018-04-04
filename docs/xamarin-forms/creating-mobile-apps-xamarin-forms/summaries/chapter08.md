---
title: Bölüm 8 özeti. Kod ve XAML uyum içinde
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ffd2d508e99508309ec07c6bc65c8d716427bdff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>Bölüm 8 özeti. Kod ve XAML uyum içinde

Bu bölümde daha derine XAML inceler ve özellikle kod ve XAML nasıl etkileşime.

## <a name="passing-arguments"></a>Bağımsız değişkenleri geçirme

Genel durumda XAML'de örneği bir sınıf genel bir parametresiz oluşturucuya sahip olmalıdır; Sonuç nesnesi özellik ayarları'nda başlatılır. Ancak, nesne örneği ve başlatılmış olduğunu iki yol vardır.

Bu genel amaçlı teknikleri olsa da, bunlar çoğunlukla MVVM görünüm modelleri bağlantılı olarak kullanılır.

### <a name="constructors-with-arguments"></a>Bağımsız değişkenlerle oluşturucular

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) örnek nasıl kullanılacağını gösteren `x:Arguments` etiketi oluşturucu bağımsız değişkenleri belirtin. Bu bağımsız değişken türünü gösteren öğesi etiketlere göre ayrılmış gerekir. Temel .NET veri türleri için aşağıdaki etiketlerin kullanılabilir:

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

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) örnek nasıl kullanılacağını gösteren `x:FactoryMethod` öğesi bir nesne oluşturmak için çağrılan bir Üreteç yöntemi belirtin. Bu tür bir Üreteç yöntemi ortak ve statik olması gerekir ve içinde tanımlandığı türünde bir nesne oluşturmanız gerekir. (Örneğin [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/)) yöntemi niteleyen ortak ve statik olduğundan ve türünde bir değer döndürür `Color`.) İçinde belirtilen Üreteç yöntemi için bağımsız `x:Arguments` etiketler.

## <a name="the-xname-attribute"></a>X: Name özniteliği

`x:Name` Özniteliği bir ad vermeniz XAML'de örneği nesneyi sağlar. Bu adları için kuralları C# değişken adları ile aynıdır. Dönüşü aşağıdaki `InitializeComponent` oluşturucuda çağrısı, arka plan kod dosyasına karşılık gelen XAML öğesine erişmek için bu adlarını başvurabilir. Adları, oluşturulan sınıfa özel alanlara XAML ayrıştırıcısı tarafından gerçekten dönüştürülür.

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) kullanımını gösteren örnek `x:Name` iki tutmak arka plan kodu dosya izin vermek için `Label` öğesi geçerli tarih ve saati ile güncel XAML'de tanımlandı.

Aynı adlı birden çok öğe aynı sayfada için kullanılamaz. Kullanırsanız, belirli bir sorun olduğunu `OnPlatform` paralel adlı oluşturmak için nesneleri her platform için. [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) örnek daha iyi bir yolu gibi şeyler gösterir.

## <a name="custom-xaml-based-views"></a>XAML tabanlı özel görünümler

XAML biçimlendirme yinelenmesinin önlemek için birkaç yolu vardır. Bir ortak tekniktir öğesinden türetilen yeni bir XAML tabanlı sınıf oluşturmak için [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/). Bu teknik örneklerde gösterildiği [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) örnek. `ColorView` Sınıfı türer `ContentView` belirli bir renk ve adını görüntülemek için while `ColorViewListPage` sınıfı türer `ContentPage` her zamanki gibi ve açıkça 17 örneklerini oluşturur `ColorView`.

Erişme `ColorView` XAML sınıfında gerektiren yaygın olarak adlı başka bir XML ad alanı bildirimi `local` aynı bütünleştirilmiş kodda sınıfları için.

## <a name="events-and-handlers"></a>Olaylar ve işleyicileri

Olayları olay işleyicileri XAML'de atanabilir, ancak olay işleyicisi arka plan kod dosyasına uygulanmalıdır. [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) nasıl XAML'de tuş kullanıcı arabirimi oluşturma ve nasıl uygulandığını gösterilir `Clicked` arka plan kod dosyasına işleyicileri.

## <a name="tap-gestures"></a>Hareketleri dokunun

Tüm `View` nesne dokunmatik giriş elde edilir ve olayları bu girişten oluşturmak. `View` Sınıfı tanımlayan bir [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/) öğesinden türetilen sınıfların bir veya daha fazla örneklerini içeren koleksiyon özelliği [ `GestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/).

[ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Oluşturur [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/) olaylar. [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) program nasıl ekleneceği gösterilmektedir `TapGestureRecognizer` dört nesnelere `BoxView` taklit bir oyun oluşturmak için öğeleri:

[![Üçlü ekran görüntüsü monkey dokunun](images/ch08fg07-small.png "kopya oyun")](images/ch08fg07-large.png#lightbox "kopya oyun")

Ancak **MonkeyTap** programın gerçekten ses gerekiyor. (Bkz [sonraki bölümde](chapter09.md).)



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 8 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [Bölüm 8 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [Bölüm 8 F # örnek](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
