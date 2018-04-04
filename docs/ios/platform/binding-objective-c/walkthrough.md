---
title: 'İzlenecek yol: bir iOS Objective-C Kitaplığı bağlama'
description: Bu makale varolan Objective-C Kitaplığı, InfColorPicker için bir Xamarin.iOS bağlama oluşturma uygulamalı bir kılavuz sağlar. Statik Objective-C Kitaplık derleme, onu bağlayarak ve bir Xamarin.iOS uygulaması'nda bağlama kullanma gibi konuları kapsar.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 7a25aa1043dcaf52406059d3fa184da36dc4875e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>İzlenecek yol: bir iOS Objective-C Kitaplığı bağlama

_Bu makale varolan Objective-C Kitaplığı, InfColorPicker için bir Xamarin.iOS bağlama oluşturma uygulamalı bir kılavuz sağlar. Statik Objective-C Kitaplık derleme, onu bağlayarak ve bir Xamarin.iOS uygulaması'nda bağlama kullanma gibi konuları kapsar._

İOS üzerinde çalışırken, bir üçüncü taraf Objective-C Kitaplığı kullanmak istediğiniz durumlarda karşılaşabilirsiniz. Bu durumlarda, bir Xamarin.iOS kullanabilirsiniz _bağlama proje_ oluşturmak için bir [C# bağlama](~/cross-platform/macios/binding/overview.md) , etmenizi sağlar Xamarin.iOS uygulamalarınızda bir kitaplığı kullanacak.

Genellikle iOS ekosistemi içinde 3 özellikleri kitaplıkları bulabilirsiniz:

* İle önceden derlenmiş statik kitaplık dosyası olarak `.a` kendi üstbilgi (.h dosyaları) ile birlikte uzantısı. Örneğin, [Google Analytics kitaplığı](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Önceden derlenmiş bir çerçeve. Yalnızca statik kitaplık, üstbilgiler ve bazen ek kaynaklar ile içeren bir klasör budur `.framework` uzantısı. Örneğin, [Google AdMob Kitaplığı](https://developers.google.com/admob/ios/download).
* Kaynak kodu dosyaları yeterlidir. Örneğin, yalnızca içeren bir kitaplık `.m` ve `.h` Objective C dosyaları.

Birinci ve İkinci senaryoda zaten olacaktır önceden derlenmiş CocoaTouch statik kitaplık bu makalede biz üçüncü senaryo odaklanacaktır şekilde. Başlamadan önce bir bağlama oluşturun, her zaman bu bağlamak boş olduğundan emin olmak için kitaplığı ile sağlanan lisans denetleyin unutmayın.

Bu makalede açık kaynak kullanarak bir bağlama proje oluşturma bir adım adım kılavuz sağlar [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C proje örnek olarak, bu kılavuzdaki tüm bilgiler ile kullanım için uyarlanabilir ancak üçüncü taraf Objective-C Kitaplığı. InfColorPicker kitaplığı renk seçim yaparak daha kullanıcı dostu kendi HSB gösterimi üzerinde bağlı bir renk seçmek kullanıcının sağlayan bir yeniden kullanılabilir görünüm denetleyicisi sağlar.

[![](walkthrough-images/run01.png "İOS çalıştıran InfColorPicker kitaplığı örneği")](walkthrough-images/run01.png#lightbox)

Bu belirli Xamarin.iOS Objective-C API kullanmak için gereken tüm adımları şu konulara değineceğiz:

- İlk olarak, Xcode kullanarak bir Objective-C statik kitaplık oluşturacağız.
- Sonra biz Xamarin.iOS statik bu kitaplıkla bağlamanız.
- Ardından, bazı (ancak tümünü değil) otomatik olarak oluşturarak hedefi Sharpie iş yükünü nasıl azaltabilir Göster Xamarin.iOS bağlama tarafından gerekli gerekli API tanımlarını.
- Son olarak, biz bağlama kullanan bir Xamarin.iOS uygulaması oluşturacaksınız.

Örnek uygulama InfColorPicker API bizim C# kodu arasında iletişim için güçlü bir temsilci kullanma gösterilmektedir. Güçlü bir temsilci kullanma gördük sonra biz zayıf temsilciler aynı görevleri gerçekleştirmek için nasıl kullanılacağını ele alacağız.

## <a name="requirements"></a>Gereksinimler

Bu makalede Xcode ve Objective-C dil aşina sahip ve okuduğunuz varsayılır bizim [bağlama Objective-C](~/cross-platform/macios/binding/index.md) belgeleri. Ayrıca, sunulan adımları tamamlamak için gerekli verilmiştir:

-  **Xcode ve iOS SDK'sı** -Apple'nın Xcode ve en son iOS API gereken yüklenmeli ve geliştirici bilgisayarda yapılandırılmış.
-  **[Xcode komut satırı araçları](#Installing_the_Xcode_Command_Line_Tools)**  -Xcode komut satırı araçlarını, Xcode (Yükleme ayrıntıları için aşağıya bakın) şu anda yüklü olan sürüm için yüklenmesi gerekir.
-  **Mac veya Visual Studio için Visual Studio** -Mac veya Visual Studio için Visual Studio en son sürümünü yüklenmeli ve geliştirme bilgisayarınızda yapılandırılmalıdır. Bir Apple Mac, bir Xamarin.iOS uygulaması geliştirmek için gereklidir ve Visual Studio kullanırken bağlanmanız gerekir [bir Xamarin.iOS yapı konağı](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **En son sürümünü hedefi Sharpie** -hedefi Sharpie aracının geçerli bir kopyasını indirilen [burada](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Hedefi yüklü Sharpie zaten varsa, en son sürüme kullanarak güncelleştirebilirsiniz `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode komut satırı araçlarını yükleme

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Yukarıda belirtildiği gibi biz Xcode komut satırı araçlarını kullanarak (özellikle `make` ve `lipo`) Bu kılavuzda. `make` Komutu yürütülebilir programları ve kitaplıklarını derlenmesini kullanarak otomatikleştirmek çok yaygın bir UNIX yardımcı olan bir _derleme görevleri dosyası_ program nasıl oluşturulması belirtir. `lipo` Komutu çok mimarisi dosyaları oluşturmak için bir OS X komut satırı yardımcı programı; birden çok birleştirir `.a` tüm donanım mimarileri tarafından kullanılan bir dosya dosyalarıyla.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Yukarıda belirtildiği gibi biz Xcode komut satırı araçları üzerinde kullanmaya başlayacağınız **Mac yapı konağı** (özellikle `make` ve `lipo`) Bu kılavuzda. `make` Komutu yürütülebilir programları ve kitaplıklarını derlenmesini kullanarak otomatikleştirmek çok yaygın bir UNIX yardımcı olan bir _derleme görevleri dosyası_ belirtir için program oluşturma. `lipo` Komutu çok mimarisi dosyaları oluşturmak için bir OS X komut satırı yardımcı programı; birden çok birleştirir `.a` tüm donanım mimarileri tarafından kullanılan bir dosya dosyalarıyla.


-----

Apple'nın göre [Xcode SSS ile komut satırından derleme](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) belgeleri, OS X 10.9 ve daha büyük, **indirmeleri** bölmesi, Xcode **Tercihler** iletişim değil artık yükleme komut satırı araçları destekler.

Araçlarını yüklemek için aşağıdaki yöntemlerden birini kullanmanız gerekir:

- **Xcode yükleme** - Xcode, yüklediğinizde sahip tüm komut satırı araçları ile birlikte gelen gelir. OS X 10.9 dolgular (yüklü `/usr/bin`), dahil herhangi bir aracı eşleyebilirsiniz `/usr/bin` Xcode içinde karşılık gelen aracına. Örneğin, `xcrun` bulunamadı veya Xcode içinde herhangi bir aracı komut satırından çalıştırma olanak tanıyan komutu.
- **Terminal uygulama** -Terminal uygulamadan komut satırı araçlarını çalıştırarak yükleyebilirsiniz `xcode-select --install` komutu:
    - Terminal uygulamayı başlatın.
    - Tür `xcode-select --install` ve basın **Enter**, örneğin:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - İstenir, komut satırı araçlarını Yükle'yi tıklatın **yükleme** düğmesi: [ ![ ] (walkthrough-images/xcode01.png "komut satırı araçlarını yükleme")](walkthrough-images/xcode01.png#lightbox)

    - Araçlar karşıdan yüklenir ve Apple'nın sunucularından kurulur: [ ![ ] (walkthrough-images/xcode02.png "araçları yükleme")](walkthrough-images/xcode02.png#lightbox)

- **Apple geliştiriciler için indirmeleri** -komut satırı araçları paketini kullanılabilir [Apple geliştiriciler için indirmeleri]() web sayfası. Oturum, Apple Kimliğiyle oturum sonra aramak ve komut satırı araçlarını indirmek: [ ![ ] (walkthrough-images/xcode03.png "komut satırı araçlarını bulma")](walkthrough-images/xcode03.png#lightbox)

Komut satırı araçları yüklü, biz Kılavuzu ile çalışmaya devam etmek hazırsınız.

## <a name="walkthrough"></a>İzlenecek yol

Bu kılavuzda, aşağıdaki adımları şu konulara değineceğiz:

- **[Bir statik kitaplık oluşturma](#Creating_A_Static_Library)**  -bir statik kitaplık oluşturma bu adımı içerir **InfColorPicker** Objective-C kodunu. Statik kitaplık olacaktır `.a` dosya uzantısı ve kitaplığı proje .NET derlemesi halinde katıştırılır.
- **[Xamarin.iOS bağlama proje oluşturma](#Create_a_Xamarin.iOS_Binding_Project)**  -statik kitaplık biz oluşturduktan sonra bir Xamarin.iOS bağlama projesi oluşturmak için kullanacağız. Meta veri Objective-C API nasıl kullanılabileceği açıklanır C# kodu biçiminde ve yeni oluşturduğumuz statik kitaplık, bağlama proje oluşur. Bu meta verileri, genellikle API tanımları da adlandırılır. Kullanacağız **[hedefi Sharpie](#Using_Objective_Sharpie)** API tanımları oluşturma bizimle yardımcı olmak için.
- **[API tanımlarını normalleştirin](#Normalize_the_API_Definitions)**  - hedefi Sharpie bize yardımcı olan bir harika iş yapar, ancak her şeyi yapamazsınız. Biz kullanılabilmesi için önce API tanımlarını yapmak için ihtiyacımız bazı değişiklikler ele alacağız.
- **[Bağlama kitaplığı kullanarak](#Using_the_Binding)**  -son olarak, yeni oluşturulan bağlama Projemizin kullanmayı göstermek için bir Xamarin.iOS uygulaması oluşturacağız.

Hangi adımların oynayan anladığınıza göre şimdi Kılavuzu geri kalanı için geçin.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Bir statik kitaplık oluşturma

Biz InfColorPicker github için kodu incelemek ise:

[![](walkthrough-images/image02.png "Github'da InfColorPicker için kodu inceleyin.")](walkthrough-images/image02.png#lightbox)

Proje şu üç dizinlerde görebiliriz:

- **InfColorPicker** -bu dizin proje Objective-C kodunu içerir.
- **PickerSamplePad** -bu dizin örnek iPad proje içerir.
- **PickerSamplePhone** -bu dizin örnek iPhone proje içerir.

Şimdi InfColorPicker projesinden indirmeniz [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) ve bizim seçme dizininde sıkıştırmasını açın. Xcode hedef açma `PickerSamplePhone` proje, biz Xcode Gezgini aşağıdaki proje yapısında bakın:

[![](walkthrough-images/image03.png "Xcode Gezgininde Proje yapısı")](walkthrough-images/image03.png#lightbox)

Bu proje InfColorPicker kaynak kodunda (kırmızı kutu) her örnek projesine doğrudan ekleyerek kodu yeniden kullanma erişir. Örnek proje kodunu içinde mavi kutusudur. Bu belirli projeye bize bir statik kitaplık ile sağlamadığından, için gerekli olan bize statik kitaplık derlemek için bir Xcode projesi oluşturun.

İlk adım, bize InfoColorPicker kaynak kodu statik kitaplığa eklemek içindir. Şimdi bunun için aşağıdakileri yapın:

1. Xcode başlatın.
2. Gelen **dosya** menüsünü seçin **yeni** > **proje...** :

    [![](walkthrough-images/image04.png "Yeni bir proje başlangıç")](walkthrough-images/image04.png#lightbox)
3. Seçin **Framework & Kitaplığı**, **Cocoa Touch statik kitaplık** şablonu ve tıklatın **sonraki** düğmesi:

    [![](walkthrough-images/image05.png "Cocoa Touch statik kitaplık şablonu seçin")](walkthrough-images/image05.png#lightbox)
4. Girin `InfColorPicker` için **proje adı** tıklatıp **sonraki** düğmesi:

    [![](walkthrough-images/image06.png "InfColorPicker proje adı girin")](walkthrough-images/image06.png#lightbox)
5. Projeyi kaydedin ve tıklayın için bir konum seçin **Tamam** düğmesi.
6. Şimdi biz statik kitaplık Projemizin InfColorPicker projeden kaynak eklemeniz gerekir. Çünkü **InfColorPicker.h** dosyası (varsayılan) bizim statik Kitaplığı'nda zaten, Xcode izin vermez bize üzerine yazmak. Gelen **Bulucu**, biz Github'dan sıkıştırması açılmış özgün projesini InfColorPicker kaynak kodunda gidin, tüm InfColorPicker dosyalarını kopyalayın ve yeni statik kitaplık Projemizin yapıştırın:

    [![](walkthrough-images/image12.png "Tüm InfColorPicker dosyalarını kopyalayın")](walkthrough-images/image12.png#lightbox)

7. Dönmek için Xcode, sağ tıklayın **InfColorPicker** klasörü ve select **"InfColorPicker..." dosyaları ekleme**:

    [![](walkthrough-images/image08.png "Dosya ekleme")](walkthrough-images/image08.png#lightbox)

8. Biz yalnızca kopyaladığınız InfColorPicker kaynak kodu dosyaları için dosyaları Ekle iletişim kutusundan gidin, tümünü seçin ve tıklayın **Ekle** düğmesi:

    [![](walkthrough-images/image09.png "Tümünü seçin ve Ekle düğmesini tıklatın")](walkthrough-images/image09.png#lightbox)

9. Kaynak kodu bizim projeye kopyalanacak:

    [![](walkthrough-images/image10.png "Kaynak kodu projeye kopyalanacak")](walkthrough-images/image10.png#lightbox)

10. Xcode Proje Gezgini seçin **InfColorPicker.m** (Bu kitaplığı yazıldı, bu dosyayı kullanılmaz biçimini nedeniyle) son iki satırı açıklamadan çıkarın ve dosya:

    [![](walkthrough-images/image14.png "InfColorPicker.m dosya düzenleme")](walkthrough-images/image14.png#lightbox)

11. Şimdi kitaplığı tarafından gerekli herhangi bir çerçeve olup olmadığını denetlemek ihtiyacımız. Benioku veya sağlanan örnek projelerinden birini açarak bu bilgileri bulabilirsiniz. Bu örnekte `Foundation.framework`, `UIKit.framework`, ve `CoreGraphics.framework` böylece bunları ekleyelim.

12. Seçin **InfColorPicker hedef > derleme aşamaları** ve genişletin **bağlantı ikiliyi kitaplıklara** bölümü:

    [![](walkthrough-images/image16b.png "Bağlantı ikiliyi kitaplıklara bölümü genişletin")](walkthrough-images/image16b.png#lightbox)

13. Kullanım **+** düğmesi gerekli çerçeveleri çerçeveleri yukarıda listelenen eklemenize olanak sağlayan iletişim kutusunu açmak için:

    [![](walkthrough-images/image16c.png "Yukarıda listelenen gerekli çerçeveleri çerçeveleri ekleyin")](walkthrough-images/image16c.png#lightbox)

14. **Bağlantı ikiliyi kitaplıklara** bölümü artık aşağıdaki görüntü gibi görünmelidir:

    [![](walkthrough-images/image16d.png "Bağlantı ikiliyi kitaplıklara bölümü")](walkthrough-images/image16d.png#lightbox)

Bu noktada Kapat çalışıyoruz, ancak henüz tam bitmek. Statik kitaplık oluşturuldu, ancak bir Fat ikili tüm gerekli mimari iOS cihazı ve iOS simülatörü için içeren oluşturmak üzere oluşturmamız gereken.

### <a name="creating-a-fat-binary"></a>Fat ikili dosya oluşturma

Tüm iOS cihazları, zaman içinde geliştirilmiş ARM mimarisi tarafından desteklenen işlemci yok. Her yeni bir mimari hala geriye dönük uyumluluk koruyarak yeni yönergeleri ve diğer geliştirmeler eklendi. İOS cihazlarda armv6, armv7, armv7s, arm64 yönerge kümelerini – rağmen sahibiz [artık armv6 kullanırız](~/ios/deploy-test/compiling-for-different-devices.md). İOS simülatörü ARM tarafından açık değildir ve istead bir x86 ve güç x86_64 simulator. Bize anlamına gelir ki olduğu biz kitaplıkları her yönerge sağlamanız gereken ayarlar.

Fat bir kitaplık `.a` tüm desteklenen mimariler içeren dosya.

Bir fat ikili oluşturma üç adım bir işlemdir:

- Bir statik kitaplık ARM 7 & ARM64 sürümü derleyin.
- Bir statik kitaplık x86 ve x84_64 sürümü derleyin.
- Kullanım `lipo` iki statik kitaplıklar bir nesnede birleştirme komut satırı aracı.

Tutarak bu üç adımı yerine basittir ve gelecekte Objective-C Kitaplığı güncelleştirmeler aldığında veya biz hata düzeltmeleri gerekiyorsa tekrarlamanız gerekebilir. Bu adımları otomatikleştirmek karar verirseniz, gelecek Bakım ve Destek iOS bağlama projesinin basitleştirir.

Birçok aracı gibi görevler - bir kabuk betiği otomatik hale getirmek kullanılabilir vardır [rake](http://rake.rubyforge.org/), [xbuild](http://www.mono-project.com/docs/tools+libraries/tools/xbuild/), ve [olun](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Biz Xcode komut satırı araçlarını, biz de olun, bu nedenle diğer bir deyişle bu kılavuz için kullanılacak yapı sistemi yüklenirken. Burada bir **derleme görevleri dosyası** , bir iOS aygıtı ve herhangi bir kitaplığı simülatörü çalışması çok mimarisi paylaşılan kitaplığı oluşturmak için kullanabilirsiniz:

```bash
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=./YOUR-PROJECT-NAME
PROJECT=$(PROJECT_ROOT)/YOUR-PROJECT-NAME.xcodeproj
TARGET=YOUR-PROJECT-NAME

all: lib$(TARGET).a

lib$(TARGET)-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

lib$(TARGET)-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET)-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

lib$(TARGET).a: lib$(TARGET)-i386.a lib$(TARGET)-armv7.a lib$(TARGET)-arm64.a
    xcrun -sdk iphoneos lipo -create -output $@ $^

clean:
    -rm -f *.a *.dll
```

Girin **derleme görevleri dosyası** seçtiğiniz, düz metin düzenleyicisinde komutları ve bölümlerle güncelleştirme **YOUR proje adı** projenizin adına sahip. Emin olmak önemlidir biz yönergeleri içinde sekmeler korunur, yukarıda verilen yönergeleri yapıştırın.

Dosya adıyla kaydedin **derleme görevleri dosyası** InfColorPicker Xcode statik yukarıda oluşturduğumuz kitaplığı olarak aynı konuma:

[![](walkthrough-images/lib00.png "Derleme görevleri dosyası adla dosyayı Kaydet")](walkthrough-images/lib00.png#lightbox)

Mac Terminal uygulamasını açın ve, derleme görevleri dosyası konumuna gidin. Tür `make` terminale basın **Enter** ve **derleme görevleri dosyası** yürütülecek:

[![](walkthrough-images/lib01.png "Örnek derleme görevleri dosyası çıktısı")](walkthrough-images/lib01.png#lightbox)

Yapma çalıştırdığınızda, çok fazla tarafından kaydırma metni görürsünüz. Her şeyin doğru şekilde çalışan, sözcükleri görürsünüz **yapı başarılı** ve `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` ve `libInfColorPickerSDK.a` dosyaları, aynı konuma kopyalanacak **derleme görevleri dosyası**:

[![](walkthrough-images/lib02.png "Derleme görevleri dosyası tarafından oluşturulan libInfColorPicker armv7.a, libInfColorPicker i386.a ve libInfColorPickerSDK.a dosyaları")](walkthrough-images/lib02.png#lightbox)

Aşağıdaki komutu kullanarak Fat ikili dosyanız mimarileri doğrulayabilirsiniz:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Bu, aşağıdaki görüntülenmelidir:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

Bu noktada, biz Xcode ve Xcode komut satırı araçlarını kullanarak bir statik kitaplık oluşturarak bizim iOS bağlama ilk adımı tamamlayana `make` ve `lipo`. Şimdi sonraki adımına geçmek ve kullanmak **hedefi Sharpie** bize için API bağlamaları oluşturulmasını otomatik hale getirmek için.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Bir Xamarin.iOS projesi bağlama oluşturma

Biz kullanmadan önce **hedefi Sharpie** bağlama işlemini otomatikleştirmek için API tanımlarını barındırmak için bir Xamarin.iOS bağlama projesi oluşturmak ihtiyacımız (biz kullanan **hedefi Sharpie** bize yardımcı olmak için Yapı) ve bize için C# bağlama oluşturun.

Şimdi aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'yu başlatın
1. Gelen **dosya** menüsünde, select **yeni** > **çözüm...** :

    ![](walkthrough-images/bind01.png "Yeni bir çözüm başlatılıyor")

1. Yeni bir çözüm iletişim kutusundan **Kitaplığı** > **iOS projesi bağlama**:

    ![](walkthrough-images/bind02.png "İOS Binding projesini seçin")

1. Tıklatın **sonraki** düğmesi.

1. "InfColorPickerBinding" olarak girin **proje adı** tıklatıp **oluşturma** düğmesi çözümü oluşturmak için:

    ![](walkthrough-images/bind02a.png "Proje adı olarak InfColorPickerBinding girin")

Çözüm oluşturulur ve iki varsayılan dosyalar dahil edilir:

![](walkthrough-images/bind03.png "Çözüm Gezgini'nde Çözüm yapısı")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Visual Studio'yu başlatın.

1. Gelen **dosya** menüsünde, select **yeni** > **proje...** :

    ![](walkthrough-images/bind01vs.png "Yeni bir proje başlangıç")

1. Yeni Proje iletişim kutusundan **iOS** > **bağlamaları Kitaplığı**:

    ![](walkthrough-images/bind02vs.png "İOS bağlamaları kitaplığı seçin")

1. "InfColorPickerBinding" olarak girin **adı** tıklatıp **Tamam** çözümü oluşturmak için düğmesi.

Çözüm oluşturulur ve iki varsayılan dosyalar dahil edilir:

![](walkthrough-images/bind03vs.png "Çözüm Gezgini'nde Çözüm yapısı")

-----



- **ApiDefinition.cs** -bu dosya nasıl Objective-C API'nin C# ' ta sarılır tanımlamak sözleşmeleri içerir.
- **Structs.cs** - bu dosyanın tüm yapıları tutacak veya numaralandırma değerlerinden oluşan arabirimleri ve temsilciler tarafından gereklidir.

Biz bu kılavuzda daha sonra iki dosyalarıyla çalışma. İlk olarak, biz bağlama projeye InfColorPicker kitaplığı eklemeniz gerekir.

### <a name="including-the-static-library-in-the-binding-project"></a>Statik kitaplık bağlama projeye dahil

Temel bağlama Projemizin hazır sahibiz artık oluşturduğumuz yukarıda için Fat ikili Kitaplığı eklemek ihtiyacımız **InfColorPicker** kitaplığı.

Kitaplığı eklemek için aşağıdaki adımları izleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Sağ **yerel başvuruları** seçin ve çözüm paneli klasöründe **yerel başvuru Ekle**:

    ![](walkthrough-images/bind04a.png "Yerel başvurular ekleyin")

1. Fat biz yapılan önceki ikili için gidin (`libInfColorPickerSDK.a`) ve basın **açık** düğmesi:

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK.a dosyasını seçin")
1. Dosyanın projede eklenecek:

    ![](walkthrough-images/bind04.png "Bir dosya da dahil olmak üzere")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopya `libInfColorPickerSDK.a` gelen, **Mac yapı konağı** bağlama projenize yapıştırın.

1. Projeye sağ tıklayın ve seçin **Ekle > varolan öğeyi...** :

    ![](walkthrough-images/bind04vs.png "Varolan bir dosya ekleme")

1. Gidin `libInfColorPickerSDK.a` ve basın **Ekle** düğmesi:

    ![](walkthrough-images/bind05vs.png "LibInfColorPickerSDK.a ekleme")

1. Dosya projeye dahil edilir.

-----


Bir dosya projeye eklendiğinde, Xamarin.iOS otomatik olarak ayarlar **yapı eylemi** dosyasını **ObjcBindingNativeLibrary**ve adlı özel bir dosya oluşturun `libInfColorPickerSDK.linkwith.cs`.


Bu dosyayı içeren `LinkWith` nasıl tanıtıcı biz yalnızca statik kitaplığa eklenen Xamarin.iOS söyler özniteliği. Bu dosyanın içeriğini aşağıdaki kod parçacığında gösterilmektedir:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` Özniteliği proje ve bazı önemli bir bağlayıcı bayrağı için statik kitaplık tanımlar.


Yapmamız gereken sonraki InfColorPicker proje için API tanımları oluşturmak üzere şeydir. Bu kılavuzda amaçları doğrultusunda, biz hedefi Sharpie dosya oluşturmak için kullanacağı **ApiDefinition.cs**.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Amaç Sharpie kullanma

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Amaç Sharpie olan bir komut satırı (Xamarin tarafından sağlanan) aracı, 3 taraf Objective-C Kitaplık C# bağlamak için gereken tanımları oluşturmanıza yardımcı. Bu bölümde, hedefi Sharpie ilk oluşturmak için kullanacağız **ApiDefinition.cs** InfColorPicker projesi için.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Amaç Sharpie olan bir komut satırı (Xamarin tarafından sağlanan) aracı, 3 taraf Objective-C Kitaplık C# bağlamak için gereken tanımları oluşturmanıza yardımcı. Bu bölümde, hedefi Sharpie üzerinde kullanacağız bizim **Mac yapı konağı** ilk oluşturmak için **ApiDefinition.cs** InfColorPicker projesi için.


-----

Başlamak için şimdi bu ayrıntılı olarak hedefi Sharpie yükleyici dosyasını indirin [Kılavuzu](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Yükleyiciyi çalıştırın ve ekrandaki yönergeleri tümünün hedefi Sharpie bizim geliştirme bilgisayara yüklemek için Yükleme Sihirbazı'ndan izleyin.

Biz hedefi Sharpie başarıyla oluşturduktan sonra yüklü, şimdi Terminal uygulamayı başlatın ve tüm bağlamasında yardımcı olmak için sağladığı araçları hakkında Yardım almak için aşağıdaki komutu girin:

```bash
sharpie -help
```

Biz yukarıdaki komutu çalıştırırsanız, aşağıdaki çıkış oluşturulur:

```bash
Europa:Resources kmullins$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, --helpShow detailed help
  -v, --versionShow version information

Available Tools:

  xcode    Get information about Xcode installations and available SDKs.

  bind     Create a Xamarin C# binding to Objective-C APIs
Europa:Resources kmullins$
```

Bu izlenecek amacıyla, biz aşağıdaki hedefi Sharpie araçları kullanacaktır:

- **xcode** - bu araçları bize geçerli bizim Xcode yükleme ve iOS ve Mac API'leri, biz yüklü sürümleri hakkında bilgi sağlar. Biz bizim bağlamaları oluşturduğunuzda biz daha sonra bu bilgileri kullanarak.
- **Bağlama** -bu aracı ayrıştırmak için kullanacağız **.h** ilk InfColorPicker projeye dosyalarında **ApiDefinition.cs** ve **StructsAndEnums.cs** dosyaları.

Belirli bir amaç Sharpie aracını kullanarak Yardım almak için aracın adını girin ve `-help` seçeneği. Örneğin, `sharpie xcode -help` aşağıdaki çıktıyı döndürür:

```bash
Europa:Resources kmullins$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]+

Options:
  -h, --help                 Show detailed help
  -v, --verbose              Be verbose with output
      --sdks                 List all available Xcode SDKs. Pass -verbose for
                               more details.
Europa:Resources kmullins$
```

Bağlama işlemi başlayabilmeniz için önce terminale aşağıdaki komutu girerek bizim geçerli yüklü SDK'ları hakkında bilgi almak ihtiyacımız `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Yukarıdaki biz olduğunu görebiliriz `iphoneos8.1` SDK bizim makinede yüklü. Yerinde bu bilgilerle biz InfColorPicker proje ayrıştırmak hazır `.h` ilk dosyalarıyla **ApiDefinition.cs** ve `StructsAndEnums.cs` InfColorPicker projesi için.

Aşağıdaki komutu girin Terminal uygulama:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Burada `[full-path-to-project]` dizinin tam yolu burada **InfColorPicker** Xcode proje dosyası, bilgisayarda bulunan ve [iphone-os] olan iOS biz yüklediyseniz, SDK'sı tarafından belirtildiği gibi `sharpie xcode -sdks` komutu. Bu örnekte, biz başarıyla geçmenizin Not  **\*.h** bir parametresi olarak içeren *tüm* bu dizinde - başlık dosyaları normalde, bunu yapmak, ancak bunun yerine dikkatle okuyun üst düzey bulmak için üstbilgi dosyaları **.h** tüm diğer ilgili dosyaları başvuran ve hedefi Sharpie geçirmeniz yeterlidir, dosya.

Aşağıdaki [çıkış](walkthrough-images/os05.png) terminale oluşturulur:

```bash
Europa:Resources kmullins$ sharpie bind -output InfColorPicker -namespace InfColorPicker -sdk iphoneos8.1 /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h -unified
Compiler configuration:
    -isysroot /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk -miphoneos-version-min=8.1 -resource-dir /Library/Frameworks/ObjectiveSharpie.framework/Versions/1.1.1/clang-resources -arch armv7 -ObjC

[  0%] parsing /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h
In file included from /Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPicker.h:60:
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* sourceColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:28:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: no 'assign',
      'retain', or 'copy' attribute is specified - 'assign' is assumed [-Wobjc-property-no-attribute]
@property (nonatomic) UIColor* resultColor;
^
/Users/kmullins/Projects/InfColorPicker/InfColorPicker/InfColorPickerController.h:29:1: warning: default property
      attribute 'assign' not appropriate for non-GC object [-Wobjc-property-no-attribute]
4 warnings generated.
[100%] parsing complete
[bind] InfColorPicker.cs
Europa:Resources kmullins$
```

Ve **InfColorPicker.enums.cs** ve **InfColorPicker.cs** dosyaları müşterilerimize Directory'de oluşturulur:

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs ve InfColorPicker.cs dosyaları")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Bu dosyaların her ikisini de yukarıda oluşturduğumuz bağlama projeyi açın. İçeriğini kopyalayın **InfColorPicker.cs** dosya ve yapıştırın **ApiDefinition.cs** dosyası, mevcut değiştirme `namespace ...` kod bloğu içeriğini  **InfColorPicker.cs** dosyası (bırakarak `using` deyimleri olduğu gibi):

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate dosyası")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Bu dosyaların her ikisini de yukarıda oluşturduğumuz bağlama projeyi açın. İçeriğini kopyalayın **InfColorPicker.cs** dosyası (gelen **Mac yapı konağı**) ve yapıştırın **ApiDefinition.cs** dosyası, mevcut değiştirme `namespace ...` kod bloğu içeriğini **InfColorPicker.cs** dosyası (bırakarak `using` deyimleri olduğu gibi).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API tanımlarını normalleştirin

Amaç Sharpie bazen olan bir sorunu çevirme `Delegates`, tanımını değiştirmek ihtiyacımız şekilde `InfColorPickerControllerDelegate` arabirim ve değiştirme `[Protocol, Model]` aşağıdaki satırla:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
Böylece tanımı aşağıdaki gibi görünür:

[![](walkthrough-images/os11.png "Tanımı")](walkthrough-images/os11.png#lightbox)

Ardından, biz içeriğini aynı işlevi görür `InfColorPicker.enums.cs` dosya kopyalama ve yapıştırma bunları `StructsAndEnums.cs` bırakarak dosya `using` deyimleri kalır:

[![](walkthrough-images/os09.png "İçeriği StructsAndEnums.cs dosyası ")](walkthrough-images/os09.png#lightbox)

Hedefi Sharpie bağlamayla ek açıklama bulabilirsiniz `[Verify]` öznitelikleri. Bu öznitelikler hedefi Sharpie (, ilişkili bildiriminin üstüne bir yorum sağlanacak) özgün C/Objective-C bildirimine bağlamayla karşılaştırarak doğru olanı vermedi doğrulamalısınız gösterir. Bağlamaları doğruladıktan sonra doğrulama özniteliği kaldırmanız gerekir. Daha fazla bilgi için bkz [doğrula](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) Kılavuzu.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Bu noktada, bağlama Projemizin, tam ve oluşturmak için hazır olması gerekir. Şimdi bizim bağlama projeyi oluşturun ve şu hata ile sona emin olun:

[Bağlama projeyi oluşturun ve hiçbir hata bulunmadığından emin olun](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Bu noktada, bağlama Projemizin, tam ve oluşturmak için hazır olması gerekir. Şimdi bizim bağlama projeyi oluşturun ve şu hata ile sona emin olun.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Bağlama işlemini kullanma

Bağlama kitaplığı yukarıda oluşturduğunuz iOS kullanmak için bir örnek iPhone uygulaması oluşturmak için aşağıdaki adımları izleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. **Xamarin.iOS projesi oluşturma** -adlı yeni bir Xamarin.iOS projesi eklemek **InfColorPickerSample** aşağıdaki ekran görüntülerinde gösterildiği gibi çözüme:

    ![](walkthrough-images/use01.png "Bir tek görünüm uygulaması ekleme")

    ![](walkthrough-images/use01a.png "Tanımlayıcı ayarlama")

1. **Bağlama projesine başvuru ekleyin** -güncelleştirme **InfColorPickerSample** bir başvuru içeriyor böylece proje **InfColorPickerBinding** proje:

    ![](walkthrough-images/use02.png "Bağlama projesine başvuru ekleme")

1. **İPhone kullanıcı arabirimi oluşturma** -çift tıklatın **MainStoryboard.storyboard** dosyasını **InfColorPickerSample** iOS Tasarımcısı düzenlemek için proje. Ekleme bir **düğmesini** görüntülemek ve çağrısından `ChangeColorButton`aşağıda gösterildiği gibi:

    ![](walkthrough-images/use03.png "Görünüme düğme ekleme")
1. **InfColorPickerView.xib ekleme** -InfColorPicker Objective-C Kitaplığı içeren bir **.xib** dosya. Xamarin.iOS değil içereceği bu **.xib** bağlama projesinde bizim örnek uygulama çalışma zamanı hataları neden olacak. Geçici çözüm bu eklemektir **.xib** Xamarin.iOS Projemizin dosyasına. Xamarin.iOS projesi, sağ tıklatın ve seçin seçin **Ekle > dosyaları Ekle**ve ekleme **.xib** dosya aşağıdaki ekran görüntüsünde gösterildiği gibi:

    ![](walkthrough-images/use04.png "InfColorPickerView.xib Ekle")

1. Sorulduğunda, kopyalama **.xib** proje dosyasına.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. **Xamarin.iOS projesi oluşturma** -adlı yeni bir Xamarin.iOS projesi eklemek **InfColorPickerSample** aşağıdaki ekran görüntüsünde gösterildiği gibi çözüme:

    ![](walkthrough-images/use01vs.png "Create Xamarin.iOS Project")

1. **Bağlama projesine başvuru ekleyin** -güncelleştirme **InfColorPickerSample** bir başvuru içeriyor böylece proje **InfColorPickerBinding** proje:

    ![](walkthrough-images/use02vs.png "Bağlama projesine başvuru ekleyin")

1. **İPhone kullanıcı arabirimi oluşturma** -çift tıklatın **MainStoryboard.storyboard** dosyasını **InfColorPickerSample** iOS Tasarımcısı düzenlemek için proje. Ekleme bir **düğmesini** görüntülemek ve çağrısından `ChangeColorButton`aşağıda gösterildiği gibi:

    ![](walkthrough-images/use03vs.png "İPhone kullanıcı arabirimi oluşturma")

1. **InfColorPickerView.xib ekleme** -InfColorPicker Objective-C Kitaplığı içeren bir **.xib** dosya. Xamarin.iOS değil içereceği bu **.xib** bağlama projesinde bizim örnek uygulama çalışma zamanı hataları neden olacak. Geçici çözüm bu eklemektir **.xib** bizim Xamarin.iOS projeden dosyasına bizim **Mac yapı konağı**. Xamarin.iOS projesi, sağ tıklatın ve seçin seçin **Ekle** > **varolan öğeyi...** ve ekleme **.xib** dosya.


-----



Ardından, Objective-C ve nasıl bunları bağlama ve C# kodu işleme protokollerin hızlı bir göz atalım.

### <a name="protocols-and-xamarinios"></a>Protokoller ve Xamarin.iOS

Objective-C, yöntemler (veya iletileri) bir protokol tanımlayan belirli durumlarda kullanılabilir. Kavramsal olarak, bunlar C# arabirimleri için çok benzer. Objective-C protokolü ve C# arabirimi arasındaki temel farklardan biri protokolleri isteğe bağlı yöntemler - uygulamak için bir sınıf yok yöntemleri sahip olabilmeleridir. Objective-C kullanan @optional anahtar sözcüğü hangi yöntemlerin isteğe bağlı olduğunu belirtmek için kullanılır. Protokolleri hakkında daha fazla bilgi için bkz: [olaylar, protokolleri ve Temsilciler](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** aşağıdaki kod parçacığında gösterildiği bir tür protokolü, sahiptir:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Bu protokolü tarafından kullanılan **InfColorPickerController** kullanıcı yeni bir renk ve, seçmiş olan istemcileri bildirmek için **InfColorPickerController** tamamlandı. Amaç Sharpie, aşağıdaki kod parçacığında gösterildiği gibi bu protokolü eşlenmiş:

```csharp
[BaseType(typeof(NSObject))]
[Model]
public partial interface InfColorPickerControllerDelegate {

    [Export ("colorPickerControllerDidFinish:")]
    void ColorPickerControllerDidFinish (InfColorPickerController controller);

    [Export ("colorPickerControllerDidChangeColor:")]
    void ColorPickerControllerDidChangeColor (InfColorPickerController controller);
}

```

Bağlama kitaplığı derlendiğinde Xamarin.iOS adlı Özet temel sınıf oluşturacak `InfColorPickerControllerDelegate`, sanal yöntemleriyle bu arabirimi uygular.

Biz bu arabirimi bir Xamarin.iOS uygulaması uygulayabilirsiniz iki yolu vardır:

- **Güçlü temsilci** -güçlü bir temsilci kullanarak içerir o alt sınıfların bir C# sınıfı oluşturma `InfColorPickerControllerDelegate` ve uygun yöntemlerini geçersiz kılar. **InfColorPickerController** bu sınıfının bir örneği, istemcilerle iletişim kurmak için kullanır.
- **Zayıf temsilci** -zayıf bir temsilci genel yöntem üzerinde bazı sınıf oluşturursunuz biraz farklı bir tekniktir (gibi `InfColorPickerSampleViewController`) ve bu yönteme gösterme `InfColorPickerDelegate` aracılığıyla protokolü bir `Export` özniteliği.

Güçlü temsilciler IntelliSense, tür güvenliği ve daha iyi kapsülleme sağlar. Bu nedenlerle, zayıf bir temsilci yerine yapabilmenizi güçlü temsilciler kullanmanız gerekir.

Bu kılavuzda iki tekniği aşağıdakiler ele alınacaktır: ilk güçlü bir temsilci uygulama ve zayıf bir temsilci uygulamak nasıl açıklayan.

### <a name="implementing-a-strong-delegate"></a>Güçlü bir temsilci uygulama

Xamarin.iOS uygulaması yanıtlamak için güçlü bir temsilci kullanarak son `colorPickerControllerDidFinish:` ileti:

**Subclass InfColorPickerControllerDelegate** - Add a new class to the project called `ColorSelectedDelegate`. Sınıfı, aşağıdaki kodu olacak biçimde düzenleyin:

```csharp
using InfColorPickerBinding;
using UIKit;

namespace InfColorPickerSample
{
    public class ColorSelectedDelegate:InfColorPickerControllerDelegate
    {
        readonly UIViewController parent;

        public ColorSelectedDelegate (UIViewController parent)
        {
            this.parent = parent;
        }

        public override void ColorPickerControllerDidFinish (InfColorPickerController controller)
        {
            parent.View.BackgroundColor = controller.ResultColor;
            parent.DismissViewController (false, null);
        }
    }
}
```

Xamarin.iOS bağlamak Objective-C temsilci olarak adlandırılan bir soyut taban sınıfı oluşturarak `InfColorPickerControllerDelegate`. Bir alt kümesi bu türe ve geçersiz kılma `ColorPickerControllerDidFinish` değerini erişmek için yöntemi `ResultColor` özelliği `InfColorPickerController`.

**ColorSelectedDelegate bir örneğini oluşturmak** -bizim olay işleyicisi örneği gerekir `ColorSelectedDelegate` önceki adımda oluşturduğumuz türü. Sınıf Düzenle `InfColorPickerSampleViewController` ve sınıfına aşağıdaki örnek değişkeni ekleyin:

```csharp
ColorSelectedDelegate selector;
```

**ColorSelectedDelegate değişkeni başlatmak** - emin olmak için `selector` geçerli bir örneği, güncelleştirme yöntemi `ViewDidLoad` içinde `ViewController` aşağıdaki kod parçacığında eşleştirmek için:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**HandleTouchUpInsideWithStrongDelegate yöntemi uygulaması** -sonraki zaman kullanıcı dokunur için olay işleyicisini uygulamak **ColorChangeButton**. Düzen `ViewController`ve aşağıdaki yöntemi ekleyin:

```csharp
using InfColorPicker;
...

private void HandleTouchUpInsideWithStrongDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.Delegate = selector;
    picker.PresentModallyOverViewController (this);
}

```

Biz öncelikle bir örneği elde `InfColorPickerController` bir statik yöntem ve güçlü bizim temsilci özelliği aracılığıyla farkında örnek yap aracılığıyla `InfColorPickerController.Delegate`. Bu özellik otomatik olarak bize hedefi Sharpie tarafından oluşturuldu. Son olarak diyoruz `PresentModallyOverViewController` görünüm gösterileceğini `InfColorPickerSampleViewController.xib` böylece kullanıcı bir renk seçebilirsiniz.

**Uygulamayı çalıştırmak** - biz tamamladıktan tüm kodumuza bu noktada. Uygulama çalıştırırsanız, arka plan rengini değiştirin yapabiliyor olmanız gerekir `InfColorColorPickerSampleView` aşağıdaki ekran görüntülerinde gösterildiği gibi:

[![](walkthrough-images/run01.png "Uygulamayı çalıştırma")](walkthrough-images/run01.png#lightbox)

Tebrikler! Bu noktada artık başarıyla oluşturuldu ve bir Xamarin.iOS uygulaması kullanmak için bir Objective-C Kitaplığı bağlı. Ardından, şimdi zayıf temsilcileri kullanma hakkında bilgi edinin.

### <a name="implementing-a-weak-delegate"></a>Zayıf bir temsilci uygulama

Bir sınıf için belirli bir temsilci Objective-C protokole bağlı sınıflara yerine Xamarin.iOS Ayrıca, Protokolü yöntemleri türetilen herhangi bir sınıf uygulama olanak tanır `NSObject`, yöntemlerinizi ile dekorasyon `ExportAttribute`ve ardından uygun seçiciler sağlama. Bu yaklaşımı seçtiğinizde sınıfı örneği atamak `WeakDelegate` özelliği için yerine `Delegate` özelliği. Zayıf bir temsilci temsilci sınıfınız farklı bir devralma hiyerarşisi aşağı olabilmesi için esneklik sunar. Uygulama ve Xamarin.iOS uygulamamız zayıf bir temsilci kullanma görelim.

**Olay işleyicisi için TouchUpInside oluşturma** -için yeni bir olay işleyicisi oluşturalım `TouchUpInside` arka plan rengini Değiştir düğmesinin olayı. Bu işleyici olarak aynı rol doldurur `HandleTouchUpInsideWithStrongDelegate` size önceki bölümde oluşturduğunuz ancak zayıf bir temsilci yerine güçlü bir temsilci kullanacağı işleyici. Sınıf Düzenle `ViewController`ve aşağıdaki yöntemi ekleyin:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**ViewDidLoad güncelleştirme** -değiştirmeniz gerekir `ViewDidLoad` böylece yeni oluşturduğumuz olay işleyicisi kullanır. Düzen `ViewController` değiştirip `ViewDidLoad` aşağıdaki kod parçacığını benzeyecek şekilde:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**ColorPickerControllerDidFinish işlemek: ileti** - `ViewController` olan tamamlandı, iOS mesajı göndereceğiz `colorPickerControllerDidFinish:` için `WeakDelegate`. Biz bu iletiyi işleyen bir C# yöntemi oluşturmanız gerekir. Bunu yapmak için bir C# yöntem oluşturma ve onunla ekleyin `ExportAttribute`. Düzen `ViewController`ve sınıfına aşağıdaki yöntemi ekleyin:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Uygulamayı çalıştırın. Artık tam olarak önceden olduğu, ancak yerine güçlü temsilci zayıf bir temsilci kullanıyor gibi davranması. Bu noktada, bu kılavuzda başarıyla tamamladınız. Şimdi oluşturmak ve bir Xamarin.iOS bağlama proje kullanmak nasıl bir anlamış olmanız gerekir.

## <a name="summary"></a>Özet

Bu makalede oluşturma ve Xamarin.iOS bağlama proje kullanma sürecinde gitti. Önce bir statik kitaplık içine var olan bir Objective-C Kitaplık derlemek nasıl ele. Ardından bir Xamarin.iOS bağlama projesinin nasıl oluşturulacağı ve hedefi Sharpie Objective-C Kitaplığı için API tanımları oluşturmak nasıl ele. Biz güncelleştirin ve bunları ortak tüketimi için uygun hale getirmek için oluşturulan API tanımlarını ince ayar açıklanır. Xamarin.iOS bağlama proje tamamlandıktan sonra Biz bu bağlama bir Xamarin.iOS uygulaması, güçlü Temsilciler ve zayıf temsilciler kullanarak odaklanmayı tüketen için taşındı.

## <a name="related-links"></a>İlgili bağlantılar

- [Bağlama örneği (örnek)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Objective-C Kitaplıklarını Bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Bağlama Ayrıntıları](~/cross-platform/macios/binding/overview.md)
- [Bağlama türü Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C geliştiriciler için Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [Çerçeve Tasarım Yönergeleri](http://msdn.microsoft.com/en-us/library/ms229042.aspx)
