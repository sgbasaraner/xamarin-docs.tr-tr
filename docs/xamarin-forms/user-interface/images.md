---
title: Xamarin.Forms görüntüleri
description: Görüntüler Xamarin.Forms ile platformlar arasında paylaşılabilir, her platform için özel olarak yüklenmiş olabilir veya görüntüleme için indirilebilir.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 47fbe67561ea9150d0fdc0b41eb5c70edbeac75e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996275"
---
# <a name="images-in-xamarinforms"></a>Xamarin.Forms görüntüleri

_Görüntüler Xamarin.Forms ile platformlar arasında paylaşılabilir, her platform için özel olarak yüklenmiş olabilir veya görüntüleme için indirilebilir._

Görüntüleri uygulama gezinti, kullanılabilirlik ve markalama önemli bir parçasıdır. Görüntüleri tüm platformlar arasında paylaşır, ancak Ayrıca olası her platformda farklı resimleri görüntülemek Xamarin.Forms uygulamaları gerekir.

Platforma özgü görüntülerini de, simgeler ve Karşılama ekranları için gereklidir; Bu, bir platform başına temelinde yapılandırılması gerekir.

Bu belge aşağıdaki konular ele alınmıştır:

- [ **Yerel görüntüleri** ](#Local_Images) -sevk iOS görüntü Retina, Android veya UWP yüksek DPI sürümleri gibi yerel çözümleri çözümleme dahil olmak üzere bu uygulama ile görüntüleri görüntüleme.
- [ **Görüntüleri katıştırılmış** ](#Embedded_Images) -bir bütünleştirilmiş kod kaynağı gömülü görüntüleri görüntüleme.
- [ **İndirilen görüntüleri** ](#Downloading_Images) - indiriliyor ve görüntüleri görüntüleme.
- [ **Simgeler ve splashscreens** ](#Icons_and_splashscreens) -platforma özgü simgeleri ve başlatma görüntüleri.

## <a name="displaying-images"></a>Görüntüleri görüntüleme

Xamarin.Forms kullanan [ `Image` ](xref:Xamarin.Forms.Image) görüntüleri bir sayfada görüntülemek için görünümü. Bu iki önemli özelliklere sahiptir:

- [`Source`](xref:Xamarin.Forms.Image.Source) -Bir [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) örneği, dosya, URI veya kaynak, görüntüyü ayarlar.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) -Nasıl (mi Genişlet, kırpma veya sinemaskop) içinde görüntülenmektedir sınırları içindeki görüntü boyutu.

[`ImageSource`](xref:Xamarin.Forms.ImageSource) örnekleri her görüntü kaynağı türü için statik yöntemler kullanarak elde edilebilir:

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) -Bir dosya adı veya her platformda çözülebilir filepath gerektirir.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) -Bir Uri nesnesinden örn gerektirir.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) -Uygulama ya da .NET Standard kitaplığı projesi ile katıştırılmış bir resim dosyası için bir kaynak tanımlayıcısı gerektirir bir **derleme eylemi: EmbeddedResource**.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) -Görüntü veri sağlayan bir akış gerektirir.

[ `Aspect` ](xref:Xamarin.Forms.Image.Aspect) Özelliği nasıl görüntünün görüntü alana uyacak şekilde ayarlanacaktır belirler:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -Resmi tamamen ve tam ekran alanı dolduracak şekilde uzatılır. Bu görüntünün bozuk neden olabilir.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -Görüntünün en boy korurken ekran alanı doldurması küçük (IE. hiçbir bozulma).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -Letterboxes (gerekliyse) görüntü görüntünün ekran alanına en uygun böylece üst/alt veya yüz bağlı olarak eklenen boş alana sahip geniş veya uzun görüntüsüdür.

Görüntüleri yüklenebileceği bir [yerel dosya](#Local_Images_in_Xaml)e [katıştırılmış kaynak](#embedded_images), veya [indirilen](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Yerel görüntüler

Görüntü dosyaları, her uygulama projesine eklenir ve paylaşılan Xamarin.Forms koddan başvuruldu. Tüm uygulamalar arasında tek bir görüntü kullanmak için *her platformda aynı dosya adı kullanılmalıdır*, ve geçerli bir Android kaynak adı olmalıdır (IE. yalnızca küçük harf, sayı, alt çizgi ve nokta izin verilir).

- **iOS** - şekilde yönetmek ve kullanmak için iOS 9 olduğundan görüntüleri desteklemek için tercih edilen **varlık Kataloğu görüntü kümeleri**, hangi içermelidir tüm sürümleri için ölçek ve çeşitli cihazları desteklemek gerekli olan görüntünün bir uygulama. Daha fazla bilgi için [bir varlık Kataloğu görüntü kümesi ekleme görüntüleri](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -yerleştirin görüntülerde **kaynakları/drawable** ile dizin **derleme eylemi: AndroidResource**. Görüntü yüksek ve düşük DPI sürümleri de sağlanabilir (uygun şekilde adlı **kaynakları** gibi alt dizinler **ldpi drawable**, **hdpı drawable**ve **xhdpi drawable**).
- **Evrensel Windows Platformu (UWP)** -görüntüleri yerleştirin uygulamanın kök dizinde **derleme eylemi: içerik**.

> [!IMPORTANT]
> İOS 9 önce görüntüleri genellikle içinde yerleştirildi **kaynakları** klasörle **derleme eylemi: BundleResource**. Ancak, bu yöntem, bir iOS uygulamasını görüntüleri ile çalışma, Apple tarafından onaylanmaz. Daha fazla bilgi için [resim boyutları ve dosya adlarını](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Dosya adlandırma ve yerleştirme için bu kurallara uymak yüklemek ve tüm platformlarda görüntüyü görüntülemek aşağıdaki XAML sağlar:

```xaml
<Image Source="waterfront.jpg" />
```

Eşdeğer C# kodu aşağıdaki gibidir:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Aşağıdaki ekran görüntüleri her platformda yerel bir görüntüyü görüntüleme sonucu göster:

[![Yerel ImageSource](images-images/local-sml.png "örnek uygulamanın yerel görüntü görüntüleme")](images-images/local.png#lightbox "örnek uygulamanın yerel görüntü görüntüleme")

Daha fazla esneklik için `Device.RuntimePlatform` özelliği kullanılabilir farklı bir resim dosyası veya bazı veya tüm platformlara yönelik yolu seçmek için bu kod örneğinde gösterildiği gibi:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Tüm platformlar arasında aynı görüntü dosya adı kullanmak için ad tüm platformlarda geçerli olmalıdır. Android drawables adlandırma kısıtlamaları vardır: yalnızca küçük harf, sayı, alt çizgi ve nokta izin verilir – ve platformlar arası uyumluluk için bu tüm platformlarda çok gelmelidir. Örnek dosya **waterfront.png** kurallarına uyar, ancak geçersiz dosya adları örnekleri içeren "su front.png", "WaterFront.png", "su front.png" ve "wåterfront.png".

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Yerel çözümleri (Retina ve yüksek DPI)

iOS, Android ve UWP nerede işletim sistemi cihazın bağlı çalışma zamanında uygun görüntüyü seçer, farklı bir resim çözümleri için destek içerir. Xamarin.Forms, dosyaları doğru adlandırılmış ve projesinde bulunan diğer çözümleri otomatik olarak desteklediği için yerel görüntüleri yüklemek için yerel platformları API'lerini kullanır.

İOS 9 görüntüler için uygun varlık Kataloğu görüntü kümesi için gereken her bir çözümü sürükleyin olduğundan görüntülerini yönetme tercih edilen yoludur. Daha fazla bilgi için [bir varlık Kataloğu görüntü kümesi ekleme görüntüleri](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

İOS 9 önce görüntünün retina sürümü de yerleştirilebilir **kaynakları** klasörü - iki ve üç kez çözümüyle bir **@2x** veya **@3x**dosya uzantısı (örn önce dosya çubuğunda sonekleri. **myimage@2x.png**). Ancak, bu yöntem, bir iOS uygulamasını görüntüleri ile çalışma, Apple tarafından onaylanmaz. Daha fazla bilgi için [resim boyutları ve dosya adlarını](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Android alternatif çözüm görüntüleri yerleştirilmelidir [-adlı dizinleri](http://developer.android.com/guide/practices/screens_support.html) Android projesinde, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Android çoklu çözüm görüntü konumu](images-images/xs-highdpisolution-sml.png "Android çoklu çözüm görüntü konumu")](images-images/xs-highdpisolution.png#lightbox "Android çoklu çözüm görüntü konumu")

UWP görüntü dosyası adları [ile olark `.scale-xxx` dosya uzantısı önce](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)burada `xxx` örn varlık için uygulanan ölçeklendirmenin yüzdesi **myimage.scale 200.png**. Görüntüleri ardından başvurulabilir kod veya Ölçek değiştiricisi olmadan XAML, örneğin henüz **myimage.png**. Platform görüntü geçerli DPI üzerinde temel en yakın uygun varlık ölçeği seçer.

### <a name="additional-controls-that-display-images"></a>Görüntüler ek denetimler

Bazı denetimler gibi bir resim görüntüleyen özelliklere sahiptir:

- [`Page`](xref:Xamarin.Forms.Page) -Tüm türetilen tür sayfasında `Page` sahip [ `Icon` ](xref:Xamarin.Forms.Page.Icon) ve [ `BackgroundImage` ](xref:Xamarin.Forms.Page.BackgroundImage) özellikleri, yerel dosya başvurusu atanabilir. Ne zaman gibi belirli koşullar altında bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) görüntüleyen bir [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), platform tarafından destelenmesi koşuluyla simgesi görüntülenir.

  > [!IMPORTANT]
  > İos'ta [ `Page.Icon` ](xref:Xamarin.Forms.Page.Icon) özelliği, bir görüntüden bir varlık Kataloğu görüntü kümesi doldurulamaz. Simge görüntüleri için bunun yerine, yük `Page.Icon` özelliğinden **kaynakları** iOS projesi klasöründe.

- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) -Sahip bir [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) yerel dosya başvurusu ayarlanabilir özelliği.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) -Sahip bir [ `ImageSource` ](xref:Xamarin.Forms.ImageCell.ImageSource) görüntüye ayarlanabilir özelliği alınan yerel bir dosyaya, bir gömülü kaynak ya da bir URI.

<a name="embedded_images" />

## <a name="embedded-images"></a>Katıştırılmış görüntüler

Katıştırılmış görüntüler (yerel görüntüleri için gibi) bir uygulama ile de gönderilir ancak her uygulamanın dosya yapısı görüntünün görüntü kopyası sahip olmak yerine dosya derleme kaynağı olarak gömülü olan. Görüntü, kod ile birlikte gibi bu görüntüleri dağıtma yöntemi bileşenleri oluşturmak için özellikle uygundur.

Bir projede bir görüntü eklemek için eklemek istediğiniz görüntü/sn seçin ve yeni öğeler eklemek için sağ tıklayın. Varsayılan olarak, görüntü bulunur **derleme eylemi: hiçbiri**; bu ayarlanması gereken **derleme eylemi: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Yapı eylemini ayarla: EmbeddedResource")

**Derleme eylemi** görüntülenebilir ve değiştirilecek **özellikleri** penceresi için bir dosya.

Bu örnekte kaynak kimliğidir **WorkingWithImages.beach.jpg**.
IDE ile birleştirerek bu varsayılan oluşturulan **varsayılan Namespace** her değer arasında bir nokta (.) dosya adı bu proje için kullanma.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](images-images/xs-buildaction.png "Yapı eylemini ayarla: EmbeddedResource")

**Derleme eylemi** da görüntülenebilir ve değiştirilecek **özellikleri** paneli bir dosya için.
Bu paneli gösterir **kaynak kimliği** kodunda referans için kullanılır. Aşağıdaki ekran görüntüsünde **kaynak kimliği** olduğu **WorkingWithImages.beach.jpg**.
IDE ile birleştirerek bu varsayılan oluşturulan **varsayılan Namespace** her değer arasında bir nokta (.) dosya adı bu proje için kullanma.
Bu kimliği, içinde düzenlenebilir **özellikleri** paneli, ancak bu örneklerin değeri **WorkingWithImages.beach.jpg** kullanılır.

![](images-images/xs-embeddedproperties.png "EmbeddedResource özellikler paneli")

-----

Projeniz klasörler halinde katıştırılmış görüntüler yerleştirirseniz, klasör adları da kaynak kodunda nokta (.) tarafından ayrılır Taşıma **beach.jpg** adlı bir klasör görüntüye **Myımages** kaynak Kimliğinde neden **WorkingWithImages.MyImages.beach.jpg**

Gömülü görüntü yüklemek için kodu basitçe geçirir **kaynak kimliği** için [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*) aşağıda gösterildiği gibi yöntemi:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(EmbeddedImages).GetTypeInfo().Assembly) };
```

> [!NOTE]
> Evrensel Windows platformu yayın modunda katıştırılmış görüntüler görüntüleme desteklemek için bu aşırı yüklemesini kullanmanız gereklidir `ImageSource.FromResource` görüntüsü için aranacak kaynak derlemeyi belirtir.

Şu anda kaynak tanımlayıcıları için örtük dönüştürme yoktur. Bunun yerine, kullanmalısınız [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*) veya `new ResourceImageSource()` katıştırılmış görüntüler yüklenemedi.

Aşağıdaki ekran görüntüleri her platformda bir katıştırılmış resim görüntüleme sonucu göster:

[![ResourceImageSource](images-images/resource-sml.png "örnek uygulama bir katıştırılmış resim görüntüleme")](images-images/resource.png#lightbox "örnek uygulama bir katıştırılmış resim görüntüleme")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>XAML kullanarak

Gelen yerleşik tür dönüştürücüsü yok olduğundan `string` için `ResourceImageSource`, bu tür görüntüleri XAML tarafından yerel olarak yüklenemiyor. Bunun yerine, basit bir özel XAML işaretleme uzantısı kullanarak resimleri yüklemek için yazılabilir bir **kaynak kimliği** XAML içinde belirtilen:

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> Evrensel Windows platformu yayın modunda katıştırılmış görüntüler görüntüleme desteklemek için bu aşırı yüklemesini kullanmanız gereklidir `ImageSource.FromResource` görüntüsü için aranacak kaynak derlemeyi belirtir.

Bu uzantıyı kullanmak için özel bir ekleyin `xmlns` XAML için proje için doğru ad alanını ve derlemeyi değerleri kullanarak. Resim kaynağını sonra bu söz dizimi kullanılarak ayarlanabilir: `{local:ImageResource WorkingWithImages.beach.jpg}`. XAML tam bir örnek aşağıda gösterilmiştir:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshooting-embedded-images"></a>Sorun giderme katıştırılmış görüntüler

<a name="Debugging_Embedded_Images" />

#### <a name="debugging-code"></a>Kodda hata ayıklama

Bazen belirli görüntü kaynağı neden yüklenmekte olmadığından anlaşılması zor olduğundan, aşağıdaki hata ayıklama kodu kaynakları doğru yapılandırılmış olduğunu doğrulamak için bir uygulama geçici olarak eklenebilir. Bilinen tüm kaynaklar için belirtilen derlemesinde gömülü çıkarır <span class="UIItem">konsol</span> yükleme kaynak hata ayıklama yardımcı olmak için.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>Diğer projelerde katıştırılmış görüntüler

Varsayılan olarak, `ImageSource.FromResource` yöntemi yalnızca görünür aynı bütünleştirilmiş kod arama görüntüleri `ImageSource.FromResource` yöntemi. Yukarıdaki hata ayıklama kodu kullanarak belirleyebilir değiştirerek belirli bir kaynağa hangi derlemelerin içeren `typeof()` deyimi bir `Type` her derlemede olduğu bilinen.

Ancak, bir katıştırılmış resim için Aranan kaynak derleme bağımsız değişken olarak belirtilebilir `ImageSource.FromResource` yöntemi:

```csharp
var imageSource = ImageSource.FromResource("filename.png", typeof(MyClass).GetTypeInfo().Assembly);
```

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Görüntüleri yükleme

Görüntüleri aşağıdaki XAML içinde gösterildiği ekran için otomatik olarak indirilebilir:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

Eşdeğer C# kodu aşağıdaki gibidir:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) Yöntemi gerektiren bir `Uri` nesne ve yeni bir [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource) , okuyan `Uri`.

Aynı zamanda aşağıdaki örnekte de çalışacak şekilde de URI dizeler için örtük bir dönüştürme yoktur:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Aşağıdaki ekran görüntüleri, uzak bir görüntü her platformda görüntüleme sonucu göster:

[![ImageSource indirilen](images-images/download-sml.png "örnek uygulamanın yüklenen bir görüntüyü görüntüleme")](images-images/download.png#lightbox "örnek uygulamanın yüklenen bir görüntüyü görüntüleme")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>İndirilen görüntüsünü önbelleğe alma

A [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource) aşağıdaki özellikleri kullanılarak yapılandırılmış indirilen görüntülerinin önbelleğe almayı da destekler:

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) -Etkin olup olmadığını önbelleğe alma (`true` varsayılan olarak).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) -A `TimeSpan` ne kadar süreyle görüntünün yerel olarak depolanacak tanımlar.

Önbelleğe alma, varsayılan olarak etkindir ve görüntünün yerel olarak 24 saat boyunca saklar. Belirli bir görüntü için önbelleğe alma devre dışı bırakmak için görüntü kaynağı aşağıdaki şekilde örneği:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Belirli bir önbellek süresini (örneğin, 5 gün) ayarlamak için aşağıdaki gibi görüntü kaynağı örneği:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Yerleşik önbelleğe alma, çok görüntüler, burada ayarlayın (bağlama görüntüyü veya istediğiniz) her hücreye kaydırma gibi senaryoları desteklemek ve hücreye geri görünüme kaydırılan, yeniden yükleme görüntüsünü ilgileniriz yerleşik önbelleği kolaylaştırır.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Simgeler ve splashscreens

İlgili değildir ancak [ `Image` ](xref:Xamarin.Forms.Image) görünümü, uygulama simgeleri ve splashscreens getirilmiştir Xamarin.Forms projelerinde görüntülerin önemli kullanın.

Simgeler ve Xamarin.Forms uygulamaları için splashscreens ayarlama, her uygulama projeleri içinde yapılır. Bu, iOS, Android ve UWP için görüntüleri boyutlandırılmış doğru oluşturma anlamına gelir. Bu görüntüleri adlı ve her bir platform gereksinimlerine göre bulunur.

## <a name="icons"></a>Simgeler

Bkz: [iOS görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md), [Google yansır](http://developer.android.com/design/style/iconography.html), ve [kutucuk ve simgesi varlıklar için yönergeler](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) bu uygulama kaynakları oluşturma hakkında daha fazla bilgi.

## <a name="splashscreens"></a>Splashscreens

Yalnızca iOS ve UWP uygulamaları (Başlangıç ekranı veya varsayılan görüntü olarak da bilinir) bir splashscreen gerektirir.

Belgelerine başvurmak [iOS görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) ve [tanıtım ekranlar](/windows/uwp/launch-resume/splash-screens/) Windows Dev Center üzerinde.

## <a name="summary"></a>Özet

Xamarin.Forms platformlar arasında kullanılacak görüntünün aynısını veya belirtilmesi platforma özel görüntüleri izin vererek bir platformlar arası uygulama görüntüleri dahil için farklı yollar sunar. İndirilen görüntüleri otomatik olarak önbelleğe alınır genel bir kodlama senaryoyu otomatikleştirme.

Uygulama simgesi ve splashscreen görüntüleri ayarı olan ve Xamarin.Forms olmayan uygulamalar için - yapılandırılmış platforma özel uygulamalar için kullanılan aynı yönergeleri izleyin.

## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithImages (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md)
- [Android yansır](http://developer.android.com/design/style/iconography.html)
- [Kutucuk ve simgesi varlıklar için yönergeler](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
