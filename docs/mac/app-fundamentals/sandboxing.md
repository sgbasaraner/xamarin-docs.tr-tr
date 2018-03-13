---
title: "Korumalı alan Xamarin.Mac uygulama"
description: "Bu makalede, korumalı alan Xamarin.Mac uygulaması App Store'dan yayın için yer almaktadır. Tüm korumalı alan, kapsayıcı dizinleri, yetkilendirmeler, kullanıcı tarafından belirlenen izinler, ayrıcalık ayırma ve çekirdek zorlama gibi yerleştirilmesini öğeleri kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9cf9cb2e4773b90ecdd9321c6627003be3fa1b8b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="sandboxing-a-xamarinmac-app"></a>Korumalı alan Xamarin.Mac uygulama

_Bu makalede, korumalı alan Xamarin.Mac uygulaması App Store'dan yayın için yer almaktadır. Tüm korumalı alan, kapsayıcı dizinleri, yetkilendirmeler, kullanıcı tarafından belirlenen izinler, ayrıcalık ayırma ve çekirdek zorlama gibi yerleştirilmesini öğeleri kapsar._

## <a name="overview"></a>Genel Bakış

Objective-C ya da Swift'te ile çalışırken, yaptığınız gibi bir Xamarin.Mac uygulamasında C# ve .NET ile çalışırken, sanal bir uygulama aynı yeteneğine sahip.

[![Çalışan uygulama örneği](sandboxing-images/intro01.png "çalışan uygulama örneği")](sandboxing-images/intro01-large.png#lightbox)

Bu makalede, korumalı alan Xamarin.Mac uygulama ve tüm korumalı alan gidin öğeleri ile çalışmanın temelleri şu konulara değineceğiz: kapsayıcı dizinleri, yetkilendirmeler, kullanıcı tarafından belirlenen izinler, ayrıcalık ayırma ve çekirdek zorlama. Aracılığıyla iş önerilen [Hello, Mac](~/mac/get-started/hello-mac.md) makalesi önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#Outlets_and_Actions) onu farklı bölümler temel kavramları ve biz bu makalede kullanmaya başlayacağınız teknikleri ele alınmaktadır.

Bir göz atalım isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve kullanıcı Arabirimi öğeleri, C# sınıfları wire için kullanılır.

## <a name="about-the-app-sandbox"></a>Uygulama korumalı alan hakkında

Uygulama korumalı alan bir Mac sistem kaynakları için bir uygulama olan erişimi sınırlandırma tarafından çalıştırılan kötü amaçlı bir uygulama nedeni zarar güçlü savunma sağlar.

Korumalı olmayan bir uygulama uygulamayı çalıştıran kullanıcının tam haklarına sahip ve erişebilir veya kullanıcının herhangi bir şey yapın. Uygulama güvenlik açıklarını (veya tarafından kullanıldığı framework) içeriyorsa, bir korsan büyük olasılıkla bu zayıf yararlanma ve üzerinde çalıştırıldığı Mac denetimini almak üzere uygulama kullanın.

Uygulama başına temelinde kaynaklarına erişimi kısıtlayarak, korumalı bir uygulama bir hırsızlığı, bozulma veya kötü amaçlı kullanıcının makine üzerinde çalışan bir uygulama bölümüne karşı savunma hattı sunar.

Uygulama korumalı alan iki aşamalıdır stratejisi sağlar (çekirdek düzeyinde zorunlu) macOS yerleşik bir erişim denetimi teknolojidir:

1. Uygulama korumalı alan açıklamak Geliştirici etkinleştirir _nasıl_ uygulamanın etkileşim işletim sistemi ile ve bu şekilde, bu işin tamamlanması için gereken ve artık erişim hakları verilir.
2. Uygulama korumalı alan sorunsuz bir şekilde başka Aç üzerinden erişim vermek ve iletişim kutuları kaydetmek, sürükleme ve bırakma işlemleri ve diğer, ortak kullanıcı etkileşim olanak tanır.

### <a name="preparing-to-implement-the-app-sandbox"></a>Uygulama korumalı alan uygulamak hazırlama

Ayrıntılı makalesinde açıklanan uygulama korumalı öğeler aşağıdaki gibidir:

- Kapsayıcı dizinleri
- Yetkilendirmeler
- Kullanıcı tarafından belirlenen izinleri
- Ayrıcalık ayırma
- Kernel Enforcement

Bu ayrıntılar anladıktan sonra Xamarin.Mac uygulamanızın uygulama Korumalı alan'ı benimsemeyi yönelik bir plan oluşturmak mümkün olur.

İlk olarak, uygulamanız (çoğu uygulamalardır) korumalı alan için iyi bir aday olup olmadığını belirlemek gerekir. Ardından, API uyumsuzlukları çözmek ve App Sandbox hangi öğelerinin, ihtiyaç duyacağınız belirlemek gerekir. Son olarak, uygulamanın savunma düzeyi en üst düzeye çıkarmak ayrıcalık ayrımı kullanarak arayın.

Uygulama korumalı alan'ı benimsemeyi olduğunda, uygulamanızın kullandığı bazı dosya sistemi konumlarına farklı olacaktır. Özellikle, uygulamanızın uygulama destek dosyalarını, veritabanları, önbellekler ve kullanıcı belgeleri olmayan diğer dosyalar için kullanılacak bir kapsayıcı dizini sahip olur. MacOS ve Xcode bu tür dosyaları eski konumlarından kapsayıcıya geçirmek için destek sağlar.

## <a name="sandboxing-quick-start"></a>Korumalı alan hızlı başlangıç

Bu bölümde, uygulama Sandbox ile çalışmaya başlama bir örnek olarak (korumalı alan altında özellikle istenen sürece sınırlı bir ağ bağlantısı gerektiren) bir Web görünümü kullanan basit bir Xamarin.Mac uygulaması oluşturacağız.

Uygulamayı gerçekten korumalı ve öğrenin ve ortak uygulama Korumalı alan hataları gidermek doğrularız.

### <a name="creating-the-xamarinmac-project"></a>Xamarin.Mac projesi oluşturma

Şimdi bizim örnek proje oluşturmak için aşağıdakileri yapın:

1. Mac ve tıklatın için Visual Studio'yu başlatın **yeni çözüm...** Bağlantı.
2. Gelen **yeni proje** iletişim kutusunda **Mac** > **uygulama** > **Cocoa uygulama**: 

    [![Yeni bir Cocoa uygulaması oluşturma](sandboxing-images/sample01.png "yeni Cocoa uygulaması oluşturma")](sandboxing-images/sample01-large.png#lightbox)
3. Tıklatın **sonraki** düğmesini tıklatın, girin `MacSandbox` tıklayın ve proje adı için **oluşturma** düğmesi: 

    [![Uygulama adı girerek](sandboxing-images/sample02.png "uygulama adı girme")](sandboxing-images/sample02-large.png#lightbox)
4. İçinde **çözüm paneli**, çift **Main.storyboard** dosyayı Xcode'da düzenlemek için açın: 

    [![Ana film şeridi düzenleme](sandboxing-images/sample03.png "ana film şeridi düzenleme")](sandboxing-images/sample03-large.png#lightbox)
5. Sürükleme bir **Web görünümü** içerik alanını doldurun ve büyür ve penceresiyle küçültmek için ayarlamak için pencerenin boyutu: 

    [![Bir web görünümü ekleme](sandboxing-images/sample04.png "bir web görünümü ekleme")](sandboxing-images/sample04-large.png#lightbox)
6. Adlı web görünümü için bir çıkış oluşturmak `webView`: 

    [![Yeni bir çıkış oluşturma](sandboxing-images/sample05.png "yeni bir çıkış oluşturma")](sandboxing-images/sample05-large.png#lightbox)
7. Mac ve çift Visual Studio'ya dönmek **ViewController.cs** dosyasını **çözüm paneli** düzenlemek için açın.
8. Aşağıdaki ekleme deyimini kullanarak: `using WebKit;`
9. Olun `ViewDidLoad` yöntemi görünüm aşağıdaki gibi: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Değişikliklerinizi kaydedin.

Uygulamayı çalıştırın ve Apple Web penceresinde aşağıdaki gibi görüntülendiğinden emin olun:

[![Bir örnek uygulamayı çalıştırma gösteren](sandboxing-images/sample06.png "bir örnek uygulamayı çalıştırma gösterme")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>İmzalama ve uygulama sağlama

Biz uygulama Sandbox etkinleştirmeden önce ilk sağlamak ve Xamarin.Mac uygulamamız imzalamak ihtiyacımız.

Aşağıdakileri sağlar:

1. Apple Geliştirici Portalı günlüğüne: 

    [![Apple Geliştirici Portalı'na oturum](sandboxing-images/sign01.png "Apple Geliştirici Portalı'na günlüğe kaydetme")](sandboxing-images/sign01-large.png#lightbox)
2. Seçin **tanımlayıcıları & profilleri, sertifikaları**: 

    [![Sertifikalar, tanımlayıcılar & profilleri seçme](sandboxing-images/sign02.png "tanımlayıcıları & profilleri, sertifikaları seçme")](sandboxing-images/sign02-large.png#lightbox)
3. Altında **Mac uygulamaları**seçin **tanımlayıcıları**: 

    [![Tanımlayıcıları seçme](sandboxing-images/sign03.png "tanımlayıcıları seçme")](sandboxing-images/sign03-large.png#lightbox)
4. Uygulama için yeni bir kimlik oluşturun: 

    [![Yeni bir uygulama kimliği oluşturma](sandboxing-images/sign04.png "yeni bir uygulama kimliği oluşturma")](sandboxing-images/sign04-large.png#lightbox)
5. Altında **sağlama profilleri**seçin **geliştirme**: 

    [![Geliştirme seçme](sandboxing-images/sign05.png "geliştirme seçme")](sandboxing-images/sign05-large.png#lightbox)
6. Yeni bir profil oluşturmak ve **Mac uygulaması geliştirme**: 

    [![Yeni bir profil oluşturma](sandboxing-images/sign06.png "yeni bir profil oluşturma")](sandboxing-images/sign06-large.png#lightbox)
7. Yukarıda oluşturduğumuz uygulama Kimliğini seçin: 

    [![Uygulama Kimliği seçme](sandboxing-images/sign07.png "uygulama kimliği seçme")](sandboxing-images/sign07-large.png#lightbox)
8. Bu profil için geliştiricilere seçin: 

    [![Ekleme geliştiriciler](sandboxing-images/sign08.png "ekleme geliştiriciler")](sandboxing-images/sign08-large.png#lightbox)
9. Bu profil için bilgisayarları seçin: 

    [![İzin verilen bilgisayarlar seçiliyor](sandboxing-images/sign09.png "izin verilen bilgisayarlar seçiliyor")](sandboxing-images/sign09-large.png#lightbox)
10. Profil bir ad verin: 

    [![Profil bir ad verip](sandboxing-images/sign10.png "profili bir ad verip")](sandboxing-images/sign10-large.png#lightbox)
11. Tıklatın **Bitti** düğmesi.

> [!IMPORTANT]
> Bazı durumlarda, yeni sağlama profili Apple Geliştirici portalından doğrudan indirmek ve yüklemek için çift gerekebilir. Durdurun ve yeni profili erişmek için önce Visual Studio Mac için yeniden başlatmanız gerekebilir.

Ardından, biz yeni uygulama kimliği ve profil geliştirme makinenizde yüklemeniz gerekir. Şimdi aşağıdakileri yapın:

1. Xcode'ı başlatın ve seçin **Tercihler** gelen **Xcode** menüsü: 

    ![Xcode'da hesaplarını düzenleme](sandboxing-images/sign11.png "xcode'da hesaplarını düzenleme")
2. Tıklayın **ayrıntıları görüntüle...**  düğmesi: 

    ![Ayrıntıları Görüntüle düğmesini tıklatarak](sandboxing-images/sign12.png "ayrıntıları görüntüle düğmesi")
3. Tıklayın **yenileme** (alt sol alt köşesindeki) düğmesini.
4. Tıklayın **Bitti** düğmesi.

Ardından, yeni uygulama kimliği ve sağlama profili Xamarin.Mac Projemizin seçmeliyiz. Şimdi aşağıdakileri yapın:

1. İçinde **çözüm paneli**, çift **Info.plist** dosyayı düzenlemek için açın.
2. Emin **paket tanımlayıcı** yukarıda oluşturduğumuz bizim uygulama kimliği eşleşen (örnek: `com.appracatappra.MacSandbox`): 

    [![Paket tanımlayıcısı düzenleme](sandboxing-images/sign13.png "paket tanımlayıcısı düzenleme")](sandboxing-images/sign13-large.png#lightbox)
3. Ardından, çift **Entitlements.plist** dosya ve olun bizim **iCloud anahtar değeri deposu** ve **iCloud kapsayıcıları** tüm yukarıda oluşturduğumuz bizim uygulama kimliği ile aynı (örnek: `com.appracatappra.MacSandbox`): 

    [![Entitlements.plist dosyasını düzenleyerek](sandboxing-images/sign17.png "Entitlements.plist dosya düzenleme")](sandboxing-images/sign17-large.png#lightbox)
3. Değişikliklerinizi kaydedin.
4. İçinde **çözüm paneli**, onun seçeneklerini düzenlemek için proje dosyasına çift tıklayarak açın:  

    ![Editign çözümün seçenekleri](sandboxing-images/sign14.png "Editign çözümün seçenekleri")
5. Seçin **Mac imzalama**, ardından denetleyin **uygulama paket oturum** ve **Yükleyici paketi oturum**. Altında **sağlama profili**, yukarıda oluşturduğumuz birini seçin: 

    ![Sağlama profili ayarlama](sandboxing-images/sign15.png "sağlama profili ayarlama")
6. Tıklatın **Bitti** düğmesi.

> [!IMPORTANT]
> **Not:** çıkıp yeni uygulama kimliği ve hazırlama Xcode tarafından yüklenen profili tanımak için Visual Studio edinilir Mac için yeniden başlatmanız gerekebilir.

#### <a name="troubleshooting-provisioning-issues"></a>Sağlama sorunlarını giderme

Bu noktada uygulamayı çalıştırın ve her şeyi imzalı ve doğru bir şekilde sağlanmış olduğundan emin olun çalışmanız gerekir. Uygulama hala eskisi çalıştırıyorsa, her şeyi iyi olur. Bir arıza olması durumunda, aşağıdakine benzer bir iletişim kutusu alabilirsiniz:

[![Sorunu iletişim sağlama örnek](sandboxing-images/sign16.png "sağlama sorunu iletişim kutusu örneği")](sandboxing-images/sign16-large.png#lightbox)

Sağlama ve sorunları imzalama en yaygın nedenler şunlardır:

- Uygulama paket Kimliğini seçili profilinin uygulama kimliği eşleşmiyor.
- Geliştirici seçilen profil Geliştirici kimliği eşleşmiyor.
- Test edilen Mac UUID'si seçili profilinin bir parçası kayıtlı değil.

Bir sorun olması durumunda Apple Geliştirici Portalı sorunu düzeltin, Xcode profillerinde yenileyin ve Mac için temiz bir Visual Studio derleme yapın

### <a name="enable-the-app-sandbox"></a>Uygulama korumalı alan etkinleştir

Uygulama korumalı alan projeleri seçeneklerinizi bir onay kutusunu seçerek etkinleştirin. Aşağıdakileri yapın:

1. İçinde **çözüm paneli**, çift **Entitlements.plist** dosyayı düzenlemek için açın.
2. Her ikisini de denetlemek **etkinleştirmek yetkilendirmeler** ve **etkinleştirmek uygulama Korumalı alan**: 

    [![Yetkilendirmeler düzenleme ve korumalı alan etkinleştirme](sandboxing-images/sign17.png "yetkilendirmeler düzenleme ve etkinleştirme korumalı alan")](sandboxing-images/sign17-large.png#lightbox)
3. Değişikliklerinizi kaydedin.

Bu noktada, uygulama Korumalı alan etkin ancak Web görünümü için gerekli ağ erişimi sağlamadınız. Uygulamayı şimdi çalıştırırsanız, boş bir pencere almanız gerekir:

[![Engellenme web erişimi gösteren](sandboxing-images/sample08.png "engellenme web erişimi gösterme")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>Uygulamanın korumalı olup olmadığını doğrulama

Davranış engelleme kaynak yanı sıra, bir Xamarin.Mac uygulaması başarıyla korumalı olup olmadığını bildirmek için üç ana yolu vardır:

1. Finder, içeriğini denetlemek `~/Library/Containers/` klasör - uygulama korumalı, ise olacaktır, uygulamanızın paket tanımlayıcısı gibi adlı bir klasör (örnek: `com.appracatappra.MacSandbox`): 

    [![Uygulamanın paketini açma](sandboxing-images/sample09.png "uygulamanın paketini açma")](sandboxing-images/sample09-large.png#lightbox)
2. Sistem uygulama etkinlik İzleyicisi'nde korumalı olarak görür:
    - Etkinlik İzleyicisi'ni başlatın (altında `/Applications/Utilities`). 
    - Seçin **Görünüm** > **sütunları** ve emin **korumalı alan** menü öğesi denetlenir.
    - Korumalı alan sütun okuduğu olun `Yes` uygulamanız için: 

    [![Etkinlik İzleyicisi'nde uygulama denetimi](sandboxing-images/sample10.png "etkinlik İzleyicisi'nde uygulama denetimi")](sandboxing-images/sample10-large.png#lightbox)
3. Uygulama ikili korumalı olup olmadığını denetleyin:
    - Terminal uygulaması başlatın.
    - Uygulamaları gidin `bin` dizin.
    - Bu komutu yürütün: `codesign -dvvv --entitlements :- executable_path` (burada `executable_path` uygulamanızı yolu): 

    [![Komut satırında uygulama denetimi](sandboxing-images/sample11.png "komut satırında uygulama denetimi")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>Korumalı bir uygulama hata ayıklama

Hata ayıklayıcı Xamarin.Mac uygulamalar korumalı alan, etkinleştirdiğinizde, varsayılan olarak, uygulamaya bağlanamıyor anlamına gelir, TCP üzerinden bağlanır etkin uygun izinler olmadan uygulamayı çalıştırmayı denerseniz, bir hata alıyorsunuz şekilde *"bağlanılamıyor hata ayıklayıcı"*. 

[![Gerekli seçeneklerini ayarlama](sandboxing-images/debug01.png "gerekli seçeneklerini ayarlama")](sandboxing-images/debug01-large.png#lightbox)

**İzin giden ağ bağlantıları (istemci)** izin için hata ayıklayıcı gerekli bir durumda, bu bir etkinleştirilmesi normal olarak hata ayıklama izin verir. Bu olmadan hata ayıklaması yapılamıyor olduğundan, biz güncelleştirdiniz `CompileEntitlements` için hedef `msbuild` hata ayıklama için korumalı herhangi bir uygulama yalnızca derlemeler için bu izni yetkilendirmeler otomatik olarak eklemek için. Yayın derlemeleri değiştirilmemiş yetkilendirmeler dosyasında belirtilen yetkilendirmeler kullanmanız gerekir.

### <a name="resolving-an-app-sandbox-violation"></a>Bir uygulama Korumalı alan ihlali çözme

Bir uygulama Korumalı alan ihlali uygulama olan bir kaynağın erişmeye çalıştığınız korumalı bir Xamarin.Mac açıkça izin ortaya çıkar. Örneğin, bizim Web artık Apple Web sitesini görüntülemek durum görünümü.

Uygulama korumalı alan ihlallerinin en yaygın kaynak Mac için Visual Studio'da belirtilen yetkilendirme ayarları uygulamanın gereksinimleriyle eşleşmiyor oluşur. Bizim örneğimizde, eksik ağ bağlantısı yeniden çalışmasını Web görünümü tutmak yetkilendirmeler yedekleyin.

#### <a name="discovering-app-sandbox-violations"></a>Uygulama Sandbox ihlalleri keşfetme

Bir uygulama Korumalı alan ihlali Xamarin.Mac uygulamanızda gerçekleşen şüpheleniyorsanız, sorun bulmak için en hızlı kullanarak yoludur **konsol** uygulama.

Aşağıdakileri yapın:

1. Söz konusu uygulamanın derleme ve Mac için Visual Studio'dan çalıştırma
2. Açık **konsol** uygulama (gelen `/Applications/Utilties/`).
3. Seçin **tüm iletileri** na gelin ve girin `sandbox` ara: 

    [![Konsolunda bir korumalı alan sorunu örneği](sandboxing-images/resolve01.png "bir korumalı alan sorunu konsolunda örneği")](sandboxing-images/resolve01-large.png#lightbox)

Yukarıdaki bizim örnek uygulama için Kernal engelliyor görebilirsiniz `network-outbound` uygulama biz hak istediniz değil bu yana Sandbox nedeniyle trafiği.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Uygulama korumalı alan yetkilendirme ihlallerini düzeltmek

Uygulama korumalı alan ihlallerini bulmak nasıl anlatıldığı, nasıl bunlar bizim uygulamanın yetkilendirmeler ayarlayarak çözülebilir görelim.

Aşağıdakileri yapın:

1. İçinde **çözüm paneli**, çift **Entitlements.plist** dosyayı düzenlemek için açın.
2. Altında **yetkilendirmeler** bölümünde, onay **izin giden ağ bağlantıları (istemci)** onay kutusu: 

    [![Yetkilendirmeler düzenleme](sandboxing-images/sign17.png "yetkilendirmeleri düzenleme")](sandboxing-images/sign17-large.png#lightbox)
3. Uygulamaya değişiklikleri kaydedin.

Biz bizim örnek uygulama için yukarıdaki işlemi, ardından oluşturmak ve çalıştırmak, web içeriği artık beklendiği gibi görüntülenir.

## <a name="the-app-sandbox-in-depth"></a>Ayrıntılı uygulama Korumalı alan

Uygulama korumalı alanı tarafından sağlanan erişim denetimi mekanizmaları birkaç ve kolay anlaşılır. Ancak, uygulama Korumalı alan her uygulama tarafından uygulanacaktır benzersiz ve uygulamanın gereksinimlerine bağlı olarak yoludur.

Kötü amaçlı kod tarafından yararlanan Xamarin.Mac uygulamanızı korumak için en iyi çabanızı verildiğinde, olması tek bir güvenlik açığı her iki uygulamada (veya çerçeveler ve kitaplıklar birini kullanır) ile uygulamanın etkileşimlerin denetim kazanmak için Sistem.

Uygulama korumalı alan, uygulamanın hedeflenen etkileşimleri sistemiyle belirtmenize olanak tanıyarak devralma önlemek (veya zarar görmesine neden olabilir sınırlamak için) tasarlanmıştır. Sistem, yalnızca kendi işin yapılması için uygulamanızın gerektirdiği kaynak ve hiçbir şey daha fazla erişim izni.

Uygulama korumalı alanı tasarlarken, en kötü durum bir senaryo için tasarlıyorsunuz. Uygulama tarafından kötü amaçlı kod tehlikeye hale, yalnızca dosya ve kaynakların uygulamanın korumalı alanı içinde erişimle sınırlı olur.

### <a name="entitlements-and-system-resource-access"></a>Yetkilendirmeler ve sistem kaynak erişimi

Yukarıdaki gördüğümüz gibi korumalı yapılmamış bir Xamarin.Mac uygulaması tam hakları ve uygulamayı çalıştıran kullanıcının erişim verilir. Kötü amaçlı kod tarafından ihlal edilmesi durumunda korumalı olmayan bir uygulama tehlikeli davranışı için bir aracı zarar yapmak için olası arasında değişen bir wide ile olarak çalışabilir.

Uygulama korumalı alan etkinleştirerek, tüm ayrıcalıkları, hangi, en az bir dizi kaldırın sonra Xamarin.Mac uygulamanızın yetkilendirmeler kullanılarak yalnızca gereksinimi bağlamında yeniden etkinleştirin. 

Düzenleyerek, uygulamanızın uygulama Korumalı alan kaynakları değiştirmek kendi **Entitlements.plist** dosyası ve denetleme veya düzenleyiciler açılır liste kutularından gereken haklar seçme:

[![Yetkilendirmeler düzenleme](sandboxing-images/sign17.png "yetkilendirmeleri düzenleme")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>Kapsayıcı dizin ve dosya sistemi erişimi

Uygulama korumalı alan Xamarin.Mac uygulamanızı uyarlar, aşağıdaki konumlara erişim vardır:

- **Uygulama kapsayıcı dizini** -ilk çalıştırılmasında özel bir işletim sistemi oluşturur _kapsayıcı dizini_ kaynaklarının tümü Git burada yalnızca erişilebilir olduğunu. Uygulamanın tam okuma/yazma bu dizine erişebilir.
- **Uygulama grubu kapsayıcı dizinleri** -uygulamanızı bir veya daha fazla erişim hakkı verilebilir _Grup kapsayıcıları_ aynı grupta uygulamalar arasında paylaşılan.
- **Kullanıcı tanımlı dosyaları** -uygulamanızı otomatik olarak açıkça açık veya sürüklenen ve kullanıcı tarafından uygulama bırakılan dosyalara erişim alır.
- **İlgili öğeleri** -uygun yetkilendirmeleri ile aynı adlı ancak farklı bir uzantıya sahip bir dosyaya, erişim uygulamanız olabilir. Örneğin, her ikisi kaydedilmiş bir belgeyi bir `.txt` dosyası ve bir `.pdf`.
- **Geçici dizin, komut satırı aracı dizinleri ve belirli world okunabilir konumları** -sistem tarafından belirtilen iyi tanımlanmış diğer konumlarda dosyalara erişim derecelerde uygulamanızı sahiptir.

#### <a name="the-app-container-directory"></a>Uygulama kapsayıcı dizini

Xamarin.Mac uygulamanın uygulama kapsayıcı dizini aşağıdaki özelliklere sahiptir:

- Kullanıcının giriş dizini gizli bir konumda bulunduğu (genellikle `~Library/Containers`) ve ile erişilen `NSHomeDirectory` , uygulamanızda işlevi (aşağıya bakın). Giriş dizininde olduğundan, her bir kullanıcı uygulama için kendi kapsayıcı alırsınız.
- Uygulama okuma/yazma kapsayıcı dizini ve tüm alt dizinler ve dosyalar için sınırsız erişimi.
- MacOS'ın yolu bulma API'leri uygulamanın kapsayıcı göre durumdadır. Örneğin, kapsayıcı kendi olacaktır **Kitaplığı** (aracılığıyla erişilen `NSLibraryDirectory`), **uygulama desteği** ve **Tercihler** alt dizinleri.
- macOS kurar ve arasındaki bağlantıyı uygular ve uygulama ve kod imzalama aracılığıyla kapsayıcısı. Uygulamayı kullanarak sızmasını başka bir uygulama çalıştığında olsa bile, **paket tanımlayıcısı**, kod imzalama nedeniyle kapsayıcısına erişmek mümkün olmaz.
- Kapsayıcı için oluşturulan kullanıcı dosyaları değil. Bunun yerine, veritabanları, önbellekler veya belirli diğer veri türleri gibi uygulamanızın kullandığı dosyalarını içindir.
- İçin _shoebox_ uygulama türleri (örneğin, Apple'nın fotoğraf uygulaması), kullanıcının içeriği kapsayıcıya çıkar.

> [!IMPORTANT]
> **Not:** ne yazık ki, Xamarin.Mac % 100 API kapsamı henüz (aksine Xamarin.iOS), sonuç olarak yok `NSHomeDirectory` API olmayan eşlenmiş Xamarin.Mac geçerli sürümde.

Geçici bir çözüm olarak, aşağıdaki kodu kullanabilirsiniz:

```csharp
[System.Runtime.InteropServices.DllImport("/System/Library/Frameworks/Foundation.framework/Foundation")]
public static extern IntPtr NSHomeDirectory();

public static string ContainerDirectory {
    get {
        return ((NSString)ObjCRuntime.Runtime.GetNSObject(NSHomeDirectory())).ToString ();
        }
}
```

#### <a name="the-app-group-container-directory"></a>Uygulama grubu kapsayıcı dizini

Mac macOS 10.7.5 sürümünden itibaren (ve büyük), bir uygulamanın kullanabileceği `com.apple.security.application-groups` grubundaki tüm uygulamalar için ortak bir paylaşılan kapsayıcısına erişmek için yetkilendirme. Veritabanı veya başka tür destek dosyalarını (örneğin, önbellekleri) gibi kullanıcı olmayan karşılıklı içerik, bu paylaşılan kapsayıcı kullanabilirsiniz.

(Bir grubun parçası olan) Grup kapsayıcıları otomatik olarak her uygulamanın Sandbox kapsayıcıya eklenir ve depolandığı yer `~/Library/Group Containers/<application-group-id>`. Grup Kimliği _gerekir_ örneğin geliştirme ekibi Kimliğinizi ve bir süre ile başlar:

```plist
<team-id>.com.company.<group-name>
```

Daha fazla bilgi için lütfen Apple'nın bkz [uygulama grubu uygulamaya ekleme](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) içinde [yetkilendirme anahtar başvurusu](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Uygulama kapsayıcısı dışında Powerbox ve dosya sistemi erişimi

Korumalı bir Xamarin.Mac uygulama dosya sistemi konumlarına kapsayıcısı dışında aşağıdaki yöntemlerle erişebilirsiniz:

- Kullanıcının (aracılığıyla, açık ve Kaydet iletişim kutularını veya sürükle ve bırak gibi diğer yöntemleri) belirli talimatıyla.
- Yetkilendirmeler için belirli dosya sistemi konumlarını kullanarak (gibi `/bin` veya `/usr/lib`).
* Dosya sistemi konumu world (paylaşım gibi) okunabilir olan belirli dizinlerdeki olduğunda.

_Powerbox_ macOS korumalı Xamarin.Mac uygulamanızın dosya erişim hakları genişletmek için kullanıcıyla etkileşime giren güvenlik teknolojisidir. Powerbox hiçbir API var, ancak uygulama çağırdığında şeffaf bir şekilde etkinleştirilmiş bir `NSOpenPanel` veya `NSSavePanel`. Powerbox erişim Xamarin.Mac uygulamanız için ayarladığınız yetkilendirmeler aracılığıyla etkinleştirilir.

Korumalı bir uygulama açık görüntüler veya iletişim kaydettiğinizde pencere Powerbox (ve değil AppKit) tarafından sunulan ve bu nedenle herhangi bir dosya veya kullanıcı erişimi dizin erişimi.

Bir kullanıcı bir dosya veya dizin açık seçer veya iletişim kaydedin (veya uygulamanın simgesine ya da sürüklediği), Powerbox ilişkili yolu uygulamanın korumalı alanda ekler.

Ayrıca, sistem otomatik olarak aşağıdaki korumalı bir uygulamaya izin verir:

- Bir sistem giriş yöntemi bağlantısı.
- Kullanıcı tarafından seçilen Hizmetleri çağrısı **Hizmetleri** menü (Hizmetleri olarak işaretlenmiş için yalnızca _sanal uygulamalar için güvenli_ hizmet sağlayıcısı tarafından).
- Açık dosyaları seçin kullanıcı tarafından **açık son** menüsü.
- Kopyala ve Yapıştır diğer uygulamalar arasında kullanın.
- Dosyaları aşağıdaki world okunabilir konumlardan okuyun:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Dosyaları okuma ve tarafından oluşturulan dizinlerde yazma `NSTemporaryDirectory`.

(Uygulama sonlandırıldığında dosyayı hala açık olduğu sürece) uygulama sonlanana kadar varsayılan olarak açık veya korumalı Xamarin.Mac uygulama tarafından kaydedilen dosyaları erişilebilir kalır. Açık dosyaları otomatik olarak uygulamanın sandbox macOS Sürdür özelliği aracılığıyla uygulamayı bir sonraki başlatılmasında geri yüklenir.

Kalıcılığı Xamarin.Mac uygulamanın kapsayıcı dışında bulunan dosyalara sağlamak için güvenlik kapsamı olan yer işaretleri (aşağıya bakın) kullanın.

#### <a name="related-items"></a>İlgili öğeler

Uygulama korumalı alan farklı uzantıları ancak aynı dosya adını taşıyan ilgili öğeler erişmek bir uygulama sağlar. Özellik iki bölümden oluşur: a) uygulamasının ilgili uzantılarının listesini `Info.plst` dosya, (b) ne uygulama bu dosyalarla istediğimizi korumalı alan söyleyin kodu.

Burada bu mantıklı iki senaryo vardır:

1. Uygulama (yeni uzantılı) dosya farklı bir sürümü kaydetmek gerekir. Örneğin, dışa aktarma bir `.txt` dosyasını bir `.pdf` dosyası. Bu durumu yönetmek için kullanmanız gerekir bir `NSFileCoordinator` dosyaya erişmek için. Çağrı `WillMove(fromURL, toURL)` yöntemi ilk olarak, yeni uzantı dosyayı taşıyın ve sonra arama `ItemMoved(fromURL, toURL)`.
2. Uygulamanın bir uzantısı olan ana dosya ve çeşitli destekleyici dosyalar ile farklı Uzantıları'nı açmak için gerekir. Örneğin, bir filmi ve alt başlık dosyası. Kullanım bir `NSFilePresenter` ikincil dosya erişmek için. Ana dosya sağlamak `PrimaryPresentedItemURL` özelliği ve ikincil dosyasını `PresentedItemURL` özelliği. Ana dosya açıldığında çağrısı `AddFilePresenter` yöntemi `NSFileCoordinator` sınıfı ikincil dosyasına kaydedilemedi.

Her iki senaryolarında, uygulama kullanıcının **Info.plist** dosya, uygulamayı açabilirsiniz belge türü bildirmelidir. Herhangi bir dosya türü için ekleme `NSIsRelatedItemType` (değerini `YES`) kendi girişi `CFBundleDocumentTypes` dizi.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Açın ve korumalı uygulamaları ile iletişim davranış Kaydet

Aşağıdaki sınırları yerleştirilir `NSOpenPanel` ve `NSSavePanel` bunları korumalı Xamarin.Mac uygulamadan çağrılırken:

- Program aracılığıyla çağrılamıyor **Tamam** düğmesi.
- Bir kullanıcının seçimde program aracılığıyla değiştirilemiyor bir `NSOpenSavePanelDelegate`.

Ayrıca, aşağıdaki devralma değişiklikleri yerinde şunlardır:

- **Korumalı olmayan uygulama** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Güvenlik kapsamı olan yer işaretleri ve kalıcı kaynak erişimi

Yukarıda belirtildiği gibi bir korumalı Xamarin.Mac uygulama bir dosya veya kapsayıcısı (PowerBox tarafından sağlanan gibi) doğrudan kullanıcı etkileşimi yapmamanız dışında kaynağa erişebilir. Ancak, bu kaynaklara erişim otomatik olarak uygulama başlatır veya sistem yeniden başlatmalar kalmaz.

Kullanarak _Security-Scoped yer işaretleri_, korumalı Xamarin.Mac uygulama kullanıcının istediğinden korumak ve kaynakları bir uygulama sonra verilen yeniden başlatmak için erişim.

#### <a name="security-scoped-bookmark-types"></a>Güvenlik kapsamı yer işareti türleri

Security-Scoped yer işaretleri ve kalıcı kaynak erişimi ile çalışırken, iki sistine kullanım örnekleri vardır:

- **App-Scoped yer işareti kalıcı bir kullanıcı tarafından belirtilen dosya veya klasöre erişim sağlar.** 

    Örneğin, korumalı Xamarin.Mac uygulamayı düzenlemek için bir dış belgesi açmak için kullanılacak veriyorsa (kullanarak bir `NSOpenPanel`), böylece aynı dosyayı yeniden gelecekte erişebilir uygulama App-Scoped yer işareti oluşturabilirsiniz.
- **Document-Scoped yer işareti, alt dosya belirli bir belge kalıcı erişmenizi sağlar.** 

Örneğin, bir proje dosyası oluşturan bir video düzenleme uygulaması ayrı ayrı görüntülere, video klip ve daha sonra tek bir filmi birleştirilir ses dosyalarını erişebilir.

Ne zaman kullanıcı içe aktaran bir kaynak dosyası projeye (aracılığıyla bir `NSOpenPanel`), uygulama projesinde depolanmış öğesi için Document-Scoped yer işareti oluşturur ve böylece dosyayı her zaman uygulamaya erişilebilir.

Document-Scoped yer işareti yer işareti veri ve belge açabilirsiniz herhangi bir uygulama tarafından çözülebilir. Bu, kullanıcının başka bir kullanıcı ve tüm yer işaretleri için de çalışacak sahip proje dosyalarını göndermek izin taşınabilirlik destekler.

> [!IMPORTANT]
> **Not:** Document-Scoped Bookman yapabilirsiniz _yalnızca_ noktası tek bir dosya ve klasör değil ve bu dosya sistemi tarafından kullanılan bir konumda olamaz (gibi `/private` veya `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Güvenlik kapsamı yer işaretlerini kullanma

İki tür Security-Scoped yer işareti kullanarak, aşağıdaki adımları gerçekleştirin gerektirir:

1. **Security-Scoped yer işaretleri kullanması gereken Xamarin.Mac uygulamada uygun yetkilendirmeleri ayarlama** -For App-Scoped yer işaretleri ayarlama `com.apple.security.files.bookmarks.app-scope` yetkilendirme anahtarına `true`. Document-Scoped yer işaretleri için ayarlanmış `com.apple.security.files.bookmarks.document-scope` yetkilendirme anahtarına `true`.
2. **Security-Scoped yer işareti oluşturmak** -herhangi bir dosya veya kullanıcı için erişim sağladığınız klasörü için bunu (aracılığıyla `NSOpenPanel` gibi), uygulama kalıcı erişimi gerekir. Kullanım `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` yöntemi `NSUrl` yer işareti oluşturmak için sınıfı.
3. **Security-Scoped yer işareti gidermek** - uygulama (örneğin, yeniden başlatma sonrasında) yeniden kaynağa erişmek gerektiğinde bir güvenlik kapsamı URL'sine yer işareti çözümlemek gerekir. Kullanım `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` yöntemi `NSUrl` yer işareti çözümlemek için sınıf.
4. **Açıkça Security-Scoped URL'den dosyaya erişmek istediğiniz sistem bilgisini** - bu adımı hemen Security-Scoped URL yukarıdaki aldıktan sonra yapılması gerektiğini veya, daha sonra istediğinizde sahip sonra kaynak erişimi yeniden kazanmak Bunu erişiminizi relinquished. Çağrı `StartAccessingSecurityScopedResource ()` yöntemi `NSUrl` Security-Scoped URL'ye erişmesini başlatmak için sınıf.
5. **Açıkça tamamladığınızda sistem bilgisini Security-Scoped URL'den dosyaya erişirken** -olabildiğince çabuk, (örneğin, kullanıcı bunu kapatırsa) uygulama artık dosyaya erişim sağlaması gerektiğinde sistem bildirmek. Çağrı `StopAccessingSecurityScopedResource ()` yöntemi `NSUrl` Security-Scoped URL'ye erişmesini durdurmak için sınıf.

Bir kaynağa erişim teknisyene sonra yeniden erişim yeniden oluşturmak için 4. adıma dönmek gerekir. Xamarin.Mac uygulama yeniden başlatılırsa, 3. adıma dönün ve yer işareti yeniden çözümlemek.

> [!IMPORTANT]
> **Not:** Security-Scoped URL kaynaklarına erişimini yayımlamayı hatası çekirdek kaynakları sızıntısı için bir Xamarin.Mac uygulaması neden olur. Sonuç olarak, uygulama artık yeniden başlatılana kadar kapsayıcısı için dosya sistemi konumlarına eklemeniz mümkün olacaktır.

### <a name="the-app-sandbox-and-code-signing"></a>Uygulama korumalı alan ve kod imzalama

Uygulama korumalı alan etkinleştirmek ve Xamarin.Mac uygulamanız (aracılığıyla yetkilendirmeler) için özel gereksinimleri etkinleştirmek sonra oturum proje etkili olması korumalı alan için kod gerekir. Kod imzalama uygulama Korumalı alan için gerekli yetkilendirmeler uygulamanın imza bağlantılı olduğundan gerçekleştirmeniz gerekir. 

bir uygulamanın kapsayıcı ve başka bir uygulama paketi kimliği uygulamaları yanıltma olsa bile bu kapsayıcı erişebilirsiniz bu şekilde kodu imzası arasında bir bağlantı macOS zorlar Bu mekanizma şöyle çalışır:

1. Sistem uygulamanın kapsayıcı oluşturduğunda, bu ayarlar bir _erişim denetim listesi_ (ACL) bu kapsayıcısı üzerinde. Listedeki ilk erişim denetim girdisi uygulamanın içeren _belirlenmiş gereksinim_ (DR) uygulama nasıl gelecek sürümlerinde açıklayan tanımasını (yükseltildiğinde).
2. Aynı paket kimliği ile bir uygulamayı başlatır, her zaman sistem uygulamanın kodu imzası belirlenmiş kapsayıcının ACL girişleri birini belirtilen gereksinimler eşleşip eşleşmediğini denetler. Sistem bir eşleşme bulamazsa, uygulama başlatmasını engeller.

İmzalama works aşağıdaki yollardan kod:

1. Xamarin.Mac proje oluşturmadan önce Apple Geliştirici Portalı'ndan geliştirme sertifikası, bir dağıtım sertifikası ve bir geliştirici kimliği sertifika edinin.
2. Mac App Store Xamarin.Mac uygulama dağıtır, bir Apple kod imzayla imzalanır.

Test ve hata ayıklama, bir uygulamanın sürümüne göre (hangi uygulama kapsayıcısı oluşturmak için kullanılacak) imzalı Xamarin.Mac kullanırsınız. Daha sonra test veya Apple uygulama Mağazası'ndan sürümünü yüklemek isterseniz, Apple imza ile imzalandığından ve bu yana (aynı kodu imza özgün uygulama kapsayıcısı olarak yok) başlatmak başarısız olur. Bu durumda, aşağıdakine benzer bir kilitlenme raporu alırsınız:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Bu sorunu gidermek için uygulamayı Apple imzalı sürümüne işaret edecek şekilde ACL girişi ayarlamasını gerekir.

Oluşturma ve sağlama için korumalı alan gerekli profilleri indirme hakkında daha fazla bilgi için lütfen bkz [imzalama ve uygulama sağlama](#Signing_and_Provisioning_the_App) yukarıdaki bölümde.

#### <a name="adjusting-the-acl-entry"></a>ACL girişi ayarlama

Xamarin.Mac uygulamayı çalıştırmak için Apple imzalı sürümünü izin vermek için aşağıdakileri yapın:

1. Terminal uygulamasını açın (içinde `/Applications/Utilities`).
2. Xamarin.Mac uygulama Apple imzalı sürümüne Bulucu penceresini açın.
3. Tür `asctl container acl add -file ` Terminal penceresinde.
4. Xamarin.Mac uygulamanın simgesini Bulucu penceresinden sürükleyip üzerinde Terminal penceresi bırakın.
5. Dosyanın tam yolunu Terminal komutta eklenir.
6. Tuşuna **Enter** komutu yürütülemedi.

Kapsayıcının ACL şimdi iki sürümünü de Xamarin.Mac uygulama için belirtilen kodu gereksinimleri içerir ve macOS şimdi çalıştırmak iki sürümden izin verir.

#### <a name="display-a-list-of-acl-code-requirements"></a>ACL kodu gereksinimleri listesini görüntüleyin

Aşağıdakileri yaparak bir kapsayıcının ACL'de kodu gereksinimlerinin listesi görüntüleyebilirsiniz:

1. Terminal uygulamasını açın (içinde `/Applications/Utilities`).
2. Türü `asctl container acl list -bundle <container-name>`.
3. Tuşuna **Enter** komutu yürütülemedi.

`<container-name>` Genellikle Xamarin.Mac uygulama için paket tanımlayıcısı değil.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Uygulama korumalı bir Xamarin.Mac uygulaması tasarlama

Uygulama korumalı alanı Xamarin.Mac uygulama tasarlarken, izlenmesi gereken ortak bir iş akışı yok. Bu, korumalı alan bir uygulamada uygulama ayrıntıları verilen uygulamanın işlevini benzersiz olacak belirtti.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Uygulama korumalı alan'ı benimsemeyi altı adımları

Xamarin.Mac uygulama için uygulama Korumalı alan genellikle tasarlarken aşağıdaki adımlardan oluşur:

1. Uygulamanın Korumalı alan için uygun olup olmadığını belirler.
2. Geliştirme ve dağıtım stratejisini tasarlayın.
3. API uyumsuzlukları çözümleyin.
4. Gerekli uygulama Korumalı alan yetkilendirmeler Xamarin.Mac projeye uygulanır.
5. XPC kullanarak ayrıcalık ayrımı ekleyin.
6. Geçiş stratejisi uygulayın.

> [!IMPORTANT]
> **Not:** yalnızca sandbox ana yürütülebilir dosya, uygulama paketi, ancak aynı zamanda dahil her yardımcı gerekir uygulama veya bu paketteki aracı. Bu Mac uygulama Mağazası'ndan dağıtılmış herhangi bir uygulama için gereklidir ve mümkünse, uygulama dağıtım herhangi diğer bir form için yapılması gerekir.

Xamarin.Mac uygulamanın paketteki tüm yürütülebilir ikili dosyalarının listesi için Terminal içinde aşağıdaki komutu yazın:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Burada `[Your-App-Bundle]` uygulamanın paket yolu ve adı değil.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Xamarin.Mac uygulama Korumalı alan için uygun olup olmadığını belirleme

Çoğu Xamarin.Mac uygulamaları uygulama Sandbox ile tam uyumludur ve bu nedenle, korumalı alan için uygundur. Uygulama, uygulama korumalı alanı izin vermeyen bir davranış gerektiriyorsa, alternatif bir yaklaşım düşünmelisiniz.

Uygulamanızı aşağıdaki davranışları gerektiriyorsa, uygulama Sandbox ile uyumlu değil:

- **Yetkilendirme Hizmetleri** -ile uygulama Sandbox, çalışamıyor açıklanan işlevlerle [yetkilendirme hizmetleri C başvurusu](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **Erişilebilirlik API'leri** -ekran okuyucular gibi sandbox yardımcı uygulamaları veya diğer uygulamalar kontrol uygulamalar olamaz.
- **Rastgele uygulamalara Apple olayları göndermek** -uygulama bilinmeyen, rastgele bir uygulama için Apple olayların gönderilmesi gerekiyorsa, korumalı olamaz. Çağrılan uygulamaların bilinen bir listesini, uygulama hala korumalı olabilir ve yetkilendirmeler çağrılan uygulama listesinde dahil etmeniz gerekir.
- **Kullanıcı bilgileri sözlükleri diğer görevlere dağıtılmış bildirimleri gönderme** -ile uygulama Sandbox içeremez bir `userInfo` için nakil sırasında sözlük bir `NSDistributedNotificationCenter` diğer görevleri Mesajlaşma nesnesi.
- **Yük çekirdek uzantıları** -çekirdek uzantıları yüklenmesini uygulama korumalı alanı tarafından engellendi.
- **Kullanıcı Giriş Aç ve Kaydet iletişim kutularındaki benzetimi** - program aracılığıyla açma düzenleme veya benzetimini yapmak veya kullanıcı girişi alter için iletişim kutuları uygulama korumalı alanı tarafından engellendi.
- **Erişme veya diğer uygulamalar üzerinde ayarı tercihleri** -diğer uygulamalar ayarlarını düzenleme uygulama korumalı alanı tarafından yasaktır.
- **Ağ ayarlarını yapılandırma** -ağ ayarlarını düzenleme uygulama korumalı alanı tarafından yasaktır.
- **Diğer uygulamalar sonlandırma** -uygulama Korumalı alan yasaklar kullanarak `NSRunningApplication` diğer uygulamalar sonlandırılacak.

### <a name="resolving-api-incompatibilities"></a>API uyumsuzlukları çözme

Bir Xamarin.Mac uygulaması için uygulama Korumalı alan tasarlarken, bazı macOS API'leri kullanımını uyumsuzluklar karşılaşabilirsiniz.

Bazı ortak sorunlar ve bunları çözmek için yapabilecekleriniz şunlardır:

- **Açma, kaydetme ve belgeleri izleme** - belgeleri dışındaki herhangi bir teknoloji kullanarak yönetiyorsanız `NSDocument`, kendisine uygulama Korumalı alan için yerleşik destek nedeniyle geçmelisiniz. `NSDocument` otomatik olarak PowerBox ile çalışır ve kullanıcı bunları Finder taşırsa belgeleri, korumalı alan içinde tutmak için destek sağlar.
- **Dosya sistemi kaynaklara erişimi korur** - kapsayıcısının dışında kaynaklarına kalıcı erişimi bağımlı uygulama Xamarin.Mac kullanıyorsanız, güvenlik kapsamına yer işaretleri erişim için.
- **Bir uygulama için bir oturum açma öğesi oluşturmak** -ile uygulama Sandbox öğesi kullanarak bir oturum açma oluşturamıyor `LSSharedFileList` veya kullanarak başlatma hizmetlerinin durumunu işleyebileceğiniz `LSRegisterURL`. Kullanım `SMLoginItemSetEnabled` elmalar içinde açıklandığı gibi işlev [Service Management Framework kullanarak oturum açma öğeleri ekleme](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) belgeleri.
- **Kullanıcı verilerine erişme** - POSIX işlevleri gibi kullanıyorsanız `getpwuid` directory Hizmetler'den kullanıcının giriş dizini için Cocoa veya çekirdek Foundation simgeler gibi kullanmayı düşünün `NSHomeDirectory`.
- **Tercihler, diğer uygulamalara erişen** - uygulama Korumalı alan yolu-API uygulamanın kapsayıcıya bulma Tercihler gerçekleşmesi bu kapsayıcıdaki değiştirme ve başka uygulamalar tercihlerinde izin erişme yönlendirir. 
- **HTML5 katıştırılmış Video Web görünümleri kullanma** -Xamarin.Mac uygulama katıştırılmış HTML5 videoları yürütmek için WebKit kullanıyorsa, uygulamayı AV Foundation framework karşı ayrıca bağlamanız gerekir. Uygulama korumalı alan, aksi takdirde bu videolar play'den CoreMedia engel olur.

### <a name="applying-required-app-sandbox-entitlements"></a>Gerekli uygulama Sandbox yetkilendirmeler uygulama

Yetkilendirmeleri denetlemek ve uygulama korumalı alanda çalıştırmak istediğiniz herhangi bir Xamarin.Mac uygulama için düzenlemeniz gerekir **etkinleştirmek uygulama Korumalı alan** onay kutusu.

Uygulamanın işlevselliğini bağlı olarak, işletim sistemi özellikleri veya kaynaklarına diğer yetkilendirmeler etkinleştirmeniz gerekebilir. Bu nedenle, uygulamanızı çalıştırmak için gereken tam düşük isteği yetkilendirmeler küçülttüğünüzde en iyi uygulama Korumalı alan works yalnızca rastgele yetkilendirmeleri etkinleştirin.

Xamarin.Mac uygulama gerekiyor. hangi yetkilendirmeler belirlemek için aşağıdakileri yapın:

1. Uygulama korumalı alan etkinleştirin ve Xamarin.Mac uygulamayı çalıştırın.
2. Uygulama özellikleri üzerinden çalıştırın.
3. Konsol uygulamasını açın (kullanılabilir `/Applications/Utilities`) ve Ara `sandboxd` ihlalleri içinde **tüm iletileri** günlük.
4. Her `sandboxd` ihlali, diğer dosya sistemi konumlarına yerine uygulama kapsayıcısı kullanarak ya da sorunu çözmek için veya uygulama Korumalı alan yetkilendirmeler kısıtlı işletim sistemi özellikleri erişimi etkinleştirmek için geçerlidir.
5. Yeniden çalıştırın ve tüm Xamarin.Mac uygulama özelliklerini yeniden sınayın.
6. Tüm kadar yineleme `sandboxd` ihlalleri çözümlendi.

### <a name="add-privilege-separation-using-xpc"></a>Ayrıcalık ayrımı XPC kullanarak ekleme

Bir Xamarin.Mac uygulaması için uygulama Korumalı alan geliştirirken, uygulamanın davranışları ayrıcalıklara ve erişim şartlarında bakın, sonra yüksek riskli işlemleri kendi XPC hizmetlerine ayırarak göz önünde bulundurun.

Daha fazla bilgi için Apple'nın bkz [XPC hizmetleri oluşturma](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) ve [Deamon'lar ve Hizmetleri Programlama Kılavuzu](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Uygulama geçiş stratejisi

Yeni bir korumalı, daha önce korumalı olmayan bir Xamarin.Mac uygulaması sürümü serbest geçerli kullanıcıların kesintisiz bir yükseltme yolu olduğundan emin olmak gerekir. 

 Apple'nın nasıl bir kapsayıcı geçiş bildirimi uygulanacağı hakkında daha fazla bilgi için okuma [bir korumalı alan için bir uygulama geçirme](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) belgeleri.

## <a name="summary"></a>Özet

Bu makalede korumalı alan ayrıntılı bir bakış Xamarin.Mac uygulama sürdü. İlk olarak, uygulama Sandbox temelleri göstermek için bir yalnızca Xamarin.Mac uygulaması oluşturduk. Ardından, korumalı alan ihlalleri gidermek nasıl gösterdi. Ardından, biz bir ayrıntılı sürdü uygulama Sandbox bakın ve son olarak, uygulama korumalı alanı Xamarin.Mac uygulama tasarlama sırasında arama.



## <a name="related-links"></a>İlgili bağlantılar

- [App Store’da Yayımlama](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [Uygulama Sandbox hakkında](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Genel Uygulama korumalı alan sorunları](https://developer.apple.com/library/content/qa/qa1773/_index.html)
