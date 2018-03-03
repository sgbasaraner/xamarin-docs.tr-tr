---
title: "Hata Ayıklama"
description: "Xamarin.iOS uygulamaları Visual Studio'da yerleşik hata ayıklayıcısı ile Mac veya Visual Studio hata ayıklaması yapılabilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: ca3afa892176a11c4688b4f4d8d34e59d1758585
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="debugging"></a>Hata Ayıklama

_Xamarin.iOS uygulamaları Visual Studio'da yerleşik hata ayıklayıcısı ile Mac veya Visual Studio hata ayıklaması yapılabilir._

Visual Studio için C# ve diğer yönetilen dilleri kod hata ayıklama için Mac'ın yerel hata ayıklama desteği ve kullanmak [LLDB](http://lldb.llvm.org/tutorial.html) C hata ayıklamak gerektiğinde, C++ ya da Objective C değiştirebildiğiniz, Xamarin.iOS projenizi ile bağlama.


> [!NOTE]
> **Önemli:** hata ayıklama modunda uygulamaları derlediğinizde, her kod satırı izlenmiş gibi Xamarin.iOS daha yavaştır ve çok daha büyük uygulamalar oluşturur. Serbest bırakmadan önce yayın derlemesi yaptığınızdan emin olun.

Xamarin.iOS hata ayıklayıcı IDE'yi tümleştirilmiştir ve geliştiricilerin simulator ve cihazda Xamarin.iOS tarafından desteklenen yönetilen dillerinden biri ile oluşturulan Xamarin.iOS uygulamalarda hata ayıklama olanak tanır.

Xamarin.iOS hata ayıklayıcı kullanan [Mono yumuşak hata ayıklayıcı](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), oluşturulan kod ve Mono çalışma zamanı hata ayıklama deneyimini sağlamak üzere IDE ile işbirliği anlamına gelir. Bu Bilgi Bankası veya iş Birliği hata ayıklaması programdan olmadan bir program denetleyen sabit hata ayıklayıcıları LLDB veya MDB gibi farklıdır.

## <a name="setting-breakpoints"></a>Kesme noktalarını ayarlama

İlk adımdır için uygulamanızın hatalarını ayıklama başlatmaya hazır olduğunuzda [uygulamanızı kesme noktalarını ayarlayın](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Bu kod, bölmek istediğiniz satır numarasını yanındaki Düzenleyicisi kenar boşluğu alanı tıklayarak gerçekleştirilir:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](debugging-in-xamarin-ios-images/debugging1.png "Kesme noktalarını ayarlama")](debugging-in-xamarin-ios-images/debugging1.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](debugging-in-xamarin-ios-images/debugging1a.png "Kesme noktalarını ayarlama")](debugging-in-xamarin-ios-images/debugging1a.png)

-----

Kodunuzda giderek ayarlanan tüm kesme noktaları görüntüleyebilirsiniz **kesme noktaları paneli**:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](debugging-in-xamarin-ios-images/image0a.png "Kesme noktaları paneli")](debugging-in-xamarin-ios-images/image0a.png)

 Kesme noktaları paneli otomatik olarak görüntülenmiyorsa, onu görünen seçerek yapabileceğiniz _Görünüm > Windows hata ayıklama > kesme noktaları_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](debugging-in-xamarin-ios-images/image0.png "Kesme noktaları paneli")](debugging-in-xamarin-ios-images/image0.png)

 Kesme noktaları paneli otomatik olarak görüntülenmiyorsa, onu görünen seçerek yapabileceğiniz _hata ayıklama > Windows > kesme noktaları_
 
-----

Herhangi bir uygulama hata ayıklama başlamadan önce her zaman yapılandırma ayarlandığından emin olun **hata ayıklama**, bu yararlı birtakım Araçlar kesme noktaları gibi hata ayıklama, veri görselleştiriciler kullanarak ve çağrı yığını görüntüleme desteklemek için içeriyor:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](debugging-in-xamarin-ios-images/debugging7.png "Hata ayıklama simulator'da") ](debugging-in-xamarin-ios-images/debugging7.png) 
 [ ![ ] (debugging-in-xamarin-ios-images/debugging7a.png "fiziksel cihaz üzerindeki hata ayıklama")](debugging-in-xamarin-ios-images/debugging7a.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](debugging-in-xamarin-ios-images/debugging7c.png "Hata ayıklama simulator'da") ](debugging-in-xamarin-ios-images/debugging7c.png) 
 [ ![ ] (debugging-in-xamarin-ios-images/debugging7d.png "fiziksel cihaz üzerindeki hata ayıklama")](debugging-in-xamarin-ios-images/debugging7d.png)

-----

## <a name="start-debugging"></a>Hata ayıklama başlatılamıyor
Hata ayıklama başlatmak için hedef cihazı seçin veya IDE'yi benzer:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![](debugging-in-xamarin-ios-images/debugging7b.png "Hedef cihazı seçin")](debugging-in-xamarin-ios-images/debugging7b.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](debugging-in-xamarin-ios-images/debugging7e.png "Hedef cihazı seçin")](debugging-in-xamarin-ios-images/debugging7e.png)

-----



Ardından tuşlarına basarak uygulamanızı dağıtmak **Yürüt** düğmesi.

Bir kesme noktası isabet zaman kodu vurgulanan sarı olur:

[ ![](debugging-in-xamarin-ios-images/image2.png "Kod vurgulanan sarı olur")](debugging-in-xamarin-ios-images/image2.png)

Hata ayıklama nesneleri değerlerini İnceleme gibi araçları, bu noktada kodunuzda neler hakkında daha fazla bilgi almak için kullanılabilir:

[ ![](debugging-in-xamarin-ios-images/image3.png "Bir renk değeri görüntüleme")](debugging-in-xamarin-ios-images/image3.png)

## <a name="conditional-breakpoints"></a>Koşullu kesme noktaları

Ayrıca, altında bir kesme noktası gerçekleşmelidir, bu ekleme olarak bilinir durumlarda dikte kurallarını ayarlayabilmeniz için bir *koşullu kesme noktası*.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Koşullu kesme noktası ayarlamak için erişim **kesme noktası Özellikleri penceresinde**, hangi yapılabilir iki yolla:


- Yeni koşullu kesme noktası eklemek için bir kesme noktası ayarlamak istediğiniz kodu satır numarasını solundaki Düzenleyicisi kenar boşluğu sağ tıklatın ve yeni kesme seçin:

    [ ![](debugging-in-xamarin-ios-images/image4.png "Yeni kesme noktası seçin")](debugging-in-xamarin-ios-images/image4.png)

- Varolan bir kesme noktası için bir koşul eklemek için kesme noktası sağ tıklatın ve **kesme noktası özellikleri** veya **kesme noktaları paneli** aşağıda gösterilen özellikleri düğmesini seçin:

    [ ![](debugging-in-xamarin-ios-images/image5.png "Kesme noktaları paneli")](debugging-in-xamarin-ios-images/image5.png)


Bu gibi durumlarda, altında istediğiniz koşulu sonra gerçekleşecek şekilde kesme noktasına girebilirsiniz:

[ ![](debugging-in-xamarin-ios-images/image6.png "Koşul için gerçekleşmesi kesme noktası girin")](debugging-in-xamarin-ios-images/image6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Koşullu kesme noktası Visual Studio 2015'te ilk ayarlamak için [normal bir kesme noktası belirleyerek](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/). Bağlam menüsünü görüntülemek için kesme noktası sağ tıklatın:

 [ ![](debugging-in-xamarin-ios-images/image4vs.png "Kesme noktası bağlam menüsü")](debugging-in-xamarin-ios-images/image4vs.png)

Seçin **koşullar...**  görüntülemek için _kesme noktası ayarları_ menüsü:

 [ ![](debugging-in-xamarin-ios-images/image6vs.png "Kesme noktası ayarları menüsü")](debugging-in-xamarin-ios-images/image6vs.png)

Burada, gerçekleşmesi için kesme altında istediğiniz koşulları girebilirsiniz

Visual Studio'nun önceki sürümleri kesme noktası koşulları kullanma hakkında daha fazla bilgi için başvurmak [Visual Studio'nun belgelerine](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints) bu konuda.

-----

## <a name="navigating-through-code"></a>Kodda gezinme

Bir kesme noktası erişildiğinde hata ayıklama araçları program yürütme üzerinde denetim elde etmenizi sağlar. IDE dört düğmeleri, çalıştırmak ve kod üzerinden adım sağlayan görüntüler.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Mac için Visual Studio'da bunlar aşağıdaki gibi görünür:

 [ ![](debugging-in-xamarin-ios-images/image7.png "Geliştirici program yürütme denetime almak hata ayıklama araçlarını etkinleştirme")](debugging-in-xamarin-ios-images/image7.png)

Bunlar:

- **Yürüt/Durdur** – bu begin/kadar sonraki kesme kod yürütmek durdur.
- **Step Over** – bu kodun sonraki satırında, yürütülür. Sonraki satıra bir işlev çağrısı ise, adım üzerinde işlevi yürütmek ve olacak kodun sonraki satırında Dur _sonra_ işlevi.
- **Step Into** – bu kodun sonraki satırında, ayrıca yürütülür. Sonraki satıra bir işlev çağrısı ise, Step Into işlevi ilk satırında, satır satır hata ayıklama işlevinin devam etmenize izin vererek durdurur. Sonraki satıra bir işlev değil, aynı Step Over olarak davranır.
- **Step Out** – bu satırına geçerli işlevi burada çağrıldı döndürür.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio'da bunlar aşağıdaki gibi görünür:

[ ![](debugging-in-xamarin-ios-images/image7vs.png "Geliştirici program yürütme denetime almak hata ayıklama araçlarını etkinleştirme")](debugging-in-xamarin-ios-images/image7vs.png)

Bunlar:

- **Yürüt/Durdur** – bu begin/kadar sonraki kesme kod yürütmek durdur.
- **Step Over (F11)** – bu kodun sonraki satırında, yürütülür. Sonraki satıra bir işlev çağrısı ise, adım üzerinde işlevi yürütmek ve olacak kodun sonraki satırında Dur _sonra_ işlevi.
- **Step Into (F10)** – bu kodun sonraki satırında, ayrıca yürütülür. Sonraki satıra bir işlev çağrısı ise, Step Into işlevi ilk satırında, satır satır hata ayıklama işlevinin devam etmenize izin vererek durdurur. Sonraki satıra bir işlev değil, aynı Step Over olarak davranır.
- **Step Out (Shift + F11)** – bu satırına geçerli işlevi burada çağrıldı döndürür.

Derinlik belgelerinde hata ayıklama hakkında daha fazla bilgi için bkz: [gidin kodu Visual Studio hata ayıklayıcısı ile](https://docs.microsoft.com/en-us/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Kesme noktaları

İçin başlangıç noktası iOS uygulamaları (10) saniye sayıda sağlar ve tamamlamak önemlidir `FinishedLaunching` uygulama temsilci yöntemi. Uygulama bu yöntem 10 saniye içinde tamamlanmazsa iOS işlemi sonlandırın.

Başka bir deyişle, programınızın başlangıç kod kesme noktalarını ayarlayın neredeyse mümkün değildir. Başlangıç kodunuzdaki hataları ayıklamanıza istiyorsanız, bazı, başlatma gecikmesi ve bir zamanlayıcı çağrılan yöntem içine ya da diğer bazı FinishedLaunching sonlandırıldı sonra çalıştırılan geri çağırma yöntemi formunda yerleştirin.

## <a name="device-diagnostics"></a>Aygıt tanılama

Hata ayıklayıcı ayarlanırken bir hata varsa, ekleyerek ayrıntılı tanılama etkinleştirebilirsiniz "-v - v - v" proje seçeneklerinizi ek mtouch değişkenlerinde için. Bu ayrıntılı hata bilgileri aygıt konsola yazdırır.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Hata ayıklama kablosuz

Uygulamanızı cihazlarınızda USB bağlantısı üzerinden hata ayıklamak için Xamarin.iOS içinde varsayılan değerdir. Bazen USB aygıtı test etmek için takma/yönlendiriciyi ExternalAccessory destekli uygulamaları geliştirmek için kablo gerekiyor olabilir. Bu durumlarda, kablosuz ağ üzerinden hata ayıklama amacıyla kullanabilirsiniz.

Kablosuz dağıtımı ve hata ayıklama hakkında daha fazla bilgi için başvurmak [kablosuz dağıtım](~/ios/deploy-test/wireless-deployment.md) Kılavuzu.

<a name="Technical_Details" />

## <a name="technical-details"></a>Teknik Ayrıntılar

Xamarin.iOS yeni Mono yumuşak hata ayıklayıcı kullanır. Ayrı bir işlemde denetlemek için işletim sistemi arabirimleri kullanarak ayrı bir işlemde denetleyen bir program standart Mono debugger, kablo protokolü aracılığıyla hata ayıklama işlevselliği kullanıma Mono çalışma zamanı sağlayarak yumuşak hata ayıklayıcı çalışır.

Başlangıçta, hata ayıklaması kişiler olacak bir uygulama hata ayıklayıcı ve hata ayıklayıcısı başlatılır çalışılacak. İçinde bir Xamarin.iOS Visual Studio için Xamarin Mac aracı uygulamada (Visual Studio) ve hata ayıklayıcısı arasında Orta adam gibi davranır.

Bu yazılım hata ayıklayıcı İşbirlikçi bir hata ayıklama düzeni cihazında çalıştırılan gerektirir. Bu hata ayıklama kod hata ayıklamayı desteklemek için her dizisi noktada ek kod içerecek şekilde işaretlenir gibi daha büyük olacaktır zaman ikili dosyanız derlemeler anlamına gelir.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Konsol erişme

Kilitlenme günlükleri ve konsol sınıfı çıktısını iPhone konsola gönderilir. "Düzenleyici" kullanma ve Cihazınızı Düzenleyici'yi seçerek Xcode ile bu konsolu erişebilir.

Alternatif olarak, Xcode başlatmak istiyor musunuz, Apple'nın kullanabilirsiniz [iPhone yapılandırma yardımcı programını](http://www.apple.com/support/iphone/enterprise/) doğrudan konsoluna erişmek için. Bu alan için bir sorun hata ayıklama durumunda, konsol günlükleri bir Windows makineden erişebilir yöntemlerine sahiptir.

Visual Studio kullanıcılar için kullanılabilir birkaç günlükleri çıkış penceresinde, ancak daha kapsamlı ve ayrıntılı günlükleri için Mac geçmeniz.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Mono'nın sınıf kitaplıkları hata ayıklama

Mono'nın sınıf kitaplıkları için kaynak kodunu Xamarin.iOS birlikte ve şeyler başlık altında nasıl çalıştığınız görmek için hata ayıklayıcı tek adımda için bunu kullanabilirsiniz.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bu özellik, hata ayıklama sırasında daha fazla bellek tüketir olduğundan, bu varsayılan olarak kapalıdır.


Bu özelliği etkinleştirmek için emin olun **yalnızca proje kodunda hata ayıklama; framework koda adım yapmanız** seçeneği altında kaldırıldığında _Mac için Visual Studio > Tercihler > hata ayıklayıcı_ gösterildiği gibi menüsü Aşağıda:

[ ![](debugging-in-xamarin-ios-images/debugging6.png "Mono'nın sınıf kitaplıkları hata ayıklama")](debugging-in-xamarin-ios-images/debugging6.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Sınıf kitaplıkları Visual Studio'da hata ayıklamak için devre dışı bırakmalısınız **sadece kendi kodumu** altında _hata ayıklama > Seçenekler_ menüsü. İçinde _hata ayıklama > Genel_ düğümü, Temizle **sadece kendi kodumu etkinleştir** onay kutusu:

[ ![](debugging-in-xamarin-ios-images/debugging6vs.png "Mono'nın sınıf kitaplıkları hata ayıklama")](debugging-in-xamarin-ios-images/debugging6vs.png)

-----

Bunu yaptıktan sonra uygulama ve tek adımda Mono'nın çekirdek sınıf kitaplıkları hiçbirine başlatabilirsiniz.


## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin ile hata ayıklama](/visualstudio/mac/debugging/)
- [Veri Görselleştirmeleri](/visualstudio/mac/data-visualizations/)
- [Bir kesme noktası ayarlama](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/set_a_breakpoint/)
- [Kod aracılığıyla adım](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/step_through_code/)
- [Günlük penceresine çıkış bilgileri](https://developer.xamarin.com/recipes/cross-platform/ide/debugging/output_information_to_log_window/)
