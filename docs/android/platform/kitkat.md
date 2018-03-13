---
title: "KitKat özellikleri"
description: "Android 4.4 (KitKat) kullanıcıların ve geliştiricilerin özelliklerine cornucopia ile yüklenen gelir. Bu kılavuz, birkaç bu özelliklerin vurgular ve kod örnekleri ve en KitKat dışında yapmanıza yardımcı olmak için uygulama ayrıntıları sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: D3FDEA1C-F076-406F-BCC3-2A55D2C6ADEE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 8fbb3f73fdc09f953ad5f7134020c1555d000d28
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="kitkat-features"></a>KitKat özellikleri

_Android 4.4 (KitKat) kullanıcıların ve geliştiricilerin özelliklerine cornucopia ile yüklenen gelir. Bu kılavuz, birkaç bu özelliklerin vurgular ve kod örnekleri ve en KitKat dışında yapmanıza yardımcı olmak için uygulama ayrıntıları sağlar._

## <a name="overview"></a>Genel Bakış

Android 4.4 (API düzeyi 19) olarak da bilinen "KitKat", geç 2013'te yayımlanmıştır. KitKat yeni özellikler ve geliştirmeler dahil olmak üzere, çeşitli sunar:

-  [Kullanıcı deneyimi](#user_experience) &ndash; kolay animasyonları geçiş framework, saydam durumunu ve gezinti çubukları ve tam ekran derinlikli modu ile Yardım kullanıcı için daha iyi bir deneyim oluşturun.

-  [Kullanıcı içeriği](#user_content) &ndash; kullanıcı dosya yönetimi Basitleştirilmiş depolama erişim çerçevesiyle; yazdırma resimleri, web siteleri ve diğer içeriği geliştirilmiş yazdırma API'leri ile daha kolay.

-  [Donanım](#hardware) &ndash; NFC ana bilgisayar tabanlı kartı öykünme NFC kartla herhangi bir uygulama dönüştürmek; düşük güç algılayıcılar ile çalıştırın `SensorManager` .

-  [Geliştirici Araçları](#developer_tools) &ndash; kullanılabilir Android hata ayıklama köprüsü istemci eylemiyle ekran kaydı uygulamalarda Android SDK'ın bir parçası olarak.


Bu kılavuz, Xamarin.Android geliştiriciler için KitKat üst düzey bir genel bakış yanı sıra KitKat var olan bir Xamarin.Android uygulamayı geçirmek için yönergeler sağlar.

## <a name="requirements"></a>Gereksinimler

KitKat kullanarak Xamarin.Android uygulamaları geliştirmek için ihtiyacınız *Xamarin.Android 4.11.0* veya Android SDK Yöneticisi aracılığıyla tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi yüklü daha yüksek ve Android 4.4 (API düzeyi 19):

[![Android SDK Yöneticisi'nde Android 4.4 seçme](kitkat-images/api19.png)](kitkat-images/api19.png#lightbox)

<a name="Migrating_Your_App_to_KitKat" />

## <a name="migrating-your-app-to-kitkat"></a>Uygulamanız için KitKat geçirme

Bu bölüm Android 4.4 geçiş mevcut uygulamalarına yardımcı olması için bazı ilk yanıt öğeleri sağlar.

### <a name="check-system-version"></a>Onay sistemi sürümü

Bir uygulamanın Android eski sürümleriyle uyumlu olması gerekiyorsa, aşağıdaki kod örneği tarafından gösterildiği gibi bir sistem sürüm denetimi KitKat özgü kod sarmalamak emin olun:

```csharp
if (Build.VERSION.SdkInt >= BuildVersionCodes.Kitkat) {
    //KitKat only code here
}
```

### <a name="alarm-batching"></a>Uyarı toplu işleme

Android alarm Hizmetleri arka planda bir uygulama belirli bir zamanda uyandırmak için kullanır. KitKat bu bir adım ileri güç korumak için uyarıları toplu işleme alır. Bu, tam bir zamanda her uygulama Uyandırma yerine KitKat aynı zaman aralığı boyunca Uyandırma ve aynı anda uyandırmak için kayıtlı birkaç uygulama grubuna tercih, anlamına gelir.
Bir uygulama belirtilen zaman aralığı boyunca uyandırmak için Android bildirmek için çağrı `SetWindow` üzerinde [ `AlarmManager` ](https://developer.xamarin.com/api/type/Android.App.AlarmManager/), geçen en düşük ve uygulama woken önce geçebilecek, milisaniye cinsinden en uzun süreyi ve işlemi gerçekleştirmek için Uyandırma.
Aşağıdaki kod yarım saat penceresi ayarlanmış zamanından itibaren bir saat arasındaki woken gereken bir uygulamanın örneğidir:

```csharp
AlarmManager alarmManager = (AlarmManager)GetSystemService(AlarmService);
alarmManager.SetWindow (AlarmType.Rtc, AlarmManager.IntervalHalfHour, AlarmManager.IntervalHour, pendingIntent);
```

Tam bir anda bir uygulama Uyandırma sürdürebilmek için `SetExact`, geçen uygulama woken tam zaman ve işlemi gerçekleştirmek için:

```csharp
alarmManager.SetExact (AlarmType.Rtc, AlarmManager.IntervalDay, pendingIntent);
```

KitKat artık tam yinelenen alarm ayarlamanıza olanak tanır. Kullanan uygulamalar [ `SetRepeating` ](https://developer.xamarin.com/api/member/Android.App.AlarmManager.SetRepeating/p/Android.App.AlarmType/System.Int64/System.Int64/Android.App.PendingIntent/) ve her uyarı el ile başlatılmasına neden olacak artık gerek çalışması için tam alarmlar gerektirir.

### <a name="external-storage"></a>Harici depolama

Harici depolama, artık iki tür - benzersiz uygulama ve verileri birden çok uygulama tarafından paylaşılan depolama alanı ayrılmıştır. Okuma ve dış depolama alanında, uygulamanızın belirli bir konuma yazma özel izinler gerektirir. Paylaşılan depolama verilerle şimdi etkileşim gerektiriyor `READ_EXTERNAL_STORAGE` veya `WRITE_EXTERNAL_STORAGE` izni. İki tür şekilde sınıflandırılabilir:

-  Üzerinde bir yöntemini çağırarak bir dosya veya dizin yolu almanızı varsa `Context` - Örneğin, [ `GetExternalFilesDir` ](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalFilesDir/p/System.String/) veya [`GetExternalCacheDirs`](https://developer.xamarin.com/api/member/Android.Content.Context.GetExternalCacheDirs%28%29/)
   - Uygulamanızı hiçbir ek izinler gerekir.

-  Bir özellik erişimi veya üzerinde bir yöntemi çağırmak tarafından bir dosya veya dizin yolu alıyorsanız `Environment` , gibi [ `GetExternalStorageDirectory` ](https://developer.xamarin.com/api/property/Android.OS.Environment.ExternalStorageDirectory/) veya [ `GetExternalStoragePublicDirectory` ](https://developer.xamarin.com/api/member/Android.OS.Environment.GetExternalStoragePublicDirectory/p/System.String/) , uygulamanızı gerektirir `READ_EXTERNAL_STORAGE` veya `WRITE_EXTERNAL_STORAGE` izni.

> [!NOTE]
> `WRITE_EXTERNAL_STORAGE` gelir `READ_EXTERNAL_STORAGE` her zaman sadece bir izin kümesi gerekir böylece izni.

### <a name="sms-consolidation"></a>SMS birleştirme

Kullanıcı için bir varsayılan uygulama kullanıcı tarafından seçilen tüm SMS içeriğini toplayarak Mesajlaşma KitKat basitleştirir. Geliştirici, uygulama için varsayılan ileti uygulaması olarak seçilebilir duruma ve uygulama seçilmezse kod ve yaşam uygun şekilde davranmakta sorumludur. SMS uygulamanıza KitKat geçiş ile ilgili daha fazla bilgi için başvurmak [alma bilgisayarınızı SMS uygulamalar için hazır KitKat](http://android-developers.blogspot.com/2013/10/getting-your-sms-apps-ready-for-kitkat.html) Google Kılavuzu.

### <a name="webview-apps"></a>Web görünümü uygulamalar

[Web görünümü](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) KitKat içinde bir yeniliği aldı. Büyük değişiklik ek güvenlik içeriği olarak yüklenmesi için bir `WebView`. Eski API sürümlerini hedefleyen uygulamaların çoğu beklendiği gibi çalışıyor olsa da, kullanan uygulamaları sınama `WebView` sınıfı kesinlikle önerilir. Etkilenen WebView API'ler hakkında daha fazla bilgi için Android başvuran [Android 4.4 WebView geçiş](http://developer.android.com/guide/webapps/migrating.html) belgeleri.

<a name="user_experience" />

## <a name="user-experience"></a>Kullanıcı Deneyimi

KitKat özellik animasyonları ve tema için saydam bir kullanıcı Arabirimi seçeneği işlemek için yeni geçiş framework dahil olmak üzere kullanıcı deneyimini geliştirmek için birkaç yeni API ile birlikte gelir. Bu değişiklikler, aşağıda ele alınmıştır.

### <a name="transition-framework"></a>Geçiş çerçevesi

Geçiş framework animasyonları uygulamak kolaylaştırır. KitKat sağlayan bir basit özellik animasyon yalnızca bir kod satırı ile gerçekleştirmek ya da kullanarak geçişleri özelleştirme *planda*.

#### <a name="simple-property-animation"></a>Basit özellik animasyon

Yeni Android geçişleri kitaplığı özellik animasyonları arkasındaki kodda basitleştirir. Framework en az kod ile basit animasyonları gerçekleştirmenizi sağlar. Örneğin, aşağıdaki örnek kullandığı kod [ `TransitionManager.BeginDelayedTransition` ](https://developer.xamarin.com/api/member/Android.Transitions.TransitionManager.BeginDelayedTransition/p/Android.Views.ViewGroup/Android.Transitions.Transition/) gösterme ve gizleme animasyon uygulamak için bir `TextView`:

```csharp
using Android.Transitions;

public class MainActivity : Activity
{
    LinearLayout linear;
    Button button;
    TextView text;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        linear = FindViewById<LinearLayout> (Resource.Id.linearLayout);
        button = FindViewById<Button> (Resource.Id.button);
        text = FindViewById<TextView> (Resource.Id.textView);

        button.Click += (o, e) => {

            TransitionManager.BeginDelayedTransition (linear);

            if(text.Visibility != ViewStates.Visible)
            {
                text.Visibility = ViewStates.Visible;
            }
            else
            {
                text.Visibility = ViewStates.Invisible;
            }
        };
    }
}
```

Yukarıdaki örnekte, bir otomatik, değişen özellik değerleri arasında Varsayılan geçiş oluşturmak için geçiş çerçevesi kullanır. Animasyonun tek satırlık bir kod tarafından işlendiğinden, kolayca bu kaydırma tarafından Android eski sürümleriyle uyumlu yapabileceğiniz `BeginDelayedTransition` sistem sürüm onay işareti çağırın. Bkz: [geçirme Your App için KitKat](#Migrating_Your_App_to_KitKat) daha fazla bilgi için bölüm.

Aşağıdaki ekran görüntüsünde animasyonun önce uygulama gösterir:

[![Animasyon başlatılmadan önce App ekran görüntüsü](kitkat-images/trans-before.png)](kitkat-images/trans-before.png#lightbox)

Aşağıdaki ekran görüntüsünde uygulama sonra animasyon gösterir:

[![Animasyon tamamlandıktan sonra uygulama ekran görüntüsü](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Sonraki bölümde yer alan planda geçişle üzerinde daha fazla denetim elde edebilirsiniz.

#### <a name="android-scenes"></a>Android planda

[Planda](https://developer.xamarin.com/api/type/Android.Transitions.Scene/) Geliştirici animasyonları üzerinde daha fazla denetim sağlamak için geçiş framework'ün bir parçası olarak tanıtılan. Planda oluşturma bir dinamik alanı kullanıcı Arabiriminde: bir kapsayıcı ve birkaç sürümleri veya "planda" için kapsayıcı XML içeriğini belirtin ve Android planda arasındaki geçişler animasyon çalışmaya geri kalanı yapar. Android planda minimum çabayla karmaşık animasyonları geliştirme tarafında oluşturmanızı sağlar.

Dinamik içerik barındıran statik UI bir adlandırılan öğedir bir *kapsayıcı* veya *Sahne temel*. Aşağıdaki örnek oluşturmak için Android Tasarımcısı'nı kullanan bir `RelativeLayout` adlı `container`:

[![RelativeLayout kapsayıcısı oluşturmak için Android Tasarımcısı'nı kullanarak](kitkat-images/container.png)](kitkat-images/container.png#lightbox)

Örnek düzeni adlı bir düğme ayrıca tanımlar `sceneButton` aşağıda `container`. Bu düğme, geçiş tetikler.

Dinamik içerik kapsayıcı içindeki iki yeni Android düzenleri gerektirir. Bu düzenleri yalnızca kodu belirtin *içinde* kapsayıcı.
Aşağıdaki örnek kod bir düzen olarak adlandırılan tanımlar *Scene1* iki metin alanları "Seti" okuma ve "Kat" sırasıyla içeren ve ikinci bir düzen adlı *Scene2* tersine aynı metin alanları içerir. XML aşağıdaki gibidir:

 **Scene1.AXML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kit"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textA"
        android:text="Kat"
        android:textSize="35sp" />
</merge>
```

 **Scene2.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <TextView
        android:id="@+id/textB"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Kat"
        android:textSize="35sp" />
    <TextView
        android:id="@+id/textA"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/textB"
        android:text="Kit"
        android:textSize="35sp" />
</merge>
```

Kullandığı Yukarıdaki örnek `merge` kısa Görünümü Kodu'nu ve görünüm hiyerarşisi basitleştirmek için. Daha fazla bilgi edinebilirsiniz `merge` düzenleri [burada](http://android-developers.blogspot.com/2009/03/android-layout-tricks-3-optimize-by.html).

Bir Sahne çağrılarak oluşturulan [ `Scene.GetSceneForLayout` ](https://developer.xamarin.com/api/member/Android.Transitions.Scene.GetSceneForLayout/p/Android.Views.ViewGroup/System.Int32/Android.Content.Context/), kapsayıcı nesnesinde Sahne'nın düzeni dosyasını ve geçerli kaynak Kimliğini geçen `Context`kod örneğinde gösterildiği gibi:

```csharp
RelativeLayout container = FindViewById<RelativeLayout> (Resource.Id.container);

Scene scene1 = Scene.GetSceneForLayout(container, Resource.Layout.Scene1, this);
Scene scene2 = Scene.GetSceneForLayout(container, Resource.Layout.Scene2, this);

scene1.Enter();
```

Varsayılan geçiş değerlerle Android canlandırır iki planda arasında düğmesini tıklatarak çevirir:

```csharp
sceneButton.Click += (o, e) => {
    Scene temp = scene2;
    scene2 = scene1;
    scene1 = temp;

    TransitionManager.Go (scene1);
};
```

Aşağıdaki ekran görüntüsünde işleminden önce animasyonun gösterilmektedir:

[![Animasyon başlamadan önce uygulamasının ekran görüntüsü](kitkat-images/trans-after.png)](kitkat-images/trans-after.png#lightbox)

Aşağıdaki ekran görüntüsünde Sahne sonra animasyon gösterilmektedir:

[![Animasyon tamamlandıktan sonra uygulamasının ekran görüntüsü](kitkat-images/scene.png)](kitkat-images/scene.png#lightbox)


> [!NOTE]
> Var olan bir [bilinen hata](https://code.google.com/p/android/issues/detail?id=62450) Android geçişleri planda neden kitaplığı kullanılarak oluşturulan `GetSceneForLayout` bir kullanıcı bir etkinlik ikinci kez gittiğinde ayırmak için. Java geçici bir çözüm açıklanan [burada](http://www.doubleencore.com/2013/11/new-transitions-framework/).


##### <a name="custom-transitions-in-scenes"></a>Görünümlerde özel geçişler

Özel bir geçiş bir xml kaynak dosyasında tanımlanan `transition` altında dizin `Resources`aşağıdaki ekran görüntüsüne gösterildiği gibi:

[![Kaynakları/geçiş dizini altındaki transition.xml dosyasının konumu](kitkat-images/resources.png)](kitkat-images/resources.png#lightbox)

Aşağıdaki kod örneği için 5 saniye canlandırır ve kullandığı bir geçiş tanımlar [bir ara overshoot](http://developer.android.com/reference/android/views/animation/OvershootInterpolator.html):

```xml
<changeBounds
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:duration="5000"
  android:interpolator="@android:anim/overshoot_interpolator" />
```

Geçiş etkinliğin oluşturulan kullanarak [TransitionInflater](https://developer.xamarin.com/api/type/Android.Transitions.TransitionInflater/)kod tarafından gösterildiği gibi:

```csharp
Transition transition = TransitionInflater.From(this).InflateTransition(Resource.Transition.transition);
```

Yeni geçiş daha sonra eklenen `Go` animasyon başlar araması:

```csharp
TransitionManager.Go (scene1, transition);
```

### <a name="translucent-ui"></a>Yarı saydam kullanıcı Arabirimi

İsteğe bağlı transclucent durumunu ve gezinti çubukları ile uygulamanızı daha tema üzerinde denetim KitKat sağlar. Sistem kullanıcı Arabirimi öğeleri Android tema tanımlamak için kullandığınız aynı XML dosyasında translucency değiştirebilirsiniz. KitKat aşağıdaki özellikleri sunmaktadır:

-  `windowTranslucentStatus` -True yapar üst durum çubuğu saydam olarak ayarlandığında.

-  `windowTranslucentNavigation` -Zaman true için alt gezinti çubuğu saydam Ayarla kolaylaştırır.

-  `fitsSystemWindows` -Transcluent için üst veya alt çubuğu ayarı saydam kullanıcı Arabirimi öğeleri altındaki içeriği varsayılan olarak geçer. Bu özelliği ayarlamak `true` saydam sistem kullanıcı Arabirimi öğeleri ile çakışan içeriği önlemek için kolay bir yoludur.


Aşağıdaki kod bir tema saydam durumunu ve gezinti çubukları tanımlar:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <style name="KitKatTheme" parent="android:Theme.Holo.Light">
        <item name="android:windowBackground">@color/xamgray</item>
        <item name="android:windowTranslucentStatus">true</item>
        <item name="android:windowTranslucentNavigation">true</item>
        <item name="android:fitsSystemWindows">true</item>
        <item name="android:actionBarStyle">@style/ActionBar.Solid.KitKat</item>
    </style>

    <style name="ActionBar.Solid.KitKat" parent="@android:style/Widget.Holo.Light.ActionBar.Solid">
        <item name="android:background">@color/xampurple</item>
    </style>
</resources>
```

Aşağıdaki ekran görüntüsünde, yukarıda tema saydam durumu ve gezinti çubukları gösterir:

[![Örnek uygulamasının ekran görüntüsü saydam durumunu ve gezinti çubukları](kitkat-images/theme.png)](kitkat-images/theme.png#lightbox)

<a name="user_content" />

## <a name="user-content"></a>Kullanıcı içeriği

### <a name="storage-access-framework"></a>Depolama erişim Framework

Depolama erişim Framework (SAF), resim, video ve belgeler gibi depolanmış içeriği ile etkileşim kullanıcılar için yeni bir yoludur. Kullanıcıların içeriği işlemek için bir uygulama seçmek için bir iletişim kutusu ile sunmak yerine KitKat tek bir birleşik konumda verilerine erişmesine olanak tanır yeni bir kullanıcı arabirimini açar. İçeriği seçilen sonra kullanıcının içerik istenen uygulamaya döndürür ve uygulama deneyimi normal olarak devam eder.

Bu değişiklik Geliştirici taraftaki iki eylem gerektirir: ilk olarak, içerik sağlayıcılarından gerektiren uygulamalar reqesting içerik yeni bir şekilde güncelleştirilmesi gerekiyor olabilir. Veri yazma ikinci, uygulamalar bir `ContentProvider` yeni framework kullanmak için değiştirilmesi gerekir. Her iki senaryoyu yeni bağımlı [ `DocumentsProvider` ](https://developer.xamarin.com/api/type/Android.Provider.DocumentsProvider/) API.

#### <a name="documentsprovider"></a>DocumentsProvider

KitKat, ile etkileşim içinde `ContentProviders` ile soyutlanır `DocumentsProvider` sınıfı. Üzerinden erişilebilir olduğu müddetçe SAF verileri fiziksel olarak nerede için önemli değil, yani `DocumentsProvider` API. Yerel sağlayıcıları, bulut Hizmetleri ve dış depolama cihazlarının aynı arabirim ve aynı şekilde davranılır tüm kullanım kullanıcının içeriği ile etkileşim kurmak için tek bir yerde kullanıcı ve geliştirici sağlama.

Bu bölümde, yüklemek ve içerik depolama erişim Framework ile kaydetmek alınmaktadır.

#### <a name="request-content-from-a-provider"></a>İstek içeriği bir sağlayıcıdan

SAF kullanıcı Arabirimi kullanarak içeriği seçmenizi istiyoruz biz KitKat anlayabilirsiniz `ActionOpenDocument` biz cihaza kullanılabilen tüm içerik sağlayıcılarını bağlanmak istediğiniz güveninin hedefi. Bazı belirterek, bu amaç için filtreleme ekleyebilirsiniz `CategoryOpenable`, (yani erişilebilir, kullanılabilir içerik) açılabilir yalnızca içerik başka bir deyişle, döndürülür. KitKat de sağlar ile içeriğinin filtreleme `MimeType`. Örneğin, görüntünün belirterek resim sonuçları için filtreleri aşağıdaki kodu `MimeType`:

```csharp
Intent intent = new Intent (Intent.ActionOpenDocument);
intent.AddCategory (Intent.CategoryOpenable);
intent.SetType ("image/*");
StartActivityForResult (intent, save_request_code);
```

Çağırma `StartActivityForResult` kullanıcı, görüntüyü seçmek için göz atabilirsiniz SAF UI başlatır:

[![Görüntüye browing için depolama erişim Framework kullanarak uygulama örnek ekran görüntüsü](kitkat-images/saf-ui.png)](kitkat-images/saf-ui.png#lightbox)

Kullanıcı bir görüntüsünü seçtikten sonra `OnActivityResult` döndürür `Android.Net.Uri` seçilen dosyanın. Aşağıdaki kod örneği, kullanıcının görüntü seçimi görüntüler:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == save_request_code) {
        imageView = FindViewById<ImageView> (Resource.Id.imageView);
        imageView.SetImageURI (data.Data);
    }
}
```

#### <a name="write-content-to-a-provider"></a>İçerik için bir sağlayıcı yazma

SAF Arabiriminden yükleme içeriği yanı sıra KitKat ayrıca içerik herhangi kaydetmenizi sağlayan `ContentProvider` uygulayan `DocumentProvider` API. İçerik kullandığı kaydetme bir `Intent` ile `ActionCreateDocument`:

```csharp
Intent intentCreate = new Intent (Intent.ActionCreateDocument);
intentCreate.AddCategory (Intent.CategoryOpenable);
intentCreate.SetType ("text/plain");
intentCreate.PutExtra (Intent.ExtraTitle, "NewDoc");
StartActivityForResult (intentCreate, write_request_code);
```

Yukarıdaki kod örneği, kullanıcının dosya adını değiştirin ve yeni dosya barındırmak için bir dizin seçin izin vererek SAF UI yükler:

[![Dosya adı için NewDoc yüklemeleri dizininde değiştirme kullanıcının ekran görüntüsü](kitkat-images/saf-save.png)](kitkat-images/saf-save.png#lightbox)

Kullanıcı bastığında **kaydetmek**, `OnActivityResult` iletilir `Android.Net.Uri` ile erişilen yeni oluşturulan dosya `data.Data`. URI, yeni dosyaya veri akışı için kullanılabilir:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);

    if (resultCode == Result.Ok && data != null && requestCode == write_request_code) {
        using (Stream stream = ContentResolver.OpenOutputStream(data.Data)) {
            Encoding u8 = Encoding.UTF8;
            string content = "Hello, world!";
            stream.Write (u8.GetBytes(content), 0, content.Length);
        }
    }
}
```

Unutmayın [ `ContentResolver.OpenOutputStream(Android.Net.Uri)` ](https://developer.xamarin.com/api/member/Android.Content.ContentResolver.OpenOutputStream/(Android.Net.Uri)) döndüren bir `System.IO.Stream`, akış sürecinin tamamı .NET ile yazılmış.

Yükleme hakkında daha fazla bilgi için oluşturma ve depolama erişim Framework içerikle düzenleme başvurmak [depolama erişim Framework Android belgelerine](http://developer.android.com/guide/topics/providers/document-provider.html).

### <a name="printing"></a>Yazdırma

Yazdırma içerik başlanmasıyla içinde KitKat Basitleştirilmiş [Yazdırma Hizmetleri](https://developer.xamarin.com/api/namespace/Android.PrintServices/) ve `PrintManager`. KitKat olan de tam olarak yararlanmak için ilk API sürümü [Google bulut yazdırma hizmeti API](https://developers.google.com/cloud-print/) kullanarak [Google bulut yazdırma uygulama](https://play.google.com/store/apps/details?id=com.google.android.apps.cloudprint).
KitKat ile otomatik olarak sevk çoğu cihazları Google bulut yazdırma uygulamasını indirin ve [HP yazdırma hizmeti eklentisi](https://play.google.com/store/apps/details?id=com.hp.android.printservice)ilk bağlandıklarında WiFi için. Bir kullanıcı kendi cihazın yazdırma ayarları giderek denetleyebilirsiniz **ayarlar > Sistem > Yazdırma**:

[![Örnek ekran yazdırma ayarları ekran görüntüsü](kitkat-images/printing.png)](kitkat-images/printing.png#lightbox)


> [!NOTE]
> Yazdırma API'leri Google bulut yazdırma ile çalışmak için varsayılan olarak ayarlanır rağmen Android hala yeni API'leri kullanarak yazdırma içerik hazırlama ve yazdırma işlemek için diğer uygulamalarına göndermek geliştiricilerin olanak sağlar.



#### <a name="printing-html-content"></a>Printing HTML Content

KitKat otomatik olarak oluşturur bir [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) olan bir web görünümü için `WebView.CreatePrintDocumentAdapter`. Yazdırma web içeriği Eşgüdümlü çaba arasında olan bir [ `WebViewClient` ](https://developer.xamarin.com/api/type/Android.Webkit.WebViewClient/) yüklemek HTML içerik bekler ve yazdırma seçeneği seçenekleri menüsü kullanılabilmesini bilmeleri etkinliği ve kullanıcıya bekler Actvity olanak tanır Yazdır seçeneğini seçip çağrıları `Print`üzerinde `PrintManager`. Bu bölüm, ekran yazdırmak için gereken temel kurulumu kapsar HTML içeriği.

Yükleme ve web içeriği yazdırma Internet izin gerektirir:

[![Internet izin uygulama ayarları](kitkat-images/internet.png)](kitkat-images/internet.png#lightbox)

##### <a name="print-menu-item"></a>Yazdırma menü öğesi

Yazdırma seçeneği genellikle etkinliğin içinde görünür [seçenekleri menüsü](http://developer.android.com/guide/topics/ui/menus.html#options-menu).
Seçenekleri menüsü kullanıcıların üzerinde bir etkinlik eylemler gerçekleştirmesine izin verir. Ekranın sağ üst köşesinde olduğundan ve şöyle görünür:

[![Yazdırma menü öğesi dispalyed ekranın sağ üst köşesinde, örnek ekran görüntüsü](kitkat-images/menu.png)](kitkat-images/menu.png#lightbox)


Cihazıyla menü öğeleri tanımlanabilir *menü*altında dizin *kaynakları*. Aşağıdaki kodu öğesi adlı bir örnek menüsü tanımlar [yazdırma](https://developer.xamarin.com/api/type/Android.Print.PrintManager/):

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@+id/menu_print"
        android:title="Print"
        android:showAsAction="never" />
</menu>
```

Etkinliğinde seçenekleri menüsü ile etkileşimi olur aracılığıyla `OnCreateOptionsMenu` ve `OnOptionsItemSelected` yöntemleri.
`OnCreateOptionsMenu` Yazdırma seçeneği gibi yeni menü öğeleri ekleme yerdir *menü* kaynakları dizin.
`OnOptionsItemSelected` Yazdırma seçeneği menüden seçerek kullanıcı dinler ve yazdırma başlar:

```csharp
bool dataLoaded;

public override bool OnCreateOptionsMenu (IMenu menu)
{
    base.OnCreateOptionsMenu (menu);
    if (dataLoaded) {
        MenuInflater.Inflate (Resource.Menu.print, menu);
    }
    return true;
}

public override bool OnOptionsItemSelected (IMenuItem item)
{
    if (item.ItemId == Resource.Id.menu_print) {
        PrintPage ();
        return true;
    }
    return base.OnOptionsItemSelected (item);
}
```

Yukarıdaki kod ayrıca adlı bir değişken tanımlar `dataLoaded` HTML içerik durumunu izlemek için. `WebViewClient` Bu değişkeni etkinlik Seçenekler menüsüne yazdırma menü öğesi ekleme bilmesi için tüm içeriği yüklendiğinde, true olarak ayarlanır.

##### <a name="webviewclient"></a>WebViewClient

İş `WebViewClient` verilerde sağlamaktır `WebView` yazdırma seçeneği ile mu menüde görünmeden önce tam olarak yüklü olduğu `OnPageFinished` yöntemi. `OnPageFinished` web içeriği yüklenmesini tamamlamak dinler ve onun seçenekleri menüsü ile yeniden oluşturmak için etkinlik söyler `InvalidateOptionsMenu`:

```csharp
class MyWebViewClient : WebViewClient
{
    PrintHtmlActivity caller;

    public MyWebViewClient (PrintHtmlActivity caller)
    {
        this.caller = caller;
    }

    public override void OnPageFinished (WebView view, string url)
    {
        caller.dataLoaded = true;
        caller.InvalidateOptionsMenu ();
    }
}
```

`OnPageFinished` Ayrıca ayarlar `dataLoaded` değeri `true`, bu nedenle `OnCreateOptionsMenu` menüsünün yerinde Yazdırma seçeneğiyle yeniden oluşturabilirsiniz.

##### <a name="printmanager"></a>PrintManager

Aşağıdaki kod örneğinde içeriğini yazdırır bir `WebView`:

```csharp
void PrintPage ()
{
    PrintManager printManager = (PrintManager)GetSystemService (Context.PrintService);
    PrintDocumentAdapter printDocumentAdapter = myWebView.CreatePrintDocumentAdapter ();
    printManager.Print ("MyWebPage", printDocumentAdapter, null);
}
```

`Print` bağımsız değişkenleri olarak alır: ("MyWebPage" Bu örnekte), yazdırma işi için bir ad bir [ `PrintDocumentAdapter` ](https://developer.xamarin.com/api/type/Android.Print.PrintDocumentAdapter/) içerikten belgeyi yazdır oluşturan ve [ `PrintAttributes` ](https://developer.xamarin.com/api/type/Android.Print.PrintAttributes/) (`null` içinde Yukarıdaki örnek). Belirleyebileceğiniz `PrintAttributes` varsayılan öznitelikler çoğu scenarions işlemesi gereken rağmen yazdırılan sayfada içeriği yerleştirme yardımcı olmak için.

Çağırma `Print` yazdırma işi seçeneklerini listeler yazdırma UI yükler. Kullanıcı Arabirimi kullanıcılara aşağıdaki ekran görüntüleri gösterildiği gibi yazdırma veya HTML içeriğini bir PDF için kaydetme seçeneği sağlar:

[![Yazdır menüsünü görüntüleme PrintHtmlActivity ekran görüntüsü](kitkat-images/print1.png)](kitkat-images/print1.png#lightbox)

[![Kaydetme PDF menü olarak görüntüleme PrintHtmlActivity ekran görüntüsü](kitkat-images/print2.png)](kitkat-images/print2.png#lightbox)

<a name="hardware" />

## <a name="hardware"></a>Donanım

KitKat yeni cihaz özelliklerini uyum sağlayacak şekilde API'leri ekler. En dikkat çeken bir şey bu: ana bilgisayar tabanlı kartı öykünme ve yeni `SensorManager`.

### <a name="host-based-card-emulation-in-nfc"></a>NFC Öykünmelerine ana bilgisayar tabanlı kartı

Ana bilgisayar tabanlı kartı öykünme (HCE) uygulamalarının taşıyıcı özel güvenli öğede kullanmadan NFC kartları veya NFC kart okuyucuları gibi davranır olanak sağlar. HCE ayarlamadan önce cihazına HCE kullanılabildiğinden emin olun `PackageManager.HasSystemFeature`:

```csharp
bool hceSupport = PackageManager.HasSystemFeature(PackageManager.FeatureNfcHostCardEmulation);
```

HCE gerektirir HCE özellik ve `Nfc` izni uygulama kaydedilebilir `AndroidManifest.xml`:

```xml
<uses-feature android:name="android.hardware.nfc.hce" />
```

[![NFC'ye izin uygulama seçenekleri ayarlama](kitkat-images/nfc.png)](kitkat-images/nfc.png#lightbox)

Çalışmak için arka planda çalıştırılabilmesi HCE sahip ve kullanıcı NFC işlem yaptığında HCE kullanarak uygulama çalışmıyorsa bile başlatmak gerekir. Biz HCE kodu olarak yazarak gerçekleştirebilirsiniz bir `Service`. HCE hizmeti uygulayan `HostApduService` aşağıdaki yöntemlerden uygulayan arabirimi:

-  *ProcessCommandApdu* -bir uygulama protokolü veri birimi (APDU) olan NFC okuyucu ve HCE hizmeti arasında gönderilen. Bu yöntem okuyucudan bir ADPU kullanır ve bir veri birimi yanıt olarak döndürür.

-  *OnDeactivated* - `HostAdpuService` HCE hizmeti artık NFC okuyucusu ile iletişim kurarken devre dışı bırakılır.


HCE hizmet ayrıca uygulama bildirimini ile kayıtlı ve uygun permissons hedefi filtre ve meta verileri ile donatılmış gerekir. Aşağıdaki kod örneğidir bir `HostApduService` Android derleme bildirimi kullanarak ile kayıtlı `Service` özniteliği (Xamarin öznitelikleri hakkında daha fazla bilgi için bkz [Android derleme bildirimi ile çalışma](~/android/platform/android-manifest.md) Kılavuzu):

```csharp
[Service(Exported=true, Permission="android.permissions.BIND_NFC_SERVICE"),
    IntentFilter(new[] {"android.nfc.cardemulation.HOST_APDU_SERVICE"}),
    MetaData("andorid.nfc.cardemulation.host.apdu_service",
    Resource="@xml/hceservice")]

class HceService : HostApduService
{
    public override byte[] ProcessCommandApdu(byte[] apdu, Bundle extras)
    {
        ...
    }

    public override void OnDeactivated (DeactivationReason reason)
    {
        ...
    }
}
```

Yukarıdaki hizmeti uygulama ile etkileşim kurmak NFC Okuyucu için bir yol sağlar, ancak NFC okuyucu hala bu hizmet, tarama için gerekli NFC kartı öykünen, hiçbir şekilde bilerek içeriyor. Hizmet tanımlamak NFC okuyucu yardımcı olmak için şu hizmet benzersiz bir atayabilirsiniz *uygulama kimliği (Yardımı)*. Biz HCE hizmeti ile ilgili diğer meta verileri ile birlikte bir yardımcı belirtin xml kaynak dosyasında kayıtlı `MetaData` özniteliği (Yukarıdaki kod örneğine bakın). Bu kaynak dosya bir veya daha fazla yardım filtreleri - karşılık gelen bir veya daha fazla NFC okuyucu aygıtlarının yardımları için benzersiz tanımlayıcı dizeleri onaltılık biçimde belirtir:

```xml
<host-apdu-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:description="@string/hce_service_description"
    android:requireDeviceUnlock="false"
    android:apduServiceBanner="@drawable/service_banner">
    <aid-group android:description="@string/aid_group_description"
                android:category="payment">
        <aid-filter android:name="1111111111111111"/>
        <aid-filter android:name="0123456789012345"/>
    </aid-group>
</host-apdu-service>
```

Yardım filtreleri ek olarak, bir yardımcı Grup ("diğer" karşı ödeme uygulama) belirtir xml kaynak dosyası ayrıca HCE hizmeti, kullanıcıya açık açıklamasını sağlar ve ödeme uygulama, kullanıcıya görüntülenecek 260 x 96 dp başlık söz konusu olduğunda.

Yukarıda özetlenen Kurulum NFC kartı öykünen bir uygulama için temel yapı taşlarını sağlar. NFC kendisini birkaç adım ve yapılandırmak için daha fazla sınama gerektirir. Ana bilgisayar tabanlı kartı öykünme hakkında daha fazla bilgi için başvurmak [Android belge portalımıza](https://developer.android.com/guide/topics/connectivity/nfc/hce.html).
NFC Xamarin ile kullanma hakkında daha fazla bilgi için kullanıma [Xamarin NFC örnekleri](https://github.com/xamarin/monodroid-samples/tree/master/NfcSample).

### <a name="sensors"></a>Algılayıcılar

KitKat cihazın algılayıcılar erişim sağlar bir [ `SensorManager` ](https://developer.xamarin.com/api/type/Android.Hardware.SensorManager/).
`SensorManager` Pil ömrünün koruma Algılayıcı bilgilerine gruplar halinde, bir uygulamaya teslimini zamanlamak işletim sistemi sağlar.

KitKat ayrıca kullanıcının adımları izlemek için iki yeni algılayıcı türü ile birlikte gelir. Bunlar üzerinde accelerometer temel alır ve şunları içerir:

-  *StepDetector* -uygulama bildirim/woken bir adım kullanıcı alır ve adım gerçekleştiği için bir saat değeri algılayıcısı sağlar.

-  *StepCounter* -kullanıcı algılayıcı kaydedildiği bu yana geçen adım sayısı ve *sonraki aygıt yeniden başlatma kadar*.

Aşağıdaki ekran görüntüsünde eylem adım sayacında gösterilmektedir:

[![Bir adım sayaç görüntüleme SensorsActivity uygulamasının ekran görüntüsü](kitkat-images/stepcounter.png)](kitkat-images/stepcounter.png#lightbox)

Oluşturabileceğiniz bir `SensorManager` çağırarak `GetSystemService(SensorService)` ve sonuç olarak atama bir `SensorManager`. Adım sayaç kullanmak için arama `GetDeafultSensor` üzerinde `SensorManager`. Algılayıcı kaydolun ve adım sayısı yardımıyla değişiklikleri dinlemek [ `ISensorEventListener` ](https://developer.xamarin.com/api/type/Android.Hardware.ISensorEventListener/) arabirimi, aşağıdaki kod örneği tarafından gösterildiği gibi:

```csharp
public class MainActivity : Activity, ISensorEventListener
{
    float count = 0;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        SetContentView (Resource.Layout.Main);

        SensorManager senMgr = (SensorManager) GetSystemService (SensorService);
        Sensor counter = senMgr.GetDefaultSensor (SensorType.StepCounter);
        if (counter != null) {
            senMgr.RegisterListener(this, counter, SensorDelay.Normal);
        }
    }

    public void OnAccuracyChanged (Sensor sensor, SensorStatus accuracy)
    {
        Log.Info ("SensorManager", "Sensor accuracy changed");
    }

    public void OnSensorChanged (SensorEvent e)
    {
        count = e.Values [0];
    }
}
```

`OnSensorChanged` Uygulama ön planda olsa da adım sayısı güncelleştirmeleri olursa çağrılır. Uygulama arka girer ya da cihaz uyku `OnSensorChanged` çağrılmaz; ancak, adımları kadar sayılacak devam edecek `UnregisterListener` olarak adlandırılır.

Aklınızda *adım sayısı değeri algılayıcı kaydetmek tüm uygulamalar için toplu*. Bunun anlamı kaldırın ve uygulamanızı yeniden yükleyin ve başlatma olsa bile `count` değişken uygulama başlangıcında 0, algılayıcı tarafından bildirilen değeri algılayıcı kaydedildi, ancak adımlar toplam sayısı da kalacak tarafından mı yoksa, uygulama veya başka bir. Uygulamanızı çağırarak adım sayaç eklemeden engelleyebilirsiniz `UnregisterListener` üzerinde `SensorManager`kod tarafından gösterildiği gibi:

```csharp
protected override void OnPause()
{
    base.OnPause ();
    senMgr.UnregisterListener(this);
}
```

Cihaz yeniden başlatıldığı adım sayısı 0 olarak sıfırlar. Uygulamanız doğru bir sayı algılayıcı veya aygıtın durumu kullanan diğer uygulamalar bağımsız olarak uygulaması için raporlama emin olmak için fazladan kod gerektirir.


> [!NOTE]
> Adım algılama ve KitKat birlikte verilir sayım API sırasında tüm telefonları ile algılayıcı bulunur. Çalıştırarak algılayıcı bulunup bulunmadığını kontrol edebilirsiniz `PackageManager.HasSystemFeature(PackageManager.FeatureSensorStepCounter);`, veya döndürülen değeri sağlamak için onay `GetDefaultSensor` değil `null`.


<a name="developer_tools" />

## <a name="developer-tools"></a>Geliştirici Araçları

### <a name="screen-recording"></a>Ekran kaydı

KitKat yeni ekran özellikleri geliştiricilerin uygulamaları eylemde kaydedebilmesi için kaydı içerir. Ekran kaydı aracılığıyla kullanılabilir [Android hata ayıklama Köprüsü (ADB)](http://developer.android.com/tools/help/adb.html) Android SDK'ın bir parçası olarak indirilebilir istemci.

Ekranınızın kaydetmek için Cihazınızı bağlama; Ardından, Android SDK yüklemenizi bulun, gitmek **platformu Araçları** dizin ve çalışma **adb** istemci:

```shell
adb shell screenrecord /sdcard/screencast.mp4
```

Yukarıdaki komut, varsayılan 3 dakikalık video 4Mbps varsayılan çözünürlükte kaydeder. Uzunluk düzenlemek için add *--zaman sınırı* bayrağı.
Çözümleme değiştirmek için ekleyin *--bit hızı* bayrağı. Aşağıdaki komutu bir dakika uzun videoyu 8Mbps kaydedilecek:

```shell
adb shell screenrecord --bit-rate 8000000 --time-limit 60 /sdcard/screencast.mp4
```

Cihazınızda videonuzu bulabilirsiniz - kayıt tamamlandığında, Galerisi'nde görünür.


## <a name="other-kitkat-additions"></a>Diğer KitKat eklemeler

Yukarıda açıklanan değişikliklerin yanı sıra, KitKat sağlar:

-  *Tam ekran kullanmak* -KitKat tanıtır yeni [derinlikli modu](http://developer.android.com/reference/android/view/View.html#setSystemUiVisibility(int)) içerik taraması, oyunlar oynamak ve çalışan bir tam ekran deneyimlerden yararlanabilecek diğer uygulamalar için.

-  *Bildirimleri özelleştirme* -alma sahip sistem bildirimlerini hakkında ek ayrıntılar [ `NotificationListenerService` ](https://developer.xamarin.com/api/type/Android.Service.Notification.NotificationListenerService/) . Bu, uygulamanızın içinde farklı bir şekilde bilgileri sunmak sağlar.

-  *Yansıtma Drawable kaynakları* -Drawable kaynaklarına sahip yeni bir [ `autoMirrored` ](http://developer.android.com/reference/android/R.attr.html#autoMirrored) sisteme söyler öznitelik çevirme için soldan sağa düzenleri gerektiren görüntüleri için yansıtılmış bir sürüm oluşturun.

-  *Duraklatmak animasyonları* -ile oluşturulan duraklatma ve sürdürme animasyonları [ `Animator` ](https://developer.xamarin.com/api/type/Android.Animation.Animator/) sınıfı.

-  *Okuma dinamik olarak değiştirme metin* -yeni metinle "Canlı bölgeleri olarak" yeni güncelleştirme dinamik olarak UI parçalarını belirtmek [ `accesibilityLiveRegion` ](http://developer.android.com/reference/android/R.attr.html#accessibilityLiveRegion) yeni metin erişilebilirlik modunda otomatik olarak okunacak şekilde özniteliği.

-  *Ses deneyiminizi artırmaya* -yapma izler daha yüksek sesle birlikte [ `LoudnessEnhancer` ](https://developer.xamarin.com/api/type/Android.Media.Audiofx.LoudnessEnhancer/) , en yüksek ve bir ses akışını ile RMS Bul [ `Visualizer` ](https://developer.xamarin.com/api/field/Android.Media.Audiofx.Visualizer.MeasurementModePeakRms/) sınıfı ve bir bilgialın[ses zaman damgası](https://developer.xamarin.com/api/type/Android.Media.AudioTimestamp/) ses video eşitlemede yardımcı olacak.

-  *Özel aralıkta ContentResolver eşitleme* -KitKat bir eşitleme isteği gerçekleştirilen süreye bazı değişkenlik ekler. Eşitleme bir `ContentResolver` özel saatteki veya çağırarak aralığı `ContentResolver.RequestSync` ve içinde geçen bir `SyncRequest`.

-  *Ayırt et arasında denetleyicileri* -içinde KitKat denetleyicileri cihazın erişilen benzersiz tamsayı tanımlayıcıları atanır `ControllerNumber` özelliği. Bu, bir oyunda birbirinden oynatıcıları anlatılmalı kolaylaştırır.

-  *Uzaktan denetim* -donanım ve yazılım tarafında birkaç değişikliklerle KitKat kullanarak bir uzaktan denetim ile bir IR vericisi Otomobillere bir aygıtı sayesinde `ConsumerIrService`ve çevre aygıtları yeni ileetkileşimde[ `RemoteController` ](https://developer.xamarin.com/api/type/Android.Media.RemoteController/) API'leri.

Yukarıdaki API değişiklikler hakkında daha fazla bilgi için lütfen Google [Android 4.4 API'leri](http://developer.android.com/about/versions/android-4.4.html) genel bakış.


## <a name="summary"></a>Özet

Bu makalede Android 4.4 (API düzeyi 19) kullanılabilen yeni API'leri bazıları sunulan en iyi yöntemler KitKat uygulamaya geçiş zaman ele ve. Anahatları belirlenmiş değişiklikleri kullanıcıyı etkileyen API'lerine deneyimi, dahil olmak üzere BT *geçiş framework* ve yönelik yeni seçenekler *tema*. Ardından, sunulan *depolama erişim Framework* ve `DocumentsProvider` sınıfı yanı sıra yeni *API'leri yazdırma*. Bunu incelediniz *NFC ana bilgisayar tabanlı kartı öykünme* ve çalışmak nasıl *düşük güç algılayıcılar*, kullanıcının adımları izlemek için iki yeni algılayıcılar dahil olmak üzere. Son olarak, gerçek zamanlı gösterileri uygulamaları yakalama gösterilen *ekran kaydı*ve KitKat API değişiklikler ve eklemeler ayrıntılı bir listesi.


## <a name="related-links"></a>İlgili bağlantılar

- [KitKat örnek](https://developer.xamarin.com/samples/KitKat/)
- [Android 4.4 API'leri](http://developer.android.com/about/versions/android-4.4.html)
- [Android KitKat](http://developer.android.com/about/versions/kitkat.html)
