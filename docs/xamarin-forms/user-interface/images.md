---
title: Xamarin.Forms görüntülerde
description: Görüntüleri Xamarin.Forms ile platform genelinde paylaşılabilir, özellikle her platform için yüklenen olabilir veya görüntülenmek üzere indirilebilir.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 6d3e5e61069723b0910b092da6631d5dc4ad8629
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244551"
---
# <a name="images-in-xamarinforms"></a>Xamarin.Forms görüntülerde

_Görüntüleri Xamarin.Forms ile platform genelinde paylaşılabilir, özellikle her platform için yüklenen olabilir veya görüntülenmek üzere indirilebilir._

Görüntüleri uygulama gezinme, kullanılabilirlik ve markalama önemli bir parçasıdır. Xamarin.Forms uygulamaları görüntüleri tüm platformlarda paylaşır, ancak Ayrıca olası her platformda farklı görüntüleri göstermek gerekir.

Platforma özgü görüntüleri de simgeler ve başlangıç ekranında için gereklidir; Bu, bir platform başına temelinde yapılandırılması gerekir.

Bu belge aşağıdaki konular ele alınmıştır:

- [ **Yerel görüntü** ](#Local_Images) -iOS görüntüyü Retina, Android veya UWP yüksek DPI sürümleri gibi yerel çözümleri çözme dahil olmak üzere uygulama ile birlikte görüntüleri görüntüleme.
- [ **Görüntüleri katıştırılmış** ](#Embedded_Images) -katıştırılmış bir derleme kaynağı olarak görüntüleri görüntüleme.
- [ **İndirilen görüntüleri** ](#Downloading_Images) - karşıdan yükleme ve görüntüleri görüntüleme.
- [ **Simgeler ve splashscreens** ](#Icons_and_splashscreens) -platforma özgü simgeler ve başlangıç görüntüler.

## <a name="displaying-images"></a>Görüntüleri görüntüleme

Xamarin.Forms kullanan [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) görüntüleri bir sayfada görüntülemek için görünümü. İki önemli özelliklere sahiptir:

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) -Bir [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) örneği, dosya, URI veya görüntülemek için görüntüyü ayarlar kaynak.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -(Esnetme, kırpma veya mektup kutusu verilip) içinde görüntülenmektedir sınırları içinde görüntü boyutunu nasıl.

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) örnekler, her görüntü kaynağı türü için statik yöntemler kullanılarak alınabilir:

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -Bir dosya adı veya her platformda çözülebilir filepath gerektirir.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -Bir Uri nesnesinden ör gerektirir.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -Kaynak tanımlayıcısı ile uygulama ya da .NET standart kitaplığı proje katıştırılmış bir görüntü dosyasına gerektiren bir **yapı eylemi: EmbeddedResource**.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -Görüntü veri sağlayan bir akış gerektirir.

[ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) Özelliği, görüntü alanını sığması için resmin nasıl ölçeklendirilir belirler:

- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/) -Resmi tamamen ve tam olarak görüntüleme alanı dolduracak şekilde uzatılır. Bu, görüntünün bozulmuş neden olabilir.
- [`AspectFill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFill/) -En boy korurken görüntü alanını doldurması resmi kırpar (IE. hiçbir bozulma).
- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/) -Letterboxes (gerekliyse) görüntüsü böylece görüntünün tamamını ekran alanına sığar üst/alt veya kenara mı bağlı olarak eklenen boş alana sahip geniş veya uzun görüntüdür.

Görüntüleri yüklenebileceği bir [yerel dosya](#Local_Images_in_Xaml), bir [katıştırılmış kaynak](#embedded_images), veya [indirilen](#Downloading_Images).

<a name="Local_Images" />

## <a name="local-images"></a>Yerel görüntüler

Görüntü dosyaları her uygulama projesine eklendi ve paylaşılan Xamarin.Forms koddan başvurulabilir. Tüm uygulamalar arasında tek bir görüntü kullanılacak *aynı adı her platformda kullanılmalıdır*, ve geçerli bir Android kaynak adı olmalıdır (IE. yalnızca küçük harfler, rakamlar, alt çizgi ve süresi izin verilir).

- **iOS** - tercih edilen yönetmek ve kullanmak için iOS 9 olduğundan görüntüleri desteklemek için bir yol **varlık Kataloğu görüntü kümeleri**, hangi içermelidir tüm sürümleri için Etkenler ölçekleme ve çeşitli aygıtları desteklemek gerekli olan görüntünün bir uygulama. Daha fazla bilgi için bkz: [ekleme görüntülere bir varlık Kataloğu resmi ayarlama](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** -yerleştirin görüntülerinde **kaynakları/drawable** ile dizin **yapı eylemi: AndroidResource**. Görüntüyü yüksek ve düşük DPI sürümleri de sağlanan (uygun şekilde adlı **kaynakları** gibi alt dizinleri **ldpi drawable**, **hdpi drawable**ve **xhdpi drawable**).
- **Evrensel Windows Platformu (UWP)** -yerleştirin görüntüleri ile uygulamanın kök dizininde **yapı eylemi: içerik**.

> [!IMPORTANT]
> İOS 9 önce görüntüleri genellikle içinde yerleştirildi **kaynakları** klasörüyle **yapı eylemi: BundleResource**. Ancak, bu yöntem, bir iOS uygulaması görüntülerle çalışma Apple'nın kullanım dışıdır. Daha fazla bilgi için bkz: [görüntü boyutları ve dosya adları](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Dosya adlandırma ve yerleştirme için bu kurallara uymak yüklemek ve tüm platformlarda görüntüyü görüntülemek aşağıdaki XAML sağlar:

```xaml
<Image Source="waterfront.jpg" />
```

Eşdeğer C# kod aşağıdaki gibidir:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

Aşağıdaki ekran görüntüleri her platformda yerel görüntü görüntüleme sonucu göster:

[![Yerel ImageSource](images-images/local-sml.png "örnek uygulama yerel görüntü görüntüleme")](images-images/local.png#lightbox "örnek uygulama yerel görüntü görüntüleme")

Daha fazla esneklik için `Device.RuntimePlatform` özelliği farklı bir resim dosyası veya bazı veya tüm platformlar için yol seçmek için bu kod örneğinde gösterildiği olarak kullanılabilir:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> Tüm platformlarda aynı görüntü filename kullanmak için adın tüm platformlarda geçerli olması gerekir. Adlandırma kısıtlamaları Android drawables sahip – yalnızca küçük harf, sayı, alt çizgi ve nokta izin verilir – ve platformlar arası uyumluluk için bu tüm diğer platformlarda çok gelmelidir. Örnek filename **waterfront.png** aşağıdaki kurallar, ancak geçersiz dosya adları örnekleri dahil "su front.png", "WaterFront.png", "su-front.png" ve "wåterfront.png".

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>Yerel çözünürlük (Retina ve yüksek DPI)

iOS, Android ve UWP burada işletim sistemi cihazın özelliklerine göre çalışma zamanında uygun görüntüyü seçer, farklı bir resim çözümleri için destek içerir. Xamarin.Forms dosyaları doğru adlı ve projesinde bulunan diğer çözümleri otomatik olarak desteklediği için yerel görüntüleri yüklemek için yerel platformları API'lerini kullanır.

İOS 9 uygun varlık Kataloğu görüntü kümesi için gereken her bir çözümü için görüntüleri sürükleme olduğundan görüntülerini yönetmek için tercih edilen yolu. Daha fazla bilgi için bkz: [ekleme görüntülere bir varlık Kataloğu resmi ayarlama](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

İOS 9 önce görüntünün retina sürümü olarak yerleştirilemedi **kaynakları** klasör - iki ve üç kez çözümüyle bir **@2x** veya **@3x**dosya uzantısını (ör önce filename eklerinde. **myimage@2x.png**). Ancak, bu yöntem, bir iOS uygulaması görüntülerle çalışma Apple'nın kullanım dışıdır. Daha fazla bilgi için bkz: [görüntü boyutları ve dosya adları](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Android alternatif çözüm görüntüleri yerleştirilmelidir [özel adlı dizinleri](http://developer.android.com/guide/practices/screens_support.html) Android projesinde, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Android birden çok çözümleme görüntü konumu](images-images/xs-highdpisolution-sml.png "Android birden çok çözümleme görüntü konumu")](images-images/xs-highdpisolution.png#lightbox "Android birden çok çözümleme görüntü konumu")

UWP resim dosya adları [ile sonekine `.scale-xxx` dosya uzantısı önce](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), burada `xxx` varlık için örneğin uygulanan ölçeklendirme yüzdesidir **myimage.scale 200.png**. Görüntüleri ardından başvurulabilir kod veya XAML Ölçek değiştiricisi olmadan, örneğin yalnızca için **myimage.png**. Platform üzerinde görüntünün geçerli DPI göre en yakın uygun varlık ölçek seçer.

### <a name="additional-controls-that-display-images"></a>Görüntüleri göstermek ek denetimler

Bir görüntü gibi görüntülemek özelliklerini bazı denetimler vardır:

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -Any türetilen tür sayfa `Page` sahip [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) ve [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) bir yerel dosya başvuru atanabilir özellikleri. Belirli koşullar altında ne zaman gibi bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) görüntüleyen bir [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), platform tarafından destekleniyorsa simgesi görüntülenir.

  > [!IMPORTANT]
  > İOS, [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) özelliği, bir varlık Kataloğu görüntü kümesindeki bir görüntüden doldurulamaz. Bunun yerine, simge görüntüleri için yük `Page.Icon` özelliğinden **kaynakları** iOS projesi klasöründe.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) -Sahip bir [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) bir yerel dosya referansı ayarlama özelliği.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) -Sahip bir [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) görüntüye ayarlanabilir özelliği alınan yerel bir dosya, katıştırılmış bir kaynağı ya da bir URI.

<a name="embedded_images" />

## <a name="embedded-images"></a>Katıştırılmış görüntüler

Katıştırılmış görüntüler de (yerel görüntüleri için gibi) bir uygulama ile gönderilen ancak görüntü kopyası her uygulamanın dosya yapısı görüntü sahip olmak yerine dosya bütünleştirilmiş bir kaynak olarak katıştırılır. Görüntü kod ile birlikte gibi görüntüleri dağıtma yöntemi bileşenleri oluşturmak için özellikle uygundur.

Görüntüyü bir proje eklemek için yeni öğeler eklemek ve eklemek istediğiniz görüntü/s seçmek için sağ tıklatın. Varsayılan olarak, görüntünün olacaktır **yapı eylemi: hiçbiri**; bu ayarlamak için gereken **yapı eylemi: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "Yapı eylemini ayarlayın: EmbeddedResource")

**Yapı eylemi** görüntülenebilir ve değiştirilecek **özellikleri** penceresi bir dosya için.

Bu örnekte kaynak kimliğidir **WorkingWithImages.beach.jpg**.
IDE birleştirerek bu varsayılan üretti **varsayılan Namespace** her değer arasında bir nokta (.) dosya adı bu proje için kullanma.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![](images-images/xs-buildaction.png "Yapı eylemini ayarlayın: EmbeddedResource")

**Derleme eylemi** da görüntülenebilir ve değiştirilecek **özellikleri** paneli bir dosya için.
Bu paneli gösterir **kaynak kimliği** kodunda referans için kullanılır. Aşağıdaki ekran görüntüsünde **kaynak kimliği** olan **WorkingWithImages.beach.jpg**.
IDE birleştirerek bu varsayılan üretti **varsayılan Namespace** her değer arasında bir nokta (.) dosya adı bu proje için kullanma.
Bu kimliği düzenlenebilecek **özellikleri** paneli, ancak bu örnekler için değer **WorkingWithImages.beach.jpg** kullanılır.

![](images-images/xs-embeddedproperties.png "EmbeddedResource özellikleri paneli")

-----

Projenizi içinde klasörler halinde katıştırılmış görüntüler yerleştirirseniz, klasör adları ayrıca kaynak kodunda nokta (.) ile ayrılmıştır Taşıma **beach.jpg** adlı bir klasör görüntüsüne **MyImages** kaynak Kimliğinde oluşturacağı **WorkingWithImages.MyImages.beach.jpg**

Katıştırılmış bir resim yüklemek için kodu yalnızca geçirir **kaynak kimliği** için [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) aşağıda gösterildiği gibi yöntemi:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg") };
```

Şu anda kaynak tanımlayıcılar için örtük dönüştürme yok. Bunun yerine, kullanmanız gereken [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) veya `new ResourceImageSource()` katıştırılmış görüntüler yüklenemiyor.

Aşağıdaki ekran görüntüleri her platformda bir katıştırılmış resim görüntüleme sonucu göster:

[![ResourceImageSource](images-images/resource-sml.png "örnek bir katıştırılmış resim görüntüleme uygulama")](images-images/resource.png#lightbox "örnek uygulama bir katıştırılmış resim görüntüleme")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>XAML kullanma

Hiçbir yerleşik türü dönüştürücü gelen olduğundan `string` için `ResourceImageSource`, bu tür görüntüleri XAML tarafından yerel olarak yüklenemiyor. Bunun yerine, basit bir özel XAML biçimlendirme uzantısı görüntüleri kullanarak yüklemek için yazılabilir bir **kaynak kimliği** XAML'de belirtilen:

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
   var imageSource = ImageSource.FromResource(Source);

   return imageSource;
 }
}
```

Bu uzantıyı kullanmak için özel bir ekleyin `xmlns` XAML için proje için doğru ad alanını ve derleme değerleri kullanarak. Görüntü kaynağı sonra Bu sözdizimi kullanılarak ayarlanabilir: `{local:ImageResource WorkingWithImages.beach.jpg}`. Tam bir XAML örnek aşağıda verilmiştir:

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

Bazen belirli görüntü kaynağı neden yüklenen değil anlaşılması zor olduğundan, aşağıdaki hata ayıklama kodu kaynakların doğru şekilde yapılandırıldığını doğrulamak için bir uygulama geçici olarak eklenebilir. Bilinen tüm kaynaklar için verilen derleme katıştırılmış çıkarır <span class="UIItem">konsol</span> kaynak sorunları yüklenirken hata ayıklama yardımcı olacak.

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

#### <a name="images-embedded-in-other-projects-dont-appear"></a>Diğer projelerinde katıştırılmış resimleri görünmüyor

`Image.FromResource` yalnızca aynı bütünleştirilmiş kod arama görüntülerinde arar `FromResource`. Yukarıdaki hata ayıklama kodu kullanarak belirleyebilir değiştirerek belirli bir kaynak hangi derlemelerin içeren `typeof()` ifadesine bir `Type` her derlemede olduğu bilinen.

<a name="Downloading_Images" />

## <a name="downloading-images"></a>Yükleme görüntüleri

Görüntüleri aşağıdaki XAML'de gösterildiği gibi görüntülenmek üzere otomatik olarak indirilebilir:

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

Eşdeğer C# kod aşağıdaki gibidir:

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) Yöntemi gerektiren bir `Uri` nesne ve yeni bir döndürür [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) , okur `Uri`.

Aynı zamanda aşağıdaki örnekte de çalışacak şekilde de URI dizeleri için örtük bir dönüştürme bulunmaktadır:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

Aşağıdaki ekran görüntüleri her platformda uzak bir görüntü görüntüleme sonucu göster:

[![ImageSource indirilen](images-images/download-sml.png "örnek uygulama indirilen görüntü görüntüleme")](images-images/download.png#lightbox "örnek uygulama indirilen görüntü görüntüleme")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>İndirilen görüntü önbelleğe alma

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) aynı zamanda aşağıdaki özellikleri kullanılarak yapılandırılmış indirilen görüntülerinin önbelleğe almayı destekler:

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) -Önbelleğe alma etkinleştirilip etkinleştirilmediği (`true` varsayılan olarak).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -A `TimeSpan` tanımlayan görüntü ne kadar yerel olarak depolanır.

Önbelleğe alma, varsayılan olarak etkindir ve görüntünün 24 saat için yerel olarak depolar. Belirli bir görüntü için önbelleğe almayı devre dışı bırakmak için aşağıdaki gibi görüntü kaynağı örneği:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

Belirli önbellek süresini (örneğin, 5 gün) ayarlamak için aşağıdaki gibi görüntü kaynağı örneği:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Yerleşik önbelleğe alma, çok hücre geri görünüme kaydırılan zaman yeniden yükleme görüntüsünü ilgilenebilmek yerleşik önbellek sağlar ve her hücrede görüntüler, buradan ayarlayın (bağlamak bir görüntü veya) kaydırma gibi senaryoları desteklemek kolaylaştırır.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>Simgeler ve splashscreens

İlgili değildir ancak [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) görünümü, uygulama simgeleri ve splashscreens olan de Xamarin.Forms projelerinde görüntülerinin önemli kullanın.

Simgeler ve splashscreens Xamarin.Forms uygulamalar için ayarlama, her uygulama projeleri yapılır. Bu, iOS, Android ve UWP görüntülerinin boyutta doğru şekilde oluşturmak anlamına gelir. Bu görüntüleri adlı ve her platform gereksinimlerine göre bulunur.

## <a name="icons"></a>Simgeler

Bkz: [iOS görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md), [Google yansır](http://developer.android.com/design/style/iconography.html), ve [döşeme ve simge varlıklar kılavuzları](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) bu uygulama kaynakları oluşturma hakkında daha fazla bilgi için.

## <a name="splashscreens"></a>Splashscreens

Yalnızca iOS ve UWP uygulamaları (başlangıç ekranından veya varsayılan görüntü olarak da bilinir) bir KarşılamaEkranı gerektirir.

İçin belgelerine başvurun [iOS görüntülerle çalışma](~/ios/app-fundamentals/images-icons/index.md) ve [tanıtım ekranlar](/windows/uwp/launch-resume/splash-screens/) Windows Dev Center üzerinde.

## <a name="summary"></a>Özet

Xamarin.Forms birkaç platformda kullanılmak üzere aynı görüntü için veya platforma özgü görüntüleri için belirtilmesine izin bir uygulamada platformlar arası, görüntüleri dahil etmek için farklı yolla sunar. İndirilen resmi da otomatik olarak önbelleğe alınan genel bir kodlama senaryoyu otomatikleştirme.

Uygulama simgesi ve KarşılamaEkranı görüntüleri ayarı olan ve Xamarin.Forms olmayan uygulamalar için olduğu gibi - yapılandırılmış platforma özgü uygulamaları için kullanılan aynı yönergeleri izleyin.


## <a name="related-links"></a>İlgili bağlantılar

- [WorkingWithImages (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS İmajlarla çalışma](~/ios/app-fundamentals/images-icons/index.md)
- [Android yansır](http://developer.android.com/design/style/iconography.html)
- [Döşeme ve simge varlıklar kılavuzları](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
