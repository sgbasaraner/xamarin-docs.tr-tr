---
title: Bir rota bir harita vurgulama
description: "Bu makalede, bir çoklu çizgi katmana bir harita Ekle açıklanmaktadır. Çoklu satır bir katmana, genellikle bir haritada bir yol göstermek veya gerekli olan herhangi bir şekli oluşturmak için kullanılan bağlantılı çizgi parçaları dizisidir."
ms.topic: article
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: c7becef16009596148b4de28e4e8f6892cb44fe1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="highlighting-a-route-on-a-map"></a>Bir rota bir harita vurgulama

_Bu makalede, bir çoklu çizgi katmana bir harita Ekle açıklanmaktadır. Çoklu satır bir katmana, genellikle bir haritada bir yol göstermek veya gerekli olan herhangi bir şekli oluşturmak için kullanılan bağlantılı çizgi parçaları dizisidir._

## <a name="overview"></a>Genel Bakış

Bir katmana harita üzerinde katmanlı bir grafiktir. Yer paylaşımları Haritası uzaklaştırılacağını gibi ölçeklendirilebilen çizim grafik içeriği destekler. Aşağıdaki ekran görüntüleri haritaya çoklu çizgi katmana ekleme sonucu göster:

![](polyline-map-overlay-images/screenshots.png)

Zaman bir [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) denetimi, bir Xamarin.Forms uygulaması iOS tarafından işlenir `MapRenderer` sınıf örneği, hangi sırayla yerel başlatır `MKMapView` denetim. Android platformunda `MapRenderer` sınıfı başlatır yerel `MapView` denetim. Üzerinde Evrensel Windows Platformu (UWP), `MapRenderer` sınıfı başlatır yerel `MapControl`. Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü harita özelleştirmeler uygulamak için avantajlarından alınabilir bir `Map` her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Özelleştirme](#Customizing_the_Map) özel bir işleyici eşlemesi için her platformda oluşturarak eşleme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) başlatılmış ve kullanılmadan önce yapılandırılması gerekir. Daha fazla bilgi için bkz: [ `Maps Control` ](~/xamarin-forms/user-interface/map.md).

Özel oluşturucu kullanılarak bir harita özelleştirme hakkında daha fazla bilgi için bkz: [bir harita PIN özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Özel eşleme oluşturma

Öğesinin bir alt kümesi oluşturmak [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) ekler sınıfı, bir `RouteCoordinates` özelliği:

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

`RouteCoordinates` Özelliği vurgulanmasını için rota tanımlama koordinatları koleksiyonu depolar.

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Bu başlatma rota vurgulanmasını eşlemek tanımlamak için enlem ve boylam koordinatları, bir dizi belirtir. Haritanın görünümüyle sonra konumlandırır [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) konumunu ve harita yakınlaştırma düzeyini oluşturarak yöntemi bir [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) gelen bir [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) ve [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Harita özelleştirme

Çoklu satır katmana eşlemesine eklemek için her uygulama projesi için özel Oluşturucu artık eklenmiş olması gerekir.

#### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` çoklu çizgi katmana ekleme yöntemi:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

Yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı koşuluyla, bu yöntem aşağıdaki yapılandırma gerçekleştirir:

- `MKMapView.OverlayRenderer` Özelliği için karşılık gelen bir temsilci ayarlayın.
- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.RouteCoordinates` özelliği ve bir dizisi olarak depolanan `CLLocationCoordinate2D` örnekleri.
- Çoklu çizgiyi statik çağrılarak oluşturulan `MKPolyline.FromCoordinates` enlem ve boylam her noktasının belirtir yöntemi.
- Çoklu çizgiyi eşlemeye çağrılarak eklenir `MKMapView.AddOverlay` yöntemi.

Ardından, uygulama `GetOverlayRenderer` katmana çizmeye özelleştirmek için yöntemi:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` ve `OnMapReady` çoklu çizgi katmana eklemek için yöntemleri:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

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
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

`OnElementChanged` Yöntemi enlem ve boylam koordinatları koleksiyonunu alır `CustomMap.RouteCoordinates` özelliği ve üye değişkeninde depolar. Daha sonra çağırır `MapView.GetMapAsync` temel alır yöntemi `GoogleMap` , bağlı görünümüne koşuluyla özel Oluşturucu yeni bir Xamarin.Forms öğesine bağlı. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` yöntemi çağrılabilir, çoklu çizgi oluşturarak oluşturulduğu bir `PolylineOptions` enlem ve boylam her noktasının belirtir nesnesi. Çoklu çizgiyi sonra eşlemeye çağrılarak eklenir `NativeMap.AddPolyline` yöntemi.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Öğesinin bir alt kümesi oluşturmak `MapRenderer` sınıfı ve geçersiz kılma kendi `OnElementChanged` çoklu çizgi katmana ekleme yöntemi:

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
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

Yeni bir Xamarin.Forms öğesi özel Oluşturucu bağlı koşuluyla, bu yöntem aşağıdaki işlemleri gerçekleştirir:

- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.RouteCoordinates` özelliği ve dönüştürülen içine bir `List` , `BasicGeoposition` koordinatları.
- Çoklu çizgiyi örneği tarafından oluşturulan bir `MapPolyline` nesnesi. `MapPolygon` Sınıfı, bir satır olarak ayarlayarak haritada görüntülemek için kullanılır, `Path` özelliğine bir `Geopath` çizgi koordinatları içeren nesne.
- Çoklu çizgi ekleyerek haritada işlenmeden `MapControl.MapElements` koleksiyonu.

## <a name="summary"></a>Özet

Bu makalede bir haritada bir yol göstermek veya gerekli olan herhangi bir şekli form için bir harita için bir çoklu çizgi katmana ekleme açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Çoklu çizgi harita Ovlerlay (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Bir harita PIN özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
