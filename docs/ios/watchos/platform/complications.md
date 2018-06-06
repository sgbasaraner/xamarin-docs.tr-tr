---
title: watchOS Xamarin zorluklar
description: Bu belge, Xamarin içinde watchOS zorluklar çalışmak açıklar. Olası, şablonları, yazma bir olası ekleme açıklanır ve örnek kodu sağlar.
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: 3c69f65091e7d6c83afe34c6d8c06477cc5d133b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791845"
---
# <a name="watchos-complications-in-xamarin"></a>watchOS Xamarin zorluklar

_Gözcü yüzeyleri için özel zorluklar yazmak geliştiricilerin watchOS sağlar_

Bu sayfayı zorluklar kullanılabilir ve bir olası watchOS 3 uygulamanıza eklemek nasıl farklı türleri açıklanmaktadır.

Her watchOS uygulama yalnızca bir olası sahip unutmayın.

Başlangıç okuyarak [Apple'nın belgeleri](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) uygulamanız için bir olası uygun olup olmadığını belirlemek için. 5 vardır `CLKComplicationFamily` aralarından seçim yapabileceğiniz görüntü türleri:

[![](complications-images/all-complications-sml.png "5 CLKComplicationFamily türleri kullanılabilir: döngüsel küçük, modüler küçük, modüler büyük, faydalı olması küçük, faydalı olması büyük")](complications-images/all-complications.png#lightbox)

Uygulamalar, yalnızca bir stil veya beş, görüntülenen verilerin bağlı olarak uygulayabilirsiniz.
Kullanıcı dijital Dama yapma kapatır gibi geçmiş ve/veya gelecekteki saatlere değerleri sağlayarak zaman seyahat destekler.

<a name="adding" />

## <a name="adding-a-complication"></a>Bir olası ekleme

### <a name="configuration"></a>Yapılandırma

Zorluklar bir izleme uygulaması oluşturma sırasında eklenen veya el ile varolan bir çözüme eklenemez.

### <a name="add-new-project"></a>Yeni Proje Ekle...

**Yeni Proje Ekle...**  Sihirbazı otomatik olarak olası denetleyici sınıfını oluşturmak ve yapılandırmak bir onay kutusu içerir **Info.plist** dosyası:

![](complications-images/file-new-project-sml.png "Olası dahil onay kutusu")

### <a name="existing-projects"></a>Mevcut projeleri

Varolan bir projeye bir olası eklemek için:

1. Yeni bir **ComplicationController.cs** sınıfı dosya ve uygulama `CLKComplicationDataSource`.
2. Uygulamanın yapılandırma **Info.plist** karmaşıklıklar söz konusudur ve hangi olası aileleri desteklenen kimlik kullanıma sunmak için.

Bu adımlar aşağıdaki daha ayrıntılı olarak açıklanmaktadır.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource sınıfı

Aşağıdaki C# şablonu uygulamak için en düşük gerekli yöntemleri içeren bir `CLKComplicationDataSource`.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
    }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
    }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
    }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
    }
}
```

İzleyin [bir olası yazma](#writing) yönergeleri kodu bu sınıfına ekleyin.

### <a name="infoplist"></a>Info.plist

Gözcü uzantının **Info.plist** dosyası adını belirtmeniz gerekir `CLKComplicationDataSource` ve desteklemek istediğiniz hangi olası aileleri:

[![](complications-images/complications-config-sml.png "Olası ailesi türleri")](complications-images/complications-config.png#lightbox)

**Veri kaynağı sınıfı** girdi listesi, o alt sınıf adları gösterir `CLKComplicationDataSource` olası mantığı içeren bir alt kümesi.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Tüm olası işlevselliği yöntemleri geçersiz kılma tek bir sınıf uygulanan `CLKComplicationDataSource` soyut sınıf (hangi uygulayan `ICLKComplicationDataSource` arabirimi).

### <a name="required-methods"></a>Gerekli yöntemleri

Olası çalıştırmak için aşağıdaki yöntemlerden uygulamanız gerekir:

- `GetPlaceholderTemplate` -Yapılandırma sırasında veya uygulama bir değer sağladığında kullanılan statik görüntü döndür.
- `GetCurrentTimelineEntry` -Olası çalıştırırken doğru görüntüleme hesaplayın.
- `GetSupportedTimeTravelDirections` -Döndürür seçeneklerinden `CLKComplicationTimeTravelDirections` gibi `None`, `Forward`, `Backward`, veya `Forward | Backward`.

### <a name="privacy"></a>Gizlilik

Kişisel verileri görüntülemek zorluklar

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` Veya `HideOnLockScreen`

Bu yöntem döndürürse `HideOnLockScreen` olası bir simge veya uygulama adı (ve hiçbir veri) gösterecektir sonra izleme kilitli olduğunda.

### <a name="updates"></a>Güncelleştirmeler

- `GetNextRequestedUpdateDate` -Zaman işletim sisteminin sonraki uygulama güncelleştirilmiş olası verileri görüntülemek için sorgu bir saati döndürür.

Ayrıca, bir güncelleştirme iOS uygulamanızdan zorlayabilirsiniz.

### <a name="supporting-time-travel"></a>Zaman seyahat destekleme

Zaman seyahat destek isteğe bağlıdır ve tarafından denetlenen `GetSupportedTimeTravelDirections` yöntemi. Döndürürse `Forward`, `Backward`, veya `Forward | Backward` aşağıdaki yöntemlerden uygulamalıdır sonra

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Bir olası yazma

Basit veri zorluklar aralığından karmaşık görüntü ve veri işlemesi zaman seyahat desteği ile görüntüleyin. Aşağıdaki kod basit, tek şablon olası nasıl oluşturulacağını gösterir.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Örnek kod

Bu örnek yalnızca destekler `UtilitarianLarge` yalnızca bu tür bir olası destekleyen belirli izleme yüzeyleri seçili için şablon. Zaman *seçme* zorluklar görüntülediği bir watch'unuzda **MY olası** ve ne zaman *çalıştıran* metni görüntüler **dakika _saat_**   (ile süresinin parçası).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>Olası şablonları

Farklı şablon sayısı her olası stili için kullanılabilir.
**Halkası** şablonları ilerleme veya başka bir değer grafik görüntülemek için kullanılan olası geçici ilerleme stili halka görüntülemenize olanak tanır.

[Apple'nın CLKComplicationTemplate belgeleri](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Döngüsel küçük

Bu şablon sınıfı adları tüm ile önek `CLKComplicationTemplateCircularSmall`:

- **RingImage** -ilerleme halka çevresinde ile tek bir resmi görüntüle.
- **RingText** -metinle çevresinde ilerleme halka tek bir satırı görüntüler.
- **SimpleImage** -yalnızca tek bir küçük resmi görüntüle.
- **SimpleText** -yalnızca küçük bir metin parçacığını görüntüle.
- **StackImage** -resim ve metin, birbirinin üzerine satırının görüntüleme
- **StackText** -iki satırlık metin görüntüleme.

### <a name="modular-small"></a>Modüler küçük

Bu şablon sınıfı adları tüm ile önek `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -metin değerleri (2 satır ve 2 sütun) oluşan küçük bir kılavuz görüntüler.
- **RingImage** -ilerleme halka çevresinde ile tek bir resmi görüntüle.
- **RingText** -metinle çevresinde ilerleme halka tek bir satırı görüntüler.
- **SimpleImage** -yalnızca tek bir küçük resmi görüntüle.
- **SimpleText** -yalnızca küçük bir metin parçacığını görüntüle.
- **StackImage** -resim ve metin, birbirinin üzerine satırının görüntüleme
- **StackText** -iki satırlık metin görüntüleme.

### <a name="modular-large"></a>Modüler büyük

Bu şablon sınıfı adları tüm ile önek `CLKComplicationTemplateModularLarge`:

- **Sütunları** -isteğe bağlı olarak her satırın solundaki görüntüye dahil olmak üzere 2 sütunlarla 3 satır kılavuzunun görüntüler.
- **StandardBody** -düz metin iki satır ile kalın başlık dize görüntüleme. Üstbilgi, sol tarafta bir görüntü isteğe bağlı olarak görüntüleyebilirsiniz.
- **Tablo** -2 x 2 kılavuzuna altındaki metin kalın başlık dize görüntüleme. Üstbilgi, sol tarafta bir görüntü isteğe bağlı olarak görüntüleyebilirsiniz.
- **TallBody** -altındaki metni büyük yazı tipi tek satırlık bir kalın başlık dizeyle görüntüler.

### <a name="utilitarian-small"></a>Faydalı küçük

Bu şablon sınıfı adları tüm ile önek `CLKComplicationTemplateUtilitarianSmall`:

- **Düz** -görüntü ve bazı metin (metin olmalıdır kısa) tek bir satırda görüntüler.
- **RingImage** -ilerleme halka çevresinde ile tek bir resmi görüntüle.
- **RingText** -metinle çevresinde ilerleme halka tek bir satırı görüntüler.
- **Kare** -(40px veya 44px 38 mm veya 42 mm Apple Watch sırasıyla için kare) kare bir resmi görüntüle.

### <a name="utilitarian-large"></a>Faydalı olması büyük

Bu olası stili için yalnızca bir şablonu yoktur: `CLKComplicationTemplateUtilitarianLargeFlat`.
Bu seçenek, tek bir görüntü ve bazı metin tümü tek bir satıra görüntüler.



## <a name="related-links"></a>İlgili bağlantılar

- [Apple belgeleri](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC video](https://developer.apple.com/videos/play/wwdc2015-209/)
