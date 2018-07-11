---
title: Oreo özellikleri
description: En son sürümünü Android uygulamaları geliştirmek için Xamarin.Android kullanmaya başlamak nasıl.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 07/06/2018
ms.openlocfilehash: af560848240fec9558cc63969bcc269eedbd5424
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947292"
---
# <a name="oreo-features"></a>Oreo özellikleri

_En son sürümünü Android uygulamaları geliştirmek için Xamarin.Android kullanmaya başlamak nasıl._

[Android 8.0 Oreo](https://developer.android.com/index.html) Google'nın en son Android sürümünü kullanılabilir. Android Oreo, Xamarin.Android geliştiricilerine birçok yeni ilgi çekici özellik sunar. Bu özellikler, XML, indirilebilir yazı tipleri, otomatik doldurma ve picture in picture (PIP), bildirim kanalları, bildirim rozetleri, özel yazı tipleri içerir. Android Oreo için bu yeni capabilties yeni API'ler de dahildir ve bu API'leri Xamarin.Android 8.0 kullandığınızda Xamarin.Android uygulamaları için kullanılabilir ve üzeri.

[![Android Oreo hero görüntüsü](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Bu makalede, Android 8.0 Oreo için Xamarin.Android uygulamalar geliştirmeye başlamanıza yardımcı olmak için yapılandırılmıştır. Bunu test etmek için bir öykünücü (veya cihaz) oluşturmak gerekli güncelleştirmeleri yükleyin ve SDK'sını yapılandırmak nasıl açıklar. Ayrıca Android Oreo özelliklerinin Xamarin.Android uygulamalarında nasıl kullanılacağını gösteren örnek uygulamalar ile Android 8.0 Oreo'daki yeni özelliklerin bir özetini sağlar.


## <a name="requirements"></a>Gereksinimler

Aşağıdaki Android Oreo özelliklerini Xamarin tabanlı uygulamalarında kullanmak için gereklidir:

-   **Visual Studio** &ndash; Windows kullanıyorsanız, 15.5 veya Visual Studio'nun sonraki bir sürümü gereklidir.  Mac kullanıyorsanız, sürüm 7.2.0 Mac için Visual Studio gerekli değildir.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 veya üzeri yüklü ve Visual Studio ile yapılandırılmış.

-   **Android SDK'sı** &ndash; Android SDK 8.0 (API 26) veya daha sonra Android SDK Yöneticisi aracılığıyla yüklenmesi gerekir.



## <a name="getting-started"></a>Başlarken

Android Oreo, Xamarin.Android ile kullanmaya başlamak için indirin ve bir Android Oreo proje oluşturmadan önce en son araç ve SDK paketlerini yükleyin:

1. Visual Studio'nun en son sürüme güncelleştirin.

2. Yükleme **Android 8.0.0 (API 26)** veya üzeri paketler ve araçlar aracılığıyla SDK Yöneticisi.

3. Android Oreo (API 26) hedefleyen yeni bir Xamarin.Android projesi oluşturun.

4. Bir öykünücü veya cihazın Android Oreo uygulamalarını test etme için yapılandırın.

Bu adımların her biri, aşağıdaki bölümlerde açıklanmıştır:



### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio ve Xamarin.Android güncelleştirmesi

Visual Studio'da Android Oreo desteği eklemek için aşağıdakileri yapın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Visual Studio 2017 kullanıyorsanız: 

    1. Visual Studio 2017 sürüm 15.7 veya üzeri için güncelleştirme (bkz [Visual Studio 2017 güncelleştirme](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. Kullanım [SDK Yöneticisi](~/android/get-started/installation/android-sdk.md) API düzeyi 26,0 veya sonraki sürümünü yüklemek için.

-   Visual Studio 2015, biz 25 SDK Tools önceki sürüme indirme ve kullanma öneririz kullanıyorsanız eski Google öykünücü yöneticisi GUI. SDK Araçları 25 API 26 yanı sıra, 27 ve daha yeni sürümü, hala kullanılabilir ve yeni platformlar için geliştirme etkilemez. Önceki VS sürümleri için Android SDK'nızı yönetmek için bu, bir arabirim sağlar.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   İçinde açıklandığı gibi Visual Studio 2017'yi en son kararlı sürüme Mac için güncelleştirme [Mac için Visual Studio güncelleştirme](https://docs.microsoft.com/visualstudio/mac/update).

-----

Xamarin Android Oreo desteği hakkında daha fazla bilgi için bkz. [Xamarin.Android 8.0 sürüm notları](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Android SDK'sını yükleyin

Xamarin.Android 8.0 ile bir proje oluşturmak için önce Xamarin Android SDK Yöneticisi'ni yüklemek için SDK platform için kullanmanız gerekir **Android 8.0 - Oreo** veya üzeri. Ayrıca Android SDK Tools 26,0 veya sonraki bir sürümü yüklemeniz gerekir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. SDK Yöneticisi'ni başlatın (Visual Studio'da **Araçlar > Android > Android SDK Yöneticisi**).

2. Yükleme **Android 8.0 - Oreo** paketleri. Android SDK öykünücüsü kullanıyorsanız, eklediğinizden emin olun **x86** sistemi görüntülerini ihtiyacınız olacak:

    [![Android SDK Yöneticisi'nde Android 8.0 paketleri seçme](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Yükleme **Android SDK Tools 26.0.2** ya da sonraki **Android SDK platformunuzun Araçlar 26.0.0** veya sonraki bir sürümü ve **Android SDK derleme araçları 26.0.0** (veya üstü):

    [![Android SDK Tools 26 Android SDK Yöneticisi'nde seçme](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. SDK Yöneticisi'ni başlatın (Mac için Visual Studio'da **Araçlar > SDK Yöneticisi**).

2. Yükleme **Android 8.0 - Oreo** SDK paketleri. Android SDK öykünücüsü kullanıyorsanız, eklediğinizden emin olun **x86** sistemi görüntülerini ihtiyacınız olacak:

    [![SDK Yöneticisi'nde Android 8.0 paketleri seçme](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Yükleme **Android SDK Tools 26.0.2** ya da sonraki **Android SDK platformunuzun Araçlar 26.0.0** veya sonraki bir sürümü ve **Android SDK derleme araçları 26.0.0** (veya üstü):

    [![Android SDK Tools 26 SDK Yöneticisi'nde seçme](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Bir Xamarin.Android projesi Başlat

Yeni bir Xamarin.Android projesi oluşturun. Xamarin ile Android geliştirmesi için yeni başlıyorsanız bkz [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android projeleri oluşturma hakkında bilgi edinmek için.

Bir Android projesi oluşturduğunuzda, hedef Android 8.0 veya üzeri sürüm ayarlarını yapılandırmanız gerekir. Örneğin, projeniz için Android 8.0 hedeflemek için hedef Android API düzeyini projenize yapılandırmalısınız **Android 8.0 (API 26)**. API 26 ya da daha sonra hedef çerçeve düzeyi de ayarlamanız önerilir. Android API düzeyi düzeylerini yapılandırma hakkında daha fazla bilgi için bkz. [anlama Android API düzeyleri](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Bir öykünücü veya cihaz yapılandırma

Android SDK Tools 26.0 yükledikten sonra Google GUI tabanlı AVD Manager varsayılan başlatma girişimi ya da daha sonra komut satırı AVD manager aracını yönlendiren aşağıdaki hata iletişim kutusu alabilirsiniz **avdmanager** yerine :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android Emulator Manager Uyarısı iletişim kutusu](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Android Emulator Manager Uyarısı iletişim kutusu](oreo-images/mac/03-avd-warning.png)

-----

Google API 26,0 ve sonraki sürümleri destekler GUI AVD manager bir değişikliğine artık sağladığından, bu ileti görüntülenir. Android 8.0 Oreo için Xamarin Android öykünücü yöneticisi veya komut satırı kullanmalısınız `avdmanager` Android Oreo için sanal cihaz oluşturmak için aracı.

Sanal cihazları oluşturmak ve yönetmek için Android cihaz Yöneticisi'ni kullanmak için bkz: [yönetme sanal cihazların Android cihaz Yöneticisi'yle](~/android/get-started/installation/android-emulator/device-manager.md).
Sanal cihazlar olmadan Android cihaz Yöneticisi'ni oluşturmak için sonraki bölümde yer alan adımları izleyin.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Sanal cihazları kullanarak avdmanager oluşturma

Kullanılacak **avdmanager** yeni bir sanal cihaz oluşturmak için aşağıdaki adımları izleyin:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Bir komut istemi penceresi açın ve ayarlayın `JAVA_HOME` Java SDK'sı, bilgisayarınızdaki konumunu. Tipik bir Xamarin yüklemesi için aşağıdaki komutu kullanabilirsiniz:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Android SDK konumu Ekle `bin` klasöre, `PATH`.
    Tipik bir Xamarin yüklemesi için aşağıdaki komutu kullanabilirsiniz:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Komut İstemi penceresini kapatın ve yeni bir komut istemi penceresi açın. Kullanarak yeni bir sanal cihaz oluşturma [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) komutu. Örneğin, adında bir AVD oluşturmak için **AVD Oreo 8.0** x86 kullanarak sistem görüntüsü API düzeyi 26, için aşağıdaki komutu kullanın:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Sorulduğunda ile **[no] özel donanım profilini oluşturmak istiyor musunuz** girebilirsiniz **hiçbir** ve varsayılan donanım profili kabul edin. Diyelim ki **Evet**, **avdmanager** donanım profili özelleştirmeye yönelik sorular listesiyle ister.

Çalıştırdıktan sonra **avdmanager** , sanal cihaz oluşturmak için cihaz aşağı açılır menüden dahil edilir:

[![Cihaz aşağı açılır menüsüne eklenen yeni AVD](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Açık bir **Terminal** penceresi ve mac'inizde Android SDK Araçları dizin konumunu değiştir Tipik bir Xamarin yüklemesi için aşağıdaki komutu kullanabilirsiniz:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Kullanarak yeni bir sanal cihaz oluşturma [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) komutu. Örneğin, adında bir AVD oluşturmak için **AVD Oreo 8.0** x86 kullanarak sistem görüntüsü API düzeyi 26, için aşağıdaki komutu kullanın:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Sorulduğunda ile **[no] özel donanım profilini oluşturmak istiyor musunuz** girebilirsiniz **hiçbir** ve varsayılan donanım profili kabul edin. Diyelim ki **Evet**, **avdmanager** özelleştirmeye yönelik sorular listesiyle ister donanım profili.

Kullandıktan sonra **avdmanager** , sanal cihaz oluşturmak için cihaz aşağı açılır menüden dahil edilir:

[![Cihaz aşağı açılır menüsüne eklenen yeni AVD](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Android öykünücüsünde test ve hata ayıklama için yapılandırma hakkında daha fazla bilgi için bkz. [Android öykünücü üzerinde hata ayıklama](~/android/deploy-test/debugging/debug-on-emulator.md).

Bir Nexus veya bir piksel gibi fiziksel bir cihaz kullanıyorsanız, ya da (OTA) havadan Güncelleştirmeler üzerinden otomatik Cihazınızı güncelleştirin veya sistem görüntüsü indirebilir ve Cihazınızı doğrudan flash. Cihazınız Android Oreo için el ile güncelleştirme hakkında daha fazla bilgi için bkz. [Nexus ve piksel cihazlar için Fabrika görüntüleri](https://developers.google.com/android/images).



## <a name="new-features"></a>Yeni Özellikler

Android Oreo çeşitli yeni özellikler ve bildirim kanalları, bildirim göstergeler, XML'de özel yazı tipleri, indirilebilir yazı tipleri, otomatik doldurma ve resim resim gibi özellikler sunar. Aşağıdaki bölümlerde, bu özellikleri vurgulayın ve bağlantıları yardımcı olmak için bunları uygulamanızda kullanmaya başlama sağlayın.



### <a name="notification-channels"></a>Bildirim kanalları

*Bildirim kanallarını* bildirimleri için uygulama tanımlı kategoriler.
Seçenekler, uygulamanızın kullanıcıları tarafından yapılan yansıtacak şekilde bildirim kanalları oluşturabilir ve bildirim göndermek için gereken her tür için bir bildirim kanalını oluşturabilirsiniz. Yeni bildirim kanalları özellik bildirimleri farklı türde kullanıcılar ayrıntılı denetiminin kendilerinde olmasına yapar. Örneğin, bir Mesajlaşma uygulaması uyguluyorsanız, bir kullanıcı tarafından oluşturulan her konuşma grubu için ayrı bildirim kanallarını oluşturabilirsiniz.

[Bildirim kanallarını](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) bir bildirim kanalı oluşturmak ve yerel bildirimleri göndermek için kullanma açıklanmaktadır. Gerçek kod örneği için bkz: [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) örnek; bu örnek uygulama iki kanallar yönetir ve ek bildirim seçeneklerini ayarlar.



### <a name="notification-badges"></a>Bildirim rozetleri

Bildirim rozetleri, bu ekran görüntüsünde gösterildiği gibi uygulama simgeleri üzerinde görünen küçük noktalar şunlardır:

[![Uygulama simgeleri üzerinde örnek bildirim rozetleri](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Bu nokta, bir veya daha fazla bildirim kanallarını, uygulama simgesi ile ilişkili uygulama için yeni bildirimleri belirtmek &ndash; bu kullanıcı henüz kapatıldı veya izlemede bildirimlerdir. Kullanıcılar uzun-bir simge üzerinde bir bildirim rozet ile ilişkili bildirimler, kapatma veya uzun basın menüsünden bildirimleri bu appeaars gören genel bakış için basın.

Android Geliştirici bildirim rozetleri hakkında daha fazla bilgi için bkz. [bildirim rozetleri](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) konu.



### <a name="custom-fonts-in-xml"></a>XML özel yazı tipleri

Android Oreo tanıtır *XML yazı tiplerini*, hangi mümkün kılar, özel yazı tipleri kaynakları olarak dahil edilip derecelendirilir. OpenType (**.otf**) ve TrueType (**.ttf**) yazı tipi biçimleri desteklenir. Kaynaklar olarak yazı tiplerini eklemek için aşağıdakileri yapın:

1. Oluşturma bir **kaynakları/yazı tipi** klasör.

2. Yazı tipi dosyaları kopyalayın (örneğin, **.ttf** ve **.otf** dosyaları) için **kaynakları/yazı tipi**. 

3. Android dosya adlandırma kuralları için uyar gerekirse her yazı tipi dosyası yeniden adlandırın (örneğin, yalnızca küçük harf kullanın *a-z*, *0-9*ve dosya adları alt çizgi). Örneğin, yazı tipi dosyası `Pacifico-Regular.ttf` gibi bir şekilde adlandırılması `pacifico.ttf`.

4. Yeni'ı kullanarak özel yazı tipi uygulamak `android:fontFamily` düzeninizi XML özniteliği. Örneğin, aşağıdaki `TextView` bildirimi kullanıp eklenen **pacifico.ttf** yazı tipi kaynak:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Ayrıca, stilini ve ağırlığı ayrıntıları yanı sıra birden çok yazı tipleri açıklayan bir yazı tipi ailesi XML dosyası da oluşturabilirsiniz. Daha fazla bilgi için bkz: Android Geliştirici [XML yazı tiplerini](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) konu.


### <a name="downloadable-fonts"></a>İndirilebilir yazı tipleri

Android Oreo ile başlayarak, uygulamalar, sağlayıcı yerine, bunları APK paketleme yazı tipleri isteyebilir. Yazı tipleri ağdan yalnızca gerektiğinde indirilir. Bu özellik, telefon bellek ve hücresel veri kullanımını koruma APK boyutunu azaltır. Ayrıca, Android desteği kitaplığına 26 paketini yükleyerek bu özellik Android API 14 ve üzeri sürümlerinde kullanabilirsiniz.

Uygulamanızı bir yazı tipi gerektiğinde, oluşturduğunuz bir `FontsRequest` nesne (indirmek için yazı tipi belirtme) ve ardından geçirin bir `FontsContract` yazı tipini karşıdan yüklemek için yöntemi. Aşağıdaki adımlarda, yazı tipi indirme işlemini daha ayrıntılı açıklanmıştır:

1.  Örneği bir [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) nesne. 

2.  Alt sınıf ve örnek oluşturma [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Uygulama [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) yazı tipi isteğin tamamlandığını işlemek için kullanılan yöntem.

4.  Uygulama [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) uygulamanıza yazı tipi isteği işlemi sırasında gerçekleşmesi hataların bilgilendirmek için kullanılan yöntem.

5.  Çağrı [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) yazı tipini yazı tipi sağlayıcıdan almak için yöntemi. 

Çağırdığınızda `RequestFonts` yöntemi, ilk denetler yazı yerel olarak önbelleğe alınmış görmek için (bir önceki çağrıya `RequestFont`). Alınmamışsa, yazı tipi sağlayıcısı çağıran, zaman uyumsuz olarak yazı tipini alır ve sonuçları çağırarak sonra uygulamanıza geri geçirir, `OnTypeFaceRetrieved` yöntemi.

[İndirilebilir yazı tipleri](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) örnek Android Oreo'da sunulan indirilebilir yazı tipleri özelliğinin nasıl kullanılacağı gösterilmektedir. 

Android Geliştirici yazı tipleri yükleme hakkında daha fazla bilgi için bkz. [indirilebilir yazı tipleri](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) konu.



### <a name="autofill"></a>Otomatik doldurma

Yeni _otomatik doldurma_ Android Oreo framework kullanıcıların oturum açma, hesap oluşturma ve kredi kartı işlemleri gibi yinelenen görevler işlemek kolaylaştırır. Kullanıcılar, (Bu hataları girişi için yol açabilir) bilgileri yeniden yazma daha az zaman harcayın. Uygulamanızı Autofill Framework ile çalışmadan önce sistem ayarlarını (kullanıcıları etkinleştirebilir veya otomatik doldurmayı devre dışı) bir otomatik doldurma hizmetinin etkinleştirilmesi gerekir.

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) örnek Autofill Framework kullanımını gösterir. İstemci etkinlikleri autofilled ve etkinlikleri istemciye otomatik doldurma veri sağlayan bir hizmet olmalıdır görünümlere sahip uygulamaları içerir.

Yeni otomatik doldurma özelliğinin ve uygulamanız otomatik doldurma için en iyi duruma getirme hakkında daha fazla bilgi için bkz: Android Geliştirici [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html) konu.



### <a name="picture-in-picture-pip"></a>Resmin uyuyabilir (PIP)

Android Oreo başka bir etkinlik ekranın üst üste yerleştiren resim resim (PIP) modunda başlatmak bir etkinliği sağlar. Bu özellik, video kayıttan yürütme için tasarlanmıştır.

Uygulamanızın etkinlik PIP modunu kullandığını belirtmek için true olarak Android bildirimine aşağıdaki bayrağı ayarlayın:

```xml
android:supportsPictureInPicture
```

PIP modunda olduğunda etkinliğinizi nasıl davranacağı belirtmek için yeni kullandığınız [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) nesne. `PictureInPictureParams` başlatma ve PIP modda (örneğin, etkinliğin tercih edilen en boy oranı) bir etkinliği güncelleştirmek için kullandığınız parametreleri kümesini temsil eder. Aşağıdaki yeni PIP yöntemleri eklenen `Activity` Android Oreo içinde:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; etkinlik PIP moduna yerleştirir. Ekranın üst köşesinde bulunan etkinlik yerleştirilir ve ekranın rest ekranda olan önceki etkinliğin doldurulur.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; etkinliğin PIP yapılandırma ayarlarını (örneğin, bir değişiklik en boy oranı) güncelleştirir.

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) örnek Oreo'da sunulan taşınabilir cihazları için resim, resim (PIP) mod temel kullanımını gösterir. Örnek, görüntü modları veya diğer etkinlikler arasında ileri ve geri geçiş sırasında hangi devam video kesintisiz yürütür.



### <a name="other-features"></a>Diğer özellikler

Android Oreo içeren diğer birçok Emoji destek kitaplığı, konum API arka plan sınırları gibi yeni özellikler, uygulamalar, yeni ses codec bileşenleri, WebView geliştirmeleri, geliştirilmiş klavye gezintisi desteği ve yeni bir AAudio (pro ses) API için geniş gam rengi yüksek performanslı düşük gecikme süreli ses, bu özellikler hakkında daha fazla bilgi için bkz: Android Geliştirici [Android Oreo özellikleri ve API'leri](https://developer.android.com/about/versions/oreo/android-8.0.html) konu.



## <a name="behavior-changes"></a>Davranış değişiklikleri

Android Oreo çeşitli sistem ve mevcut uygulamaların işlevselliğini üzerinde bir etkisi olan API davranış değişiklikleri içerir. Bu değişiklikler aşağıda açıklanmıştır.


### <a name="background-execution-limits"></a>Arka plan yürütme sınırları

Kullanıcı deneyimini geliştirmek için Android Oreo uygulamaları neler yapabileceğinizi sınırlamalar getirir arka planda çalışırken. Örneğin, kullanıcı bir videoyu izlemeyi veya oyun oynamak, arka planda çalışan bir uygulamanın ön planda çalışan bir video yoğun uygulama performansını zarar. Sonuç olarak, Android Oreo kullanıcıyla doğrudan etkileşim olmayan uygulamaları aşağıdaki kısıtlamaları getirir:

1.  **Arka plan, hizmet sınırlamaları** &ndash; arka planda bir uygulamayı çalıştırırken, birkaç dakika, yine de izin verilir oluşturup hizmetlerini kullanmak için bir pencere vardır. Android o pencereyi sonunda, uygulamanın arka plan hizmetini durdurur ve olarak davranır _boşta_.

2.  **Sınırlamalar yayın** &ndash; Android 7.0 (API 25) almak için uygulamayı kaydeder ve yayın üzerinde sınırlamalar yerleştirilir. Android Oreo bu sınırlamaları daha katı yapar. Örneğin, Android Oreo uygulamaları kendi Bildirimlerde artık örtük yayınlar için yayın alıcıları kaydedebilirsiniz.

Yeni arka plan yürütme sınırları hakkında daha fazla bilgi için bkz: Android Geliştirici [arka plan yürütme sınırları](https://developer.android.com/about/versions/oreo/background.html) konu.


### <a name="breaking-changes"></a>Yeni Değişiklikler

Android Oreo hedef ya da uygun olduğunda aşağıdaki değişiklikleri desteklemek için uygulamalarını yüksek değiştirmeniz gerekir uygulamalar:

- Android Oreo bireysel bildirimleri önceliğini ayarlama özelliğini kaldırır. Bunun yerine, önerilen önem düzeyini bir bildirim kanalı oluşturulurken ayarlayın. Bir bildirim kanalı için atadığınız önem düzeyini üzere gönderdiğiniz tüm bildirim iletileri için geçerlidir.

- Android Oreo hedefleyen uygulamalar için `PendingIntent.GetService()` arka planda çalışmaya Hizmetleri yerleştirildiği yeni sınırları nedeniyle çalışmıyor. Android Oreo hedefliyorsanız, kullanmanız gereken [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) yerine.  


## <a name="sample-code"></a>Örnek kod

Android Oreo özelliklerden yararlanmak nasıl göstermek çeşitli Xamarin.Android örnekleri kullanılabilir:

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) sunulan Android Oreo'da yeni bildirim kanallarını sistem nasıl yapılacağı açıklanır. Bu örnek iki bildirim kanalları yönetir: bir varsayılan önem ve diğer yüksek önem düzeyine sahip.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo'da sunulan taşınabilir cihazları için resim, resim (PIP) mod temel kullanımını gösterir. Örnek, görüntü modları veya diğer etkinlikler arasında ileri ve geri geçiş sırasında hangi devam video kesintisiz yürütür.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) Autofill Framework kullanımını gösterir. İstemci etkinlikleri autofilled ve etkinlikleri istemciye otomatik doldurma veri sağlayan bir hizmet olmalıdır görünümlere sahip uygulamaları içerir.

-   [İndirilebilir yazı tipleri](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) daha önce açıklanan indirilebilir yazı tipi özelliğini nasıl kullanacağınızı gösteren bir örnek sağlar.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) EmojiCompat destek kitaplığı kullanımını gösterir. Bu kitaplık gösteren eksik emoji karakterleri "tofu" karakter olarak uygulamanızdan önlemek için kullanabilirsiniz.

-   [Konum güncelleştirmeleri Pending Intent](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) cihaz konumu hakkında güncelleştirmeler almak için bu konumu API kullanımını kullanarak bir `PendingIntent`.

-   [Konum güncelleştirmeleri ön plan hizmeti](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) ilişkili ve başlatılan ön plan hizmetini kullanarak cihaz konumu hakkında güncelleştirmeler almak için konum API kullanımı gösterilmiştir.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**C# ile Android 8.0 Oreo geliştirme**


## <a name="summary"></a>Özet

Bu makalede, Android Oreo sunulan ve yükleme ve en son araçları ve Xamarin.Android geliştirme paketleri üzerinde Android Oreo yapılandırma adımları açıklanmıştır. Android Oreo'da kullanılabilir anahtar özelliklerine genel bakış, birçok yeni özellik için kaynak kod örneği için bağlantılarla birlikte sağlanan. API belgelerine bağlantılar dahil ve Android Oreo için uygulamaları oluştururken yardımcı olması için Android Geliştirici konuları kullanmaya başlayın. Ayrıca, mevcut uygulamaları etkileyebilir en önemli Android Oreo davranış değişiklikleri vurgulanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android 8.0 Oreo](https://developer.android.com/index.html)
