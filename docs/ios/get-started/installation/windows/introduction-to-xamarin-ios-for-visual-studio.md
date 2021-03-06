---
title: Visual Studio Xamarin.iOS için giriş
description: Bu belge, yapı ve Visual Studio kullanarak Xamarin.iOS uygulamaları test açıklar. Proje oluşturma açıklanır, ana bilgisayardan Windows çalıştıran ve bir uygulama hata ayıklama ve bir Mac bilgisayara bağlayarak yapı.
ms.prod: xamarin
ms.assetid: bf3c779f-959f-428d-babb-428f363f7e4e
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: e07119bee6478a503ca6c586fa3348206ccd16f7
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786206"
---
# <a name="introduction-to-xamarinios-for-visual-studio"></a>Visual Studio Xamarin.iOS için giriş

Windows için Xamarin iOS uygulamalarının yazılmış ve derleme ve dağıtım hizmeti sağlayan ağa bağlı bir Mac ile Visual Studio'dan Test olanak sağlar.

Bu makalede, yükleme ve Xamarin.iOS araçları Visual Studio kullanarak iOS uygulamaları oluşturmak için her bilgisayarda yapılandırma adımları yer almaktadır.

Visual Studio içinde iOS için geliştirmeye çok sayıda fayda sağlar:

-  Oluşturma platformlar arası çözümleri iOS, Android ve Windows uygulamaları.
-  Sık kullandığınız Visual Studio araçları kullanarak (gibi **Resharper** ve **Team Foundation Server**) iOS kaynak kodu da dahil olmak üzere tüm platformlar arası projeleri için.
-  Tüm Apple'nın API'leri Xamarin.iOS bağlamaları yararlanarak sırasında bilinen bir IDE ile çalışır.

<a name="Requirements_and_Installation" />

## <a name="requirements-and-installation"></a>Gereksinimleri ve yükleme

İçin Visual Studio iOS için geliştirmeye bağlı gereken birkaç gereksinimi yoktur. Kısaca söz edildiği gibi özetinde, bir Mac IPA dosyalarını derlemek için gereklidir ve uygulamaları cihazlara Apple'nın sertifikaları ve kod imzalama araçları olmadan dağıtılamaz.

Birkaç yapılandırma seçeneği kullanılabilir olduğundan, geliştirme gereksinimleriniz için en iyi çalıştığı karar vermem. Bunlar, aşağıda listelenmiştir:

-  Mac ana geliştirme makinenizi kullanın ve Visual Studio yüklüyse Windows sanal makine çalıştırın. VM yazılım gibi kullanmanızı öneririz [Parallels](http://www.parallels.com/products/desktop/) veya [VMWare](http://www.vmware.com/products/fusion/) .
-  Mac yalnızca bir yapı konağı olarak kullanın. Bu senaryoda, bir Windows makineyle aynı ağa bağlı olurdu [gerekli](~/cross-platform/get-started/installation/windows.md#installation) araçları yüklü.

Her iki durumda da, aşağıdaki adımları izlemelisiniz:

- [Mac için Visual Studio yükleme](https://docs.microsoft.com/visualstudio/mac/installation)
- [Windows'ta Xamarin araçlarını yükleme](~/cross-platform/get-started/installation/windows.md)

## <a name="connecting-to-the-mac"></a>Mac bilgisayara bağlayarak

Visual Studio, Mac yapı ana bilgisayarına bağlanmak için yönergeleri izleyin [Mac çiftine](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Kılavuzu.

## <a name="visual-studio-toolbar-overview"></a>Visual Studio araç çubuğuna genel bakış

Visual Studio için Xamarin iOS, standart araç ve yeni iOS araç çubuğu öğeleri ekler.
Bu araç çubuklarını işlevler aşağıda açıklanmıştır.

### <a name="standard-toolbar"></a>Standart araç çubuğu

Xamarin iOS geliştirme için ilgili denetimleri kırmızı daire içine alınır:

 [![](introduction-to-xamarin-ios-for-visual-studio-images/03.png "Xamarin iOS geliştirme için ilgili denetimleri kırmızı daire içinde")](introduction-to-xamarin-ios-for-visual-studio-images/03.png#lightbox "Xamarin iOS geliştirme için ilgili denetimleri kırmızı daire içinde")

-  **Başlat** - hata ayıklama veya uygulama seçili platformda çalışan başlatır. Bağlı bir Mac olmalıdır (iOS araç çubuğunda durum göstergesi bakın).
-  **Çözüm yapılandırmaları** – kullanmak üzere yapılandırma seçmenize olanak tanır (örn., hata ayıklama, yayın).
-  **Çözüm platformları** -iPhone veya dağıtım için iPhoneSimulator seçmenize olanak sağlar.

### <a name="ios-toolbar"></a>iOS araç çubuğu

Visual Studio'da araç iOS her Visual Studio sürümünde benzer. Bunlar tüm aşağıda verilmiştir:

[![](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png "iOS araç çubuğu")](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png#lightbox)

Her öğe aşağıda açıklanmıştır:

-  **Mac Aracısı/Bağlantı Yöneticisi** – Xamarin Mac arası iletişim kutusunu görüntüler. Bu simge görünür *turuncu* bağlanırken ve *yeşil* bağlandığında.
-  **İOS simülatörü Göster** – iOS simülatörü penceresi Mac üzerinde öne getirir
-  **Derleme sunucusundaki IPA dosyasını Göster** – açılır Bulucu konumu uygulamanın IPA'sı olarak Mac üzerinde çıktı dosyası.

## <a name="ios-output-options"></a>iOS çıkış seçenekleri

### <a name="output-window"></a>Çıktı Penceresi

Seçeneği bulunmaktadır *çıkış* derleme, dağıtma ve bağlantı iletileri ve hataları bulmak için görüntüleme bölmesi.

Aşağıdaki ekran görüntüsünde, proje türüne bağlı olarak değişebilir kullanılabilir çıktı windows gösterir:

[![](introduction-to-xamarin-ios-for-visual-studio-images/output-sml.png "Kullanılabilir çıktı windows")](introduction-to-xamarin-ios-for-visual-studio-images/output-large.png#lightbox)

- **Xamarin** – Bu, yalnızca Xamarin Mac ve etkinleştirme durumu bağlantısı gibi ilgili bilgileri içerir.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output3-sml.png "Yalnızca Xamarin Mac ve etkinleştirme durumu bağlantısı gibi ilgili bilgileri")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

- **Xamarin tanılama** – bu gibi etkileşim ile ve Android için Xamarin projeniz ilgili ayrıntılı bilgileri gösterir.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output4-sml.png "Xamarin projesi hakkında ayrıntılı bilgi")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

Hata ayıklama ve yapı gibi diğer varsayılan Visual Studio çıkış bölmeleri çıkış görünüm içinde hala kullanılabilir ve hata ayıklama çıktı ve MSBuild çıktı için kullanılır:

-  **Hata ayıklama**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output2-sml.png "Hata ayıklama çıktısı")](introduction-to-xamarin-ios-for-visual-studio-images/output2-large.png#lightbox)

- **Yapı** & **derleme sırası**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output1-sml.png "MSBuild çıktı")](introduction-to-xamarin-ios-for-visual-studio-images/output1-large.png#lightbox)

## <a name="ios-project-properties"></a>iOS proje özellikleri

Visual Studio'nun proje özellikleri erişilebilir proje adına sağ tıklayıp seçerek *özellikleri* bağlam menüsünde. Bu, iOS Uygulamanızı yapılandırmak aşağıdaki ekran görüntüsünde gösterildiği gibi sağlar:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosproperties.png "Bir iOS uygulaması yapılandırma")

-  *iOS paket imzalama* – kod imzalama kimlikleri doldurmak için Mac bilgisayara bağlanır ve sağlama profilleri:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/bundlesigning.png "Kod kimlikleri imzalama ve sağlama profilleri doldurma")

-  *iOS IPA seçenekleri* – IPA dosya Mac'ın dosya sisteminde kaydedilecek:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/ipaoptions.png "iOS IPA seçenekleri")

-  *iOS Çalıştırma Seçenekleri* – ek parametreleri yapılandır:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosrunoptions.png "iOS Çalıştırma Seçenekleri")

## <a name="creating-a-new-project-for-ios-applications"></a>İOS uygulamaları için yeni bir proje oluşturma

Yeni bir iOS projesi gelen Visual Studio içinde oluşturarak yapılır herhangi bir proje türü'olduğu gibi. Seçme **Dosya > Yeni proje** bazı yeni bir iOS projesi oluşturmak için kullanılabilir proje türleri gösteren aşağıda gösterilen iletişim kutusunu açar:

![Yeni proje oluşturma](introduction-to-xamarin-ios-for-visual-studio-images/newproject.w157.png)

Seçme **iOS uygulaması (Xamarin)** yeni bir Xamarin.iOS uygulaması oluşturmak için aşağıdaki şablonlardan gösterir:

![Bir iOS uygulaması için şablonu seçme](introduction-to-xamarin-ios-for-visual-studio-images/newproject-2.w157.png)

Film şeridi ve .xib dosyaları iOS Tasarımcısı kullanarak Visual Studio'da düzenlenebilir. Film şeridi oluşturmak için film şeridi şablonlardan birini seçin. Bu oluşturacak bir **Main.storyboard** dosyasını **Çözüm Gezgini** aşağıdaki ekran görüntüsüne gösterildiği gibi:

![Çözüm Gezgini'nde Main.storyboard dosyası](introduction-to-xamarin-ios-for-visual-studio-images/solution-explorer-new.w157.png)

Oluşturma veya düzenleme Şeridinizin başlatmak için çift `Main.storyboard` iOS Tasarımcısı açmak için:

![](introduction-to-xamarin-ios-for-visual-studio-images/iosdesigner.png "İOS Tasarımcısı'nda Main.storyboard")

Görünümünüze nesneleri eklemek için kullanın **araç** öğeleri, tasarım yüzeyine sürükleyip bölmesi. Araç kutusunu seçerek eklenebilir **Görünüm > Araç**, zaten eklendi. Nesne özellikleri değiştirilebilir, ayarlanan, kendi düzenleri ve olayları kullanarak oluşturulabilir **özellikleri** aşağıda gösterildiği gibi bölmesi:

![](introduction-to-xamarin-ios-for-visual-studio-images/properties.png "Özellikler bölmesi")

 İOS Tasarımcısı'nı kullanarak daha fazla bilgi için başvurmak [Tasarımcısı](~/ios/user-interface/designer/index.md) kılavuzları.

## <a name="running--debugging-ios-applications"></a>Çalışan & iOS uygulamaları hata ayıklama

### <a name="device-logging"></a>Cihaz günlüğe kaydetme

Visual Studio 2017, Android ve iOS günlük klavye takımı birleştirilmiş.

Android ve iOS cihazları için günlükleri göstermek için Visual Studio için yeni cihaz günlük araç penceresi sağlar. Aşağıdaki komutları yürüterek gösterilebilir:

- **Görünüm > Diğer Pencereler > cihaz günlük**
- **Araçlar > iOS > cihaz günlük**
- **iOS araç > cihaz günlük**

Araç penceresi gösterilen sonra kullanıcı fiziksel cihaz aygıtları aşağı açılır listeden seçebilirsiniz. Bir aygıt seçildiğinde, tabloya günlükleri otomatik olarak eklenir. Cihazlar arasında geçiş yapma durdurun ve cihaz günlüğü Başlat.

Combobox içinde görünmesi cihazlar için sırayla bir iOS projesi yüklenmesi gerekir. İOS için Visual Studio ayrıca olmalıdır [Mac sunucuya bağlı](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Mac bağlı iOS cihazları bulmak için

Bu araç penceresi sağlar: günlük girişlerini, cihaz seçimi için açılır listesinde, günlük girişlerini, arama kutusuna ve Dur/Yürüt/Duraklat düğmeleri temizlemek için bir yol tablosu.

### <a name="set-debugging-stops"></a>Hata ayıklama durakları ayarlama

Kesme noktaları programın yürütülmesi geçici olarak durdurmak için hata ayıklayıcı için sinyal için uygulamanızda herhangi bir noktada ayarlayabilirsiniz. Visual Studio'da bir kesme noktası ayarlamak için kesme istediğiniz kod satırı sayısını yanındaki düzenleyicinizi kenar boşluğu alanını tıklatın:

![](introduction-to-xamarin-ios-for-visual-studio-images/image18.png "Hata ayıklama noktası ayarlama")

Hata Ayıklamayı Başlat ve benzeticisi veya aygıt bir kesme noktası uygulamanıza gitmek için kullanın. Bir kesme noktası isabet, satır vurgulanır ve Visual Studio'nun normal hata ayıklama davranışı etkin olacaktır:, tekrar adımla veya kodun dışına yerel değişkenler inceleyin veya komut penceresi kullanın.

Bu ekran OS X: Parallels kullanarak iOS simülatörü çalışan Visual Studio yanındaki gösterir.

![](introduction-to-xamarin-ios-for-visual-studio-images/image19.png "Bu ekran OS x'te Parallels kullanarak iOS simülatörü çalışan Visual Studio yanındaki gösterir.")

### <a name="examine-local-variables"></a>Yerel değişkenler inceleyin

![](introduction-to-xamarin-ios-for-visual-studio-images/image20.png "Hata ayıklama ile yerel değişkenler inceleniyor")

## <a name="summary"></a>Özet

Bu makalede, Visual Studio için Xamarin iOS kullanma açıklanmaktadır. Listelenen oluşturma, derleme ve bir iOS uygulamasını Visual Studio'dan Test etme için kullanılabilen çeşitli özellikler ve oluşturma ve basit iOS uygulama hatalarını ayıklama aracılığıyla gitti.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin.iOS yükleme](~/ios/get-started/installation/windows/index.md)
- [Cihaz sağlama](~/ios/get-started/installation/device-provisioning/index.md)
- [Kodda iOS kullanıcı Arabirimi oluşturma](~/ios/app-fundamentals/ios-code-only.md)
- [Visual Studio ortamınızı XMA (video) ile bir Mac bağlanma](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
