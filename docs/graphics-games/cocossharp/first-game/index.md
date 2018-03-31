---
title: CocosSharp oyun geliştirmeye giriş
description: Bu çok parçalı izlenecek CocosSharp kullanarak basit bir 2B oyun oluşturulacağını gösterir. Grafik, giriş ve fizik gibi ortak oyun programlama kavramları kapsar.
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA99A61-A48D-4207-9A35-4C62F10C9AA5
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 5ab6f68aed791dd21516d663367ac5435e92d6cc
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="introduction-to-game-development-with-cocossharp"></a>CocosSharp oyun geliştirmeye giriş

_Bu çok parçalı izlenecek CocosSharp kullanarak basit bir 2B oyun oluşturulacağını gösterir. Grafik, giriş ve fizik gibi ortak oyun programlama kavramları kapsar._

Platformlar arası oyunlar yapmak için teknolojisi CocosSharp 2B oyun altyapısı sağlar. Desteklenen platformlar tam listesi için bkz: [CocosSharp wiki github'da](https://github.com/mono/CocosSharp/wiki). CocosSharp de F # ile tam olarak işlevsel olmasına rağmen C# kod örnekleri için Bu öğreticide kullanacaksınız.

CocosSharp çekirdek tarafından sağlanan [MonoGame framework](http://www.monogame.net/), kendisini bir platformlar arası, donanım hızlandırılmış API varlıklar içeri aktarmak için grafik, ses, oyun durum yönetimi, giriş ve içerik işlem hattı sağlama. CocosSharp iyi 2B oyunlar için uygun bir verimli Soyutlama Katmanı ' dir. Ayrıca, oyunlar karmaşıklığı büyüdükçe büyük oyunlar kendi çekirdek kitaplıkları dışında kendi iyileştirmeler gerçekleştirebilirsiniz. Diğer bir deyişle, geliştiricilerin hızla oyun boyutu veya karmaşıklık sınırlamaksızın başlamak etkinleştirme kullanım kolaylığı ve performans, bir karışımını CocosSharp sağlar.

Bu izlenecek yol odağı boş proje ayarlama ilk bölümü.  İkinci bölümü, tüm oyun mantığımızı yazma kapsar. 

Bu örneklerde sonuna oyuncunun hedef ekran dışına dönmeden Top tutmak yatay olarak çalışmak için bir raket slayt olduğu basit bir oyun oluşturduk. Her Sıçrama oyuncunun puan bir nokta artmasına neden olur.

![](images/image1.png "Her Sıçrama oyuncunun puan bir nokta artmasına neden olur")

# <a name="walkthrough-parts"></a>İzlenecek yol bölümleri

* [Bölüm 1 – CocosSharp proje oluşturma](~/graphics-games/cocossharp/first-game/part1.md)
* [Bölüm 2 – BouncingGame uygulama](~/graphics-games/cocossharp/first-game/part2.md)

## <a name="related-links"></a>İlgili bağlantılar

- [Oyun içeriği (örnek)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
- [Projeyi (örnek)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [NuGet üzerinde CocosSharp PCL](http://www.nuget.org/packages/CocosSharp.PCL.Shared/)
- [CocosSharp API belgeleri](https://developer.xamarin.com/api/namespace/CocosSharp/)
