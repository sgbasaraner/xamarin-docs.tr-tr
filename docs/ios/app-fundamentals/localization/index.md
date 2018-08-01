---
title: Xamarin.iOS, yerelleştirme
description: Bu belge iOS yerelleştirme özellikleri ve Xamarin.iOS uygulamalarında bu özellikleri kullanmak nasıl açıklar. Dil, yerel ayar, dizeleri dosyaları, başlatma görüntüleri ve diğer ele alınmaktadır.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: 2a6096efc18f40d18ea37573e77d93796e812cc2
ms.sourcegitcommit: 4cc17681ee4164bdf2f5da52ac1f2ae99c391d1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39387446"
---
# <a name="localization-in-xamarinios"></a>Xamarin.iOS, yerelleştirme

_Bu belge iOS SDK'sı yerelleştirme özelliklerini ve Xamarin ile erişmeye nasıl ele alınmaktadır._

Başvurmak [uluslararası duruma getirme kodlamaları](encodings.md) Unicode olmayan verilerini işlemelisiniz uygulamalarda karakter kümeleri/kod sayfaları dahil olmak üzere ilgili yönergeler için.

## <a name="ios-platform-features"></a>iOS Platform Özellikleri

Bu bölümde iOS içindeki yerelleştirme özelliklerinden bazıları açıklanmaktadır. Atlamak [sonraki bölümde](#basics) belirli kod ve örnekler görmek için.

### <a name="language"></a>Dil

Kullanıcıların kendi dilinde seçim **ayarları** uygulama. Bu ayar, uygulamalar ve işletim sistemi tarafından gösterilen görüntüleri ve dil dizeleri etkiler. 

Bir uygulamada kullanılan dil belirlemek için ilk öğesi alma `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Bu değer bir dil kodu gibi olacaktır `en` İngilizce ' `es` İspanyolca, `ja` Japonca için vs. Döndürülen değer şunlardan biri (en iyi eşleşmeyi belirlemek için geri dönüş kurallarını kullanarak) uygulama tarafından desteklenen yerelleştirmeler sınırlıdır.

Uygulama kodu her zaman bu değerini – Xamarin denetleyin gerek yoktur ve hem iOS, kullanıcının dili için otomatik olarak doğru dize veya kaynak sağlamak için yardımcı olan özellikler sağlar. Bu belgenin geri kalanında bu özellikleri açıklanmaktadır.

> [!NOTE]
> Kullanım `NSLocale.PreferredLanguages` uygulama tarafından desteklenen yerelleştirmeler bağımsız olarak kullanıcının dil tercihleriyle belirlemek için. İOS 9 değiştirildi. Bu yöntem tarafından döndürülen değer; bkz: [Teknik Not TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) Ayrıntılar için.

### <a name="locale"></a>Yerel Ayar

Kullanıcıları seçin, yerel ayarda **ayarları** uygulama. Bu ayar, tarih, saat, sayı ve para birimi biçimlendirildiğini biçimini etkiler.

Bu, kullanıcıların kendi ondalık ayırıcısı virgül veya bir nokta ve gün, ay ve yıl içinde tarih görüntüleme sırasını olup 12 saat veya 24 saatlik zaman biçimlerinden görüp görmediğinizi seçmesine olanak sağlar.

Xamarin ile hem Apple'nın iOS sınıfları erişebilirsiniz (`NSNumberFormatter`) System.Globalization .NET sınıflarda yanı sıra. Farklı özellikler her kullanılabilir olduğundan, daha iyi kendi gereksinimlerine göre uygun olan geliştiriciler değerlendirmelidir. Özellikle, almaya ve uygulama içi satın alma fiyatları StoreKit kullanarak görüntüleme, döndürülen fiyat bilgileri için Apple'nın biçimlendirme sınıfları kullanmanız gerekir.

Geçerli yerel ayarı iki yoldan birini sorgulanabilir:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

İlk değer işletim sistemi tarafından önbelleğe alınabilir ve böylece kullanıcının şu anda seçili yerel ayar her zaman yansıtmayabilir. Şu anda seçili yerel ayarı almak için ikinci değer kullanın.

> [!NOTE]
> Mono (Xamarin.iOS temel bağlı .NET çalışma zamanı) ve Apple iOS API'leri dil/bölge bileşimleri aynı kümelerini desteklemez.
> Bu nedenle, bir dil/bölge birleşimi iOS seçmek mümkün **ayarları** Mono geçerli bir değer eşlenmiyor uygulama. Örneğin, İspanya için iPhone ait dil İngilizce ve kendi bölge ayarı farklı değerler elde etmek üzere aşağıdaki API'leri neden olur:
> 
> - `CurrentThead.CurrentCulture`: en-US (Mono API)
> - `CurrentThread.CurrentUICulture`: en-US (Mono API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (Apple API'si)
>
> Mono kullandığından `CurrentThread.CurrentUICulture` kaynak seçmek için ve `CurrentThread.CurrentCulture` tarihleri ve para birimleri biçimlendirmek için Mono tabanlı yerelleştirme (örneğin, ile .resx dosyaları) bu dil/bölge birleşimleri için beklenen sonuçlar getirebilir değil. Bu durumda, gerektiğinde yerelleştirmek için Apple'nın API'leri kullanır.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS oluşturur bir `NSCurrentLocaleDidChangeNotification` kullanıcı kendi yerel zaman güncelleştirir. Çalıştıran ve uygun değişiklikleri Arayüzünde yapabileceğiniz rağmen uygulamalar için bu bildirimi dinleyebilirsiniz.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>İOS temel bilgileri yerelleştirme

Aşağıdaki iOS özelliklerini kolayca, kullanıcıya görünen için yerelleştirilmiş kaynaklar sağlamak için Xamarin de yararlanılabilir. Başvurmak [TaskyL10n örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) bu fikirleri uygulamak için.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Varsayılan ve desteklenen dilleri Info.plist içinde belirtme

İçinde [teknik Q & A QA1828: iOS uygulamanızı için dili nasıl belirler](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple iOS uygulamanızı kullanmak için bir dil nasıl seçtiği açıklar. Aşağıdaki faktörleri hangi dil görüntülenen etkiler:

- Kullanıcının tercih edilen diller (bulunan **ayarları** uygulama)
- Yerelleştirmeler (.lproj klasörleri) uygulaması ile birlikte
- `CFBundleDevelopmentRegion` (**Info.plist** uygulama için varsayılan dil belirten değeri)
- `CFBundleLocalizations` (**Info.plist** desteklenen tüm yerelleştirmeler belirten dizi)

Teknik soru- cevap içinde belirtildiği gibi `CFBundleDevelopmentRegion` uygulamanın varsayılan bölge ve dil temsil eder. Uygulamayı açıkça herhangi bir kullanıcının tercih edilen dili desteklemiyorsa, bu alana göre belirtilen dil kullanır. 

> [!IMPORTANT]
> iOS 11 işletim sisteminin önceki sürümlerinde kıyasla kesinlikle bu dil seçimi mekanizması uygular. Bu nedenle, kendi desteklenen yerelleştirmeler – da .lproj klasörler dahil olmak üzere veya için bir değer ayarlayarak açıkça bildirmiyor herhangi bir iOS 11 uygulama `CFBundleLocalizations` – iOS 10 yaptığından farklı bir dil iOS 11 ' görüntülenebilir.

Varsa `CFBundleDevelopmentRegion` içinde belirtilmemiş **Info.plist** dosyasını Xamarin.iOS derleme araçları şu anda varsayılan değeri kullanın `en_US`. Bu gelecekteki bir sürümde değişebilir olsa da varsayılan dil İngilizce olduğu anlamına gelir.

Uygulamanızı bir beklenen dil seçer emin olmak için aşağıdaki adımları uygulayın:

- Varsayılan dili belirtin. Açık **Info.plist** ve **kaynak** görünümü için bir değer ayarlamak için `CFBundleDevelopmentRegion` anahtar; XML biçiminde, aşağıdakine benzer görünmelidir:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

Bu örnekte "es", bir kullanıcının hiçbiri tercih edilen diller desteklenir belirtin, İspanyolca için varsayılan kullanılır.

- Desteklenen tüm yerelleştirmeler bildirin. İçinde **Info.plist**, kullanın **kaynak** görünümü için bir dizi ayarlanacak `CFBundleLocalizations` anahtar; XML biçiminde, aşağıdakine benzer görünmelidir:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

.Resx dosyalarını bu sağlamalısınız gibi .NET mekanizmalarını kullanarak yerelleştirilmiş bir xamarin iOS uygulamaları **Info.plist** değerleri de.

Bunlar hakkında daha fazla bilgi için **Info.plist** anahtarları, Apple'nın atın [bilgi özelliği liste anahtar başvurusu](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html).

### <a name="getlocalizedstring-method"></a>GetLocalizedString yöntemi

`NSBundle.MainBundle.GetLocalizedString` Yöntemi içinde depolanan yerelleştirilmiş metin şuna **.strings** projesindeki dosyalar. Bu dosyalar ile özel olarak adlandırılmış dizinlerde dil tarafından düzenlenir bir **.lproj** soneki.

#### <a name="strings-file-locations"></a>.strings dosya konumları

- **Base.lproj** varsayılan dil kaynakları içeren dizindir.
  Proje kök dizininde genellikle bulunur (ancak aynı zamanda yerleştirilebilir **kaynakları** klasörü).
- **<language>.lproj** dizinleri oluşturulan desteklenen her dil için genellikle **kaynakları** klasör.

Bir dizi farklı olabilir **.strings** her dil dizindeki dosyaları:

- **Localizable.strings** – yerelleştirilmiş metin ana listesi.
- **InfoPlist.strings** – belirli özel anahtarları, uygulama adı gibi şeyler çevirmek için bu dosyayı verilir.
- **< görsel taslak-adı > .strings** – çevirileri için kullanıcı arabirimi öğeleri bir görsel taslak içeren isteğe bağlı bir dosya.

**Derleme eylemi** bu dosyalar için **paket kaynak**.

#### <a name="strings-file-format"></a>.strings dosya biçimi

Yerelleştirilmiş dize değerleri için sözdizimi aşağıdaki gibidir:

```console
/* comment */
"key"="localized-value";
```

Dizelerdeki karakterleri kaçış:

* `\"`  Teklif
* `\\`  Ters eğik çizgi
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

İOS görüntüdeki yerelleştirmek için:

1. Örnek kodda, görüntüye bakın:

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. Varsayılan görüntü dosyası yerleştirin **flag.png** içinde **Base.lproj** (yerel geliştirme dil dizin).

3. İsteğe bağlı olarak resimde yerelleştirilmiş sürümlerini yerleştirin **.lproj** klasörleri her bir dilin (örn.) **Es.lproj**, **ja.lproj**). Aynı adı kullanın **flag.png** her dil dizinde.

Görüntünün belirli bir dil için mevcut değilse, iOS varsayılan dil yerel klasöre geri döner ve buradan resim yüklenemiyor.


#### <a name="launch-images"></a>Başlatma görüntüleri

Başlatma görüntüleri (ve XIB veya görsel taslak iPhone 6 modelleri için) için standart adlandırma kurallarını kullanın. bunları yerleştirirken **.lproj** her dil için dizinler.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Uygulama adı

Yerleştirme bir **InfoPlist.strings** dosyası bir **.lproj** dizin uygulamanın bazı değerleri geçersiz kılmanıza olanak tanır **Info.plist**, uygulama adı dahil olmak üzere:

```console
"CFBundleDisplayName" = "LeónTodo";
```

İçin kullanabileceğiniz diğer anahtarlar [uygulamaya özgü dizelerini yerelleştirmek](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) şunlardır:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>Tarihler ve saatler

Yerleşik .NET date ve time işlevleri kullanmak mümkün olsa da (geçerli birlikte `CultureInfo`) yönelik bir yerel ayar tarihleri ve saatleri biçimlendirmek için bu yerel ayara özgü kullanıcı (Bu dil ayrı olarak ayarlanabilir) ayarları yok.

İOS kullanma `NSDateFormatter` kullanıcının yerel ayar tercihiyle aynı çıktı oluşturmak için. Aşağıdaki örnek kod, temel bir tarih ve saat biçimlendirme seçenekleri gösterir:

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

Amerika Birleşik Devletleri İngilizce Sonuçları:

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

İspanya'daki İspanyolca sonuçları:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Apple'a başvuran [tarih Biçimlendiricileri](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) daha fazla bilgi için belgelere bakın. Yerel ayar duyarlı tarih ve saat biçimlendirme sınarken her ikisini de denetlemek **iPhone dil** ve **bölge** ayarları.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Sağdan sola (RTL) düzeni

iOS RTL kullanan uygulamalar oluşturmaya yardımcı olmak için özellikleri sunar:

* Kullanım Otomatik Düzen'ın `leading` ve `trailing` (, sol ve sağ İngilizce için karşılık gelen, ancak RTL dilleri için ters çevrilir) denetim aligment için öznitelikler.
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Denetimidir RTL bilincinde olmasını denetimleri yerleştirmek için yararlıdır.
* Kullanım `TextAlignment = UITextAlignment.Natural` (Bu, çoğu dil ancak sağ RTL bırakılır) metin hizalama için.
* `UINavigationController` otomatik olarak geri düğmesini çevirir ve manyetik yön tersine döner.

Aşağıdaki ekran görüntüleri Göster [yerelleştirilmiş Tasky örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) Arapça ve İbranice (İngilizce alanlara girilen rağmen):

[![](images/rtl-ar-sml.png "Arapça yerelleştirme")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "İbranice yerelleştirme")](images/rtl-he.png#lightbox "Hebrew")

iOS otomatik olarak ters çevrilip `UINavigationController`, ve diğer denetimleri içine yerleştirilir `UIStackView` veya otomatik düzen ile hizalanır.
Sağdan sola metin yerelleştirilmiş kullanarak **.strings** dosyaları aynı şekilde LTR metin.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Kullanıcı Arabiriminde kod yerelleştirme

[(Kodda yerelleştirilmiş) Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) örnek kullanıcı arabirimi kod (yerine XIBs veya film şeritleri) burada oluşturulan bir uygulamayı yerelleştirme işlemini gösterir.

### <a name="project-structure"></a>Proje yapısı

![](images/solution-code.png "Kaynakları ağacı")

### <a name="localizablestrings-file"></a>Localizable.strings dosyası

Yukarıda açıklandığı gibi **Localizable.strings** dosya biçimi, anahtar-değer çiftleri içerir. Anahtar dize amacını açıklar ve uygulamada kullanılmak üzere çevrilen metni değerdir.

İspanyolca (**es**) yerelleştirmeler örnek için aşağıda gösterilmektedir:

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

Uygulama kodunda (olduğunu bir etiketin metin veya bir girdinin yer tutucusu, vb.) bir kullanıcı arabiriminin görünen metin ayarlanır her yerde kod iOS kullanır. `GetLocalizedString` işlevi görüntülemek için doğru çeviri almak için:

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>Görsel taslak kullanıcı arabirimleri yerelleştirme

Örnek [Tasky (yerelleştirilmiş film şeridi)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) metin denetimleri bir görsel taslak üzerinde yerelleştirmek gösterilmektedir.

### <a name="project-structure"></a>Proje yapısı

**Base.lproj** dizin film şeridi içerir ve ayrıca uygulamada kullanılan herhangi bir görüntü içermelidir.

Diğer bir dil dizinleri içeren bir **Localizable.strings** kodda başvurulan tüm dize kaynakları için dosya yanı sıra bir **MainStoryboard.strings** metin çevirileri içeren dosya görsel taslak.

![](images/solution-storyboard.png "Kaynakları ağacı")

Dil dizinleri, mevcut bir geçersiz kılmak için yerelleştirilmiş herhangi bir görüntü kopyası içermelidir **Base.lproj**.

### <a name="object-id--localization-id"></a>Nesne Kimliği / yerelleştirme kimliği

Oluşturma ve düzenleme denetimleri bir görsel taslağın içinde her denetimi seçin ve yerelleştirme için kullanılacak kimliği:

* Mac için Visual Studio'da yer almaktadır **özellikler panelinde** ve çağrılır **yerelleştirme kimliği**.
* Xcode'da, çağrıldığı **nesne kimliği**.

Bu dize değeri, genellikle aşağıdaki ekran görüntüsünde gösterildiği gibi "NF3-h8-xmR" gibi bir biçime sahiptir:

![](images/xs-designer-localization-id.png "Görsel taslak yerelleştirme Xcode görünümü")

Bu değer kullanılıyor **.strings** çevrilmiş metnin her denetim için otomatik olarak atamak için dosya.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Görsel taslak çeviri dosyasının biçimi benzer **Localizable.strings** dosya hariç, anahtar (sol değer), kullanıcı tanımlı olamaz ancak bunun yerine belirli bir biçimde olmalıdır: `ObjectID.property`.

Örnekte **Mainstoryboard.strings** aşağıda görebilirsiniz `UITextField`s sahip bir `placeholder` yerelleştirilebilen; text özelliği `UILabel`s sahip bir `text` özelliği; ve `UIButton`s varsayılan metni kullanılarak ayarlanır `normalTitle`:

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
> Boyut sınıfları ile görsel taslak kullanarak uygulamada görünmez çevirileri neden olabilir. [Apple'nın Xcode sürüm notları](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) üç şey doğruysa, bir görsel taslak veya XIB doğru yerelleştirmek değil olduğunu gösterir: boyut sınıflarını kullanan Evrensel temel yerelleştirme hem de yapı hedefi ayarlanır ve iOS 7.0 derleme hedefi. İki özdeş dosyalarına görsel taslak dizeleri dosyanızı oluşturmaya düzeltmesidir: **MainStoryboard~iphone.strings** ve **MainStoryboard~ipad.strings**aşağıdaki ekran görüntüsünde gösterildiği gibi:
> 
> ![](images/xs-dup-strings.png "Dizeleri dosyaları")

<a name="appstore" />

## <a name="app-store-listing"></a>App Store listeleme

Apple'nın SSS izleyen [App Store yerelleştirme](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) çevirileri uygulamanızı satışta olduğu her bir ülkede girmek için. Uygulamanızı ayrıca yerelleştirilmiş bir içeriyorsa görünecek çevirileri yalnızca kendi uyarı Not **.lproj** dil için dizin.

## <a name="summary"></a>Özet

Bu makale, yerleşik kaynak işleme ve görsel taslak özellikleri kullanarak iOS uygulamalarını yerelleştirme temellerini kapsar.

İOS, Android ve platformlar arası uygulama (Xamarin.Forms dahil) için i18n ve L10n hakkında daha fazla edinebilirsiniz [bu platformlar arası rehberi](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>İlgili bağlantılar

- [(Kod yerelleştirilmiş) Tasky (örnek)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [(Yerelleştirilmiş film şeridi) Tasky (örnek)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple yerelleştirme Kılavuzu](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Platformlar arası yerelleştirmesine genel bakış](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms yerelleştirme](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android yerelleştirme](~/android/app-fundamentals/localization.md)
