---
title: Xamarin.Mac, Windows
description: Bu makale, windows ve bir Xamarin.Mac uygulamasını panellerinde çalışmayı kapsar. Bu oluşturma windows ve Xcode ve arabirim oluşturucu görsel taslakları ve .xib dosyaları yükleniyor ve program aracılığıyla çalışma, panellerinde açıklar.
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: b60b8a6a7c56347d6abf71f8c5149ddd556d3da8
ms.sourcegitcommit: ee66db647ae9d94b54b1c5d9093075a620d0c6b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43116371"
---
# <a name="windows-in-xamarinmac"></a>Xamarin.Mac, Windows

_Bu makale, windows ve bir Xamarin.Mac uygulamasını panellerinde çalışmayı kapsar. Bu oluşturma windows ve Xcode ve arabirim oluşturucu görsel taslakları ve .xib dosyaları yükleniyor ve program aracılığıyla çalışma, panellerinde açıklar._

C# ve .NET ile bir Xamarin.Mac uygulamasında çalışırken, erişim için aynı Windows olan ve içinde çalışan bir geliştirici, paneller *Objective-C* ve *Xcode* yapar. Xamarin.Mac Xcode ile doğrudan tümleşir çünkü Xcode'un kullanabileceğiniz _arabirim Oluşturucu_ oluşturmak ve Windows ve panel korumak (veya isteğe bağlı olarak bunları doğrudan C# kodu oluşturmak için).

Bir Xamarin.Mac uygulamasını kendi amacına bağlı olarak, yönetmek ve bilgileri ile çalışır ve görüntüler koordine etmek için bir veya daha fazla Windows ekranında sunabilir. Bir pencerenin asıl işlevler şunlardır:

1. Görünümleri ve denetimleri yerleştirilir ve yönetilebilen bir alan sağlayacak.
2. Kabul etmek ve kullanıcı etkileşimine klavye ve fare olayları yanıtlamak için.

Windows (örneğin, uygulama devam etmeden önce kapatıldı gereken bir dışarı aktarma iletişim) kalıcı veya geçici bir durumda (örneğin, birden çok belge aynı anda açık olan bir metin düzenleyicisine) kullanılabilir olabilir.

Paneller penceresi özel bir tür olan (temel öğesinin `NSWindow` sınıfı), genellikle hizmet yardımcı olan bir işlevi metin biçimi denetçiler ve sistem renk seçici gibi windows yardımcı programı gibi bir uygulamada.

[![](window-images/intro01.png "Xcode penceresinde düzenleme")](window-images/intro01.png#lightbox)

Bu makalede, biz bir Xamarin.Mac uygulamasında paneller ve Windows ile çalışmanın temel bilgileri ele alacağız. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` komutları Objective-C nesneleri ve kullanıcı Arabirimi öğeleri için C# sınıfları kablo-yedekleme kullanılır.

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows giriş

Yukarıda belirtildiği gibi bir pencere içinde görünümler ve denetimleri yerleştirilir ve yönetilebilen bir alan sağlar ve kullanıcı etkileşimi (veya klavye veya fare aracılığıyla) dayalı olarak olayları yanıt verir.

Apple'nın göre uygulama macOS, Windows beş ana türü vardır:

- **Belge penceresini** -bir belge penceresi bir elektronik tablo veya metin belgesi gibi dosya tabanlı kullanıcı verilerini içerir.
- **Uygulama penceresi** -belge tabanlı (örneğin, bir Mac üzerinde Takvim uygulaması) değil bir uygulamanın ana pencere uygulama penceredir.
- **Paneli** - bir panelin gezinen diğer windows ve araçlar sağlar veya belgeler, kullanıcılar ile çalışabilir denetimleri açın. Bazı durumlarda, bir panel saydam olabilir (ne zaman gibi büyük grafiklerle çalışma).
- **İletişim** -bir iletişim kutusu yanıt olarak bir kullanıcı eylemi açılır ve genellikle yolları kullanıcıların eylem tamamlayabilir sağlar. Bir iletişim kutusu, kapatılabilmesi için kullanıcıdan bir yanıt gerektirir. (Bkz [iletişim kutuları ile çalışma](~/mac/user-interface/dialog.md))
- **Uyarılar** -özel (bir hata gibi) ciddi bir sorun meydana geldiğinde görüntülenen iletişim türünü bir uyarıdır veya (örneğin, bir dosyayı silmek hazırlanıyor) bir uyarı olarak. Bir uyarı iletişim kutusu olduğundan, kapatılabilmesi için ayrıca bir kullanıcının yanıt gerektirir. (Bkz [uyarılarla](~/mac/user-interface/alert.md))

Daha fazla bilgi için [hakkında Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Ana anahtarı ve etkin olmayan Windows

Bir Xamarin.Mac uygulamasını, Windows, arayın ve nasıl kullanıcı şu anda bunlarla etkileşimde bulunan farklı göre davranır. En önemli belge veya kullanıcının odak noktası olan bir uygulama penceresi adlı _ana penceresi_. Çoğu durumda bu pencereyi ayrıca olacaktır _anahtar penceresi_ (şu anda kullanıcı girişi kabul pencere). Ancak bu her zaman böyle değildir, bir renk seçici örneğin ve açık (Bu ana pencerenin olmaya) belge penceresinde bir öğenin durumunu değiştirme ile kullanıcı etkileşim anahtar pencerenin olması.

Anahtar (ayrı olmaları durumunda) Windows ve ana her zaman etkin _etkin olmayan Windows_ ön planda olmayan açık pencereleri. Örneğin, bir metin düzenleyicisi uygulama birden fazla belge sahip açık bir zaman ana pencerenin etkin olacaktır. yalnızca, diğer tüm etkin olacaktır. 

Daha fazla bilgi için [hakkında Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows adlandırma

Bir pencere bir başlık çubuğu görüntüleyebilir ve başlığı görüntülendiğinde, bu genellikle uygulamanın adını, üzerinde çalışılmakta olan belgenin adını ya da işlev penceresinin (gibi denetçisini). Görüş tarafından tanınan ve belgelerle çalışmaz çünkü bazı uygulamalar bir başlık çubuğu gösterme.

Apple aşağıdaki yönergeler önerilir:

- Ana, belge olmayan bir pencere başlığı için uygulamanızın adını kullanın. 
- Yeni bir belge penceresi adı `untitled`. İlk yeni belge için bir sayı başlığı Ekle yok (gibi `untitled 1`). Kullanıcı kaydetme ve ilk titling önce başka bir yeni belge oluşturursa, bu pencereyi çağrısı `untitled 2`, `untitled 3`vb.

Daha fazla bilgi için [adlandırma Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Tam ekran Windows

MacOS, uygulamanın penceresini tam ekran gizleme (hangi ekranın üst kısmına imleci hareket ettirerek ortaya) uygulama menü çubuğu dahil her şeyi gidebilirsiniz dikkat dağıtıcı ücretsiz etkileşim içerik sağlamaktır.

Apple, aşağıdaki yönergeleri önerir:

- Tam ekran gitmek bir pencere için mantıklı olup olmadığını belirler. Tam ekran moduna kısa etkileşimleri (örneğin, bir hesap makinesi) sağlayan uygulamalar sağlamak olmamalıdır.
- Tam ekran görev onu gerektiriyorsa araç çubuğunu gösterin. Genellikle araç çubuğu tam ekran modunda gizlidir.
- Tam ekran pencerenin görevini tamamlamak gereken tüm özellikleri kullanıcılar olmalıdır.
- Mümkünse, kullanıcı bir tam ekran penceresinde olsa da Bulucu etkileşim kaçının.
- Daha fazla ekran alanını ana görev odağı kaydırma olmadan yararlanın.

Daha fazla bilgi için [tam ekran Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>Panel

Bir Panel denetimleri ve etkin belge ya da seçimi (örneğin, Sistem Renk Seçici) etkileyen seçenekleri içeren bir yardımcı pencere şöyledir:

[![](window-images/panel01.png "Bir renk paneli")](window-images/panel01.png#lightbox)

Panel ya da olabilir _uygulamaya özgü_ veya _yükleyebilecek_. Uygulamaya özgü Pano uygulamanın belge pencereleri üstüne Kaydır ve uygulamanın arka planda gerektiğinde kaybolur. Sistem çapında panelleri (gibi **yazı tipleri** paneli), float uygulama ne olursa olsun tüm açık pencereleri üzerinde. 

Apple, aşağıdaki yönergeleri önerir:

- Genel olarak, standart bir paneli kullanın, saydam panelleri, yalnızca gerektiğinde ve grafik açısından yoğun görevleri için kullanılmalıdır.
- Kullanıcılara önemli denetimleri veya görevini doğrudan etkileyen bilgileri için kolay erişim vermek için bir panel kullanmayı düşünün.
- Paneller gerekli olarak gösterme ve gizleme.
- Paneller, başlık çubuğunu her zaman içermelidir.
- Paneller, etkin bir simge durumuna Küçült düğmesini içermemelidir.

#### <a name="inspectors"></a>Denetçiler

Çoğu modern macOS uygulamaları yardımcı denetimleri ve etkin belge ya da seçimi olarak etkileyen seçenekleri sunmak _denetçiler_ ana pencerenin parçası olan (gibi **sayfaları** uygulama aşağıda gösterilen), Paneli Windows kullanmak yerine:

[![](window-images/panel02.png "Bir örnek denetçisi")](window-images/panel02.png#lightbox)

Daha fazla bilgi için [panelleri](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1) Apple'nın bölümünü [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) ve bizim [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) bir tamuygulamasınıiçinörnekuygulama**Denetçisi arabirimi** bir Xamarin.Mac uygulaması.

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Oluşturma ve xcode'da Windows bakımı

Yeni bir Xamarin.Mac Cocoa uygulaması oluşturduğunuzda, varsayılan olarak standart boş bir pencere alın. Bu windows tanımlanmış bir `.storyboard` dosya otomatik olarak projeye dahil. Windows tasarımınızı düzenlemeye **Çözüm Gezgini**, çift tıklayarak `Main.storyboard` dosyası:

[![](window-images/edit01.png "Ana görsel taslak seçme")](window-images/edit01.png#lightbox)

Bu pencere tasarım Xcode'un arabirimi Oluşturucu'da açın:

[![](window-images/edit02.png "Xcode kullanıcı Arabiriminde düzenleme")](window-images/edit02.png#lightbox)

İçinde **özniteliği denetçisi**, tanımlamak ve pencereniz denetlemek için kullanabileceğiniz birkaç özellik vardır:

- **Başlık** -pencerenin başlık çubuğunu görüntülenecek metni budur.
- **Otomatik kaydetme** -bu _anahtarı_ , konum ve ayarlar otomatik olarak kaydedildiğinde, pencere kimlik için kullanılacak.
- **Başlık çubuğu** -pencerenin başlık çubuğunu görüntülenmiyor.
- **Birleşik başlık ve araç** - bunu parçası olması başlık çubuğunda bir araç penceresi içerir.
- **Tam boyutlu içerik görünümü** -başlık çubuğunun altında olacak şekilde pencerenin içerik alanı sağlar.
- **Gölge** -penceresi gölge yok.
- **Doku** -dokulu windows üzerinde gövde her yerden sürükleyerek taşınabildiğinden ve etkileri (gibi canlılık verir) kullanabilirsiniz.
- **Kapatma** -Pencereyi Kapat düğmesine sahip.
- **Simge Durumuna Küçült** -pencereyi simge durumuna küçültme düğmesi yoktur.
- **Yeniden boyutlandırma** -pencereyi yeniden boyutlandırma denetim yok.
- **Araç çubuğu düğmesi** -pencereyi gizle/göster araç çubuğu düğmesi yoktur.
- **Geri yüklenebilen** -pencerenin konumunu ve otomatik olarak kaydedilip geri ayarlar.
- **Konumunda görünür başlatma** -penceresi otomatik olarak ne zaman gösterilir `.xib` dosya yüklenir.
- **Üzerinde Gizle devre dışı bırakma** -uygulama arka plan girdiğinde pencere gizli ise.
- **Yayın kapanır** -kapalı olduğunda bellekten temizleneceği penceresi.
- **Her zaman görünen araç ipuçları** -olan sürekli olarak görüntülenen araç ipuçları.
- **Görünüm döngü yeniden hesaplar** -penceresi çizilmeden önce yeniden hesaplanması görünümü sırası.
- **Alanları**, **Exposé** ve **dönüşümü** -tüm pencere bu macOS ortamlarda nasıl davranacağını tanımlayın. 
- **Tam ekran** -bu pencereyi bir tam ekran moduna girebilirsiniz belirler. 
- **Animasyon** -penceresini kullanılabilir animasyon türü denetler.
- **Görünüm** -penceresinin görünümünü denetler. Şu an için yalnızca bir görünüm, Açık Deniz Mavisi yoktur.

Apple'nın bkz [Windows Giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) ve [NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow) daha fazla ayrıntı için belgeleri.

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>Varsayılan boyut ve konum ayarlama

Pencereniz'ın ilk konumunu ayarlayın ve onun boyutunu denetlemek için geçiş **boyutu denetçisi**:

[![](window-images/edit07.png "Varsayılan boyut ve konum")](window-images/edit07.png#lightbox)

Buradan ilk penceresinin boyutunu ayarlayın, minimum ve maksimum boyutu verin, ilk konum ekranda ayarlayabilir ve pencere kenarlıklarını denetim.

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>Özel ana pencere denetleyicisi ayarlanıyor

Çıkışlar ve eylemleri C# kod için UI öğeleri göstermek için oluşturabilir, Xamarin.Mac uygulamasını özel pencere denetleyicisi bitvise gerekecektir.

Aşağıdakileri yapın:

1. Uygulamanın görsel taslak Xcode'un arabirimi Oluşturucusu'nda açın.
2. Seçin `NSWindowController` tasarım yüzeyindeki.
3. Geçiş **kimlik denetçisi** görüntüleyin ve girin `WindowController` olarak **sınıf adı**: 

    [![](window-images/windowcontroller01.png "Sınıf adı ayarlama")](window-images/windowcontroller01.png#lightbox)
4. Değişikliklerinizi kaydetmek ve eşitlemek Mac için Visual Studio geri dönün.
5. A `WindowController.cs` dosyası projenize eklenecek **Çözüm Gezgini** Mac için Visual Studio'da: 

    [![](window-images/windowcontroller02.png "Windows denetleyicisi seçme")](window-images/windowcontroller02.png#lightbox)
6. Xcode'un arabirim oluşturucu görsel taslağı yeniden açın.
7. `WindowController.h` Dosya kullanım için kullanılabilir olacak: 

    [![](window-images/windowcontroller03.png "WindowController.h dosyasını düzenleme")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>Kullanıcı Arabirimi öğeleri ekleme

Bir pencere içeriğini tanımlamak için denetimlerden sürükleyin **kitaplığı denetçisi** üzerine **Arayüzü Düzenleyicisi**. Lütfen bkz. bizim [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) oluşturup denetimlerini etkinleştirmek için arabirim Oluşturucu kullanma hakkında daha fazla bilgi için.

Örneğin, diyelim ki bir araç kutusundan sürükleyebilir **kitaplığı denetçisi** penceresinde üzerine **Arayüzü Düzenleyicisi**:

[![](window-images/edit03.png "Bir araç kitaplığından seçme")](window-images/edit03.png#lightbox)

Sonraki adımda bir **metni görünümü** ve araç çubuğunun altında alanı dolduracak şekilde boyutlandırın:

[![](window-images/edit04.png "Metin görünümü ekleme")](window-images/edit04.png#lightbox)

İstediğimiz beri **metni görünümü** daraltmak ve pencere boyutunu değiştikçe için şimdi geçin **kısıtlaması Düzenleyicisi** ve aşağıdaki kısıtlamalar ekleyin:

[![](window-images/edit05.png "Kısıtlama düzenleme")](window-images/edit05.png#lightbox)

Tıklayarak için **kırmızı ben-kirişleri** düzenleyicisinin en iyi ve tıklatarak **ekleme 4 kısıtlamaları**, biz verilen X, Y koordinatları takılıyor ve büyütmek ya da yatay ve dikey olarak küçültmek için metin görünümü söylemiş olursunuz olarak pencere yeniden boyutlandırıldı.

Son olarak, şimdi kullanıma **metni görünümü** kullanan kod için bir **çıkışı** (seçimini yaparak `ViewController.h` dosyası):

[![](window-images/edit06.png "Bir çıkış yapılandırma")](window-images/edit06.png#lightbox)

Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

İle çalışma hakkında daha fazla bilgi için **çıkışlar** ve **eylemleri**, lütfen bizim [çıkışı ve eylem](~/mac/get-started/hello-mac.md#outlets-and-actions) belgeleri.

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>Standart penceresi iş akışı

Oluşturduğunuz ve Xamarin.Mac uygulamanızda çalışmak bir pencere için temel olarak ne yalnızca yukarıda uyguladığımız aynı işlemdir:

1. Otomatik olarak projenize eklenir varsayılan olmayan yeni windows için yeni bir pencere tanımı projeye ekleyin. Bu aşağıda ayrıntılı olarak açıklanmıştır.
2. Çift `Main.storyboard` dosyayı penceresi tasarım Xcode'un arabirimi Oluşturucusu'nda düzenlemek için açın.
3. Yeni bir pencere kullanıcı arabiriminin tasarım sürükleyin ve ana penceresi kullanarak penceresi kanca _geçişler Uyarlamasız_ (daha fazla bilgi için [geçişler Uyarlamasız](~/mac/platform/storyboards/indepth.md#Segues) bölümünü bizim [görselTaslaklarileçalışma](~/mac/platform/storyboards/indepth.md) belgeleri).
3. Tüm gerekli pencere özellikleri kümesinde **özniteliği denetçisi** ve **boyutu denetçisi**.
4. Oluşturma arabiriminize ve bunları yapılandırmak için gerekli denetimleri sürükleyin **özniteliği denetçisi**.
5. Kullanım **boyutu denetçisi** yeniden boyutlandırmak için kullanıcı Arabirimi öğeleri işlemek için.
6. Pencerenin UI öğelerine C# kodu aracılığıyla kullanıma **çıkışlar** ve **eylemleri**.
7. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

Oluşturulan temel pencerenin sahibiz, normal işlemler sırasında windows ile çalışırken uygulamasının yapacağı bir Xamarin.Mac göz atacağız. 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>Varsayılan pencere görüntüleme

Varsayılan olarak, yeni bir Xamarin.Mac uygulamasını otomatik olarak tanımlanan pencerede gösterilir `MainWindow.xib` dosya başlatıldığı zaman:

[![](window-images/display01.png "Çalışan bir örnek penceresi")](window-images/display01.png#lightbox)

Biz yukarıdaki penceresinin tasarım değiştirilmiş olduğundan, varsayılan bir araç çubuğu artık içeriyor ve **metni görünümü** denetimi. Aşağıdaki bölüm `Info.plist` dosyasıdır bu pencereyi görüntülemek için sorumlu:

[![](window-images/display00.png "Info.plist düzenleme")](window-images/display00.png#lightbox)

**Ana arabirimi** açılan kullanılan ana uygulama kullanıcı Arabirimi kullanılacak görsel taslağı seçin (Bu durumda `Main.storyboard`).

Görünüm denetleyicisi (alt birincil birlikte) görüntülenir, ana Windows denetlemek için projeye otomatik olarak eklenir. İçinde tanımlanan `ViewController.cs` dosya ve bağlı **dosya sahibi** arabirimi Oluşturucusu altında **kimlik denetçisi**:

[![](window-images/display02.png "Dosya sahibi ayarı")](window-images/display02.png#lightbox)

Bizim pencere için bir başlığı olmasını istiyoruz `untitled` ilk açıldığında Haydi geçersiz kılma `ViewWillAppear` yönteminde `ViewController.cs` aşağıdaki gibi aramak için:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> Biz pencerenin değerini ayarlama `Title` özelliğinde `ViewWillAppear` yöntemi yerine `ViewDidLoad` yöntemi görünümü belleğe yüklenmiş, ancak değil henüz tam olarak örneği oluşturulduğu için. Erişmeye çalıştığında `Title` özelliğinde `ViewDidLoad` get yöntemini bir `null` pencerenin henüz oluşturulmuş ve kablolu özelliği henüz yukarı olduğundan özel durum.

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>Program aracılığıyla bir pencereyi kapatma

Program aracılığıyla bir pencerede bir Xamarin.Mac uygulamasını kapatmak için istediğiniz zamanlar olabilir, kullanıcıya tıklayın olması dışında pencerenin **kapatmak** düğmeyi veya bir menü öğesini kullanarak. macOS kapatmak için iki farklı yöntemleri sağlayan bir `NSWindow` program aracılığıyla: `PerformClose` ve `Close`.

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

Çağırma `PerformClose` yöntemi bir `NSWindow` pencerenin tıklayarak kullanıcının benzetimini yaparak **Kapat** kısa bir süre içinde düğme vurgulama ve ardından pencerenin kapatma düğmesi.

Uygulama uyguluyorsa `NSWindow`'s `WillClose` olay pencere kapatılmadan hemen önce oluşturulur. Olay döndürürse `false`, ardından pencerenin kapatılmadı. Pencere yoksa bir **Kapat** düğmesine veya kapatılamıyor herhangi bir sebeple işletim sistemi, uyarı sesini yayar.

Örneğin:

```csharp
MyWindow.PerformClose(this);
```

Kapatmak denenecek `MyWindow` `NSWindow` örneği. Başarılı olduysa, pencere kapalı olabilir, aksi takdirde uyarı sesini yayılan ve açık kalır.

<a name="Close" />

### <a name="close"></a>Close

Çağırma `Close` yöntemi bir `NSWindow` değil pencerenin tıklayarak kullanıcı benzetim **Kapat** düğmesi kısa bir süre içinde düğme vurgulayarak, yalnızca pencereyi kapatır.

Bir pencere kapatılacak görünür olması gerekmez ve `NSWindowWillCloseNotification` kapatıldığından kuşkulanılıyor penceresi için varsayılan bildirim Merkezi'nde bildirim gönderilecektir.

`Close` Yöntemi farklı iki önemli şekilde `PerformClose` yöntemi:

1. Yükseltmek denemez `WillClose` olay.
2. Bu kullanıcı'yı tıklatarak benzetimini değil **Kapat** tarafından kısa bir süre içinde düğme vurgulama düğmesi.

Örneğin:

```csharp
MyWindow.Close();
```

Kapatmak istiyor musunuz `MyWindow` `NSWindow` örneği.

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>Değiştirilen Windows içeriği

MacOS, kullanıcıyı bilgilendirmek için bir yol Apple ayarının, bir pencerenin içeriği (`NSWindow`) kullanıcı tarafından değiştirilmiş ve kaydedilmiş olması gerekir. Pencerenin değiştirilen içerik varsa, bir küçük siyah nokta onun içinde görüntülenecek **Kapat** pencere öğesi:

[![](window-images/close01.png "Değiştirilen işareti içeren bir pencere")](window-images/close01.png#lightbox)

Kullanıcı pencereyi kapatın veya çıkmak çalışırsa varken kaydedilmemiş bir Mac uygulaması için pencere içeriğinin değiştirir, sunması gerektiğini bir [iletişim kutusu](~/mac/user-interface/dialog.md) veya [kalıcı sayfası](~/mac/user-interface/dialog.md) ve değişiklikleri kaydedebilirler izin verin ilk:

[![](window-images/close02.png "Bir kayıt sayfası penceresi kapatıldığında gösteriliyor")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>Bir pencere değiştirilmiş olarak işaretleme

Bir pencere içeriği değiştirildiğinde olarak işaretlemek için aşağıdaki kodu kullanın:

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

Ve değişikliği kaydedildikten sonra değiştirilen bayrağı kullanılarak temizleyin:

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>Bir pencereyi kapatmadan önce değişiklikleri kaydediliyor

Değiştirilen içerik önceden kaydetmek bir pencereyi kapatmak ve bunları izin vererek kullanıcıdan izlemek için öğesinin oluşturmanız gerekecektir `NSWindowDelegate` ve geçersiz kılma kendi `WindowShouldClose` yöntemi. Örneğin:

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

Bu temsilci örneğini pencerenizi eklemek için aşağıdaki kodu kullanın:

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>Uygulamayı kapatmadan önce değişiklikleri kaydediliyor

Son olarak, Xamarin.Mac uygulamanız, Windows birini değiştirilmiş içeriği ve çıkmadan önce değişiklikleri kaydetmek kullanıcı izin görmek için denetlemelisiniz. Bunu yapmak için Düzenle, `AppDelegate.cs` dosya, geçersiz kılma `ApplicationShouldTerminate` yöntemi ve aşağıdaki gibi görünmesi:

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

Çoğu alan belgesi Mac uygulamalarını aynı anda birden çok belge düzenleyebilirsiniz. Örneğin, bir metin düzenleyicisi, birden çok metin dosyaları aynı anda düzenleme için açma olabilir. Varsayılan olarak, yeni Xamarin.Mac uygulamamız sahip bir **dosya** menüsüyle bir **yeni** otomatik olarak kablolu için'li öğesi `newDocument:` **eylem**.

Bu yeni öğe etkinleştirmek ve birden çok kopyasını aynı anda birden çok belge düzenlemek için sunduğumuz ana penceresi açmak kullanıcı izin vermek için kullanacağız.

Düzenleyelim bizim `AppDelegate.cs` dosyasını açıp aşağıdaki hesaplanan özellik ekleyin:

```csharp
public int UntitledWindowCount { get; set;} =1;
```

Bu size (başına, yukarıda açıklandığı gibi Apple'nın yönergeleri) kullanıcıya geri bildirim verebilir böylece kaydedilmemiş dosyaları sayısını izlemek için kullanacağız.

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

Bu kod penceresinde Denetleyicimizin yeni bir sürümünü oluşturur, yeni pencere yükler, ana ve anahtar penceresi kolaylaştırır ve başlığını ayarlar. Şimdi biz uygulamamızı çalıştırmak ve seçmek **yeni** gelen **dosya** yeni bir düzenleyici penceresi açılır ve görüntülenen menüsü:

[![](window-images/display04.png "Yeni bir adsız penceresi eklendi")](window-images/display04.png#lightbox)

Biz açarsanız **Windows** menüsünde, uygulama otomatik olarak izleme ve işleme Aç bizim windows görebilirsiniz:

[![](window-images/display05.png "Windows menüsü")](window-images/display05.png#lightbox)

Xamarin.Mac uygulamasında Menülerle çalışma hakkında daha fazla bilgi için lütfen bkz. bizim [Menülerle çalışma](~/mac/user-interface/menu.md) belgeleri.

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>Şu andaki etkin pencere alma

Birden çok windows (belgeler) açmak için bir Xamarin.Mac uygulamada ne zaman geçerli, üstteki pencere (anahtar) almanız gerekir zamanlar vardır. Aşağıdaki kod, anahtar pencereyi döndürür:

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

Herhangi bir sınıf ya da geçerli, anahtar pencere erişmesi gereken yöntemini çağrılabilir. Pencere açık olması durumunda onu döndürür `null`.

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>Tüm uygulama Windows erişme

Tüm Xamarin.Mac uygulamanız şu anda açık olan windows erişmek için gerek duyduğunuz zamanlar olabilir. Örneğin, açmak için kullanıcının istediği bir dosya zaten mevcut bir pencerede açmak görmektir.

`NSApplication.SharedApplication` Tutan bir `Windows` içeren tüm açık pencereleri uygulamanızda bir dizi özelliği. Bu dizi üzerindeki tüm uygulamanın geçerli windows erişmek için yineleyebilirsiniz. Örneğin:

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

Örnek kod, biz özel döndürülen her pencere atama `ViewController` uygulamamızı ve özel bir değeri test sınıfında `Path` özelliği açmak için kullanıcının istediği bir dosyanın yolu. Dosya zaten açık değilse, biz o pencereyi öne getirme.

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>Kodda pencere boyutunu ayarlama

Uygulama bir kod penceresinde yeniden boyutlandırmak için gereken zamanı zamanlar vardır. Yeniden boyutlandırma ve pencereyi yeniden konumlandırmak için ayar 's `Frame` özelliği. Bir pencerenin boyutu ayarlarken genellikle penceresi macOS'ın koordinat sistemini nedeniyle aynı konumda tutmak için kaynak, ayrıca ayarlamanız gerekir.

Sol üst köşede (0,0) temsil ettiği iOS, macOS, ekranın sol alt köşesinde (0,0) temsil ettiği matematik bir koordinat sistemini kullanır. İOS koordinatlar artışını sağa doğru aşağı taşıyın. MacOS, koordinatlar yukarı sağındaki değer artar. 

Aşağıdaki kod örneği bir boyutlandırır:

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Bir windows boyut ve konum kod ayarladığınızda, arabirim oluşturucu içinde ayarladığınız minimum ve maksimum boyutları dikkate emin olmanız gerekir. Bu değil otomatik olarak kabul edilir ve daha büyük ya da bu sınırlar küçük pencereyi yapmak mümkün olacaktır.

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>Pencere boyutu değişiklikleri izleme

Xamarin.Mac uygulamanız içinde pencerenin boyutu değiştiğinde izlemek için gerek duyduğunuz zamanlar olabilir. Örneğin, yeni boyuta sığması için içeriği yeniden çizmek için.

Boyut değişikliklerini izlemek için önce Xcode'un arabirim Oluşturucu penceresi denetleyicisi için özel bir sınıf atadığınızdan emin olun. Örneğin, `MasterWindowController` aşağıdaki:

[![](window-images/resize01.png "Kimlik denetçisi")](window-images/resize01.png#lightbox)

Ardından, özel pencere denetleyicisi sınıfı ve izleme Düzenle `DidResize` Canlı boyutu değişikliklerin bildirileceği için denetleyicinin penceredeki olay. Örneğin:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

İsteğe bağlı olarak kullanabileceğiniz `DidEndLiveResize` kullanıcı pencere boyutunu değiştirme işlemini tamamladıktan sonra yalnızca bildirim almak için olay. Örneğin:

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

## <a name="setting-a-windows-title-and-represented-file"></a>Başlık kullanıcının ve dosya temsil edilen bir penceresi ayarlama

Belgeleri temsil eden pencere ile çalışırken `NSWindow` sahip bir `DocumentEdited` olması durumunda özellik kümesine `true` küçük bir nokta dosya değiştirildi ve kapatmadan önce kaydedilmiş bir göstergesi kullanıcıya vermek için Kapat düğmesine görüntüler.

Düzenleyelim bizim `ViewController.cs` dosyasını açıp aşağıdaki değişiklikleri yapın:

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

Biz de izleme `WillClose` penceresi ve durumunu denetleme olayı `DocumentEdited` özelliği. Eğer öyleyse `true` kullanıcı dosyasındaki değişiklikleri kaydedin vermek gerekir. Uygulamamızı çalıştırmak ve bazı metinler girin, nokta görüntülenir:

[![](window-images/file01.png "Değiştirilen penceresi")](window-images/file01.png#lightbox)

Biz penceresini kapatmak çalışırsanız, size bir uyarı alırsınız:

[![](window-images/file02.png "Görüntüleme Kaydet iletişim kutusu")](window-images/file02.png#lightbox)

Bir belge bir dosyadan yükleniyor, şu penceresinin başlık dosyanın ayarlayabilirsiniz kullanarak ad `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` yöntemi (düşünüldüğünde `path` dosyanın açılmasını temsil eden bir dize). Ayrıca, biz kullanarak dosya URL'sini ayarlayabilirsiniz `window.RepresentedUrl = url;` yöntemi.

İşletim sistemi tarafından bilinen bir dosya türü için URL'sine, onun simgesi başlık çubuğunda görüntülenir. Kullanıcı simgeyi sağ tıkladığında, dosya yolu gösterilir.

Düzenleyelim `AppDelegate.cs` dosyasını açıp aşağıdaki yöntemi ekleyin:

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

Şimdi uygulamamıza çalıştırıyoruz, seçin **Aç...**  gelen **dosya** bir metin dosyasını menüsü, select **açın** iletişim kutusu ve açın:

[![](window-images/file03.png "Açık bir iletişim kutusu")](window-images/file03.png#lightbox)

Dosya görüntülenecek ve başlık dosyasının simgesiyle ayarlanır:

[![](window-images/file04.png "Bir dosyanın içeriğini yüklendi")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>Projeye yeni bir pencere ekleme

Ana belge penceresi yanı sıra, kullanıcı tercihlerine veya denetçisi bölmeleri gibi diğer türleri windows görüntülemek bir Xamarin.Mac uygulamasını gerekebilir.

Yeni bir pencere eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosyayı Xcode'un arabirimi Oluşturucusu'nda düzenleme için açın.
2. Yeni bir sürükleyin **penceresi denetleyicisi** gelen **Kitaplığı** ve bırakın **tasarım yüzeyine**:

    [![](window-images/new01.png "Yeni bir pencere denetleyicisi kitaplıkta seçme")](window-images/new01.png#lightbox)
3. İçinde **kimlik denetçisi**, girin `PreferencesWindow` için **film şeridi kimliği**: 

    [![](window-images/new02.png "Görsel taslak kimliği ayarlama")](window-images/new02.png#lightbox)
5. Arabiriminizin tasarım: 

    [![](window-images/new03.png "Kullanıcı Arabirimi tasarlama")](window-images/new03.png#lightbox)
6. Uygulama menüsünü açın (`MacWindows`) seçeneğini **tercihleri...** , Control tuşuna tıklama ve yeni pencere sürükleyin: 

    [![](window-images/new05.png "Bir segue oluşturma")](window-images/new05.png#lightbox)
7. Seçin **Göster** açılan menüsünde.
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.

Kodu çalıştırmak ve seçeneğini belirlerseniz **tercihleri...**  gelen **uygulama menüsü**, penceresi görüntülenir:

[![](window-images/new04.png "Bir örnek tercihleri menüsü")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>Panel ile çalışma

Bu makalenin başında belirtildiği gibi bir panel diğer pencerelerin gezinen ve araçları veya belgeler açıkken, kullanıcılar ile çalışabilir denetimleri sağlar. 

Yalnızca başka herhangi bir tür oluşturan ve Xamarin.Mac uygulamanızda çalışmak penceresinin gibi işlem temel olarak aynıdır:

1. Yeni bir pencere tanımı projeye ekleyin.
2. Çift `.xib` dosyayı penceresi tasarım Xcode'un arabirimi Oluşturucusu'nda düzenlemek için açın.
2. Tüm gerekli pencere özellikleri kümesinde **özniteliği denetçisi** ve **boyutu denetçisi**.
4. Oluşturma arabiriminize ve bunları yapılandırmak için gerekli denetimleri sürükleyin **özniteliği denetçisi**.
5. Kullanım **boyutu denetçisi** yeniden boyutlandırmak için kullanıcı Arabirimi öğeleri işlemek için.
6. Pencerenin UI öğelerine C# kodu aracılığıyla kullanıma **çıkışlar** ve **eylemleri**.
7. Yaptığınız değişiklikleri kaydedin ve Xcode ile eşitlemek Mac için Visual Studio için dönün.

İçinde **özniteliği denetçisi**, belirli panel için aşağıdaki seçenekleriniz vardır:

[![](window-images/panel03.png "Öznitelik denetçisi")](window-images/panel03.png#lightbox)

- **Stil** -panelinden stil ayarlamanızı izin ver: normal paneli (görünür bir standart penceresi gibi), yardımcı program paneli (daha küçük bir başlık çubuğu içerir), baş üstü paneli (saydam olan ve başlık çubuğunun arka planda bir parçasıdır).
- **Olmayan etkinleştirme** -içinde belirler paneli anahtar penceresi olur.
- **Belge kalıcı** -belge kalıcı, paneli yalnızca kaydırmak uygulamanın windows üzerinde Aksi takdirde tüm kayar.


Yeni bir Panel eklemek için aşağıdakileri yapın:

1. İçinde **Çözüm Gezgini**, projeye sağ tıklayıp **Ekle** > **yeni dosya...** .
2. Yeni dosya iletişim kutusunda **Xamarin.Mac** > **denetleyicili Cocoa penceresi**:

    [![](window-images/panels00.png "Yeni bir pencere denetleyicisi ekleme")](window-images/panels00.png#lightbox)
3. Girin `DocumentPanel` için **adı** tıklatıp **yeni** düğmesi.
4. Çift `DocumentPanel.xib` dosyayı arabirimi Oluşturucusu'nda düzenleme için açın: 

    [![](window-images/new02.png "Pannel düzenleme")](window-images/new02.png#lightbox)
5. Var olan pencereyi silin ve bir panelinden sürükleyin **kitaplığı denetçisi** içinde **Arayüzü Düzenleyicisi**: 

    [![](window-images/panels01.png "Var olan pencereyi siliniyor")](window-images/panels01.png#lightbox)
6. Bölmenin en fazla kanca **dosya sahibi** - **penceresi** - **çıkışı**: 

    [![](window-images/panels02.png "Hat üzeri paneli için sürükleme")](window-images/panels02.png#lightbox)
7. Geçiş **kimlik denetçisi** bölmenin sınıf ayarlanmış ve `DocumentPanel`: 

    [![](window-images/panels03.png "Ayar bölmenin sınıfı")](window-images/panels03.png#lightbox)
6. Değişikliklerinizi kaydetmek ve Xcode ile eşitlemek Mac için Visual Studio geri dönün.
7. Düzen `DocumentPanel.cs` dosya ve sınıf tanımını aşağıdakiyle değiştirin: 

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

Uygulamamızı çalıştırıyoruz, paneli görüntülenir:

[![](window-images/panels04.png "Çalışan bir uygulamanın panelinde")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> Paneli Windows, Apple tarafından kullanım dışı bırakıldı ve ile değiştirilmelidir **denetçisi arabirimleri**. Oluşturma tam bir örnek için bir **denetçisi** Xamarin.Mac uygulamada, lütfen bkz. bizim [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) örnek uygulaması.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, Windows ve panel bir Xamarin.Mac uygulama çalışma ayrıntılı bir bakış duruma getirdi. Biz farklı türleri gördünüz ve Windows ve panelleri, oluşturma ve Windows ve arabirim Oluşturucu Xcode'un panellerinde korumak ve C# kodunda paneller ve Windows ile çalışma konusunda kullanır.

## <a name="related-links"></a>İlgili bağlantılar

- [MacWindows (örnek)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (örnek)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Menülerle çalışma](~/mac/user-interface/menu.md)
- [OS X İnsan Arabirimi yönergelerine](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows giriş](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
