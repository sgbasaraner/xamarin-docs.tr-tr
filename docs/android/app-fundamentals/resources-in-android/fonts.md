---
title: Yazı Tipleri
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 04/13/2018
ms.openlocfilehash: 086576ea7d806bb0768fbe4563df7fca99244ccb
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="fonts"></a>Yazı Tipleri

## <a name="overview"></a>Genel Bakış

API düzeyi 26 ile başlayarak, Android SDK'sı bir düzen gibi kaynaklar olarak kabul edilmesi yazı tiplerini sağlar ya da drawables. [Android destek kitaplığı 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) backport API düzeyi 14 veya daha yüksek hedef uygulamalarla yeni yazı tipi API'nin olur.

Atamak API 26 veya Android destek kitaplığı v26 yükledikten sonra bir Android uygulaması yazı tiplerini kullanmanın iki yolu vardır:

1. **Android bir kaynak olarak yazı tipi paketini** &ndash; bu yazı tipini her zaman uygulama için kullanılabilir, ancak APK boyutunu artıracaktır sağlar. 
2. **Yazı tipleri karşıdan** &ndash; Android de destekler yazı tipinden indirme bir _yazı tipi sağlayıcısı_. Yazı tipi sağlayıcısı yazı cihazda zaten olup olmadığını denetler. Gerekirse, yazı tipi indirilir ve cihazda önbelleğe. Bu yazı tipini birden çok uygulama arasında paylaşılabilir.

Benzer yazı tiplerini (veya birkaç farklı stillerde olabilir bir yazı tipi) gruplandırılmış içine _yazı tipi aileleri_. Bu yazı tipini, ağırlık gibi belirli özniteliklerini belirtmek geliştiricilere sağlar ve Android yazı tipi ailesi uygun yazı tipini otomatik olarak seçer.

Android desteği kitaplığı v26 yazı tipi API düzeyine 26 backport desteği olur. Eski API düzeylerini hedeflerken bildirmek ise gerekli `app` XML ad alanı ve kullanarak çeşitli yazı tipi özniteliklerini adlandırmak için `android:` ad alanı ve `app:` ad alanı. Yalnızca `android:` ad alanı kullanılır ve yazı tipleri 25 veya daha az API düzeyi çalıştıran görüntülenen cihazlar olmaz. Örneğin, bu XML parçacığını yeni bildirir [ _yazı tipi ailesi_ ](#font_families) API Düzey 14 ve daha yüksek çalışır kaynak:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular" 
            android:fontStyle="normal" 
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular" 
            app:fontStyle="normal" 
            app:fontWeight="400" />

</font-family>
```

Yazı tipleri düzgün bir şekilde bir Android uygulamasına sağlanan sürece, bunlar bir UI pencere öğesi ayarlayarak uygulanabilir [ `fontFamily` özniteliği](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). Örneğin, aşağıdaki kod parçacığını bir yazı tipi kutusu TextView içinde görüntülemek nasıl gösterir:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Bu kılavuzda ilk yazı tiplerini Android bir kaynak olarak kullanmak üzere nasıl ele almaktadır ve çalışma zamanında yazı tiplerini yüklemek nasıl tartışmak taşıyın.

## <a name="fonts-as-a-resource"></a>Bir kaynak olarak yazı tipleri

Bir Android APK bir yazı tipi paketleme, her zaman kullanılabilir olmasını sağlar. Yazı tipi dosyası (ya da bir. Fot adı veya bir. OTF dosyası) bir alt dizinindeki dosyaları kopyalayarak bir Xamarin.Android uygulaması gibi diğer herhangi bir kaynağa eklenir **kaynakları** bir Xamarin.Android projesi klasörü. Yazı tipleri kaynakları saklanır bir **yazı tipi** , alt dizin **kaynakları** projenin klasör. 

> [!NOTE]
> Yazı tipleri olmalıdır bir **yapı eylemi** , **AndroidResource** veya bunların son APK da paketlenmiş değil. Yapı eyleminin IDE tarafından otomatik olarak ayarlanması gerekir.

Pek çok benzer yazı tipi dosyaları (örneğin, aynı yazı tipiyle farklı ağırlıkları veya stiller) olduğunda bir yazı tipi ailesi gruplandırmak mümkündür.

<a name="font_families" />

### <a name="font-families"></a>Yazı tipi aileleri

Yazı tipi ailesi, farklı ağırlıkları ve stiller sahip yazı tiplerini kümesidir. Örneğin, kalın veya Yatık yazı tipi için ayrı yazı tipi dosyaları olabilir. Yazı tipi ailesi tarafından tanımlanan `font` tutulan bir XML dosyası öğelerinde **kaynakları/yazı tipi** dizin. Her yazı tipi ailesi kendi XML dosyası olmalıdır.

Oluşturmak için bir yazı tipi ailesi ilk için tüm yazı tipi eklemek **kaynakları/yazı tipi** klasör. Ardından yazı tipi ailesi için yazı tipi klasöründe yeni bir XML dosyası oluşturun. XML dosyasının adı, başvurulan yazı tipleri herhangi bir benzeşim veya ilişki sahiptir; Kaynak dosyanın herhangi bir yasal Android kaynak dosya adı olabilir. Bu XML dosyasını bir kök sahip `font-family` içeren bir veya daha fazla öğe `font` öğeleri. Her `font` öğesi bir yazı tipi özniteliklerini bildirir.

Aşağıdaki XML bir yazı tipi ailesi örneğidir _kaynakları Sans Pro_ birçok farklı yazı tipi ağırlıkları tanımlar yazı tipi. Bu dosya olarak kaydedilir **kaynakları/yazı tipi** adlı klasörü **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular" 
          android:fontStyle="normal" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_regular" 
          app:fontStyle="normal" 
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold" 
          android:fontStyle="normal" 
          android:fontWeight="800" 
          app:font="@font/sourcesanspro_bold" 
          app:fontStyle="normal" 
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic" 
          android:fontStyle="italic" 
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic" 
          app:fontStyle="italic" 
          app:fontWeight="400" />
</font-family>
```

`fontStyle` Özniteliğine sahip iki olası değerler:

* **Normal** &ndash; normal bir yazı tipi
* **İtalik** &ndash; Yatık yazı tipi

`fontWeight` Özniteliği karşılık gelen CSS'ye `font-weight` özniteliği ve yazı tipinin kalınlığını belirtir. Bu, 100-900 aralığındaki bir değerdir. Aşağıdaki liste, ortak yazı tipi ağırlık değerleri ve kullanıcıların adı açıklanmaktadır:

* **İnce** &ndash; 100
* **Çok hafif** &ndash; 200
* **Açık** &ndash; 300
* **Normal** &ndash; 400
* **Orta** &ndash; 500
* **Yarı kalın** &ndash; 600
* **Kalın** &ndash; 700
* **Extra Kalın** &ndash; 800
* **Siyah** &ndash; 900

Yazı tipi ailesi tanımlandıktan sonra bu bildirimli olarak ayarlayarak kullanılabilir `fontFamily`, `textStyle`, ve `fontWeight` düzeni dosyanın öznitelikleri.  Örneğin aşağıdaki XML parçacığını 400 ağırlık yazı tipi (normal) ve bir italik metin stilini ayarlar:

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

### <a name="programmatically-assigning-fonts"></a>Program aracılığıyla yazı tiplerini atama

Yazı tipleri program aracılığıyla ayarlanabilir kullanarak [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) alma yöntemi bir [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) nesnesi. Çok sayıda görünümleri olan bir `TypeFace` pencere öğesi için yazı tipi atamak için kullanılan özellik. Bu kod parçacığını programlı olarak yazı tipi kutusu TextView üzerinde nasıl ayarlanacağı gösterir:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont` Yöntemi otomatik olarak yükler bir yazı tipi ailesi içinde ilk yazı tipi.  Belirli bir stil eşleşen bir yazı tipi yüklemek için kullanmak `Typeface.Create` yöntemi. Bu yöntem, belirtilen stili eşleşen bir yazı tipi yüklemek çalışacaktır. Örnek olarak, bu kod parçacığında kalın yüklenmeye çalışılacak `Typeface` tanımlanan bir yazı tipi ailesi nesnesinden **kaynakları/yazı tipleri**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>Yazı tipleri indirme

Uygulama kaynağı olarak paketleme yazı tipleri yerine, Android, bir uzak kaynaktan yazı tipleri yükleyebilirsiniz. Bu APK boyutunu azaltma arzu etkisi olmaz. 

Yazı tipleri Yardımı ile yüklenen bir _yazı tipi sağlayıcısı_. Karşıdan yükleme ve yazı tipleri cihazda tüm uygulamalar için önbelleğe almayı yönetir özel bir içerik sağlayıcı budur. Android 8.0 içeren gelen yazı tiplerini yüklemek için bir yazı tipi sağlayıcısı [Google yazı tipi depo](http://fonts.google.com). Bu varsayılan yazı tipi Itanium tabanlı sistemler için API Düzey 14 Android destek kitaplığı v26 ile backported sağlayıcısıdır.

Bir uygulama için bir yazı tipi istekte bulunduğunda, yazı tipi sağlayıcısı ilk yazı tipi zaten aygıtta olup olmadığını kontrol eder. Aksi durumda, ardından yazı tipi indirmeyi dener. İndirilen, ardından Android yazı olamaz, varsayılan sistem yazı tipini kullanır. Yazı tipi yüklendikten sonra yalnızca ilk istekte uygulama cihazda tüm uygulamalar için kullanılabilir.

Bir yazı tipi yüklemek için bir istek yapıldığında, uygulama yazı tipi sağlayıcısı doğrudan sorgulamaz. Bunun yerine, uygulamalar bir örneğini kullanacak [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (veya [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) destek kitaplığı 26 kullanılıyorsa).  

Android 8.0 iki farklı şekilde indirme yazı tipini destekler:

1. **Bir kaynak olarak indirilebilir yazı tipleri bildirme** &ndash; uygulama Android indirilebilir yazı tiplerini XML kaynak dosyaları aracılığıyla bildirebilir. Bu dosyalar tüm Android uygulama başlar ve cihazda önbellek zaman uyumsuz olarak yazı tiplerini yüklemek için gereken meta verileri içerir.
2. **Program aracılığıyla** &ndash; Android API düzeyinde 26 API'ler Uygulama çalışırken yazı tiplerini programlı olarak indirmek uygulama izin verir. Uygulamaları oluşturacak bir `FontRequest` nesne için belirli bir yazı tipi ve bu nesneye geçirin `FontsContract` sınıfı. `FontsContract` Geçen `FontRequest` ve yazı tipinden alır bir _yazı tipi sağlayıcısı_. Android yazı zaman uyumlu olarak indirir. Örnek oluşturma bir `FontRequest` bu kılavuzda gösterilir.

Hangi yaklaşımın kullanıldığında bağımsız olarak, yazı tipleri önce Xamarin.Android uygulamasına eklenmelidir kaynaklar dosyaları indirilebilir. İlk olarak, kullandığınız bir XML dosyasında bildirilmelidir **kaynakları/yazı tipi** dizini bir yazıtipi ailesinin bir parçası olarak. Bu kod parçacığında yazı indirmeyi örneğidir [Google yazı tipleri açık kaynak koleksiyonu](https://fonts.google.com) Android 8.0 (veya destek kitaplığı v26) ile birlikte gelen varsayılan yazı tipi sağlayıcısını kullanarak:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts" 
             android:fontProviderPackage="com.google.android.gms" 
             android:fontProviderQuery="VT323" 
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts" 
             app:fontProviderPackage="com.google.android.gms" 
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
>
</font-family>
```

`font-family` Öğesi Android yazı tiplerini yüklemek için gerektirdiği bilgiler bildirme aşağıdaki öznitelikleri içerir:

1. **fontProviderAuthority** &ndash; istek için kullanılacak yazı tipi sağlayıcısının yetkilisi.
2. **fontPackage** &ndash; istek için kullanılacak yazı tipi sağlayıcısı için paketi. Bu, sağlayıcısının kimliğini doğrulamak için kullanılır.
3. **fontQuery** &ndash; bu istenen yazı tipini bulun yazı tipi sağlayıcısı yardımcı olacak bir dizedir. Yazı tipi sağlayıcıya özel yazı tipi sorgu hakkında ayrıntılar. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Sınıfını [indirilebilir yazı tipleri](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) örnek uygulaması sağlar bazı bilgiler sorgu biçimi için yazı tipi Google yazı tipleri açık kaynak koleksiyondan.
4. **fontProviderCerts** &ndash; sağlayıcı ile imzalanması gerektiğini sertifikalar için karma kümesi listesiyle kaynak dizi.

Yazı tipleri tanımlandıktan sonra ilgili bilgi sağlamak gerekli olabilir _yazı tipi sertifikaları_ yüklemeyle ilgili.

### <a name="font-certificates"></a>Yazı tipi sertifikaları

Yazı tipi sağlayıcısı cihazda önceden yüklenmiş olarak bulunmuyor veya uygulama kullanıyorsanız `Xamarin.Android.Support.Compat` kitaplığı, Android yazı tipi sağlayıcısı'nın güvenlik sertifikaları gerektirir. Bu sertifikalar tutulan bir dizi kaynak dosyasında listelenen **kaynakları/değerleri** dizin. 

Örneğin, aşağıdaki XML adlı **Resources/values/fonts_cert.xml** ve sertifikalar için Google yazı tipi sağlayıcısı depolar: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

Yerinde bu kaynak dosyaları ile uygulama yazı tiplerini indirme yeteneğine sahiptir.

### <a name="declaring-downloadable-fonts-as-resources"></a>Kaynaklar olarak indirilebilir yazı tipleri bildirme

İndirilebilir yazı tiplerini listeleme tarafından **AndroidManifest.XML**, Android zaman uyumsuz olarak yükleyecek yazı tiplerini uygulama ilk kez başlatıldığında. Yazı tipini kendileri kullanıcının buna benzer bir dizi kaynak dosyasını, listelenir: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

Bu yazı tiplerini yüklemek için de bildirilmesi sahip oldukları **AndroidManifest.XML** ekleyerek `meta-data` bir alt öğesi olarak `application` öğesi. Örneğin, bir kaynak dosyasında bildirilen indirilebilir yazı tipleri **Resources/values/downloadable_fonts.xml**, bu kod parçacığında bildirime eklenecek olacaktır: 

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>Yazı tipi yazı tipi API'leri ile indirme

Program aracılığıyla oluşturarak bir yazı tipi yüklemek olası bir [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) nesne ve olarak geçirme `FontContractCompat.RequestFont` yöntemi. `FontContractCompat.RequestFont` Yöntemi ilk yazı tipi cihazda mevcut olup olmadığını denetleyin ve ardından gerekirse zaman uyumsuz olarak sorgu yazı tipini uygulama yüklemek için deneyin ve yazı tipi sağlayıcısı olur. Varsa `FontRequest` Android varsayılan sistem yazı tipi kullanır yazı tipi yüklemek alamıyor. 

A `FontRequest` nesnesi yazı tipi sağlayıcı tarafından bulun ve bir yazı tipi yüklemek için kullanılan bilgileri içerir. A `FontRequest` bilgi dört adet gerektirir:

1. **Yazı tipi sağlayıcısı yetkilisi** &ndash; istek için kullanılacak yazı tipi sağlayıcısının yetkilisi.
2. **Yazı tipi paket** &ndash; istek için kullanılacak yazı tipi sağlayıcısı için paketi. Bu, sağlayıcısının kimliğini doğrulamak için kullanılır.
3. **Yazı tipi sorgu** &ndash; bu istenen yazı tipini bulun yazı tipi sağlayıcısı yardımcı olacak bir dizedir. Yazı tipi sağlayıcıya özel yazı tipi sorgu hakkında ayrıntılar. Dize ayrıntılarını yazı tipi sağlayıcıya özgü. [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) Sınıfını [indirilebilir yazı tipleri](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) örnek uygulaması sağlar bazı bilgiler sorgu biçimi için yazı tipi Google yazı tipleri açık kaynak koleksiyondan.
4. **Yazı tipi sağlayıcısı sertifikaları** &ndash; sağlayıcı ile imzalanmasını sertifikalar için karma kümesi listesiyle kaynak dizi. 

Bu kod parçacığında yeni bir örnek oluşturma örneği olan `FontRequest` nesnesi:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

Önceki kod parçacığını içinde `FontToDownload` yazı tipi Google yazı tipleri açık kaynak koleksiyondan yardımcı olacak bir sorgu. 

Geçirilmeden önce `FontRequest` için `FontContractCompat.RequestFont` yöntemi, oluşturulmalıdır iki nesne vardır:

* **`FontsContractCompat.FontRequestCallback`** &ndash; Genişletilmelidir soyut bir sınıf budur. Olacak bir geri çağırma olduğu zaman çağrılan `RequestFont` tamamlandı. Bir Xamarin.Android uygulaması bir alt kümesi olmalıdır `FontsContractCompat.FontRequestCallback` ve geçersiz kılma `OnTypefaceRequestFailed` ve `OnTypefaceRetrieved`, yükleme başarısız olursa veya sırasıyla başarılı olduğunda gerçekleştirilecek eylemleri sağlama.
* **`Handler`** &ndash; Bu bir `Handler` tarafından doğrulayacak `RequestFont` gerekiyorsa, bir iş parçacığı üzerinde yazı tipi yüklemek için. Yazı tipleri gereken **değil** UI iş parçacığında indirilebilir.

Bu kod parçacığında, bir yazı tipi Google yazı tipleri açık kaynak koleksiyonundan zaman uyumsuz olarak yükleyecek bir C# sınıfı örneğidir. Bunu uygulayan `FontRequestCallback` arabirim ve C# olayını başlatır, `FontRequest` bitirdi. 

```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";

    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };


    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }

    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```

Bu yardımcı kullanmak için yeni bir `FontDownloadHelper` oluşturulan ve bir olay işleyicisi atanır:  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) => 
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>Özet

Bu kılavuzun indirilebilir yazı tipleri ve yazı tipleri kaynaklar olarak desteklemek için Android 8.0 yeni API'lerinde açıklanmıştır. Nasıl bir APK varolan yazı tipleri katıştırmak için ve bir düzende kullanılacağını açıklanmıştır. Program aracılığıyla veya yazı tipi meta veriler kaynak dosyalarında bildirme Android 8.0 bir yazı tipi sağlayıcısından indirme yazı tipleri nasıl desteklediği açıklanmıştır.

## <a name="related-links"></a>İlgili bağlantılar

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Yazı tipi](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android desteği kitaplığı 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Android yazı tiplerini kullanma](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS font ağırlık belirtimi](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google yazı tipleri açık kaynak koleksiyonu](https://fonts.google.com/)
- [Pro Sans kaynak](https://fonts.google.com/specimen/Source+Sans+Pro)
