---
title: Xamarin.Mac hata iletileri (mmp)
description: Bu belge mmp, yürütülebilir bir Mac uygulama derlenmiş derlemelerine paketlemek için kullanılan aracı tarafından oluşturulan hataları listeler.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5B26339F-A202-4E41-9229-D0BC9E77868E
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/27/2018
ms.openlocfilehash: f5cf4a9003d3fb468ffcf337c33730cb12238c44
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792765"
---
# <a name="xamarinmac-error-messages-mmp"></a>Xamarin.Mac hata iletileri (mmp)

## <a name="mm0xxx-mmp-error-messages"></a>MM0xxx: mmp hata iletileri

Örneğin Parametreler, ortam, Araçlar eksik.

<a name="MM0000" />

#### <a name="mm0000-unexpected-error---please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM0000: Beklenmeyen bir hata - Lütfen dosyası bir hata raporu adresindeki http://bugzilla.xamarin.com

Beklenmeyen bir hata durumu oluştu. Lütfen [bir hata raporu dosya](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) mümkün olduğunca fazla bilgi ile de dahil olmak üzere:

* Tam ile en fazla ayrıntı günlükleri, yapı (ör `-v -v -v -v` içinde **ek mmp bağımsız değişkenleri**);
* Hatayı yeniden oluşturmaya en az test durumu; ve
* Tüm sürüm bilgileri

Tam sürüm bilgilerini almak için en kolay yolu kullanmaktır **Xamarin Studio** menüsünde **hakkında Xamarin Studio** öğesi, **ayrıntıları göster** düğmesine tıklayın ve sürüm kopyalayıp yapıştırın bilgiler (kullanabileceğiniz **kopyalama bilgi** düğmesi).

<a name="MM0001" />

#### <a name="mm0001-this-version-of-xamarinmac-requires-mono-0-the-current-mono-version-is-1-please-update-the-monoframework-from-httpmono-projectcomdownloads"></a>MM0001: Xamarin.Mac bu sürümü Mono gerektirir {0} (geçerli Mono sürümü {1}). Lütfen gelen Mono.framework güncelleştirin http://mono-project.com/Downloads

<a name="MM0003" />

#### <a name="mm0003-application-name-0exe-conflicts-with-an-sdk-or-product-assembly-dll-name"></a>MM0003: Uygulama adı '{0}.exe' bir SDK veya ürün derlemesi (.dll) adıyla çakışıyor.

<a name="MM0007" />

#### <a name="mm0007-the-root-assembly-0-does-not-exist"></a>MM0007: Kök derleme '{0}' mevcut değil

<a name="MM0008" />

#### <a name="mm0008-you-should-provide-one-root-assembly-only-found-0-assemblies-1"></a>MM0008: Bir kök derleme yalnızca, bulunan sağlamalıdır {0} derlemeler: '{1}'

<a name="MM0010" />

#### <a name="mm0010-could-not-parse-the-command-line-arguments-0"></a>MM0010: komut satırı bağımsız değişkenleri ayrıştırılamadı: {0}

<!-- 0013 is unused -->

<a name="MM0016" />

#### <a name="mm0016-the-option-0-has-been-deprecated"></a>MM0016: Seçeneği '{0}' kullanım dışı bırakıldı.

<a name="MM0017" />

#### <a name="mm0017-you-should-provide-a-root-assembly"></a>MM0017: Kök derleme sağlamalıdır

<a name="MM0018" />

#### <a name="mm0018-unknown-command-line-argument-0"></a>MM0018: Bilinmeyen komut satırı bağımsız değişken: '{0}'

<a name="MM0020" />

#### <a name="mm0020-the-valid-options-for-0-are-1"></a>MM0020: İçin geçerli seçenek '{0}'olan'{1}'.

<a name="MM0023" />

#### <a name="mm0023-application-name-0exe-conflicts-with-another-user-assembly"></a>MM0023: Uygulama adı '{0}.exe' başka bir kullanıcı derlemesi ile çakışıyor.

<a name="MM0026" />

#### <a name="mm0026-could-not-parse-the-command-line-argument-0-1"></a>MM0026: komut satırı bağımsız değişkeni ayrıştırılamadı '{0}': {1}

<a name="MM0043" />

#### <a name="mm0043-the-boehm-garbage-collector-is-not-supported-the-sgen-garbage-collector-has-been-selected-instead"></a>MM0043: Boehm atık toplayıcı desteklenmiyor. SGen atık toplayıcı yerine seçilmedi.

<a name="MM0050" />

#### <a name="mm0050-you-cannot-provide-a-root-assembly-if---no-root-assembly-is-passed"></a>MM0050: Hayır kök derleme aktarılırsa--kök derleme sağlayamaz.

<a name="MM0051" />

#### <a name="mm0051-an-output-directory---output-is-required-if---no-root-assembly-is-passed"></a>MM0051: Bir çıkış dizini (--çıktı) Hayır kök derleme aktarılırsa--gereklidir.

<a name="MM0053" />

#### <a name="mm0053-cannot-disable-new-refcount-with-the-unified-api"></a>MM0053: Unified API ile yeni refcount devre dışı bırakılamıyor.

<a name="MM0056" />

#### <a name="mm0056-cannot-find-xcode-in-any-of-our-default-locations-please-install-xcode-or-pass-a-custom-path-using---sdkrootpath"></a>MM0056: Xcode bizim varsayılan konumlardan herhangi birinde bulunamıyor. Xcode yüklemek ya da--sdkroot kullanarak özel bir yol geçirmek =<path>

<a name="MM0059" />

#### <a name="mm0059-could-not-find-the-currently-selected-xcode-on-the-system-0"></a>MM0059: şu anda seçili Xcode sistemde bulunamadı: {0};

<a name="MM0060" />

#### <a name="mm0060-could-not-find-the-currently-selected-xcode-on-the-system-xcode-select---print-path-returned-0-but-that-directory-does-not-exist"></a>MM0060: şu anda seçili Xcode sistemde bulunamadı. 'xcode döndürülen Seç--yazdırma yolu' '{0}', ancak bu dizin mevcut değil.

<a name="MM0068" />

#### <a name="mm0068-invalid-value-for-target-framework-0"></a>MM0068: Hedef framework için geçersiz değer: {0}.

<a name="MM0071" />

#### <a name="mm0071-unknown-platform--this-usually-indicates-a-bug-in-xamarinmac-please-file-a-bug-report-at-httpsbugzillaxamarincom-with-a-test-case"></a>MM0071: Bilinmeyen platform: *. Bu genellikle Xamarin.Mac bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya https://bugzilla.xamarin.com bir test çalışması ile.

Bu genellikle Xamarin.Mac bir hata olduğunu gösterir; Lütfen sırasında bir hata raporu dosya [ https://bugzilla.xamarin.com ](https://bugzilla.xamarin.com/enter_bug.cgi?product=Xamarin.Mac) bir test çalışması ile.

<a name="MM0079" />

#### <a name="mm0079-internal-error---no-executable-was-copied-into-the-app-bundle-please-contact-supportxamarincom"></a>MM0079: İç hata - hiçbir yürütülebilir uygulama kümeye kopyalandı. Lütfen başvurun 'support@xamarin.com'

<a name="MM0080" />

#### <a name="mm0080-disabling-newrefcount---new-refcountfalse-is-deprecated"></a>MM0080: NewRefCount devre dışı bırakma,--yeni-refcount:false, kullanım dışıdır.

<!-- 0088 used by mtouch -->
<!-- 0089 used by mtouch -->

<a name="MM0091" />

#### <a name="mm0091-this-version-of-xamarinmac-requires-the--sdk-shipped-with-xcode--either-upgrade-xcode-to-get-the-required-header-files-or-use-the-dynamic-registrar-or-set-the-managed-linker-behaviour-to-link-platform-or-link-framework-sdks-only-to-try-to-avoid-the-new-apis"></a>MM0091: Bu sürümü Xamarin.Mac gerektirir * SDK (Xcode ile birlikte gelen *). Ya da gerekli başlık dosyaları almak veya dinamik kayıt kullanın veya yönetilen bağlayıcı davranışa bağlantı Platform veya yalnızca bağlantı Framework SDK'ları için (yeni API'leri önlemek) ayarlamak için Xcode yükseltin.

Xamarin.Mac başlık dosyalarından statik kayıt şirketi ile uygulamanızı hata iletisinde belirtilen SDK sürümü gerektirir. Bu hatayı düzeltmek için önerilen yöntem gerekli SDK almak için Xcode yükseltmek için bu tüm gerekli başlık dosyaları dahil eder. Yüklü Xcode birden çok sürümü veya varsayılan olmayan bir konumda bir Xcode kullanmak istiyorsanız, IDE'nin tercihlerinde Xcode konumun doğru ayarladığınızdan emin olun.

Bir alternatif, olası çözüm olan yönetilen bağlayıcı etkinleştirmek için. Bu, kullanılmayan API, çoğu durumda, üst bilgi dosyaları eksik (veya tamamlanmamış) nerede yeni API dahil kaldırır. Ancak projeniz olandan, Xcode daha yeni bir SDK sürümünde sunulan API kullanıyorsa bu çalışmaz sağlar.

İkinci olası, alternatif bir çözüm olan dinamik kayıt bunun yerine kullanın. Bu, dinamik olarak türleri kaydederek bir başlangıç maliyeti zorunlu tuttukları ancak üstbilgi dosyası gereksinimini kaldırmak. 

Son straw çözüm Xamarin.Mac daha eski bir sürümü kullanmak olabilir, projenize SDK destekleyen bir gerektirir.

<a name="MM0097" />

#### <a name="mm0097-machineconfig-file-0-can-not-be-found"></a>MM0097: machine.config dosyasının '{0}' bulunamadı.

<a name="MM0098" />

#### <a name="mm0098-aot-compilation-is-only-available-on-unified"></a>MM0098: Uygulama Nesne AĞACI derleme yalnızca Unified üzerinde kullanılabilir

<a name="MM0099" />

#### <a name="mm0099-internal-error-0-please-file-a-bug-report-with-a-test-case-httpbugzillaxamarincom"></a>MM0099: İç hata {0}. Lütfen bir test çalışması ile bir hata raporu dosya (http://bugzilla.xamarin.com).

<a name="MM0114" />

#### <a name="mm0114-hybrid-aot-compilation-requires-all-assemblies-to-be-aot-compiled"></a>MM0114: Karma uygulama nesne AĞACI derleme tüm derlemelerde derlenmiş uygulama nesne AĞACI olmasını gerektirir.

## <a name="mm1xxx-file-copy--symlinks-project-related"></a>MM1xxx: dosya kopyalama / simgesel bağlantı (Proje ilgili)

<a name="MM1034" />

#### <a name="mm1034-could-not-create-symlink-file---target-error-number"></a>MM1034: simgesel oluşturulamadı '{} dosyası' -> '{hedef}': hata {number}

### <a name="mm14xx-product-assemblies"></a>MM14xx: Ürün derlemeler

<a name="MM1401" />

#### <a name="mm1401-the-required-0-assembly-is-missing-from-the-references"></a>MM1401: Gerekli '{0}' derleme başvuruları eksik

<a name="MM1402" />

#### <a name="mm1402-the-assembly-0-is-not-compatible-with-this-tool"></a>MM1402: Derleme '{0}' Bu aracı ile uyumlu değil

<a name="MM1403" />

#### <a name="mm1403-0-1-could-not-be-found-target-framework-0-is-unusable-to-package-the-application"></a>MM1403: {0} '{1}' bulunamadı. Hedef çerçevesi '{0}' uygulama paketi için kullanılamaz.

<a name="MM1404" />

#### <a name="mm1404-target-framework-0-is-invalid"></a>MM1404: Hedef Framework'ü '{0}' geçersiz.

<a name="MM1405" />

#### <a name="mm1405-usefullxammacframework-must-always-target-framework-net-45-not-0-which-is-invalid"></a>MM1405: useFullXamMacFramework her zaman framework .NET 4.5, hedeflemelidir değil '{0}' geçersiz

<a name="MM1406" />

#### <a name="mm1406-target-framework-0-is-invalid-when-targetting-xamarinmac-45-net-framwork"></a>MM1406: Hedef Framework'ü '{0}' geçersiz olduğunda atamak Xamarin.Mac 4.5 .NET çerçevesinin.

<a name="MM1407" />

#### <a name="mm1407-mismatch-between-xamarinmac-reference-0-and-target-framework-selected-1"></a>MM1407: Xamarin.Mac başvuru arasında uyuşmazlık '{0}'ve seçilen hedef çerçevesi'{1}'.

### <a name="mm15xx-assembly-gathering-not-requiring-linker-errors"></a>MM15xx: Derleme (Bağlayıcı gerektirmeyen) toplama hataları

<a name="MM1501" />

#### <a name="mm1501-can-not-resolve-reference-0"></a>MM1501: başvuru çözümleyemiyor: {0}

### <a name="machocs"></a>MachO.cs

<a name="MM1600" />

#### <a name="mm1600-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1600: Olmayan bir makine-O dinamik kitaplığı (Bilinmeyen üstbilgi ' 0 x{0}'): {1}.

<a name="MM1601" />

#### <a name="mm1601-not-a-static-library-unknown-header-0-1"></a>MM1601: Olmayan bir statik kitaplık (Bilinmeyen üstbilgi '{0}'): {1}.

<a name="MM1602" />

#### <a name="mm1602-not-a-mach-o-dynamic-library-unknown-header-0x0-1"></a>MM1602: Olmayan bir makine-O dinamik kitaplığı (Bilinmeyen üstbilgi ' 0 x{0}'): {1}.

<a name="MM1603" />

#### <a name="mm1603-unknown-format-for-fat-entry-at-position-0-in-1"></a>MM1603: Konumundaki fat girişi bilinmeyen biçimi {0} içinde {1}.

<a name="MM1604" />

#### <a name="mm1604-file-of-type-0-is-not-a-macho-file-1"></a>MM1604: Dosya türü {0} MachO dosyası değil ({1}).

## <a name="mm2xxx-linker"></a>MM2xxx: Linker

### <a name="mm20xx-linker-general-errors"></a>MM20xx: Bağlayıcı (Genel) hataları

<a name="MM2001" />

#### <a name="mm2001-could-not-link-assemblies"></a>MM2001: derlemeleri bağlanamadı

<a name="MM2002" />

#### <a name="mm2002-can-not-resolve-reference-0"></a>MM2002: başvuru çözümleyemiyor: {0}

<a name="MM2003" />

#### <a name="mm2003-option-0-will-be-ignored-since-linking-is-disabled"></a>MM2003: Seçenek '{0}' bağlama devre dışı olduğundan göz ardı edilir

<a name="MM2004" />

#### <a name="mm2004-extra-linker-definitions-file-0-could-not-be-located"></a>MM2004: Ek bağlayıcı tanımları dosya '{0}' bulunamadı.

<a name="MM2005" />

#### <a name="mm2005-definitions-from-0-could-not-be-parsed"></a>MM2005: Tanımları '{0}' ayrıştırılamadı.

<a name="MM2006" />

#### <a name="mm2006-native-library-0-was-referenced-but-could-not-be-found"></a>MM2006: Yerel Kitaplığı '{0}' başvuruldu ancak öğe bulunamadı.

<a name="MM2007" />

#### <a name="mm2007-xamarinmac-unified-api-against-a-full-net-profile-does-not-support-linking-pass-the--nolink-flag"></a>MM2007: Xamarin.Mac Unified API tam bir .NET profili karşı bağlama desteklemez. -Nolink bayrağı geçirin.

<a name="MM2009" />

#### <a name="mm2009-referenced-by-01------this-message-is-related-to-mm2006-"></a>MM2009: başvurduğu {0}.{1}     ** Bu iletiyi MM2006 için ilgili **

<a name="MM2010" />

#### <a name="mm2010-unknown-httpmessagehandler-0-valid-values-are-httpclienthandler-default-cfnetworkhandler-or-nsurlsessionhandler"></a>MM2010: Bilinmeyen HttpMessageHandler `{0}`. HttpClientHandler (varsayılan), CFNetworkHandler veya NSUrlSessionHandler geçerli değerler:

<a name="MM2011" />

#### <a name="mm2011-unknown-tlsprovider-0--valid-values-are-default-or-appletls"></a>MM2011: Bilinmeyen TLSProvider '{0}.  Varsayılan veya appletls geçerli değerler:

<a name="MM2012" />

#### <a name="mm2012-only-first-0-of-1-referenced-by-warnings-shown--this-message-related-to-2009-"></a>MM2012: Yalnızca ilk {0} , {1} "gösterilen uyarıları başvurduğu". ** Bu iletiyi ilgili 2009 olarak \*\*

<a name="MM2013" />

#### <a name="mm2013-failed-to-resolve-the-reference-to-0-referenced-in-1-the-app-will-not-include-the-referenced-assembly-and-may-fail-at-runtime"></a>MM2013: başvuru çözülemedi "{0}", başvurulan"{1}". Uygulama başvurulan derlemeyi içermez ve çalışma zamanında başarısız olabilir.

<a name="MM2014" />

#### <a name="mm2014-xamarinmac-extensions-do-not-support-linking-request-for-linking-will-be-ignored--this-message-is-obsolete-in-xm-36-"></a>MM2014: Xamarin.Mac uzantıları bağlamayı desteklemez. Bağlama yoksayılacak isteği. ** Bu iletiyi XM içinde 3.6 + kullanılmıyor \*\*

<!-- 2015 used by mtouch -->

<a name="MM2016" />

#### <a name="mm2016-invalid-tlsprovider-0-option-the-only-valid-value-1-will-be-used"></a>MM2016: Geçersiz TlsProvider `{0}` seçeneği. Tek geçerli değer `{1}` kullanılır.

<a name="MM2017" />

#### <a name="mm2017-could-not-process-xml-description-0"></a>MM2017: XML açıklama işleyemedi: {0}

<a name="MM202x" />

#### <a name="mm202x-binding-optimizer-failed-processing-"></a>MM202x: Bağlama iyileştirici işlenemedi `...`.

<a name="MM2100" />

#### <a name="mm2100-xamarinmac-classic-api-does-not-support-platform-linking"></a>MM2100: Xamarin.Mac Klasik API Platform bağlama desteklemez.

<a name="MM2103" />

#### <a name="mm2103-error-processing-assembly--"></a>MM2103: Hata işleme derleme '\*': *

Bir derlemeyi işlenirken beklenmeyen bir hata oluştu.

Soruna neden bütünleştirilmiş hata iletisinde adlandırılmıştır. Derleme bu sorunu gidermek için de sağlanması gerekir bir [hata raporu](https://bugzilla.xamarin.com) etkin ayrıntı düzeyine sahip bir tam derleme günlüğü birlikte (yani `-v -v -v -v` içinde **ek mtouch bağımsız değişkenleri**).

<a name="MM2104" />

#### <a name="mm2104-unable-to-link-assembly-0-as-it-is-mixed-mode"></a>MM2104: Derleme bağlantı kurulamıyor '{0}' karma mod olduğu gibi.

Karışık mod derlemeleri bağlayıcı tarafından işlenemez.

Bkz: https://msdn.microsoft.com/library/x0w2664k.aspx karma mod derlemeler hakkında daha fazla bilgi için.

## <a name="mm3xxx-aot"></a>MM3xxx: Uygulama Nesne AĞACI

### <a name="mm30xx-aot-general-errors"></a>MM30xx: Uygulama Nesne AĞACI (Genel) hataları

<a name="MM3001" />

#### <a name="mm3001-could-not-aot-the-assembly-0"></a>MM3001: AOT derlemesi yüklenemedi '{0}'

<!-- 3002 used by mtouch -->
<!-- 3003 used by mtouch -->
<!-- 3004 used by mtouch -->
<!-- 3005 used by mtouch -->
<!-- 3006 used by mtouch -->
<!-- 3007 used by mtouch -->
<!-- 3008 used by mtouch -->

<a name="MM3009" />

#### <a name="mm3009-aot-of-0-was-requested-but-was-not-found"></a>MM3009: Uygulama Nesne AĞACI, '{0}' istendi ancak bulunamadı

<a name="MM3010" />

#### <a name="mm3010-exclusion-of-aot-of-0-was-requested-but-was-not-found"></a>MM3010: Uygulama Nesne AĞACI hariç '{0}' istendi ancak bulunamadı

## <a name="mm4xxx-code-generation"></a>MM4xxx: kod oluşturma

### <a name="mm40xx-driverm"></a>MM40xx: driver.m

<a name="MM4001" />

#### <a name="mm4001-the-main-template-could-not-be-expanded-to-0"></a>MM4001: Ana şablon için genişletilemedi `{0}`.

### <a name="mm41xx-registrar"></a>MM41xx: Kaydedici

<a name="MM4134" />

#### <a name="mm4134-your-application-is-using-the-0-framework-which-isnt-included-in-the-macos-sdk-youre-using-to-build-your-app-this-framework-was-introduced-in-osx-2-while-youre-building-with-the-macos-1-sdk-this-configuration-is-not-supported-with-the-static-registrar-pass---registrardynamic-as-an-additional-mmp-argument-in-your-projects-mac-build-option-to-select-alternatively-select-a-newer-sdk-in-your-apps-mac-build-options"></a>MM4134: Uygulamanızı kullanarak '{0}' MacOS ve uygulamanızı oluşturmak için kullandığınız SDK'ın dahil edilmemiştir framework (Bu çerçeve OSX sunulan {2}ile MacOS oluşturmakta olduğunuz yaparken {1} SDK.) Bu yapılandırma, statik kayıt şirketi (pass--Kaydedici: dinamik seçeneğinde seçmek için projenizin Mac yapı ek mmp bağımsız değişken olarak) ile desteklenmiyor. Alternatif olarak daha yeni bir SDK'sı, uygulamanızın Mac derleme seçenekleri belirleyin.

## <a name="mm5xxx-gcc-and-toolchain"></a>MM5xxx: GCC ve araç zinciri

### <a name="mm51xx-compilation"></a>MM51xx: derleme

<a name="MM5101" />

#### <a name="mm5101-missing-0-compiler-please-install-xcode-command-line-tools-component"></a>MM5101: Eksik '{0}' derleyici. Lütfen 'Komut satırı araçları' bileşenini yükleyin.

<!-- 5102 used by mtouch -->

<a name="MM5103" />

#### <a name="mm5103-failed-to-compile-error-code---0-please-file-a-bug-report-at-httpbugzillaxamarincom"></a>MM5103: derlenemedi. Hata kodu - {0}. Lütfen sırasında bir hata raporu dosya http://bugzilla.xamarin.com

<!-- 5104 used by mtouch -->

### <a name="mm52xx-linking"></a>MM52xx: bağlama

<a name="MM5202" />

#### <a name="mm5202-monoframework-mdk-is-missing-please-install-the-mdk-for-your-monoframework-version-from-httpmono-projectcomdownloads"></a>MM5202: Mono.framework MDK eksik. Lütfen Mono.framework sürümünden MDK yükleyin http://mono-project.com/Downloads

<a name="MM5203" />

#### <a name="mm5203-cant-find-libxammaca-likely-because-of-a-corrupted-xamarinmac-installation-please-reinstall-xamarinmac"></a>MM5203: libxammac.a, olası bir bozuk Xamarin.Mac yüklemesi nedeniyle bulunamıyor. Lütfen Xamarin.Mac yeniden yükleyin.

<a name="MM5204" />

#### <a name="mm5204-invalid-architecture-x8664-is-only-supported-on-non-classic-profiles"></a>MM5204: Geçersiz mimarisi. x86_64 yalnızca klasik olmayan profilleri desteklenir.

<a name="MM5205" />

#### <a name="mm5205-invalid-architecture-0-valid-architectures-are-i386-and-x8664-when---profilemobile"></a>MM5205: Geçersiz mimarisi '{0}'. Geçerli mimarileri: i386 ve x86_64 (zaman--profil mobil =).

<a name="MM5218" />

#### <a name="mm5218-cant-ignore-the-dynamic-symbol-symbol---ignore-dynamic-symbolsymbol-because-it-was-not-detected-as-a-dynamic-symbol"></a>MM5218: dinamik simgenin {simgesi} yoksayamaz (--Yoksay-dinamik-simge = {simgesi}) dinamik bir simge bulunamadığından.

Bkz: [eşdeğer mtouch uyarı](~/ios/troubleshooting/mtouch-errors.md#MT5218).

<!-- 5206 used by mtouch -->
<!-- 5207 used by mtouch -->
<!-- 5208 used by mtouch -->
<!-- 5209 used by mtouch -->
<!-- 5210 used by mtouch -->
<!-- 5211 used by mtouch -->
<!-- 5212 used by mtouch -->
<!-- 5213 used by mtouch -->
<!-- 5214 used by mtouch -->
<!-- 5215 used by mtouch -->
<!-- 5216 used by mtouch -->
<!-- 5217 used by mtouch -->

### <a name="mm53xx-other-tools"></a>MM53xx: diğer araçları

<a name="MM5301" />

#### <a name="mm5301-pkg-config-could-not-be-found-please-install-the-monoframework-from-httpmono-projectcomdownloads"></a>MM5301: pkg-config bulunamadı. Lütfen gelen Mono.framework yükleyin http://mono-project.com/Downloads

<!-- 5302 used by mtouch -->
<!-- 5303 used by mtouch -->
<!-- 5304 used by mtouch -->

<a name="MM5305" />

#### <a name="mm5305-missing-otool-tool-please-install-xcode-command-line-tools-component"></a>MM5305: Eksik 'otool' aracı. Lütfen 'Komut satırı araçları' bileşeni yükleyin

<a name="MM5306" />

#### <a name="mm5306-missing-dependencies-please-install-xcode-command-line-tools-component"></a>MM5306: bağımlılıkları eksik. Lütfen 'Komut satırı araçları' bileşeni yükleyin

<a name="MM5308" />

#### <a name="mm5308-xcode-license-agreement-may-not-have-been-accepted--please-launch-xcode"></a>MM5308: Xcode lisans sözleşmesini kabul.  Xcode başlatın.

<a name="MM5309" />

#### <a name="mm5309-native-linking-failed-with-error-code-1--check-build-log-for-details"></a>MM5309: Yerel bağlama 1 hata koduyla başarısız oldu.  Ayrıntılar için yapı günlüğünü denetleyin.

<a name="MM5310" />

#### <a name="mm5310-installnametool-failed-with-an-error-code-0-check-build-log-for-details"></a>MM5310: install_name_tool hata koduyla başarısız oldu '{0}'. Ayrıntılar için yapı günlüğünü denetleyin.

<!-- MM6xxx: mmp internal tools -->
<!-- MM7xxx: reserved -->

## <a name="mm8xxx-runtime"></a>MM8xxx: çalışma zamanı

### <a name="mm800x-misc"></a>MM800x: diğer

<!-- 8000 used by mtouch -->
<!-- 8001 used by mtouch -->
<!-- 8002 used by mtouch -->
<!-- 8003 used by mtouch -->
<!-- 8004 used by mtouch -->
<!-- 8005 used by mtouch -->
<!-- 8006 used by mtouch -->
<!-- 8007 used by mtouch -->
<!-- 8008 used by mtouch -->
<!-- 8009 used by mtouch -->
<!-- 8010 used by mtouch -->
<!-- 8011 used by mtouch -->
<!-- 8012 used by mtouch -->
<!-- 8013 used by mtouch -->
<!-- 8014 used by mtouch -->
<!-- 8015 used by mtouch -->
<!-- 8016 used by mtouch -->

<a name="MM8017" />

#### <a name="mm8017-the-boehm-garbage-collector-is-not-supported-please-use-sgen-instead"></a>MM8017: Boehm atık toplayıcı desteklenmiyor. Lütfen SGen kullanın.

