---
title: Bir bölge bir harita vurgulama
description: Bu makalede, bir bölge harita üzerinde vurgulamak için bir harita Çokgen katmana eklemek anlatılmıştır. Kapalı bir şekil çokgenler olan ve bunların evin içindekiler doldurduğunuz.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: d87237015b9e3d896766894d552c650047137146
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="highlighting-a-region-on-a-map"></a>Bir bölge bir harita vurgulama

_Bu makalede, bir bölge harita üzerinde vurgulamak için bir harita Çokgen katmana eklemek anlatılmıştır. Kapalı bir şekil çokgenler olan ve bunların evin içindekiler doldurduğunuz._

## <a name="overview"></a>Genel Bakış

Bir katmana harita üzerinde katmanlı bir grafiktir. Yer paylaşımları Haritası uzaklaştırılacağını gibi ölçeklendirilebilen çizim grafik içeriği destekler. Aşağıdaki ekran görüntüleri için bir harita Çokgen katmana ekleme sonucu göster:

![](polygon-map-overlay-images/screenshots.png)

Zaman bir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) denetimi, bir Xamarin.Forms uygulaması iOS tarafından işlenir `MapRenderer` sınıf örneği, hangi sırayla yerel başlatır `MKMapView` denetim. Android platformunda `MapRenderer` sınıfı başlatır yerel `MapView` denetim. Üzerinde Evrensel Windows Platformu (UWP), `MapRenderer` sınıfı başlatır yerel `MapControl`. Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü harita özelleştirmeler uygulamak için avantajlarından alınabilir bir `Map` her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Özelleştirme](#Customizing_the_Map) özel bir işleyici eşlemesi için her platformda oluşturarak eşleme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) başlatılmış ve kullanılmadan önce yapılandırılması gerekir. Daha fazla bilgi için bkz. [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Özel oluşturucu kullanılarak bir harita özelleştirme hakkında daha fazla bilgi için bkz: [bir harita PIN özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Özel eşleme oluşturma

Öğesinin bir alt kümesi oluşturmak [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) ekler sınıfı, bir `ShapeCoordinates` özelliği:

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

`ShapeCoordinates` Özelliği vurgulanmasını bölge tanımlamasına koordinatları koleksiyonu depolar.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Bu başlatma vurgulanmasını eşlemek bölge tanımlamak için enlem ve boylam koordinatları, bir dizi belirtir. Haritanın görünümüyle sonra konumlandırır [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) konumunu ve harita yakınlaştırma düzeyini oluşturarak yöntemi bir [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) gelen bir [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) ve [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Harita özelleştirme

Harita Çokgen katmana eklemek için her uygulama projesi için özel Oluşturucu artık eklenmiş olması gerekir.

#### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` yöntemi Çokgen katmana eklemek için:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

Yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı koşuluyla, bu yöntem aşağıdaki yapılandırma gerçekleştirir:

- `MKMapView.OverlayRenderer` Özelliği için karşılık gelen bir temsilci ayarlayın.
- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.ShapeCoordinates` özelliği ve bir dizisi olarak depolanan `CLLocationCoordinate2D` örnekleri.
- Çokgen statik çağrılarak oluşturulan `MKPolygon.FromCoordinates` enlem ve boylam her noktasının belirtir yöntemi.
- Çokgen harita çağrılarak eklenir `MKMapView.AddOverlay` yöntemi. Bu yöntem ilk ve son noktaları bağlayan bir çizgi çizerek Çokgen otomatik olarak kapatılır.

Ardından, uygulama `GetOverlayRenderer` katmana çizmeye özelleştirmek için yöntemi:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` ve `OnMapReady` yöntemleri Çokgen katmana eklemek için:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

`OnElementChanged` Yöntemi enlem ve boylam koordinatları koleksiyonunu alır `CustomMap.ShapeCoordinates` özelliği ve üye değişkeninde depolar. Daha sonra çağırır `MapView.GetMapAsync` temel alır yöntemi `GoogleMap` , bağlı görünümüne koşuluyla özel Oluşturucu yeni bir Xamarin.Forms öğesine bağlı. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` yöntemi çağrılabilir, Çokgen oluşturarak oluşturulduğu bir `PolygonOptions` enlem ve boylam her noktasının belirtir nesnesi. Çokgen sonra eşlemeye çağrılarak eklenir `NativeMap.AddPolygon` yöntemi. Bu yöntem ilk ve son noktaları bağlayan bir çizgi çizerek Çokgen otomatik olarak kapatılır.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` yöntemi Çokgen katmana eklemek için:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
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

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

Yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı koşuluyla, bu yöntem aşağıdaki işlemleri gerçekleştirir:

- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.ShapeCoordinates` özelliği ve dönüştürülen içine bir `List` , `BasicGeoposition` koordinatları.
- Çokgen örneği tarafından oluşturulan bir `MapPolygon` nesnesi. `MapPolygon` Sınıfı ayarlayarak haritada çok nokta şekli görüntülemek için kullanılır, `Path` özelliğine bir `Geopath` şekli koordinatları içeren nesne.
- Çokgen harita üzerinde ekleyerek işlenen `MapControl.MapElements` koleksiyonu. Çokgen otomatik olarak ilk ve son noktaları bağlayan bir çizgi çizerek kapatılacak olduğunu unutmayın.

## <a name="summary"></a>Özet

Bu makalede bir bölge harita vurgulamak için bir harita için bir Çokgen katmana ekleme açıklanmıştır. Kapalı bir şekil çokgenler olan ve bunların evin içindekiler doldurduğunuz.


## <a name="related-links"></a>İlgili bağlantılar

- [Çokgen harita katmanını (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Bir Harita Raptiyesini Özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
