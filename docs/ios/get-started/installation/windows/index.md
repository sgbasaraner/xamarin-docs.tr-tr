---
title: "Xamarin.iOS Windows yükleme"
description: "Bu makalede, Visual Studio için Xamarin.iOS ayarlamak gösterilmiştir. Visual Studio için Xamarin uzantısını yükleme işlemi kapsayan ve Apple Mac üzerinde yüklü SDK'sı bağlanma açıklanır"
ms.topic: article
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/29/2017
ms.openlocfilehash: cfbe2df23317ee3ad11c9970ab892ddcc251b9d6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="installing-xamarinios-on-windows"></a>Xamarin.iOS Windows yükleme

_Bu makalede, Visual Studio için Xamarin.iOS ayarlamak gösterilmiştir. Visual Studio için Xamarin uzantısını yükleme işlemi kapsayan ve Apple Mac üzerinde yüklü SDK'sı bağlanma açıklanır_

## <a name="overview"></a>Genel Bakış

Visual Studio için Xamarin.iOS iOS uygulamalarının yazılır ve Windows bilgisayarlarda, derleme ve dağıtım hizmeti sağlayan için ağa bağlı bir Mac test olanak sağlar.

Visual Studio içinde iOS için geliştirmeye yapabilmenizi sağlar:

- İOS, Android ve Windows için platformlar arası çözümler oluşturmak uygulamalar.
- Visual Studio araçlarını kullanın (gibi *Resharper* ve *Team Foundation Server*) iOS kaynak kodu da dahil olmak üzere tüm platformlar arası projeleri için.
- Tüm Apple'nın API'leri Xamarin.iOS bağlamaları yararlanarak sırasında bilinen bir IDE ile çalışma

Xamarin.iOS Visual Studio için Visual Studio içindeki bir Windows sanal makine Mac'te (Parallels ya da VMWare kullanarak) veya bir Mac bilgisayar aynı ağ üzerinde görünürdür ayrı bir makine açık olduğunda çalıştığı yapılandırmayı destekler. Hangi yapılandırmanın en uygun bağımsız olarak, Visual Studio derhal ve güvenli bir şekilde SSH kullanarak Mac bilgisayara bağlanır.

Bu makalede, yükleme ve Xamarin.iOS araçları Mac ve Windows makine yanı sıra üzerinde böylece geliştiriciler, hata ayıklama, uygulamaları geliştirmek ve Visual Studio kullanarak Xamarin.iOS dağıtmak Mac ana bilgisayarına bağlanmak için adımları yapılandırma adımları yer almaktadır.

Aşağıdaki diyagram Xamarin.iOS geliştirme iş akışı basit bir genel bakış gösterilir:

[![Xamarin.iOS geliştirme iş akışı](images/xma2.png)](images/xma2.png#lightbox)

> [!IMPORTANT]
>  Visual Studio gerçekte projeler derlemek için ayrı bir MSBuild işlemi başlatır. Bu işlem, Visual Studio oluşturduğunda gerçekte iki SSH bağlantısını Windows Mac olduğu anlamına gelir, Mac için yeni bir bağlantı oluşturur. Derleme kaynağı [komut satırı](~/ios/get-started/installation/windows/connecting-to-mac/index.md) yalnızca bir MSBuild işlemi oluşturur. Bu diyagramda kolaylık sağlamak için tüm bağlantıları yalnızca bir ok temsil edilir.

## <a name="requirements"></a>Gereksinimler

Xamarin.iOS Visual Studio için harika bir feat gerçekleştirir: oluşturma, derleme ve Visual Studio IDE kullanarak bir Windows bilgisayarda iOS uygulamalarında hata ayıklama geliştiricilerin olanak sağlar. Bu tek başına yapamayacağı – iOS uygulamalarının Apple'nın derleyici oluşturulamaz ve Apple'nın sertifikaları ve kod imzalama araçları dağıtılamaz. Başka bir deyişle, bir Xamarin.iOS Visual Studio yüklemesi için bu görevleri gerçekleştirmek için ağa bağlı bir Mac OS X bilgisayar bağlantısı gerektirir. Xamarin'ın araçları yapılandırdıktan sonra işlemi mümkün olduğunca sorunsuz hale getirir.


<a name="system-requirements"/>

### <a name="system-requirements"></a>Sistem Gereksinimleri

Sistem gereksinimleri şunlardır:

### <a name="windows"></a>Windows

1. Windows 7 veya daha yüksek.

2. Visual Studio 2015 Professional ya da daha yüksek

    a. Bir Enterprise lisansınız varsa, Visual Studio Enterprise yüklemeniz gerekir.

3. Visual Studio için Xamarin.

Xamarin araçları Visual Studio Express sürümleri ile eklenti desteğinin olmaması için kullanılamaz. Xamarin Visual Studio Community desteklenir.

### <a name="mac"></a>Mac

1. Mac çalışan macOS Sierra (10,12) veya üzeri (en yeni kararlı sürüm önerilen rağmen).

2. Xamarin iOS SDK. Bu Mac için Visual Studio indirme sırasında yüklenir

3. Apple'nın Xcode IDE ve iOS SDK'sı (Mac uygulama Mağazası'ndan en yeni kararlı sürüm önerilir).

**Windows bilgisayar ağ üzerinden Mac ulaşabilmesi olmalıdır.**

### <a name="apple-developer-account"></a>Apple Geliştirici hesabı

Uygulama bir aygıta dağıtmak için veya uygulama mağazasında göndermek için bir Apple Developer hesabı gereklidir. İlgili Geliştirici sertifikaları ve sağlama profilleri oluşturulmalı ve Visual Studio için Xamarin.iOS çalışabilmeniz için önce ağ Mac üzerinde yüklü. Bkz: [cihaz sağlamayı](~/ios/get-started/installation/device-provisioning/index.md) makale adımlar geliştirme sertifikası edinme ve bir aygıtı sağlamak için.

## <a name="features"></a>Özellikler 

Visual Studio için Xamarin.iOS oluşturma, düzenleme, oluşturma ve Windows Xamarin.iOS projelerden dağıtımını sağlar. Aşağıdaki özellikleri içerir:

- Yeni iOS projeler oluşturun.

- İOS projeler ve Xamarin.Android ve UWP projeleri de dahil platformlar arası çözümler düzenleyin.

- İOS projeler ve Xamarin.Android ve UWP projeleri de dahil platformlar arası çözümler derleyin.

- Film şeridi ve .xib iOS Tasarımcısı kullanarak destekler.

- Dağıtma ve iOS uygulamaları, uygulama bir simulator veya Mac için bağlı bir cihazda çalıştığı hata ayıklama

- Windows, iOS simülatörü kullanma hakkında daha fazla bilgi için Windows – iOS simulator'da başvurmak için [bu kılavuzda](~/tools/ios-simulator.md).

<a name="configuring" />

## <a name="configuring-your-mac"></a>Mac yapılandırma

<a name="installation"/>

### <a name="installation"></a>Yükleme

Yapmanız gerekenler mac konakta Xamarin.iOS araçlarını yüklemek için [Mac için Visual Studio yükleme](https://docs.microsoft.com/visualstudio/mac/installation).

Yazılım yüklendikten sonra Xamarin.iOS macOS bağlanmak Visual Studio için Xamarin izin verecek şekilde yapılandırmak için sonraki bölümlerde bulunan adımları izleyin.

> [!IMPORTANT]
>  Windows makine olarak bağlı olduğu Mac Xamarin.iOS aynı sürümünü kullanıyor olmalıdır. Bu doğru olduğundan emin olmak için:
>
> - **Visual Studio 2015 ve önceki**: aynı olduğundan emin olun [güncelleştirmeleri kanal](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) Mac için Visual Studio
>
> - **Visual Studio 2017, yayın sürümü**: üzerinde olduğundan emin olun **kararlı** kanal, Visual Studio for Mac
>
> - **Visual Studio 2017, Önizleme sürümü**: üzerinde olduğundan emin olun **alfa** kanal, Visual Studio for Mac

<a name="configuration" />


### <a name="configuration"></a>Yapılandırma

Visual Studio için Xamarin uzantısı Mac arasındaki iletişimi erişmesine izin gerekecektir **uzaktan oturum açma** Mac üzerinde Bunu ayarlamak için aşağıdaki adımları izleyin:

1. Açık *Spotlight* (**Cmd alanı**) arayın ve **uzaktan oturum açma** ve ardından **paylaşım** sonucu. Bu açılır **sistem tercihleri** adresindeki **paylaşım** paneli.

![Uzaktan oturum açma için Spotlight arama](images/spotlight.png)

2. Değer çizgilerinin **uzaktan oturum açma** seçeneğini **hizmet** Mac bağlanmak Visual Studio için Xamarin izin vermek üzere soldaki listesi

![Değer çizgilerinin hizmeti listesinde uzak oturum seçeneği](images/sharing.png)

3. Olduğundan emin olun **uzaktan oturum açma** erişim için izin verilecek şekilde ayarlandığını **tüm kullanıcılar**, ya da Mac kullanıcı adınız veya Grup listesinde eklenir, sağdaki listeden kullanıcılar izin verilen.

Aynı ağda ise Mac artık Visual Studio tarafından bulunabilir olması gerekir.

> [!NOTE]
> İmzalı uygulamalar engellemek için varsayılan olarak ayarlanmış macOS güvenlik duvarınız varsa, izin gerekebilir `mono-sgen` gelen bağlantıları almak için. Bu durumda, isteyen bir uyarı iletişim kutusu görünür.

<a name="developersetup"/>

### <a name="ios-developer-setup"></a>iOS Developer Kurulumu

İOS geliştirme için Mac makine ilgili imzalama kimliklerle yapılandırıldığını önemlidir. Bu, böylece App Store veya geçici üzerinden dağıtılabilir uygulamalarınızı doğru şekilde imzalamak sağlar. Xamarin iOS geliştirme için Mac ayarlama hakkında yönergeler için aşağıdaki bağlantıyı izleyin:

- [Cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md?ide=vs)

Mac yapılandırıldıktan sonra Windows bilgisayarınızı ayarlama zamanı geldi.

<a name="windowsinstallation"/>

## <a name="windows-installation"></a>Windows yükleme

Xamarin, Visual Studio 2017 veya 2015 yüklemesi parçası olarak yüklenebilir. Xamarin için Visual Studio Araçları'nı yüklemek için bkz: [Windows yüklemesini](~/cross-platform/get-started/installation/windows.md) Kılavuzu.

## <a name="installation-complete"></a>Yükleme tamamlandı

Yükleme işlemi tamamlandıktan sonra da hala çalışan her şeyi almak için gereken birkaç adım vardır:

- [Visual Studio mac'e bağlanma](#connectingtomac) – Visual Studio bağlanması gerekir Mac yapı konağı Xamarin.iOS projeleri oluşturmadan önce.
- [Visual Studio araç yapılandırma](#toolbar) – Bu, kolayca Visual Studio'da Xamarin.iOS özelliklerine erişmesine olanak tanır.

<a name="connectingtomac" /> 

### <a name="connecting-to-the-mac"></a>Mac bilgisayara bağlayarak

Bir bağlantı Xamarin.iOS Visual Studio için makine arasında bir SSH bağlantısı aracılığıyla, Mac yapı konak yapılır. Bağlantı hakkında daha fazla bilgi için başvurmak [Mac bilgisayara bağlayarak](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Kılavuzu.

Mac bağlanmak için aşağıdaki adımları izleyin:

- Gözat **Araçlar > Seçenekler** ve altında **Xamarin** seçin **iOS ayarları**:

  [![İOS ayarları ekranı](images/image2.png)](images/image2.png#lightbox)

- Mac şekilde sağlandığını sağlanan [yapılandırılmış](#configuration) izin vermek için **uzaktan oturum açma**, Mac listeden seçin gerekir:

  [![Uzak ana bilgisayar iletişim kutusu](images/xma3.png)](images/xma3.png#lightbox)

- Bu Mac ana bilgisayarın yönetici kimlik bilgileri ister:

  [![Oturum açma iletişim kutusu](images/xma4.png)](images/xma4.png#lightbox)

- Bağlıyken 'Bağlantı başarılı' adının yanındaki simge makine görüntülenir:

  [![Bağlantı başarılı adının yanındaki simge makine görüntüleme uzak sahip iletişim kutusu](images/image6.png)](images/image6.png#lightbox)

Visual Studio her başlattığınızda kurulması.

<a name="toolbar" />

## <a name="visual-studio-toolbar-configuration"></a>Visual Studio araç yapılandırması

Bir iOS projesi açıldığında iOS araç varsayılan olarak görünür ve yapılandırılması gerekmez.

İOS araç görünmüyorsa, aşağıdaki adımlar kullanılabilir.

Araç çubuğu ilk kez açıldığında yapılandırmak için **Görünüm > Araç Çubukları** menü ve emin olun **iOS** girişi seçilidir. Bu ekran görüntüsünde gösterildiği gibi menü öğesini seçin — araç görünür olduğunu göstermek için ticked:

[![Araç çubukları seçin > iOS](images/image31.png)](images/image31.png#lightbox)

### <a name="visual-studio-2015"></a>Visual Studio 2015

Visual Studio 2017'den önceki sürümlerde **çözüm platformları** düğmesi standart araç çubuğuna eklenmesi gerekebilir. Böylece, bir iOS aygıtı veya iOS simülatörü'nü hata ayıklama sırasında seçilmelidir. Bunu ayarlamak için aşağıdaki yönergeleri izleyin

Standart çubuğunun sağ tarafındaki menü düğmesini tıklatın:

- Seçin **ekleme veya kaldırma düğmeleri**
- Seçin **çözüm platformları**

[![Çözüm Platformu seçin](images/image35.png)](images/image35.png#lightbox)

**Standart** ve **iOS** araç çubukları bu ekran görüntüsünde şimdi benzer:

[![Standart ve iOS araç çubukları şimdi bu ekran görüntüsüne benzer olmalıdır](images/image36.png)](images/image36.png#lightbox)

Araç çubuğu Yapılandırma tamamlandıktan sonra Visual Studio için Xamarin iOS kullanmaya başlamak hazırsınız.

## <a name="summary"></a>Özet

Bu makalede, Visual Studio için Xamarin iOS kullanarak yükleme ve yapılandırma için adım adım kılavuz sunulmuştur.

Yükleme ve önkoşul araçlarında Windows ve Mac OS X yapılandırma ele.


## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](~/cross-platform/get-started/installation/windows.md)
- [Cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md)
- [Visual Studio için Xamarin.iOS’a Giriş](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)
- [Visual Studio ortamınızı XMA (video) ile bir Mac bağlanma](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
