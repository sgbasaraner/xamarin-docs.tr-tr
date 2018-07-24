---
title: Bir harita Raptiyesini özelleştirme
description: Bu makalede, özelleştirilmiş bir PIN ve PIN'i verilerin özelleştirilmiş bir görünümü ile yerel bir harita her platformda görüntüler harita denetiminin özel Oluşturucu oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 351119a8b0089f78d4ce98729a1516c3cd7bae7b
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39203091"
---
# <a name="customizing-a-map-pin"></a>Bir harita Raptiyesini özelleştirme

_Bu makalede, özelleştirilmiş bir PIN ve PIN'i verilerin özelleştirilmiş bir görünümü ile yerel bir harita her platformda görüntüler harita denetiminin özel Oluşturucu oluşturma işlemini gösterir._

Her bir Xamarin.Forms görünüm yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici içerir. Olduğunda bir [ `Map` ](xref:Xamarin.Forms.Maps.Map) iOS, bir Xamarin.Forms uygulaması tarafından işlenen `MapRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `MKMapView` denetimi. Android platformunda `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapView` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapControl`. Oluşturucu ve Xamarin.Forms denetimleri eşleyen yerel denetim sınıfları hakkında daha fazla bilgi için bkz. [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagramda arasındaki ilişkiyi gösterir [ `Map` ](xref:Xamarin.Forms.Maps.Map) ve onu uygulayan karşılık gelen yerel denetimler:

![](customized-pin-images/map-classes.png "Harita denetimi ve uygulama yerel denetimleri arasındaki ilişki")

İşleme için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için kullanılan bir [ `Map` ](xref:Xamarin.Forms.Maps.Map) her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) harita üzerinde her platform için özel Oluşturucu.

Her öğe artık sırayla uygulanacağı açıklanmıştır bir `CustomMap` özelleştirilmiş bir PIN ve PIN'i verilerin özelleştirilmiş bir görünümü ile yerel bir harita her platformda görüntüler Oluşturucu.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) başlatılır ve yapılandırılmış önce kullanır. Daha fazla bilgi için bkz. [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Özel harita oluşturma

Özel Harita denetimi tarafından sınıflara oluşturulabilir [ `Map` ](xref:Xamarin.Forms.Maps.Map) aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

`CustomMap` Denetimi .NET Standard kitaplığı projesinde oluşturulur ve API'si için özel eşleme tanımlar. Özel eşleme sunan `CustomPins` koleksiyonunu temsil eden özellik `CustomPin` nesnelerin her platformda Yerel Harita denetimi tarafından işlenir. `CustomPin` Sınıfı, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Bu sınıfı tanımlayan bir `CustomPin` özelliklerini devralmasını olarak [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) sınıfı ve ekleyerek bir `Url` özelliği.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Özel Harita kullanma

`CustomMap` Denetim başvurulabilir XAML içinde .NET Standard kitaplığı projesinde konumu için bir ad alanı bildirmek ve özel eşleme denetimi ad alanı öneki kullanarak. Aşağıdaki kod örnekte gösterildiği nasıl `CustomMap` denetimi bir XAML sayfası tarafından kullanılabilir:

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

`local` Ad alanı ön eki adı herhangi bir şey. Ancak, `clr-namespace` ve `assembly` değerleri özel eşleme ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde, ön ek özel eşleme başvurmak için kullanılır.

Aşağıdaki kod örnekte gösterildiği nasıl `CustomMap` denetimi bir C# sayfası tarafından kullanılabilir:

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

`CustomMap` Örneği, her platformda yerel haritada görüntülemek için kullanılır. Sahip [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) özelliği görünen stilini ayarlar [ `Map` ](xref:Xamarin.Forms.Maps.Map), içinde tanımlanan olası değerler ile [ `MapType` ](xref:Xamarin.Forms.Maps.MapType) sabit listesi. İOS ve Android için genişliği ve yüksekliği harita ayarlanmış özellikleri kullanılarak `App` platforma özgü projelerinde başlatılır sınıfı.

Harita ve PIN'leri konumunu, varsa, aşağıdaki kod örneğinde gösterildiği gibi başlatılır:

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

Bu başlatma özel bir PIN ekler ve haritanın görünüm ile konumlandırır [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) oluşturarak konumu ve harita yakınlaştırma düzeyini değiştiren yöntemi bir [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) bir gelen[ `Position` ](xref:Xamarin.Forms.Maps.Position) ve [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

Özel oluşturucu artık yerel eşleme denetimleri özelleştirmek için her uygulama projesine eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `MapRenderer` özel Haritası işleyen sınıfı.
1. Geçersiz kılma `OnElementChanged` özelleştirmek için mantığı yazmak ve özel Haritası işleyen yöntemi. Bu yöntem, karşılık gelen Xamarin.Forms özel eşleme oluşturulduğunda çağrılır.
1. Ekleme bir `ExportRenderer` bunu Xamarin.Forms özel eşleme oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Her platform projesinde özel bir oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse denetimin taban sınıfı için varsayılan oluşturucu kullanılır.

Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](customized-pin-images/solution-structure.png "CustomMap özel Oluşturucu Proje Sorumlulukları")

`CustomMap` Öğesinden türetilen Oluşturucu platforma özgü sınıflar tarafından işlenen denetim `MapRenderer` her platform için sınıf. Bu her sonuçları `CustomMap` aşağıdaki ekran görüntülerinde gösterildiği gibi platforma özgü denetimleriyle işlenen denetim:

![](customized-pin-images/screenshots.png "Her platformda CustomMap")

`MapRenderer` Sınıfı kullanıma sunan `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms özel eşleme oluşturulurken çağrılan yöntemi. Bu yöntem bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikler Xamarin.Forms öğesini temsil, oluşturucu *olduğu* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olduğu* bağlı olarak, sırasıyla. Örnek uygulamada `OldElement` özelliği `null` ve `NewElement` özelliği, bir başvuru içerecek `CustomMap` örneği.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde her platforma özgü işleyici sınıfı, sistemi, yerel denetim özelleştirme gerçekleştirmek için yerdir. Belirlenmiş bir başvuru platformunda kullanılan yerel denetlemek için erişilebilir `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetime başvuru aracılığıyla alınabilir `Element` özelliği.

Olay işleyicileri için abone olurken dikkatli'nin alınması gereken `OnElementChanged` yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

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

Yerel bir denetimi yapılandırılmalıdır ve yalnızca özel Oluşturucu yeni bir Xamarin.Forms öğe eklendiğinde olay işleyicileri abone. Benzer şekilde, abone tüm olay işleyicileri yalnızca Oluşturucu bağlı olduğu öğe değiştiğinde gelen aboneliği olması gerekir. Bu yaklaşımı benimsemeyi bellek sızıntılardan etkilese değil özel bir oluşturucu oluşturmaya yardımcı olur.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` özniteliği işleyici Xamarin.Forms ile kaydeder. Öznitelik, iki parametre – işlenen Xamarin.Forms özel denetimin tür adı ve özel Oluşturucu türü adını alır. `assembly` Özniteliğiyle önek öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki bölümlerde her platforma özgü özel Oluşturucu sınıfın uygulaması açıklanmaktadır.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki ekran görüntüleri, özelleştirmeden önce ve sonra haritayı göster:

![](customized-pin-images/map-layout-ios.png "Harita denetiminin özelleştirmeden önce ve sonra")

İos'ta PIN olarak adlandırılan bir *ek açıklama*, ve özel bir görüntü veya sistem tarafından tanımlanan bir PIN çeşitli renklerinin olabilir. Ek açıklamalar isteğe bağlı olarak gösterilecek bir *belirtme çizgisi*, yanıt ek açıklama seçerek kullanıcı olarak görüntülenir. Belirtme çizgisi görüntüler `Label` ve `Address` özelliklerini `Pin` isteğe bağlı sol ve sağ aksesuar görünümlerle örneği. Yukarıdaki ekran görüntüsünde sol aksesuar bir monkey görüntüsü olan sağ aksesuar görünümü ile görünümdür *bilgi* düğmesi.

Aşağıdaki kod örneği, iOS platformu için özel Oluşturucu gösterir:

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

`OnElementChanged` Yöntemi aşağıdaki yapılandırmasını gerçekleştirir [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) koşuluyla yeni bir Xamarin.Forms öğesine bağlı özel oluşturucu örneği:

- [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) Özelliği `GetViewForAnnotation` yöntemi. Bu yöntem olduğunda çağrılır [ek açıklamanın konumu haritada görünür duruma](#Displaying_the_Annotation)ve görüntülemek için ek açıklama önceden özelleştirmek için kullanılır.
- Olay işleyicileri `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, ve `DidDeselectAnnotationView` olayları kayıtlı. Bu olaylar olduğunda harekete kullanıcı [doğru açıklama balonu aksesuar dokunduğunda](#Tapping_on_the_Right_Callout_Accessory_View)ve ne zaman kullanıcı [seçer](#Selecting_the_Annotation) ve [dilimleyiciye](#Deselecting_the_Annotation) ek açıklama, sırasıyla. Yalnızca ' % s'öğesi işleyici değişiklikleri iliştirildiğinde gelen olayları kaldırdınız.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Ek açıklama görüntüleme

`GetViewForAnnotation` Ek açıklama konumunu haritada görünür hale gelir ve görüntülemek için ek açıklama önceden özelleştirmek için kullanılan yöntemi çağrılır. Ek açıklamanın iki bölümden oluşur:

- `MkAnnotation` – Başlık, alt başlık ve ek açıklama konumunu içerir.
- `MkAnnotationView` – ek açıklama ve isteğe bağlı olarak, ek açıklama kullanıcı dokunduğunda gösterilen belirtme çizgisi temsil etmek için bir görüntü içerir.

`GetViewForAnnotation` Yöntemi kabul bir `IMKAnnotation` döndürür ve açıklamanın verileri içeren bir `MKAnnotationView` haritada görüntülemek için ve aşağıdaki kod örneğinde gösterilmiştir:

```csharp
protected override MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
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

Bu yöntem, ek açıklama, özel bir görüntü olarak görüntülenecek yerine, ek açıklama dokunulduğunda sistem tanımlı PIN ve, belirtme çizgisi ek içerik sola ve sağa ek açıklama başlığını ve adresini içeren görüntülenir sağlar . Bu şu şekilde gerçekleştirilir:

1. `GetCustomPin` Özel PIN veri ek açıklama için döndürülecek yöntemi çağrılır.
1. Bellekten kazanacak şekilde açıklamanın görünümü çağrısı ile yeniden kullanım için bir havuzda toplanır [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. `CustomMKAnnotationView` Sınıfını genişleten `MKAnnotationView` sınıfıyla `Id` ve `Url` aynı özelliklere karşılık gelen özelliklerle `CustomPin` örneği. Yeni bir örneğini `CustomMKAnnotationView` oluşturulur, ek açıklama olması şartıyla `null`:
    - `CustomMKAnnotationView.Image` Harita üzerindeki ek açıklama temsil edecek görüntüye özelliğini ayarlayın.
    - `CustomMKAnnotationView.CalloutOffset` Özelliği bir `CGPoint` ek açıklama belirtme çizgisi ortalanacağını belirtir.
    - `CustomMKAnnotationView.LeftCalloutAccessoryView` Özelliği, açıklama başlığını ve adresini solunda görünecek bir monkey görüntüsü için ayarlanır.
    - `CustomMKAnnotationView.RightCalloutAccessoryView` Özelliği bir *bilgi* ek açıklama başlığını ve adresini sağında görünen düğme.
    - `CustomMKAnnotationView.Id` Özelliği `CustomPin.Id` özelliği tarafından döndürülen `GetCustomPin` yöntemi. Bu ek açıklama, sahip olacak şekilde tanımlanmasını sağlar [belirtme çizgisi ek özelleştirilebilir](#Selecting_the_Annotation), isterseniz.
    - `CustomMKAnnotationView.Url` Özelliği `CustomPin.Url` özelliği tarafından döndürülen `GetCustomPin` yöntemi. URL ne zaman gidilecek kullanıcı [doğru açıklama balonu aksesuar Görünümü'nde görüntülenen düğmeye dokunduğunda](#Tapping_on_the_Right_Callout_Accessory_View).
1. [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) Özelliği `true` belirtme çizgisi ek açıklama dokunduğunuzda görüntülenir.
1. Ek açıklama, görüntüleme için harita üzerinde döndürülür.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Ek açıklama seçme

Kullanıcı üzerindeki ek açıklama, dokunduğunda `DidSelectAnnotationView` sırayla yürütür olay harekete geçirilir `OnDidSelectAnnotationView` yöntemi:

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

Ekleyerek mevcut belirtme çizgisi (sol ve sağ aksesuar görünümleri içeren) Bu yöntemin genişlettiği bir `UIView` Seçili ek açıklama sahip olması koşuluyla Xamarin logo görüntüsü içeren örneği bunu kendi `Id` özelliği için `Xamarin`. Bu senaryolar için farklı ek açıklamalar farklı çağrılar burada görüntülenebilir sağlar. `UIView` Örneği, mevcut belirtme çizgisi üzerinde ortalanmış görüntülenir.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Doğru açıklama balonu Donatı görünümünde dokunarak

Kullanıcı ne zaman dokunduğunda üzerinde *bilgi* doğru açıklama balonu aksesuar görünümünde düğme `CalloutAccessoryControlTapped` sırayla yürütür olay harekete geçirilir `OnCalloutAccessoryControlTapped` yöntemi:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Bu yöntem, bir web tarayıcısı açılır ve depolanan adresine gider `CustomMKAnnotationView.Url` özelliği. Adres oluştururken tanımlandığına dikkat edin `CustomPin` .NET Standard kitaplığı projesi koleksiyonunda.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Ek açıklama kaldırma

Ek açıklama görüntülenir ve kullanıcının harita üzerinde dokunduğunda `DidDeselectAnnotationView` sırayla yürütür olay harekete geçirilir `OnDidDeselectAnnotationView` yöntemi:

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

Bu yöntem, mevcut belirtme çizgisi seçilmezse, belirtme çizgisi (Xamarin logo görüntüsü) genişletilmiş bir parçası, ayrıca görüntülenmesini durdurur ve kaynaklarını serbest bırakılacak sağlar.

Özelleştirme hakkında daha fazla bilgi için bir `MKMapView` örneği için bkz. [iOS haritalar](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Aşağıdaki ekran görüntüleri, özelleştirmeden önce ve sonra haritayı göster:

![](customized-pin-images/map-layout-android.png "Harita denetiminin özelleştirmeden önce ve sonra")

Android'de PIN olarak adlandırılan bir *işaret*, ve özel bir görüntü veya çeşitli renkler sistem tarafından tanımlanan bir işaretçi ya da olabilir. İşaretçileri gösterebilir bir *bilgi penceresinin*, yanıt işaret üzerinde dokunarak kullanıcı olarak görüntülenir. Bilgi penceresi görüntüler `Label` ve `Address` özelliklerini `Pin` örneği ve diğer içerik eklemek için özelleştirilebilir. Ancak, tek seferde yalnızca bir bilgi penceresi gösterilebilir.

Aşağıdaki kod örneği, Android platformu için özel Oluşturucu gösterir:

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

Özel oluşturucu, yeni bir Xamarin.Forms öğesine bağlı olması koşuluyla `OnElementChanged` yöntem çağrılarını `MapView.GetMapAsync` temel alan yöntemi `GoogleMap` görünümüne bağlanır. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` geçersiz kılma çağrılacak. Bu yöntem için bir olay işleyicisi kaydeder `InfoWindowClick` başlatılan olay [bilgi penceresinin tıklandığında](#Clicking_on_the_Info_Window)ve yalnızca ' % s'öğesi işleyici değişiklikleri iliştirildiğinde öğesinden kaldırıldı. `OnMapReady` Ayrıca çağrıları geçersiz `SetInfoWindowAdapter` yöntemi belirtmek için `CustomMapRenderer` sınıf örneği bilgi penceresi özelleştirmek için yöntemler sağlar.

`CustomMapRenderer` Sınıfının Implements `GoogleMap.IInfoWindowAdapter` arabirimini [bilgi penceresi özelleştirme](#Customizing_the_Info_Window). Bu arabirim, aşağıdaki yöntemlerden uygulanmalıdır olduğunu belirtir:

- `public Android.Views.View GetInfoWindow(Marker marker)` – Bu yöntem, bir özel bilgi penceresinin bir işaretleyici geri dönmek için çağrılır. Döndürürse `null`, varsayılan penceresi işleme kullanılır. Döndürürse bir `View`, ardından, `View` bilgi pencere çerçevesi içinde yer alır.
- `public Android.Views.View GetInfoContents(Marker marker)` – Bu yöntem döndürmek için çağrılan bir `View` bilgi penceresinin içeriğini içeren ve yalnızca olursa çağrılacak `GetInfoWindow` yöntemi döndürür `null`. Döndürürse `null`, bilgi pencere içeriğinin varsayılan işleme kullanılır.

Örnek uygulama, yalnızca bilgi penceresi içeriği özelleştirilir ve bu nedenle `GetInfoWindow` yöntemi döndürür `null` bunu etkinleştirmek için.

#### <a name="customizing-the-marker"></a>İşaretin özelleştirme

Bir işaretçi temsil etmek için kullanılan simge çağırarak özelleştirilebilir `MarkerOptions.SetIcon` yöntemi. Bu geçersiz kılma tarafından gerçekleştirilebilir `CreateMarker` her biri için çağrılan yöntem `Pin` eşlemesine eklenir:

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

Bu yöntem yeni bir oluşturur `MarkerOption` her bir örneğin `Pin` örneği. Konum, etiket ve işaretçisinin adresi ayarladıktan sonra simgesi ile ayarlanır `SetIcon` yöntemi. Bu yöntem bir `BitmapDescriptor` simgesi ile işlemek gereken verileri içeren bir nesne `BitmapDescriptorFactory` oluşturulmasını kolaylaştırmak için yardımcı yöntemler sağlayan bir sınıf `BitmapDescriptor`.

Kullanma hakkında daha fazla bilgi için `BitmapDescriptorFactory` bir işaret özelleştirmek için bkz: sınıf [bir işaret özelleştirme](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Bilgi penceresini özelleştirme

Kullanıcı işaretçisi üzerinde dokunduğunda `GetInfoContents` yöntemi yürütüldüğünde, aşağıdaki koşullarda `GetInfoWindow` yöntemi döndürür `null`. Aşağıdaki örnekte gösterildiği kod `GetInfoContents` yöntemi:

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

Bu yöntem döndürür bir `View` bilgi penceresinin içeriğini içeren. Bu şu şekilde gerçekleştirilir:

- A `LayoutInflater` örneği alınır. Bu, karşılık gelen bir düzen XML dosyası oluşturmak için kullanılan `View`.
- `GetCustomPin` Yöntemi bilgileri penceresi için özel bir PIN verileri döndürmek için çağrılır.
- `XamarinMapInfoWindow` Düzeni varsa şişirileceğini `CustomPin.Id` özelliğini eşittir `Xamarin`. Aksi takdirde, `MapInfoWindow` Düzen şişirileceğini. Bu senaryolar için farklı işaretçiler farklı bilgi pencere düzenlerini burada görüntülenebilir sağlar.
- `InfoWindowTitle` Ve `InfoWindowSubtitle` kaynakları inflated düzeninden alınır ve bunların `Text` özelliklerini ayarlamak için karşılık gelen verilerden `Marker` şartıyla kaynaklara olmayan örnek `null`.
- `View` Örneği görüntülenmek üzere harita üzerinde döndürülür.

> [!NOTE]
> Bir bilgi penceresi Canlı değil `View`. Bunun yerine, Android dönüştürecek `View` statik bir bit eşlem ve, bir görüntü olarak görüntüleyin. Tıklama olayları bir tıklama olayı için herhangi bir dokunma olayları veya hareketlerini yanıt veremez ve her bir denetim bilgileri penceresinde kendi için yanıt yanıt verebilir, bir bilgi penceresi sırasında anlamına gelir.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Bilgileri penceresinde tıklayarak

Kullanıcı bilgileri penceresinde tıkladığında `InfoWindowClick` sırayla yürütür olay harekete geçirilir `OnInfoWindowClick` yöntemi:

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

Bu yöntem, bir web tarayıcısı açılır ve depolanan adresine gider `Url` özelliğini `CustomPin` örnek `Marker`. Adres oluştururken tanımlandığına dikkat edin `CustomPin` .NET Standard kitaplığı projesi koleksiyonunda.

Özelleştirme hakkında daha fazla bilgi için bir `MapView` örneği için bkz. [haritalar API'si](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Aşağıdaki ekran görüntüleri, özelleştirmeden önce ve sonra haritayı göster:

![](customized-pin-images/map-layout-uwp.png "Harita denetiminin özelleştirmeden önce ve sonra")

UWP üzerinde PIN çağrılır bir *Haritası simgesi*, ve özel bir görüntü veya sistem tarafından tanımlanan varsayılan görüntü ya da olabilir. Harita simgesini gösterebilirsiniz bir `UserControl`, yanıt harita simgesine dokunarak kullanıcı olarak görüntülenir. `UserControl` Herhangi bir içeriği görüntüleyebilir de dahil olmak üzere `Label` ve `Address` özelliklerini `Pin` örneği.

Aşağıdaki kod örneği, UWP özel Oluşturucu gösterir:

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

`OnElementChanged` Yöntemi özel Oluşturucu, yeni bir Xamarin.Forms öğesine bağlı olması koşuluyla aşağıdaki işlemleri gerçekleştirir:

- Temizlendiğinden `MapControl.Children` bir olay işleyicisi için kaydolmadan önce var olan kullanıcı arabirimi öğeleri eşlemden kaldırmak için koleksiyon `MapElementClick` olay. Kullanıcı dokunduğunda veya tıkladığında bu olay harekete bir `MapElement` üzerinde `MapControl`ve yalnızca ' % s'öğesi işleyici değişiklikleri iliştirildiğinde öğesinden kaldırıldı.
- Her pın'de `customPins` koleksiyon görüntülenir harita üzerinde doğru coğrafi konumda şu şekilde:
  - PIN için konum olarak oluşturulan bir `Geopoint` örneği.
  - A `MapIcon` örneği PIN temsil etmek için oluşturulur.
  - Temsil etmek için kullanılan görüntünün `MapIcon` ayarlanarak belirtilir `MapIcon.Image` özelliği. Harita üzerinde diğer öğeleri tarafından görünmeyebilir gibi ancak harita simgesinin görüntüsü her zaman gösterilecek, garanti edilmez. Bu nedenle, harita simgesinin `CollisionBehaviorDesired` özelliği `MapElementCollisionBehavior.RemainVisible`görünür kalmasını sağlamak için.
  - Konumunu `MapIcon` ayarlanarak belirtilir `MapIcon.Location` özelliği.
  - `MapIcon.NormalizedAnchorPoint` Resimdeki işaretçi yaklaşık konumunu özelliğini ayarlayın. Bu özellik temsil eden resmin sol üst köşesinde, varsayılan değeri (0,0) korur, harita yakınlaştırma düzeyini değişiklikleri farklı bir konuma işaret eden görüntü neden olabilir.
  - `MapIcon` Örneği eklenir `MapControl.MapElements` koleksiyonu. Üzerinde görüntülenmesini Haritası simgesi sonuçlanır `MapControl`.

> [!NOTE]
> Aynı görüntüyü birden fazla eşleme simgelerini kullanırken `RandomAccessStreamReference` örneği bildirilen en iyi performans için sayfa veya uygulama düzeyinde.

#### <a name="displaying-the-usercontrol"></a>UserControl görüntüleme

Bir kullanıcı eşleme simgesinde dokunduğunda `OnMapElementClick` yöntemi yürütülür. Aşağıdaki kod örneği, bu yöntem gösterir:

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

Bu yöntem, oluşturur bir `UserControl` örneği PIN hakkındaki bilgileri görüntüler. Bu şu şekilde gerçekleştirilir:

- `MapIcon` Örneği alınır.
- `GetCustomPin` Yöntemi görüntülenecek özel bir PIN verileri döndürmek için çağrılır.
- A `XamarinMapOverlay` örneği özel PIN verileri görüntülemek için oluşturulur. Bir kullanıcı denetimi sınıftır.
- Görüntülenecek coğrafi konumda `XamarinMapOverlay` üzerinde örnek `MapControl` olarak oluşturulan bir `Geopoint` örneği.
- `XamarinMapOverlay` Örneği eklenir `MapControl.Children` koleksiyonu. Bu koleksiyon, haritada gösterilecek XAML kullanıcı arabirimi öğeleri içerir.
- Coğrafi konumunu `XamarinMapOverlay` örneği harita üzerinde ayarlanmış çağırarak `SetLocation` yöntemi.
- Göreli konum `XamarinMapOverlay` belirtilen konuma karşılık gelen, örnek çağırarak ayarlanmış `SetNormalizedAnchorPoint` yöntemi. Bu değişikliklerin harita sonucunda yakınlaştırma düzeyini de sağlar `XamarinMapOverlay` her zaman doğru konumda görüntülenen örnek.

Alternatif olarak, PIN hakkında bilgi harita üzerinde zaten görüntüleniyor, dokunma harita üzerinde kaldırır `XamarinMapOverlay` gelen örnek `MapControl.Children` koleksiyonu.

#### <a name="tapping-on-the-information-button"></a>Bilgi düğmesine dokunarak

Kullanıcı ne zaman dokunduğunda üzerinde *bilgi* düğmesine `XamarinMapOverlay` kullanıcı denetimi `Tapped` sırayla yürütür olay harekete geçirilir `OnInfoButtonTapped` yöntemi:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Bu yöntem, bir web tarayıcısı açılır ve depolanan adresine gider `Url` özelliği `CustomPin` örneği. Adres oluştururken tanımlandığına dikkat edin `CustomPin` .NET Standard kitaplığı projesi koleksiyonunda.

Özelleştirme hakkında daha fazla bilgi için bir `MapControl` örneği için bkz. [Haritalar ve konum genel bakış](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) MSDN'de.

## <a name="summary"></a>Özet

Bu makalede gösterilen için özel Oluşturucu Oluşturma `Map` geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak denetim. Xamarin.Forms.Maps kullanıcılar için deneyimi hızlı ve tanıdık bir eşleme sağlamak için API'ler her platformda yerel eşlemesi kullanmak haritalar görüntülemek için bir çoklu platform soyutlama sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Haritalar denetimi](~/xamarin-forms/user-interface/map.md)
- [iOS haritalar](~/ios/user-interface/controls/ios-maps/index.md)
- [Haritalar API’si](~/android/platform/maps-and-location/maps/maps-api.md)
- [Özelleştirilmiş PIN (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
