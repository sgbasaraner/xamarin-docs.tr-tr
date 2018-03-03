---
title: "Google öykünücüsü yöneticisi"
description: "Oluşturma ve Google Öykünücüsü Yöneticisi'ni kullanarak Android sanal cihazları yönetme"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: f275ff6c7d3e6eeec5eb3878cc39633d70238f66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="google-emulator-manager"></a>Google öykünücüsü yöneticisi

Donanım hızlandırma etkinleştirildiğini doğruladıktan sonra (açıklandığı gibi [Android öykünücüsü donanım hızlandırmasını](~/android/get-started/installation/android-emulator/hardware-acceleration.md)), test ve uygulamanızı hata ayıklama için kullanılacak sanal cihaz oluşturmak için sonraki adımdır. Eski Google Öykünücüsü Yöneticisi'ni kullanabilirsiniz (olarak da bilinen *Android sanal cihazı (AVD) Yöneticisi'ni*) Android SDK öykünücüsü tarafından kullanılacak sanal cihaz oluşturmak için.

> [!NOTE]
> **Not:** Android 8.0 Oreo hedefliyorsanız, kullanmalısınız [Xamarin Android Aygıt Yöneticisi'ni](~/android/get-started/installation/android-emulator/xamarin-device-manager.md) oluşturmak ve sanal cihaz yapılandırmak için.

<a name="sysimg" />

## <a name="installing-system-images"></a>Sistem görüntüleri yükleme

Hedeflemek istediğiniz Android API düzeylerini bağlı olarak, indirin ve Android SDK öykünücüsü tarafından kullanılan API düzeyi özgü sistem görüntüleri yükleyin. Her Android API düzeyi için bir dizi vardır **x86** yükleyip sanal cihaz oluşturmak için gerekecek sistem görüntüler.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Gerekli sistem görüntüleri yüklemek için Android SDK Yöneticisi'ni başlatın (**Araçlar > Android > Android SDK Manager**) ve desteklemek istediğiniz API düzeylerini gidin. Her API düzeyi için aşağıdaki sistem görüntüleri yanındaki onay işareti etkinleştirin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Gerekli sistem görüntüleri yüklemek için Android SDK Yöneticisi'ni başlatın (**Araçlar > SDK Manager**) ve desteklemek istediğiniz API düzeylerini gidin. Her API düzeyi için aşağıdaki sistem görüntüleri yanındaki onay işareti etkinleştirin:

-----

-   **Intel x86 Atom sistem görüntüsü**
-   **Google API'leri Intel x86 Atom sistem görüntüsü**

İkinci Sistem görüntüsünü sanal cihaza Google API'leri (örneğin, Google haritalar API) ekler. 

Aşağıdaki ekran görüntüsünde **Intel x86 Atom** görüntüleri yüklenir, böylece Android 6.0 çalıştıran sanal cihazlar oluşturulabilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Android 6.0 x86 sistem görüntüleri için Android öykünücüsü seçme](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Android 6.0 x86 sistem görüntüleri için Android öykünücüsü seçme](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png)

-----

64-bit uygulamalar geliştiriyorsanız, bunun yerine aşağıdaki sistem görüntüleri yükleyin:

-   **Intel x86 Atom_64 sistem görüntüsü**
-   **Google API'leri Intel x86 Atom_64 sistem görüntüsü**

32-bit uygulamaları çalıştırmak için bu 64-bit sistem görüntüleri kullanabilirsiniz; Ancak, 32-bit **Intel x86 Atom sistem görüntüsü** Android SDK öykünücüsü biraz daha hızlı çalışır.

Android takmak için uygulama geliştiriyorsanız, aşağıdaki sistem görüntüleri yükleyin:

-   **Android yıpranması Intel x86 Atom sistem görüntüsü**
-   **Google API'leri Intel x86 Atom sistem görüntüsü**

Bu sistem görüntüleri yüklendikten sonra oluşturabileceğiniz **x86**-Android sanal cihaz (Bu açıklanmaktadır sonraki) sanal aygıt yapılandırması sırasında uygun API düzeyi ve CPU/ABI Seçenekler'i seçerek tabanlı.


<a name="virtualdevice" />

## <a name="configuring-virtual-devices"></a>Sanal cihaz yapılandırma

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sanal cihazlar aracılığıyla yapılandırılır **Android Emulator Manager** (olarak da adlandırılan _Android sanal cihaz Yöneticisi_ veya _AVD Yöneticisi_). Visual Studio'dan Android öykünücü yöneticisini başlatmak için tıklatın **Android Emulator Manager** araç çubuğunda simge:

[ ![AVD simgesinin yeri](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png)

Menü çubuğundan Android Öykünücüsü Yöneticisi'ni seçerek de başlatabilirsiniz **Araçlar > Android > Android Emulator Manager**:

[![Android Emulator Manager menü öğesi konumu](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png)

**Android sanal cihazı (AVD) Yöneticisi'ni** iletişim varolan Android sanal aygıtların listesini görüntüler:

[![Android sanal cihaz Yöneticisi](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Sanal cihazlar aracılığıyla yapılandırılır **Android Emulator Manager** (olarak da adlandırılan _Android sanal cihaz Yöneticisi_ veya _AVD Yöneticisi_). 

Menü çubuğundan Android Öykünücüsü Yöneticisi'ni seçerek başlatabilirsiniz **Araçlar > Google öykünücü yöneticisini**:

[![Android Emulator Manager menü öğesi konumu](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png)

**Android sanal cihazı (AVD) Yöneticisi'ni** iletişim varolan Android sanal aygıtların listesini görüntüler:

[![Android sanal cihaz Yöneticisi](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png)

-----

Yeni sanal cihaz görüntüleri farklı aygıt özellikleri ve API düzeylerini oluşturabilirsiniz &ndash; özel cihaz tanımları ve sanal cihaz oluşturma sonraki bölümde açıklanmaktadır.

<a name="custom-def" />

### <a name="creating-a-custom-device-definition"></a>Bir özel cihaz tanımı oluşturma

Bir özel cihaz tanımı oluşturmak için tıklatın **oluştur...**  içinde **Android sanal cihazı (AVD) Yöneticisi'ni**. Bu açılır **yeni Android sanal cihazı (AVD) oluşturma** iletişim:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Özel cihaz tanımı üzerinde nexus 6 tabanlı](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel cihaz tanımı üzerinde nexus 6 tabanlı](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png)

-----

Bu iletişim kutusunda aşağıdaki seçenekleri yapılandırın:

-   **AVD adı** &ndash; aygıt tanımınız için benzersiz bir ad. Yukarıdaki örnek ekran görüntüsünde adı ayarlamak **MyNexus**. AVD ad boşluk içeremez Not &ndash; **Tamam** düğmesi devre dışı bırakılacak AVD adında boşluk kullanmayı denerseniz.

-   **Aygıt** &ndash; taklit etmek istediğiniz donanım profilini seçin (örneğin, **Nexus 5** veya **Nexus 6**).

-   **Hedef** &ndash; sanal cihaz için Android API düzeyini seçin. Bu ayar, uygulamanızın Minimum Android sürümüne eşit veya daha büyük olmalıdır.

-   **CPU/ABI** &ndash; seçin **Google API'leri Intel Atom (x86)** Google API'leri cihaz tanımı'nda kullanılabilir olmayacaktır.

-   **Dış Görünüm** &ndash; sanal cihaz görünümünü seçin. Yukarıdaki örnek ekran görüntüsünde **HVGA** kaplama seçili (Bu makalenin sonunda öykünücüsü ekran örneğidir **HVGA** kaplama).

-   **Bellek seçenekleri** &ndash; genellikle, varsayılan RAM ayarı çok yüksek ve, Windows, uyarıyı neden olur: **öykünen RAM büyüktür 768 M başlatılamayabilir**. Çoğu kullanıcı için ayarı RAM 768 MB (yukarıdaki ekran görüntüsünde gösterildiği gibi) öneririz. Büyük RAM değerler öykünücüsü yavaşlatabilir.

-   **Ana bilgisayar GPU kullanmak** &ndash; bu seçenek, bilgisayarın grafik işlem birimi (grafik işlemleri gerçekleştirmek için GPU) ana bilgisayarı kullanmak üzere öykünücü sağlar. Daha fazla öykünücü performansını artırmak bu seçeneği etkinleştirmeniz önerilir. Daha fazla bilgi için **öykünme seçenekleri** bölümünde, bkz: [kullanılan seçenekler için anlık görüntü ve kullanım konak GPU öykünme nelerdir?](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for)


Varsayılan ayarlarına geri kalan seçenekler bırakılabilir. Hazır olduğunuzda tıklatın **Tamam** yeni sanal cihazı oluşturmak için. Yeni sanal aygıt yapılandırmasını sonuçlarını sonraki iletişim kutusunda ayrıntılı:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Yeni bir AVD oluşturduktan sonra sonuçları iletişim kutusu](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

![Yeni bir AVD oluşturduktan sonra sonuçları iletişim kutusu](google-emulator-manager-images/mac/07-create-results.png)

-----

Bu iletişim kutusunda listelenen yapılandırma özellikleri ayrıntılı bir açıklaması için bkz: [donanım profilinin özelliklerini](https://developer.android.com/studio/run/managing-avds.html#hpproperties).
Tıklattıktan sonra **Tamam**, yeni aygıt yapılandırma varolan Android sanal cihaz listede görüntülenir. Aşağıdaki ekran görüntüsünde **MyNexus** listesine eklendi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihaz listesine eklenen MyNexus](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Cihaz listesine eklenen MyNexus](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png)

-----

Yeni özel sanal cihaz aynı zamanda aygıt aşağı açılır menüyü eklenir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihaz açılan menüsüne eklenen MyNexus](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Cihaz açılan menüsüne eklenen MyNexus](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png)

-----


<a name="cloning" />

### <a name="cloning-a-device-definition"></a>Bir cihaz tanımı kopyalama

Var olan bir cihaz tanımı seçmek mümkündür ve *kopya* yeni bir özel cihaz tanımı oluşturmak için. Bu, ihtiyaçlarınızı karşılamak için yalnızca birkaç küçük ayarlamalar gereken varolan aygıt tanımını olduğunda kullanılacak bir iyi stratejidir. **Aygıt tanımları** sekmesinde **Android sanal cihazı (AVD) Yöneticisi'ni** tüm kullanılabilir cihaza tanımlarını listeler:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kullanılabilir aygıta tanımları listesi](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kullanılabilir aygıta tanımları listesi](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png)

-----

Önceden yapılandırılmış aygıtlar bu listede değişiklik yapılamaz &ndash; yalnızca kullanıcı tarafından oluşturulan sanal cihazlar düzenlenebilir. Bir cihaz tanımı seçerek ve tıklatarak yeni bir cihaz tanımı önceden yapılandırılmış aygıt tanımından türetilen mümkündür **kopya**. Örneğin, seçme **Nexus 5** tanımı ve tıklayarak **kopya** aşağıdaki iletişim kutusunu gösterir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Kopya aygıt iletişim](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Kopya aygıt iletişim](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png)

-----

Sonraki ekran görüntüsünde, ad olarak değiştirilmesini **Nexus 5 özel** ve yeni bir özel cihaz tanımı oluşturmak için aygıt parametrelerini değiştirilebilir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Özel Nexus 5 AVD](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Özel Nexus 5 AVD](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png)

-----

Tıklatarak **kopya aygıt** artık yeni bir cihaz tanımı oluşturur **aygıt tanımları** listesi:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 5 özel yeni bir kullanıcı cihaz tanımı görüntülenir](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Nexus 5 özel yeni bir kullanıcı cihaz tanımı görüntülenir](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png)

-----

Yukarıda gösterildiği gibi her bir kullanıcı tarafından oluşturulan cihaz tanımı yeşil bir simge ile görüntülendiğine dikkat edin. Bu yeni bir cihaz tanımı tanımı seçerek ve tıklatarak yeni bir AVD oluşturmak için kullanılan **oluşturma AVD**. Bu görüntüler **yeni Android sanal cihazı (AVD) oluşturma** iletişim. Aşağıdaki örnekte, adı **AVD\_için\_Nexus\_5\_özel** yeni sanal cihaz için otomatik olarak oluşturuldu:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![AVD Nexus 5 özel kullanıcı aygıt tanımından oluşturma](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![AVD Nexus 5 özel kullanıcı aygıt tanımından oluşturma](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png)

-----

Sonra **Tamam** tıklandığında, özel cihaz yapılandırması, var olan Android sanal aygıtlar listesinde görüntülenir. Ayrıca, bu cihaz aşağı açılır menüyü eklenir:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Cihaz açılan menüsüne eklenen yeni özel AVD](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Cihaz açılan menüsüne eklenen yeni özel AVD](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png)

-----

