---
title: İzlenecek yol - Apple'nın Instruments aracını kullanma
description: Bu makalede, Xamarin ile oluşturulan bir iOS uygulamasına bellek sorunlarını tanılamak için Apple'nın Instruments aracını kullanmayı açıklar. Bu başlatma araçları, yığın anlık bellek artışı ve daha fazlasını Analiz işlemini göstermektedir.
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a751488b4063a594904393faa605d36c3414d2ec
ms.sourcegitcommit: 021027b78cb2f8061b03a7c6ae59367ded32d587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39182188"
---
# <a name="walkthrough---using-apples-instruments-tool"></a>İzlenecek yol - Apple'nın Instruments aracını kullanma

_Bu makalede, Xamarin ile oluşturulan bir iOS uygulamasına bellek sorunlarını tanılamak için Apple'nın Instruments aracını kullanma konusunda yol göstermektedir. Bu başlatma araçları, yığın anlık görüntülerini alabilir ve bellek artışı analiz etmek nasıl gösterir. Ayrıca, görüntülemek ve bellek sorunu neden tam kod satırlarını saptamak için araçları kullanmayı gösterir._

Bu sayfada nasıl kullanılacağını gösteren **Xcode'un Instruments aracı** iOS uygulamada bir bellek sorunu tanılamak için.
İlk olarak, indirme [MemoryDemo örnek](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) açın **önce** Mac için Visual Studio çözümü

## <a name="diagnosing-the-memory-issues"></a>Bellek sorunlarını tanılama

1. Mac için Visual Studio'dan başlatmak **Instruments** gelen **Araçlar > başlatma Araçları** menü öğesi.
2. Uygulamanın cihaza seçerek karşıya **çalıştırın > cihaza Yükle** menü öğesi.
3. Seçin **ayırmaları** şablonu (Turuncu simge beyaz kutu ile)

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Ayırmaları şablonu seçin")

4. Seçin **bellek tanıtım** uygulama **için profil oluşturma bir şablon seçin:** pencerenin üst kısmındaki liste. İOS cihazında ilk gösterir uygulamaları yüklü menüyü genişletmek için tıklayın.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Bellek Demo uygulamasını seçin")

5. Tuşuna **Seç** başlatmak için (penceresinin sağ alt) düğmesini **Instruments**. Üst bölmede ThiJs şablonu iki öğeyi gösterilir: ayırma ve VM İzleyicisi.

6. Tuşuna **kayıt** düğmesine (sol üst kırmızı daire) uygulama başlatılır araçları.

7. Seçin **VM İzleyicisi** satır üst bölmede (uygulamanın çalıştığı, iki bölümü içerecek: kirli ve yerleşik boyutu). İçinde **denetçisi** bölmesinde seçin **görüntü ayarlarını göster** seçeneği (dişli simgesi) sonra değer çizgisi **otomatik olarak anlık görüntüsünün alınması** bu sağ içinde gösterilen onay kutusunu Ekran:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Dişli simgesini görüntüleme ayarlarını göster seçeneğini belirtin, sonra otomatik olarak anlık görüntüsünün alınması onay kutusunu işaretleyerek")

8. Seçin **ayırmaları** satır üst bölmede (uygulamanın çalıştığı, Yazar *tüm yığın ve anonim VM*)
9. İçinde **denetçisi** bölmesinde seçin **görüntü ayarlarını göster** seçeneğini (dişli simgesi) sonra ENTER tuşuna tıklayın **işareti nesil** bir taban çizgisi oluşturmak için düğme. Küçük bir kırmızı bayrak pencerenin üstünde bir zaman çizelgesi görünür
10. Uygulama kaydırın ve ardından **işareti nesil** yeniden (birkaç kez yineleyin)
11. Tıklayın **Durdur** düğmesi.
12. Genişletin **nesil** düğümle en büyük **büyüme** ve sıralama ölçütü **büyüme** (Azalan).
13. Değişiklik **denetçisi** bölmesine **genişletilmiş Ayrıntı Göster** gösterilir ("E"), hangi **yığın izlemesi**.

14. Bildirim  **&lt;nesne olmayan >** düğüm, aşırı bellek artışı gösterir. Daha fazla bilgi-yığın izlemeyle uygulamanızda eklemek için sağ tıklayın, bu düğüm yanındaki oka tıklayın **kaynak konumu** bölmesine:

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "Kaynak konumu bölmesine eklemek")

15. Sıralama ölçütü **boyutu** ve görüntüleme **genişletilmiş ayrıntı** görüntüle:

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Sıralama ölçütü: boyutunu ve görünümünü genişletilmiş ayrıntı görünümü")

16. İstenen girişi çağrı yığınında ilgili kodu görmek için tıklayın:

    ![](walkthrough-apples-instrument-images/05-related-code.png "İlgili kodu görüntüleme")

Bu durumda, yeni bir görüntü oluşturulur ve her hücre ya da mevcut koleksiyon görünümü yeniden kullanılıyor hücreleri olan bir koleksiyondaki depolanır.

## <a name="resolving-the-memory-issues"></a>Bellek sorunlarını çözme

Bu sorunları çözmek ve uygulama araçları yeniden çalıştırmak mümkündür.

Tek bir örnek sınıf düzeyinde bildirmek, görüntüyü yeniden kullanılabilir ve hücre nesnesi olan yerine mevcut bir havuzdan aşağıda gösterilen her zaman oluşturulan yeniden kullanılabilir:

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

Şimdi uygulamayı çalıştırdığınızda, bellek kullanımı önemli ölçüde azalır-- **büyüme** kod düzeltmeden önce olduğu gibi Nesilleri arasında artık Kib (KB) içinde MIB (megabayt cinsinden) yerine ölçülür:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Uygulama bellek kullanımını gösteren")

Geliştirilmiş kod kullanılabilir [MemoryDemo örnek](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/) içinde **sonra** Mac için Visual Studio çözümü

Bu topluluk blogu hakkında [Xamarin.iOS çöp toplama](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/) Xamarin.iOS bellek sorunlarıyla ilgilenmek için kullanışlı bir başvurudur.

## <a name="summary"></a>Özet

Bu makalede Instruments bellek sorunlarını tanılamak için nasıl kullanılacağı gösterilmektedir.
Mac için Visual Studio içinden gelen araçları başlatın, bellek ayırma şablon yükleme ve kullanım anlık bellek sorunlarını saptamak için adım açıklanmıştır.
Son olarak, uygulama sorun düzeltilmiştir doğrulamak için yeniden examined.

## <a name="related-links"></a>İlgili bağlantılar

- [MemoryDemo örnek](https://developer.xamarin.com/samples/monotouch/Profiling/MemoryDemo/)
- [Xamarin.iOS çöp toplama (blog gönderisi)](http://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
