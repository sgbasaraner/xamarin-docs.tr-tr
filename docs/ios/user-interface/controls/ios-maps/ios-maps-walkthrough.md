---
title: "Ek açıklamalar ve yer paylaşımları"
description: "Bu makalede harita Seti ek açıklama ve katmana özellikleriyle birlikte çalışmak üzere nasıl gösteren bir adım adım kılavuz sunar. Bir ek açıklama ve katmana Xamarin gelişmesi 2013 konferans konumda görüntüleyen bir uygulama için bir harita ekleme gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7eea7231ec4300a368e4612cbed2ba4ebc044a26
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="annotations-and-overlays--walkthrough"></a>Ek açıklamalar ve yer paylaşımları – izlenecek yol

Bu kılavuzda yapı oluşturacağız uygulama aşağıda gösterilmiştir:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Bir örnek MapKit uygulama")](ios-maps-walkthrough-images/00-map-overlay.png)
 
Tamamlanan kodu bulabilirsiniz **MapsWalkthroughComplete** klasör içinde [harita gösterim örneği](https://developer.xamarin.com/samples/monotouch/MapDemo/).

Yeni bir oluşturarak başlayalım **iOS boş proje**ve ilgili bir ad verip. Biz MapView görüntülemek için Görünüm denetleyiciye kodu ekleyerek başlayacaksınız ve yeni sınıflar bizim MapDelegate ve özel ek açıklamalar için sonra oluşturacağız. Bunu başarmak için aşağıdaki adımları izleyin:

## <a name="viewcontroller"></a>ViewController


1. Aşağıdaki ad alanlarına eklemek `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Ekleme bir `MKMapView` ile birlikte değişken sınıfı için örnek bir `MapDelegate` örneği. Oluşturacağız `MapDelegate` kısa süre içinde:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. Denetleyicinin içinde `LoadView` yöntemi Ekle bir `MKMapView` ve ayarlamak `View` denetleyicisinin özelliği:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Ardından, eşlemesinde başlatmak için kod ekleyeceğiz ' ViewDidLoad'' yöntemi.

1. İçinde `ViewDidLoad` eşleme türü ayarlama, kullanıcı konumu gösterir ve yakınlaştırma ve kaydırma izin vermek için kodu ekleyin:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. Ardından, haritayı Ortala ve kendi bölge ayarlamak için kodu ekleyin:

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. Yeni bir örneğini oluşturmak `MapDelegate` ve atayın `Delegate` , `MKMapView`. Yeniden, biz implcodeent gerekir `MapDelegate` kısa süre içinde:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. İOS 8 itibariyle yetkilendirme konumlarını kullanın, böylece bu bizim örnek ekleyelim, kullanıcıdan isteyen. İlk olarak tanımlayan bir `CLLocationManager` sınıf düzeyi değişkeni:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. İçinde `ViewDidLoad` yöntemi, istiyoruz uygulama çalıştıran aygıtları iOS 8 kullanıyor ve bu uygulama kullanılırken biz yetkilendirme isteyecek denetlemek:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Son olarak, düzenlemek ihtiyacımız **Info.plist** dosya konumlarını isteyen nedeni kullanıcılarının öneriyoruz. İçinde **kaynak** menüsünü **Info.plist**, aşağıdaki anahtarını ekleyin:
    
    `NSLocationWhenInUseUsageDescription` 
    
    ve dizesi: 

    `Maps Demo`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – özel ek açıklamalar için bir sınıf


1. Adlı ek açıklama için özel bir sınıf kullanma oluşturacağız `ConferenceAnnotation`. Aşağıdaki sınıf projeye ekleyin:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController - katmana ve ek açıklama ekleme

1. İle `ConferenceAnnotation` yerinde biz bunu eşlemeye ekleyebilirsiniz. Geri `ViewDidLoad` yöntemi `ViewController`, ek açıklamanın haritanın merkezi koordinatta ekleyin:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. Ayrıca otel kaplamasını olmasını istiyoruz. Oluşturmak için aşağıdaki kodu ekleyin `MKPolygon` koordinatları sağlanan otel için kullanarak ve çağrısı tarafından eşlemesine eklemek `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
Bu kodda tamamlar `ViewDidLoad`. Uygulamak ihtiyacımız artık bizim `MapDelegate` ek açıklamanın oluşturma işlemek ve görünümleri sırasıyla yardımcı sınıfı.


## <a name="mapdelegate"></a>MapDelegate

1. Adlı bir sınıf oluşturmak `MapDelegate` devraldığı `MKMapViewDelegate` ve bir `annotationId` eklenti için bir yeniden tanımlayıcı olarak kullanılacak değişkeni:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Yalnızca bir ek açıklama burada yeniden kod kesinlikle gerekli değildir, ancak bunu dahil etmek için iyi bir uygulamadır şekilde sahibiz.

1. Uygulama `GetViewForAnnotation` yöntemi için bir görünüme dönmek için `ConferenceAnnotation` kullanarak **conference.png** görüntü ile bu kılavuzda yer:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. Ek açıklamayı kullanıcı dokunur olduğunda, bu Şehir Ankara'da gösteren bir görüntüyü istiyoruz. Aşağıdaki değişkenleri eklemek `MapDelegate` görüntü ve görüntülemek için görünümü için:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Ardından, ek açıklamanın dokunduğunuz görüntü göstermek için uygulama `DidSelectAnnotation` yöntemini aşağıdaki şekilde:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Resmin harita üzerinde başka bir yerde dokunarak ek açıklama kullanıcı seçimini kaldırır durumundayken gizlemek için uygulama `DidSelectAnnotationView` yöntemini aşağıdaki şekilde:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    Biz artık ek açıklamanın kodunu kullanıyor. Kalan tek şey kodu eklemek için `MapDelegate` otel katmana için görünümü oluşturmak için.

1. Aşağıdaki uygulanması eklemek `GetViewForOverlay` için `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Uygulamayı çalıştırın. Özel ek açıklama ve bir katmana ile etkileşimli bir harita şimdi sahibiz! Ek açıklamayı dokunun ve aşağıda gösterildiği gibi Ankara'da görüntüsü görüntülenir:

 [![](ios-maps-walkthrough-images/01-map-image.png "Ek açıklamayı dokunun ve Ankara'da görüntüsü görüntülenir")](ios-maps-walkthrough-images/01-map-image.png)

## <a name="summary"></a>Özet

Bu makalede belirtilen çokgen için bir katmana ekleme yanı sıra haritaya ek açıklama ekleme inceledik. Biz de dokunma desteği görüntü üzerinde bir harita animasyon için ek açıklama eklemek üzere nasıl gösterilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [Harita Demo (örnek)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [iOS eşlemeleri](~/ios/user-interface/controls/ios-maps/index.md)
