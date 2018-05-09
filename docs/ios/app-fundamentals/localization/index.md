---
title: iOS yerelleştirme
description: Bu belgede iOS SDK'ın yerelleştirme özelliklerini ve Xamarin ile erişmek nasıl kapsar.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: 5ee04614a500618846ad3acf2a38f279351d6e9d
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="ios-localization"></a>iOS yerelleştirme

_Bu belgede iOS SDK'ın yerelleştirme özelliklerini ve Xamarin ile erişmek nasıl kapsar._

Başvurmak [uluslararası Kodlamalar](encodings.md) Unicode olmayan veri işlemelidir uygulamalarında karakter kümelerini/kod sayfaları dahil olmak üzere ilgili yönergeler için.

## <a name="ios-platform-features"></a>iOS Platform Özellikleri

Bu bölümde iOS içindeki yerelleştirme özelliklerinden bazıları açıklanmaktadır. Geçin [sonraki bölümde](#basics) özel kod ve örnekler görmek için.

### <a name="language"></a>Dil

Kullanıcılar kendi dilini seçin **ayarları** uygulama. Bu ayar, uygulamalar ve işletim sistemi tarafından görüntülenen görüntüleri ve dil dizeleri etkiler. 

Bir uygulamada kullanılan dil belirlemek için ilk öğesi almak `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Bu değer bir dil kodu gibi olacaktır `en` İngilizce ' `es` İspanyolca için `ja` Japonca için vb. Döndürülen değer (en iyi eşleşmeyi belirlemek için geri dönüş kurallarını kullanarak) uygulama tarafından desteklenen yerelleştirmeler birini sınırlıdır.

Uygulama kodu bu değer için – Xamarin denetlemek her zaman gerekmez ve iOS hem de otomatik olarak kullanıcının dil için doğru dize veya kaynak sağlamak için yardımcı olan özellikler sağlar. Bu belgenin geri kalanında bu özellikleri açıklanmaktadır.

> [!NOTE]
> Kullanım `NSLocale.PreferredLanguages` uygulama tarafından desteklenen yerelleştirmeler bakılmaksızın kullanıcının dil tercihlerini belirlemek için. İOS 9 değiştirilen bu yöntem tarafından döndürülen değer; bkz: [Teknik Not TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) Ayrıntılar için.

### <a name="locale"></a>Yerel Ayar

Kullanıcıların kendi yerel ayarda seçim **ayarları** uygulama. Bu ayar, tarih, saat, sayılar ve para birimi biçimlendirildiğini biçimini etkiler.

Bu, kullanıcıların kendi ondalık ayırıcı virgül veya bir nokta, gün, ay ve yıl içinde tarih görüntüleme sırasını olup olmadığını ve 12 veya 24 saat saat biçimleri Bkz seçin sağlar.

Xamarin ile hem Apple'nın iOS sınıfları erişiminiz (`NSNumberFormatter`) System.Globalization .NET sınıflarda yanı sıra. Her kullanılabilen farklı özellikler gibi daha iyi kendi gereksinimlerine göre uygun olan geliştiriciler değerlendirmelidir. Özellikle, alma ve uygulama içi satın alma fiyatları StoreKit kullanarak görüntüleme döndürülen fiyat bilgileri için Apple'nın biçimlendirme sınıfları kullanmanız gerekir.

Geçerli yerel iki yöntemden birini sorgulanabilir:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

İlk değeri işletim sistemi tarafından önbelleğe alınabilir ve böylece her zaman kullanıcının şu anda seçili yerel gösterebilir değil. Seçili yerel edinmek için ikinci değeri kullanın.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS oluşturan bir `NSCurrentLocaleDidChangeNotification` kullanıcı kendi yerel zaman güncelleştirir. Çalıştıran ve uygun değişiklikleri UI yapabilirsiniz uygulamalar bu bildirim için dinleme.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>İOS içinde yerelleştirme temelleri

İOS aşağıdaki özelliklerini kolayca, kullanıcıya görünen için yerelleştirilmiş kaynaklar sağlamak için Xamarin de yararlanılabilir. Başvurmak [TaskyL10n örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) bu fikirleri uygulamak nasıl görmek için.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Varsayılan ve desteklenen dilleri Info.plist dosyasında belirtme

İçinde [teknik Q & A QA1828: iOS uygulamanızı için dil nasıl belirlediğini](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple iOS bir uygulamada kullanmak için bir dil nasıl seçtiği açıklar. Aşağıdaki etmenlere hangi dilde görüntülenir etkileyebilir:

- Kullanıcı tercih edilen dilleri (bulunan **ayarları** uygulama)
- (.Lproj klasörleri) uygulamayla birlikte yerelleştirmeler
- `CFBundleDevelopmentRegion` (**Info.plist** değeri uygulama için varsayılan dili belirtme)
- `CFBundleLocalizations` (**Info.plist** desteklenen tüm yerelleştirmeler belirtme dizi)

Teknik Q & A belirtildiği gibi `CFBundleDevelopmentRegion` bir uygulamanın varsayılan bölge ve dil temsil eder. Uygulama açıkça herhangi bir kullanıcının tercih edilen dilleri desteklemiyorsa, bu alana göre belirtilen dili kullanır. 

> [!IMPORTANT]
> iOS 11 işletim sisteminin önceki sürümlere kıyasla kesinlikle bu dil seçimi mekanizması uygular. Bu nedenle, açıkça desteklenen kendi yerelleştirmeler – .lproj klasörler dahil olmak üzere veya bir değeri ayarlanırken bildirmiyor herhangi bir iOS 11 uygulama `CFBundleLocalizations` – iOS 10 kıyasla iOS 11 farklı bir dilde görüntülenebilir.

Varsa `CFBundleDevelopmentRegion` içinde belirtilmemiş **Info.plist** dosyasını Xamarin.iOS derleme araçları şu anda varsayılan değeri kullanın `en_US`. Bu sonraki sürümlerde değişebilir, ancak varsayılan dil İngilizce olduğu anlamına gelir.

Uygulamanızı beklenen bir dil seçer emin olmak için aşağıdaki adımları uygulayın:

- Bir varsayılan dil belirtin. Açık **Info.plist** ve **kaynak** görünüm için bir değer ayarlamak için `CFBundleDevelopmentRegion` anahtar; XML'de, aşağıdakine benzer görünmelidir:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

Bu örnekte "es" İspanyolca için varsayılan bir kullanıcının hiçbiri tercih edilen zaman diller desteklenir belirtmek için kullanılır.

- Desteklenen tüm yerelleştirmeler bildirin. İçinde **Info.plist**, kullanın **kaynak** görünüm için bir dizi ayarlamak için `CFBundleLocalizations` anahtar; XML'de, aşağıdakine benzer görünmelidir:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

.Resx dosyaları bu sağlamalısınız gibi .NET mekanizmalarını kullanarak yerelleştirilmiş Xamarin.iOS uygulamaları **Info.plist** değerleri de.

Bunlar hakkında daha fazla bilgi için **Info.plist** anahtarları, Apple'nın göz ele [bilgi özellik listesi anahtar başvurusu](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="localizedstring-method"></a>LocalizedString yöntemi

`NSBundle.MainBundle.LocalizedString` Yöntemi içinde depolanan yerelleştirilmiş metin arayan **.strings** projedeki dosyaları. Bu dosyalar ile özel olarak adlandırılmış dizinlerde dile göre düzenlenir bir **.lproj** soneki.

#### <a name="strings-file-locations"></a>.strings dosya konumları

- **Base.lproj** varsayılan dil kaynakları içeren dizindir.
  Genellikle proje kök dizininde bulunur (ancak aynı zamanda yerleştirilebilir **kaynakları** klasörü).
- **<language>.lproj** dizinleri oluşturulan desteklenen her dil için genellikle **kaynakları** klasör.

Bir dizi farklı olabilir **.strings** her dil dizindeki dosyaları:

- **Localizable.strings** – yerelleştirilmiş metin ana listesi.
- **InfoPlist.strings** – belirli özel anahtarları, uygulama adı gibi şeyler çevirmek için bu dosyada verilir.
- **< film şeridi-adı > .strings** – film şeridi kullanıcı arabirimi öğeleri için çeviriler içeren isteğe bağlı bir dosya.

**Yapı eylemi** bu dosyaları olmalıdır **paket kaynak**.

#### <a name="strings-file-format"></a>.strings dosya biçimi

Yerelleştirilmiş dize değerlerini sözdizimi aşağıdaki gibidir:

```console
/* comment */
"key"="localized-value";
```

Aşağıdaki karakterleri dizelere kaçış:

* `\"`  Teklif
* `\\`  ters eğik çizgi
* `\n`  Yeni satır

Bir örnek **es/Localizable.strings** (örn. Örnek dosyasından İspanyolca):

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>Görüntüler

İOS görüntüdeki yerelleştirme için:

1. Görüntünün kodda, örneğin bakın:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Varsayılan görüntü dosyası yerleştirin **flag.png** içinde **Base.lproj** (yerel geliştirme dil dizin).

3. İsteğe bağlı olarak görüntüde yerelleştirilmiş sürümleri yerleştirin **.lproj** klasörleri her bir dilin (ör.) **Es.lproj**, **ja.lproj**). Aynı adı kullanmak **flag.png** her dil dizinde.

Görüntü için belirli bir dil yoksa, iOS varsayılan yerel dili klasörüne geri dönmesi ve görüntünün buradan yükleyin.


#### <a name="launch-images"></a>Görüntüleri başlatma

Başlatma görüntüleri (ve XIB veya film şeridi iPhone 6 modelleri için) için standart adlandırma kuralları kullanmak bunları yerleştirirken **.lproj** her dil için dizinler.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Uygulama adı

Yerleştirme bir **InfoPlist.strings** dosyasını bir **.lproj** dizin uygulamanın bazı değerleri geçersiz kılmanıza olanak tanır **Info.plist**, uygulama adı dahil olmak üzere:

```console
"CFBundleDisplayName" = "LeónTodo";
```

İçin kullanabileceğiniz diğer anahtarlar [uygulamaya özgü dizeleri yerelleştirme](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) şunlardır:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Tarihler ve saatler

Yerleşik .NET tarih ve saat işlevleri kullanmak mümkün olsa da (geçerli birlikte `CultureInfo`) tarihler ve saatler için bir yerel biçimlendirmek için bu yerel ayarlara özgü kullanıcı-(ayrı olarak dilden ayarlanabilir) ayarları yoksay.

İOS kullanma `NSDateFormatter` kullanıcı yerel ayar tercihleriyle eşleşen bir çıktı oluşturmak için. Aşağıdaki örnek kod, temel tarih ve saat biçimlendirme seçeneklerini gösterir:

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

Amerika Birleşik Devletleri İngilizce için sonuçları:

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

İspanyolca İspanya için sonuçları:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Apple'a başvuran [tarih Biçimlendiricileri](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) daha fazla bilgi için. Yerel ayara duyarlı tarih ve saat biçimlendirmesi sınarken her ikisini de denetlemek **iPhone dil** ve **bölge** ayarlar.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Sağdan sola (RTL) düzeni

iOS RTL algılayan uygulamaları oluşturmaya yardımcı olmak üzere birçok özellik sağlar:

* Kullanım Otomatik Düzeni'nın `leading` ve `trailing` (hangi sol ve sağ İngilizce için karşılık gelen ancak RTL dilleri için ters) denetim aligment için öznitelikler.
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Denetim, RTL bulundurmanız denetimleri yerleştirmek için özellikle yararlıdır.
* Kullanım `TextAlignment = UITextAlignment.Natural` (hangi çoğu dil ancak sağ RTL bırakılır) metin hizalaması için.
* `UINavigationController` otomatik olarak geri düğmesini çevirir ve geçirme yönünü tersine çevirir.

Aşağıdaki ekran görüntüleri Göster [yerelleştirilmiş Tasky örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) Arapça ve İbranice (İngilizce alanları girilmiş rağmen):

[![](images/rtl-ar-sml.png "Arapça yerelleştirme")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "İbranice yerelleştirme")](images/rtl-he.png#lightbox "Hebrew")

iOS otomatik olarak ters çevrilip `UINavigationController`, ve diğer denetimlerin içine yerleştirilen `UIStackView` veya otomatik düzeniyle hizalı.
RTL metin yerelleştirilmiş kullanarak **.strings** dosyaları aynı şekilde LTR metin olarak.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Kullanıcı Arabiriminde kod yerelleştirme

[(Kodda yerelleştirilmiş) Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) örnek kod (yerine XIBs veya film şeritleri) kullanıcı arabirimi burada yerleşik uygulama yerelleştirme nasıl gösterir.

### <a name="project-structure"></a>Proje yapısı

![](images/solution-code.png "Kaynakları ağacı")

### <a name="localizablestrings-file"></a>Localizable.strings dosyası

Yukarıda açıklandığı gibi **Localizable.strings** dosya biçimi anahtar-değer çiftlerini oluşur. Anahtarını dize amacını açıklayan ve uygulamada kullanılacak çevrilmiş metni değerdir.

İspanyolca (**es**) yerelleştirmeler örnek aşağıda gösterilmektedir:

```console
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>Yerelleştirme gerçekleştirme

(Bunu olup bir etiketin metin veya giriş 's yer tutucusu, vb.) bir kullanıcı arabiriminin görüntülenecek metni ayarlamak nerede olursa olsun uygulama kodunda kodu iOS kullanır. `LocalizedString` işlevi görüntülemek için doğru çevirisi almak için:

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Film şeridi Uı'lar yerelleştirme

Örnek [Tasky (yerelleştirilmiş film şeridi)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) film şeridi denetimlerinde üzerindeki metin yerelleştirme gösterilmektedir.

### <a name="project-structure"></a>Proje yapısı

**Base.lproj** dizin film şeridi içerir ve ayrıca uygulamada kullanılan tüm görüntüleri içermelidir.

Bir dil dizinleri içeren bir **Localizable.strings** kodda, başvurulan herhangi bir dize kaynakları için dosya yanı sıra bir **MainStoryboard.strings** çevirileri metin içeren dosya Film şeridi.

![](images/solution-storyboard.png "Kaynakları ağacı")

Dil dizinlerinin, mevcut bir geçersiz kılmak için yerelleştirilmiş tüm görüntüleri bir kopyasını içermelidir **Base.lproj**.

### <a name="object-id--localization-id"></a>Nesne Kimliği / yerelleştirme kimliği

Oluştururken ve film şeridi denetimlerinde düzenleme, her bir denetimi seçin ve yerelleştirme için kullanmak üzere kimlik denetleyin:

* Mac için Visual Studio'da bulunur **özellikleri paneli** ve çağrılır **yerelleştirme kimliği**.
* Xcode'da, adlı **nesne kimliği**.

Bu dize değeri, genellikle aşağıdaki ekran görüntüsünde gösterildiği gibi "NF3-h8-xmR" gibi bir biçime sahiptir:

![](images/xs-designer-localization-id.png "Film şeridi yerelleştirme Xcode görünümü")

Bu değer kullanılır **.strings** çevrilmiş metni her denetim için otomatik olarak atamak için dosya.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Film şeridi çeviri dosyasının biçimi benzer **Localizable.strings** dosya dışında anahtar (sol değer) kullanıcı tarafından tanımlanan olamaz, ancak bunun yerine belirli bir biçimde olmalıdır: `ObjectID.property`.

Örnekte **Mainstoryboard.strings** aşağıda görebilirsiniz `UITextField`s sahip bir `placeholder` yerelleştirilebilir; metin özelliği `UILabel`s sahip bir `text` özellik; ve `UIButton`s varsayılan metin kullanılarak ayarlanır `normalTitle`:

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> Film şeridi boyutu sınıflarıyla kullanarak uygulamada görünmez çevirileri neden olabilir. [Apple'nın Xcode sürüm notları](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) üç şey doğruysa film şeridi veya XIB doğru yerelleştirme değil gerektiğini belirtin: boyutu sınıfları kullanır, temel yerelleştirme ve yapı hedefi Evrensel ayarlanır ve yapı iOS 7.0 hedefler. İki özdeş dosyalarıyla film şeridi dizeleri dosyanızı çoğaltmak için durumda düzeltme: **MainStoryboard~iphone.strings** ve **MainStoryboard~ipad.strings**, aşağıdaki ekran görüntüsünde gösterildiği gibi:
> 
> ![](images/xs-dup-strings.png "Dizeleri dosyaları")

<a name="appstore" />

## <a name="app-store-listing"></a>Uygulama mağazası listeleme

Apple'nın SSS izleyen [App Store yerelleştirme](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) uygulamanızı indirimde olan her ülke çevirileri girmek için. Uygulamanızı da bir yerelleştirilmiş içeriyorsa görünecek çevirileri yalnızca kendi uyarı Not **.lproj** dil için dizin.

## <a name="summary"></a>Özet

Bu makalede yerleşik kaynak işleme ve film şeridi özelliklerini kullanarak iOS uygulamaları yerelleştirme temelleri kapsar.

İçinde (Xamarin.Forms dahil) iOS, Android ve platformlar arası uygulamalar için i18n ve L10n hakkında daha fazla öğrenebilirsiniz [bu platformlar arası kılavuz](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>İlgili bağlantılar

- [(Kodda yerelleştirilmiş) Tasky (örnek)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (yerelleştirilmiş film şeridi) (örnek)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple yerelleştirme Kılavuzu](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Platformlar arası yerelleştirme genel bakış](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms yerelleştirme](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android yerelleştirme](~/android/app-fundamentals/localization.md)
