---
title: Xamarin ile sürekli tümleştirme giriş
description: Bu belgede, Xamarin ile sürekli tümleştirme açıklanmaktadır. Sürüm denetimi ve sürekli tümleştirme ortamlarla açıklanır.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: topgenorth
ms.author: toopge
ms.date: 07/19/2017
ms.openlocfilehash: 67fc32fc9f79d54274642fbab2d0c2f8afd14d8c
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066513"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin ile sürekli tümleştirme giriş

_Sürekli Tümleştirme otomatik derleme derler ve isteğe bağlı olarak kod eklendiğinde veya projenin sürüm denetimi deponuza geliştiriciler tarafından değiştirilen bir uygulama testleri bir yazılım mühendislik uygulamadır. Bu makalede, sürekli tümleştirme genel kavramlar ve bazı seçenekleri sürekli tümleştirme için Xamarin projelerle ele alınacaktır._

Paralel olarak çalışmak geliştiricilere yönelik yazılım projelerde yaygın bir durumdur. Belirli bir noktada, bunların tümü tümleştirmek gerekli olan tek bir iş akışları paralel codebase son ürün yapan. Yazılım geliştirme erken gün içinde bir proje zor ve riskli işlem sonunda bu tümleştirme gerçekleştirildi.

Sürekli Tümleştirme (CI) her geliştiricinin değişiklikleri sürekli olarak ortak kod temeli Birleştirmekte tarafından gibi karmaşıklıkları kaçınmak için tüm geliştiriciler genellikle etmeden her projenin yapılan değişiklikler kod deposu paylaşılan. Her iade otomatik derleme tetikler ve varolan tüm kodlar yeni sunulan kodu bölün alamadık doğrulamak için otomatikleştirilmiş testleri çalıştırır.  Bu şekilde CI hatalarını ve sorunlarını hemen ortaya çıkarır ve tüm ekip üyelerinin her başkalarıyla güncel iş kalmasını sağlar. Bu bağlı ve kararlı bir codebase sonuçlanır.

Sürekli Tümleştirme sistemleri iki ana bölümden oluşur:

-  **Sürüm denetimi** – sürüm denetimi (kaynak denetimi veya kaynak kodu Yönetimi olarak da bilinir VC), bir projenin kod tümünün tek bir paylaşılan depoya birleştirir ve her dosyaya her değişiklik tam geçmişini tutar. Bu depo genellikle olarak bilinir *mainline* veya *ana* dal, sonuçta üretim oluşturabilir veya uygulamanın sürüm yayın için kullanılacak kaynak kodunu içerir. Birçok açık kaynak ve genellikle ekipleri ve kapsamlı değişiklik açabilecekleri ana dala riski olmadan denemeler gerçekleştir ikincil dalları içine kod bir kopyasını çatallaştırma bireylere izin bu görev için ticari ürünler vardır. İkincil bir dal değişiklikleri doğrulandığı sonra bunlar daha sonra ana dala birleştirilmiş hepsini bir araya olabilir.
-  **Sürekli tümleştirme sunucusu** – sürekli tümleştirme Server tüm bir projenin yapılarının (kaynak kodu, görüntüler, videoları, veritabanları, otomatikleştirilmiş testleri, vb.) toplama, uygulama derlemek ve otomatikleştirilmiş testleri çalıştırma sorumlu. Yeniden, birçok açık kaynak ve ticari CI sunucu araçları vardır.

Geliştiriciler genellikle bir veya daha fazla dalları çalışan bir kopyasını iş başlangıçta burada yapılır kendi istasyonlarında var. Uygun bir çalışma kümesini tamamlandıktan sonra değişiklikleri "içine işaretli" veya "diğer geliştiriciler çalışma kopyalarını yayar uygun dal kaydedilen". Bir takım tüm aynı kod çalışmakta olduğunuz sağlar nasıl budur.

Yeniden sürekli tümleştirme ile değişiklikleri teslim etme eylemi projeyi oluşturun ve kaynak kodu doğruluğunu doğrulamak için otomatikleştirilmiş testleri çalıştırma CI neden olur. Derleme hataları veya test hatalar varsa, bir CI sunucu sorumlu Geliştirici bildirir (e-posta aracılığıyla, anlık ileti, Twitter, Growl, vb.) şekilde çözemiyorsa sorunu düzeltebilirsiniz. (Bir "iade geçişli" adlı hata varsa CI sunucuları bile yürütme reddedebilir.)

Aşağıdaki diyagram bu işlemi gösterilmektedir:

[![](intro-to-ci-images/intro01-small.png "Bu diyagramda bu işlem gösterilir")](intro-to-ci-images/intro01.png#lightbox)

Mobil uygulamaları için sürekli tümleştirme benzersiz zorlukları tanıtır. Uygulamalar, yalnızca fiziksel aygıtlarda kullanılabilir algılayıcılar GPS veya kamera gibi gerektirebilir. Ayrıca, benzeticileri veya Öykünücüler yalnızca yaklaşık donanım ve Gizle veya sorunları soyutlamaması. Sonunda, mobil uygulama gerçekten müşteri hazır olduğundan emin olarak gerçek donanım üzerinde test etmek gereklidir.

[Uygulama Center Test](https://docs.microsoft.com/appcenter/test-cloud) doğrudan fiziksel aygıtların yüzlerce uygulamaları test ederek belirli bu sorunu giderir. Geliştiriciler için güçlü bir UI testi izin otomatik kabul testleri yazma. Bu testler uygulama merkezine yüklendiğinde, CI sunucunun bunları otomatik olarak bir CI işleminin bir parçası aşağıdaki çizimde gösterildiği gibi çalıştırabilirsiniz:

[![](intro-to-ci-images/intro02-small.png "Bu testler uygulama merkezine yüklendiğinde, CI sunucu bunları otomatik olarak bir CI işleminin bir parçası Bu diyagramda gösterildiği gibi çalıştırabilir")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Sürekli Tümleştirme Bileşenleri

Ticari ve açık kaynaklı araçları CI desteklemek için tasarlanmış kapsamlı bir ekosistemi vardır. Bu bölümde, en yaygın olanlara birkaç açıklanmaktadır.

## <a name="version-control"></a>Sürüm Denetimi

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services ve Team Foundation Server

[Visual Studio Team Services](https://visualstudio.microsoft.com/team-services/) (VSTS) ve [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) olan derleme Microsoft'un işbirliği araçları sürekli tümleştirme hizmetleri, izleme görev, Çevik planlama ve raporlama araçlarını ve sürüm Denetim. Sürüm denetimi ile VSTS ve TFS (Team Foundation sürüm denetimi veya TFVC'yi) kendi sistem veya GitHub üzerinde barındırılan projeleri ile çalışabilir.

 - Visual Studio Team Services aracılığıyla bulut hizmetleri sağlar. Herhangi bir ayrılmış donanım veya altyapı gerektirir ve herhangi bir yere Visual Studio, coğrafi olarak olan takımlar için cazip hale getirme gibi popüler geliştirme araçları ve web tarayıcıları aracılığıyla erişilebilen, birincil avantajı olmasıdır Dağıtılmış. Beş geliştirici ekipleri için boş ya da daha az, büyüyen bir takım uyum sağlamak için hangi ek lisanslar satın sonra.
 - TFS şirket içi Windows sunucuları için tasarlanmış ve yerel ağ veya bu ağa bir VPN bağlantısı üzerinden erişilebilir. Birincil avantajı, tam olarak yapı sunucularının yapılandırmasını denetlemek ve herhangi bir ek yazılım veya hizmetleri gerekli yükleyebilirsiniz ' dir. TFS küçük ekipleri için ücretsiz giriş seviyesi Express sürümü vardır.

TFS ve VSTS Visual Studio ile sıkı bir şekilde tümleştirilmiştir ve birçok sürüm denetimi ve tek bir IDE rahatlık CI görevleri gerçekleştirmek geliştiricilerin olanak sağlar. Takım Gezgini her yerde eklentisi Eclipse (aşağıya bakın) için de kullanılabilir. Mac için Visual Studio TFS veya VSTS için herhangi bir destek sağlamaz.

Visual Studio Team hizmetin yapı sistem (Android, iOS ve Windows) hedef istediğiniz her platform için bir yapı tanımı içinde oluşturduğunuz Xamarin projeleri için doğrudan desteğe sahiptir. Uygun Xamarin lisans her derleme tanımı için gereklidir. Yerel bağlanmak mümkündür, bu amaç için Visual Studio Team Services sunucusuna Xamarin özellikli TFS derlemesi. Bu kurulum ile VSTS sıraya alınan derlemeleri yerel sunucuya verilmiş. Ayrıntılar için başvurmak [dağıtma ve bir yapı sunucusunu yapılandırmak](https://docs.microsoft.com/vsts/pipelines/agents/agents?view=vsts). Alternatif olarak, Jenkins veya takım şehir gibi başka bir yapı aracını kullanabilirsiniz.

Visual Studio, Visual Studio Team Services ve Team Foundation Server, bkz: tüm uygulama yaşam döngüsü yönetimi (ALM) özelliklerini tam özeti [Xamarin uygulamalarıyla uygulama yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/mt162217(v=vs.140).aspx) konusuna bakın.


### <a name="team-explorer-everywhere"></a>Takım Gezgini Her Yerde

[Takım Gezgini her yerde](http://msdn.microsoft.com/library/gg413285.aspx) dışında Visual Studio geliştirme ekipleri için Team Foundation Server ve Visual Studio Team Services gücünü getirir. Takım projeleri şirket içinde veya bulutta Eclipse ya da platformlar arası komut satırı istemci OS X ve Linux için bağlanmak geliştiricilerin sağlar. Takım Gezgini her yerde tam sağlar (Git dahil), sürüm denetimi için iş öğeleri erişebilir ve özellikleri Windows olmayan platformları için derleme.


### <a name="git"></a>Git

[Git](http://git-scm.com) Linux çekirdek için kaynak kodunu yönetmek için başlangıçta geliştirilmiş bir popüler açık kaynak sürüm denetim çözümü olup. Bu yazılım ölçekteki projelerle popüler olan çok hızlı ve esnek bir sistemdir. Bunu kolayca zayıf Internet erişimi olan tek geliştiricilerden dünya span büyük takıma ölçeklendirir. Git hangi sırayla minimum risk ile geliştirme paralel akışları teşvik dallanma çok kolay hale getirir.

Bir web tarayıcısı aracılığıyla tamamen veya aracılığıyla Git çalışabilir [GUI istemcileri](http://git-scm.com/downloads/guis) Linux, Mac OSX ve Windows çalıştıran. Ortak depoları için ücretsizdir; Özel depoları gerektiren bir [planı Ücretli](https://github.com/pricing).

Visual Studio 2015 ve Visual Studio Mac için Git için yerel destek sağlar; eski sürümleri için Microsoft sağlayan bir [Git indirilebilir uzantısı](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Yukarıda belirtildiği gibi Visual Studio Team Services ve TFS Git sürüm denetimi TFVC'yi yerine kullanabilirsiniz.


### <a name="subversion"></a>Alt sürüme

[Alt sürüme](http://subversion.apache.org) (SVN) 2000'den itibaren kullanımda olan bir popüler, açık kaynaklı sürüm denetimi sistemi değil. SVN OS X, Windows, FreeBSD, Linux ve UNIX tüm modern sürümlerinde çalışır. Mac için Visual Studio SVN için yerel destek vardır. Visual Studio için SVN destek Getir üçüncü taraf uzantılar vardır.


## <a name="continuous-integration-environments"></a>Sürekli Tümleştirme ortamları

Sürekli Tümleştirme ortamını ayarlama yapı hizmeti ile bir sürüm denetim sistemi birleştirmek anlamına gelir.  İkincisi, iki en yaygın olanlardır:

- [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) Visual Studio Team Services ve TFS yapı sistemidir. Geliştiricilerin tetiklemek için kullanışlı bir onu derlemeleri, hangi yapar otomatik testleri çalıştırma ve sonuçları görmek ile Visual Studio, tümleşiktir.
- Jenkins sunucusudur bir açık kaynak CI yazılım geliştirme her türlü desteklemek için eklentileri zengin ekosistemi ile. Windows üzerinde çalışır ve Mac OS x Jenkins herhangi belirli IDE ile tümleşik değildir. Bunun yerine, bu yapılandırılmış ve bir web arabirimi yönetilir. Jenkins CI yüklemek ve hangi küçük ekipleri çekici yapar yapılandırmak de kolaydır.

TFS/VSTS tek başına kullanabilir veya Jenkins TFS/VSTS veya aşağıdaki bölümlerde açıklandığı gibi Git ile birlikte kullanabilirsiniz.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services ve Team Foundation Server

Konusunda anlatıldığı gibi Visual Studio Team Services ve Team Foundation Server sağlayan her iki sürümü denetlemek ve Hizmetleri oluşturun. Yapı Hizmetleri, her hedef platform için her zaman Xamarin iş veya kurumsal bir lisans gerektirir.

Visual Studio Team Services ile her hedef platformu için ayrı yapı tanımı oluşturun ve uygun lisans girin. Yapılandırdıktan sonra VSTS derlemeleri çalışacak ve bulut testlerinde. Bkz: [Team Foundation Build](https://msdn.microsoft.com/Library/vs/alm/Build/overview) daha fazla ayrıntı için.

Team Foundation Server ile bir yapı makine belirli hedef platformlar için aşağıdaki gibi yapılandırın:

- **Android ve Windows:** yükleme Visual Studio ve Xamarin araçlarını (Android ve Windows için her iki) ve Xamarin lisanslarınızı yapılandırmak. Burada Aracısı TFS derlemesi paylaşılan bir konuma sunucuda Android SDK bulabileceğiniz taşımak gereklidir. Ayrıntılar için bkz [yapılandırma TFVC'yi](https://docs.microsoft.com/vsts/tfvc/overview).
- **iOS ve Xamarin:** yükleme Visual Studio ve Xamarin araçları uygun lisans ile Windows Server'da. Ardından Visual Studio Mac için yapı ana bilgisayarı olarak hizmet ve son uygulama paketi (IPA iOS, OS X için uygulama) oluşturma ağ üzerinden erişilebilen bir Mac OS X makineye yükleyin.

Aşağıdaki diyagram bu topografi gösterir:

[![](intro-to-ci-images/intro03-small.png "Bu diyagramda bu topografi gösterilir")](intro-to-ci-images/intro03.png#lightbox)

VSTS derlemeleri yerel sunucuya verilmiş bir Visual Studio Team Services projesi için yerel bir TFS sunucusu bağlayın mümkündür. Ayrıntılar için bkz [dağıtma ve bir yapı sunucusunu yapılandırmak](http://msdn.microsoft.com/library/ms181712.aspx) konusuna bakın.

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services ve Jenkins

Uygulamalarınızı oluşturmak için Jenkins kullanırsanız, Visual Studio Team Services veya Team Foundation Server kodunuzu depolamak ve Jenkins CI derlemeleriniz için kullanılacak devam edebilirsiniz. Takım projenizin Git deposu veya kod TFVC'yi için ne zaman iade kodu bastığınızda Jenkins yapı tetikleyebilir. Ayrıntılar için bkz [Jenkins Visual Studio Team Services ile](https://docs.microsoft.com/en-us/vsts/service-hooks/services/jenkins?view=vsts).

[![](intro-to-ci-images/intro04-small.png "Uygulamalarınızı oluşturmak için Jenkins kullanırsanız, Visual Studio Team Services veya Team Foundation Server kodunuzu depolamak ve Jenkins CI derlemeleriniz için kullanmaya devam")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git ve Jenkins

Başka bir ortak CI ortamını tamamen OS X dayalı olabilir. Bu senaryo, Git kaynak kodu denetimi ve Jenkins için yapı sunucusu kullanmakla ilgilidir. Bunların her ikisi de Visual Studio ile tek bir Mac OS X bilgisayarda yüklü Mac için çalışıyor. Bu Visual Studio Team Services + önceki bölümde tartışılan Jenkins ortamı için çok benzer.

[![](intro-to-ci-images/intro05-small.png "Bu Visual Studio Team Services + önceki bölümde tartışılan Jenkins ortamı için çok benzer")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Not: Jenkins olan [Xamarin tarafından desteklenmeyen](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**


# <a name="summary"></a>Özet

Bu belge sürekli tümleştirme kavramını ve yazılım geliştirme ekiplerinin getirir avantajları sunar. Sürüm denetimi önemini rolü ve sorumlulukları yapı server'ın yanı sıra açıklanmıştır. Belgeyi daha sonra kaynak kodu denetimi için kullanım ve sunucuları yapı araçlardan bazıları tartışmak için geçti. Ayrıca kalitesini ve uygulamalarını işlevselliğini kanıtlamak otomatikleştirilmiş testleri çalıştırarak harika uygulamaları yayımlamak geliştiriciler yardımcı olan uygulama Center Test sunulmuştur. Uygulamalar ve uygulama merkezi testleri bulunabilir gönderme üzerinde belgeleri ayrıntılı [burada](https://docs.microsoft.com/appcenter/test-cloud). Son olarak, anlamanıza yardımcı olması için bu araçları ve bileşenler bir araya getireceğinizi nasıl biz ana hatlarıyla kuruluşlar için sürekli tümleştirme oluşturabilirsiniz birkaç farklı CI ortamlarda. Visual Studio Team Services ve Team Foundation Server Xamarin projelerle kullanarak daha fazla bilgi için bkz: [yapılandırma TFVC'yi](https://docs.microsoft.com/vsts/tfvc/overview) ve bu [sürekli tümleştirme giriş](https://docs.microsoft.com/vsts/build-release/actions/ci-cd-part-1). Benzer şekilde, Jenkins kullanıyorsanız, bkz: [kullanarak Jenkins xamarin'le](~/tools/ci/jenkins-walkthrough.md) sürekli tümleştirme kurulumu hakkında ayrıntılı bilgi için.
