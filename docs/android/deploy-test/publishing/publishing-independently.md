---
title: Bağımsız olarak yayımlama
ms.prod: xamarin
ms.assetid: 6FB4DEF2-01AD-C5FE-0950-CE1BF088A9C6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/21/2017
ms.openlocfilehash: 2cb2167f534251e15455e11b6a2c85f53fb48b8c
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067007"
---
# <a name="publishing-independently"></a>Bağımsız olarak yayımlama

Var olan Android Pazar kullanmadan bir uygulamayı yayımlamak mümkündür. Bu bölümde, diğer yayımlama yöntemlerinden ve Xamarin.Android lisans düzeylerini anlatılmıştır.


## <a name="xamarin-licensing"></a>Xamarin lisanslama

Dört lisansları, geliştirme, dağıtımı ve Xamarin.Android uygulamaları dağıtımını için kullanılabilir:

-   **Visual Studio Community** &ndash; Öğrenciler, küçük ekipleri ve Windows kullanan OSS geliştiriciler için.

-   **Visual Studio Professional** &ndash; her bir geliştirici veya küçük ekipleri (yalnızca Windows). Bu lisans standart veya Bulut aboneliği, ek Xamarin University içeriğe erişim ve kullanım kısıtlamaları sunar.

-   **Visual Studio Enterprise** &ndash; oluşan ekiplere yönelik herhangi bir boyuta (yalnızca Windows). Bu lisans Kurumsal özellikler içerir standart ya da bulut aboneliği.

Ziyaret [visualstudio.com](https://visualstudio.microsoft.com/xamarin/) Community Edition karşıdan yüklemeniz veya Professional ve Enterprise sürümleri satın alma hakkında daha fazla bilgi edinin.


## <a name="allow-installation-from-unknown-sources"></a>Bilinmeyen kaynaklardan yüklemesine izin ver

Varsayılan olarak, kullanıcıların karşıdan yükleme ve Google Play dışındaki konumlardan uygulamaları Android engeller. Market dışı kaynaklardan gelen yüklenmesine izin vermek için bir kullanıcı etkinleştirmelisiniz *bilinmeyen kaynaklardan* bir aygıtta bir uygulamanın yüklemeye çalışmadan önce ayarlama. Bu ayarı altında bulunabilir **ayarlar > Güvenlik**, aşağıdaki çizimde gösterildiği gibi:

[![Güvenlik ayarları ekranında](publishing-independently-images/settings.png)](publishing-independently-images/settings.png#lightbox)


> [!IMPORTANT]
> Bazı ağ sağlayıcıları bu ayardan bağımsız olarak bilinmeyen kaynaklardan gelen uygulamaların yüklenmesini engelleyebilir.



## <a name="publishing-by-e-mail"></a>E-posta ile yayımlama

APK yayın bir e-postaya ekleyerek kullanıcılara uygulama dağıtmak için hızlı ve kolay bir yoludur. Android destekli cihazında e-posta kullanıcı oturum açtığında, Android APK eki algılar ve görüntüleme bir **yükleme** düğme aşağıdaki resimde gösterildiği gibi:

[![Yükle düğmesi ek için](publishing-independently-images/publishing-via-email.png)](publishing-independently-images/publishing-via-email.png#lightbox)

E-posta yoluyla dağıtım basit olsa da, korsanlık veya yetkisiz dağıtım karşı birkaç koruma sağlamaz. Ayrıca, burada uygulama alıcılarını birkaç ve uygulamayı dağıtma değil güvenilen durumlar için en iyi ayrılmıştır.


## <a name="publishing-by-web"></a>Web yayımlayarak

Bir web sunucusu tarafından bir uygulama dağıtmak mümkündür. Bu, web sunucusuna uygulama karşıya yükleme ve ardından indirme bağlantısının kullanıcılara sağlayarak gerçekleştirilir. Bir Android destekli cihaz uygulamayı yükler ve bağlantı gözatar, yükleme tamamlandıktan sonra bu uygulama otomatik olarak yüklenir.


## <a name="manually-installing-an-apk"></a>Bir APK el ile yükleme

El ile yükleme, uygulamaları yüklemek için üçüncü bir seçenektir. Bir uygulamanın el ile yüklenmesini etkilemek için:

1.   **APK kullanıcı için bir kopyasını dağıtmak** &ndash; bu kopyayı bir CD veya USB flash sürücü gibi dağıtılmış.
1.   **(Kullanıcı) bir Android cihazında uygulamayı yükleyen** &ndash; komut satırı kullanmak *Android hata ayıklama köprüsü* (**adb**) aracı.   **adb** bir öykünücü örneğinde veya bir Android destekli aygıt ile iletişimi sağlayan çok yönlü bir komut satırı aracıdır. Android SDK'sı içerir **adb**; dizinde bulunan  **<sdk>/platform-tools /**.

Android cihazı bir USB kablosuyla bilgisayarınıza bağlanması gerekir.
Windows bilgisayarları tarafından kabul edilecek telefon satıcıdan ek USB sürücüler de gerekebilir **adb**. Yükleme yönergeleri için bu ek USB sürücüleri olduğundan bu belgenin kapsamı dışındadır.

Herhangi bir verilmeden önce **adb** komutlar, hangi öykünücüsü örnekleri veya cihazlara bağlı, varsa bilmeniz faydalı olduğu. Ne kullanarak bağlı olduğu bir listesini görmek mümkündür `devices` aşağıdaki kod parçacığında gösterildiği gibi komut:

```shell
$ adb devices
List of devices attached
        0149B2EC03012005device
```

Bağlı aygıtlar onayladıktan sonra uygulama vererek yüklenebilir `install` komutunu **adb**:

```shell
$ adb install <path-to-apk>
```

Aşağıdaki kod parçacığında bağlı bir aygıt için bir uygulama yükleme örneği gösterilmektedir:

```shell
$ adb install helloworld.apk
3772 KB/s (3013594 bytes in 0.780s)
        pkg: /data/local/tmp/helloworld.apk
Success
```

Uygulama zaten yüklediyseniz, `adb install` aşağıdaki örnekte gösterildiği gibi APK yükleyin ve bir hata raporu olacak kuramayacaktır:

```shell
$ adb install helloworld.apk
4037 KB/s (3013594 bytes in 0.728s)
        pkg: /data/local/tmp/helloworld.apk
Failure [INSTALL_FAILED_ALREADY_EXISTS]
```

Aygıttan uygulamayı kaldırmak gerekli olacaktır. İlk olarak, sorun `adb uninstall` komutu:

```shell
adb uninstall <package_name>
```

Aşağıdaki kod parçacığını bir uygulama kaldırma, bir örnek verilmiştir:

```shell
$ adb uninstall mono.samples.helloworld
Success
```
