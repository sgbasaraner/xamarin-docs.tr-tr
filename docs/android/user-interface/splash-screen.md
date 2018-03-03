---
title: "Giriş ekranı"
description: "Android uygulaması özellikle zaman uygulama önce bir aygıtta başlatıldıktan yukarı başlatmak için biraz zaman alır. Karşılama ekranı başlangıç görüntülenebilir yukarı ilerleme kullanıcıya veya markalama belirtmek için."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 9acb1ad6ab1425edb98b938e8c03edc3704f50ae
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="splash-screen"></a>Giriş ekranı

_Android uygulaması özellikle zaman uygulama önce bir aygıtta başlatıldıktan yukarı başlatmak için biraz zaman alır. Karşılama ekranı başlangıç görüntülenebilir yukarı ilerleme kullanıcıya veya markalama belirtmek için._

<a name="overview" />

## <a name="overview"></a>Genel Bakış

Android uygulaması için başlatılması biraz zaman alır, özellikle ilk kez sırasında uygulama bir cihazda çalıştırma (bazen bu olarak adlandırılır bir _cold start_). Giriş ekranı görüntülenebilir kullanıcıya ilerleme başlatın ya da tanımlamak ve uygulama yükseltmek için markalama bilgileri görüntüleyebilir.

Bu kılavuz bir Android uygulamasını Karşılama ekranı uygulamak için bir yöntem açıklanır. Aşağıdaki adımları kapsar:

1.  Drawable kaynak giriş ekranı için oluşturuluyor.

2.  Drawable kaynak görüntüler yeni bir tema tanımlama.

3.  Önceki adımda oluşturduğunuz tema tarafından tanımlanan giriş ekranı olarak kullanılacak uygulama için yeni bir etkinlik ekleniyor.

[![Uygulama ekran tarafından izlenen örnek Xamarin logosu giriş ekranı](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png)


<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Bu kılavuz uygulamasını Android API düzeyi 15 (Android 4.0.3) hedefleyen varsayar ya da daha yüksek. Uygulama aynı zamanda olmalıdır **Xamarin.Android.Support.v4** ve **Xamarin.Android.Support.v7.AppCompat** NuGet paketlerini projeye eklenir.

Tüm kod ve bu kılavuzdaki XML bulunabilir [KarşılamaEkranı](https://developer.xamarin.com/samples/monodroid/SplashScreen) bu kılavuz için örnek proje.

<a name="implement" />

## <a name="implementing-a-splash-screen"></a>Bir giriş ekranının uygulama

İşlemek ve giriş ekranı görüntülemek için en hızlı özel bir tema oluşturun ve giriş ekranı sergiler aktivite uygulamak için yoludur. Etkinlik işlendiğinde temayı yükler ve etkinlik arka plan (Tema tarafından başvurulan) drawable kaynak uygular. Bu yaklaşım düzeni dosyasını oluşturmak için gereken önler.

Giriş ekranı markalı görüntüleyen bir etkinlik olarak uygulanan drawable, tüm başlatmaları gerçekleştirir ve tüm görevleri başlatır. Uygulama ten sonra giriş ekranı etkinlik ana etkinlik başlatır ve kendisini uygulama arka yığından kaldırır.

<a name="drawable" />

### <a name="creating-a-drawable-for-the-splash-screen"></a>Bir Drawable için giriş ekranı oluşturma

Giriş ekranı etkinlik giriş ekranı arka planda drawable XML görüntüler. Görüntü için (JPG veya PNG gibi) bir eşlemli görüntü görüntülemek üzere kullanmak gereklidir.

Bu kılavuzda, kullandığımız bir [katman listesi](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) uygulamadaki giriş ekranı görüntüsü ortalamak için. Aşağıdaki kod parçacığında örneğidir bir `drawable` kaynağı kullanan bir `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

Bu `layer-list` giriş ekranı görüntüsü Merkezi **splash.png** tarafından belirtilen bir arka plan üzerinde `@color/splash_background` kaynak.
Bu dosyaya koyun **kaynakları/drawable** klasörünü (örneğin, **Resources/drawable/splash_screen.xml**).

Drawable giriş ekranı oluşturulduktan sonra sonraki adıma giriş ekranı için bir tema oluşturmaktır.

<a name="theme" />

### <a name="implementing-a-theme"></a>Bir tema uygulama

Giriş ekranı etkinlik için özel bir tema oluşturmak için Düzenle (veya ekleyin) dosyası **values/styles.xml** ve yeni bir `style` giriş ekranı öğesi. Bir örnek **values/style.xml** dosya aşağıda gösterilmektedir ile bir `style` adlı **MyTheme.Splash**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

**MyTheme.Splash** çok spartan olan &ndash; penceresi arka plan bildirir, açıkça başlık çubuğunda penceresinden kaldırır ve tam ekran olduğunu bildirir. Kullanıcı Arabirimi, uygulamanızın ilk düzeni etkinlik Şişir önce öykünen bir giriş ekranı oluşturmak istiyorsanız, kullanabileceğiniz `windowContentOverlay` yerine `windowBackground` stili tanımında. Bu durumda, aynı zamanda değiştirmeniz gerektiğini **splash_screen.xml** drawable böylece UI bir öykünmesini görüntüler.

<a name="activity" />

### <a name="create-a-splash-activity"></a>Bir giriş etkinlik oluşturmak

Şimdi bizim giriş görüntüsü vardır ve herhangi bir başlangıç görevi gerçekleştiren başlatmak Android için yeni bir etkinlik gerekir. Aşağıdaki kod, bir tam giriş ekranı uygulama örneğidir:

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` açıkça varsayılan tema uygulamanın geçersiz kılma önceki bölümde oluşturulan tema kullanır.
Bir düzende yüklemeye gerek yoktur `OnCreate` gibi tema drawable arka planı olarak bildirir.

Ayarlamak önemlidir `NoHistory=true` etkinlik geri yığından kaldırılır, böylece özniteliği. Başlatma işlemi iptal ediliyor geri düğmesi engellemek için de geçersiz kılabilirsiniz `OnBackPressed` ve hiçbir şey yapma:

```csharp
public override void OnBackPressed() { }
```

Başlatma işi zaman uyumsuz olarak gerçekleştirilen `OnResume`. Başlangıç iş bırakmaz yavaşlaması veya başlatma ekranı görünümünü gecikme için bu gereklidir. İş tamamlandığında, `SplashActivity` başlatacak `MainActivity` ve kullanıcının uygulamayla etkileşimi başlayabilir.

Bu yeni `SplashActivity` uygulama Başlatıcısı etkinlik olarak ayarlayarak ayarlanır `MainLauncher` özniteliğini `true`. Çünkü `SplashActivity` Başlatıcısı etkinlik düzenlemeniz gerekir artık `MainActivity.cs`, kaldırıp `MainLauncher` özniteliğini `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```

<a name="summary" />

## <a name="summary"></a>Özet

Bu kılavuzda ele alınan bir Xamarin.Android uygulaması'nda bir karşılama ekranı uygulamak için bir yol; yani, özel bir tema başlatma etkinlik uygulanıyor.


## <a name="related-links"></a>İlgili bağlantılar

- [KarşılamaEkranı (örnek)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [Katman listesi Drawable](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ Malzeme tasarım desenleri - başlatma ekranlar](https://www.google.com/design/spec/patterns/launch-screens.html)
