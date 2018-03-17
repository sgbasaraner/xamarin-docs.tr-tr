---
title: Jenkins Xamarin ile kullanma
description: "Bu kılavuz, Jenkins sürekli tümleştirme sunucusu olarak ayarlayın ve Xamarin ile oluşturulan mobil uygulamaları derleme otomatikleştirmek göstermektedir. Jenkins OS x'te yüklemek, yapılandırmak ve kaynak kodu yönetimi sistemi değişiklikler uygulandıktan zaman Xamarin.iOS ve Xamarin.Android uygulamaları derlemek için işler ayarlamak nasıl açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1E6825DF-1254-4FCB-B94D-ADD33D1B5309
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: ff754a690627e7e2f0a5cd39dd669a4c9ddd47fb
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="using-jenkins-with-xamarin"></a>Jenkins Xamarin ile kullanma

_Bu kılavuz, Jenkins sürekli tümleştirme sunucusu olarak ayarlayın ve Xamarin ile oluşturulan mobil uygulamaları derleme otomatikleştirmek göstermektedir. Jenkins OS x'te yüklemek, yapılandırmak ve kaynak kodu yönetimi sistemi değişiklikler uygulandıktan zaman Xamarin.iOS ve Xamarin.Android uygulamaları derlemek için işler ayarlamak nasıl açıklar._

[Xamarin ile sürekli tümleştirme giriş](~/tools/ci/intro-to-ci.md) sürekli tümleştirme bozuk ya da uyumsuz kod erken uyarı sağlayan yararlı yazılım geliştirme uygulaması olarak tanıtır. Çıkan ve yazılım dağıtımı için uygun durumda tutar CI geliştiricilerin sorunları ve sorunları gidermek üzere sağlar. Bu kılavuzda, her iki belge içerikten birlikte kullanmak alınmaktadır.

Bu kılavuz, Jenkins OS X çalıştıran ayrılmış bir bilgisayara yükleyin ve bilgisayar başlatıldığında otomatik olarak çalışacak şekilde yapılandırmanız gösterilmektedir. Jenkins yüklendikten sonra MS Build desteklemek için ek eklentileri yükleriz. Jenkins Git kutu dışı destekler. TFS kaynak kodu denetimi için kullanılıyorsa, bir ek eklentisi ve komut satırı yardımcı programları da yüklenmesi gerekir.

Jenkins yapılandırılır ve gerekli tüm eklentileri yüklü sonra Xamarin.Android ve Xamarin.iOS projeleri derlemek için bir veya daha fazla iş oluşturacağız. Bir iş, adımları ve bazı işlemler gerçekleştirmek için gerekli meta veriler koleksiyonudur. Bir işi genellikle aşağıdakilerden oluşur:

-  **Kaynak kodu Yönetimi (SCM)** – kaynak kodu denetimi için bağlanma ve almak için dosyaları hakkında bilgi içeren bir meta veri girişi Jenkins yapılandırma dosyalarında budur.
-  **Tetikleyiciler** – Tetikleyicileri ne zaman bir geliştirici değişiklikleri kaynak kodu depoya kaydeder gibi bazı eylemler, temel bir işi başlatmak için kullanılır.
-  **Yönergeler yapı** – bir eklenti veya kaynak kodu derleme ve mobil cihazlarda yüklü bir ikili üreten bir betik budur.
-  **İsteğe bağlı yapı eylemleri** – Bu kod, kod imzalama, statik çözümlemesini birim testlerini çalıştırma içerebilir veya diğer gerçekleştirmek için başka bir iş başlatmayı ilgili iş oluşturun.
-  **Bildirimleri** – bir işi bir yapı durumu hakkında bildirim çeşit gönderebilir.
-  **Güvenlik** – isteğe bağlı olsa da, kesinlikle önerilir Jenkins güvenlik özellikleri de etkinleştirilmiş olmalıdır.


Bu kılavuz, her biri bu noktalarını kapsayan bir Jenkins server kurulumunu nasıl aracılığıyla yol gösterir. Bunu sonu biz Kurulum ve Jenkins IPA APK'ın bizim Xamarin mobil projelerde oluşturup yapılandırmak nasıl bir iyi anlamış olmanız gerekir.

## <a name="requirements"></a>Gereksinimler

İdeal yapı sunucu oluşturma ve büyük olasılıkla uygulamayı test etme tek amacı ayrılmış tek başına bir bilgisayardır. Diğer roller (örneğin, bir web sunucusunun) için gerekli olabilecek yapıları yapı zarar verme değil, özel bir bilgisayar sağlar. Örneğin, yapı sunucunun de bir web sunucusu gibi davranan, web sunucusu bazı ortak kitaplığı çakışan bir sürümünü gerektirebilir. Bu çakışma nedeniyle web sunucusunda düzgün çalışmayabilir ya da Jenkins kullanıcılara dağıtıldığında çalışmıyor derlemeleri oluşturabilir.

Xamarin mobil uygulamaları için yapı sunucu çok benzer bir geliştiricinin iş istasyonu şekilde ayarlayın. Hangi Jenkins Mac Visual Studio içinde bir kullanıcı hesabı varsa ve Xamarin.iOS ve Xamarin.Android yüklenecek. Kod imzalama sertifikası, sağlama profilleri ve anahtar depolarındaki tümünün de yüklü olması gerekir. *Genellikle yapı sunucunun kullanıcı hesabı, geliştirici hesaplardan - tüm yazılım, anahtarları ve yapı sunucu kullanıcı hesabıyla oturum açmışken sertifikaları yükleme ve yapılandırma emin olun.*

Aşağıdaki diyagram tipik Jenkins derleme sunucusundaki bu öğelerin tümünü gösterir:

 [![](jenkins-walkthrough-images/image1.png "Bu diyagram tipik Jenkins derleme sunucusundaki bu öğelerin tümünü gösterir")](jenkins-walkthrough-images/image1.png#lightbox)

iOS uygulamaları yalnızca yerleşik ve Mac OS X çalıştıran bir bilgisayarda oturum. Mac Mini makul bir daha düşük maliyetli seçeneği ancak OS X 10.10 (Yosemite) çalıştırabilen herhangi bir bilgisayar veya üzeri yeterli değil.

TFS kaynak kodu denetimi için kullanılıyorsa, yüklemek istediğiniz [Takım Gezgini her yerde](http://www.microsoft.com/en-ca/download/details.aspx?id=40785), Microsoft tarafından kullanılabilir. Takım Gezgini her yerde TFS OS X Terminali, platformlar arası erişim sağlar.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="installing-jenkins"></a>Jenkins yükleme

Jenkins kullanarak ilk yüklemek için bir görevdir. Jenkins OS X: üzerinde çalıştırmak için üç yolu vardır

-  Arka planda çalışan bir arka plan programı gibi.
-  Tomcat, Jetty veya JBoss gibi bir servlet kapsayıcısı içinde.
-  Bir kullanıcı hesabı altında çalışan normal bir işlem olarak.


Çoğu geleneksel sürekli tümleştirme uygulamaları arka planda bir arka plan programı olarak da çalıştırabilirsiniz (OS x veya \*nix) veya bir hizmet olarak (Windows). Bu, GUI etkileşimi gerekli olduğu ve yapı ortamı kurulumu kolayca gerçekleştirilebilir burada senaryoları için uygun değildir. Mobil uygulamalar, ayrıca keystores ve Jenkins arka plan programı gibi çalıştırırken, erişimi sorunlu olabilecek sertifika imzalama gerektirir. Bu sorunları nedeniyle Jenkins bir kullanıcı hesabı altında yapı sunucu üzerinde çalışan üçüncü senaryo – bu belgeyi odaklanır.

Jenkins.App Jenkins yüklemek için kullanışlı bir yoludur. Bu bir başlangıç basitleştirir AppleScript sarmalayıcı ve durdurma Jenkins sunucusunun adıdır. Bir bash kabuğunda çalıştırmak yerine Jenkins bir uygulama olarak yerleştirme simgesiyle aşağıdaki ekran görüntüsünde gösterildiği gibi çalışır:

 [![](jenkins-walkthrough-images/image2.png "Bir bash kabuğunda çalıştırmak yerine Jenkins bir uygulama olarak yerleştirme simgesiyle bu ekran görüntüsünde gösterildiği gibi çalışır")](jenkins-walkthrough-images/image2.png#lightbox)

Başlatma veya durdurma Jenkins başlatma veya durdurma Jenkins.App olarak basit bir işlemdir.

Jenkins.App yüklemek için aşağıdaki ekran görüntüsünde gösterilen projenin indirme sayfasından en son sürümünü yükleyin:

 [![](jenkins-walkthrough-images/image3.png "Uygulama, en son projeleri sürümü bu ekran görüntüsünde gösterilen sayfasında, yükleme")](jenkins-walkthrough-images/image3.png#lightbox)

Zip dosyasını ayıklayın `/Applications` yapı sunucunuz ve onu olduğu gibi başka bir OS X uygulaması başlangıç klasörü.
İlk kez Jenkins.App başlattığınızda Jenkins karşıdan bildiren bir iletişim kutusu sunacaktır:

 [![](jenkins-walkthrough-images/image4.png "Uygulama, Jenkins karşıdan bildiren bir iletişim kutusu sunacaktır")](jenkins-walkthrough-images/image4.png#lightbox)

Jenkins.App, yükleme tamamlandıktan sonra Jenkins başlangıç özelleştirmek aşağıdaki ekran görüntüsünde görülen isteyip istemediğinizi soran başka bir iletişim kutusu görüntülenir:

 [![](jenkins-walkthrough-images/image5.png "Uygulama, yükleme tamamlandı, başka bir iletişim kutusu, Jenkins başlangıç özelleştirmek bu ekran görüntüsünde görülen isteyip istemediğinizi soran görüntülenir")](jenkins-walkthrough-images/image5.png#lightbox)

Jenkins özelleştirme isteğe bağlıdır ve Jenkins çoğu durumlarda çalışabilmesi için uygulama başlatıldığında – her zaman varsayılan ayarları gerçekleştirilmesi gerekmez.

Jenkins özelleştirmek gerekliyse, tıklayın **varsayılanları değiştirmek** düğmesi. Bu iki ardışık iletişim kutuları ile sunacaktır: Java komut satırı parametrelerini soran, diğeri için Jenkins komut satırı parametreleri soran. Aşağıdaki iki ekran görüntüleri, bu iki iletişim kutularının göster:

 [![](jenkins-walkthrough-images/image6.png "Bu ekran iletişim kutuları gösterir")](jenkins-walkthrough-images/image6.png#lightbox)

 [![](jenkins-walkthrough-images/image7.png "Bu ekran iletişim kutuları gösterir")](jenkins-walkthrough-images/image7.png#lightbox)

Jenkins çalışmaya başladıktan sonra bilgisayarda kullanıcı oturumu içinde her seferinde başlayacak şekilde bir oturum açma öğesi olarak ayarlamak isteyebilirsiniz. Yuva Jenkins simgesine sağ tıklayıp seçerek bunu yapabilirsiniz **seçenekleri... Oturum Aç**aşağıdaki ekran görüntüsünde gösterildiği gibi:

 [![](jenkins-walkthrough-images/image8.png "Yuva Jenkins simgesine sağ tıklayarak ve oturum açma, adresindeki OptionsOpen seçerek bu ekran görüntüsünde gösterildiği gibi bunu yapabilirsiniz")](jenkins-walkthrough-images/image8.png#lightbox)

Bu her zaman otomatik olarak başlatılacak şekilde Jenkins.App neden olacak kullanıcı oturum açtığında, ancak değil bilgisayar önyüklendiğinde ayarlama. OS X önyükleme sırasında otomatik olarak ile oturum açma için kullanacağı bir kullanıcı hesabı belirtmek mümkündür. Açık **sistem tercihleri**seçip **kullanıcı & grupları** bu ekran görüntüsünde gösterildiği gibi simgesi:

 [![](jenkins-walkthrough-images/image9.png "Sistem tercihleri açın ve bu ekran görüntüsünde gösterildiği gibi kullanıcı grupları simgesini seçin")](jenkins-walkthrough-images/image9.png#lightbox)

Tıklayın **oturum açma seçenekleri** düğmesine tıklayın ve ardından OS X için oturum açma önyükleme sırasında kullanacağı hesabı seçin.

Bu noktada Jenkins yüklendi. Xamarin mobil uygulamalar istiyoruz, ancak biz bazı eklentiler yüklemeniz gerekir.


### <a name="installing-plugins"></a>Eklenti yükleme

Jenkins.App yükleyici tamamlandığında, Jenkins başlatın ve http://localhost: 8080, URL ile web tarayıcısı aşağıdaki ekran görüntüsünde gösterildiği gibi başlatın:

 [![](jenkins-walkthrough-images/image10.png "Bu ekran görüntüsünde gösterildiği gibi 8080")](jenkins-walkthrough-images/image10.png#lightbox)

Bu sayfadan seçin **Jenkins > yönetmek Jenkins > eklentileri yönetme** üst sol alt köşedeki aşağıdaki ekran görüntüsünde gösterildiği gibi menüsünden:

 [![](jenkins-walkthrough-images/image11.png "Bu sayfadan üst sol alt köşedeki menüsünden Jenkins yönetmek Jenkins yönetmek eklentileri seçin")](jenkins-walkthrough-images/image11.png#lightbox)

Bu görüntüler **Jenkins eklentisi Yöneticisi** sayfası. Kullanılabilir sekmesinde tıklarsanız, indirilir ve yüklenir 600'den fazla eklentiniz listesini görürsünüz. Bu, aşağıdaki ekran görüntüsünde gösterilen:

 [![](jenkins-walkthrough-images/image12.png "Kullanılabilir sekmesini tıklatın, indirilir ve yüklenir 600'den fazla eklentiniz listesini görürsünüz")](jenkins-walkthrough-images/image12.png#lightbox)

Tüm 600 birkaç can sıkıcı olabilir bulmak için eklenti ve hataya kaydırma. Jenkins arabirimi sağ üst köşesinde filtre arama alanı sağlar. Aramak için bu filtre alanına kullanarak bulmayla ve yüklü biri veya tümü aşağıdaki eklenti basitleştirir:

-  **Jenkins MSBuild eklentisi** – bu eklentisi, Visual Studio ve Visual Studio (.sln) Mac çözümler ve projeler (.csproj) için yapı mümkün kılar.
-  **Ortam Injector eklentisi** – bu işi ortam değişkenlerini ayarlama ve düzeyi oluşturmak mümkün kılan bir isteğe bağlıdır, ancak yararlı eklentidir. Uygulama kodu için kullanılan parolalar oturum gibi aynı zamanda değişkenleri için ek koruma sağlar. Bazen olarak kısaltılır *EnvInject eklentisi* .
-  **Team Foundation Server eklentisi** – Bu, yalnızca, isteğe bağlı bir eklentidir Team Foundation Server veya kaynak kodu denetimi için Team Foundation Hizmetleri kullanıyorsanız, gerekli.

Jenkins Git herhangi bir ek eklentiniz destekler.

Tüm eklentileri yükledikten sonra Jenkins yeniden başlatın ve her eklenti için genel ayarları yapılandırma istersiniz. Bir eklenti için genel ayarları seçerek bulunabilir **Jenkins > yönetmek Jenkins > yapılandırma sistem** üst sol Köşeden aşağıdaki ekran görüntüsünde gösterildiği gibi:

 [![](jenkins-walkthrough-images/image13.png "Bir eklenti için genel ayarları Jenkins seçerek bulunabilir / yönetmek Jenkins / sol üst yapılandırma sistemden el köşesi")](jenkins-walkthrough-images/image13.png#lightbox)

Bu menü seçeneğini seçtiğinizde, ne götürülürsünüz **yapılandırma sistemi [Jenkins]** sayfası. Bu sayfa Jenkins kendisini yapılandırmak ve bazı genel eklentisi değerleri ayarlamak için bölümleri içerir.  Aşağıdaki ekran görüntüsünde, bu sayfanın bir örnek gösterilmektedir:

 [![](jenkins-walkthrough-images/image14.png "Bu ekran bu sayfasının bir örneği gösterir")](jenkins-walkthrough-images/image14.png#lightbox)


#### <a name="configuring-the-msbuild-plugin"></a>MSBuild eklentisi yapılandırma

MSBuild eklentisini kullanmak için yapılandırılmalıdır **/Library/Frameworks/Mono.framework/Commands/xbuild** Visual Studio için Mac çözüm ve proje dosyalarını derlemek için. Aşağı kaydırarak **yapılandırma sistemi [Jenkins]** kadar sayfa **eklemek MSBuild** düğmesi görünür, aşağıdaki ekran görüntüsünde gösterildiği gibi:

 [![](jenkins-walkthrough-images/image15.png "MSBuild Ekle düğmesi görününceye kadar yapılandırma sistem Jenkins sayfayı aşağı kaydırın")](jenkins-walkthrough-images/image15.png#lightbox)

Bu düğmesini tıklatın ve doldurmak **adı** ve **yolu** için **MSBuild** görünen formdaki alanları. Adını, **MSBuild** yükleme sırasında anlamlı bir şey olmalıdır **MSBuild yolu** yolu olmalıdır `xbuild`, normalde **/Library/çerçeveler / Mono.framework/Commands/xbuild**. Biz kaydetme veya sayfanın altındaki Uygula düğmesini tıklatarak değişiklikleri kaydettikten sonra Jenkins kullanabilmek için `xbuild` çözümünüzü derlemek için.

#### <a name="configuring-the-tfs-plugin"></a>TFS eklentisi yapılandırma

Bu bölümde, kaynak kodu denetimi için TFS kullanmak istiyorsanız zorunludur.

TFS sunucusu ile etkileşim bir OS X iş istasyonu için sırayla Takım Gezgini her yerde iş istasyonunda yüklenmelidir. Takım Gezgini her yerde bir TFS erişmek için platformlar arası komut satırı istemcisi içerir Microsoft'tan araç kümesidir. Takım Gezgini her yerde Microsoft'tan indirilebilen ve üç adımda yüklü:

1. Arşiv dosyasını kullanıcı hesabı ile erişilebilen bir dizine ayıklayın. Örneğin, dosyaya sıkıştırmasını **~/tee**.
2. Biri Yukarıdaki adımda sıkıştırması açılmış dosyalarının bulunduğu klasörü dahil etmek için kabuk veya sistem yolu yapılandırın. Örneğin,

        echo export PATH~/tee/:$PATH' >> ~/.bash_profile

3. Takım Gezgini her yerde yüklendiğinden emin olmak için bir terminal oturumu açın ve yürütme `tf` komutu. Tf doğru olarak yapılandırılmışsa, terminal oturumunda aşağıdaki çıktı görürsünüz:

        $ tf
        Team Explorer Everywhere Command Line Client (version 11.0.0.201306181526)

        Available commands and their options:

TFS için komut satırı istemcisi yüklendikten sonra Jenkins tam yolu yapılandırılmalıdır `tf` komut satırı istemcisi. Aşağı kaydırarak **yapılandırma sistemi [Jenkins]** Team Foundation Server bölümünü bulana kadar aşağıdaki ekran görüntüsünde gösterildiği gibi sayfa:

 [![](jenkins-walkthrough-images/image17.png "Team Foundation Server bölümünü bulana kadar yapılandırma sistem Jenkins sayfayı aşağı kaydırın")](jenkins-walkthrough-images/image17.png#lightbox)

Tam yolunu girin `tf` tıklayın ve komut **kaydetmek** düğmesi.

### <a name="configure-jenkins-security"></a>Jenkins güvenliği yapılandırma

Ayarlayın ve her türlü iş anonim olarak çalıştırmak herhangi bir kullanıcı için mümkündür ilk yüklendiğinde, Jenkins devre dışı, güvenlik vardır. Bu bölümde kimlik doğrulama ve yetkilendirme yapılandırmak için Jenkins kullanıcı veritabanını kullanarak güvenliğinin nasıl yapılandırılacağı ele alınmaktadır.

Güvenlik ayarları seçerek bulunabilir **Jenkins > yönetmek Jenkins > Genel Güvenlik Yapılandırması**bu ekran görüntüsünde gösterildiği gibi:

 [![](jenkins-walkthrough-images/image18.png "Güvenlik ayarları Jenkins seçerek bulunabilir / yönetmek Jenkins / genel güvenlik yapılandırması")](jenkins-walkthrough-images/image18.png#lightbox)

Üzerinde **genel güvenlik yapılandırması** sayfasında, onay **etkinleştir güvenlik** onay kutusunu ve **erişim denetimi** form görünmelidir, sonraki ekran görüntüsüne benzer:

 [![](jenkins-walkthrough-images/image19.png "Genel Güvenlik Yapılandırması sayfasında etkinleştir güvenlik denetimi onay ve erişim denetimi formun görünmelidir, bu ekran görüntüsüne benzer")](jenkins-walkthrough-images/image19.png#lightbox)

Değiştirme için radyo düğmesi **Jenkins kendi kullanıcı veritabanı** içinde **güvenlik bölgesi bölümü**ve emin olun **kaydolmak kullanıcıların** Ayrıca, gösterilen olarak denetlenir Aşağıdaki ekran görüntüsü:

 [![](jenkins-walkthrough-images/image20.png "Değiştirme güvenlik bölgesi bölümünde Jenkins kendi kullanıcı veritabanı için radyo düğmesi ve kaydolmak için kullanıcıların ver de kullanıma emin olun")](jenkins-walkthrough-images/image20.png#lightbox)

Son olarak, Jenkins yeniden başlatın ve yeni bir hesap oluşturun. Oluşturulan ilk hesap kök hesabıdır ve bu hesap için bir yönetici otomatik olarak yükseltilir. Geri gidin **genel güvenlik yapılandırması** sayfasında ve onay kutusunu temizleyin **matris tabanlı güvenlik** radyo düğmesi. Kök hesabı tam erişim verilmesi ve aşağıdaki ekran görüntüsünde gösterildiği gibi salt okunur erişim, anonim hesap verilmelidir:

 [![](jenkins-walkthrough-images/image21.png "Kök hesabı tam erişim verilmesi ve anonim hesap salt okunur erişim verilmesi gerekir")](jenkins-walkthrough-images/image21.png#lightbox)

Bu ayarları kaydedilir ve Jenkins yeniden başlatıldıktan sonra güvenlik açılır.


#### <a name="disabling-security"></a>Güvenlik devre dışı bırakma

Unutulan parolayı veya Jenkins genelinde kilitleme olması durumunda, aşağıdaki adımları izleyerek güvenlik devre dışı bırakmak mümkündür:

1. Stop Jenkins. Jenkins.app kullanıyorsanız, yuva Jenkins.App simgesine sağ tıklayarak ve açılır menüsünden Çık seçerek bunu yapabilirsiniz:

    ![](jenkins-walkthrough-images/image19.png "Yuva ve açılır menüsünden Çık'ı seçerek uygulama simgesi")
2. Dosyayı açmak **~/.jenkins/config.xml** bir metin düzenleyicisinde.
3. Değerini değiştirme `<usesecurity></usesecurity>` öğesinden `true` için `false`.
4. Silme `<authorizationstrategy></authorizationstrategy>` ve `<securityrealm></securityrealm>` dosyasından öğeleri.
5. Jenkins yeniden başlatın.

## <a name="setting-up-a-job"></a>Proje ayarlama

En üst düzeyde Jenkins tüm yazılımına oluşturmak için gereken çeşitli görevleri düzenler bir *iş*. Bir işi, kaynak kodu derleme ne sıklıkla çalışmalı, yapı için gerekli olan tüm özel değişkenler almak nasıl ve yapı başarısız olursa, geliştiricilerin bildirmek nasıl gibi yapı hakkında bilgi sağlayan ilişkili meta verileri de vardır.

İşlerini seçerek oluşturulur **Jenkins > Yeni proje** aşağıdaki ekran görüntüsünde gösterildiği gibi sağ taraftaki üst köşedeki menüsünden:


![](jenkins-walkthrough-images/image22.png "İşleri üst sağ köşede menüsünden Jenkins yeni iş seçerek oluşturulur")

Bu görüntüler **yeni iş [Jenkins]** sayfası. İş için bir ad girin ve seçin **serbest stili yazılım projesi derleme** radyo düğmesi. Aşağıdaki ekran görüntüsü, bunun bir örneğini gösterir:

![](jenkins-walkthrough-images/image23.png "İş için bir ad girin ve yapı serbest stili yazılım proje radyo düğmesini seçin")

Tıklatarak **Tamam** düğmesi iş için yapılandırma sayfası gösterir. Aşağıdaki ekran görüntüsünde şuna benzemelidir:

![](jenkins-walkthrough-images/image24.png "Bu ekran şuna benzemelidir")

Jenkins işleri aşağıdaki yolda bulunan sabit diskinde bir dizinde düzenler: **~/.jenkins/jobs/[JOB adı]**

Bu klasör, tüm dosyaları ve yapıları işe günlükleri, yapılandırma dosyalarını ve derlenmesi gerekiyor kaynak kodu gibi belirli içerir.

İlk işi oluşturulduktan sonra bir veya daha fazlasını yapılandırılması gerekir:

 - Kaynak kodu yönetimi sistemi belirtilmelidir.
 - Bir veya daha fazla *Eylemler yapı* projeye eklenmelidir. Bu adımları veya görevleri uygulama oluşturmak için gerekli olan.
 - Bir iş atanmalıdır *tetikleyici yapı* – kodunu alabilir ve son projeyi derlemek için ne sıklıkta Jenkins bildiren yönergeler kümesi.

### <a name="configuring-source-code-control"></a>Kaynak kodu denetimi yapılandırma

Jenkins mu ilk görevi olan almak kaynak kodunu kaynak kodu yönetim sisteminden. Jenkins popüler kaynak kodu yönetimi sistemleri günümüzün çoğunu destekler. Bu bölüm iki popüler sistemleri, Git ve Team Foundation Server kapsar. Bu kaynak kodu Yönetimi sistemlerinin her biri aşağıdaki bölümlerde daha ayrıntılı olarak ele alınmıştır.

#### <a name="using-git-for-source-code-control"></a>Kaynak kodu denetimi için Git kullanma

Kaynak kodu denetimi için TFS kullanıyorsanız [atla](#Using_TFS_for_Source_Code_Management) bu bölümünde ve TFS kullanarak bir sonraki bölüme geçin.

Jenkins hiçbir ek eklentileri gerekli Git kutu dışı – destekler. Git kullanmak için tıklayın **Git** radyo düğmesinin ve aşağıdaki ekran görüntüsünde gösterildiği gibi Git deposu için URL'yi girin:

![](jenkins-walkthrough-images/image25.png "Git kullanmak için Git radyo düğmesine tıklayın ve için Git deposu URL'sini girin")

Değişiklikler kaydedildikten sonra Git yapılandırma tamamlanmış olur.

#### <a name="using-tfs-for-source-code-management"></a>Kaynak kodu yönetimi için TFS kullanma

Bu bölüm, yalnızca TFS kullanıcılar için geçerlidir.

Tıklayın **Team Foundation Server** radyo düğmesini seçin ve TFS Yapılandırma bölümü görünmelidir, aşağıdaki ekran görüntüsünde nedir benzer:


![](jenkins-walkthrough-images/image26.png "Team Foundation Server radyo düğmesine tıklayın ve TFS Yapılandırma bölümü görüntülenmelidir")

TFS için gerekli bilgileri sağlayın. Aşağıdaki ekran görüntüsünde tamamlanan formun örneği gösterilmektedir:

![](jenkins-walkthrough-images/image27.png "Bu ekran görüntüsünde tamamlanan formun örneği gösterilmektedir")

#### <a name="testing-the-source-code-control-configuration"></a>Kaynak kodu denetimi yapılandırmasını sınama

Uygun kaynak kodu denetimi yapılandırıldıktan sonra tıklatın **kaydetmek** değişiklikleri kaydedin. Bu, aşağıdaki ekran görüntüsüne benzer iş için giriş sayfasına döndürür:

![](jenkins-walkthrough-images/image28.png "Bu ekran görüntüsüne benzer iş için giriş sayfasına bu döndürür")

Belirtilen herhangi bir derleme eylem olsa bile kaynak kodu denetimi düzgün şekilde yapılandırıldığını doğrulamak için basit el ile bir derlemeyi tetiklemek yoldur. Bir yapı el ile başlatmak için işin giriş sayfasına sahip bir **şimdi yapı** bağlantı sol tarafındaki menüde aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](jenkins-walkthrough-images/image29.png "Bir yapı el ile başlatmak için sol tarafındaki menüde bir şimdi yapı bağlantı iş giriş sayfası vardır")

Bir yapı başlatıldığında yapı geçmişi iletişim yanıp sönen mavi bir daire, bir ilerleme çubuğu, yapı numarası ve yapı, aşağıdaki ekran görüntüsüne benzer başlatılışında görüntüler:

![](jenkins-walkthrough-images/image30.png "Bir yapı başlatıldığında yanıp mavi daire, bir ilerleme çubuğu, yapı numarası ve yapı başlatılışında yapı geçmişi iletişim kutusunu görüntüler")

İş başarılı olursa, mavi bir daire görüntülenir. İş başarısız olursa, kırmızı bir daire görüntülenir.

Jenkins derleme bir parçası olarak doğabilecek sorunları gidermeye yardımcı olmak için tüm iş için konsol çıktısı yakalar. Konsol çıkışı görmek için işe tıklayın **yapı geçmiş**ve ardından **konsol çıktısı** sol menüde bağlantı. Aşağıdaki ekran görüntüsü gösterildiği **konsol çıktısı** bağlantı yanı sıra bazı iş başarılı çıktısı:

![](jenkins-walkthrough-images/image31.png "Bu ekran görüntüsü, konsol çıktısı bağlantı yanı sıra bazı başarılı bir işin çıktısını gösterir")

#### <a name="location-of-build-artifacts"></a>Derleme yapıları konumu

Jenkins adlı özel bir klasöre tüm kaynak kodu alacaktır bir *çalışma*. Bu dizin, aşağıdaki konumda klasör içinde bulunabilir:

    ~/.jenkins/jobs/[JOB NAME]/workspace

Çalışma alanı yoluna adında bir ortam değişkeni depolanacak `$WORKSPACE`.

Bir iş için giriş sayfasına giderek ve üzerinde tıklatarak Jenkins çalışma klasöründe Gözat mümkündür **çalışma** sol menüde bağlantı. Aşağıdaki ekran görüntüsünde adlı bir işi için çalışma örneği gösterilmektedir **HelloWorld**:

![](jenkins-walkthrough-images/image32.png "Bu ekran HelloWorld adlı bir işi için çalışma örneği gösterilmektedir.")

### <a name="build-triggers"></a>Tetikleyiciler derleme

Jenkins derlemelerde başlatma için birkaç farklı stratejiler vardır – bunlar olarak bilinir *Tetikleyicileri yapı*. Yapı tetikleyici Jenkins bir iş başlatabilir ve projeyi derlemek karar vermenize yardımcı olur. İki daha yaygın yapı tetikleyicileri şunlardır:

- **Düzenli aralıklarla yapı** – Bu tetikleyici her iki saatte gibi belirli aralıklarla veya hafta içi günlerde gece bir işi başlatmak Jenkins neden olur. Yapı olup olmamasına bakılmaksızın başlatılır kaynak kodu deposunda değişiklikler olmuştur.
- **Yoklama SCM** – Bu tetikleyici kaynak kodu denetimi düzenli aralıklarla yoklama yapar. Kaynak kodu depoya değişiklikleri kaydedildi, Jenkins yeni bir yapı başlatılır.

Bir geliştirici ayırmak yapı neden değişiklikleri onayladığında, hızlı geri bildirim SCM yoklama popüler bir tetikleyici çünkü sağlar. Bu, bazı son tamamlanan kodu sorunlara neden ve değişiklikleri göz önünde yeni hala durumdayken sorunu gidermeye geliştiricilerinin takımlar uyarmak için yararlıdır.

Dönemsel derlemeleri genellikle dağıtılabilir sınayıcılar uygulamaya bir sürümünü oluşturmak için kullanılır. Örneğin, böylece QA takım üyeleri iş önceki haftanın test düzenli bir yapı için Cuma Akşam zamanlanmış.


### <a name="compiling-a-xamarinios-applications"></a>Xamarin.iOS uygulamaları derleme
Xamarin.iOS projeleri komut satırını kullanarak derlenmesi `xbuild` veya `msbuild`. Kabuk komutu Jenkins çalıştıran kullanıcı hesabının bağlamında yürütülür. Böylece uygulama dağıtım için düzgün şekilde paketlenmiş kullanıcı hesabı sağlama profili erişimi olduğundan önemlidir. Bu kabuk komut iş yapılandırma sayfasına eklemek mümkündür.

Ekranı aşağı kaydırarak **yapı** bölümü. Tıklatın **Ekle derleme adımı** düğmesine tıklayın ve ardından **Kabuk yürütme**tarafından aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](jenkins-walkthrough-images/image33.png "Ekle derleme adımı düğmesini tıklatın ve yürütme Kabuğu'nu seçin")


[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

### <a name="building-a-xamarinandroid-project"></a>Building a Xamarin.Android Project

Bir Xamarin.Android projesi oluşturma bir Xamarin.iOS projesi oluşturmayı çok benzer. Bir APK bir Xamarin.Android projesinde oluşturmak için Jenkins aşağıdaki iki adımdan gerçekleştirecek şekilde yapılandırılması gerekir:

 - MSBuild eklentisi kullanarak proje derleme
 - Oturum ve ZIP APK geçerli sürüm anahtar deposu ile hizalar.

Bu iki adımı sonraki iki bölümlerde daha ayrıntılı olarak ele alınacaktır.

### <a name="creating-the-apk"></a>APK oluşturma

Tıklayın **Ekle derleme adımı** düğmesine tıklayın ve ardından **Visual Studio Proje ya da MSBuild kullanma çözüm yapı**, aşağıdaki ekran görüntüsünde gösterildiği gibi:

![](jenkins-walkthrough-images/image36.png "APK tıklatın Ekle derleme adımı düğmesini ve select derleme bir Visual Studio projesi veya MSBuild kullanma çözüm oluşturma")

Derleme adımı projeye eklendikten sonra görünen form alanları doldurun. Aşağıdaki ekran görüntüsünde tamamlanan formun örneğidir:

![](jenkins-walkthrough-images/image37.png "Derleme adımı projeye eklendikten sonra görünen form alanlarını doldurun")


Bu derleme adımı yürütecek `xbuild` içinde **$WORKSPACE** klasör. MSBuild yapı dosya kümesine **Xamarin.Android.csproj** dosya. **Komut satırı bağımsız değişkenleri** hedef yayın derlemesi belirtin **PackageForAndroid**. Bu adım çarpımını bir APK olacaktır, şu konumda:

    $WORKSPACE/[PROJECT NAME]/bin/Release

Aşağıdaki ekran görüntüsü bu APK örneği gösterilmektedir:

![](jenkins-walkthrough-images/image38.png "Bu ekran görüntüsü, bu APK örneği gösterir.")

Bu APK, özel bir anahtar ile imzalanmamış ve zip hizalanmalıdır dağıtım için hazır değil.

#### <a name="signing-and-zipaligning-the-apk-for-release"></a>İmzalama ve Zipaligning sürüm APK

İmzalama ve zipaligning APK iki ayrı komut satırı araçları'ndan Android SDK'sı tarafından gerçekleştirilen teknik olarak iki ayrı görevlerdir. Ancak, bir yapı eylemi gerçekleştirmek uygundur. İmzalama ve zipaligning bir APK hakkında daha fazla bilgi için bir Android uygulaması yayın için hazırlama Xamarin'ın belgelerine bakın.

Bu komutların her ikisi proje proje değişebilir komut satırı parametreleri gerektirir. Ayrıca, bu komut satırı parametreleri yapı çalıştırırken, konsol çıktısı görünmemelidir parolaları bazılarıdır. Bu komut satırı parametreleri bazıları ortam değişkenleri depolarız. İmzalama ve/veya zip hizalamak için gerekli ortam değişkenleri aşağıdaki tabloda açıklanmıştır:

|Ortam değişkeni|Açıklama|
|--- |--- |
|KEYSTORE_FILE|Bu anahtar APK imzalama yoldur.|
|KEYSTORE_ALIAS|APK imzalamak için kullanılan bir anahtar anahtar.|
|INPUT_APK|Tarafından oluşturulan APK `xbuild`.|
|SIGNED_APK|İmzalı APK üretilen tarafından `jarsigner`.|
|FINAL_APK|Zip budur tarafından üretilen APK hizalı `zipalign`.|
|STORE_PASS|Bu dosyayı singing için anahtar deposu içeriğine erişmek için kullanılan paroladır.|

Gereksinimleri bölümünde açıklandığı gibi bu ortam değişkenleri EnvInject eklentisi kullanarak derleme sırasında ayarlanabilir. Yeni bir derleme işi olmalıdır adım eklenen temel ekleme ortam değişkenlerini sonraki ekran görüntüsünde gösterildiği gibi:

![](jenkins-walkthrough-images/image39.png "Yeni bir derleme işi olmalıdır adım eklenen temel ekleme ortam değişkenleri")

İçinde **özellikleri içerik** şu biçimde görünür alan, ortam değişkenleri eklenir, her satır, bir form:

    ENVIRONMENT_VARIABLE_NAME = value

Aşağıdaki ekran görüntüsü APK imzalamak için gerekli ortam değişkenleri gösterir:

![](jenkins-walkthrough-images/image40.png "Bu ekran APK imzalamak için gerekli ortam değişkenleri gösterir")

APK dosyaları için ortam değişkenleri bazıları üzerine kuruludur olduğunu fark `WORKSPACE` ortam değişkeni.

Anahtar deposunun içeriğine erişmek için parola son ortam değişkenidir: `STORE_PASS`. Parolalar, örtülü veya günlük dosyalarında atlanmış hassas değerlerdir. EnvInject eklentisi, böylece günlüklerinde gösterme bu değerleri korumak için yapılandırılabilir.

Hemen önce **yapı** iş yapılandırma bölümü bir **yapı ortamı** bölümü. Zaman **parolaları Ekle** onay kutusunu yükseğe, bazı form alanları görünmesi. Bu form alanları, adını ve ortam değişkeninin değerini yakalamak için kullanılır. Aşağıdaki ekran görüntüsü ekleme örneğidir `STORE_PASS` ortam değişkeni:

![](jenkins-walkthrough-images/image41.png "Bu ekran STOREPASS ortam değişkeni ekleyerek, bir örnek verilmiştir")

Ortam değişkenleri başlatıldıktan sonra sıradaki adım imzalamak için bir derleme adımı ekleme ve APK hizalama zip olacak. Ortam değişkenleri eklemek için derleme adımı başka hemen olacaktır sonra **Kabuk yürütme** yürütecek komut yapı `jarsigner` ve `zipalign`. Her komut aşağıdaki kod parçacığında gösterildiği gibi bir satır yukarı gösterecektir:


    jarsigner -verbose -sigalg MD5withRSA -digestalg SHA1 -keystore $KEYSTORE_FILE -storepass $STORE_PASS -signedjar $SIGNED_APK $INPUT_APK $KEYSTORE_ALIAS
    zipalign -f -v 4 $SIGNED_APK $FINAL_APK

Aşağıdaki ekran görüntüsü girmek nasıl bir örneği gösterir `jarsigner` ve `zipalign` adım içine komutlar:

![](jenkins-walkthrough-images/image42.png "Bu ekran jarsigner ve zipalign komutları adımla girmek nasıl bir örneği gösterir")

Tüm yapı eylemleri yerinde olduktan sonra her şeyin çalıştığını doğrulamak için el ile bir derlemeyi tetiklemeyi iyi bir uygulamadır. Derleme başarısız olursa, **konsol çıktısı** ne yapılandırmanın başarısız olmasına neden hakkında bilgi için gözden geçirilmesi gerekir.

### <a name="submitting-tests-to-test-cloud"></a>Testleri Test buluta gönderiliyor

Otomatikleştirilmiş testler için Test Kabuk komutları kullanarak bulut gönderilebilir. Bir Test çalışmasında Xamarin Test Cloud ayarlama hakkında daha fazla bilgi için Kılavuzlar kullanmak için sahip olduğumuz [Xamarin.UITest](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/) veya [Calabash](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).


## <a name="summary"></a>Özet

Bu kılavuzdaki biz Jenkins Mac OS X üzerinde bir yapı sunucu olarak sunulan ve derlemek ve Xamarin mobil uygulamalar yayın için hazırlamak için yapılandırılmış. Derleme işlemi desteklemek için birden fazla eklenti ile birlikte bir Mac OS X bilgisayarda Jenkins yüklü. Biz oluşturulur ve kod TFS veya Git çekme ve sonra bu kodu bir yayın hazır uygulamasına derleme bir işi yapılandırılmış. Biz de işleri çalıştırılması gereken zaman zamanlamak için iki farklı şekilde incelediniz.

## <a name="related-links"></a>İlgili bağlantılar

- [Sürekli Tümleştirme](https://developer.xamarin.com~/testcloud/ci/)
- [Xamarin Test Cloud testleri gönderiliyor](https://developer.xamarin.com~/testcloud/submitting/)
- [Neden Jenkins Xamarin desteklenmez?](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md)
