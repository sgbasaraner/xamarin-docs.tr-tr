---
title: Xamarin.Android izinleri
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: b8a8005c69c8aaee5d92bdabb3429bd52fc76b4a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30770241"
---
# <a name="permissions-in-xamarinandroid"></a>Xamarin.Android izinleri


## <a name="overview"></a>Genel Bakış

Android uygulamaları kendi korumalı alanda çalışır ve için güvenlik nedeniyle bazı sistem kaynakları veya donanım cihazda erişiminiz yok. Bu kaynakları kullanabilir önce kullanıcı açıkça uygulama izni vermeniz gerekir. Örneğin, bir uygulama bir cihazda açık izniniz olmadan GPS kullanıcıdan erişemiyor. Android oluşturur bir `Java.Lang.SecurityException` uygulama izni olmadan korunan bir kaynağa erişmeyi denerse.

İzinleri içinde bildirilen **AndroidManifest.xml** uygulama geliştirilmiş uygulama geliştiricisi tarafından. Android Bu izinler için kullanıcının izni almak için iki farklı iş akışları sahiptir:
 
* Uygulama yüklendiğinde Android 5.1 (API düzeyi 22) veya alt hedeflenen uygulamalar için izin isteği oluştu. Kullanıcı izinleri ver değil, sonra app yüklenmemesi. Uygulama yüklendikten sonra uygulamayı kaldırmayı izinler dışında bir yolu yoktur.
* Android 6.0 (API düzeyi 23) başlayarak, kullanıcıların izinleri hakkında daha fazla denetime verildi; vermek veya uygulamayı cihazda yüklü olduğu sürece izinler. Bu ekran Google kişiler uygulama için izin ayarları gösterir. Çeşitli izinleri listeler ve etkinleştirme veya devre dışı izinleri olanak tanır:

![Örnek izinleri ekran](permissions-images/01-permissions-check.png) 

Android uygulamaları, korunan bir kaynağa erişmek için izniniz olup olmadığını görmek için zamanında denetlemeniz gerekir. Uygulama izni yoksa, izinleri vermek için kullanıcı için Android SDK'sı tarafından sağlanan yeni API'leri kullanarak istekleri olmalısınız. İzinler iki kategoriye ayrılır:

* **Normal izinleri** &ndash; kullanıcının güvenlik veya gizlilik için çok az güvenlik riski izinlerinden bunlar. Android 6.0 yükleme sırasında otomatik olarak normal izin verecektir. Lütfen Android yönelik belgelere bir [normal izinlerle tam listesi](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Tehlikeli izinler** &ndash; normal izinler aksine, tehlikeli izinler kullanıcının güvenlik veya gizlilik koruyan izinlerdir. Bu özel verilen kullanıcı tarafından olmalıdır. Gönderme veya SMS iletisi alma tehlikeli izni gerektiren bir eylem örneğidir.

> [!IMPORTANT]
> Bir izin ait olduğu kategoriyi zaman içinde değişebilir.  "Normal" izni olabileceğinden kategorilere ayrılmış bir iznine tehlikeli izni gelecekte API düzeylerini yükseltilmiş mümkündür.

Tehlikeli izinler daha alt bölünür içine [ _izin grupları_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Bir izin grubu mantıksal olarak ilgili izinleri tutar. Kullanıcının izni grubunun bir üyesi izni veriyorsa, Android, bu grubun tüm üyelerine otomatik olarak izin verir. Örneğin, [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) izin grubu tutan her ikisi de `WRITE_EXTERNAL_STORAGE` ve `READ_EXTERNAL_STORAGE` izinleri. Kullanıcı izin verirse `READ_EXTERNAL_STORAGE`, sonra `WRITE_EXTERNAL_STORAGE` izni aynı anda otomatik olarak verilir.

Bir veya daha fazla izin istemeden önce bunu neden uygulama izni istemeden önce izni gerekiyor. konusunda bir stratejinin sağlamak için en iyi uygulamadır. Kullanıcı stratejinin anlar sonra uygulama kullanıcıdan izin isteyebilir. İzni verin ve değillerse varsa anlamak istediklerinde stratejinin anlayarak, kullanıcı bir bilinçli karar verebilirsiniz. 

Denetleme ve izinleri isteyen tüm iş akışı olarak bilinen bir _çalıştırma izinleri_ denetleyin ve aşağıdaki çizimde özetlenebilir: 

[![Çalıştırma izni onay akış çizelgesi](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android destek kitaplığı backports bazı izinler Android eski sürümleri için yeni API'ler. Bir API düzey denetimi her seferinde gerçekleştirmek gerekli değildir bu backported API sürümünde Android cihaz otomatik olarak kontrol eder.  

Bu belgede bir Xamarin.Android uygulamasına izinleri eklemek nasıl ele alınacaktır ve nasıl Android 6.0 (API düzeyi 23) hedef uygulamaları ya da daha yüksek bir çalıştırma izni denetimi gerçekleştirmeniz gerekir.


> [!NOTE]
> Bu uygulamayı Google Play tarafından nasıl filtre donanım izinlerini etkileyebilir mümkündür. Uygulama için kamera iznini gerektiriyorsa, örneğin, ardından Google Play uygulaması yüklü bir kamera yok bir cihazda Google Play Store'da göstermez.


<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Xamarin.Android projeleri içerdiğini kesinlikle önerilir [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet paketi. Sürekli belirli API eski sürümleri, Android, bir ortak sağlamak için gerek kalmadan arabirim bu paket backport izin uygulamayı çalıştıran Android sürümünü denetleyin.


## <a name="requesting-system-permissions"></a>Sistem izinleri isteyen

Android izinlerle çalışma ilk adımı, Android izinler bildirim dosyası bildirmektir. Bu uygulama atamak olduğunu API düzeyi bağımsız olarak yapılmalıdır.

Android 6.0 hedef veya kullanıcı, belirli bir noktada geçmişte izni sonraki açışınızda geçerli olacaktır izni olduğundan daha yüksek olmadığını varsayar olamaz uygulamalar. Android 6.0 hedefleyen bir uygulama her zaman bir çalışma zamanı izni denetimi gerçekleştirmeniz gerekir. Android 5.1 veya alt hedef uygulamaları çalıştırma izni denetimi gerçekleştirmek gerekmez.

> [!NOTE]
> Uygulamalar, yalnızca ihtiyaç izinleri istemeniz gerekir.


### <a name="declaring-permissions-in-the-manifest"></a>Bildiriminde izinleri bildirme

İzinleri eklenir **AndroidManifest.xml** ile `uses-permission` öğesi. Örneğin, bir uygulama cihazın konumunu bulmak için ise ince gerektirir ve konum izinleri kurs. Aşağıdaki iki öğeyi bildirime eklenir: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'ya yerleşik araç desteği kullanarak izinlerini bildirme mümkündür:

1. Çift **özellikleri** içinde **Çözüm Gezgini** seçip **Android derleme bildirimi** Özellikleri penceresinde sekme:

    [![Android derleme bildirimi sekmesinde gerekli izinler](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Uygulama bir AndroidManifest.xml yoksa tıklatın **Hayır AndroidManifest.xml bulundu. Bir eklemek için tıklatın** aşağıda gösterildiği gibi:

    [![No AndroidManifest.xml message](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Uygulamanız gereken tüm izinleri seçin **gerekli izinleri** listelemek ve kaydedin:

    [![Seçili örnek kamera izinleri](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'ya yerleşik araç desteği kullanarak izinlerini bildirme mümkündür:

1. Projede çift tıklatın **çözüm paneli** seçip **Seçenekleri > Yapı > Android uygulaması**:

    [![Gösterilen gerekli izinleri bölüm](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Tıklatın **Android derleme bildirimi ekleme** proje zaten yoksa düğmesini bir **AndroidManifest.xml**:

    [![Projenin Android derleme bildirimi eksik.](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Uygulamanız gereken tüm izinleri seçin **gerekli izinleri** listesinde ve tıklatın **Tamam**:

    [![Seçili örnek kamera izinleri](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android otomatik olarak bazı izinler derleme zamanında hata ayıklama yapıları ekleyeceksiniz. Bu, daha kolay uygulama hata ayıklama hale getirir. Özellikle, iki önemli izinlerdir `INTERNET` ve `READ_EXTERNAL_STORAGE`. Bu izinleri otomatik olarak ayarlama etkinleştirilmesi için görünmez **gerekli izinleri** listesi. Yayın derlemeler, Bununla birlikte, kesin olarak ayarlanmış olan izinleri kullan **gerekli izinleri** listesi. 

Android 5.1 (API düzeyi 22) veya alt hedef uygulamaları için hiçbir şey yoktur daha fazla yapılmalıdır. Android 6.0 (API 23 düzey 23) veya üzeri çalışacak uygulamaları çalıştırma izni denetler gerçekleştirme sonraki bölümüne açın devam etmelisiniz. 


### <a name="runtime-permission-checks-in-android-60"></a>Çalışma zamanı izni Android 6. 0'denetler

`ContextCompat.CheckSelfPermission` Yöntemi (Android destek kitaplığı ile de kullanılabilir), belirli bir izin verildi denetlemek için kullanılır. Bu yöntem döndürülecek bir [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) iki değerden birini olan enum:

* **`Permission.Granted`** &ndash; Belirtilen izin verildi.
* **`Permission.Denied`** &ndash; Belirtilen iznine sahip değil.

Bu kod parçacığında, bir etkinlik giderek kamera iznini denetlemek nasıl örneğidir: 

```csharp
if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.Camera) == (int)Permission.Granted) 
{
    // We have permission, go ahead and use the camera.
} 
else 
{
    // Camera permission is not granted. If necessary display rationale & request.
}
```

Kullanıcının izni bilinçli bir karar yapılan bir izin neden bir uygulama için gerekli olduğu konusunda bilgilendirmek için en iyi bir uygulamadır. Buna örnek olarak, fotoğraf ve coğrafi etiketleri geçen uygulama olacaktır bunları. Kameraya izin gereklidir, ancak uygulamanın ayrıca cihazın konumunu gerekir neden bu açık olmayabilecek kullanıcı işaretlenmemiştir. Stratejinin neden tercih ve kamera izni gereklidir konumunu izninizin olduğunu anlamasına yardımcı olmak için bir ileti görüntülenmelidir.

`ActivityCompat.ShouldShowRequestPermissionRational` Yöntemi stratejinin kullanıcıya gösterilmesi gerekip gerekmediğini belirlemek için kullanılır. Bu yöntem döndürülecek `true` verilen izni stratejinin yeniden görüntüleniyorsa. Bu ekran uygulama neden cihazın konumunu bilmek ister açıklayan bir uygulama tarafından görüntülenen Snackbar örneği gösterilmektedir:

![Stratejinin konumu](permissions-images/07-rationale-snackbar.png) 

Kullanıcı izin verirse `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` yöntemi çağrılabilir. Bu yöntem, aşağıdaki parametreleri gerektirir:

* **Etkinlik** &ndash; bu izinleri isteyen ve sonuçları Android tarafından haberdar olmak için etkinliktir.
* **izinleri** &ndash; istenecek izinlerin bir listesi.
* **requestCode** &ndash; izin isteği sonuçlarını eşleştirmek için kullanılan bir tamsayı değeri bir `RequestPermissions` çağırın. Bu değer sıfırdan büyük olmalıdır.

Bu kod parçacığını bahsedilen iki yöntemden birini örneğidir. İlk olarak, bir denetim izni stratejinin gösterilmesi gerekip gerekmediğini belirlemek için gerçekleştirilir. Gösterilecek stratejinin ise, bir Snackbar ile stratejinin görüntülenir. Kullanıcı tıklarsa **Tamam** Snackbar içinde uygulama izinleri isteyecek. Kullanıcı stratejinin kabul, sonra uygulama izinleri istemek için devam etmemelisiniz. Stratejinin gösterilmezse, etkinlik izni isteyin:

```csharp
if (ActivityCompat.ShouldShowRequestPermissionRationale(this, Manifest.Permission.AccessFineLocation)) 
{
    // Provide an additional rationale to the user if the permission was not granted
    // and the user would benefit from additional context for the use of the permission.
    // For example if the user has previously denied the permission.
    Log.Info(TAG, "Displaying camera permission rationale to provide additional context.");

    var requiredPermissions = new String[] { Manifest.Permission.AccessFineLocation };
    Snackbar.Make(layout, 
                   Resource.String.permission_location_rationale,
                   Snackbar.LengthIndefinite)
            .SetAction(Resource.String.ok, 
                       new Action<View>(delegate(View obj) {
                           ActivityCompat.RequestPermissions(this, requiredPermissions, REQUEST_LOCATION);
                       }    
            )
    ).Show();
}
else 
{
    ActivityCompat.RequestPermissions(this, new String[] { Manifest.Permission.Camera }, REQUEST_LOCATION);
}
```

`RequestPermission` Kullanıcı zaten izni olsa bile çağrılabilir. Sonraki çağrılar gerekli değildir, ancak kullanıcı izni onaylayın (veya iptal etmek için) fırsatı ile sağlarlar. Zaman `RequestPermission` olduğu olarak adlandırılan, denetimi devre dışı izinleri kabul etmek için bir kullanıcı Arabirimi görüntüler işletim sistemi karmalayan:  

![Permssion iletişim](permissions-images/08-location-permission-dialog.png)

Kullanıcı tamamlandıktan sonra Android sonuçları etkinliği için bir geri çağırma yöntemi döndürür `OnRequestPermissionResult`. Bu yöntem arabirimi bir parçası olan `ActivityCompat.IOnRequestPermissionsResultCallback` hangi gerekir uygulanan etkinlik tarafından. Bu arabirim tek bir yönteme sahip `OnRequestPermissionsResult`, hangi çağrılabilir kullanıcının seçimlerini etkinlik bildirmek için Android tarafından. Kullanıcı izni, ardından uygulama devam edin ve korumalı kaynağa kullanın. Nasıl uygulayacağınıza dair bir örnek `OnRequestPermissionResult` aşağıda gösterilmiştir: 

```csharp
public override void OnRequestPermissionsResult(int requestCode, string[] permissions, Permission[] grantResults)
{
    if (requestCode == REQUEST_LOCATION) 
    {
        // Received permission result for camera permission.
        Log.Info(TAG, "Received response for Location permission request.");

        // Check if the only required permission has been granted
        if ((grantResults.Length == 1) && (grantResults[0] == Permission.Granted)) {
            // Location permission has been granted, okay to retrieve the location of the device.
            Log.Info(TAG, "Location permission has now been granted.");
            Snackbar.Make(layout, Resource.String.permision_available_camera, Snackbar.LengthShort).Show();            
        } 
        else 
        {
            Log.Info(TAG, "Location permission was NOT granted.");
            Snackbar.Make(layout, Resource.String.permissions_not_granted, Snackbar.LengthShort).Show();
        }
    } 
    else 
    {
        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
}
```  


## <a name="summary"></a>Özet

Bu kılavuzda ele alınan ekleme ve bir Android cihaz izinlerini denetleyin. İzinlerin (API düzeyi < 23) eski Android uygulamaları ve yeni Android uygulamalar arasında nasıl çalıştığını fark (API düzeyi > 22). Android 6. 0'çalıştırma izni denetimleri gerçekleştirmek nasıl ele.


## <a name="related-links"></a>İlgili bağlantılar

- [Normal izinlerin listesi](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Çalışma zamanı izinleri örnek uygulaması](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin.Android izinleri işleme](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest)
