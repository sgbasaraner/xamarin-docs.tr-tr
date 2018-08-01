---
title: Bölüm 1 – platformlar arası MonoGame oluşturma
description: Bu kılavuz, iOS ve Android MonoGame kullanarak yeni bir proje oluşturmak gösterilmiştir. Mac çözümüyle her platform için bir proje yanı sıra, platformlar arası paylaşılan kod projesi için Visual Studio sonucudur. Bu proje çalıştırıldığında boş bir mavi ekran görüntülenir.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bd7990b94e678c205f9ce636f4eb0d28180fc6ec
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921995"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Bölüm 1 – platformlar arası MonoGame oluşturma

_Bu kılavuz, iOS ve Android MonoGame kullanarak yeni bir proje oluşturmak gösterilmiştir. Mac çözümüyle her platform için bir proje yanı sıra, platformlar arası paylaşılan kod projesi için Visual Studio sonucudur. Bu proje çalıştırıldığında boş bir mavi ekran görüntülenir._

Platformlar arası oyunlar geliştirme kodu yeniden kullanma büyük bölümünü ile MonoGame sağlar. Bu kılavuz, bir paylaşılan kod projesi için platformlar arası kod yanı sıra iOS ve Android için projeleri içeren bir çözümü kurma üzerinde odaklanır.

Biz bittiğinde, biz sahip oyun güncelleştirme mantığı gerçekleştirmek için uygun yapısına sahip bir projeniz ve saniyede 30 kare adresindeki mantığı çizim oyun. Tüm MonoGame projesi için temel projesi olarak kullanılabilir. Projemizin çalıştırıldığında şuna benzeyecektir:

![Boş mavi ekran](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Mac için Visual Studio MonoGame ekleme

MonoGame eklenti Visual Studio'ya Mac için eklenebilir Mac üzerinde seçin **Mac için Visual Studio** > **Eklenti Yöneticisi...**  . Windows, select ** araçları ** > **Eklenti Yöneticisi...**  . Seçin **galeri** sekmesinde, genişletin **oyun geliştirme** kategori ve seçin **MonoGame eklentisi**, ardından **yükleme**:

![Visual Studio Mac uzantılar Galerisi MonoGame seçmek için](part1-images/image2.png)

> [!IMPORTANT]
> **Not**: varsa **oyun geliştirme** bölümü Eklenti Yöneticisi'nde görünmez, el ile yükle ve en son sürümünü buradan yükleyin: http://www.monogame.net/downloads/. Visual Studio Mac şablonlarının görünmesi için yeniden başlatmanız gerekebilir.

Biz sonraki bölümde göreceğiniz gibi MonoGame şablonları yüklendikten sonra Visual Studio'da Mac için görünür.

## <a name="creating-a-new-solution"></a>Yeni bir çözüm oluşturma

Mac seçin için Visual Studio'da **Dosya > Yeni bir çözüm**. İçinde **yeni proje** iletişim kutusunda, tıklatın **çeşitli**, kaydırın **genel** bölümünde, select ** Evrensel MonoGame mobil uygulama ** seçeneği ve İleri'yi tıklatın.

![Yeni Proje iletişim kutusu MonoGame uygulaması oluşturma](part1-images/image3.png)

Projeyi WalkingGame olarak adlandırın ve Oluştur'u tıklatın:

![Bir ad ve Konum çekme yeni proje iletişim kutusu](part1-images/image4.png)

Şimdi Projemizin yalnızca diğer iOS veya Android projesi gibi yürütülür. Proje cornflower mavi arka plan görüntüleme çalıştırmanız gerekir:

![Boş mavi uygulama arka plan](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Android derleme hataları çözme

Geçerli sürümü MonoGame'nın şablonları, birkaç sözdizimi hataları Android's içeren `Activity1.cs` dosya. Bu sorunları düzeltmek için yerini `OnCreate` aşağıdaki işleviyle:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>Özet

Bu kılavuzda Mac için Visual Studio kullanarak bir platformlar arası MonoGame projesi oluşturmak nasıl ele Bunun sonucunda, boş bir mavi ekrandır. Bu proje, herhangi bir iOS ve Android oyun için başlangıç noktası olarak kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API belgeleri](http://www.monogame.net/documentation/?page=main)
