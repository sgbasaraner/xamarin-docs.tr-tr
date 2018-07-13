---
title: Bir haritadaki rotayı vurgulama
description: Bu makalede, bir çoklu çizgi katmana bir haritaya eklemek açıklanmaktadır. Çoklu katman, genellikle bir yol bir haritada gösterme veya gerekli olan herhangi bir şekli oluşturmak için kullanılan bağlı çizgi segmentleri dizisidir.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 786f050495d4682b719178f2723c482929544678
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998729"
---
# <a name="highlighting-a-route-on-a-map"></a>Bir haritadaki rotayı vurgulama

_Bu makalede, bir çoklu çizgi katmana bir haritaya eklemek açıklanmaktadır. Çoklu katman, genellikle bir yol bir haritada gösterme veya gerekli olan herhangi bir şekli oluşturmak için kullanılan bağlı çizgi segmentleri dizisidir._

## <a name="overview"></a>Genel Bakış

Bir katmana, harita üzerindeki katmanlı bir grafiktir. Katmanları ile eşleme, yakınlaştırılmış olarak ölçeklendirilen çizim grafik içeriği destekler. Aşağıdaki ekran görüntüleri bir çizgi katmana haritaya eklemenin sonucunu göstermektedir:

![](polyline-map-overlay-images/screenshots.png)

Olduğunda bir [ `Map` ](xref:Xamarin.Forms.Maps.Map) bir Xamarin.Forms uygulaması iOS tarafından işlenen denetim `MapRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `MKMapView` denetimi. Android platformunda `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapView` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapControl`. İşleme sürecini avantajı için özel Oluşturucu oluşturarak platforma özgü harita özelleştirmeleri uygulamak için uygulanabilecek bir `Map` her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Özelleştirme](#Customizing_the_Map) eşleme için özel Oluşturucu her platformda oluşturarak eşleme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) başlatılır ve yapılandırılmış önce kullanır. Daha fazla bilgi için bkz. [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Özel oluşturucu kullanılarak bir haritayı özelleştirme hakkında daha fazla bilgi için bkz: [bir harita Raptiyesini özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Özel harita oluşturma

Oluşturma bir öğesinin [ `Map` ](xref:Xamarin.Forms.Maps.Map) ekler sınıfı, bir `RouteCoordinates` özelliği:

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

`RouteCoordinates` Özelliği vurgulanmasını rota tanımlayan koordinat koleksiyonunu depolar.

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Bu başlatma, bir dizi yol vurgulanmasını haritada tanımlamak için enlem ve boylam koordinatları belirtir. Ardından haritanın görünüm ile konumlandırır [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) oluşturarak konumu ve harita yakınlaştırma düzeyini değiştiren yöntemi bir [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) gelen bir [ `Position` ](xref:Xamarin.Forms.Maps.Position) ve [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Haritayı özelleştirme

Çoklu çizgi katmana haritaya eklemek için her bir uygulama projesine artık özel Oluşturucu eklenmesi gerekir.

#### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` yöntemi çoklu katman eklemek için:

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

Özel oluşturucu, yeni bir Xamarin.Forms öğesine bağlı şartıyla bu yöntem aşağıdaki yapılandırmayı gerçekleştirir:

- `MKMapView.OverlayRenderer` Özelliği için karşılık gelen bir temsilci ayarlayın.
- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.RouteCoordinates` özelliği ve bir dizi olarak depolanan `CLLocationCoordinate2D` örnekleri.
- Çoklu çizgi statik çağırarak oluşturulan `MKPolyline.FromCoordinates` yöntemi enlem ve boylamını her noktasının belirtir.
- Çoklu çizgi çağırarak haritaya eklenen `MKMapView.AddOverlay` yöntemi.

Ardından, uygulama `GetOverlayRenderer` katmana işlenmesi özelleştirmek için yöntemi:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` ve `OnMapReady` yöntemler çoklu katman eklemek için:

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

`OnElementChanged` Yöntemi enlem ve boylam koordinatları koleksiyonunu alır `CustomMap.RouteCoordinates` özelliği ve bunları bir üye değişkeni depolar. Ardından `MapView.GetMapAsync` temel alan yöntemi `GoogleMap` , bağlı görünümüne koşuluyla özel Oluşturucu, yeni bir Xamarin.Forms öğesine eklenir. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` yöntemi çağrılacak, çoklu çizgi örnekleme tarafından oluşturulduğu bir `PolylineOptions` enlem ve boylamını her noktasının belirten bir nesne. Çoklu çizginin ardından haritaya çağrılarak eklenir `NativeMap.AddPolyline` yöntemi.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` yöntemi çoklu katman eklemek için:

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

Özel oluşturucu, yeni bir Xamarin.Forms öğesine bağlı şartıyla bu yöntem aşağıdaki işlemleri gerçekleştirir:

- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.RouteCoordinates` özelliği ve dönüştürülen içine bir `List` , `BasicGeoposition` koordinatları.
- Çoklu çizgi örnekleme tarafından oluşturulan bir `MapPolyline` nesne. `MapPolygon` Sınıfı, bir satır olarak ayarlayarak haritada görüntülemek için kullanılır, `Path` özelliğini bir `Geopath` çizgi koordinatları içeren nesne.
- Çoklu çizgi ekleyerek harita üzerinde işlenen `MapControl.MapElements` koleksiyonu.

## <a name="summary"></a>Özet

Bu makalede, bir rota bir haritada gösterme veya gerekli olan herhangi bir şekli oluşturmak için bir harita için çoklu katman eklemek açıklanmıştır.


## <a name="related-links"></a>İlgili bağlantılar

- [Çoklu çizgi Haritası Ovlerlay (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Bir Harita Raptiyesini Özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
