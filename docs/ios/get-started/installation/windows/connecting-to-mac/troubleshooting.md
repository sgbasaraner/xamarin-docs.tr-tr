---
title: Bağlantı için bir Xamarin.iOS yapı konak sorunlarını giderme
description: Bu kılavuz, bağlantı ve SSH sorunlar da dahil olmak üzere Yeni Bağlantı Yöneticisi'ni kullanarak karşılaşılabilir sorunlarıyla ilgili sorun giderme adımları sağlar.
ms.prod: xamarin
ms.assetid: A1508A15-1997-4562-B537-E4A9F3DD1F06
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 575e6705679539af6d3e5fae3ffc5721d9f79ba6
ms.sourcegitcommit: c2d1249cb67b877ee0d9cb8d095ec66fd51d8c31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291054"
---
# <a name="connection-troubleshooting-for-a-xamarinios-build-host"></a>Bağlantı için bir Xamarin.iOS yapı konak sorunlarını giderme

_Bu kılavuz, bağlantı ve SSH sorunlar da dahil olmak üzere Yeni Bağlantı Yöneticisi'ni kullanarak karşılaşılabilir sorunlarıyla ilgili sorun giderme adımları sağlar._

## <a name="log-file-location"></a>Günlük dosyası konumu

- **Mac** – ~/Library/Logs/Xamarin-[büyük. İKİNCİL]
- **Windows** – %LOCALAPPDATA%\Xamarin\Logs

Günlük dosyaları göz atarak konumlandırılabilir **yardımcı &gt; Xamarin &gt; Zip günlükleri** Visual Studio.


## <a name="wheres-the-xamarin-build-host-app"></a>Xamarin derleme uygulama ana bilgisayar?

Xamarin derleme Konaktan Xamarin.iOS eski sürümleri artık gerekli değildir. Visual Studio artık otomatik olarak Aracısı uzak oturum açma dağıtır ve arka planda çalışır. Mac veya Windows makinelerde çalışacaktır hiçbir ek uygulaması yoktur.


## <a name="troubleshooting-remote-login"></a>Uzak oturum açma sorunlarını giderme

> [!IMPORTANT]
> Sorun giderme adımları öncelikle yeni bir sistemde ilk kurulumu sırasında gerçekleşen sorunları yöneliktir.  Daha önce bağlantısı başarıyla belirli bir ortamda kullanmış ve ardından aniden veya aralıklı bağlantısı bu çalışmayı durduruyor, aşağıdakilerden herhangi birini yardımcı olur, doğrudan denetimini (çoğu durumda) atlayabilirsiniz: 
>   * Kalan işlemler altında aşağıda açıklandığı gibi KILL [hataları var olan yapı konak işlemleri nedeniyle](#errors). 
>   * Aracıları altında açıklandığı gibi temizleyin [Aracısı, IDB, yapı ve Tasarımcısı aracıları Temizleme](#clearing), kablolu bir Internet bağlantısı kullanır ve altında açıklandığı gibi doğrudan IP adresi bağlanma [için bağlanılamadı MacBuildHost.local. Lütfen yeniden deneyin. ](#tryagain).  
> Ardından bu seçenekleri hiçbiri sorunu düzeltin, lütfen'ndaki yönergeleri izleyin [9. adım](#stepnine) yeni bir hata raporu dosya.

1. Uyumlu Xamarin.iOS sürümlerin Mac üzerinde yüklü olup olmadığını denetleyin Yapmak için bu Visual Studio 2017 ile üzerinde olduğundan emin olun **kararlı** dağıtım kanal Mac için Visual Studio'da Visual Studio 2015'te ve önceki her iki IDE üzerinde aynı dağıtım kanalında olduğundan emin olun.
    * Mac için Visual Studio'da Git **Mac için Visual Studio > Güncelleştirmeleri denetle...**  görüntülemek veya değiştirmek için **güncelleştirme kanalı**.
    * Visual Studio 2015 ve önceki sürümlerinde, dağıtım kanal altında denetleyin **Araçlar > Seçenekler > Xamarin > diğer**.

2. Olduğundan emin olun **uzaktan oturum açma** Mac üzerinde etkin Ayarlamak için erişim **yalnızca bu kullanıcılar**ve Mac kullanıcı liste veya grup dahil olduğundan emin olun:

    [![](troubleshooting-images/troubleshooting-image1.png "Yalnızca bu kullanıcılar için erişimi ayarlama")](troubleshooting-images/troubleshooting-image1.png#lightbox)

3. Güvenlik duvarınızın 22 - SSH için varsayılan bağlantı noktası üzerinden gelen bağlantılara izin verdiğini kontrol edin:

    [![](troubleshooting-images/troubleshooting-image2.png "Güvenlik Duvarı bağlantı noktası 22 üzerinden gelen bağlantılara izin verdiğini denetleyin")](troubleshooting-images/troubleshooting-image2.png#lightbox)

    Devre dışı bıraktıysanız **otomatik olarak imzalanan yazılımın gelen bağlantıları almak izin ver**, OS X izin vermek isteyen eşleştirme işlemi sırasında bir iletişim kutusu düzenleyecek `mono-sgen` veya `mono-sgen32` gelen bağlantıları almak için. Tıklattığınızdan emin olun **izin** bu iletişim kutusunda:

    [![](troubleshooting-images/troubleshooting-image4a.png "Bu iletişim kutusunda ver")](troubleshooting-images/troubleshooting-image4a.png#lightbox)

4. Bu Mac kullanıcı hesabında oturum açmış ve etkin bir GUI oturuma sahip olduğunu onaylayın.

5. Mac ile bağlandığı emin olun _kullanıcıadı_ yerine _tam adı_. Bu, aksanlı karakterler içeren tam adları için bilinen bir sınırlama önler.

    Bulabilirsiniz, _kullanıcıadı_ çalıştırarak `whoami` komutunu **Terminal.app**.

    Örneğin, aşağıdaki ekran görüntüsünde hesap adı olacaktır **amyb** ve **silinecektir yanıklara**:

    [![](troubleshooting-images/troubleshooting-image5a.png "Terminal uygulamadan hesabı adı alınıyor")](troubleshooting-images/troubleshooting-image5a.png#lightbox)


6. Mac için kullandığınız IP adresinin doğru olduğunu denetleyin. IP adresi altında bulabilirsiniz **sistem tercihleri > paylaşım > Uzak oturum açma** Mac üzerinde

    [![](troubleshooting-images/troubleshooting-image17.png "Sistem tercihleri uygulama IP adresi")](troubleshooting-images/troubleshooting-image17.png#lightbox)

7. Mac adresini doğruladıktan sonra deneyin bir `ping` bu adrese `cmd.exe` Windows'da:

    ```
    ping 10.1.8.95
    ```
    
    Ping işlemi başarısız olduysa Mac değil _yönlendirilebilir_ Windows bilgisayardan. Bu sorunu 2 bilgisayarlar arasında yerel ağ yapılandırmasının düzeyinde çözülmesi gerekir. Her iki makinelerin aynı yerel ağ üzerinde olduğundan emin olun.

8. Ardından, kapatılmadığını `ssh` OpenSSH istemciden bağlanabilir başarıyla mac'e Windows'dan. Bu programı yüklemek için bir yoldur yüklemek için [Windows için Git](https://git-for-windows.github.io/). Daha sonra başlatabilirsiniz bir **Git Bash** komut istemi ve girişimi `ssh` , kullanıcı adınızı ile Mac ve IP adresi:

    ```bash
    ssh amyb@10.1.8.95
    ```
    <a name="stepnine" />

9. Varsa **8. adım başarılı**, gibi basit bir komut çalıştırmayı deneyebilirsiniz `ls` bağlantı üzerinden:

    ```bash
    ssh amyb@10.1.8.95 'ls'
    ```
    
    Bu giriş dizininize Mac üzerinde içeriğini listele Varsa `ls` komutu doğru çalışır ancak Visual Studio bağlantı hala, kontrol edebilirsiniz başarısız [bilinen sorunlar ve sınırlamalar](#knownissues) için Xamarin belirli zorluklar ilgili bölümü. Hiçbiri bu sorununuzu eşleşirse, lütfen Geliştirici topluluğu üzerinde yeni bir hata raporu giderek dosya **Yardım > geri bildirim gönder > bir sorun bildirmek** Visual Studio ve altında açıklanan günlükleri ekleme [ayrıntılı günlüğü'nü denetleyin Dosyaları](#verboselogs).

10. Varsa **8 başarısız adım**, terminale Mac SSH sunucusu kabul etmiş olup olmadığını görmek için aşağıdaki komutu çalıştırabilirsiniz _herhangi_ bağlantıları:

    ```bash
    ssh localhost
    ```
    
11. 8. adım başarısız olursa, ancak **adım 10 başarılı**, ardından bağlantı noktası 22 Mac yapı konaktaki ağ yapılandırması nedeniyle Windows'dan erişilebilir değil büyük olasılıkla bir sorundur. Olası yapılandırma sorunlarını şunları içerir:

    - OS X Güvenlik Duvarı ayarlarını bağlantı vermemek. 3. adım iki kez kontrol emin olun.

        Bazen OS X Güvenlik Duvarı için uygulama başına yapılandırması da burada sistem tercihlerinde gösterilen ayarları gerçek davranışını yansıtmaz geçersiz bir durumda düşebilir. Yapılandırma dosyası silinirken (**/Library/Preferences/com.apple.alf.plist**) ve bilgisayar yeniden başlatıldığı yardımcı varsayılan davranışı geri yükleyin. Dosyayı silmek için bir yoldur girmek için **/Library/Preferences** altında **Git &gt; klasörüne gidin** Bulucu ve ardından taşıyın **com.apple.alf.plist** dosya Çöp kutusu.

    - Yönlendiriciler Mac ve Windows bilgisayarı arasında bir güvenlik duvarı ayarlarını bağlantıyı engelliyor.

    - Windows'un kendisi, uzak bağlantı noktası 22 giden bağlantılara izin vermeme. Bu, olağan dışı olacaktır. Windows Güvenlik Duvarı'nı giden bağlantılara izin vermeyecek şekilde yapılandırılması mümkündür, ancak tüm giden bağlantılara izin vermek için varsayılan ayardır.

    - Mac yapı ana bilgisayar bağlantı noktası 22 tüm dış ana bilgisayarlardan erişmesine izin vermeme bir `pfctl` kuralı. Yapılandırdığınız bilmiyorsanız bu düşüktür `pfctl` geçmişte.

12. 8. adım başarısız olursa ve **10 başarısız adım**, sonra sorun Mac üzerinde SSH sunucu işlemi çalışmıyor veya geçerli kullanıcının oturum açma izin vermek için yapılandırılmamış olabilir. Bu durumda, daha karmaşık bir olasılık araştırmanız önce adım 2 uzaktan oturum açma ayarları iki kez kontrol emin olun.

<a name="knownissues" />

### <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

> [!NOTE]
> Bu bölüm, yalnızca başarıyla Mac kullanıcı adı ve parolayı OpenSSH SSH istemcisini kullanarak Mac yapı konağa 8 ve 9 Yukarıdaki adımlarda anlatıldığı gibi bağladınız mı geçerlidir.

#### <a name="invalid-credentials-please-try-again"></a>"Geçersiz kimlik bilgileri. Lütfen yeniden deneyin."

Bilinen nedenler:

- **Sınırlama** – hesabı kullanarak yapı ana bilgisayara oturum açmak çalışırken bu hata görüntülenebilir _tam adı_ adı vurgulu bir karakter içeriyorsa. Bu bir kısıtlamasıdır [SSH.NET kitaplığı](https://sshnet.codeplex.com/) SSH bağlantısı için Xamarin kullanan. **Geçici çözüm**: adım yukarıdaki 5'in bakın.

#### <a name="unable-to-authenticate-with-ssh-keys-please-try-to-log-in-with-credentials-first"></a>"SSH anahtarları ile kimlik doğrulaması yapılamıyor. Lütfen ilk kimlik bilgileriyle oturum deneyin"

Bilinen neden:

- **SSH güvenlik kısıtlaması** – bu ileti genellikle bir dosya veya tam yolunu dizinlerde anlamına gelir **$HOME/.ssh/authorized\_anahtarları** Mac içinetkinyazmaizinlerinesahip_diğer_ veya _grup_ üyeleri. **Ortak düzeltme**: çalıştırmak `chmod og-w "$HOME"` Mac üzerinde Terminal komut istemindeki Hangi belirli dosya veya dizin hakkındaki ayrıntılar için çalıştırın soruna neden olan `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` Terminal ve ardından açık **sshd.log** dosya masaüstünüzden ve Ara "kimlik doğrulaması reddedildi: hatalı sahipliği veya modları".

#### <a name="trying-to-connect-never-completes"></a>"... Bağlanmaya" hiçbir zaman tamamlanmıyor

- **Hata [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**  – Bu sorun üzerinde Xamarin 4.1 durum **oturum açma kabuğu** içinde **Gelişmiş Seçenekler** Mac kullanıcı için bağlam menüsü  **Sistem tercihleri &gt; kullanıcılar &amp; grupları** dışında bir değere ayarlanır **/bin/bash**. (Xamarin 4.2 ile başlayarak, bu senaryo bunun yerine "bağlanamadı" hata iletisine yol açar.) **Geçici çözüm**: değişiklik **oturum açma kabuğu** özgün varsayılan dön **/bin/bash**.

<a name="tryagain" />

#### <a name="couldnt-connect-to-macbuildhostlocal-please-try-again"></a>"MacBuildHost.local için bağlanamadı. Lütfen yeniden deneyin."

Bildirilen nedenler:

- **Hata** – birkaç kullanıcı günlük dosyalarındaki daha ayrıntılı bir hata ile birlikte "beklenmeyen bir hata oluştu SSH kullanıcı için yapılandırılırken... Bu hata iletisini gördünüz Oturum işlemi zaman aşımına uğradı"bir Active Directory ya da diğer dizin hizmeti etki alanı kullanıcı hesabı kullanarak yapı ana bilgisayara oturum açma girişiminde bulunulduğunda. **Geçici çözüm:** yerel bir kullanıcı hesabı kullanmayı yapı ana bilgisayara oturum açın.

- **Hata** – bazı kullanıcılar bu hata bağlantı iletişim kutusunda Mac adını çift tıklatarak yapı ana bilgisayara bağlanmaya çalışılırken gördünüz. **Olası geçici çözüm**: [Mac el ile eklemeniz](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manually-add-a-mac) IP adresini kullanarak.

- **Hata [#35971](https://bugzilla.xamarin.com/show_bug.cgi?id=35971)**  – bazı kullanıcılar, Windows ve Mac yapı ana bilgisayar arasında bir kablosuz ağ bağlantısı kullanırken bu hatası arasında çalıştırma. **Olası geçici çözüm**: her iki bilgisayar kablolu ağ bağlantısı taşıyın.

- **Hata [#36642](https://bugzilla.xamarin.com/show_bug.cgi?id=36642)**  – üzerinde Xamarin 4.0, bu iletiyi dilediğiniz zaman görünür **$ giriş/.bashrc** Mac dosyasını içeren bir hata oluştu. (Xamarin 4.1, hataları başlayarak **.bashrc** dosya artık bağlantı işlemini etkiler.) **Geçici çözüm**: taşıma **.bashrc** dosya yedekleme konumuna (veya onu gerekmez biliyorsanız Sil).

- **Hata [#52264](https://bugzilla.xamarin.com/show_bug.cgi?id=52264)**  – bu hata görüntülenebilir **oturum açma kabuğu** içinde **Gelişmiş Seçenekler** Mac kullanıcı için bağlam menüsünde **sistem Tercihler > kullanıcıları ve grupları** dışında bir değere ayarlanır **/bin/bash**. **Geçici çözüm**: değişiklik **oturum açma kabuğu** özgün varsayılan dön **/bin/bash**.

- **Sınırlama** – bu hata, internet erişimi olan bir yönlendirici Mac yapı konak bağlıysa (veya Mac Windows bilgisayarının geriye doğru DNS araması için sorulduğunda zaman aşımına uğruyor bir DNS sunucusu kullanıyorsanız) ortaya çıkabilir. Visual Studio SSH parmak izi almak için yaklaşık 30 dakika sürer ve sonunda bağlanma başarısız.

    **Olası geçici çözüm**: Ekle "UseDNS yok" için **sshd\_config** dosya. Bu SSH ayarını değiştirmeden önce hakkında okuduğunuzdan emin olun. Örneğin bkz [unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option](http://unix.stackexchange.com/questions/56941/what-is-the-point-of-sshd-usedns-option).

    Aşağıdaki adımlar, ayarını değiştirmek için bir yol açıklamaktadır. Mac adımları tamamlamak için bir yönetici hesabı için oturum açmanız gerekir.

    1. Konumunu onaylayın **sshd\_config** çalıştırarak dosya `ls /etc/ssh/sshd_config` ve `ls /etc/sshd_config` Terminal bir komut isteminde. Tüm geri kalan adımları için mu konumu kullandığınızdan emin olun _değil_ "böyle dosya veya dizin yok" döndürür.

        [![](troubleshooting-images/troubleshooting-image18.png "'Ls /etc/ssh/sshd_config' ve 'ls /etc/sshd_config' terminale çalıştırma")](troubleshooting-images/troubleshooting-image18.png#lightbox)

    3. Çalıştırma `cp /etc/ssh/sshd_config "$HOME/Desktop/"` Terminal dosyayı masaüstünüze kopyalayın.

    4. Bir metin düzenleyicisinde masaüstünüzden dosyasını açın. Örneğin, çalıştırabilirsiniz `open -a TextEdit "$HOME/Desktop/sshd_config"` Terminal.

    5. Aşağıdaki satırı dosyanın sonuna ekleyin:

        ```
        UseDNS no
        ```
        
    6. Söyleyin satırları kaldırın `UseDNS yes` yeni ayar etkili olur emin olmak için.

    7. Dosyayı kaydedin.

    8. Çalıştırma `sudo cp "$HOME/Desktop/sshd_config" /etc/ssh/sshd_config` düzenlenen dosyayı geri yerine kopyalamak için Terminal. İstenirse, parolanızı girin.

    9. Devre dışı bırakın ve yeniden etkinleştirmeniz **uzaktan oturum açma** altında **sistem tercihleri &gt; paylaşım &gt; uzaktan oturum açma** SSH sunucuyu yeniden başlatmanız.

<a name="clearing" />

#### <a name="clearing-the-broker-idb-build-and-designer-agents-on-the-mac"></a>Aracısı, IDB, yapı ve Mac Tasarımcı aracısında temizleme

Günlük dosyalarınızı bir sorun "yükleme sırasında", Göster "Karşıya" veya "Başlangıç" adımlar herhangi bir Mac aracıları için silmeyi deneyebilirsiniz **XMA** yeniden yüklemek için Visual Studio zorlamak için Önbellek klasörü.

1. Aşağıdaki komutu terminale Mac üzerinde çalıştırın:

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```
    
2. Denetim tıklatın **XMA** klasörü ve select **taşımak için çöp**:

    [![](troubleshooting-images/troubleshooting-image8.png "Çöp kutusu için XMA klasörünü taşıma")](troubleshooting-images/troubleshooting-image8.png#lightbox)

3. Temizlemek için yardımcı olabilecek bir önbellek Windows de yoktur. Windows üzerinde yönetici olarak bir komut istemi açın:

    ```
    del %localappdata%\Temp\Xamarin\XMA
    ```
    
### <a name="warning-messages"></a>Uyarı iletileri

Bu bölümde çıkış penceresine ve genellikle yoksayabilirsiniz günlükleri görünebilir birkaç iletileri anlatılmaktadır.

#### <a name="there-is-a-mismatch-between-the-installed-xamarinios--and-the-local-xamarinios"></a>"... Yüklü Xamarin.iOS ve yerel Xamarin.iOS arasında bir uyuşmazlık var."

Mac ve Windows aynı Xamarin dağıtım kanala güncelleştirilir onayladıktan sürece, bu uyarı yoksayılabilir.

#### <a name="failed-to-execute-ls-usrbinmono-exitstatus1"></a>"'Ls /usr/bin/mono' yürütülemedi: ExitStatus = 1"

Mac OS X 10.11 sürümünü (El Capitan) çalıştığı sürece bu ileti yoksayılabilir veya daha yeni. Xamarin de denetler bu iletiyi OS X 10.11 sürümünü bir sorunu olmadığından **/usr/local/bin/mono**, doğru olduğu için konumu beklenen `mono` OS X 10.11 sürümünü üzerinde.

#### <a name="bonjour-service-macbuildhost-did-not-respond-with-its-ip-address"></a>"Bonjour hizmet 'MacBuildHost' kendi IP adresiyle yanıt vermedi."

Bağlantı iletişim kutusu Mac yapı ana bilgisayarın IP adresi görüntülemez fark sürece bu ileti yoksayılabilir. IP adresi _olan_ bu iletişim kutusunda eksik, yine [Mac el ile eklemeniz](~/ios/get-started/installation/windows/connecting-to-mac/index.md#manually-add-a-mac).

#### <a name="invalid-user-a-from-101895-and-inputuserauthrequest-invalid-user-a-preauth"></a>"Geçersiz kullanıcı bir 10.1.8.95 öğesinden" ve "Giriş\_userauth\_isteği: geçersiz kullanıcı bir [ön kimlik doğrulama]"

Bu iletileri bakarsanız fark edebilirsiniz **sshd.log**. Bu iletiler normal bağlantı işleminin bir parçasıdır. Xamarin kullanıcıadı kullandığından göründükleri **bir** geçici olarak alınırken _SSH parmak izi_.

### <a name="output-window-and-log-files"></a>Çıktı penceresi ve günlük dosyaları

Visual Studio derleme konağa bağlanırken bir hata değerse, ek iletiler için denetlemek için 2 konumları vardır: Çıktı penceresi ve günlük dosyaları.

#### <a name="output-window"></a>Çıktı Penceresi

Çıktı penceresi başlatmak için en iyi yerdir. Ana bağlantı adımları ve hataları ile ilgili iletileri görüntüler. Çıktı penceresinde Xamarin iletilerini görüntülemek için:

1. Seçin **Görünüm > çıkış** menülerden veya tıklatın **çıkış** sekmesi.
2. Tıklatın **Göster çıktı** açılır menü.
3. Seçin **Xamarin**.

[![](troubleshooting-images/troubleshooting-image11.png "Xamarin Çıkış sekmesinde seçin")](troubleshooting-images/troubleshooting-image11.png#lightbox)

#### <a name="log-files"></a>Günlük dosyaları

Çıktı penceresi sorunu tanılamak için yeterli bilgi içermiyorsa, günlük dosyalarını aramak için sonraki yerdir. Günlük dosyaları çıktı penceresinde görünmeyen ek tanılama iletileri içerir. Günlük dosyalarını görüntülemek için:

1. Visual Studio'yu başlatın.

    > [!IMPORTANT]
    > Unutmayın **.svclogs** varsayılan olarak etkin değildir. Bunlara erişmek için Visual Studio içinde açıklandığı gibi ayrıntılı günlükleri ile başlayın gerekecektir [sürüm günlükleri](~/cross-platform/troubleshooting/questions/version-logs.md#visual-studio-startup-verbose-logs) Kılavuzu. Daha fazla bilgi için bkz [etkinlik günlüğü sorunlarını giderme uzantılarıyla](https://blogs.msdn.microsoft.com/visualstudio/2010/02/24/troubleshooting-extensions-with-the-activity-log/) blogu.

2. Yapı ana bilgisayara bağlanmaya çalışır.

3. Visual Studio bağlantı hatası isabetler sonra günlüklerinden toplamak **Yardım > Xamarin > Zip günlükleri**:

    [![](troubleshooting-images/troubleshooting-image12.png "Yardım'dan günlükleri toplamak > Xamarin > Zip günlükleri")](troubleshooting-images/troubleshooting-image12.png#lightbox)

4. .Zip dosyasını açtığınızda, aşağıdaki örneğe benzer dosyaların bir listesini görürsünüz. Bağlantı hataları için en önemli dosyalardır  **\*Ide.log** ve  **\*Ide.svclog** dosyaları. Bu dosyalar iki biraz farklı biçimlerde aynı iletilerini içerir. **.Svclog** XML ve iletileri aracılığıyla göz atmak istiyorsanız kullanışlıdır. **.Log** düz metin ve komut satırı araçlarını kullanarak iletileri filtrelemek istiyorsanız kullanışlıdır.

    Tüm iletileri aracılığıyla göz atmak için seçin ve açın **.svclog** dosyası:

    [![](troubleshooting-images/troubleshooting-image13.png "Svclog dosyasını seçin")](troubleshooting-images/troubleshooting-image13.png#lightbox)

5. **.Svclog** dosya açılır **Microsoft hizmet izleme görüntüleyicisini**. İletilerin ilgili gruplar görmek için iş parçacığı tarafından iletileri göz atabilirsiniz. İş parçacığı tarafından göz atmak için önce seçin **grafik** sekmesini ve ardından **Düzen modunu** açılır menü ve select **iş parçacığı**:

    [![](troubleshooting-images/troubleshooting-image14.png "Düzen Modu açılır menüsünü tıklatın ve iş parçacığı seçin")](troubleshooting-images/troubleshooting-image14.png#lightbox)

<a name="verboselogs" />

#### <a name="verbose-log-files"></a>Ayrıntılı günlük dosyaları

Normal günlük dosyaları sorunu tanılamak için yeterli bilgi hala sağlamazsanız denemek için bir son ayrıntılı günlük kaydını etkinleştirmek için tekniktir. Hata raporları hakkında ayrıntılı günlükler da tercih edilen.

1. Visual Studio çıkın.

2. Başlangıç bir [ **Geliştirici komut istemi**](https://msdn.microsoft.com/library/ms229859(v=vs.110).aspx).

3. Visual Studio ile ayrıntılı günlük kaydı başlatmak için komut isteminde aşağıdaki komutu çalıştırın:

    ```bash
    devenv /log
    ```

4. Visual Studio'dan yapı ana bilgisayara bağlanmaya çalışır.

5. Visual Studio bağlantı hatası isabetler sonra günlüklerinden toplamak **Yardım > Xamarin > Zip günlükleri**.

6. Tüm son günlük iletilerini masaüstünüzdeki bir dosyaya SSH sunucudan kopyalamak için Mac üzerinde terminale aşağıdaki komutu çalıştırın:

    ```bash
    grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"
   ```

Bu ayrıntılı günlük dosyaları doğrudan bu sorunu gidermek için yeterli ipuçları belirtmezseniz, lütfen [yeni bir hata raporu dosya](https://bugzilla.xamarin.com/newbug) ve adım 5 ve 6. adım .log dosyasından .zip dosyasını ekleyin.

## <a name="troubleshooting-automatic-mac-provisioning"></a>Mac otomatik sağlama sorunlarını giderme

### <a name="ide-log-files"></a>IDE günlük dosyaları

Kullanarak herhangi bir sorunla karşılaşırsanız [Mac otomatik sağlamayı](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning), depolanan Visual Studio 2017 IDE günlükleri, bir göz atalım **%LOCALAPPDATA%\Xamarin\Logs\15.0**.

## <a name="troubleshooting-build-and-deployment-errors"></a>Derleme ve dağıtım hatalarını giderme

Bu bölümde, Visual Studio başarıyla yapı ana bilgisayara bağlandıktan sonra oluşabilecek bazı sorunlar ele alınmaktadır.

### <a name="unable-to-connect-to-address1921681222-with-usermacuser"></a>"Adresine bağlanılamıyor ='192.168.1.2:22 ' kullanıcıyla 'macuser' ="

Bilinen nedenler:

- **Xamarin 4.1 güvenlik özelliği** – bu hatayı _olacak_ Xamarin 4.1 veya daha yüksek kullandıktan sonra Xamarin 4. 0 düşürmek olduğunda meydana gelir. Bu durumda hata ek tarafından eşlik "özel anahtarı şifrelenir, ancak parola boş" uyarı. Bu bir _bilerek_ Xamarin 4.1 içinde yeni bir güvenlik özelliği nedeniyle değiştirin. **Önerilen düzeltme**: Sil **kimliği\_rsa** ve **kimliği\_rsa.pub** gelen **%LOCALAPPDATA%\Xamarin\MonoTouch**ve ardından Mac yapı ana bilgisayara bağlanın.

- **SSH güvenlik kısıtlaması** – bu ileti ek eşlik uyarı "kimlik doğrulaması gerçekleştirilemedi ssh anahtarları varolan kullanarak kullanıcı", bu genellikle bir dosya veya tam yolunu dizinlerde anlamına gelir **$ HOME/.ssh/authorized\_anahtarları** Mac için etkin yazma izinlerine sahip _diğer_ veya _grup_ üyeleri. **Ortak düzeltme**: çalıştırmak `chmod og-w "$HOME"` Mac üzerinde Terminal komut istemindeki Hangi belirli dosya veya dizin hakkındaki ayrıntılar için çalıştırın soruna neden olan `grep sshd /var/log/system.log > "$HOME/Desktop/sshd.log"` Terminal ve ardından açık **sshd.log** dosya masaüstünüzden ve Ara "kimlik doğrulaması reddedildi: hatalı sahipliği veya modları".

### <a name="solutions-cannot-be-loaded-from-a-network-share"></a>Bir ağ paylaşımından çözümleri yüklenemiyor

Yerel Windows dosya sistemi veya eşlenmiş sürücüsünde iseler çözümleri yalnızca derlenmiş.

Bir ağ paylaşımına kaydedilir çözümleri hataları throw veya tamamen derlemek Reddet olabilir. Tüm **.sln** Visual Studio'da kullanılan dosyaları yerel Windows dosya sisteminde kaydedilmesi.

Aşağıdaki hata nedeniyle bu sorun durum:

```bash
error : Building from a network share path is not supported at the moment. Please map a network drive to '\\SharedSources\HelloWorld\HelloWorld' or copy the source to a local directory.
```

ilgili hata: [#36195](https://bugzilla.xamarin.com/show_bug.cgi?id=36195)

### <a name="missing-provisioning-profiles-or-failed-to-create-the-a-fat-library-error"></a>Sağlama profilleri eksik ya da "oluşturulamadı fat kitaplığı" hatası

Mac üzerinde Xcode başlatın ve geliştirici hesabınızla oturum açtıysa, Apple ve geliştirme profili yüklenir, iOS emin olun:

[![](troubleshooting-images/troubleshooting-image7.png "Apple Geliştirici hesabınızla oturum açmış olduğu doğrulanıyor ve geliştirme profili iOS indirilir")](troubleshooting-images/troubleshooting-image7.png#lightbox)

### <a name="a-socket-operation-was-attempted-to-an-unreachable-network"></a>"Yuva işlemi erişilemeyen bir ağ için denendi"

Bildirilen nedenler:

- **Geliştirme [#36118](https://bugzilla.xamarin.com/show_bug.cgi?id=36118)**  – Visual Studio derleme ana bilgisayara bağlanmak için bir IPv6 adresi kullanıldığında başarılı derlemeleri bu hatayı önleyebilirsiniz. (Yapı konak bağlantı henüz IPv6 adreslerini desteklemez.)

### <a name="xamarinios-visual-studio-plugin-fails-to-load-after-reinstallation-of-betaalpha-channel"></a>Xamarin.iOS Visual Studio eklentisi beta/alfa kanal yüklenmesinden sonra yükleme başarısız olur.

İlgili hata [#40781](https://bugzilla.xamarin.com/show_bug.cgi?id=40781).

Visual Studio MEF Bileşeni önbellek yenileme başarısız olduğunda bu sorun ortaya çıkabilir. Bu durumda, bu Visual Studio uzantısı yükleme yardımcı olabilir: [https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd)

Bu önbellek Bozulması ile sorunlarını düzeltmek için Visual Studio MEF Bileşeni önbelleği temizler.

<a name="errors" />

### <a name="errors-due-to-existing-build-host-processes-on-the-mac"></a>Mac üzerinde var olan yapı ana bilgisayarı işlemlerinin nedeniyle hataları

Önceki yapı konak bağlantıları işlemlerini bazen geçerli etkin bağlantı davranışını etkileyebilir. Mevcut tüm işlemler için denetlemek için Visual Studio'yu kapatın ve ardından aşağıdaki komutları terminale Mac üzerinde çalıştırın:

```bash
ps -A | grep mono
```

[![](troubleshooting-images/troubleshooting-image10.png "Mac terminale komutları çalıştırma")](troubleshooting-images/troubleshooting-image10.png#lightbox)

Sonlandırmak için var olan işlemleri aşağıdaki komutu kullanın:

```bash
killall mono
```

### <a name="clearing-the-mac-build-cache"></a>Mac derleme önbelleği temizleniyor

Bir yapı sorun giderme ve davranış Mac üzerinde depolanan geçici derleme dosyalarının herhangi biri için ilgili olmayan emin olmak için derleme Önbellek klasörü silebilirsiniz.

1. Aşağıdaki komutu terminale Mac üzerinde çalıştırın:

    ```bash
    open "$HOME/Library/Caches/Xamarin"
    ```

2. Denetim tıklatın **mtbs** klasörü ve select **taşımak için çöp**:

    [![](troubleshooting-images/troubleshooting-image9.png "Çöp kutusu için mtbs klasörünü taşıma")](troubleshooting-images/troubleshooting-image9.png#lightbox)


## <a name="related-links"></a>İlgili bağlantılar

- [Mac ile eşleştirme](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Xamarin Mac yapı aracısını - Xamarin Üniversitesi Şimşek Ders](https://www.youtube.com/watch?v=MBAPBtxkjFQ)
