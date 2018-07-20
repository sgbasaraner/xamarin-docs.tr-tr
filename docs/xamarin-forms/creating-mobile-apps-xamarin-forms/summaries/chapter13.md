---
title: 13. Bölüm özeti. Bit eşlemler
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: 13. Bölüm özeti. Bit eşlemler'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: d863ce1c6195ddaef164c3a15817a4ff87a3c332
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156632"
---
# <a name="summary-of-chapter-13-bitmaps"></a>13. Bölüm özeti. Bit eşlemler

> [!NOTE] 
> Bu sayfadaki notları kitapta tanıtılan malzeme gelen Xamarin.Forms nerede ayrıldığını alanları gösterir.

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) öğesi bir bit eşlem görüntüler. Tüm Xamarin.Forms platformlar JPEG, PNG, GIF ve BMP dosyası biçimleri için destek.

Bit eşlemler Xamarin.Forms içinde dört yerlerden gelir:

- Web üzerinden bir URL tarafından belirtilen
- Katıştırılmış kaynak olarak paylaşılan kitaplık
- Platform uygulaması projelerinde kaynak olarak gömülü
- .NET tarafından başvurulabilen bir yerden `Stream` dahil olmak üzere, nesne `MemoryStream`

Bit eşlem kaynakları platformu projelerinde platforma özgü olsa da bit eşlem paylaşılan Kitaplığı'nda platformdan bağımsız, kaynaklardır.

> [!NOTE] 
> Kitabın metin .NET standart kitaplıkları tarafından değiştirilmiştir taşınabilir sınıf kitaplıkları için başvuru yapar. Kayıt defterinden tüm örnek kod, .NET standart kitaplıkları kullanmak için dönüştürüldü.

Bit eşlem ayarlanarak belirtilir [ `Source` ](xref:Xamarin.Forms.Image.Source) özelliği `Image` türünde bir nesne için [ `ImageSource` ](xref:Xamarin.Forms.ImageSource), üç türevleri bir Özet sınıf:

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) temel web üzerinde bir bit eşlem erişmek için bir `Uri` küme nesnesi kendi [ `Uri` ](xref:Xamarin.Forms.UriImageSource.Uri) özelliği
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) ayarlamak bir klasör ve dosya yolunda dayalı bir platform uygulama projesinde depolanan bir bit eşlem erişmek için kendi [ `File` ](xref:Xamarin.Forms.FileImageSource.File) özelliği
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) .NET kullanarak bir bit eşlem yükleme `Stream` döndürerek belirtilen nesne bir `Stream` gelen bir `Func` ayarlayın, [ `Stream` ](xref:Xamarin.Forms.StreamImageSource.Stream) özelliği

Alternatif olarak (ve daha sık) aşağıdaki statik yöntemlerini kullanabilirsiniz `ImageSource` sınıfı, tüm hangi iade `ImageSource` nesneler:

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) temel web üzerinde bir bit eşlem erişmek için bir `Uri` nesnesi
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) PCL uygulamada katıştırılmış bir kaynağı olarak depolanan bir bit eşlem erişmek için; [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) veya [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) başka bir kaynak derlemedeki bir bit eşlem erişmek için
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) bir platform uygulama projesinden bir bit eşlem erişmek için
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) temel bir bit eşlem yüklemeye yönelik bir `Stream` nesnesi

Hiçbir sınıf eşdeğeri yoktur `Image.FromResource` yöntemleri. `UriImageSource` Sınıfı, önbelleğe almayı denetlemek istiyorsanız kullanışlıdır. `FileImageSource` Sınıfı, XAML içinde kullanışlıdır. `StreamImageSource` zaman uyumsuz yüklenmesi için yararlıdır `Stream` nesneleri, oysa `ImageSource.FromStream` uyumludur.

## <a name="platform-independent-bitmaps"></a>Platformdan bağımsız bit eşlemler

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) projesi kullanarak web üzerinde bir bit eşlem yükler `ImageSource.FromUri`. `Image` Ayarlanır `Content` özelliği `ContentPage`, sayfa boyutunu sınırlıdır. Kısıtlanmış bir bit eşleşmemin boyutundan bağımsız olarak `Image` öğe kapsayıcısı boyutunu uzatılabilir ve bit eşlem en büyük boyutuna görüntülenen `Image` eşleşmemin en boy oranını koruyarak öğesi. Alanları `Image` bit eşlem renkli ile ötesinde [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor).

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) örnek benzer, ancak yalnızca ayarlar `Source` URL'si özelliği. Dönüştürme tarafından işlenen [ `ImageSourceConverter` ](xref:Xamarin.Forms.ImageSourceConverter) sınıfı.

### <a name="fit-and-fill"></a>Uygun ve dolgu

Ayarlayarak bit eşlemin nasıl uzatılacağını denetleyebilirsiniz [ `Aspect` ](xref:Xamarin.Forms.Image.Aspect) özelliği `Image` aşağıdaki üyeleri birine [ `Aspect` ](xref:Xamarin.Forms.Aspect) sabit listesi:

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): en boy oranı (varsayılan) uyar
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): alanı doldurur, en boy oranını kabul etmez
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): alanı doldurur, ancak en boy oranı, bit eşlem parçası kırparak elde uyar

### <a name="embedded-resources"></a>Gömülü kaynaklar

Bir PCL veya PCL bir klasörde bir bit eşlem dosyası ekleyebilirsiniz. Ona bir **derleme eylemi** , **EmbeddedResource**. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) örnek nasıl kullanılacağını gösterir `ImageSource.FromResource` dosyası yüklenemiyor. Derleme adı ve isteğe bağlı klasör adından önce gelen bir nokta ve dosya adı tarafından izlenen bir nokta, sonrasında yönteme geçirilen kaynak adı oluşur.

Program kümeleri `VerticalOptions` ve `HorizontalOptions` özelliklerini `Image` için `LayoutOptions.Center`, getiren `Image` sınırlandırılmamış öğesi. `Image` Ve bit eşlem aynı boyutta:

- İOS ve Android üzerinde `Image` bit eşlemin piksel boyutudur. Bit eşlem piksel ve ekran piksel arasında bire bir eşleme yoktur.
- Evrensel Windows platformu üzerinde `Image` CİHAZDAN bağımsız birimler bit eşlemin piksel boyutudur. Çoğu cihazlarda, her bir bit eşlem piksel birden çok ekran piksel kaplar.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) örnek yerleştiren bir `Image` dikey olarak `StackLayout` XAML içinde. Adlı bir işaretleme uzantısı [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) XAML katıştırılmış kaynağa başvurması için yardımcı olur. Bu sınıf, BT derlemesinde kaynaklar yalnızca yükleyen bir kitaplıkta yerleştirilemez şekilde, makinenin bulunduğu.

### <a name="more-on-sizing"></a>Boyutlandırma hakkında daha fazla bilgi

Genellikle, tüm platformlar arasında tutarlı bir şekilde boyutu bit eşlemler için tercih edilir.
Denemeler yapmaya **StackedBitmap**, ayarlayabileceğiniz bir `WidthRequest` üzerinde `Image` dikey öğesinde `StackLayout` boyutu platformları, ancak arasında tutarlı hale getirmek için yalnızca bu tekniği kullanarak boyutunu azaltabilirsiniz.

Ayrıca `HeightRequest` görüntüyü platformlarda tutarlı boyutları, ancak bu tekniği çeşitlikleri kısıtlanmış bit eşlem genişliği sınırlar. İçinde bir yatay görüntü `StackLayout`, `HeightRequest` kaçınılmalıdır.

CİHAZDAN bağımsız birimler telefon genişliği daha geniş bir bit eşlem ile başlayan ve ayarlamak için en iyi yaklaşımdır `WidthRequest` istenen genişliğe CİHAZDAN bağımsız birimler. Bu gösterilmiştir [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) örnek.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) Lewis Carroll'ın, Bölüm 7 görüntüler *Wonderland Alice'in Serüvenleri* John Tenniel tarafından özgün çizimler ile:

[![Üç ekran görüntüsü mad Çay taraf](images/ch13fg16-small.png "Mad Hatters Çay taraf kitap metin")](images/ch13fg16-large.png#lightbox "Mad Hatters Çay taraf kitap metin")

### <a name="browsing-and-waiting"></a>Göz atma ve bekleniyor

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) örnek Xamarin web sitesinde depolanan stok görüntüleri göz atmak kullanıcının sağlar. .NET kullanan [ `WebRequest` ](xref:System.Net.WebRequest) bit eşlemler listesini içeren bir JSON dosyası indirmeniz sınıfı.

> [!NOTE]
> Xamarin.Forms programlar kullanması gereken [ `HttpClient` ](xref:System.Net.Http.HttpClient) yerine [ `WebRequest` ](xref:System.Net.WebRequest) dosyaları internet üzerinden erişmek için. 

Programın kullandığı bir [ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator) sorun olup bittiğini olduğunu belirtmek için. Gibi her bir bit eşlem yükleniyor, salt okunur [ `IsLoading` ](xref:Xamarin.Forms.Image.IsLoading) özelliği `Image` olduğu `true`. `IsLoading` Yedeklendiği bağlanabilir bir özelliğe göre bu nedenle bir `PropertyChanged` bu özellik değiştiğinde olay harekete geçirilir. Program bu olaya bir işleyici ekler ve geçerli ayarını kullanan `IsLoaded` ayarlanacak [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) özelliği `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Bit eşlemler akış

`ImageSource.FromStream` Yöntemi oluşturur bir `ImageSource` bir .NET tabanlı `Stream`. Yönteme geçirilen bir `Func` nesnesi döndüren bir `Stream` nesne.

### <a name="accessing-the-streams"></a>Akışları olarak erişme

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) örnek nasıl kullanılacağını gösterir `ImaageSource.FromStream` yöntemi bir gömülü kaynak depolanan bir bit eşlem yük ve web üzerinde bir bit eşlem yüklenemiyor.

### <a name="generating-bitmaps-at-run-time"></a>Çalışma zamanında bit eşlem oluşturma

Kod oluşturun ve ardından depolamak kolayca sıkıştırılmamış BMP dosyası biçimi tüm Xamarin.Forms platformları destekleyecek bir `MemoryStream`. Bu teknik algorithmically çalışma zamanında bit eşlemler oluşturma uygulanan gibi sağlar [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) sınıfını **Xamrin.FormsBook.Toolkit** kitaplığı.

"Kendin" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) örnek, kullanımını gösterir `BmpMaker` gradyan bir görüntü ile bir bit eşlem oluşturmak için.

## <a name="platform-specific-bitmaps"></a>Platforma özel bit eşlemler

Tüm Xamarin.Forms platformlar, bit eşlemler platform uygulaması derlemelerde depolamaya izin ver. Xamarin.Forms uygulaması tarafından alınan, bu platform bit eşlemler türlerinin [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource). Bunlar için kullanın:

- [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) özelliği [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) özelliği [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- [ `Image` ](xref:Xamarin.Forms.Button) özelliği `Button`

Platform derlemeleri, simgeler ve Karşılama ekranları için bit eşlemler zaten içerir:

- İOS projesi içerisinde de **kaynakları** klasörü
- Android projede alt **kaynakları** klasörü
- Windows projelerinde de **varlıklar** klasörü (Windows platformları bit eşlemler klasöre kısıtlamaz rağmen)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) örnek platform uygulama projelerinden bir simge görüntülemek için kod kullanır.

### <a name="bitmap-resolutions"></a>Bit eşlem çözümleri

Tüm platformlar, bit eşlem resimleri farklı cihaz çözünürlüğü için birden çok sürümünü saklama izin verir. Çalışma zamanında doğru sürüm ekranın cihaz çözünürlüğü göre yüklenir.

İos'ta bu bit eşlemler, dosya adının sonuna bir son ek olarak ayrılır:

- 160 DPI cihazlar (CİHAZDAN bağımsız birim 1 piksel) için bir sonek yok
- '@2x' soneki 320 DPI cihazlar (DIU için 2 piksel cinsinden)
- '@3x' soneki 480 DPI cihazlar (DIU için 3 piksel cinsinden)

Bir inç kare olarak gösterilmesi hedeflenen bir bit eşlem üç sürümde var:

- Görüntüm.jpg, 160 piksel kare
- MyImage@2x.jpg 320 piksel kare
- MyImage@3x.jpg 480 piksel kare

Program bu bit eşlem Görüntüm.jpg olarak başvurmak ancak doğru sürümünün çalışma zamanında ekran çözünürlüğüne göre alınır. Sınırlandırılmamış, bit eşlem 160 CİHAZDAN bağımsız birimler her zaman işlenir.

Android için bit eşlemler çeşitli alt klasörler içinde depolanan **kaynakları** klasörü:

- drawable-ldpi (düşük DPI) 120 DPI cihazlar (DIU 0.75 piksel)
- drawable-mdpi (Orta) 160 DPI cihazlar (DIU 1 piksel)
- drawable-hdpı (yüksek) 240 DPI cihazlar (DIU için 1,5 piksel cinsinden)
- drawable-xhdpi (çok yüksek) 320 DPI cihazlar (DIU için 2 piksel cinsinden)
- drawable-xxhdpi (çok çok yüksek) 480 DPI cihazlar (DIU için 3 piksel cinsinden)
- drawable-xxxhdpi (üç ek verildiği üzere Yükseliş) 640 DPI cihazlar (DIU için 4 piksel cinsinden)

Bir kare inç işlenmesine yönelik bir bit eşlem bit eşlem çeşitli sürümlerini aynı ada ancak farklı bir boyuta sahip olur ve bu klasörlerde depolanır:

- drawable-ldpi/Görüntüm.jpg, 120 piksel kare
- drawable-mdpi/Görüntüm.jpg, 160 piksel kare
- drawable-hdpı/Görüntüm.jpg, kare 240 piksel
- drawable-xhdpi/Görüntüm.jpg, 320 piksel kare
- drawable-xxhdpi/Görüntüm.jpg, 480 piksel kare
- drawable-xxxhdpi/Görüntüm.jpg, 640 piksel kare

Bit eşlem 160 CİHAZDAN bağımsız birimler her zaman işlenir. (Standart Xamarin.Forms çözüm şablonu yalnızca hdpı xhdpi ve xxhdpi klasörleri içerir.)

UWP projesi bir Ölçeklendirme çarpanı piksel CİHAZDAN bağımsız birim başına yüzde olarak örneğin oluşan bir bit eşlem adlandırma şeması destekler:

- MyImage.scale 200.jpg, 320 piksel kare

Yalnızca bazı yüzdeleri geçerlidir. Bu kitap için örnek programlardan ile görüntüleri dahil **ölçek 200** sonekleri, ancak geçerli Xamarin.Forms çözüm şablonları dahil **ölçek 100**, **ölçek 125**, **ölçek-150**, ve **ölçek 400**.

Bit eşlemler platform projelere eklerken **derleme eylemi** olmalıdır:

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP: **içerik**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) örnek oluşan iki düğme benzeri nesneleri oluşturur `Image` öğeleri ile bir `TapGestureRecognizer` yüklü. Nesneleri bir inç kare olması amaçlanmıştır. `Source` Özelliği `Image` kullanılarak ayarlanan `OnPlatform` ve `On` platformlarda farklı olabilecek dosya adlarını başvurmak için nesne. Bit eşlem resimleri hangi boyutu bit eşlem alınır ve işlenen görebilmeniz için bunların piksel boyutunu belirten bir sayı içerir.

### <a name="toolbars-and-their-icons"></a>Araç çubukları ve bunların simgeleri

Platforma özgü bit eşlem birincil kullanımlarından biridir ekleyerek oluşturulur Xamarin.Forms araç [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) nesneleri için [ `ToolbarItems` ](xref:Xamarin.Forms.Page.ToolbarItems) tarafındantanımlanankoleksiyonu`Page`. `ToobarItem` öğesinden türetilen [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) aldığı bazı özellikleri devralır.

En önemli `ToolbarItem` özellikleri:

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) platforma bağlı olarak görünebilir metin ve `Order`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) tür `FileImageSource` platforma bağlı olarak görünebilir görüntüsü ve `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) tür [ `ToolbarItemOrder` ](xref:Xamarin.Forms.ToolbarItemOrder), numaralandırması üç üyesiyle bir [ `Default` ](xref:Xamarin.Forms.ToolbarItemOrder.Default), [ `Primary` ](xref:Xamarin.Forms.ToolbarItemOrder.Primary), ve [ `Secondary` ](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

Sayısını `Primary` öğeleri üç veya dört sınırlı olmalıdır. Eklemeniz bir `Text` tüm öğeler için ayarlama. Çoğu platformda, yalnızca `Primary` öğelerini gerektirmek bir `Icon` Windows 8.1 gerektirir, ancak bir `Icon` tüm öğeler için. Simgeleri 32 CİHAZDAN bağımsız birimler kare olmalıdır. `FileImageSource` Türü, platforma özgü olduğunu gösterir.

`ToolbarItem` Ateşlenir bir [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) dokunduğunuzda, benzer olay bir `Button`. `ToolbarItem` Ayrıca destekler [ `Command` ](xref:Xamarin.Forms.MenuItem.Command) ve [ `CommandParameter` ](xref:Xamarin.Forms.MenuItem.CommandParameter) özellikleri genellikle MVVM bağlantılı olarak kullanılır. (Bkz [Bölüm 18, MVVM](chapter18.md)).

Hem iOS hem de Android araç çubuğunu görüntüleyen bir sayfa olmasını gerektiren bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) veya bir sayfa tarafından gittiğinizde bir `NavigationPage`. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) program kümeleri `MainPage` özelliği, `App` sınıfının [ `NavigationPage` Oluşturucusu](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) ile bir `ContentPage` bağımsız değişken, bir araç çubuğu oluşturma ve olay işleyicisini gösterir.

### <a name="button-images"></a>Düğme görüntüleri

Ayarlamak için platforma özel bit eşlemler kullanabilirsiniz [ `Image` ](xref:Xamarin.Forms.Button.Image) özelliği `Button` 32 CİHAZDAN bağımsız birimler karenin tarafından gösterildiği gibi bir bit eşlemi [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) örnek.

> [!NOTE]
> Düğmeler görüntülerinde kullanımını geliştirilmiştir. Bkz: [düğmeleriyle bit eşlemler kullanarak](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).

## <a name="related-links"></a>İlgili bağlantılar

- [13. Bölüm tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [13. Bölüm örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Görüntülerle Çalışma](~/xamarin-forms/user-interface/images.md)
- [Bit eşlemler düğmeleriyle kullanma](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
