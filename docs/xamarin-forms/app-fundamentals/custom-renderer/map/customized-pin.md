---
title: "Bir harita PIN özelleştirme"
description: "Bu makalede her platformda yerel bir harita özelleştirilmiş bir PIN ve PIN'i veri özelleştirilmiş bir görünümünü görüntüler harita denetiminin özel Oluşturucu Oluşturma gösterilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 312bda44a6b390c6ba486d5a3d60dfe4fb770a2e
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
---
# <a name="customizing-a-map-pin"></a>Bir harita PIN özelleştirme

_Bu makalede her platformda yerel bir harita özelleştirilmiş bir PIN ve PIN'i veri özelleştirilmiş bir görünümünü görüntüler harita denetiminin özel Oluşturucu Oluşturma gösterilir._

Her Xamarin.Forms görünüm yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu yok. Zaman bir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) iOS, bir Xamarin.Forms uygulaması tarafından işlenen `MapRenderer` sınıf örneği, hangi sırayla yerel başlatır `MKMapView` denetim. Android platformunda `MapRenderer` sınıfı başlatır yerel `MapView` denetim. Üzerinde Evrensel Windows Platformu (UWP), `MapRenderer` sınıfı başlatır yerel `MapControl`. Oluşturucu ve Xamarin.Forms denetimleri Eşle yerel denetim sınıfları hakkında daha fazla bilgi için bkz: [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagram arasındaki ilişkiyi gösterir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) ve uyguladıktan karşılık gelen yerel denetimlere:

![](customized-pin-images/map-classes.png "Harita denetiminin ve uygulama yerel denetimleri arasındaki ilişki")

Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için kullanılan bir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda eşlemesi için özel Oluşturucu.

Her öğe artık sırasıyla uygulamak için incelenecektir bir `CustomMap` her platformda yerel bir harita özelleştirilmiş bir PIN ve PIN'i veri özelleştirilmiş bir görünümünü görüntüler Oluşturucu.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) başlatılmış ve kullanılmadan önce yapılandırılması gerekir. Daha fazla bilgi için bkz: [ `Maps Control` ](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Özel eşleme oluşturma

Özel Harita denetiminin sınıflara tarafından oluşturulabilir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` Denetimi taşınabilir sınıf kitaplığı (PCL) projesinde oluşturulur ve özel eşleme için API tanımlar. Özel Harita sunan `CustomPins` koleksiyonunu temsil eden özellik `CustomPin` her platformda Yerel Harita denetiminin tarafından işlenen nesneleri. `CustomPin` Sınıfı, aşağıdaki kod örneğinde gösterilir:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Bu sınıfı tanımlayan bir `CustomPin` özelliklerini devralma olarak [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) sınıfı ve ekleyerek bir `Url` özelliği.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Özel Harita kullanma

`CustomMap` Denetim başvurulabilir XAML'de PCL projesinde konumu için bir ad alanı bildirme ve özel Harita denetimi ad alanı öneki kullanarak. Aşağıdaki örnekte gösterildiği kod nasıl `CustomMap` denetim XAML sayfası tarafından tüketilen:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

`local` Ad alanı öneki adlı bir şey. Ancak, `clr-namespace` ve `assembly` değerlerin özel eşleme ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde öneki özel Harita başvurmak için kullanılır.

Aşağıdaki örnekte gösterildiği kod nasıl `CustomMap` denetim bir C# sayfası tarafından tüketilen:

```csharp
public class MapPageCS : ContentPage
{
  public MapPageCS ()
  {
    var customMap = new CustomMap {
      MapType = MapType.Street,
      WidthRequest = App.ScreenWidth,
      HeightRequest = App.ScreenHeight
    };
    ...

    Content = customMap;
  }
}
```

`CustomMap` Örneği, her platformda yerel eşlemeyi görüntülemek için kullanılır. Bunun [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) özelliği görüntü stilini ayarlar [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/), içinde tanımlanan olası değerler ile [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/) numaralandırması. İOS ve Android için genişlik ve yükseklik harita ayarlanmış özellikleri yoluyla `App` platforma özgü projelerinde başlatılmış sınıfı.

Harita ve PIN konumunu, varsa, aşağıdaki kod örneğinde gösterildiği gibi başlatılır:

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

Bu başlatma özel bir PIN ekler ve haritanın görünümüyle konumlandırır [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) konumunu ve harita yakınlaştırma düzeyini oluşturarak yöntemi bir [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) bir gelen[ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) ve [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

Özel oluşturucu artık yerel harita denetimleri özelleştirmek için her uygulama projesi eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `MapRenderer` özel Harita işler sınıfı.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için mantığı yazmak ve özel Harita işleyen yöntemi. Karşılık gelen Xamarin.Forms özel Harita oluşturulduğunda, bu yöntem çağrılır.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms özel eşlemesi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Her platform projesinde özel Oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse, varsayılan oluşturucu denetimin taban sınıfı için kullanılır.

Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](customized-pin-images/solution-structure.png "CustomMap özel Oluşturucu Proje Sorumlulukları")

`CustomMap` Öğesinden türetilen platforma özgü Oluşturucu sınıflar tarafından işlenen denetim `MapRenderer` her platform için sınıf. Bu her sonuçları `CustomMap` aşağıdaki ekran görüntülerinde gösterildiği gibi platforma özgü denetimleriyle işlenen denetim:

![](customized-pin-images/screenshots.png "Her platformda CustomMap")

`MapRenderer` Sınıf çıkarır `OnElementChanged` Xamarin.Forms özel Harita karşılık gelen yerel denetimi oluşturmak için oluşturulduğunda çağrılan yöntemi. Bu yöntem alır bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikleri Xamarin.Forms öğesini temsil, oluşturucu *olan* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olan* bağlı olarak, sırasıyla. Örnek uygulamasında `OldElement` özelliği `null` ve `NewElement` özelliği bir başvuru içerecek `CustomMap` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, yerel denetim özelleştirme gerçekleştirmek için yerdir. Platformda kullanılan yerel denetlemek için belirlenmiş bir başvuru üzerinden erişilen `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetlemek için bir başvuru aracılığıyla elde edilebilir `Element` özelliği.

Olay işleyicileri için abone olurken dikkatli'nin alınması gereken `OnElementChanged` aşağıdaki kod örneğinde gösterildiği şekilde yöntemi:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Yerel bir denetimi yapılandırılmalıdır ve olay işleyicileri yalnızca özel Oluşturucu yeni bir Xamarin.Forms öğesi eklendiğinde abone. Benzer şekilde, abone tüm olay işleyicileri yalnızca Oluşturucu bağlı öğesi değiştiğinde alanından aboneliği olmalıdır. Bu yaklaşım benimsenmesi bellek sızıntılarını yaşar olmayan özel bir oluşturucu oluşturmak için yardımcı olur.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` Xamarin.Forms ile oluşturucuyu kaydeder özniteliği. Öznitelik iki parametre – işlenen Xamarin.Forms özel denetim türü adı ve özel Oluşturucu Tür adını alır. `assembly` Özniteliğine öneki belirtir. öznitelik tüm derlemesi için geçerlidir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfı uyarlamasını açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki ekran görüntüleri Haritası öncesinde ve sonrasında özelleştirme göster:

![](customized-pin-images/map-layout-ios.png "Harita denetiminin öncesinde ve sonrasında özelleştirme")

İOS PIN adlı bir *ek açıklama*, ve özel bir görüntü veya sistem tanımlı bir PIN çeşitli renklerin olabilir. Ek açıklamalar isteğe bağlı olarak gösterilecek bir *belirtme çizgisi*, yanıt ek açıklamanın seçerek kullanıcı olarak görüntülenir. Belirtme çizgisi görüntüler `Label` ve `Address` özelliklerini `Pin` isteğe bağlı sol ve sağ aksesuar görünümlerle örneği. Yukarıdaki ekran görüntüsünde, sol aksesuar bir monkey görüntüsü ile olan sağa aksesuar görünüm görünümdür *bilgi* düğmesi.

Aşağıdaki kod örneğinde iOS platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

`OnElementChanged` Yöntemi aşağıdaki yapılandırmasını gerçekleştirir [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) özel Oluşturucu yeni bir Xamarin.Forms öğesi bağlı olması koşuluyla örneği:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) Özelliği ayarlanmış `GetViewForAnnotation` yöntemi. Bu yöntem aldığında çağrılan [ek açıklamanın konumu haritada görünür hale](#Displaying_the_Annotation)ve görüntülemek için ek açıklama önceki özelleştirmek için kullanılır.
- Olay işleyicileri için `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, ve `DidDeselectAnnotationView` olayları kayıtlı. Bu olayların ne zaman yangın kullanıcı [belirtme çizgisi sağ aksesuar dokunur](#Tapping_on_the_Right_Callout_Accessory_View), ne zaman ve kullanıcı [seçer](#Selecting_the_Annotation) ve [seçimini kaldırır](#Deselecting_the_Annotation) ek açıklama sırasıyla. Yalnızca Oluşturucu öğesi değişiklikleri iliştirildiğinde gelen aboneliği bir olaylardır.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Ek açıklamanın görüntüleme

`GetViewForAnnotation` Yöntemi çağrılır ek açıklamanın konumu haritada görünür hale gelir ve görüntülemek için ek açıklama önceki özelleştirmek için kullanılır. Ek açıklamanın iki bölümden oluşur:

- `MkAnnotation` – Başlık, alt başlık ve ek açıklama konumunu içerir.
- `MkAnnotationView` – ek açıklama ve isteğe bağlı olarak, kullanıcının ek açıklamanın ne zaman dokunur gösterilen bir belirtme çizgisi temsil etmek için görüntü içerir.

`GetViewForAnnotation` Yöntemi kabul eden bir `IMKAnnotation` açıklamanın verileri içeren ve döndürür bir `MKAnnotationView` haritaya görüntülenmek ve aşağıdaki kod örneğinde gösterildiği:

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Bu yöntem ek açıklamanın özel görüntü görüntülenir yerine ek açıklamanın dokunduğunuz olduğunda sistem tarafından tanımlanan PIN ve, belirtme çizgisi ek içerik sol ve sağ ek açıklama başlık ve adres içeren görüntülenir sağlar . Bu şekilde gerçekleştirilir:

1. `GetCustomPin` Yöntemi ek açıklama için özel PIN verileri döndürmek için çağrılır.
1. Bellek tasarruf etmek için ek açıklamanın görünüm çağrısıyla yeniden kullanım için havuza alınmış [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. `CustomMKAnnotationView` Sınıfını genişleten `MKAnnotationView` ile sınıf `Id` ve `Url` aynı özelliklerinde karşılık özellikleri `CustomPin` örneği. Yeni bir örneğini `CustomMKAnnotationView` oluşturulur, ek açıklamanın koşuluyla `null`:
  - `CustomMKAnnotationView.Image` Özelliği ayarlanmış görüntüye ek açıklama harita üzerinde temsil eder.
  - `CustomMKAnnotationView.CalloutOffset` Özelliği ayarlanmış bir `CGPoint` ek açıklama belirtme çizgisi ortalanmış olduğunu belirtir.
  - `CustomMKAnnotationView.LeftCalloutAccessoryView` Özelliği, ek açıklama başlık ve adres solunda görünecek bir monkey görüntüsünü ayarlanır.
  - `CustomMKAnnotationView.RightCalloutAccessoryView` Özelliği ayarlanmış bir *bilgi* ek açıklama başlık ve adres sağında görünür düğmesi.
  - `CustomMKAnnotationView.Id` Özelliği ayarlanmış `CustomPin.Id` özellik tarafından döndürülen `GetCustomPin` yöntemi. Bu, sahip olması tanımlanması ek açıklamanın sağlar [belirtme çizgisi daha fazla özelleştirilebilir](#Selecting_the_Annotation)istenirse.
  - `CustomMKAnnotationView.Url` Özelliği ayarlanmış `CustomPin.Url` özellik tarafından döndürülen `GetCustomPin` yöntemi. URL ne zaman gidilecek kullanıcı [sağ belirtme çizgisi aksesuar görünümünde görüntülenen düğmeye dokunur](#Tapping_on_the_Right_Callout_Accessory_View).
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) Özelliği ayarlanmış `true` böylece ek açıklamanın dokunduğunuz belirtme çizgisi görüntülenir.
1. Ek açıklamanın görüntülenmek üzere haritada döndürülür.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Ek açıklamanın seçme

Ek açıklamayı, kullanıcı dokunur zaman `DidSelectAnnotationView` sırayla yürütür olay ateşlenir `OnDidSelectAnnotationView` yöntemi:

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

Ekleyerek (yani sol ve sağ aksesuar görünümlerini içerir) mevcut belirtme çizgisi bu yöntemin genişlettiği bir `UIView` Seçili ek açıklama sahip olması koşuluyla Xamarin logo görüntüsü içeren örneği kendisine onun `Id` içinayarlananözelliği`Xamarin`. Bu senaryolar farklı çağrılar için farklı ek açıklamalar burada görüntülenebilir sağlar. `UIView` Örneği, varolan belirtme çizgisi ortalanmış görüntülenir.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Sağ belirtme çizgisi donatıyı görünümünde dokunun

Kullanıcı dokunur üzerinde ne zaman *bilgi* düğmesine sağ belirtme çizgisi aksesuar görünümünde `CalloutAccessoryControlTapped` sırayla yürütür olay ateşlenir `OnCalloutAccessoryControlTapped` yöntemi:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Bu yöntem, bir web tarayıcısı açar ve depolanan adresine gider `CustomMKAnnotationView.Url` özelliği. Adres oluştururken tanımlandığına Not `CustomPin` PCL projesinde koleksiyonu.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Ek açıklamanın seçimi kaldırma

Ek açıklamanın görüntülenir ve kullanıcı dokunur haritada `DidDeselectAnnotationView` sırayla yürütür olay ateşlenir `OnDidDeselectAnnotationView` yöntemi:

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

Bu yöntem, varolan belirtme çizgisi yok seçildiğinde, görüntülenmesini (Xamarin logo görüntüsü) belirtme çizgisi genişletilmiş parçası de durdurulur ve kaynaklarını serbest bırakılacak sağlar.

Özelleştirme hakkında daha fazla bilgi için bir `MKMapView` örnek için bkz: [iOS eşlemeleri](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Aşağıdaki ekran görüntüleri Haritası öncesinde ve sonrasında özelleştirme göster:

![](customized-pin-images/map-layout-android.png "Harita denetiminin öncesinde ve sonrasında özelleştirme")

Android PIN adlı bir *işaret*, ve özel bir görüntü veya sistem tanımlı bir işaret çeşitli renklerin ya da olabilir. İşaretçileri gösterebilir bir *bilgi penceresi*, yanıt üzerinde işaret dokunarak kullanıcı olarak görüntülenir. Bilgi penceresinde görüntüler `Label` ve `Address` özelliklerini `Pin` örneği ve diğer içerik eklemek için özelleştirilebilir. Ancak, aynı anda yalnızca bir bilgi penceresi gösterilebilir.

Aşağıdaki kod örneğinde, Android platformu için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

Özel oluşturucu yeni bir Xamarin.Forms öğesine bağlı olduğu koşuluyla `OnElementChanged` yöntem çağrılarını `MapView.GetMapAsync` temel alır yöntemi `GoogleMap` görünüme bağlıdır. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` geçersiz kılma çağrılabilir. Bu yöntem için bir olay işleyicisi kaydeder `InfoWindowClick` tetiklenen olay [bilgi penceresi tıklandığında](#Clicking_on_the_Info_Window)ve gelen yalnızca Oluşturucu öğesi değişiklikleri iliştirildiğinde iptal. `OnMapReady` Ayrıca çağrıları geçersiz `SetInfoWindowAdapter` yöntemi belirtmek için `CustomMapRenderer` sınıf örneği bilgi penceresi özelleştirmek için yöntemleri sunar.

`CustomMapRenderer` Uygulayan sınıf `GoogleMap.IInfoWindowAdapter` arabirimini [bilgisi penceresini özelleştirme](#Customizing_the_Info_Window). Bu arabirim, aşağıdaki yöntemlerden uygulanmalı belirtir:

- `public Android.Views.View GetInfoWindow(Marker marker)` – Bu yöntem, bir işaretçi için özel bilgiler penceresine dönmek için çağrılır. Döndürürse `null`, varsayılan penceresi işleme kullanılır. Döndürürse bir `View`, ardından, `View` bilgisi pencere çerçevesi içinde yer alır.
- `public Android.Views.View GetInfoContents(Marker marker)` – Döndürülecek bu yöntem çağrılır bir `View` bilgisi penceresinin içeriği içeren ve yalnızca olursa çağrılacak `GetInfoWindow` yöntemi döndürür `null`. Döndürürse `null`, bilgi penceresi içeriğin varsayılan işleme kullanılır.

Örnek uygulama, yalnızca bilgi penceresi içerik özelleştirildikten ve böylece `GetInfoWindow` yöntemi döndürür `null` bunu etkinleştirmek için.

#### <a name="customizing-the-marker"></a>İşaretin özelleştirme

Bir işaretçi göstermek için kullanılan simge çağırarak özelleştirilebilir `MarkerOptions.SetIcon` yöntemi. Bu geçersiz kılma tarafından gerçekleştirilebilir `CreateMarker` her biri için çağrılan yöntem `Pin` eşlemeye eklenir:

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

Bu yöntem yeni bir oluşturur `MarkerOption` her örneği `Pin` örneği. Konum, etiket ve adres işaretin ayarladıktan sonra simge ile ayarlanır `SetIcon` yöntemi. Bu yöntem alır bir `BitmapDescriptor` ile simgesi işlemek için gerekli verileri içeren bir nesne `BitmapDescriptorFactory` oluşturulmasını basitleştirmeye yardımcı yöntemler sağlayan sınıf `BitmapDescriptor`.

Kullanma hakkında daha fazla bilgi için `BitmapDescriptorFactory` bir işaret özelleştirmek için bkz: sınıf [bir işaret özelleştirme](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Bilgi penceresini özelleştirme

Bir kullanıcı işaret üzerinde dokunur zaman `GetInfoContents` yöntemi yürütüldüğünde, aşağıdaki koşullarda `GetInfoWindow` yöntemi döndürür `null`. Aşağıdaki örnekte gösterildiği kod `GetInfoContents` yöntemi:

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.Id.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

Bu yöntem bir `View` bilgi penceresinin içeriğini içeren. Bu şekilde gerçekleştirilir:

- A `LayoutInflater` örneği alınır. Bu, karşılık gelen bir düzen XML dosyası örneği oluşturmak için kullanılan `View`.
- `GetCustomPin` Yöntemi bilgi penceresi için özel PIN verileri döndürmek için çağrılır.
- `XamarinMapInfoWindow` Düzeni, şişirileceğini `CustomPin.Id` özelliği eşittir `Xamarin`. Aksi takdirde `MapInfoWindow` Düzen şişirileceğini. Bu senaryolar için farklı işaretçileri farklı bilgi pencere düzenlerini burada görüntülenebilir sağlar.
- `InfoWindowTitle` Ve `InfoWindowSubtitle` kaynakları inflated düzeninden alınır ve bunların `Text` özelliklerini ayarlamak için karşılık gelen verileri `Marker` örnek kaynakları doldurulmaz koşuluyla `null`.
- `View` Örneği haritada görüntülenmek üzere döndürdü.

> [!NOTE]
> Bir bilgi penceresi canlı bir değil `View`. Bunun yerine, Android dönüştürecek `View` bir statik bitmap ve bir görüntü olarak görüntüler. Tıklama olayları bilgi penceresi sırasında click olayı için herhangi bir touch olayları veya hareketleri yanıt veremez ve bilgi penceresinde ayrı ayrı denetimler kendi için yanıt veremez yanıtlayabilir bu anlamına gelir.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Bilgi penceresinde tıklatarak

Kullanıcı bilgileri penceresinde tıklattığında `InfoWindowClick` sırayla yürütür olay ateşlenir `OnInfoWindowClick` yöntemi:

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

Bu yöntem, bir web tarayıcısı açar ve depolanan adresine gider `Url` özelliğini `CustomPin` örnek `Marker`. Adres oluştururken tanımlandığına Not `CustomPin` PCL projesinde koleksiyonu.

Özelleştirme hakkında daha fazla bilgi için bir `MapView` örnek için bkz: [haritalar API'si](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Aşağıdaki ekran görüntüleri Haritası öncesinde ve sonrasında özelleştirme göster:

![](customized-pin-images/map-layout-uwp.png "Harita denetiminin öncesinde ve sonrasında özelleştirme")

UWP üzerinde PIN adlı bir *eşlemi simgesini*, ve özel bir görüntü veya sistem tarafından tanımlanan varsayılan görüntü ya da olabilir. Bir harita simgesi gösterebilir bir `UserControl`, yanıt harita simgesine dokunarak kullanıcı olarak görüntülenir. `UserControl` Herhangi bir içerik görüntüleyebilirsiniz dahil olmak üzere `Label` ve `Address` özelliklerini `Pin` örneği.

Aşağıdaki kod örneğinde UWP özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

`OnElementChanged` Yöntemi yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı koşuluyla, aşağıdaki işlemleri gerçekleştirir:

- Temizlenmeden `MapControl.Children` için bir olay işleyicisi kaydetmeden önce var olan kullanıcı arabirimi öğeleri eşlemesinden kaldırmak için koleksiyon `MapElementClick` olay. Bu olay kullanıcı dokunur veya üzerinde tıklattığında ateşlenir bir `MapElement` üzerinde `MapControl`ve gelen yalnızca Oluşturucu öğesi değişiklikleri iliştirildiğinde iptal.
- Her PIN `customPins` koleksiyonu görüntülenir harita üzerinde doğru coğrafi konumda şu şekilde:
  - PIN için konum olarak oluşturulan bir `Geopoint` örneği.
  - A `MapIcon` örneği PIN temsil etmek üzere oluşturulur.
  - Temsil etmek için kullanılan görüntü `MapIcon` ayarlayarak belirtilen `MapIcon.Image` özelliği. Harita üzerinde diğer öğeler tarafından görünmeyebilir gibi ancak harita simgesinin görüntüsü her zaman gösterilecek için kesin değildir. Bu nedenle, harita simgesinin `CollisionBehaviorDesired` özelliği ayarlanmış `MapElementCollisionBehavior.RemainVisible`görünür kalmasını sağlamak için.
  - Konumunu `MapIcon` ayarlayarak belirtilen `MapIcon.Location` özelliği.
  - `MapIcon.NormalizedAnchorPoint` Özelliği, resimdeki işaretçi yaklaşık konumunu ayarlanır. Bu özellik, görüntünün sol üst köşesinin temsil eden, varsayılan değer (0,0) korur durumunda harita yakınlaştırma düzeyini değişiklikleri farklı bir konuma işaret eden görüntü sonuçlanabilir.
  - `MapIcon` Örneği eklenir `MapControl.MapElements` koleksiyonu. Bu üzerinde görüntülenmesini eşlemi simgesini sonuçlanır `MapControl`.

> [!NOTE]
> Aynı görüntüyü birden çok eşleme simgelerini kullanırken `RandomAccessStreamReference` örneği bildirilen en iyi performans için sayfa veya uygulama düzeyinde.

#### <a name="displaying-the-usercontrol"></a>UserControl görüntüleme

Bir kullanıcı eşlemesi simgesinde dokunur zaman `OnMapElementClick` yöntemi yürütüldüğünde. Aşağıdaki kod örneği, bu yöntem gösterilmektedir:

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Id.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

Bu yöntem oluşturur bir `UserControl` PIN hakkındaki bilgileri görüntüler örneği. Bu şekilde gerçekleştirilir:

- `MapIcon` Örneği alınır.
- `GetCustomPin` Yöntemi görüntülenecek özel PIN verileri döndürmek için çağrılır.
- A `XamarinMapOverlay` örneği özel PIN verileri görüntülemek için oluşturulur. Bir kullanıcı denetimi sınıftır.
- Görüntülenecek coğrafi konumda `XamarinMapOverlay` üzerinde örnek `MapControl` olarak oluşturulan bir `Geopoint` örneği.
- `XamarinMapOverlay` Örneği eklenir `MapControl.Children` koleksiyonu. Bu koleksiyon haritada görüntülenmelerini XAML kullanıcı arabirimi öğeleri içerir.
- Coğrafi konumunu `XamarinMapOverlay` harita örneğinde çağırarak ayarlanmış `SetLocation` yöntemi.
- Göreli konum `XamarinMapOverlay` belirtilen konuma karşılık gelen, örnek olarak ayarlanmış çağırarak `SetNormalizedAnchorPoint` yöntemi. Bu harita sonucunda yakınlaştırma düzeyini seçeneğinde değişiklik sağlar `XamarinMapOverlay` her zaman doğru konumda görüntülenmesini örneği.

Alternatif olarak, PIN hakkında bilgi haritada zaten görüntülenmektedir kullanıyorsanız harita üzerinde dokunma kaldıran `XamarinMapOverlay` gelen örnek `MapControl.Children` koleksiyonu.

#### <a name="tapping-on-the-information-button"></a>Bilgi düğmesine dokunun

Kullanıcı dokunur üzerinde ne zaman *bilgi* düğmesini `XamarinMapOverlay` kullanıcı denetimi `Tapped` sırayla yürütür olay ateşlenir `OnInfoButtonTapped` yöntemi:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Bu yöntem, bir web tarayıcısı açar ve depolanan adresine gider `Url` özelliği `CustomPin` örneği. Adres oluştururken tanımlandığına Not `CustomPin` PCL projesinde koleksiyonu.

Özelleştirme hakkında daha fazla bilgi için bir `MapControl` örnek için bkz: [harita ve konum genel bakış](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) konusuna bakın.

## <a name="summary"></a>Özet

Bu makale için özel Oluşturucu Oluşturma gösterilen `Map` denetim, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme. Xamarin.Forms.Maps kullanıcılar için hızlı ve bilinen bir eşlemesi sağlamak için API'ler her platformda deneyimi yerel eşlemesi kullanmak eşlemeleri görüntüleme için platformlar arası Özet sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [MAPS denetimi](~/xamarin-forms/user-interface/map.md)
- [iOS eşlemeleri](~/ios/user-interface/controls/ios-maps/index.md)
- [Haritalar API’si](~/android/platform/maps-and-location/maps/maps-api.md)
- [Özelleştirilmiş PIN (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
