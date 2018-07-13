---
title: Bir haritadaki bölgeyi vurgulama
description: Bu makalede, harita üzerinde bir bölge vurgulamak için bir harita Çokgen katmana ekleme açıklanmaktadır. Kapalı şekil çokgenler olduğunu ve bunların evin içindekiler doldurmuş olmanız.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998140"
---
# <a name="highlighting-a-region-on-a-map"></a>Bir haritadaki bölgeyi vurgulama

_Bu makalede, bir Çokgen katmana harita üzerinde bir bölge vurgulamak için bir harita eklemek açıklanmıştır. Kapalı şekil çokgenler olduğunu ve bunların evin içindekiler doldurmuş olmanız._

## <a name="overview"></a>Genel Bakış

Bir katmana, harita üzerindeki katmanlı bir grafiktir. Katmanları ile eşleme, yakınlaştırılmış olarak ölçeklendirilen çizim grafik içeriği destekler. Aşağıdaki ekran görüntüleri için bir harita Çokgen katmana eklemenin sonucunu göstermektedir:

![](polygon-map-overlay-images/screenshots.png)

Olduğunda bir [ `Map` ](xref:Xamarin.Forms.Maps.Map) bir Xamarin.Forms uygulaması iOS tarafından işlenen denetim `MapRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `MKMapView` denetimi. Android platformunda `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapView` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `MapRenderer` sınıfın örneğini oluşturur, bir yerel `MapControl`. İşleme sürecini avantajı için özel Oluşturucu oluşturarak platforma özgü harita özelleştirmeleri uygulamak için uygulanabilecek bir `Map` her platformda. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Tüketen](#Consuming_the_Custom_Map) Xamarin.Forms özel eşleme.
1. [Özelleştirme](#Customizing_the_Map) eşleme için özel Oluşturucu her platformda oluşturarak eşleme.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) başlatılır ve yapılandırılmış önce kullanır. Daha fazla bilgi için bkz. [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Özel oluşturucu kullanılarak bir haritayı özelleştirme hakkında daha fazla bilgi için bkz: [bir harita Raptiyesini özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Özel harita oluşturma

Oluşturma bir öğesinin [ `Map` ](xref:Xamarin.Forms.Maps.Map) ekler sınıfı, bir `ShapeCoordinates` özelliği:

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

`ShapeCoordinates` Özelliği vurgulanmasını bölge tanımlamasına koordinatları koleksiyonunu depolar.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Bu başlatma vurgulanmasını harita bölge tanımlamak için enlem ve boylam koordinatları bir dizi belirtir. Ardından haritanın görünüm ile konumlandırır [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) oluşturarak konumu ve harita yakınlaştırma düzeyini değiştiren yöntemi bir [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) gelen bir [ `Position` ](xref:Xamarin.Forms.Maps.Position) ve [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Haritayı özelleştirme

Çokgen katmana haritaya eklemek için her bir uygulama projesine artık özel Oluşturucu eklenmesi gerekir.

#### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` yöntemi Çokgen katmana eklemek için:

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

Özel oluşturucu, yeni bir Xamarin.Forms öğesine bağlı şartıyla bu yöntem aşağıdaki yapılandırmayı gerçekleştirir:

- `MKMapView.OverlayRenderer` Özelliği için karşılık gelen bir temsilci ayarlayın.
- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.ShapeCoordinates` özelliği ve bir dizi olarak depolanan `CLLocationCoordinate2D` örnekleri.
- Çokgen statik çağırarak oluşturulan `MKPolygon.FromCoordinates` yöntemi enlem ve boylamını her noktasının belirtir.
- Çokgen çağırarak haritaya eklenen `MKMapView.AddOverlay` yöntemi. Bu yöntem, ilk ve son noktayı bağlayan bir çizgi çizerek Çokgen otomatik olarak kapanır.

Ardından, uygulama `GetOverlayRenderer` katmana işlenmesi özelleştirmek için yöntemi:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` ve `OnMapReady` yöntemleri Çokgen katmana eklemek için:

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

`OnElementChanged` Yöntemi enlem ve boylam koordinatları koleksiyonunu alır `CustomMap.ShapeCoordinates` özelliği ve bunları bir üye değişkeni depolar. Ardından `MapView.GetMapAsync` temel alan yöntemi `GoogleMap` , bağlı görünümüne koşuluyla özel Oluşturucu, yeni bir Xamarin.Forms öğesine eklenir. Bir kez `GoogleMap` örneği kullanılabilir `OnMapReady` yöntemi çağrılacak, Çokgen örnekleme tarafından oluşturulduğu bir `PolygonOptions` enlem ve boylamını her noktasının belirten bir nesne. Çokgen çağırarak haritaya daha sonra eklenen `NativeMap.AddPolygon` yöntemi. Bu yöntem, ilk ve son noktayı bağlayan bir çizgi çizerek Çokgen otomatik olarak kapanır.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Evrensel Windows platformu üzerinde özel Oluşturucu Oluşturma

Oluşturma bir öğesinin `MapRenderer` sınıf ve geçersiz kılma kendi `OnElementChanged` yöntemi Çokgen katmana eklemek için:

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

Özel oluşturucu, yeni bir Xamarin.Forms öğesine bağlı şartıyla bu yöntem aşağıdaki işlemleri gerçekleştirir:

- Enlem ve boylam koordinatları koleksiyonunu öğesinden alınan `CustomMap.ShapeCoordinates` özelliği ve dönüştürülen içine bir `List` , `BasicGeoposition` koordinatları.
- Çokgen örneği tarafından oluşturulan bir `MapPolygon` nesne. `MapPolygon` Sınıfı ayarlayarak bir çok noktası şekli haritada görüntülemek için kullanılır, `Path` özelliğini bir `Geopath` şekli koordinatları içeren nesne.
- Çokgen harita üzerinde ekleyerek işlenen `MapControl.MapElements` koleksiyonu. Çokgen otomatik olarak ilk ve son noktayı bağlayan bir çizgi çizerek kapatılacak olduğunu unutmayın.

## <a name="summary"></a>Özet

Bu makalede bir haritaya harita bölgesi vurgulamak için bir Çokgen katmana eklemek açıklanmıştır. Kapalı şekil çokgenler olduğunu ve bunların evin içindekiler doldurmuş olmanız.


## <a name="related-links"></a>İlgili bağlantılar

- [Çokgen harita yer paylaşımı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Bir Harita Raptiyesini Özelleştirme](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
