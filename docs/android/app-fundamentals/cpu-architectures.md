---
title: CPU mimarisi
description: "Xamarin.Android 32-bit ve 64-bit aygıtlar dahil olmak üzere birkaç CPU mimariyi destekler. Bu makalede, bir uygulama bir veya daha fazla CPU Android desteklenen mimariler için hedef açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4BC889D-9164-49BB-9B7B-F6C4E4E109F1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 6139b5e27e9689da6366a2107acc14a6adcfc928
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="cpu-architectures"></a>CPU mimarisi

_Xamarin.Android 32-bit ve 64-bit aygıtlar dahil olmak üzere birkaç CPU mimariyi destekler. Bu makalede, bir uygulama bir veya daha fazla CPU Android desteklenen mimariler için hedef açıklanmaktadır._

## <a name="cpu-architectures-overview"></a>CPU mimarisi genel bakış

Uygulamanızı sürüm için hazırlarken, hangi platformu CPU mimarisi belirtmelisiniz, uygulama destekler. Tek bir APK makine birden çok desteklemek için içerebilecek farklı mimari. Her mimari özgü kodu koleksiyonu ile ilişkili bir *uygulama ikili arabirimi* (ABI). Çalışma zamanında Android ile etkileşim kurmak için bu makine kodu nasıl beklenen her ABI tanımlar.
Bunun nasıl çalıştığı hakkında daha fazla bilgi için bkz: [çok çekirdekli aygıtları &amp; Xamarin.Android](~/android/deploy-test/multicore-devices.md).


## <a name="how-to-specify-supported-architectures"></a>Desteklenen mimariler belirtme

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Genellikle açıkça bir mimari (veya mimarileri) belirlediğiniz için uygulamanızı yapılandırıldığında **sürüm**. İçin uygulamanızı yapılandırıldığında **hata ayıklama**, **kullanım çalışma zamanı paylaşılan** ve **kullanım hızlı dağıtım** seçenekleri etkinleştirilmişse, açık mimari seçimi devre dışı bırakın.

Visual Studio'da çift **özellikleri** projenizde altında **Çözüm Gezgini** seçip **Android seçenekleri** sayfası. Tıklatın **paketleme** sekmesinde ve doğrulayın **kullanım çalışma zamanı paylaşılan** devre dışı bırakıldı (Bu ayarın kapatılması açıkça desteklemek için hangi ABIs seçin izin verir). Tıklatın **Gelişmiş** sekmesi ve altında **Gelişmiş Özellikler**, desteklemek istediğiniz mimarileri denetleyin:

[ ![Pushservice ve pushservice-v7a seçme](cpu-architectures-images/vs/01-abi-selections-sml.png)](cpu-architectures-images/vs/01-abi-selections.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Genellikle açıkça bir mimari (veya mimarileri) belirlediğiniz için uygulamanızı yapılandırıldığında **sürüm**. İçin uygulamanızı yapılandırıldığında **hata ayıklama**, **kullanımı paylaşılan Mono çalışma zamanı** ve **hızlı derleme dağıtım** seçenekleri etkinleştirilmişse, hangi açık mimari engelle Seçim.

Mac için Visual Studio'da projenizde bulmak **çözüm** paneli, projenizin yanındaki dişli simgesine tıklayın ve seçin **seçenekleri**. İçinde **proje seçenekleri** iletişim kutusunda, tıklatın **Android derleme**. Tıklatın **genel** sekmesinde ve doğrulayın **kullanımı paylaşılan Mono çalışma zamanı** devre dışı bırakıldı (Bu ayarın kapatılması açıkça desteklemek için hangi ABIs seçin izin verir). Tıklatın **Gelişmiş** sekmesi ve altında **desteklenen ABIs**, desteklemek istediğiniz mimari ABIs denetleyin:

[ ![Pushservice ve pushservice-v7a seçme](cpu-architectures-images/xs/01-abi-selections-sml.png)](cpu-architectures-images/xs/01-abi-selections.png)

-----


Xamarin.Android aşağıdaki mimariyi destekler:

-   **pushservice** &ndash; ARMv5TE yönerge kümesi en az destek ARM tabanlı CPU. Unutmayın `armeabi` iş parçacığı açısından güvenli değildir ve birden çok CPU cihazlarda kullanılmamalıdır.

-   **pushservice-v7a** &ndash; ARM tabanlı CPU donanım kayan nokta işlemlerini ve birden çok CPU (SMP) cihazı ile. Unutmayın `armeabi-v7a` makine kodu ARMv5 cihazlarda çalışmaz.

-   **arm64 v8a** &ndash; CPU'lar 64-bit ARMv8 mimarisine bağlı.

-   **x86** &ndash; x86 (veya IA-32) desteği CPU yönerge kümesi. Bu yönerge kümesi, Pentium MMX, SSE, SSE2 ve SSE3 yönergeleri de dahil olmak üzere Pro eşdeğerdir.

-   **x86_64** 64-bit x86 destek CPU (olarak da adlandırılan *x64* ve *AMD64*) yönerge kümesi.

Xamarin.Android varsayılanları `armeabi-v7a` için **sürüm** oluşturur. Bu ayar daha önemli ölçüde daha iyi performans sağlar `armeabi`. Bir 64-bit ARM platform (örneğin, Nexus 9) hedefliyorsanız seçin `arm64-v8a`. X x86 uygulamanızı dağıtıyorsanız aygıt, select `x86`. 64 bit CPU mimarisi hedef x86 aygıtı kullanıyorsa, seçin `x86_64`.

## <a name="targeting-multiple-platforms"></a>Birden çok platformu hedefleme

Birden fazla CPU mimari hedeflemek için birden fazla ABI (ödün verme pahasına daha büyük APK dosya boyutu) seçebilirsiniz. Kullanabileceğiniz **seçili ABI başına bir paketi (.apk) oluşturmak** seçeneği (açıklanan [paketleme özelliklerini ayarla](~/android/deploy-test/release-prep/index.md#Set_Packaging_Properties)) mimarisi desteklenen her biri için ayrı bir APK oluşturmak için.

Seçin gerekmez **arm64 v8a** veya **x86_64** 64-bit aygıtlarda; hedeflemek için 64-bit desteği 64 bit donanımda uygulamanızı çalıştırmak için gerekli değildir. Örneğin, 64-bit ARM aygıtları (gibi [Nexus 9](http://www.google.com/nexus/9/)) için yapılandırılmış uygulamaların çalıştırabilirsiniz `armeabi-v7a`. 64-bit desteği etkinleştirme birincil avantajı, daha fazla bellek adreslemek uygulamanız için olası hale getirmektir.

> [!NOTE]
> **Not:**: 64 bit çalışma zamanı desteği şu anda Deneysel bir özellik. 64 bit çalışma zamanları olduğunu unutmayın *değil* 64 bit aygıtlarda uygulamanızı çalıştırmak için gereklidir. 

## <a name="additional-information"></a>Ek Bilgiler

Bazı durumlarda, her mimari için ayrı bir APK oluşturmanız gerekebilir (sizin APK boyutunu azaltmak için veya uygulamanız paylaşılan belirli bir CPU mimarisi belirli kitaplıklar).
Bu yaklaşımı hakkında daha fazla bilgi için bkz: [yapı ABI özgü APKs](~/android/deploy-test/building-apps/abi-specific-apks.md).
