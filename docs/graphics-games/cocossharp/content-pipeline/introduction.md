---
title: İçerik ardışık düzen giriş
description: Ardışık Düzen uygulamalar veya uygulamaları bölümleri olan içerik, bu dosyaları oyun projeler tarafından yüklenen bir biçime dönüştürmek için kullanılır. MonoGame içeriği ardışık düzen CocosSharp ve MonoGame projelerin dosyalarını dönüştürmek için bir özel içerik ardışık düzen uygulamasıdır.
ms.prod: xamarin
ms.assetid: 40628B5F-FAF7-4FA7-A929-6C3FEA83F8EC
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: a369c5ba61033eb61c0f188c03b21e08c71784fb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33922841"
---
# <a name="introduction-to-content-pipelines"></a>İçerik ardışık düzen giriş

_Ardışık Düzen uygulamalar veya uygulamaları bölümleri olan içerik, bu dosyaları oyun projeler tarafından yüklenen bir biçime dönüştürmek için kullanılır. MonoGame içeriği ardışık düzen CocosSharp ve MonoGame projelerin dosyalarını dönüştürmek için bir özel içerik ardışık düzen uygulamasıdır._

Bu makale üzerinde öncelikle odaklanan içerik ardışık kavramsal anlayış sağlar *MonoGame içeriği ardışık düzen*, CocosSharp ve MonoGame ile kullanılan bir içerik ardışık düzen uygulaması olduğu.


## <a name="what-is-a-content-pipeline"></a>İçerik ardışık düzeni nedir?

Terim *içerik ardışık düzen* bir dosyayı bir biçimden diğerine dönüştürme işlemi için genel bir terimdir. *Giriş* içerik kanalı genellikle Photoshop görüntü dosyaları gibi bir yazma aracı tarafından yüzdelik bir dosyasıdır. İçerik ardışık düzen oluşturur *çıkış* dosyasını doğrudan oyun projesi tarafından yüklenen bir biçimde. Genellikle çıktı dosyaları hızlı yükleme için en iyi duruma getirilir ve disk boyutu azalır.

İçerik ardışık düzen komut satırı yürütülebilir dosyaları, ayrılmış GUI tabanlı uygulamalar veya eklenti Visual Studio gibi başka bir uygulamada katıştırılmış. Oyun yürütülmeden önce içerik ardışık düzen genellikle çalıştırın. İçerik ardışık düzen Visual Studio gibi bazı uygulama ile ilişkili ise, derleme zamanı sırasında yürütür. İçerik ardışık düzen başına bir uygulamadır, sonra da bunu yapmak için kullanıcı tarafından açıkça belirtilmediği zaman çalıştırabilirsiniz. Uygulama veya belirli bir giriş dosyası dönüştürmek için sorumlu mantığı (gibi bir **.png**) ilişkili bir çıktı dosyası olarak adlandırılır bir *Oluşturucu*. 

Biz çalışma zamanında şu şekilde yüklenmesi için yazma bir dosyayı alır yolu görselleştirebilirsiniz:

![](introduction-images/image1.png "Bu diyagramda çalışma zamanında yüklenen için yazma bir dosyayı alır yolu görselleştirilen")

## <a name="why-use-a-content-pipeline"></a>İçerik bir ardışık düzen neden kullanılır?

İçerik ardışık düzen derleme sürelerini artırır ve geliştirme sürecinin karmaşıklık eklemek yazarlık ve oyun arasında ek bir adım tanıtır. Bu noktalar rağmen içerik ardışık düzen oyun geliştirme için çok sayıda fayda tanıtmaktadır:


### <a name="converting-to-a-format-understood-by-the-game"></a>Oyunu tarafından anlaşılan bir biçime dönüştürme

CocosSharp ve MonoGame çeşitli türlerde içerik yüklenmesi için yöntemleri sağlar; Ancak, içeriği düzgün yüklenmiş önce biçimlendirilmiş olması gerekir. Çoğu içerik türlerini dönüştürme yüklenen önce bazı türü gerektirir. Örneğin, ses efekti **.wav** biçimi dönüştürülür, içine bir **.xnb** CocosSharp ve MonoGame yüklenmesini desteklemez olduğundan çalışma zamanında yüklenecek dosyayı **.wav** dosya biçimi.


### <a name="converting-to-a-format-native-to-the-hardware"></a>Donanım için yerel bir biçime dönüştürme

Farklı donanım içerik çalışma zamanında farklı davranabilir. Örneğin, CocosSharp oyunlar resim dosyalarını oluştururken yükleyebilirsiniz bir `CCSprite` örneği. İOS ve Android dosyaları yüklemek için aynı kodu kullanılabilse de, her platform yüklenen dosya farklı bir şekilde depolar. Sonuç olarak MonoGame içerik ardışık düzen doku biçimleri **.xnb** dosyaları hedef platforma bağlı olarak farklı.


### <a name="reducing-size-on-disk"></a>Disk boyutunu azaltma 

Ardışık Düzen bilgileri kaldırmak için kullanılan içerik, yazar zaman yararlı olur, ancak çalışma zamanında gerekli olduğu. Özgün (giriş) dosyasını varolan içeriği korumak içeriği oluşturucuların yardımcı olabilecek tüm bilgileri depolayabilirsiniz ancak çıktı dosyası genel oyun dosya küçük tutmak için stripped-down olabilir. Bu faktör özellikle indirilen yerine yükleme medyasında dağıtılmış mobil oyunlar için kullanışlıdır.


### <a name="reducing-load-time"></a>Yükleme süresini azaltma

Oyunlar görselleri iyileştirmek veya yeni özellikler eklemek için çalışma zamanı performansını iyileştirmek için içeriğin değişiklikler gerektirebilir. Örneğin birçok 3B oyun bir kez ışık hesaplamak ve sonra bu hesaplamanın sonucu karmaşık planda işlenirken kullanın. Oyun yapılandırıldığında yükleme içeriği şekilde basımı karşılamayacak kadar pahalı olabilir, bu hesaplamaları itibaren hesaplama yerine gerçekleştirilebilir. Sonuçta elde edilen hesaplamalar aksi mümkün olandan çok daha hızlı yüklenmesi içeriği etkinleştirme içeriğinde dahil edilebilir. 


## <a name="xnb-file-extension"></a>xnb dosya uzantısı

**.Xnb** dosya uzantısıdır Monogame içerik ardışık düzen tarafından yüzdelik tüm dosya uzantısı. Bu, Microsoft XNA ait içerik ardışık düzen tarafından yüzdelik dosya uzantısı ile eşleşir.

**.Xnb** uzantısı bağımsız olarak özgün dosya türü kullanılır. Diğer bir deyişle, görüntü dosyaları (**.png**), ses dosyaları (**.wav**), ve herhangi bir özel dosya türünün tüm olarak yüzdelik **.xnb** dosyaları içerik kanalı yoluyla geçirildiğinde. Farklı bir dosya uzantısı ayırt etmek için kullanılamaz olduğundan sonra Yük CocosSharp ve MonoGame yöntemleri biçimlendirir. **.xnb** dosyaları değil beklediğiniz uzantıları dosya yüklenirken.

CocosSharp ve MonoGame .xnb dosyaları oluşturulabilir ve bu konu ele Monogame ardışık düzen aracını kullanarak [bu kılavuzda](~/graphics-games/cocossharp/content-pipeline/walkthrough.md).


## <a name="summary"></a>Özet

Bu makalede genel bir bakış ve avantajların yanı sıra içerik ardışık düzen, genel olarak, MonoGame içeriği ardışık düzen giriş yanı sıra sağlanan.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame ardışık düzen belgeleri](http://www.monogame.net/documentation/?page=Pipeline)
