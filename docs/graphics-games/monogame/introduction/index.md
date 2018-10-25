---
title: MonoGame ile oyun geliştirmeye giriş
description: Bu çok bölümlü izlenecek MonoGame kullanarak basit 2B bir uygulamanın nasıl oluşturulacağını gösterir.  Ortak oyun kapsayan giriş, grafik gibi programlama kavramları, varlık ve fizik oyun.
ms.prod: xamarin
ms.assetid: D781401F-7A96-4098-9645-5F98AEAF7F71
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 4ab98d59bc74672f9531f4dbd3c33a6270582612
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "33920812"
---
# <a name="introduction-to-game-development-with-monogame"></a>MonoGame ile oyun geliştirmeye giriş

_Bu çok bölümlü izlenecek MonoGame kullanarak basit 2B bir uygulamanın nasıl oluşturulacağını gösterir.  Ortak oyun kapsayan giriş, grafik gibi programlama kavramları, varlık ve fizik oyun._

Bu makalede, platformlar arası oyunlar oluşturmaya yönelik MonoGame API teknoloji açıklanır. Platformları tam bir listesi için bkz. [MonoGame Web sitesi](http://www.monogame.net/). Bu öğreticide kullanacağınız C# kod örnekleri için MonoGame ile tam olarak işlevsel olmasına rağmen F# de.

Platformlar arası MonoGame, donanım hızlandırılmış grafik, ses, oyun durum yönetimi, giriş ve bir içeriği ardışık düzeni varlıklarını içeri aktararak için sağlama API'si. Çoğu oyun altyapıları, MonoGame sağlamaz veya herhangi bir desen veya proje yapı.  Bu geliştiriciler istedikleri kodlarını düzenlemek ücretsiz olduğu anlamına gelir, ancak bu da kurulum kodu biraz önce yeni bir proje başlatılırken gerektiğini anlamına gelir.

Bu kılavuzun ilk bölümünde, boş bir projeyi ayarlama odaklanır. Son bölümde tüm oyun mantığı ve içerik, en çok hangi platformlar arası olacaktır – yazma kapsar.

Bu izlenecek yol sonunda player animasyonlu bir karakter dokunma girişi ile burada denetleyebilirsiniz basit bir oyun oluşturduk.  Bu (koşullar kaybeder veya win Hayır olduğundan) Teknik tam oyun olmamasına karşın, çok sayıda oyun geliştirme kavramları göstermektedir ve birçok türdeki oyunlar için temel olarak kullanılabilir. 

Bu izlenecek yolda sonucu aşağıda gösterilmektedir:

![Örnek oyun karakteri fare aşağıdaki animasyonu](images/image1.gif)

## <a name="monogame-and-xna"></a>Monogame ve XNA

MonoGame kitaplığı, Microsoft'un XNA kitaplıkta söz dizimi hem işlevselliğini taklit etmek üzere tasarlanmıştır.  Tüm MonoGame nesneleri ile herhangi bir değişiklik MonoGame kullanılacak çoğu XNA kodu verme Microsoft.Xna ad alanı altında– mevcut. 

Geliştiriciler ile XNA tanıdık zaten MonoGame'nın söz dizimi ile tanıdık gelecektir ve MonoGame ile çalışma hakkında ek bilgi isteyen geliştiriciler mevcut çevrimiçi XNA izlenecek yollar, API belgeleri ve tartışmalar başvurmak mümkün olacaktır.


## <a name="walkthrough-parts"></a>İzlenecek yol bölümleri

- [Bölüm 1-platformlar arası MonoGame projesi oluşturma](~/graphics-games/monogame/introduction/part1.md)
- [Bölüm 2-Walkinggame'i uygulama](~/graphics-games/monogame/introduction/part2.md)

## <a name="related-links"></a>İlgili bağlantılar

- [Walkinggame'i MonoGame projesi (örnek)](https://developer.xamarin.com/samples/mobile/WalkingGameMG/)
- [İOS XNB yazı tipleri](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Content/fonts)
- [XNB yazı tipleri Android](https://github.com/mono/CocosSharp/tree/master/Samples/GameStarterKit/GameStarterKit/Assets/Content/fonts)
- [NuGet Android'de MonoGame](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [NuGet MonoGame iOS](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API belgeleri](http://www.monogame.net/documentation/?page=main)
