---
title: Sistem uygulama olarak Xamarin.Android yükleme
description: Bu kılavuz, bir sistem uygulaması ve bir kullanıcı uygulama ve sistem uygulaması bir Xamarin.Android uygulaması yükleme arasındaki farklar ele alınacaktır. Bu kılavuz, özel Android ROM görüntüleri yazarları için geçerlidir. Özel bir ROM oluşturulacağını açıklayan değil
ms.prod: xamarin
ms.assetid: 0113143B-7D8D-4C4C-B2F5-B966A2E7CE1F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 94f2108a55cea520782aa5eac959195be09929b5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="installing-xamarinandroid-as-a-system-app"></a>Sistem uygulama olarak Xamarin.Android yükleme

_Bu kılavuz, bir sistem uygulaması ve bir kullanıcı uygulama ve sistem uygulaması bir Xamarin.Android uygulaması yükleme arasındaki farklar ele alınacaktır. Bu kılavuz, özel Android ROM görüntüleri yazarları için geçerlidir. Özel bir ROM oluşturulacağını açıklayan değil_

## <a name="system-app"></a>Sistem uygulama

Özel Android ROM görüntülerin yazarlar ya da Android aygıtlarını üreticilerine bir Xamarin.Android uygulaması olarak eklemek istediğiniz bir _sistem uygulama_ bir ROM veya bir aygıt dağıtırken. Sistem uygulama aygıt işlevine önemli ya da özel ROM Yazar her zaman kullanılabilir istediği işlevselliği sağlamak için kabul edilen bir uygulamadır.

Sistem uygulamaları klasörüne yüklenir **/sistem/uygulama/** (dosya sisteminde bir salt okunur dizin) ve silinemez veya kullanıcı tarafından bu kullanıcının kök erişimi olmadıkça taşındı. Buna karşılık, kullanıcı (genellikle Google Play veya dışarıdan uygulama tarafından) tarafından yüklenen bir uygulama olarak bilinen bir _kullanıcı uygulama_. Kullanıcı uygulamalar kullanıcı tarafından silinebilir ve çoğu durumda farklı bir konuma (örneğin, harici depolama çeşit) aygıtta taşınabilir.

Sistem uygulamaları tam olarak kullanıcı uygulamaları gibi davranır ancak aşağıdaki istisnalarla vardır:

- Sistem uygulamaları gibi normal bir yalnızca yükseltilebilir _kullanıcı uygulama_. Ancak, uygulama bir kopyasını her zaman varolduğundan **/sistem/uygulama/**, özgün sürümü uygulamaya geri almak her zaman mümkündür.

- Sistem uygulamaları bir kullanıcı uygulama için kullanılabilir değil belirli yalnızca sistem izinleri verilebilir. Yalnızca sistem izni örneğidir [ `BLUETOOTH_PRIVILEGED` ](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED), uygulamaları herhangi bir kullanıcı etkileşimi olmadan Bluetooth aygıtlarla eşleştirmeye izin verir.

Sistem uygulaması olarak bir Xamarin.Android uygulaması dağıtmak mümkündür. Bir APK özel ROM sağlamaya ek olarak, iki paylaşılan kitaplık vardır **libmonodroid.so** ve **libmonosgen 2.0.so** , gerekir el ile kopyalanması APK ROM görüntü genişletilemiyor için. Bu kılavuz adımlarını açıklar.

## <a name="restrictions"></a>Kısıtlamalar

Bu kılavuz, özel Android ROM görüntüleri yazarları için geçerlidir. Özel bir ROM oluşturulacağını açıklayan değil

Bu kılavuz bilindiğini varsayar [Xamarin.Android için bir yayın APK paketleme](~/android/deploy-test/publishing/index.md) anlaşılmasını [CPU mimarileri](~/android/app-fundamentals/cpu-architectures.md) Android uygulamaları için.

## <a name="install-a-xamarinandroid-app-as-a-system-app"></a>Bir sistem uygulaması bir Xamarin.Android uygulaması yükleme

Aşağıdaki adımlar, bir sistem uygulaması gibi bir Xamarin.Android uygulaması yükleme açıklar.

1. **Bir yayın APK Xamarin.Android uygulamasının paketini** &ndash; bu tarafından daha ayrıntılı anlatılan [bir uygulama yayımlama](~/android/deploy-test/publishing/index.md) Kılavuzu.

2. **APK paylaşılan kitaplıklar ayıklamak** &ndash; herhangi bir SIKIŞTIRMA yardımcı programı kullanarak APK dosyasını açın ve içeriğini inceleyin **/lib/** klasör. Bu klasör için her bir alt olacaktır _uygulama ikili arabirimi_ (ABI) uygulama tarafından desteklenir; bu klasörün içeriğini tüm bu belirli uygulamasını gerekli olan paylaşılan kitaplıklar içerir ABI:

    ![Taskypro.zip pushservice-v7a klasöründeki .so dosyaların ekran görüntüsü](install-system-app-images/install-system-app-01.png)

   Önceki ekran görüntüsünde, yalnızca bir desteklenen ABI yoktur (**pushservice-v7a**) iki bulunduran **.so** uygulama tarafından gerekli olan dosyalar. Bunu yalnızca cihaz veya cihaz ROM hedef mimarisi için uygun olduğunu, yani kopyalamayın ABI dosyalarını ayıklamak gerekli olduğuna dikkat edin **.so** dosyaları buradan **x86** klasörüne bir  **pushservice-v7a** aygıt ya da ROM'u

3. **/ Sistem/lib .so dosyaları kopyalamak** &ndash; kopya **.so** için önceki adımda APK ayıklanan dosyaları **/sistem/lib/** özel ROM klasörü

4. **APK dosya/sistem/uygulamaya kopyalayın** &ndash; APK dosyasına kopyalamak için son adımdır **/sistem/uygulama** klasöründe ROM'u


## <a name="summary"></a>Özet

Bu kılavuzda ele alınan arasındaki farkı bir _sistem uygulama_ ve _kullanıcı uygulama_ve bir sistem uygulaması gibi bir Xamarin.Android uygulaması yükleme açıklanmıştır.



## <a name="related-links"></a>İlgili bağlantılar

- [Bir uygulama yayımlama](~/android/deploy-test/publishing/index.md)
- [CPU Mimarileri](~/android/app-fundamentals/cpu-architectures.md)
- [BLUETOOTH_PRIVILEGED](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED)
- [ABI Management](https://developer.android.com/ndk~/abis.html)
