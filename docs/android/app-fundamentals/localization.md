---
title: "Android yerelleştirme"
description: "Bu belgede, Android SDK ve Xamarin ile erişmek nasıl yerelleştirme özelliklerini tanıtır."
ms.topic: article
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: adfc0da404c6b9df79c3b2be51f8cafa302a6bc3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="android-localization"></a>Android yerelleştirme

_Bu belgede, Android SDK ve Xamarin ile erişmek nasıl yerelleştirme özelliklerini tanıtır._

## <a name="android-platform-features"></a>Android platformu özellikleri

Bu bölüm Android ana yerelleştirme özelliklerini açıklar. Geçin [sonraki bölümde](#basics) özel kod ve örnekler görmek için.

### <a name="locale"></a>Yerel Ayar

Kullanıcılar kendi dilini seçin **ayarlar > dili & Giriş**. Bu seçim görüntülenen dil ve kullanılan bölgesel ayarları (ör. denetler Tarih ve sayı biçimlendirmesini için).

Geçerli yerel ayar geçerli bağlamın sorgulanabilir `Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

Bu değer, bir dil kodu ve bir çizgiyle ayrılmış bir yerel ayar kodu içeren bir yerel ayar tanımlayıcısı olacaktır. Başvuru için İşte bir [Java yerel ayarlar listesi](http://www.oracle.com/technetwork/java/javase/locales-137662.html) ve [StackOverflow aracılığıyla yerel Android desteklenen](http://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

Ortak örnekler:

* `en_US` İngilizce (Birleşik Statees)
* `es_ES` İspanyolca (İspanya)
* `ja_JP` Japonca (Japonya)
* `zh_CN` Çince (Çin)
* `zh_TW` Çince (Tayvan)
* `pt_PT` Portekizce (Portekiz)
* `pt_BR` Portekizce (Brezilya)

### <a name="localechanged"></a>LOCALE_CHANGED

Android oluşturur `android.intent.action.LOCALE_CHANGED` kullanıcı kendi dil seçimi değiştiğinde.

Etkinlikler kabul bu ayarlayarak işlemek için `android:configChanges` özniteliği faaliyete şöyle:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Uluslararası duruma getirme temelleri Android içinde

Android'ın yerelleştirme stratejisi aşağıdaki anahtar bölümleri şunlardır:

- Yerelleştirilmiş dizeleri, görüntüler ve diğer kaynakları içeren kaynak klasörleri'ni kullanın.

- `GetText` yerelleştirilmiş dizeleri kodda almak için kullanılan yöntemi

- `@string/id` yerelleştirilmiş dizeleri düzenleri otomatik olarak yerleştirilecek AXML dosyalarında.

### <a name="resource-folders"></a>Kaynak klasörleri

Android uygulamaları gibi kaynak klasörlerde çoğu içeriğini yönetin:

* **Düzen** -AXML düzeni dosyalarını içerir.
* **drawable** -görüntüler ve diğer drawable kaynakları içerir.
* **değerleri** -dizeleri içerir.
* **Ham** -veri dosyalarını içerir.

Kullanımı ile Çoğu geliştirici bilginiz **DPI** üzerinde sonekleri **drawable** her aygıt için doğru sürüm seçin Android izin vererek, görüntüyü birden fazla sürümünü sağlamak için dizin. Aynı mekanizması, dil ve kültür tanımlayıcıları kaynak dizinlerle suffixing tarafından birden çok dil çevirileri sağlamak için kullanılır.

![Birden çok kültürel tanımlayıcıları için kaynakları/drawable ve kaynakları/değerleri klasörlerinin ekran görüntüsü](localization-images/resources.png)

> [!NOTE]
> **Not:** gibi üst düzey bir dili belirtirken `es` iki karakter gerekli; yalnızca tam bir yerel ayar belirtirken, dizin adı biçimi tire ve küçük gerektirir ancak **r** iki ayırmak için Örneğin, bölümleri **pt rBR** veya **zh-rCN**. Bu, alt çizgi (ör. olan kodda, döndürülen değer karşılaştırma `pt_BR`). Bunların her ikisi de .NET değerine farklı `CultureInfo` sınıfını kullanır, sahip olduğu bir tire yalnızca (ör.) `pt-BR`). Xamarin platformlarında çalışırken bu farklılıklar aklınızda tutun.

#### <a name="stringsxml-file-format"></a>Strings.XML dosya biçimi

Yerelleştirilmiş bir **değerleri** dizine (ör.) **değerleri es** veya **değerleri pt rBR**) adlı bir dosya içermelidir **Strings.xml** yerel çevrilmiş metni içerecek.

Her bir XML öğesi olarak belirtilen kimliği kaynak ile çevrilebilir dizedir `name` özniteliği ve çevrilmiş dize değeri olarak:

```xml
<string name="app_name">TaskyL10n</string>
```

Normal XML kurallara göre atlamanız gerekir ve `name` geçerli bir Android kaynak kimliği (boşluk veya tire) olmalıdır. Burada, örnek için varsayılan dizeleri (İngilizce) dosyası örneği verilmiştir:

**values/Strings.xml**

```xml
<resources>
    <string name="app_name">TaskyL10n</string>
    <string name="taskadd">Add Task</string>
    <string name="taskname">Name</string>
    <string name="tasknotes">Notes</string>
    <string name="taskdone">Done</string>
    <string name="taskcancel">Cancel</string>
</resources>
```

İspanyolca dizin **değerleri es** aynı ada sahip bir dosya içerir (**Strings.xml**) çevirileri içerir:

**değerleri-es/Strings.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TaskyLeon</string>
    <string name="taskadd">agregar tarea</string>
    <string name="taskname">Nombre</string>
    <string name="tasknotes">Notas</string>
    <string name="taskdone">Completo</string>
    <string name="taskcancel">Cancelar</string>
</resources>
```

![Birden çok ekran klasörler, her bir Strings.xml dosyasını içeren değerleri](localization-images/values.png)

Dizeleri dosyaları kurulum ile çevrilmiş değerlerin düzenlerini ve kod başvurulabilir.

### <a name="axml-layout-files"></a>AXML düzeni dosyaları

Yerelleştirilmiş dizeleri düzeni dosyalarında başvurmak için `@string/id` sözdizimi. Bu örnek gösterir gelen XML parçacığını `text` özellikleri ile ayarlanan yerelleştirilmiş kaynak kimlikleri (diğer bir öznitelikleri atlanmış):

```xml
<TextView
    android:id="@+id/NameLabel"
    android:text="@string/taskname"
    ... />
<CheckBox
    android:id="@+id/chkDone"
    android:text="@string/taskdone"
    ... />
```

### <a name="gettext-method"></a>GetText yöntemi

Kodda çevrilen dizeleri almak için kullanmak `GetText` yöntemi ve kaynak kimliği geçirin:

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>Miktar dizeleri

Android dize kaynaklarını de olanak tanır oluşturmanıza *miktar dizeleri* farklı miktarları için farklı Çeviriler sağlamak üzere çevirmenler izin ver:

* "Sol 1 görev var."
* "2 vardır görevleri hala yapmak için."

(genel yerine "vardır sol n görevlerin").

İçinde **Strings.xml**

```xml
<plurals name="numberOfTasks">
         <!--
                    As a developer, you should always supply "one" and "other"
                    strings. Your translators will know which strings are actually
                    needed for their language.
             -->
         <item quantity="one">There is %d task left.</item>
         <item quantity="other">There are %d tasks still to do.</item>
 </plurals>
```

Tam dize kullanım işlemek için `GetQuantityString` kaynak kimliği ve değer görüntülenecek geçirerek yöntemi (hangi geçirilen iki kez). İkinci parametre Android tarafından belirlemek için kullanılan *hangi* `quantity` kullanılacak dize, üçüncü parametre (her ikisi de gereklidir) dizeye gerçekten yerine değeridir.

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

Geçerli `quantity` anahtarlar şunlardır:

* sıfır
* bir
* iki
* birkaç
* birçok
* Diğer

Daha ayrıntılı olarak açıklanan [Android belgeleri](http://developer.android.com/guide/topics/resources/string-resource.html#Plurals). Belirli bir dile 'özel' işleme, bu gerektirmiyorsa `quantity` dizeleri yok sayılacak (örneğin, yalnızca kullandığı İngilizce `one` ve `other`; belirten bir `zero` dize herhangi bir etkisi olacaktır, değil kullanılır).

### <a name="images"></a>Görüntüler

Yerelleştirilmiş görüntüleri dizeleri dosyalarıyla aynı kuralları izleyin: uygulamada başvurulan tüm görüntüleri yerleştirilmelidir **drawable** ; böylece bir geri dönüş dizinleri.

Yerel ayarlara özgü görüntüleri sonra yerleştirilmelidir tam drawable klasörlerde gibi **es drawable** veya **ja drawable** (dpi tanımlayıcıları da eklenebilir).

Bu ekran görüntüsünde, dört görüntüleri kaydedilir **drawable** dizin, ancak yalnızca bir **flag.png**, diğer dizinlerde kopyaları yerelleştirilmiş.

![Her bir veya daha fazla yerelleştirilmiş .png dosyalarını içeren ekran birden çok drawable klasörlerde görüntüsü](localization-images/drawable.png)


#### <a name="other-resource-types"></a>Diğer kaynak türleri

Bu gibi durumlarda, alternatif, başka türlerde ayrıca düzenleri, animasyonları ve raw dosyaları dahil olmak üzere dile özgü kaynaklar sağlayabilirsiniz. Bir veya daha fazla hedef diller için belirli ekranı düzeni sağlayabilir anlamına gelir, örneğin özellikle çok uzun metin etiketlerini verir Almanca bir düzen oluşturabilirsiniz.

Android 4.2 sunulan desteği [sağdan sola (RTL) dilleri](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) uygulama ayarı ayarlarsanız `android:supportsRtl="true"`. Kaynak niteleyici `"ldrtl"` RTL görüntülemek için tasarlanmış özel düzenler içerecek şekilde bir direcory ad dahil edilebilir.

Android belgeleri için kaynak Dizin adlandırmada ve geri dönüş hakkında daha fazla bilgi için başvurmak [alternatif kaynak sağlanmasını](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).


### <a name="app-name"></a>Uygulama adı

Uygulama adı kullanarak yerelleştirme kolay bir `@string/id` içinde için `MainLauncher` etkinlik:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>Sağdan sola (RTL) dilleri

Android 4.2 ve daha yeni ayrıntılı olarak açıklanmıştır RTL düzenleri için tam destek sağlar [yerel RTL destek blog](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html).

Android 4.2 (API düzeyi 17) kullanırken ve değerleri ile belirtilebilir daha yeni aligment `start` ve `end` yerine `left` ve `right` (örneğin `android:paddingStart`). Ayrıca yeni API'leri gibi olan `LayoutDirection`, `TextDirection`, ve `TextAlignment` uyum ekranlar RTL okuyucular için oluşturmanıza yardımcı olmak üzere.

Aşağıdaki ekran görüntüsü gösterildiği [yerelleştirilmiş **Tasky** örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) Arapça:

[![Arapça Tasky uygulamasının ekran görüntüsü](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png) 

Sonraki hali gösterilmektedir [yerelleştirilmiş **Tasky** örnek](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) İbranice:

[![İbranice Tasky uygulamasının ekran görüntüsü](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png)

RTL metin yerelleştirilmiş kullanarak **Strings.xml** dosyaları aynı şekilde LTR metin olarak.

<a name="testing" />

## <a name="testing"></a>Sınama

Varsayılan yerel ayar sınamanız emin olun. Varsayılan kaynaklar (örneğin eksik olan) herhangi bir nedenden dolayı yüklerse, uygulama kilitleniyor.

### <a name="emulator-testing"></a>Sınama öykünücüsü

Google için başvuruda [Android öykünücüsünde sınama](https://developer.android.com/guide/topics/resources/localization.html#testing) bölüm ADB Kabuğu'nu kullanarak belirli bir yerel ayar için bir öykünücü ayarlama hakkında yönergeler için.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>Cihaz test etme

Bir cihazda sınamak için dilde değiştirme **ayarları** uygulama.
**İpucu:** böylece dil özgün ayarına geri dönebilirsiniz simgeleri Not ve menü öğelerinin konumu olun.


## <a name="summary"></a>Özet

Bu makalede, yerleşik kaynak yönetimi kullanarak Android uygulamaları yerelleştirme temelleri yer almaktadır. İOS, Android ve platformlar (dahil Xamarin.Forms) uygulamaları için i18n ve L10n hakkında daha fazla bilgi edinebilirsiniz [bu platformlar arası kılavuz](~/cross-platform/app-fundamentals/localization.md).



## <a name="related-links"></a>İlgili bağlantılar

- [(Kodda yerelleştirilmiş) Tasky (örnek)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Android ile kaynakları yerelleştirme](http://developer.android.com/guide/topics/resources/localization.html)
- [Platformlar arası yerelleştirme genel bakış](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms yerelleştirme](~/xamarin-forms/app-fundamentals/localization.md)
- [iOS yerelleştirme](~/ios/app-fundamentals/localization/index.md)
