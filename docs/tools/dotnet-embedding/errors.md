---
title: .NET katıştırma hataları
ms.prod: xamarin
ms.assetid: 932C3F0C-D968-42D1-BB14-D97C73361983
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 64caaf6610d9f9193a686d91b4731cd4d4953fa6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="em0xxx-binding-error-messages"></a>EM0xxx: bağlama hata iletileri

Örneğin Parametreler, ortamı

<!-- 0xxx: the generator itself, e.g. parameters, environment -->
<h3><a name="EM0000"/>EM0000: Beklenmeyen bir hata - Lütfen sırasında bir hata raporu doldurun https://github.com/mono/Embeddinator-4000/issues</h3>

Beklenmeyen bir hata durumu oluştu. Lütfen [bir sorun dosya](https://github.com/mono/Embeddinator-4000/issues) mümkün olduğunca fazla bilgi ile de dahil olmak üzere:

* Tam günlükleri, en fazla ayrıntı ile derleme
* Hatayı yeniden oluşturmaya en az bir test çalışması
* Tüm sürüm bilgileri

Tam sürüm bilgilerini almak için en kolay yolu kullanmaktır **Xamarin Studio** menüsünde **hakkında Xamarin Studio** öğesi, **ayrıntıları göster** düğmesine tıklayın ve sürüm kopyalayıp yapıştırın bilgiler (kullanabileceğiniz **kopyalama bilgi** düğmesi).

<h3><a name="EM0001"/>EM0001: Çıktı dizini oluşturulamadı `X`</h3>

Tarafından belirtilen dizin adı `-o=DIR` yok ve oluşturulamadı. Dosya sistemi için geçersiz bir ad olabilir.

<h3><a name="EM0002"/>EM0002: Seçeneğini `X` desteklenmiyor</h3>

Aracı seçeneğini desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<h3><a name="EM0003"/>EM0003: Platform `X` geçerli değil.</h3>

Aracı platformu desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<h3><a name="EM0004"/>EM0004: Hedef `X` geçerli değil.</h3>

Aracı hedef desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<h3><a name="EM0005"/>EM0005: Derleme hedef `X` geçerli değil.</h3>

Aracı derleme hedef desteklemez `X`. Bu ortamda geçerli değildir veya başka bir aracı sürümü bunu destekler mümkündür.

<h3><a name="EM0006"/>EM0006: Xcode konumunu bulamıyor.</h3>

Aracı konumu şu anda seçili Xcode kullanarak bulunamadı `xcode-select -p` komutu. Bu komutun başarılı ve Xcode konumun doğru döndürür doğrulayın.

<h3><a name="EM0007"/>EM0007: '{sdk}' için sdk sürümü alınamadı.</h3>

Aracı'nı kullanarak SDK sürümü alınamadı `xcrun --show-sdk-version --sdk {sdk}` komutu. Bu komutun başarılı ve SDK sürümü döndürür doğrulayın.

<h3><a name="EM0008"/>EM0008: '{arch}' mimarisi {platformu} için geçerli değil. {Platform} için geçerli mimariler şunlardır: '{mimarileri}'.</h3>

Hata iletisinde mimarisi hedeflenen platformu için geçerli değil. Lütfen geçerli bir mimari--ABI seçeneği geçirilir doğrulayın.

<h3><a name="EM0009"/>EM0009: Özellik `X` Oluşturucu tarafından uygulanmaz.</h3>

Bu oluşturucunun sonraki bir sürümde düzeltme düşündüğünüz bilinen bir sorundur. Katkılar Hoş Geldiniz.

<h3><a name="EM0010"/>EM0010: çerçeveler '{simulatorFramework}' ve '{deviceFramework}' dosyası ' {}' hem de varolduğundan birleştirilemez.</h3>

Olmadığı için ortak bir dosya aralarında aracı hata iletisinde çerçeveleri birleştirilemedi.

Bu eklenmesinde bir hata Embeddinator 4000 gösterebilir; Lütfen sırasında bir hata raporu dosya [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) bir test çalışması ile.

<h3><a name="EM0011"/>EM0011: Derleme `X` yok.</h3>

Araç derlemesi bulunamadı `X` değişkenlerinde belirtilen.

<h3><a name="EM0012"/>EM0012: Derleme adı `X` benzersiz değil</h3>

Sağlanan birden fazla derleme varsa aynı, iç adı ve çalışma zamanında bunları ayırt etmek mümkün olmayacaktır.

En olası nedeni, bir derleme birden çok kez komut satırı bağımsız değişkenleri üzerinde belirtilen ' dir. Ancak özgün adı ve birden çok kopya olan bir adlandırılmış derleme hala tutar olamaz Kerberos'ta.

<h3><a name="EM0013"/>EM0013: 'X', 'Y' tarafından başvurulan derleme bulunamıyor.</h3>

Aracı 'X', 'Y' derlemesi tarafından başvurulan derleme bulunamadı. Lütfen tüm başvurulan derlemeler bağlanması için derleme ile aynı dizinde olduğundan emin olun.

<h3><a name="EM0014"/>EM0014: {ürün} bulunamadı ({ürün} {min_version} gereklidir).</h3>

Hata iletisinde belirtilen bağımlılık sistemde bulunamadı.

<h3><a name="EM0015"/>EM0015: {ürünün} geçerli bir sürümü bulunamadı ({version} bulundu, ancak en az {min_version} gereklidir).</h3>

Bağımlılık hata iletisi sistemde bulundu, ancak çok eski olduğundan belirtiliyor. Lütfen yeni bir sürüme güncelleştirin.

<h3><a name="EM0016">EM0016: simgesel oluşturulamadı '{} dosyası' -> '{hedef}': hata {number}</h3>

Hata iletisinde belirtilen simgesel oluşturulamadı.

<h3><a name="EM0026"/>EM0026 verebilir değil komut satırı bağımsız değişkeni 'A' ayrıştırılamadı: *</h3>

Verilen komut satırı seçeneği söz dizimi `A` aracı tarafından ayrıştırılamadı. Büyük olasılıkla yanlış, belge veya Yardım doğru sözdizimi için gözden geçirin.

<h3><a name="EM0099"/>EM0099: İç hata *. Lütfen bir test çalışması ile bir hata raporu dosya (https://github.com/mono/Embeddinator-4000/issues).</h3>

Embeddinator 4000 içinde bir iç tutarsızlık denetimi başarısız olduğunda bu hata iletisini bildirilir.

Bu eklenmesinde bir hata Embeddinator 4000 gösterir; Lütfen sırasında bir hata raporu dosya [ https://github.com/mono/Embeddinator-4000/issues ](https://github.com/mono/Embeddinator-4000/issues) bir test çalışması ile.


<!-- 1xxx: code processing -->

# <a name="em1xxx-code-processing"></a>EM1xxx: Kod işleme

<h3><a name="EM1010"/>Tür `T` çünkü oluşturulmaz `X` desteklenmez.</h3>

Bu bir **uyarı** , türü `T` göz ardı edilir (yani hiçbir şey oluşturulur) kullandığından `X`, desteklenmeyen bir özellik.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.


<h3><a name="EM1011"/>Tür `T` yerel karşılık gelen eksik olduğundan oluşturulmaz.</h3>

Bu bir **uyarı** , türü `T` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü bir şey yok karşılık gelen yerel platform sahip .NET framework gelen sunmaya kullanır.


<h3><a name="EM1011"/>Tür `T` yerel karşılık gelen hazırlama koduyla eksik olduğundan oluşturulmaz.</h3>

Bu bir **uyarı** , türü `T` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü çerçeveden fazladan hazırlama gerektiren bir şey .NET sunmaya kullanır.

Not: Bu kaybolabileceğini olan şeydir, bazı kısıtlamalarla aracı gelecek bir sürümünde desteklenen.


<h3><a name="EM1020"/>Oluşturucu `C` parametre türü nedeniyle oluşturulmaz `T` desteklenmiyor.</h3>

Bu bir **uyarı** , Oluşturucusu `C` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü türünde bir parametre `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.


<h3><a name="EM1021"/>Oluşturucu `C` hiçbir sarmalayıcı oluşturulur varsayılan değerlere sahip.</h3>

Bu bir **uyarı** , Oluşturucusu varsayılan parametreleri `C` fazladan kod üretme değil. Varolan bir yöntemi zaten aynı imzaya sahip en yaygın neden olur. Örneğin .NET içinde olması mümkündür:

```
public class MyType {
    public MyType () { ... }
    public MyType (int i = 0) { ... }
}
```

Oluşturulan bu gibi durumlarda yalnızca iki `init` Seçici oluşturuldu, her iki arama mono, içine ancak sonraki için hiçbir sarmalayıcı mevcut.


<h3><a name="EM1030"/>Yöntem `M` çünkü oluşturulmaz dönüş türü `T` desteklenmiyor.</h3>

Bu bir **uyarı** , yöntem `M` göz ardı edilir (yani hiçbir şey oluşturulur), dönüş türü olduğundan `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.


<h3><a name="EM1031"/>Yöntem `M` parametre türü nedeniyle oluşturulmaz `T` desteklenmiyor.</h3>

Bu bir **uyarı** , yöntem `M` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü türünde bir parametre `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.


<h3><a name="EM1032"/>Yöntem `M` hiçbir sarmalayıcı oluşturulur varsayılan değerlere sahip.</h3>

Bu bir **uyarı** , yönteminin varsayılan parametreleri `M` fazladan kod üretme değil. Varolan bir yöntemi zaten aynı imzaya sahip en yaygın neden olur. Örneğin .NET içinde olması mümkündür:

```
public class MyType {
    public int Increment () { ... }
    public int Increment (int i = 0) { ... }
}
```

Oluşturulan bu gibi durumlarda yalnızca iki `increment` Seçici oluşturuldu, her iki arama mono, içine ancak sonraki için hiçbir sarmalayıcı mevcut.


<h3><a name="EM1033"/>Yöntem `M` işleci kolay ada sahip başka bir yöntemi gösterir çünkü oluşturulmaz.</h3>

Bu bir **uyarı** , yöntem `M` işleci kolay ada sahip başka bir yöntemi gösterir çünkü oluşturulmaz. (https://msdn.microsoft.com/en-us/library/ms229032(v=vs.110).aspx)


<h3><a name="EM1034"/>Genişletme yöntemi `M` ilkel tür üzerinde oluşturulamayacağı için bir kategori içinde oluşturulmaz `T`. Normal, statik bir yöntem oluşturuldu.</h3>

Bu bir **uyarı** bir primivite üzerinde bir genişletme yöntemi yazın (örneğin `System.Int32`) bulundu. İçinde ObjC ilkel türüne kategorileri oluşturmak mümkün değil. Bunun yerine oluşturucunun olması oluşturacak normal, statik bir yöntem.



<h3><a name="EM1040"/>Özellik `P` parametre türü nedeniyle oluşturulmaz `T` desteklenmiyor.</h3>

Bu bir **uyarı** , özellik `P` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü gösterilen türü `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<h3><a name="EM1041"/>Üzerinde özellikleri dizine `T` birden çok dizin oluşturulmuş özellik desteklenmediğinden oluşturulmaz.</h3>

Bu bir **uyarı** , Dizinli Özellikler `T` göz ardı edilir (yani hiçbir şey oluşturulur) birden çok dizin oluşturulmuş özellik desteklenmediğinden.


<h3><a name="EM1050"/>Alan `F` alan türü nedeniyle oluşturulmaz `T` desteklenmiyor.</h3>

Bu bir **uyarı** , alan `F` göz ardı edilir (yani hiçbir şey oluşturulur) çünkü gösterilen türü `T` desteklenmiyor.

Olmamalıdır neden daha fazla bilgi veren bir önceki uyarı türü `T` desteklenmiyor.

Not: Aracı'nın yeni sürümleri ile desteklenen özellikler gelişmesi.

<h3><a name="EM1051"/>Öğe `E` yerine oluşturulan olarak `F` adını önemli bir objective-c seçicisiyle çakıştığından.</h3>

Bu bir **uyarı** , öğe `E` yerine oluşturulmayacak olarak `F` adını önemli bir objective-c seçicisiyle çakıştığından.

Seçici üzerinde [NSObjectProtocol](https://developer.apple.com/reference/objectivec/1418956-nsobject?language=objc) objective-c önemli anlama sahip ve dikkatle geçersiz kılınmalıdır.

Not: Aracı'nın yeni sürümleri ile ayrılmış Seçici listesini gelişmesi.


<!-- 2xxx: code generation -->

# <a name="em2xxx-code-generation"></a>EM2xxx: Kod oluşturma


<!-- 3xxx: reserved -->
<!-- 4xxx: reserved -->
<!-- 5xxx: reserved -->
<!-- 6xxx: reserved -->
<!-- 7xxx: reserved -->
<!-- 8xxx: reserved -->
<!-- 9xxx: reserved -->
