---
title: Takım Şehir Xamarin ile kullanma
description: Bu kılavuz, mobil uygulamaları derlemek ve bunları Xamarin Test Cloud göndermek için TeamCity kullanarak sahip adımlarını ele alınacaktır.
ms.prod: xamarin
ms.assetid: AC2626CB-28A7-4808-B2A9-789D67899546
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/23/2017
ms.openlocfilehash: 34702fafdd0d767362b0ca32ab56e880ed7cb366
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="using-team-city-with-xamarin"></a>Takım Şehir Xamarin ile kullanma

_Bu kılavuz, mobil uygulamaları derlemek ve bunları Xamarin Test Cloud göndermek için TeamCity kullanarak sahip adımlarını ele alınacaktır._

' Da anlatıldığı gibi [sürekli tümleştirme giriş](~/tools/ci/intro-to-ci.md) Kılavuzu, sürekli tümleştirme (CI) olduğunda yararlı bir uygulama kalitesini mobil uygulamaları geliştirme. Sürekli Tümleştirme sunucu yazılımı için çok sayıda uygulanabilir seçenek vardır; Bu kılavuz üzerine odaklanacaktır [TeamCity](http://www.jetbrains.com/teamcity/) JetBrains gelen.

Birkaç farklı permütasyon TeamCity yüklemesinin vardır. Bunlardan bazıları listesi aşağıdadır:

- **Windows hizmeti** – Bu senaryoda, Windows bir Windows hizmeti olarak önyüklendiğinde yukarı TeamCity başlatır. Mac yapı tüm iOS uygulamaları derleme için olan bir konak eşleştirilmelidir.

- **OS X arka plan başlatma** – kavramsal olarak, bu önceki adımda açıklanan bir Windows hizmeti olarak çalışan çok benzer. Varsayılan olarak derlemeleri kök hesap altında çalıştırılacaktır.

- **OS X kullanıcı hesabında** – TeamCity kullanıcı her oturum açışında başlayan bir kullanıcı hesabı altında çalışacak şekilde mümkündür.

Önceki senaryoları, TeamCity bir kullanıcı hesabı altında OS x'te basit ve kolay kurulum için çalışıyor.

TeamCity ayarlama ile ilgili birkaç adım vardır:

- **TeamCity yükleme** – TeamCity yüklemesi bu kılavuzda ele alınmamaktadır. Bu kılavuz TeamCity yüklü ve bir kullanıcı hesabı altında çalışır durumda olduğunu varsayar. Yönergeler [TeamCity yükleme](http://confluence.jetbrains.com/display/TCD8/Installation) bulunabilir [TeamCity 8 belge](http://confluence.jetbrains.com/display/TCD8/TeamCity+Documentation) JetBrains tarafından.

- **Yapı sunucu hazırlama** – Araçlar, gerekli yazılımları yüklemek bu adımı içerir ve sertifikalar için gereken mobil uygulamaları oluşturmak ve bunları dağıtım için hazırlayın.

- **Bir yapı komut dosyası oluşturma** – Bu adım kesinlikle gerekli değildir, ancak katılımsız uygulamaları oluşturmak için yararlı bir yardımcı bir yapı komut dosyasıdır. Yapı betik kullanarak kaynaklanabilecek ve sürekli tümleştirme değil yapılmaktadır olsa bile, dağıtım, ikili dosyaları oluşturmak için tutarlı ve tekrarlanabilir bir yol sağlayan derleme sorunlarını gidermeye yardımcı olur.

- **A TeamCity proje oluşturma** – önceki üç adımı tamamlandıktan sonra tüm meta verilerin içerecek TeamCity proje oluşturmanız gerekir kaynak kodunu almak, projeler derlemek ve Xamarin Test Cloud sınamaları göndermek gerekli.

## <a name="requirements"></a>Gereksinimler

İle deneyimi [Xamarin Test Cloud](https://developer.xamarin.com/guides/testcloud) gereklidir.

TeamCity 8.1 aşina gereklidir. Bu belgenin kapsamı dışındadır TeamCity yüklemesidir. TeamCity OS X Mavericks üzerinde yüklü olduğundan ve normal bir kullanıcı hesabı ve kök hesabı altında çalışan varsayılır.

Yapı sunucunun sürekli tümleştirme ayrılmış OS X çalıştıran tek başına bir bilgisayar olmalıdır. İdeal olarak, yapı sunucu bir veritabanı sunucusu, web sunucusu ya da geliştirme iş istasyonu gibi diğer tüm rolleri sorumlu olmaz.

> [!IMPORTANT]
> Bu kılavuz, Xamarin "gözetimsiz" yüklemesini kapsamaz.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="preparing-the-build-server"></a>Yapı sunucuyu hazırlama

Bir yapı yapılandırma önemli bir adım tüm gerekli araçlar, yazılım ve mobil uygulamaları oluşturmak için sertifika yüklemektir. Yapı sunucunun mobil çözüm derlemek ve tüm testleri çalıştırmak olması önemlidir. Yapılandırma sorunları en aza indirmek için Araçlar ve yazılım TeamCity barındırma aynı kullanıcı hesabı yüklenmelidir. Gerekli olan bir listesi verilmiştir:

1. **Mac için Visual Studio** – bu Xamarin.iOS ve Xamarin.Android içerir.
2. **Xamarin bileşen Deposu'nda oturum açma** – Bu isteğe bağlı bir adımdır ve yalnızca uygulamanızı Xamarin bileşeni Mağazası'ndan bileşenleri kullanıyorsa gereklidir. TeamCity derleme uygulama derlemeye çalıştığında proaktif olarak bileşen deposuna bu noktada günlüğü sorunları önler.
3. **Xcode** – Xcode derleme ve oturum iOS uygulamaları için gereklidir.
4. **Xcode komut satırı araçları** – bu 1. adımında yükleme bölümünün açıklanan [güncelleştirme Ruby rbenv ile](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) Kılavuzu.
5. **Identity & sağlama profilleri imzalama** – sertifikaları alma ve sağlama profiliniz XCode aracılığıyla. Lütfen Apple'nın kılavuzu üzerinde [imzalama kimlikleri dışarı aktarma ve sağlama profilleri](https://developer.apple.com/library/ios/recipes/xcode_help-accounts_preferences/articles/export_signing_assets.html) daha fazla ayrıntı için.
6. **Android Keystores** – gerekli Android keystores TeamCity kullanıcı, yani erişimi olduğunu bir dizine kopyalayın `~/Documents/keystores/MyAndroidApp1`.
7. **Calabash** – Calabash kullanılarak yazılmış testleri uygulamanız varsa, bu isteğe bağlı bir adımdır. Lütfen bakın [yükleme Calabash OS X Mavericks üzerinde](https://developer.xamarin.com/guides/testcloud/calabash/osx-installation/) Kılavuzu ve [güncelleştirme Ruby rbenv ile](https://developer.xamarin.com/guides/testcloud/calabash/updating-ruby-using-rbenv/) daha fazla bilgi için Kılavuzu.

Aşağıdaki diyagram bu bileşenlerin tümünü gösterir:

![](teamcity-images/image1.png "Bu diyagramda bu bileşenlerin tümünü gösterilir")

Tüm yazılımların yüklü olduğu sonra kullanıcı hesabı ile oturum açın ve tüm yazılım doğru şekilde yüklendiğinden ve çalıştığından emin olun. Bu çözümü derleme ve Test bulut uygulamaya gönderme içermelidir. Bu büyük ölçüde yapı komut dosyasını çalıştırarak bir sonraki bölümde açıklandığı gibi basit hale getirilebilir.

## <a name="create-a-build-script"></a>Yapı komut dosyası oluşturma

Derleme ve Test bulut mobil uygulamaları kendi kendine gönderme tüm yönlerini işlemek TeamCity tamamen mümkün olsa da, bir derleme betiği oluşturmak için kesinlikle önerilir. Derleme betiğinin aşağıdaki avantajları sağlar:

1. **Belge** – yazılım nasıl yapılandırıldığını üzerinde belgelerin form derleme betiğindeki görür. Bu "uygulama dağıtımı ile ilişkili olan ve geliştiricilerin işlevselliğini odaklanmasına olanak tanır Sihirli" bazıları kaldırır.
1. **Yinelenebilirlik** – uygulama derlenir ve dağıtılır, her seferinde onu kim veya ne iş yaptığını bağımsız olarak tam olarak aynı şekilde gerçekleştiğinden emin bir yapı komut dosyası sağlar. Bu yinelenebilir tutarlılık tüm sorunlar veya yanlış yürütülen derleme nedeniyle, işi hataları veya İnsan hatası kaldırır.
1. **Sürüm oluşturma** – bir yapı komut dosyası kaynak denetim sistemi dahil edilebilir. Bu derleme betiğindeki değişiklikleri izlenen, izlenir ve düzeltilen hatalar veya yanlışlıklar bulunursa, anlamına gelir.
1. **Ortamı hazırlama** – bir yapı komut dosyası 3 taraf bağımlılıkları gerekli yüklemeye mantık içerebilir. Bu, uygulamaların uygun bileşenleri ile oluşturulan güvence altına alır.

Derleme betiğinin bir Powershell dosyası (Windows) veya bir bash komut dosyasını (OS X) kadar basit olabilir. Derleme betiğinin oluştururken, komut dosyası dili için birkaç seçeneğiniz vardır:

- [**Rake** ](https://github.com/jimweirich/rake) – üzerinde Ruby dayalı olarak projeler derleme için bir etki alanına özgü dil (DSL) budur. Rake popülerliği avantajlarından ve kitaplıkları zengin ekosistemi vardır.

- [**psake** ](https://github.com/psake/psake) – bu yazılım oluşturmak için bir Windows Powershell kitaplığıdır

- [**SAHTE** ](http://fsharp.github.io/FAKE/) – F varolan .NET kitaplıklarına gerekirse kullanmak mümkün kılan # dayalı DSL budur.

Komut dosyası dili kullanılır, Tercihler ve gereksinimlerine bağlıdır. [TaskyPro Calabash](https://github.com/xamarin/test-cloud-samples/tree/master/TaskyPro/TaskyPro-Calabash) örnek olarak Rake kullanma örneği içeren bir [komut dosyası derleme](https://github.com/xamarin/test-cloud-samples/blob/master/TaskyPro/TaskyPro-Calabash/Rakefile).

> [!NOTE]
> MSBuild veya NAnt, ancak bu yetersizliği gibi bir XML tabanlı yapı sistem anlamlılık ve yazılım oluşturmak için adanmış bir DSL Bakımı kullanmak da mümkündür.

### <a name="parameterizing-the-build-script"></a>Derleme betiğinin kümesini parametreleştirme

Derleme ve yazılım testi işlemi gizli tutulması bilgileri gerektirir. Özellikle bir APK oluşturma anahtar deposu ve/veya anahtar deposunun anahtar diğer adı için bir parola gerektirebilir. Benzer şekilde, Test bulut geliştiriciler için benzersiz bir API anahtarı gerekir. Bu tür değerleri yapı komut dosyasında kodlanmış sabit olmamalıdır. Bunun yerine bunların değişkenleri olarak yapı komut dosyasına geçirilmesi gerekir.

İOS cihaz kimliği veya ne Test bulut için kullanması gereken cihazları test tanımlamak Android cihaz kimliği çalıştırdığı gibi daha az hassas değerlerdir. Bunlar korunması gereken değerleri değildir, ancak Yapıdan yapıya değişebilir.

Derleme betiğindeki dışında değişkenlerin bu türleri depolama de, örneğin geliştiricilere yapı komut dosyası bir kuruluş içinde paylaşmak kolaylaştırır. Geliştiriciler tam aynı komut dosyasını derleme sunucusu olarak kullanabilir, ancak kendi keystores ve API anahtarlarını kullanabilirsiniz.

Bu hassas değerlerini depolamak için kullanılabilecek iki seçenek vardır:

- **Bir yapılandırma dosyası** – bu değer sürüm denetimine değil işaretlenmelidir Test bulut API anahtarını korumak için. Dosya her makine için oluşturulabilir. Değerleri bu dosyadan okunan nasıl kullanılan komut dosyası dile bağlıdır.

- **Ortam değişkenleri** – bunlar kolayca bir makine başına temeline ve ared bağımsız temel alınan komut dosyası dili olarak ayarlanabilir.

Olumlu ve olumsuz yönleri her Bu seçenekler vardır. Bu kılavuz yapı komut dosyalarını oluştururken bu teknik önerir şekilde TeamCity ortam değişkenleri ile sorunsuz şekilde çalışır.

### <a name="build-steps"></a>Derleme adımları

Derleme betiğinin aşağıdaki adımları gerçekleştirmeniz mümkün olması gerekir:

- **Uygulamayı derleyin** – bu uygulama doğru sağlama profili ile imzalanmasını içerir.

- **Xamarin Test Cloud uygulamaya gönderme** – Bu, imzalama ve uygun bir anahtar ile APK hizalama ZIP içerir.

Bu iki adımı aşağıdaki daha ayrıntılı olarak açıklanacaktır.

#### <a name="compiling-a-xamarinios-application"></a>Bir Xamarin.iOS uygulaması derleme

[!include[](~/tools/ci/includes/commandline-compile-of-xamarin-ios-ipa.md)]

#### <a name="compiling-a-xamarinandroid-application"></a>Bir Xamarin.Android uygulaması derleme

Bir Android uygulaması derlemek için kullanmak **xbuild** (veya **msbuild** Windows'da):

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:SignAndroidPackage /p:Configuration=Release /path/to/android.csproj
```

Xamarin Android uygulaması derlemek için dikkat **xbuild** iOS uygulamanızı oluşturmak için kullanan proje ve **xbuild** çözüm gerektirir.

#### <a name="submitting-xamarinuitests-to-test-cloud"></a>Test buluta Xamarin.UITests gönderiliyor

UITests kullanarak gönderildiğinde `test-cloud.exe` aşağıdaki kod parçacığında gösterildiği gibi uygulama:

```bash
test-cloud.exe <path-to-apk-or-ipa-file> <test-cloud-team-api-key> --devices <device-selection-id> --assembly-dir <path-to-tests-containing-test-assemblies> --nunit-xml report.xml --user <email>
```

Testi çalıştırdığınızda, test sonuçlarını adlı bir NUnit stili XML dosyası biçiminde döndüreceği **report.xml**. TeamCity derleme günlüğünde bilgileri görüntüler.

Test buluta UITests gönderme hakkında daha fazla bilgi için üzerinde Xamarin'ın kılavuzuna başvurun [gönderme testleri Test buluta](https://developer.xamarin.com/guides/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/).

#### <a name="submitting-calabash-tests-to-test-cloud"></a>Bulut Test etmek için Calabash testleri gönderiliyor

Calabash testleri kullanarak gönderildiğinde `test-cloud` aşağıdaki kod parçacığında gösterildiği gibi gem:

```bash
test-cloud submit /path/to/APK-or-IPA <test-cloud-team-api-key> --devices <device-id> --user <email>
```
Bir Android uygulamasını Test buluta göndermek için önce calabash android kullanarak APK test sunucusunu yeniden oluşturmak gereklidir:

```bash
$ calabash-android build </path/to/signed/APK>
$ test-cloud submit /path/to/APK <test-cloud-team-api-key> --devices <ANDROID_DEVICE_ID> --profile=android --config=config/cucumber.yml --pretty
```
Calabash testleri gönderme ile ilgili daha fazla bilgi için lütfen üzerinde Xamarin'ın kılavuzuna başvurun [gönderme Calabash testleri Test buluta](https://developer.xamarin.com/guides/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/).

## <a name="creating-a-teamcity-project"></a>TeamCity proje oluşturma

TeamCity yüklü olduğundan ve Mac için Visual Studio, projenizin oluşturabilirsiniz sonra projeyi derlemek ve Test buluta göndermek üzere TeamCity proje oluşturma zamanı geldi.

1. Web tarayıcısı aracılığıyla TeamCity açarak başlatıldı. Kök projesine gidin:

    ![Kök proje Git](teamcity-images/image2.png "kök proje Git") kök proje altında yeni bir alt projesi oluşturun:

    ![Kök proje altındaki kök projesine gidin, yeni bir alt proje oluşturma](teamcity-images/image3.png "Bul kök proje altındaki kök projesi için yeni bir alt projesi oluşturma")
2. Alt Proje oluşturulduktan sonra yeni bir yapı yapılandırması ekleyin:

    ![Alt Proje oluşturulduktan sonra yeni bir yapı yapılandırma eklemek](teamcity-images/image5.png "alt proje oluşturulduktan sonra yeni bir yapı yapılandırması Ekle")
3. VC proje için yapı yapılandırması ekleyin. Bu sürüm denetim ayarı ekran gerçekleştirilir:

    ![Bu sürüm denetim ayarı ekran yapılır](teamcity-images/image6.png "bu sürüm denetim ayarı ekran gerçekleştirilir")

    Oluşturulan hiçbir VC proje ise, aşağıda gösterilen yeni VC kök sayfasından oluşturmak için seçeneğiniz vardır:

    ![Oluşturulan hiçbir VC proje varsa, yeni VC kök sayfasından oluşturmak için seçeneğiniz vardır](teamcity-images/image7.png "oluşturulan hiçbir VC proje varsa, yeni VC kök sayfasından oluşturmak için seçeneğiniz vardır")

    VC kök bağlandıktan sonra derleme adımları tespit TeamCity checkout proje ve otomatik olarak deneyin. İle TeamCity bilginiz varsa, algılanan oluşturma adımlarının birini seçebilirsiniz. Şu an için algılanan oluşturma adımlarının yoksaymak güvenlidir.

4. Ardından, bir derleme tetikleyici yapılandırın. Belirli koşullar gerçekleştiğinde ne zaman bir kullanıcı kodu depoya kaydeder gibi bir yapıyı sıraya koyar. Aşağıdaki ekran görüntüsünde bir yapı tetikleyici eklemeyi gösterir:

    ![Bu ekran görüntüsünde bir yapı tetikleyici eklemeyi gösterir](teamcity-images/image8.png "bu ekran görüntüsünde bir yapı tetikleyici eklemeyi gösterir") yapı tetikleyici yapılandırma örneği aşağıdaki ekran görüntüsünde görebilirsiniz:

    ![Bu ekran görüntüsünde bir yapı tetikleyici yapılandırma örneği görülebilir](teamcity-images/image9.png "bu ekran görüntüsünde bir yapı tetikleyici yapılandırma örneği görülebilir")

5. Ortam değişkenleri olarak bazı değerlerini depolama yapı komut kümesini parametreleştirme önceki bölümde önerilen. Bu değişkenler yapı yapılandırma parametreleri ekran aracılığıyla eklenebilir. Test bulut API anahtarı, iOS cihaz kimliği ve aşağıdaki ekran görüntüsünde gösterildiği gibi Android cihaz kimliği için değişkenleri ekleyin:

    ![Test bulut API anahtarı, iOS cihaz kimliği ve Android cihaz kimliği için değişkenleri eklemek](teamcity-images/image11.png "Test bulut API anahtarı, iOS cihaz kimliği ve Android cihaz kimliği için değişkenleri ekleyin")

6. Sıraya alma Test bulut uygulamaya ve uygulama derlemek için derleme betiğindeki çağıracağı bir derleme adımı eklemek için son adımdır bakın. Aşağıdaki ekran görüntüsü, bir uygulama oluşturmak için bir Rakefile kullanan bir derleme adımı örneğidir:

    ![Bu ekran görüntüsünde bir uygulama oluşturmak için bir Rakefile kullanan bir derleme adımı örneğidir](teamcity-images/image12.png "bu ekran görüntüsünde bir uygulama oluşturmak için bir Rakefile kullanan bir derleme adımı örneğidir")

7. Bu noktada, yapı yapılandırması tamamlanır. Projeyi düzgün şekilde yapılandırıldığını doğrulamak için bir derlemeyi tetiklemek için iyi bir fikirdir. Bunu yapmak için iyi bir depoya küçük, önemsiz bir değişikliği kaydetmek için yoldur. TeamCity yürütme algılar ve bir yapı başlatmak gerekir.

8. Yapı tamamlandıktan sonra yapı günlüğünü İncele ve herhangi bir sorun ya da ilgilenilmesi gereken derleme uyarılarla olup olmadığını görebilirsiniz.

## <a name="summary"></a>Özet

Bu kılavuz TeamCity Xamarin mobil uygulamaları oluşturmak ve bunları Test buluta göndermek için nasıl kullanılacağını ele. Derleme işlemini otomatikleştirmek için bir yapı komut dosyası oluşturma açıklanmıştır. Derleme betiğinin uygulama derleme, Test buluta gönderme ve sonuçları için bekleyen ilgilenir

Ardından bir geliştirici kodu uygulayan her zaman bir yapıyı sıraya ve derleme betiğindeki çağıracak TeamCity içinde bir proje oluşturmak nasıl ele.

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin Test Cloud (UITest) testleri gönderiliyor](https://developer.xamarin.com~/testcloud/uitest/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Xamarin Test Cloud (Calabash) testleri gönderiliyor](https://developer.xamarin.com~/testcloud/calabash/working-with/submitting-tests-to-xamarin-test-cloud/)
- [Yükleme ve TeamCity yapılandırma](http://confluence.jetbrains.com/display/TCD8/Installing+and+Configuring+the+TeamCity+Server)
