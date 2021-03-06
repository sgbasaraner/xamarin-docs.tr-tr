---
title: Bir hata raporu, ne zaman ve nasıl dosya?
description: Bu belgeyi zaman, nerede ve nasıl açıklayan bir hata raporu dosya. Ayrıca, en iyi mühendisleri etkinleştirmek iyi sorunu tanılamak hata raporu sağlar.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: conceptdev
ms.author: crdun
ms.date: 08/01/2018
ms.openlocfilehash: f20740ff1e16187be3d3703b3da07329f6f52daf
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514344"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Bir hata raporu, ne zaman ve nasıl dosya?

> [!TIP]
> Kullanım **sorun bildir** Visual Studio'daki menü öğesi &ndash; bu sorunu gidermek için hata raporu birlikte tanı bilgileri gönderir.
>
> Ayrıntılı yönergeler için vardır [Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017) ve [Mac için Visual Studio](https://docs.microsoft.com/visualstudio/mac/report-a-problem).
>
> Varolan raporlar için arama yapabilirsiniz [Visual Studio Geliştirici topluluğu](https://developercommunity.visualstudio.com/) Web sitesi.

## <a name="file-a-bug-if"></a>Bir hata ise dosya...

Bir kümeniz adımları mühendislerin bir sorunu yeniden oluşturmak için kullanmanız mümkün olacaktır düşünün.

VEYA

Sorunla ilgili bazı kesin durumlarda özellikle de tanımlayabilir, görünür sorunun belirtileri dikkatli bir şekilde tanımlayabilirsiniz. <sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>Adres hataları hızla ve verimli bir şekilde yardımcı olması için en iyi uygulamalar

1. <a name="ref-1" />Arama [Visual Studio Geliştirici topluluğu](https://developercommunity.visualstudio.com/) ve raporlar veya sorun doğrudan gidermeye kullanım önerileri hata mevcut web.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Ne oldu ve gerçekleştirilecek beklenen bir açıklama da dahil olmak üzere sorunu açıkça ve mümkün olduğunca kısaca açıklayın.

1. <a name="ref-3" />Herhangi bir ilgili Yığın izlemeleri, hata iletisi metni içeren veya çöküş günlüklerini (kullanırsanız **sorun bildir** özelliği, bunlar olabilir birlikte otomatik olarak). <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Görüntülenen ekran görüntüsü ek düz metin olarak çok önemli hata iletileri yazın.

1. <a name="ref-5" />Olabildiğince az kod olarak ile ilgili hata oluşmazsa küçük, bağımsız test durumu içerir.  Sorun (yerleşik şablonlardan birini kullanarak oluşturulan) yepyeni bir projeyle oluşturamıyorsanız, Lütfen sorunu gösteren bir projeyi ZIP ve hata raporu ekleyin.  Örnek Proje mümkün olduğunca basit eklemeden önce yapın. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Burada hata ile karşılaşıldı, dahil olmak üzere işletim sistemi ortamını açıklayın ve [Xamarin sürümlerini](~/cross-platform/troubleshooting/questions/version-logs.md) ve bağımlılıkları.

## <a name="additional-details"></a>Ek ayrıntılar

1. <a name="note-1" />[*^*](#ref-1) Diğer müşteriler, aynı sorunu görmeye olup olmadığını doğrulayabilirsiniz ideal olarak "görünür Belirtiler" açıklamasına yeterli ayrıntı içermelidir (aynı hata iletileri, aynı performansında bir kilitlenme aynı yığın izlemesinden _vs._ ). "Kesin durumlarda", benzer bir şey söylemek, iyi bir örnek olur: "ı normalde sorun %75 zaman isabet, ancak bu şeyi değiştirirsem sonra sorunu tamamen kaçınabilirim." Başka bir benzer bir "precise duruma" Xamarin önceki bir sürümü için eski sürüme düşürme sorun durursa örneğidir.

1. <a name="note-2" />[*^*](#ref-2) Beklediğiniz gibi hata metni (veya herhangi bir benzersiz açıklayıcı metin) genellikle en iyi arama terimleri parçacıklarıdır. Hoş Geldiniz ayrıntıları ekleyin veya yeni bir dosya eksik, var olan bir hata raporu ise, daha iyi rapor hata.

1. <a name="note-3" />[*^*](#ref-3) Başka bir iyi soru aynı sorunu herhangi bir Java için bildirilen olduğundan Objective-C ya da Swift uygulamalar. Bu durumda, sorun büyük olasılıkla parçası Android veya iOS kendisi yerine Xamarin parçası olmasıdır.

1. <a name="note-4" />[*^*](#ref-4) Birkaç örnek bilgi verilmiştir:

    1. Lütfen bir proje oluşturma sırasında oluşan hataları eklemek için tam [tanılama derleme çıkışını](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) hata raporu üzerinde.

    1. Oluşturma veya bir iOS projesi Visual Studio'dan hata ayıklama sırasında oluşan hatalar için lütfen çalıştırın **Yardım > Xamarin > Zip günlükleri** sonra hata çıkma ve hata raporu üzerinde oluşturulan .zip dosyasını içerir.

    1. Özel durumlar veya kilitlenmelere Android veya iOS uygulamaları için lütfen ilgili dahil [günlükleri Xamarin.Android ve Xamarin.iOS uygulamaları için hata ayıklama](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps).

1. <a name="note-5" />[*^*](#ref-5) Mümkünse, belirli bir sorun için bir seçenek sorun dosyaları az sayıda özgün çözümünüze ait yepyeni bir çözüme ekleyerek yeniden oluşturmaktır. Xamarin takım genellikle daha büyük test çalışmaları (yeniden oluşturma adımlarını açıkça açıklanmıştır varsayılarak), ancak en iyi hata hızlıca çözülmesi fırsat daha basit test çalışmaları verin üzerinde bile sorunları araştırmak mümkün olacaktır.

1. <a name="note-6" />[*^*](#ref-6) Eğer öyleyse _değil_ olası dosyaları az sayıda yeni bir çözüme ekleyerek sorunu yeniden oluşturmak, daha sonra zip ve tam uygulamanız için tam Çözüm klasörü ekleme. Lütfen silin `bin`, `obj`, `Components`, ve `packages` daha küçük dosya zip klasörleri. (IDE ve yapı işlemi genellikle geri yüklemek veya bu klasörlerin içeriğini gerektiği gibi yeniden oluşturun.) Ortaya çıkan çözüm hala özgün sorunu gösteren sürece istediğiniz gibi çok kodu ve kaynak dosyaları projeden de silebilirsiniz.
