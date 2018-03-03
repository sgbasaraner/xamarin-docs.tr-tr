---
title: "MonoGame oyun geliştirmeye giriş"
description: "Bu çok parçalı izlenecek MonoGame kullanarak basit bir 2B uygulamasının nasıl oluşturulacağını gösterir.  Ortak oyunun kapsadığı giriş, grafik gibi programlama kavramları oyun varlıkları ve fizik."
ms.topic: article
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 8161920139413cd1b28adcebaf56e6d8265120ef
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-game-development-with-monogame"></a>MonoGame oyun geliştirmeye giriş

_Bu çok parçalı izlenecek MonoGame kullanarak basit bir 2B uygulamasının nasıl oluşturulacağını gösterir.  Ortak oyunun kapsadığı giriş, grafik gibi programlama kavramları oyun varlıkları ve fizik._

Bu makalede, platformlar arası oyunlar sağlama MonoGame API teknolojisi açıklanmaktadır. Platformlar tam bir listesi için bkz: [MonoGame Web sitesi](http://www.monogame.net/). MonoGame de F # ile tam olarak işlevsel olmasına rağmen C# kod örnekleri için Bu öğreticide kullanacaksınız.

MonoGame bir çapraz platform, donanım varlıkları içeri aktarmak için grafik, ses, oyun durum yönetimi, giriş ve içerik işlem hattı sağlama API hızlandırılmış. Çoğu oyun motoru MonoGame sağlamaz veya herhangi bir desen veya proje yapısını zorunlu tuttukları.  Bu geliştiricileri kendi kodlarını istedikleri gibi düzenlemek ücretsiz anlamına gelmekle birlikte, bu da kurulum kodu biraz önce yeni bir proje başlatırken gerekli anlamına gelir.

Bu kılavuzda ilk bölümü boş bir projeyi ayarlama konusunda odaklanır. Son kısım, tüm bizim oyun mantığı ve hangisinin platform en olacağını içerik – yazma kapsar.

Bu örneklerde sonuna player dokunmatik giriş animasyonlu bir karakterle burada denetleyebilirsiniz basit bir oyun oluşturduk.  Bu (koşullar kaybeder veya win Hayır olduğundan) Teknik olarak tam oyun olmamasına karşın, çok sayıda oyun geliştirme kavramları gösterir ve temel olarak türlerde oyunlar için kullanılabilir. 

Bu kılavuzda sonucunu gösterir:

![](images/image1.gif "Kılavuzda yerleşik uygulama")

# <a name="monogame-and-xna"></a>Monogame ve XNA

MonoGame kitaplığı, Microsoft'un XNA kitaplıkta sözdizimi ve işlevsellik taklit etmek üzere tasarlanmıştır.  Hiçbir değişikliğe sahip MonoGame içinde kullanılacak çoğu XNA kodu izin vererek Microsoft.Xna ad alanı – altındaki tüm MonoGame nesneleri vardır. 

XNA ile hakkında bilgi sahibi geliştiriciler önceden MonoGame'nın sözdizimi hakkında bilginiz olması gerekir ve MonoGame ile çalışma hakkında ek bilgi isteyen geliştiriciler mevcut çevrimiçi XNA izlenecek yollar, API belgelerine ve tartışmaları başvuru mümkün olacaktır.


# <a name="walkthrough-parts"></a>İzlenecek yol bölümleri

- [Bölüm 1 – platformlar arası MonoGame proje oluşturma](~/graphics-games/monogame/introduction/part1.md)
- [Bölüm 2 – WalkingGame uygulama](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>İlgili bağlantılar

- [WalkingGame MonoGame proje (örnek)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [XNB yazı tiplerini iOS](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB yazı tiplerini Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [NuGet MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet MonoGame iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API belgeleri](http://www.monogame.net/documentation/?page=main)
