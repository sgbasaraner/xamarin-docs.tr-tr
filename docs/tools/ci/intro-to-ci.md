---
title: Xamarin ile Sürekli Tümleştirmeye Giriş
description: Bu belgede, Xamarin ile sürekli tümleştirme açıklanmaktadır. Sürüm denetimi ve sürekli tümleştirme ortamlarla ele alınmaktadır.
ms.prod: xamarin
ms.assetid: C034200E-2947-4309-9DDD-80DAC505C43F
author: lobrien
ms.author: laobri
ms.date: 07/19/2017
ms.openlocfilehash: 5468495885e3af2afa2692ccad9191b669fa3328
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "37066513"
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin ile Sürekli Tümleştirmeye Giriş

_Sürekli tümleştirme, otomatik bir yapı derler ve kod eklendiğinde veya projenin sürüm denetimi deposu geliştiricilerin değiştiren isteğe bağlı olarak bir uygulama testleri bir yazılım Mühendisliği uygulamadır. Bu makalede, sürekli tümleştirme genel kavramlar ve bazı seçenekleri kullanılabilir sürekli tümleştirme için Xamarin projeleriyle ele alınacaktır._

Paralel olarak çalışması, geliştiriciler için yazılım projeleri üzerinde yaygındır. Belirli bir noktada, bunların tümünün tümleştirmek gerekli paralel bir iş akışlarını son ürün yapan kod tabanı. Yazılım geliştirme erken gün içinde bu tümleştirme zor ve riskli işlemi bir Proje sonunda gerçekleştirildi.

Sürekli Tümleştirme (CI), sürekli olarak ortak kod tabanının her geliştiricinin değişiklikleri Birleştirmekte tarafından böyle karmaşıklık kaçınmak için tüm geliştiriciler genellikle iade her kod deposu projenin değişiklikleri paylaşılan. Her iade otomatik bir yapı tetikler ve yeni çıkan kod mevcut kodlar sonu olmadı doğrulamak için otomatik testler çalıştırır.  Bu şekilde, CI, hataları ve sorunları hemen ortaya çıkarır ve tüm takım üyeleri her başkalarıyla güncel iş kalmasını sağlar. Bu, bir bağlı ve kararlı kod temelinde sonuçlanır.

Sürekli Tümleştirme sistemleri iki ana bölümden oluşur:

- **Sürüm denetimi** – sürüm denetimi (kaynak denetimi veya kaynak kodu Yönetimi olarak da anılır VC), bir projenin kodunu tümünün tek bir paylaşılan depoya birleştirir ve her dosya için her değişiklik tam geçmişini tutar. Bu depo sık olarak adlandırılan *mainline* veya *ana* dal, en sonunda üretim uygulamanın sürüm yayını mı kullanılacak kaynak kodunu içerir. Birçok açık kaynak ve genellikle takımlar veya nerede bunlar kapsamlı değişiklik yapabilir veya ana dala riski olmadan denemeler gerçekleştirin ikincil dalları içine kod bir kopyasını çatal sağlayabilmesine olanak ticari ürünleri hakkında bilgi için bu görev vardır. İkincil bir daldaki değişiklikleri doğrulandıktan sonra bunlar daha sonra tekrar ana dalla hep birlikte olabilir.
- **Sürekli Tümleştirme sunucu** – sürekli tümleştirme sunucusudur tüm proje yapıtları (kaynak kodu, resimler, videolar, veritabanları, otomatik testler, vb.) toplamak, uygulama derlemek ve otomatik testler çalıştırmak için sorumlu. Yeniden birçok açık kaynak ve ticari CI sunucu araçları vardır.

Geliştiricileri, genellikle bir veya daha fazla dalı çalışan bir kopyasını nerede iş başlangıçta yapılır kendi istasyonlarında sahiptir. Uygun bir çalışma kümesi tamamlandıktan sonra değişiklikleri "olarak işaretli" veya "bunları diğer geliştiriciler çalışan kopyalarını yayar uygun dalına işlendi". Takım tüm aynı kod üzerinde çalışmakta olduğunuz sağlar nasıl budur.

Değişiklikler işleniyor eylemi yeniden projeyi oluşturun ve kaynak kodu doğruluğunu doğrulamak için otomatikleştirilmiş testler CI server ile sürekli tümleştirme, neden olur. Derleme hataları veya test hataları varsa, bir CI sunucusu sorumlu Geliştirici bildirir (aracılığıyla e-posta, anlık ileti, Twitter, Growl, vb.) için izinli sorunu düzeltebilirsiniz. (Bir "Geçitli iade" olarak adlandırılan bir hata varsa CI sunucuları bile işleme reddedebilir.)

Aşağıdaki diyagram bu işlemi göstermektedir:

[![](intro-to-ci-images/intro01-small.png "Bu işlem bu diyagramda gösterilmektedir")](intro-to-ci-images/intro01.png#lightbox)

Mobil uygulamalar, sürekli tümleştirme için benzersiz zorluklar tanıtır. Uygulamalar yalnızca fiziksel cihazlarda kullanılabilen sensörlerden kamera ve GPS gibi gerektirebilir. Ayrıca, simülatörleri veya öykünücüleri yalnızca donanım yaklaşık olan ve Gizle veya sorunları gizlememeniz. Sonunda gerçekten müşteri hazır olduğundan emin olarak gerçek donanım üzerinde bir mobil uygulama testi gereklidir.

[App Center Test](https://docs.microsoft.com/appcenter/test-cloud) uygulamalarını doğrudan fiziksel cihazlar yüzlerce test ederek belirli bu sorunu çözüyor. Geliştiriciler için güçlü bir UI testi sağlayan otomatik edenleri testler yazın. Bu testleri App Center'a yüklendiğinde, CI sunucunun bunları otomatik olarak bir CI işleminin bir parçası Aşağıdaki diyagramda gösterildiği gibi çalıştırabilirsiniz:

[![](intro-to-ci-images/intro02-small.png "Bu testleri App Center'a yüklendiğinde, CI sunucu bunları otomatik olarak bir CI işleminin bir parçası Bu diyagramda gösterildiği gibi çalıştırabilir")](intro-to-ci-images/intro02.png#lightbox)

# <a name="components-of-continuous-integration"></a>Sürekli Tümleştirme Bileşenleri

CI desteklemek için tasarlanan ticari ve açık kaynaklı araçları geniş bir ekosistemi vardır. Bu bölümde, en yaygın olanlarından bazıları açıklanmaktadır.

## <a name="version-control"></a>Sürüm Denetimi

### <a name="azure-devops-and-team-foundation-server"></a>Azure DevOps ve Team Foundation Server

[Azure DevOps](https://azure.microsoft.com/services/devops/) ve [Team Foundation Server](https://visualstudio.microsoft.com/tfs/) (TFS) olan sürekli tümleştirme için Microsoft'un işbirliği araçları, hizmetleri, izleme görev, Çevik planlama ve Raporlama araçları ve sürüm denetimi oluşturun. Azure DevOps ve TFS ile sürüm denetimi, kendi sistemi (Team Foundation sürüm denetimini veya TFVC) veya GitHub üzerinde barındırılır projeleri ile çalışabilirsiniz.

- Visual Studio Team Services ile bulut hizmetleri sağlar. Ayrılmış donanım ya da altyapı gerektirir ve herhangi bir Visual Studio, coğrafi olarak olan takımlar için cazip hale getirme gibi popüler geliştirme araçlarına ve web tarayıcısı aracılığıyla erişilebilen, birincil avantajı olduğu Dağıtılmış. Bu beş geliştiriciden oluşan ekipler için ücretsiz veya daha az, büyüyen bir ekibiniz uyum sağlamak için Ek lisanslar satın alınabilir sonra olur.
- TFS şirket içi Windows sunucuları için tasarlanmış ve yerel ağ veya bu ağa bir VPN bağlantısı üzerinden erişilebilir. Birincil avantajı, tam olarak yapı sunucularını'nın yapılandırmasını kontrol ve seçtiğiniz ek yazılım veya hizmetleri gerekli yükleyebilirsiniz ' dir. TFS, küçük takımlar için ücretsiz bir giriş düzeyi Express sürümü vardır.

Hem TFS'de hem de Azure DevOps Visual Studio ile sıkı bir şekilde tümleştirilmiştir ve birçok sürüm denetimi ve tek bir IDE konforuyla CI görevlerini gerçekleştirmek geliştiricilerin olanak tanır. Team Explorer Everywhere eklentisi Eclipse (aşağıya bakın) için de kullanılabilir. Mac için Visual Studio'nun [TFVC kullanılabilir önizlemesini](/visualstudio/mac/tf-version-control/).

[Azure DevOps işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/languages/xamarin/) (Android, iOS ve Windows) hedefine istediğiniz her platform için bir yapı tanımı içinde oluşturduğunuz Xamarin projeleri için doğrudan desteğe sahiptir. Her derleme tanımı için uygun Xamarin lisansı gereklidir. Yerel bir bağlanmak mümkündür, bu amaç için Xamarin özellikli TFS yapı sunucusu Azure DevOps için. Bu kurulum ile Azure DevOps sıraya derlemeleri yerel sunucuya verilmiş. Ayrıntılar için başvurmak [derleme ve yayın aracıları](https://docs.microsoft.com/azure/devops/pipelines/agents/agents). Alternatif olarak, Jenkins veya takım şehir gibi başka bir derleme aracını kullanabilirsiniz.

Visual Studio, Azure DevOps ve Team Foundation Server, bkz: uygulama yaşam döngüsü yönetimi (ALM) özelliklerinin tümünü tam özeti [Xamarin uygulamaları ile DevOps](https://docs.microsoft.com/visualstudio/cross-platform/application-lifecycle-management-alm-with-xamarin-apps).

### <a name="team-explorer-everywhere"></a>Takım Gezgini Her Yerde

[Team Explorer Everywhere](https://docs.microsoft.com/azure/devops/java/download-eclipse-plug-in/) Team Foundation Server ve Visual Studio Team Services gücünü Visual Studio'nun dışında geliştiren takımlara getiriyor. Bu hizmet sayesinde geliştiriciler, şirket içi veya buluttaki takım projelerine Eclipse veya platformlar arası komut satırı istemcisinden OS X ve Linux için bağlanmak. Tam sağlar Team Explorer Everywhere sürüm denetimi (Git dahil), iş öğelerine erişim ve Windows dışı platformların yapı özelliklerine.

### <a name="git"></a>Git

[Git](http://git-scm.com) , Linux çekirdeğinin için kaynak kodu yönetmek için geliştirilmiştir popüler açık kaynak sürüm denetimi çözümü. Bu, her boyuttaki yazılım projelerinde ile popüler çok hızlı, esnek bir sistemdir. Bunu kolayca kötü Internet erişimi olan tek geliştiricilerden dünyayı kapsayan büyük takımlara ölçeklendirir. Git hangi sırayla geliştirmenin en az risk ile paralel akışları teşvik edebilir dallanma çok kolay hale getirir.

Git, bir web tarayıcısı aracılığıyla tamamen veya ile çalışabilir [GUI istemcileri](http://git-scm.com/downloads/guis) , Linux, Mac OSX ve Windows üzerinde çalıştırın. Ortak depoları için ücretsizdir; özel depo gerektiren bir [Ücretli planı](https://github.com/pricing).

Visual Studio 2015 ve Mac için Visual Studio Git için yerel destek sağlar; Eski sürümler için Microsoft'un sağladığı bir [Git için indirilebilir uzantısı](http://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c). Yukarıda belirtildiği gibi Visual Studio Team Services ve TFS Git sürüm denetimi TFVC yerine kullanabilirsiniz.

### <a name="subversion"></a>Subversion

[Subversion](http://subversion.apache.org) (SVN) olduğundan, 2000'den itibaren kullanımda olan popüler, açık kaynaklı bir sürüm denetim sistemidir. SVN OS X, Windows, FreeBSD, Linux ve UNIX, tüm modern sürümlerinde çalışır. Mac için Visual Studio SVN için yerel destek sunar. SVN destek Visual Studio'ya taşıyın üçüncü taraf uzantılar vardır.

## <a name="continuous-integration-environments"></a>Sürekli Tümleştirme ortamları

Sürekli Tümleştirme ortamını ayarlama, bir sürüm denetim sistemi ile derleme hizmeti birleştirme anlamına gelir.  İkincisi, iki en yaygın olanlarından şunlardır:

- [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/) Azure DevOps ve TFS derleme sistemidir. Ayrıca, görsel hale getirir, geliştiricilerin tetiklemek için kullanışlı, derlemeler, otomatik olarak testleri çalıştırmak ve sonuçları görmek Studio ile sıkı bir şekilde tümleşiktir.
- Jenkins, bir açık kaynak CI server, yazılım geliştirme tüm türleri desteklemek için eklentileri oluşan zengin bir ekosistem ile. Windows üzerinde çalışır ve Mac OS x Jenkins herhangi belirli bir IDE ile tümleşik değil. Bunun yerine, yapılandırılır ve bir web arabirimi üzerinden yönetilir. Jenkins CI yükleme ve yapılandırma, küçük takımlar için çekici yapar de kolaydır.

TFS/Azure DevOps tek başına kullanabilir veya Jenkins TFS/Azure DevOps veya Git ile birlikte aşağıdaki bölümlerde açıklandığı gibi kullanabilirsiniz.

### <a name="visual-studio-team-services-and-team-foundation-server"></a>Visual Studio Team Services ve Team Foundation Server

Açıklandığı gibi Visual Studio Team Services ve Team Foundation Server sağlayan her iki sürümü denetlemek ve hizmetler oluşturun. Derleme Hizmetleri, her hedef platform için her zaman bir Xamarin Business veya Enterprise Lisansı gerektirir.

Visual Studio Team Services ile her hedef platformu için ayrı bir yapı tanımı oluşturma ve uygun lisans girin. Yapılandırılmış, Azure DevOps sonra çalışma oluşturur ve bulutta test eder. Bkz: [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/) daha fazla ayrıntı için.

Team Foundation Server ile bir yapı makinesi belirli hedef platformlar için aşağıdaki gibi yapılandırın:

- **Android ve Windows:** yükleme Visual Studio ve Xamarin Araçları (Android ve Windows için her iki) ve Xamarin lisanslarınızı yapılandırın. Android SDK'sı burada TFS yapı aracısı sunucudaki paylaşılan bir konuma bulabileceğiniz taşımak gereklidir. Ayrıntılar için bkz [yapılandırma TFVC](https://docs.microsoft.com/azure/devops/repos/tfvc/overview).
- **iOS ve Xamarin:** yükleme Visual Studio ve Xamarin araçları uygun lisansa sahip Windows server üzerinde. Ardından bir derleme konağı işlevi görecek ve son uygulama paketi (IPA iOS, OS X için uygulama) oluşturma ağ üzerinden erişilebilen bir Mac OS X makinede Mac için Visual Studio'yu yükleyin.

Aşağıdaki diyagram bu topografi gösterir:

[![](intro-to-ci-images/intro03-small.png "Bu diyagramda bu topografi gösterilir.")](intro-to-ci-images/intro03.png#lightbox)

Visual Studio Team Services proje Azure DevOps oluşturur, böylece bir temsilci yerel sunucuya yerel bir TFS sunucuya bağlanmak mümkündür. Ayrıntılar için bkz [derleme ve yayın aracıları](https://docs.microsoft.com/azure/devops/pipelines/agents/agents/).

### <a name="visual-studio-team-services-and-jenkins"></a>Visual Studio Team Services ve Jenkins

Uygulamalarınızı oluşturmayı Jenkins kullanırsanız, kodunuzu Visual Studio Team Services veya Team Foundation Server depolamak ve Jenkins CI derlemelerinize yönelik kullanmaya devam edebilirsiniz. Takım projenizin Git deposu veya ne zaman, kod TFVC'ye iade kod gönderdiğinizde bir Jenkins derlemesi tetikleme yapılmasını sağlayabilirsiniz. Ayrıntılar için bkz [Jenkins ile Azure DevOps](https://docs.microsoft.com/azure/devops/service-hooks/services/jenkins).

[![](intro-to-ci-images/intro04-small.png "Uygulamalarınızı oluşturmayı Jenkins kullanırsanız, kodunuzu Visual Studio Team Services veya Team Foundation Server depolayın ve Jenkins CI derlemelerinize yönelik kullanmaya devam edin")](intro-to-ci-images/intro04.png#lightbox)

### <a name="git-and-jenkins"></a>Git ve Jenkins

Başka bir ortak CI ortama tamamen OS X bağlı olabilir. Bu senaryo için yapı sunucusuna kaynak kodu denetimi ve Jenkins Git kullanarak içerir. Bunların her ikisi de Visual Studio ile tek bir Mac OS X bilgisayara yüklü Mac için çalışıyor. Bu Visual Studio Team Services + Jenkins ortamı önceki bölümde açıklanan çok benzer.

[![](intro-to-ci-images/intro05-small.png "Bu Visual Studio Team Services + Jenkins ortamı önceki bölümde açıklanan çok benzer")](intro-to-ci-images/intro05.png#lightbox)

> [!IMPORTANT]
> **Jenkins [Microsoft tarafından desteklenmeyen](~/cross-platform/troubleshooting/questions/xamarin-jenkins.md).**

# <a name="summary"></a>Özet

Bu belge kavramı, sürekli tümleştirme ve yazılım geliştirme ekipleriyle getirdiği avantajları kullanıma sunuldu. Sürüm denetimi önemini rolü ve sorumlulukları yapı sunucusunun yanı sıra açıklanmıştır. Belgeyi daha sonra kullanmak için kaynak kodu denetimi ve yapı sunucularınız araçlardan bazıları tartışmak için oluştu. Ayrıca geliştiricilerin kalite ve işlevsellik uygulamalarını inanıyor otomatik testler çalıştırarak harika uygulama yayımlamanıza yardımcı olan App Center Test kullanıma sunduk. Şirket uygulamaları ve App Center testleri bulunabilir göndererek belgeleri ayrıntılı [burada](https://docs.microsoft.com/appcenter/test-cloud). Son olarak, anlamanıza yardımcı olması için bu araçları ve bileşenleri bir araya getireceğinizi nasıl size kuruluşlar için sürekli tümleştirme oluşturabilirsiniz birkaç farklı CI ortamları özetlenen. Daha fazla bilgi için Visual Studio Team Services ve Team Foundation Server Xamarin projeleri ile kullanma [yapılandırma TFVC](https://docs.microsoft.com/azure/devops/repos/tfvc/overview/) ve bu [sürekli tümleştirme giriş](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer/). Benzer şekilde, Jenkins kullanıyorsanız, bkz. [Xamarin ile Jenkins kullanma](~/tools/ci/jenkins-walkthrough.md) bir sürekli tümleştirme kurulumu hakkında ayrıntılı bilgi.
