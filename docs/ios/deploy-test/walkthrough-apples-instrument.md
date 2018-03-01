---
title: "İzlenecek yol - Apple'nın Instruments aracını kullanma"
description: "Bu makalede Apple'nın Instruments aracının Xamarin ile oluşturulan bir iOS uygulaması bellek sorunları tanılamak için nasıl kullanılacağı anlatılmaktadır. Araçları başlatın, yığın anlık görüntülerini almak ve bellek büyüme çözümlemek nasıl gösterir. Ayrıca, Instruments görüntülemek ve bellek soruna neden tam satır kod sabitleme için nasıl kullanılacağını gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 949a2ea4d5838b664e19251e69a32efccc98d496
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-apples-instruments-tool"></a>İzlenecek yol - Apple'nın Instruments aracını kullanma

_Bu makalede Apple'nın Instruments aracının Xamarin ile oluşturulan bir iOS uygulaması bellek sorunları tanılamak için nasıl kullanılacağı anlatılmaktadır. Araçları başlatın, yığın anlık görüntülerini almak ve bellek büyüme çözümlemek nasıl gösterir. Ayrıca, Instruments görüntülemek ve bellek soruna neden tam satır kod sabitleme için nasıl kullanılacağını gösterir._

Bu sayfayı nasıl kullanılacağı ortaya **Xcode'nın Instruments aracı** bir iOS uygulaması bir bellek sorunu tanılamak için.
İlk olarak, indirme [MemoryDemo örnek](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) açarak **önce** Mac için Visual Studio'da Çözüm

## <a name="diagnosing-the-memory-issues"></a>Bellek sorunları tanılama

1.  Mac için Visual Studio'dan başlatma **Instruments** gelen **Araçlar > başlatma Instruments** menü öğesi.
2.  Seçerek aygıta uygulama karşıya **Çalıştır > cihaza karşıya** menü öğesi.
3.  Seçin **ayırmaları** şablonu (Turuncu simge beyaz kutu ile)

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Ayırmalar şablonunu seçin")

4.  Seçin **bellek Demo** uygulama **için bir profil şablonu seçin:** pencerenin üstündeki listesi. İOS cihazında ilk gösterir yüklü uygulamaların menü genişletmek için tıklatın.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Bellek Demo uygulamayı seçin")

5.  Tuşuna **Seç** başlatmak için (penceresinin sağ alt) düğmesini **Instruments**. Üst bölmede ThiJs şablonu iki öğeyi gösterir: ayırma ve VM İzleyicisi.

6.  Tuşuna **kaydı** uygulama başlatacak Instruments düğmesini (kırmızı daire sol üst).

7.  Seçin **VM İzleyicisi** üst bölmede satır (uygulama çalışırken, bu iki bölüm içerir: kirli ve yerleşik boyutu). İçinde **denetçisi** bölmesinde seçin **görüntü ayarlarını göster** seçeneği (dişli simgesi) sonra değer çizgisi **otomatik olarak anlık görüntüsünün alınması** bu sağ içinde gösterilen onay kutusunu Ekran:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Dişli simgesini görüntüleme ayarları göster seçeneğini belirtin, sonra otomatik olarak anlık görüntüsünün alınması onay kutusunu değer")

8.  Seçin **ayırmaları** üst bölmede satır (uygulama çalışırken, söyleyin *tüm öbek ve anonim VM*)
9.  İçinde **denetçisi** bölmesinde seçin **görüntü ayarlarını göster** seçeneği (dişli simgesi) ardından tuşuna tıklayın **işareti oluşturma** düğmesine bir taban çizgisi oluşturmak için. Küçük bir kırmızı bayrak pencerenin üstündeki zaman çizelgesi görünür
10.  Uygulama kaydırın ve ardından **işareti nesil** yeniden (birkaç kez yineleyin)
11.  Tıklatın **durdurmak** düğmesi.
12.  Genişletme **oluşturma** düğümle en büyük **büyüme** ve sıralama ölçütü **büyüme** (Azalan).
13.  Değişiklik **denetçisi** bölmesine **genişletilmiş Ayrıntı Göster** ("E"), gösterir **yığın izleme**.

14.  Bildirim **< nesnesi olmayan >** düğümü aşırı bellek büyüme gösterir. Daha fazla ayrıntı görmek - sağ tıklayın eklemek için yığın izlemesi için bu düğümü yanındaki oka tıklayın **kaynak konumu** bölmesine:

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "Kaynak konumu için bölmesi ekleme")

15.  Sıralama ölçütü **boyutu** ve görüntüleme **genişletilmiş ayrıntı** görünümü:

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Boyuta göre sıralayın ve genişletilmiş ayrıntı Görünüm")

16.  İlgili kod görmek için çağrı yığınında istenen girişi tıklatın:

    ![](walkthrough-apples-instrument-images/05-related-code.png "İlgili kodu görüntüleme")

Bu durumda, yeni bir görüntü oluşturulur ve bir koleksiyon için her hücre ya da varolan bir koleksiyon görünümü yeniden kullanılıyor hücreleri olan depolanır.

## <a name="resolving-the-memory-issues"></a>Bellek sorunlarını çözme

Bu sorunları gidermek ve uygulama araçları yeniden mümkündür.

Tek bir örnek için sınıf düzeyinde bildirme, görüntüyü yeniden kullanılabilir ve hücre nesnesini olmak yerine var olan bir havuzundan aşağıda gösterildiği gibi her zaman oluşturulan yeniden kullanılabilir:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Dequeue a cell from the reuse pool
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);

    // Reuse the image declared at the class level
    imageCell.ImageView.Image = image;

    return imageCell;
}
```

Şimdi uygulamayı çalıştırdığınızda, bellek kullanımını önemli ölçüde azalır-- **büyüme** kodu düzeltmeden önce haliyle nesli arasında şimdi Kib (KB) (megabayt cinsinden) MIB yerine ölçülür:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Uygulama bellek kullanımı gösterme")

Geliştirilmiş kod kullanılabilir [MemoryDemo örnek](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) içinde **sonra** Mac için Visual Studio'da Çözüm

Bu topluluk Web günlüğü hakkında [Xamarin.iOS çöp toplama](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/) Xamarin.iOS bellek sorunlarıyla ilgilenmek için yararlı bir başvuru.


## <a name="summary"></a>Özet

Bu makalede Instruments bellek sorunları tanılamak için nasıl kullanılacağı gösterilmektedir.
Mac için gelen araçları Visual Studio içinde başlatın, yük bellek ayırma şablon ve kullanım anlık bellek sorunları belirlemenize adım açıklanmıştır.
Son olarak, uygulama sorun giderilmiştir doğrulamak için yeniden examined.


## <a name="related-links"></a>İlgili bağlantılar

- [MemoryDemo örnek](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Xamarin.iOS çöp toplama](https://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/)
