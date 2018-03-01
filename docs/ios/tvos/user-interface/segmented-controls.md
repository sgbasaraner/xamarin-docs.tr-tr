---
title: "Bölümlenmiş denetimleri ile çalışma"
description: "Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde bölümlenmiş denetimleriyle çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: fd31413b777e1179e7f4faf6f91f91bc6c41e82b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-segmented-controls"></a>Bölümlenmiş denetimleri ile çalışma

_Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde bölümlenmiş denetimleriyle çalışma kapsar._


Bölümlenmiş bir denetim her biri bir simge veya metin içerebilir ve kullanıcı için ilgili bir seçenek kümesi sağlamak için kullanılan doğrusal öğeleri kümesi sağlar.

[ ![](segmented-controls-images/segment01.png "Örnek segment denetimleri")](segmented-controls-images/segment01.png)

Apple bölümlenmiş denetimleri ile çalışmak için aşağıdaki önerileri vardır:

- **GB'ye sağlamak** -dikkatli GB'ye diğer arasında sağlamak üzere Al olmalıdır [odaklanabilir öğeleri](~/ios/tvos/app-fundamentals/navigation-focus.md) ve bölümlenmiş bir denetim. Tek bir Segment (değil tıklandığında) odakta olduğunda ve bunlar aslında geçerli kesimindeki başka bir odaklanabilir öğe seçmek istediğinizde kullanıcı kesimlerini yanlışlıkla değiştirebilir seçili duruma gelir.
- **İçerik filtreleme için bölünmüş görünümleri kullanma** -bölümlenmiş denetimleri bölünmüş görünümler içeriği ve filtreler arasında gezinmeyi kolaylaştırmak için tasarlanmış gibi filtreleme içerik için iyi seçenek yapma.
- **Yedi kesimleri maksimum sınır** -en fazla sekiz aşağıda Segment sayısına (8) olarak ayarlamaya çalışmanız gerekir bu gitmek için yer arasında kanepenizde ve daha kolay şuradan Ayrıştır daha kolay olur.
- **Tutarlı kesim içerik boyutu kullanmak** - tüm kesimine sahip aynı genişliğe ve, mümkünse, her bir segment ile aynı boyutta içeriği ayarlamaya çalışmanız gerekir. Bu yalnızca Segment denetimleri görsel olarak daha çok güzel yapar, ancak bir bakışta okuması daha kolay hale getirir.
- **Karıştırma simgeler ve metin kaçının** -tek tek her segmentinde bir simge veya metin ancak ikisini içerebilir. Simgeler ve aynı bölümlenmiş denetimi metinde karışık mümkün olsa da, bu kaçınılmalıdır.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>Segment simgeleri hakkında

Arama için Büyüteç gibi Segment simgeler için basit, tanınabilir görüntüleri kullanarak Apple önerir. Fazla karmaşık simgeler basit Beyanları, simgeleri sınırlamak en iyisidir TV ekranında yer tanı zordur.

Metin ve belirli bir kesim simgeleri karıştıramazsınız ve simgeler ve tek bir bölümlenmiş denetim metinde karıştırma kaçınmalısınız. Tüm simgeleri veya tüm metin olmalıdır.

<a name="Summary" />

## <a name="segment-text"></a>Segment metin

Apple Segment metni ile çalışmak için aşağıdaki önerileri sağlar:

- **Kısa, anlamlı isimleri kullanmak** -Segment başlık kullanıcının verilen Segment seçerken beklemesi gereken içerik türünü açıkça durum. Örneğin: müzik veya videolar.
- **İlk harfler büyük harf kullanın** -kesimleri başlık her sözcüğün, makaleler, bağlaç ve edatlar değerinden dört (4) karakterleri dışında katılamayacağını.
- **Kısa, odaklanmış başlıkları kullanmak** -başlıklar, kısa ve kesim seçildiğinde beklenir içerik türünü odaklanmıştır tutun.

Yeniden, metin ve belirli bir kesim simgeleri karıştıramazsınız ve simgeler ve tek bir bölümlenmiş denetim metinde karıştırma kaçınmalısınız.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>Segment denetimleri ve film şeritleri

Xamarin.tvOS uygulamada Segment denetimleri ile çalışmak için en kolay yolu, onları iOS Tasarımcısı kullanarak uygulamanın UI eklemektir.

[[ide name="xs"]]

1. İçinde **çözüm paneli**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **Segment denetim** gelen **araç** ve görünümünde bırak: 

    [ ![](segmented-controls-images/segment02.png "Bir Segment denetimi")](segmented-controls-images/segment02.png)
1. İçinde **pencere öğesi sekmesini** , **özelliği paneli**, Segment denetiminin çeşitli özellikler gibi ayarlayabilirsiniz kendi **stili** ve **durumu**: 

    [ ![](segmented-controls-images/segment03.png "Pencere öğesi sekmesi")](segmented-controls-images/segment03.png)
1. Kullanım **kesimleri** denetleyicideki bölümlerinin sayısını denetlemek için alan.
1. Belirli bir kesim gelen seçin **Segment açılır** gibi tek tek özelliklerini ayarlamak için **başlık** veya **görüntü** ve belirli bir kesim ise denetlemek için  **Etkin** veya **seçili** denetimi olduğunda görüntülenir.
1. Son olarak, Ata **adları** denetimlere için C# kodunda yanıt vermesi. Örneğin: 

    [ ![](segmented-controls-images/segment04.png "Bir ad atayın")](segmented-controls-images/segment04.png)
1. Değişikliklerinizi kaydedin.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve düzenlemek için açın.
1. Sürükleme bir **Segment denetim** gelen **araç** ve görünümünde bırak: 

    [ ![](segmented-controls-images/segment02-vs.png "Bir Segment denetimi")](segmented-controls-images/segment02-vs.png)
1. İçinde **pencere öğesi sekmesini** , **özelliği Explorer**, Segment denetiminin çeşitli özellikler gibi ayarlayabilirsiniz kendi **stili** ve **durumu**: 

    [ ![](segmented-controls-images/segment03-vs.png "Pencere öğesi sekmesi")](segmented-controls-images/segment03-vs.png)
1. Kullanım **kesimleri** denetleyicideki bölümlerinin sayısını denetlemek için alan.
1. Belirli bir kesim gelen seçin **Segment açılır** gibi tek tek özelliklerini ayarlamak için **başlık** veya **görüntü** ve belirli bir kesim ise denetlemek için  **Etkin** veya **seçili** denetimi olduğunda görüntülenir.
1. Son olarak, Ata **adları** denetimlere için C# kodunda yanıt vermesi. Örneğin: 

    [ ![](segmented-controls-images/segment04-vs.png "Bir ad atayın")](segmented-controls-images/segment04-vs.png)
1. Değişikliklerinizi kaydedin.
    
-----

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>Bölümlenmiş denetimleri ile çalışma

Bölümlenmiş bir denetim doğrusal öğeleri kümesi sağlar s yukarıda belirtildiği gibi her biri bir simge veya metin içerebilir ve kullanıcı için ilgili bir seçenek kümesi sağlamak için kullanılır.

Xamarin.tvOS uygulamanızda ile bölümlenmiş denetimleri çalışabilir birkaç farklı yolu vardır.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>Adları ve olayları sunulan

Segment denetiminizi arabirimi Tasarımcısı'nda oluşturduysanız ve adlı bir denetim ve bir olay sunulan kesim değiştirmeye yanıt için aşağıdaki kodu kullanabilirsiniz:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take action based on the number of players selected
    switch(PlayerCount.SelectedSegment) {
    case 0:
        // Do something if the segment is selected
        ...
        break;
    case 1:
        // Do something if the segment is selected
        ...
        break;
    case 2:
        // Do something if the segment is selected
        ...
        break;
    }
}
```

Yukarıdaki örnek söz konusu olduğunda, Segment denetimi olarak ortaya çıkmıştır bir `PlayerCount` adı ve `PlayerCountChanged` olay eylem. Eylemler ve çıkışlar ile çalışma hakkında daha fazla bilgi için lütfen bkz [çıkışlar ve Eylemler ile kod yazma](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) bölümünü bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md).

`SelectedSegment` Özelliği dizinini alır veya şu anda seçili kesim tabanlı sıfır (0) olarak ayarlar. Beş (5) kesimleri varsa, ilk kesimi dizini sıfır (0) ve son dizini alacak şekilde dört (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>Segment değiştirme

Herhangi bir zamanda hem sayı hem de bölümlenmiş denetimlerinizi içeriği değiştirebilirsiniz. Yeni bir simge kesimi eklemek için aşağıdaki kodu kullanın:

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

İkinci parametre kesim nerede olacağını tanımlayan bir sıfır (0) tabanlı dizini kullanarak eklenir. Son parametre ise `true` INSERT animasyonlu.

Belirli bir kesim kaldırmak için aşağıdakileri kullanın:

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Veya aşağıdaki tüm parçaları kaldırmak için:

```csharp
SegmentedControl.RemoveAllSegments();
```

Yeniden son parametre ise `true`, kaldırma animasyonlu. Kullanım `NumberOfSegments` bölümlerinin geçerli sayısını döndürmek için özellik.

Alınacak **başlık** veya **simgesi** belirli bir kesim için aşağıdakileri kullanın:

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

Ve değiştirmek için **başlık** veya **simgesi**, aşağıdakileri kullanın:

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Belirli bir kesim olup olmadığını görmek için **etkin**, aşağıdakileri kullanın:

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

Ve **etkinleştir/devre dışı bırak** bir Segment verildiğinde, aşağıdakileri kullanın:

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>Bölümlenmiş denetiminin görünüşünü değiştirme

Bir görüntüye belirli bir kesim arka planı değiştirmek için aşağıdaki kodu kullanabilirsiniz:

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Burada `UIControlState` için resim olarak ayarlama denetim durumunu belirtir:

- Normal
- Vurgulanmış
- Devre dışı
- Seçili
- Odaklanmış

Ve `UIBarMetrics` olarak kullanılacak ölçümleri belirtir:

- Varsayılan
- Sıkıştır
- DefaultPrompt
- CompactPrompt

Ayrıca, ayırıcıyı kullanarak kesimleri arasında ayarlayabilirsiniz:

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

Burada ilk `UIControlState` sol bölme ve ikinci segmente durumunu belirtir `UIControlState` sağındaki Segment durumunu belirtir.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve Xamarin.tvOS uygulama içinde bölümlenmiş denetimiyle çalışma kapsamına.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
