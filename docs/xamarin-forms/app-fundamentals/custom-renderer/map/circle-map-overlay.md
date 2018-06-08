---
title: Bir harita döngüsel bir alanı vurgulama
description: Bu makalede, döngüsel bir harita alanı vurgulamak için bir harita için döngüsel bir katmana ekleme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 1bec7a318bebc40c050104a51408473d89f483a5
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846529"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Bir harita döngüsel bir alanı vurgulama

_Bu makalede, döngüsel bir harita alanı vurgulamak için bir harita için döngüsel bir katmana ekleme açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Bir katmana harita üzerinde katmanlı bir grafiktir. Yer paylaşımları Haritası uzaklaştırılacağını gibi ölçeklendirilebilen çizim grafik içeriği destekler. Aşağıdaki ekran görüntüleri için bir harita döngüsel bir katmana ekleme sonucu göster:

![](circle-map-overlay-images/screenshots.png)

Zaman bir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) denetimi, bir Xamarin.Forms uygulaması iOS tarafından işlenir `MapRenderer` sınıf örneği, hangi sırayla yerel başlatır `MKMapView` denetim. Android platformunda `MapRenderer` sınıfı başlatır yerel `MapView` denetim. Üzerinde Evrensel Windows Platformu (UWP), `MapRenderer` sınıfı başlatır yerel `MapControl`. Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü harita özelleştirmeler uygulamak için avantajlarından alınabilir bir `Map` her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Özelleştirme](#Customizing_the_Map) özel bir işleyici eşlemesi için her platformda oluşturarak eşleme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/") başlatılmış ve kullanılmadan önce yapılandırılması gerekir. Daha fazla bilgi için bkz: [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Özel oluşturucu kullanılarak bir harita özelleştirme hakkında daha fazla bilgi için bkz: [bir harita PIN özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Özel eşleme oluşturma

Oluşturma bir `CustomCircle` olan sınıfı `Position` ve `Radius` özellikleri:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Ardından, bir alt sınıfı oluşturun [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) türünde bir özellik ekler sınıfı, `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Özel Harita kullanma

Tüketen `CustomMap` bir örneğini XAML sayfası örneğinde bildirme tarafından denetimi:

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

Alternatif olarak, tüketen `CustomMap` bir örneğini C# sayfa örneğinde bildirme tarafından denetimi:

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

Initialize `CustomMap` kontrol gerektiği gibi:

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

Bu başlatma ekler [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) ve `CustomCircle` örnekler özel Harita ve haritanın görünümüyle konumlandırır [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) konumunu ve yakınlaştırma değişiklikleri yöntemi Harita oluşturarak düzeyini bir [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) gelen bir [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) ve [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Harita özelleştirme

Özel oluşturucu artık döngüsel katmana eşlemesine eklemek için her uygulama projesi eklenmesi gerekir.

#### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` yöntemi döngüsel katmana eklemek için:

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

Yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı koşuluyla, bu yöntem aşağıdaki yapılandırma gerçekleştirir:

- `MKMapView.OverlayRenderer` Özelliği için karşılık gelen bir temsilci ayarlayın.
- Daireye statik ayarlanarak oluşturulur `MKCircle` dairenin merkezi ve dairenin RADIUS metre belirtir nesnesi.
- Daire harita çağrılarak eklenir `MKMapView.AddOverlay` yöntemi.

Ardından, uygulama `GetOverlayRenderer` katmana çizmeye özelleştirmek için yöntemi:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` ve `OnMapReady` yöntemleri döngüsel katmana eklemek için:

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

`OnElementChanged` Yöntem çağrılarını `MapView.GetMapAsync` temel alır yöntemi `GoogleMap` , bağlı görünümüne koşuluyla özel Oluşturucu yeni bir Xamarin.Forms öğesine bağlı. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` yöntemi çağrılabilir, daire oluşturarak oluşturulduğu bir `CircleOptions` dairenin merkezi ve dairenin RADIUS metre belirtir nesnesi. Daireye sonra eşlemeye çağrılarak eklenir `NativeMap.AddCircle` yöntemi.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` yöntemi döngüsel katmana eklemek için:

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

Yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı koşuluyla, bu yöntem aşağıdaki işlemleri gerçekleştirir:

- Daire konumunu ve RADIUS alınır `CustomMap.Circle` özelliği ve geçirilen `GenerateCircleCoordinates` enlem ve boylam oluşturur yöntemi daire çevre koordinatları. Bu yardımcı yöntemi için kod aşağıda verilmiştir.
- Daire çevre koordinatları dönüştürülen bir `List` , `BasicGeoposition` koordinatları.
- Daireye örneği tarafından oluşturulan bir `MapPolygon` nesnesi. `MapPolygon` Sınıfı ayarlayarak haritada çok nokta şekli görüntülemek için kullanılır, `Path` özelliğine bir `Geopath` şekli koordinatları içeren nesne.
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

Bu makalede haritaya döngüsel bir harita alanı vurgulamak için döngüsel bir katmana ekleme konusunda açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Döngüsel harita Ovlerlay (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Bir Harita Raptiyesini Özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
