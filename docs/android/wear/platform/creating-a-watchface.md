---
title: "Gözcü yüz oluşturma"
description: "Bu kılavuz, Android takmak için bir özel izleme yüz hizmeti uygulamak açıklanmaktadır. Bir kırpılmış bir analog stili izleme yüz oluşturmak için daha fazla kod tarafından izlenen dijital izleme yüz hizmet aşağı oluşturmak için adım adım yönergeler sağlanmıştır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D3F9A40-A820-458D-A12A-D784BB11F643
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: fb3a2a9e60bda2a99a719bf75d23c29d42a94bdb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="creating-a-watch-face"></a>Gözcü yüz oluşturma

_Bu kılavuz, Android takmak için bir özel izleme yüz hizmeti uygulamak açıklanmaktadır. Bir kırpılmış bir analog stili izleme yüz oluşturmak için daha fazla kod tarafından izlenen dijital izleme yüz hizmet aşağı oluşturmak için adım adım yönergeler sağlanmıştır._

## <a name="overview"></a>Genel Bakış 

Bu kılavuzda, özel bir Android takmak izleme yazıtipi oluşturma essentials göstermek için bir temel izleme yüz hizmeti oluşturulur. İlk izleme yüz hizmeti saat ve dakika olarak geçerli saati görüntüleyen basit bir dijital izleme görüntüler: 

[![Dijital izleme yüz](creating-a-watchface-images/01-initial-face.png "ilk dijital izleme yüz örnek ekran görüntüsü")](creating-a-watchface-images/01-initial-face.png#lightbox)

Bu dijital izleme yüz geliştirilen ve test sonra daha fazla kod için daha yükseltmek için üç el ile analog izleme yüz Gelişmiş eklenir: 

[![Analog izleme yüz](creating-a-watchface-images/02-example-watchface.png "son analog izleme yüz örnek ekran görüntüsü")](creating-a-watchface-images/02-example-watchface.png#lightbox)

Gözcü yüz Hizmetleri toplanmış ve yıpranması uygulama bir parçası olarak yüklenir. Aşağıdaki örneklerde, `MainActivity` izleme yüz hizmet paketlenir ve akıllı saate uygulamanın bir parçası olarak dağıtılan böylece yıpranması uygulama şablonu koddan fazlasını içerir. Gerçekte, bu uygulama yıpranması cihaz (veya öykünücü) yüklenen izleme yüz hizmet alma zamanıyla ilgili bir araç olarak hata ayıklama ve test için hizmet verecektir. 

## <a name="requirements"></a>Gereksinimler

Gözcü yüz hizmet uygulamak için aşağıdakiler gereklidir:

-   Android 5.0 (API düzeyi 21) veya üzeri yıpranması cihaz veya öykünücü üzerinde.

-   [Xamarin Android takmak destek kitaplıkları](https://www.nuget.org/packages/Xamarin.Android.Wear) Xamarin.Android projeye eklenmelidir. 

Android 5.0 en az API düzeyi izleme yüz hizmeti, Android 5.1 uygulamak için veya üzeri önerilir, ancak. Android cihaz düşük güç olsa da ekranda görüntülenenleri denetlemek yıpranması uygulamaları daha yüksek izin vermeyi veya takmak Android 5.1 (API 22) çalıştıran cihazlar *ortam* modu. Cihaz, düşük güç ayrıldığında *ortam* Bu mod, konusu *etkileşimli* modu. Bu modları hakkında daha fazla bilgi için bkz: [tutma Your App görünür](https://developer.android.com/training/wearables/apps/always-on.html). 


## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Adlı yeni bir Android takmak projesi oluşturun **WatchFace** (yeni Xamarin.Android projeleri oluşturma hakkında daha fazla bilgi için bkz: [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)):

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Yeni Proje iletişim kutusu](creating-a-watchface-images/03-wear-project-vs-sml.png "yeni proje iletişim kutusunda takmak uygulama seçin")](creating-a-watchface-images/03-wear-project-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Yeni Proje iletişim kutusu](creating-a-watchface-images/03-wear-project-xs-sml.png "yeni proje iletişim kutusunda takmak uygulama seçin")](creating-a-watchface-images/03-wear-project-xs.png#lightbox)

-----


Paket adı ayarlamak `com.xamarin.watchface`:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Paket adı ayarı](creating-a-watchface-images/04-package-name-vs.png "com.xamarin.watchface için paket adını ayarlayın")](creating-a-watchface-images/04-package-name-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Paket adı ayarı](creating-a-watchface-images/04-package-name-xs.png "com.xamarin.watchface için paket adını ayarlayın")](creating-a-watchface-images/04-package-name-xs.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ayrıca, aşağı kaydırın ve etkinleştirme **Internet** ve **WAKE_LOCK** izinleri: 

[![Gerekli izinleri](creating-a-watchface-images/05-required-permissions-vs.png "Internet etkinleştirmek ve WAKE_LOCK izinleri")](creating-a-watchface-images/05-required-permissions-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Minimum Android sürümü kümesine **Android 5.1 (API düzeyi 22)**. Ayrıca, etkinleştirme **Internet** ve **WakeLock** izinleri:

[![Gerekli izinleri](creating-a-watchface-images/05-required-permissions-xs.png "Internet etkinleştirmek ve WakeLock izinleri")](creating-a-watchface-images/05-required-permissions-xs.png#lightbox)

-----

Ardından, indirme [preview.png](creating-a-watchface-images/preview.png) &ndash; bu eklenecek **drawables** bu kılavuzda daha sonra klasör.


## <a name="add-the-xamarin-android-wear-package"></a>Xamarin Android yıpranması Paketi Ekle

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

NuGet Paket Yöneticisi'ni başlatın (Visual Studio'da sağ **başvuruları** içinde **Çözüm Gezgini** seçip **NuGet paketlerini Yönet...** ). En son kararlı sürümü için projesini güncelleştirme **Xamarin.Android.Wear**: 

[![NuGet Paket Yöneticisi eklemek](creating-a-watchface-images/06-add-wear-pkg-vs-sml.png "Xamarin.Android.Wear Paketi Ekle")](creating-a-watchface-images/06-add-wear-pkg-vs.png#lightbox)

Sonraki, eğer **Xamarin.Android.Support.v13** olan yüklü, kaldırın:

[![NuGet Paket Yöneticisi Kaldır](creating-a-watchface-images/07-uninstall-v13-sml.png "Xamarin.Support.v13 Kaldır")](creating-a-watchface-images/07-uninstall-v13.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

NuGet Paket Yöneticisi'ni başlatın (Mac için Visual Studio'da sağ **paketleri** içinde **çözüm bölmesinde** seçip **paketleri Ekle...** ). En son kararlı sürümü için projesini güncelleştirme **Xamarin.Android.Wear**: 

[![NuGet Paket Yöneticisi eklemek](creating-a-watchface-images/06-add-wear-pkg-xs-sml.png "Xamarin.Android.Wear Paketi Ekle")](creating-a-watchface-images/06-add-wear-pkg-xs.png#lightbox)

-----


Derleme ve uygulamayı bir yıpranması cihaz veya öykünücü çalıştırma (Bunun nasıl yapılacağı hakkında daha fazla bilgi için bkz: [Başlarken](~/android/wear/get-started/index.md) Kılavuzu). Aşağıdaki uygulama ekran yıpranması aygıtta görmeniz gerekir:

[![Uygulama ekran](creating-a-watchface-images/08-app-screen.png "yıpranması cihazda uygulama ekran")](creating-a-watchface-images/08-app-screen.png#lightbox)

Bu noktada, bir izleme yüz hizmet uygulaması henüz sağlamadığı temel yıpranması uygulama izleme yüz işlevi yoktur. Bu hizmet sonraki eklenir. 

 
## <a name="canvaswatchfaceservice"></a>CanvasWatchFaceService

Android yıpranması Implements izlemek yüzeyleri aracılığıyla `CanvasWatchFaceService` sınıfı. `CanvasWatchFaceService` türetilmiş `WatchFaceService`, kendisi türetilir `WallpaperService` aşağıdaki çizimde gösterildiği gibi: 

[![Devralma diyagramı](creating-a-watchface-images/09-inheritance-diagram-sml.png "CanvasWatchFaceService devralma diyagramı")](creating-a-watchface-images/09-inheritance-diagram.png#lightbox)

`CanvasWatchFaceService` iç içe bir içeren `CanvasWatchFaceService.Engine`; bunu başlatır bir `CanvasWatchFaceService.Engine` izleme yüz çizim asıl işi yapan nesne. `CanvasWatchFaceService.Engine` türetilmiş `WallpaperService.Engine` Yukarıdaki diyagramda gösterildiği gibi. 

Bu şemada gösterilmez olan bir `Canvas` , `CanvasWatchFaceService` izleme yüz çizim için kullandığı &ndash; bu `Canvas` aracılığıyla geçirilen `OnDraw` aşağıda açıklandığı gibi yöntemi. 

Aşağıdaki bölümlerde, aşağıdaki adımları izleyerek özel izleme yüz hizmet oluşturulacak: 

1.  Adlı bir sınıf tanımlama `MyWatchFaceService` öğesinden türetilen `CanvasWatchFaceService`. 

2.  İçinde `MyWatchFaceService`, adlı bir iç içe geçmiş sınıf oluşturmak `MyWatchFaceEngine` öğesinden türetilen `CanvasWatchFaceService.Engine`. 

3.  İçinde `MyWatchFaceService`, uygulama bir `CreateEngine` başlatır yöntemi `MyWatchFaceEngine` ve döndürür.

4.  İçinde `MyWatchFaceEngine`, uygulama `OnCreate` izleme yazıtipi stili oluşturma ve diğer başlatma görevlerini gerçekleştirmek için yöntem. 

5.  Uygulama `OnDraw` yöntemi `MyWatchFaceEngine`. Gözcü yüz çizilmesi gerektiğinde bu yöntem çağrılır (yani *geçersiz kılınan*). `OnDraw` saat, dakika ve ikinci ellerini gibi izleme yüz öğeleri çizer (ve yeniden çizer) yöntemidir. 

6.  Uygulama `OnTimeTick` yöntemi `MyWatchFaceEngine`. 
    `OnTimeTick` dakika (içinde ortam ve etkileşimli modları) veya tarih/saat ne zaman değiştiğini en az bir kez çağrılır. 

Hakkında daha fazla bilgi için `CanvasWatchFaceService`, Android bkz [CanvasWatchFaceService](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.html) API belgeleri.
Benzer şekilde, [CanvasWatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html) izleme yüz gerçek uyarlamasını açıklar.


### <a name="add-the-canvaswatchfaceservice"></a>CanvasWatchFaceService Ekle

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Adlı yeni bir dosya ekleyin **MyWatchFaceService.cs** (Visual Studio'da sağ **WatchFace** içinde **Çözüm Gezgini**,'ı tıklatın **Ekle > Yeni öğe...** seçip **sınıfı**).

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Adlı yeni bir dosya ekleme **MyWatchFaceService.cs** (Mac için Visual Studio'da sağ **WatchFace** projesi,'ı tıklatın **Ekle > yeni dosya …** seçip **boş sınıfı**). 

-----

Bu dosyanın içeriğini aşağıdaki kodla değiştirin: 

```csharp
using System;
using Android.Views;
using Android.Support.Wearable.Watchface;
using Android.Service.Wallpaper;
using Android.Graphics;

namespace WatchFace
{
    class MyWatchFaceService : CanvasWatchFaceService
    {
        public override WallpaperService.Engine OnCreateEngine()
        {
            return new MyWatchFaceEngine(this);
        }

        public class MyWatchFaceEngine : CanvasWatchFaceService.Engine
        {
            CanvasWatchFaceService owner;
            public MyWatchFaceEngine (CanvasWatchFaceService owner) : base(owner)
            {
                this.owner = owner;
            }
        }
    }
}
```

`MyWatchFaceService` (türetilmiş `CanvasWatchFaceService`) "ana" izleme yazıtipinin programdır. `MyWatchFaceService` yalnızca bir yöntemi, uygulayan `OnCreateEngine`, oluşturur ve döndürür bir `MyWatchFaceEngine` nesne (`MyWatchFaceEngine` türetildiği `CanvasWatchFaceService.Engine`). Örneklenen `MyWatchFaceEngine` Nesne döndürdü, olarak bir `WallpaperService.Engine`. Kapsüllenen `MyWatchFaceService` oluşturucuya nesnesi geçirildi. 

`MyWatchFaceEngine` Gerçek izleme yüz uygulamasıdır &ndash; izleme yüz çizer kodunu içerir. Ayrıca, sistem olaylarına (ortam/etkileşimli modları, ekran kapatma, vs.) ekran değişerek gibi işler. 

 
### <a name="implement-the-engine-oncreate-method"></a>Uygulama altyapısı OnCreate yöntemi

`OnCreate` Yöntemi izleme yüz başlatır. Aşağıdaki alana ekleme `MyWatchFaceEngine`: 

```csharp
Paint hoursPaint;
```

Bu `Paint` nesne, geçerli saati izleme yüz çizmek için kullanılacak. Sonra aşağıdaki yöntemi ekleyin `MyWatchFaceEngine`: 

```csharp
public override void OnCreate(ISurfaceHolder holder)
{
    base.OnCreate (holder);

    SetWatchFaceStyle (new WatchFaceStyle.Builder(owner)
        .SetCardPeekMode (WatchFaceStyle.PeekModeShort)
        .SetBackgroundVisibility (WatchFaceStyle.BackgroundVisibilityInterruptive)
        .SetShowSystemUiTime (false)
        .Build ());

    hoursPaint = new Paint();
    hoursPaint.Color = Color.White;
    hoursPaint.TextSize = 48f;
}
```

`OnCreate` kısa bir süre sonra çağrılır `MyWatchFaceEngine` başlatılır. Bu ayarlar `WatchFaceStyle` (denetleyen yıpranması cihaz kullanıcı ile nasıl etkileşim kurduğu) ve başlatır `Paint` zaman görüntülemek için kullanılan nesne. 

Çağrı `SetWatchFaceStyle` şunları yapar:

1.  Ayarlar *gözlem modu* için `PeekModeShort`, görüntü küçük "peek" kartlarında olarak görünmesi bildirimler neden olur. 

2.  Arka plan görünürlüğünü ayarlar `Interruptive`, yalnızca kısa bir süre interruptive bildirim temsil gösterilecek bir gözlem kartı arka planı neden olur.

3.  Özel İzleme yüz zaman yerine görüntüleyebilmesi izleme yüz çizilen varsayılan sistem kullanıcı Arabirimi süresi devre dışı bırakır.

Bunlar ve diğer izleme yazıtipi stili seçenekleri hakkında daha fazla bilgi için bkz: Android [WatchFaceStyle.Builder](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceStyle.Builder.html) API belgeleri.

Sonra `SetWatchFaceStyle` tamamlandıktan `OnCreate` başlatır `Paint` nesne (`hoursPaint`) ve beyaz ve metin boyutuna 48 piksel rengini ayarlar ([Textsıze](https://developer.android.com/reference/android/graphics/Paint.html#setTextSize%28float%29) piksel cinsinden belirtilmesi gerekir). 


### <a name="implement-the-engine-ondraw-method"></a>Uygulama altyapısı OnDraw yöntemi

`OnDraw` Yöntemi en önemli olan belki `CanvasWatchFaceService.Engine` yöntemi &ndash; , aslında çizer basamak gibi yüz öğeleri izlemek ve saat yüz ellerini olduğunu yöntemdir. Aşağıdaki örnekte, bir saat dizesi izleme yüz çizer.
Aşağıdaki yöntemi ekleyin `MyWatchFaceEngine`:

```csharp
public override void OnDraw (Canvas canvas, Rect frame)
{
    var str = DateTime.Now.ToString ("h:mm tt");
    canvas.DrawText (str, 
        (float)(frame.Left + 70), 
        (float)(frame.Top  + 80), hoursPaint);
}
```

Android çağırdığında `OnDraw`, içinde başarılı bir `Canvas` örneği ve içinde yüz çizilen sınırlar. Yukarıdaki kod örneğinde, `DateTime` geçerli saati saat ve dakika (12 saat biçiminde) hesaplamak için kullanılır. Sonuç saat dizesi kullanarak tuvalde çizilir `Canvas.DrawText` yöntemi. Dize 70 piksel üzerinden sol kenarı ve 80 piksel aşağı üst kenarından görünür. 

Hakkında daha fazla bilgi için `OnDraw` yöntemi, Android bkz [onDraw](https://developer.android.com/reference/android/support/wearable/watchface/CanvasWatchFaceService.Engine.html#onDraw(android.graphics.Canvas, android.graphics.Rect)) API belgeleri.


### <a name="implement-the-engine-ontimetick-method"></a>Uygulama altyapısı OnTimeTick yöntemi

Android düzenli aralıklarla çağırır `OnTimeTick` izleme yüz tarafından gösterilen zaman güncelleştirmek için yöntem. Bu, (içindeki ortam ve etkileşimli modda), dakika başına en az bir kez veya tarih veya saat dilimi değişti zaman çağrılır. Aşağıdaki yöntemi ekleyin `MyWatchFaceEngine`: 

```csharp
public override void OnTimeTick()
{
    Invalidate();
}
```

Bu uygulaması, `OnTimeTick` yalnızca çağırır `Invalidate`. `Invalidate` Yöntemi zamanlamaları `OnDraw` izleme yüz yeniden çizmek için. 

Hakkında daha fazla bilgi için `OnTimeTick` yöntemi, Android bkz [onTimeTick](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onTimeTick()) API belgeleri.

 
## <a name="register-the-canvaswatchfaceservice"></a>CanvasWatchFaceService kaydetme

`MyWatchFaceService` kaydedilmelidir **AndroidManifest.xml** ilişkili yıpranması uygulamanın. Bunu yapmak için aşağıdaki XML ekleyin `<application>` bölümü: 

```xml
<service 
    android:name="watchface.MyWatchFaceService" 
    android:label="Xamarin Sample" 
    android:allowEmbedded="true" 
    android:taskAffinity="" 
    android:permission="android.permission.BIND_WALLPAPER">
    <meta-data 
        android:name="android.service.wallpaper" 
        android:resource="@xml/watch_face" />
    <meta-data 
        android:name="com.google.android.wearable.watchface.preview" 
        android:resource="@drawable/preview" />
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
        <category android:name="com.google.android.wearable.watchface.category.WATCH_FACE" />
    </intent-filter>
</service>
```

Bu XML şunları yapar:

1.  Ayarlar `android.permission.BIND_WALLPAPER` izni. Bu izni aygıttaki sistem duvar kağıdını değiştirme izleme yüz hizmet izni verir. Bu izni ayarlayın, Not `<service>` bölüm yerine dış `<application>` bölümü. 

2.  Tanımlayan bir `watch_face` kaynak. Bu kaynağın bildirir kısa bir XML dosyası olduğu bir `wallpaper` kaynak (Bu dosya, sonraki bölümde de oluşturulur). 

3.  Adlı drawable bir görüntü bildirir `preview` izleme Seçici seçimi ekran görüntülenir. 

4.  İçeren bir `intent-filter` biliyor Android olanak `MyWatchFaceSevice` izleme yüz görüntüleme. 

Temel kodunu tamamlanan `WatchFace` örnek. Sonraki adım, gerekli kaynakları eklemektir.

 
## <a name="add-resource-files"></a>Kaynak Dosyaları Ekle

İzleme hizmeti çalıştırmadan önce eklemelisiniz **watch_face** kaynak ve Önizleme görünümü. İlk olarak, en yeni bir XML dosyası oluşturma **Resources/xml/watch_face.xml** ve içeriğini aşağıdaki XML ile değiştirin: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<wallpaper xmlns:android="http://schemas.android.com/apk/res/android" />
```

Bu dosyanın derleme eylem kümesine **AndroidResource**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Derleme eylemi](creating-a-watchface-images/10-android-resource-vs.png "AndroidResource eyleme kümesi oluşturma")](creating-a-watchface-images/10-android-resource-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Derleme eylemi](creating-a-watchface-images/10-android-resource-xs.png "AndroidResource eyleme kümesi oluşturma")](creating-a-watchface-images/10-android-resource-xs.png#lightbox)

-----

Bu kaynak dosyası basit bir tanımlar `wallpaper` izleme yüz için kullanılacak öğe. 

Henüz yapmadıysanız, indirme [preview.png](creating-a-watchface-images/preview.png). Adresinden yükleme **Resources/drawable/preview.png**. Bu dosyaya eklediğinizden emin olun `WatchFace` projesi. Bu Önizleme görünümü yıpranması aygıtta izleme yüz seçicisinde kullanıcıya görüntülenir. Kendi izleme yüz için önizleme görüntüsü oluşturmak için bir ekran görüntüsünü izleme yüz çalışırken alabilir. (Yıpranması aygıtlardan ekran görüntüleri alma hakkında daha fazla bilgi için bkz: [ekran görüntüleri alma](~/android/wear/deploy-test/debug-on-device.md#screenshots)). 


## <a name="try-it"></a>Deneyin!

Derleme ve yıpranması aygıta uygulama dağıtabilirsiniz. Önceki gibi görünmesini yıpranması uygulama ekran görmeniz gerekir. Yeni izleme yüz etkinleştirmek için aşağıdakileri yapın: 

1.  Gözcü ekran arka planı görene kadar sağa geçirme. 

2.  Dokunma ve herhangi bir yere ekranı arka plan üzerinde iki saniye basılı tutun.

3.  Sol sağdan çeşitli izleme yüzeyleri Gözat sağa. 

4.  Seçin **Xamarin örnek** izlemek yüz (sağ tarafta gösterilen): 

    [![Watchface Seçici](creating-a-watchface-images/11-watchface-picker.png "Xamarin örnek izleme yüz bulmaya geçirme")](creating-a-watchface-images/11-watchface-picker.png#lightbox)

5.  Dokunun **Xamarin örnek** seçmek için yüz izleyin. 

Bu, o ana kadarki uygulanan özel izleme yüz hizmetini kullanmak için yıpranması cihaz izleme yüz değiştirir: 

[![Dijital izleme yüz](creating-a-watchface-images/12-digital-watchface.png "yıpranması cihazda çalışan özel dijital izleme")](creating-a-watchface-images/12-digital-watchface.png#lightbox)

Uygulama uygulama böylece en az bir görece kaba izleme yazıtipi olmasıdır (örneğin, bir izleme yüz arka plan içermez ve arama değil `Paint` görünümlerini geliştirmek için yumuşatma yöntemleri). Ancak, özel izleme yüz oluşturmak için gerekli olan tam kemikler işlevleri uygulayın. 

Sonraki bölümde, bu izleme yüz daha karmaşık bir uygulama yükseltilir. 

 
## <a name="upgrading-the-watch-face"></a>Gözcü yüz yükseltme

Bu kılavuzda geri kalanında `MyWatchFaceService` bir analog stili izleme karşılaştığı görüntülenecek yükseltilir ve diğer özellikleri destekleyecek şekilde genişletilmiştir. Yükseltilen izleme yüz oluşturmak için aşağıdaki özellikleri eklenecek: 

1.  Analog saat, dakika ve ikinci el ile süresini gösterir.

2.  Değişiklikler görünürlüğü yanıt verir.

3.  Ortam modu ve etkileşimli mod arasındaki değişiklikleri yanıt verir. 

4.  Temel alınan yıpranması cihaz özelliklerini okur.

5.  Otomatik olarak bir saat dilimi değişiklik ne zaman yer aldığı güncelleştirir. 

Aşağıdaki kod değişiklikleri uygulamadan önce indirme [drawable.zip](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/Resources/drawable.zip?raw=true), onu sıkıştırmasını açın ve sıkıştırması açılmış .png dosyaları taşıma **kaynakları/drawable** (önceki üzerine **preview.png**). Yeni .png dosyalar eklediğinizde `WatchFace` projesi.


### <a name="update-engine-features"></a>Güncelleştirme altyapısı özellikleri

Sonraki adım yükseltmedir **MyWatchFaceService.cs** bir analog izleme yüz çizer ve yeni özellikleri destekleyen bir uygulamaya. Değiştir **MyWatchFaceService.cs** izleme yüz kodda analog sürümüyle [MyWatchFaceService.cs](https://github.com/xamarin/monodroid-samples/blob/master/wear/WatchFace/WatchFace/MyWatchFaceService.cs) (kesme ve bu kaynak mevcut Yapıştır  **MyWatchFaceService.cs**). 

Bu sürümü **MyWatchFaceService.cs** daha fazla kod mevcut yöntemler ekler ve daha fazla işlevsellik eklemek için ek geçersiz kılınan yöntemleri içerir. Aşağıdaki bölümler, kaynak kodun Kılavuzlu Tur sağlar.

#### <a name="oncreate"></a>OnCreate

Güncelleştirilmiş **OnCreate** yöntemi izleme Yüzey Stili eskisi yapılandırır, ancak bazı ek adımlar içerir: 

1.  Arka plan görüntüsü olarak ayarlar **xamarin_background** içinde bulunduğu kaynak **Resources/drawable-hdpi/xamarin_background.png**. 

2.  Başlatır `Paint` saat el dakika el ve saniyeyi çizim nesneleri.

3.  Başlatır bir `Paint` izleme yüz kenarına etrafında saat çizgilerine çizim nesnesi. 

4.  Çağıran bir zamanlayıcı oluşturur `Invalidate` (yeniden düzenlenen) yöntemi böylece saniyeyi saniyede çizilir. Bu zamanlayıcı gereklidir çünkü `OnTimeTick` çağrıları `Invalidate` yalnızca bir kez dakikada.

Bu örnek yalnızca birini içeren **xamarin_background.png** görüntü; ancak, özel izleme yüz destekleyecek her ekran yoğunluğu farklı arka plan resmini oluşturmak isteyebilirsiniz. 

#### <a name="ondraw"></a>OnDraw

Güncelleştirilmiş **OnDraw** yöntemi aşağıdaki adımları kullanarak bir analog stili izleme yüz çizer: 

1.  Şimdi tutulur geçerli saati alır bir `time` nesnesi. 

2.  Çizim yüzeyini ve kendi merkezi sınırları belirler.

3.  Arka plan çizildiğinde cihaz sığacak şekilde boyutlandırılmış arka plan çizer.

4.  On iki çizer *çizgilerine* (Saat yüzünü saatleri karşılık gelen) saatin yüzünü geçici. 

5.  Açı, döndürme ve her gözlem el uzunluğunu hesaplar.

6.  Her elin izleme yüzey üzerinde çizer. Gözcü ortam modundaysa saniyeyi çizilmiş değil unutmayın. 

 
#### <a name="onpropertieschanged"></a>OnPropertiesChanged

Bu yöntem bildirmek için çağrılır `MyWatchFaceEngine` (örneğin, düşük bit ortam modu ve yazma bileşenini koruma) yıpranması cihaz özellikleri hakkında. İçinde `MyWatchFaceEngine`, ortam modu düşük bit için bu yöntem yalnızca denetler (düşük bit ortam modunda ekran daha az BITS her rengini destekler). 

Bu yöntem hakkında daha fazla bilgi için bkz: Android [onPropertiesChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onPropertiesChanged%28android.os.Bundle%29) API belgeleri.


#### <a name="onambientmodechanged"></a>OnAmbientModeChanged

Yıpranması aygıt girdiğinde veya ortam modu çıktığında bu yöntem çağrılır. İçinde `MyWatchFaceEngine` uygulaması, izleme yüz devre dışı bırakır düzgünleştirme ortam modunda olduğunda. 

Bu yöntem hakkında daha fazla bilgi için bkz: Android [onAmbientModeChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onAmbientModeChanged%28boolean%29) API belgeleri.


#### <a name="onvisibilitychanged"></a>OnVisibilityChanged

İzleme durumuna geldiğinde görünür mü yoksa gizli bu yöntem çağrılır. İçinde `MyWatchFaceEngine`, bu yöntem kasalar / (aşağıda görünürlük durumuna göre açıklanmıştır) saat dilimi alıcı kaydını siler. 

Bu yöntem hakkında daha fazla bilgi için bkz: Android [onVisibilityChanged](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html#onVisibilityChanged%28boolean%29) API belgeleri.

 
### <a name="time-zone-feature"></a>Saat dilimi özelliği

Yeni **MyWatchFaceService.cs** saat dilimi her değiştiğinde (saat dilimlerinde seyahat while gibi) geçerli saati güncelleştirmek için işlevselliği de içerir. Sonuna **MyWatchFaceService.cs**, bir saat dilimini değiştirme `BroadcastReceiver` tanımlanan saat dilimi değiştirilen hedefi nesneleri işler: 

```csharp
public class TimeZoneReceiver: BroadcastReceiver
{
    public Action<Intent> Receive { get; set; }
    public override void OnReceive (Context context, Intent intent)
    {
        if (Receive != null)
            Receive (intent);
    }
}
```

`RegisterTimezoneReceiver` Ve `UnregisterTimezoneReceiver` yöntemleri tarafından çağrılır `OnVisibilityChanged` yöntemi. 
`UnregisterTimezoneReceiver` Gözcü yüz görünürlük durumu olarak değiştirildiğinde, gizli olarak adlandırılır. Gözcü yüz yeniden görünür olduğunda `RegisterTimezoneReceiver` olarak adlandırılır (bkz `OnVisibilityChanged` yöntemi). 

Altyapı `RegisterTimezoneReceiver` yöntemi bu saat dilimi alıcının için bir işleyici bildirir `Receive` olay; bu işleyici güncelleştirmeleri `time` nesnesi ile bir saat dilimi geçilen her yeni saat: 

```csharp
timeZoneReceiver = new TimeZoneReceiver ();
timeZoneReceiver.Receive = (intent) => {
    time.Clear (intent.GetStringExtra ("time-zone"));
    time.SetToNow ();
};
```

Hedefi bir filtre oluşturulur ve için saat dilimi alıcı kayıtlı:

```csharp
IntentFilter filter = new IntentFilter(Intent.ActionTimezoneChanged);
Application.Context.RegisterReceiver (timeZoneReceiver, filter);
```

`UnregisterTimezoneReceiver` Yöntemi, saat dilimi alıcı kaydını iptal eder:

```csharp
Application.Context.UnregisterReceiver (timeZoneReceiver);
```

### <a name="run-the-improved-watch-face"></a>Geliştirilmiş izleme yüz çalıştırın

Derleme ve uygulamayı yıpranması aygıta yeniden dağıtın. Gözcü yüz önce izleme yüz Seçici seçin. Sol tarafta izleme Seçici önizlemede gösterilen ve yeni izleme yüz sağ tarafta gösterilir:

[![Analog izleme yüz](creating-a-watchface-images/13-analog-watchface.png "geliştirilmiş analog yüz Seçici ve cihaz")](creating-a-watchface-images/13-analog-watchface.png#lightbox)

Bu ekran görüntüsünde, saniyeyi kez saniye başına taşımaktır. Bu kod bir yıpranması cihazda çalıştırdığınızda, Gözcü ortam moduna girdiğinde saniyeyi kaybolur.

 
## <a name="summary"></a>Özet

Bu kılavuzda, özel bir Android takmak watchface uygulanan ve test. `CanvasWatchFaceService` Ve `CanvasWatchFaceService.Engine` sınıfları kullanıma sunulmuştur ve temel altyapısı sınıfı yöntemlerinin basit dijital izleme yüz oluşturmak için uygulanan. Bu uygulama bir analog izleme yüz oluşturmak için daha fazla işlevsellik güncelleştirildi ve görünürlük, ortam modu ve cihaz özellikleri farklılıkları değişiklikleri işlemek için ek yöntemleri uygulanan. Son olarak, izleme otomatik olarak bir saat dilimi zaman çapraz zaman güncelleştirilebilmesi için saat dilimi yayın alıcı uygulanmıştır. 


## <a name="related-links"></a>İlgili bağlantılar

- [Gözcü yüzeyleri oluşturma](https://developer.android.com/training/wearables/watch-faces/index.html)
- [WatchFace örnek](https://developer.xamarin.com/samples/monodroid/wear/WatchFace)
- [WatchFaceService.Engine](https://developer.android.com/reference/android/support/wearable/watchface/WatchFaceService.Engine.html)
