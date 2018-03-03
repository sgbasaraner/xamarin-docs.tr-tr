---
title: "iOS yerelleştirme"
description: "Bu belgede iOS SDK'ın yerelleştirme özelliklerini ve Xamarin ile erişmek nasıl kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: ea91dbcf7148651cb5d10acae4ada8bb6758c39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-localization"></a>iOS yerelleştirme

_Bu belgede iOS SDK'ın yerelleştirme özelliklerini ve Xamarin ile erişmek nasıl kapsar._

Başvurmak [uluslararası Kodlamalar](encodings.md) Unicode olmayan veri işlemelidir uygulamalarında karakter kümelerini/kod sayfaları dahil olmak üzere ilgili yönergeler için.

## <a name="ios-platform-features"></a>iOS platformu özellikleri

Bu bölümde iOS içindeki yerelleştirme özelliklerinden bazıları açıklanmaktadır. Geçin [sonraki bölümde](#basics) özel kod ve örnekler görmek için.

### <a name="language"></a>Dil

Kullanıcılar kendi dilini seçin **ayarları** uygulama. Bu ayar, işletim sistemi yanı sıra dil ayarları algılayan uygulamaları görüntülenen görüntüleri ve dil dizeleri etkiler.

Kullanıcıların İngilizce, İspanyolca, Japonca, Fransızca veya kendi uygulamalarında görüntülenen başka bir dilde görmek isteyip istemediklerini burada karar verir budur.

Gerçek iOS 7 desteklenen dillerin listesi: İngilizce (ABD), İngilizce (Birleşik Krallık), Çince (Basitleştirilmiş), Çince (Geleneksel), Fransızca, Almanca, İtalyanca, Japonca, Korece, İspanyolca, Arapça, Katalanca, Hırvatça, Çekçe, Danca, Felemenkçe, Fince, Yunanca, İbranice Macarca, Endonezya dili, Malayca, Norveççe, Lehçe, Portekizce, Portekizce (Brezilya), Rumence, Rusça, Slovakça, İsveççe, Tay dili, Türkçe, Ukrayna dili, Vietnam dili, İngilizce (Avustralya), İspanyolca (Meksika).

Geçerli dil ilk öğesi erişerek sorgulanabilir `PreferredLanguages` dizi:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

Bu değer bir dil kodu gibi olacaktır `en` İngilizce ' `es` İspanyolca için `ja` Japonca için vb. _Döndürülen değer (en iyi eşleşmeyi belirlemek için geri dönüş kurallarını kullanarak) uygulama tarafından desteklenen yerelleştirmeler birini sınırlıdır._

Uygulama kodu bu değer için - Xamarin denetlemek her zaman gerekmez ve iOS hem de otomatik olarak kullanıcının dil için doğru dize veya kaynak sağlamak için yardımcı olan özellikler sağlar. Bu belgenin geri kalanında bu özellikleri açıklanmaktadır.

> [!NOTE]
> **Not:** iOS 9 önce önerilen kodu: `var lang = NSLocale.PreferredLanguages [0];`.
>
> İOS 9 - değiştirilen bu kodu tarafından döndürülen sonuçları görmek [Teknik Not TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) daha fazla bilgi için.
>
> Kullanmaya devam edebilirsiniz `NSLocale.PreferredLanguages [0]` kullanıcı tarafından seçilen gerçek değerler belirlemek için (yerelleştirmeler bağımsız olarak uygulamanız destekler).

### <a name="locale"></a>Yerel Ayar

Kullanıcıların kendi yerel ayarda seçim **ayarları** uygulama. Bu ayar, tarihler, saatler, sayılar ve para birimi biçimlendirileceğini biçimini etkiler.

Bu, kullanıcıların kendi ondalık ayırıcı virgül veya bir nokta, gün, ay ve yıl içinde tarih görüntüleme sırasını olup olmadığını ve saat biçimleri, 12 veya 24 saat Bkz seçin sağlar.

Xamarin ile hem Apple'nın iOS sınıfları erişiminiz (`NSNumberFormatter`) System.Globalization .NET sınıflarda yanı sıra. Her kullanılabilen farklı özellikler gibi daha iyi kendi gereksinimlerine göre uygun olan geliştiriciler değerlendirmelidir. Özellikle, alma ve StoreKit kullanarak, uygulama içi satın alma fiyatlar görüntüleme döndürülen fiyat bilgileri için Apple'nın biçimlendirme sınıfları kesinlikle kullanmalısınız.

Geçerli yerel ya da sorgulanabilir:

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

İlk değeri işletim sistemi tarafından önbelleğe alınabilir ve böylece her zaman kullanıcının şu anda seçili yerel gösterebilir değil. Seçili yerel edinmek için ikinci değeri kullanın.


### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS oluşturan bir `NSCurrentLocaleDidChangeNotification` kullanıcı kendi yerel zaman güncelleştirir. Uygulamalar bu bildirim için dinleme çalıştırıyorsanız ve kullanıcı Arabirimi için uygun değişiklikleri yapın.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>İOS içinde yerelleştirme temelleri

İOS aşağıdaki özelliklerini kolayca, kullanıcıya görünen için yerelleştirilmiş kaynaklar sağlamak için Xamarin de yararlanılabilir. Başvurmak [TaskyL10n örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) bu fikirleri uygulamak nasıl görmek için.

### <a name="infoplist"></a>Info.plist

Başlamadan önce yapılandırma **Info.plist** aşağıdaki anahtarları dosyasıyla:

- `CFBundleDevelopmentRegion` -Uygulama (geliştiriciler tarafından konuşulan ve film şeritleri ve dize ve görüntü kaynakları kullanılan genellikle dili) için varsayılan dili. Aşağıdaki örnekte **tr** (İngilizce için) belirtilir.
- `CFBundleLocalizations` -bir dizi de gibi dil kodlarını kullanarak bir uygulama tarafından desteklenen diğer yerelleştirmeler **es** (İspanyolca) ve **pt-PT** (Portekizce Portekiz içinde açıkça söylenir).

```xml
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
<key>CFBundleLocalizations</key>
<array>
  <string>de</string>
  <string>es</string>
  <string>ja</string>
  ...
</array>
```

### <a name="localizedstring-method"></a>LocalizedString yöntemi

`NSBundle.MainBundle.LocalizedString` Yöntemi içinde depolanan yerelleştirilmiş metin arayan **.strings** projedeki dosyaları. Bu dosyalar ile özel olarak adlandırılmış dizinlerde dile göre düzenlenir bir **.lproj** soneki.

#### <a name="strings-file-locations"></a>.strings dosya konumları

- **Base.lproj** varsayılan dil kaynakları içeren dizindir.
  Genellikle proje kök dizininde bulunur (ancak aynı zamanda yerleştirilebilir **kaynakları** klasörü).
- **<language>.lproj** dizinleri oluşturulan desteklenen her dil için genellikle **kaynakları** klasör.

Bir dizi farklı olabilir **.strings** her dil dizindeki dosyaları:

- **Localizable.strings** -yerelleştirilmiş metin ana listesi.
- **InfoPlist.strings** - belirli özel anahtarları, uygulama adı gibi şeyleri çevirmek için bu dosyada verilir.
- **< film şeridi-adı > .strings** -film şeridi kullanıcı arabirimi öğeleri için çeviriler içeren isteğe bağlı bir dosya.

**Yapı eylemi** bu dosyaları olmalıdır **paket kaynak**.

#### <a name="strings-file-format"></a>.strings dosya biçimi

Yerelleştirilmiş dize değerlerini sözdizimi aşağıdaki gibidir:

```csharp
/* comment */
"key"="localized-value";
```

Aşağıdaki karakterleri dizelere kaçış:

* `\"`  Teklif
* `\\`  ters eğik çizgi
* `\n`  Yeni satır

Bir örnek **es/Localizable.strings** (örn. Örnek dosyasından İspanyolca):

```csharp
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

3. İsteğe bağlı olarak görüntüde yerelleştirilmiş sürümleri yerleştirin **.lproj** klasörleri her bir dilin (ör.) **es.lproj**, **ja.lproj**). Aynı adı kullanmak **flag.png** her dil dizinde.

Görüntü için belirli bir dil yoksa, iOS varsayılan yerel dili klasörüne geri dönmesi ve görüntünün buradan yükleyin.


#### <a name="launch-images"></a>Görüntüleri başlatma

Başlatma görüntüleri (ve XIB veya film şeridi iPhone 6 modelleri için) için standart adlandırma kuralları kullanmak bunları yerleştirirken **.lproj** her dil için dizinler.

```csharp
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>Uygulama adı

Yerleştirme bir **InfoPlist.strings** dosyasını bir **.lproj** dizin uygulamanın bazı değerleri geçersiz kılmanıza olanak tanır **Info.plist**, uygulama adı dahil olmak üzere:

```csharp
"CFBundleDisplayName" = "LeónTodo";
```

İçin kullanabileceğiniz diğer anahtarlar [uygulamaya özgü dizeleri yerelleştirme](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) şunlardır:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright


<!--
## App icon

Does not seem to be possible (although it definitely used to be!).
-->

### <a name="localization-native-development-region"></a>Yerelleştirme yerel geliştirme bölge

Varsayılan dizeleri (bulunan **Base.lproj** klasörü) 'geri dönüş' dili olarak varsayılacak. Bunun anlamı bir çeviri kodda istenen ve için bulunamadı bir geçerli dil **Base.lproj** klasörü varsayılan dizeyi kullanmak arama yapılır (eşleşme bulunamazsa, çeviri tanımlayıcı dizesinde kendisi ise görüntülenen).

Geliştiriciler, geri dönüş plist anahtarını ayarlayarak olması için farklı bir dil seçebilirsiniz `CFBundleDevelopmentRegionKey`. Değeri, yerel geliştirme dilini dil kodu ayarlanmalıdır. Bu ekran görüntüsü, yerel geliştirme bölgesiyle Xamarin Studio'da plist İspanyolca (es) ayarlamak gösterir:

![](images/cfbundledevelopmentregion.png "InfoPList.strings özelliği")

Film şeritleri ve kodunuz genelinde kullanılan varsayılan dil İngilizce değilse, uygulama kodunuzda kullanılan yerel dili yansıtmak için bu değeri ayarlayın.

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

```csharp
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

İspanyolca İspanya için sonuçları:

```csharp
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Apple'a başvuran [tarih Biçimlendiricileri](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) daha fazla bilgi için. Yerel ayara duyarlı tarih ve saat biçimlendirmesi sınarken her ikisini de denetlemek **iPhone dil** ve **bölge** ayarlar.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>Sağdan sola (RTL) düzeni

iOS RTL algılayan uygulamaları oluşturmaya yardımcı olmak üzere birçok özellik sağlar:

* Kullanım **Otomatik Düzen** `leading` ve `trailing` denetim aligment öznitelikleri (için karşılık geldiği *sol* ve *sağ* İngilizce için örneğin, ancak. RTL dilleri için tersine).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) Denetim, RTL bulundurmanız denetimleri yerleştirmek için özellikle yararlıdır.
* Kullanım `TextAlignment = UITextAlignment.Natural` metin hizalaması için (hangi *sol* çoğu diller için ancak *sağ* RTL için).
* `UINavigationController` otomatik olarak geri düğmesini çevirir ve geçirme yönünü tersine çevirir.

Aşağıdaki ekran görüntüleri Göster [yerelleştirilmiş **Tasky** örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) Arapça ve İbranice (İngilizce alanları girilmiş rağmen):

[ ![](images/rtl-ar-sml.png "Arapça yerelleştirme")](images/rtl-ar.png "Arabic") 

[ ![](images/rtl-he-sml.png "İbranice yerelleştirme")](images/rtl-he.png "Hebrew")

iOS otomatik olarak ters çevrilip `UINavigationController`, ve diğer denetimlerin içine yerleştirilen `UIStackView` veya otomatik düzeniyle hizalı.
RTL metin yerelleştirilmiş kullanarak **.strings** dosyaları aynı şekilde LTR metin olarak.


<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>Kullanıcı Arabiriminde kod yerelleştirme

[(Kodda yerelleştirilmiş) Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) örnek kod (yerine XIBs veya film şeritleri) kullanıcı arabirimi burada yerleşik uygulama yerelleştirme nasıl gösterir.

### <a name="project-structure"></a>Proje yapısı



![](images/solution-code.png "Kaynakları ağacı")

### <a name="localizablestrings-file"></a>Localizable.strings dosyası

Yukarıda açıklandığı gibi **Localizable.strings** dosya biçimi belirten bir kullanıcı tarafından seçilen dize olduğu anahtarla anahtar-değer çiftlerini oluşur

İspanyolca (**es**) yerelleştirmeler örnek aşağıda gösterilmektedir:

```csharp
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

* Mac için Visual Studio'da bu özellikleri defterinde bulunan ve adlı **yerelleştirme kimliği**.
* Xcode'da, adlı **nesne kimliği**.

Bu genellikle gibi bir form içeren bir dize değeridir **"NF3 h8 xmR"**:

![](images/xs-designer-localization-id.png "Film şeridi yerelleştirme Xcode görünümü")

Bu değer kullanılır **.strings** çevrilmiş metni her denetim için otomatik olarak atamak için dosya.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

Film şeridi çeviri dosyasının biçimi benzer **Localizable.strings** dosya dışında anahtar (sol değer) kullanıcı tarafından tanımlanan olamaz, ancak bunun yerine belirli bir biçimde olmalıdır: `ObjectID.property`.

Örnekte **Mainstoryboard.strings** aşağıda görebilirsiniz `UITextField`s sahip bir `placeholder` yerelleştirilebilir; metin özelliği `UILabel`s sahip bir `text` özellik; ve `UIButton`s varsayılan metin kullanılarak ayarlanır `normalTitle`:

```csharp
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> **Film şeridi boyutu sınıflarıyla kullanarak** görüntülenmiyor Çeviriler neden olabilir. Bu büyük olasılıkla ilgili [bu sorunu](http://stackoverflow.com/questions/24989208/xcode-6-does-not-localize-interface-builder) Apple belgeleri nerede diyor: film şeridi veya XIB değil yerelleştirme doğru aşağıdaki üç koşulların tümü doğruysa, yerelleştirme: film şeridi veya XIB boyutu sınıfları kullanır. Temel yerelleştirme ve yapı hedefi Evrensel ayarlanır. Yapı iOS 7.0 hedefler.
İki özdeş dosyalarıyla film şeridi dizeleri dosyanızı çoğaltmak için durumda düzeltme: **MainStoryboard~iphone.strings** ve **MainStoryboard~ipad.strings**:

> ![](images/xs-dup-strings.png "Dizeleri dosyaları")


<!--
# Native Formatting

Xamarin.iOS applications can take advantage of the .NET framework's formatting options for localizing
numbers and dates.

### Date string formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1


NSDateFormatter formatter = new NSDateFormatter ();
formatter.DateFormat = "MMMM/dd/yyyy";
NSString dateString = new NSString (formatter.ToString (d));


### Number formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfNumberFormatting10_4.html#//apple_ref/doc/uid/TP40002368-SW1

-->

<a name="appstore" />

## <a name="app-store-listing"></a>Uygulama mağazası listeleme

Apple'nın SSS izleyen [App Store yerelleştirme](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) uygulamanızı indirimde olan her ülke çevirileri girmek için. Uygulamanızı da bir yerelleştirilmiş içeriyorsa görünecek çevirileri yalnızca kendi uyarı Not **.lproj** dil için dizin.

<!--

Once you’ve entered your application into iTunes Connect the default language
metadata and screenshots will appear as shown:

![]( "itunes connect 1")

Use the language list on the right to select other languages to provide
translated application name, description, search keywords, URLs and screenshots.
The complete list of languages is shown in this screenshot:

![]( "itunes connect 2")
-->

## <a name="summary"></a>Özet

Bu makalede yerleşik kaynak işleme ve film şeridi özelliklerini kullanarak iOS uygulamaları yerelleştirme temelleri kapsar.

İçinde (Xamarin.Forms dahil) iOS, Android ve platformlar arası uygulamalar için i18n ve L10n hakkında daha fazla öğrenebilirsiniz [bu platformlar arası kılavuz](~/cross-platform/app-fundamentals/localization.md).


## <a name="related-links"></a>İlgili bağlantılar

- [(Kodda yerelleştirilmiş) Tasky (örnek)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (yerelleştirilmiş film şeridi) (örnek)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple yerelleştirme Kılavuzu](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [Platformlar arası yerelleştirme genel bakış](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms yerelleştirme](~/xamarin-forms/app-fundamentals/localization.md)
- [Android yerelleştirme](~/android/app-fundamentals/localization.md)
