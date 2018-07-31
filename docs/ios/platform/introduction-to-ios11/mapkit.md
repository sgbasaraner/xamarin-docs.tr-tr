---
title: İOS 11 mapkit'teki yeni özellikler
description: "Bu belge iOS 11'deki yeni MapKit özellikleri açıklar: gruplandırma işaretçileri, pusula düğmesi, Ölçek görünümü ve kullanıcı izleme düğmesi."
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: c060a7bbc8d5968aeaca5f84743cdf22513dfbec
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350592"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>İOS 11 mapkit'teki yeni özellikler

iOS 11 MapKit için aşağıdaki yeni özellikleri ekler:

- [Ek açıklama kümeleme](#clustering)
- [Pusula düğmesi](#compass)
- [Ölçek görüntüle](#scale)
- [Kullanıcı izleme düğmesi](#user-tracking)

![Kümelenmiş işaretçileri gösteren harita ve düğme compass](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Yakınlaştırma sırasında otomatik olarak gruplandırma işaretçileri

Örnek [MapKit örnek "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) nasıl Kümelemesi özelliğini yeni iOS 11 ek açıklama uygulanacağı gösterilmektedir.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Oluşturma bir `MKPointAnnotation` öğesinin alt sınıfı

Nokta ek açıklama sınıfı eşlemedeki her işaretçisi temsil eder. Kullanarak tek tek eklenebilir `MapView.AddAnnotation()` veya kullanarak bir dizi `MapView.AddAnnotations()`.

Noktası ek açıklama sınıflar görsel gösterimi yoktur, bunlar yalnızca işaretleyiciyle ilişkili verileri temsil etmek için gereklidir (en önemlisi de `Coordinate` özelliğinin, enlem ve boylam harita üzerinde) ve herhangi bir özel özellikler:

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Oluşturma bir `MKMarkerAnnotationView` tek işaretçileri için alt sınıfı

İşaret ek açıklama görünüm her ek açıklama görsel temsilini olduğu ve özellikleri gibi kullanılarak stil uygulanmış:

- **MarkerTintColor** – işaret rengi.
- **GlyphText** – metin işaretçisi görüntülenir.
- **GlyphImage** – işaretçisi gösterilen görüntüyü ayarlar.
- **DisplayPriority** – (yığın davranış) z düzenini belirler harita olduğunda işaretçileriyle kalabalık. Birini `Required`, `DefaultHigh`, veya `DefaultLow`.

Otomatik küme desteklemek için de ayarlamanız gerekir:

- **ClusteringIdentifier** – bu hangi işaretçileri birlikte kümelenmiş denetler. Tüm işaretleri için aynı tanımlayıcıyı kullanın veya birlikte gruplanır denetlenmesine farklı tanımlayıcılar kullanın.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Oluşturma bir `MKAnnotationView` işaretçilerinin kümeleri temsil etmek için

While bir kümesine işaretlerinin temsil eden ek açıklama görünümü _verebilir_ basit bir görüntü olması, kullanıcıların kaç işaretçileri birlikte gruplanmış hakkında görsel ipuçlarını sağlama uygulamanın beklediğiniz.

[Örnek kod](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) CoreGraphics her işaret türü oranını halka grafik gösterimi yanı sıra küme sayısı işlemek için kullanır.

Ayrıca ayarlamanız gerekir:

- **DisplayPriority** – (yığın davranış) z düzenini belirler harita olduğunda işaretçileriyle kalabalık. Birini `Required`, `DefaultHigh`, veya `DefaultLow`.
- **CollisionMode** – `Circle` veya `Rectangle`.

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. Kayıt görünümü sınıfları

Ne zaman harita görünümü denetimi oluşturulur ve bir görünüm eklendi, eşleme giriş ve çıkış uzaklaştırılacağını gibi otomatik küme davranışı etkinleştirmek için ek açıklama görünüm türleri kaydedin:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Harita işleme!

Harita işlendiğinde ek açıklama işaretlerini kümelenmiş veya yakınlaştırma düzeyine bağlı olarak çizilir. Yakınlaştırma düzeyi değiştikçe işaretçileri kümeleri içine ve dışına animasyon.

![Simülatör kümelenmiş işaretçilerini haritada gösterme](mapkit-images/cyclemap-sml.png)

Başvurmak [eşler bölüm](~/ios/user-interface/controls/ios-maps/index.md) MapKit ile verileri görüntüleme hakkında daha fazla bilgi.

<a name="compass" />

## <a name="compass-button"></a>Pusula düğmesi

iOS 11 compass harita dışında açılır ve başka bir yere görünüm işlemek için özelliği ekler. Bkz: [Tandm örnek uygulaması](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) örneği.

(Harita yönlendirme değiştiğinde Canlı animasyon dahil), bir compass gibi görünen bir düğme oluşturmak ve başka bir denetimde işler.

![Gezinti çubuğunda pusula düğmesi](mapkit-images/compass-sml.png)

Aşağıdaki kod, pusula düğmesi oluşturur ve gezinti çubuğunda işler:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` Özellik eşleme görünümü içinde varsayılan compass görünürlüğünü denetleme için kullanılabilir.

<a name="scale" />

## <a name="scale-view"></a>Ölçek görüntüle

Başka bir yerde görünümü kullanarak ölçek eklemek `MKScaleView.FromMapView()` görünümü hiyerarşideki başka bir yerde eklemek için ölçek görünümü örneğini almak için yöntemi.

![Bir haritada yayılan ölçek görüntüle](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` Özellik eşleme görünümü içinde varsayılan compass görünürlüğünü denetleme için kullanılabilir.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Kullanıcı izleme düğmesi

Kullanıcı izleme düğmesi, kullanıcının geçerli konumuna haritada ortalar. Kullanım `MKUserTrackingButton.FromMapView()` görünümü hiyerarşideki başka bir yere ekleyin düğmeyi örneğini almak ve biçimlendirme değişiklikleri uygulamak için yöntemi.

![Bir haritada yayılan kullanıcı konumu düğmesi](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>İlgili bağlantılar

- [MapKit örnek 'Tandm'](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Yeni içinde MapKit (WWDC) (video) nedir](https://developer.apple.com/videos/play/wwdc2017/237/)
