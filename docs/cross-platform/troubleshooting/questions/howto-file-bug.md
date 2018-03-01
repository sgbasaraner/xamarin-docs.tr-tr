---
title: "Bir hata raporu, ne zaman ve nasıl dosya?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 3a57c0843a68b454c8cb21c95b280d2731d064cd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Bir hata raporu, ne zaman ve nasıl dosya?


Xamarin'ın Bugzilla hata İzleyici'buraya dosya hataları: [https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Bir hata durumunda dosya...


Xamarin mühendisleri tarafından Xamarin neden bir sorunu yeniden oluşturmak için kullanmak mümkün olacaktır düşündüğünüz adımları kümesine sahiptir.

VEYA

Özellikle de sorunla ilgili kesin bazı durumlarda açıklayabilirsiniz varsa görünür sorun belirtileri dikkatle tanımlayabilirsiniz. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Xamarin adresi hataları hızlı ve verimli şekilde yardımcı olması için en iyi uygulamalar


1. <a name="ref-1" />Arama [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) ve raporları veya doğrudan sorun gidermeye kullanım önerileri hata mevcut web.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />İzleyin [yönergeleri yazılırken hata](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) açıkça ve mümkün olduğunca kısaca ne oldu ve şeklindeydi açıklaması da dahil olmak üzere gerçekleşecek şekilde beklendiği gibi sorun açıklamak için.

1. <a name="ref-3" />Dahil herhangi bir ilgili Yığın izlemeleri, hata iletisi metni veya günlükleri kilitlenme. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Ekran ekleri çok düz metin olarak görünen herhangi bir önemli hata iletisi yazın.

1. <a name="ref-5" />Hata ile mümkün olduğunca az kod olarak oluşmazsa küçük, kendi içinde bulunan test durumu içerir.  (Yerleşik şablonlarından birini kullanarak oluşturulan) yepyeni bir proje sorun oluşturamıyorsanız, lütfen sorun gösteren bir projeyi zip ve hata raporu ekleyin.  Örnek Proje olabildiğince basit eklemeden önce yapın. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Burada hata karşılaşıldı, de dahil olmak üzere işletim sistemi ortamını tanımlayın ve [Xamarin sürümleri](~/cross-platform/troubleshooting/questions/version-logs.md) ve bağımlılıkları.

---

## <a name="additional-details"></a>Ek ayrıntılar

1. <a name="note-1" />[*^*](#ref-1) Diğer müşteriler aynı sorun görüyorsanız olup olmadığını doğrulayabilirsiniz ideal olarak "görünür belirtilerin" açıklaması yeterli ayrıntıları içermesi (aynı hata iletileri, aynı performans düşüşünü, bir kilitlenme aynı yığın izlemesinden _vs._ ). "İçin kesin durumlarda", aşağıdakine benzer diyebilirsiniz, iyi bir örnek olacaktır: "ı normalde sorun 75 zaman isabet yüzdesi, ancak bu bir şey değiştirirseniz sonra ı sorunu tamamen önleyebilirsiniz." Başka bir benzer bir "kesin durum" Xamarin önceki bir sürümü için eski sürüme düşürmeyi sorun vermediğinde örnektir.

1. <a name="note-2" />[*^*](#ref-2) Beklediğiniz gibi hata metni (veya herhangi bir benzersiz olarak açıklayıcı metin) genellikle en iyi arama terimleri parçacıklarıdır. Ayrıntılar eklemek veya yeni bir dosya Hoş Geldiniz eksik olan bir hata raporu ise, daha iyi rapor hata.

1. <a name="note-3" />[*^*](#ref-3) Başka bir iyi soru aynı sorun herhangi bir Java için bildirilen olduğu Objective-C ya da SWIFT uygulamalar. Bu durumda, sorunu Xamarin parçası yerine, Android veya iOS kendisini büyük olasılıkla parçası ' dir.

1. <a name="note-4" />[*^*](#ref-4) Bilgi dahil etmek için bazı örnekler:

    1. Bir proje oluştururken oluşan hataları Lütfen eklemek için tam [tanılama yapı çıktı](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) hata raporu üzerinde.
    
    1. Oluşturma veya bir iOS projesi Visual Studio'da hata ayıklama sırasında oluşan hatalar için lütfen çalıştırın _Yardım > Xamarin > Zip günlükleri_ sonra hata basarsa ve sonuçta elde edilen .zip dosyası hata raporunda içerir.
    
    1. Özel durumlar veya çökme (Crash), Android veya iOS uygulamaları için lütfen ilgili dahil "[hata ayıklama günlüklerini Xamarin.Android ve Xamarin.iOS uygulamaları için](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) Mümkünse, belirli sorun için bir mükemmel az sayıdaki dosyanın özgün çözümünüzden yepyeni bir çözüme ekleyerek sorunu yeniden seçenektir. Xamarin takım genellikle bile büyük test durumları (yeniden oluşturma adımları açıkça açıklanacak varsayılarak), ancak en iyi hatayı hızla çözümlenir fırsat daha basit test çalışmaları verin üzerinde sorunları araştırmak mümkün olacaktır.


1. <a name="note-6" />[*^*](#ref-6) Eğer öyleyse _değil_ olası yepyeni bir çözüme dosyaları az sayıda ekleyerek sorunu yeniden oluşturmak, sonra zip ve tam uygulamanız için tam Çözüm klasörü ekleyin. Lütfen silin `bin`, `obj`, `Components`, ve `packages` daha küçük dosya zip yapmak için klasörler. (IDE ve yapı işlemi genellikle geri yüklemek veya bu klasörlerin içeriğini gerektiği gibi yeniden oluşturun.) Sonuçta elde edilen çözüm hala özgün sorun gösterilmektedir sürece da istediğiniz gibi sayıda kod ve kaynak dosyalarından proje silebilirsiniz.

