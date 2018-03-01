---
title: "Raporlama hataları"
description: "Bu belgede hatalar çözmek için yaklaşımlar açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f7ded8fdc1274f7c98d8f7134f6a87c7ba767646
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="reporting-bugs"></a>Raporlama hataları

## <a name="overview"></a>Genel Bakış

Bazen hepimizin istiyoruz şekilde çalışması için bir API almak için kullanamama veya çalışılırken bir hata olarak çözmek bir proje üzerinde çalışırken takılmış. Mobil ve Masaüstü uygulamalarınızı yazılırken başarılı olması için Xamarin adresindeki Amacımız içindir ve yardımcı olacak bazı kaynaklar sağladık.

Tüm bu kaynaklar, bunları hızlı bir şekilde sorununuzu çözmenize yardımcı olacak hazırlama, bazı adımlar vardır:

- En iyi rapor kilitlenme olabildiğince sorunu kök nedenini:
 
     - "Uygulamam kilitleniyor" tanılamak zordur. "Boş bir dizi bu çağrıyı döndüğünüzde çöküyor Uygulamam" çözmeye çalışmak çok daha kolaydır.

     - "I çalışmaya NSTable alınamıyor" "Benim NSTableDelegate yöntemlerden hiçbiri bu durumda çağrılacak gibi görünüyor. daha" daha az yardımcı olur

- Mümkünse sorunu gösteren bir küçük örnek program sağlar. Kaynak kodu sorunu arayan sayfaları aracılığıyla sorunda daha fazla zaman ve çaba büyüklük alır.

- Ne görünmesi bir soruna neden uygulamanıza yaptığınız değişiklikleri sorunun kaynağı hızlı bir şekilde daraltabilirsiniz bilerek. Yakın zamanda soruna neden bölümü bulmak için uygulamanızın bölümlerini kırpma Xamarin.Mac sürümleri yükseltme yaparsam veya hangi değişiklik sorunu sunulan bulmak için önceki test derlemeler belirtmeye çok yararlı olabilir.


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>Uygulamanızı sahip bir çıkış kilitlendiğinde yapmanız gerekenler

Çoğu durumda, Mac için Visual Studio hata ayıklayıcısını özel durumları yakalar ve uygulamanızda çöküyor ve kök nedeni izlemenize yardımcı olması. Ancak bazı durumlarda, uygulamanızın nerede ve noktasında Sıçrama çıkın çok az kayıpla veya hiç çıkış vardır. Bunlar içerebilir:

- Kod sorunları imzalama.
- Mono belirli çalışma zamanı çöküyor.
- Bazı Objective-c özel durumlar ve kilitleniyor.
- Bazı çok erken işlem yaşam süresi çöküyor.
- Bazı taşan yığın.
- Listelenen macOS sürümü, **Info.plist** yüklü macOS sürümünüz veya geçersiz daha yeni.

Bu programlar hata ayıklama can sıkıcı olabilir, bulma olarak gerekli bilgileri zor olabilir. Yardımcı olabilecek birkaç yaklaşımlar şunlardır:

- Listelenen macOS sürüm emin **Info.plist** ile aynı bilgisayarda yüklü macOS sürümü gibidir.
- Visual Studio için Mac uygulama çıktısını denetleyin (**Görünüm** -> **klavye takımı** -> **uygulama çıktısı**) için yığın izlemeler veya çıkış kırmızı Çıktı açıklayabilir Cocoa.
- Komut satırından uygulamanızı çalıştırın ve çıktıyı bakın (içinde **Terminal** uygulama) kullanarak: 

     `MyApp.app/Contents/MacOS/MyApp` (burada `MyApp` , uygulamanızın adıdır)
- "MONO_LOG_LEVEL" komutunuzu komut satırında örneğin ekleyerek çıktıyı artırabilirsiniz: 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- Yerel hata ayıklayıcı bağlayamıyor (`lldb`) (Bu bir Ücretli lisansı gerekir) herhangi bir daha fazla bilgi sağlayıp sağlamadığını görmek için işlem için. Örneğin, aşağıdakileri yapın:

    1. Girin `lldb MyApp.app/Contents/MacOS/MyApp` Terminal.
    2. Girin `run` Terminal.
    3. Girin `c` Terminal.
    4. Hata Ayıklama tamamlandığında çıkın.
- Son çare olarak önce çağırma `NSApplication.Init` içinde `Main` yöntemi (veya diğer yerler gerektiği gibi), bir dosyaya başlatma, hangi adımın sorun çalıştırıyorsanız izlemek için bilinen bir konuma metin yazabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

Aşağıdaki bölümlerde, bilinen sorunlar ve çözümleri kapsar.

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>Korumalı uygulamalar hata ayıklayıcıda bağlanamadı

Hata ayıklayıcı Xamarin.Mac uygulamalar korumalı alan, etkinleştirdiğinizde, varsayılan olarak, uygulamaya bağlanamıyor anlamına gelir, TCP üzerinden bağlanır etkin uygun izinler olmadan uygulamayı çalıştırmayı denerseniz, bir hata alıyorsunuz şekilde *"bağlanılamıyor hata ayıklayıcı"*. 

[![Yetkilendirmeler düzenleme](troubleshooting-images/debug01.png "yetkilendirmeleri düzenleme")](troubleshooting-images/debug01-large.png)

**İzin giden ağ bağlantıları (istemci)** izin için hata ayıklayıcı gerekli bir durumda, bu bir etkinleştirilmesi normal olarak hata ayıklama izin verir. Bu olmadan hata ayıklaması yapılamıyor olduğundan, biz güncelleştirdiniz `CompileEntitlements` için hedef `msbuild` hata ayıklama için korumalı herhangi bir uygulama yalnızca derlemeler için bu izni yetkilendirmeler otomatik olarak eklemek için. Yayın derlemeleri değiştirilmemiş yetkilendirmeler dosyasında belirtilen yetkilendirmeler kullanmanız gerekir.

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: 437 kodlama için kullanılabilir veri yok.
 
3 taraf kitaplıklar Xamarin.Mac uygulamanızda dahil olmak üzere, formda bir hata alabilirsiniz "System.NotSupportedException: 437 kodlama için kullanılabilir veri yok." derlemek ve uygulamayı çalıştırmak çalışırken. Örneğin, kitaplıklar, gibi `Ionic.Zip.ZipFile`, bu işlem sırasında özel durum.

Bu gidip Xamarin.Mac proje seçenekleri açarak çözülebilir **Mac yapı** > **uluslararası** ve denetimi **Batı** Uluslararası hale getirme:

[![Derleme seçenekleri düzenleme](troubleshooting-images/issue01.png "düzenleme derleme seçenekleri")](troubleshooting-images/issue01-large.png)

### <a name="failed-to-compile-mm5103"></a>(Mm5103) derlenemedi

Bu hata genellikle sürüm Xcode yeni bir sürümü olduğunda ve yeni sürümü yüklü ancak Hayır çalıştırmak, kaynaklanır. Yeni bir Xcode sürümüyle derlemek denemeden önce öncelikle bu sürümü en az bir kez çalıştırmanız gerekir.

Xcode, yeni bir sürümünü çalıştıran ilk kez Xamarin.Mac tarafından gereken birkaç komut satırı araçlarını yükler. Ayrıca, Xcode veya Xamarin.Mac sürümünüzü güncelleştirdikten sonra temiz bir yapı yapmanız gerekir.

Bu sorunu çözemezseniz, lütfen [dosyalama](#filing-a-bug).

### <a name="missing-entitlementsplist"></a>Eksik entitlements.plist

Mac için Visual Studio en son sürümünü yetkilendirmeler bölümünden kaldırılmış **Info.plist** Düzenleyicisi ve ayrı yerleştirilmiş **Entitlements.plist** Düzenleyicisi'ni (daha iyi platformlar arası desteği Xamarin.iOS ile).

Yeni Visual Studio ile for Mac yüklü bir yeni Xamarin.Mac uygulaması projesi oluşturduğunuzda, bir **Entitlements.plist** dosya proje ağacına otomatik olarak eklenir:

![Yetkilendirmeler seçme](troubleshooting-images/entitlements01.png "yetkilendirmeler seçme")

Çift ise **Entitlements.plist** dosya, yetkilendirmeler Düzenleyicisi'ni görüntülenir:

[![Yetkilendirmeler düzenleme](troubleshooting-images/entitlements02.png "yetkilendirmeleri düzenleme")](troubleshooting-images/entitlements02-large.png)

Varolan Xamarin.Mac projeleri için el ile oluşturmanız gerekir **Entitlements.plist** nde projeye sağ tıklanarak dosya **çözüm paneli** ve seçerek **Ekle**  >  **Yeni dosya...** . Ardından, **Xamarin.Mac** > **boş özellik listesi**:

![Yeni bir özellik listesi ekleme](troubleshooting-images/entitlements03.png "yeni bir özellik listesi ekleme")

Girin `Entitlements` tıklayın ve ad için **yeni** düğmesi. Projenizi yetkilendirmeler dosyasını daha önce eklediyseniz, yeni bir dosya oluşturmak yerine proje eklemek için istenir:

[![Bir dosyanın üzerine yazma doğrulama](troubleshooting-images/entitlements04.png "bir dosyanın üzerine yazma doğrulanıyor")](troubleshooting-images/entitlements04-large.png)

## <a name="contacting-support-business-or-enterprise-licenses"></a>Destek (iş veya enterprise lisans)

Bir iş veya enterprise lisansınız varsa, Xamarin mühendisleri destek biletlerini aracılığıyla doğrudan yardım istemek uygundur. Bkz: [xamarin.com/support](http://xamarin.com/support) Ayrıntılar için.

## <a name="community-support-on-the-forums"></a>Forumlarda topluluk desteği

Xamarin ürünler kullanılarak Geliştirici topluluğu inanılmaz ve birçok ziyaret bizim [Xamarin.Mac forumları](http://forums.xamarin.com/categories/mac) deneyimleri ve uzmanlara paylaşmak için. Buna ek olarak, Xamarin mühendisleri düzenli aralıklarla yardımcı olmak için forumunu ziyaret edin.

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>Bir hata dosyalama

Görüşleriniz bizim için önemlidir. Xamarin.Mac herhangi bir sorun bulursanız:

- Arama [sorunu deposu](https://github.com/xamarin/xamarin-macios/issues) 
- GitHub konulara geçmeden önce Xamarin sorunları üzerinde izlenmekte olan [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Lütfen var. sorunları eşleştirmek için arama yapın.
- Eşleşen bir sorunu bulamazsanız, Lütfen yeni bir sorun dosya [GitHub sorunu depo](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub sorunlar tüm ortak verilmektedir. Yorumlar veya ekleri gizlemek mümkün değildir. 

Lütfen aşağıdaki mümkün olduğunca çok şunları içerir:                                                                                                                                          

- Sorunu yeniden oluşturma, basit bir örnek. Bu **çok** mümkün olduğunda. 
- Kilitlenme tam yığın izleme.
- Kilitlenme çevreleyen C# kod. 
