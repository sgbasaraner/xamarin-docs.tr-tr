---
title: Uygulama paketi boyutu
description: Bu makalede, bir xamarin Android uygulama paketi ve hata ayıklama sırasında verimli paket dağıtımı için kullanılan ve geliştirme aşamalarını yayın ilişkili stratejiler bağlı bölümlerini inceler.
ms.prod: xamarin
ms.assetid: 8D70CDDD-3D3C-9949-8045-AB8F93D18E74
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: babe45c39f6a69dd9384f3bce8fe28ada31ebfdf
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="application-package-size"></a>Uygulama paketi boyutu

_Bu makalede, bir xamarin Android uygulama paketi ve hata ayıklama sırasında verimli paket dağıtımı için kullanılan ve geliştirme aşamalarını yayın ilişkili stratejiler bağlı bölümlerini inceler._


## <a name="overview"></a>Genel Bakış

Xamarin.Android mekanizmaları çeşitli bir etkin hata ayıklama ve yayın dağıtım işlemi korurken paket boyutu en aza indirmek için kullanır. Bu makalede, Xamarin.Android sürümde bakın ve dağıtım iş akışı ve nasıl Xamarin.Android platform biz yapı küçük uygulama paketleri yayın olmasını sağlar ve hata ayıklama.


## <a name="release-packages"></a>Yayın paketleri

Tam olarak içerilen uygulama dağıtmayı paket uygulama, ilişkili kitaplıkları, içerik, Mono çalışma zamanı ve gerekli temel sınıf kitaplığı (BCL) derlemeler içermelidir. Biz varsayılan "Hello World" şablonu izlerseniz, örneğin, bir tam paketi yapı içeriğini şöyle olabilir:

[![Bağlayıcı önce paket boyutu](app-package-size-images/hello-world-package-size-before-linker.png)](app-package-size-images/hello-world-package-size-before-linker.png#lightbox)

İsteriz daha büyük bir indirme boyutu 15,8 MB'dir. Sorun BCL kitaplıkları aynıdır mscorlib, sistem ve Mono.Android, uygulamanızı çalıştırmak için gerekli olan bileşenler çok sağlayan içerirler. Ancak, bu bileşenlerin dışlanacak tercih edilebilir böylece, uygulamanızda kullanmıyor olabilirsiniz işlevselliği de sağlar.

Biz dağıtım için uygulama oluştururken, biz bağlama olarak bilinen bir işlemi yürütmeyi uygulama inceler ve doğrudan kullanılmayan herhangi bir kod kaldırır. Bu işlem işlevine benzer, [çöp toplama](~/android/internals/garbage-collection.md) yığında ayrılmış bellek sağlar. Ancak nesneler üzerinde çalışan yerine bağlama kodunuzu üzerinde çalışır. Örneğin, olduğundan tüm ad alanı içinde System.dll göndermek ve e-posta almak için ancak uygulamanızın değil yaparsanız kod yalnızca yer harcama bu işlevselliğini kullanın. Hello World uygulamasının bağlayıcı çalıştırdıktan sonra bizim paket şimdi şöyle görünür:

[![Bağlayıcı sonra paket boyutu](app-package-size-images/hello-world-package-size-after-linker.png)](app-package-size-images/hello-world-package-size-after-linker.png#lightbox)

Görebiliriz gibi bu değil kullanılan BCL önemli miktarda kaldırır. En son BCL boyutuna hangi uygulamanın gerçekten kullandığı bağlı olduğunu unutmayın. Biz ApiDemo olarak adlandırılan daha önemli bir örnek uygulamayı göz atın, örneğin, görebiliriz ApiDemo BCL Hello daha fazlasını kullandığından BCL bileşen boyutu artar, dünya yapar:

![Bağlama sonra ApiDemo paket boyutu](app-package-size-images/api-demo-package-size-after-linker.png)

Aşağıda gösterildiği gibi uygulama paket boyutu genellikle yaklaşık 2.9 MB uygulamanız ve bağımlılıklarını büyük olacaktır.


## <a name="debug-packages"></a>Paketleri hata ayıklama

Hata ayıklama derlemeleri için şeyler biraz farklı şekilde ele alınır. Sürekli bir aygıta yeniden dağıtırken, uygulamanın biz hata ayıklama paketleri hızı boyutu yerine dağıtım için en iyi duruma getirmek için olabildiğince hızlı olması gerekir.

Android kopyalayıp paketi boyutunun olabildiğince küçük olmasını istiyoruz şekilde bir paketi yüklemek nispeten daha yavaştır. Yukarıda da açıklandığı gibi paket boyutu en aza indirmek için bir olası bağlayıcı yoludur. Ancak, bağlantı yavaş ve genellikle yalnızca son dağıtım bu yana değişmiş bölümlerini uygulamanın dağıtılacağı istiyoruz. Bunu başarmak için biz uygulamamız çekirdek Xamarin.Android bileşenlerinden ayırın.

Cihazda, biz hata ayıklama ilk kez biz adlı iki büyük paketler kopyalama *çalışma zamanı paylaşılan* ve *paylaşılan Platform*. Paylaşılan Platform Android API düzeyi belirli derlemeleri içerirken Mono çalışma zamanı ve BCL, paylaşılan çalışma zamanı içerir:

[![Paylaşılan çalışma zamanı paket boyutu](app-package-size-images/shared-runtime-package-size.png)](app-package-size-images/shared-runtime-package-size.png#lightbox)

Bu temel bileşenleri kopyalama yalnızca yapılır kez oldukça biraz zaman alır gibi ancak bunları kullanan hata ayıklama modunda çalışan sonraki uygulamaların verir. Son olarak, küçük ve hızlı gerçek uygulama kopyalayın:

![Gerçek uygulama küçüktür](app-package-size-images/hello-world-debug-application-no-link.png)

### <a name="fast-assembly-deployment"></a>Hızlı derleme dağıtım

*Hızlı derleme dağıtım* derleme seçeneği, daha fazla derlemeler derlemeleri doğrudan cihazda yalnızca bir kez yükleme uygulamanın paketinde ekleyerek değil hata ayıklama yükleme paketi boyutunu azaltmak için kullanılabilir ve sonrasında son dağıtımı değiştirilmiş olan dosyaların üzerine yalnızca kopyalama.

Etkinleştirmek için *hızlı derleme dağıtım*, aşağıdakileri yapın:

1.  Çözüm Gezgini'nde Android projesine sağ tıklayın ve seçin **seçenekleri**.

2.  Proje Seçenekleri iletişim kutusundan seçin **Android derleme** :  

    ![Proje seçenekleri Android derleme](app-package-size-images/fastdev0.png)

3.  Denetleme **kullanımı paylaşılan Mono çalışma zamanı onay kutusunu** ve **hızlı derleme dağıtım** onay kutularını:  

    ![Paketleme sekmesi altında seçilen onay kutuları](app-package-size-images/fastdev.png)

4.  Tıklatın **Tamam** düğmesine değişiklikleri kaydetmek ve proje Seçenekleri iletişim kutusunu kapatın.


Uygulama yerleşik bir sonraki seferde hata ayıklama derlemeleri (Bunlar zaten etmediyseniz) doğrudan aygıta yüklenir ve (içermeyen derlemeler) daha küçük bir uygulama paketi cihazda yüklenecek. Bu test etmek için hazır ve çalışır uygulamasında yapılacak değişiklikler alma süresini kısaltmak.

Long verip bunu tarafından paylaşılan çalışma zamanı ve paylaşılan platform ilk dağıtabilir, biz bir uygulamaya her değişiklik yaptığınızda, böylece biz hızlı Değiştir/dağıtmak/Çalıştır döngüsü Biz yeni sürümü hızla ve painlessly, dağıtabilir.


## <a name="summary"></a>Özet

Bu makalede Xamarin.Android sürüm ve hata ayıklama profili paketleme modellerinin incelendi. Ayrıca, hata ayıklama sırasında verimli paket dağıtımına ve geliştirme aşamalarını yayın için Mono Android platformu için kullandığı stratejileri arama.
