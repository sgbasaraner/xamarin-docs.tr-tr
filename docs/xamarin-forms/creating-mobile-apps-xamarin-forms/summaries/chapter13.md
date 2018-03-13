---
title: "Bölüm 13 özeti. Bit eşlemler"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 74e5e47a481d02fe11be4b770b818d2c88b517f7
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>Bölüm 13 özeti. Bit eşlemler

Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğesi bir bit eşlem görüntüler. Tüm Xamarin.Forms platformlar JPEG, PNG, GIF ve BMP dosya biçimleri için destek.

Bit eşlemler Xamarin.Forms içinde dört yerlerden getirir:

- Web üzerinden bir URL tarafından belirtilen
- Katıştırılmış bir kaynak olarak ortak taşınabilir sınıf kitaplığı
- Katıştırılmış bir kaynak olarak platform uygulama projeleri
- Bir .NET tarafından başvurulan bir yerden `Stream` dahil olmak üzere, nesne `MemoryStream`

Bit eşlem kaynakları platform projelerinde platforma özgü işlenirken PCL bit eşlem kaynaklarında platformdan bağımsız.

Bit eşlem ayarlayarak belirtilen [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) özelliği `Image` türünde bir nesneye [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), bir Özet sınıf üç türevleri:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) bir bit eşlem temel web üzerinden erişmek için bir `Uri` nesne kümesine kendi [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) özelliği
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) bir platform uygulama projesinde depolanan bir bit eşlem erişmek için temel ayarlamak bir klasör ve dosya yolu, [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) özelliği
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) bir .NET kullanarak bir bit eşlem yüklenmesi için `Stream` döndürerek belirtilen nesne bir `Stream` gelen bir `Func` ayarlamak kendi [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) özelliği

Alternatif olarak (ve daha sık) aşağıdaki statik yöntemlerini kullanabilirsiniz `ImageSource` sınıfı, tüm hangi geri dönüş `ImageSource` nesneler:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) bir bit eşlem temel web üzerinden erişmek için bir `Uri` nesnesi
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) PCL, uygulama içinde katıştırılmış bir kaynağı olarak depolanan bir bit eşlem erişmek veya [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) veya [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) bir bit eşlem başka bir kaynak derlemede erişmek için
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) bir platform uygulama projesinde bir bit eşlem erişmek için
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) temel bir bit eşlem yüklenmesi için bir `Stream` nesnesi

Hiçbir sınıfı eşdeğeri yoktur `Image.FromResource` yöntemleri. `UriImageSource` Sınıfı, önbelleğe alınmasını denetleyen gerekiyorsa yararlıdır. `FileImageSource` Sınıfı XAML'de yararlıdır. `StreamImageSource` zaman uyumsuz yüklenmesi için yararlıdır `Stream` , ancak nesneleri `ImageSource.FromStream` uyumludur.

## <a name="platform-independent-bitmaps"></a>Platformdan bağımsız bit eşlemler

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) proje yükleyen bir bit eşlem kullanarak web üzerinden `ImageSource.FromUri`. `Image` Ayarlanır `Content` özelliği `ContentPage`, sayfa boyutu için sınırlı değildir. Bit eşlem'ın boyutu, kısıtlı bir bağımsız olarak `Image` öğe kapsayıcısı boyutunu uzatılabilir ve bit eşlem en büyük boyutuna içinde görüntülenir `Image` bit eşlem'ın en boy oranını koruyarak öğesi. Alanlarının `Image` bit eşlem renkli ile ötesine [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/).

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) örnek benzer ancak yalnızca ayarlar `Source` URL'ye özelliği. Dönüştürme tarafından işlenen [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) sınıfı.

### <a name="fit-and-fill"></a>Uyum ve doldurma

Bit eşlem ayarlayarak nasıl uzatılmış denetleyebilirsiniz [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) özelliği `Image` aşağıdaki üyeleri birine [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) numaralandırma:

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): en boy oranı (varsayılan) dikkate alır
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): alan doldurur, en boy oranını uymaz
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): alan doldurur, ancak en boy oranı, bit eşlem parçası kırpma tarafından gerçekleştirilen dikkate alır

### <a name="embedded-resources"></a>Gömülü kaynaklar

Bir bit eşlem dosyası bir PCL veya PCL klasöründe ekleyebilirsiniz. Şablona bir **yapı eylemi** , **EmbeddedResource**. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) örnek nasıl kullanılacağını gösteren `ImageSource.FromResource` dosyası yüklenemiyor. Derleme adı ve ardından isteğe bağlı klasör adı bir nokta ve dosya adı tarafından izlenen bir nokta sonrasında, yönteme geçirilen kaynak adı oluşur.

Program kümeleri `VerticalOptions` ve `HorizontalOptions` özelliklerini `Image` için `LayoutOptions.Center`, hangi yapar `Image` Kısıtlanmamış öğesi. `Image` Ve bit eşlem aynı boyutu:

- İOS ve Android, `Image` bit eşlem piksel boyutudur. Bit eşlem ve ekran pikseller arasında bire bir eşleme yoktur.
- Windows çalışma zamanı platformlarda `Image` bit eşlem CİHAZDAN bağımsız birimler piksel boyutudur. Çoğu aygıtlarda birden fazla ekran piksel her bit eşlem piksel kaplar.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) örnek yerleştiren bir `Image` dikey olarak `StackLayout` XAML'de. Adlı bir işaretleme uzantısı [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) XAML katıştırılmış kaynağa başvurması için yardımcı olur. Bu sınıf, kaynakları hangi bu derlemesinden yalnızca yükler kitaplığa yerleştirilemez şekilde, makinenin bulunduğu.

### <a name="more-on-sizing"></a>Boyutlandırma hakkında daha fazla bilgi

Genellikle, tüm platformlar arasında tutarlı bir şekilde boyutu bit eşlemler için tercih edilir.
İle denemeler **StackedBitmap**, ayarlayabileceğiniz bir `WidthRequest` üzerinde `Image` dikey öğesinde `StackLayout` boyutu platformları, ancak arasında tutarlı hale getirmek için yalnızca bu teknik kullanılarak boyutunu azaltabilirsiniz.

Ayrıca ayarlayabilirsiniz `HeightRequest` görüntüyü platformlarda tutarlı boyutları, ancak bu teknik yönlülük bit eşlem kısıtlanmış genişliğini sınırlar. Dikey bir görüntüde için `StackLayout`, `HeightRequest` kaçınılmalıdır.

CİHAZDAN bağımsız birimler telefon genişliği daha geniş bir bit eşlem ile başlamalı ve ayarlamak için en iyi yaklaşımdır `WidthRequest` istenen genişliğe CİHAZDAN bağımsız birimler. Bu, gösterilmiştir [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) örnek.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) Gamze Carroll'ın, Bölüm 7 görüntüler *Wonderland Alice'in Serüvenleri* John Tenniel tarafından özgün çizimler ile:

[![Mad Çay taraf Üçlü ekran](images/ch13fg16-small.png "Mad Hatters Çay taraf kitap metin")](images/ch13fg16-large.png#lightbox "Mad Hatters Çay taraf kitap metin")

### <a name="browsing-and-waiting"></a>Göz atma ve bekleniyor

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) örnek Xamarin web sitesinde depolanan stok görüntüleri göz atmak kullanıcı izin verir. .NET kullanan `WebRequest` bit eşlemler listesini içeren bir JSON dosyası indirmek için sınıf.

Program kullanan bir [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) bir şey olacağına olduğunu belirtmek için. Gibi her bit eşlem'i yüklerken, salt okunur [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) özelliği `Image` olan `true`. `IsLoading` Özelliği yedeklendiği bağlanabilir bir özelliğe göre bu nedenle bir `PropertyChanged` bu özelliği değiştiğinde olay tetiklenir. Program bu olay için bir işleyici ekler ve geçerli ayarını kullanır `IsLoaded` ayarlamak için [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) özelliği `ActivityIndicator`.

## <a name="streaming-bitmaps"></a>Bit eşlemler akış

`ImageSource.FromStream` Yöntemi oluşturur bir `ImageSource` bir .NET tabanlı `Stream`. Yöntemine geçirilen bir `Func` döndürür nesnesi bir `Stream` nesnesi.

### <a name="accessing-the-streams"></a>Akışlar erişme

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) örnek nasıl kullanılacağını gösteren `ImaageSource.FromStream` yöntemi katıştırılmış bir kaynağı depolanan bir bit eşlem yük ve web üzerinden bir bit eşlem'i yüklemek için.

### <a name="generating-bitmaps-at-run-time"></a>Bit eşlemler çalışma zamanında oluşturma

Kodda oluşturmak ve ardından depolamak kolay sıkıştırılmamış BMP dosyası biçiminde tüm Xamarin.Forms platformlarını destekleyen bir `MemoryStream`. Bu teknik algorithmically çalışma zamanında bit eşlemler oluşturma uygulanan gibi verir [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) sınıfını **Xamrin.FormsBook.Toolkit** kitaplığı.

"Kendin" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) kullanımını gösteren örnek `BmpMaker` bir bit eşlem gradyan bir görüntü oluşturmak için.

## <a name="platform-specific-bitmaps"></a>Platforma özgü bit eşlemler

Tüm Xamarin.Forms platformlar, bit eşlemler platform uygulama derlemelerde depolamaya izin ver. Xamarin.Forms uygulaması tarafından alınan, bu platform bit eşlemler türlerinin [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/). Bunlar için kullanın:

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) özelliği [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) özelliği [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) özelliği `Button`

Platform derlemesi zaten simgeler ve başlangıç ekranında için bit eşlemler içerir:

- İOS projesinde içinde **kaynakları** klasörü
- Android projede alt **kaynakları** klasörü
- Windows projelerinde içinde **varlıklar** klasörü (Windows platformları bit eşlemler klasöre kısıtlamaz rağmen)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) örnek kod platform uygulama projelerden simge görüntülemek için kullanır.

### <a name="bitmap-resolutions"></a>Bit eşlem çözümleri

Tüm platformlar farklı cihaz çözünürlükleri için görüntüleri bit eşlem birden fazla sürümünü depolamaya izin ver. Çalışma zamanında uygun sürüm ekranın aygıt çözünürlüğü göre yüklenir.

İos'ta, bu bit eşlemler filename ekini tarafından ayrılır:

- Sonek 160 DPI cihazlar (CİHAZDAN bağımsız birim 1 piksel)
- '@2x' soneki 320 DPI cihazlar (DIU 2 piksel)
- '@3x' soneki 480 DPI cihazlar (DIU 3 piksel)

Bir inç kare olarak görüntülenecek hedeflenen bir bit eşlem üç sürümlerde var:

- Görüntüm.jpg, 160 piksel kare
- MyImage@2x.jpg kare 320 piksel
- MyImage@3x.jpg kare 480 piksel

Program bu bitmap Görüntüm.jpg olarak bakın, ancak uygun sürüm çalışma zamanında ekran çözünürlüğü üzerinde temel alınır. Kısıtlanmamış, bit eşlem her zaman 160 CİHAZDAN bağımsız birimler işlemez.

Android için bit eşlemler çeşitli alt klasörlerinde depolanan **kaynakları** klasörü:

- drawable-ldpi (düşük DPI) 120 DPI cihazlar (DIU; 0,75 piksel)
- drawable-mdpi (Orta) 160 DPI cihazlar (DIU 1 piksel)
- drawable-hdpi (yüksek) 240 DPI cihazlar (DIU için 1,5 piksel cinsinden)
- drawable-xhdpi (çok yüksek) 320 DPI cihazlar (DIU 2 piksel)
- drawable-xxhdpi (fazladan çok yüksek) 480 DPI cihazlar (DIU 3 piksel)
- drawable-xxxhdpi (üç ek highs) 640 DPI cihazlar (DIU 4 piksel)

Bir kare inç işlenmesine yönelik bir bit eşlem için bit eşlem'in çeşitli sürümlerine aynı adlı ancak farklı bir boyutu vardır ve bu klasörlerde depolanan:

- drawable-ldpi/MyImage.jpg at 120 pixels square
- drawable-mdpi/Görüntüm.jpg, 160 piksel kare
- drawable-hdpi/Görüntüm.jpg, kare 240 piksel
- drawable-xhdpi/Görüntüm.jpg, 320 piksel kare
- drawable-xxhdpi/Görüntüm.jpg, kare 480 piksel
- drawable-xxxhdpi/Görüntüm.jpg, kare 640 piksel

Bit eşlem her zaman 160 CİHAZDAN bağımsız birimler işlemez. (Standart Xamarin.Forms çözüm şablonu yalnızca hdpi, xhdpi ve xxhdpi klasörleri içerir.)

Windows çalışma zamanı projeleri, CİHAZDAN bağımsız birim başına piksel cinsinden ölçekleme oranı yüzde olarak örneğin oluşan şema adlandırma bir bit eşlem destekler:

- MyImage.scale-200.jpg, 320 piksel kare

Yalnızca bazı yüzdeleri geçerlidir. Bu kitap örnek programlar yalnızca görüntülerle dahil **ölçek-200** sonekleri ancak dahil geçerli Xamarin.Forms çözümünü şablonları **ölçek 100**, **ölçek 125**, **ölçek 150**, ve **ölçek 400**. 

Bit eşlemler platform projelerine eklerken **yapı eylemi** olmalıdır:

- iOS: **BundleResource**
- Android: **AndroidResource**
- Windows çalışma zamanı: **içeriği**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) örnek oluşan iki düğmesi gibi nesneler oluşturur `Image` öğeleriyle bir `TapGestureRecognizer` yüklü. Nesnelerin tek inç kare olması amaçlanmıştır. `Source` Özelliği `Image` kullanılarak ayarlanan `OnPlatform` ve `On` farklı dosya adları platformlarda başvurmak için nesneleri. Bit eşlem görüntüleri hangi boyutu bit eşlem alınır ve işlenen görebilmeleri piksel boyutlarını gösteren numaralarını içerir.

### <a name="toolbars-and-their-icons"></a>Araç çubukları ve bunların simgeleri

Platforma özgü bit eşlemler birincil kullanımlarını biri ekleyerek oluşturulan Xamarin.Forms araç [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) nesneleri [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) tarafındantanımlanankoleksiyonu`Page`. `ToobarItem` türetilen [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) aldığı bazı özellikleri devralır.

En önemli `ToolbarItem` özellikleri şunlardır:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) platforma bağlı olarak görünebilir metin ve `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) tür `FileImageSource` platformuna bağlı olarak görünebilir görüntüsünün ve `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) tür [ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/), numaralandırması üç üyeleriyle bir [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/), [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/), ve [ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

Sayısı `Primary` öğeleri üç veya dört ile sınırlı. Eklemeniz bir `Text` tüm öğeler için ayarlama. Çoğu platformda, yalnızca için `Primary` öğeleri gerektiren bir `Icon` ancak Windows 8.1 gerektirir bir `Icon` tüm öğeler için. Simgeler kare 32 CİHAZDAN bağımsız birimler olmalıdır. `FileImageSource` Platforma özgü türünü belirtir.

`ToolbarItem` Ateşlenir bir [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) dokunduğunuz, benzer olay bir `Button`. `ToolbarItem` Ayrıca destekler [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) ve [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) özellikleri genellikle MVVM bağlantılı olarak kullanılır. (Bkz [Bölüm 18, MVVM](chapter18.md)).

İOS ve Android gerektiren bir araç çubuğu görüntüleyen bir sayfa olmasını bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) veya tarafından gittiğinizde sayfası bir `NavigationPage`. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) program kümeleri `MainPage` özelliği, `App` sınıfının [ `NavigationPage` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) ile bir `ContentPage` bağımsız değişken, bir araç çubuğu oluşturma ve olay işleyicisi gösterir.

### <a name="button-images"></a>Düğme resimlerini

Ayrıca platforma özgü bit eşlemler ayarlamak için kullanabileceğiniz [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) özelliği `Button` 32 CİHAZDAN bağımsız birimler kare tarafından gösterildiği gibi bir bit eşlem için [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) örnek.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 13 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [Bölüm 13 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [Görüntülerle Çalışma](~/xamarin-forms/user-interface/images.md)
