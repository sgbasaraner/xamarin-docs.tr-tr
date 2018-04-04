---
title: .xib kod oluşturma
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: b887dbf09693452f62f744669ad9713927020cea
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="xib-code-generation"></a>.xib kod oluşturma

> [!IMPORTANT]
>  Bu belgede yalnızca Xcode'nın arabirimi Oluşturucu ile Mac'ın tümleştirme için Visual Studio açıklanmaktadır gibi eylemleri ve çıkışlar Xamarin Tasarımcısı'nda iOS için kullanılmaz. İOS Tasarımcısı hakkında daha fazla bilgi için lütfen gözden [iOS Tasarımcısı](~/ios/user-interface/designer/index.md) belge.

Apple arabirimi Oluşturucu aracı ("IB"), kullanıcı arabirimleri görsel olarak tasarlamak için kullanılabilir. IB tarafından oluşturulan arabirimi tanımları kaydedilir **.xib** dosyaları. Pencere öğeleri ve diğer nesneleri **.xib** dosyaları "özel bir kullanıcı tanımlı tür olabilen bir sınıf kimliği" verilir. Bu pencere öğeleri davranışını özelleştirmek için ve özel pencere öğeleri yazma izin verir.

Bu kullanıcı sınıfları normalde UI denetleyicisi sınıfların alt sınıflarıdır. Sahip oldukları *çıkışlar* (özelliklerine benzer) ve *Eylemler* (benzer olayları), ağ bağlantısı arabirimi nesnelere. Çalışma zamanında IB dosya yüklendiğinde nesneleri oluşturma ve çıkışlar ve eylemleri çeşitli kullanıcı Arabirimi nesneleri dinamik olarak bağlı. Bu yönetilen sınıflar tanımlarken, tüm eylemleri ve IB bekliyor olanlarla eşleşmesi için çıkışlar tanımlamanız gerekir. Mac için Visual Studio, bu basitleştirmek için arkasındaki koda benzeri modelini kullanır. Bu Xcode Objective-C için yaptığı için benzer, ancak kuralları ve kod oluşturma modeli .NET geliştiricileri için fazla tanımanız tweaked.

İle çalışma **.xib** dosyaları için Visual Studio içinde Xamarin.iOS şu anda desteklenmiyor.

## <a name="xib-files-and-custom-classes"></a>.xib dosyaları ve özel sınıflar

Varolan türlerinden Cocoa Touch kullanarak yanı sıra, bu özel türleri tanımlama mümkündür **.xib** dosyaları. Diğer tanımlanan türleri kullanmak da mümkündür **.xib** dosyaları veya tamamen C# kod içinde tanımlanan. Şu anda, arabirimi Oluşturucu geçerli dışında tanımlanan türlerin ayrıntılarını farkında olmayan **.xib** değil bunları listesinde görüntülenecektir veya kendi özel çıkışlar ve Eylemler göstermek için dosya. Bu sınırlama kaldırma süre gelecekte için planlanan.

Özel sınıflar tanımlanabilir bir **.xib** arabirimi Oluşturucu "Sınıfları" sekmesindeki "Alt kümesi Ekle" komutunu kullanarak dosya. Bu "Arkasındaki koda" sınıflar olarak bakın. Varsa **.xib** dosya sahip bir ". xib.designer.cs" karşılık gelen daha sonra Mac için Visual Studio Proje dosyasında otomatik olarak doldurur, tüm özel sınıflar için kısmi sınıflar tanımlarıyla **.xib**. Biz "Tasarımcı sınıfları olarak" Bu kısmi sınıflara bakın.

## <a name="generating-code"></a>Kod Üretme

Herhangi **{0} .xib** bir yapı eylemi dosyasıyla *sayfa*, bir **{0}.xib.designer.cs** projede mevcut da dosya, Mac için Visual Studio, kısmi sınıflarda üretir Tüm içinde bulabilir kullanıcı sınıfları için tasarımcı dosya **.xib** çıkışlar için özellikler ve tüm eylemler için kısmi yöntemleri ile dosya. Kod oluşturma, yalnızca bu dosyanın varlığını tarafından etkinleştirilir.

Tasarımcı otomatik olarak dosyasıdır ne zaman güncelleştirilmiş **.xib** dosya değişiklikleri ve Visual Studio Mac odak yeniden kazanabilmesi için. Değişikliklerin üzerine gelecek sefer Visual Studio Mac güncelleştirmeler için dosya olarak Tasarımcısı dosyasına el ile değiştirilmemelidir.

## <a name="registration-and-namespaces"></a>Kayıt ve ad alanları

Mac için Visual Studio normal .NET proje namespacing ile tutarlı hale getirmek için tasarımcı dosya konumu için projenin varsayılan ad alanını kullanarak Tasarımcı sınıfları oluşturur. Designer dosyaları ad alanı, varsayılan proje "ad" ve ".NET ilkeleri adlandırma" ayarları tarafından yönetilir. Projenizin varsayılan ad alanı değişirse, kısmi sınıflar artık eşleştiğini bulabilirsiniz şekilde MD yeni ad alanındaki sınıflar yeniden oluşturur, kaybolacağını unutmayın.

Sınıfı Objective-C çalışma zamanı tarafından bulunabilir duruma getirmek için Mac için Visual Studio geçerlidir bir `[Register (name)]` öznitelik sınıfı. Xamarin.iOS otomatik olarak kaydeder rağmen `NSObject`-türetilmiş sınıfları, tam nitelenmiş .NET adlarını kullanır. Mac Bu her sınıf emin olmak için geçersiz kılmalar için Visual Studio tarafından uygulanan öznitelik kullanılan ad kayıtlı **.xib** dosya. Tasarımcı dosyaları oluşturmak için Visual Studio Mac için kullanmadan IB içinde özel sınıflar kullanırsanız, bu beklenen Objective-C sınıf adlarını eşleştirme, yönetilen sınıflar el ile yapmak için geçerli olabilir.

Birden çok içinde sınıfları tanımlanamaz **.xib**, veya çakışan.

## <a name="non-designer-class-parts"></a>Tasarımcı olmayan sınıf bölümleri

Tasarımcı kısmi sınıflar amaçlanmayan olarak kullanılması-değil. Çıkışlar özeldir ve hiçbir temel sınıf belirtilir. Her sınıfın temel sınıf ayarlar, kullanır veya çıkışlar gösterir ve yerel kod sınıfından yüklenirkenörneğioluşturmakiçingereklioluşturuculartanımlarbaşkabirdosyasındakarşılıkgelenbir"designerolmayan"sınıfbölümüvarsabeklenir**.xib**. Varsayılan **.xib** şablonları bunu ancak olarak tanımlamak için ek özel sınıfları bir **.xib**, Tasarımcısı olmayan bölümü el ile eklemeniz gerekir.

Bunun nedeni, esneklik için gerekiyor. Örneğin, birden çok arkasındaki koda sınıfları ortak bir Özet sınıf, hangi alt sınıflar tarafından IB sınıflandırma için sınıf yönetilen bir alt kümesi olabilir.

Bunlar yerleştirmek için geleneksel bir **{0}.xib.cs** yanındaki dosya **{0}.xib.designer.cs** Tasarımcı dosya.

<a name="generated" />

## <a name="generated-actions-and-outlets"></a>Oluşturulan Eylemler ve çıkışları

Kısmi Tasarımcı sınıflarda Mac için Visual Studio IB ve bağlı tüm eylemler için karşılık gelen kısmi yöntemler tanımlanan tüm bağlı çıkışlar karşılık gelen özelliklerini oluşturur.

### <a name="outlet-properties"></a>Çıkış özellikleri

Tasarımcı sınıfları özel sınıf içinde tanımlanan tüm çıkışlar karşılık gelen özellikleri içerir. Bu özellikler bulunmasına geç bağlama etkinleştirmek için-Objective C köprüsü Xamarin.iOS uygulaması ayrıntılarını ' dir. Bunları yalnızca arkasındaki koda sınıfından kullanılmak üzere özel alanlar eşdeğer olarak göz önünde bulundurmalısınız. Genel hale getirmek isterseniz, diğer özel alan için yaptığınız gibi erişimci özellikleri Tasarımcısı olmayan sınıf bölümüne ekleyin.

Çıkış özellikleri bir tür için tanımlanan varsa `id` (eşdeğer `NSObject`) sonra tasarımcı kod Oluşturucu kolaylık sağlamak için bu çıkışı bağlı nesneleri dayanan güçlü olası türü şu anda belirler.
Açıkça kesinlikle çıkışlar özel bir sınıf tanımlarken yazdığınız önerilir ancak, bu gelecek sürümlerinde desteklenmiyor olabilir.

### <a name="action-properties"></a>Eylem özellikleri

Kısmi yöntemler özel sınıf içinde tanımlanan tüm eylemler için karşılık gelen Tasarımcı sınıflar içerir. Bunlar, bir uygulama olmadan yöntemleridir. Kısmi yöntemler amacı iki yönlüdür:

1.  Yazarsanız `partial` uygulanan olmayan tüm kısmi yöntemler imzalarını Tasarımcısı olmayan sınıf bölümünün sınıf gövdesinde Mac için Visual Studio için otomatik tamamlama sunacaktır.
2.  Kısmi yöntem imzaları bunların karşılık gelen eylem olarak ele, böylece Objective-C dünyasına gösterir, uygulanan bir öznitelik vardır.


İsterseniz, kısmi yöntemi yok sayın ve farklı bir yöntem öznitelik uygulayarak eylemi uygulamak veya bir temel sınıf atlayabilir çalışmasına izin.

Eylemler gönderen türü için tanımlı varsa `id` (eşdeğer `NSObject`), sonra da tasarımcı kod Oluşturucu şu anda bu eyleme bağlı nesnelerde tabanlı güçlü olası türü belirler. Açıkça kesinlikle eylemleri özel bir sınıf tanımlarken yazdığınız önerilir ancak, bu gelecek sürümlerinde desteklenmiyor olabilir.

Diğer diller için oluşturulmaz şekilde kısmi yöntemler CodeDOM desteklemediğinden bu kısmi yöntemler yalnızca C# ' ta oluşturulduğunu unutmayın.

## <a name="cross-xib-class-usage"></a>Çapraz XIB sınıfı kullanımı

Bazı durumlarda, aynı sınıfın birden çok from başvurmak kullanıcıların istediğiniz **.xib** sekmesini denetleyicileriyle örneğin dosyaları. Bu, başka bir sınıf tanımı başvuran özel tarafından yapılabilir **.xib** dosya ya da yeniden ikincide tanımlayarak aynı sınıf adı ile **.xib**.

İkinci durumda Mac işleme için Visual Studio nedeniyle sorunlu **.xib** tek tek dosyalar. Otomatik olarak algılamak ve aynı parçalı sınıf içinde birden çok designer dosyaları tanımlandığında kayıt özniteliği birden çok kez uygulama çakışmalı anlamayabilir şekilde yinelenen tanımları, birleştirme olamaz. Bu sorunu çözmek yeni Mac için Visual Studio sürümlerinde çalışır, ancak her zaman beklendiği gibi çalışmayabilir. Gelecekte bu desteklenmeyen hale gelmesi olasıdır ve bunun yerine, Mac için Visual Studio tüm içinde tanımlanan tüm türleri hale getirecektir **.xib** dosyaları ve yönetilen kod tümünden doğrudan görünür projesinde **.xib** dosyaları.

## <a name="type-resolution"></a>Türü Çözümlemesi

IB içinde kullanılan Objective-C tür adları türleridir. Bu kayıt öznitelikleri kullanımıyla CLR türleriyle eşlenir. Çıkış ve eylem kodu oluştururken, Mac için Visual Studio Xamarin.iOS çekirdek tarafından Sarmalanan tüm Objective-C türleri için karşılık gelen CLR Türleri çözün ve tür adları tam olarak nitelemek.

Ancak, isteğe bağlı olarak bu gibi durumlarda verbatim türü adı çıkarır için kod Oluşturucu şu anda kullanıcı kodu ya da kitaplıkları, Objective-C türü adlarından CLR Türleri çözümlenemiyor. Başka bir deyişle, karşılık gelen CLR türü Objective-C türle aynı ada ve bunu kullanarak kod olarak aynı ad alanında olması gerekir. Bu projedeki tüm Objective-C türleri kod oluşturma sırasında dikkate alarak gelecekte süre düzeltilmesi planlanmaktadır.
