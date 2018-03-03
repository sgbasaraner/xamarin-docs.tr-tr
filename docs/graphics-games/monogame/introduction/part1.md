---
title: "Bölüm 1 – platformlar arası MonoGame oluşturma"
description: "Bu kılavuz, iOS ve Android MonoGame kullanarak yeni bir proje oluşturmak gösterilmiştir. Mac çözümüyle her platform için bir proje yanı sıra, platformlar arası paylaşılan kod projesi için Visual Studio sonucudur. Bu proje çalıştırıldığında boş bir mavi ekran görüntülenir."
ms.topic: article
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 0d1352b4129dc1cf8be42e813787b9b73f80cd3e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Bölüm 1 – platformlar arası MonoGame oluşturma

_Bu kılavuz, iOS ve Android MonoGame kullanarak yeni bir proje oluşturmak gösterilmiştir. Mac çözümüyle her platform için bir proje yanı sıra, platformlar arası paylaşılan kod projesi için Visual Studio sonucudur. Bu proje çalıştırıldığında boş bir mavi ekran görüntülenir._

Platformlar arası oyunlar geliştirme kodu yeniden kullanma büyük bölümünü ile MonoGame sağlar. Bu kılavuz, bir paylaşılan kod projesi için platformlar arası kod yanı sıra iOS ve Android için projeleri içeren bir çözümü kurma üzerinde odaklanır.

Biz bittiğinde, biz sahip oyun güncelleştirme mantığı gerçekleştirmek için uygun yapısına sahip bir projeniz ve saniyede 30 kare adresindeki mantığı çizim oyun. Tüm MonoGame projesi için temel projesi olarak kullanılabilir. Projemizin çalıştırıldığında şuna benzeyecektir:

![](part1-images/image1.png "Proje çalıştırıldığında şuna benzeyecektir")


# <a name="adding-monogame-to-visual-studio-for-mac"></a>Mac için Visual Studio MonoGame ekleme

MonoGame eklenti Visual Studio'ya Mac için eklenebilir Mac üzerinde seçin **Mac için Visual Studio** > **Eklenti Yöneticisi...**  . Windows, select ** araçları ** > **Eklenti Yöneticisi...**  . Seçin **galeri** sekmesinde, genişletin **oyun geliştirme** kategori ve seçin **MonoGame eklentisi**, ardından **yükleme**:

![](part1-images/image2.png "Galeri sekmesini seçin, oyun geliştirme kategorisini genişletin ve MonoGame eklentisi seçin ve ardından Yükle'yi tıklatın")

> [!IMPORTANT]
> **Not**: varsa **oyun geliştirme** bölümü Eklenti Yöneticisi'nde görünmez, el ile yükle ve en son sürümünü buradan yükleyin: http://www.monogame.net/downloads/. Visual Studio Mac şablonlarının görünmesi için yeniden başlatmanız gerekebilir.



Biz sonraki bölümde göreceğiniz gibi MonoGame şablonları yüklendikten sonra Visual Studio'da Mac için görünür.


# <a name="creating-a-new-solution"></a>Yeni bir çözüm oluşturma

Mac seçin için Visual Studio'da **Dosya > Yeni bir çözüm**. İçinde **yeni proje** iletişim kutusunda, tıklatın **çeşitli**, kaydırın **genel** bölümünde, select ** Evrensel MonoGame mobil uygulama ** seçeneği ve İleri'yi tıklatın.

![](part1-images/image3.png "Yeni Proje iletişim kutusunda, üzerinde çeşitli'ı tıklatın, genel bölümüne, select Evrensel MonoGame mobil uygulama seçeneğini gidin ve İleri'yi tıklatın")

Projeyi WalkingGame olarak adlandırın ve Oluştur'u tıklatın:

![](part1-images/image4.png "Projeyi WalkingGame olarak adlandırın ve Oluştur'u tıklatın")

Şimdi Projemizin yalnızca diğer iOS veya Android projesi gibi yürütülür. Proje cornflower mavi arka plan görüntüleme çalıştırmanız gerekir:

![](part1-images/image5.png "Proje cornflower mavi arka plan görüntüleme çalıştırmanız gerekir")


# <a name="fixing-android-compile-errors"></a>Android derleme hataları çözme

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


# <a name="summary"></a>Özet

Bu kılavuzda Mac için Visual Studio kullanarak bir platformlar arası MonoGame projesi oluşturmak nasıl ele Bunun sonucunda, boş bir mavi ekrandır. Bu proje, herhangi bir iOS ve Android oyun için başlangıç noktası olarak kullanılabilir.

## <a name="related-links"></a>İlgili bağlantılar

- [MonoGame Android NuGet](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [MonoGame iOS NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [MonoGame API belgeleri](http://www.monogame.net/documentation/?page=main)
