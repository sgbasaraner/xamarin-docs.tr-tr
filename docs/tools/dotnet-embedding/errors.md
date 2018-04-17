---
title: .NET hataları katıştırma
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 04/11/2018
ms.openlocfilehash: c7e3e05ebd429573bf5ad4e1da1af333b897efef
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="net-embedding-errors"></a>.NET hataları katıştırma

## <a name="em0xxx-binding-error-messages"></a>EM0xxx: hata iletileri bağlama

Örneğin Parametreler, ortamı

<!-- 0xxx: the generator itself, e.g. parameters, environment -->

<a name="EM0000" />

### <a name="em0000-unexpected-error---please-fill-a-bug-report-at-httpsgithubcommonoembeddinator-4000issues"></a>EM0000: Beklenmeyen bir hata - Lütfen sırasında bir hata raporu doldurun https://github.com/mono/Embeddinator-4000/issues

Beklenmeyen bir hata durumu oluştu. Lütfen [bir sorun dosya](https://github.com/mono/Embeddinator-4000/issues) mümkün olduğunca fazla bilgi ile de dahil olmak üzere:

* Tam günlükleri, en fazla ayrıntı ile derleme
* Hatayı yeniden oluşturmaya en az bir test çalışması
* Tüm sürüm bilgileri

Tam sürüm bilgilerini almak için en kolay yolu kullanmaktır **Xamarin Studio** menüsünde **hakkında Xamarin Studio** öğesi, **ayrıntıları göster** düğmesine tıklayın ve sürüm kopyalayıp yapıştırın bilgiler (kullanabileceğiniz **kopyalama bilgi** düğmesi).

<a name="EM0001" />

### <a name="em0001-could-not-create-output-directory-x"></a>EM0001: Çıktı dizini oluşturulamadı `X`

Tarafından belirtilen dizin adı `-o=DIR` yok ve oluşturulamadı. Dosya sistemi için geçersiz bir ad olabilir.

<a name="EM0002" />

### <a name="em0002-option-x-is-not-supported"></a>EM0002: Seçeneğini `X` desteklenmiyor

Aracı seçeneğini desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<a name="EM0003" />

### <a name="em0003-the-platform-x-is-not-valid"></a>EM0003: Platform `X` geçerli değil.

Aracı platformu desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<a name="EM0004" />

### <a name="em0004-the-target-x-is-not-valid"></a>EM0004: Hedef `X` geçerli değil.

Aracı hedef desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<a name="EM0005" />

### <a name="em0005-the-compilation-target-x-is-not-valid"></a>EM0005: Derleme hedef `X` geçerli değil.

Aracı derleme hedef desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<a name="EM0006" />

### <a name="em0006-could-not-find-the-xcode-location"></a>EM0006: Xcode konumunu bulamıyor.

Aracı konumu şu anda seçili Xcode kullanarak bulunamadı `xcode-select -p` komutu. Bu komutun başarılı ve Xcode konumun doğru döndürür doğrulayın.

<a name="EM0007" />

### <a name="em0007-could-not-get-the-sdk-version-for-sdk"></a>EM0007: '{sdk}' için sdk sürümü alınamadı.

Aracı'nı kullanarak SDK sürümü alınamadı `xcrun --show-sdk-version --sdk {sdk}` komutu. Bu komutun başarılı ve SDK sürümü döndürür doğrulayın.

<a name="EM0008" />

### <a name="em0008-the-architecture-arch-is-not-valid-for-platform-valid-architectures-for-platform-are-architectures"></a>EM0008: '{arch}' mimarisi {platformu} için geçerli değil. {Platform} için geçerli mimariler şunlardır: '{mimarileri}'.

Hata iletisinde mimarisi hedeflenen platformu için geçerli değil. Lütfen geçerli bir mimari--ABI seçeneği geçirilir doğrulayın.

<a name="EM0009" />

### <a name="em0009-the-feature-x-is-not-currently-implemented-by-the-generator"></a>EM0009: Özellik `X` Oluşturucu tarafından uygulanmaz.

Bu oluşturucunun sonraki bir sürümde düzeltme düşündüğünüz bilinen bir sorundur. Katkılar Hoş Geldiniz.

<a name="EM0010" />

### <a name="em0010-cant-merge-the-frameworks-simulatorframework-and-deviceframework-because-the-file-file-exists-in-both"></a>EM0010: çerçeveler '{simulatorFramework}' ve '{deviceFramework}' dosyası ' {}' hem de varolduğundan birleştirilemez.

Olmadığı için ortak bir dosya aralarında aracı hata iletisinde çerçeveleri birleştirilemedi.

Bu eklenmesinde bir hata Embeddinator 4000 gösterebilir; Lütfen sırasında bir hata raporu dosya [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) bir test çalışması ile.

<a name="EM0011" />

### <a name="em0011-the-assembly-x-does-not-exist"></a>EM0011: Derleme `X` yok.

Araç derlemesi bulunamadı `X` değişkenlerinde belirtilen.

<a name="EM0012" />

### <a name="em0012-the-assembly-name-x-is-not-unique"></a>EM0012: Derleme adı `X` benzersiz değil

Sağlanan birden fazla derleme varsa aynı, iç adı ve çalışma zamanında bunları ayırt etmek mümkün olmayacaktır.

En olası nedeni, bir derleme birden çok kez komut satırı bağımsız değişkenleri üzerinde belirtilen ' dir. Ancak özgün adı ve birden çok kopya olan bir adlandırılmış derleme hala tutar olamaz Kerberos'ta.

<a name="EM0013" />

### <a name="em0013-cant-find-the-assembly-x-referenced-by-y"></a>EM0013: 'X', 'Y' tarafından başvurulan derleme bulunamıyor.

Aracı 'X', 'Y' derlemesi tarafından başvurulan derleme bulunamadı. Lütfen tüm başvurulan derlemeler bağlanması için derleme ile aynı dizinde olduğundan emin olun.

<a name="EM0014" />

### <a name="em0014-could-not-find-product-product-minversion-is-required"></a>EM0014: {ürün} bulunamadı ({ürün} {min_version} gereklidir).

Hata iletisinde belirtilen bağımlılık sistemde bulunamadı.

<a name="EM0015" />

### <a name="em0015-could-not-find-a-valid-version-of-product-found-version-but-at-least-minversion-is-required"></a>EM0015: {ürünün} geçerli bir sürümü bulunamadı ({version} bulundu, ancak en az {min_version} gereklidir).

Bağımlılık hata iletisi sistemde bulundu, ancak çok eski olduğundan belirtiliyor. Lütfen yeni bir sürüme güncelleştirin.

<a name="EM0016" />

### <a name="em0016-could-not-create-symlink-file---target-error-number"></a>EM0016: simgesel oluşturulamadı '{} dosyası' -> '{hedef}': hata {number}

Hata iletisinde belirtilen simgesel oluşturulamadı.

<a name="EM0026" />

### <a name="em0026-could-not-parse-the-command-line-argument-a-"></a>EM0026 verebilir değil komut satırı bağımsız değişkeni 'A' ayrıştırılamadı: *

Verilen komut satırı seçeneği söz dizimi `A` aracı tarafından ayrıştırılamadı. Büyük olasılıkla yanlış, belge veya Yardım doğru sözdizimi için gözden geçirin.

<a name="EM0099" />

### <a name="em0099-internal-error--please-file-a-bug-report-with-a-test-case-httpsgithubcommonoembeddinator-4000issues"></a>EM0099: İç hata *. Lütfen bir test çalışması ile bir hata raporu dosya (https://github.com/mono/Embeddinator-4000/issues).

Embeddinator 4000 içinde bir iç tutarsızlık denetimi başarısız olduğunda bu hata iletisini bildirilir.

Bu eklenmesinde bir hata Embeddinator 4000 gösterir; Lütfen sırasında bir hata raporu dosya [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) bir test çalışması ile.

<!-- 1xxx: code processing -->

## <a name="em1xxx-code-processing"></a>EM1xxx: Kod işleme

<a name="EM1010" />

### <a name="em1010-type-t-is-not-generated-because-x-are-not-supported"></a>EM1010: Yazın `T` çünkü oluşturulmaz `X` desteklenmez.

Bu bir **uyarı** , türü `T` göz ardı edilir (yani hiçbir şey oluşturulur) kullandığından `X`, desteklenmeyen bir özellik.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<a name="EM1011" />

### <a name="em1011-type-t-is-not-generated-because-it-lacks-marshaling-code-with-a-native-counterpart"></a>EM1011: Yazın `T` yerel karşılık gelen hazırlama koduyla eksik olduğundan oluşturulmaz.

Bu bir **uyarı** , türü `T` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü bir ek hazırlama gerektiren .NET framework kullanıma sunar.

Not: Bu, bazı kısıtlamalarla aracı gelecek bir sürümünde desteklenen bir şeydir.

<a name="EM1020" />

### <a name="em1020-constructor-c-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1020: Oluşturucusu `C` parametre türü nedeniyle oluşturulmaz `T` desteklenmiyor.

Bu bir **uyarı** , Oluşturucusu `C` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü türünde bir parametre `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<a name="EM1021" />

### <a name="em1021-constructor-c-has-default-values-for-which-no-wrapper-is-generated"></a>EM1021: Oluşturucusu `C` hiçbir sarmalayıcı oluşturulur varsayılan değerlere sahip.

Bu bir **uyarı** , Oluşturucusu varsayılan parametreleri `C` fazladan kod üretme değil. Varolan bir yöntemi zaten aynı imzaya sahip en yaygın neden olur. Örneğin .NET içinde olması mümkündür:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

Oluşturulan bu gibi durumlarda yalnızca iki `init` Seçici oluşturuldu, her iki arama mono, içine ancak sonraki için hiçbir sarmalayıcı mevcut.

<a name="EM1030" />

### <a name="em1030-method-m-is-not-generated-because-return-type-t-is-not-supported"></a>EM1030: Yöntemi `M` çünkü oluşturulmaz dönüş türü `T` desteklenmiyor.

Bu bir **uyarı** , yöntem `M` göz ardı edilir (yani hiçbir şey oluşturulur), dönüş türü olduğundan `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<a name="EM1031" />

### <a name="em1031-method-m-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1031: Yöntemi `M` parametre türü nedeniyle oluşturulmaz `T` desteklenmiyor.

Bu bir **uyarı** , yöntem `M` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü türünde bir parametre `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<a name="EM1032" />

### <a name="em1032-method-m-has-default-values-for-which-no-wrapper-is-generated"></a>EM1032: Yöntemi `M` hiçbir sarmalayıcı oluşturulur varsayılan değerlere sahip.

Bu bir **uyarı** , yönteminin varsayılan parametreleri `M` fazladan kod üretme değil. Varolan bir yöntemi zaten aynı imzaya sahip en yaygın neden olur. Örneğin .NET içinde olması mümkündür:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

Oluşturulan bu gibi durumlarda yalnızca iki `increment` Seçici oluşturuldu, her iki arama mono, içine ancak sonraki için hiçbir sarmalayıcı mevcut.

<a name="EM1033" />

### <a name="em1033-method-m-is-not-generated-because-another-method-exposes-the-operator-with-a-friendly-name"></a>EM1033: Yöntemi `M` işleci kolay ada sahip başka bir yöntemi gösterir çünkü oluşturulmaz.

Bu bir **uyarı** , yöntem `M` işleci kolay ada sahip başka bir yöntemi gösterir çünkü oluşturulmaz. (https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)

<a name="EM1034" />

### <a name="em1034-extension-method-m-is-not-generated-inside-a-category-because-they-cannot-be-created-on-primitive-type-t-a-normal-static-method-was-generated"></a>EM1034: Uzantı yöntemi `M` ilkel tür üzerinde oluşturulamayacağı için bir kategori içinde oluşturulmaz `T`. Normal, statik bir yöntem oluşturuldu.

Bu bir **uyarı** bir primivite üzerinde bir genişletme yöntemi yazın (örneğin `System.Int32`) bulundu. İçinde ObjC ilkel türüne kategorileri oluşturmak mümkün değil. Bunun yerine oluşturucunun olması oluşturacak normal, statik bir yöntem.

<a name="EM1040" />

### <a name="em1040-property-p-is-not-generated-because-of-parameter-type-t-is-not-supported"></a>EM1040: Özellik `P` parametre türü nedeniyle oluşturulmaz `T` desteklenmiyor.

Bu bir **uyarı** , özellik `P` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü gösterilen türü `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<a name="EM1041" />

### <a name="em1041-indexed-properties-on-t-is-not-generated-because-multiple-indexed-properties-are-not-supported"></a>EM1041: üzerinde özellikleri dizine `T` birden çok dizin oluşturulmuş özellik desteklenmediğinden oluşturulmaz.

Bu bir **uyarı** , Dizinli Özellikler `T` göz ardı edilir (yani hiçbir şey oluşturulur) birden çok dizin oluşturulmuş özellik desteklenmediğinden.

<a name="EM1050" />

### <a name="em1050-field-f-is-not-generated-because-of-field-type-t-is-not-supported"></a>EM1050: Alan `F` alan türü nedeniyle oluşturulmaz `T` desteklenmiyor.

Bu bir **uyarı** , alan `F` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü gösterilen türü `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<a name="EM1051" />

### <a name="em1051-element-e-is-generated-instead-as-f-because-its-name-conflicts-with-an-important-objective-c-selector"></a>EM1051: Öğesi `E` yerine oluşturulan olarak `F` adını önemli bir objective-c seçicisiyle çakıştığından.

Bu bir **uyarı** , öğe `E` yerine oluşturulmayacak olarak `F` adını önemli bir objective-c seçicisiyle çakıştığından.

Seçici üzerinde [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) objective-c önemli anlama sahip ve dikkatle geçersiz kılınmalıdır.

Not: Aracı'nın yeni sürümleri ile ayrılmış Seçici listesini gelişmesi.

<a name="EM1052" />

### <a name="em1052-element-e-is-not-generated-its-name-conflicts-with-other-elements-on-the-same-class"></a>EM1052: Öğesi `E` değil oluşturulan diğer öğeleri aynı sınıfı üzerinde adıyla çakışıyor.

Bu bir **uyarı** o öğeye `E` diğer öğeleri aynı sınıfı üzerinde adıyla çakışıyor olarak oluşturulmaz.

<a name="EM1053" />

### <a name="em1053-target-e-is-not-supported-for-xamarinios-and-xamarinmac-only-the-framework-option-is-considered-supported-and-should-be-used"></a>EM1053: Hedef `E` Xamarin.iOS ve Xamarin.Mac için desteklenmiyor. Yalnızca `framework` seçeneği desteklenen ve kullanılması gereken değerlendirilir.

Bu bir **uyarı** hedefleyen `E` Xamarin.iOS ve Xamarin.Mac kullanım için desteklenmeyen olarak kabul edilir. 

Statik veya dinamik Embeddinator kitaplıkların tüketimi ek çalışma adımları veya tweaks gerektirebilir ve çoğu kullanım durumlarında kaçınılmalıdır.

Kaldırmayı düşünün, `--target` parametresi veya geçişi `--target=framework` yerine.

<!-- 2xxx: code generation -->

## <a name="em2xxx-code-generation"></a>EM2xxx: Kod oluşturma

<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
