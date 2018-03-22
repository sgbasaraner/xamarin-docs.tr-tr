---
title: "Mac bilgisayara bağlayarak"
description: "Visual Studio için Xamarin.iOS oluşturma, derleme ve Visual Studio IDE kullanarak bir Windows bilgisayarda iOS uygulamalarının hatalarını geliştiricilerin olanak sağlar. Bu kılavuz, özellikleri Xamarin.iOS Visual Studio için sağlanan ve nasıl Mac bağlantısı yapı konak yapılan açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39DD7B3F-3E69-4E2A-B743-4C26AF613025
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: e4f7b55fa859473e84298151bc08878bc2161192
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="connecting-to-the-mac"></a>Mac bilgisayara bağlayarak

_Visual Studio için Xamarin.iOS oluşturma, derleme ve Visual Studio IDE kullanarak bir Windows bilgisayarda iOS uygulamalarının hatalarını geliştiricilerin olanak sağlar. Bu kılavuz, özellikleri Xamarin.iOS Visual Studio için sağlanan ve nasıl Mac bağlantısı yapı konak yapılan açıklar._

Visual Studio gibi birçok avantaj sunar SSH üzerinden Mac bilgisayara bağlanır:

- Visual Studio'yu başlatın ve derleme aracısı doğrudan denetleyebilirsiniz. El ile başlatma ve durdurma gerektiren bir kullanıcıya görünen uygulama artık yoktur.

- Visual Studio'da yeni bağlantı Yöneticisi bulmak, kimlik doğrulaması ve Mac yapı konak unutmayın.

- Tüm iletişimin güvenli bir şekilde SSH yoluyla tünelli olduğundan, yalnızca bir tek bağlantı bağlantı noktası 22 için gereklidir.

- Gerçekleşmeden hemen visual Studio değişiklikleri bildirilir. Örneğin, bir iOS aygıtı araç çubuğunda zaman takılı anında güncelleştirilir.

- Visual Studio birden çok örneği aynı anda bağlanabilir.

- Bağlantı üzerinde geliştirme intrude değil. Bu yalnızca bir bağlantı Mac için Mac, hata ayıklama veya iOS Tasarımcısı kullanarak gibi gerekli olduğu bir işlemi gerçekleştirirken ister.

Mac Bağlantı Aracısı tarafından denetlenen farklı bölümleri – Örneğin, iOS Tasarımcı Aracısı'nı ve derleme aracısı – işlevlerini için birden çok işlem oluşur. Bu aracı denetlenir ve Visual Studio tarafından güncelleştirildi ve kilitlenme olsaydı herhangi bir bağımsız işlemler otomatik olarak yeniden başlatılır.

Aşağıdaki diyagram Xamarin.iOS geliştirme iş akışı basit bir genel bakış gösterilir:

[![iOS geliştirme iş akışı](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
> Visual Studio gerçekte projeler derlemek için ayrı bir MSBuild işlemi başlatır. Bu işlem, Visual Studio oluşturduğunda gerçekte iki SSH bağlantısını Windows Mac olduğu anlamına gelir, Mac için yeni bir bağlantı oluşturur. Derleme kaynağı [komut satırı](#commandline) yalnızca bir MSBuild işlemi oluşturur. Bu diyagramda kolaylık sağlamak için tüm bağlantıları yalnızca bir ok temsil edilir.

## <a name="requirements"></a>Gereksinimler

Xamarin.iOS Visual Studio için harika bir feat gerçekleştirir: oluşturma, derleme ve Visual Studio IDE kullanarak bir Windows bilgisayarda iOS uygulamalarında hata ayıklama geliştiricilerin olanak sağlar. Bu tek başına yapamayacağı – iOS uygulamalarının Apple'nın derleyici oluşturulamaz ve Apple'nın sertifikaları ve kod imzalama araçları dağıtılamaz. Bu, Xamarin.iOS Visual Studio yüklemesi için ağa bağlı bir Mac OS X bilgisayarla bağlantı gerektirdiği anlamına gelir (için refered olarak olduğu *konak* veya *konak yapı*) sizin için bu görevleri gerçekleştirmek için. Xamarin'ın araçları yapılandırdıktan sonra işlemi mümkün olduğunca sorunsuz hale getirir.

### <a name="system-requirements"></a>Sistem Gereksinimleri

Sistem gereksinimleri bulunabilir [yükleme Xamarin.iOS Windows](~/ios/get-started/installation/windows/index.md#system-requirements) Kılavuzu


#### <a name="compatibility"></a>Uyumluluk

> [!IMPORTANT]
> Windows makine olarak bağlı olduğu Mac Xamarin.iOS aynı sürümünü kullanıyor olmalıdır. Bu doğru olduğundan emin olmak için:                                                    
>                                                                                                                 
> - **Visual Studio 2015 ve önceki**: aynı olduğundan emin olun [güncelleştirmeleri kanal](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) Mac için Visual Studio
>                                                                                                                 
> - **Visual Studio 2017, yayın sürümü**: üzerinde olduğundan emin olun **kararlı** kanal, Visual Studio for Mac
>                                                                                                                 
> - **Visual Studio 2017, Önizleme sürümü**: üzerinde olduğundan emin olun **alfa** kanal, Visual Studio for Mac Visual Studio Xamarin.iOS SDK ve Xcode kayıtlı ve uyumlu sürümlerde denetlemez.
>   Derleme hataları kaynaklanan derleme aracısı tarafından denetlenir; ve Tasarımcısı hataları kaynaklanan iOS Tasarımcısı aracısı tarafından.

### <a name="connecting-to-the-mac"></a>Mac bilgisayara bağlayarak

#### <a name="mac-setup"></a>Mac Kurulumu

Mac ana bilgisayar ayarlamak için Visual Studio için Xamarin uzantısı Mac arasındaki iletişimi etkinleştir Bunu yapmak için izin **uzaktan oturum açma** aşağıdaki adımları izleyerek Mac:

1. Açık *Spotlight* (**⌘ alanı**) arayın ve *uzaktan oturum açma* ve ardından *paylaşım* sonucu. Bu açılır *sistem tercihleri* adresindeki *paylaşım* paneli:

   [![Uzaktan oturum açma için Spotlight arama](images/spotlight.png)](images/spotlight.png#lightbox)

2. Değer çizgilerinin *uzaktan oturum açma* seçeneğini *hizmet* Mac bilgisayara bağlanmak Visual Studio için Xamarin izin vermek için soldaki listesi:

   [![Değer çizgilerinin hizmeti listesinde uzak oturum seçeneği](images/sharing.png)](images/sharing.png#lightbox)

3. Olduğundan emin olun *uzaktan oturum açma* erişim için izin verilecek şekilde ayarlandığını *tüm* kullanıcılar veya Mac kullanıcı adınız veya grup sağdaki listede izin verilen kullanıcılar listesinde eklenmiştir.

İmzalı uygulamaları engelleyecek şekilde varsayılan olarak ayarlanmış OS X Güvenlik duvarı varsa, buna ek olarak, izin gerekebilir `mono-sgen` gelen bağlantıları almak için. Bu durumda, isteyen bir uyarı iletişim kutusu görünür.

Mac'inizde geçerli, açık oturum koşuluyla aynı ağ üzerinde ise, artık Visual Studio tarafından bulunabilir olması gerekir.

Visual Studio başlatın ve bu yüzden, bir kullanıcı olarak çalıştırmanız gereken şey Mac'inizde aracısını durdurun.

### <a name="windows-setup"></a>Windows Kurulumu

Emin olun [yükleme](~/ios/get-started/installation/windows/index.md) Windows makinenizde Xamarin araçları.

### <a name="connecting-to-the-mac-build-host"></a>Mac yapı ana bilgisayara bağlanma

Mac yapı ana bilgisayara bağlanmak için iki yol vardır:

İOS araç çubuğunda:

[![İOS araç çubuğu](images/image1.png)](images/image1.png#lightbox)

Veya **Araçlar > Seçenekler** Visual Studio'da seçme **Xamarin > iOS ayarları** tıklatıp **Xamarin Mac arası bulma** düğmesi:

[![Finding Xamarin Mac Agent](images/image2.png)](images/image2.png#lightbox)

Her iki durumda da gezinme neden **Mac Aracısı** iletişim kutusunda, aşağıda gösterilmiştir:

[![Mac aracı iletişim kutusu](images/image3.png)](images/image3.png#lightbox)

Bu ya da daha önce bağlanmış ve bilinen makine ya da kullanılabilir makineleri olarak depolanan tüm makinelerin bir listesini görüntüler *uzaktan oturum açma*.

Mac, ona bağlanmak için çift tıklayarak seçin. Bir Mac bilgisayara bağlanan ilk kez uzaktan bağlantıya izin vermek için Mac kullanıcı kimlik bilgilerinizi girmeniz istenir:

[![Mac kullanıcı kimlik bilgilerini girin](images/image4.png)](images/image4.png#lightbox)

Aracı Mac için yeni bir SSH bağlantısını oluşturmak için bu kimlik bilgilerini kullanır Başarılı olursa, bir SSH anahtarı oluşturulur ve olacaktır [kayıtlı](#commandline) içinde `authorized_keys` bu Mac dosyada Sonraki bağlantılarda aracı, en son bağlanılan bilinen yapı ana bilgisayara bağlanmak için kullanıcı adı ve anahtar dosyasını kullanmasını sağlar.

> [!NOTE]
> Kullanmalısınız _kullanıcıadı_ ve _tam adı_ kimlik bilgilerinizi girerken.  Bu kullanarak öğrenebilirsiniz `whoami` Terminal komutu.  Örneğin, aşağıdaki ekran görüntüsünde hesap adı olacaktır **amyb** ve **silinecektir yanıklara**:
>
> ![Kullanıcı adı Terminal uygulamada bulma](images/image5.png)

Bağlantı başarıyla yapıldığında ile ana bilgisayar seçimi iletişim kutusunda görüntülenir bir **bağlı** simgesinin yanında, aşağıda gösterildiği gibi:

[![Ana bilgisayar seçimi iletişim kutusunu yanında bağlı simgesi](images/image6.png)](images/image6.png#lightbox)

Yalnızca olabilir bir bağlı Mac herhangi bir zamanda.

Listesinde, her bir makine bağlı ya da aksi takdirde bir bağlam menüsü olanak sağ tıklatın, görünecektir **Bağlan**, **Bağlantıyı Kes**, veya **Mac unuttunuz** olarak gerekli:

[![Bağlanma, bağlantıyı kesme veya Unut bu Mac bağlam menüleri](images/image7.png)](images/image7.png#lightbox)

İsterseniz **bu Mac unuttunuz**, yeniden bağlanmak için kimlik bilgilerinizi yeniden girmeniz gerekir.

<a name="manual-add"/>

### <a name="manually-adding-a-mac"></a>El ile bir Mac ekleme

Bazı durumlarda ana bilgisayar seçimi iletişim kutusunda listelenen mDNS adını göremiyorsanız, Mac el ile eklemek isteyebilirsiniz. Bunu yapmak için aşağıdaki adımları izleyin:

1. Mac'ın IP adresi ya da göz atarak bulun **sistem tercihleri > paylaşım > Uzak oturum açma** Mac:

   [![Sistem tercihleri Mac'ın IP adresi](images/image8.png)](images/image8.png#lightbox)

   Veya komut satırı kullanmayı tercih ederseniz, girerek IP adresinizi bulabilirsiniz `ipconfig getifaddr en0` Terminal içine (bağlantı türüne bağlı olarak değişkeni olabileceğini unutmayın `en1`, `en2` vs.):

   [![Terminal uygulamasında IP adresi](images/image9.png)](images/image9.png#lightbox)

2. Visual Studio ve ana bilgisayar seçimi iletişim kutusunda dönüş **Mac Ekle...** :

   [![Ana bilgisayar seçimi iletişim kutusu](images/image10.png)](images/image10.png#lightbox)

3. Mac Ekle iletişim kutusuna, Mac IP adresini girin ve tıklayın **Ekle**:

   [![Mac adresini Mac Ekle iletişim kutusuna girin](images/image11.png)](images/image11.png#lightbox)

4. Son olarak, kullanıcı adı (değil tam ad), Mac yönetici hesabı ve karşılık gelen bir parola girin:

   [![Kullanıcı adı ve parola girin](images/image12.png)](images/image12.png#lightbox)

Tıkladığınızda **oturum açma**, Visual Studio SSH kullanarak Mac makinesine günlüğe kaydeder ve bu Mac bilinen bir makine ekleyeceksiniz.

<a name="commandline"/>

### <a name="command-line-support"></a>Komut satırı desteği

Aracı, bir Xamarin.iOS yapılandırma komut satırından da destekler.  Kullanmak için aşağıdaki gerekli parametreleri için MSBuild geçmesi gerekir:

- `ServerAddress` – Mac sunucunun IP adresi.

- `ServerUser` – Mac sunucuya oturum açmak için kullanılacak kullanıcı adı (tam adı değil).

- `ServerPassword` (İsteğe bağlı) Mac ana bilgisayara oturum açmak için – kullanılan parola.

`ServerPassword` Parametresi zorunlu değildir.

Bunun yerine, bir parola geçirildi, ilk kez Visual Studio veya komut satırı kullanarak ya da bu belirli Windows, Mac ve kullanıcı yapılandırması için bir anahtar çifti oluşturulur ve gelecekte kullanım için Windows bilgisayarında depolanır. İçinde bulunur **%localappdata%\Xamarin\MonoTouch\id_rsa**.  Değil geçirirseniz `ServerPassword` parametresi `id_rsa` keyfile kimlik doğrulaması için kullanılır.

Mac 10.211.55.2'na bağlanmak için bir örnek komut **xamUser** hesabı parolayla **İstanbul** aşağıda gösterilmiştir:

```bash
C:\samples\App1>msbuild App1.sln /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhoneSimulator /p:ServerPassword=mypassword
```

### <a name="summary"></a>Özet

Bu Visual Studio ve Visual Studio kullanarak Xamarin.iOS uygulamaları geliştirmek sağlayarak Mac, iOS yapı ve tasarımcı araçları arasında keşfedilen bağlantı makalesi.

### <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](~/ios/get-started/installation/windows/index.md)
- [Bağlantı Sorunlarını Giderme](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Visual Studio ortamınızı XMA (video) ile bir Mac bağlanma](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
