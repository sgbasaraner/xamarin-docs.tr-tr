---
title: Sertifikalar ve tanımlayıcıları
description: Bu kılavuz gerekli sertifikaları ve Xamarin.Mac uygulama yayımlamak için gereken tanımlayıcıları oluşturmada size yol gösterir.
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d94819be2c014aec5edfae19959ce949ee8dcd4b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="certificates-and-identifiers"></a>Sertifikalar ve tanımlayıcıları

_Bu kılavuz gerekli sertifikaları ve bir Xamarin.Mac uygulamayı yayımlamak için gerekli olacak tanımlayıcıları oluşturmada size yol gösterir._

## <a name="certificates-and-identifiers"></a>Sertifikalar ve tanımlayıcıları

Ziyaret [Apple Developer Member Center'da](http://developer.apple.com) Mac geliştirme için yapılandırılır. Ana menü aşağıda gösterilmiştir:

[![Apple Geliştirici üye Merkezi'nde](certificates-identifiers-images/devcenter01.png "Apple Developer Member Center'da")](certificates-identifiers-images/devcenter01-large.png#lightbox)

Tıklayın **tanımlayıcıları & profilleri, sertifikaları** bağlantı:

[![Sertifikalar, tanımlayıcılar & profilleri seçme](certificates-identifiers-images/devcenter02.png "tanımlayıcıları & profilleri, sertifikaları seçme")](certificates-identifiers-images/devcenter02-large.png#lightbox)

Ardından, tıklayın **sertifikaları bağlantısına** altında **Mac uygulamaları** bölümü:

[![Sertifikaları bağlantısını seçerek](certificates-identifiers-images/devcenter03.png "sertifikaları bağlantısına seçme")](certificates-identifiers-images/devcenter03-large.png#lightbox)

Tıklayın **tüm** 'ı tıklatın ve bağlama **+** düğmesi:

[![Tümünü seçme ve yeni bir öğe ekleme](certificates-identifiers-images/certif01.png "tüm seçerek ve yeni bir öğe ekleme")](certificates-identifiers-images/certif01-large.png#lightbox)

Burada indirme alanından **ara sertifika** (dünya çapında Geliştirici ilişkileri sertifika yetkilisi ve geliştirici kimliği sertifika yetkilisi) gerekiyorsa. Ancak, bu kurulum geliştirici tarafından Xcode için otomatik olarak olmalıdır.

Bu bölüm geri kalan her Mac Geliştirici hesabı kurulumu tamamlamak için dört bölümden anlatılmaktadır.

-   **Mac uygulama kimliği'ni kaydetmeniz** – Geliştirici yazmadan her uygulama için aşağıdaki adımları izlemeniz gerekir.
-   **MacOS sistemleri kaydetmek** – yalnızca budur bilgisayar ekleme test etmek için gereklidir.
-   **Sertifikaları oluşturma** – sadece bir kez seting yukarı olduğunda gerekli sertifikaları ve bunları yenilenirken üstü.
-   **Sağlama profili oluşturma** – Geliştirici yeni sistemleri eklerken yazılan her yeni bir uygulama için ve aşağıdaki adımları izlemeniz gerekir.

Tıklatın **genel bakış** üst bağlantı herhangi bir zamanda bu menüye dönmek için sayfanın sol.

### <a name="register-mac-app-id"></a>Mac uygulama kimliği kaydetme

Geliştirici yazılan her bir uygulama için bir uygulama kimliği'ni kaydetmeniz gerekir. "MacWriter" adlı bir temel örnek uygulama için bir giriş oluşturmak için aşağıdaki adımları kullanın.

1. Girin bir **uygulama kimliği açıklaması** ve seçin **uygulama hizmetleri** uygulama gerektiren: 

    [![Açıklama ve uygulama hizmetleri girme](certificates-identifiers-images/devcenter04.png "açıklama ve uygulama hizmetleri girme")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. Girin bir **paket kimliği** tıklatın ve uygulama için **devam** düğmesi: 

    [![Bir paket kimliği girme](certificates-identifiers-images/devcenter05.png "bir paket kimliği girme")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. Bilgileri doğrulayın ve tıklatın **gönderme** düğmesi: 

    [![Bilgileri doğrulanıyor](certificates-identifiers-images/devcenter06.png "bilgileri doğrulanıyor")](certificates-identifiers-images/devcenter06-large.png#lightbox)

Bazı **uygulama hizmetleri** yapılandırma (örneğin, iCloud) daha fazla gerektirebilir. Bu durumda, yeni oluşturduğunuz yeni uygulama Kimliğini seçin ve'ı tıklatın **Düzenle** düğmesi:

[![Yeni uygulama kimliği düzenleme](certificates-identifiers-images/devcenter07.png "yeni uygulama kimliği düzenleme")](certificates-identifiers-images/devcenter07-large.png#lightbox)

İCloud hizmetleri yapılandırmak için tıklatın **Düzenle** düğmesi:

[![İCloud Hizmetleri Yapılandırma](certificates-identifiers-images/devcenter08.png "iCloud Hizmetleri'ni yapılandırma")](certificates-identifiers-images/devcenter08-large.png#lightbox)

Buradan Geliştirici bunlar kullanarak veritabanlarını yapılandırabilirsiniz:

[![Veritabanlarını yapılandırma](certificates-identifiers-images/devcenter09.png "veritabanları yapılandırılıyor")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### <a name="register-macos-systems"></a>MacOS sistemleri kaydetme

Geliştirici, test etmek için bir sağlama profili oluşturmak için Mac bilgisayarlarını kayıtlı olması gerekir. Bunlar en fazla Mac uygulamalarını test etme için 100 bilgisayarları kaydedebilirsiniz.

Mac Geliştirici Merkezi seçin **tüm** gelen **aygıtları** 'ye tıklayın **+** düğmesi:

[![Yeni bilgisayar ekleme](certificates-identifiers-images/devcenter10.png "yeni bilgisayar ekleme")](certificates-identifiers-images/devcenter10-large.png#lightbox)

Girin bir **adı** ve **UUID** ekleyin ve'ı tıklatın bilgisayarın **devam** düğmesi. Bilgi ve ardından gözden **kaydetmek** düğmesi:

[![Yeni bilgisayar bilgilerini girme](certificates-identifiers-images/devcenter11.png "yeni bilgisayar bilgilerini girme")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### <a name="create-certificates"></a>Sertifikaları oluşturma

Mac uygulamalarını imzalamak için kullanılan sertifikaları birkaç farklı türde oluşturmak için sertifikalar bölümünü kullanın:

[![Yeni bir sertifika oluşturma](certificates-identifiers-images/certif01.png "yeni bir sertifika oluşturma")](certificates-identifiers-images/certif01-large.png#lightbox)

Ana üç sertifika vardır:

-   **Mac geliştirme sertifikası** – isteğe bağlı Geliştirici iCloud ya da anında iletme bildirimleri gibi özellikleri kullanmak planları, ancak genel uygulama geliştirme için gerekli. Bu özelliklere erişmek izin sağlama profilleri oluşturabilmeniz için önce Geliştirici geliştirme sertifikası gerekir.
-   **Mac App Store** – geliştirici bir sertifika, uygulama ve başka bir sertifika için yükleyiciyi gerekir.
-   **Geliştirici kimliği** – sertifikaları uygulama ve yükleyici için Mac App Store dışında dağıtmak tercih.

Aşağıdaki bölümlerde yukarıdaki sertifika türleri oluşturma örnekler sağlar.

#### <a name="mac-development-certificate"></a>Mac geliştirme sertifikası

Daha önce belirtildiği gibi Mac uygulaması geliştirme sertifikası iCloud gibi macOS özellikler veya anında iletme bildirimleri kullanılan sürece gerekli değildir.

Oluşturulan için yeni bir geliştirme sertifikası aşağıdakileri yapın:

1. Seçin **Mac geliştirme** radyo düğmesini seçin ve **devam**: 

     [![Geliştirme sertifikası ekleme](certificates-identifiers-images/certif02.png "geliştirme sertifikası ekleme")](certificates-identifiers-images/certif02-large.png#lightbox)
2. Sonraki ekranda, Anahtarlık erişimi bir sertifika imzalama isteği dosyasını karşıya yüklemek için oluşturmak için nasıl kullanılacağını açıklanmaktadır: 

    [![Anahtarlık erişimi karşıya yükleme ekranında](certificates-identifiers-images/certif03.png "Anahtarlık erişimi karşıya yükleme ekranı")](certificates-identifiers-images/certif03-large.png#lightbox)
3. Son sertifika oluşturulduğunda, daha sonra anlaşılacak böylece sertifika için anlamlı ortak bir ad seçin. Sonraki adımda bulunabilir şekilde dosyasının kaydedildiği unutmayın: 

     ![Sertifika verme](certificates-identifiers-images/image12.png "sertifika verme")
4. Bir sertifika isteği dosyasını (uzantısı `.certSigningRequest`) Mac üzerinde yerel olarak kaydedilecek Nereye kaydettiğinizi unutmayın (varsayılan locationis Masaüstü) sonraki adımda seçilmesi gerekir: 

     [![Sertifika dosyası karşıya yükleniyor](certificates-identifiers-images/image13.png "sertifika dosyası karşıya yükleniyor")](certificates-identifiers-images/image13-large.png#lightbox)
5. Tıklatın **karşıdan** içinde yüklemek için çift tıklayın ve sertifika almak için **Anahtarlık**: 

     [![Geliştirme sertifikası indirme](certificates-identifiers-images/image15.png "geliştirme sertifikası yükleniyor")](certificates-identifiers-images/image15-large.png#lightbox)
6. Tıklatın **karşıdan** içinde yüklemek için çift tıklayın ve sertifika almak için **Anahtarlık**. **Geliştirici sertifika yardımcı programını** gibi bu sertifikalar gösterir: 

     [![Geliştirici sertifika yardımcı programını](certificates-identifiers-images/image16.png "Geliştirici sertifika yardımcı programını")](certificates-identifiers-images/image16-large.png#lightbox)
7. Ayrıca, görünür **Anahtarlık** şöyle: 

     ![Anahtarlık erişimi sertifikada](certificates-identifiers-images/image17.png "Anahtarlık erişimi sertifikada")

Daha önce belirtildiği Geliştirici iCloud ve anında iletme bildirimleri gibi macOS özellikleri uygulama sürece bir geliştirici sertifikası her zaman gerekli değildir. Ayrıca oluşturmak için gerekli bir **geliştirme sağlama profili**, hangi gerekecektir Mac App Store uygulamaları test etmek için.

#### <a name="mac-app-store-certificates"></a>Mac App Store sertifikaları

App Store'dan bir uygulama yayımlamayı **Mac App Store** uygulama ve Mac Yükleyici paketi imzalamak için kullanılan sertifika oluşturulması gerekir.

1. Seçin **Mac App Store** tıklatın ve sertifika türü olarak **devam** düğmesi: 

    [![Bir uygulama mağazası sertifika oluşturma](certificates-identifiers-images/certif04.png "bir uygulama mağazası sertifika oluşturma")](certificates-identifiers-images/certif04-large.png#lightbox)
2. Türünü seçin, oluşturmak için sertifika (bir uygulama mağazasında serbest bırakmak için her tür gerekecektir): 

    [![Sertifika türü seçme](certificates-identifiers-images/certif05.png "sertifika türü seçme")](certificates-identifiers-images/certif05-large.png#lightbox)
3. Sonraki sayfaya nasıl kullanılacağı açıklanmaktadır **Anahtarlık erişimi** bir sertifika isteği dosyasını oluşturmak için. Yönergeleri izleyin: 

     [![Anahtarlık isteği oluşturma](certificates-identifiers-images/certif06.png "Anahtarlık isteği oluşturma")](certificates-identifiers-images/certif06-large.png#lightbox)
4. Bir açıklayıcı seçin **ortak ad** – adı metinde "Uygulama mağazası uygulaması" örnek kullanmak için: 

     ![Açıklayıcı bir ad girmeyi](certificates-identifiers-images/image20.png "açıklayıcı bir ad girin")
5. Bir sertifika isteği dosyasını (uzantısı `.certSigningRequest`) Mac üzerinde yerel olarak kaydedilecek Nereye kaydettiğinizi unutmayın (varsayılan konum Masaüstü,): 

     [![Sertifikayı kaydetme](certificates-identifiers-images/image21.png "sertifikayı kaydetme")](certificates-identifiers-images/image21-large.png#lightbox)
6. Tıklatın **karşıdan** sertifikanızı almak ve bunu yüklemek için çift tıklatın **Anahtarlık**: 

      [![Uygulama mağazası sertifika indirme](certificates-identifiers-images/image23.png "App Store sertifika yükleme")](certificates-identifiers-images/image23-large.png#lightbox)
7. Tıklatın **devam** ve başka bir sertifika için olacaktır bu kez karşıdan yüklemek için tam aynı adımları *yükleyici*: 

     [![Yükleyici seçme](certificates-identifiers-images/image24.png "yükleyici seçme")](certificates-identifiers-images/image24-large.png#lightbox)
8. Bir açıklayıcı seçin **ortak ad** – örneği kullanmak için adında "AppStore yükleyici" metin: 

     ![Sertifika adını ayarlama](certificates-identifiers-images/image25.png "sertifika adını ayarlama")
9. Bir sertifika isteği dosyasını (uzantısı `.certSigningRequest`) Mac üzerinde yerel olarak kaydedilecek Nereye kaydettiğinizi unutmayın (varsayılan konum Masaüstü,): 

     [![Sertifika karşıya](certificates-identifiers-images/image26.png "sertifikası karşıya yükleniyor")](certificates-identifiers-images/image26-large.png#lightbox) 

     [![Dağıtım sertifikası indirme](certificates-identifiers-images/image28.png "dağıtım sertifikası yükleniyor")](certificates-identifiers-images/image28-large.png#lightbox)
10. Tıklatın **karşıdan** içinde yüklemek için çift tıklayın ve sertifika almak için **Anahtarlık**. Bu gibi sertifikalar Geliştirici sertifikası yardımcı programı'nı gösterir: 

     [![Geliştirici sertifika yardımcı programını](certificates-identifiers-images/image29.png "Geliştirici sertifika yardımcı programını")](certificates-identifiers-images/image29-large.png#lightbox)
11. İki yeni sertifika artık görünür durumda olur **Anahtarlık**: 

     [![Anahtarlık erişimi sertifikada](certificates-identifiers-images/image30.png "Anahtarlık erişimi sertifikada")](certificates-identifiers-images/image30-large.png#lightbox)

#### <a name="developer-id-certificates"></a>Geliştirici kimliği sertifikaları

Xamarin.Mac uygulama (sürüm Apple App Store aracılığıyla değil) otomatik olarak serbest bırakmak için geliştirici sürümü ve yükleme için uygulamayı imzalamak için bir geliştirici kimliği sertifika gerekir.

Aşağıdakileri yapın:

1. Gelen **sertifikaları** bölümünde, Başlat'ı tıklatın tarafından **+** düğmesine ve ardından **Geliştirici kimliği** radyo düğmesi: 

    [![Bir geliştirici kimliği ekleme](certificates-identifiers-images/certif07.png "bir geliştirici kimliği ekleme")](certificates-identifiers-images/certif07-large.png#lightbox)
2. Tıklatın **devam** düğmesine tıklayın ve kimlik oluşturmak için geliştirici türünü seçin: 

    [![Geliştirici kimliği türü seçme](certificates-identifiers-images/certif08.png "Geliştirici kimliği türü seçme")](certificates-identifiers-images/certif08-large.png#lightbox)
3. İki gerekli olacak bir uygulamayı imzalamak için ve bir uygulamanın yükleyici imzalamak için. Sertifika istekleri için bu anahtarları adlandırırken dikkatli olun: metni içeren açıklayıcı adlar kullanın `Application` ve `Installer` bunlar daha sonra ayırt edici olabilir.
4. Sonraki ekranda sertifikayı oluşturmak nasıl ayrıntılı yönergeleri sağlar **devam** düğmesi: 

    [![Sertifika oluşturma](certificates-identifiers-images/certif09.png "sertifikası nasıl oluşturulur")](certificates-identifiers-images/certif09-large.png#lightbox)
5. Bir açıklayıcı seçin **ortak ad** – adı metinde "Developer kimliği uygulaması" örnek kullanmak için: 

     ![Sertifika için bir ad girmeyi](certificates-identifiers-images/image33.png "sertifika için bir ad girme")
6. Bir sertifika isteği dosyasını (uzantısı `.certSigningRequest`) Mac üzerinde yerel olarak kaydedilecek Nereye kaydettiğinizi unutmayın (varsayılan konum Masaüstü,): 

     [![Sertifika karşıya](certificates-identifiers-images/certif10.png "sertifikası karşıya yükleniyor")](certificates-identifiers-images/certif10-large.png#lightbox) 

     [![Geliştirici kimliği indirme](certificates-identifiers-images/certif11.png "geliştirici kodu indiriliyor")](certificates-identifiers-images/certif11-large.png#lightbox)
7. Tıklatın **karşıdan** içinde yüklemek için çift tıklayın ve sertifika almak için **Anahtarlık**.
8. Tıklatın **devam** ve başka bir sertifika için olacaktır bu kez karşıdan yüklemek için tam aynı adımları *yükleyici*.
9. Bir açıklayıcı seçin **ortak ad** – adında "Developer kimliği yükleyici" metin örnek kullanmak için: 

     ![Ortak bir ad girmeyi](certificates-identifiers-images/image38.png "ortak bir ad girin")
10. Bir sertifika isteği dosyasını (uzantısı `.certSigningRequest`) Mac üzerinde yerel olarak kaydedilecek Nereye kaydettiğinizi unutmayın (varsayılan konum Masaüstü,): 

     [![Bir sertifika karşıya](certificates-identifiers-images/certif10.png "bir sertifikası karşıya yükleniyor")](certificates-identifiers-images/certif10-large.png#lightbox)
11. Sertifika indirme için kullanılabilir ise – tıklatın **karşıdan** düğmesini tıklatmadan önce **Bitti**: 

     [![Bir sertifika indirme](certificates-identifiers-images/certif11.png "bir sertifika yükleme")](certificates-identifiers-images/certif11-large.png#lightbox)
12. Tıklatın **karşıdan** içinde yüklemek için çift tıklayın ve sertifika almak için **Anahtarlık**. **Geliştirici sertifika yardımcı programını** gibi bu sertifikalar gösterir: 

     [![Geliştirici sertifika yardımcı programını](certificates-identifiers-images/certif12.png "Geliştirici sertifika yardımcı programını")](certificates-identifiers-images/certif12-large.png#lightbox)
13. Aşağıdaki öğeler görünür **Anahtarlık**: 

     [![Anahtarlık erişimi sertifikada](certificates-identifiers-images/image43.png "Anahtarlık erişimi sertifikada")](certificates-identifiers-images/image43-large.png#lightbox)


## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](/visualstudio/mac/installation/)
- [Merhaba, Mac örnek](~/mac/get-started/hello-mac.md)
- [Mac App Store, uygulamaları dağıtma](https://developer.apple.com/devcenter/mac/checklist/)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
