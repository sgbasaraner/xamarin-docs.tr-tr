---
title: Updating Existing iOS Apps
description: Birleşik API kullanmak için mevcut bir Xamarin.iOS uygulaması güncelleştirmek için aşağıdaki adımları izleyin.
ms.prod: xamarin
ms.assetid: 303C36A8-CBF4-48C0-9412-387E95024CAB
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: ce7e269f76567f824c4cc65916bed32f0f6c08d1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="updating-existing-ios-apps"></a>Updating Existing iOS Apps

_Birleşik API kullanmak için mevcut bir Xamarin.iOS uygulaması güncelleştirmek için aşağıdaki adımları izleyin._

Birleşik API kullanmak için mevcut bir uygulamayı güncelleştirme proje dosyasının kendisini de ad alanları ve uygulama kodunda kullanılan API'leri değişiklikleri gerektirir.

## <a name="the-road-to-64-bits"></a>64 bit yol

Yeni birleşik API'ları, bir Xamarin.iOS mobil uygulamasından 64-bit aygıt mimarileri desteklemek için gereklidir. 1 Şubat 2015'ten itibaren tüm yeni uygulama gönderileri iTunes App Store için 64 bit mimariyi destekleyen Apple gerektirir.

Proje dosyalarını el ile dönüştürebilir veya Xamarin Mac için Visual Studio ve Visual Studio Klasik API Unified API için geçiş işlemini otomatikleştirmek araç sağlar. Bu makalede, otomatik araçları kullanarak yüksek oranda önerilir, ancak her iki yöntem ele alınacaktır.

### <a name="before-you-start"></a>Başlamadan önce...

Birleşik API için mevcut kodunuzu güncelleştirmeden önce tüm ortadan önerilir *derleme uyarıları*. Birçok *uyarıları* için birleşik geçirmek sonra Klasik API'de hataları olacaktır. Klasik API'sinden derleyici iletilerini genellikle ipuçları ne güncelleştirmek sağladığından başlamadan önce bunları düzeltmek daha kolay olur.

## <a name="automated-updating"></a>Otomatik güncelleştirme

Uyarılar sabit sonra Mac veya Visual Studio için Visual Studio'da var olan bir iOS projesi seçin ve Seç **Xamarin.iOS Unified API geçiş** gelen **proje** menüsü. Örneğin:

![](updating-ios-apps-images/beta-tool1.png "Geçiş Xamarin.iOS Unified API için proje menüsünden.")

Otomatik geçiş çalışmadan önce bu uyarı için kabul etmeniz gerekir (tabii, yedeklemeler/kaynak denetimi üzerinde bu adventure başlamadan önce olduğundan emin olun):

![](updating-ios-apps-images/beta-tool2.png "Otomatik geçiş çalışmadan önce bu uyarı olarak kabul ediyorum")

Aracı'nda gösterilen tüm adımlar temelde otomatikleştirir **el ile güncelleştirme** bölümü aşağıda sunulan ve birleşik API için var olan bir Xamarin.iOS projesi dönüştürme önerilen yöntemdir.

## <a name="steps-to-update-manually"></a>El ile güncelleştirme adımları

Uyarılar sabit sonra yeniden el ile yeni birleşik API kullanmak için Xamarin.iOS uygulamaları güncelleştirmek için aşağıdaki adımları izleyin:

### <a name="1-update-project-type--build-target"></a>1. Güncelleştirme proje türü & yapı hedefi

Proje türü değiştirmek, **csproj** dosyaları buradan `6BC8ED88-2882-458C-8E55-DFD12B67127B` için `FEACFBD2-3405-455C-9665-78FE426C6842`. Düzen **csproj** dosyasını bir metin düzenleyicisinde ilk öğe olarak değiştirerek `<ProjectTypeGuids>` gösterildiği gibi öğe:

![](updating-ios-apps-images/csproj.png "Bir metin düzenleyicisinde ProjectTypeGuids öğesindeki ilk öğe gösterildiği gibi değiştirerek csproj dosyayı düzenleyin.")

Değişiklik **alma** içeren öğeyi `Xamarin.MonoTouch.CSharp.targets` için `Xamarin.iOS.CSharp.targets` gösterildiği gibi:

![](updating-ios-apps-images/csproj2.png "Gösterildiği gibi Xamarin.iOS.CSharp.targets Xamarin.MonoTouch.CSharp.targets içeren içeri aktarma öğesi değiştirme")

### <a name="2-update-project-references"></a>2. Proje başvurularını güncelleştirme

İOS uygulama projenin genişletin **başvuruları** düğümü. Başlangıçta gösterecektir bir * bozuk - **monotouch** başvurusu (biz yalnızca proje türü değiştiğinden) bu ekran görüntüsüne benzer:

![](updating-ios-apps-images/references.png "Proje türü değiştiğinden, başlangıçta bozuk monotouch başvuru bu ekran görüntüsüne benzer gösterecektir")

İOS uygulama projesine sağ tıklayın **Düzenle başvuruları**, tıklayın **monotouch** başvuru ve kırmızı "X" düğmesini kullanarak silin.

![](updating-ios-apps-images/references-delete-monotouch-sml.png "Düzenleme başvuruları iOS uygulama projesine sağ tıklayın ardından monotouch referansını tıklayın ve kırmızı kullanarak Sil düğmesine X")

Şimdi başvuruları listesi ve onay sonuna kaydırın **Xamarin.iOS** derleme.

![](updating-ios-apps-images/references-add-xamarinios-sml.png "Şimdi başvuruları listesi ve onay Xamarin.iOS derleme sonuna kaydırın")

Tuşuna **Tamam** proje başvuruları değişiklikleri kaydedin.

### <a name="3-remove-monotouch-from-namespaces"></a>3. MonoTouch ad alanlarını kaldırma

Kaldırma **MonoTouch** ad alanları önekten `using` deyimleri veya yerde bir classname tam olarak (ör. uygun olarak ayarlandı `MonoTouch.UIKit` yalnızca hale `UIKit`).

### <a name="4-remap-types"></a>4. Türleri yeniden eşleme

[Yerel türler](~/cross-platform/macios/nativetypes.md) atanmış olması, daha önce örnekleri gibi kullanılan bazı türlerini değiştirecek sunulan `System.Drawing.RectangleF` ile `CoreGraphics.CGRect` (örneğin). Türlerinin tam listesi bulunabilir [yerel türler](~/cross-platform/macios/nativetypes.md) sayfası.

### <a name="5-fix-method-overrides"></a>5. Yöntemi geçersiz kılmaları Düzelt

Bazı `UIKit` yöntemleri yeni değiştirilen imzalarına sahip [yerel türler](~/cross-platform/macios/nativetypes.md) (gibi `nint`). Özel alt sınıfların bu yöntemleri geçersiz kılarsanız imzalar artık eşleşir ve hatalara neden. Bu yöntemi geçersiz kılmalar yerel türler kullanma yeni imza eşleştirmek için bir alt değiştirerek düzeltin.

Örnekler değiştirme `public override int NumberOfSections (UITableView tableView)` döndürülecek `nint` ve her ikisi de dönüş türü ve parametre türleri değiştirme `public override int RowsInSection (UITableView tableView, int section)` için `nint`.

## <a name="considerations"></a>Dikkat Edilecekler

Aşağıdaki noktaları dikkate bu uygulama bir veya daha fazla bileşen veya NuGet paketi dayalıysa var olan bir Xamarin.iOS projesi Klasik API'SİNDEN yeni birleşik API dönüştürülürken dikkat edilmelidir.

### <a name="components"></a>Bileşenler

Uygulamanıza dahil herhangi bir bileşeni de birleşik API için güncelleştirilmesi gerekir veya derlemeye çalıştığınızda bir çakışma alırsınız. Herhangi bir dahil bileşeni için geçerli sürümü deposundan Xamarin bileşeni Unified API destekleyen yeni bir sürümle değiştirin ve temiz bir yapı yapın. Yazar tarafından dönüştürülmemiş herhangi bir bileşeni bileşen deposunda yalnızca uyarı 32 bit görüntüler.

### <a name="nuget-support"></a>NuGet desteği

Biz Unified API desteği ile çalışmak için NuGet değişiklikleri katkıda bulunan, ancak değil yapılmış olan NuGet, yeni bir sürüm Biz yeni API'leri tanımak için NuGet alma değerlendirmek için.

Bileşenleri gibi o zamana kadar birleşik API'lerini destekleyen bir sürüm projenize dahil herhangi bir NuGet paket geçin ve daha sonra temiz bir yapı yapmak gerekir.

> [!IMPORTANT]
> Formda bir hata varsa _"hatası 3 'monotouch.dll' ve 'Xamarin.iOS.dll' aynı Xamarin.iOS projede içeremez - 'Xamarin.iOS.dll' 'monotouch.dll' tarafından başvurulan sırada açıkça başvurulmaktadır ' xxx, sürüm 0.0.000, = Culture = neutral, PublicKeyToken = null'"_ Unified API uygulamanıza dönüştürdükten sonra genellikle bir bileşen veya NuGet paketi birleşik API'sine güncelleştirilmemiş projesinde sahip nedeniyle istenir. Varolan bileşeni/NuGet kaldırmak, birleşik API'lerini destekleyen bir sürüme güncelleştirmek ve temiz bir yapı yapmanız gerekir.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Xamarin.iOS uygulamaları 64 Bit etkinleştirme derlemeler

Birleşik API'sine dönüştürülen bir Xamarin.iOS mobil uygulama için geliştirici 64-bit makine için uygulama uygulamanın seçeneklerinden oluşturulmasını etkinleştirmek için hala gerekir. Lütfen bakın **etkinleştirme 64 Bit oluşturur, Xamarin.iOS uygulamaları** , [32/64 bit Platform konuları](~/cross-platform/macios/32-and-64/index.md#enable-64) belge 64 bit etkinleştirme hakkında ayrıntılı yönergeler için oluşturur.

## <a name="finishing-up"></a>Tamamlama

Xamarin.iOS uygulamanızı Klasikten birleşik API'lerine dönüştürmek için otomatik veya el ile yöntemi kullanmayı tercih olup olmadığına bakılmaksızın, ayrıca, el ile müdahale gerektirir birkaç örneği vardır. Lütfen bkz. bizim [güncelleştirme kod birleşik API için ipuçları](~/cross-platform/macios/unified/updating-tips.md) belge için bilinen sorunlar ve geçici çözüm çalışır.

## <a name="related-links"></a>İlgili bağlantılar

- [Kodu Unified API’ye Güncelleştirmeye İlişkin İpuçları](~/cross-platform/macios/unified/updating-tips.md)
- [Platformlar Arası Uygulamalarda Yerel Türlerle Çalışma](~/cross-platform/macios/native-types-cross-platform.md)
- [Klasik vs Unified API farklılıkları](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
- [Birleşik API için (video) geçirme](http://university.xamarin.com/lightninglectures/migrating-to-the-unified-api)
