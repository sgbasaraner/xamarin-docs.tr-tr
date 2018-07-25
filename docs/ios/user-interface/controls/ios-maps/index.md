---
title: Xamarin.iOS eşlemelerinde
description: Bu belge iOS MapKit çerçevesi ve Xamarin.iOS ile nasıl kullanıldığını açıklar. Bu, bir harita, kaydırma ve yakınlaştırma, kullanıcı konumu ile çalışma, ek açıklama ekleyin, belirtme ve katmanları ile çalışmak ve yerel arama gerçekleştirmek stil eklemek nasıl ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 5343f53b77319b08424263103834ffcf10e261a0
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242072"
---
# <a name="maps-in-xamarinios"></a>Xamarin.iOS eşlemelerinde

Haritalar, tüm modern mobil işletim sistemlerindeki genel bir özelliğidir. iOS Map Kit framework aracılığıyla eşleme desteği sunar. Map Kit ile uygulamaları zengin, etkileşimli haritalar bir kolayca ekleyebilirsiniz. Bu haritalar bir harita üzerinde konumları işaretlemek için ek açıklamaları ekleme ve grafik rastgele şekilleri planlamanızda gibi yollarla çeşitli özelleştirilebilir. Map Kit, hatta bir cihazın geçerli konumunu göstermek için yerleşik destek sunmaktadır.

## <a name="adding-a-map"></a>Bir eşleme ekleniyor

Bir uygulamaya bir harita eklemek gerçekleştirilir ekleyerek bir `MKMapView` aşağıda gösterildiği gibi hiyerarşisini Görüntüle için örnek:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` olan bir `UIView` öğesinin alt sınıfı bir harita görüntülenir. Yukarıdaki kodu kullanarak haritada yalnızca ekleme, etkileşimli bir harita oluşturur:

 ![](images/00-map.png "Örnek eşleme")

## <a name="map-style"></a>Harita stili

 `MKMapView` Haritalar 3 farklı türlerde destekler. Harita stil uygulamak için ayarlamanız yeterlidir `MapType` arasında bir değer özelliğini `MKMapType` sabit listesi:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Aşağıdaki ekran görüntüsünde, mevcut olan farklı eşleme stilleri gösterir:

 ![](images/01-mapstyles.png "Bu ekran göster kullanılabilen farklı eşleme stilleri")

## <a name="panning-and-zooming"></a>Kaydırma ve yakınlaştırma

 `MKMapView` Harita etkileşim özellikleri için destek gibi içerir:

-  Tabletinizde hareket yakınlaştırma
-  Bir kaydırma hareketi kaydırma


Bu özellikler etkinleştirilebilir ya da yalnızca ayarlayarak devre dışı `ZoomEnabled` ve `ScrollEnabled` özelliklerini `MKMapView` örneği, varsayılan değer olduğu için her ikisi de true. Örneğin, statik bir haritaya görüntülemek için uygun özellikleri false olarak ayarlamanız yeterlidir:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Kullanıcı konumu

Kullanıcı etkileşimi yanı sıra `MKMapView` cihazın konumunu görüntüleyen için yerleşik destek de vardır. Bunu kullanarak yapar *çekirdek konumu* framework. Kullanıcının konumuna erişebilmek için önce kullanıcıdan gerekir. Bunu yapmak için bir örneğini oluşturmak `CLLocationManager` ve çağrı `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

İOS 8.0, arama girişimi önce sürümlerinde unutmayın `RequestWhenInUseAuthorization` bir hataya neden olur. 8'den önceki sürümleri desteklemek istiyorsanız, bu çağrı yapmadan önce iOS sürümünü kontrol ettiğinizden emin olun.

Kullanıcının konumuna erişimi de değişiklikler gerektirir **Info.plist**. Konum verileri ile ilgili aşağıdaki anahtarları ayarlamanız gerekir:

- **NSLocationWhenInUseUsageDescription** - uygulamanızla etkileşim sırasında kullanıcının konumuna erişmek için.
- **NSLocationAlwaysUsageDescription** - ne zaman uygulamanızın arka planda kullanıcının konumuna erişir.

Bu anahtarları açarak ekleyebilirsiniz **Info.plist** seçerek *kaynak* düzenleyicisinin alt.

Güncelledikten sonra **Info.plist** ve konumlarına erişim izni için kullanıcıdan, ayarlayarak kullanıcının konumunu haritada gösterebilirsiniz `ShowsUserLocation` özelliği true:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "İzin konuma erişim Uyarısı")
 
## <a name="annotations"></a>Ek Açıklamalar

 `MKMapView` Ayrıca bir harita üzerindeki ek açıklama olarak bilinen görüntüleme görüntüleri destekler. Bu, özel görüntüler veya sistem tanımlı PIN çeşitli renklerinin olabilir. Örneğin, aşağıdaki ekran görüntüsünde iki bir PIN ile bir harita gösterir ve özel bir görüntü:

 ![](images/03-annotations.png "Bir harita hem bir PIN ile bu ekran görüntüsünde gösterilir ve özel bir görüntü")

### <a name="adding-an-annotation"></a>Bir ek açıklama ekleme

Bir eklenti iki bölümden oluşur:

-  `MKAnnotation` Hakkında ek açıklama başlık ve açıklamanın konumu gibi model verileri içeren bir nesne.
-  `MKAnnotationView` , Görüntü ve isteğe bağlı olarak, ek açıklama kullanıcı dokunduğunda gösterilen belirtme çizgisi görüntüsünü içerir.


Map Kit kullanan iOS temsilci düzeni bir eşleme için ek açıklamaları eklemek için burada `Delegate` özelliği `MKMapView` örneğine ayarlanmış bir `MKMapViewDelegate`. Bu temsilcinin uygulamasında, döndürmekten sorumlu olduğu `MKAnnotationView` bir ek açıklama için.

Bir ek açıklama eklemek için önce ek açıklama çağrılarak eklenir `AddAnnotations` üzerinde `MKMapView` örneği:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Ek açıklama konumunu bir haritada görünür olduğunda `MKMapView` kendi temsilcinin çağıracak `GetViewForAnnotation` almak için yöntemi `MKAnnotationView` görüntülenecek.

Örneğin, aşağıdaki kod bir sistem tarafından sağlanan döndürür `MKPinAnnotationView`:

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>Ek açıklamalar yeniden kullanma

Bellek, tasarruf etmek için `MKMapView` ek açıklama görünümü kullanıcının yeniden kullanmak, benzer şekilde tablo hücreleri yeniden havuzda toplanmasını sağlar. Havuzdan bir ek açıklama görünümü elde çağrısıyla yapılır `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Belirtme çizgisi gösteriliyor

Daha önce bahsedildiği gibi bir ek açıklama belirtme çizgisi isteğe bağlı olarak gösterebilirsiniz. Belirtme çizgisi yalnızca gösterecek şekilde ayarlayın `CanShowCallout` üzerinde true `MKAnnotationView`. Bu ek açıklama dokunulduğunda, gösterildiği gibi görüntülenmesini açıklamanın başlığına sonuçlanır:

 ![](images/04-callout.png "Ek açıklamaların görüntülenmesini başlık")

### <a name="customizing-the-callout"></a>Belirtme çizgisi özelleştirme

Belirtme çizgisi da sol ve sağ aksesuar görünümleri göstermek için aşağıda gösterildiği gibi özelleştirilebilir:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Bu kod aşağıdaki çağrı sonuçları:

 ![](images/05-callout-accessories.png "Bir örnek belirtme")

Kullanıcının doğru Donatı dokunarak işlemek için yalnızca uygulama `CalloutAccessoryControlTapped` yönteminde `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Yer paylaşımları

Başka bir şekilde katman grafik bir haritada yer paylaşımları kullanıyor. Katmanları ile eşleme, yakınlaştırılmış olarak ölçeklendirilen çizim grafik içeriği destekler. iOS katmanları dahil olmak üzere, çeşitli türleri için destek sağlar:

-  Çokgen - bazı bir haritadaki bölgeyi vurgulama için yaygın olarak kullanılan.
-  Kullansa - genellikle bir yol gösteren sırasında görülen.
-  Daireler - bir eşleme dairesel bir alanı vurgulamak için kullanılır.


Ayrıca, ayrıntılı, özelleştirilmiş çizim kodu ile rastgele geometrileri göstermek için özel yer paylaşımları oluşturulabilir. Örneğin, hava durumu radar özel bir katmana için iyi bir aday olabilir.

#### <a name="adding-an-overlay"></a>Bir katmana ekleme

Benzer şekilde ek açıklamaları, bir katman eklenmesini 2 bölümleri kapsar:

-  Katman için bir model nesnesi oluşturma ve ekleme `MKMapView` .
-  İçinde katmana için bir görünüm oluşturma `MKMapViewDelegate` .


Katman modeli herhangi biri `MKShape` öğesinin alt sınıfı. Xamarin.iOS içerir `MKShape` çokgenler, çoklu çizgilere ve daireler, alt sınıfların aracılığıyla `MKPolygon`, `MKPolyline` ve `MKCircle` sırasıyla sınıfları.

Örneğin, aşağıdaki kod eklemek için kullanılan bir `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Görünüm için bir katmana bir `MKOverlayView` tarafından döndürülen örnek `GetViewForOverlay` içinde `MKMapViewDelegate`. Her `MKShape` karşılık gelen `MKOverlayView` verilen şeklin görüntülemek nasıl olduğunu bilir. İçin `MKPolygon` yoktur `MKPolygonView`. Benzer şekilde, `MKPolyline` karşılık gelen `MKPolylineView`ve `MKCircle` yoktur `MKCircleView`.

Örneğin, aşağıdaki kodu bir `MKCircleView` için bir `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Bu bir daire harita üzerinde gösterildiği gibi görüntülenir:

 ![](images/06-circle-overlay.png "Haritada görüntülenen bir daire")

## <a name="local-search"></a>Yerel arama

iOS yerel arama API'si ile zaman uyumsuz aramaları belirli bir coğrafi bölgede ilgi noktası sağlayan harita Seti içerir.

Yerel arama gerçekleştirmek için bir uygulama adımları izlemelisiniz:

1.  Oluşturma `MKLocalSearchRequest` nesne.
1.  Oluşturma bir `MKLocalSearch` nesnesinden `MKLocalSearchRequest` .
1.  Çağrı `Start` metodunda `MKLocalSearch` nesne.
1.  Alma `MKLocalSearchResponse` bir geri çağırma nesnesi.


API'nin yerel arama kullanıcı arabirimi sağlar. Kullanılacak bir harita bile gerek yoktur. Ancak, yerel arama pratik kullanılmasını sağlamak için bir arama sorgusu belirtin ve sonuçları görüntülemek için bazı yol sağlamak üzere bir uygulama gerekir. Konum verileri sonuçlarını içerecek olduğundan, buna ek olarak, bu genellikle bir haritada gösterilecek anlamlı olacaktır.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Yerel bir arama kullanıcı Arabirimi ekleme

Arama giriş kabul etmek için başka bir yöntem bir `UISearchBar`, tarafından sağlanan bir `UISearchController` ve sonuçları bir tabloda görüntülenir.

Aşağıdaki kodu ekler `UISearchController` (bir arama çubuğu özelliğe sahip) içinde `ViewDidLoad` yöntemi `MapViewController`:

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;
            
//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller 
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });


    }
}
```

### <a name="updating-the-search-results"></a>Arama sonuçları güncelleştiriliyor

`SearchResultsUpdater` İşletme arasında bir aracı gibi davranır `searchController`ait arama çubuğu ve arama sonuçları. 

Bu örnekte biz öncelikle arama yöntemi oluşturmanız gerekir `SearchResultsViewController`. Gerekir oluştururuz bunu yapmanın bir `MKLocalSearch` nesne ve için bir arama göndermek için bir `MKLocalSearchRequest`, geçirilen geri çağırma sonuçları alınır `Start` yöntemi `MKLocalSearch` nesne. Sonuç olarak döndürülür bir `MKLocalSearchResponse` oluşan bir diziyi içeren bir nesne `MKMapItem` nesneler:

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });


}
```

Ardından bizim `MapViewController` özel uygulanışı oluşturacağız `UISearchResultsUpdating`, için atanan `SearchResultsUpdater` özelliği bizim `searchController` içinde [yerel arama kullanıcı Arabirimi ekleme](#Adding_a_Local_Search_UI) bölümü:

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

Yukarıdaki uygulama sonuçlardan bir öğe seçildiğinde bir ek açıklama aşağıda gösterildiği gibi haritaya ekler:

 ![](images/08-search-results.png "Sonuçlardan bir öğenin seçili olduğunda haritaya eklenen bir ek açıklama")
 
> [!IMPORTANT]
> `UISearchController` iOS 8'de kullanılmıştır. Bu'den önceki cihazlar desteklemek istediğiniz sonra kullanmanız gerekecektir `UISearchDisplayController`.



## <a name="summary"></a>Özet

Bu makalede incelenir *harita* *Seti* framework iOS için. İlk olarak ne görünüyordu `MKMapView` sınıfı, bir uygulamada eklenecek etkileşimli haritalar olanak tanır. Ardından ek açıklamalar ve yer paylaşımları kullanarak haritalar daha fazla özelleştirmek nasıl gösterilmiştir. Son olarak, iOS 6.1, harita pakete eklenmiş olan yerel arama özellikleri incelenirken nasıl kullanılacağını gösteren ilgi noktası için konum tabanlı sorgular gerçekleştirmek ve bunları haritaya ekleyebilirsiniz.

## <a name="related-links"></a>İlgili bağlantılar

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (örnek)](https://developer.xamarin.com/samples/monotouch/MapDemo)
