---
title: Bir Xamarin.Mac uygulamasını korumalı alana alma
description: Bu makale App Store yayına yönelik bir Xamarin.Mac uygulamasını korumalı alana alma kapsar. Kapsayıcı dizinleri, yetkilendirmeleri, kullanıcı tarafından belirlenen izinler, ayrıcalık ayrımı ve çekirdeği zorlama gibi korumalı alana alma tarihinden itibaren tüm öğeleri içerir.
ms.prod: xamarin
ms.assetid: 06A2CA8D-1E46-410F-8C31-00EA36F0735D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9e6cfd39260b76c5097d8faa24f406f45681ac8d
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "43780658"
---
# <a name="sandboxing-a-xamarinmac-app"></a>Bir Xamarin.Mac uygulamasını korumalı alana alma

_Bu makale App Store yayına yönelik bir Xamarin.Mac uygulamasını korumalı alana alma kapsar. Kapsayıcı dizinleri, yetkilendirmeleri, kullanıcı tarafından belirlenen izinler, ayrıcalık ayrımı ve çekirdeği zorlama gibi korumalı alana alma tarihinden itibaren tüm öğeleri içerir._

## <a name="overview"></a>Genel Bakış

Objective-C ya da Swift'te çalışırken olduğu gibi bir Xamarin.Mac uygulamasında çalışırken, C# ve .NET ile sanal uygulama aynı yeteneğine sahip.

[![Çalışan uygulamaya örneği](sandboxing-images/intro01.png "çalıştıran uygulama örneği")](sandboxing-images/intro01-large.png#lightbox)

Bu makalede, korumalı alana alma bir Xamarin.Mac uygulamasını ve korumalı alana alma Git tüm öğeleri ile çalışma hakkındaki temel bilgileri şu konulara değineceğiz: kapsayıcı dizinleri, yetkilendirmeleri, kullanıcı tarafından belirlenen izinler, ayrıcalık ayrımı ve çekirdeği zorlama. Aracılığıyla iş önerilen [Merhaba, Mac](~/mac/get-started/hello-mac.md) makale önce özellikle [Xcode ve arabirim Oluşturucu giriş](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) ve [çıkışlar ve eylemleri](~/mac/get-started/hello-mac.md#outlets-and-actions) olarak bölümlerde temel kavramları ve bu makalede kullanacağız tekniklerini ele alınmaktadır.

Bir göz atın isteyebilirsiniz [gösterme C# sınıfları / Objective-C yöntemlere](~/mac/internals/how-it-works.md) bölümünü [Xamarin.Mac iç işlevleri](~/mac/internals/how-it-works.md) de açıklar belge `Register` ve `Export` öznitelikleri Objective-C nesneleri ve UI öğeleri, C# sınıfları kablo kullanılır.

## <a name="about-the-app-sandbox"></a>Uygulama korumalı alanı hakkında

Uygulama korumalı alanı, sistem kaynakları için bir uygulamanın sahip olduğu erişimi kısıtlayarak bir Mac bilgisayarda çalıştırılan kötü amaçlı bir uygulamayı kaynaklanan hasarın karşı güçlü koruma sağlar.

Korumalı olmayan bir uygulama uygulamayı çalıştıran kullanıcının tüm haklara sahip olduğu ve erişebileceğini veya uygulamanın kullanıcının herhangi bir şey yapın. Uygulama güvenlik açıkları (veya bunu kullanarak herhangi bir çerçeveyi) içeriyorsa, bir bilgisayar korsanının büyük olasılıkla bu zayıf yararlanma ve uygulamanın çalıştığı MAC denetimi kullanmak.

Uygulama başına temelinde kaynaklarına erişimi sınırlayarak, korumalı bir uygulama bir hırsızlık, bozulma veya kötü amaçlı kullanıcının makine üzerinde çalışan bir uygulama yoluna karşı savunma hattı sunar.

Uygulama korumalı alanı iki aşamalıdır stratejisi sunar (çekirdek düzeyinde zorunlu tutulur) macOS yerleşik bir erişim denetimi teknolojisidir:

1. Uygulama korumalı alanı tanımlamak Geliştirici sağlayan _nasıl_ uygulama etkileşim işletim sistemini ve bu şekilde, bu işin tamamlanması için gereken ve artık erişim hakları bahşedilir.
2. Uygulama korumalı alanı, kullanıcının sorunsuz bir şekilde daha fazla açık üzerinden sisteme erişim vermek ve iletişim kutuları Kaydet sürükleyip işlemleri ve diğer yaygın kullanıcı etkileşimlerini izin verir.

### <a name="preparing-to-implement-the-app-sandbox"></a>Uygulama korumalı alanı uygulamak hazırlama

Makalesinde ayrıntılı olarak ele uygulama korumalı alanı öğelerini aşağıdaki gibidir:

- Kapsayıcı dizinleri
- Yetkilendirmeler
- Kullanıcı tarafından belirlenen izinler
- Ayrıcalık ayrımı
- Çekirdeği zorlama

Bu ayrıntılar anladıktan sonra uygulama korumalı alanı Xamarin.Mac uygulamanızdaki'nu benimseme bir plan oluşturmanız mümkün olacaktır.

İlk olarak, uygulamanızı korumalı alana alma (çoğu uygulama olduğu) için iyi bir adaydır olup olmadığını belirlemek gerekir. Ardından, API uyumsuzlukları çözün ve uygulama korumalı alanı, hangi öğelerin gerektirir belirlemeniz gerekir. Son olarak, uygulamanın defense düzeyi en üst düzeye çıkarmak için ayrıcalık ayrımı kullanarak bakın.

Uygulama korumalı alanı benimsenmesi, uygulamanızın kullandığı bazı dosya sistemi konumları farklı olacaktır. Özellikle, uygulamanızın uygulama destek dosyaları, veritabanları, önbellekler ve kullanıcı belgelerini olmayan diğer dosyalar için kullanılacak bir kapsayıcı dizini gerekir. MacOS hem Xcode bu tür dosyaları eski konumlarından kapsayıcıya geçirmek için destek sağlar.

## <a name="sandboxing-quick-start"></a>Korumalı alana alma hızlı başlangıç

Bu bölümde, uygulama korumalı alanı ile çalışmaya başlama örneği olarak (özellikle istenmedikçe korumalı alana alma altında kısıtlı bir ağ bağlantısı gerektiren) bir Web görünümü kullanan basit bir Xamarin.Mac uygulamasını oluşturacağız.

Uygulamayı gerçekten korumalı öğrenin ve genel uygulama korumalı alanı hatalarını gidermek olduğunuzu doğrulayacağız ve.

### <a name="creating-the-xamarinmac-project"></a>Xamarin.Mac projesi oluşturma

Diyelim ki bizim örnek proje oluşturmak için aşağıdakileri yapın:

1. Mac ve tıklatın için Visual Studio'yu başlatın **yeni çözüm...** bağlantı.
2. Gelen **yeni proje** iletişim kutusunda **Mac** > **uygulama** > **Cocoa uygulaması**: 

    [![Yeni bir Cocoa uygulaması oluşturma](sandboxing-images/sample01.png "yeni bir Cocoa uygulaması oluşturma")](sandboxing-images/sample01-large.png#lightbox)
3. Tıklayın **sonraki** düğmesi, girin `MacSandbox` tıklayın ve proje adı için **Oluştur** düğmesi: 

    [![Uygulama adı girerek](sandboxing-images/sample02.png "uygulama adı girerek")](sandboxing-images/sample02-large.png#lightbox)
4. İçinde **çözüm bölmesi**, çift **Main.storyboard** dosyayı Xcode'da düzenlemek üzere açın: 

    [![Ana görsel taslak düzenleme](sandboxing-images/sample03.png "ana görsel taslak düzenleme")](sandboxing-images/sample03-large.png#lightbox)
5. Sürükleme bir **Web görünümü** büyütme ve küçültme penceresiyle ayarlayın ve içerik alanı dolduracak şekilde penceresinden, boyut: 

    [![Web görünümü ekleme](sandboxing-images/sample04.png "web görünüm ekleme")](sandboxing-images/sample04-large.png#lightbox)
6. Adlı web görünümü için bir çıkış oluşturma `webView`: 

    [![Yeni çıkış oluşturma](sandboxing-images/sample05.png "yeni çıkış oluşturma")](sandboxing-images/sample05-large.png#lightbox)
7. Mac ve çift için Visual Studio'ya geri dönün **ViewController.cs** dosyası **çözüm bölmesi** düzenlemek üzere açın.
8. Aşağıdaki using deyimi: `using WebKit;`
9. Olun `ViewDidLoad` yöntemi görünüm aşağıdaki gibi: 

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();
    webView.MainFrame.LoadRequest(new NSUrlRequest(new NSUrl("http://www.apple.com")));
}
```

10. Değişikliklerinizi kaydedin.

Uygulamayı çalıştırmak ve Apple Web penceresinde şu şekilde görüntülendiğinden emin olun:

[![Örnek uygulamayı çalıştırma gösteren](sandboxing-images/sample06.png "örnek uygulamayı çalıştırma gösteriliyor")](sandboxing-images/sample06-large.png#lightbox)

<a name="Signing_and_Provisioning_the_App" />

### <a name="signing-and-provisioning-the-app"></a>İmzalama ve uygulama sağlama

Uygulama korumalı alanı etkinleştirmeden önce ilk sağlama ve Xamarin.Mac uygulamamız imzalamak ihtiyacımız var.

Aşağıdakileri sağlar:

1. Apple Geliştirici Portalı oturum: 

    [![Apple Geliştirici portalda oturumunuzu açarken](sandboxing-images/sign01.png "Apple Developer Portal'a günlüğe kaydetme")](sandboxing-images/sign01-large.png#lightbox)
2. Seçin **tanımlayıcıları & profilleri, Sertifikalar**: 

    [![Tanımlayıcıları & profilleri, sertifikalar seçerek](sandboxing-images/sign02.png "tanımlayıcıları & profilleri, sertifikalar seçme")](sandboxing-images/sign02-large.png#lightbox)
3. Altında **Mac uygulamaları**seçin **tanımlayıcıları**: 

    [![Tanımlayıcıları seçerek](sandboxing-images/sign03.png "tanımlayıcıları seçme")](sandboxing-images/sign03-large.png#lightbox)
4. Uygulama için yeni bir kimlik oluşturun: 

    [![Yeni bir uygulama kimliği oluşturma](sandboxing-images/sign04.png "yeni bir uygulama kimliği oluşturma")](sandboxing-images/sign04-large.png#lightbox)
5. Altında **sağlama profilleri**seçin **geliştirme**: 

    [![Geliştirme seçerek](sandboxing-images/sign05.png "geliştirme seçme")](sandboxing-images/sign05-large.png#lightbox)
6. Yeni bir profil oluşturun ve seçin **Mac uygulama geliştirme**: 

    [![Yeni bir profil oluşturma](sandboxing-images/sign06.png "yeni bir profil oluşturma")](sandboxing-images/sign06-large.png#lightbox)
7. Yukarıda oluşturduğumuz uygulama Kimliğini seçin: 

    [![Uygulama Kimliği seçerek](sandboxing-images/sign07.png "uygulama kimliği seçme")](sandboxing-images/sign07-large.png#lightbox)
8. Geliştiriciler bu profil için seçin: 

    [![Ekleme geliştiriciler](sandboxing-images/sign08.png "ekleme geliştiriciler")](sandboxing-images/sign08-large.png#lightbox)
9. Bu profil için bilgisayarları seçin: 

    [![İzin verilen bilgisayarlar seçiliyor](sandboxing-images/sign09.png "izin verilen bilgisayarlar seçiliyor")](sandboxing-images/sign09-large.png#lightbox)
10. Profil, bir ad verin: 

    [![Profil bir ad vererek](sandboxing-images/sign10.png "profili bir ad verin")](sandboxing-images/sign10-large.png#lightbox)
11. Tıklayın **Bitti** düğmesi.

> [!IMPORTANT]
> Bazı durumlarda yeni sağlama profili Apple Geliştirici Portalı'ndan doğrudan indirmek ve yüklemek için buna çift tıklayarak gerekebilir. Durdur ve yeni profili erişebilir önce Mac için Visual Studio'yu yeniden başlatmanız gerekebilir.

Ardından, geliştirme makinenizde yeni uygulama kimliği ve profili yüklemek ihtiyacımız var. Şimdi aşağıdakileri yapın:

1. Xcode'ı başlatın ve seçin **tercihleri** gelen **Xcode** menüsü: 

    ![Xcode hesaplarında düzenleme](sandboxing-images/sign11.png "Xcode hesaplarında düzenleme")
2. Tıklayarak **ayrıntıları görüntüle...**  düğmesi: 

    ![Ayrıntıları Görüntüle düğmesine tıklayarak](sandboxing-images/sign12.png "ayrıntılarını görüntüle düğmesi")
3. Tıklayarak **Yenile** (sol alt köşesinde) düğmesini.
4. Tıklayarak **Bitti** düğmesi.

Ardından, yeni uygulama Kimliğini ve sağlama profili Xamarin.Mac Projemizin seçmeliyiz. Şimdi aşağıdakileri yapın:

1. İçinde **çözüm bölmesi**, çift **Info.plist** dosyayı düzenlemek için açın.
2. Emin **paket grubu tanımlayıcısı** yukarıda oluşturduğumuz bizim uygulama kimliği eşleşir (örnek: `com.appracatappra.MacSandbox`): 

    [![Paket tanımlayıcısı düzenleme](sandboxing-images/sign13.png "paket grubu tanımlayıcısı düzenleme")](sandboxing-images/sign13-large.png#lightbox)
3. Ardından, çift **Entitlements.plist** dosya ve olun bizim **iCloud anahtar-değer Store** ve **iCloud kapsayıcıları** yukarıda oluşturduğumuz bizim uygulama kimliği eşleşen (örnek: `com.appracatappra.MacSandbox`): 

    [![Entitlements.plist dosyayı düzenlemeye](sandboxing-images/sign17.png "Entitlements.plist dosyasını düzenleme")](sandboxing-images/sign17-large.png#lightbox)
3. Değişikliklerinizi kaydedin.
4. İçinde **çözüm bölmesi**, proje dosyası düzenleme için onun seçeneklerini açmak için çift tıklayın:  

    ![Editign çözümün Seçenekler](sandboxing-images/sign14.png "Editign çözümün seçenekleri")
5. Seçin **Mac imzalama**, ardından **uygulama paket grubunu imzalamak** ve **Yükleyici paketini imzala**. Altında **sağlama profili**, yukarıda oluşturduğumuz birini seçin: 

    ![Sağlama profili ayarlama](sandboxing-images/sign15.png "sağlama profili ayarlama")
6. Tıklayın **Bitti** düğmesi.

> [!IMPORTANT]
> Çıkıp yeni uygulama Kimliğini ve sağlama, Xcode tarafından yüklenen profil tanımak için Visual Studio almak Mac için yeniden başlatmanız gerekebilir.

#### <a name="troubleshooting-provisioning-issues"></a>Sağlama ile ilgili sorunları giderme

Bu noktada uygulamayı çalıştırın ve her şeyi imzalı ve doğru bir şekilde sağlanmış olduğundan emin olun çalışmanız gerekir. Uygulama yine eskisi gibi çalışır, her şeyi iyi olur. Bir hata olması durumunda, aşağıdakine benzer bir iletişim kutusu alabilirsiniz:

[![Sorun iletişim sağlama örnek](sandboxing-images/sign16.png "sorunu iletişim sağlama örneği")](sandboxing-images/sign16-large.png#lightbox)

Sağlama ve sorunları imzalama en yaygın nedenleri şunlardır:

- Uygulama paket Kimliğini seçili profilinin uygulama kimliği ile eşleşmiyor.
- Geliştirici kimliği seçilen profil Geliştirici kimliği ile eşleşmiyor.
- Test edilen Mac UUID'si seçili profil bir parçası olarak kayıtlı değil.

Bir sorun olması durumunda Apple Developer Portal'a sorunu düzeltin, Xcode profillerinde yenileyin ve Mac için Visual Studio'da temiz bir derleme yapın

### <a name="enable-the-app-sandbox"></a>Uygulama korumalı alanı etkinleştir

Uygulama korumalı alanı, bir onay kutusu, projeler seçeneklerinden birini belirleyerek etkinleştirin. Aşağıdakileri yapın:

1. İçinde **çözüm bölmesi**, çift **Entitlements.plist** dosyayı düzenlemek için açın.
2. Her ikisini de denetlemek **yetkilendirmeleri etkinleştirin** ve **etkinleştirme uygulamasını korumalı alana alma**: 

    [![Yetkilendirmelerini düzenlemek ve korumalı alana alma etkinleştirme](sandboxing-images/sign17.png "yetkilendirmelerini düzenlemek ve etkinleştirme korumalı alana alma")](sandboxing-images/sign17-large.png#lightbox)
3. Değişikliklerinizi kaydedin.

Bu noktada uygulama korumalı alanı etkinleştirdiniz ancak gerekli ağ erişimi için Web görünümü sağlamadınız. Uygulamayı şimdi çalıştırırsanız, boş bir pencere almanız gerekir:

[![Web erişimi engellenme gösteren](sandboxing-images/sample08.png "engellenme web erişim gösteriliyor")](sandboxing-images/sample08-large.png#lightbox)

### <a name="verifying-that-the-app-is-sandboxed"></a>Uygulama korumalı olduğu doğrulanıyor

Davranış engelleme kaynak yanı sıra bir Xamarin.Mac uygulamasını korumalı başarıyla uygulanmış olup olmadığını bildirmek için üç ana yolu vardır:

1. Bulucu'da içeriğini denetlemek `~/Library/Containers/` klasörü - uygulama, korumalı, ise olacaktır, uygulamanın paket tanımlayıcısı gibi adlı bir klasör (örnek: `com.appracatappra.MacSandbox`): 

    [![Uygulamanın paket açma](sandboxing-images/sample09.png "uygulamanın paketini açma")](sandboxing-images/sample09-large.png#lightbox)
2. Sistem, uygulama etkinliği İzleyicisi'nde korumalı olarak görür:
    - Etkinlik izleme başlatma (altında `/Applications/Utilities`). 
    - Seçin **görünümü** > **sütunları** olduğundan emin olun **korumalı alan** menü öğesi denetlenir.
    - Korumalı alan sütunu okuduğu olun `Yes` uygulamanız için: 

    [![Etkinlik izleyicisinin uygulamada denetimi](sandboxing-images/sample10.png "etkinlik izleyicisinin uygulamada denetleniyor")](sandboxing-images/sample10-large.png#lightbox)
3. İkili uygulama korumalı olup olmadığını denetleyin:
    - Terminal uygulamasını başlatın.
    - Uygulamaları'na gidin `bin` dizin.
    - Bu komutu yürütün: `codesign -dvvv --entitlements :- executable_path` (burada `executable_path` uygulamanızı yolu): 

    [![Komut satırında uygulama denetimi](sandboxing-images/sample11.png "komut satırında uygulama denetleniyor")](sandboxing-images/sample11-large.png#lightbox)

### <a name="debugging-a-sandboxed-app"></a>Korumalı bir uygulama hata ayıklama

Hata ayıklayıcı Xamarin.Mac uygulamaları, korumalı alana alma, etkinleştirdiğinizde, varsayılan olarak, uygulamaya bağlanamıyor olduğu anlamına gelir, TCP üzerinden bağlanır etkin uygun izinler olmadan uygulamayı çalıştırmayı denerseniz, hata alırsınız. Bu nedenle *"bağlanılamıyor hata ayıklayıcı"*. 

[![Gerekli ayarları](sandboxing-images/debug01.png "gerekli seçeneklerini ayarlama")](sandboxing-images/debug01-large.png#lightbox)

**İzin giden ağ bağlantılarını (istemci)** hata ayıklayıcı için gerekli bir izindir, bunu etkinleştirilmesi normalde hata ayıklama izin verir. Bu olmadan ayıklanamıyor beri güncelleştirdik `CompileEntitlements` için hedef `msbuild` yalnızca hata ayıklama için korumalı alan olan herhangi bir uygulama yapıları için bu izin için yetkilendirme otomatik olarak eklemek için. Yayın derlemeleri üzerinde değişiklik yapılmadan yetkilendirmeler dosyasında belirtilen yetkilendirmeler kullanmanız gerekir.

### <a name="resolving-an-app-sandbox-violation"></a>Uygulama korumalı alanı ihlali çözümleme

Uygulama, bir kaynağın erişmeye korumalı bir Xamarin.Mac açıkça izin uygulama korumalı alanı ihlali oluşur. Örneğin, bizim Web artık Apple Web görüntülenecek işaretleyebilmesine görüntüleyin.

Uygulama korumalı alanı ihlallerinin en yaygın kaynak Mac için Visual Studio'da belirtilen yetkilendirme ayarları, uygulamanın gereksinimleriyle eşleşmiyor oluşur. Yeniden Bizim örneğimizde, eksik bir ağ bağlantısı çalışmasını Web görünümünü korumak yetkilendirmeler yedekleyin.

#### <a name="discovering-app-sandbox-violations"></a>Uygulama korumalı alanı ihlalleri bulma

Uygulama korumalı alanı ihlali Xamarin.Mac uygulamanızda gerçekleşen şüpheleniyorsanız, sorun bulunacak en hızlı yolu kullanmaktır **konsol** uygulama.

Aşağıdakileri yapın:

1. Söz konusu uygulamayı derleyin ve Mac için Visual Studio'dan çalıştırma
2. Açık **konsol** uygulama (gelen `/Applications/Utilties/`).
3. Seçin **tüm iletileri** Kenar çubuğunda ve girin `sandbox` ara: 

    [![Konsolunda bir korumalı alana alma sorunu örneği](sandboxing-images/resolve01.png "konsolunda bir korumalı alana alma sorunu örneği")](sandboxing-images/resolve01-large.png#lightbox)

Bizim örnek bir uygulama için yukarıdaki Kernal engelliyor görebilirsiniz `network-outbound` uygulama Biz bu hakkı istediniz değil olduğundan sanal nedeniyle trafik.

#### <a name="fixing-app-sandbox-violations-with-entitlements"></a>Uygulama korumalı alanı yetkilendirme ihlalleri düzeltme

Uygulama korumalı alana alma ihlalleri bulma gördük, nasıl bunlar bizim uygulamanın yetkilendirmeler ayarlayarak çözülebileceğini görelim.

Aşağıdakileri yapın:

1. İçinde **çözüm bölmesi**, çift **Entitlements.plist** dosyayı düzenlemek için açın.
2. Altında **yetkilendirmeler** bölümünde onay **izin giden ağ bağlantılarını (istemci)** onay kutusu: 

    [![Yetkilendirmelerini düzenlemek](sandboxing-images/sign17.png "yetkilendirmelerini düzenlemek")](sandboxing-images/sign17-large.png#lightbox)
3. Değişiklikleri uygulamaya kaydedin.

Biz Yukarıdaki örnek uygulamamız için işlemi, ardından derleyin ve çalıştırın, web içeriği artık beklendiği gibi görüntülenir.

## <a name="the-app-sandbox-in-depth"></a>Kapsamlı uygulama korumalı alanı

Uygulama korumalı alanı tarafından sağlanan erişim denetimi mekanizmaları birkaç ve kolay anlaşılır. Ancak, uygulama korumalı alanı tarafından her uygulamanın benimsenmesi benzersiz ve uygulama gereksinimlerine bağlı olarak yoludur.

Kötü amaçlı kod tarafından kötüye Xamarin.Mac uygulamanızı korumak için en iyi çalışmalarınızı göz önünde bulundurulduğunda, olması yalnızca tek bir güvenlik açığı her iki uygulamada (veya bir kitaplık veya çerçeveleri birini kullanır) uygulamanın etkileşim denetlemek amacıyla sistemi.

Uygulama korumalı alanı, sistemin hedeflenen etkileşim uygulamanın belirtmenize olanak tanıyarak devralma önlemek (veya zarara neden olabilir sınırlandırmak için) tasarlanmıştır. Sistem, yalnızca uygulamanızın ihtiyaç duyduğu işin yapılması için kaynak ve hiçbir şey daha fazla erişim izni verir.

Uygulama korumalı alanı tasarlarken, iki katına bir senaryo için tasarlarken. Uygulamayı kötü amaçlı kod tarafından tehlikeye olur, dosyalara ve kaynaklara uygulama korumalı alanı içinde yalnızca erişimle sınırlı olur.

### <a name="entitlements-and-system-resource-access"></a>Yetkilendirmeler ve sistem kaynak erişimi

Yukarıda gördüğümüz gibi korumalı ayarlanmamış bir Xamarin.Mac uygulamasını tam hakları ve uygulamayı çalıştıran kullanıcının erişim izni verilir. Kötü amaçlı kod tarafından ihlal edilmesi durumunda, korumalı olmayan bir uygulama olarak tehlikeli davranışı için bir aracı zarar yapmak için olası arasında değişen bir geniş ile davranabilir.

Uygulama korumalı alanı etkinleştirerek, tüm bir en az ayrıcalık kümesi, hangi, kaldırma sonra yalnızca gereksinim olarak Xamarin.Mac uygulamanızın yetkilendirmeler kullanarak yeniden etkinleştirin. 

Düzenleyerek, uygulamanızın uygulama korumalı alanı kaynakları değiştirmek, **Entitlements.plist** dosya denetleniyor ve düzenleyicileri açılır liste kutularında gerekli olan hakların seçme:

[![Yetkilendirmelerini düzenlemek](sandboxing-images/sign17.png "yetkilendirmelerini düzenlemek")](sandboxing-images/sign17-large.png#lightbox)

### <a name="container-directories-and-file-system-access"></a>Kapsayıcı dizin ve dosya sistemi erişimi

Xamarin.Mac uygulamanızı uygulama korumalı alanı devralır, aşağıdaki konumlara erişim vardır:

- **Uygulama kapsayıcı dizini** -ilk çalıştırmada, özel bir işletim sistemini oluşturur _kapsayıcı dizini_ tüm kaynaklarını Git burada yalnızca erişebildiğinizi. Uygulama bu dizinde okuma/yazma erişimi gerekir.
- **Uygulama grubu kapsayıcı dizinleri** -uygulama bir veya daha fazla erişim verilebilir _Grup kapsayıcılarına_ aynı grupta uygulamalar arasında paylaşılan.
- **Kullanıcı tarafından belirtilen dosyalarını** -uygulamanızı otomatik olarak açıkça açık veya sürüklediğiniz ve uygulama kullanıcı tarafından iptal edilen dosyalara erişim elde eder.
- **İlgili öğeleri** -uygun yetkilendirmelerle, aynı ada ancak farklı bir uzantıya sahip bir dosyaya, erişim uygulamanız olabilir. Örneğin, her ikisi de olarak kaydedilmiş bir belge bir `.txt` dosyası ve bir `.pdf`.
- **Geçici dizin, komut satırı aracı dizinleri ve dünya okunabilir belirli konumlara** -uygulamanızın sistem tarafından belirtilen iyi tanımlanmış diğer konumlarda dosyaları için çeşitli düzeylerde erişimi vardır.

#### <a name="the-app-container-directory"></a>Uygulama kapsayıcı dizini

Xamarin.Mac uygulamanın uygulama kapsayıcı dizini aşağıdaki özelliklere sahiptir:

- Kullanıcının giriş dizini gizli bir konumda içinde (genellikle `~Library/Containers`) ile erişilebilir `NSHomeDirectory` uygulamanızda işlevi (aşağıya bakın). Giriş dizini içinde olduğundan, her kullanıcı için uygulama kendi kapsayıcı alırsınız.
- Uygulama, kapsayıcı dizini ve tüm alt dizinler ve dosyalar için okuma/yazma erişimi varmazlar.
- Uygulamanın kapsayıcı göre çoğu macOS'ın yol bulma API. Örneğin, kapsayıcı kendi olur **Kitaplığı** (üzerinden erişilen `NSLibraryDirectory`), **uygulama desteği** ve **tercihleri** alt dizinleri.
- macOS oluşturur ve arasındaki bağlantıyı zorlar ve uygulama ve kod imzalama aracılığıyla kapsayıcısı. Uygulamayı kullanarak sızmasını başka bir uygulama çalıştığında olsa bile, **paket grubu tanımlayıcısı**, kod imzalama nedeniyle kapsayıcısına erişmek mümkün olmayacaktır.
- Kapsayıcı için oluşturulan kullanıcı dosyaları değil. Bunun yerine, veritabanları, önbellekler veya belirli diğer veri türleri gibi uygulamanızın kullandığı dosyalar içindir.
- İçin _ayakkabı_ (örneğin, Apple'nın fotoğraf uygulaması) uygulama türleri, kullanıcının içeriği kapsayıcısına gider.

> [!IMPORTANT]
> Ne yazık ki, Xamarin.Mac % 100 API kapsamı henüz (aksine Xamarin.iOS), sonuç olarak yok `NSHomeDirectory` API olmayan eşlenmiş Xamarin.Mac geçerli sürümde.

Geçici bir çözüm, aşağıdaki kodu kullanabilirsiniz:

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

Başlangıç Mac MacOS 10.7.5 (ve büyük), bir uygulama kullanabilirsiniz `com.apple.security.application-groups` grubundaki tüm uygulamalar için ortak bir paylaşılan kapsayıcısına erişmek için yetkilendirme. Bu paylaşılan kapsayıcı veritabanı ya da (örneğin, önbellekler) destek dosyalarının diğer türleri gibi kullanıcı olmayan yönelik içerikler için kullanabilirsiniz.

Grup kapsayıcıları (bir grubun parçası olmaları durumunda) otomatik olarak her uygulamanın sanal bir kapsayıcıya eklenir ve depolandığı yer `~/Library/Group Containers/<application-group-id>`. Grup Kimliği _gerekir_ geliştirme ekibi Kimliğinizi ve nokta ile başlamak, örneğin:

```plist
<team-id>.com.company.<group-name>
```

Apple'nın daha fazla bilgi için bkz [uygulama grubu bir uygulamaya ekleme](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19) içinde [yetkilendirme anahtar başvurusu](https://developer.apple.com/library/prerelease/mac/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/AboutEntitlements.html#//apple_ref/doc/uid/TP40011195).

#### <a name="powerbox-and-file-system-access-outside-of-the-app-container"></a>Uygulama kapsayıcısı dışında Powerbox ve dosya sistemi erişimi

Korumalı bir Xamarin.Mac uygulama, kapsayıcısı dışındaki dosya sistemi konumları aşağıdaki yöntemlerle erişebilirsiniz:

- Kullanıcı (aracılığıyla, açık ve Kaydet iletişim kutuları veya diğer yöntemleri sürükle ve bırak gibi) belirli yönü.
- Yetkilendirmeler için belirli bir dosya sistemi konumları kullanarak (gibi `/bin` veya `/usr/lib`).
* Dosya sistemi konumundan (paylaşım gibi) okunabilir world olan belirli dizinlerdeki olduğunda.

_Powerbox_ macOS, korumalı Xamarin.Mac uygulamanızın dosyaya erişim haklarına genişletmek için kullanıcı ile etkileşime giren güvenlik teknolojisidir. Powerbox hiçbir API vardır, ancak uygulama çağırdığında şeffaf şekilde etkinleştirilir bir `NSOpenPanel` veya `NSSavePanel`. Xamarin.Mac uygulamanız için ayarladığınız yetkilendirmeler aracılığıyla Powerbox erişim etkin.

Korumalı bir uygulama açık görüntüler veya Kaydet iletişim kutusu penceresini Powerbox (ve AppKit değil) tarafından sunulan ve bu nedenle herhangi bir dosya veya kullanıcının erişimi olan bir dizine erişebilir.

Bir kullanıcı bir dosya veya dizin açma seçer veya Kaydet iletişim kutusu (veya uygulamanın simgesine ya da sürüklediğinde olduğunda) Powerbox uygulamanın Korumalı alan için ilişkili bir yol ekler.

Ayrıca, sistem otomatik olarak aşağıdaki korumalı bir uygulamaya izin verir:

- Bir sistemi bağlantısı yöntemi giriş.
- Kullanıcı tarafından seçilen Hizmetleri çağıran **Hizmetleri** menü (Hizmetleri olarak işaretlenmiş için yalnızca _korumalı alan uygulamalar için güvenli_ hizmet sağlayıcısı tarafından).
- Açık dosyalar seçin kullanıcı tarafından **açık son** menüsü.
- Diğer uygulamalar arasında kopyala ve Yapıştır kullanın.
- Dosyaları aşağıdaki dünya okunabilir konumlardan okuyun:
    - `/bin`
    - `/sbin`
    - `/usr/bin`
    - `/usr/lib`
    - `/usr/sbin`
    - `/usr/share`
    - `/System`
- Okuma ve tarafından oluşturulan dizinlerde dosyaları yazma `NSTemporaryDirectory`.

(Uygulama sonlandırıldığında dosya hala açık değilse), uygulama sonlanana kadar varsayılan olarak açık veya korumalı bir Xamarin.Mac uygulama tarafından kaydedilen dosyaları erişilebilir kalır. Açık dosyalar otomatik olarak macOS sürdürme özelliği üzerinden uygulamanın sanal uygulamayı bir sonraki başlatılmasında geri yüklenir.

Bir Xamarin.Mac uygulama kapsayıcısı dışında bulunan dosyalar için kalıcılığı sağlamak için güvenlik kapsamı yer işaretlerini (aşağıya bakın) kullanın.

#### <a name="related-items"></a>İlgili öğeler

Uygulama korumalı alanı, ancak farklı uzantıları aynı dosya olan ilgili öğelere erişmek bir uygulama sağlar. Özellik iki bölümden oluşur: a) uygulamasının ilgili uzantılarının listesi `Info.plst` b), bu dosyalarda ne uygulama gerçekleştirecekseniz korumalı alan bildirmek için kodu dosyası.

Bu yarayacak yerde iki senaryo vardır:

1. Uygulama kaydedemezsiniz (yeni uzantılı) dosyasının farklı bir sürümü gerekir. Örneğin, dışa aktarma bir `.txt` dosyasını bir `.pdf` dosya. Bu durumu yönetmek için kullanmanız gerekir bir `NSFileCoordinator` dosyaya erişmek için. Sizi ararız `WillMove(fromURL, toURL)` yöntemi ilk olarak, yeni uzantının dosyayı taşıyın ve sonra çağrı `ItemMoved(fromURL, toURL)`.
2. Uygulamanın bir uzantıya sahip ana dosya ve çeşitli destekleyici dosyalar farklı uzantılarıyla açmak gerekir. Örneğin, bir filmi ve alt yazı dosyasını. Kullanım bir `NSFilePresenter` ikincil dosya erişim elde etmek için. Ana dosyasına sağlamak `PrimaryPresentedItemURL` özelliği ve ikincil dosyasını `PresentedItemURL` özelliği. Ana dosya açıldığında, çağrı `AddFilePresenter` yöntemi `NSFileCoordinator` ikincil dosyayı kaydetmek için sınıf.

Her iki senaryolarında, uygulama kullanıcının **Info.plist** dosya uygulamayı açmak için bir belge türü belirtmesi gerekir. Herhangi bir dosya türü için ekleme `NSIsRelatedItemType` (değeriyle `YES`) kendi girişi `CFBundleDocumentTypes` dizisi.

#### <a name="open-and-save-dialog-behavior-with-sandboxed-apps"></a>Açın ve korumalı uygulamalarla iletişim davranış Kaydet

Aşağıdaki sınırlar yerleştirilir `NSOpenPanel` ve `NSSavePanel` bunları korumalı bir Xamarin.Mac uygulamasını çağırırken:

- Program aracılığıyla çağrılamıyor **Tamam** düğmesi.
- Bir kullanıcının seçiminde program aracılığıyla değiştirilemez bir `NSOpenSavePanelDelegate`.

Ayrıca, yerinde aşağıdaki devralma değişiklikler şunlardır:

- **Korumalı olmayan uygulama** - `NSOpenPanel` `NSSavePanel``NSPanel``NSWindow``NSResponder``NSObject``NSOpenPanel``NSSavePanel``NSObject``NSOpenPanel``NSSavePanel`

### <a name="security-scoped-bookmarks-and-persistent-resource-access"></a>Güvenlik kapsamı yer işaretleri ve kalıcı kaynak erişimi

Yukarıda belirtildiği gibi bir dosya veya kaynak (PowerBox tarafından sağlanan gibi) doğrudan kullanıcı etkileşimi yoluyla, kapsayıcısı dışındaki bir korumalı bir Xamarin.Mac uygulamasını erişebilirsiniz. Ancak, bu kaynaklara erişimini otomatik olarak uygulama piyasaya sürülmesi veya sistem yeniden devam etmez.

Kullanarak _Security-Scoped yer işaretleri_, korumalı bir Xamarin.Mac uygulamasını kullanıcının amacını korumak ve verilen bir uygulama sonra kaynakları yeniden erişim sağlamak.

#### <a name="security-scoped-bookmark-types"></a>Güvenlik kapsamı yer işareti türleri

Kalıcı kaynak erişimi Security-Scoped yer işaretleri ile çalışırken, iki sistine kullanım örnekleri vardır:

- **Bir App-Scoped yer işareti, bir kullanıcı tarafından belirtilen dosya veya klasör sürekli erişim sağlar.** 

    Örneğin, bir harici belge düzenlemek için açmak için kullanmak istediğiniz korumalı Xamarin.Mac uygulama izin veriyorsa (kullanarak bir `NSOpenPanel`), böylece aynı dosya ileride erişebilir App-Scoped yer uygulama oluşturabilirsiniz.
- **Document-Scoped yer işareti, bir alt dosya bir özel belge kalıcı erişim sağlar.** 

Örneğin, bir proje dosyası oluşturan bir video düzenleme uygulaması ayrı görüntüleri, video klipleri ve daha sonra tek bir filmi birleştirilir ses dosyalarını erişebilir.

Ne zaman kullanıcı içe aktaran bir kaynak dosyası projeye (aracılığıyla bir `NSOpenPanel`), uygulama projesinde depolanan öğenin Document-Scoped yer işareti oluşturur ve böylece dosyayı her zaman uygulamaya erişilebilir.

Document-Scoped yer işareti, yer işareti verileri ve belge açabileceği herhangi bir uygulama tarafından çözülebilir. Bu, kullanıcının başka bir kullanıcı ve tüm yer işaretlerini onlar için de kullanılabilir olan proje dosyalarını göndermek taşınabilirlik destekler.

> [!IMPORTANT]
> Document-Scoped yer işareti olabilir _yalnızca_ noktası tek bir dosya ve klasör değil ve bu dosya, sistem tarafından kullanılan bir konumda olamaz (örneğin `/private` veya `/Library`).

#### <a name="using-security-scoped-bookmarks"></a>Güvenlik kapsamı yer işaretlerini kullanma

Security-Scoped yer işareti ya da türünü kullanarak, aşağıdaki adımları gerektirir:

1. **Uygun yetkilendirmeler Security-Scoped yer işaretlerini kullanmak için gereken Xamarin.Mac ayarlanacağı** -For App-Scoped yer işaretleri ayarlama `com.apple.security.files.bookmarks.app-scope` yetkilendirme anahtarına `true`. Document-Scoped yer işaretleri için ayarlanmış `com.apple.security.files.bookmarks.document-scope` yetkilendirme anahtarına `true`.
2. **Security-Scoped yer işareti oluşturma** -herhangi bir dosya veya kullanıcının erişim ayarının klasörü için bunu (aracılığıyla `NSOpenPanel` örneğin), uygulamayı kalıcı erişim gerekir. Kullanım `public virtual NSData CreateBookmarkData (NSUrlBookmarkCreationOptions options, string[] resourceValues, NSUrl relativeUrl, out NSError error)` yöntemi `NSUrl` yer işareti oluşturmak için sınıf.
3. **Security-Scoped yer işareti çözmek** - uygulama kaynak yeniden (örneğin, yeniden başlatıldıktan) erişmesi gerektiğinde yer işareti için bir güvenlik kapsamı URL çözümlemek gerekir. Kullanım `public static NSUrl FromBookmarkData (NSData data, NSUrlBookmarkResolutionOptions options, NSUrl relativeToUrl, out bool isStale, out NSError error)` yöntemi `NSUrl` yer işareti çözmek için sınıf.
4. **Açıkça Security-Scoped URL'den dosyaya erişmek istediğiniz sistem bilgisini** - bu adımın yukarıdaki Security-Scoped URL'sini aldıktan hemen sonra yapılması gerektiğini veya, daha sonra istediğinizde kaynak atandıktan sonra erişebilmek Bunu erişiminizi relinquished. Çağrı `StartAccessingSecurityScopedResource ()` yöntemi `NSUrl` Security-Scoped URL erişmeye başlamak için sınıf.
5. **İşiniz sisteme açıkça bildirmek Security-Scoped URL'den dosyaya erişimde** -olabildiğince çabuk, (örneğin, kullanıcının bu kapanıyorsa) uygulamanın artık dosyaya erişim ihtiyacı olduğunda sistem bilgilendirmek. Çağrı `StopAccessingSecurityScopedResource ()` yöntemi `NSUrl` Security-Scoped URL'ye erişilirken durdurmak için sınıf.

Bir kaynağa erişim teknisyene sonra yeniden erişim yeniden oluşturmak için 4. adıma dönmek gerekir. Xamarin.Mac uygulamasını yeniden başlatılırsa, 3. adıma dönün ve yer işareti yeniden çözümleyebilir.

> [!IMPORTANT]
> Hata Security-Scoped URL kaynaklarına erişimini serbest bırakmak için çekirdek kaynaklara yönelik bir Xamarin.Mac uygulamasını neden olur. Sonuç olarak, uygulama artık yeniden başlatılana kadar kapsayıcıya bir dosya sistemi konumları eklemek mümkün olacaktır.

### <a name="the-app-sandbox-and-code-signing"></a>Uygulama korumalı alanı ve kod imzalama

Uygulama korumalı alanı etkinleştirmek ve (yetkilendirme) aracılığıyla Xamarin.Mac uygulamanız için belirli gereksinimler etkinleştirme sonra oturum proje etkili olması korumalı alana alma için kod gerekir. Kod uygulamasını korumalı alana alma için gerekli yetkilendirmeler uygulamanın imza bağlantılı olduğundan imzalama gerçekleştirmeniz gerekir. 

macOS uygulamanın kapsayıcı ve başka bir uygulama, uygulama paketi kimliği kimlik sahtekarlığı bile bu kapsayıcı erişebilir, böylece kod imzası arasında bir bağlantı uygular. Bu mekanizma gibi çalışır:

1. Uygulama kapsayıcısı sistem oluşturduğu zaman, bu ayarlar bir _erişim denetim listesi_ (ACL), kapsayıcı üzerindeki. Listedeki ilk erişim denetim girişi uygulamanın içeren _atanan gereksinim_ (DR), uygulamanın nasıl gelecekteki sürümlerinde açıklayan tanımasını (yükseltildiğinde).
2. Bir uygulamayı aynı uygulamanın paket Kimliğini ile başlar, her zaman uygulamanın kod imzası belirtilen kapsayıcının ACL girişleri birinde belirtilen gereksinimleri eşleştiğini sistem denetler. Sistem bir eşleşme bulamazsa, uygulamayı başlatmasını engeller.

İmzalama works aşağıdaki şekilde kod:

1. Xamarin.Mac proje oluşturmadan önce Apple Geliştirici Portalı'ndan bir geliştirme sertifikası, bir dağıtım sertifikası ve Geliştirici kimlik sertifikası alın.
2. Xamarin.Mac uygulamasını Mac App Store dağıtır, bir Apple kod imza ile imzalanmış.

Test ve hata ayıklama, (hangi uygulama kapsayıcısı oluşturmak için kullanılacak) oturumunuz Xamarin.Mac uygulamanın bir sürümünü kullanırsınız. Daha sonra test veya Apple App Store ' sürümünü yüklemek istiyorsanız, bir Apple imza ile imzalandığından ve bu yana (aynı kod imzası özgün uygulama kapsayıcısı yok) başlatmak başarısız olur. Bu durumda, aşağıdakine benzer bir kilitlenme raporu alırsınız:

```csharp
Exception Type:  EXC_BAD_INSTRUCTION (SIGILL)
```

Bunu düzeltmek için uygulamayı Apple imzalı sürümüne işaret edecek şekilde ACL girişi ayarlamak gerekir.

Oluşturma ve sağlama profilleri korumalı alana alma için gerekli yükleme hakkında daha fazla bilgi için lütfen bkz [imzalama ve uygulama sağlama](#Signing_and_Provisioning_the_App) yukarıdaki bölümde.

#### <a name="adjusting-the-acl-entry"></a>ACL girişi ayarlama

Xamarin.Mac uygulamasını çalıştırmak için Apple imzalı sürümünü izin vermek için aşağıdakileri yapın:

1. Terminal uygulamasını açın (içinde `/Applications/Utilities`).
2. Xamarin.Mac uygulamasını Apple imzalı sürümünü bir Bulucu penceresi açın.
3. Tür `asctl container acl add -file ` Terminal penceresinde.
4. Xamarin.Mac uygulamanın simgesi Bulucu penceresinden sürükleyip Terminal penceresinde bırakın.
5. Terminalde komut dosyasının tam yolu eklenir.
6. Tuşuna **Enter** komutu yürütülemedi.

Kapsayıcının ACL şimdi iki sürümü de Xamarin.Mac uygulamasını belirlenen Kod gereksinimlerini içerir ve macOS şimdi çalıştırmak iki sürümden izin verir.

#### <a name="display-a-list-of-acl-code-requirements"></a>ACL kodu gereksinimleri listesini görüntüleme

Aşağıdakileri yaparak bir kapsayıcının ACL'de kodu gereksinimleri listesini görüntüleyebilirsiniz:

1. Terminal uygulamasını açın (içinde `/Applications/Utilities`).
2. Tür `asctl container acl list -bundle <container-name>`.
3. Tuşuna **Enter** komutu yürütülemedi.

`<container-name>` Genellikle Xamarin.Mac uygulama için paket grubu tanımlayıcısı olur.

## <a name="designing-a-xamarinmac-app-for-the-app-sandbox"></a>Uygulama korumalı alanı bir Xamarin.Mac uygulamasını tasarlama

Uygulama korumalı alanı bir Xamarin.Mac uygulamasını tasarlarken, izlenmesi gereken ortak bir iş akışı yok. Bu, korumalı alana alma, bir uygulamada uygulama ayrıntıları verilen uygulamanın işlevselliğini için benzersiz olacak belirtti.

### <a name="six-steps-for-adopting-the-app-sandbox"></a>Uygulama korumalı alanı'nu benimseme altı adımı

Uygulama korumalı alanı, genellikle bir Xamarin.Mac uygulamasını tasarlarken aşağıdaki adımlardan oluşur:

1. Uygulama korumalı alana alma için uygun olup olmadığını belirler.
2. Bir geliştirme ve dağıtım stratejisine tasarlayın.
3. API uyumsuzlukları çözümleyin.
4. Gerekli uygulama korumalı alanı Yetkilendirmelerini Xamarin.Mac projesi için geçerlidir.
5. Ayrıcalık ayrımı XPC kullanarak ekleyin.
6. Bir geçiş stratejisi uygulayın.

> [!IMPORTANT]
> Yalnızca, uygulama paketi, ancak dahil edilen her yardımcı ayrıca ana yürütülebilir dosya korumalı alan gerekir uygulama veya bu paketteki aracı. Bu Mac App Store ' dağıtılmış herhangi bir uygulama için gereklidir ve mümkün olduğunda, uygulama dağıtımı herhangi diğer bir form için yapılması gerekir.

Xamarin.Mac uygulamanın paketteki tüm yürütülebilir ikili dosyaların bir listesi için terminalde aşağıdaki komutu yazın:

```bash
find -H [Your-App-Bundle].app -print0 | xargs -0 file | grep "Mach-O .*executable"
```

Burada `[Your-App-Bundle]` ad ve uygulamanın paket yolu.

### <a name="determine-whether-a-xamarinmac-app-is-suitable-for-sandboxing"></a>Bir Xamarin.Mac uygulamasını korumalı alana alma için uygun olup olmadığını belirleme

Xamarin.Mac uygulamaların çoğu uygulama korumalı alanı ile tamamen uyumludur ve bu nedenle, korumalı alana alma için uygundur. Uygulama, uygulama korumalı alanı izin vermeyen bir davranış gerektiriyorsa, alternatif bir yaklaşım düşünmelisiniz.

Uygulamanız aşağıdaki davranışlardan birini gerektiriyorsa, bu uygulama korumalı alanı ile uyumlu değil:

- **Yetkilendirme Hizmetleri** -ile uygulama korumalı alanı, sizin çalışamıyor açıklanan işlevlerle [yetkilendirme hizmetleri C başvurusu](https://developer.apple.com/library/prerelease/mac/documentation/Security/Reference/authorization_ref/index.html#//apple_ref/doc/uid/TP30000826).
- **Erişilebilirlik API'leri** -ekran okuyucular gibi korumalı alan yardımcı uygulamaları veya diğer uygulamaları denetleyen uygulamaları olamaz.
- **Rastgele uygulamaları için Apple olayları gönderme** -uygulama bilinmeyen, rastgele bir uygulama için Apple olayları gönderme gerektiriyorsa, korumalı olamaz. Çağrılan uygulamaların bilinen bir listesini, uygulama hala korumalı olabilir ve yetkilendirmeler adlı uygulama listesini eklemeniz gerekir.
- **Kullanıcı bilgileri sözlükleri diğer görevlere dağıtılmış bildirimleri gönderme** -ile uygulama korumalı alanı, içeremez bir `userInfo` sözlük için aktarırken bir `NSDistributedNotificationCenter` diğer görevleri Mesajlaşma için nesne.
- **Çekirdek uzantıları yükleme** -çekirdek uzantılarının yüklenmesini uygulama korumalı alanı tarafından kullanılamaz.
- **Kullanıcı Giriş açık ve Kaydet iletişim kutuları benzetimi** - program aracılığıyla açma düzenleme veya benzetimini yapmak veya alter kullanıcı girişi için iletişim kutuları uygulama korumalı alanı tarafından kullanılamaz.
- **Erişme veya diğer uygulamalarda ayarı tercihleri** -diğer uygulamalar ayarlarını düzenleme uygulama korumalı alanı tarafından kullanılamaz.
- **Ağ ayarlarını yapılandırma** -ağ ayarlarını düzenleme uygulama korumalı alanı tarafından kullanılamaz.
- **Diğer uygulamalara sonlandırma** -uygulama korumalı alanı yasaklar kullanarak `NSRunningApplication` diğer uygulamaları sonlandırmak için.

### <a name="resolving-api-incompatibilities"></a>API uyumsuzluklar çözümleme

Uygulama korumalı alanı bir Xamarin.Mac uygulamasını tasarlarken, bazı macOS API kullanımı ile uyumsuzlukları karşılaşabilirsiniz.

Bazı sık karşılaşılan sorunlar ve bunları çözmek için şeyler şunlardır:

- **Açma, kaydetme ve izleme belgeleri** - belgeleri dışındaki herhangi bir teknoloji kullanarak yönetiyorsanız `NSDocument`, kendisine uygulama korumalı alanı yönelik yerleşik destek nedeniyle geçmelisiniz. `NSDocument` otomatik olarak PowerBox ile çalışır ve kullanıcı bunları Finder'da taşınırsa, belgeler, korumalı alan içinde tutmak için destek sağlar.
- **Dosya sistem kaynaklarına erişimi korumak** - uygulama, kapsayıcısı dışındaki kaynaklar için kalıcı erişim bağlıdır Xamarin.Mac kullanırsanız yer işaretleri güvenlik kapsamına erişimi sürdürmek için.
- **Uygulama için bir oturum açma öğesi oluşturma** -ile uygulama korumalı alanı, öğeyi kullanarak bir oturum açma oluşturamazsınız `LSSharedFileList` ya da kullanarak başlatma hizmetlerinin durumunu işleyebileceğiniz `LSRegisterURL`. Kullanım `SMLoginItemSetEnabled` elma içinde anlatıldığı gibi işlev [Hizmet Yönetimi çerçevesi kullanarak oturum açma öğeleri ekleme](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingLoginItems.html#//apple_ref/doc/uid/10000172i-SW5-SW1) belgeleri.
- **Kullanıcı verilerine erişme** - POSIX işlevleri gibi kullanıyorsanız `getpwuid` kullanıcının giriş dizininin dizin hizmetleri verilerini almalarına için Cocoa veya çekirdek Foundation semboller gibi kullanmayı düşünün `NSHomeDirectory`.
- **Tercihleri, diğer uygulamalara** - uygulama korumalı alanı yolu-API'leri uygulamanın kapsayıcıya bulma tercihleri gerçekleştiğinde, kapsayıcı içindeki değiştirme ve başka bir uygulama tercihlerinde izin erişme yönlendirir. 
- **HTML5 videodur Web görünümleri kullanarak** -Xamarin.Mac uygulamasını katıştırılmış HTML5 videoları oynatmak için WebKit kullanıyorsa, uygulamayı AV Foundation altyapısına karşı de bağlamanız gerekir. Uygulama korumalı alanı, aksi takdirde bu videoları play'den CoreMedia engeller.

### <a name="applying-required-app-sandbox-entitlements"></a>Gerekli uygulama korumalı alanı yetkilendirmeler uygulanıyor

İçin kontrol edin ve uygulama korumalı alanı içinde çalıştırmak istediğiniz herhangi bir Xamarin.Mac uygulama Yetkilendirmelerini düzenlemek ihtiyacınız olacak **etkinleştirme uygulamasını korumalı alana alma** onay kutusu.

Uygulamanın işlevselliğini bağlı olarak, işletim sistemi özellikleri ya da kaynaklara erişimi diğer yetkilendirmeleri etkinleştirin gerekebilir. Bu nedenle, uygulamanızı çalıştırmak için gereken en az için istek yetkilendirmeler küçülttüğünüzde en iyi uygulama korumalı alana alma works yalnızca rastgele yetkilendirmeleri etkinleştirin.

Bir Xamarin.Mac uygulamasını gerektirir hangi yetkilendirmeler belirlemek için aşağıdakileri yapın:

1. Uygulama korumalı alanı etkinleştirmek ve Xamarin.Mac uygulamasını çalıştırın.
2. Uygulama özellikleri çalıştırın.
3. Konsol uygulamasını açın (kullanılabilir `/Applications/Utilities`) ve Ara `sandboxd` ihlallerini **tüm iletileri** günlük.
4. Her `sandboxd` ihlali, diğer dosya sistemi konumları yerine uygulama kapsayıcısı kullanarak bu sorunu çözmek veya işletim sistemi özellikleri kısıtlı erişim sağlamak için uygulama korumalı alanı Yetkilendirmelerini uygulayın.
5. Yeniden çalıştırın ve tüm Xamarin.Mac uygulama özelliklerini yeniden test edin.
6. Yineleme kadar tüm `sandboxd` ihlalleri çözümlenen olmuştur.

### <a name="add-privilege-separation-using-xpc"></a>Ayrıcalık ayrımı XPC kullanarak Ekle

Bir Xamarin.Mac uygulamasını korumalı uygulama geliştirirken, uygulamanın davranışlarını ayrıcalıklara ve erişim şartlarında arayın, ardından kendi XPC hizmetlerini yüksek riskli işlemlere ayırarak göz önünde bulundurun.

Daha fazla bilgi için bkz. Apple'nın [XPC hizmetleri oluşturma](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/CreatingXPCServices.html#//apple_ref/doc/uid/10000172i-SW6) ve [Daemon'ları ve Hizmetleri Programlama Kılavuzu](https://developer.apple.com/library/prerelease/mac/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i).

### <a name="implement-a-migration-strategy"></a>Bir geçiş stratejisi uygulayın

Yeni bir korumalı, önceden korumalı olmayan bir Xamarin.Mac uygulamasını sürümü yayınlıyorsanız geçerli kullanıcıların yumuşak bir yükseltme yolu olduğundan emin olmak gerekir. 

 Apple'nın nasıl bir kapsayıcı geçiş bildirim uygulanacağı hakkında daha fazla bilgi için okuma [bir korumalı alan için bir uygulama geçirme](https://developer.apple.com/library/prerelease/mac/documentation/Security/Conceptual/AppSandboxDesignGuide/MigratingALegacyApp/MigratingAnAppToASandbox.html#//apple_ref/doc/uid/TP40011183-CH6-SW1) belgeleri.

## <a name="summary"></a>Özet

Bu makalede, bir Xamarin.Mac uygulamasını korumalı alana alma ayrıntılı bilgi sürdü. İlk olarak, uygulama korumalı alanı ile ilgili temel bilgileri göstermek için bir yalnızca Xamarin.Mac uygulamasını oluşturduk. Ardından, korumalı alan ihlali gidermek nasıl gösterdi. Ardından, ayrıntılı bir attığımız uygulama korumalı alanı konum ve son olarak, uygulama korumalı alanı bir Xamarin.Mac uygulamasını tasarlama sırasında incelemiştik.



## <a name="related-links"></a>İlgili bağlantılar

- [App Store’da Yayımlama](~/mac/deploy-test/publishing-to-the-app-store/index.md)
- [Uygulama korumalı alanı hakkında](https://developer.apple.com/library/content/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html)
- [Yaygın uygulama korumalı alana alma sorunları](https://developer.apple.com/library/content/qa/qa1773/_index.html)
