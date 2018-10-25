---
title: Bölüm 1-platformlar arası MonoGame oluşturma
description: Bu kılavuz, iOS ve Android MonoGame kullanarak yeni bir proje oluşturma işlemi gösterilmektedir. Her platform için bir proje yanı sıra, platformlar arası paylaşılan kod proje çözüm Mac için Visual Studio'nun bir sonucudur. Bu proje, yürütme sırasında boş bir mavi ekran görüntülenir.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 82b1408cafedf98a8619e8e039ba00b332f74516
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "33921995"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Bölüm 1-platformlar arası MonoGame oluşturma

_Bu kılavuz, iOS ve Android MonoGame kullanarak yeni bir proje oluşturma işlemi gösterilmektedir. Her platform için bir proje yanı sıra, platformlar arası paylaşılan kod proje çözüm Mac için Visual Studio'nun bir sonucudur. Bu proje, yürütme sırasında boş bir mavi ekran görüntülenir._

MonoGame ile kod yeniden kullanımını büyük kısmı platformlar arası oyunlar geliştirilmesini sağlar. Bu izlenecek yol, bir paylaşılan kod projesi için platformlar arası kod yanı sıra, iOS ve Android için projeleri içeren bir çözümü kurma üzerinde odaklanır.

İşimiz bittiğinde, biz oyun güncelleştirme mantığı gerçekleştirmek için doğru yapıya sahip bir proje olan ve saniyede 30 kare, mantıksal çizim oyun. Tüm MonoGame projesi için projenin temel kullanılabilir. Projemizin yürütüldüğünde şöyle görünür:

![Boş mavi ekran](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Mac için Visual Studio'ya MonoGame ekleme

MonoGame bir eklenti Visual Studio için Mac için eklenebilir Mac üzerinde seçin **Mac için Visual Studio** > **Eklenti Yöneticisi...**  . Windows, select ** Araçlar üzerinde ** > **Eklenti Yöneticisi...**  . Seçin **galeri** sekmesinde, genişletin **oyun geliştirme** kategori seçip alt **MonoGame eklentisi**, ardından **yükleme**:

![Visual Studio Mac uzantılar Galerisi MonoGame seçmek için](part1-images/image2.png)

> [!IMPORTANT]
> **Not**: varsa **oyun geliştirme** bölümü Eklenti Yöneticisi'nde görünmez, el ile indirin ve en son sürümünü yükleyin: http://www.monogame.net/downloads/. Görüntülenecek şablonları için Mac için Visual Studio'yu yeniden başlatmanız gerekebilir.

Sonraki bölümde göreceğiz şekilde MonoGame şablonları yüklendikten sonra Mac için Visual Studio'da görünür.

## <a name="creating-a-new-solution"></a>Yeni bir çözüm oluşturma

Mac seçin için Visual Studio'daki **Dosya > Yeni Çözüm**. İçinde **yeni proje** iletişim kutusunda, tıklayarak **çeşitli**, kaydırarak **genel** bölümünden ** Evrensel MonoGame mobil uygulama ** seçeneğini ve İleri'ye tıklayın.

![Yeni Proje iletişim kutusu MonoGame uygulaması oluşturma](part1-images/image3.png)

Walkinggame'i Projeyi adlandırın ve Oluştur'a tıklayın:

![Bir ad ve Konum çekme yeni proje iletişim kutusu](part1-images/image4.png)

Artık Projemizin yalnızca başka iOS veya Android projesi gibi yürütülür. Proje kantaron mavi arka plan görüntüleme çalıştırmanız gerekir:

![Boş mavi uygulama arka plan](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android derleme hataları düzeltme

MonoGame'nın Şablonları'nın geçerli sürümü Android içinde birkaç söz dizimi hataları içeren `Activity1.cs` dosya. Bu sorunları gidermek için değiştirin `OnCreate` işlevi aşağıdaki:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>Özet

Bu kılavuzda ele Mac için Visual Studio kullanarak bir platformlar arası MonoGame projesi oluşturma Bunun sonucunda, boş bir mavi ekrandır. Bu proje, herhangi bir iOS ve Android oyun için başlangıç noktası olarak kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API belgeleri](http://www.monogame.net/documentation/?page=main)
