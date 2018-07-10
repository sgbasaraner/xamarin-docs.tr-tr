---
title: 'İzlenecek yol: bir iOS Objective-C kitaplığını bağlama'
description: Bu makalede, bir Xamarin.iOS bağlaması için mevcut bir Objective-C kitaplığını InfColorPicker oluşturmayla ilgili uygulamalı bir kılavuz sağlar. Bu, statik bir Objective-C kitaplığını derleme, bağlama ve bir Xamarin.iOS uygulaması'nda bağlama kullanma gibi konular ele alınmaktadır.
ms.prod: xamarin
ms.assetid: D3F6FFA0-3C4B-4969-9B83-B6020B522F57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2017
ms.openlocfilehash: 8285a82920f0d95a88855c5257535048c6de41d5
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854863"
---
# <a name="walkthrough-binding-an-ios-objective-c-library"></a>İzlenecek yol: bir iOS Objective-C kitaplığını bağlama

_Bu makalede, bir Xamarin.iOS bağlaması için mevcut bir Objective-C kitaplığını InfColorPicker oluşturmayla ilgili uygulamalı bir kılavuz sağlar. Bu, statik bir Objective-C kitaplığını derleme, bağlama ve bir Xamarin.iOS uygulaması'nda bağlama kullanma gibi konular ele alınmaktadır._

İOS üzerinde çalışırken, bir üçüncü taraf Objective-C kitaplığını kullanmak istediğiniz durumlarda karşılaşabilirsiniz. Bu durumlarda, bir Xamarin.iOS kullanabileceğiniz _bağlama projesi_ oluşturmak için bir [C# bağlama](~/cross-platform/macios/binding/overview.md) eklemenize olanak tanıyan, Xamarin.iOS uygulamalarınızda kitaplığı kullanmak.

Genellikle iOS ekosisteminde 3 model olarak sağlanır kitaplıkları bulabilirsiniz:

* İle önceden derlenmiş bir statik kitaplık dosyası olarak `.a` uzantısı birlikte kendi üstbilgi (.h dosyaları). Örneğin, [Google Analytics kitaplığı](https://developers.google.com/analytics/devguides/collection/ios/v3/sdk-download?hl=es#download_sdk)
* Önceden derlenmiş bir çerçeve. Statik kitaplık, üst bilgileri ve bazen ek kaynaklarını içeren bir klasör budur `.framework` uzantısı. Örneğin, [Google'nın AdMob Kitaplığı](https://developers.google.com/admob/ios/download).
* Kaynak kodu dosyaları yeterlidir. Örneğin, yalnızca içeren bir kitaplık `.m` ve `.h` Objective C dosyaları.

Birinci ve İkinci senaryoda zaten olacaktır önceden derlenmiş CocoaTouch statik kitaplık için bu makaledeki biz üçüncü senaryoya odaklanır. Unutmayın, başlamadan önce farklı bir bağlama oluşturun, her zaman bu bağlamak boş olduğundan emin olmak için şu kitaplıkla sağlanan lisans denetleyin.

Bu makalede, açık kaynak kullanarak bir bağlama projesi oluşturma adım adım kılavuz sağlanır [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) Objective-C, bu kılavuzdaki tüm bilgileri ile kullanılmak üzere uyarlanabilir ancak örnek olarak, proje üçüncü taraf Objective-C kitaplığını. Renk seçim daha kullanıcı dostu yaparak HSB gösterimine alarak bir rengi seçmesini sağlayan bir yeniden kullanılabilir görünüm denetleyicisi InfColorPicker kitaplık sağlar.

[![](walkthrough-images/run01.png "İos'ta çalışan InfColorPicker kitaplık örneği")](walkthrough-images/run01.png#lightbox)

Biz bu belirli Xamarin.iOS Objective-C API'yi kullanmak için gerekli tüm adımları ele alacağız:

- İlk olarak, Xcode kullanarak Objective-C statik kitaplık oluşturacağız.
- Ardından, biz bu statik kitaplık Xamarin.iOS ile bağlamak.
- Ardından, bazı (ancak hepsini değil) otomatik olarak oluşturarak hedefi Sharpie iş yükünü nasıl azaltabilir Göster Xamarin.iOS bağlama tarafından gerekli gerekli API tanımlarını.
- Son olarak, bağlama kullanan bir Xamarin.iOS uygulaması oluşturacağız.

Örnek uygulamayı InfColorPicker API ile C# kodumuz arasındaki iletişim için güçlü bir temsilci kullanmayı gösterilecektir. Güçlü bir temsilci kullanmayı gördük sonra biz zayıf temsilciler aynı görevleri gerçekleştirmek için nasıl kullanılacağı ele alacağız.

## <a name="requirements"></a>Gereksinimler

Bu makalede, Xcode ve Objective-C dili bazı konusunda varsa ve okuduğunuz varsayılır bizim [Objective-C bağlama](~/cross-platform/macios/binding/index.md) belgeleri. Ayrıca, sunulan adımları tamamlamak için gerekli verilmiştir:

-  **Xcode ve iOS SDK'sı** -Apple'nın Xcode ve en son iOS API gereken yüklenmeli ve geliştiricinin bilgisayarında yapılandırılmalıdır.
-  **[Xcode komut satırı araçları](#Installing_the_Xcode_Command_Line_Tools)**  -Xcode komut satırı araçları, Xcode (Yükleme ayrıntıları için aşağıya bakın) şu anda yüklü sürümü yüklenmelidir.
-  **Visual Studio veya Mac için Visual Studio** -Visual Studio veya Mac için Visual Studio'nun en son sürümü yüklenmeli ve geliştirme bilgisayarında yapılandırılmış. Bir Apple Mac, bir Xamarin.iOS uygulaması geliştirmek için gereklidir ve Visual Studio kullanarak bağlanmanız gerekir [bir Xamarin.iOS yapı konağı](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
-  **En son sürümünü hedefi Sharpie** -hedefi Sharpie aracının geçerli bir kopyasını indirilen [burada](~/cross-platform/macios/binding/objective-sharpie/get-started.md). Hedefi yüklü Sharpie zaten varsa, bu en son sürüme kullanarak güncelleştirebilirsiniz `sharpie update`

<a name="Installing_the_Xcode_Command_Line_Tools"/>

## <a name="installing-the-xcode-command-line-tools"></a>Xcode komut satırı araçlarını yükleme

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Yukarıda belirtildiği gibi Xcode komut satırı araçları kullanacağız (özellikle `make` ve `lipo`) Bu izlenecek yolda. `make` Komutu yürütülebilir programlar ve kitaplıkları derlemesi kullanarak otomatikleştirmek çok yaygın bir UNIX yardımcı olan bir _derleme görevleri dosyası_ programın nasıl oluşturulması gerektiğini belirtir. `lipo` Çok mimari dosyaları oluşturmak için bir OS X komut satırı yardımcı programı bir komuttur; birden çok birleştirecek `.a` dosyalarıyla tüm donanım mimarileri tarafından kullanılan bir dosya.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Yukarıda belirtildiği gibi Xcode komut satırı araçları hakkında kullanacağız **Mac derleme konağı** (özellikle `make` ve `lipo`) Bu izlenecek yolda. `make` Komutu yürütülebilir programlar ve kitaplıkları derlemesi kullanarak otomatikleştirmek çok yaygın bir UNIX yardımcı olan bir _derleme görevleri dosyası_ sonlanması için program oluşturmayı da öğrenin. `lipo` Çok mimari dosyaları oluşturmak için bir OS X komut satırı yardımcı programı bir komuttur; birden çok birleştirecek `.a` dosyalarıyla tüm donanım mimarileri tarafından kullanılan bir dosya.


-----

Apple'nın göre [Xcode SSS ile komut satırından derleme](https://developer.apple.com/library/ios/technotes/tn2339/_index.html) belgeleri, OS X 10.9 ve üzeri, **indirir** Xcode bölmesinde **tercihleri** iletişim yok Artık, yükleme komut satırı araçlarını destekler.

Araçları yüklemek için aşağıdaki yöntemlerden birini kullanmanız gerekir:

- **Xcode'u yüklemeyi** - Xcode yüklediğinizde bu, komut satırı araçlarıyla birlikte sunulur. OS X 10.9 içinde shims (yüklü `/usr/bin`), dahil herhangi bir aracı eşleyebilirsiniz `/usr/bin` Xcode içinde karşılık gelen aracı. Örneğin, `xcrun` komutu bulun veya komut satırından Xcode içinde herhangi bir aracı çalıştırmanızı sağlar.
- **Terminal uygulama** -Terminal uygulamayı, komut satırı araçlarını çalıştırarak yükleyebilirsiniz `xcode-select --install` komutu:
    - Terminal uygulamasını başlatın.
    - Tür `xcode-select --install` basın **Enter**, örneğin:

    ```bash
    Europa:~ kmullins$ xcode-select --install
    ```

    - Açmanız istenir komut satırı araçları yüklemek için **yükleme** düğmesi: [ ![ ] (walkthrough-images/xcode01.png "komut satırı araçlarını yükleme")](walkthrough-images/xcode01.png#lightbox)

    - Araçlar indirilir ve Apple'nın sunucularından yüklü: [ ![ ] (walkthrough-images/xcode02.png "araçları indirme")](walkthrough-images/xcode02.png#lightbox)

- **Apple geliştiriciler için indirmeleri** -komut satırı araçları paketi kullanılabilir [Apple geliştiriciler için indirmeleri]() web sayfası. Oturum, Apple ID ile oturum ardından aramak ve komut satırı araçları indirin: [ ![ ] (walkthrough-images/xcode03.png "komut satırı araçlarını bulma")](walkthrough-images/xcode03.png#lightbox)

Yüklü olan komut satırı araçları ile adım adım kılavuza devam hazırız.

## <a name="walkthrough"></a>İzlenecek yol

Bu kılavuzda, aşağıdaki adımları şu konulara değineceğiz:

- **[Statik kitaplık oluşturma](#Creating_A_Static_Library)**  -Bu adım, bir statik kitaplık oluşturma içerir **InfColorPicker** Objective-C kodu. Statik kitaplık olacaktır `.a` dosya uzantısı ve kitaplık projesinin .NET bütünleştirilmiş kod içine katıştırılır.
- **[Bir Xamarin.iOS bağlaması projesi oluşturma](#Create_a_Xamarin.iOS_Binding_Project)**  -biz bir statik kitaplık aldıktan sonra bir Xamarin.iOS bağlaması projesi oluşturmak için kullanacağız. Objective-C API nasıl kullanılabileceğini açıklayan C# kodu biçiminde meta veriler ve oluşturduğumuz statik kitaplık bağlama projesi oluşur. Bu meta veriler, genellikle API tanımlarını da adlandırılır. Kullanacağız **[hedefi Sharpie](#Using_Objective_Sharpie)** API tanımlarını oluşturma bize yardımcı olacak.
- **[API tanımlarını Normalleştir](#Normalize_the_API_Definitions)**  - hedefi Sharpie bize yardımcı olmak için harika bir iş yapar, ancak her şey yapamaz. API tanımlarını kullanılabilmesi için önce yapmanız gereken bazı değişiklikler açıklayacağız.
- **[Bağlama kitaplığı kullanan](#Using_the_Binding)**  -son olarak, yeni oluşturulan bağlama Projemizin kullanmayı göstermek için bir Xamarin.iOS uygulaması oluşturacağız.

Hangi adımla daha uğraşmanız gerekir anlamak, gözden geçirme geri kalanı için geçelim.

<a name="Creating_A_Static_Library"/>

## <a name="creating-a-static-library"></a>Statik kitaplık oluşturma

Github'da InfColorPicker için kod İnceleme Biz ise:

[![](walkthrough-images/image02.png "Github'da InfColorPicker için kod İnceleme")](walkthrough-images/image02.png#lightbox)

Aşağıdaki üç dizin projedeki görebiliriz:

- **InfColorPicker** -bu dizin proje Objective-C kodunu içerir.
- **PickerSamplePad** -örnek bir iPad projesi bu dizini içerir.
- **PickerSamplePhone** -örnek bir iPhone projesi bu dizini içerir.

Şimdi InfColorPicker projesinden indirmeniz [GitHub](https://github.com/InfinitApps/InfColorPicker/archive/master.zip) ve bizim seçme dizinde sıkıştırmasını açın. Xcode hedefi açmayı `PickerSamplePhone` proje, aşağıdaki proje yapısını Xcode Gezgin görürüz:

[![](walkthrough-images/image03.png "Xcode Gezgini içindeki Proje yapısı")](walkthrough-images/image03.png#lightbox)

Bu proje, her bir örnek projeye doğrudan InfColorPicker kaynak kodunda (kırmızı kutu) ekleyerek kod yeniden kullanımını elde eder. Örnek Proje mavi bir kutu içinde kodudur. Bu belirli proje bize statik bir kitaplıkla sağlanmadığı için gerekli bize statik kitaplık derlemek için bir Xcode projesi oluşturun.

İlk adım, bize InfoColorPicker kaynak kodu statik kitaplığa eklemek içindir. Şimdi bunun için şunları yapın:

1. Xcode başlatın.
2. Gelen **dosya** menüsünü seçin **yeni** > **proje...** :

    [![](walkthrough-images/image04.png "Yeni bir projeye Başlarken")](walkthrough-images/image04.png#lightbox)
3. Seçin **Framework & Kitaplığı**, **Cocoa Touch statik kitaplık** şablonu ve tıklatın **sonraki** düğmesi:

    [![](walkthrough-images/image05.png "Cocoa Touch statik kitaplığı şablonu seçin")](walkthrough-images/image05.png#lightbox)

4. Girin `InfColorPicker` için **proje adı** tıklatıp **sonraki** düğmesi:

    [![](walkthrough-images/image06.png "InfColorPicker proje adını girin")](walkthrough-images/image06.png#lightbox)
5. Tıklayın ve projeyi kaydetmek için bir konum seçin **Tamam** düğmesi.
6. Şimdi biz kaynak InfColorPicker projeden bizim statik kitaplığı projesine eklemeniz gerekir. Çünkü **InfColorPicker.h** dosyası (varsayılan) statik kitaplığımızda zaten, Xcode izin bize üzerine. Gelen **Bulucu**, biz Github'dan sıkıştırması açılan özgün projede InfColorPicker kaynak koduna gidin, tüm InfColorPicker dosyaları kopyalayın ve bizim yeni bir statik kitaplık projesi yapıştırın:

    [![](walkthrough-images/image12.png "Tüm InfColorPicker dosyaları Kopyala")](walkthrough-images/image12.png#lightbox)

7. Sağ tıklayın, döndürmek için Xcode **InfColorPicker** klasörü ve select **"InfColorPicker..." için dosyaları Ekle**:

    [![](walkthrough-images/image08.png "Dosya ekleme")](walkthrough-images/image08.png#lightbox)

8. Az önce kopyaladığınız InfColorPicker kaynak kodu dosyaları için dosyaları Ekle iletişim kutusundan gidin, tümünü seçin ve tıklayın **Ekle** düğmesi:

    [![](walkthrough-images/image09.png "Tümünü seçin ve Ekle düğmesine tıklayın")](walkthrough-images/image09.png#lightbox)

9. Kaynak kodu, bizim projesine kopyalanır:

    [![](walkthrough-images/image10.png "Kaynak kod projesine kopyalanır.")](walkthrough-images/image10.png#lightbox)

10. Xcode Proje Gezgini ' seçin **InfColorPicker.m** dosya ve son iki satırları açıklama (Bu kitaplık yazıldı, bu dosya kullanılmaz şekli nedeniyle):

    [![](walkthrough-images/image14.png "InfColorPicker.m dosyasını düzenleme")](walkthrough-images/image14.png#lightbox)

11. Artık gerekli kitaplık için tüm çerçeveleri olup olmadığını denetlemek ihtiyacımız var. Bu bilgiler Benioku dosyasını veya sağlanan örnek projelerinden birini açarak bulabilirsiniz. Bu örnekte `Foundation.framework`, `UIKit.framework`, ve `CoreGraphics.framework` şimdi ekleyin.

12. Seçin **InfColorPicker hedef > derleme aşamaları** genişletin **bağlantı Binary With Libraries** bölümü:

    [![](walkthrough-images/image16b.png "Bağlantı Binary With Libraries bölümü genişletin")](walkthrough-images/image16b.png#lightbox)

13. Kullanım **+** düğmesi, yukarıda listelenen gerekli çerçeveler çerçeveleri eklemenize olanak sağlayan bir iletişim kutusunu açmak için:

    [![](walkthrough-images/image16c.png "Gerekli çerçeveler çerçeveleri, yukarıda listelenen Ekle")](walkthrough-images/image16c.png#lightbox)

14. **Bağlantı Binary With Libraries** bölümü artık aşağıdaki resimdeki gibi görünmelidir:

    [![](walkthrough-images/image16d.png "Bağlantı Binary With Libraries bölümü")](walkthrough-images/image16d.png#lightbox)

Bu noktada Kapat çalışıyoruz, ancak henüz tamamlandı. Statik kitaplık oluşturuldu, ancak bir Fat ikili tüm iOS cihaz hem de iOS simülatörü için gerekli olan mimari içeren oluşturmak için oluşturmamız gereken.

### <a name="creating-a-fat-binary"></a>Fat ikili oluşturma

Tüm iOS cihazları, zaman içinde geliştirdik ARM mimarisi tarafından desteklenen işlemcilere sahip. Her yeni bir mimari, yine de geriye dönük uyumluluğu koruyarak yeni yönergeleri ve diğer geliştirmeler eklendi. İOS cihazlarında armv6, armv7, armv7s, arm64 yönerge kümesi – olsa da sahibiz [artık armv6 kullandığımız](~/ios/deploy-test/compiling-for-different-devices.md). İOS simülatörü, ARM tarafından desteklenen değil ve bir x86 ve desteklenen x86_64 benzetici istead olur. Bizim için anlamına gelir ki olduğunu size her yönerge kitaplıkları sağlamalısınız ayarlar.

Fat Kitaplığı `.a` desteklenen tüm mimariler içeren dosya.

Bir fat ikili oluşturma üç adımlı bir işlemdir:

- Bir 7 ARM ve ARM64 sürümünü statik kitaplığı derleyin.
- Bir statik kitaplık x86 ve x84_64 sürümünü derleyin.
- Kullanım `lipo` iki statik kitaplıklar tek birleştirmek için komut satırı aracı.

Bu üç adımı çok basittir ve gelecekte Objective-C kitaplığını güncelleştirmeler aldığında veya hata düzeltmeleri kılarız tekrarlamanız gerekebilir olsa da. Adımları otomatikleştirmek karar verirseniz, gelecek Bakım ve Destek iOS bağlama projenin basitleştirecektir.

-Bir kabuk betiği gibi görevleri otomatik hale getirmek birçok araç vardır [rake](http://rake.rubyforge.org/), [xbuild](http://www.mono-project.com/docs/tools+libraries/tools/xbuild/), ve [olun](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man1/make.1.html). Xcode komut satırı araçlarını yüklediğimiz, biz de olun, bu nedenle diğer bir deyişle bu kılavuz için kullanılacak yapı sistemi yüklü. İşte bir **derleme görevleri dosyası** , bir iOS cihazı ve herhangi bir kitaplığı için simülatör üzerinde çalışacak bir çok mimari paylaşılan kitaplık oluşturmak için kullanabilirsiniz:

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

Girin **derleme görevleri dosyası** seçtiğiniz, düz metin düzenleyicisi komutları ve bölümleri güncelleştirme **YOUR proje adı** projenizin adına sahip. Emin olmak önemlidir biz yönergeleri sekmelerde korunmuş yönergeler yukarıda yapıştırın.

Dosya adıyla kaydedin **derleme görevleri dosyası** aynı konuma InfColorPicker Xcode statik yukarıda oluşturduğumuz kitaplığı:

[![](walkthrough-images/lib00.png "Dosyanın derleme görevleri dosyası adıyla kaydedin")](walkthrough-images/lib00.png#lightbox)

Mac bilgisayarınızda Terminal uygulamasını açın ve, derleme görevleri dosyasının konumuna gidin. Tür `make` terminale basın **Enter** ve **derleme görevleri dosyası** yürütülecek:

[![](walkthrough-images/lib01.png "Örnek derleme görevleri dosyası çıktısı")](walkthrough-images/lib01.png#lightbox)

Yapma çalıştırdığınızda, çok miktarda metin tarafından kaydırma görürsünüz. Her şeyin düzgün çalıştığı, sözcükleri göreceğiniz **derleme başarılı** ve `libInfColorPicker-armv7.a`, `libInfColorPicker-i386.a` ve `libInfColorPickerSDK.a` dosyalarını aynı konuma kopyalanır **derleme görevleri dosyası**:

[![](walkthrough-images/lib02.png "Derleme görevleri dosyası tarafından oluşturulan libInfColorPicker armv7.a ve libInfColorPicker i386.a libInfColorPickerSDK.a dosyaları")](walkthrough-images/lib02.png#lightbox)

Mimariler, Fat ikili dosyanız içinde aşağıdaki komutu kullanarak doğrulayabilirsiniz:

```bash
xcrun -sdk iphoneos lipo -info libInfColorPicker.a
```

Aşağıdaki görüntülenmelidir:

```bash
Architectures in the fat file: libInfColorPicker.a are: i386 armv7 x86_64 arm64
```

Bu noktada, biz Xcode ve Xcode komut satırı araçlarını kullanarak bir statik kitaplık oluşturarak bizim iOS bağlama ilk adımı tamamladınız `make` ve `lipo`. Şimdi, sonraki adıma geçin ve kullanmak **hedefi Sharpie** ABD API'si bağlamalarını oluşturulmasını otomatik hale getirmek için.

<a name="Create_a_Xamarin.iOS_Binding_Project"/>

## <a name="create-a-xamarinios-binding-project"></a>Bir Xamarin.iOS projesi bağlama oluşturma

Kullanabilmeniz için önce **hedefi Sharpie** bağlama işlemi otomatikleştirmek için API tanımlarını barındırmak için bir Xamarin.iOS bağlaması projesi oluşturmak ihtiyacımız (kullanacağız, **hedefi Sharpie** yardımcı olmak için derleme) ve bizim için C# bağlaması oluşturun.

Şimdi aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Mac için Visual Studio'yu başlatın
1. Gelen **dosya** menüsünde **yeni** > **çözüm...** :

    ![](walkthrough-images/bind01.png "Yeni bir çözüm başlatılıyor")

1. Yeni çözüm iletişim kutusundan **Kitaplığı** > **iOS projesi bağlama**:

    ![](walkthrough-images/bind02.png "İOS bağlama projesi seçin")

1. Tıklayın **sonraki** düğmesi.

1. "InfColorPickerBinding" olarak girin **proje adı** tıklatıp **Oluştur** çözümü oluşturmak için:

    ![](walkthrough-images/bind02a.png "InfColorPickerBinding proje adı girin")

Çözümü oluşturulur ve iki varsayılan dosyaları dahil edilir:

![](walkthrough-images/bind03.png "Çözüm Gezgini'nde Çözüm yapısı")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


1. Visual Studio'yu başlatın.

1. Gelen **dosya** menüsünde **yeni** > **proje...** :

    ![Yeni bir projeye Başlarken](walkthrough-images/bind01vs.png "yeni bir projeye Başlarken")

1. Yeni Proje iletişim kutusundan **Visual C# > iPhone & iPad > iOS bağlamaları kitaplığı (Xamarin)**:

    [![İOS bağlamaları kitaplığı seçin](walkthrough-images/bind02.w157-sml.png)](walkthrough-images/bind02.w157.png#lightbox)

1. "InfColorPickerBinding" olarak girin **adı** tıklatıp **Tamam** çözümü oluşturmak için.

Çözümü oluşturulur ve iki varsayılan dosyaları dahil edilir:

![](walkthrough-images/bind03vs.png "Çözüm Gezgini'nde Çözüm yapısı")

-----

- **ApiDefinition.cs** -bu dosya nasıl Objective-C API'nin C# içinde sarmalanır tanımlayan sözleşme içerir.
- **Structs.cs** - bu dosya yapılar tutacak veya numaralandırma değerlerinden oluşan arabirimlerde ve temsilcilerde tarafından gereklidir.

Biz bu iki dosyayı daha sonra bu kılavuzdaki ile çalışma. İlk olarak biz InfColorPicker kitaplığı bağlama projeye eklemeniz gerekir.

### <a name="including-the-static-library-in-the-binding-project"></a>Statik kitaplık bağlama projeye dahil

Temel bağlama Projemizin hazır sahibiz artık yukarıda oluşturduğumuz için Fat ikili Kitaplığı eklemek ihtiyacımız **InfColorPicker** kitaplığı.

Kitaplığı eklemek için aşağıdaki adımları izleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. Sağ **yerel başvurular** seçin ve çözüm bölmesi klasöründe **yerel başvurular ekleme**:

    ![](walkthrough-images/bind04a.png "Yerel başvurular ekleme")

1. Fat daha önce yaptığımız ikili dosyasını gidin (`libInfColorPickerSDK.a`) tuşuna basın **açık** düğmesi:

    ![](walkthrough-images/bind05.png "LibInfColorPickerSDK.a dosyasını seçin")
1. Dosya projeye eklenecek:

    ![](walkthrough-images/bind04.png "Bir dosya dahil olmak üzere")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Kopyalama `libInfColorPickerSDK.a` gelen, **Mac derleme konağı** bağlama projenize kopyalayıp yapıştırın.

1. Projeye sağ tıklayın ve seçin **Ekle > mevcut öğe...** :

    ![](walkthrough-images/bind04vs.png "Var olan bir dosya ekleme")

1. Gidin `libInfColorPickerSDK.a` basın **Ekle** düğmesi:

    ![](walkthrough-images/bind05vs.png "LibInfColorPickerSDK.a ekleme")

1. Dosya projeye dahil edilir.

-----

Zaman **.a** dosya projeye Xamarin.iOS otomatik olarak ayarlayacak eklenir **derleme eylemi** dosyanın **ObjcBindingNativeLibrary**, özel bir dosya oluşturun adlı `libInfColorPickerSDK.linkwith.cs`.


Bu dosyayı içeren `LinkWith` Xamarin.iOS nasıl tanıtıcı biz yalnızca statik kitaplığa eklenen belirten özniteliği. Bu dosyanın içeriğini aşağıdaki kod parçacığında gösterilir:

```csharp
using ObjCRuntime;

[assembly: LinkWith ("libInfColorPickerSDK.a", SmartLink = true, ForceLoad = true)]
```

`LinkWith` Öznitelik statik kitaplık projesi ve bazı önemli bağlayıcı bayrakları için tanımlar.


Yapmamız gereken bir sonraki şey, API tanımlarını InfColorPicker projesi oluşturmaktır. Bu izlenecek yolda amacı doğrultusunda, hedefi Sharpie dosyası oluşturmak için kullanacağız **ApiDefinition.cs**.

<a name="Using_Objective_Sharpie"/>

## <a name="using-objective-sharpie"></a>Amaç Sharpie kullanma

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Amaç Sharpie olan bir komut satırı (Xamarin tarafından sağlanan) aracı, C# 3 bir taraf Objective-C kitaplığını bağlama için gereken tanımları oluşturmak size yardımcı olabilir. Bu bölümde, hedefi Sharpie ilk oluşturmak için kullanacağız **ApiDefinition.cs** InfColorPicker projesi için.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Amaç Sharpie olan bir komut satırı (Xamarin tarafından sağlanan) aracı, C# 3 bir taraf Objective-C kitaplığını bağlama için gereken tanımları oluşturmak size yardımcı olabilir. Bu bölümde, hedefi Sharpie üzerinde kullanacağız bizim **Mac derleme konağı** ilk oluşturmak için **ApiDefinition.cs** InfColorPicker projesi için.


-----

Başlamak için şimdi bu ayrıntılı olarak hedefi Sharpie yükleyicisini indirin [Kılavuzu](~/cross-platform/macios/binding/objective-sharpie/get-started.md#installing). Yükleyiciyi çalıştırın ve ekrandaki istemleri hepsini bizim geliştirme bilgisayarında hedefi Sharpie yüklemek için yükleme sihirbazını izleyin.

Biz hedefi Sharpie başarıyla oluşturduktan sonra yüklü şimdi Terminal uygulamasını başlatın ve tüm bağlamasında yardımcı olmak için sağladığı araçları hakkında Yardım almak için aşağıdaki komutu girin:

```bash
sharpie -help
```

Biz yukarıdaki komutu çalıştırırsanız, aşağıdaki çıktı oluşturulur:

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

Bu gözden geçirme amacıyla biz aşağıdaki hedefi Sharpie araçları kullanarak:

- **xcode** - bu araçları bize bizim geçerli bir Xcode yüklemesi ve iOS ve Mac yüklü olan API'leri sürümleri hakkında bilgi sağlar. Size sunduğumuz bağlamaları oluşturduğunuzda size daha sonra bu bilgileri kullanacaklardır.
- **Bağlama** -ayrıştırmak için bu aracı kullanacağız **.h** ilk InfColorPicker projeye dosyalarında **ApiDefinition.cs** ve **StructsAndEnums.cs** dosyaları.

Belirli bir amaç Sharpie aracı hakkında Yardım almak için aracının adını girin ve `-help` seçeneği. Örneğin, `sharpie xcode -help` aşağıdaki çıktı döndürür:

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

Bağlama işlemi başlatmadan önce terminale aşağıdaki komutu girerek geçerli yüklü Larımız hakkında bilgi almak ihtiyacımız `sharpie xcode -sdks`:

```bash
amyb:Desktop amyb$ sharpie xcode -sdks
sdk: appletvos9.2    arch: arm64
sdk: iphoneos9.3     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: watchos2.2      arch: armv7
```

Yukarıdakilerden, biz olduğunu görebiliriz `iphoneos9.3` SDK bizim makinede yüklü. Bu bilgileri bir yerde InfColorPicker proje ayrıştırılacak hazırız `.h` ilk dosyalarına **ApiDefinition.cs** ve `StructsAndEnums.cs` InfColorPicker projesi için.

Aşağıdaki komutu girin Terminal uygulamasını:

```bash
sharpie bind --output=InfColorPicker --namespace=InfColorPicker --sdk=[iphone-os] [full-path-to-project]/InfColorPicker/InfColorPicker/*.h
```

Burada `[full-path-to-project]` dizinine tam yolu burada **InfColorPicker** Xcode proje dosyası, bizim bilgisayarda bulunur ve iphone-os olduğu yüklediğimiz, SDK'sı iOS tarafından belirtildiği gibi `sharpie xcode -sdks` komutu. Bu örnekte biz geçirilen Not  **\*.h** içeren bir parametre olarak *tüm* üstbilgi dosyalarını bu dizinde - normalde, bunu değil, ancak bunun yerine dikkatle okuyun üst düzey bulmak için üst bilgi dosyaları **.h** başvuran tüm diğer ilgili dosyaları ve hedefi Sharpie için geçirmeniz yeterlidir, dosya.

Aşağıdaki [çıkış](walkthrough-images/os05.png) terminalde oluşturulur:

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

Ve **InfColorPicker.enums.cs** ve **InfColorPicker.cs** bizim dizinde dosya oluşturulur:

[![](walkthrough-images/os06.png "InfColorPicker.enums.cs ve InfColorPicker.cs dosyaları")](walkthrough-images/os06.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Bu dosyaların her ikisini de, yukarıda oluşturduğumuz bağlama projesi açın. İçeriğini kopyalayın **InfColorPicker.cs** yapıştırın ve dosya **ApiDefinition.cs** dosya, var olan değiştirerek `namespace ...` kod bloğu içeriğiyle  **InfColorPicker.cs** dosyası (bırakarak `using` deyimleri bozulmamış):

![](walkthrough-images/os07.png "InfColorPickerControllerDelegate dosyası")


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Bu dosyaların her ikisini de, yukarıda oluşturduğumuz bağlama projesi açın. İçeriğini kopyalayın **InfColorPicker.cs** dosyası (gelen **Mac derleme konağı**) yapıştırın **ApiDefinition.cs** dosya, var olan değiştirerek `namespace ...` kod bloğu içeriğiyle **InfColorPicker.cs** dosyası (bırakarak `using` deyimleri olduğu gibi).


-----

<a name="Normalize_the_API_Definitions"/>

## <a name="normalize-the-api-definitions"></a>API tanımlarını normalleştirin

Hedefi Sharpie bazen olan bir sorunu çevirme `Delegates`, biz tanımını değiştirmeniz gerekecektir `InfColorPickerControllerDelegate` Değiştir ve arabirimi `[Protocol, Model]` aşağıdaki satırı:

```csharp
[BaseType(typeof(NSObject))]
[Model]
```
Böylece tanımı aşağıdaki gibi görünür:

[![](walkthrough-images/os11.png "Tanımı")](walkthrough-images/os11.png#lightbox)

Ardından, içeriği aynı işlemi yaptığımız `InfColorPicker.enums.cs` kopyalama ve yapıştırma bunları dosya `StructsAndEnums.cs` bırakarak dosya `using` deyimleri sorunsuz:

[![](walkthrough-images/os09.png "İçeriği StructsAndEnums.cs dosyası ")](walkthrough-images/os09.png#lightbox)

Hedefi Sharpie bağlamayla ek açıklamalı bulabilirsiniz `[Verify]` öznitelikleri. Bu öznitelikler, hedefi Sharpie bağlama (Bu bir yorum ilişkili bildiriminin üstüne sağlanacaktır) özgün C/Objective-C bildirimi ile karşılaştırarak en doğru şey olmadı doğrulamalıdır gösterir. Bağlamaları doğruladıktan sonra doğrulama özniteliği kaldırmanız gerekir. Daha fazla bilgi için [doğrulama](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) Kılavuzu.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)


Bu noktada, bağlama Projemizin eksiksiz ve oluşturmak için hazır olması gerekir. Diyelim ki bizim bağlama projeyi oluşturun ve size herhangi bir hata ile sona emin olun:

[Bağlama projeyi oluşturun ve hiçbir hata olmadığından emin olun](walkthrough-images/os12.png)


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)


Bu noktada, bağlama Projemizin eksiksiz ve oluşturmak için hazır olması gerekir. Diyelim ki bizim bağlama projeyi derleyin ve size herhangi bir hata ile sona emin olun.


-----

<a name="Using_the_Binding"/>

## <a name="using-the-binding"></a>Bağlama işlemini kullanma

Bağlama kitaplığı yukarıda oluşturduğunuz iOS kullanarak bir örnek iPhone uygulaması oluşturmak için aşağıdaki adımları izleyin:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. **Xamarin.iOS projesi oluşturma** -adlı yeni bir Xamarin.iOS projesi Ekle **InfColorPickerSample** aşağıdaki ekran görüntülerinde gösterildiği gibi çözüm için:

    ![](walkthrough-images/use01.png "Tek görünüm uygulaması ekleme")

    ![](walkthrough-images/use01a.png "Tanımlayıcı ayarlama")

1. **Bağlama projeye başvuru ekleyin** -güncelleştirme **InfColorPickerSample** başvuru sahip olacak şekilde proje **InfColorPickerBinding** proje:

    ![](walkthrough-images/use02.png "Bağlama projeye başvuru ekleme")

1. **İPhone kullanıcı arabirimi oluşturma** -çift tıklayın **MainStoryboard.storyboard** dosyası **InfColorPickerSample** iOS Designer'daki düzenlemek için proje. Ekleme bir **düğmesi** görüntülemek ve onu çağırmak `ChangeColorButton`aşağıda gösterildiği gibi:

    ![](walkthrough-images/use03.png "Görünüm için bir düğme ekleme")

1. **InfColorPickerView.xib ekleme** -InfColorPicker Objective-C kitaplığını içeren bir **.xib** dosya. Xamarin.iOS bu içermeyecek **.xib** bağlama projesinde örnek uygulamamızdaki çalışma zamanı hatalarına neden olacak. Geçici çözüm bu eklemektir **.xib** Xamarin.iOS Projemizin dosyasına. Xamarin.iOS projesi, sütuna sağ tıklayıp seçin **Ekle > Add Files**ve ekleme **.xib** aşağıdaki ekran görüntüsünde gösterildiği gibi dosya:

    ![](walkthrough-images/use04.png "InfColorPickerView.xib ekleyin")

1. İstendiğinde, kopyalama **.xib** dosyayı projeye.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **Xamarin.iOS projesi oluşturma** -adlı yeni bir Xamarin.iOS projesi eklemek **InfColorPickerSample** kullanarak **tek görünüm uygulaması** şablonu:

    [![iOS uygulaması (Xamarin) projesi](walkthrough-images/use01.w157-sml.png)](walkthrough-images/use01.w157.png#lightbox)

    [![Şablonu seçin](walkthrough-images/use01-2.w157-sml.png)](walkthrough-images/use01-2.w157.png#lightbox)

1. **Bağlama projeye başvuru ekleyin** -güncelleştirme **InfColorPickerSample** başvuru sahip olacak şekilde proje **InfColorPickerBinding** proje:

    ![](walkthrough-images/use02vs.png "Bağlama projeye başvuru ekleyin")

1. **İPhone kullanıcı arabirimi oluşturma** -çift tıklayın **MainStoryboard.storyboard** dosyası **InfColorPickerSample** iOS Designer'daki düzenlemek için proje. Ekleme bir **düğmesi** görüntülemek ve onu çağırmak `ChangeColorButton`aşağıda gösterildiği gibi:

    ![](walkthrough-images/use03vs.png "İPhone kullanıcı arabirimi oluşturma")

1. **InfColorPickerView.xib ekleme** -InfColorPicker Objective-C kitaplığını içeren bir **.xib** dosya. Xamarin.iOS bu içermeyecek **.xib** bağlama projesinde örnek uygulamamızdaki çalışma zamanı hatalarına neden olacak. Geçici çözüm bu eklemektir **.xib** Xamarin.iOS Projemizin dosyasına bizim **Mac derleme konağı**. Xamarin.iOS projesi, sütuna sağ tıklayıp seçin **Ekle** > **mevcut öğe...** ve ekleme **.xib** dosya.

-----

Ardından, Objective-C ve nasıl bunları bağlama ve C# kodu işleme protokolleri hızlı bir göz atalım.

### <a name="protocols-and-xamarinios"></a>Protokoller ve Xamarin.iOS

Objective-C, bir protokol, yöntemler (veya iletileri) tanımlar. Bazı durumlarda kullanılabilir. Kavramsal olarak, bunlar C# ' ta arabirimler çok benzer. Objective-C protokolü ve C# arabirimi arasındaki temel farklardan biri protokolleri isteğe bağlı yöntemler - bir sınıf uygulamak için sahip olmayan yöntemleri sahip olabilmeleridir. Objective-C kullanan @optional anahtar sözcüğü, hangi yöntemlerin isteğe bağlı olarak belirtmek için kullanılır. Protokolleri hakkında daha fazla bilgi için bkz. [olaylar, protokoller ve Temsilciler](~/ios/app-fundamentals/delegates-protocols-and-events.md).

**InfColorPickerController** aşağıdaki kod parçacığında gösterilen bir tür Protokolü vardır:

```csharp
@protocol InfColorPickerControllerDelegate

@optional

- (void) colorPickerControllerDidFinish: (InfColorPickerController*) controller;
// This is only called when the color picker is presented modally.

- (void) colorPickerControllerDidChangeColor: (InfColorPickerController*) controller;

@end
```

Bu protokolü tarafından kullanılan **InfColorPickerController** kullanıcı yeni bir renk ve, çekilen istemciler bildirmek için **InfColorPickerController** tamamlandı. Amaç Sharpie, aşağıdaki kod parçacığında gösterildiği gibi bu protokolü eşlenmiş:

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

Bağlama kitaplığı derlendiğinde Xamarin.iOS adlı bir soyut temel sınıf oluşturacak `InfColorPickerControllerDelegate`, sanal yöntemleri ile bu arabirimi uygular.

Biz bu arabirim bir Xamarin.iOS uygulamasına uygulayabilirsiniz iki yolu vardır:

- **Güçlü temsilci** -güçlü bir temsilci kullanılmasına alt sınıflara ayıran bir C# sınıfı oluşturma `InfColorPickerControllerDelegate` ve uygun yöntemleri geçersiz kılar. **InfColorPickerController** bu sınıfın bir örneği, istemcilerle iletişim kurmak için kullanır.
- **Zayıf temsilci** -zayıf bir temsilci bazı sınıfı genel bir yöntem kapsamında biraz daha farklı bir tekniktir (gibi `InfColorPickerSampleViewController`) ve ardından bu yönteme gösterme `InfColorPickerDelegate` protokolü aracılığıyla bir `Export` özniteliği.

Güçlü Temsilciler, IntelliSense, tür güvenliği ve daha iyi kapsülleme sağlayın. Bu nedenlerle, zayıf bir temsilci yerine gerçekleştirebileceğiniz güçlü temsilciler kullanmanız gerekir.

Bu kılavuzda hem tekniklerini ele alınacaktır: ilk güçlü bir temsilci uygulamak ve ardından zayıf bir temsilci uygulamak nasıl açıklayan.

### <a name="implementing-a-strong-delegate"></a>Güçlü bir temsilci uygulama

Xamarin.iOS uygulama yanıt vermek için güçlü bir temsilci kullanarak son `colorPickerControllerDidFinish:` ileti:

**Alt InfColorPickerControllerDelegate** -adlı projeye yeni bir sınıf ekleyin `ColorSelectedDelegate`. Sınıfı, aşağıdaki kod sahip olacak şekilde düzenleyin:

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

Xamarin.iOS bağlama Objective-C temsilci olarak adlandırılan bir soyut temel sınıf oluşturarak `InfColorPickerControllerDelegate`. Öğesinin alt sınıfı bu tür ve geçersiz kılma `ColorPickerControllerDidFinish` değerini erişmeye yöntemi `ResultColor` özelliği `InfColorPickerController`.

**ColorSelectedDelegate örneğini oluşturmak** -bizim olay işleyicisi örneği gerekir `ColorSelectedDelegate` önceki adımda oluşturduğumuz türü. Sınıf Düzenle `InfColorPickerSampleViewController` ve sınıfa aşağıdaki örnek değişkeni ekleyin:

```csharp
ColorSelectedDelegate selector;
```

**ColorSelectedDelegate değişkeni başlatmak** - emin olmak için `selector` geçerli bir örneği, güncelleştirme yöntemi `ViewDidLoad` içinde `ViewController` aşağıdaki kod parçacığı eşleştirmek için:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithStrongDelegate;
    selector = new ColorSelectedDelegate (this);
}
```
**HandleTouchUpInsideWithStrongDelegate yöntemi uygulamak** -sonraki kullanıcı ne zaman dokunduğu için olay işleyicisini uygulayın **ColorChangeButton**. Düzen `ViewController`, aşağıdaki yöntemi ekleyin:

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

Biz öncelikle örneği elde `InfColorPickerController` bir statik yöntem ve örnek özelliği aracılığıyla güçlü bizim temsilci farkında olun aracılığıyla `InfColorPickerController.Delegate`. Bu özellik otomatik olarak bizim için hedefi Sharpie tarafından oluşturuldu. Son olarak diyoruz `PresentModallyOverViewController` görünümüne geçmek için `InfColorPickerSampleViewController.xib` böylece kullanıcı, bir renk seçebilirsiniz.

**Uygulamayı çalıştırmak** - uyguladığımız tüm kodumuz bu noktada. Uygulamayı çalıştırdığınızda, arka plan rengini değiştirmek erişebileceğinizi `InfColorColorPickerSampleView` aşağıdaki ekran görüntülerinde gösterildiği gibi:

[![](walkthrough-images/run01.png "Uygulamayı çalıştırma")](walkthrough-images/run01.png#lightbox)

Tebrikler! Bu noktada başarıyla oluşturduğunuz ve bir Objective-C kitaplığını kullanılmak üzere bir Xamarin.iOS uygulaması bağlı. Ardından, şimdi zayıf temsilcileri kullanma hakkında bilgi edinin.

### <a name="implementing-a-weak-delegate"></a>Zayıf bir temsilci uygulama

Bir sınıf için belirli bir temsilci Objective-C protokolüne bağlı sınıflara yerine Xamarin.iOS ayrıca türetilen herhangi bir sınıf, protokol yöntemleri uygulamak sağlar `NSObject`, yöntemlerinizi ile dekorasyon `ExportAttribute`ve ardından uygun Seçici sağlama. Bu yaklaşımı benimsemeniz, sınıfı bir örneğini atadığınız `WeakDelegate` özelliği için yerine `Delegate` özelliği. Zayıf bir temsilci temsilcinin sınıfınıza farklı devralma hiyerarşisinde aşağı esnekliğine sunar. Bakalım nasıl uygulanacağını ve Xamarin.iOS uygulamamız zayıf bir temsilci kullanın.

**Olay işleyicisi oluşturmak için TouchUpInside** -için yeni bir olay işleyici oluşturalım `TouchUpInside` olay düğmenin arka plan rengini değiştirebilirsiniz. Bu işleyici, aynı rol olarak dolduracağı `HandleTouchUpInsideWithStrongDelegate` size önceki bölümde oluşturduğunuz ancak zayıf bir temsilci güçlü bir temsilci yerine kullanacağınız işleyici. Sınıf Düzenle `ViewController`, aşağıdaki yöntemi ekleyin:

```csharp
private void HandleTouchUpInsideWithWeakDelegate (object sender, EventArgs e)
{
    InfColorPickerController picker = InfColorPickerController.ColorPickerViewController();
    picker.WeakDelegate = this;
    picker.SourceColor = this.View.BackgroundColor;
    picker.PresentModallyOverViewController (this);
}
```
**ViewDidLoad güncelleştirme** -ki değiştirmeli `ViewDidLoad` böylece oluşturduğumuz olay işleyicisi kullanır. Düzen `ViewController` değiştirip `ViewDidLoad` aşağıdaki kod parçacığı benzeyecek şekilde:


```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    ChangeColorButton.TouchUpInside += HandleTouchUpInsideWithWeakDelegate;
}

```

**ColorPickerControllerDidFinish işlemek: ileti** - `ViewController` olan bittiğinde, iOS mesajı göndereceğiz `colorPickerControllerDidFinish:` için `WeakDelegate`. Bu iletiyi işleyebilen bir C# yöntemi için oluşturmamız gerekir. Bunu yapmak için size bir C# yöntemi oluşturma ve onunla adorn `ExportAttribute`. Düzen `ViewController`, sınıfına aşağıdaki yöntemi ekleyin:

```csharp
[Export("colorPickerControllerDidFinish:")]
public void ColorPickerControllerDidFinish (InfColorPickerController controller)
{
    View.BackgroundColor = controller.ResultColor;
    DismissViewController (false, null);
}

```

Uygulamayı çalıştırın. Artık tam olarak daha önceki işlevlerini sürdürmektedir, ancak güçlü temsilci yerine zayıf bir temsilci kullanıyor çalışacaktır. Bu noktada Bu izlenecek yolda başarıyla tamamladınız. Artık, bir Xamarin.iOS bağlaması projesi oluşturup nasıl bir anlayışa sahipsiniz.

## <a name="summary"></a>Özet

Bu makalede, oluşturma ve bir Xamarin.iOS bağlaması projesi kullanma işleminde size öğrendiniz. İlk statik bir kitaplıkta mevcut bir Objective-C kitaplığını derlemeye nasıl ele almıştık. Ardından bir Xamarin.iOS bağlaması projenin nasıl oluşturulacağını ve hedefi Sharpie Objective-C Kitaplığı için API tanımları oluşturmak için nasıl kullanılacağı ele. Ortak tüketim için uygun hale getirmek için oluşturulan API tanımlarını ince ve güncelleştirmek nasıl ele almıştık. Xamarin.iOS bağlaması proje bittikten sonra tüketen bir Xamarin.iOS uygulaması, temsilciler güçlü ve zayıf temsilcileri kullanma odaklanmasına Bu bağlama için geçtiğimizi.

## <a name="related-links"></a>İlgili bağlantılar

- [Bağlama örneği (örnek)](https://developer.xamarin.com/samples/monotouch/InfColorPicker/)
- [Objective-C Kitaplıklarını Bağlama](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Bağlama Ayrıntıları](~/cross-platform/macios/binding/overview.md)
- [Bağlama türleri Başvuru Kılavuzu](~/cross-platform/macios/binding/binding-types-reference.md)
- [Objective-C geliştiricileri için Xamarin](~/ios/get-started/objective-c-developers/index.md)
- [Çerçeve Tasarım Yönergeleri](http://msdn.microsoft.com/library/ms229042.aspx)
- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
