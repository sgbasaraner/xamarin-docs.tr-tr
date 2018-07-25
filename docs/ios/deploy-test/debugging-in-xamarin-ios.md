---
title: Xamarin.iOS uygulamaları için hata ayıklama
description: Bu belge hata ayıklayıcı Mac ya da Visual Studio 2017 için Visual Studio'da hata ayıklama ayarı kesme noktaları, kablosuz hata ayıklama ve daha fazlası dahil olmak üzere bir Xamarin.iOS uygulaması için nasıl kullanılacağını açıklar.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4b21a69e49c8c7fd79de8edac9858c4714657f1c
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242322"
---
# <a name="debugging-xamarinios-apps"></a>Xamarin.iOS uygulamaları için hata ayıklama

_Xamarin.iOS uygulamaları Visual Studio veya Mac için Visual Studio'da yerleşik hata ayıklayıcısı ile ayıklanabilir._

Visual Studio için C# ve başka yönetilen dillerde kod hata ayıklama için Mac yerel hata ayıklama desteği ve kullanmak [LLDB](http://lldb.llvm.org/tutorial.html) C hata ayıklamak ihtiyacınız olduğunda, Objective C ya da C++ koduna, Xamarin.iOS projenizi bağlama.


> [!NOTE]
> Uygulama hata ayıklama modunda derleme yaptığınızda, Xamarin.iOS her kod satırı izleme gibi daha yavaş ve büyük uygulamalar oluşturur. Serbest bırakmadan önce bir yayın yapısı yaptığınızdan emin olun.

Xamarin.iOS hata ayıklayıcı, IDE'ye tümleşiktir ve bu hizmet sayesinde geliştiriciler, simülatör ve cihazda Xamarin.iOS tarafından desteklenen yönetilen dillerinden biri ile oluşturulmuş Xamarin.iOS uygulamalarında hata ayıklamak.

Xamarin.iOS hata ayıklayıcı kullanan [Mono geçici hata ayıklayıcı](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), oluşturulan kod ve Mono çalışma zamanı hata ayıklama deneyimini sağlamak için IDE ile işbirliği anlamına gelir. Bu bilgi veya hata ayıklanan programın işbirliği olmadan bir program denetleyen sabit hata ayıklayıcıları LLDB veya MDB gibi farklıdır.

## <a name="setting-breakpoints"></a>Kesme noktaları ayarlama

İlk adımıdır için uygulamanızı hata ayıklamaya başlamak hazır olduğunuzda [kesme noktaları ayarlayın, uygulamanızın](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Bu, kesmek istediğiniz kod satırı sayısını yanındaki Düzenleyici kenar boşluğu alanına tıklayarak gerçekleştirilir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging1.png "Kesme noktaları ayarlama")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging1a.png "Kesme noktaları ayarlama")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Kodunuzda giderek ayarlanan kesme noktaları görüntüleyebileceğiniz **kesme noktaları paneli**:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/image0a.png "Kesme noktaları paneli")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Kesme noktaları paneli otomatik olarak görüntülenmiyorsa, görünür seçerek yapabileceğiniz _Görüntüle > Windows hata ayıklama > kesme noktaları_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/image0.png "Kesme noktaları paneli")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Kesme noktaları paneli otomatik olarak görüntülenmiyorsa, görünür seçerek yapabileceğiniz _hata ayıklama > Windows > kesme noktaları_
 
-----

Herhangi bir uygulamanın hatalarını ayıklamaya başlamadan önce her zaman yapılandırma ayarlandığından emin olun **hata ayıklama**gibi bu yararlı birtakım Araçlar kesme noktaları gibi hata ayıklama, görselleştiriciler verileri kullanarak ve çağrı yığınını görüntüleme desteği içerir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7.png "Simulator'da hata ayıklama")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "fiziksel bir cihazda hata ayıklama")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7c.png "Simulator'da hata ayıklama")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "fiziksel bir cihazda hata ayıklama")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Hata Ayıklamayı Başlat
Hata ayıklamayı başlatmak için hedef cihaz seçin veya IDE'nizi benzer:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7b.png "Hedef cihaz seçin")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7e.png "Hedef cihaz seçin")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



Tuşuna basarak uygulamanızı dağıtmaya **Play** düğmesi.

Kod, bir kesme noktasına ulaştığınızda, vurgulanan sarı olur:

[![](debugging-in-xamarin-ios-images/image2.png "Kod vurgulanan sarı olur")](debugging-in-xamarin-ios-images/image2.png#lightbox)

Hata ayıklama gibi nesneleri değerlerini İnceleme araçları, bu noktada kodunuzda olup bitenler hakkında daha fazla bilgi almak için kullanılabilir:

[![](debugging-in-xamarin-ios-images/image3.png "Bir renk değeri görüntüleme")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Koşullu kesme noktaları

Kuralları altında bir kesme noktası ortaya, bu ekleme olarak bilinir duruma dikte de ayarlayabilirsiniz bir *koşullu kesme noktası*.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Koşullu kesme noktası ayarlamak için erişim **kesme noktası Özellikler penceresi**, hangi yapılabilir iki yolla:


- Yeni bir koşullu kesme noktası eklemek için bir kesme noktası ayarlamak istediğiniz kod için satır numarası solundaki Düzenleyici kenar sağ tıklayın ve yeni kesme noktası seçin:

    [![](debugging-in-xamarin-ios-images/image4.png "Yeni kesme noktası seçin")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Varolan bir kesme noktası için bir koşul eklemek için kesme noktasına sağ tıklatın ve **kesme noktası özelliklerini** veya **kesme noktaları paneli** aşağıda gösterilen Özellikler düğmesi seçin:

    [![](debugging-in-xamarin-ios-images/image5.png "Kesme noktaları paneli")](debugging-in-xamarin-ios-images/image5.png#lightbox)


Bu gibi durumlarda, istediğiniz koşulu sonra gerçekleşmesi için kesme noktasına girebilirsiniz:

[![](debugging-in-xamarin-ios-images/image6.png "Gerçekleşmesi kesme noktası koşulunu girin")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio 2015'te, ilk koşullu kesme noktası ayarlamak için [normal bir kesme noktası ayarlamak](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Bağlam menüsünü görüntülemek için kesme noktasına sağ tıklayın:

 [![](debugging-in-xamarin-ios-images/image4vs.png "Kesme noktası bağlam menüsü")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Seçin **koşulları...**  görüntülenecek _kesme noktası ayarları_ menüsü:

 [![](debugging-in-xamarin-ios-images/image6vs.png "Kesme noktası ayarlar menüsü")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

Gerçekleşmesi için kesme noktası altında istediğiniz koşulları buraya girin

Visual Studio'nun önceki sürümlerinde kesme noktası koşulları kullanma hakkında daha fazla bilgi için [Visual Studio'nun belgeleri](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints) bu konuda.

-----

## <a name="navigating-through-code"></a>Kodda gezinme

Bir kesme noktasına ulaşıldığında hata ayıklama araçları program yürütme denetime yararlanmanıza olanak tanıyacak. IDE, böylece çalıştırma ve kodunuz içinde adım adım dört düğme görüntülenir.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da, aşağıdakine benzer olacaktır:

 [![](debugging-in-xamarin-ios-images/image7.png "Geliştirici program yürütme üzerinde denetim altına almak hata ayıklama araçlarını etkinleştir")](debugging-in-xamarin-ios-images/image7.png#lightbox)

Bunlar:

- **Yürütme/durdurma** – bu başlamak/sonraki kesme noktasına kadar bir kod yürütmeden durdurma.
- **Step Over** – Bu kod sonraki satırı yürütür. Sonraki satır bir işlev çağrısı ise, adım üzerinde işlevi yürütme ve eder stop kodun sonraki satırında _sonra_ işlevi.
- **İçine adımla** – Bu ayrıca sonraki kod satırına yürütülür. Sonraki satır bir işlev çağrısı ise, içine adımla işlevin ilk satır, satır satır işlevi, hata ayıklamaya devam etmenize imkan sağlar durdurur. Sonraki satır bir işlev değilse, aynı Step Over olarak davranır.
- **Step Out** – bu satıra burada geçerli işlev çağrıldı döndürür.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da, aşağıdaki gibi görünür:

[![](debugging-in-xamarin-ios-images/image7vs.png "Geliştirici program yürütme üzerinde denetim altına almak hata ayıklama araçlarını etkinleştir")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Bunlar:

- **Yürütme/durdurma** – bu başlamak/sonraki kesme noktasına kadar bir kod yürütmeden durdurma.
- **Adımla (F11)** – Bu kod sonraki satırı yürütür. Sonraki satır bir işlev çağrısı ise, adım üzerinde işlevi yürütme ve eder stop kodun sonraki satırında _sonra_ işlevi.
- **İçine adımla (F10)** – Bu ayrıca sonraki kod satırına yürütülür. Sonraki satır bir işlev çağrısı ise, içine adımla işlevin ilk satır, satır satır işlevi, hata ayıklamaya devam etmenize imkan sağlar durdurur. Sonraki satır bir işlev değilse, aynı Step Over olarak davranır.
- **Step Out (Shift + F11)** – bu satıra burada geçerli işlev çağrıldı döndürür.

Derinlik belgelerinde hata ayıklama hakkında daha fazla bilgi için bkz. [gidin ve Visual Studio hata koduyla](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Kesme noktaları

İçin başlangıç noktası iOS uygulamaları yalnızca birkaç saniye (10) sağlar ve tamamlamak önemlidir `FinishedLaunching` yöntemi uygulama ekleyin. Uygulama bu yöntem 10 saniye içinde tamamlanmazsa, iOS işlem sonlandırılır.

Bu, programınızın başlatma kodunu üzerinde kesme noktaları ayarlamak neredeyse imkansız olduğu anlamına gelir. Başlangıç kodunuzu hata ayıklamak istiyorsanız, bazı başlatılmasını geciktirmek ve Zamanlayıcı çağrılan yönteme girmeyi veya diğer türde bir FinishedLaunching sonlandıktan sonra çalıştırılan bir geri çağırma yöntemi put gerekir.

## <a name="device-diagnostics"></a>Cihaz tanılama

Hata ayıklayıcı ayarlanırken bir hata varsa, ekleyerek ayrıntılı tanılama etkinleştirebilirsiniz "-v - v - v", proje seçenekleri diğer mtouch bağımsız değişkenleri için. Bu, ayrıntılı hata bilgileri cihaz konsola yazdırır.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Hata ayıklama kablosuz

Xamarin.iOS, uygulamanızda cihazlarınızda USB bağlantısı üzerinden hata ayıklamak için varsayılandır. Bazen bir USB cihazı test etmek için takma/modemi çıkarmayı ExternalAccessory destekli uygulamaları geliştirmek için kablo gerekiyor olabilir. Bu gibi durumlarda, kablosuz ağ üzerinden hata ayıklama kullanabilirsiniz.

Kablosuz dağıtım ve hata ayıklama hakkında daha fazla bilgi için [kablosuz dağıtım](~/ios/deploy-test/wireless-deployment.md) Kılavuzu.

<a name="Technical_Details" />

## <a name="technical-details"></a>Teknik Ayrıntılar

Xamarin.iOS yeni Mono geçici hata ayıklayıcı kullanır. Ayrı bir işlemde denetlemek için işletim sistemi arabirimlerini kullanarak ayrı bir işlemde denetleyen bir program olan standart Mono hata ayıklayıcısını, geçici hata ayıklayıcı, kablo protokolü aracılığıyla hata ayıklama işlevselliği kullanıma sunma Mono çalışma zamanı sağlayarak çalışır.

Başlangıçta, hataları ayıklanan kişi olarak bir uygulama hata ayıklayıcı ve hata ayıklayıcı başlar çalışılacak. Xamarin.ios'ta Visual Studio için Xamarin Mac Aracısı'nı (Visual Studio'da) ve hata ayıklayıcı arasındaki orta adam gibi davranır.

Bu geçici bir hata ayıklayıcı ortak bir hata ayıklama düzeni cihazda çalıştırırken gerektirir. Başka bir deyişle, hata ayıklama kodu her dizisi noktada hata ayıklamayı desteklemek için ek bir kod içerecek şekilde işaretlenmiş gibi daha büyük ne zaman ikili dosyanız oluşturur.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Konsol erişme

Kilitlenme günlüklerini ve konsolu sınıfı çıktısını iPhone konsola gönderilir. Bu konsol organizer, cihaz seçilerek ve "şablonda" Xcode ile erişebilirsiniz.

Alternatif olarak, Xcode ' başlatmak istiyor musunuz, Apple'nın kullanabilirsiniz [iPhone yapılandırma yardımcı programı](http://www.apple.com/support/iphone/enterprise/) doğrudan konsoluna erişmek için. Bu, bir sorun alanına hata ayıklaması yapıyorsanız, konsol günlüklerini bir Windows makineden erişebilirsiniz yöntemlerine sahiptir.

Visual Studio kullanıcılar için kullanılabilir birkaç günlükleri çıkış penceresinde, ancak daha kapsamlı ve ayrıntılı günlükleri için bir Mac bilgisayarınızda geçmeniz.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Mono'nın sınıf kitaplıklarında hata ayıklama

Mono'nın sınıf kitaplıkları için kaynak kodu Xamarin.iOS birlikte ve şeyleri nasıl başlık altında çalışmakta olduğunuz görmek için tek adımda hata ayıklayıcı için bunu kullanabilirsiniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu özellik, hata ayıklama sırasında daha fazla bellek tüketir olduğundan, bu varsayılan olarak kapalıdır.


Bu özelliği etkinleştirmek için emin **yalnızca proje kodunda hata ayıklama; çerçeve koduna geçme kodlarındaki** seçeneği altında kaldırıldığında _Mac için Visual Studio > Tercihler > hata ayıklayıcı_ gösterildiği gibi menüsü Aşağıda:

[![](debugging-in-xamarin-ios-images/debugging6.png "Mono'nın sınıf kitaplıklarında hata ayıklama")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sınıf kitaplıkları Visual Studio'da hata ayıklamak için devre dışı bırakmalısınız **yalnızca kendi kodum** altında _Hata Ayıkla > Seçenekler_ menü. İçinde _hata ayıklama > Genel_ düğümü, NET **yalnızca benim kodumu etkinleştir** onay kutusu:

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Mono'nın sınıf kitaplıklarında hata ayıklama")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

Bunu yaptıktan sonra uygulamanızı ve tek adımda Mono'nın temel sınıf kitaplıkları birini başlatabilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin ile hata ayıklama](/visualstudio/mac/debugging/)
- [Veri Görselleştirmeleri](/visualstudio/mac/data-visualizations/)
- [Bir kesme noktası ayarlayın](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)
- [Kodu adımlayın](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)
- [Günlük penceresini çıkış bilgileri](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)
