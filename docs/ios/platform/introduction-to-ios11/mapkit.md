---
title: "İOS 11 MapKit yeni özellikler"
ms.topic: article
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 7d21d4ff8960a113cb79582d5cb0849d09f8aaa7
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="new-features-in-mapkit-on-ios-11"></a>İOS 11 MapKit yeni özellikler

iOS 11 MapKit için aşağıdaki yeni özellikleri ekler:

- [Kümeleme ek açıklaması](#clustering)
- [Pusula düğmesi](#compass)
- [Ölçek görünümü](#scale)
- [Kullanıcı izleme düğmesi](#user-tracking)

![Kümelenmiş işaretçileri gösteren harita ve düğmesi pusula](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Yakınlaştırma sırasında otomatik olarak gruplandırma işaretçileri

Örnek [MapKit örnek "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) nasıl Kümelemesi özelliğini yeni iOS 11 ek açıklama uygulandığını gösterir.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Oluşturma bir `MKPointAnnotation` alt sınıfı

Noktası ek açıklama sınıfı harita üzerinde her işaretçisi temsil eder. Kullanarak tek tek eklenebilir `MapView.AddAnnotation()` veya kullanarak bir dizinin `MapView.AddAnnotations()`.

Noktası ek açıklama sınıflarının görsel bir sahip değil ve bunlar yalnızca işaretçisi ile ilişkili verileri temsil etmek için gereklidir (en önemlisi, `Coordinate` enlem ve boylam harita üzerinde olan özelliği) ve herhangi bir özel özellik:

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Oluşturma bir `MKMarkerAnnotationView` tek işaretlerin alt sınıfı

İşaretçi ek açıklama görünümü her ek açıklama visual gösterimidir ve özellikleri gibi kullanılarak biçimlendirilmiş:

- **MarkerTintColor** – işaret rengini.
- **GlyphText** – görüntülenen metin işaretçisi.
- **GlyphImage** – işaret görüntülenir görüntüyü ayarlar.
- **DisplayPriority** – z düzenini (yığın davranış) belirler harita olduğunda işaretçileri olan kalabalık. Aşağıdakilerden birini kullanmak `Required`, `DefaultHigh`, veya `DefaultLow`.

Otomatik kümelemeyi desteklemek için de ayarlamanız gerekir:

- **ClusteringIdentifier** – bu hangi işaretçileri birlikte kümelenmiş denetler. Aynı tanımlayıcı tüm işaretlerini kullanın veya birlikte gruplanır şeklini denetlemek için farklı tanımlayıcılar kullanın.

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

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Oluşturma bir `MKAnnotationView` işaretçileri kümelerini göstermek için

While işaretçileri kümesini temsil eden ek açıklama görünümü _verebilir_ basit görüntü olmalıdır, kullanıcıların kaç işaretçileri birlikte gruplandırılmış hakkında görsel Yardım sağlamak için uygulama beklediğiniz.

[Örnek koduna](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) CoreGraphics işaretçileri her işaret türü oranını halka grafik gösterimi yanı sıra küme sayısı işlemek için kullanır.

Ayrıca ayarlamanız gerekir:

- **DisplayPriority** – z düzenini (yığın davranış) belirler harita olduğunda işaretçileri olan kalabalık. Aşağıdakilerden birini kullanmak `Required`, `DefaultHigh`, veya `DefaultLow`.
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

Ne zaman harita görünümü denetimi oluşturulur ve bir görünüme eklenen, harita içeri ve dışarı uzaklaştırılacağını gibi otomatik kümeleme davranışı etkinleştirmek için ek açıklama görünüm türleri kaydedin:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Harita işlemek!

Harita işlendiğinde ek açıklama işaretçileri kümelenmiş veya yakınlaştırma düzeyine bağlı olarak çizilir. Yakınlaştırma düzeyi değiştikçe işaretçileri kümeleri ve bu moddan animasyon.

![Simulator haritada kümelenmiş işaretçilerini gösterme](mapkit-images/cyclemap-sml.png)

Başvurmak [eşlemeleri bölüm](~/ios/user-interface/controls/ios-maps/index.md) MapKit verilerle görüntüleme hakkında daha fazla bilgi.

<a name="compass" />

## <a name="compass-button"></a>Pusula düğmesi

iOS 11 harita dışında pusula açılır ve başka bir yere görünümü işlemek için yeteneği ekler. Bkz: [Tandm örnek uygulaması](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) bir örnek.

(Harita yönlendirmesini değiştiğinde dinamik animasyon dahil), bir pusula gibi görünen bir düğme oluşturmak ve başka bir denetimde işler.

![Gezinti çubuğunda pusula düğmesi](mapkit-images/compass-sml.png)

Aşağıdaki kodu pusula düğmesi oluşturur ve gezinti çubuğunda işler:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

`ShowsCompass` Özelliği, varsayılan pusula harita görünümünü içinde görünürlüğünü denetlemek için kullanılabilir.

<a name="scale" />

## <a name="scale-view"></a>Ölçek görünümü

Başka bir yerde görünümü kullanarak ölçek eklemek `MKScaleView.FromMapView()` görünüm hiyerarşideki başka bir yerde eklemek için ölçek görünümünde örneğini almak için yöntemi.

![Haritada yayılan ölçek görünümü](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

`ShowsScale` Özelliği, varsayılan pusula harita görünümünü içinde görünürlüğünü denetlemek için kullanılabilir.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Kullanıcı izleme düğmesi

Kullanıcı izleme düğmesi, kullanıcının geçerli konumuna haritada toplanır. Kullanım `MKUserTrackingButton.FromMapView()` düğmesinin bir örneği elde, biçimlendirme değişiklikleri uygulamak ve başka bir yerde görünüm hiyerarşisinde eklemek için yöntem.

![Haritada yayılan kullanıcı konumu düğmesi](mapkit-images/user-location-sml.png)

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
