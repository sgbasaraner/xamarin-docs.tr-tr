---
title: Özel bir düzen oluşturma
description: Bu makalede, nasıl özel yerleşim sınıfı yazılacağını açıklar ve alt öğelerini yatay sayfa boyunca düzenler ve ek satırlar sonraki alt öğelerini görüntüsünü sarmalar bir yönlendirme duyarlı WrapLayout sınıfını gösterir.
ms.prod: xamarin
ms.assetid: B0CFDB59-14E5-49E9-965A-3DCCEDAC2E31
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/29/2017
ms.openlocfilehash: f225a80a1c386668b71d1cdd47409eb017f19033
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244941"
---
# <a name="creating-a-custom-layout"></a>Özel bir düzen oluşturma

_Xamarin.Forms – StackLayout, AbsoluteLayout, RelativeLayout ve kılavuz, dört düzeni sınıfları tanımlar ve her alt farklı bir şekilde düzenler. Ancak, bazen onu Xamarin.Forms tarafından sağlanmayan bir düzen kullanarak sayfa içeriği düzenlemek gereklidir. Bu makalede, nasıl özel yerleşim sınıfı yazılacağını açıklar ve alt öğelerini yatay sayfa boyunca düzenler ve ek satırlar sonraki alt öğelerini görüntüsünü sarmalar bir yönlendirme duyarlı WrapLayout sınıfını gösterir._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms tüm düzeni sınıfları türetin [ `Layout<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) sınıfı ve genel tür sınırlamak [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ve türetilmiş türler. Buna karşılık, `Layout<T>` sınıfı türer [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) mekanizması konumlandırma ve boyutlandırma alt öğeleri sağlayan sınıf.

Her bir görsel öğe olarak bilinen kendi tercih edilen boyut belirlemek için sorumlu olduğu *istenen* boyutu. [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/), ve [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) türetilmiş türler kendi alt ya da alt kendilerini göreli boyutunu ve konumunu belirlemek için sorumlu. Bu nedenle, burada üst belirler ne alt boyutu olmalıdır, bir üst-alt ilişkisi düzeni içerir, ancak istenen alt boyutuna uygun dener.

Xamarin.Forms düzeni ve geçersiz kılma döngüleri çok iyi anlaşılmasını, özel bir düzen oluşturmak için gereklidir. Bu döngü şimdi incelenecektir.

## <a name="layout"></a>Düzen

Bir sayfayla görsel ağaç üstündeki düzeni başlar ve her bir sayfasında görsel öğe kapsayacak şekilde visual ağacının tüm dalları aracılığıyla devam eder. Diğer öğeler için üst öğeleri boyutlandırma ve bunların alt öğeleri kendilerini göreli konumlandırma sorumludur.

[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Sınıfı tanımlayan bir [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) düzeni işlemleri için bir öğe ölçer yöntemi ve [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) belirtir yöntemi dikdörtgen öğesi içinde işlenir. Bir uygulamayı başlatır ve ilk sayfası görüntülendiğinde, bir *düzeni döngüsü* oluşan ilk `Measure` çağrıları ve ardından `Layout` çağrıları, başlatır üzerinde [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) nesnesi:

1. Düzen döngüsü sırasında her üst öğenin çağırmadan sorumludur `Measure` alt yöntemi.
1. Alt ölçülen sonra her üst öğenin çağırmadan sorumludur `Layout` alt yöntemi.

Bu döngü her görsel öğeyi sayfada çağrıları almasını sağlar `Measure` ve `Layout` yöntemleri. İşlem, aşağıdaki çizimde gösterilmiştir:

![](custom-images/layout-cycle.png "Xamarin.Forms düzeni döngüsü")

> [!NOTE]
> Bir şey düzenini etkileyen değişirse düzeni döngüleri da görsel ağaç alt kümesinde oluşabilir unutmayın. Bu eklenemez veya koleksiyondan gibi kaldırılamaz öğeleri içeren bir [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/), bir değişiklik [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) bir öğe veya öğenin boyutunu değişikliği özelliği.

Olan her Xamarin.Forms sınıfı bir `Content` veya `Children` özelliğine sahip bir geçersiz kılınabilir [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) yöntemi. Öğesinden türetilen özel düzen sınıfları [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) bu yöntemi geçersiz kılın ve emin olmanız gerekir [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) ve [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) yöntemleri istediğiniz özel düzene sağlamak için öğenin tüm alt üzerinde çağrılır.

Ayrıca, her sınıf, türer [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) veya [ `Layout<View>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/) geçersiz kılmanız gerekir [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) düzeni sınıfı burada yöntemi Çağrı yaparak olması gereken boyutu belirler [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) , alt öğelerinin yöntemleri.

> [!NOTE]
> Öğeleri belirler boyutlarına göre *kısıtlamaları*, belirtmek öğenin üst içinde bir öğe için ne kadar alan vardır. Kısıtlamaları geçirilen [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) ve [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) yöntemler 0'dan aralığında `Double.PositiveInfinity`. Bir öğe *kısıtlı*, veya *tam olarak kısıtlı*, yapılan bir çağrı aldığında, [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) yöntemi sonsuz olmayan bağımsız değişkenlerle - öğesi sınırlı değildir belirli bir boyuta. Bir öğe *Kısıtlanmamış*, veya *kısmen kısıtlı*, yapılan bir çağrı aldığında, `Measure` en az bir değişken eşit yöntemiyle `Double.PositiveInfinity` – sonsuz kısıtlaması olabilir Otomatik boyutlandırma belirten olarak düşünülen.

## <a name="invalidation"></a>Geçersiz kılma

Geçersiz kılma olarak öğe bir sayfa üzerinde değişiklik yeni düzen döngüsü tetikler işlemidir. Artık doğru boyutunu veya konumunu sahip olduğunuzda öğeleri geçersiz olarak kabul edilir. Örneğin, varsa [ `FontSize` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/) özelliği bir [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) değişiklikleri `Button` artık doğru boyutta olacağı için geçersiz olarak kabul edilir. Yeniden boyutlandırma `Button` sonra bir sayfa kalanında düzeninde değişiklikler ripple etkisini olabilir.

Öğeleri geçersiz kendilerini çağırarak [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) yöntemi, genellikle öğesi özelliği değiştiğinde öğeyi yeni bir boyutu neden olabilir. Bu yöntemin [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) yeni düzen döngüsü tetiklemek için öğenin üst işleme olay.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Sınıfı ayarlar için bir işleyici [ `MeasureInvalidated` ](https://developer.xamarin.com/api/event/Xamarin.Forms.VisualElement.MeasureInvalidated/) eklenen her alt olayda kendi `Content` özelliği veya `Children` koleksiyonu ve işleyicisini ayırır olduğunda alt kaldırılır. Bu nedenle, alt öğelerinden boyutu değiştiğinde her öğenin alt öğesi yok görsel ağaç uyarı. Aşağıdaki diyagramda, nasıl bir değişiklik görsel ağaç bir öğenin boyutunu ağaca ripple değişiklikler neden olabilir gösterilmektedir:

![](custom-images/invalidation.png "Görsel ağaç geçersiz kılma")

Ancak, `Layout` sınıfı bir sayfanın düzenini çocuk boyutu değişikliğin etkisini sınırlamak dener. Ardından düzeni kısıtlı boyutu ise, bir alt boyutu değişiklik görsel ağaç üst düzeni daha yüksek bir şey etkilemez. Ancak, genellikle bir düzen boyutunu değişikliği nasıl düzeni alt düzenler etkiler. Bu nedenle, bir düzen 's boyutunda herhangi bir değişiklik düzeni için bir düzen döngüsü başlatılır ve düzeni çağrıları alacak kendi [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) ve [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) yöntemleri.

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) Sınıfı ayrıca tanımlayan bir [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) benzer bir amacı vardır yöntemi [ `InvalidateMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.InvalidateMeasure()/) yöntemi. `InvalidateLayout` Nasıl düzeni yerleştirir ve alt boyutları etkileyen bir değişiklik yapıldığında yöntemi'nin çağrılacak. Örneğin, `Layout` sınıfı çağırır `InvalidateLayout` her bir alt eklenen veya kaldırılan bir düzeninden yöntemi.

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Yineleyen çağırmaları en aza indirmek için bir önbellek uygulamak için geçersiz kılınabilir [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) düzeni ait alt yöntemleri. Geçersiz kılma `InvalidateLayout` yöntemi, bir bildirim alt zaman eklenen veya düzenden kaldırılır sağlayacaktır. Benzer şekilde, [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) yöntem geçersiz düzeni 's öğelerinden boyutu değiştiğinde bildirim sağlamak için. Her iki yöntemi geçersiz kılmalar için önbellek temizleyerek özel bir düzen yanıt vermesi gerekir. Daha fazla bilgi için bkz: [hesaplama ve veri önbelleğe alma](#caching).

## <a name="creating-a-custom-layout"></a>Özel bir düzen oluşturma

Özel bir düzen oluşturma işlemi aşağıdaki gibidir:

1. Türeyen bir sınıf oluşturun `Layout<View>` sınıfı. Daha fazla bilgi için bkz: [bir WrapLayout oluşturma](#creating).
1. [*isteğe bağlı*] düzeni sınıfında ayarlanmalıdır herhangi bir parametre için bağlanabilir özellikleri tarafından desteklenen özellikler ekleyin. Daha fazla bilgi için bkz: [ekleme bağlanabilir özellikleri yedeklenen özellikleri](#adding_properties).
1. Geçersiz kılma [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) çağrılacak yöntem [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) düzeni için Düzen ait alt ve return istenen boyuta yöntemi. Daha fazla bilgi için bkz: [OnMeasure yöntemini geçersiz kılma](#onmeasure).
1. Geçersiz kılma [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) çağrılacak yöntem [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) düzeni ait alt yöntemi. Çağrılacak hatası [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) yöntemi her alt bir düzende hiçbir zaman doğru boyut ve konum alma alt neden olur ve bu nedenle alt sayfada görünür olmaz. Daha fazla bilgi için bkz: [LayoutChildren yöntemini geçersiz kılma](#layoutchildren).

  > [!NOTE]
>  İçinde alt numaralandırılırken [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) ve [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) geçersiz kılmalar, tüm alt Atla olan [ `IsVisible` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsVisible/) özelliği içinayarlanmış`false`. Bu, özel düzen görünmez çocuklar için boşluk bırakın olmaz güvence altına alır.

1. [*isteğe bağlı*] geçersiz kılma [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) çocuklar için eklendiğinde veya düzenden kaldırıldığında bildirilmesi yöntemi. Daha fazla bilgi için bkz: [InvalidateLayout yöntemini geçersiz kılma](#invalidatelayout).
1. [*isteğe bağlı*] geçersiz kılma [ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) düzeni 's öğelerinden boyutu değiştiğinde bildirim almak için yöntem. Daha fazla bilgi için bkz: [OnChildMeasureInvalidated yöntemini geçersiz kılma](#onchildmeasureinvalidated).

> [!NOTE]
> Unutmayın [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) düzeninin boyutu alt yerine kendi üst tarafından yönetilir, geçersiz kılma'nin çağrılacak olmaz. Geçersiz kılma birine veya ikisine de kısıtlamaları sonsuz veya düzeni sınıfın varsayılan olmayan varsa ancak çağrılacak [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) veya [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özellik değerleri. Bu nedenle, [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) geçersiz kılma sırasında elde edilen alt boyutları Bel olamaz [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) yöntem çağrısı. Bunun yerine, `LayoutChildren` çağırmanız gerekir [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) yöntemini çağırmadan önce düzeni ait alt [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) yöntemi. Alt boyutunu alternatif olarak, elde `OnMeasure` geçersiz kılma önbelleğe alınacağını daha sonra önlemek için `Measure` içinde çağrılarını `LayoutChildren` geçersiz kılma ancak düzeni sınıfı boyutları yeniden alınması gerektiğinde bilmeniz gerekir. Daha fazla bilgi için bkz: [hesaplama ve düzeni veri önbelleğe alma](#caching).

Düzen sınıfı ekleyerek sonra kullanılabilecek bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)ve alt düzenine ekleyerek. Daha fazla bilgi için bkz: [WrapLayout tüketen](#consuming).

<a name="creating" />

### <a name="creating-a-wraplayout"></a>Bir WrapLayout oluşturma

Örnek uygulama yönlendirmesini duyarlı gösterir `WrapLayout` , alt öğelerini yatay sayfa boyunca düzenler ve ek satırlar sonraki alt öğelerini görüntüsünü sarmalar sınıfı.

`WrapLayout` Sınıfı olarak bilinen, her bir alt için aynı miktarda alan ayırır *hücre boyutu*, alt en büyük boyutuna göre. Alt hücre boyutunu hücrede yerleştirilebilir küçük temel alarak kendi [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) özellik değerleri.

`WrapLayout` Sınıf tanımını aşağıdaki kod örneğinde gösterilir:

```csharp
public class WrapLayout : Layout<View>
{
  Dictionary<Size, LayoutData> layoutDataCache = new Dictionary<Size, LayoutData>();
  ...
}
```

<a name="caching" />

#### <a name="calculating-and-caching-layout-data"></a>Hesaplama ve düzeni verileri önbelleğe alma

`LayoutData` Yapısı bir dizi özellikleri alt koleksiyonu hakkındaki verileri depolar:

- `VisibleChildCount` – düzende görünür olan alt öğe sayısı.
- `CellSize` – düzeni boyutuna göre tüm alt öğelerinin en büyük boyutu.
- `Rows` – satırların sayısı.
- `Columns` – sütun sayısı.

`layoutDataCache` Alan birden çok depolamak için kullanılan `LayoutData` değerleri. Uygulama başladığında, iki `LayoutData` nesneleri önbelleğe alınır içine `layoutDataCache` geçerli yönlendirmesini – bir kısıtlama bağımsız değişkenleri için sözlük `OnMeasure` geçersiz kılma ve bir `width` ve `height` bağımsız değişkenleri için `LayoutChildren` geçersiz kılar. Yatay yönlendirme cihaz döndürme zaman `OnMeasure` geçersiz kılmak ve `LayoutChildren` geçersiz kılma yeniden çağrılabilir, başka bir iki neden olacak `LayoutData` sözlükteki önbelleğe alınması nesneleri. Bununla birlikte, cihaz dikey yönlendirmeyi döndürülürken daha fazla hesaplama gereklidir çünkü `layoutDataCache` gerekli verileri zaten sahip.

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

- Bir hesaplanan olup olmadığını belirler `LayoutData` değeri zaten önbellekte ve kullanılabilir durumdaysa döndürür.
- Aksi takdirde çağırma tüm alt, numaralandırır `Measure` yöntemi bir sonsuz genişliği ve yüksekliği, her alt ve en çok alt boyutunu belirler.
- Yoktur sağlanan, en az bir görünür alt, satır ve sütun sayısı, gerekli hesaplar ve ardından bir hücre boyutu boyutlara dayanan çocuklar için `WrapLayout`. Not hücre boyutunu genellikle en çok alt boyutundan biraz daha geniş olduğunu, ancak aynı zamanda daha küçük olabilir, varsa `WrapLayout` için en uzun alt en alt veya yeterince uzun yetecek kadar geniş değil.
- Yeni depolar `LayoutData` önbellekteki değer.

<a name="adding_properties" />

#### <a name="adding-properties-backed-by-bindable-properties"></a>Bağlanabilir özellikleri tarafından desteklenen özellikler ekleme

`WrapLayout` Sınıfı tanımlayan `ColumnSpacing` ve `RowSpacing` değerleri, satır ve sütunları yerleşiminde ayırmak için kullanılır ve bağlanabilir özellikleri tarafından yedeklenen hangi özellikleri. Aşağıdaki kod örneğinde bağlanabilir özellikleri gösterilmektedir:

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

Her bağlanabilirse özelliğinin özelliği değiştirildi işleyiciyi çağırır `InvalidateLayout` yeni bir düzen tetiklemek için yöntemi geçersiz kılma geçirmek `WrapLayout`. Daha fazla bilgi için bkz: [InvalidateLayout yöntemini geçersiz kılma](#invalidatelayout) ve [OnChildMeasureInvalidated yöntemini geçersiz kılma](#onchildmeasureinvalidated).

<a name="onmeasure" />

#### <a name="overriding-the-onmeasure-method"></a>OnMeasure yöntemini geçersiz kılma

`OnMeasure` Geçersiz kılma, aşağıdaki kod örneğinde gösterilir:

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

Geçersiz kılma çağırır `GetLayoutData` yöntemi ve yapıları bir `SizeRequest` de dikkate alarak sırasında döndürülen verileri bir nesneden `RowSpacing` ve `ColumnSpacing` özellik değerleri. Hakkında daha fazla bilgi için `GetLayoutData` yöntemi, bkz: [hesaplama ve veri önbelleğe alma](#caching).

> [!IMPORTANT]
> [ `Measure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Measure/p/System.Double/System.Double/Xamarin.Forms.MeasureFlags/) Ve [ `OnMeasure` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.OnMeasure/p/System.Double/System.Double/) hiçbir zaman request yöntemleri sonsuz bir boyut döndürerek bir [ `SizeRequest` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SizeRequest/) ayarlamak bir özellik değeri `Double.PositiveInfinity`. Ancak, en az bir kısıtlama bağımsız değişkenlerinin `OnMeasure` olabilir `Double.PositiveInfinity`.

<a name="layoutchildren" />

#### <a name="overriding-the-layoutchildren-method"></a>LayoutChildren yöntemini geçersiz kılma

`LayoutChildren` Geçersiz kılma, aşağıdaki kod örneğinde gösterilir:

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

Geçersiz kılma çağrısı ile başlayan `GetLayoutData` yöntemi ve tüm alt öğelerini boyut ve her çocuğun hücrede Yerleştir numaralandırır. Bu çağırarak sağlanır [ `LayoutChildIntoBoundingRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildIntoBoundingRegion/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/) dayalı bir dikdörtgen içinde alt konumlandırmak için kullanılan yöntemi kendi [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) ve [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)özellik değerleri. Bu çocuğun yapılan bir çağrı yapmaya eşdeğerdir [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) yöntemi.

> [!NOTE]
> Dikdörtgen geçirilen Not `LayoutChildIntoBoundingRegion` yöntemi alt bulunduğu tüm alanı içerir.

Hakkında daha fazla bilgi için `GetLayoutData` yöntemi, bkz: [hesaplama ve veri önbelleğe alma](#caching).

<a name="invalidatelayout" />

#### <a name="overriding-the-invalidatelayout-method"></a>InvalidateLayout yöntemini geçersiz kılma

[ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) Geçersiz kılma çocuklar için eklendiğinde veya düzeninden ya da bir kaldırıldığında çağrıldığında, `WrapLayout` özellikleri değişiklikleri değeri, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
protected override void InvalidateLayout()
{
  base.InvalidateLayout();
  layoutInfoCache.Clear();
}
```

Geçersiz kılma düzeni geçersiz kılar ve tüm önbelleğe alınan düzen bilgilerini atar.

> [!NOTE]
> Durdurmak için [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) sınıfı çağırma [ `InvalidateLayout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.InvalidateLayout()/) her bir alt eklenen veya kaldırılan bir düzeninden yöntemini geçersiz kılın [ `ShouldInvalidateOnChildAdded` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildAdded/p/Xamarin.Forms.View/) ve [ `ShouldInvalidateOnChildRemoved` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.ShouldInvalidateOnChildRemoved/p/Xamarin.Forms.View/) yöntemleri ve return `false`. Alt öğeleri eklendiğinde veya kaldırılan düzeni sınıfı sonra özel bir işlem uygulayabilirsiniz.

<a name="onchildmeasureinvalidated" />

#### <a name="overriding-the-onchildmeasureinvalidated-method"></a>OnChildMeasureInvalidated yöntemini geçersiz kılma

[ `OnChildMeasureInvalidated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.OnChildMeasureInvalidated()/) Geçersiz kılma düzeni 's öğelerinden boyutunu değiştirdiğinde ve aşağıdaki kod örneğinde gösterildiği çağrılır:

```csharp
protected override void OnChildMeasureInvalidated()
{
  base.OnChildMeasureInvalidated();
  layoutInfoCache.Clear();
}
```

Geçersiz kılma alt düzeni geçersiz kılar ve önbelleğe alınan düzeni bilgilerin tümünü atar.

<a name="consuming" />

### <a name="consuming-the-wraplayout"></a>WrapLayout kullanma

`WrapLayout` Sınıfı üzerinde koyarak nin tüketim bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) aşağıdaki XAML kod örneğinde gösterildiği şekilde türetilmiş türünde:

```xaml
<ContentPage ... xmlns:local="clr-namespace:ImageWrapLayout">
    <ScrollView Margin="0,20,0,20">
        <local:WrapLayout x:Name="wrapLayout" />
    </ScrollView>
</ContentPage>
```

Eşdeğer C# kod aşağıda verilmiştir:

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

Alt sonra eklenebilir `WrapLayout` gerektiği gibi. Aşağıdaki örnekte gösterildiği kod [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) eklenmesini öğeleri `WrapLayout`:

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

Zaman sayfayı içeren `WrapLayout` görünür, örnek uygulamayı zaman uyumsuz olarak fotoğraf listesini içeren uzak bir JSON dosyası erişir, oluşturur bir [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) öğesini her fotoğraf ve ekler`WrapLayout`. Bu, aşağıdaki ekran görüntülerinde gösterilen görünüm sonuçlanır:

![](custom-images/portait-screenshots.png "Örnek uygulama dikey ekran görüntüleri")

Aşağıdaki ekran görüntüleri Göster `WrapLayout` , yatay yönlendirmeye döndürüldüğüne sonra:

![](custom-images/landscape-ios.png "İOS uygulama yatay ekran örnek")
![](custom-images/landscape-android.png "örnek Android uygulama yatay ekran") 
 ![ ] (custom-images/landscape-uwp.png " Örnek UWP uygulaması yatay ekran görüntüsü")

Her satırda sütun sayısı Fotoğraf boyutu, ekran genişliği ve CİHAZDAN bağımsız birim başına piksel sayısı bağlıdır. [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Öğeler zaman uyumsuz olarak yük fotoğraf ve bu nedenle `WrapLayout` sınıfı sık çağrı alacaksınız kendi [ `LayoutChildren` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Layout.LayoutChildren/p/System.Double/System.Double/System.Double/System.Double/) yöntemi her olarak `Image` öğe yüklenen fotoğrafa dayalı yeni bir boyutunu alır.

## <a name="summary"></a>Özet

Bu makalede, nasıl özel yerleşim sınıfı yazılacağını açıklandığı ve yönlendirme duyarlı gösterilen `WrapLayout` , alt öğelerini yatay sayfa boyunca düzenler ve ek satırlar sonraki alt öğelerini görüntüsünü sarmalar sınıfı.


## <a name="related-links"></a>İlgili bağlantılar

- [WrapLayout (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/CustomLayout/WrapLayout/)
- [Özel düzenler](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter26.md)
- [Özel düzenler Xamarin.Forms (video) oluşturma](https://evolve.xamarin.com/session/56e20f83bad314273ca4d81c)
- [Düzen<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/)
- [Düzen](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [VisualElement](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)
