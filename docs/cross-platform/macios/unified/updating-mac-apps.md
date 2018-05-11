---
title: Mevcut Mac uygulamaları güncelleştirme
description: Birleşik API kullanmak için mevcut bir Xamarin.Mac uygulamayı güncelleştirmek için aşağıdaki adımları izleyin.
ms.prod: xamarin
ms.assetid: 26673CC5-C1E5-4BAC-BEF4-9A386B296FD5
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: a2e3df4db13ccbf8001b762bf29a3eb53cacd35a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
---
# <a name="updating-existing-mac-apps"></a>Mevcut Mac uygulamaları güncelleştirme

_Birleşik API kullanmak için mevcut bir Xamarin.Mac uygulamayı güncelleştirmek için aşağıdaki adımları izleyin._

Birleşik API kullanmak için mevcut bir uygulamayı güncelleştirme proje dosyasının kendisini de ad alanları ve uygulama kodunda kullanılan API'leri değişiklikleri gerektirir.

## <a name="the-road-to-64-bits"></a>64 bit yol

Yeni birleşik API'ları, 64 bit aygıt mimarileri Xamarin.Mac uygulamasından desteklemek için gereklidir. 1 Şubat 2015'ten itibaren Apple Mac App Store için tüm yeni uygulama gönderileri 64 bit mimariyi desteklemesini gerektirir.

Proje dosyalarını el ile dönüştürebilir veya Xamarin Mac için Visual Studio ve Visual Studio Klasik API Unified API için geçiş işlemini otomatikleştirmek araç sağlar. Bu makalede, otomatik araçları kullanarak yüksek oranda önerilir, ancak her iki yöntem ele alınacaktır.

### <a name="before-you-start"></a>Başlamadan önce...

Birleşik API için mevcut kodunuzu güncelleştirmeden önce tüm ortadan önerilir *derleme uyarıları*. Birçok *uyarıları* için birleşik geçirmek sonra Klasik API'de hataları olacaktır. Klasik API'sinden derleyici iletilerini genellikle ipuçları ne güncelleştirmek sağladığından başlamadan önce bunları düzeltmek daha kolay olur.

## <a name="automated-updating"></a>Otomatik güncelleştirme

Uyarılar sabit sonra Mac veya Visual Studio için Visual Studio'da varolan Mac projesini seçin ve Seç **Xamarin.Mac Unified API geçiş** gelen **proje** menüsü. Örneğin:

![](updating-mac-apps-images/beta-tool1.png "Geçiş Xamarin.Mac Unified API için proje menüsünden.")

Otomatik geçiş çalışmadan önce bu uyarı için kabul etmeniz gerekir (tabii, yedeklemeler/kaynak denetimi üzerinde bu adventure başlamadan önce olduğundan emin olun):

![](updating-mac-apps-images/migrate01.png "Otomatik geçiş çalışmadan önce bu uyarı olarak kabul ediyorum")

Birleşik API Xamarin.Mac uygulamada kullanırken seçilebilir iki desteklenen hedef Framework türleri şunlardır:

- **Xamarin.Mac mobil Framework** -Xamarin.iOS ve bir alt kümesini tam destek Xamarin.Android tarafından kullanılan aynı bizi .NET framework budur **Masaüstü** framework. Üst bağlama davranışı nedeniyle daha küçük ortalama ikili sağladığından bu önerilen bir çerçevedir.
- **Xamarin.Mac .NET 4.5 Framework** -bu framework yeniden, bir alt kümesidir **Masaüstü** framework. Ancak, kırpar çok daha az tam olarak **Masaüstü** framework daha **mobil** framework ve yapmanız gereken _"yalnızca iş"_ çoğu NuGet paketlerini veya 3 taraf kitaplıkları ile. Bu standart tüketmeye Geliştirici sağlar **Masaüstü** daha büyük uygulama paketleri üreten desteklenen çerçevelerden, ancak bu seçenek kullanmaya devam ederken derlemeler. Bu ile uyumlu olmayan burada 3 taraf .NET derlemelerini kullanıldığından önerilen yapıdır **Xamarin.Mac mobil Framework**. Desteklenen derlemeleri listesi için lütfen bkz bizim [derlemeleri](~/cross-platform/internals/available-assemblies.md) belgeleri.

Hedef çerçeveler hakkında ayrıntılı bilgi ve Xamarin.Mac uygulamanız için belirli bir hedef seçme etkilerini için lütfen bkz. bizim [hedef çerçeveyi](~/mac/platform/target-framework.md) belgeleri. 

Aracı'nda gösterilen tüm adımlar temelde otomatikleştirir **el ile güncelleştirme** bölümü aşağıda sunulan ve birleşik API için varolan Xamarin.Mac projesini dönüştürme önerilen yöntemdir.

## <a name="steps-to-update-manually"></a>El ile güncelleştirme adımları

Uyarılar sabit sonra yeniden el ile yeni birleşik API kullanmak için Xamarin.Mac uygulamaları güncelleştirmek için aşağıdaki adımları izleyin:

### <a name="1-update-project-type--build-target"></a>1. Güncelleştirme proje türü & yapı hedefi

Proje türü değiştirmek, **csproj** dosyaları buradan `42C0BBD9-55CE-4FC1-8D90-A7348ABAFB23` için `A3F8F2AB-B479-4A4A-A458-A89E7DC349F1`. Düzen **csproj** dosyasını bir metin düzenleyicisinde ilk öğe olarak değiştirerek `<ProjectTypeGuids>` gösterildiği gibi öğe:

![](updating-mac-apps-images/csproj.png "Bir metin düzenleyicisinde ProjectTypeGuids öğesindeki ilk öğe gösterildiği gibi değiştirerek csproj dosyayı düzenleyin.")

Değişiklik **alma** içeren öğeyi `Xamarin.Mac.targets` için `Xamarin.Mac.CSharp.targets` gösterildiği gibi:

![](updating-mac-apps-images/csproj2.png "Gösterildiği gibi Xamarin.Mac.CSharp.targets Xamarin.Mac.targets içeren içeri aktarma öğesi değiştirme")

Kodun hemen ardından aşağıdaki satırları ekleyin `<AssemblyName>` öğe:

```xml
<TargetFrameworkVersion>v2.0</TargetFrameworkVersion>
<TargetFrameworkIdentifier>Xamarin.Mac</TargetFrameworkIdentifier>

```

Örnek:

![](updating-mac-apps-images/csproj3.png "Bu kod satırları < AssemblyName > öğeden sonra ekleyin")

### <a name="2-update-project-references"></a>2. Proje başvurularını güncelleştirme

Mac uygulama projenin genişletin **başvuruları** düğümü. Başlangıçta gösterecektir bir * bozuk - **XamMac** başvurusu (biz yalnızca proje türü değiştiğinden) bu ekran görüntüsüne benzer:

![](updating-mac-apps-images/references.png "Başlangıçta bozuk XamMac başvuru bu ekran görüntüsüne benzer gösterecektir")

Tıklatın **dişli simgesi** yanında **XamMac** seçin ve girişi **silmek** kırık referans kaldırmak için.

Ardından, sağ tıklayın **başvuruları** klasöründe **Çözüm Gezgini** seçip **Düzenle başvurular**. Başvuruları listesi sonuna kaydırın ve yanı sıra işaretleyin **Xamarin.Mac**.

![](updating-mac-apps-images/references2.png "Başvuruları listesi alt kısmına kaydırın ve Xamarin.Mac yanında bir onay işareti koyun")

Tuşuna **Tamam** proje başvuruları değişiklikleri kaydedin.

### <a name="3-remove-monomac-from-namespaces"></a>3. MonoMac ad alanlarını kaldırma

Kaldırma **MonoMac** ad alanları önekten `using` deyimleri veya yerde bir classname tam olarak (ör. uygun olarak ayarlandı `MonoMac.AppKit` yalnızca hale `AppKit`).

### <a name="4-remap-types"></a>4. Türleri yeniden eşleme

[Yerel türler](~/cross-platform/macios/nativetypes.md) atanmış olması, daha önce örnekleri gibi kullanılan bazı türlerini değiştirecek sunulan `System.Drawing.RectangleF` ile `CoreGraphics.CGRect` (örneğin). Türlerinin tam listesi bulunabilir [yerel türler](~/cross-platform/macios/nativetypes.md) sayfası.

### <a name="5-fix-method-overrides"></a>5. Yöntemi geçersiz kılmaları Düzelt

Bazı `AppKit` yöntemleri yeni değiştirilen imzalarına sahip [yerel türler](~/cross-platform/macios/nativetypes.md) (gibi `nint`). Özel alt sınıfların bu yöntemleri geçersiz kılarsanız imzalar artık eşleşir ve hatalara neden. Bu yöntemi geçersiz kılmalar yerel türler kullanma yeni imza eşleştirmek için bir alt değiştirerek düzeltin. 

## <a name="considerations"></a>Dikkat Edilecekler

Aşağıdaki noktaları dikkate bu uygulama bir veya daha fazla bileşen veya NuGet paketi dayalıysa varolan Xamarin.Mac projesini yeni birleşik API için Klasik API'SİNDEN dönüştürülürken dikkat edilmelidir. 

### <a name="components"></a>Bileşenler

Uygulamanıza dahil herhangi bir bileşeni de birleşik API için güncelleştirilmesi gerekir veya derlemeye çalıştığınızda bir çakışma alırsınız. Herhangi bir dahil bileşeni için geçerli sürümü deposundan Xamarin bileşeni Unified API destekleyen yeni bir sürümle değiştirin ve temiz bir yapı yapın. Yazar tarafından dönüştürülmemiş herhangi bir bileşeni bileşen deposunda yalnızca uyarı 32 bit görüntüler.

### <a name="nuget-support"></a>NuGet desteği

Biz Unified API desteği ile çalışmak için NuGet değişiklikleri katkıda bulunan, ancak değil yapılmış olan NuGet, yeni bir sürüm Biz yeni API'leri tanımak için NuGet alma değerlendirmek için. 

Bileşenleri gibi o zamana kadar birleşik API'lerini destekleyen bir sürüm projenize dahil herhangi bir NuGet paket geçin ve daha sonra temiz bir yapı yapmak gerekir.

> [!IMPORTANT]
> Formda bir hata varsa _"hatası 3 'monomac.dll' ve 'Xamarin.Mac.dll' aynı Xamarin.Mac projede içeremez - 'Xamarin.Mac.dll' 'monomac.dll' tarafından başvurulan sırada açıkça başvurulmaktadır ' xxx, sürüm 0.0.000, kültür = = neutral, PublicKeyToken = null'"_ Unified API uygulamanıza dönüştürdükten sonra genellikle bir bileşen veya NuGet paketi birleşik API'sine güncelleştirilmemiş projesinde sahip nedeniyle istenir. Varolan bileşeni/NuGet kaldırmak, birleşik API'lerini destekleyen bir sürüme güncelleştirmek ve temiz bir yapı yapmanız gerekir.

## <a name="enabling-64-bit-builds-of-xamarinmac-apps"></a>64 Bit etkinleştirme Xamarin.Mac uygulamaların derlemeler

Birleşik API'sine dönüştürülen bir Xamarin.Mac mobil uygulama için geliştirici 64-bit makine için uygulama uygulamanın seçeneklerinden oluşturulmasını etkinleştirmek için hala gerekir. Lütfen bakın **etkinleştirme 64 Bit oluşturur, Xamarin.Mac uygulamaları** , [32/64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md) belge 64 bit etkinleştirme hakkında ayrıntılı yönergeler için oluşturur.
    
## <a name="finishing-up"></a>Tamamlama

Xamarin.Mac uygulamanız için birleşik API Klasikten dönüştürmek için otomatik veya el ile yöntemi kullanmayı tercih olup olmadığına bakılmaksızın, ayrıca, el ile müdahale gerektirir birkaç örneği vardır. Lütfen bkz. bizim [güncelleştirme kod birleşik API için ipuçları](~/cross-platform/macios/unified/updating-tips.md) belge için bilinen sorunlar ve geçici çözüm çalışır.

## <a name="related-links"></a>İlgili bağlantılar

- [Kodu Unified API’ye Güncelleştirmeye İlişkin İpuçları](~/cross-platform/macios/unified/updating-tips.md)
- [Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
