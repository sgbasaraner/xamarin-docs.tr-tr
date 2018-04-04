---
title: Dokunma
description: Dokunmatik ekranlar bugünün aygıtların çoğu üzerinde kullanıcıların hızlı ve verimli şekilde doğal ve sezgisel şekilde cihazlarla etkileşime girmesine izin. Bu etkileşimi yalnızca basit dokunma algılama sınırlı değildir - hareketleri de kullanmak da mümkündür. Örneğin, tutarak yakınlaştırma hareketi kullanıcı yakınlaştırmak veya uzaklaştırmak iki parmakları ekran parçası çimdik tarafından bu çok yaygın bir örneği bulunmaktadır. Bu kılavuzda dokunma inceler ve Android hareketleri.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: d6c6d7fd02511ede84b7cfa75575d755f11bab99
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="touch"></a>Dokunma

_Dokunmatik ekranlar bugünün aygıtların çoğu üzerinde kullanıcıların hızlı ve verimli şekilde doğal ve sezgisel şekilde cihazlarla etkileşime girmesine izin. Bu etkileşimi yalnızca basit dokunma algılama sınırlı değildir - hareketleri de kullanmak da mümkündür. Örneğin, tutarak yakınlaştırma hareketi kullanıcı yakınlaştırmak veya uzaklaştırmak iki parmakları ekran parçası çimdik tarafından bu çok yaygın bir örneği bulunmaktadır. Bu kılavuzda dokunma inceler ve Android hareketleri._

## <a name="touch-overview"></a>Dokunma genel bakış

iOS ve Android dokunma işleyecek şekilde benzerdir. Her ikisini birden çok touch - ekranında iletişim birçok noktaları - ve karmaşık hareketleri destekler. Bu kılavuz bazı kavramları benzerlikler yanı sıra dokunma gerçekleştirilmesinin particularities tanıtır ve her iki platformlarda hareketleri.

Android kullanan bir `MotionEvent` touch veri ve yöntemleri için rötuşları dinlemek için Görünüm nesnesinde yalıtan nesne.

Touch veri yakalamayı yanı sıra iOS ve Android hareketleri içine rötuşları desenlerini yorumlanması için yöntemdir. Bu hareketi tanıyıcıları sırayla döndürme görüntü veya bırakma sayfasının gibi uygulamaya özgü komutlar yorumlamak için kullanılabilir. Android kolay karmaşık özel hareketler ekleme yapmak için kaynakları yanı sıra, desteklenen hareketleri sayıda sağlar.

Android veya iOS çalıştığınız olup olmadığını rötuşları ve hareketi tanıyıcıları arasında seçim kafa karıştırıcı bir olabilir. Bu kılavuz, genel olarak, tercih hareketi tanıyıcıları verilmesi gerektiğini önerir. Hareketi tanıyıcıları sorunları ve daha iyi saklama büyük ayrılması sağlayan ayrık sınıflar uygulanır. Bu mantığı yazılan kod miktarını en aza farklı görünümleri arasında paylaşmak kolaylaştırır.

Bu kılavuzda her işletim sistemi için benzer bir biçimdedir: ilk olarak, platformun dokunma API'leri sunulan ve bunlar üzerinde hangi dokunma etkileşimleri yerleşiktir foundation olduğu açıklanmaktadır. Ardından, biz hareketi tanıyıcıları – dünyasına ilk bazı ortak hareketleri keşfetme ve uygulamalar için özel hareketler oluşturma ile tamamlama tarafından daha yakından inceleyin. Son olarak, bir finger-paint programı oluşturmak için alt düzey dokunma izleme kullanılarak tek tek parmakları izlemek nasıl görürsünüz.

## <a name="sections"></a>Bölümler

-  [Android’de Dokunma Özelliği](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [İzlenecek yol: Android dokunma kullanma](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [Çok dokunma izleme](touch-tracking.md)

## <a name="summary"></a>Özet

Bu kılavuzda Android dokunma incelendi. Her iki işletim sistemleri için biz dokunmayı etkinleştir ve dokunma olaylarına yanıt öğrendiniz. Ardından, biz hareketleri ve hareketi tanıyıcıları bazıları hakkında Android ve iOS daha yaygın senaryolardan bazıları işlemek için sağladığını öğrendiniz. Özel hareketler oluşturma ve bunları uygulamalarda uygulama bileşen başvuru incelenir. Bir kılavuz eylem her işletim sistemi için kavramlar ve API gösterilen ve ayrıca tek tek parmakları izlemek nasıl gördüğünüz.



## <a name="related-links"></a>İlgili bağlantılar

- [Android Touch başlatın (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android Touch son (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [FingerPaint (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
