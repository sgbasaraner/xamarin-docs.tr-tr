---
title: Bir haritadaki dairesel bir alanı vurgulama
description: Bu makalede, harita dairesel bir alanı vurgulamak için bir eşleme için döngüsel bir katmana ekleme açıklanmaktadır. İOS ve Android API döngüsel katmana haritaya eklemek için sunarken, UWP üzerinde katmana Çokgen işlenir.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 3064296d4c78a3342fb27afc971c37a029987e5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998563"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Bir haritadaki dairesel bir alanı vurgulama

_Bu makalede, harita dairesel bir alanı vurgulamak için bir eşleme için döngüsel bir katmana ekleme açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Bir katmana, harita üzerindeki katmanlı bir grafiktir. Katmanları ile eşleme, yakınlaştırılmış olarak ölçeklendirilen çizim grafik içeriği destekler. Aşağıdaki ekran görüntüleri, döngüsel bir katmana haritaya eklemenin sonucunu göstermektedir:

![](circle-map-overlay-images/screenshots.png)

Olduğunda bir [ `Map` ](xref:Xamarin.Forms.Maps.Map) bir Xamarin.Forms uygulaması iOS tarafından işlenen denetim `MapRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `MKMapView` denetimi. Android platformunda `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapView` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapControl`. İşleme sürecini avantajı için özel Oluşturucu oluşturarak platforma özgü harita özelleştirmeleri uygulamak için uygulanabilecek bir `Map` her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Özelleştirme](#Customizing_the_Map) eşleme için özel Oluşturucu her platformda oluşturarak eşleme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) başlatılır ve yapılandırılmış önce kullanır. Daha fazla bilgi için bkz. [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Özel oluşturucu kullanılarak bir haritayı özelleştirme hakkında daha fazla bilgi için bkz: [bir harita Raptiyesini özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Özel harita oluşturma

Oluşturma bir `CustomCircle` sınıf `Position` ve `Radius` özellikleri:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Öğesinin oluşturup [ `Map` ](xref:Xamarin.Forms.Maps.Map) sınıf türünde bir özellik ekler, `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Özel Harita kullanma

Tüketen `CustomMap` denetimi bir örneğini XAML sayfası örneğinde bildirmek:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Alternatif olarak, tüketen `CustomMap` denetimi bir örneğini C# sayfa örnekteki bildirmek:

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

Başlatma `CustomMap` gerektiği gibi denetleyen:

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    var pin = new Pin {
      Type = PinType.Place,
      Position = new Position (37.79752, -122.40183),
      Label = "Xamarin San Francisco Office",
      Address = "394 Pacific Ave, San Francisco CA"
    };

    var position = new Position (37.79752, -122.40183);
    customMap.Circle = new CustomCircle {
      Position = position,
      Radius = 1000
    };

    customMap.Pins.Add (pin);
    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (position, Distance.FromMiles (1.0)));
  }
}
```

Bu başlatma ekler [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) ve `CustomCircle` özel eşleme için örnekler ve haritanın görünüm ile konumlandırır [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) yakınlaştırma ve konumunu değiştiren yöntemi Harita oluşturarak düzeyi bir [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) gelen bir [ `Position` ](xref:Xamarin.Forms.Maps.Position) ve [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Haritayı özelleştirme

Döngüsel katmana haritaya eklemek için her bir uygulama projesine artık özel Oluşturucu eklenmesi gerekir.

#### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` yöntemi döngüsel katmana eklemek için:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKCircleRenderer circleRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    circleRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                var circle = formsMap.Circle;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                var circleOverlay = MKCircle.Circle(new CoreLocation.CLLocationCoordinate2D(circle.Position.Latitude, circle.Position.Longitude), circle.Radius);
                nativeMap.AddOverlay(circleOverlay);
            }
        }
        ...
    }
}

```

Özel oluşturucu, yeni bir Xamarin.Forms öğesine bağlı şartıyla bu yöntem aşağıdaki yapılandırmayı gerçekleştirir:

- `MKMapView.OverlayRenderer` Özelliği için karşılık gelen bir temsilci ayarlayın.
- Statik ayarlayarak daire oluşturulan `MKCircle` nesnesini Orta dairenin ve RADIUS dairenin metre cinsinden belirtir.
- Daire çağırarak haritaya eklenen `MKMapView.AddOverlay` yöntemi.

Ardından, uygulama `GetOverlayRenderer` katmana işlenmesi özelleştirmek için yöntemi:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKCircleRenderer circleRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (circleRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          circleRenderer = new MKCircleRenderer(overlay as MKCircle) {
              FillColor = UIColor.Red,
              Alpha = 0.4f
          };
      }
      return circleRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` ve `OnMapReady` yöntemleri döngüsel katmana eklemek için:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        CustomCircle circle;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Xamarin.Forms.Maps.Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                circle = formsMap.Circle;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var circleOptions = new CircleOptions();
            circleOptions.InvokeCenter(new LatLng(circle.Position.Latitude, circle.Position.Longitude));
            circleOptions.InvokeRadius(circle.Radius);
            circleOptions.InvokeFillColor(0X66FF0000);
            circleOptions.InvokeStrokeColor(0X66FF0000);
            circleOptions.InvokeStrokeWidth(0);

            NativeMap.AddCircle(circleOptions);
        }
    }
}
```

`OnElementChanged` Yöntem çağrılarını `MapView.GetMapAsync` temel alan, yöntem `GoogleMap` , bağlı görünümüne koşuluyla özel Oluşturucu, yeni bir Xamarin.Forms öğesine eklenir. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` yöntemi çağrılacak, dairenin örnekleme tarafından oluşturulduğu bir `CircleOptions` nesnesini Orta dairenin ve RADIUS dairenin metre cinsinden belirtir. Daire çağırarak haritaya daha sonra eklenen `NativeMap.AddCircle` yöntemi.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` yöntemi döngüsel katmana eklemek için:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        const int EarthRadiusInMeteres = 6371000;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }
            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;
                var circle = formsMap.Circle;

                var coordinates = new List<BasicGeoposition>();
                var positions = GenerateCircleCoordinates(circle.Position, circle.Radius);
                foreach (var position in positions)
                {
                    coordinates.Add(new BasicGeoposition { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
        // GenerateCircleCoordinates helper method (below)
    }
}
```

Özel oluşturucu, yeni bir Xamarin.Forms öğesine bağlı şartıyla bu yöntem aşağıdaki işlemleri gerçekleştirir:

- Daire konumunu ve RADIUS alınır `CustomMap.Circle` özelliği ve geçirilen `GenerateCircleCoordinates` enlem ve boylam oluşturan yöntemi daire çevre koordinatları. Bu yardımcı yöntem için kod, aşağıda gösterilmiştir.
- Daire çevre koordinatları metne dönüştürülecek bir `List` , `BasicGeoposition` koordinatları.
- Daire örneği tarafından oluşturulan bir `MapPolygon` nesne. `MapPolygon` Sınıfı ayarlayarak bir çok noktası şekli haritada görüntülemek için kullanılır, `Path` özelliğini bir `Geopath` şekli koordinatları içeren nesne.
- Çokgen harita üzerinde ekleyerek işlenen `MapControl.MapElements` koleksiyonu.


```
List<Position> GenerateCircleCoordinates(Position position, double radius)
{
    double latitude = position.Latitude.ToRadians();
    double longitude = position.Longitude.ToRadians();
    double distance = radius / EarthRadiusInMeteres;
    var positions = new List<Position>();

    for (int angle = 0; angle <=360; angle++)
    {
        double angleInRadians = ((double)angle).ToRadians();
        double latitudeInRadians = Math.Asin(Math.Sin(latitude) * Math.Cos(distance) + Math.Cos(latitude) * Math.Sin(distance) * Math.Cos(angleInRadians));
        double longitudeInRadians = longitude + Math.Atan2(Math.Sin(angleInRadians) * Math.Sin(distance) * Math.Cos(latitude), Math.Cos(distance) - Math.Sin(latitude) * Math.Sin(latitudeInRadians));

        var pos = new Position(latitudeInRadians.ToDegrees(), longitudeInRadians.ToDegrees());
        positions.Add(pos);
    }

    return positions;
}
```

## <a name="summary"></a>Özet

Bu makalede bir haritaya harita dairesel bir alanı vurgulamak için döngüsel bir katmana eklemek açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Daire harita Ovlerlay (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Bir Harita Raptiyesini Özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
