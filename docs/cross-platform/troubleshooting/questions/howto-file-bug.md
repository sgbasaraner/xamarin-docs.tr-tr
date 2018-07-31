---
title: Bir hata raporu, ne zaman ve nasıl dosya?
description: Bu belgeyi zaman, nerede ve nasıl açıklayan bir hata raporu dosya. Ayrıca, en iyi mühendisleri etkinleştirmek iyi sorunu tanılamak hata raporu sağlar.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: b70fe29a79e099f1141c1295d907b48afaa2c3c7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351612"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Bir hata raporu, ne zaman ve nasıl dosya?

Dosya hataları Xamarin'in Bugzilla hata İzleyicisi burada: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all ](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Bir hata ise dosya...

Sahip olduğunuz bir dizi adım düşündüğünüz Xamarin mühendislerin Xamarin tarafından neden olan bir sorunu yeniden oluşturmak için kullanabilirsiniz.

VEYA

Sorunla ilgili bazı kesin durumlarda özellikle de tanımlayabilir, görünür sorunun belirtileri dikkatli bir şekilde tanımlayabilirsiniz. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Xamarin adresi hataları hızla ve verimli bir şekilde yardımcı olması için en iyi uygulamalar


1. <a name="ref-1" />Arama [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) ve raporlar veya sorun doğrudan gidermeye kullanım önerileri hata mevcut web.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />İzleyin [yazma yönergeleri hatanın](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) açıkça ve mümkün olduğunca kısa bir açıklama ne oldu ve olduğu dahil olmak üzere olmasını beklendiği gibi sorunu tanımlamak için.

1. <a name="ref-3" />Herhangi bir ilgili Yığın izlemeleri, hata iletisi metni içeren veya günlükleri kilitlenme. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Görüntülenen ekran görüntüsü ek düz metin olarak çok önemli hata iletileri yazın.

1. <a name="ref-5" />Olabildiğince az kod olarak ile ilgili hata oluşmazsa küçük, bağımsız test durumu içerir.  Sorun (yerleşik şablonlardan birini kullanarak oluşturulan) yepyeni bir projeyle oluşturamıyorsanız, Lütfen sorunu gösteren bir projeyi ZIP ve hata raporu ekleyin.  Örnek Proje mümkün olduğunca basit eklemeden önce yapın. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Burada hata ile karşılaşıldı, dahil olmak üzere işletim sistemi ortamını açıklayın ve [Xamarin sürümlerini](~/cross-platform/troubleshooting/questions/version-logs.md) ve bağımlılıkları.

---

## <a name="additional-details"></a>Ek ayrıntılar

1. <a name="note-1" />[*^*](#ref-1) Diğer müşteriler, aynı sorunu görmeye olup olmadığını doğrulayabilirsiniz ideal olarak "görünür Belirtiler" açıklamasına yeterli ayrıntı içermelidir (aynı hata iletileri, aynı performansında bir kilitlenme aynı yığın izlemesinden _vs._ ). "Kesin durumlarda", benzer bir şey söylemek, iyi bir örnek olur: "ı normalde sorun %75 zaman isabet, ancak bu şeyi değiştirirsem sonra sorunu tamamen kaçınabilirim." Başka bir benzer bir "precise duruma" Xamarin önceki bir sürümü için eski sürüme düşürme sorun durursa örneğidir.

1. <a name="note-2" />[*^*](#ref-2) Beklediğiniz gibi hata metni (veya herhangi bir benzersiz açıklayıcı metin) genellikle en iyi arama terimleri parçacıklarıdır. Hoş Geldiniz ayrıntıları ekleyin veya yeni bir dosya eksik, var olan bir hata raporu ise, daha iyi rapor hata.

1. <a name="note-3" />[*^*](#ref-3) Başka bir iyi soru aynı sorunu herhangi bir Java için bildirilen olduğundan Objective-C ya da Swift uygulamalar. Bu durumda, sorun büyük olasılıkla parçası Android veya iOS kendisi yerine Xamarin parçası olmasıdır.

1. <a name="note-4" />[*^*](#ref-4) Birkaç örnek bilgi verilmiştir:

    1. Lütfen bir proje oluşturma sırasında oluşan hataları eklemek için tam [tanılama derleme çıkışını](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) hata raporu üzerinde.
    
    1. Oluşturma veya bir iOS projesi Visual Studio'dan hata ayıklama sırasında oluşan hatalar için lütfen çalıştırın _Yardım > Xamarin > Zip günlükleri_ sonra hata çıkma ve hata raporu üzerinde oluşturulan .zip dosyasını içerir.
    
    1. Özel durumlar veya kilitlenmelere Android veya iOS uygulamaları için lütfen ilgili ekleyin "[günlükleri Xamarin.Android ve Xamarin.iOS uygulamaları için hata ayıklama](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) Mümkünse, belirli sorun için bir mükemmel dosyaları az sayıda özgün çözümünüze ait yepyeni bir çözüme ekleyerek sorunu yeniden oluşturmak için seçenektir. Xamarin takım genellikle daha büyük test çalışmaları (yeniden oluşturma adımlarını açıkça açıklanmıştır varsayılarak), ancak en iyi hata hızlıca çözülmesi fırsat daha basit test çalışmaları verin üzerinde bile sorunları araştırmak mümkün olacaktır.


1. <a name="note-6" />[*^*](#ref-6) Eğer öyleyse _değil_ olası dosyaları az sayıda yeni bir çözüme ekleyerek sorunu yeniden oluşturmak, daha sonra zip ve tam uygulamanız için tam Çözüm klasörü ekleme. Lütfen silin `bin`, `obj`, `Components`, ve `packages` daha küçük dosya zip klasörleri. (IDE ve yapı işlemi genellikle geri yüklemek veya bu klasörlerin içeriğini gerektiği gibi yeniden oluşturun.) Ortaya çıkan çözüm hala özgün sorunu gösteren sürece istediğiniz gibi çok kodu ve kaynak dosyaları projeden de silebilirsiniz.

