---
title: Bölüm 2 özeti. Bir uygulamanın anatomisi
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 2 özeti. Bir uygulamanın anatomisi'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d1daceba29e45adf64947c89555cc4e75a850d32
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995283"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Bölüm 2 özeti. Bir uygulamanın anatomisi

Bir Xamarin.Forms uygulamasında ekran alanı kaplayan nesneleri olarak da bilinir *görsel öğeleri*, kapsüllenmiş tarafından [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) sınıfı. Görsel öğeler, bu sınıflar için karşılık gelen üç kategoriye ayrılabilir:

- [Sayfa](xref:Xamarin.Forms.Page)
- [Düzen](xref:Xamarin.Forms.Layout)
- [Görünümü](xref:Xamarin.Forms.View)

A `Page` Türev ekranın tamamını ya da neredeyse ekranın tamamını kaplar. Genellikle, bir sayfanın alt olan bir `Layout` alt görsel öğeleri düzenlemek için Türev. Alt `Layout` diğer olabilir `Layout` sınıfları veya `View` türevleri (genellikle adlı *öğeleri*), metin, bit eşlemler, kaydırıcıları, düğmeler, liste kutuları ve benzeri gibi tanıdık nesneler olduğu.

Bu bölümde odaklanarak bir uygulama oluşturma işlemini gösterir [ `Label` ](xref:Xamarin.Forms.Label), olduğu `View` metni görüntüler Türev.

## <a name="say-hello"></a>Merhaba deyin

Xamarin platformu yüklü yeni bir Xamarin.Forms çözümü Visual Studio veya Visual Studio Mac için oluşturabileceğiniz [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) çözüm için ortak kodun bir taşınabilir sınıf kitaplığı kullanır. Bu, bir Xamarin.Forms çözümü Visual Studio'da oluşturulan herhangi bir değişiklik gösterir. Çözüm altı projeleri (ikisi geçerli Xamarin.Forms çözüm şablonları ile oluşturulmamış son) oluşur:

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), taşınabilir sınıf kitaplığı (PCL), diğer projeleri tarafından paylaşılan
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), Android için uygulama projesi
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), iOS için uygulama projesi
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), Evrensel Windows Platformu (Windows 10 ve Windows 10 Mobile) için bir uygulama projesi
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), Windows 8.1 için bir uygulama projesi
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), Windows Phone 8.1 için bir uygulama projesi

Bu uygulama projelerinden herhangi birinin başlangıç projesi olun ve ardından yapı ve bir cihaz veya simülatör programı çalıştırın.

Çoğu Xamarin.Forms programlarınızın uygulama projeleri değiştirmekte gerekmez. Bu genellikle yalnızca programı başlatmak için küçük saptamalar kalır. Odağınızı çoğu uygulama için ortak taşınabilir sınıf kitaplığı olacaktır.

## <a name="inside-the-files"></a>İçindeki dosyaları

Tarafından görüntülenen görselleri **Hello** program oluşturucusunun içinde tanımlanmış [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) sınıfı. `App` Xamarin.Forms sınıfından türetilen [ `Application` ](xref:Xamarin.Forms.Application).

**Başvuruları** bölümünü **Hello** PCL projesine aşağıdaki Xamarin.Forms derlemeleri içerir:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**Başvuruları** beş uygulama projeleri bölümleri tek tek platformlar için geçerli olan ek derlemeler içerir:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Her uygulama projeleri statik bir çağrı içerir `Forms.Init` yönteminde `Xamarin.Forms` ad alanı. Bu, bir Xamarin.Forms kitaplığı başlatır. Farklı bir sürümü `Forms.Init` her platform için tanımlanır. Bu yöntem çağrıları aşağıdaki sınıflarda bulunabilir:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App` sınıfı `OnLaunched` yöntemi](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` sınıfı `OnLaunched` yöntemi](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` sınıfı `OnLaunched` yöntemi](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Ayrıca, her platformun oluşturmalıdır `App` PCL konumda sınıfı. Böyle bir çağrıda `LoadApplication` aşağıdaki sınıflarda:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

Aksi takdirde, bu uygulama projeleri normal "hiçbir şey yapma" programlardır.

## <a name="pcl-or-sap"></a>PCL veya SAP?

Taşınabilir sınıf kitaplığı (PCL) ya da paylaşılan varlık projesi (SAP) ortak kodla bir Xamarin.Forms çözümü oluşturmak mümkündür. SAP çözüm oluşturmak için Visual Studio'da paylaşılan seçeneğini belirleyin. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) çözüm, SAP şablonu herhangi bir değişiklik yapılmadıysa gösterir.

Tüm ortak kod platform uygulaması projeleri tarafından başvurulan bir kitaplığı projesi, PCL yaklaşım paketleri. SAP yaklaşım ile ortak kodun etkili bir şekilde tüm platform uygulaması projelerinde var ve bunlar arasında paylaşılır.

Çoğu Xamarin.Forms geliştiricileri, PCL yaklaşımı tercih eder. Bu kitap, PCL çözümleri çoğu. SAP kullananlar dahil bir **Sap** proje adı soneki.

Tüm Xamarin.Forms platformlar desteklemek için PCL tarafından kullanılan .NET sürümünü aşağıdaki platformlara uyum sağlamak gerekir:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (Klasik)

Bu bilgisayar profili 111 bilinir.

SAP yaklaşımıyla paylaşılan proje kodu çeşitli platformlar için farklı kod C# önişlemci yönergeleri kullanarak çalıştırabilirsiniz (`#if`, #`elif`, ve `#endif`) önceden tanımlanmış bu ile tanımlayıcıları:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

Bu bölümün sonraki kısımlarında anlatıldığı gibi bir PCL'de hangi platformu çalışma zamanında, kullanmakta olduğunuz belirleyebilirsiniz.

## <a name="labels-for-text"></a>Metin etiketlerini

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) çözüm, yeni bir C# dosyasına ekleme gösterir **Greetings** proje. Bu dosya adında bir sınıf tanımlar `GreetingsPage` türetilen `ContentPage`. Bu kitap, tek bir çoğu proje içeren `ContentPage` adı soneki ile projenin adı olan Türev `Page` eklenir.

`GreetingsPage` Oluşturucusu örnekleyen bir [ `Label` ](xref:Xamarin.Forms.Label) metni görüntüler Xamarin.Forms görünümün görünüm. [ `Text` ](xref:Xamarin.Forms.Label.Text) Özelliği tarafından görüntülenen metni kümesine `Label`. Bu program ayarlar `Label` için `Content` özelliği `ContentPage`. Oluşturucusuna `App` sınıfı ardından başlatır `GreetingsPage` ve ayarlar kendi `MainPage` özelliği.

Metin, sayfanın sol üst köşesinde görüntülenir. İOS, bu sayfanın durum çubuğu çakışıyor anlamına gelir. Bu sorunun çeşitli çözümler vardır:

### <a name="solution-1-include-padding-on-the-page"></a>1. çözüm. Doldurma sayfasında içerir

Ayarlanmış bir [ `Padding` ](xref:Xamarin.Forms.Page.Padding) özellik sayfasında. `Padding` tür [ `Thickness` ](xref:Xamarin.Forms.Thickness), dört özellik bir yapıya sahiptir:

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` İçerik nerede tutulur, bir sayfa içinde bir alan tanımlar. Böylece `Label` iOS durum çubuğu üzerine yazılmasını önlemek için.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>2. çözüm. Yalnızca iOS (yalnızca SAP) için doldurma içerir

Bir 'Doldurma' özelliği yalnızca bir C# önişlemci yönergesi ile SAP kullanarak iOS ayarlayın. Bu gösterilmiştir [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) çözüm.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>3. çözüm. Yalnızca iOS (PCL veya SAP) için doldurma içerir

Xamarin.Forms kitabı için kullanılan sürümünde bir `Padding` kullanarak iOS PCL veya SAP belirli özellik seçilebilir [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) veya [ `Device.OnPlatform<T>` ](xref:Xamarin.Forms.Device.OnPlatform*) statik yöntem. Bu yöntemler artık kullanım dışı

`Device.OnPlatform` Yöntemleri, platforma özgü kodu çalıştırmak için veya platforma özgü değerleri seçmek için kullanılır. Dahili olarak, yaptıkları kullanım [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) üyesi döndüren statik salt okunur özelliği [ `TargetPlatform` ](xref:Xamarin.Forms.TargetPlatform) sabit listesi:

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) Windows 8.1, Windows Phone 8.1 ve tüm UWP cihazlar.
- [`WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone), daha önce Windows Phone 8.0 tanımlamak için kullanılan ancak olup artık kullanılmayan
- [`Other`](xref:Xamarin.Forms.TargetPlatform.Other) kullanılmıyor

`Device.OnPlatform` Yöntemleri `Device.OS` özelliği ve `TargetPlatform` kullanım dışı artık tüm sabit listesi. Bunun yerine, [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) özelliği ve karşılaştırma `string` dönüş değeri ile aşağıdaki statik alanlar:

- [`iOS`](xref:Xamarin.Forms.Device.iOS), "iOS" dizesi
- [`Android`](xref:Xamarin.Forms.Device.Android), "Android" dizesi
- [`UWP`](xref:Xamarin.Forms.Device.UWP), dize "UWP Windows çalışma zamanı platformuna başvuran",
- `Windows`, Windows çalışma zamanı (Windows 8.1 ve Windows Phone 8.1, kullanım dışı) "Windows" dizesi
- `WinPhone`, Windows Phone 8.0 (kullanım dışı) için "WinPhone" dizesi

[ `Device.Idiom` ](xref:Xamarin.Forms.Device.Idiom) Statik salt okunur özelliği ilgilidir. Bu bir üyesini döndürür [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom), bu üyeler vardır:

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) kullanılmıyor

İOS ve Android, kesme arasında `Tablet` ve `Phone` 600 birim dikey genişliğidir. Windows platformuna yönelik `Desktop` gösteren Windows 10 altında çalışan bir UWP uygulaması `Tablet` bir Windows 8.1 programdır ve `Phone` Windows 10 veya Windows Phone 8.1 uygulaması altında çalışan bir UWP uygulaması gösterir.

## <a name="solution-3a-set-margin-on-the-label"></a>Çözüm 3a. Küme kenar boşluğu etiketi

[ `Margin` ](xref:Xamarin.Forms.View.Margin) Özellik sunulmuştur çok geç kitap dahil edilecek, ancak aynı zamanda türünde olduğu `Thickness` ve açık ayarının `Label` hesaplamasına dahil görünümü dışındaki bir alanı tanımlamak için görünümün düzeni.

`Padding` Özelliği yalnızca üzerinde kullanılabilir [ `Layout` ](xref:Xamarin.Forms.Layout) ve [ `Page` ](xref:Xamarin.Forms.Page) türevleri. `Margin` Özelliği kullanılabilir tüm [ `View` ](xref:Xamarin.Forms.View) türevleri.

## <a name="solution-4-center-the-label-within-the-page"></a>4 çözüm. Etiketi sayfanın içinde merkezi

Merkezi `Label` içinde `Page` (veya diğer sekiz yerlerden biri put) olarak ayarlayarak [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özelliklerini `Label` türünde bir değer için [ `LayoutOptions` ](xref:Xamarin.Forms.LayoutOptions). `LayoutOptions` Yapısını tanımlayan iki özellikleri:

- Bir [ `Alignment` ](xref:Xamarin.Forms.LayoutOptions.Alignment) türünün özelliği [ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment), dört üye olan bir sabit listesi: [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start), sol veya bağlı olarak üst anlamına gelir Yönlendirme [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center), [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End), sağ veya alt yön, bağlı olduğu anlamına gelir ve [ `Fill` ](xref:Xamarin.Forms.LayoutAlignment.Fill).

- Bir [ `Expands` ](xref:Xamarin.Forms.LayoutOptions.Expands) türünün özelliği `bool`.

Genellikle bu özellikleri doğrudan kullanılmaz. Bunun yerine, bu iki özelliklerin birleşimleri sekiz statik salt okunur özellikler türü tarafından sağlanan `LayoutOptions`:

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` ve `VerticalOptions` Xamarin.Forms düzeninde en önemli özellikleri ve daha ayrıntılı olarak ele alınmıştır [ **bölüm 4. Yığını kaydırma**](chapter04.md).

Sonuçla işte `HorizontalOptions` ve `VerticalOptions` özelliklerini `Label` her ikisi de `LayoutOptions.Center`:

[![Üç ekran greetings programının](images/ch02fg05-small.png "Yatayda ve Dikeyde ortalanmış etiket")](images/ch02fg05-large.png#lightbox "Yatayda ve Dikeyde ortalanmış etiket")

## <a name="solution-5-center-the-text-within-the-label"></a>Çözüm 5. Etiket metni Ortala

Ayrıca merkezi metin (veya sayfasında sekiz başka konumlara yerleştirerek) olarak ayarlayarak [ `HorizontalTextAlignment` ](xref:Xamarin.Forms.Label.HorizontalTextAlignment) ve [ `VerticalTextAlignment` ](xref:Xamarin.Forms.Label.VerticalTextAlignment) özelliklerini `Label` üyesinin[ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment) sabit listesi:

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start), anlamı sol veya üst (bağlı olarak Yönlendirme)
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End), sağdaki veya alttaki (Yönlendirme) bağlı olarak anlamına gelir.

Bu iki özellik yalnızca tanımlanan `Label`bilgileriyse `HorizontalAlignment` ve `VerticalAlignment` özellikleri tarafından tanımlanan `View` ve tüm tarafından devralınan `View` türevleri. Görsel sonuçlarla benzer görünebilir ancak bir sonraki bölümde gösterildiği gibi çok farklı olur.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 2 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [Bölüm 2 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [Bölüm 2'de F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin.Forms ile çalışmaya başlama](~/xamarin-forms/get-started/index.md)
