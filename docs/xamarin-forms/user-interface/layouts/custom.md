---
title: Özel bir düzen oluşturma
description: Bu makalede, özel düzen sınıfının nasıl kullanılacağını açıklar ve alt öğeleri yatay sayfada yerleştirir ve ardından sonraki alt öğelerine ek satırlar görüntülenmesini sarmalar bir yönlendirme duyarlı WrapLayout sınıfını gösterir.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: 0c16fd3930926a05ed7796391962d0fc8996dc96
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995381"
---
# <a name="creating-a-custom-layout"></a>Özel bir düzen oluşturma

_Xamarin.Forms – StackLayout, AbsoluteLayout RelativeLayout ve kılavuz, dört düzen sınıfları tanımlar ve her alt öğeleri farklı bir şekilde düzenler. Ancak bazen bunu Xamarin.Forms tarafından sağlanmayan bir düzen kullanarak sayfa içeriği düzenlemek gereklidir. Bu makalede, özel düzen sınıfının nasıl kullanılacağını açıklar ve alt öğeleri yatay sayfada yerleştirir ve ardından sonraki alt öğelerine ek satırlar görüntülenmesini sarmalar bir yönlendirme duyarlı WrapLayout sınıfını gösterir._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms içinde tüm düzen sınıfların türetilmesi [ `Layout<T>` ](xref:Xamarin.Forms.Layout`1) sınıfı ve genel tür için sınırlama [ `View` ](xref:Xamarin.Forms.View) ve onun türetilmiş türlerine. Buna karşılık, `Layout<T>` sınıf türetilir [ `Layout` ](xref:Xamarin.Forms.Layout) konumlandırma ve boyutlandırma alt öğelerini mekanizması sağlar sınıfını.

Her bir görsel öğe olarak bilinir, kendi tercih edilen boyut belirlemekten sorumludur *istenen* boyutu. [`Page`](xref:Xamarin.Forms.Page), [ `Layout` ](xref:Xamarin.Forms.Layout), ve [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) türetilen türler, kendi alt ya da alt, kendilerine göre boyutunu ve konumunu belirlemekten sorumludur. Bu nedenle, burada üst belirler ne alt boyutu olmalıdır, bir üst-alt ilişkisi Düzen gerektirir ancak istenen alt boyutunu uyum sağlayacak şekilde çalışacaktır.

Xamarin.Forms düzeni ve geçersiz kılma döngüleri Azure'un, özel bir düzen oluşturmak için gereklidir. Bu döngü artık açıklanmıştır.

## <a name="layout"></a>Düzen

Üst kısmında bir sayfa ile görsel ağaç düzeni başlar ve tüm dallar sayfasında her görsel öğe kapsayacak şekilde görsel ağacı yoluyla ilerler. Üst öğeleri diğer öğelere olan öğeler, kendilerini göreli alt öğelerini konumlandırma ve boyutlandırma sorumludur.

[ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Sınıfı tanımlayan bir [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Düzen işlemleri için bir öğe ölçer yöntemi ve bir [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) belirten yöntemi dikdörtgen öğesi içinde işlenir. Bir uygulamayı başlatır ve ilk sayfası görüntülendiğinde, bir *Düzen döngüsü* oluşan ilk `Measure` çağrıları ve ardından `Layout` çağrıları, başlangıçlar üzerinde [ `Page` ](xref:Xamarin.Forms.Page) nesnesi:

1. Düzen döngüsü sırasında her bir üst öğe çağırmadan sorumludur `Measure` alt yöntemi.
1. Alt ölçüldükten sonra her bir üst öğe çağırmadan sorumludur `Layout` alt yöntemi.

Bu döngü, her bir görsel öğe sayfasında çağrıları almasını sağlar `Measure` ve `Layout` yöntemleri. İşlem, aşağıdaki diyagramda gösterilmiştir:

![](custom-images/layout-cycle.png "Xamarin.Forms Düzen döngüsü")

> [!NOTE]
> Bir şey düzenini etkiler değişirse Düzen döngüleri da görsel ağacı bir alt kümesi üzerinde oluşabilir not edin. Bu eklenmesi veya kaldırılmış olduğu gibi bir koleksiyondaki öğeleri içeren bir [ `StackLayout` ](xref:Xamarin.Forms.StackLayout), bir değişiklik [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) bir öğe veya öğenin boyutundaki bir değişikliği özelliği.

Tüm Xamarin.Forms sınıf bir `Content` veya `Children` özelliğine sahip bir geçersiz kılınabilir [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) yöntemi. Öğesinden türetilen özel düzen sınıfları [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) bu yöntemi yok sayın ve emin olmanız gerekir [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) ve [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) yöntemleri İstenen özel düzen sağlamak için öğenin tüm alt üzerinde çağrılır.

Ayrıca, her sınıf, türetilen [ `Layout` ](xref:Xamarin.Forms.Layout) veya [ `Layout<View>` ](xref:Xamarin.Forms.Layout`1) kılmalı [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) Düzen sınıfı burada yöntemi Bu çağrı yaparak olması gereken boyutu belirler [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) alt yöntemleri.

> [!NOTE]
> Öğeleri belirlemek boyutlarına göre *kısıtlamaları*, belirtmek ne kadar alan üst öğenin içinde bir öğe için kullanılabilir. Geçirilen kısıtlamalar [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) ve [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) yöntemler, 0'dan değişebilir `Double.PositiveInfinity`. Bir öğe *kısıtlı*, veya *tamamen kısıtlı*, bir çağrı aldığında, [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) yöntemi sonsuz olmayan bağımsız değişkenlerle - öğe sınırlıdır belirli bir boyut. Bir öğe *sınırlandırılmamış*, veya *kısmen kısıtlı*, bir çağrı aldığında, `Measure` en az bir değişken eşittir yöntemiyle `Double.PositiveInfinity` – sonsuz kısıtlaması olabilir Otomatik boyutlandırma belirten olarak düşünülebilir.

## <a name="invalidation"></a>Geçersiz kılma

Geçersiz kılma olarak yeni bir düzen döngüsü bir öğe bir sayfa üzerinde değişiklik tetikler işlemidir. Artık doğru boyut ve konum varsa, öğeleri geçersiz olarak kabul edilir. Örneğin, varsa [ `FontSize` ](xref:Xamarin.Forms.Button.FontSize) özelliği bir [ `Button` ](xref:Xamarin.Forms.Button) değişiklikleri `Button` artık doğru boyuta sahip olacağından geçersiz olarak kabul edilir. Yeniden boyutlandırma `Button` sonra değişiklikleri bir dalgalanma etkisine Düzen sayfasının rest aracılığıyla olabilir.

Öğeleri geçersiz kendilerini çağırarak [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) yöntemi genellikle özellik öğe değiştiğinde yeni bir öğe boyutu içinde neden olabilir. Bu yöntemin [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) yeni bir düzen döngüsü tetiklemek için öğenin üst işleme olayı.

[ `Layout` ](xref:Xamarin.Forms.Layout) Sınıfı için bir işleyici ayarlar [ `MeasureInvalidated` ](xref:Xamarin.Forms.VisualElement.MeasureInvalidated) eklenen her alt olay kendi `Content` özelliği veya `Children` koleksiyonu ve işleyici ayırır, alt kaldırılır. Bu nedenle, alt öğelerinden birine boyutu değiştiğinde her öğenin alt öğeleri olan görsel ağaç uyarı. Görsel ağaçta bir öğenin boyutundaki bir değişikliği ağacın ripple değişiklikleri nasıl neden olabilir, aşağıdaki diyagramda gösterilmektedir:

![](custom-images/invalidation.png "Geçersiz kılma görsel ağaç")

Ancak, `Layout` sınıfı bir sayfanın düzenini üzerinde çocuğun boyutundaki bir değişikliği etkisini sınırlamak dener. Ardından düzenini kısıtlı boyutu ise, bir alt boyutu değişiklik görsel ağaç üst düzen daha yüksek bir şey etkilemez. Ancak, genellikle bir düzenini boyutundaki bir değişikliği nasıl alt düzen düzenler etkiler. Bu nedenle, herhangi bir düzen'ın boyutta değişiklik yerleşimi için bir düzen döngüsü başlatılır ve düzeni için aramalarını almaz, [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) ve [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) yöntemleri.

[ `Layout` ](xref:Xamarin.Forms.Layout) Sınıfı da tanımlar bir [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) benzer bir amaca sahip yöntemi [ `InvalidateMeasure` ](xref:Xamarin.Forms.VisualElement.InvalidateMeasure) yöntemi. `InvalidateLayout` Nasıl düzenini yerleştirir ve alt öğelerindeki boyutları etkileyen bir değişiklik yapıldığında yöntemi'nin çağrılacak. Örneğin, `Layout` sınıfı çağırır `InvalidateLayout` her bir alt eklenen veya kaldırılan bir düzenden yöntemi.

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Yinelenen çağrılarını en aza indirmek için bir önbellek uygulamak için geçersiz kılınabilir [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) düzeni'nın alt yöntemleri. Geçersiz kılma `InvalidateLayout` yöntemi, alt öğeleri eklendiğinde veya düzenden kaldırılır ne zaman bir bildirimi sağlayacaktır. Benzer şekilde, [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) yöntemi düzenini 's öğelerinden biri boyutu değiştiğinde bir bildirim sağlamak için kılınabilir. Her iki yöntemi geçersiz kılmalar için özel bir düzen önbelleği temizleyerek yanıt vermelidir. Daha fazla bilgi için [hesaplama ve verileri önbelleğe almayı](#caching).

## <a name="creating-a-custom-layout"></a>Özel bir düzen oluşturma

Özel bir düzen oluşturma işlemi aşağıdaki gibidir:

1. Türetilen bir sınıf oluşturmanız `Layout<View>` sınıfı. Daha fazla bilgi için [bir WrapLayout oluşturma](#creating).
1. [*isteğe bağlı*] düzeni sınıf üzerinde ayarlanmalıdır herhangi bir parametre bağlanabilir özellikler tarafından desteklenen özellikler ekleyin. Daha fazla bilgi için [ekleme bağlanabilir özellikler desteklenen özellikler](#adding_properties).
1. Geçersiz kılma [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) çağrılacak yöntem [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) düzeni için Düzen ait alt ve istenen boyuta return yöntemi. Daha fazla bilgi için [OnMeasure yöntemini geçersiz kılma](#onmeasure).
1. Geçersiz kılma [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) çağrılacak yöntem [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Düzen ait alt yöntemi. Çağrılacak hatası [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) her alt bir düzende yöntemi hiçbir zaman bir doğru boyut ve konum alma alt neden olur ve bu nedenle alt sayfada görünür olmaz. Daha fazla bilgi için [LayoutChildren yöntemini geçersiz kılma](#layoutchildren).

  > [!NOTE]
>  Çocuklarını sıralanırken [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) ve [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) geçersiz kılmalar, tüm alt atlayın, [ `IsVisible` ](xref:Xamarin.Forms.VisualElement.IsVisible) özelliği içinayarlanmış`false`. Bu, özel düzeni için görünmez alt boşluk bırakın olmaz garanti eder.

1. [*isteğe bağlı*] geçersiz kılma [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) alt öğeleri eklendiğinde veya düzenden kaldırılır bildirilmesi için yöntemi. Daha fazla bilgi için [InvalidateLayout yöntemini geçersiz kılma](#invalidatelayout).
1. [*isteğe bağlı*] geçersiz kılma [ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) düzenini 's öğelerinden biri boyutu değiştiğinde bildirim almak için yöntemi. Daha fazla bilgi için [OnChildMeasureInvalidated yöntemini geçersiz kılma](#onchildmeasureinvalidated).

> [!NOTE]
> Unutmayın [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) düzenini alt yerine kendi üst öğesi tarafından yönetilir, geçersiz kılma'nin çağrılacak olmaz. Geçersiz kılma birine veya ikisine de kısıtlamaları sonsuz veya varsayılan olmayan Düzen sınıf varsa, ancak çağrılacak [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) veya [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellik değerleri. Bu nedenle, [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) geçersiz kılma sırasında elde edilen alt boyutları bağımlı olamaz [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) yöntem çağrısı. Bunun yerine, `LayoutChildren` çağırmalıdır [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) yöntemini çağırmadan önce düzeni'nın alt [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) yöntemi. Alternatif olarak, alt boyutu içinde elde edilen `OnMeasure` geçersiz kılma önbelleğe alınacağını daha sonra önlemek için `Measure` çağrılarını içinde `LayoutChildren` geçersiz kılma ancak bir düzeni sınıf boyutları yeniden elde edilmesi gerektiğinde bilmeniz gerekir. Daha fazla bilgi için [hesaplama ve Düzen verileri önbelleğe almayı](#caching).

Düzen sınıfı ekleyerek sonra kullanılabilecek bir [ `Page` ](xref:Xamarin.Forms.Page)ve alt öğeleri ekleyerek düzeni. Daha fazla bilgi için [WrapLayout tüketen](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Bir WrapLayout oluşturma

Örnek uygulama yönlendirme duyarlı gösterir `WrapLayout` alt öğeleri yatay sayfada yerleştirir ve ardından sonraki alt öğelerine ek satırlar görüntülenmesini sarmalar sınıfı.

`WrapLayout` Sınıfı olarak bilinir, her alt için aynı miktarda alan ayırır *hücre boyutu*, alt öğelerini, maksimum boyutuna bağlı olarak. Hücre boyutunu hücrede konumlandırılmalıdır küçük bir alt temel alarak kendi [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) özellik değerleri.

`WrapLayout` Sınıf tanımını aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>Hesaplama ve Düzen verileri önbelleğe alma

`LayoutData` Yapısı, bir dizi özelliği alt koleksiyonu hakkında veri depolar:

- `VisibleChildCount` – düzende görünür olan alt öğelerin sayısı.
- `CellSize` – düzenini boyutuna ayarlanmış tüm alt en büyük boyutu.
- `Rows` – satırların sayısı.
- `Columns` – sütun sayısı.

`layoutDataCache` Alan birden çok depolamak için kullanılan `LayoutData` değerleri. Uygulama başlatıldığında, iki `LayoutData` nesneleri önbelleğe alınır içine `layoutDataCache` geçerli yönlendirme – bir kısıtlama bağımsız değişkenleri için sözlük `OnMeasure` geçersiz kılma ve biri `width` ve `height` bağımsız değişkenleri için `LayoutChildren` geçersiz kılar. Cihaz yatay yöndeki döndürürken `OnMeasure` geçersiz kılmak ve `LayoutChildren` geçersiz kılma yeniden çağrılacak, başka bir iki neden olacak `LayoutData` sözlüğe önbelleğe alınmasını nesneleri. Ancak, cihaz dikey yönlendirme döndürülürken, daha fazla hesaplama gereklidir çünkü `layoutDataCache` gerekli verileri zaten sahip.

Aşağıdaki örnekte gösterildiği kod `GetLayoutData` özelliklerini hesaplar yöntemi `LayoutData` yapılandırılmış bir belirli boyutuna göre:

```csharp
LayoutData GetLayoutData(double width, double height)
{
  Size size = new Size(width, height);

  // Check if cached information is available.
  if (layoutDataCache.ContainsKey(size))
  {
    return layoutDataCache[size];
  }

  int visibleChildCount = 0;
  Size maxChildSize = new Size();
  int rows = 0;
  int columns = 0;
  LayoutData layoutData = new LayoutData();

  // Enumerate through all the children.
  foreach (View child in Children)
  {
    // Skip invisible children.
    if (!child.IsVisible)
      continue;

    // Count the visible children.
    visibleChildCount++;

    // Get the child's requested size.
    SizeRequest childSizeRequest = child.Measure(Double.PositiveInfinity, Double.PositiveInfinity);

    // Accumulate the maximum child size.
    maxChildSize.Width = Math.Max(maxChildSize.Width, childSizeRequest.Request.Width);
    maxChildSize.Height = Math.Max(maxChildSize.Height, childSizeRequest.Request.Height);
  }

  if (visibleChildCount != 0)
  {
    // Calculate the number of rows and columns.
    if (Double.IsPositiveInfinity(width))
    {
      columns = visibleChildCount;
      rows = 1;
    }
    else
    {
      columns = (int)((width + ColumnSpacing) / (maxChildSize.Width + ColumnSpacing));
      columns = Math.Max(1, columns);
      rows = (visibleChildCount + columns - 1) / columns;
    }

    // Now maximize the cell size based on the layout size.
    Size cellSize = new Size();

    if (Double.IsPositiveInfinity(width))
      cellSize.Width = maxChildSize.Width;
    else
      cellSize.Width = (width - ColumnSpacing * (columns - 1)) / columns;

    if (Double.IsPositiveInfinity(height))
      cellSize.Height = maxChildSize.Height;
    else
      cellSize.Height = (height - RowSpacing * (rows - 1)) / rows;

    layoutData = new LayoutData(visibleChildCount, cellSize, rows, columns);
  }

  layoutDataCache.Add(size, layoutData);
  return layoutData;
}
```

`GetLayoutData` Yöntemi aşağıdaki işlemleri gerçekleştirir:

- Hesaplanan olup olmadığını belirler `LayoutData` değeri önbellekte zaten ve kullanılabilir olup olmadığını döndürür.
- Aksi takdirde, çağırma tüm alt numaralandırır `Measure` yöntemi ile bir sonsuz genişlik ve yükseklik, her alt ve en fazla alt boyutunu belirler.
- Var. sağlanan görünür en az bir alt, bu satırları ve sütunları gerekli sayısını hesaplar ve ardından boyutlarına göre alt öğeleri için bir hücre boyutunu hesaplar `WrapLayout`. Hücre boyutu genellikle en fazla alt boyutundan daha geniş olduğu halde de daha küçük olabilir, Not, `WrapLayout` için en uzun alt en alt veya yeterince uzun yetecek kadar geniş değilse.
- Yeni depolar `LayoutData` önbelleğinde değeri.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Bağlanabilir özellikler tarafından desteklenen özellikler ekleme

`WrapLayout` Sınıfı tanımlar `ColumnSpacing` ve `RowSpacing` değerleri satırları ve sütunları düzeninde ayırmak için kullanılır ve bağlanabilir özellikler tarafından desteklenen hangi özellikleri. Bağlanabilir özellikler aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public static readonly BindableProperty ColumnSpacingProperty = BindableProperty.Create(
  "ColumnSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });

public static readonly BindableProperty RowSpacingProperty = BindableProperty.Create(
  "RowSpacing",
  typeof(double),
  typeof(WrapLayout),
  5.0,
  propertyChanged: (bindable, oldvalue, newvalue) =>
  {
    ((WrapLayout)bindable).InvalidateLayout();
  });
```

Her bağlanılabilir özellik özellik değişti işleyicisini çağırır `InvalidateLayout` yeni bir düzen tetiklemek için yöntemi geçersiz kılma geçirmek `WrapLayout`. Daha fazla bilgi için [InvalidateLayout yöntemini geçersiz kılma](#invalidatelayout) ve [OnChildMeasureInvalidated yöntemini geçersiz kılma](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>OnMeasure yöntemini geçersiz kılma

`OnMeasure` Geçersiz kılma, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
protected override SizeRequest OnMeasure(double widthConstraint, double heightConstraint)
{
  LayoutData layoutData = GetLayoutData(widthConstraint, heightConstraint);
  if (layoutData.VisibleChildCount == 0)
  {
    return new SizeRequest();
  }

  Size totalSize = new Size(layoutData.CellSize.Width * layoutData.Columns + ColumnSpacing * (layoutData.Columns - 1),
                layoutData.CellSize.Height * layoutData.Rows + RowSpacing * (layoutData.Rows - 1));
  return new SizeRequest(totalSize);
}
```

Geçersiz kılma çağırır `GetLayoutData` yöntemi ve yapıları bir `SizeRequest` da hesaba katarak sırasında döndürülen verileri bir nesneden `RowSpacing` ve `ColumnSpacing` özellik değerleri. Hakkında daha fazla bilgi için `GetLayoutData` yöntemi bkz [hesaplama ve verileri önbelleğe almayı](#caching).

> [!IMPORTANT]
> [ `Measure` ](xref:Xamarin.Forms.VisualElement.Measure(System.Double,System.Double,Xamarin.Forms.MeasureFlags)) Ve [ `OnMeasure` ](xref:Xamarin.Forms.VisualElement.OnMeasure(System.Double,System.Double)) yöntemleri hiçbir istek sonsuz bir boyut döndürerek bir [ `SizeRequest` ](xref:Xamarin.Forms.SizeRequest) ayarlanan bir özellik değeri `Double.PositiveInfinity`. Ancak, en az bir kısıtlama bağımsız değişkenlerin `OnMeasure` olabilir `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>LayoutChildren yöntemini geçersiz kılma

`LayoutChildren` Geçersiz kılma, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
protected override void LayoutChildren(double x, double y, double width, double height)
{
  LayoutData layoutData = GetLayoutData(width, height);

  if (layoutData.VisibleChildCount == 0)
  {
    return;
  }

  double xChild = x;
  double yChild = y;
  int row = 0;
  int column = 0;

  foreach (View child in Children)
  {
    if (!child.IsVisible)
    {
      continue;
    }

    LayoutChildIntoBoundingRegion(child, new Rectangle(new Point(xChild, yChild), layoutData.CellSize));
    if (++column == layoutData.Columns)
    {
      column = 0;
      row++;
      xChild = x;
      yChild += RowSpacing + layoutData.CellSize.Height;
    }
    else
    {
      xChild += ColumnSpacing + layoutData.CellSize.Width;
    }
  }
}
```

Geçersiz kılma çağrısı ile başlayan `GetLayoutData` yöntemi ve tüm alt boyut ve bunları her çocuğun hücrede konumlandırmak için numaralandırır. Bu harekete geçirerek gerçekleştirilir [ `LayoutChildIntoBoundingRegion` ](xref:Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle)) dayalı bir dikdörtgen içindeki bir alt konumlandırmak için kullanılan yöntemi kendi [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) ve [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions)özellik değerleri. Bu çocuk çağrı yapmaya eşdeğerdir [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) yöntemi.

> [!NOTE]
> Dikdörtgen geçirilen Not `LayoutChildIntoBoundingRegion` yöntemi, alt ikamet tüm alanı içerir.

Hakkında daha fazla bilgi için `GetLayoutData` yöntemi bkz [hesaplama ve verileri önbelleğe almayı](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>InvalidateLayout yöntemini geçersiz kılma

[ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) Alt öğeleri eklendiğinde veya düzeninden veya bir geçersiz kılma çağrılır, `WrapLayout` özellik değişikliği değer, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Geçersiz kılma Düzen geçersiz kılar ve tüm önbelleğe alınan Düzen bilgileri atar.

> [!NOTE]
> Durdurmak için [ `Layout` ](xref:Xamarin.Forms.Layout) sınıfı çağırma [ `InvalidateLayout` ](xref:Xamarin.Forms.Layout.InvalidateLayout) her bir alt eklenen veya kaldırılan bir düzenden yöntemi yok sayın [ `ShouldInvalidateOnChildAdded` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded(Xamarin.Forms.View)) ve [ `ShouldInvalidateOnChildRemoved` ](xref:Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved(Xamarin.Forms.View)) yöntemleri ve dönüş `false`. Alt öğeleri eklendiğinde veya kaldırıldığında Düzen sınıf sonra özel bir işleminiz uygulayabilir.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated yöntemini geçersiz kılma

[ `OnChildMeasureInvalidated` ](xref:Xamarin.Forms.Layout.OnChildMeasureInvalidated) Geçersiz kılma düzenini 's öğelerinden biri boyutu ve aşağıdaki kod örneğinde gösterilen çağrılır:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Geçersiz kılma alt düzen geçersiz kılar ve tüm önbelleğe alınan Düzen bilgileri atar.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>WrapLayout kullanma

`WrapLayout` Sınıfı üzerinde koyarak kullanılabilir bir [ `Page` ](xref:Xamarin.Forms.Page) türetilmiş tür, aşağıdaki XAML kod örneğinde gösterildiği gibi:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Eşdeğer C# kodu aşağıda gösterilmiştir:

```csharp
public class ImageWrapLayoutPageCS : ContentPage
{
  WrapLayout wrapLayout;

  public ImageWrapLayoutPageCS()
  {
    wrapLayout = new WrapLayout();

    Content = new ScrollView
    {
      Margin = new Thickness(0, 20, 0, 20),
      Content = wrapLayout
    };
  }
  ...
}
```

Alt öğeleri ardından eklenebilir `WrapLayout` gerektiğinde. Aşağıdaki örnekte gösterildiği kod [ `Image` ](xref:Xamarin.Forms.Image) eklenen öğeleri `WrapLayout`:

```csharp
protected override async void OnAppearing()
{
  base.OnAppearing();

  var images = await GetImageListAsync();
  foreach (var photo in images.Photos)
  {
    var image = new Image
    {
      Source = ImageSource.FromUri(new Uri(photo + string.Format("?width={0}&height={0}&mode=max", Device.RuntimePlatform == Device.UWP ? 120 : 240)))
    };
    wrapLayout.Children.Add(image);
  }
}

async Task<ImageList> GetImageListAsync()
{
  var requestUri = "https://docs.xamarin.com/demo/stock.json";
  using (var client = new HttpClient())
  {
    var result = await client.GetStringAsync(requestUri);
    return JsonConvert.DeserializeObject<ImageList>(result);
  }
}
```

Zaman içeren sayfa `WrapLayout` görünür örnek uygulama, zaman uyumsuz olarak fotoğraf listesini içeren uzak bir JSON dosyası erişirse, oluşturur bir [ `Image` ](xref:Xamarin.Forms.Image) her öğe, fotoğraf ve için ekler`WrapLayout`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünümünü sonuçlanır:

![](custom-images/portait-screenshots.png "Örnek uygulama dikey ekran görüntüleri")

Aşağıdaki ekran görüntüleri Göster `WrapLayout` yatay için bunu unutmayınız sonra:

![](custom-images/landscape-ios.png "İOS uygulama yatay ekran örnek")
![](custom-images/landscape-android.png "örnek Android uygulama yatay ekran") 
 ![ ] (custom-images/landscape-uwp.png " UWP uygulaması için örnek yatay ekran görüntüsü")

Fotoğraf boyutu, ekran genişliği ve CİHAZDAN bağımsız birim başına piksel sayısı her bir satırdaki sütun sayısını bağlıdır. [ `Image` ](xref:Xamarin.Forms.Image) Öğeleri fotoğrafları, zaman uyumsuz olarak yükleyin ve bu nedenle `WrapLayout` sınıfı sık çağrı alırsınız, [ `LayoutChildren` ](xref:Xamarin.Forms.Layout.LayoutChildren(System.Double,System.Double,System.Double,System.Double)) yöntemi her olarak `Image` öğe yeni bir boyutu üzerinde yüklü fotoğraf göre alır.

## <a name="summary"></a>Özet

Bu makalede, bir özel düzen sınıf yazma açıklanmıştır ve yönlendirmesini duyarlı gösterilen `WrapLayout` alt öğeleri yatay sayfada yerleştirir ve ardından sonraki alt öğelerine ek satırlar görüntülenmesini sarmalar sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [WrapLayout (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Özel düzenler](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Xamarin.Forms içinde özel düzenleri (video) oluşturma](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Düzen<T>](xref:Xamarin.Forms.Layout`1)
- [Düzen](xref:Xamarin.Forms.Layout)
- [VisualElement](xref:Xamarin.Forms.VisualElement)
