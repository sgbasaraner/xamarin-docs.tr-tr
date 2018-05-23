---
title: Mac çifti
description: Bu kılavuz, Visual Studio 2017 Mac yapı ana bilgisayara bağlanmak için Mac çiftine kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: e2f9b23bb298b0bb01f7e5491963daed4521ac9c
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2018
---
# <a name="pair-to-mac"></a>Mac çifti

_Bu kılavuz, Visual Studio 2017 Mac yapı ana bilgisayara bağlanmak için Mac çiftine kullanmayı açıklar._

## <a name="overview"></a>Genel Bakış

Yalnızca Mac'te çalışan Apple'nın derleme araçları erişim gerektiren yerel iOS uygulamaları oluşturma Bu nedenle, Visual Studio 2017 Xamarin.iOS uygulamaları oluşturmak için ağ üzerinden erişilebilen bir Mac bilgisayara bağlamanız gerekir.

Visual Studio 2017'ın çifti Mac özelliği bulur bağlanır, kimlik doğrulamasını ve Windows tabanlı iOS geliştiricileri üretken çalışabilmeniz için Mac yapı konakları hatırlıyor. 

Mac çifti aşağıdaki geliştirme iş akışını sağlar:

- Geliştiriciler Visual Studio 2017 içinde Xamarin.iOS kod yazabilirsiniz.

- Visual Studio 2017 Mac yapı konak bir ağ bağlantı açar ve derleyin ve iOS uygulamayı imzalamak için bu makinede derleme araçları kullanır.

- Mac üzerinde ayrı bir uygulamayı çalıştırmak için gerekli – Visual Studio 2017 Mac derlemeleri SSH üzerinden güvenli bir şekilde çağırır.

- Gerçekleşmeden hemen visual Studio 2017 değişiklikleri bildirilir. Örneğin, bir iOS aygıtı için Mac prize takılı veya ağda kullanılabilir hale gelir, iOS anında araç güncelleştirir.

- Visual Studio 2017 birden çok örneğini aynı anda mac'e bağlayabilirsiniz.

- İOS uygulamaları oluşturmak için Windows komut satırı kullanmak da mümkündür.

> [!NOTE]
> Bu kılavuzdaki yönergeleri izlemeden önce aşağıdaki adımları tamamlayın: 
> 
> - Bir Windows makinesinde [Visual Studio 2017 yükleyin](~/cross-platform/get-started/installation/windows.md)
> - Mac'te [yükleyin](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) ve [Mac için Visual Studio](https://docs.microsoft.com/visualstudio/mac/installation)
>
> Mac için Visual Studio yüklememeyi tercih ediyorsanız, Visual Studio 2017 Xamarin.iOS ve Mono ile otomatik olarak Mac yapı konak yapılandırabilirsiniz.
> Daha fazla bilgi için bkz: [otomatik Mac sağlama](#automatic-mac-provisioning).

## <a name="enable-remote-login-on-the-mac"></a>Mac üzerinde uzaktan oturum açmayı etkinleştirme

Mac yapı ana bilgisayar ayarlamak için önce uzak oturum açma etkinleştirin:

1. Mac sistem tercihleri açın ve gidin **paylaşım** bölmesi.

2. Denetleme **uzaktan oturum açma** içinde **hizmet** listesi.

    ![Uzak oturum açma etkinleştirme](images/sharing.png "uzaktan oturum açma etkinleştirme")

    İçin erişime izin verecek şekilde yapılandırılmış olduğundan emin olun **tüm kullanıcılar**, ya da Mac kullanıcı adınız veya Grup listesinde yer izin verilen kullanıcılar.

3. İstenirse, macOS Güvenlik Duvarı'nı yapılandırın.

    MacOS güvenlik duvarını gelen bağlantıları engellemek üzere ayarlarsanız, izin gerekebilir `mono-sgen` gelen bağlantıları almak için. Bu durumda, isteyen bir uyarı görüntülenir.

4. Windows makine aynı ağ üzerinde ise, Mac artık Visual Studio 2017 bulunabilir olması gerekir. Mac hala değil bulunabilirlik ise deneyin [el ile bir Mac ekleme](#manually-add-a-mac) veya göz atın [sorun giderme kılavuzu](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md).

## <a name="connect-to-the-mac-from-visual-studio-2017"></a>Visual Studio 2017 mac'e bağlanma

Bu uzak oturum etkin artık Mac için Visual Studio 2017 bağlanmak

1. Visual Studio 2017 ' var olan bir iOS projesi açın veya seçerek yeni bir tane oluşturun **Dosya > Yeni > Proje** ve bir iOS proje şablonu seçme.

2. Açık **Mac çiftine** iletişim. 

    - Kullanım **Mac çiftine** düğmesi iOS araç çubuğu:

        ![Mac düğmesi vurgulanmış çiftine ile iOS araç](images/ios-toolbar.png "Mac düğmesi vurgulanmış çiftine ile iOS araç çubuğu")

    - Ya da seçin **Araçlar > iOS > Mac çiftine**.

    - **Mac çiftine** iletişim tüm daha önce bağlanmış ve şu anda kullanılabilir Mac yapı ana bilgisayarların listesini görüntüler:

        ![Mac iletişim çiftine](images/pairtomac.png "Mac iletişim çifti")

3. Mac listeden seçin. **Bağlan**'a tıklayın. 

4. Kullanıcı adı ve parola girin.
    
    - Tüm paticular Mac, bağlandığınız ilk kez bu makine için kullanıcı adı ve parola girmeniz istenir:

        ![Mac için bir kullanıcı adı ve parola girme](images/auth.png "Mac için bir kullanıcı adı ve parola girme")

        > [!TIP]
        > Oturum açarken tam adı yerine, sistem kullanıcı adı kullanın.

    - Mac çifti, Mac için yeni bir SSH bağlantısını oluşturmak için bu kimlik bilgilerini kullanır. Başarılı olursa, bir anahtar eklenen **authorized_keys** Mac dosyada Aynı Mac sonraki bağlantılar otomatik olarak oturum açma.

5. Mac çiftine Mac otomatik olarak yapılandırır.

    [Visual Studio 2017 sürüm 15,6 başlangıç](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Visual Studio 2017 yükler veya Mono güncelleştirir ve Xamarin.iOS bağlı Mac'te yapı konağı olarak (Xcode hala gerekir Not elle yüklenmesi). Bkz: [otomatik Mac sağlama](#automatic-mac-provisioning) daha fazla ayrıntı için.

6. Bağlantı durumu simgesi arayın.
    
    - Ne zaman Visual Studio 2017 bağlı bir Mac, bu Mac'ın öğesinde **Mac çiftine** iletişim şu anda bağlı olduğunu belirten bir simge görüntüler:

        ![Mac bağlı](images/connected.png "Mac bağlı")

      Aynı anda yalnızca bir bağlı Mac olabilir.

      > [!TIP]
      > Tüm Mac sağ **Mac çiftine** olanak tanıyan bir bağlam menüsü liste getirir **Bağlan...** , **Bu Mac unuttunuz**, veya **bağlantısını**:
      >
      > ![Mac bağlam menülerini çiftine](images/contextmenu.png "çiftine Mac bağlam menüleri") 
      >
      > Seçerseniz **bu Mac unuttunuz**, seçili Mac için kimlik bilgilerinizi unutulursa. Bu Mac yeniden bağlanmak için kullanıcı adı ve parolanızı yeniden girmeniz gerekir.

Bir Mac yapı konağa başarıyla eşleştirilmiş değilse, Visual Studio 2017 Xamarin.iOS uygulamaları oluşturmak hazır olursunuz. Bir göz atalım [Visual Studio Xamarin.iOS için giriş](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md) Kılavuzu.

Mac eşleştirin mümkün edilmemiş, deneyin [el ile bir Mac ekleme](#manually-add-a-mac) veya göz atın [sorun giderme kılavuzu](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md).

## <a name="manually-add-a-mac"></a>El ile bir Mac Ekle

Listelenen belirli bir Mac görmüyorsanız **Mac çiftine** iletişim kutusunda, el ile ekleyin:

1. Mac'ın IP adresi bulun. 

    - Açık **sistem tercihleri > paylaşımı > Uzak oturum açma** Mac:

        [![Sistem tercihleri Mac'ın IP adresi > paylaşım](images/sharing-ipaddress.png "sistem tercihleri Mac'ın IP adresi > paylaşım")](images/sharing.png#lightbox)

    - Alternatif olarak, komut satırını kullanın. Terminale bu komutu yürütün: 
   
        ```bash
        $ ipconfig getifaddr en0
        196.168.1.8
        ```

      Ağ yapılandırmanıza bağlı olarak, bir arabirim adı dışında kullanmanız gerekebilir `en0`. Örneğin: `en1`, `en2`vb.

2. Visual Studio 2017'ın içinde **Mac çiftine** iletişim kutusunda **Mac Ekle...** :

    [![Mac iletişim çiftine Mac Ekle düğmesini](images/addtomac.png "Mac iletişim çiftine eklemek Mac düğmesi")](images/addtomac-large.png#lightbox)

3. Mac'ın IP adresi girin ve tıklayın **Ekle**:

    ![Mac'ın IP adresi girerek](images/enteripaddress.png "Mac'ın IP adresi girerek")

4. Mac için kullanıcı adı ve parola girin:

    ![Bir kullanıcı adı ve parola girin](images/auth.png "bir kullanıcı adı ve parola girme")

   > [!TIP]
   > Oturum açarken tam adı yerine, sistem kullanıcı adı kullanın.

5. Tıklatın **oturum açma** Visual Studio 2017 mac'e SSH üzerinden bağlanma ve bilinen makineler listesine ekleyin.

## <a name="automatic-mac-provisioning"></a>Mac otomatik sağlama

İle başlayarak [Visual Studio 2017 sürüm 15,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning), Mac çiftine otomatik olarak Xamarin.iOS uygulamaları oluşturmak için gerekli yazılım ile bir Mac sağlar: Mono, Xamarin.iOS (yazılım framework değil Mac IDE için Visual Studio ) ve çeşitli araçlar Xcode ile ilgili (ancak değil Xcode kendisini).

> [!IMPORTANT]
> - Mac çiftine Xcode yükleyemezsiniz; Ayrıca Mac yapı konakta onu el ile yüklemeniz gerekir. Xamarin.iOS geliştirme için gereklidir.
> - Mac otomatik sağlamayı uzaktan oturum açma Mac üzerinde etkinleştirildiğinden ve Mac ağ üzerinden erişilebilen Windows makine gerektirir. Bkz: [etkinleştirme uzaktan oturum açma Mac](#enable-remote-login-on-the-mac) daha fazla ayrıntı için.

Visual Studio 2017 olduğunda Mac çiftine gerçekleştirir gerekli yazılım yüklemeleri/güncelleştirmeleri [Mac bilgisayara bağlayarak](#connect-to-the-mac-from-visual-studio-2017).

### <a name="mono"></a>Mono

Mac çiftine Mono yüklendiğinden emin olmak için kontrol eder. Yüklü değilse, Mac çiftine indirin ve Mac üzerinde Mono en son kararlı sürümü yükleyin 

İlerleme durumu, aşağıdaki ekran görüntüleri (yakınlaştırma için tıklatın) tarafından gösterildiği gibi çeşitli istemleri tarafından belirtilir:

||Onay yükleyin|İndirme|Yükleme
|---|---|---|---|
|Mono|[![Mono yüklemesi eksik](images/mono-missing.png "eksik Mono yükleme")](images/mono-missing-large.png#lightbox)|[![Mono indirme](images/mono-downloading.png "Mono indirme")](images/mono-downloading-large.png#lightbox)|[![Mono yükleme](images/mono-installing.png "Mono yükleme")](images/mono-installing-large.png#lightbox)|

### <a name="xamarinios"></a>Xamarin.iOS 

Mac çiftine Windows makinesinde yüklü olan sürümle eşleştirmek için Mac üzerinde Xamarin.iOS yükseltir.

> [!IMPORTANT]
> Mac çiftine alfa/beta tarafından yapılan kararlı Mac üzerinde Xamarin.iOS düşürmek değil. For Mac yüklü Visual Studio varsa ayarlayın, [yayın kanal](https://docs.microsoft.com/visualstudio/mac/update) gibi:
> - Visual Studio 2017 kullanıyorsanız seçin **kararlı** Visual Studio for Mac güncelleştirmelerini kanal
> - Visual Studio 2017 Preview kullanıyorsanız seçin **alfa** Visual Studio for Mac güncelleştirmelerini kanal

İlerleme durumu, aşağıdaki ekran görüntüleri (yakınlaştırma için tıklatın) tarafından gösterildiği gibi çeşitli istemleri tarafından belirtilir:

||Onay yükleyin|İndirme|Yükleme
|---|---|---|---|
|Xamarin.iOS|[![Xamarin.iOS yüklemesi eksik](images/xamios-missing.png "eksik Xamarin.iOS yükleme")](images/xamios-missing-large.png#lightbox)|[![Xamarin.iOS indirme](images/xamios-downloading.png "Xamarin.iOS indirme")](images/xamios-downloading-large.png#lightbox)|[![Xamarin.iOS yükleme](images/xamios-installing.png "Xamarin.iOS yükleme")](images/xamios-installing-large.png#lightbox)|

### <a name="xcode-tools-and-license"></a>Xcode araçlarını ve lisans

Ayrıca, Mac çiftine Xcode yüklü lisansını kabul olup olmadığını belirlemek için kontrol eder. Mac çiftine Xcode yüklemez olsa da, aşağıdaki ekran görüntülerinde (yakınlaştırma için tıklatın) gösterildiği gibi lisans kabulünü için iste:

||Onay yükleyin|Lisans Kabulünü|
|---|---|---|
|Xcode|[![Xcode yüklemesi eksik](images/xcode-missing.png "eksik Xcode yükleme")](images/xcode-missing-large.png#lightbox)|[![Xcode lisans](images/xcode-license.png "Xcode lisans")](images/xcode-license-large.png#lightbox)|

Ayrıca, Mac çiftine yükleyin veya Xcode ile dağıtılmış çeşitli paketler güncelleştirin. Örneğin:

- **MobileDeviceDevelopment.pkg**
- **XcodeExtensionSupport.pkg**
- **MobileDevice.pkg**
- **XcodeSystemResources.pkg**

Bu paketleri yükleme, bir istem duymadan ve hızla gerçekleşir.

> [!NOTE]
> Bu araçları Xcode komut satırı olan macOS 10.9 itibariyle araçlarından, ayrı [Xcode ile yüklü](https://developer.apple.com/library/content/technotes/tn2339/_index.html).

### <a name="troubleshooting-automatic-mac-provisioning"></a>Mac otomatik sağlama sorunlarını giderme

Otomatik sağlama Mac kullanarak güçlükle karşılaşırsanız, depolanmış Visual Studio 2017 IDE günlüklerini bakalım **%LOCALAPPDATA%\Xamarin\Logs\15.0**. Bu günlükler daha iyi hatayı tanılamak ve destek alma yardımcı olmak için hata iletileri içerebilir.

## <a name="build-ios-apps-from-the-windows-command-line"></a>İOS uygulamaları Windows komut satırı derleme 

Xamarin.iOS uygulamaları komut satırından derleme Mac destekler çifti. Örneğin:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```
Parametreleri geçirilen `msbuild` yukarıdaki örnekte şunlardır:

- `ServerAddress` – Mac yapı konak IP adresi.
- `ServerUser` – Mac yapı konağı açarken kullanılacak kullanıcı adı.
  Tam adınızı yerine sistem kullanıcı adınızı kullanın.
- `ServerPassword` – Mac yapı konağı açarken kullanılacak parola.
 
> [!NOTE]
> Visual Studio 2017 depolar `msbuild` aşağıdaki dizinde: **C:\Program Files (x86) \Microsoft Visual Studio\2017\<sürüm > \MSBuild\15.0\Bin**

Visual Studio 2017 ya da komut satırı, belirli bir Mac yapı konağa Mac çiftine açtığında ilk kez SSH anahtarlarını ayarlar. Bu anahtarları ile sonraki oturum açma bilgileri, bir kullanıcı adı veya parola gerektirmez. Yeni oluşturulan anahtarları depolanır **%LOCALAPPDATA%\Xamarin\MonoTouch**.

Varsa `ServerPassword` parametresi, bir komut satırı derleme çağrısından atlanırsa, kaydedilmiş SSH anahtarları kullanılarak Mac yapı konağına oturum açma girişimlerini Mac çifti.

## <a name="summary"></a>Özet

Bu makalede Mac çiftine Xamarin.iOS yerel iOS uygulamaları oluşturmak Visual Studio 2017 geliştiriciler etkinleştirme Mac yapı konak, Visual Studio 2017 bağlanmak için nasıl kullanılacağı açıklanmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

- [Bağlantı Sorunlarını Giderme](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin Mac yapı aracısını - Xamarin Üniversitesi Şimşek Ders](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
- [Visual Studio için Xamarin.iOS’a Giriş](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [Düğümlerde iOS simülatörü için Windows](~/tools/ios-simulator.md)
- [Kablosuz Dağıtım](~/ios/deploy-test/wireless-deployment.md)

