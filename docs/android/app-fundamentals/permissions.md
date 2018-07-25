---
title: Xamarin.Android izinleri
ms.prod: xamarin
ms.assetid: 3C440714-43E3-4D31-946F-CA59DAB303E8
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/09/2018
ms.openlocfilehash: 3e12aa47404d8ee4e52ddada3d99f91250e6c54d
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242270"
---
# <a name="permissions-in-xamarinandroid"></a>Xamarin.Android izinleri


## <a name="overview"></a>Genel Bakış

Android uygulamaları, kendi korumalı alanında çalıştırın ve güvenlik için nedeniyle bazı sistem kaynakları veya donanım cihazda erişiminiz yok. Bu kaynakları kullanabilir önce kullanıcıya açıkça uygulamaya izin vermesi gerekir. Örneğin, bir uygulama, kullanıcıdan açık izni olmadan herhangi bir cihazda GPS erişemez. Android oluşturur bir `Java.Lang.SecurityException` uygulama izni olmadan korunan bir kaynağa erişmeye çalışırsa.

İçinde bildirilen izinleri **AndroidManifest.xml** uygulaması geliştirdiğinizde, uygulama geliştiricisi tarafından. Android, bu izinler için kullanıcı onayı almak için iki farklı iş akışı vardır:
 
* Android 5.1 (API düzeyi 22) veya alt hedeflenen uygulamalar, uygulama yüklendiğinde izin isteği oluştu. Kullanıcı izinleri veremez, ardından uygulamayı yüklü. Uygulama yüklendikten sonra uygulamayı kaldırarak dışında izinleri iptal etmek için hiçbir yolu yoktur.
* Android 6.0 (API düzeyi 23) itibaren kullanıcılar hakkında daha fazla denetime izin verilen; verebilir veya uygulamasının cihazda yüklü olduğu sürece izinlerini iptal edin. Bu ekran görüntüsünde, Google Contacts uygulama için izin ayarları gösterilmektedir. Çeşitli izinleri listeler ve etkinleştirme veya devre dışı izin verir:

![Örnek izinleri ekranı](permissions-images/01-permissions-check.png) 

Android uygulamaları, korunan bir kaynağa erişim izni olup olmadığını görmek için zamanında denetlemeniz gerekir. Uygulama izni yoksa, izinleri vermek için yeni kullanıcı için Android SDK'sı tarafından sağlanan API'leri kullanarak istekleri yapmalısınız. İzinler iki kategoriye ayrılır:

* **Normal izinleri** &ndash; kullanıcının güvenlik veya gizlilik için çok az güvenlik riski izinler şunlardır. Android 6.0, yükleme sırasında otomatik olarak normal izin verecektir. Lütfen Android belgelerine bir [normal izinlerle tam listesi](https://developer.android.com/guide/topics/permissions/normal-permissions.html).
* **Tehlikeli izinler** &ndash; normal aksine, tehlikeli izinler bu kullanıcının güvenlik veya gizlilik korumak izinlerdir. Bu özel verilen kullanıcı tarafından olmalıdır. Gönderme veya SMS iletisi alma tehlikeli izni gerektiren bir eylem örneğidir.

> [!IMPORTANT]
> Bir izne ait olduğu kategoriyi zaman içinde değişebilir.  Bu, "normal" izni olabileceğinden, kategoriye ayrıldığıyla izin tehlikeli izni gelecekte API düzeylerini yükseltilmiş mümkündür.

Tehlikeli izinler daha alt bölünür içine [ _izin gruplarına_](https://developer.android.com/guide/topics/permissions/requesting.html#perm-groups). Bir izin grubuna mantıksal olarak ilgili izinleri tutar. Kullanıcının izni grubunun bir üyesi izni veriyorsa, Android, bu grubun tüm üyelerine otomatik olarak izin verir. Örneğin, [ `STORAGE` ](https://developer.android.com/reference/android/Manifest.permission_group.html#STORAGE) izin grubu hem de tutan `WRITE_EXTERNAL_STORAGE` ve `READ_EXTERNAL_STORAGE` izinleri. Kullanıcı izin verirse `READ_EXTERNAL_STORAGE`, ardından `WRITE_EXTERNAL_STORAGE` aynı anda izin verilen otomatik olarak.

Bir veya daha fazla izinleri istemeden önce neden uygulama izni istemeden önce izni gerekiyor. farklı bir stratejinin sağlamak için bir en iyi yöntemdir. Kullanıcı stratejinin anlayan sonra uygulama kullanıcıdan izin isteyebilir. İzni verin ve aksi takdirde varsa anlamak istiyorsanız stratejinin anlayarak, kullanıcı bir konusunda bilinçli karar verebilirsiniz. 

Denetleme ve izinleri isteyen tüm iş akışı olarak bilinen bir _çalışma zamanı izinleri_ denetleyin ve aşağıdaki diyagramda özetlenebilir: 

[![Çalışma zamanı izni onay akış çizelgesi](permissions-images/02-permissions-workflow-sml.png)](permissions-images/02-permissions-workflow.png#lightbox)

Android desteği kitaplığı backports bazı izinler Android eski sürümleri için yeni API'leri. Bir API düzey denetimi her zaman gerçekleştirmek gerekli değildir bu backported API'leri otomatik olarak cihazdaki Android sürümünü denetleyin.  

Bu belge, bir Xamarin.Android uygulamasına izinleri ekleme işlemi ele alınmaktadır ve nasıl Android 6.0 (API düzeyi 23) hedefleyen uygulamaları ya da daha yüksek bir çalışma zamanı izni denetimi gerçekleştirmeniz gerekir.


> [!NOTE]
> Bu uygulamayı Google Play tarafından nasıl filtre donanım izinlerini etkileyebilir mümkündür. Uygulama izni için kamera gerektiriyorsa, örneğin, ardından Google Play uygulaması yüklü bir kamera sahip olmayan bir cihazda Google Play Store görünmez.


<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Xamarin.Android projeleri içerdiğini önemle tavsiye edilir [Xamarin.Android.Support.Compat](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/) NuGet paketi. Android sağlayan bir ortak, eski sürümleri, özel API'ların gerek olmadan sürekli arabirimi bu paket backport izin uygulamayı çalıştıran Android sürümünü denetleyin.


## <a name="requesting-system-permissions"></a>Sistem izinleri isteyen

Android izinleriyle birlikte çalışma ilk adımı, izinler Android bildirim dosyası bildirmektir. Bu uygulamayı hedeflediği API düzeyi bağımsız olarak yapılmalıdır.

Hedef Android 6.0 veya kullanıcı belirli bir noktada geçmişte olduğunda ve izin sonraki geçerli olacağını izni olduğundan daha yüksek olan varsayamazsınız uygulamalar. Android 6. 0'ı hedefleyen bir uygulama her zaman bir çalışma zamanı izni denetimi gerçekleştirmeniz gerekir. Android 5.1 veya alt hedefleyen uygulamalar, bir çalışma zamanı izni denetimi gerçekleştirmek gerekmez.

> [!NOTE]
> Uygulamalar, yalnızca ihtiyaç duydukları izinleri istemeniz gerekir.


### <a name="declaring-permissions-in-the-manifest"></a>İzinleri bildiriminde bildirme

İzinler eklenir **AndroidManifest.xml** ile `uses-permission` öğesi. Örneğin, bir uygulama, cihazın konumunu bulmak için ise ince gerektirir ve yeri izinleri kursu. Aşağıdaki iki öğeyi bildirimine eklenir: 

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da yerleşik olarak bulunan araç desteği kullanarak izinleri bildirmek mümkündür:

1. Çift **özellikleri** içinde **Çözüm Gezgini** seçip **Android bildirim** Özellikler penceresinde sekme:

    [![Android bildirim sekmesindeki gerekli izinler](permissions-images/04-required-permissions-vs-sml.png)](permissions-images/04-required-permissions-vs.png#lightbox)

2. Uygulama bir AndroidManifest.xml yoksa tıklayın **Hayır AndroidManifest.xml bulunamadı. Eklemek için tıklayın** aşağıda gösterildiği gibi:

    [![AndroidManifest.xml ileti yok](permissions-images/05-no-manifest-vs-sml.png)](permissions-images/05-no-manifest-vs.png#lightbox)

3. Uygulamanızın ihtiyaçlarını herhangi bir izni seçin **gerekli izinler** listelemek ve kaydedin:

    [![Seçili örneğin kamera izinleri](permissions-images/06-selected-permission-vs-sml.png)](permissions-images/06-selected-permission-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio ile oluşturulmuş araç desteği kullanarak izinleri bildirmek mümkündür:

1. Projede çift **çözüm bölmesi** seçip **Seçenekleri > derleme > Android uygulaması**:

    [![Gerekli izinler bölümünde gösterilen](permissions-images/04-required-permissions-xs-sml.png)](permissions-images/04-required-permissions-xs.png#lightbox)

2. Tıklayın **Android bildirimi Ekle** proje zaten yoksa düğmesine bir **AndroidManifest.xml**:

    [![Projenin Android bildirimi eksik](permissions-images/05-no-manifest-xs-sml.png)](permissions-images/05-no-manifest-xs.png#lightbox)

3. Uygulamanızın ihtiyaçlarını herhangi bir izni seçin **gerekli izinler** listelemek ve tıklayın **Tamam**:

    [![Seçili örneğin kamera izinleri](permissions-images/03-select-permission-xs-sml.png)](permissions-images/03-select-permission-xs.png#lightbox)
    
-----

Xamarin.Android otomatik olarak bazı izinler oluşturma zamanında hata ayıklama yapıları için ekler. Bu, daha kolay uygulama hata ayıklama hale getirir. Özellikle, iki önemli izinlerdir `INTERNET` ve `READ_EXTERNAL_STORAGE`. Bu izinleri otomatik olarak ayarlama etkinleştirilmesi için görünmez **gerekli izinler** listesi. Yayın oluşturur, ancak, açıkça ayarlanmış olan izinlere kullanın **gerekli izinler** listesi. 

Android 5.1 (API düzeyi 22) veya alt hedefleyen uygulamalar için hiçbir şey yoktur daha fazla yapılmalıdır. (API 23 düzeyi 23) Android 6.0 veya üzeri çalıştıran uygulamaları açın çalıştırma izni denetimleri gerçekleştirmek için sonraki bölüme devam etmelisiniz. 


### <a name="runtime-permission-checks-in-android-60"></a>Android 6. 0'çalışma zamanı iznini denetler

`ContextCompat.CheckSelfPermission` Yöntemi (Android desteği kitaplığı ile kullanılabilir), belirli bir izin verilip verilmediğine denetlemek için kullanılır. Bu metodun döndüreceği bir [ `Android.Content.PM.Permission` ](https://developer.xamarin.com/api/type/Android.Content.PM.Permission/) iki değerden birine sahip bir sabit listesi:

* **`Permission.Granted`** &ndash; Belirtilen izin verildi.
* **`Permission.Denied`** &ndash; Belirtilen izin verilmedi.

Bu kod parçacığı, bir etkinlik kamera izin olup olmadığını denetlemek nasıl bir örneğidir: 

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

İzin vermek için bilinçli bir karar hale getirilebilir, böylece dair bir iznin neden bir uygulama için gerekli olduğunu kullanıcıya bildirmek için iyi bir uygulamadır. Buna örnek olarak, fotoğraf ve coğrafi etiketleri alan bir uygulama olacaktır bunları. Kamera izni gereklidir, ancak uygulamanın cihazın konumunu da gerekir. Bu nedenle, açık olmayabilecek kullanıcı işaretlenmemiştir. Bu stratejinin neden konum izni tercih ve kamera izni gerekli olduğunu anlamak kullanıcının yardımcı olmak için bir ileti görüntülenmelidir.

`ActivityCompat.ShouldShowRequestPermissionRational` Yöntemi stratejinin kullanıcıya gösterilmesi gerekip gerekmediğini belirlemek için kullanılır. Bu metodun döndüreceği `true` belirli bir izni stratejinin görüntülenmesi gerekiyorsa. Bu ekran görüntüsünde uygulama neden cihazın konumunu bilmek ister açıklayan bir uygulama tarafından görüntülenen bir Snackbar örneği gösterilmektedir:

![Konumu için stratejinin](permissions-images/07-rationale-snackbar.png) 

Kullanıcı izin verirse `ActivityCompat.RequestPermissions(Activity activity, string[] permissions, int requestCode)` yöntemi çağrılabilir. Bu yöntem için aşağıdaki parametreler gereklidir:

* **Etkinlik** &ndash; bu izinleri isteme ve sonuçları bir Android tarafından bilgilendirilmesi etkinliktir.
* **izinleri** &ndash; istenecek izinlerin bir listesi.
* **requestCode** &ndash; izin isteği sonuçlarını eşleştirmek için kullanılan bir tamsayı değeri bir `RequestPermissions` çağırın. Bu değer, sıfırdan büyük olmalıdır.

Bu kod parçacığı, ele alınan iki yöntemden biriyle bir örnektir. İlk olarak, izin stratejinin gösterilmesi gerekip gerekmediğini belirlemek için bir onay yapılır. Ardından stratejinin gösterilecek ise, bir Snackbar stratejinin ile görüntülenir. Kullanıcı tıklarsa **Tamam** Snackbar içinde uygulama izinleri isteyecek. Kullanıcı stratejinin kabul, sonra uygulama izinleri istemek için devam etmemelisiniz. Stratejinin gösterilmezse, etkinlik izin isteği:

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

`RequestPermission` Kullanıcı zaten izin vermiş olsanız bile çağrılabilir. Sonraki çağrılar gerekli değildir, ancak kullanıcının iznini doğrulamak (veya iptal etmek için) fırsatı ile sağlarlar. Zaman `RequestPermission` olan çağrılır, denetimi devre dışı izinlerini kabul etmek için bir kullanıcı Arabirimi görüntüler işletim sistemine devredildiği:  

![Permssion iletişim](permissions-images/08-location-permission-dialog.png)

Kullanıcı tamamlandıktan sonra Android sonuçlar etkinliği için bir geri çağırma yöntemi döndürür `OnRequestPermissionResult`. Bu yöntem bir arabirim parçasıdır `ActivityCompat.IOnRequestPermissionsResultCallback` hangi uygulanmalı etkinlik tarafından. Bu arabirim tek bir yöntemi olan `OnRequestPermissionsResult`, hangi çağrılacak kullanıcının seçeneğiniz etkinlik bildirmek için Android tarafından. Kullanıcının izni vermiş, ardından uygulama devam edin ve korumalı kaynağı kullanın. Nasıl uygulayacağınıza dair örnek `OnRequestPermissionResult` aşağıda gösterilmiştir: 

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

Bu kılavuzda ele alınan ekleme ve bir Android cihaz izinleri denetleyin. İzinlerin eski Android apps (API düzey 23 <) ve yeni Android uygulamalar arasında nasıl çalıştığını fark (API düzey 22 >). Bu Android 6. 0'çalışma zamanı izni denetimleri gerçekleştirmek açıklanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Normal izinlerle listesi](https://developer.android.com/guide/topics/permissions/normal-permissions.html)
- [Çalışma zamanı izinleri örnek uygulaması](https://github.com/xamarin/monodroid-samples/tree/master/android-m/RuntimePermissions)
- [Xamarin.Android izinleri işleme](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)
