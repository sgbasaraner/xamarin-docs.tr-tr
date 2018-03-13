---
title: "Oreo özellikleri"
description: "Xamarin.Android Android en son sürümü için uygulama geliştirmek için kullanmaya başlamak nasıl."
ms.topic: article
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 03be7b624ffa9dd8774f291b96be27499cccab2b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="oreo-features"></a>Oreo özellikleri

_Xamarin.Android Android en son sürümü için uygulama geliştirmek için kullanmaya başlamak nasıl._

[Android 8.0 Oreo](https://developer.android.com/index.html) Google Android en son sürümü kullanılabilir. Android Oreo Xamarin.Android geliştiricilerine ilgi birçok yeni özellik sunar. Bu özellikler, XML, indirilebilir yazı tiplerini, otomatik doldurmaya ve resim içinde resim (PIP) bildirim kanalları, bildirim rozetleri, özel yazı tipi içerir. Bu yeni capabilties yeni API'ler Android Oreo içerir ve bu API'leri ve sonrasında Xamarin.Android 8.0 kullandığınızda Xamarin.Android uygulamaları için kullanılabilir.

[![Android Oreo kahramanı görüntüsü](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Bu makale için Android 8.0 Oreo Xamarin.Android uygulamaları geliştirmeye başlamanıza yardımcı olmak için yapılandırılmıştır. Gerekli güncelleştirmeleri yüklemek, SDK'yı yapılandırmak ve test etmek için bir öykünücü (veya cihaz) oluşturmak nasıl açıklanmaktadır. Ayrıca Android Oreo özelliklerinin Xamarin.Android uygulamaları nasıl kullanılacağını gösteren örnek uygulamalar ile Android 8.0 Oreo'deki yeni özelliklerin bir özetini sağlar.


## <a name="requirements"></a>Gereksinimler

Aşağıdaki Xamarin tabanlı uygulamalarda Android Oreo özellikleri kullanmak için gereklidir:

-   **Visual Studio** &ndash; Windows kullanıyorsanız, 15,5 veya Visual Studio sonraki bir sürümü gereklidir.  Mac kullanıyorsanız, Visual Studio sürümü 7.2.0 Mac için gereklidir.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 or later must be installed and configured with Visual Studio.

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.



## <a name="getting-started"></a>Başlarken

Android Oreo Xamarin.Android ile kullanmaya başlamak için indirin ve Android Oreo proje oluşturmadan önce SDK paketlerini ve en son araçları yüklemeniz gerekir:

1. Visual Studio en son sürüme güncelleştirin.

2. Yükleme **Android 8.0.0 (API 26)** veya üzeri paketleri ve araçları aracılığıyla SDK Yöneticisi.

3. Yeni bir Xamarin.Android projesi hedefleyen Android Oreo (API 26) oluşturun.

4. Bir öykünücü veya cihaz Android Oreo uygulamalarını test etme için yapılandırın.

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır:



### <a name="update-visual-studio-and-xamarinandroid"></a>Visual Studio ve Xamarin.Android güncelleştir

Visual Studio Android Oreo desteği eklemek için aşağıdakileri yapın:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Visual Studio 2017 kullanıyorsanız: 

    1. Visual Studio 2017 15,5 veya sonraki bir sürümü için güncelleştirme (bkz [güncelleştirme Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/update-visual-studio)).

    2. SDK Yöneticisi'ni [yükleme yönergeleri](~/android/get-started/installation/android-sdk.md#installation) Xamarin SDK Yöneticisi'ni yüklemek için.
       Google değişikliğine API 26,0 ve sonraki sürümleri destekler GUI SDK manager sağlamadığından Xamarin SDK Yöneticisi yüklü olması gerekir.

-   Biz 25 SDK Araçları önceki sürüme indirme ve kullanarak öneririz Visual Studio 2015 kullanıyorsanız, eski Google öykünücü yöneticisini GUI. SDK Araçları 25 API 26 yanı sıra, 27 ve yeni, hala kullanılabilir ve yeni platformlar için geliştirme etkisi olmaz. VS eski sürümleri için Android SDK yönetmek için bu, bir arabirim sağlar.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

-   İçinde anlatıldığı gibi Visual Studio 2017 en son kararlı sürümü Mac için güncelleştirme [Mac için Visual Studio güncelleştirme](https://docs.microsoft.com/en-us/visualstudio/mac/update).

-----

Xamarin Android Oreo desteği hakkında daha fazla bilgi için bkz: [Xamarin.Android 8.0 sürüm notları](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Android SDK'sını yükleyin

Xamarin.Android 8.0 ile bir proje oluşturmak için önce Xamarin Android SDK Yöneticisi SDK platformu için yüklemek için kullanmanız gerekir **Android 8.0 - Oreo** veya sonraki bir sürümü. Android SDK Araçları 26,0 veya sonraki sürümünü de yüklemeniz gerekir.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. SDK Yöneticisi'ni başlatın (Visual Studio'da sırasıyla **Araçlar > Android > Android SDK Manager**).

2. Yükleme **Android 8.0 - Oreo** paketler. Android SDK öykünücüsü kullanıyorsanız, eklediğinizden emin olun **x86** ihtiyacınız olacak sistem görüntüler:

    [![Android SDK Yöneticisi'nde Android 8.0 paketleri seçme](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Yükleme **Android SDK Araçları 26.0.2** veya sonraki sürümlerde, **Android SDK platformunuzun Araçlar 26.0.0** veya sonraki bir sürümü ve **Android SDK derleme-araçları 26.0.0** (veya üstü):

    [![Android SDK Araçları 26 Android SDK Yöneticisi'nde seçme](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. SDK Yöneticisi'ni başlatın (Mac için Visual Studio'da sırasıyla **Araçlar > SDK Manager**).

2. Yükleme **Android 8.0 - Oreo** SDK paketler. Android SDK öykünücüsü kullanıyorsanız, eklediğinizden emin olun **x86** ihtiyacınız olacak sistem görüntüler:

    [![Android 8.0 paketleri SDK Yöneticisi'nde seçme](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Yükleme **Android SDK Araçları 26.0.2** veya sonraki sürümlerde, **Android SDK platformunuzun Araçlar 26.0.0** veya sonraki bir sürümü ve **Android SDK derleme-araçları 26.0.0** (veya üstü):

    [![Android SDK Araçları 26 SDK Yöneticisi'nde seçme](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Start a Xamarin.Android Project

Yeni bir Xamarin.Android projesi oluşturun. Xamarin Android geliştirme yeniyseniz, bkz: [Hello, Android](~/android/get-started/hello-android/index.md) Xamarin.Android projeleri oluşturma hakkında bilgi edinmek için.

Bir Android projesi oluşturduğunuzda, hedef Android 8.0 veya sonraki sürüm ayarlarını yapılandırmanız gerekir. Örneğin, Android 8.0 için projenizi hedeflemek için projenize hedef Android API düzeyini yapılandırmalısınız **Android 8.0 (API 26)**. API 26 ya da daha sonra hedef çerçeve düzeyi de ayarlamanız önerilir. Android API düzeyi düzeyleri yapılandırma hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Bir öykünücü veya cihaz yapılandırma

Android SDK Araçları 26.0 yükledikten sonra varsayılan Google GUI tabanlı AVD Yöneticisi başlatma girişimi veya komut satırı AVD Yöneticisi aracını kullanmak için bildirir aşağıdaki hata iletişim kutusunda daha sonra alabilirsiniz **avdmanager** yerine :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android Emulator Manager Uyarısı iletişim kutusu](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Android Emulator Manager Uyarısı iletişim kutusu](oreo-images/mac/03-avd-warning.png)

-----

Google artık değişikliğine API 26,0 ve sonraki sürümleri destekler GUI AVD Yöneticisi sağladığından bu ileti görüntülenir. Android 8.0 Oreo için Xamarin Android Öykünücüsü Yöneticisi'ni veya komut satırı kullanmalıdır `avdmanager` sanal cihazlar için Android Oreo oluşturmak için aracı.

Oluşturmak ve sanal cihazları yönetmek için Xamarin Android Aygıt Yöneticisi'ni kullanmak için bkz: [Xamarin Android Aygıt Yöneticisi'ni](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Sanal cihazlar olmadan Xamarin Android Emulator Manager oluşturmak için sonraki bölümde yer alan adımları izleyin.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Sanal cihazlar kullanarak avdmanager oluşturma

Kullanılacak **avdmanager** yeni bir sanal cihaz oluşturmak için aşağıdaki adımları izleyin:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Bir komut istemi penceresi açın ve ayarlayın `JAVA_HOME` konumu bilgisayarınızda Java SDK'sı olarak. Tipik bir Xamarin yükleme için aşağıdaki komutu kullanabilirsiniz:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Android SDK'sı konumu Ekle `bin` klasörüne, `PATH`.
    Tipik bir Xamarin yükleme için aşağıdaki komutu kullanabilirsiniz:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Komut İstemi penceresini kapatın ve yeni bir komut istemi penceresi açın. Kullanarak yeni bir sanal cihaz oluşturma [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) komutu. Örneğin, adında bir AVD oluşturmak için **AVD Oreo 8.0** x86 kullanarak sistem görüntüsü API düzeyi 26, için aşağıdaki komutu kullanın:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  İle sorulduğunda **özel donanım profili [yok] oluşturmak ister misiniz** girebileceğiniz **hiçbir** ve varsayılan donanım profilini kabul edin. Söylediğinizde **Evet**, **avdmanager** donanım profili özelleştirme sorular listesini ister.

Çalıştırdıktan sonra **avdmanager** , sanal cihazı oluşturmak için aygıt aşağı açılır menüde dahil edilir:

[![Cihaz açılan menüsüne eklenen yeni AVD](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1.  Açık bir **Terminal** penceresi ve Mac üzerinde Android SDK Araçları dizin konumunu değiştirme Tipik bir Xamarin yükleme için aşağıdaki komutu kullanabilirsiniz:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Kullanarak yeni bir sanal cihaz oluşturma [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) komutu. Örneğin, adında bir AVD oluşturmak için **AVD Oreo 8.0** x86 kullanarak sistem görüntüsü API düzeyi 26, için aşağıdaki komutu kullanın:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  İle sorulduğunda **özel donanım profili [yok] oluşturmak ister misiniz** girebileceğiniz **hiçbir** ve varsayılan donanım profilini kabul edin. Söylediğinizde **Evet**, **avdmanager** özelleştirme sorular listesini ister donanım profili.

Kullandıktan sonra **avdmanager** , sanal cihazı oluşturmak için aygıt aşağı açılır menüde dahil edilir:

[![Cihaz açılan menüsüne eklenen yeni AVD](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Android öykünücüsünde test ve hata ayıklama için yapılandırma hakkında daha fazla bilgi için bkz: [Android SDK öykünücüsü](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Bir Nexus veya piksel gibi fiziksel bir aygıtı kullanıyorsanız, ya da Cihazınızı otomatik hava (OTA) Güncelleştirmeler üzerinden güncelleştirebilir veya bir sistem görüntüsünü karşıdan yüklemek ve Cihazınızı doğrudan flash. Android Oreo Cihazınızı el ile güncelleştirme hakkında daha fazla bilgi için bkz: [Nexus ve piksel cihazlar Fabrika görüntülerinin](https://developers.google.com/android/images).



## <a name="new-features"></a>Yeni Özellikler

Android Oreo çeşitli yeni özellikler ve bildirim kanalları, bildirim rozetleri, XML özel yazı tipi, indirilebilir yazı tiplerini, otomatik doldurmaya ve resim içinde resim gibi özellikler sunar. Aşağıdaki bölümlerde bu özellikler vurgulayın ve bağlantıları yardımcı olmak için bunları uygulamanızda kullanmaya başlama sağlayın.



### <a name="notification-channels"></a>Bildirim kanalları

*Bildirim kanallarını* bildirimler için uygulama tarafından tanımlanan kategori.
Seçenekler, uygulamanızın kullanıcılar tarafından yapılan yansıtacak şekilde bildirim kanalları oluşturabilirsiniz ve her göndermesi gerekir. bildirim türü için bir bildirim kanalı oluşturabilirsiniz. Yeni bildirim kanalları özellik, kullanıcıların hassas bildirimleri farklı türde üzerinde denetime sahip olmak için mümkün kılar. Örneğin, bir Mesajlaşma uygulaması uyguluyorsanız, bir kullanıcı tarafından oluşturulan her bir konuşma grubu için ayrı bildirim kanalları oluşturabilirsiniz.

[Bildirim kanallarını](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) bir bildirim kanalı oluşturmak ve yerel bildirimleri göndermek için kullanmak üzere açıklanmaktadır. Gerçek dünya kod örneği için bkz: [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) örnek; bu örnek uygulama iki kanalı yönetir ve ek bildirim seçeneklerini ayarlar.



### <a name="notification-badges"></a>Bildirim rozetleri

Bildirim rozetleri uygulama bu ekran görüntüsünde gösterildiği gibi görünmezse küçük noktalar şunlardır:

[![Uygulama simgeleri üzerinde örnek bildirim rozetleri](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Bu noktalar, bu uygulama simgesi ile ilişkili uygulama bir veya daha fazla bildirim kanalı için yeni bildirimleri belirtmek &ndash; kullanıcı henüz kapatıldığında veya üzerinde işlem bildirimleri bunlar. Kullanıcılar uzun-üzerindeki bir simge bildirim rozet ile ilişkili bildirimleri kapatılıyor veya uzun tuşuna menüsünden bildirimleri o appeaars hareket bakışta tuşuna basarak.

Android Geliştirici bildirim rozetleri hakkında daha fazla bilgi için bkz: [bildirim rozetleri](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) konu.



### <a name="custom-fonts-in-xml"></a>XML özel yazı tipleri

Android Oreo tanıtır *XML yazı tiplerini*, hangi mümkün kılar, özel yazı tipi kaynakları olarak eklemenizi. OpenType (**.otf**) ve TrueType (**.ttf**) yazı tipi biçimleri desteklenir. Yazı tipleri kaynaklar olarak eklemek için aşağıdakileri yapın:

1. Oluşturma bir **kaynakları/yazı tipi** klasör.

2. Yazı tipi dosyalarınızı kopyalayın (örneğin, **.ttf** ve **.otf** dosyaları) için **kaynakları/yazı tipi**. 

3. Adlandırma kuralları Android dosyasına eklenecek şekilde gerekirse, her yazı tipi dosyası yeniden adlandırın (yani, yalnızca küçük harf kullanımı *a-z*, *0-9*ve dosya adları alt çizgi). Örneğin, yazı tipi dosyası `Pacifico-Regular.ttf` gibi bir adlandırılacak `pacifico.ttf`.

4. Yeni kullanarak özel yazı tipi uygulamak `android:fontFamily` düzeninizi XML özniteliği. Örneğin, aşağıdaki `TextView` bildirimini kullanır eklenen **pacifico.ttf** yazı tipi kaynak:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Stil ve Ağırlık ayrıntıları yanı sıra birden çok yazı tipi açıklayan bir yazı tipi ailesi XML dosyası da oluşturabilirsiniz. Daha fazla bilgi için bkz: Android Geliştirici [XML yazı tiplerini](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) konu.


### <a name="downloadable-fonts"></a>İndirilebilir yazı tipleri

Android Oreo ile başlayarak, uygulamalar, APK paketleme yerine bir sağlayıcı yazı tipleri isteyebilir. Yazı tipleri ağdan yalnızca gerektiğinde indirilir. Bu özellik telefon bellek ve hücresel veri kullanımı koruma APK boyutunu azaltır. Bu özellik Android API 14 ve sonraki sürümlerinde Android destek kitaplığına 26 paketi yükleyerek de kullanabilirsiniz.

Uygulamanızı bir yazı tipi gerektiğinde, oluşturduğunuz bir `FontsRequest` (karşıdan yüklemek için yazı tipi belirtme) nesne ve ona geçirin bir `FontsContract` yazı tipi yüklemek için yöntem. Aşağıdaki adımlar, yazı tipi yükleme işlemi daha ayrıntılı açıklamaktadır:

1.  Örneği bir [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) nesnesi. 

2.  Bir alt kümesi ve örneği [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Uygulama [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) tamamlama yazı tipi isteği işlemek için kullanılan yöntem.

4.  Uygulama [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) yazı tipi isteği işlemi sırasında gerçekleşecek herhangi bir hata uygulamanızı bilgilendirmek için kullanılan yöntem.

5.  Çağrı [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) yazı tipi yazı tipi sağlayıcısı'ndan alma yöntemi. 

Çağırdığınızda `RequestFonts` yöntemi, onu önce denetler yazı yerel olarak önbelleğe görmek için (önceki çağrısından `RequestFont`). Alınmamışsa, yazı tipi sağlayıcısı çağıran, yazı tipi zaman uyumsuz olarak alır ve daha sonra sonuçları çağırarak geri uygulamanıza geçirir, `OnTypeFaceRetrieved` yöntemi.

[İndirilebilir yazı tipleri](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) örnek Android Oreo içinde sunulan indirilebilir yazı tipleri özelliğinin nasıl kullanılacağı gösterilmektedir. 

Yazı tipleri yükleme hakkında daha fazla bilgi için bkz: Android Geliştirici [indirilebilir yazı tipleri](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) konu.



### <a name="autofill"></a>Otomatik doldurma

Yeni _otomatik doldurmaya_ Android Oreo framework kullanıcıların oturum açma, hesap oluşturma ve kredi kartı işlemleri gibi yinelenen görevleri işlemek kolaylaştırır. Kullanıcıların (ve hataları girişi için yol açabilir) bilgileri yeniden yazma daha az süre beklemesini. Uygulamanızı otomatik doldurmaya Framework ile çalışmadan önce sistem ayarlarını (kullanıcıları etkinleştirebilir veya devre dışı otomatik doldurmaya) bir otomatik doldurmaya hizmetinin etkinleştirilmesi gerekir.

[AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) örnek otomatik doldurmaya Framework kullanımını gösterir. İstemci etkinlikleri autofilled ve etkinlikleri istemciye otomatik doldurmaya veri sağlayan bir hizmet olmalıdır görünümlere sahip uygulamaları içerir.

Yeni otomatik doldurma özelliğinin ve uygulamanız otomatik doldurmaya için en iyi duruma getirme hakkında daha fazla bilgi için bkz: Android Geliştirici [otomatik doldurmaya Framework](https://developer.android.com/guide/topics/text/autofill.html) konu.



### <a name="picture-in-picture-pip"></a>Resim içinde resim (PIP)

Android Oreo, başka bir etkinliğin ekranın üst üste getirme resim içinde resim (PIP) modunda başlatmak bir etkinlik mümkün kılar. Bu özellik, video kayıttan yürütme için tasarlanmıştır.

Uygulamanızın etkinlik PIP modunu kullanabilirsiniz belirtmek için true Android bildirimindeki aşağıdaki bayrağı ayarlayın:

```xml
android:supportsPictureInPicture
```

PIP modunda olduğunda nasıl etkinliklerinizi hareket etmesi gerektiğini belirtmek için yeni kullandığınız [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) nesnesi. `PictureInPictureParams` başlatma ve aktivite PIP modda (örneğin, etkinliğin tercih edilen en boy oranını) güncelleştirmek için kullandığınız parametreleri kümesini temsil eder. Aşağıdaki yeni PIP yöntemleri eklenme `Activity` Android Oreo içinde:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; etkinlik PIP moduna geçirir. Etkinlik ekranın üst köşesine yerleştirilir ve ekran kalan ekranda edildi önceki etkinliğin ile doldurulur.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; etkinliğin PIP yapılandırma ayarlarını (örneğin, bir değişiklik en boy oranını) güncelleştirir.

[PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) örnek Oreo içinde sunulan taşınabilir cihazları için resim içinde resim (PIP) mod temel kullanımını gösterir. Örnek, görüntü modları veya diğer etkinlikler arasında ileri ve geri değiştirilirken devam bir video kesintisiz oynatılır.



### <a name="other-features"></a>Diğer özellikler

Android Oreo içeren diğer birçok Emoji destek kitaplığı konumu API, arka plan sınırları gibi yeni özellikler, uygulamalar, yeni ses codec bileşenleri, Web görünümü geliştirmeleri, geliştirilmiş klavye gezinti desteği ve için yeni bir AAudio (pro ses) API wide gam rengi yüksek performanslı düşük gecikme süreli ses, bu özellikler hakkında daha fazla bilgi için bkz: Android Geliştirici [Android Oreo özellikleri ve API'leri](https://developer.android.com/about/versions/oreo/android-8.0.html) konu.



## <a name="behavior-changes"></a>Davranış değişiklikleri

Android Oreo çeşitli sistem ve var olan uygulamaların işlevselliğini üzerinde bir etkisi olabilir API davranış değişiklikleri içerir. Bu değişiklikler aşağıda açıklanmıştır.


### <a name="background-execution-limits"></a>Arka plan yürütme sınırları

Kullanıcı deneyimini geliştirmek için Android Oreo uygulamaları neler yapabileceğinizi sınırlamalar getirir arka planda çalışırken. Örneğin, kullanıcı bir video izlerken veya bir oyuna, arka planda çalışan bir uygulama ön planda çalışan bir video yoğunluklu uygulama performansını zarar. Sonuç olarak, Android Oreo kullanıcı ile doğrudan etkileşim olmayan uygulamaları aşağıdaki kısıtlamaları getirir:

1.  **Arka plan Service sınırlamalar** &ndash; uygulama arka planda çalıştırırken, birkaç dakika bunu hala verilir oluşturmak ve hizmetleri kullanmak için bir pencere vardır. Bu pencere sonunda, Android uygulamanın arka plan hizmetini durdurur ve olarak değerlendirir _boşta_.

2.  **Sınırlamalar yayın** &ndash; Android 7.0 (API 25) almak için bir uygulama kaydeden yayınları sınırlamaları yerleştirilir. Android Oreo sınırlamalara daha sıkı hale getirir. Örneğin, Android Oreo uygulamaları örtük yayınlar için yayın alıcılar artık kendi bildirimleri kaydedebilirsiniz.

Android Geliştirici yeni arka plan yürütme sınırları hakkında daha fazla bilgi için bkz: [arka plan yürütme sınırları](https://developer.android.com/about/versions/oreo/background.html) konu.


### <a name="breaking-changes"></a>Yeni Değişiklikler

Android Oreo hedef ya da daha yüksek uygunsa aşağıdaki değişiklikleri desteklemek için uygulamalarını değiştirmelisiniz uygulamalar:

- Android Oreo tek tek bildirimleri önceliğini ayarlama özelliği kaldırır. Bunun yerine, önerilen önem düzeyini bir bildirim kanalı oluştururken ayarlayın. Bir bildirim kanalı atadığınız önem düzeyini ona gönderdiğiniz tüm bildirim iletileri için geçerlidir.

- Android Oreo hedefleyen uygulamalar için `PendingIntent.GetService()` arka planda başlatılan hizmetler getirilen yeni sınırları nedeniyle çalışmıyor. Android Oreo hedefliyorsanız, kullanmanız gereken [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) yerine.  


## <a name="sample-code"></a>Örnek kod

Android Oreo özelliklerden yararlanmak nasıl göstermek birkaç Xamarin.Android örnekler vardır:

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Android Oreo içinde sunulan yeni bildirim kanalları sisteminin nasıl kullanılacağı gösterilmektedir. Bu örnek iki bildirim kanalları yönetir: biri varsayılan önem ve diğer yüksek önem düzeyine sahip.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Oreo içinde sunulan taşınabilir cihazları için resim içinde resim (PIP) mod temel kullanımını gösterir. Örnek, görüntü modları veya diğer etkinlikler arasında ileri ve geri değiştirilirken devam bir video kesintisiz oynatılır.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) otomatik doldurmaya Framework kullanımını gösterir. İstemci etkinlikleri autofilled ve etkinlikleri istemciye otomatik doldurmaya veri sağlayan bir hizmet olmalıdır görünümlere sahip uygulamaları içerir.

-   [İndirilebilir yazı tipleri](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) daha önce açıklanan indirilebilir yazı tipleri özelliğinin nasıl kullanılacağı gösteren bir örnek sağlar.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) EmojiCompat destek kitaplığı kullanımını gösterir. Bu kitaplığı gösteren eksik emoji karakterleri "tofu" karakter olarak uygulamanızdan önlemek için kullanabilirsiniz.

-   [Konum güncelleştirmeleri bekleyen hedefi](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) hakkında bir cihazın konumunu güncelleştirmeleri almak için konum API kullanımı gösterilmiştir kullanarak bir `PendingIntent`.

-   [Konum güncelleştirmeleri ön plan hizmeti](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) konumu API ilişkili ve başlatılan ön plan hizmetini kullanarak bir cihazın konumunu hakkında güncelleştirmeleri almak için nasıl kullanılacağını gösterir.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**C# ile Android 8.0 Oreo geliştirme**


## <a name="summary"></a>Özet

Bu makalede, Android Oreo tanıtılan ve yüklemek ve en yeni araç ve xamarin Android geliştirme için paketler üzerinde Android Oreo yapılandırmak nasıl açıklanmıştır. Android Oreo içinde kullanılabilir anahtar özelliklerine birçok yeni özellik için kaynak kod örneği için bağlantılar ile birlikte sağlanan. API belgelerine bağlantılar dahil ve uygulamalar için Android Oreo oluştururken yardımcı olması için Android Geliştirici konuları çalışmaya başlayın. Ayrıca, mevcut uygulamalarınızı etkileyebilir en önemli Android Oreo davranış değişiklikleri vurgulanır.


## <a name="related-links"></a>İlgili bağlantılar

- [Android 8.0 Oreo](https://developer.android.com/index.html)
