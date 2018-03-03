---
title: "Kablosuz dağıtma"
description: "Bu önizleme özelliği iOS veya Apple TV cihazlara dağıtım için bir ağ bağlantısı üzerinden sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AB4C5A9-4FBB-4DCB-BD72-0022D5439E65
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/09/2018
ms.openlocfilehash: 11961a21a7c4188c505c822a35531036fd953405
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="wireless-deployment"></a>Kablosuz dağıtma

_Bu önizleme özelliği iOS veya Apple TV cihazlara dağıtım için bir ağ bağlantısı üzerinden sağlar._

![Önizleme sürümü](~/media/shared/preview.png)

Geliştirici iş akışı önemli bir kısmını bir aygıta dağıtıyor. Xcode 9 dağıtmak ve uygulamanızın hatalarını ayıklama istediğiniz her zaman için donanım aygıtlarınızın sahip olmak yerine bir iOS cihazı veya Apple TV için bir ağ üzerinden dağıtma seçeneği sunulmuştur. Bu özellik şu anda önizlemede değil, Mac ve Visual Studio 15,6 sürümü için Visual Studio'da sunulmuştur.

Bu kılavuzda eşleştirin ve ağ üzerinden bir aygıta dağıtmak nasıl ayrıntıları verilmektedir.

## <a name="requirements"></a>Gereksinimler

Kablosuz dağıtım olarak kullanılabilir bir **Önizleme** özelliği Mac için Visual Studio ve Visual Studio.


Kablosuz dağıtımını kullanmak için aşağıdakilere sahip olmanız gerekir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

- macOS 10.12.4
- Mac için Visual Studio en son Önizleme sürümü 
    - Bu anahtarı yüklemek için [alfa veya Beta kanal](https://docs.microsoft.com/en-us/visualstudio/mac/update) Mac için Visual Studio'da
- Xcode 9.0 veya üzeri
- Bir aygıtla iOS 11.0 veya tvOS 11.0 ve üzeri

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- En son [Önizleme sürümü](https://www.visualstudio.com/vs/preview/) Visual Studio
- Bir aygıtla iOS 11.0 veya tvOS 11.0 ve üzeri

Mac yapı ana bilgisayarda aşağıdaki bileşenler yüklü olmalıdır:

- macOS 10.12.4
- Visual Studio için Mac Önizleme
    - Anahtara yüklemek için [alfa veya Beta kanal](https://docs.microsoft.com/en-us/visualstudio/mac/update) Mac için Visual Studio'da
- Xcode 9.0 veya üzeri

-----

## <a name="connecting-a-device"></a>Bir aygıt bağlanma

Dağıtma ve kablosuz Cihazınızda hata ayıklamak için iOS cihazı veya Apple TV Xcode ile Mac üzerindeki eşleştirin gerekir Eşleştirilmiş sonra Visual Studio aygıt hedef listesinden seçebilirsiniz. 

Aşağıdaki eşleştirme işlemi, yalnızca cihaz başına bir kez yapılması gerekir. Xcode bağlantı ayarlarını saklar.

<a name="pair" />

### <a name="pairing-an-ios-device-with-xcode"></a>Xcode ile iOS cihazı eşleştirme

1. Xcode açın ve gidin **penceresi > aygıtlar ve benzeticileri**.
2. İOS Cihazınızı Şimşek kablosu kullanarak Mac takın. İçin seçmek için gerek duyabileceğiniz **güven bu bilgisayar** Cihazınızda.
3. Cihazınızı seçin ve ardından **Connect ağ üzerinden** Cihazınızı eşleştirin için onay kutusunu: ![Bağlan ağ seçeneği gösteren cihaz ve Simulator penceresi](wireless-deployment-images/image2.png)

### <a name="pairing-an-apple-tv-with-xcode"></a>Apple TV Xcode ile eşleştirme

1. Mac ve Apple TV aynı ağa bağlı olduğunuzdan emin olun.

2. Xcode açın ve gidin **penceresi > aygıtlar ve benzeticileri**.

3. Apple TV Git **ayarlar > uzaktan kumandalar ve cihazlar > Uzak uygulama ve cihazlar**.

4. Apple TV seçin **keşfedilen** Xcode alanında ve Apple TV görüntülenen doğrulama kodunu girin.

5. Tıklatın **Bağlan** düğmesi. Başarıyla eşleştirilmiş, Apple TV bir ağ bağlantısı simgesi görünür.

## <a name="deploy-to-a-device"></a>Bir aygıta dağıtmak

Ne zaman bir aygıt kablosuz olarak bağlı ve cihazın USB bağlıymış gibi dağıtım için kullanılacak hazır, aygıt hedef listesinde gösterir.

Fiziksel cihaz üzerindeki test etmek için cihaz olmalıdır [sağlanan](~/ios/get-started/installation/device-provisioning/index.md). Bir cihaza dağıtmayı denemeden önce bunu emin olun. 

Bir iOS veya tvOS aygıta dağıtmak için aşağıdaki adımları kullanın:

1. Dağıtım makine ve hedef aygıt aynı kablosuz ağda olduğundan emin olun. 

2. Hedef aygıt listeden Cihazınızı seçin ve uygulamayı çalıştırın.

2. Aygıtınız kilitliyse, cihazın kilidini açmak için istenir. Cihaz kilitli olduğunda, uygulamanızın cihaza dağıtılır.

Daha önce kümesi kesme noktaları kullanın ve her zaman yaptığınızı olarak hata ayıklama akışınıza devam kablosuz hata ayıklama kablosuz dağıtım sonrasında otomatik olarak etkinleştirilir.

## <a name="troubleshooting"></a>Sorun giderme

1. İOS cihazı veya Apple TV bağlandığını Mac aynı ağa her zaman olun

2. Cihaz Visual Studio'da göstermez, Xcode'nın denetleyin **aygıtlar ve benzeticileri** penceresi. 

    * Xcode **yok** bağlı olarak, Cihazınızı çalışın Göster [çifti](#pair) Cihazınızı yeniden.

    * Xcode bağlı olarak cihaz gösteriyorsa, Visual Studio ve aygıtınızı yeniden başlatmayı deneyin.

3. Henüz yapmadıysanız, gerekecek [sağlama](~/ios/get-started/installation/device-provisioning/index.md) Cihazınızı.

4. Önceki adımlarla sabit bu özellik bir sorun varsa, Lütfen bir sorun dosya [Geliştirici topluluğu](https://developercommunity.visualstudio.com/spaces/41/index.html).

## <a name="related-links"></a>İlgili bağlantılar

- [Kablosuz aygıt Xcode ile eşleştirin](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad)
