---
title: Xamarin.iOS errors
ms.topic: article
ms.prod: xamarin
ms.assetid: 9F76162B-D622-45DA-996B-2FBF8017E208
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/26/2017
ms.openlocfilehash: 5a11ca19de7ce06088478f1a39dae5a246d701a8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinios-errors"></a>Xamarin.iOS errors


## <a name="mt0xxx-mtouch-error-messages"></a>MT0xxx: mtouch hata iletileri

Örneğin Parametreler, ortam, Araçlar eksik.

<!--
 MT0xxx mtouch itself, e.g. parameters, environment (e.g. missing tools)
 https://github.com/xamarin/xamarin-macios/blob/master/tools/mtouch/error.cs
    -->

### <a name="a-namemt0000mt0000-unexpected-error---please-fill-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT0000"/>MT0000: Beklenmeyen bir hata - lütfen http://bugzilla.xamarin.com sırasında bir hata raporu doldurun

Beklenmeyen bir hata durumu oluştu. Lütfen [bir hata raporu dosya](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) mümkün olduğunca fazla bilgi ile de dahil olmak üzere:

* Tam ile en fazla ayrıntı günlükleri, yapı (ör `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**);
* Hatayı yeniden oluşturmaya en az test durumu; ve
* Tüm sürüm bilgileri

Tam sürüm bilgilerini almak için en kolay yolu kullanmaktır **Mac için Visual Studio** menüsünde **Mac için Visual Studio hakkında** öğesi, **ayrıntıları göster** düğmesi ve kopyalayıp yapıştırın sürüm bilgileri (kullanabileceğiniz **kopyalama bilgi** düğmesi).

### <a name="a-namemt0001mt0001--devname-was-provided-without-any-device-specific-action"></a><a name="MT0001"/>MT0001: '-devname' herhangi bir cihaza özel eylem sağlandı

-Devname herhangi bir cihaza özel eylemi olduğunda mtouch aktarılırsa yayılan bir uyarı budur (-logdev /-installdev /-killdev /-launchdev /-listapps) istendi.

### <a name="a-namemt0002mt0002-could-not-parse-the-environment-variable-"></a><a name="MT0002"/>MT0002: ortam değişkeni ayrıştırılamadı *.

Bu hata, bir Geçersiz ortam anahtarı ayarlamaya çalışırsanız oluşur. değişken değer çifti =. Doğru biçim aşağıdadır: `mtouch --setenv=VARIABLE=VALUE`

### <a name="a-namemt0003mt0003-application-name-exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a><a name="MT0003"/>MT0003: Uygulama adı ' * .exe ' bir SDK veya ürün derlemesi (.dll) adıyla çakışıyor.

Yürütülebilir derlemenin adı ve uygulamanın adı herhangi bir dll için uygulama adı aynı olamaz. Lütfen, yürütülebilir dosyanın adını değiştirin.

### <a name="a-namemt0004mt0004-new-refcounting-logic-requires-sgen-to-be-enabled-too"></a><a name="MT0004"/>MT0004: Yeni refcounting mantık SGen çok etkin olmasını gerektirir.

Refcounting uzantısı etkinleştirirseniz, projenin iOS derleme seçenekleri (Gelişmiş sekmesinde) içinde SGen atık toplayıcı etkinleştirmeniz gerekir.

Bu gereksinim 7.2.1 Xamarin.iOS ile başlayan kaldırılmış, yeni refcounting mantığı Boehm ve SGen atık toplayıcıları ile etkinleştirilebilir.

### <a name="a-namemt0005mt0005-the-output-directory--does-not-exist"></a><a name="MT0005"/>MT0005: Çıktı dizini * yok.

Lütfen dizin oluşturun.

Bu hata artık oluşturulmaz, yoksa mtouch dizini otomatik olarak oluşturur.

### <a name="a-namemt0006mt0006-there-is-no-devel-platform-at--use---platformplat-to-specify-the-sdk"></a><a name="MT0006"/>MT0006: Hiçbir devel platform konumunda olduğundan *, kullanın--platform SDK belirtmek için PLAT =.

Xamarin.iOS hata iletisinde belirtilen konumda SDK dizini bulunamıyor. Yolun doğru olduğundan emin olun.

### <a name="a-namemt0007mt0007-the-root-assembly--does-not-exist"></a><a name="MT0007"/>MT0007: Kök derleme * yok.

Xamarin.iOS hata iletisinde belirtilen konumda derlemesi bulunamıyor. Yolun doğru olduğundan emin olun.

### <a name="a-namemt0008mt0008-you-should-provide-one-root-assembly-only-found--assemblies-"></a><a name="MT0008"/>MT0008: Bir kök derleme yalnızca, bulunan # derlemeleri sağlamalısınız: *.

Yalnızca bir kök derleme olabilir ancak birden fazla kök derleme mtouch için geçirildi.

### <a name="a-namemt0009mt0009-error-while-loading-assemblies-"></a><a name="MT0009"/>MT0009: derlemeleri yüklenirken hata oluştu: *.

Kök derleme başvurularını derlemeleri yüklenirken bir hata oluştu. Daha fazla bilgi derleme çıktısında sağlanabilir.

### <a name="a-namemt0010mt0010-could-not-parse-the-command-line-arguments-"></a><a name="MT0010"/>MT0010: komut satırı bağımsız değişkenleri ayrıştırılamadı: *.

Komut satırı bağımsız değişkenleri ayrıştırılırken bir hata oluştu. Bunların tümü doğru olduğunu doğrulayın.

### <a name="a-namemt0011mt0011--was-built-against-a-more-recent-runtime--than-monotouch-supports"></a><a name="MT0011"/>MT0011: * MonoTouch desteklediğinden daha daha yeni bir çalışma zamanı karşı (*) oluşturuldu.

Proje Xamarin.iOS BCL kullanarak oluşturulmadı sınıf kitaplığına bir başvuru olduğundan bu uyarı genellikle bildirilir.

.NET 4.0 SDK'yı kullanarak bir uygulamayı yalnızca .NET 2. 0'ı destekleyen bir sistemde çalışmayabilir aynı şekilde, .NET 4.0 kullanılarak oluşturulmuş bir kitaplık Xamarin.iOS üzerinde çalışmayabilir, Xamarin.iOS üzerinde mevcut değil API kullanabilir.

Genel olarak bir Xamarin.iOS sınıf kitaplığı kitaplığını oluşturmak için çözümüdür. Bu yeni bir Xamarin.iOS sınıf kitaplığı proje oluşturarak gerçekleştirilebilir ve tüm kaynak dosyalarını ekleyin. Kitaplık için kaynak kodu yoksa satıcısına başvurun ve bunların kitaplığı Xamarin.iOS uyumlu bir sürümü sağladıkları isteği.

### <a name="a-namemt0012mt0012-incomplete-data-is-provided-to-complete-"></a><a name="MT0012"/>MT0012: Eksik veri tamamlamak için sağlanan *.

Bu hata artık Xamarin.iOS geçerli sürümde bildirilmedi.

### <a name="a-namemt0013mt0013-profiling-support-requires-sgen-to-be-enabled-too"></a><a name="MT0013"/>MT0013: Destek profil sgen çok etkin olmasını gerektirir.

SGen (--sgen) profil, etkinleştirilmelidir (--Profil) etkinleştirilmiştir.

### <a name="a-namemt0014mt0014-the-ios--sdk-does-not-support-building-applications-targeting-"></a><a name="MT0014"/>MT0014: İOS * SDK hedefleme uygulamaları oluşturma desteklemediği *.

Bu aşağıdaki durumlarda oluşabilir:

*  ARMv6 etkin ve Xcode 4.5 veya üstü yüklü değil.
*  ARMv7s etkindir ve Xcode 4,4 veya önceki bir sürümü yüklü.


Xcode'nın yüklü sürümü seçili mimarilerin desteklediğini doğrulayın.

### <a name="a-namemt0015mt0015-invalid-abi--supported-abis-are-i386-x8664--armv7-armv7llvm-armv7llvmthumb2-armv7s-armv7sllvm-armv7sllvmthumb2-arm64-and-arm64llvm"></a><a name="MT0015"/>MT0015: Geçersiz ABI: *. Desteklenen ABIs şunlardır: i386, x86_64, armv7, armv7 llvm, armv7 + llvm + thumb2, armv7s, armv7s + llvm, armv7s + llvm + thumb2, arm64 ve arm64 + llvm.

Geçersiz bir ABI mtouch için geçirildi. Lütfen geçerli bir ABI belirtin.

### <a name="a-namemt0016mt0016-the-option--has-been-deprecated"></a><a name="MT0016"/>MT0016: Seçeneği * kullanım dışı bırakıldı.

Belirtilen mtouch seçeneği kullanım dışı bırakıldı ve yok sayılacak.

### <a name="a-namemt0017mt0017-you-should-provide-a-root-assembly"></a><a name="MT0017"/>MT0017: Kök derleme sağlamalıdır.

Bir kök derleme (genellikle ana yürütülebilir) belirtmek için gerekli olan bir uygulama oluştururken.

### <a name="a-namemt0018mt0018-unknown-command-line-argument-"></a><a name="MT0018"/>MT0018: Bilinmeyen komut satırı bağımsız değişkeni: *.

Hata iletisinde belirtilen komut satırı bağımsız değişkeni Mtouch tanımıyor.

### <a name="a-namemt0019mt0019-only-one---loginstallkilllaunchdev-or---launchdebugsim-option-can-be-used"></a><a name="MT0019"/>MT0019: Tek--[günlük | yükleme | KILL | başlatma] geliştirme veya--[başlatın | hata ayıklama] SIM seçeneği kullanılabilir.

Aynı anda kullanılamaz mtouch için birkaç seçenek vardır:

-  --logdev
-  --installdev
-  --killdev
-  --launchdev
-  --launchdebug
-  --launchsim




### <a name="a-namemt0020mt0020-the-valid-options-for--are-"></a><a name="MT0020"/>MT0020 İçin geçerli seçenek '\*'olan'\*'.



### <a name="a-namemt0021mt0021-cannot-compile-using-gccg---use-gcc-when-using-the-static-registrar-this-is-the-default-when-compiling-for-device-either-remove-the---use-gcc-flag-or-use-the-dynamic-registrar---registrardynamic"></a><a name="MT0021"/>MT0021 olamaz derleme gcc kullanarak / g ++ (--kullanım gcc) (Bu, varsayılan cihaz için derlerken) statik kayıt kullanırken. Kaldırabilir ya da kullanım gcc bayrak veya dinamik kayıt kullanın (--Kaydedici: dinamik).



### <a name="a-namemt0022mt0022-the-options---unsupported--enable-generics-in-registrar-and---registrar-are-not-compatible"></a><a name="MT0022"/>MT0022 Seçenekleri '--desteklenmeyen--enable-genel türler-içinde-kayıt şirketi' ve '--Kaydedici ' uyumlu değil.

Her iki seçenek kaldırmak `--unsupported--enable-generics-in-registrar` ve `--registrar`. Varsayılan kayıt 7.2.1 Xamarin.iOS ile başlayarak, genel türler destekler.

Bu hata artık gösterilir (komut satırı bağımsız değişkeni `--unsupported--enable-generics-in-registrar` mtouch kaldırılmıştır).

### <a name="a-namemt0023mt0023-application-name-exe-conflicts-with-another-user-assembly"></a><a name="MT0023"/>MT0023 uygulama adı ' * .exe ' başka bir kullanıcı derlemesi ile çakışıyor.

Yürütülebilir derlemenin adı ve uygulamanın adı herhangi bir dll için uygulama adı aynı olamaz. Lütfen, yürütülebilir dosyanın adını değiştirin.

### <a name="a-namemt0024mt0024-could-not-find-required-file-"></a><a name="MT0024"/>MT0024 bulamadı gerekli dosya ' *'.



### <a name="a-namemt0025mt0025-no-sdk-version-was-provided-please-add---sdkxy-to-specify-which-ios-sdk-should-be-used-to-build-your-application"></a><a name="MT0025"/>MT0025 Hayır SDK sürümü sağlandı. Lütfen eklemek `--sdk=X.Y` hangi iOS belirtmek için SDK uygulamanızı oluşturmak için kullanılmalıdır.



### <a name="a-namemt0026mt0026-could-not-parse-the-command-line-argument--"></a><a name="MT0026"/>MT0026 verebilir değil ayrıştırma komut satırı bağımsız değişkeni ' *': *



### <a name="a-namemt0027mt0027-the-options--and--are-not-compatible"></a><a name="MT0027"/>MT0027 Seçenekleri\*'ve'\*' uyumlu değil.



### <a name="a-namemt0028mt0028-cannot-enable-pie--pie-when-targeting-ios-41-or-earlier-please-disable-pie--piefalse-or-set-the-deployment-target-to-at-least-ios-42"></a><a name="MT0028"/>MT0028 etkinleştiremiyor PASTA (-pasta) iOS 4.1 veya önceki hedeflerken. Lütfen PASTA devre dışı bırakın (-pasta: false) veya dağıtım hedefi en az kümesine iOS 4.2



### <a name="a-namemt0029mt0029-repl---enable-repl-is-only-supported-in-the-simulator---sim"></a><a name="MT0029"/>MT0029: REPL (--etkinleştir repl) yalnızca benzeticisinde desteklenir (--SIM).

Simülatörü oluşturuluyorsa REPL yalnızca desteklenir. Bunun anlamı geçirirseniz `--enable-repl` mtouch için aynı zamanda geçmelidir `--sim`.

### <a name="a-namemt0030mt0030-the-executable-name--and-the-app-name--are-different-this-may-prevent-crash-logs-from-getting-symbolicated-properly"></a><a name="MT0030"/>MT0030: Yürütülebilir dosya adına (\*) ve uygulama adı (\*) bu engelleyebilir kilitlenme günlükleri düzgün symbolicated öğesinden farklı.

Ne zaman Xcode symbolicates (adları işlev ve dosya/satır numaraları için bellek adresleri çevirir) uygulama ve yürütülebilir dosya (uzantısı olmadan) farklı adlara sahip işlemi başarısız olabilir.

Düzeltmek için bu ya da değişikliği 'Uygulama adı' projenin yapı/iOS uygulama seçenekleri veya Değiştir 'Derleme adı' projenin yapı/çıkış seçenekleri.

### <a name="a-namemt0031mt0031-the-command-line-arguments---enable-background-fetch-and---launch-for-background-fetch-require---launchsim-too"></a><a name="MT0031"/>MT0031: Komut satırı bağımsız değişkenleri '--etkinleştir arka planda getirmeye ' ve '--başlatma için arka planda getirmeye ' gerekli '--launchsim' çok.

### <a name="a-namemt0032mt0032-the-option---debugtrack-is-ignored-unless---debug-is-also-specified"></a><a name="MT0032"/>MT0032: Seçeneği '--debugtrack' sürece yoksayılır '--hata ayıklama ' da belirtilmiş.

### <a name="a-namemt0033mt0033--a-xamarinios-project-must-reference-either-monotouchdll-or-xamariniosdll"></a><a name="MT0033"/>MT0033 bir Xamarin.iOS projesi monotouch.dll veya Xamarin.iOS.dll başvurmalıdır

### <a name="a-namemt0034mt0034--cannot-include-both-monotouchdll-and-xamariniosdll-in-the-same-xamarinios-project----is-referenced-explicitly-while--is-referenced-by-"></a><a name="MT0034"/>MT0034 içeremez 'monotouch.dll' ve 'Xamarin.iOS.dll' aynı Xamarin.iOS projede - '\*' olduğunu açıkça başvurulan while '\*' tarafından başvurulan ' *'.

<!-- MT0035 unused -->

### <a name="a-namemt0036mt0036--cannot-launch-a--simulator-for-a--app-please-enable-the-correct-architectures-in-your-projects-ios-build-options-advanced-page"></a><a name="MT0036"/>MT0036 olamaz başlatma bir * simülatörü bir * uygulama. Lütfen doğru architecture(s) projenizin iOS derleme seçenekleri (Gelişmiş sayfası) içinde etkinleştirin.

### <a name="a-namemt0037mt0037--monotouchdll-is-not-64-bit-compatible-either-reference-xamariniosdll-or-do-not-build-for-a-64-bit-architecture-arm64-andor-x8664"></a><a name="MT0037"/>MT0037 monotouch.dll 64-bit uyumludur. Xamarin.iOS.dll başvuru ya da 64-bitlik bir mimari için (ARM64 ve/veya x86_64) derleme değil.

### <a name="a-namemt0038mt0038--the-old-registrars---registraroldstaticolddynamic-are-not-supported-when-referencing-xamariniosdll"></a><a name="MT0038"/>MT0038 Eski kaydedicilerin (--Kaydedici: oldstatic | olddynamic) Xamarin.iOS.dll başvururken desteklenmez.

### <a name="a-namemt0039mt0039--applications-targeting-armv6-cannot-reference-xamariniosdll"></a><a name="MT0039"/>MT0039 ARMv6 hedefleme uygulamaları Xamarin.iOS.dll başvuramaz.

### <a name="a-namemt0040mt0040--could-not-find-the-assembly--referenced-by-"></a><a name="MT0040"/>MT0040 bulamadı derlemesi '\*', tarafından başvuruda bulunulan'\*'.

### <a name="a-namemt0041mt0041--cannot-reference-both-monotouchdll-and-xamariniosdll"></a><a name="MT0041"/>MT0041 'monotouch.dll' ve 'Xamarin.iOS.dll' başvuramaz.

### <a name="a-namemt0042mt0042--no-reference-to-either-monotouchdll-or-xamariniosdll-was-found-a-reference-to-monotouchdll-will-be-added"></a><a name="MT0042"/>Monotouch.dll veya Xamarin.iOS.dll MT0042 Hayır başvuru bulundu. Monotouch.dll bir başvuru eklenir.

### <a name="a-namemt0043mt0043-the-boehm-garbage-collector-is-currently-not-supported-when-referencing-xamariniosdll-the-sgen-garbage-collector-has-been-selected-instead"></a><a name="MT0043"/>MT0043: Boehm atık toplayıcı şu anda 'Xamarin.iOS.dll' başvururken desteklenmiyor. SGen atık toplayıcı yerine seçilmedi.

Yalnızca SGen çöp toplayıcı Unified projeleri ile desteklenir. Çöp toplayıcı Boehm belirten hiçbir ek mtouch işaretleri olmadığından emin olun.

### <a name="a-namemt0044mt0044---listsim-is-only-supported-with-xcode-60-or-later"></a><a name="MT0044"/>MT0044:--listsim yalnızca Xcode 6.0 veya sonraki sürümlerde desteklenir.

Yeni bir Xcode sürümü yükleyin.

### <a name="a-namemt0045mt0045---extension-is-only-supported-when-using-the-ios-80-or-later-sdk"></a><a name="MT0045"/>MT0045:--uzantısı iOS kullanırken yalnızca desteklenen SDK 8.0 (veya üstü).

<!-- MT0046 is not reported anymore -->

### <a name="a-namemt0047mt0047-the-minimum-deployment-target-for-unified-applications-is-511-the-current-deployment-target-is--please-select-a-newer-deployment-target-in-your-projects-ios-application-options"></a><a name="MT0047"/>MT0047: Unified uygulamalar için en düşük dağıtım hedef 5.1.1, geçerli dağıtım hedefi ' *'. Lütfen daha yeni bir dağıtım hedef projenizin iOS uygulama seçenekleri seçin.

<!-- MT0048 is not reported anymore -->

### <a name="a-namemt0049mt0049-framework-is-supported-only-if-deployment-target-is-80-or-later--features-might-not-work-correctly"></a><a name="MT0049"/>MT0049: Yalnızca dağıtım hedef 8.0 veya üstünü ise *.framework desteklenir. * özellikleri düzgün çalışmayabilir.

Belirtilen çerçeve, dağıtım hedef başvurduğu iOS sürümünde desteklenmiyor. Dağıtım hedefi iOS bir sürüme güncelleştirmek ya da uygulamadan belirtilen çerçeve kullanımını kaldırın.

<!-- MT0050 is not reported anymore -->

### <a name="a-namemt0051mt0051-xamarinios--requires-xcode-50-or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0051"/>MT0051: Xamarin.iOS * Xcode 5.0 veya sonrasını gerektirir. Geçerli Xcode sürüm (bulunan *) olan *.

Daha yeni bir Xcode yükleyin.

### <a name="a-namemt0052mt0052-no-command-specified"></a><a name="MT0052"/>MT0052: komutu belirtildi.

Hiçbir eylem mtouch için belirtildi.

<!-- 0053 is used by mmp -->

### <a name="a-namemt0054mt0054-unable-to-canonicalize-the-path--"></a><a name="MT0054"/>MT0054: Yol temeline için ' *': *

Bu bir iç hatadır. Bu hatayı görürseniz, lütfen dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt0055mt0055-the-xcode-path--does-not-exist"></a><a name="MT0055"/>MT0055: Xcode yolu ' *' yok.

Xcode yolu geçirilen kullanarak `--sdkroot` yok. Lütfen geçerli bir yol belirtin.

### <a name="a-namemt0056mt0056-cannot-find-xcode-in-the-default-location-applicationsxcodeapp-please-install-xcode-or-pass-a-custom-path-using---sdkroot-path"></a><a name="MT0056"/>MT0056: Xcode varsayılan konumda bulamıyor (/ Applications/Xcode.app). Xcode yüklemek ya da--sdkroot kullanarak özel bir yol geçirmek <path>.
### <a name="a-namemt0057mt0057-cannot-determine-the-path-to-xcodeapp-from-the-sdk-root--please-specify-the-full-path-to-the-xcodeapp-bundle"></a><a name="MT0057"/>MT0057: sdk kökünden yolu için Xcode.app belirleyemiyor ' *'. Lütfen Xcode.app paket tam yolunu belirtin.

Yolun geçirilen kullanarak `--sdkroot` geçerli bir Xcode uygulama belirtmiyor. Lütfen bir Xcode uygulama için bir yol belirtin.

### <a name="a-namemt0058mt0058-the-xcodeapp--is-invalid-the-file--does-not-exist"></a><a name="MT0058"/>MT0058: Xcode.app '\*' geçersiz (dosya '\*' yok).

Yolun geçirilen kullanarak `--sdkroot` geçerli bir Xcode uygulama belirtmiyor. Lütfen bir Xcode uygulama için bir yol belirtin.

### <a name="a-namemt0059mt0059-could-not-find-the-currently-selected-xcode-on-the-system-"></a><a name="MT0059"/>MT0059: şu anda seçili Xcode sistemde bulunamadı: *

### <a name="a-namemt0060mt0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned--but-that-directory-does-not-exist"></a><a name="MT0060"/>MT0060: şu anda seçili Xcode sistemde bulunamadı. 'xcode döndürülen Seç--yazdırma yolu' ' *', ancak bu dizin mevcut değil.

### <a name="a-namemt0061mt0061-no-xcodeapp-specified-using---sdkroot-using-the-system-xcode-as-reported-by-xcode-select---print-path-"></a><a name="MT0061"/>MT0061: (--sdkroot kullanarak), belirtilen hiçbir Xcode.app Xcode 'xcode-göre Seç--yazdırma yolu' bildirilen sistemiyle: *

Xcode olacağı açıklayan bir bilgilendirme uyarısı budur hiçbiri belirtilmediğinden kullanılır.

### <a name="a-namemt0062mt0062-no-xcodeapp-specified-using---sdkroot-or-xcode-select---print-path-using-the-default-xcode-instead-"></a><a name="MT0062"/>MT0062: (--sdkroot 'xcode-seçin--yazdırma-path' veya'ı kullanarak), bunun yerine varsayılan Xcode kullanarak belirtilen hiçbir Xcode.app: *

Xcode olacağı açıklayan bir bilgilendirme uyarısı budur hiçbiri belirtilmediğinden kullanılır.

### <a name="a-namemt0063mt0063-cannot-find-the-executable-in-the-extension--no-cfbundleexecutable-entry-in-its-infoplist"></a><a name="MT0063"/>MT0063: yürütülebilir dosya uzantısı'nda bulunamıyor * (hiçbir CFBundleExecutable girişi Info.plist)

Bir giriş derleme sırasında otomatik olarak oluşturulması gerekir ancak her Info.plist (CFBundleExecutable girişi kullanarak) yürütülebilir olması gerekir.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0064mt0064-xamarinios-only-supports-embedded-frameworks-with-unified-projects"></a><a name="MT0064"/>MT0064: Xamarin.iOS yalnızca katıştırılmış çerçeveleri Unified projelerle destekler.

Xamarin.iOS birleşik API kullanırken yalnızca katıştırılmış çerçeveleri destekler; Lütfen Unified API kullanmak için projenizin güncelleştirin.

### <a name="a-namemt0065mt0065-xamarinios-only-supports-embedded-frameworks-when-deployment-target-is-at-least-80-current-deployment-target--embedded-frameworks-"></a><a name="MT0065"/>MT0065: dağıtım hedefi en az 8.0 olduğunda Xamarin.iOS yalnızca katıştırılmış çerçeveleri destekler (geçerli dağıtım hedefi: * çerçeveleri katıştırılmış: *)

(Daha önceki iOS sürümlerini desteklemez çünkü katıştırılmış çerçeveler) dağıtım hedefi en az 8.0 olduğunda Xamarin.iOS yalnızca katıştırılmış çerçeveleri destekler.

Lütfen 8.0 veya daha yüksek bir projenin Info.plist dağıtım hedef güncelleştirin.

### <a name="a-namemt0066mt0066-invalid-build-registrar-assembly-"></a><a name="MT0066"/>MT0066: Geçersiz yapı Kaydedici derleme: *

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0067mt0067-invalid-registrar-"></a><a name="MT0067"/>MT0067: Geçersiz Kaydedici: *

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0068mt0068-invalid-value-for-target-framework-"></a><a name="MT0068"/>MT0068: Hedef framework için geçersiz değer: *.

Geçersiz hedef framework kullanılarak geçildi hedef framework bağımsız değişkeni. Lütfen geçerli bir hedef framework belirtin.

<!--### <a name="MT0069"/>MT0069: currently unused -->

### <a name="a-namemt0070mt0070-invalid-target-framework--valid-target-frameworks-are-"></a><a name="MT0070"/>MT0070: Geçersiz hedef framework: *. Geçerli bir hedef çerçeveler şunlardır: *.

Geçersiz hedef framework kullanılarak geçildi hedef framework bağımsız değişkeni. Lütfen geçerli bir hedef framework belirtin.

### <a name="a-namemt0071mt0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinios-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT0071"/>MT0071: Bilinmeyen platform: *. Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen bir test çalışması ile http://bugzilla.xamarin.com sırasında bir hata raporu dosya.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0072mt0072-extensions-are-not-supported-for-the-platform-"></a><a name="MT0072"/>MT0072: Uzantıları platformu için desteklenmez. ' *'.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0073mt0073-xamarinios--does-not-support-a-deployment-target-of--the-minimum-is--please-select-a-newer-deployment-target-in-your-projects-infoplist"></a><a name="MT0073"/>MT0073: Xamarin.iOS * dağıtım hedefi desteklemiyor * (en az. *). Lütfen daha yeni bir dağıtım hedef projenizin Info.plist dosyasında seçin.

Hata iletisinde belirtilen minimum dağıtım hedefidir; Lütfen daha yeni bir dağıtım hedef projenin Info.plist dosyasında seçin.

Dağıtım hedefi güncelleştirme mümkün değilse, lütfen Xamarin.iOS daha eski bir sürümü kullanın.

### <a name="a-namemt0074mt0074-xamarinios--does-not-support-a-minimum-deployment-target-of--the-maximum-is--please-select-an-older-deployment-target-in-your-projects-infoplist-or-upgrade-to-a-newer-version-of-xamarinios"></a><a name="MT0074"/>MT0074: Xamarin.iOS * minimum dağıtım hedefi desteklemiyor * (en fazla *). Projenizin Info.plist dosyasında eski bir dağıtım hedefi seçin ya da Xamarin.iOS daha yeni bir sürüme yükseltin.

Xamarin.iOS Xamarin.iOS belirli bu sürümü için oluşturulmuş sürümden daha yüksek bir sürüm için en düşük dağıtım hedef ayarlanmasını desteklemiyor.

Lütfen projenin Info.plist dosyasında eski bir minimum dağıtım hedefi seçin ya da Xamarin.iOS daha yeni bir sürüme yükseltin.

### <a name="a-namemt0075mt0075-invalid-architecture--for--projects-valid-architectures-are-"></a><a name="MT0075"/>MT0075: Geçersiz mimarisi ' *' için * projeleri. Geçerli mimarileri şunlardır: *

Geçersiz bir mimari belirtildi. Lütfen mimarisi geçerli olduğunu doğrulayın.

### <a name="a-namemt0076mt0075-no-architecture-specified-using-the---abi-argument-an-architecture-is-required-for--projects"></a><a name="MT0076"/>MT0075: Mimari (--ABI bağımsız değişkeni kullanarak) belirtildi. Bir mimari için gerekli olan * projeleri.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0077mt0076-watchos-projects-must-be-extensions"></a><a name="MT0077"/>MT0076: WatchOS projeleri uzantıları olması gerekir.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0078mt0077-incremental-builds-are-enabled-with-a-deployment-target--80-currently--this-is-not-supported-the-resulting-application-will-not-launch-on-ios-9-so-the-deployment-target-will-be-set-to-80"></a><a name="MT0078"/>MT0077: Artımlı derlemeler ile dağıtım hedef < 8.0 etkin (şu anda *). Bu durum desteklenmez (elde edilen uygulama değil başlayacaktır iOS 9), dağıtım hedef 8.0 olarak ayarlanacak şekilde.

Böylece artımlı iş düzgün derlemeler için bu yapı 8.0 için dağıtım hedef ayarlanmış bildiren bir uyarı budur.

Artımlı derlemeler yalnızca dağıtım hedefi (elde edilen uygulama iOS 9 aksi başlatmaz çünkü) en az 8.0 olduğunda desteklenir.

### <a name="a-namemt0079mt0078-the-recommended-xcode-version-for-xamarinios--is-xcode--or-later-the-current-xcode-version-found-in--is-"></a><a name="MT0079"/>MT0078: Önerilen Xcode sürüm Xamarin.iOS için * xcode'un * veya sonraki bir sürümü. Geçerli Xcode sürüm (bulunan *) olan *.

Xcode geçerli sürümü, Xcode önerilen sürümünü Xamarin.iOS bu sürümü için olmadığını bildiren bir uyarı budur.

Lütfen en iyi davranışı sağlamak için Xcode yükseltin.

### <a name="a-namemt0080mt0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a><a name="MT0080"/>MT0080: NewRefCount devre dışı bırakma,--yeni-refcount:false, kullanım dışıdır.

Bu, bildiren bir uyarı olduğu yeni devre dışı bırakma isteği refcount (--yeni - refcount:false) yoksayıldı.

Yeni refcount özelliği tüm projeleri için zorunludur ve böylece artık devre dışı bırakmak mümkün değildir.

### <a name="a-namemt0081mt0081-the-command-line-argument---download-crash-report-also-requires---download-crash-report-to"></a><a name="MT0081"/>MT0081: Komut satırı bağımsız değişkeni--yükleme kilitlenme raporu da--yükleme-kilitlenme-rapor-gerektirir.
### <a name="a-namemt0082mt0082-repl---enable-repl-is-only-supported-when-linking-is-not-used---nolink"></a><a name="MT0082"/>MT0082: REPL (--etkinleştir repl) bağlama kullanılmadığı yalnızca desteklenir (--nolink).
### <a name="a-namemt0083mt0083-asm-only-bitcode-is-not-supported-on-watchos-use-either---bitcodemarker-or---bitcodefull"></a><a name="MT0083"/>MT0083: Yalnızca Asm bitcode'u watchOS üzerinde desteklenmiyor. Ya da--bitcode'u kullanın: işaret ya da--bitcode'u: tam.
### <a name="a-namemt0084mt0084-bitcode-is-not-supported-in-the-simulator-do-not-pass---bitcode-when-building-for-the-simulator"></a><a name="MT0084"/>MT0084: Benzeticisinde Bitcode'u desteklemiyor. --Bitcode'u simülatörü oluştururken geçmeyin.
### <a name="a-namemt0085mt0085-no-reference-to--was-found-it-will-be-added-automatically"></a><a name="MT0085"/>MT0085: Başvuru için ' *' bulundu. Bu otomatik olarak eklenir.
### <a name="a-namemt0086mt0086-a-target-framework---target-framework-must-be-specified-when-building-for-tvos-or-watchos"></a><a name="MT0086"/>MT0086: Hedef framework (--hedef framework) TVOS veya WatchOS oluştururken belirtilmelidir.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0087mt0087-incremental-builds---fastdev-is-not-supported-with-the-boehm-gc-incremental-builds-will-be-disabled"></a><a name="MT0087"/>MT0087: Artımlı derlemeler (--fastdev) Boehm GC ile desteklenmiyor. Artımlı derlemeler devre dışı bırakılacak.

### <a name="a-namemt0088mt0088-the-gc-must-be-in-cooperative-mode-for-watchos-apps-please-remove-the---coopfalse-argument-to-mtouch"></a><a name="MT0088"/>MT0088: GC watchOS uygulamalar için işbirlikçi modunda olması gerekir. Lütfen kaldırmak mtouch coop: false bağımsız değişkeni.

### <a name="a-namemt0089mt0089-the-option--cannot-take-the-value--when-cooperative-mode-is-enabled-for-the-gc"></a><a name="MT0089"/>MT0089: Seçeneği '\*'değerini alamaz'\*' için GC işbirlikçi modu etkinleştirildiğinde.

### <a name="a-namemt0091mt0091-this-version-of-xamarinios-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-set-the-managed-linker-behaviour-to-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a><a name="MT0091"/>MT0091: Xamarin.iOS bu sürümü gerektirir * SDK (Xcode ile birlikte gelen *). Ya da gerekli başlık dosyaları almak veya yönetilen bağlayıcı davranışa bağlantı Framework SDK'ları (yeni API'leri önlemek denemek için) yalnızca ayarlamak için Xcode yükseltin.

Xamarin.iOS uygulamanızı oluşturmak için hata iletisinde belirtilen SDK sürümündeki üstbilgi dosyaları gerektirir. Bu hatayı düzeltmek için önerilen yöntem gerekli SDK almak için Xcode yükseltmek için bu tüm gerekli başlık dosyaları dahil eder. Yüklü Xcode birden çok sürümü veya varsayılan olmayan bir konumda bir Xcode kullanmak istiyorsanız, IDE'nin tercihlerinde Xcode konumun doğru ayarladığınızdan emin olun.

Olası bir alternatif çözüm yönetilen bağlayıcı sağlamaktır. Bu, kullanılmayan API, çoğu durumda, üst bilgi dosyaları eksik (veya tamamlanmamış) nerede yeni API dahil kaldırır. Ancak projeniz olandan, Xcode daha yeni bir SDK sürümünde sunulan API kullanıyorsa bu çalışmaz sağlar.

Xamarin.iOS daha eski bir sürümü kullanmak için bir son straw çözüm olabilir, projenize SDK destekleyen bir gerektirir.

<!-- MT0092 used by mlaunch -->

### <a name="a-namemt0093mt0093-could-not-find-mlaunch"></a><a name="MT0093"/>MT0093: 'mlaunch' bulunamadı.

<!-- MT0094 is not reported anymore -->

### <a name="a-namemt0095mt0095-aot-files-could-not-be-copied-to-the-destination-directory-dest-error"></a><a name="MT0095"/>MT0095: Uygulama Nesne Ağacı dosyaları {taşınmaya} hedef dizinine kopyalanamadı: {error}

### <a name="a-namemt0096mt0096-no-reference-to-xamariniosdll-was-found"></a><a name="MT0096"/>MT0096: Xamarin.iOS.dll için hiçbir başvurusu bulunamadı.

<!-- MT0097: used by mmp -->
<!-- MT0098: used by mmp -->

### <a name="a-namemt0099mt0099-internal-error--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0099"/>MT0099: İç hata *. Bir test çalışması (http://bugzilla.xamarin.com) ile bir hata raporu dosya.

Xamarin.iOS içinde bir iç tutarsızlık denetimi başarısız olduğunda bu hata iletisini bildirilir.

Bu Xamarin.iOS bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0100mt0100-invalid-assembly-build-target--please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a><a name="MT0100"/>MT0100: Geçersiz derleme hedef: ' *'. Bir test çalışması (http://bugzilla.xamarin.com) ile bir hata raporu dosya.

Xamarin.iOS içinde bir iç tutarsızlık denetimi başarısız olduğunda bu hata iletisini bildirilir.

Bu her zaman Xamarin.iOS bir hata olduğunu; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) bir test çalışması ile.

### <a name="a-namemt0101mt0101-the-assembly--is-specified-multiple-times-in---assembly-build-target-arguments"></a><a name="MT0101"/>MT0101: Derleme ' *' derleme yapı hedefi değişkenlerinde--birden çok kez belirtildi.

Hata iletisinde belirtilen derleme değişkenlerdeki--derleme yapı hedef birden çok kez belirtildi. Her derleme yalnızca bir kez belirtiliyor emin olun.

### <a name="a-namemt0102mt0102-the-assemblies--and--have-the-same-target-name--but-different-targets--and-"></a><a name="MT0102"/>MT0102: Derlemeleri\*'ve'\*' hedefi ile aynı ada sahip ('\*'), ancak farklı hedefler ('\*' ve ' *').

Hata iletisinde belirtilen derleme çakışan derleme hedefleri sahip.

Örneğin:

    --assembly-build-target:Assembly1.dll=framework=MyBinary --assembly-build-target:Assembly2.dll=dynamiclibrary=MyBinary

Bu örnek dinamik kitaplığını ve bir çerçeve ile aynı marka oluşturulmaya çalışılırken (`MyBinary`).

### <a name="a-namemt0103mt0103-the-static-object--contains-more-than-one-assembly--but-each-static-object-must-correspond-with-exactly-one-assembly"></a><a name="MT0103"/>MT0103: Statik nesne '\*' birden fazla derleme içeriyor ('\*'), ancak her statik nesnesi tam olarak bir derleme karşılık gelmelidir.

Hata iletisinde belirtilen derlemeleri tümü tek bir statik nesneye derlenir. Bu izin, her derleme farklı bir statik nesneye derlenmiş olmalıdır.

Örneğin:

    --assembly-build-target:Assembly1.dll=staticobject=MyBinary --assembly-build-target:Assembly2.dll=staticobject=MyBinary

Bu örnek bir statik nesnesi oluşturmaya çalışır (`MyBinary`) iki derlemelerinin oluşur (`Assembly1.dll` ve `Assembly2.dll`), izin verilmeyen.

### <a name="a-namemt0105mt0105-no-assembly-build-target-was-specified-for-"></a><a name="MT0105"/>MT0105: Bir derlemenin derleme hedef için belirtilen ' *'.

Ne zaman derlemeyi belirtmeyi yapı hedef kullanarak `--assembly-build-target`, uygulamadaki her derleme atanmış bir yapı hedefi olması gerekir.

Hata iletisinde belirtilen derleme atanan hedef yapı derleme olmadığında bu hata bildirilir.

Belgelerine bakın `--assembly-build-target` daha fazla bilgi için.

### <a name="a-namemt0106mt0106-the-assembly-build-target-name--is-invalid-the-character--is-not-allowed"></a><a name="MT0106"/>MT0106: Hedef adı derleme '\*' geçersiz: karakteri '\*' verilmez.

Derleme yapı hedef adı geçerli bir dosya adı olmalıdır.

Örneğin bu değerleri bu hataya neden olur:

    --assembly-build-target:Assembly1.dll=staticobject=my/path.o

çünkü `my/path.o` geçerli bir dosya adı dizin ayırıcı karakterde nedeniyle değil.

### <a name="a-namemt0107mt0107-the-assemblies--have-different-custom-llvm-optimizations--which-is-not-allowed-when-they-are-all-compiled-to-a-single-binary"></a><a name="MT0107"/>MT0107: Derlemeleri\*' farklı özel LLVM iyileştirmeler sahip (\*), izin verilmeyen tüm tek bir ikili derlenen olduğunda.

### <a name="a-namemt0108mt0108-the-assembly-build-target--did-not-match-any-assemblies"></a><a name="MT0108"/>MT0108: Hedef derleme ' *' herhangi derlemeler ile eşleşmiyor.

### <a name="a-namemt0109mt0109-the-assembly-0-was-loaded-from-a-different-path-than-the-provided-path-provided-path-1-actual-path-2"></a><a name="MT0109"/>MT0109: Sağlanan yol daha farklı bir yoldan derleme '{0}' yüklendi (yolu sağlanan: {1}, gerçek yol: {2}).

Bu, uygulama tarafından başvurulan bir derleme başka bir konumdan istenenden yüklü olduğunu belirten bir uyarıdır.

Bu uygulama birden çok derleme aynı ada sahip, ancak (yalnızca ilk derleme kullanılacak) beklenmeyen sonuçlara neden olabilir farklı konumlardan başvurulduğu anlamına gelebilir.

### <a name="a-namemt0110mt0110-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-third-party-binding-libraries-and-that-compiles-to-bitcode"></a><a name="MT0110"/>MT0110: Xamarin.iOS bu sürümü, üçüncü taraf bağlama kitaplıkları içeren ve bitcode'u için derler projelerinde artımlı derlemeler desteklemediği için artımlı derlemeler devre dışı bırakıldı.

Xamarin.iOS bu sürümü, üçüncü taraf bağlama kitaplıkları içeren ve bitcode (tvOS ve watchOS projeleri) derler projelerinde artımlı derlemeler desteklemediği için artımlı derlemeler devre dışı bırakıldı.

Herhangi bir işlem yapmanız gereken, bu ileti yalnızca bilgilendirme amaçlıdır.

Daha fazla bilgi için hata #[51710](https://bugzilla.xamarin.com/show_bug.cgi?id=51710).

Bu uyarı artık bildirilmedi.

### <a name="a-namemt0111mt0111-bitcode-has-been-enabled-because-this-version-of-xamarinios-does-not-support-building-watchos-projects-using-llvm-without-enabling-bitcode"></a><a name="MT0111"/>MT0111: Xamarin.iOS bu sürümü yapı watchOS desteklemediğinden Bitcode'u etkinleştirildi bitcode'u etkinleştirmeden LLVM kullanarak projeleri.

Bitcode'u etkinleştirildi otomatik olarak bu sürümü Xamarin.iOS yapı watchOS desteklemediğinden bitcode'u etkinleştirmeden LLVM kullanarak projeleri.

Herhangi bir işlem yapmanız gereken, bu ileti yalnızca bilgilendirme amaçlıdır.

Daha fazla bilgi için hata #[51634](https://bugzilla.xamarin.com/show_bug.cgi?id=51634).

### <a name="a-namemt0112mt0112-native-code-sharing-has-been-disabled-because-"></a><a name="MT0112"/>MT0112: Yerel kod paylaşımı için devre dışı bırakıldı. *

Kod paylaşımını devre dışı bırakılabilir birden çok nedeni vardır:

* kapsayıcı uygulamanın dağıtım hedefi iOS 8. 0 ' eski olduğundan (bunu sahip *)).

Yerel kod paylaşımını kullanıcı çerçeveler kullanılarak uygulanan çünkü yerel kod paylaşımı, iOS 8.0 ile kullanılmaya başlanan iOS 8.0 gerektirir.

* kapsayıcı uygulama I18N derlemeler (*) içerdiğinden.

Kapsayıcı uygulama I18N derlemeler içeriyorsa yerel kod Paylaşımı şu anda desteklenmiyor.

* kapsayıcı uygulama yönetilen bağlayıcı (*) için özel xml tanımları olduğundan.

Yerel kod paylaşımını gerektirir yönetilen bağlayıcı için özel xml tanımlarını kullandığınızda projeleri için desteklenmiyor.


### <a name="a-namemt0113mt0113-native-code-sharing-has-been-disabled-for-the-extension--because-"></a><a name="MT0113"/>MT0113: Yerel kod paylaşımını uzantısı devre dışı bırakıldı. ' *' olduğundan *.

* bitcode'u seçenekleri arasında kapsayıcı uygulama farklı olduğundan (\*) ve uzantısı (\*).

  Yerel kod paylaşımını bitcode'u seçenekleri kod paylaşmak projeler arasında eşleşmesini gerektirir.

* çünkü derleme-derleme-hedef seçenekleri arasında kapsayıcı uygulama farklı (\*) ve uzantısı (\*).

  Yerel kod paylaşımını olmasını gerektirir derleme-derleme-hedef kod paylaşmak projeler arasında seçenekleri aynıdır.

  Artımlı derlemeler bu koşul değil ya da etkin veya devre dışı tüm projelerde ortaya çıkabilir.

* I18N derlemeler arasında kapsayıcı uygulama farklı olduğundan (\*) ve uzantısı (\*).

  Yerel kod paylaşımı için I18N derlemeler dahil uzantıları şu anda desteklenmiyor.

* Uygulama Nesne AĞACI derleyici bağımsız değişkenleri arasında kapsayıcı uygulama farklı olduğu (\*) ve uzantısı (\*).

  Yerel kod paylaşımını Uygulama Nesne AĞACI derleyici değişkenleri kod paylaşmak projeleri arasında farklı değildir gerektirir.

* Uygulama Nesne AĞACI derleyici başka bir bağımsız değişken kapsayıcı uygulama arasında farklı olduğundan (\*) ve uzantısı (\*).

  Yerel kod paylaşımını Uygulama Nesne AĞACI derleyici değişkenleri kod paylaşmak projeleri arasında farklı değildir gerektirir.

  'Tüm 32 bit kayan nokta işlemleri gerçekleştirmek 64-bit float ' projeleri arasında farklılık varsa bu durum oluşur.

* LLVM etkin değil veya her iki kapsayıcı uygulamada devre dışı olduğundan (\*) ve uzantısı (\*).

  Yerel kod paylaşımını LLVM etkin veya kod paylaşmak tüm projeleri için devre dışı olmasını gerektirir.

* Yönetilen bağlayıcı ayarları arasında kapsayıcı uygulama farklı olduğundan (\*) ve uzantısı (\*).

  Yerel kod paylaşımını yönetilen Bağlayıcı ayarlarını kod paylaşan tüm projelerde aynı olmasını gerektirir.

* Atlanan derlemeler yönetilen bağlayıcı için kapsayıcı uygulama arasında farklı olduğundan (\*) ve uzantısı (\*).

  Yerel kod paylaşımını yönetilen Bağlayıcı ayarlarını kod paylaşan tüm projelerde aynı olmasını gerektirir.

* Yönetilen bağlayıcı (*) için özel xml tanımları uzantısına sahip olduğundan.

  Yerel kod paylaşımını gerektirir yönetilen bağlayıcı için özel xml tanımlarını kullandığınızda projeleri için desteklenmiyor.

* kapsayıcı uygulama için ABI oluşmuyor çünkü * (uzantısı için bu ABI oluşturma sırasında).

  Yerel kod paylaşımını kapsayıcı uygulama için uygulama uzantıyı derlemeler mimari derlemeler gerektirir.

  Örneğin: Bu koşul ARM64 + ARMv7 için uzantı oluşturur, ancak kapsayıcı uygulama ARM64 için yalnızca derlemeler oluşur.

* kapsayıcı uygulama için ABI oluşturuyor çünkü \*, uzantının ABI ile uyumlu değil (\*).

  Yerel kod paylaşım için tam aynı API derleme tüm projeleri gerektirir.

  Örneğin: ARMv7, llvm + thumb2 için uzantı oluşturur, ancak kapsayıcı uygulama yalnızca derlemeler ARMv7 + llvm için bu durum oluşur.

* kapsayıcı uygulama derleme başvurduğundan '\*'from'\*', başka bir sürümden uzantısı başvuran ' *'.

  Yerel kod paylaşımını kod paylaşmak tüm projeleri için tüm derlemeleri aynı sürümde kullanmanızı gerektirir.

<!-- MT0114: used by mmp -->

### <a name="a-namemt0115mt0115-it-is-recommended-to-reference-dynamic-symbols-using-code---dynamic-symbol-modecode-when-bitcode-is-enabled"></a><a name="MT0115"/>MT0115: Kod kullanarak dinamik semboller başvurusu için önerilen (--dinamik simgesi modu kodu =) bitcode'u etkin olduğunda.

Xamarin.iOS projeleri genellikle yerel semboller dinamik olarak yerel bağlayıcı görmez bu simgeleri kullanılır çünkü yerel bağlayıcı yerel bağlama işlemi sırasında bu tür yerel semboller kaldırabilir yani başvurur.

Xamarin.iOS gibi semboller tutmak için yerel bağlayıcı genellikle sorar (kullanarak `-u symbol` bağlayıcı bayrağı), ancak ne zaman bitcode'u için yerel bağlayıcı derleme kabul etmiyor `-u` bayrağı.

Xamarin.iOS başka bir çözüm uygulanan: Biz bu simgeleri başvuruda bulunan ek yerel kodu oluşturmak ve bu simgeleri kullanılan yerel bağlayıcı böylece görürsünüz. Bu, bitcode'u için derleme sırasında otomatik olarak gerçekleştirilir.

Varsa `--dynamic-symbol-mode=linker` için mtouch, çözüm devre dışı bırakılır ve Xamarin.iOS geçirmeye çalışır bu alternatif geçirilen `-u` yerel bağlayıcı için. Bu büyük olasılıkla yerel bağlayıcı hatalarına neden.

Çözüm kaldırmaktır `--dynamic-symbol-mode=linker` bağımsız değişkenden projenin Derleme Seçenekleri'nde ek mtouch bağımsız değişkenler.

<!-- 0116 - 0124: free to use -->

### <a name="a-namemt0125mt0125-the---assembly-build-target-command-line-argument-is-ignored-in-the-simulator"></a><a name="MT0125"/>MT0125: Derleme-derleme-hedef komut satırı bağımsız değişkeni benzeticisinde yoksayılır.

Hiçbir eylem gerekli değildir, bu ileti yalnızca bilgilendirme amaçlıdır.

### <a name="a-namemt0126mt0126-incremental-builds-have-been-disabled-because-incremental-builds-are-not-supported-in-the-simulator"></a><a name="MT0126"/>MT0126: artımlı derlemeler benzeticisinde desteklenmediğinden artımlı derlemeler devre dışı bırakıldı.

Hiçbir eylem gerekli değildir, bu ileti yalnızca bilgilendirme amaçlıdır.

### <a name="a-namemt0127mt0127-incremental-builds-have-been-disabled-because-this-version-of-xamarinios-does-not-support-incremental-builds-in-projects-that-include-more-than-one-third-party-binding-libraries"></a><a name="MT0127"/>MT0127: Xamarin.iOS bu sürümü bir üçüncü taraf birden fazla bağlama kitaplıkları dahil projelerinde artımlı derlemeler desteklemediğinden artımlı derlemeler devre dışı bırakıldı.

Xamarin.iOS bu sürümü her zaman birden çok üçüncü taraf bağlama kitaplıkları projeleriyle doğru oluşmuyor çünkü artımlı derlemeler otomatik olarak devre dışı bırakıldı.

Hiçbir eylem gerekli değildir, bu ileti yalnızca bilgilendirme amaçlıdır.

Daha fazla bilgi için hata #[52727](https://bugzilla.xamarin.com/show_bug.cgi?id=52727).

## <a name="mt1xxx-project-related-error-messages"></a>MT1xxx: Proje hata iletileri ilgili.

### <a name="mt10xx-installer--mtouch"></a>MT10xx: Yükleyici / mtouch

<!--
 MT1xxx file copy / symlinks (project related)
  MT10xx installer.cs / mtouch.cs
  -->

### <a name="a-namemt1001mt1001-could-not-find-an-application-at-the-specified-directory"></a><a name="MT1001"/>MT1001 özelliği belirtilen dizine uygulama bulamadı



### <a name="a-namemt1002mt1002-could-not-create-symlinks-files-were-copied"></a><a name="MT1002"/>MT1002 oluşturamadı simgesel bağlantı, dosyalar kopyalandı



### <a name="a-namemt1003mt1003-could-not-kill-the-application--you-may-have-to-kill-the-application-manually"></a><a name="MT1003"/>MT1003 verebilir olmayan uygulama Sonlandır ' *'. Uygulamayı el ile KILL gerekebilir.



### <a name="a-namemt1004mt1004-could-not-get-the-list-of-installed-applications"></a><a name="MT1004"/>MT1004 alınamadı yüklü uygulamaların listesi.



### <a name="a-namemt1005mt1005-could-not-kill-the-application--on-the-device----you-may-have-to-kill-the-application-manually"></a><a name="MT1005"/>MT1005 verebilir olmayan uygulama Sonlandır '\*'aygıtta'\*': *-uygulamayı el ile KILL gerekebilir.



### <a name="a-namemt1006mt1006-could-not-install-the-application--on-the-device--"></a><a name="MT1006"/>MT1006 yükleyemedi uygulama '\*'aygıtta'\*': *.



### <a name="a-namemt1007mt1007-failed-to-launch-the-application--on-the-device---you-can-still-launch-the-application-manually-by-tapping-on-it"></a><a name="MT1007"/>MT1007 uygulama başlatılamadı. '\*'aygıtta'\*': *. Hala uygulamayı el ile üzerindeki e dokunabilirsiniz başlatabilirsiniz.



### <a name="a-namemt1008mt1008-failed-to-launch-the-simulator"></a><a name="MT1008"/>MT1008: simulator başlatılamadı.



Mtouch simulator başlatılamadı, bu hata bildirilir.   Bu durum bazen zaten çalıştıran eski veya ölü simulator işlemi olduğundan oluşabilir.

Verilen UNIX komut satırında aşağıdaki komutu takılan simulator işlemleri sonlandırmak için kullanılabilir:

```bash
$ launchctl list|grep UIKitApplication|awk '{print $3}'|xargs launchctl remove
```

### <a name="a-namemt1009mt1009-could-not-copy-the-assembly--to--"></a><a name="MT1009"/>MT1009 kopyalayamadı derlemesi '\*'to'\*': *

Bu, belirli sürümlerinde Xamarin.iOS, bilinen bir sorundur.

Bu, olanak meydana gelirse, aşağıdaki geçici çözüm deneyin:

```csharp
sudo chmod 0644 /Library/Frameworks/Xamarin.iOS.framework/Versions/Current/lib/mono/*/*.mdb
```

Ancak, bu sorunu en son sürümünü Xamarin.iOS çözümlendi olduğundan, lütfen en yeni bir hatanın dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) tam sürüm bilgilerini ve yapı günlük çıktısı.

### <a name="a-namemt1010mt1010-could-not-load-the-assembly--"></a><a name="MT1010"/>MT1010 yükleyemedi derlemesi ' *': *



### <a name="a-namemt1011mt1011-could-not-add-missing-resource-file-"></a><a name="MT1011"/>MT1011 ekleyemedi eksik kaynak dosyası: ' *'



### <a name="a-namemt1012mt1012-failed-to-list-the-apps-on-the-device--"></a><a name="MT1012"/>Cihazdaki uygulamalar MT1012 listelenemedi ' *': *



### <a name="a-namemt1013mt1013-dependency-tracking-error-no-files-to-compare-please-file-a-bug-report-at-httpbugzillaxamarincom-with-a-test-case"></a><a name="MT1013"/>MT1013 hata izleme bağımlılık: karşılaştırmak için hiçbir dosya. Lütfen bir test çalışması ile http://bugzilla.xamarin.com sırasında bir hata raporu dosya.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) test caes ile.

### <a name="a-namemt1014mt1014-failed-to-re-use-cached-version-of--"></a><a name="MT1014"/>Önbelleğe alınan sürümünü yeniden kullanmayı MT1014 başarısız ' *': *.



### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 yürütülebilir dosya oluşturma başarısız oldu. ' *': *

### <a name="a-namemt1015mt1015--failed-to-create-the-executable--"></a><a name="MT1015"/>MT1015 yürütülebilir dosya oluşturma başarısız oldu. ' *': *

### <a name="a-namemt1016mt1016-failed-to-create-the-notice-file-because-a-directory-already-exists-with-the-same-name"></a><a name="MT1016"/>MT1016: aynı ada sahip bir dizin zaten var olduğundan bildirim dosyası oluşturulamadı.

Dizin kaldırma `NOTICE` projeden.

### <a name="a-namemt1017mt1017-failed-to-create-the-notice-file-"></a><a name="MT1017"/>MT1017: bildirim dosyası oluşturulamadı: *.

### <a name="a-namemt1018mt1018-your-application-failed-code-signing-checks-and-could-not-be-installed-on-the-device--check-your-certificates-provisioning-profiles-and-bundle-ids-probably-your-device-is-not-part-of-the-selected-provisioning-profile-error-0xe8008015"></a><a name="MT1018"/>MT1018: Uygulamanızı kod imzalama denetimleri başarısız oldu ve cihazda yüklenemedi ' *'. Sağlama profilleri, sertifikalarınızı denetleyin ve kimlikler paket. Büyük olasılıkla Cihazınızı seçilen sağlama profilinin bir parçası değil (hata: 0xe8008015).
### <a name="a-namemt1019mt1019-your-application-has-entitlements-not-supported-by-your-current-provisioning-profile-and-could-not-be-installed-on-the-device--please-check-the-ios-device-log-for-more-detailed-information-error-0xe8008016"></a><a name="MT1019"/>MT1019: Uygulamanız, geçerli bir sağlama profili tarafından desteklenmeyen yetkilendirmeleri içeren ve cihazda yüklenemedi ' *'. Daha ayrıntılı bilgi için lütfen iOS aygıtı günlük denetleyin (hata: 0xe8008016).

Bu durum oluşabilir:

* Uygulamanız, geçerli sağlama profili desteklemediği yetkilendirmeler sahiptir.
  Olası çözümler:
  - Yetkilendirmeler destekleyen farklı bir sağlama profili belirtin, uygulama gereksinimlerinizi.
  - Desteklenmeyen geçerli sağlama profilinde yetkilendirmeler kaldırın.
* Dahil değil, kullanmakta olduğunuz sağlama profilinde dağıtmak için çalıştığınız aygıt.
  Olası çözümler:
  - Xcode'da bir şablondan yeni bir uygulama oluşturmak, aynı sağlama profili seçin ve aynı cihaza dağıtabilirsiniz. Bazen Xcode (Xcode yapmanız gerekenler ister diğer durumlarda) yeni cihazları otomatik olarak sağlama profilleri yenileyebilirsiniz.
  -İOS Geliştirici Merkezi'ne gidin ve yeni aygıtla sağlama profili güncelleştirme ardından makinenize güncelleştirilmiş sağlama profili indirin.


Hata hakkında daha fazla bilgi iOS aygıtı günlük basılır çoğu durumda, sorunu tanılamaya yardımcı olabilir.

### <a name="a-namemt1020mt1020-failed-to-launch-the-application--on-the-device--"></a><a name="MT1020"/>MT1020: uygulama başlatılamadı. '\*'aygıtta'\*': *

### <a name="a-namemt1021mt1021-could-not-copy-the-file--to--2"></a><a name="MT1021"/>MT1021: dosyayı kopyalayamadı '\*'to'\*': {2}

Bir dosya kopyalanamadı. Kopyalama işlemi hata iletisinden hata hakkında daha fazla bilgi vardır.

### <a name="a-namemt1022mt1022-could-not-copy-the-directory--to--2"></a><a name="MT1022"/>MT1022: dizini kopyalanamadı '\*'to'\*': {2}

Bir dizin kopyalanamadı. Kopyalama işlemi hata iletisinden hata hakkında daha fazla bilgi vardır.

### <a name="a-namemt1023mt1023-could-not-communicate-with-the-device-to-find-the-application---"></a><a name="MT1023"/>MT1023: uygulamayı bulmak için aygıt ile iletişim kuramadı ' *': *

Bir uygulama cihazda arama çalışılırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma

### <a name="a-namemt1024mt1024-the-application-signature-could-not-be-verified-on-device--please-make-sure-that-the-provisioning-profile-is-installed-and-not-expired-error-0xe8008017"></a><a name="MT1024"/>MT1024: Aygıtta uygulama imzası doğrulanamadı ' *'. Lütfen sağlama profilinin'in yüklü olduğundan ve dolmadığından emin olun (hata: 0xe8008017).

İmza doğrulanamadı çünkü aygıt uygulama yükleme reddetti.

Sağlama profili'nin yüklü olduğundan ve dolmadığından emin olun.

### <a name="a-namemt1025mt1025-could-not-list-the-crash-reports-on-the-device-"></a><a name="MT1025"/>MT1025: cihazda kilitlenme raporlar listelenemedi *.

Cihaz kilitlenme raporları listelemek çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1026mt1026-could-not-download-the-crash-report--from-the-device-"></a><a name="MT1026"/>MT1026: kilitlenme raporu indirilemedi * aygıttan *.

Kilitlenme raporları aygıttan yüklemeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1027mt1027-cant-use-xcode-7-to-launch-applications-on-devices-with-ios--xcode-7-only-supports-ios-6"></a><a name="MT1027"/>MT1027: İOS cihazlarda uygulamaları başlatmak için Xcode 7 + kullanamazsınız * (Xcode 7'in yalnızca desteklediği iOS 6 +).

Xcode 7 + iOS sürüm 6.0 aşağıda ile cihazlarda uygulamaları başlatmak için kullanmak mümkün değil.

Lütfen Xcode daha eski bir sürümünü kullanmanız veya uygulamasını el ile başlatmak için dokunun.

### <a name="a-namemt1028mt1028-invalid-device-specification--expected-ios-watchos-or-all"></a><a name="MT1028"/>MT1028: Geçersiz aygıt belirtimi: ' *'. Beklenen 'ios', 'watchos' veya 'all'.

Cihaz belirtimi geçirilen kullanarak--aygıt geçersiz. Geçerli değerler: 'ios', 'watchos' veya 'all'.

### <a name="a-namemt1029mt1029-could-not-find-an-application-at-the-specified-directory-"></a><a name="MT1029"/>MT1029: Belirtilen dizin bir uygulama bulunamadı: *

--Launchdev için geçirilen uygulama yolu yok. Lütfen geçerli bir uygulama paketi belirtin.

### <a name="a-namemt1030mt1030-launching-applications-on-device-using-a-bundle-identifier-is-deprecated-please-pass-the-full-path-to-the-bundle-to-launch"></a><a name="MT1030"/>MT1030: bir paket tanımlayıcısı kullanılarak cihazda uygulamalar başlatma kullanım dışıdır. Lütfen tam yol başlatmak için paket geçirin.

Bu yol yalnızca paket kimliği yerine aygıtta başlatmak için uygulamasına geçirmek için önerilir.

### <a name="a-namemt1031mt1031-could-not-launch-the-app--on-the-device--because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1031"/>MT1031: uygulama başlatamadı '\*'aygıtta'\*' cihaz kilitli olduğundan. Lütfen aygıtın kilidini açmak ve yeniden deneyin.

Lütfen aygıtın kilidini açmak ve yeniden deneyin.

### <a name="a-namemt1032mt1032-this-application-executable-might-be-too-large--mb-to-execute-on-device-if-bitcode-was-enabled-you-might-want-to-disable-it-for-development-it-is-only-required-to-submit-applications-to-apple"></a><a name="MT1032"/>MT1032: Bu uygulama yürütülebilir dosya çok büyük olabilir (* MB) aygıtta yürütülecek. Bitcode'u etkinleştirilirse geliştirme için devre dışı bırakmak isteyebilirsiniz, uygulamaları Apple göndermek için gerekli olan.

### <a name="a-namemt1033mt1033-could-not-uninstall-the-application--from-the-device--"></a><a name="MT1033"/>MT1033: uygulama kaldırılamadı '\*'aygıttan'\*': *

<!-- 1034 used by mmp -->

### <a name="a-namemt1035mt1035-cannot-include-different-versions-of-the-framework-name"></a><a name="MT1035"/>MT1035: '{name}' framework'ün farklı sürümlerini içeremez

İle aynı framework'ün farklı sürümlerini bağlamak mümkün değildir.

Bu, genellikle bir uzantı framework (büyük olasılıkla aracılığıyla bir üçüncü taraf bağlama derleme) kapsayıcı uygulama değerinden farklı bir sürümünü başvurduğunda gerçekleşir.

Bu hatayı olacaktır birden çok [MT1036](#MT1036) her farklı framework yolları listelenirken hata.

### <a name="a-namemt1036mt1036-framework-name-included-from-path-related-to-previous-error"></a><a name="MT1036"/>MT1036: Framework '{name}' nden: {path} (önceki hata ilgili)

Bu hata yalnızca ile birlikte bildirilen [MT1036](#MT1036). Lütfen bakın [MT1036](#MT1036) daha fazla bilgi için.

### <a name="mt11xx-debug-service"></a>MT11xx: Hizmetinde hata ayıklama

<!--
  MT11xx DebugService.cs
  -->

### <a name="a-namemt1101mt1101-could-not-start-app"></a><a name="MT1101"/>MT1101 başlatılamadı uygulama



### <a name="a-namemt1102mt1102-could-not-attach-to-the-app-to-kill-it-"></a><a name="MT1102"/>MT1102 verebilir değil (Bu sonlandırmak için) uygulamaya ekleme: *



### <a name="a-namemt1103mt1103-could-not-detach"></a><a name="MT1103"/>MT1103 verebilir değil ayırma



### <a name="a-namemt1104mt1104-failed-to-send-packet-"></a><a name="MT1104"/>MT1104 paket gönderme başarısız oldu: *



### <a name="a-namemt1105mt1105-unexpected-response-type"></a><a name="MT1105"/>MT1105 beklenmeyen yanıt türü



### <a name="a-namemt1106mt1106-could-not-get-list-of-applications-on-the-device-request-timed-out"></a><a name="MT1106"/>MT1106 alınamadı uygulamaların listesini cihazda: İstek zaman aşımına uğradı.



### <a name="a-namemt1107mt1107-application-failed-to-launch-"></a><a name="MT1107"/>MT1107: Uygulama başlatılamadı: *

Lütfen aygıtınız kilitliyse denetleyin.

Bir kurumsal uygulamayı dağıtıyorsunuz ya da boş bir sağlama profili kullanarak sahip olabileceğiniz Geliştirici güven (Bu anlatılmıştır <a href="http://stackoverflow.com/a/30726375/183422">burada</a>).

### <a name="a-namemt1108mt1108-could-not-find-developer-tools-for-this-xx-yy-device"></a><a name="MT1108"/>MT1108: geliştirici araçları için bu XX (YY) cihaz bulunamadı.

Mtouch birkaç işlemlerinden gerektiren <tt>DeveloperDiskImage.dmg</tt> bulunması için dosya.   Bu dosya Xcode bir parçasıdır ve buna karşı oluşturmak için kullandığınız SDK göre genellikle bulunur <tt>Xcode.app/Contents/Developer/iPhoneOS.platform/DeviceSupport/VERSION/DeveloperDiskImage.dmg</tt>.

Bağlı aygıt eşleşen bir DeveloperDiskImage.dmg olmadığından ya da bu hata oluşabilir.


### <a name="a-namemt1109mt1109-application-failed-to-launch-because-the-device-is-locked-please-unlock-the-device-and-try-again"></a><a name="MT1109"/>MT1109: Uygulama aygıt kilitlendiğinden başlatılamadı. Lütfen aygıtın kilidini açmak ve yeniden deneyin.

Lütfen aygıtınız kilitliyse denetleyin.

### <a name="a-namemt1110mt1110-application-failed-to-launch-because-of-ios-security-restrictions-please-ensure-the-developer-is-trusted"></a><a name="MT1110"/>MT1110: Uygulama iOS güvenlik kısıtlamaları nedeniyle başlatılamadı. Lütfen Geliştirici güvenilir olduğundan emin olun.

Bir kurumsal uygulamayı dağıtıyorsunuz ya da boş bir sağlama profili kullanarak sahip olabileceğiniz Geliştirici güven (Bu anlatılmıştır <a href="http://stackoverflow.com/a/30726375/183422">burada</a>).

### <a name="mt12xx-simulator"></a>MT12xx: Simulator

<!--
  MT12xx simcontroller.cs
  -->

### <a name="a-namemt1201mt1201-could-not-load-the-simulator-"></a><a name="MT1201"/>MT1201: simulator yüklenemedi: *
### <a name="a-namemt1202mt1202-invalid-simulator-configuration-"></a><a name="MT1202"/>MT1202: Geçersiz simulator yapılandırma: *
### <a name="a-namemt1203mt1203-invalid-simulator-specification-"></a><a name="MT1203"/>MT1203: Geçersiz simulator belirtimi: *
### <a name="a-namemt1204mt1204-invalid-simulator-specification--runtime-not-specified"></a><a name="MT1204"/>MT1204: Geçersiz simulator belirtimi ' *': çalışma zamanı belirtilmedi.
### <a name="a-namemt1205mt1205-invalid-simulator-specification--device-type-not-specified"></a><a name="MT1205"/>MT1205: Geçersiz simulator belirtimi ' *': Aygıt türü belirtilmedi.
### <a name="a-namemt1206mt1206-could-not-find-the-simulator-runtime-"></a><a name="MT1206"/>MT1206: simulator çalışma zamanı bulunamadı. ' *'.
### <a name="a-namemt1207mt1207-could-not-find-the-simulator-device-type-"></a><a name="MT1207"/>MT1207: simulator aygıt türü bulunamadı. ' *'.
### <a name="a-namemt1208mt1208-could-not-find-the-simulator-runtime-"></a><a name="MT1208"/>MT1208: simulator çalışma zamanı bulunamadı. ' *'.
### <a name="a-namemt1209mt1209-could-not-find-the-simulator-device-type-"></a><a name="MT1209"/>MT1209: simulator aygıt türü bulunamadı. ' *'.
### <a name="a-namemt1210mt1210-invalid-simulator-specification--unknown-key-"></a><a name="MT1210"/>MT1210: Geçersiz simulator belirtimi: \*, bilinmeyen bir anahtara '\*'
### <a name="a-namemt1211mt1211-the-simulator-version--does-not-support-the-simulator-type-"></a><a name="MT1211"/>MT1211: Simulator Sürüm '\*'simulator türünü desteklemiyor'\*'
### <a name="a-namemt1212mt1212-failed-to-create-a-simulator-version-where-type---and-runtime--"></a><a name="MT1212"/>MT1212: simulator sürüm oluşturulamadı girildiği = * ve çalışma zamanı = *.
### <a name="a-namemt1213mt1213-invalid-simulator-specification-for-xcode-4-"></a><a name="MT1213"/>MT1213: Xcode 4 geçersiz simulator belirtimi: *
### <a name="a-namemt1214mt1214-invalid-simulator-specification-for-xcode-5-"></a><a name="MT1214"/>MT1214: Geçersiz simulator belirtimi için Xcode 5: *
### <a name="a-namemt1215mt1215-invalid-sdk-specified-"></a><a name="MT1215"/>MT1215: Belirtilen geçersiz SDK: *
### <a name="a-namemt1216mt1216-could-not-find-the-simulator-udid-"></a><a name="MT1216"/>MT1216: UDID simulator bulunamadı. ' *'.
### <a name="a-namemt1217mt1217-could-not-load-the-app-bundle-at-"></a><a name="MT1217"/>MT1217: konumunda uygulama paketi yüklenemedi ' *'.
### <a name="a-namemt1218mt1218-no-bundle-identifier-found-in-the-app-at-"></a><a name="MT1218"/>MT1218: Uygulamaya hiçbir paket tanımlayıcısı bulundu ' *'.
### <a name="a-namemt1219mt1219-could-not-find-the-simulator-for-"></a><a name="MT1219"/>MT1219: simülatörü bulunamadı. ' *'.
### <a name="a-namemt1220mt1220-could-not-find-the-latest-simulator-runtime-for-device-"></a><a name="MT1220"/>MT1220: son simulator çalışma zamanı için cihaz bulunamadı. ' *'.

Bu genellikle Xcode ile ilgili bir sorun gösterir.

Bu sorunu düzeltmeyi denemek için gerekenler:

* Simulator kez Xcode'da kullanın.
* --Sdk kullanarak açık bir SDK sürümü geçirmek <version>.
* Xcode yeniden yükleyin.

### <a name="a-namemt1221mt1221-could-not-find-the-paired-iphone-simulator-for-the-watchos-simulator-"></a><a name="MT1221"/>MT1221: eşleştirilmiş iPhone benzeticisi WatchOS simülatörü bulunamadı. ' *'.

WatchOS simulator WatchOS uygulamada başlatılırken bir eşleştirilmiş iOS simülatörü de olmalıdır.

Benzeticileri iOS Xcode'nın aygıtlar kullanıcı arabirimini kullanarak benzeticileri eşleştirilebilir izleyin (menü penceresi cihazları ->).

### <a name="mt13xx-linkwith"></a>MT13xx: [LinkWith]

<!--
  MT13xx [LinkWith]
  -->


### <a name="a-namemt1301mt1301-native-library---was-ignored-since-it-does-not-match-the-current-build-architectures-"></a><a name="MT1301"/>MT1301 yerel Kitaplığı `*` (\*) geçerli yapı architecture(s) eşleşmediğinden yok sayıldı (\*)



### <a name="a-namemt1302mt1302-could-not-extract-the-native-library--from--please-ensure-the-native-library-was-properly-embedded-in-the-managed-assembly-if-the-assembly-was-built-using-a-binding-project-the-native-library-must-be-included-in-the-project-and-its-build-action-must-be-objcbindingnativelibrary"></a><a name="MT1302"/>MT1302 ayıklayamadı yerel Kitaplığı ' *' öğesinden '+'. Lütfen (Derleme bağlaması proje kullanılarak oluşturuldu, yerel kitaplığı projeye dahil gerekir, ve yapı eylemi 'ObjcBindingNativeLibrary' olması gerekiyorsa) yerel kitaplığı düzgün yönetilen derlemede katıştırılan emin olun.

### <a name="a-namemt1303mt1303-could-not-decompress-the-native-framework--from--please-review-the-build-log-for-more-information-from-the-native-zip-command"></a><a name="MT1303"/>MT1303: yerel framework açılamıyor '\*'from'\*'. Lütfen yerel 'zip' komutundan daha fazla bilgi için yapı günlüğünü gözden geçirin.

Belirtilen yerel çerçeve bağlama kitaplığından açılamıyor.

Lütfen yerel 'zip' komutundan bu hatayla ilgili daha fazla bilgi için bulid günlüğünü gözden geçirin.

### <a name="a-namemt1304mt1304-the-embedded-framework--in--is-invalid-it-does-not-contain-an-infoplist"></a><a name="MT1304"/>MT1304: Katıştırılmış framework ' *', * geçersiz: bir Info.plist içermiyor.

Belirtilen katıştırılmış çerçeve bir Info.plist içermiyor ve bu nedenle geçerli bir çerçeve değil.

Lütfen framework geçerli olduğundan emin olun.

### <a name="a-namemt1305mt1305-the-binding-library--contains-a-user-framework--but-embedded-user-frameworks-require-ios-80-the-current-deployment-target-is--please-set-the-deployment-target-in-the-infoplist-file-to-at-least-80"></a><a name="MT1305"/>MT1305: Bağlama Kitaplığı '\*' kullanıcı framework içerir (\*), ancak katıştırılmış kullanıcı çerçeveler iOS 8.0 gerektirir (geçerli dağıtım hedefi *). Lütfen en az 8.0 Info.plist dosyasında dağıtım hedef ayarlayın.

Belirtilen bağlama kitaplığı katıştırılmış bir çerçeve içeriyor ancak Xamarin.iOS, yalnızca katıştırılmış çerçeveler iOS 8.0 veya üstünü destekler.

Lütfen bu hatayı gidermek için en az 8.0 Info.plist dosyasında dağıtım hedef ayarlayın (veya katıştırılmış çerçeveleri kullanmayın).

### <a name="mt14xx-crash-reports"></a>MT14xx: Kilitlenme raporları

<!--
  MT14xx    CrashReports.cs
  -->

### <a name="a-namemt1400mt1400-could-not-open-crash-report-service-afcconnectionopen-returned-"></a><a name="MT1400"/>MT1400: kilitlenme raporu Hizmet açılamadı: döndürülen AFCConnectionOpen *

Kilitlenme raporları aygıttan erişmeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1401mt1401-could-not-close-crash-report-service-afcconnectionclose-returned-"></a><a name="MT1401"/>MT1401: kilitlenme raporu hizmet kapatılamadı: döndürülen AFCConnectionClose *

Kilitlenme raporları aygıttan erişmeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1402mt1402-could-not-read-file-info-for--afcfileinfoopen-returned-"></a><a name="MT1402"/>MT1402: dosya bilgisi okunamadı *: döndürülen AFCFileInfoOpen *

Kilitlenme raporları aygıttan erişmeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1403mt1403-could-not-read-crash-report-afcdirectoryopen--returned-"></a><a name="MT1403"/>MT1403: kilitlenme raporu okunamadı: AFCDirectoryOpen (*) döndürdü: *

Kilitlenme raporları aygıttan erişmeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1404mt1404-could-not-read-crash-report-afcfilerefopen--returned-"></a><a name="MT1404"/>MT1404: kilitlenme raporu okunamadı: AFCFileRefOpen (*) döndürdü: *

Kilitlenme raporları aygıttan erişmeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1405mt1405-could-not-read-crash-report-afcfilerefread--returned-"></a><a name="MT1405"/>MT1405: kilitlenme raporu okunamadı: AFCFileRefRead (*) döndürdü: *

Kilitlenme raporları aygıttan erişmeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

### <a name="a-namemt1406mt1406-could-not-list-crash-reports-afcdirectoryopen--returned-"></a><a name="MT1406"/>MT1406: kilitlenme raporlar listelenemedi: AFCDirectoryOpen (*) döndürdü: *

Kilitlenme raporları aygıttan erişmeye çalışırken bir hata oluştu.

Bunu çözmek denemek için gerekenler:

* Uygulama aygıttan silin ve yeniden deneyin.
* Cihaz çıkarın ve yeniden bağlayın.
* Cihaz yeniden başlatın.
* Mac yeniden başlatma
* Aygıt (Bu herhangi bir kilitlenme raporu CİHAZDAN kaldırır) iTunes eşitleyin.

<!--- 1407 used by mmp -->

### <a name="mt16xx-macho"></a>MT16xx: MachO

<!--
  MT16xx    MachO.cs
  -->

### <a name="a-namemt1600mt1600-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1600"/>MT1600: Olmayan bir makine-O dinamik kitaplığı (Bilinmeyen üstbilgisi '0 x *'): *.

Söz konusu dinamik kitaplığını işlenirken bir hata oluştu.

Lütfen geçerli bir makine-O dinamik kitaplığı dinamik kitaplığı olduğundan emin olun.

Bir kitaplık biçimi kullanılarak doğrulanabilir `file` bir terminal komutunu:

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1601mt1601-not-a-static-library-unknown-header--"></a><a name="MT1601"/>MT1601: Olmayan bir statik kitaplık (Bilinmeyen üstbilgisi ' *'): *.

Söz konusu statik kitaplık işlenirken bir hata oluştu.

Lütfen geçerli bir makine-O statik kitaplık statik kitaplık olduğundan emin olun.

Bir kitaplık biçimi kullanılarak doğrulanabilir `file` bir terminal komutunu:

    file -arch all -l /path/to/library.a

### <a name="a-namemt1602mt1602-not-a-mach-o-dynamic-library-unknown-header-0x-"></a><a name="MT1602"/>MT1602: Olmayan bir makine-O dinamik kitaplığı (Bilinmeyen üstbilgisi '0 x *'): *.

Söz konusu dinamik kitaplığını işlenirken bir hata oluştu.

Lütfen geçerli bir makine-O dinamik kitaplığı dinamik kitaplığı olduğundan emin olun.

Bir kitaplık biçimi kullanılarak doğrulanabilir `file` bir terminal komutunu:

    file -arch all -l /path/to/library.dylib

### <a name="a-namemt1603mt1603-unknown-format-for-fat-entry-at-position--in-"></a><a name="MT1603"/>MT1603: Konumundaki fat girişi için bilinmeyen biçimi *, *.

Söz konusu fat arşiv işlenirken bir hata oluştu.

Fat arşiv geçerli olduğundan emin olun.

Fat arşiv biçimi kullanılarak doğrulanabilir `file` bir terminal komutunu:

    file -arch all -l /path/to/file

### <a name="a-namemt1604mt1604-file-of-type--is-not-a-macho-file-"></a><a name="MT1604"/>MT1604: Dosya türü * MachO dosyası (*) değil.

Söz konusu MachO dosyası işlenirken bir hata oluştu.

Lütfen geçerli bir makine-O dinamik kitaplık dosyası olduğundan emin olun.

Bir dosya biçimi kullanılarak doğrulanabilir `file` bir terminal komutunu:

    file -arch all -l /path/to/file

## <a name="mt2xxx-linker-error-messages"></a>MT2xxx: Bağlayıcı hata iletileri

<!--
 MT2xxx Linker
  MT20xx Linker (general) errors
  -->

### <a name="a-namemt2001mt2001-could-not-link-assemblies"></a><a name="MT2001"/>MT2001 verebilir değil bağlantı derlemeler

Bu hata yönetilen bağlayıcı örneğin bir özel durum, beklenmeyen bir hatayla karşılaştığı anlamına gelir ve oluşturulamadı tamamlamak veya işlenmekte olan derlemeyi kaydedin. Tam hata hakkında daha fazla bilgi örn: derleme günlüğü parçası olacak

``` 
error MT2001: Could not link assemblies.
    Method: `System.Void Todo.TodoListPageCS/<<-ctor>b__1_0>d::SetStateMachine(System.Runtime.CompilerServices.IAsyncStateMachine)`
    Assembly: `QuickTodo, Version=1.0.6297.28241, Culture=neutral, PublicKeyToken=null`
Reason: Value cannot be null.
Parameter name: instruction
```

Bu tür sorunlar için bir hata raporu dosya önemlidir. Çoğu durumda, uygun düzeltme yayımlanana kadar geçici bir çözüm sağlanabilir. Yukarıdaki bilgilerin sorunu çözmek için (bir test çalışması ve/veya derleme binairy birlikte) önemlidir.


### <a name="a-namemt2002mt2002-can-not-resolve-reference-"></a><a name="MT2002"/>MT2002 değil çözümleyebildiğinden başvuru: *



### <a name="a-namemt2003mt2003-option--will-be-ignored-since-linking-is-disabled"></a><a name="MT2003"/>MT2003 seçeneği ' *' bağlama devre dışı olduğundan göz ardı edilir



### <a name="a-namemt2004mt2004-extra-linker-definitions-file--could-not-be-located"></a><a name="MT2004"/>MT2004 ek bağlayıcı tanımları dosya ' *' bulunamadı.



### <a name="a-namemt2005mt2005-definitions-from--could-not-be-parsed"></a><a name="MT2005"/>MT2005 tanımları ' *' ayrıştırılamadı.



### <a name="a-namemt2006mt2006-can-not-load-mscorlibdll-from--please-reinstall-xamarinios"></a><a name="MT2006"/>MT2006: gelen mscorlib.dll yüklenemiyor: *. Please reinstall Xamarin.iOS.

Bu, genellikle Xamarin.iOS yükleme ile ilgili bir sorun olduğunu gösterir. Xamarin.iOS yeniden yüklemeyi deneyin.

<!--- 2007 used by mmp -->
<!--- 2009 used by mmp -->

### <a name="a-namemt2010mt2010-unknown-httpmessagehandler--valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a><a name="MT2010"/>MT2010: Bilinmeyen HttpMessageHandler `*`. HttpClientHandler (varsayılan), CFNetworkHandler veya NSUrlSessionHandler geçerli değerler:

### <a name="a-namemt2011mt2011-unknown-tlsprovider---valid-values-are-default-or-appletls"></a><a name="MT2011"/>MT2011: Bilinmeyen TlsProvider `*`.  Değerler varsayılan veya appletls geçerlidir.

Verilen değer `tls-provider=` geçerli bir TLS (Aktarım Katmanı Güvenliği) sağlayıcı.

`default` Ve `appletls` olan geçerli değerler yalnızca ve her ikisi de temsil eden SSL/TLS desteği yerel Apple TLS API kullanarak sağlamaktır aynı seçeneği.

<!--- 2012 used by mmp -->
<!--- 2013 used by mmp -->
<!--- 2014 used by mmp -->

### <a name="a-namemt2015mt2015-invalid-httpmessagehandler--for-watchos-the-only-valid-value-is-nsurlsessionhandler"></a><a name="MT2015"/>MT2015: Geçersiz HttpMessageHandler `*` watchOS için. NSUrlSessionHandler yalnızca geçerli değeridir.

Bu proje dosyası geçersiz HttpMessageHandler belirttiğinden oluşan bir uyarıdır.

Önizleme araçlarımız önceki sürümlerinde varsayılan proje dosyasında geçersiz bir değer oluşturulur.

Bu uyarıyı çözmenin proje dosyasını bir metin düzenleyicisinde açın ve tüm HttpMessageHandler düğümleri XML'den kaldırın.

### <a name="a-namemt2016mt2016-invalid-tlsprovider-legacy-option-the-only-valid-value-appletls-will-be-used"></a><a name="MT2016"/>MT2016: Geçersiz TlsProvider `legacy` seçeneği. Tek geçerli değer `appletls` kullanılır.

`legacy` Tam olarak yönetilen SSLv3 olan sağlayıcıyı / TLSv1 tek sağlayıcısı sevk Xamarin.iOS ile artık. Bu eski sağlayıcı kullanmakta olduğunuz ve şimdi daha yeni yapı projeleri `appletls` biri.

Bu uyarı düzeltmek için proje dosyasını bir metin düzenleyicisinde açın ve Tümünü Kaldır ' MtouchTlsProvider'' XML düğümü.

### <a name="a-namemt2017mt2017-could-not-process-xml-description"></a><a name="MT2017"/>MT2017: XML açıklama işleyemedi.

Bu bir hata yok anlamına gelir [özel XML bağlayıcı yapılandırma dosyası](https://developer.xamarin.com/guides/cross-platform/advanced/custom_linking/) sağladığınız, lütfen dosyanızı gözden geçirin.

### <a name="a-namemt2018mt2018-the-assembly--is-referenced-from-two-different-locations--and-"></a><a name="MT2018"/>MT2018: Derleme '\*' iki farklı konumlardan başvuruluyor: '\*' ve ' *'.

Hata iletisinde belirtilen derleme birden çok konumdan yüklenir. Her zaman bir derlemenin aynı sürümü kullandığınızdan emin olun.

### <a name="a-namemt2019mt2019-can-not-load-the-root-assembly-"></a><a name="MT2019"/>MT2019: kök derlemesi yüklenemiyor ' *'

Kök derlemesi yüklenemedi. Lütfen hata iletisi yolunda varolan bir dosyaya başvuruyor ve geçerli bir .NET derlemesi olduğundan emin olun.

### <a name="a-namemt202xmt202x-binding-optimizer-failed-processing-"></a><a name="MT202x"/>MT202x: Bağlama iyileştirici işlenemedi `...`.

Beklenmeyen bir şey iyileştirmek kod bağlama oluşturulan oluştu. Hata iletisinde soruna neden öğesi olarak adlandırılır. Adlı derleme (veya türü veya adlı yöntemi içeren) Bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](http://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenler**).

Son rakam `x` olacaktır:
* `0` derleme adı için;
* `1` tür adı için;
* `3` Yöntem adı için;

### <a name="a-namemt2030mt2030-remove-user-resources-failed-processing-"></a><a name="MT2030"/>MT2030: Kaldır kullanıcı kaynakları işlenemedi `...`.

Beklenmeyen bir şey kullanıcı kaynaklarını kaldırmak oluştu. Soruna neden bütünleştirilmiş hata iletisinde adlandırılmıştır. Derleme bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](http://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

Kullanıcı, uygulama paketini oluşturmak için derleme zamanında, ayıklanması gereken bütünleştirilmiş kodları (kaynakları) içinde bulunan dosyalar kaynaklardır. Şunları içerir:

* `__monotouch_content_*` ve `__monotouch_pages_*` kaynakları ve
* Yerel kitaplıkları bağlama derleme içine gömülü;

### <a name="a-namemt2040mt2040-default-httpmessagehandler-setter-failed-processing-"></a><a name="MT2040"/>MT2040: işlem başarısız oldu varsayılan HttpMessageHandler ayarlayıcı `...`.

Beklenmeyen bir şey varsayılan ayarlanmaya çalışılırken hata oluştu `HttpMessageHandler` uygulama için. Lütfen dosya bir [hata raporu](http://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

### <a name="a-namemt2050mt2050-code-remover-failed-processing-"></a><a name="MT2050"/>MT2050: Kod kaldırıcısını işlenemedi `...`.

Beklenmeyen bir şey uygulama ile sevkiyat BCL kodu kaldırmak oluştu. Lütfen dosya bir [hata raporu](http://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

### <a name="a-namemt2060mt2060-sealer-failed-processing-"></a><a name="MT2060"/>MT2060: Sealer işlenemedi `...`.

Beklenmeyen bir şey türleri veya yöntemleri (son) korumaya çalışırken veya bazı yöntemler devirtualizing oluştu. Soruna neden bütünleştirilmiş hata iletisinde adlandırılmıştır. Derleme bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](http://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

### <a name="a-namemt2070mt2070-metadata-reducer-failed-processing-"></a><a name="MT2070"/>MT2070: Meta veri Reducer işlenemedi `...`.

Beklenmeyen bir şey uygulamadan meta verileri azaltmak oluştu. Soruna neden bütünleştirilmiş hata iletisinde adlandırılmıştır. Derleme bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](http://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

### <a name="a-namemt2080mt2080-marknsobjects-failed-processing-"></a><a name="MT2080"/>MT2080: MarkNSObjects işlenemedi `...`.

Beklenmeyen bir şey işaretlemek oluştu `NSObject` alt sınıfların uygulamadan. Soruna neden bütünleştirilmiş hata iletisinde adlandırılmıştır. Derleme bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](http://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

<!-- MT21xx: more linker errors -->

<!--- 2100 used by mmp -->

### <a name="a-namemt2101mt2101-cant-resolve-the-reference--referenced-from-the-method--in-"></a><a name="MT2101"/>MT2101: başvurusu çözümlenemiyor '\*', başvurulan yönteminden'\*', ' *'.

Geçersiz derleme başvurusu hata iletisinde açıklanan yöntemi işlenirken karşılaşıldı.

Soruna neden bütünleştirilmiş hata iletisinde adlandırılmıştır. Derleme bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](https://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

### <a name="a-namemt2102mt2102-error-processing-the-method--in-the-assembly--"></a><a name="MT2102"/>MT2102: yöntemi işlenirken hata '\*'derlemesindeki'\*': *

Beklenmeyen bir hata iletisinde belirtilen yöntemini işaretlemek oluştu.

Soruna neden bütünleştirilmiş hata iletisinde adlandırılmıştır. Derleme bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](https://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

## <a name="mt3xxx-aot-error-messages"></a>MT3xxx: Uygulama Nesne AĞACI hata iletileri

<!--
 MT3xxx AOT
  MT30xx AOT (general) errors
  -->

### <a name="a-namemt3001mt3001-could-not-aot-the-assembly-"></a><a name="MT3001"/>MT3001 verebilir AOT derlemesi ' *'

Bu genellikle Uygulama Nesne AĞACI derleyici bir hata gösterir. Lütfen dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS) hatayı oluşturmak için kullanılan bir proje ile.

Bazen artımlı derlemelerde projenin iOS derleme seçeneği devre dışı bırakarak bu sorunu çözmek mümkündür (ancak hala bir hata, bu nedenle Lütfen yine de bildirin).

### <a name="a-namemt3002mt3002-aot-restriction-method--must-be-static-since-it-is-decorated-with-monopinvokecallback-see-httpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbackshttpsdeveloperxamarincomguidesiosadvancedtopicslimitationsreversecallbacks"></a><a name="MT3002"/>MT3002 Uygulama Nesne AĞACI kısıtlama: yöntemi ' *' [MonoPInvokeCallback] ile donatılmış beri statik olmalıdır. Bkz: [https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks](https://developer.xamarin.com/guides/ios/advanced_topics/limitations/#Reverse_Callbacks)

Bu hata iletisini Uygulama Nesne AĞACI derleyiciden gelir.



### <a name="a-namemt3003mt3003-conflicting---debug-and---llvm-options-soft-debugging-is-disabled"></a><a name="MT3003"/>MT3003 Çakışan--hata ayıklama ve--llvm seçenekleri. Geçici hata ayıklama devre dışı bırakılır.

LLVM etkinleştirildiğinde, hata ayıklama desteklenmiyor. Uygulama hata ayıklama gerekiyorsa, ilk LLVM devre dışı bırakın.

### <a name="a-namemt3004mt3004-could-not-aot-the-assembly--because-it-doesnt-exist"></a><a name="MT3004"/>MT3004 verebilir AOT derlemesi ' *' var olmadığı için.



### <a name="a-namemt3005mt3005-the-dependency--of-the-assembly--was-not-found-please-review-the-projects-references"></a><a name="MT3005"/>MT3005 Bağımlılık '\*'derlemenin'\*' bulunamadı. Lütfen projenin başvurular gözden geçirin.

Bu genellikle, bir derlemeyi platform derleme (genellikle .NET 4 sürümü mscorlib.dll) başka bir sürümüne başvuruyor. oluşur.

Bu desteklenmez ve düzgün derleme veya düzgün yürütme (derleme Xamarin.iOS sürümüne sahip değil mscorlib.dll .NET 4 sürümünden API kullanabilirsiniz).

### <a name="a-namemt3006mt3006-could-not-compute-a-complete-dependency-map-for-the-project-this-will-result-in-slower-build-times-because-xamarinios-cant-properly-detect-what-needs-to-be-rebuilt-and-what-does-not-need-to-be-rebuilt-please-review-previous-warnings-for-more-details"></a><a name="MT3006"/>MT3006 verebilir projesi için tam bağımlılık Haritası değil işlem. Xamarin.iOS düzgün bir şekilde yeniden oluşturulması gerekiyor (ve ne yeniden gerekmez) algılayamaz bu yavaş derleme sürelerini sonuçlanır. Lütfen daha fazla ayrıntı için önceki uyarılarını gözden geçirin.

 derleme veya düzgün yürütme (derleme Xamarin.iOS sürümüne sahip değil mscorlib.dll .NET 4 sürümünden API kullanabilirsiniz).

### <a name="a-namemt3007mt3007-debug-info-files-mdb-will-not-be-loaded-when-llvm-is-enabled"></a><a name="MT3007"/>MT3007: llvm etkinleştirildiğinde, hata ayıklama bilgisi dosyaları (*.mdb) yüklenmeyecek.

### <a name="a-namemt3008mt3008-bitcode-support-requires-the-use-of-the-llvm-aot-backend---llvm"></a><a name="MT3008"/>MT3008: Bitcode'u destek LLVM Uygulama Nesne AĞACI arka uç kullanılmasını gerektirir (--llvm)

Bitcode'u destek LLVM Uygulama Nesne AĞACI arka uç kullanılmasını gerektiren (--llvm).

Bitcode'u desteğini devre dışı bırakın ya da LLVM etkinleştirin.

<!--- 3009 used by mmp -->
<!--- 3010 used by mmp -->

## <a name="mt4xxx-code-generation-error-messages"></a>MT4xxx: Kod oluşturma hata iletileri

### <a name="mt40xx-main"></a>MT40xx: Main

<!--
 MT4xxx code generation
  MT40xx main.m
  -->

### <a name="a-namemt4001mt4001-the-main-template-could-not-be-expanded-to-"></a><a name="MT4001"/>MT4001 Ana şablon için genişletilemedi `*`.

Main.m üretilirken bir hata oluştu. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4002mt4002-failed-to-compile-the-generated-code-for-pinvoke-methods-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4002"/>MT4002 P/Invoke yöntemleri için oluşturulan kod derlenemedi. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya

P/Invoke yöntemleri için oluşturulan kodu derlemek başarısız oldu. Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt41xx-registrar"></a>MT41xx: Kaydedici

<!--
  MT41xx registrar.m
  -->

### <a name="a-namemt4101mt4101-the-registrar-cannot-build-a-signature-for-type-"></a><a name="MT4101"/>MT4101 Kayıt türü için bir imza oluşturamaz `*`.

Çalışma zamanı nasıl hazırlanacağını Objective-c denetleyicisinden bilmiyor dışarı aktarılan API türü bulundu

Xamarin.iOS türü söz konusu destek düşünüyorsanız, Lütfen bir geliştirme isteğiyle dosya [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4102mt4102-the-registrar-found-an-invalid-type--in-signature-for-method--use--instead"></a><a name="MT4102"/>MT4102 Kayıt bulundu geçersiz bir tür `*` yöntemi için imzada `*`. Bunun yerine `*` kullanın.

Bu şu anda yalnızca bir türüyle gerçekleşir: System.DateTime. Objective-C eşdeğer (NSDate) kullanın.

### <a name="a-namemt4103mt4103-the-registrar-found-an-invalid-type--in-signature-for-method--the-type-implements-inativeobject-but-does-not-have-a-constructor-that-takes-two-intptr-bool-arguments"></a><a name="MT4103"/>MT4103 Kayıt bulundu geçersiz bir tür `*` yöntemi için imzada `*`: türü INativeObject uygular, ancak iki alan bir oluşturucu yok (IntPtr, bool) bağımsız değişkenleri

Bu meydana zaman kayıt şirketi karşılaştığınız sözü edilen özelliklere sahip bir imzada türü. Kayıt türünün yeni örnekleri oluşturmak gerekebilir ve bu durumda (IntPtr, bool) bir oluşturucu gerektirir imza - ilk bağımsız değişken (IntPtr) belirtir yönetilen işleyici çağıran yerel sahipliğini üzerinize aktarır ise ikinci oluştu (Bu değer 'Beklet' nesnesinde çağrılacağı false ise) işler.

### <a name="a-namemt4104mt4104-the-registrar-cannot-marshal-the-return-value-for-type--in-signature-for-method-"></a><a name="MT4104"/>MT4104 Kayıt türü için dönüş değerini sıralanamıyor `*` yöntemi için imzada `*`.

Çalışma zamanı nasıl hazırlanacağını Objective-c denetleyicisinden bilmiyor dışarı aktarılan API türü bulundu

Xamarin.iOS türü söz konusu destek düşünüyorsanız, Lütfen bir geliştirme isteğiyle dosya [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4105mt4105-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4105"/>MT4105 Türünde parametresi kayıt şirketi sıralanamıyor `*` yöntemi için imzada `*`.

Xamarin.iOS türü söz konusu destek düşünüyorsanız, Lütfen bir geliştirme isteğiyle dosya [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4106mt4106-the-registrar-cannot-marshal-the-return-value-for-structure--in-signature-for-method-"></a><a name="MT4106"/>MT4106 Yapısı için dönüş değeri kayıt şirketi sıralanamıyor `*` yöntemi için imzada `*`.

Çalışma zamanı nasıl hazırlanacağını Objective-c denetleyicisinden bilmiyor dışarı aktarılan API türü bulundu

Xamarin.iOS türü söz konusu destek düşünüyorsanız, Lütfen bir geliştirme isteğiyle dosya [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4107mt4107-the-registrar-cannot-marshal-the-parameter-of-type--in-signature-for-method-"></a><a name="MT4107"/>MT4107 Türünde parametresi kayıt şirketi sıralanamıyor `*` yöntemi için imzada `+`.

Çalışma zamanı nasıl hazırlanacağını Objective-c denetleyicisinden bilmiyor dışarı aktarılan API türü bulundu

Xamarin.iOS türü söz konusu destek düşünüyorsanız, Lütfen bir geliştirme isteğiyle dosya [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4108mt4108-the-registrar-cannot-get-the-objectivec-type-for-managed-type-"></a><a name="MT4108"/>MT4108 Kayıt şirketi yönetilen türü için ObjectiveC türü alınamıyor `*`.

Çalışma zamanı nasıl hazırlanacağını Objective-c denetleyicisinden bilmiyor dışarı aktarılan API türü bulundu

Xamarin.iOS türü söz konusu destek düşünüyorsanız, Lütfen bir geliştirme isteğiyle dosya [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com).

### <a name="a-namemt4109mt4109-failed-to-compile-the-generated-registrar-code-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4109"/>MT4109 oluşturulan kayıt kod derlenemedi. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya

Kayıt için oluşturulan kodu derlenemedi. Derleme günlüğünde neden kodu derleme değil açıklayan yerel Derleyici çıktısı içerir.

Bu her zaman Xamarin.iOS bir hata olduğunu; Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](http://bugzilla.xamarin.com) projenizi ya da bir test çalışması ile.

### <a name="a-namemt4110mt4110-the-registrar-cannot-marshal-the-out-parameter-of-type--in-signature-for-method-"></a><a name="MT4110"/>MT4110 Kayıt türünün out parametresi sıralanamıyor `*` yöntemi için imzada `*`.



### <a name="a-namemt4111mt4111-the-registrar-cannot-build-a-signature-for-type--in-method-"></a><a name="MT4111"/>MT4111 Kayıt türü için bir imza oluşturamaz `*` yönteminde `*`.



### <a name="a-namemt4112mt4112-the-registrar-found-an-invalid-type--registering-generic-types-with-objective-c-is-not-supported-and-may-lead-to-random-behavior-andor-crashes-for-backwards-compatibility-with-older-versions-of-xamarinios-it-is-possible-to-ignore-this-error-by-passing---unsupported--enable-generics-in-registrar-as-an-additional-mtouch-argument-in-the-projects-ios-build-options-page-see-developerxamarincomguidesiosadvancedtopicsregistrarhttpsdeveloperxamarincomguidesiosadvancedtopicsregistrar-for-more-information"></a><a name="MT4112"/>MT4112 Kayıt bulundu geçersiz bir tür `*`. Genel türler Objective-C ile kaydetme desteklenmiyor ve rastgele davranışı ve/veya bir kilitlenme neden olabilir (için geriye dönük Xamarin.iOS eski sürümleriyle uyumluluk geçirerek bu hatayı yok saymayı mümkündür `--unsupported--enable-generics-in-registrar` ek mtouch olarak Projenin iOS derleme seçenekleri sayfasındaki bağımsız değişken. Bkz: [developer.xamarin.com/guides/ios/advanced_topics/registrar](https://developer.xamarin.com/guides/ios/advanced_topics/registrar) daha fazla bilgi için).



### <a name="a-namemt4113mt4113-the-registrar-found-a-generic-method--exporting-generic-methods-is-not-supported-and-will-lead-to-random-behavior-andor-crashes"></a><a name="MT4113"/>MT4113 Kayıt şirketi genel yöntem bulunamadı: '\*.\*'. Genel yöntemler verme desteklenmiyor ve rastgele davranışı ve/veya bir kilitlenme götürür.



### <a name="a-namemt4114mt4114-unexpected-error-in-the-registrar-for-the-method----please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4114"/>Yöntem için Kaydedici MT4114 beklenmeyen hata oluştu '\*.\*'-Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya



### <a name="a-namemt4116mt4116-could-not-register-the-assembly--"></a><a name="MT4116"/>MT4116 kaydedemedi derlemesi ' *': *



### <a name="a-namemt4117mt4117-the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4117"/>MT4117 Kayıt şirketi bulunan bir imza uyuşmazlığı yönteminde '*.*'-yöntem alır seçiciyi gösterir * Yönetilen yöntemi sahipken parametreleri * parametreleri.



### <a name="a-namemt4118mt4118-cannot-register-two-managed-types--and--with-the-same-native-name-"></a><a name="MT4118"/>MT4118 kaydedilemiyor iki yönetilen türler ('\*'ve'\*') yerel aynı adla ('* ').



### <a name="a-namemt4119mt4119-could-not-register-the-selector--of-the-member--because-the-selector-is-already-registered-on-a-different-member"></a><a name="MT4119"/>MT4119 kaydedemedi Seçici '\*'üyesinin'\*. *' seçiciyi üzerinde farklı bir üye zaten kaydedildiği için.



### <a name="a-namemt4120mt4120-the-registrar-found-an-unknown-field-type--in-field--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4120"/>MT4120 Kayıt şirketi bulunan bir bilinmeyen alan türü '\*'alanındaki'\*. *'. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya

Bu hata, bir Xamarin.iOS hata gösterir. Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4121mt4121-cannot-use-gccg-to-compile-the-generated-code-from-the-static-registrar-when-using-the-accounts-framework-the-header-files-provided-by-apple-used-during-the-compilation-require-clang-either-use-clang---compilerclang-or-the-dynamic-registrar---registrardynamic"></a><a name="MT4121"/>MT4121 kullanamaz GCC / G ++ (derleme sırasında kullanılan Apple tarafından sağlanan üstbilgi dosyaları gerektir Clang) hesapları framework kullanılırken statik kayıt oluşturulan kodu derlemek için. Clang kullanın ya da (--derleyici: clang) veya dinamik kayıt (--Kaydedici: dinamik).



### <a name="a-namemt4122mt4122-cannot-use-the-clang-compiler-provided-in-the--sdk-to-compile-the-generated-code-from-the-static-registrar-when-non-ascii-type-names--are-present-in-the-application-either-use-gccg---compilergccg-the-dynamic-registrar---registrardynamic-or-a-newer-sdk"></a><a name="MT4122"/>MT4122 kullanamaz sağlanan Clang derleyici *.* Statik kayıt şirketi ASCII olmayan olduğunda oluşturulan kodu derlemek için SDK adlarını yazın ('* ') uygulama yok. GCC kullanın ya da / G ++ (--derleyici: gcc | g ++), dinamik kayıt (--Kaydedici: dinamik) veya daha yeni bir SDK.



### <a name="a-namemt4123mt4123-the-type-of-the-variadic-parameter-in-the-variadic-function--must-be-systemintptr"></a><a name="MT4123"/>MT4123 Variadic işlevinde variadic parametresinin türü ' *' System.IntPtr olması gerekir.



### <a name="a-namemt4124mt4124-invalid--found-on--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4124"/>MT4124 geçersiz * bulunan ' *'. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya

Bu hata, bir Xamarin.iOS hata gösterir. Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4125mt4125-the-registrar-found-an-invalid-type--in-signature-for-method--the-interface-must-have-a-protocol-attribute-specifying-its-wrapper-type"></a><a name="MT4125"/>MT4125 Kayıt bulundu geçersiz bir tür '\*'ın için imzası'\*': arabirim sarmalayıcı türünü belirten bir iletişim kuralı özniteliğe sahip olması gerekir.



### <a name="a-namemt4126mt4126-cannot-register-two-managed-protocols--and--with-the-same-native-name-"></a><a name="MT4126"/>MT4126 kaydedilemiyor iki yönetilen iletişim kuralı ('\*'ve'\*') yerel aynı adla ('* ').



### <a name="a-namemt4127mt4127-cannot-register-more-than-one-interface-method-for-the-method--which-is-implementing-"></a><a name="MT4127"/>MT4127 kaydedilemiyor yöntemi için birden fazla arabirim yöntemi '\*' (uygulama '\*').



### <a name="a-namemt4128mt4128--the-registrar-found-an-invalid-generic-parameter-type--in-the-method--the-generic-parameter-must-have-an-nsobject-constraint"></a><a name="MT4128"/>MT4128 Kayıt şirketi bulunan bir genel geçersiz parametre türü '\*'yönteminde'\*'. Genel parametresini 'NSObject' kısıtlaması olmalıdır.

### <a name="a-namemt4129mt4129--the-registrar-found-an-invalid-generic-return-type--in-the-method--the-generic-return-type-must-have-an-nsobject-constraint"></a><a name="MT4129"/>MT4129 Kayıt bulundu geçersiz bir genel dönüş türü '\*'yönteminde'\*'. Genel dönüş türü, 'NSObject' kısıtlaması olmalıdır.

### <a name="a-namemt4130mt4130--the-registrar-cannot-export-static-methods-in-generic-classes-"></a><a name="MT4130"/>MT4130 Kayıt şirketi genel sınıflardaki statik yöntemler veremiyor ('* ').

### <a name="a-namemt4131mt4131--the-registrar-cannot-export-static-properties-in-generic-classes-"></a><a name="MT4131"/>MT4131 Kayıt şirketi statik özelliklerinde genel sınıfları dışarı aktarılamaz ('\*.\*').

### <a name="a-namemt4132mt4132--the-registrar-found-an-invalid-generic-return-type--in-the-property--the-return-type-must-have-an-nsobject-constraint"></a><a name="MT4132"/>MT4132 Kayıt bulundu geçersiz bir genel dönüş türü '\*'özelliğinde'\*'. Dönüş türü, 'NSObject' kısıtlaması olmalıdır.

### <a name="a-namemt4133mt4133--cannot-construct-an-instance-of-the-type--from-objective-c-because-the-type-is-generic-runtime-exception"></a><a name="MT4133"/>Türünün bir örneğini oluşturmak MT4133 olamaz ' *' Objective-c türü genel olduğundan. [Özel durum çalışma zamanı]

### <a name="a-namemt4134mt4134--your-application-is-using-the--framework-which-isnt-included-in-the-ios-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-ios--while-youre-building-with-the-ios--sdk-please-select-a-newer-sdk-in-your-apps-ios-build-options"></a><a name="MT4134"/>MT4134 Uygulamanızı kullanarak ' *' iOS SDK'sı ve uygulamanızı oluşturmak için kullandığınız bulunup framework (Bu framework iOS sunulmuştur * ile iOS oluşturmakta olduğunuz yaparken * SDK.) Lütfen daha yeni bir SDK'sı, uygulamanızın iOS derleme seçenekleri seçin.

### <a name="a-namemt4135mt4135--the-member--has-an-export-attribute-that-doesnt-specify-a-selector-a-selector-is-required"></a><a name="MT4135"/>MT4135 Üye '\*.\*' bir seçici belirtmeyen bir dışarı aktarma özniteliği vardır. Bir seçici gereklidir.

### <a name="a-namemt4136mt4136--the-registrar-cannot-marshal-the-parameter-type--of-the-parameter--in-the-method-"></a><a name="MT4136"/>MT4136 Parametre türü kayıt şirketi sıralanamıyor '\*'parametresinin'\*'yönteminde'\*. *'

<!-- MT4137 is unused -->

### <a name="a-namemt4138mt4138--the-registrar-cannot-marshal-the-property-type--of-the-property-"></a><a name="MT4138"/>MT4138 Özellik türü kayıt şirketi sıralanamıyor '\*'özelliğinin'\*. *'.

### <a name="a-namemt4139mt4139--the-registrar-cannot-marshal-the-property-type--of-the-property--properties-with-the-connect-attribute-must-have-a-property-type-of-nsobject-or-a-subclass-of-nsobject"></a><a name="MT4139"/>MT4139 Özellik türü kayıt şirketi sıralanamıyor '\*'özelliğinin'\*. *'. Özellikler [Bağlan] özniteliğiyle NSObject türünde bir özelliğe (veya NSObject öğesinin bir alt kümesi) içermelidir.

### <a name="a-namemt4140mt4140--the-registrar-found-a-signature-mismatch-in-the-method----the-selector-indicates-the-variadic-method-takes--parameters-while-the-managed-method-has--parameters"></a><a name="MT4140"/>MT4140 Kayıt şirketi bulunan bir imza uyuşmazlığı yönteminde '*.*'-variadic yöntemi alır seçiciyi gösterir * Yönetilen yöntemi sahipken parametreleri * parametreleri.

### <a name="a-namemt4141mt4141--cannot-register-the-selector--on-the-member--because-xamarinios-implicitly-registers-this-selector"></a><a name="MT4141"/>MT4141 kaydedilemiyor Seçici '\*'on 'üye\*' çünkü bu Seçici Xamarin.iOS örtük olarak kaydeder.

Framework türü sınıflara ve bir 'Beklet', uygulama çalışılırken 'release ' Bu oluşur veya 'dealloc' yöntemi:

```csharp
class MyNSObject : NSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Bu, ancak sınıf framework öğesinin ilk alt değilse bu yöntemleri geçersiz kılmak için olası yazın şöyledir:

```csharp
class MyNSObject : NSObject
{
}

class MyCustomNSObject : MyNSObject
{
    [Export ("retain")]
    new void Retain () {}

    [Export ("release")]
    new void Release () {}

    [Export ("dealloc")]
    new void Dealloc () {}
}
```

Bu durumda Xamarin.iOS geçersiz kılar `retain`, `release` ve `dealloc` üzerinde `MyNSObject` sınıfı ve hiçbir çakışma.

### <a name="a-namemt4142mt4142-failed-to-register-the-type-"></a><a name="MT4142"/>MT4142: türü kaydedilemedi ' *'.
### <a name="a-namemt4143mt4143-the-objectivec-class--could-not-be-registered-it-does-not-seem-to-derive-from-any-known-objectivec-class-including-nsobject"></a><a name="MT4143"/>MT4143: ObjectiveC sınıfı ' *' yüklenemedi kayıtlı, onu (NSObject dahil) bilinen herhangi ObjectiveC sınıfından türetilen görünmüyor.
### <a name="a-namemt4144mt4144-cannot-register-the-method--since-it-does-not-have-an-associated-trampoline-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4144"/>MT4144: register yöntemi ' *' ilişkili trampoline olmadığından. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4145mt4145-invalid-enum--enums-with-the-native-attribute-must-have-a-underlying-enum-type-of-either-long-or-ulong"></a><a name="MT4145"/>MT4145: Geçersiz sıralama ' *': numaralandırmaları [Yerel] özniteliği ile temel bir numaralandırma türü 'uzun' veya 'ulong' olması gerekir.
### <a name="a-namemt4146mt4146-the-name-parameter-of-the-registrar-attribute-on-the-class---contains-an-invalid-character--"></a><a name="MT4146"/>MT4146: Sınıf kayıt özniteliğinin adı parametresi\*' ('\*') geçersiz bir karakter içeriyor: '\*' (\*).

Bir Objectice-C sınıf adını anlamına boşluk içeremez `Register` karşılık gelen yönetilen Sınıf özniteliği olamaz bir `Name` parametresi içeremez boşluk ya da.

Lütfen doğrulayın `Register` hata iletisinde belirtilen yönetilen Sınıf özniteliği herhangi boşluk içeremez.

### <a name="a-namemt4147mt4147-detected-a-protocol-inheriting-from-the-jsexport-protocol-while-using-the-dynamic-registrar-it-is-not-possible-to-export-protocols-to-javascriptcore-dynamically-the-static-registrar-must-be-used-add---registrarstatic-to-the-additional-mtouch-arguments-in-the-projects-ios-build-options-to-select-the-static-registrar"></a><a name="MT4147"/>MT4147: dinamik kayıt kullanırken JSExport protokolünden devralan bir protokol algıladı. Protokolleri JavaScriptCore için dinamik olarak dışarı aktarmak mümkün değildir; statik kayıt kullanılmalıdır (Ekle '--Kaydedici: statik projenin iOS statik kayıt seçmek için derleme seçenekleri ek mtouch değişkenlerinde olarak).
### <a name="a-namemt4148mt4148-the-registrar-found-a-generic-protocol--exporting-generic-protocols-is-not-supported"></a><a name="MT4148"/>MT4148: Kayıt şirketi, bir genel Protokolü bulundu: ' *'. Genel protokolleri verme desteklenmiyor.
### <a name="a-namemt4149mt4149-cannot-register-the-method--because-the-type-of-the-first-parameter--does-not-match-the-category-type-"></a><a name="MT4149"/>MT4149: register yöntemi '\*.\*' çünkü ilk parametre türü ('\*') kategori türüyle eşleşmiyor ('\*').
### <a name="a-namemt4150mt4150-cannot-register-the-type--because-the-type-property--in-its-category-attribute-does-not-inherit-from-nsobject"></a><a name="MT4150"/>MT4150: türü kaydedilemiyor '\*' çünkü Type özelliği ('\*'), kategorisinde özniteliği NSObject devralmıyor.
### <a name="a-namemt4151mt4151-cannot-register-the-type--because-the-type-property-in-its-category-attribute-isnt-set"></a><a name="MT4151"/>MT4151: türü kaydedilemiyor ' *', kategori öznitelik türü özelliğinde olmadığından.
### <a name="a-namemt4152mt4152-cannot-register-the-type--as-a-category-because-it-implements-inativeobject-or-subclasses-nsobject"></a><a name="MT4152"/>MT4152: türü kaydedilemiyor ' *' kategori INativeObject veya alt sınıfların NSObject uyguladığından.
### <a name="a-namemt4153mt4153-cannot-register-the-type--as-a-category-because-its-generic"></a><a name="MT4153"/>MT4153: türü kaydedilemiyor '\*' bir kategori olarak genel olduğundan.
### <a name="a-namemt4154mt4154-cannot-register-the-method--as-a-category-method-because-its-generic"></a><a name="MT4154"/>MT4154: register yöntemi '\*' kategori yöntemi olarak genel olduğundan.
### <a name="a-namemt4155mt4155-cannot-register-the-method--with-the-selector--as-a-category-method-on--because-the-objective-c-already-has-an-implementation-for-this-selector"></a><a name="MT4155"/>MT4155: register yöntemi '\*'with 'Seçicisi\*' bir kategori yöntemi olarak ' *' Objective-C bu Seçici için bir uygulama zaten sahip olduğu.
### <a name="a-namemt4156mt4156-cannot-register-two-categories--and--with-the-same-native-name-"></a><a name="MT4156"/>MT4156: iki kategoride kaydedilemiyor ('\*'ve'\*') yerel aynı adla ('* ').
### <a name="a-namemt4157mt4157-cannot-register-the-category-method--because-at-least-one-parameter-is-required-and-its-type-must-match-the-category-type-"></a><a name="MT4157"/>MT4157: Kategori yöntemi kaydedilemiyor '\*' en az bir parametre gerektiğinden (ve onun türü kategori türüyle eşleşmelidir '\*')
### <a name="a-namemt4158mt4158-cannot-register-the-constructor--in-the-category--because-constructors-in-categories-are-not-supported"></a><a name="MT4158"/>MT4158: Oluşturucusu kaydedilemiyor * kategorisinde * kategorilerdeki oluşturucular desteklenmediğinden.
### <a name="a-namemt4159mt4159-cannot-register-the-method--as-a-category-method-because-category-methods-must-be-static"></a><a name="MT4159"/>MT4159: register yöntemi ' *' kategori yöntemi olarak kategori yöntemleri statik olması gerektiğinden.

### <a name="a-namemt4160mt4160-invalid-character---found-in-selector--for-"></a><a name="MT4160"/>MT4160: Geçersiz karakter '\*' (\*) Seçici içinde bulunan '\*'için'\*'.

### <a name="a-namemt4161mt4161-the-registrar-found-an-unsupported-structure--all-fields-in-a-structure-must-also-be-structures-field--with-type-2-is-not-a-structure"></a><a name="MT4161"/>MT4161: Kayıt bulundu desteklenmeyen yapısı '\*': bir yapıyı tüm alanlarda yapıları olması gerekir (alan '\*' türüyle '{2}' bir yapı değil).

Kayıt şirketi desteklenmeyen alanlar yapısıyla bulundu.

Objective-C için sunulan bir yapı tüm alanlar da yapıları (sınıfları değil) olması gerekir.

### <a name="a-namemt4162mt4162-the-type--used-as--2-is-not-available-in---it-was-introduced-in---please-build-with-a-newer--sdk-usually-done-by-using-the-most-recent-version-of-xcode"></a><a name="MT4162"/>MT4162: Türü '\*' (olarak kullanılan * {2}) kullanılamaz ** (de başlamış * *)\* Lütfen yeni bir derleme * SDK'sı (genellikle Xcode en son sürümü kullanılarak yapılır.

Kayıt şirketi, geçerli SDK'ın dahil edilmeyen bir tür bulundu.

Lütfen Xcode yükseltin.

### <a name="a-namemt4163mt4163-internal-error-in-the-registrar--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT4163"/>MT4163: İç hata (*) kayıt. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya

Bu hata, bir Xamarin.iOS hata gösterir. Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4164mt4164-cannot-export-the-property--because-its-selector--is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4164"/>MT4164: özellik veremiyor '\*' olduğundan, Seçici '\*' Objective-C anahtar sözcük. Lütfen farklı bir ad kullanın.

Seçici söz konusu özellik için geçerli bir Objective-C tanımlayıcısı değil.

Lütfen geçerli bir Objective-C tanımlayıcı Seçici kullanın.

### <a name="a-namemt4165mt4165-the-registrar-couldnt-find-the-type-systemvoid-in-any-of-the-referenced-assemblies"></a><a name="MT4165"/>MT4165: Kaydedici başvurulan derlemeleri hiçbirinde 'System.Void' türü bulunamadı.

Bu hata olasılıkla Xamarin.iOS bir hata gösterir. Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4166mt4166-cannot-register-the-method--because-the-signature-contains-a-type--that-isnt-a-reference-type"></a><a name="MT4166"/>MT4166: register yöntemi '\*' imza bir tür içerdiğinden (\*) bir başvuru türü değil.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4167mt4167-cannot-register-the-method--because-the-signature-contains-a-generic-type--with-a-generic-argument-type-that-isnt-an-nsobject-subclass-"></a><a name="MT4167"/>MT4167: register yöntemi '\*' imza genel bir tür içerdiğinden (\*) bir genel bağımsız değişken türüyle NSObject bir alt (*) değil.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt4168mt4168-cannot-register-the-type-managedname-because-its-objective-c-name-exportedname-is-an-objective-c-keyword-please-use-a-different-name"></a><a name="MT4168"/>MT4168: türü kaydedilemiyor ' {yönetilen\_adı}' olduğundan Objective-C adı ' {dışarı\_adı}' Objective-C anahtar sözcük. Lütfen farklı bir ad kullanın.

Objective-C adı söz konusu türü için geçerli bir Objective-C tanımlayıcısı değil.

Lütfen geçerli bir Objective-C tanımlayıcı kullanın.

## <a name="mt5xxx-gcc-and-toolchain-error-messages"></a>MT5xxx: GCC ve araç zinciri hata iletileri

### <a name="mt51xx-compilation"></a>MT51xx: derleme

<!--
 MT5xxx GCC and toolchain
  MT51xx compilation
  -->

### <a name="a-namemt5101mt5101-missing--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5101"/>MT5101 eksik ' *' derleyici. Lütfen 'Komut satırı araçları' bileşeni yükleyin



### <a name="a-namemt5102mt5102-failed-to-assemble-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5102"/>Dosya bir araya getirmek için MT5102 başarısız ' *'. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya



### <a name="a-namemt5103mt5103-failed-to-compile-the-file--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5103"/>MT5103 dosyası derlenemedi. ' *'. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya



### <a name="a-namemt5104mt5104-could-not-find-neither-the--nor-the--compiler-please-install-xcode-command-line-tools-component"></a><a name="MT5104"/>MT5104 bulamadı Hiçbiri '\*'veya '\*' derleyici. Lütfen 'Komut satırı araçları' bileşeni yükleyin

<!-- 5105 is used by mmp -->

### <a name="a-namemt5106mt5106-could-not-compile-the-files--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5106"/>MT5106: dosya derleme değil ' *'. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="mt52xx-linking"></a>MT52xx: Linking

<!--
  MT52xx linking
  -->

### <a name="a-namemt5201mt5201-native-linking-failed-please-review-the-build-log-and-the-user-flags-provided-to-gcc-"></a><a name="MT5201"/>MT5201 yerel bağlama başarısız oldu. Lütfen derleme günlüğünde ve gcc için sağlanan kullanıcı bayrakları gözden geçirin: *

### <a name="a-namemt5202mt5202-native-linking-failed-please-review-the-build-log"></a><a name="MT5202"/>MT5202 yerel bağlama başarısız oldu. Lütfen yapı günlüğünü gözden geçirin.

### <a name="a-namemt5203mt5203-native-linking-warning-"></a><a name="MT5203"/>MT5203 uyarı bağlama yerel: *

<!--- 5204-5208 are not used -->

### <a name="a-namemt5209mt5209-native-linking-error-"></a><a name="MT5209"/>MT5209 hata bağlama yerel: *

### <a name="a-namemt5210mt5210-native-linking-failed-undefined-symbol--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-are-properly-linked-in"></a><a name="MT5210"/>MT5210: Yerel bağlama başarısız oldu, tanımlanmamış simge: *. Lütfen gerekli tüm çerçeveler başvurulan ve yerel kitaplıkları düzgün bir şekilde bağlı doğrulayın.

Yerel bağlayıcı yerde başvurulan bir simge bulamadığında bu gerçekleşir. Bunun birkaç nedeni vardır:

* Bir üçüncü taraf bağlama çerçevesi gerektiriyor, ancak bağlama bu belirtmiyor kendi `[LinkWith]` özniteliği. Çözüm:
  - Üçüncü taraf bağlama yazarı olduğunuz veya kendi kaynağına erişiminiz varsa, bağlamanın değiştirme `[LinkWith]` özniteliği gerekli framework içerir:

            [LinkWith ("mylib.a", Frameworks = "SystemConfiguration")]

  - Üçüncü taraf bağlama değiştirilemiyor varsa, el ile gerekli çerçevesiyle geçirerek bağlayabilirsiniz `-gcc_flags '-framework SystemFramework'` için `mtouch` (Bu projenin iOS derleme seçenekleri sayfasında ek mtouch değişkenlerinde değiştirilerek yapılır. Bu her proje yapılandırması için yapılması gereken unutmayın).
* Bazı durumlarda bir yönetilen bağlama birkaç yerel kitaplıklarının oluşur ve tüm bağlamaları eklenmelidir. Yalnızca gerekli tüm yerel kitaplıklar bağlama projeye eklemek için çözüm olacak şekilde birden fazla yerel kitaplığı her bağlama projesinde olması mümkündür.</li>
* Yönetilen bir bağlama yerel Kitaplığı'nda yoksa yerel semboller ifade eder.
    Bu genellikle bir bağlama süre için var ve belirli bir yerel sınıf ya da kaldırılmıştır veya bağlama güncelleştirilmez karşın, yeniden adlandırılmış, yerel kod bu işlem sırasında değiştirilmiş olur.
* P/Invoke yok yerel bir sembol belirtir. Xamarin.iOS 7.4 ile başlayan bir <a href="#MT5214">MT5214</a> bu durumda hata bildirilir (daha fazla bilgi için bkz: MT5214).
* Bir üçüncü taraf bağlama / kitaplığı C++ kullanarak yapılmış, ancak bağlama bu belirtmiyor kendi `[LinkWith]` özniteliği. Bu genellikle tanımak oldukça kolaydır, simgeler sahip olduğundan karıştırılmış C++ simgelerini değil (bir ortak örnektir `__ZNKSt9exception4whatEv`).
  - Üçüncü taraf bağlama yazarı olduğunuz veya kendi kaynağına erişiminiz varsa, bağlamanın değiştirme `[LinkWith]` ayarlamak için öznitelik `IsCxx` bayrağı:

            [LinkWith ("mylib.a", IsCxx = true)]

  - Üçüncü taraf bağlama değiştirilemez ya da üçüncü taraf kitaplığı ile el ile bağlıyorsanız, ileterek eşdeğer bayrağı ayarlayabilirsiniz <code>-cxx</code> (Bu işlem projenin iOS derleme seçenekleri sayfasında ek mtouch değişkenlerinde değiştirerek mtouch için . Bu her proje yapılandırması için yapılması gereken unutmayın).



### <a name="a-namemt5211mt5211-native-linking-failed-undefined-objective-c-class--the-symbol--could-not-be-found-in-any-of-the-libraries-or-frameworks-linked-with-your-application"></a><a name="MT5211"/>MT5211: Yerel bağlama başarısız oldu, tanımlanmamış Objective-C sınıfı: \*. Simgenin '\*' uygulamanız ile bağlantılı çerçeveler ve kitaplıklar hiçbirinde bulunamadı.

Yerel bağlayıcı yerde başvurulan bir Objective-C sınıfı bulamadığında bu gerçekleşir. Bu durum birkaç nedeni vardır: aynı [MT5210](#MT5210) ve ayrıca:

* Bir üçüncü taraf bağlama Objective-C Protokolü bağlı, ancak onunla açıklama değil <code>[Protocol]</code> kendi API tanımı özniteliği. Çözüm:
  - Eksik ekleme `[Protocol]` özniteliği:

              [BaseType (typeof (NSObject))]
              [Protocol] // Add this
              public interface MyProtocol
              {
              }



### <a name="a-namemt5212mt5212-native-linking-failed-duplicate-symbol-"></a><a name="MT5212"/>MT5212: Yerel bağlama başarısız oldu, yinelenen simgesi: *.

Bu yerel bağlayıcı yinelenen simgeleri tüm yerel kitaplıkları arasında karşılaştığında gerçekleşir. Bu hatayı olabilir bir veya daha fazla [MT5213](#MT5213) simgenin her oluşumu konumuyla hataları. Bu hata için olası nedenler:


* Aynı yerel kitaplığı iki kez dahil edilir.
* Aynı simgeleri tanımlamak için iki farklı yerel kitaplıkları gerçekleşir.
* Yerel bir kitaplık doğru şekilde oluşturulmadı ve aynı sembolün birden çok kez içerir.
  Bu bir terminal komutları aşağıdaki kümesini kullanarak onaylayın (i386 x86_64/armv7/armv7s/göre hangi mimarisi, oluşturmak için arm64 değiştirin):

        # Native libraries are usually fat libraries, containing binary code for
        # several architectures in the same file. First we extract the binary
        # code for the architecture we're interested in.
        lipo libNative.a -thin i386 -output libNative.i386.a

        # Now query the native library for the duplicated symbol.
        nm libNative.i386.a | fgrep 'SYMBOL'

        # You can also list the object files inside the native library.
        # In most cases this will reveal duplicated object files.
        ar -t libNative.i386.a

  Bu sorunu gidermek için olası birkaç yolu vardır:

  - İstek yerel kitaplığı sağlayıcısı Düzelt ve güncelleştirilmiş sürümü sağlayın.
  - Kendiniz (sorun aslında yinelenen nesne dosyaları ise, bu yalnızca çalışır) fazladan nesne dosyaları kaldırarak Düzelt


            # Find out if the library is a fat library, and which
            # architectures it contains.
            lipo -info libNative.a

            # Extract each architecture (i386/x86_64/armv7/armv7s/arm64) to a separate file
            lipo libNative.a -thin ARCH -output libNative.ARCH.a

            # Extract the object files for the offending architecture
            # This will remove the duplicates by overwriting them
            # (since they have the same filename)
            mkdir -p ARCH
            cd ARCH
            ar -x ../libNative.ARCH.a

            # Reassemble the object files in an .a
            ar -r ../libNative.ARCH.a *.o
            cd ..

            # Reassemble the fat library
            lipo *.a -create -output libNative.a


  - Kullanılmayan kodu kaldırmak için bağlayıcı isteyin. Aşağıdaki koşulların tümü yerine getirilirse Xamarin.iOS bu otomatik olarak yapar:
    - Tüm üçüncü taraf bağlamaları `[LinkWith]` öznitelikleri SmartLink etkin:

            [assembly: LinkWith ("libNative.a", SmartLink = true)]

    - Hayır `-gcc_flags` mtouch (alanındaki Ek mtouch bağımsız değişkenleri projenin iOS derleme seçenekleri) geçirilir.
    - Doğrudan kullanılmayan kod ekleyerek kaldırmak için bağlayıcı istemeniz mümkündür `-gcc_flags -dead_strip` projenin iOS ek mtouch değişkenlerinde için derleme seçenekleri.


### <a name="a-namemt5213mt5213-duplicate-symbol-in--location-related-to-previous-error"></a><a name="MT5213"/>MT5213: Yinelenen sembol: * (konum ilgili önceki hata)

Bu hata yalnızca ile birlikte bildirilen [MT5212](#MT5212). Lütfen bakın [MT5212](#MT5212) daha fazla bilgi için.

### <a name="a-namemt5214mt5214--native-linking-failed-undefined-symbol--this-symbol-was-referenced-the-managed-member--please-verify-that-all-the-necessary-frameworks-have-been-referenced-and-native-libraries-linked"></a><a name="MT5214"/>MT5214 başarısız, bağlama yerel tanımsız simgesi: *. Bu simgenin başvuruldu yönetilen üye *. Tüm gerekli çerçeveleri bağlı başvurulan ve yerel kitaplıkları verildiğinden emin olun.

Yönetilen kod yok yerel bir yöntemi için bir P/Invoke içerdiğinde bu hata bildirilir. Örneğin:

```csharp
using System.Runtime.InteropServices;
class MyImports {
   [DllImport ("__Internal")]
   public static extern void MyInexistentFunc ();
}
```

Birkaç olası çözümleri şunlardır:

  -  P/Invokes söz konusu kaynak kodundan kaldırın.
  -  (Bu projenin iOS derleme seçenekleri "Bağlayıcı davranışı" "Tüm derlemelerin" ayarlayarak yapılır) tüm derlemeler için yönetilen bağlayıcı etkinleştirin. Bu etkili bir şekilde uygulamadan tüm P/kullanmadığınız Invokes kaldırır (otomatik olarak yerine el ile önceki noktası gibi). Bu simulator derlemeleriniz biraz daha yavaş hale getirir ve uygulamanızı kesilebilir yansıma kullanarak - bağlayıcı hakkında daha fazla bilgi bulunabilir, dezavantajı olan [burada](~/ios/deploy-test/linker.md) )
  -  Eksik yerel sembolleri içeren ikinci bir yerel kitaplığı oluşturun. Bu yalnızca bir geçici çözüm olduğuna dikkat edin (Bu işlevleri çağırmak denemeye görülüyorsa, uygulamanızı kilitlenir).

### <a name="a-namemt5215mt5215-references-to--might-require-additional--frameworkxxx-or--lxxx-instructions-to-the-native-linker"></a><a name="MT5215"/>MT5215: başvurular ' *' ek - gerektirebilir framework yerel bağlayıcı XXX veya - lXXX yönergeleri =

P/Invoke söz konusu kitaplığı başvuru algılandı, ancak uygulama ile DLL'e değil belirten bir uyarı budur.

### <a name="a-namemt5216mt5216-native-linking-failed-for--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT5216"/>MT5216: İçin yerel bağlama başarısız *. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya

Bu hata, uygulama nesne AĞACI Derleyici çıktısı bağlarken bildirilir.

Bu hata olasılıkla Xamarin.iOS bir hata gösterir. Lütfen sırasında bir hata raporu dosya [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt5217mt5217-native-linking-possibly-failed-because-the-linker-command-line-was-too-long--characters"></a><a name="MT5217"/>MT5217: bağlayıcı komut satırı çok uzun olduğundan yerel bağlama büyük olasılıkla başarısız oldu (* karakter).

Yerel bağlama başarısız oldu ve bağlayıcı komutu çok uzun olduğundan bu durumun oluştuğu mümkündür.

Xamarin.iOS projeleri genellikle yerel semboller dinamik olarak yerel bağlayıcı görmez bu simgeleri kullanılır çünkü yerel bağlayıcı yerel bağlama işlemi sırasında bu tür yerel semboller kaldırabilir yani başvurur.

Xamarin.iOS kullanarak bu tür sembol tutmak için yerel bağlayıcı genellikle ister `-u symbol` birçok gibi semboller, tüm komut satırı olup olmadığını ancak bağlayıcı bayrağı, işletim sistemi tarafından belirlenen maksimum komut satırı uzunluğunu aşıyor.

Dinamik tür sembolleri için birkaç olası kaynakları vardır:

* P/Invokes yöntemleri statik olarak bağlantılı kitaplıklarında (dll adı olduğu `__Internal` DllImport özniteliği içinde `[DllImport ("__Internal")]`).
* Alan projeleri bağlama gelen statik olarak bağlantılı kitaplıklar bellek konumlarda başvurular (`[Field]` öznitelikleri).
* Objective-C sınıfları (artımlı derlemeler kullanırken veya statik kayıt kullanmadığınızda) projeleri bağlama gelen statik olarak bağlantılı kitaplıklar başvuru.

Olası çözümler:

* Yönetilen bağlayıcı (yalnızca SDK derlemeleri yerine tüm derlemeler için mümkünse) etkinleştirin. Olmayan dinamik simgeleri bağlayıcı komut satırı böylece aşıldı için en fazla bu yeterli kaynakları kaldırabilir.
* P/Invokes, alan başvuruları ve/veya Objective-C sınıfları sayısını azaltın.
* Dinamik simgeler adları daha kısa olacak yeniden yazın.
* Geçirmek `-dlsym:false` projenin iOS bir ek mtouch bağımsız değişken olarak derleme seçenekleri. Bu seçenek ile Xamarin.iOS yerel bir başvuru uygulama nesne AĞACI derlenmiş kod oluşturur ve bu simgeyi tutmak için bağlayıcı istemeniz gerekmez. Bununla birlikte, cihaz için yalnızca bu works oluşturur ve statik kitaplığında mevcut olmayan işlevlere P/Invokes varsa bağlayıcı hatalarını neden olur.
* Geçirmek `--dynamic-symbol-mode=code` projenin iOS bir ek mtouch bağımsız değişken olarak derleme seçenekleri. Bu seçenek ile Xamarin.iOS komut satırı bağımsız değişkenleri kullanarak bu simgeleri tutmak için yerel bağlayıcı isteyen yerine bu simgeleri başvuran ek yerel kod oluşturur. Bu yaklaşımın dezavantajı, yürütülebilir dosya boyutunu biraz artacağını ' dir.
* Statik kayıt geçirerek etkinleştirmek `--registrar:static` yapı projenin iOS bir ek mtouch bağımsız değişken olarak (aygıt derlemeler için statik Kaydedici zaten varsayılan olduğundan simulator, derlemeler için) seçenekleri. Bu yüzden bu tür sınıflar tutmak için yerel bağlayıcı isteyin gerek yoktur statik kayıt statik olarak, Objective-C sınıfları başvuran kodu oluşturur.
* Artımlı derlemeler (için cihaz derlemeleri) devre dışı bırakın. Artımlı derlemeler etkinleştirildiğinde, statik kayıt şirketi tarafından oluşturulan kodu Xamarin.iOS tutmak için bağlayıcı kaldırmasını isteyebilirsiniz anlamına Objective-C sınıfları başvurulan yerel bağlayıcı tarafından kabul olmaz. Bu nedenle artımlı derlemeler devre dışı bırakma, bu gereksinimi engeller.

### <a name="a-namemt5218mt5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a><a name="MT5218"/>MT5218: dinamik simgenin {simgesi} yoksayamaz (--Yoksay-dinamik-simge = {simgesi}) dinamik bir simge bulunamadığından.

Komut satırı bağımsız değişkeni `--ignore-dynamic-symbol=symbol` geçirildi, ancak bu simgeyi el ile korunmalıdır dinamik bir simge olarak tanınan bir simge değil.

Bu iki ana nedeni vardır:

* Sembol adı geçersiz.
    * Sembol adı alt çizgi başına yok.
    * Objective-C sınıfları için simge `OBJC_CLASS_$_<classname>`.
* Simgenin doğru ancak zaten (bazı seçenekleri nedenler tam listesi değiştirmek için dinamik simgelerin oluşturun) normal yöntemlerle korunur bir simge gelir.

### <a name="mt53xx-other-tools"></a>MT53xx: Diğer araçları

<!--
  MT53xx other tools
  -->

### <a name="a-namemt5301mt5301-missing-strip-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5301"/>MT5301: 'Şerit' aracı eksik. Lütfen 'Komut satırı araçları' bileşeni yükleyin



### <a name="a-namemt5302mt5302-missing-dsymutil-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5302"/>MT5302: Eksik 'dsymutil' aracı. Lütfen 'Komut satırı araçları' bileşeni yükleyin



### <a name="a-namemt5303mt5303-failed-to-generate-the-debug-symbols-dsym-directory-please-review-the-build-log"></a><a name="MT5303"/>MT5303: hata ayıklama simgeleri (dSYM dizin) oluşturulamadı. Lütfen yapı günlüğünü gözden geçirin.

Dsymutil hata ayıklama simgeleri oluşturmak için son .app dizin çalıştırırken bir hata oluştu. Lütfen dsymutil'ın bir çıktı görmeniz ve nasıl düzeltilmesi görmek için yapı günlüğünü gözden geçirin.

### <a name="a-namemt5304mt5304-failed-to-strip-the-final-binary-please-review-the-build-log"></a><a name="MT5304"/>MT5304: son ikili dizeden başarısız oldu. Lütfen yapı günlüğünü gözden geçirin.

Uygulamayı hata ayıklama bilgileri kaldırmak için 'Şerit' Aracı çalıştırırken bir hata oluştu.

### <a name="a-namemt5305mt5305-missing-lipo-tool-please-install-xcode-command-line-tools-component"></a><a name="MT5305"/>MT5305: Eksik 'lipo' aracı. Lütfen 'Komut satırı araçları' bileşeni yükleyin



### <a name="a-namemt5306mt5306-failed-to-create-the-a-fat-library-please-review-the-build-log"></a><a name="MT5306"/>MT5306: oluşturulamadı fat kitaplığı. Lütfen yapı günlüğünü gözden geçirin.

'Lipo' Aracı'nı çalıştırırken bir hata oluştu. Lütfen 'lipo' tarafından bildirilen hatayı görmek için yapı günlüğünü gözden geçirin.

### <a name="a-namemt5307mt5307-failed-to-sign-the-executable-please-review-the-build-log"></a><a name="MT5307"/>MT5307: yürütülebilir dosya oturumu açılamadı. Lütfen yapı günlüğünü gözden geçirin.

Uygulama imzalarken bir hata oluştu. Lütfen 'aşağıda' tarafından bildirilen hatayı görmek için yapı günlüğünü gözden geçirin.

<!-- 5308 is used by mmp -->
<!-- 5309 is used by mmp -->
<!-- 5310 is used by mmp -->

## <a name="mt6xxx-mtouch-internal-tools-error-messages"></a>MT6xxx: mtouch iç hata iletileri araçları

### <a name="mt600x-stripper"></a>MT600x: Stripper

<!--
 MT6xxx mtouch internal tools
  MT600x Stripper
  -->

### <a name="a-namemt6001mt6001-running-version-of-cecil-doesnt-support-assembly-stripping"></a><a name="MT6001"/>MT6001: Çıkarma derleme CECIL çalışan sürümünü desteklemiyor

### <a name="a-namemt6002mt6002-could-not-strip-assembly-"></a><a name="MT6002"/>MT6002: derleme Şerit değil `*`.

Uygulamada derlemeleri yönetilen koddan (IL kodu kaldırma) çıkarma bağlanırken bir hata oluştu.

### <a name="a-namemt6003mt6003-unauthorizedaccessexception-message"></a><a name="MT6003"/>MT6003: [UnauthorizedAccessException ileti]

Hata ayıklama simgeleri uygulamadan çıkarma bir güvenlik hatası oluştu.

## <a name="mt7xxx-msbuild-error-messages"></a>MT7xxx: MSBuild error messages

<!--
 MT7xxx msbuild errors
  -->

### <a name="a-namemt7001mt7001-could-not-resolve-host-ips-for-wifi-debugger-settings"></a><a name="MT7001"/>MT7001: ana bilgisayar IP WiFi hata ayıklayıcı ayarları için çözümlenemedi.

*MSBuild görevi: DetectDebugNetworkConfigurationTaskBase*

Sorun giderme adımları:

- çalıştırmayı denediğinizde `csharp -e 'System.Net.Dns.GetHostEntry (System.Net.Dns.GetHostName ()).AddressList'` (, size bir IP adresi ve bir hata açıktır).
- çalıştırmayı denediğinizde "ping \`ana bilgisayar adı\`" hangi size daha fazla bilgi gibi: `cannot resolve MyHost.local: Unknown host`

İsteğe bağlı olarak bazı durumlarda, bir "yerel ağ" sorunu olan ve bilinmeyen ana bilgisayar ekleyerek çözülebilir `127.0.0.1   MyHost.local` içinde `/etc/hosts`.

### <a name="a-namemt7002mt7002-this-machine-does-not-have-any-network-adapters-this-is-required-when-debugging-or-profiling-on-device-over-wifi"></a><a name="MT7002"/>MT7002: Bu makinenin ağ bağdaştırıcısı yok. Bu, hata ayıklama veya WiFi cihazda profil durumlarda gereklidir.

*MSBuild görevi: DetectDebugNetworkConfigurationTaskBase*

### <a name="a-namemt7003mt7003-the-app-extension--does-not-contain-an-infoplist"></a><a name="MT7003"/>MT7003: Uygulama Uzantısı ' *' bir Info.plist içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7004mt7004-the-app-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7004"/>MT7004: Uygulama Uzantısı ' *' bir Cfbundleıdentifier belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7005mt7005-the-app-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7005"/>MT7005: Uygulama Uzantısı ' *' bir CFBundleExecutable belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7006mt7006-the-app-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7006"/>MT7006: Uygulama Uzantısı '\*' geçersiz bir Cfbundleıdentifier sahip (\*), ana uygulama paketin Cfbundleıdentifier ile (*) başlamaz.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7007mt7007-the-app-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7007"/>MT7007: Uygulama Uzantısı '\*' bir Cfbundleıdentifier sahip (\*) ".key" geçersiz sonekiyle sona erer.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7008mt7008-the-app-extension--does-not-specify-a-cfbundleshortversionstring"></a><a name="MT7008"/>MT7008: Uygulama Uzantısı ' *' bir CFBundleShortVersionString belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7009mt7009-the-app-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7009"/>MT7009: Uygulama Uzantısı ' *' geçersiz bir Info.plist vardır: bir NSExtension sözlük içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7010mt7010-the-app-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionpointidentifier-value"></a><a name="MT7010"/>MT7010: Uygulama Uzantısı ' *' geçersiz bir Info.plist vardır: NSExtension sözlük NSExtensionPointIdentifier değeri içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7011mt7011-the-watchkit-extension--has-an-invalid-infoplist-the-nsextension-dictionary-does-not-contain-an-nsextensionattributes-dictionary"></a><a name="MT7011"/>MT7011: WatchKit uzantısı ' *' geçersiz bir Info.plist vardır: NSExtension sözlük NSExtensionAttributes sözlük içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7012mt7012-the-watchkit-extension--does-not-have-exactly-one-watch-app"></a><a name="MT7012"/>MT7012: WatchKit uzantısı ' *' tam olarak bir izleme uygulama yok.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7013mt7013-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-must-contain-the-watch-companion-capability"></a><a name="MT7013"/>MT7013: WatchKit uzantısı ' *' geçersiz bir Info.plist vardır: UIRequiredDeviceCapabilities 'watch-yardımcı' özelliği içermesi gerekir.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7014mt7014-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7014"/>MT7014: İzleme uygulama ' *' bir Info.plist içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7015mt7015-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7015"/>MT7015: İzleme uygulama ' *' bir Cfbundleıdentifier belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7016mt7016-the-watch-app--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7016"/>MT7016: İzleme uygulama '\*' geçersiz bir Cfbundleıdentifier sahip (\*), ana uygulama paketin Cfbundleıdentifier ile (*) başlamaz.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7017mt7017-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected-watch-4-but-found--"></a><a name="MT7017"/>MT7017: İzleme uygulama '\*' geçerli bir UIDeviceFamily değeri yok. Beklenen '(4) izleme' bulundu ancak '\* (*)'.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7018mt7018-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7018"/>MT7018: İzleme uygulama ' *' bir CFBundleExecutable belirtmiyor

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7019mt7019-the-watch-app--has-an-invalid-wkcompanionappbundleidentifier-value--it-does-not-match-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7019"/>MT7019: İzleme uygulama '\*' WKCompanionAppBundleIdentifier değeri geçersiz ('\*'), ana uygulama paketin Cfbundleıdentifier eşleşmiyor ('* ').

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7020mt7020-the-watch-app--has-an-invalid-infoplist-the-wkwatchkitapp-key-must-be-present-and-have-a-value-of-true"></a><a name="MT7020"/>MT7020: İzleme uygulama ' *' geçersiz bir Info.plist vardır: WKWatchKitApp anahtarı mevcut olmalı ve 'true' değerine sahip.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7021mt7021-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7021"/>MT7021: İzleme uygulama ' *' geçersiz bir Info.plist vardır: LSRequiresIPhoneOS anahtarı mevcut olmaması gerekir.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7022mt7022-the-watch-app--does-not-contain-a-watch-extension"></a><a name="MT7022"/>MT7022: İzleme uygulama ' *' izleme uzantısı içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7023mt7023-the-watch-extension--does-not-contain-an-infoplist"></a><a name="MT7023"/>MT7023: İzleme uzantısı ' *' bir Info.plist içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7024mt7024-the-watch-extension--does-not-specify-a-cfbundleidentifier"></a><a name="MT7024"/>MT7024: İzleme uzantısı ' *' bir Cfbundleıdentifier belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7025mt7025-the-watch-extension--does-not-specify-a-cfbundleexecutable"></a><a name="MT7025"/>MT7025: İzleme uzantısı ' *' bir CFBundleExecutable belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7026mt7026-the-watch-extension--has-an-invalid-cfbundleidentifier--it-does-not-begin-with-the-main-app-bundles-cfbundleidentifier-"></a><a name="MT7026"/>MT7026: İzleme uzantısı '\*' geçersiz bir Cfbundleıdentifier sahip (\*), ana uygulama paketin Cfbundleıdentifier ile (*) başlamaz.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7027mt7027-the-watch-extension--has-a-cfbundleidentifier--that-ends-with-the-illegal-suffix-key"></a><a name="MT7027"/>MT7027: İzleme uzantısı '\*' bir Cfbundleıdentifier sahip (\*) ".key" geçersiz sonekiyle sona erer.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7028mt7028-the-watch-extension--has-an-invalid-infoplist-it-does-not-contain-an-nsextension-dictionary"></a><a name="MT7028"/>MT7028: İzleme uzantısı ' *' geçersiz bir Info.plist vardır: bir NSExtension sözlük içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7029mt7029-the-watch-extension--has-an-invalid-infoplist-the-nsextensionpointidentifier-must-be-comapplewatchkit"></a><a name="MT7029"/>MT7029: İzleme uzantısı ' *' geçersiz bir Info.plist vardır: NSExtensionPointIdentifier "com.apple.watchkit" olması gerekir.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7030mt7030-the-watch-extension--has-an-invalid-infoplist-the-nsextension-dictionary-must-contain-nsextensionattributes"></a><a name="MT7030"/>MT7030: İzleme uzantısı ' *' geçersiz bir Info.plist vardır: NSExtension sözlük NSExtensionAttributes içermesi gerekir.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7031mt7031-the-watch-extension--has-an-invalid-infoplist-the-nsextensionattributes-dictionary-must-contain-a-wkappbundleidentifier"></a><a name="MT7031"/>MT7031: İzleme uzantısı ' *' geçersiz bir Info.plist vardır: NSExtensionAttributes sözlüğün bir WKAppBundleIdentifier içermesi gerekir.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7032mt7032-the-watchkit-extension--has-an-invalid-infoplist-uirequireddevicecapabilities-should-not-contain-the-watch-companion-capability"></a><a name="MT7032"/>MT7032: WatchKit uzantısı ' *' geçersiz bir Info.plist vardır: UIRequiredDeviceCapabilities 'watch-yardımcı' özelliği içeremez.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7033mt7033-the-watch-app--does-not-contain-an-infoplist"></a><a name="MT7033"/>MT7033: İzleme uygulama ' *' bir Info.plist içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7034mt7034-the-watch-app--does-not-specify-a-cfbundleidentifier"></a><a name="MT7034"/>MT7034: İzleme uygulama ' *' bir Cfbundleıdentifier belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7035mt7035-the-watch-app--does-not-have-a-valid-uidevicefamily-value-expected--but-found--"></a><a name="MT7035"/>MT7035: İzleme uygulama '\*' geçerli bir UIDeviceFamily değeri yok. Beklenen '\*'bulundu ancak'\* (\*)'.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7036mt7036-the-watch-app--does-not-specify-a-cfbundleexecutable"></a><a name="MT7036"/>MT7036: İzleme uygulama ' *' bir CFBundleExecutable belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7037mt7037-the-watchkit-extension-extensionname-has-an-invalid-wkappbundleidentifier-value--it-does-not-match-the-watch-apps-cfbundleidentifier-"></a><a name="MT7037"/>MT7037: WatchKit uzantı '{UzantıAdı}' WKAppBundleIdentifier geçersiz ('\*'), izleme uygulamanın Cfbundleıdentifier eşleşmiyor ('\*').

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7038mt7038-the-watch-app--has-an-invalid-infoplist-the-wkcompanionappbundleidentifier-must-exist-and-must-match-the-main-app-bundles-cfbundleidentifier"></a><a name="MT7038"/>MT7038: İzleme uygulama ' *' geçersiz bir Info.plist vardır: WKCompanionAppBundleIdentifier bulunmalı ve ana uygulama paketin Cfbundleıdentifier eşleşmesi gerekir.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7039mt7039-the-watch-app--has-an-invalid-infoplist-the-lsrequiresiphoneos-key-must-not-be-present"></a><a name="MT7039"/>MT7039: İzleme uygulama ' *' geçersiz bir Info.plist vardır: LSRequiresIPhoneOS anahtarı mevcut olmaması gerekir.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7040mt7040-the-app-bundle-appbundlepath-does-not-contain-an-infoplist"></a><a name="MT7040"/>MT7040: {AppBundlePath} uygulama paketi bir Info.plist içermiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7041mt7041-main-infoplist-path-does-not-specify-a-cfbundleidentifier"></a><a name="MT7041"/>MT7041: Bir Cfbundleıdentifier ana Info.plist yolu belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7042mt7042-main-infoplist-path-does-not-specify-a-cfbundleexecutable"></a><a name="MT7042"/>MT7042: Bir CFBundleExecutable ana Info.plist yolu belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7043mt7043-main-infoplist-path-does-not-specify-a-cfbundlesupportedplatforms"></a><a name="MT7043"/>MT7043: Bir CFBundleSupportedPlatforms ana Info.plist yolu belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7044mt7044-main-infoplist-path-does-not-specify-a-uidevicefamily"></a><a name="MT7044"/>MT7044: Bir UIDeviceFamily ana Info.plist yolu belirtmiyor.

*MSBuild görevi: ValidateAppBundleTaskBase*

### <a name="a-namemt7045mt7045-unrecognized-format-"></a><a name="MT7045"/>MT7045: Tanınmayan biçimi: *.

*MSBuild görevi: PropertyListEditorTaskBase*

Burada * olabilir:

- dize
- dizi
- dict
- bool
- gerçek
- tamsayı
- Tarih
- veri

### <a name="a-namemt7046mt7046-add-entry--incorrectly-specified"></a><a name="MT7046"/>MT7046: Ekleyin: girişi, *, hatalı belirtilmiş.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7047mt7047-add-entry--contains-invalid-array-index"></a><a name="MT7047"/>MT7047: Ekleyin: girişi, *, geçersiz bir dizi dizini içerir.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7048mt7048-add--entry-already-exists"></a><a name="MT7048"/>MT7048: Ekleyin: * girişi zaten mevcut.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7049mt7049-add-cant-add-entry--to-parent"></a><a name="MT7049"/>MT7049: Ekleyin: girişi, eklenemiyor *, üst öğeye.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7050mt7050-delete-cant-delete-entry--from-parent"></a><a name="MT7050"/>MT7050: Silin: Giriş, silinemez *, üst.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7051mt7051-delete-entry--contains-invalid-array-index"></a><a name="MT7051"/>MT7051: Silin: girişi, *, geçersiz bir dizi dizini içerir.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7052mt7052-delete-entry--does-not-exist"></a><a name="MT7052"/>MT7052: Silin: girişi, *, mevcut değil.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7053mt7053-import-entry--incorrectly-specified"></a><a name="MT7053"/>MT7053: Alma: girişi, *, hatalı belirtilmiş.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7054mt7054-import-entry--contains-invalid-array-index"></a><a name="MT7054"/>MT7054: Alma: girişi, *, geçersiz bir dizi dizini içerir.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7055mt7055-import-error-reading-file-"></a><a name="MT7055"/>MT7055: Alma: dosya okuma hatası: *.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7056mt7056-import-cant-add-entry--to-parent"></a><a name="MT7056"/>MT7056: Alma: girişi, eklenemiyor *, üst öğeye.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7057mt7057-merge-cant-add-array-entries-to-dict"></a><a name="MT7057"/>MT7057: Birleştirme: dizi girişleri için dict. eklenemiyor

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7058mt7058-merge-specified-entry-must-be-a-container"></a><a name="MT7058"/>MT7058: Birleştirme: Belirtilen giriş bir kapsayıcı olmalıdır.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7059mt7059-merge-entry--contains-invalid-array-index"></a><a name="MT7059"/>MT7059: Birleştirme: girişi, *, geçersiz bir dizi dizini içerir.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7060mt7060-merge-entry--does-not-exist"></a><a name="MT7060"/>MT7060: Birleştirme: girişi, *, mevcut değil.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7061mt7061-merge-error-reading-file-"></a><a name="MT7061"/>MT7061: Merge: Error Reading File: *.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7062mt7062-set-entry--incorrectly-specified"></a><a name="MT7062"/>MT7062: Ayarlayın: girişi, *, hatalı belirtilmiş.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7063mt7063-set-entry--contains-invalid-array-index"></a><a name="MT7063"/>MT7063: Ayarlayın: girişi, *, geçersiz bir dizi dizini içerir.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7064mt7064-set-entry--does-not-exist"></a><a name="MT7064"/>MT7064: Ayarlayın: girişi, *, mevcut değil.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7065mt7065-unknown-propertylist-editor-action-"></a><a name="MT7065"/>MT7065: Bilinmeyen ÖzellikListesi Düzenleyicisi eylem: *.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7066mt7066-error-loading--"></a><a name="MT7066"/>MT7066: Yükleme hatası ' *': *.

*MSBuild görevi: PropertyListEditorTaskBase*

### <a name="a-namemt7067mt7067-error-saving--"></a><a name="MT7067"/>MT7067: Hata kaydetme ' *': *.

*MSBuild görevi: PropertyListEditorTaskBase*


## <a name="mt8xxx-runtime-error-messages"></a>MT8xxx: Çalışma zamanı hata iletileri

<!--
 MT8xxx runtime
  MT800x misc
  -->

### <a name="a-namemt8001mt8001--version-mismatch-between-the-native-xamarinios-runtime-and-monotouchdll-please-reinstall-xamarinios"></a><a name="MT8001"/>Yerel Xamarin.iOS çalışma zamanı ve monotouch.dll arasında MT8001 sürüm uyuşmazlığı. Please reinstall Xamarin.iOS.

### <a name="a-namemt8002mt8002--could-not-find-the-method--in-the-type-"></a><a name="MT8002"/>MT8002 bulamadı yöntemi '\*'içindeki type'\*'.

### <a name="a-namemt8003mt8003--failed-to-find-the-closed-generic-method--on-the-type-"></a><a name="MT8003"/>Kapalı genel yöntem bulmak için MT8003 başarısız '\*'türünde'\*'.

### <a name="a-namemt8004mt8004-cannot-create-an-instance-of--for-the-native-object-0x-of-type--because-another-instance-already-exists-for-this-native-object-of-type-"></a><a name="MT8004"/>MT8004: bir örneği oluşturulamıyor * için yerel nesne 0 x * (tür ' *'), yerel bu nesne için başka bir örneği zaten mevcut olduğundan (tür *).

### <a name="a-namemt8005mt8005-wrapper-type--is-missing-its-native-objectivec-class-"></a><a name="MT8005"/>MT8005: Kapsayıcı türü '\*'kendi yerel ObjectiveC sınıfı eksik'\*'.

### <a name="a-namemt8006mt8006-failed-to-find-the-selector--on-the-type-"></a><a name="MT8006"/>MT8006: Seçici bulmak başarısız '\*'türünde'\*'

### <a name="a-namemt8007mt8007-cannot-get-the-method-descriptor-for-the-selector--on-the-type--because-the-selector-does-not-correspond-to-a-method"></a><a name="MT8007"/>MT8007: yöntem tanımlayıcı seçicisini alınamıyor '\*'türünde'\*', seçici bir yönteme karşılık gelmiyor

### <a name="a-namemt8008mt8008-the-loaded-version-of-xamariniosdll-was-compiled-for--bits-while-the-process-is--bits-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8008"/>MT8008: Xamarin.iOS.dll yüklenen sürümü için derlenmiş * işlemi olsa da, BITS * BITS. Lütfen http://bugzilla.xamarin.com dosyalama.

Bu bir şey yapı işleminde yanlış olduğunu gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8009mt8009-unable-to-locate-the-block-to-delegate-conversion-method-for-the-method-s-parameter--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8009"/>MT8009: Bloğu dönüştürme yöntemi yöntemi için temsilci bulunamıyor *.*' s parametre #*. Lütfen http://bugzilla.xamarin.com dosyalama.

Bu, bir API doğru bağlı değildi gösterir. Xamarin tarafından kullanıma sunulan bir API varsa lütfen bizim bugzilla dosyalama ([http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS)), bir üçüncü taraf bağlama varsa lütfen satıcısına başvurun.

### <a name="a-namemt8010mt8010-native-type-size-mismatch-between-xamariniosmacdll-and-the-executing-architecture-xamariniosmacdll-was-built-for--bit-while-the-current-process-is--bit"></a><a name="MT8010"/>MT8010: Xamarin yerel tür boyutu uyuşmazlığı. [iOS | Mac] .dll ve yürütülen mimarisi. Xamarin. [iOS | Mac] .dll için yapılmış *-bit olsa da geçerli işlem *-bit.

Bu bir şey yapı işleminde yanlış olduğunu gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8011mt8011-unable-to-locate-the-delegate-to-block-conversion-attribute-delegateproxy-for-the-return-value-for-the-method--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8011"/>MT8011: Yönteminin dönüş değeri için blok dönüştürme özniteliğinin ([DelegateProxy]) temsilciye bulunamıyor *.*. Lütfen http://bugzilla.xamarin.com dosyalama.

Xamarin.iOS (bir temsilci için bir blok dönüştürmek için) çalışma zamanında gerekli yöntemi bulamadı.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8012mt8012-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8012"/>MT8012: Dönüş değeri yöntemi için geçersiz DelegateProxyAttribute *.*: temsilci NULL'dur. Lütfen http://bugzilla.xamarin.com dosyalama.

Söz konusu yöntemi DelegateProxy özniteliği geçersiz.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8013mt8013-invalid-delegateproxyattribute-for-the-return-value-for-the-method--delegatetype-2-specifies-a-type-without-a-handler-field-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8013"/>MT8013: Dönüş değeri yöntemi için geçersiz DelegateProxyAttribute *.*: temsilci ({2}) 'İşleyicisi' alan olmadan türü belirtir. Lütfen http://bugzilla.xamarin.com dosyalama.

Söz konusu yöntemi DelegateProxy özniteliği geçersiz.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8014mt8014-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-null-please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8014"/>MT8014: Dönüş değeri yöntemi için geçersiz DelegateProxyAttribute *.*: temsilci 's ({2}) 'İşleyicisi' alanı null. Lütfen http://bugzilla.xamarin.com dosyalama.

Söz konusu yöntemi DelegateProxy özniteliği geçersiz.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8015mt8015-invalid-delegateproxyattribute-for-the-return-value-for-the-method--the-delegatetypes-2-handler-field-is-not-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8015"/>MT8015: Dönüş değeri yöntemi için geçersiz DelegateProxyAttribute *.*: temsilci 's ({2}) 'İşleyicisi' alanı bir temsilci değil, bir *. Lütfen http://bugzilla.xamarin.com dosyalama.

Söz konusu yöntemi DelegateProxy özniteliği geçersiz.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8016mt8016-unable-to-convert-delegate-to-block-for-the-return-value-for-the-method--because-the-input-isnt-a-delegate-its-a--please-file-a-bug-at-httpbugzillaxamarincom"></a><a name="MT8016"/>MT8016: Yönteminin dönüş değeri için engellemek için temsilci dönüştürülemiyor *.*, bir temsilci giriş olmadığı için bu bir *. Lütfen http://bugzilla.xamarin.com dosyalama.

Söz konusu yöntemi DelegateProxy özniteliği geçersiz.

Bu genellikle Xamarin.iOS bir hata olduğunu gösterir; Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

<!-- 8017 is used by mmp -->

### <a name="a-namemt8018mt8018-internal-consistency-error-please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8018"/>MT8018: İç tutarsızlık hatası. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8019mt8019-could-not-find-the-assembly--in-the-loaded-assemblies"></a><a name="MT8019"/>MT8019: derlemesi bulunamadı. * yüklenen derlemelerde.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8020mt8020-could-not-find-the-module-with-metadatatoken--in-the-assembly-"></a><a name="MT8020"/>MT8020: MetadataToken modülüyle bulunamadı * derlemesindeki *.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8021mt8021-unknown-implicit-token-type-"></a><a name="MT8021"/>MT8021: Bilinmeyen örtük belirteç türü: *.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8022mt8022-expected-the-token-reference--to-be-a--but-its-a--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8022"/>MT8022: belirteç başvurusu beklenen * olacak şekilde bir *, ancak bir *. Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).

### <a name="a-namemt8023mt8023-an-instance-object-is-required-to-construct-a-closed-generic-method-for-the-open-generic-method--token-reference--please-file-a-bug-report-at-httpbugzillaxamarincom"></a><a name="MT8023"/>MT8023: Açık genel yöntem için kapalı bir genel yöntem oluşturmak için bir örnek nesne gereklidir: * (simge başvuru: *). Lütfen http://bugzilla.xamarin.com sırasında bir hata raporu dosya.

Xamarin.iOS bir hata gösterir. Lütfen en dosyalama [http://bugzilla.xamarin.com](https://bugzilla.xamarin.com/enter_bug.cgi?product=iOS).
