---
title: Bölüm 2 özeti. Bir uygulama anatomisi
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: c206b124349614db7249609707bd22e8a4efe6d8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>Bölüm 2 özeti. Bir uygulama anatomisi

Bir Xamarin.Forms uygulaması ekranında alan kaplar nesneleri olarak da bilinir *görsel öğeleri*, kapsüllenmiş tarafından [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) sınıfı. Görsel öğelerin bu sınıfların karşılık gelen üç kategoride bölünebilir:

- [Sayfa](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [Düzen](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [Görünümü](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

A `Page` türevi tüm ekranı veya neredeyse tüm ekranı kaplar. Genellikle, bir sayfa alt olan bir `Layout` alt görsel öğelerini düzenlemek için türevi. Alt `Layout` diğer olabilir `Layout` sınıfları veya `View` türevleri (adlandırılırlar *öğeleri*), metin, bit eşlemler, kaydırıcılar, düğmeleri, liste kutuları vb. gibi bilinen nesneler olduğu.

Bu bölümde, üzerine odaklanan bir uygulama oluşturmak gösterilmiştir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), olduğu `View` metni görüntüleyen türevi.

## <a name="say-hello"></a>Merhaba deyin

Yüklü Xamarin platformuyla yeni bir Xamarin.Forms çözümünü Visual Studio veya Visual Studio'da Mac için oluşturabileceğiniz [ **Hello** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) çözüm taşınabilir sınıf kitaplığı ortak kodunu kullanır. Visual Studio'da değişikliğe ile oluşturulan bir Xamarin.Forms çözümünü gösterir. Çözüm altı projeleri (ikisi de geçerli Xamarin.Forms çözümünü şablonlarıyla oluşturulmaz son) oluşur:

- [**Merhaba**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), taşınabilir sınıf kitaplığı (PCL) paylaşılan göre diğer projeleri
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), bir uygulama projesi Android için
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), iOS için uygulama projesi
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), bir uygulama projesi için evrensel Windows Platformu (Windows 10 ve Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), Windows 8.1 için uygulama projesi
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), Windows Phone 8.1 için uygulama projesi

Bu uygulama projeleri başlangıç projesi yapın ve ardından yapı ve bir aygıt veya simulator programını çalıştırın.

Birçok Xamarin.Forms programlarınızı, uygulama projeleri değiştiriliyor olmaz. Bu genellikle yalnızca programı başlatmak için çok küçük saplamalar kalır. Odağınız çoğunu tüm uygulamalar için ortak taşınabilir sınıf kitaplığı olacaktır.

## <a name="inside-the-files"></a>İçindeki dosyaları

Tarafından görüntülenen görselleri **Hello** program oluşturucuda tanımlanmış [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) sınıfı. `App` Xamarin.Forms sınıfından türetilen [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/).

**Başvuruları** bölümünü **Hello** PCL proje aşağıdaki Xamarin.Forms derlemelerini içerir:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**Başvuruları** beş uygulama projeleri bölümleri tek tek platformları için geçerli ek derlemeler içerir:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Her uygulama projeleri statik yapılan bir çağrı içerir `Forms.Init` yönteminde `Xamarin.Forms` ad alanı. Xamarin.Forms kitaplığı başlatır. Farklı bir sürümünü `Forms.Init` her platform için tanımlanır. Bu yönteme çağrıları aşağıdaki sınıflarda bulunabilir:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App` sınıfı, `OnLaunched` yöntemi](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App` sınıfı, `OnLaunched` yöntemi](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App` sınıfı, `OnLaunched` yöntemi](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

Ayrıca, her platform oluşturmalıdır `App` PCL konumda sınıfı. Bu çağrı oluşuyor `LoadApplication` aşağıdaki sınıflarda:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

Aksi takdirde, bu uygulama projeleri normal "hiçbir şey yapma" programlardır.

## <a name="pcl-or-sap"></a>PCL veya SAP?

Taşınabilir sınıf kitaplığı (PCL) veya bir paylaşılan varlık proje (SAP) ortak kodla bir Xamarin.Forms çözüm oluşturmak mümkündür. Bir SAP çözümü oluşturmak için Visual Studio'da paylaşılan seçeneği seçin. [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) çözüm değişikliğe SAP şablonla gösterir.

Tüm ortak kod platform uygulama projeleri tarafından başvurulan kitaplığı projesinde PCL yaklaşım paketleri. SAP yaklaşımı ortak kodun etkili bir şekilde tüm platform uygulama projelerinde var ve bunlar arasında paylaşılır.

Çoğu Xamarin.Forms Geliştirici PCL yaklaşımı tercih eder. Bu kitap, PCL çözümleri çoğu. SAP kullananlar içeren bir **Sap** proje adı soneki.

Tüm Xamarin.Forms platformları destekleyecek biçimde PCL tarafından kullanılan .NET sürümünü aşağıdaki platformları uyum gerekir:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (Klasik)

Bu bilgisayar profili 111 bilinir.

SAP yaklaşımda paylaşılan bir proje kodda farklı kod çeşitli platformlar için C# önişlemci yönergeleri kullanarak çalıştırabilirsiniz (`#if`, #`elif`, ve `#endif`) bunlarla tanımlayıcıları önceden tanımlanmış:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

Daha sonra bu bölümde göreceğiniz gibi bir PCL çalışma zamanında, kullanmakta olduğunuz hangi platformu belirleyebilirsiniz.

## <a name="labels-for-text"></a>Metin için etiketleri

[ **Tebrikler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) çözüm gösteren yeni bir C# dosyasına eklemek nasıl **Tebrikler** projesi. Bu dosya adında bir sınıfı tanımlar `GreetingsPage` , türetilen `ContentPage`. Bu kitap, tek bir çoğu projeleri içeren `ContentPage` adı soneki projesinin adı olan türevi `Page` eklenir.

`GreetingsPage` Oluşturucusu başlatır bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) metni görüntüleyen bir Xamarin.Forms görünümdür görünümü. [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) Özelliği tarafından görüntülenen metni kümesine `Label`. Bu program ayarlar `Label` için `Content` özelliği `ContentPage`. Oluşturucusunun `App` sınıfı ardından başlatır `GreetingsPage` ve ayarlar kendi `MainPage` özelliği.

Metin sayfasının sol üst köşesinde görüntülenir. İos'ta, bu sayfanın durum çubuğu çakışıyor anlamına gelir. Bu sorun için çeşitli çözümler vardır:

### <a name="solution-1-include-padding-on-the-page"></a>Çözüm 1. Doldurma sayfasında içerir

Ayarlanmış bir [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) özellik sayfasında. `Padding` tür [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), dört özelliklere sahip bir yapısı:

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` İçerik nerede dışlandı bir alanı içinde bir sayfa tanımlar. Böylece `Label` iOS durum çubuğu üzerine yazılmasını önlemek için.

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>Çözüm 2. Yalnızca iOS (yalnızca SAP) için doldurma içerir

Yalnızca bir C# önişlemci yönergesi ile bir SAP kullanarak iOS bir 'Doldurma' özelliğini ayarlayın. Bu, gösterilmiştir [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) çözümü.

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>Çözüm 3. Yalnızca iOS (PCL veya SAP) için doldurma içerir

Xamarin.Forms defteri için kullanılan sürümünde bir `Padding` kullanarak iOS PCL veya SAP için belirli özellik seçilebilir [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) veya [ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) statik yöntemi. Bu yöntem artık kullanımdan kaldırıldı

`Device.OnPlatform` Yöntemleri platforma özgü kodu çalıştırmak için veya platforma özgü değerlerini seçmek için kullanılır. Dahili olarak, yaptıkları kullanımı [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) üyesi döndürür statik salt okunur özellik [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) numaralandırma:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) Windows 8.1, Windows Phone 8.1 ve tüm UWP cihazlar için.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), daha önce Windows Phone 8.0 tanımlamak için kullanılan ancak olduğu artık kullanılmayan
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) kullanılmıyor

`Device.OnPlatform` Yöntemleri `Device.OS` özelliği ve `TargetPlatform` numaralandırma kullanım dışı şimdi tüm. Bunun yerine, kullanın [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) özelliği ve karşılaştırma `string` dönüş değeri şu statik alanlara sahip:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), "iOS" dizesi 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), "Android" dizesi
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), "Windows çalışma zamanı platformuna başvuran UWP" dizesi
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/), Windows çalışma zamanı (Windows 8.1 ve Windows Phone 8.1) "Windows" dizesi
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), Windows Phone 8.0 için "WinPhone" dizesi 

[ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) Statik salt okunur özelliği ile ilgili. Bu üye döndürür [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/), bu üyeler vardır:

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) kullanılmıyor

İOS ve Android, arasında kesme `Tablet` ve `Phone` dikey genişliği 600 birim. Windows platformu için `Desktop` Windows 10 altında çalışan bir UWP uygulaması gösterir `Tablet` bir Windows 8.1 program ve `Phone` Windows 10 veya Windows Phone 8.1 uygulama altında çalışan bir UWP uygulaması gösterir.

## <a name="solution-3a-set-margin-on-the-label"></a>Çözüm 3a. Etiket kümesi kenar boşluğu

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) Özellik sunulmuştur çok geç Defteri'nde dahil edilecek, ancak bunu da türü `Thickness` ve üzerinde ayarlayın `Label` hesaplamasında dahil görünümü dışında bir alanını tanımlamak için görünümün düzeni.

`Padding` Özelliği edinilebilir yalnızca [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) ve [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) türevleri. `Margin` Özelliği kullanılabilir tüm [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) türevleri.

## <a name="solution-4-center-the-label-within-the-page"></a>Çözüm 4. Etiketi sayfada içinde merkezi

Merkezi `Label` içinde `Page` (veya diğer sekiz yerlerden biri put) ayarlayarak [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özelliklerini `Label` türünde bir değer için [ `LayoutOptions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/). `LayoutOptions` Yapısı iki özellik tanımlar:

- Bir [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) türündeki özelliği [ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), dört üye olan bir numaralandırma: [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), sol veya bağlı olarak üst anlamına gelir Yönlendirme, [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), başka bir deyişle, sağ veya alt yönlendirmesini bağlı olarak ve [ `Fill` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/).

- Bir [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) türündeki özelliği `bool`.

Genellikle bu özellikleri doğrudan kullanılmaz. Bunun yerine, bu iki özellik birleşimlerini sekiz statik salt okunur özellikler türü tarafından sağlanır `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` ve `VerticalOptions` Xamarin.Forms düzeninde en önemli özelliklerdir ve daha ayrıntılı olarak ele alınmıştır [ **bölüm 4. Yığın kaydırma**](chapter04.md).

Sonucu ile işte `HorizontalOptions` ve `VerticalOptions` özelliklerini `Label` her ikisi de kümesine `LayoutOptions.Center`:

[![Tebrikler programının Üçlü ekran](images/ch02fg05-small.png "yatay ve dikey olarak ortalanmış etiket")](images/ch02fg05-large.png#lightbox "yatay ve dikey olarak ortalanmış etiket")

## <a name="solution-5-center-the-text-within-the-label"></a>Çözüm 5. Metin etiketi içinde merkezi

Metin Merkezi (ya da sayfasında sekiz başka konumlara yerleştirin) ayarlayarak [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) ve [ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) özelliklerini `Label` üyesiiçin[ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) numaralandırma:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), anlamı sol veya üst (bağlı olarak Yönlendirme)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/), sağ veya alt (Yönlendirme) bağlı olarak anlamına gelir

Bu iki özellik yalnızca göre tanımlanan `Label`, ancak `HorizontalAlignment` ve `VerticalAlignment` tarafından tanımlanan özellikler `View` ve tüm tarafından devralınan `View` türevleri. Görsel sonuçları benzer görünebilir, ancak sonraki bölümde gösterilmektedir çok farklı olan.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 2 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Bölüm 2 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Bölüm 2 F # örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Xamarin.Forms ile çalışmaya başlama](~/xamarin-forms/get-started/index.md)
