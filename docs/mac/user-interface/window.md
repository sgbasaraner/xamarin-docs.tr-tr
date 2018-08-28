---
title: Xamarin.Mac Windows
description: Bu makalede windows ve paneller Xamarin.Mac uygulamada çalışmaya kapsar. Oluşturma windows ve Xcode ve arabirim film şeritleri ve .xib dosyaları yükleme ve bunlarla program aracılığıyla çalışma Oluşturucusu paneller açıklar.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 39efcf3554469219cc29d70ee059fe645c41280d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794078"
---
# <a name="windows-in-xamarinmac"></a>Xamarin.Mac Windows

_Bu makalede windows ve paneller Xamarin.Mac uygulamada çalışmaya kapsar. Oluşturma windows ve Xcode ve arabirim film şeritleri ve .xib dosyaları yükleme ve bunlarla program aracılığıyla çalışma Oluşturucusu paneller açıklar._

C# ve .NET ile Xamarin.Mac uygulamada çalışırken, aynı Windows erişimi ve içinde çalışan bir geliştirici paneller *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir nedeniyle, Xcode'nın kullanabilirsiniz _arabirimi Oluşturucu_ ve Windows ve paneller korumak (veya isteğe bağlı olarak bunları doğrudan C# kodunda oluşturmak için).

Kendi amacına bağlı olarak, yönetmek ve bilgileri ile birlikte çalışır ve görüntüler koordine etmek için bir veya daha fazla Windows ekranında Xamarin.Mac uygulama oluşturabilir. Bir pencere asıl işlevler şunlardır:

1. Hangi görünümleri ve denetimleri bir alanı sağlamak üzere yerleştirilir ve yönetilebilir.
2. Kabul etmek ve yanıt olarak klavye ve fare ile kullanıcı etkileşimi olaylarına tepki vermek için.

Windows (örneğin, uygulama devam etmeden önce kapatıldığında gerekir bir dışarı aktarma iletişim kutusu) kalıcı veya geçici bir durumda (örneğin, birden çok belge aynı anda açık olan bir metin düzenleyicisi) kullanılan olabilir.

Paneller olan özel türde bir pencere (bir alt sınıfı taban `NSWindow` sınıfı), genellikle hizmet eden bir yardımcı işlevi metin biçimi denetçiler ve sistem renk seçici gibi yardımcı programı windows gibi bir uygulamada.

[![](window-images/intro01.png "Xcode penceresinde düzenleme")](window-images/intro01.png#lightbox)

Bu makalede, sizi bir Xamarin.Mac uygulaması'nda Windows ve paneller ile çalışmanın temelleri ele alacağız. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları kablo, C# sınıflarının Objective-C nesneleri ve kullanıcı Arabirimi öğeleri yukarı için kullanılır.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows giriş

Yukarıda belirtildiği gibi bir pencere, görünümler ve denetimleri yerleştirilir ve yönetilebilen bir alan sağlar ve kullanıcı etkileşimi (ya da klavye veya fare aracılığıyla) göre olaylara yanıt verir.

Apple göre beş ana türlerinde Windows macOS uygulama vardır:

- **Belge penceresini** -bir belge penceresi bir elektronik tablo veya bir metin belgesi gibi dosya tabanlı kullanıcı verilerini içerir.
- **Uygulama penceresi** -bir uygulama penceresi belge tabanlı (örneğin, Takvim uygulama Mac'te) değil bir uygulamanın ana penceredir.
- **Panel** - bir panel gezinen diğer windows ve araçlar sağlar veya belgeleri durumdayken, kullanıcılar ile çalışabilir denetimleri açın. Bazı durumlarda, bir panel saydam olabilir (ne zaman gibi büyük grafiklerle çalışma).
- **İletişim** -bir iletişim kutusu yanıt olarak bir kullanıcı eylemi görüntülenir ve genelde yol kullanıcılar eylemi tamamlamak sağlar. Bir iletişim kutusu kapatılabilmesi için kullanıcıdan bir yanıt gerektirir. (Bkz [iletişim kutuları ile çalışma](~/mac/user-interface/dialog.md))
- **Uyarıları** -bir uyarı (hata gibi) ciddi bir sorun oluştuğunda görüntülenen iletişim özel türüdür ya da (örneğin, bir dosyayı silmek hazırlanıyor) bir uyarı olarak. Bir uyarı iletişim kutusu olduğundan kapatılabilmesi için de kullanıcı yanıtı gerektirir. (Bkz [uyarılarla çalışma](~/mac/user-interface/alert.md))

Daha fazla bilgi için bkz: [Windows hakkında](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Ana, anahtar ve etkin olmayan Windows

Windows Xamarin.Mac uygulamasında bakın ve farklı nasıl kullanıcı şu anda bunları ile etkileşimde bulunan göre davranır. En önemli belge veya kullanıcının dikkat odağı olan uygulama penceresi adlı _ana penceresi_. Çoğu durumda bu pencereyi de olur _anahtar penceresi_ (şu anda kullanıcı girişi kabul pencere). Ancak bu her zaman durumda değilse, bir renk seçici örneğin ve açık bir öğe (hangi ana penceresi hala olur) belge penceresinde durumunu değiştirmek için ile kullanıcı etkileşim anahtar pencerenin olması.

Anahtar (ayrı olmaları durumunda) Windows ve ana her zaman etkin _etkin olmayan Windows_ ön planda olmayan açık Windows. Örneğin, bir metin düzenleyici uygulaması birden fazla belge sahip olabilir, her seferinde ana penceresi etkin yalnızca, diğerlerini etkin olmayan açılması. 

Daha fazla bilgi için bkz: [Windows hakkında](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows adlandırma

Bir pencere başlık çubuğu görüntüleyebilir ve başlığı görüntülendiğinde, genellikle uygulamanın adını, üzerinde çalıştığınız belgenin adını ya da (örneğin, denetleyici) penceresinin işlevi. Çünkü bunlar görüş tarafından tanınmıyor ve belgelerle çalışmıyor bazı uygulamalar bir başlık çubuğu görüntülemiyor.

Apple aşağıdaki yönergeleri öneririz:

- Uygulama adınız ana, belge olmayan bir pencere başlığı için kullanın. 
- Yeni bir belge penceresi adı `untitled`. İlk yeni belge için bir sayı başlık append yok (gibi `untitled 1`). Kullanıcı kaydetme ve ilk titling önce başka bir yeni belge oluşturursa, bu pencereyi çağrısı `untitled 2`, `untitled 3`vb.

Daha fazla bilgi için bkz: [adlandırma Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Tam ekran Windows

MacOS içinde bir uygulamanın penceresini tam ekran gizleme (hangi imleci ekranın üst kısmına taşıyarak ortaya) uygulama menü çubuğu dahil her şeyi gidebilirsiniz dikkat dağıtıcı ücretsiz etkileşim içerik sağlamaktır.

Apple aşağıdaki yönergeleri önerir:

- Tam ekran gitmek bir pencere için mantıklıdır olup olmadığını belirler. (Örneğin, bir hesap makinesi) kısa etkileşimleri sağlayan uygulamalar tam ekran modu sağlar döndürmemelidir.
- Tam ekran görev onu gerektiriyorsa araç çubuğunu göster. Genellikle araç tam ekran modunda gizli durumdadır.
- Tam ekran penceresi görevi tamamlamak gereken tüm özellikler kullanıcılar olması gerekir.
- Mümkünse, kullanıcı bir tam ekran penceresinde durumdayken Bulucu etkileşim kaçının.
- Daha yüksek ekran alanının ana görev çıktığınızda odak kaydırma olmadan yararlanın.

Daha fazla bilgi için bkz: [tam ekran Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Paneller

Bir Panel denetimleri ve etkin belgeyi veya (örneğin, Sistem Renk Seçici) seçimini etkileyen seçenekleri içeren bir yardımcı bir penceredir:

[![](window-images/panel01.png "Bir renk paneli")](window-images/panel01.png#lightbox)

Paneller olabilir ya da _uygulamaya özgü_ veya _yükleyebilecek_. Uygulamaya özgü paneller uygulamanın belge pencereleri üst float ve uygulama arka planda olduğunda kaybolur. Sistem çapında paneller (gibi **yazı tipleri** Masası), uygulama olursa olsun tüm açık windows üstünde float. 

Apple aşağıdaki yönergeleri önerir:

- Genel olarak, standart bir panel kullanmak için saydam paneller yalnızca gerektiğinde ve grafik açısından yoğun görevler için kullanılmalıdır.
- Kullanıcıların önemli denetimleri veya doğrudan kendi görev etkiler bilgilerine kolay erişim vermek için bir panel kullanmayı düşünün.
- Paneller gerektiği gibi gösterme ve gizleme.
- Paneller başlık çubuğunu her zaman içermelidir.
- Paneller etkin Simge Durumuna Küçült düğmesini içermemelidir.

#### <a name="inspectors"></a>Denetçiler

Çoğu modern macOS uygulamaları sunmanıza yardımcı denetimleri ve etkin belge veya seçimi olarak etkileyen seçenekleri _denetçiler_ ana penceresi parçası olan (gibi **sayfaları** aşağıda gösterilen uygulama), Panel Windows kullanmak yerine:

[![](window-images/panel02.png "Örnek Inspector")](window-images/panel02.png#lightbox)

Daha fazla bilgi için bkz: [paneller](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) ve bizim [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) bir tamuygulamasınıiçinörnekuygulama**Denetçisi arabirimi** Xamarin.Mac uygulama.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Oluşturma ve Windows xcode'da koruma

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş, bir pencere alın. Bu windows tanımlanmış bir `.storyboard` otomatik olarak projeye dahil dosyası. Windows tasarımınızı düzenlemek için **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](window-images/edit01.png "Ana film şeridi seçme")](window-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'nın arabirimi Oluşturucusu'nda açın:

[![](window-images/edit02.png "Xcode kullanıcı Arabiriminde düzenleme")](window-images/edit02.png#lightbox)

İçinde **özniteliği denetçisi**, tanımlayın ve pencerenizi denetlemek için kullanabileceğiniz birkaç özellik vardır:

- **Başlık** -bu pencerenin titlebar görüntülenecek metindir.
- **Otomatik kaydetme** -bu _anahtar_ , konum ve ayarlar otomatik olarak kaydedildiğinde penceresi kimlik için kullanılacak.
- **Başlık çubuğu** -penceresinin başlık çubuğunu görüntülenmiyor.
- **Birleşik başlık ve araç** - bunun bir parçası olması başlık çubuğunda bir araç penceresi içerir.
- **Tam boyutlu içerik görünümü** -içerik alanının penceresinin başlık çubuğunun altında olmasını sağlar.
- **Gölge** -penceresi gölge yok.
- **Doku** -doku windows kendi gövdesinde herhangi bir yere sürükleyerek taşınabildiğinden ve etkileri (gibi canlılık verir) kullanabilirsiniz.
- **Kapat** -pencereyi kapat düğmesi yok.
- **Simge Durumuna Küçült** -penceresini simge durumuna Küçült düğmesini sahip.
- **Yeniden boyutlandırma** -pencerede yeniden boyutlandırma denetim yok.
- **Araç çubuğu düğmesi** -penceresini göster/gizle araç çubuğu düğmesi yok.
- **Geri yüklenebilen** -pencere konumunu ve otomatik olarak kaydedilip geri ayarlar.
- **Konumunda görünür başlatma** -penceresi otomatik olarak ne zaman gösterilir `.xib` dosya yüklenir.
- **Gizleme üzerinde devre dışı** -uygulama arka girdiğinde penceresi gizlenir.
- **Yayın zaman kapalı** -kapalı olduğunda bellekten temizlendi penceresi.
- **Her zaman görünen araç ipuçları** -olan sürekli olarak görüntülenen araç ipuçları.
- **Görünüm döngü yeniden hesaplar** -penceresi çizilmeden önce yeniden hesaplanması görünüm sırası.
- **Alanları**, **Exposé** ve **dönüşümü** -tüm pencere bu macOS ortamlarda nasıl davranacağını tanımlayın. 
- **Tam ekran** -bu pencereyi tam ekran moduna girerseniz belirler. 
- **Animasyon** -animasyon penceresi için kullanılabilir türünü denetler.
- **Görünüm** -penceresinin görünümünü denetler. Şu an için yalnızca bir görünüm, açık mavi yoktur.

Apple'nın bkz [Windows Giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) ve [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) daha fazla ayrıntı için belgeleri.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Varsayılan boyut ve konum ayarlama

Pencerenizi ilk konumunu ayarlayın ve onun boyutunu denetlemek için geçmek **boyutu denetçisi**:

[![](window-images/edit07.png "Varsayılan boyut ve konum")](window-images/edit07.png#lightbox)

Buradan penceresinin başlangıç boyutunu ayarlayın, minimum ve maksimum boyutu verin, ilk konum ekranda ayarlayabilir ve pencerenin kenarlıklarını denetim.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Özel ana penceresi denetleyicisini ayarlama

Çıkışlar ve eylemleri C# kod için UI öğeleri göstermek için oluşturabilecek, Xamarin.Mac uygulama özel pencere denetleyicisi kullanılmasını gerekir.

Aşağıdakileri yapın:

1. Uygulamanın film şeridi Xcode'nın arabirimi Oluşturucu'da açın.
2. Seçin `NSWindowController` tasarım yüzeyine içinde.
3. Geçiş **kimlik denetçisi** görüntüleyin ve girin `WindowController` olarak **sınıf adı**: 

    [![](window-images/windowcontroller01.png "Sınıf adı ayarlama")](window-images/windowcontroller01.png#lightbox)
4. Değişikliklerinizi kaydetmek ve Visual Studio eşitlemek için Mac için geri dönün.
5. A `WindowController.cs` dosya projenizde eklenecek **Çözüm Gezgini** Mac için Visual Studio'da: 

    [![](window-images/windowcontroller02.png "Windows denetleyicisi seçme")](window-images/windowcontroller02.png#lightbox)
6. Xcode'nın arabirimi Oluşturucu film şeridi yeniden açın.
7. `WindowController.h` Dosya kullanılabilir olacaktır: 

    [![](window-images/windowcontroller03.png "WindowController.h dosya düzenleme")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Kullanıcı Arabirimi öğeleri ekleme

Pencere içeriğine tanımlamak için denetimlerden sürükleyin **kitaplığı denetçisi** üzerine **arabirimi Düzenleyicisi**. Lütfen bakın bizim [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) arabirimi Oluşturucu Oluşturma ve denetimleri etkinleştirme kullanma hakkında daha fazla bilgi için.

Örnek olarak, şimdi sürükleme araç çubuğundan **kitaplığı denetçisi** penceresinde üzerine **arabirimi Düzenleyicisi**:

[![](window-images/edit03.png "Kitaplıktan bir araç seçme")](window-images/edit03.png#lightbox)

Ardından, sürükleyin bir **metin görünümü** ve araç çubuğunun altında alanı dolduracak şekilde boyutu:

[![](window-images/edit04.png "Metin görünümü ekleme")](window-images/edit04.png#lightbox)

İstediğimiz beri **metin görünümü** küçültmek ve pencere boyutunu değiştikçe büyüme için şimdi geçmek **kısıtlaması Düzenleyicisi** ve aşağıdaki kısıtlamalar ekleyin:

[![](window-images/edit05.png "Kısıtlamaları düzenleme")](window-images/edit05.png#lightbox)

Tıklayarak için **kırmızı t-kirişleri** Düzenleyicisinin üst ve tıklatmak **eklemek 4 kısıtlamaları**, verilen X, Y koordinatları takılıyor ve büyütür veya yatay ve dikey olarak küçültür metin görünüme söylemiş olursunuz olarak pencere yeniden boyutlandırılır.

Son olarak, şimdi kullanıma **metin görünümü** kullanan kod için bir **çıkışı** (seçimini yaparak `ViewController.h` dosyası):

[![](window-images/edit06.png "Prizine yapılandırma")](window-images/edit06.png#lightbox)

Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

İle çalışma hakkında daha fazla bilgi için **çıkışlar** ve **Eylemler**, lütfen bakın bizim [çıkışı ve eylem](~/mac/get-started/hello-mac.md#Outlets_and_Actions) belgeleri.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Standart pencere iş akışı

Oluşturun ve Xamarin.Mac uygulamanızda çalışmak herhangi penceresi için işlem temelde ne biz yalnızca yukarıda yaptığınızdan aynıdır:

1. Projenize otomatik olarak eklenen varsayılan olmayan yeni windows için yeni bir pencere tanımı projeye ekleyin. Bu aşağıda ayrıntılı olarak açıklanmıştır.
2. Çift `Main.storyboard` dosyayı penceresi tasarımı Xcode'nın arabirimi Oluşturucusu'nda düzenlemek için açın.
3. Yeni bir pencere kullanıcı arabiriminin tasarım sürükleyin ve ana penceresi kullanarak penceresi kanca _Segues_ (daha fazla bilgi için bkz: [Segues](~/mac/platform/storyboards/indepth.md#Segues) bölümünü bizim [Filmşeritleriileçalışma](~/mac/platform/storyboards/indepth.md) belgelerine).
3. Tüm gerekli pencere özellikleri kümesinde **özniteliği denetçisi** ve **boyutu denetçisi**.
4. Arabiriminizin oluşturmak ve bunları yapılandırmak için gerekli denetimleri sürükleyin **özniteliği denetçisi**.
5. Kullanım **boyutu denetçisi** yeniden boyutlandırma için kullanıcı Arabirimi öğeleri işlemek için.
6. Pencerenin kullanıcı Arabirimi öğeleri için C# kodu aracılığıyla kullanıma **çıkışlar** ve **Eylemler**.
7. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

Oluşturulan temel bir pencere sahibiz, normal işlemler sırasında windows ile çalışırken, uygulama mu Xamarin.Mac göreceğiz. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Varsayılan pencere görüntüleme

Varsayılan olarak, yeni bir Xamarin.Mac uygulama otomatik olarak tanımlanan penceresi görüntüleyecektir `MainWindow.xib` dosya başlatıldığında:

[![](window-images/display01.png "Çalışan bir örnek penceresi")](window-images/display01.png#lightbox)

Biz yukarıdaki penceresinin tasarım değiştirilmiş olduğundan, onu artık varsayılan araç içeriyor ve **metin görünümü** denetim. Aşağıdaki bölüm `Info.plist` dosyasıdır sorumlu bu pencereyi görüntülemek için:

[![](window-images/display00.png "Info.plist düzenleme")](window-images/display00.png#lightbox)

**Ana arabirimi** açılır ana uygulama kullanıcı Arabirimi kullanılacak film şeridi seçmek için kullanılır (Bu durumda `Main.storyboard`).

View Controller (yanı sıra, birincil görünüm) görüntülenen ana Windows denetlemek için proje otomatik olarak eklenir. İçinde tanımlanan `ViewController.cs` dosya ve bağlı **dosyanın sahibi** altında arabirimi oluşturucusunda **kimlik denetçisi**:

[![](window-images/display02.png "Dosyanın sahibi ayarlama")](window-images/display02.png#lightbox)

Bizim penceresi için bir başlığı olmasını isteriz `untitled` ilk açıldığında sağlandığından geçersiz kılma `ViewWillAppear` yönteminde `ViewController.cs` aşağıdaki gibi aramak için:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Biz pencerenin değerini ayarlama `Title` özelliğinde `ViewWillAppear` yöntemi yerine `ViewDidLoad` yöntemi görünümü belleğe yüklenmiş, ancak bunu değil henüz tam olarak örneği olduğundan. Erişmeye çalıştığında `Title` özelliğinde `ViewDidLoad` get yöntemi bir `null` penceresi kurmadı oluşturulan ve kablolu özelliği henüz yukarı beri özel durum.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Program aracılığıyla bir pencereyi kapatma

Program aracılığıyla Xamarin.Mac uygulamasındaki bir pencereyi kapatmak istediğiniz zamanlar olabilir, olması dışında kullanıcıyı tıklatın pencerenin **kapatmak** düğmesini veya menü öğesini kullanarak. macOS kapatmak için iki farklı yöntemleri sağlayan bir `NSWindow` program aracılığıyla: `PerformClose` ve `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Çağırma `PerformClose` yöntemi bir `NSWindow` pencerenin tıklamak kullanıcıyı taklit eden **Kapat** kısa bir süre içinde düğmesi vurgulama ve ardından pencereyi tarafından düğmesi.

Uygulama uyguluyorsa `NSWindow`'s `WillClose` olay, pencere kapatılmadan önce gerçekleştirilecektir. Olay döndürürse `false`, pencerenin kapatılmamış sonra. Pencerenin yoksa bir **Kapat** düğmesini tıklatın veya kapatılamaz herhangi bir sebeple işletim sistemi uyarı ses yayma.

Örneğin:

```csharp
MyWindow.PerformClose(this);
```

Kapatmak denenecek `MyWindow` `NSWindow` örneği. Başarılı olduysa, pencere kapatılacak, başka uyarı ses yayılan ve açık kalır.

<a name="Close" />

### <a name="close"></a>Close

Çağırma `Close` yöntemi bir `NSWindow` değil benzetimini yapar pencerenin tıklayarak kullanıcı **Kapat** düğmesi kısa bir süre içinde düğmesi vurgulayarak, yalnızca penceresini kapatır.

Bir pencere kapatılacak görünür olması gerekmez ve bir `NSWindowWillCloseNotification` kapatılan penceresi için varsayılan bildirim Merkezi'ne bildirim gönderilecektir.

`Close` Yöntemi farklı önemli iki yolla `PerformClose` yöntemi:

1. Yükseltmek denemez `WillClose` olay.
2. Kullanıcı'yı tıklatarak benzetmez **Kapat** kısa bir süre içinde düğmesi vurgulama tarafından düğmesi.

Örneğin:

```csharp
MyWindow.Close();
```

Kapatmak istiyor musunuz `MyWindow` `NSWindow` örneği.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Değiştirilen Windows içeriği

MacOS içinde kullanıcıya bildirmek için bir yöntem Apple sağlamıştır, pencerenin içeriğini (`NSWindow`) kullanıcı tarafından değiştirilmiş ve kaydedilmesi gerekiyor. Pencerenin değiştirilen içerik varsa, küçük siyah bir nokta, içinde görüntülenir **Kapat** pencere öğesi:

[![](window-images/close01.png "Değiştirilen işaret içeren bir pencere")](window-images/close01.png#lightbox)

Kullanıcı pencereyi kapatmak veya çıkmak çalışırsa pencerenin içerikteki Mac varken kaydedilmemiş uygulama değişiklikler, sunması gerektiğini bir [iletişim kutusu](~/mac/user-interface/dialog.md) veya [kalıcı sayfası](~/mac/user-interface/dialog.md) ve değişiklikleri kaydedebilirler kullanıcıya izin ver ilk:

[![](window-images/close02.png "Pencere kapatıldığında gösterildikten sayfası kaydetme")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Bir pencere değiştirilmiş olarak işaretleme

Bir pencere içeriğini değiştirdi olarak işaretlemek için aşağıdaki kodu kullanın:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

Ve değişiklik kaydedildikten sonra değiştirilmiş bayrağını kullanarak temizleyin:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Bir pencere kapatmadan önce gerçekleştirilen değişiklikler kaydediliyor

Değiştirilen içerik önceden kaydetmek bir pencereyi kapatmak ve vermeden kullanıcıdan izlemek için bir alt sınıfı oluşturmanız gerekecektir `NSWindowDelegate` ve geçersiz kılma kendi `WindowShouldClose` yöntemi. Örneğin:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on resu;t
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

Bu temsilci örneği pencerenize eklemek için aşağıdaki kodu kullanın:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Uygulama kapatmadan önce gerçekleştirilen değişiklikler kaydediliyor

Son olarak, Xamarin.Mac uygulamanızı herhangi bir Windows değiştirilmiş içeriği içeren ve kullanıcının çıkmadan önce değişiklikleri kaydetmesine olanak tanıyan olmadığını denetlemeniz gerekir. Bunu yapmak için düzenleyin, `AppDelegate.cs` dosya, geçersiz kılma `ApplicationShouldTerminate` yöntemi ve şu şekilde görünür yapın:

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>Birden çok Windows ile çalışma

Çoğu tabanlı belge Mac uygulamaları, aynı anda birden çok belge düzenleyebilirsiniz. Örneğin, bir metin düzenleyicisi aynı anda düzenleme için açma birden çok metin dosyası olabilir. Varsayılan olarak, yeni Xamarin.Mac uygulamamız sahip bir **dosya** menüsüyle bir **yeni** otomatik olarak kablolu için'li öğesi `newDocument:` **eylem**.

Bu yeni öğe etkinleştirmek ve birden çok kopyasının aynı anda birden çok belge düzenlemek için bizim ana penceresi açmak kullanıcı izin vermek için çağıracaksınız.

Düzenleyelim bizim `AppDelegate.cs` dosya ve aşağıdaki özellik hesaplanan ekleyin:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Bu size geri bildirim (başına, yukarıda açıklanan şekilde Apple'nın yönergeleri) kullanıcı izni verebilirsiniz şekilde kaydedilmemiş dosyaların sayısını izlemek için kullanacağız.

Ardından, aşağıdaki yöntemi ekleyin:

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Bu kodu bizim penceresi denetleyicisi yeni bir sürümünü oluşturur, yeni pencere yükler, ana ve anahtar penceresi kolaylaştırır ve başlık ayarlar. Biz uygulamamızı çalıştırmak ve seçerseniz şimdi **yeni** gelen **dosya** yeni bir düzenleyici penceresi açılır ve görüntülenen menü:

[![](window-images/display04.png "Yeni bir adsız pencere eklendi")](window-images/display04.png#lightbox)

Biz açarsanız **Windows** menüsünde uygulama otomatik olarak izleme ve işleme bizim açık windows görebilirsiniz:

[![](window-images/display05.png "Windows menüsü")](window-images/display05.png#lightbox)

Xamarin.Mac uygulamasında Menülerle çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Menülerle çalışma](~/mac/user-interface/menu.md) belgeleri.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Şu anda etkin pencereyi alma

Birden çok windows (belgeler) açmak için bir Xamarin.Mac uygulamada, ne zaman geçerli, üstteki pencerede (anahtar pencere) almanız gerekir zamanlar vardır. Aşağıdaki kod anahtar penceresi döndürür:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Herhangi bir sınıf veya geçerli, anahtar penceresi erişmesi gereken yöntemi çağrılabilir. Pencere açık olması durumunda onu döndürür `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Tüm uygulama Windows erişme

Burada, tüm Xamarin.Mac uygulamanız şu anda açık olan windows erişim için gereken zamanlar olabilir. Örneğin, açmak için kullanıcının istediği bir dosya olmadığını görmek için zaten mevcut bir pencerede açmak olur.

`NSApplication.SharedApplication` Tutan bir `Windows` uygulamanızda açık olan tüm pencereleri dizisini içeren özelliği. Uygulamanın geçerli windows tümünün erişmek için bu dizinin yineleyebilirsiniz. Örneğin:

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Örnek kodda biz özel döndürülen her penceresine atama `ViewController` uygulamamıza ve özel bir değer sınama sınıfında `Path` kullanıcının istediği açmak için bir dosya yolu karşı özelliği. Dosyanın zaten açıksa, biz bu pencereyi öne getiren.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Kodda pencere boyutunu ayarlama

Uygulama kodu penceresinde yeniden boyutlandırmak için gerektiği zaman zamanlar vardır. Yeniden boyutlandırma ve bir pencere yeniden konumlandırmak için ayar 's `Frame` özelliği. Bir pencere boyutunu ayarlarken, genellikle pencere macOS'ın koordinat sistemi nedeniyle aynı konumda tutmak için kaynak, aynı zamanda ayarlamanız gerekir.

Sol üst köşe (0,0) temsil ettiği iOS, ekranın sol alt köşesinde (0,0) temsil ettiği macOS mathematic koordinat sistemi kullanır. İOS koordinatları artışını sağa doğru aşağı taşır. MacOS içinde yukarı doğru sağa değeri koordinatları artar. 

Aşağıdaki örnek kod bir pencere göre yeniden boyutlandırır:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Bir windows boyutunu ve konumunu kodda ayarladığınızda arabirimi Oluşturucusu'nda ayarladığınız minimum ve maksimum boyutlarını dikkate emin olmanız gerekir. Bu değil otomatik olarak çalıştırılır ve pencerenin daha büyük ya da bu sınırlamaları daha küçük hale getiremezsiniz olacaktır.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Pencere boyutu değişiklikleri izleme

Burada bir pencere boyutunu Xamarin.Mac uygulamanızı içinde değişiklikleri izlemek için gereken zamanlar olabilir. Örneğin, içeriği yeni boyuta sığması için yeniden çizmek için.

Boyut değişiklikleri izlemek için önce Xcode'nın arabirimi Oluşturucu penceresi denetleyicisi için özel bir sınıf atadığınızdan emin olun. Örneğin, `MasterWindowController` aşağıdaki:

[![](window-images/resize01.png "Kimlik denetçisi")](window-images/resize01.png#lightbox)

Ardından, özel pencere denetleyici sınıfı ve İzleyici Düzenle `DidResize` Canlı boyutu değişikliklerin bildirilmesi için denetleyicinin penceresinde olay. Örneğin:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

İsteğe bağlı olarak kullanabileceğiniz `DidEndLiveResize` kullanıcı pencere boyutunu değiştirme tamamlandıktan sonra yalnızca bildirim almak için olay. Örneğin:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>Bir pencere ayarı başlık adı ve dosya temsil

Belgeler, temsil eden windows ile çalışırken `NSWindow` sahip bir `DocumentEdited` olması durumunda özellik kümesine `true` küçük bir nokta dosya değiştirilmiş ve kapatmadan önce kaydedilmesi gereken bir göstergesi kullanıcıya vermek için Kapat düğmesini görüntüler.

Düzenleyelim bizim `ViewController.cs` dosya ve aşağıdaki değişiklikleri yapın:

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

Biz de izleme `WillClose` penceresi ve durumunu denetleme olayda `DocumentEdited` özelliği. Eğer öyleyse `true` kullanıcı dosyasındaki değişiklikleri kaydedin vermek gerekir. Bizim uygulamayı çalıştırın ve bazı metin girin, nokta görüntülenir:

[![](window-images/file01.png "Değiştirilen penceresi")](window-images/file01.png#lightbox)

Biz pencereyi kapatmak çalışırsanız, size bir uyarı alırsınız:

[![](window-images/file02.png "Kaydetme görüntüleme iletişim")](window-images/file02.png#lightbox)

Biz bir belge bir dosyadan yüklüyorsanız biz penceresinin başlık dosyanın ayarlayabilirsiniz kullanarak ad `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` yöntemi (o `path` açılmakta dosyasını temsil eden bir dizedir). Ayrıca, biz kullanarak dosya URL'sini ayarlayabilirsiniz `window.RepresentedUrl = url;` yöntemi.

İşletim sistemi tarafından bilinen bir dosya türü için URL işaret ediyorsanız, buna ait simgesi başlık çubuğunda görüntülenir. Kullanıcı sağa simgesine tıkladığında, dosyasının yolu gösterilir.

Düzenleyelim `AppDelegate.cs` dosya ve aşağıdaki yöntemi ekleyin:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

Şimdi biz uygulamamıza çalıştırırsanız seçin **Aç...**  gelen **dosya** bir metin dosyası menüsünde select **açmak** iletişim kutusu ve açın:

[![](window-images/file03.png "Açık bir iletişim kutusu")](window-images/file03.png#lightbox)

Dosya görüntülenir ve başlık dosya simge ile ayarlanır:

[![](window-images/file04.png "Bir dosyanın içeriğini yüklendi")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Bir projeye yeni bir pencere ekleme

Ana belge penceresine yanı sıra Xamarin.Mac uygulama kullanıcıya Tercihler veya Inspector paneller gibi diğer windows türlerini görüntülemek gerekebilir.

Yeni bir pencere eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosyayı Xcode'nın arabirimi Oluşturucusu'nda düzenlemek için açın.
2. Yeni bir sürükleyin **penceresi denetleyicisi** gelen **Kitaplığı** ve bırakın **tasarım yüzeyi**:

    [![](window-images/new01.png "Yeni bir pencere denetleyicisi kitaplıkta seçme")](window-images/new01.png#lightbox)
3. İçinde **kimlik denetçisi**, girin `PreferencesWindow` için **film şeridi kimliği**: 

    [![](window-images/new02.png "Film şeridi kimliği ayarlama")](window-images/new02.png#lightbox)
5. Arabiriminizin tasarım: 

    [![](window-images/new03.png "Kullanıcı arabirimini tasarlama")](window-images/new03.png#lightbox)
6. Uygulama menüsünü açın (`MacWindows`), select **tercihleri...** , Denetim tıklatın ve yeni pencere sürükleyin: 

    [![](window-images/new05.png "Bir segue oluşturma")](window-images/new05.png#lightbox)
7. Seçin **Göster** açılan menüsünden.
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Şu kodu çalıştırın ve seçerseniz **tercihleri...**  gelen **uygulama menüsü**, penceresi görüntülenir:

[![](window-images/new04.png "Bir örnek Tercihler menüsü")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Paneller ile çalışma

Bu makalenin başlangıcında belirtildiği gibi bir panel diğer windows gezinen ve Araçlar ya da belgeler açık durumdayken, kullanıcılar ile çalışabilir denetimleri sağlar. 

Yalnızca diğer türleri oluşturmak ve Xamarin.Mac uygulamanızda çalışmak penceresinin gibi işlem temelde aynıdır:

1. Yeni bir pencere tanımı projeye ekleyin.
2. Çift `.xib` dosyayı penceresi tasarımı Xcode'nın arabirimi Oluşturucusu'nda düzenlemek için açın.
2. Tüm gerekli pencere özellikleri kümesinde **özniteliği denetçisi** ve **boyutu denetçisi**.
4. Arabiriminizin oluşturmak ve bunları yapılandırmak için gerekli denetimleri sürükleyin **özniteliği denetçisi**.
5. Kullanım **boyutu denetçisi** yeniden boyutlandırma için kullanıcı Arabirimi öğeleri işlemek için.
6. Pencerenin kullanıcı Arabirimi öğeleri için C# kodu aracılığıyla kullanıma **çıkışlar** ve **Eylemler**.
7. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

İçinde **özniteliği denetçisi**, bölmeleri belirli aşağıdaki seçeneklere sahip olursunuz:

[![](window-images/panel03.png "Öznitelik denetçisi")](window-images/panel03.png#lightbox)

- **Stil** -panelinden stilini ayarlamak izin: normal paneli (standart bir pencerede görülüyor), yardımcı programı paneli (daha küçük bir başlık çubuğu varsa), HUD paneli (saydam olduğu ve başlık çubuğu arka planda bir parçasıdır).
- **Olmayan etkinleştirme** -içinde belirler paneli anahtar pencere olur.
- **Belge kalıcı** -belge kalıcı, bölmenin yalnızca float uygulamanın windows, başka yukarıdaki tüm kayar.


Yeni bir Panel eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayın ve seçin **Ekle** > **yeni dosya...** .
2. Yeni dosya iletişim kutusunda seçin **Xamarin.Mac** > **Cocoa penceresi denetleyicisiyle**:

    [![](window-images/panels00.png "Yeni bir pencere denetleyicisi ekleme")](window-images/panels00.png#lightbox)
3. Girin `DocumentPanel` için **adı** tıklatıp **yeni** düğmesi.
4. Çift `DocumentPanel.xib` dosyayı arabirimi Oluşturucusu'nda düzenlemek için açın: 

    [![](window-images/new02.png "Pannel düzenleme")](window-images/new02.png#lightbox)
5. Varolan pencere silin ve bir panelinden sürükleyin **kitaplığı denetçisi** içinde **arabirimi Düzenleyicisi**: 

    [![](window-images/panels01.png "Varolan pencere silme")](window-images/panels01.png#lightbox)
6. Bölmenin en fazla kanca **dosyanın sahibi*- **penceresi*- **çıkışı**: 

    [![](window-images/panels02.png "Kablo paneli sürükleme")](window-images/panels02.png#lightbox)
7. Geçiş **kimlik denetçisi** ve bölmenin sınıfı kümesine `DocumentPanel`: 

    [![](window-images/panels03.png "Ayar bölmenin sınıfı")](window-images/panels03.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.
7. Düzen `DocumentPanel.cs` dosya ve sınıf tanımını şu şekilde değiştirin: 

    `public partial class DocumentPanel : NSPanel`
8. Değişiklikleri dosyaya kaydedin.

Düzen `AppDelegate.cs` dosya ve olun `DidFinishLaunching` yöntemi görünüm aşağıdaki gibi:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

Biz uygulamamızı çalıştırmak, bölmenin görüntülenir:

[![](window-images/panels04.png "Çalışan bir uygulamanın panelinde")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Panel Windows Apple'nın kullanım ve ile değiştirilmelidir **denetçisi arabirimleri**. Oluşturma tam bir örnek için bir **denetçisi** Xamarin.Mac uygulamada, lütfen bkz. bizim [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) örnek uygulama.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, Windows ve paneller Xamarin.Mac uygulamada çalışma ayrıntılı bir bakış sürdü. Biz farklı türleri gördünüz ve Windows ve paneller, oluşturma ve Windows ve Xcode'nın arabirimi Oluşturucu paneller korumak ve Windows ve paneller C# kodunda çalışmak nasıl kullanır.

## <a name="related-links"></a>İlgili bağlantılar

- [MacWindows (örnek)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (örnek)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Menülerle çalışma](~/mac/user-interface/menu.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
