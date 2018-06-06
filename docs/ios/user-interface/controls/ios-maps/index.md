---
title: Xamarin.iOS eşlemeleri
description: Bu belgede iOS MapKit framework ve Xamarin.iOS ile nasıl kullanıldığı açıklanmaktadır. Bir harita, kaydırma ve yakınlaştırma, kullanıcı konumu ile çalışma, ek açıklama ekleyin, belirtme ve yer paylaşımları ile çalışır ve yerel arama yapmak stili ekleme açıklanır.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3649c8eb9c8c1a82940b8e2eece7d2bfd005d024
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790236"
---
# <a name="maps-in-xamarinios"></a>Xamarin.iOS eşlemeleri

MAPS, modern mobil işletim sistemlerinin tümünde ortak bir özelliktir. iOS harita Seti çerçevesi aracılığıyla yerel olarak eşleme destek sunar. Harita Seti ile uygulamaları kolayca zengin ve etkileşimli eşlemeleri ekleyebilirsiniz. Bu eşlemeler, bir harita konumları işaretlemek için ek açıklamaları ekleme ve rastgele şekil grafik üst üste getirme gibi yollarla, çeşitli özelleştirilebilir. Harita Seti bile bir aygıt geçerli konumunu göstermek için yerleşik desteğe sahiptir.

## <a name="adding-a-map"></a>Bir harita ekleme

Uygulamaya bir harita ekleme gerçekleştirilir ekleyerek bir `MKMapView` aşağıda gösterildiği gibi görünüm hiyerarşisine örneği:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` olan bir `UIView` bir harita görüntüleyen bir alt kümesi. Yalnızca yukarıdaki kodu kullanarak harita ekleme etkileşimli bir harita oluşturur:

 ![](images/00-map.png "Bir örnek eşleme")

## <a name="map-style"></a>Harita stili

 `MKMapView` eşlemelerinin 3 farklı stillerini destekler. Bir harita stili uygulamak için basitçe ayarlamak `MapType` arasında bir değer özelliğine `MKMapType` numaralandırma:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Aşağıdaki ekran görüntüsünde kullanılabilir farklı harita Stilleri Göster:

 ![](images/01-mapstyles.png "Bu ekran kullanılabilir farklı harita Stilleri Göster")

## <a name="panning-and-zooming"></a>Kaydırma ve yakınlaştırma

 `MKMapView` Harita etkileşim özellikleri için destek gibi içerir:

-  Tutarak hareketi yakınlaştırma
-  Pan hareketi kaydırma


Bu özellikler etkin ya da yalnızca ayarlayarak devre dışı `ZoomEnabled` ve `ScrollEnabled` özelliklerini `MKMapView` örneği, varsayılan değeri olduğu için her ikisi de true. Örneğin, statik haritaya görüntülemek için yalnızca uygun özellikleri false olarak ayarlayın:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Kullanıcı konumu

Kullanıcı etkileşimi yanı sıra `MKMapView` cihazın konumunu görüntüleme için yerleşik destek de vardır. Bunu kullanarak yapar *çekirdek konumu* framework. Kullanıcının konumuna erişebilmesi için kullanıcıya sor gerekir. Bunu yapmak için bir örneğini oluşturmak `CLLocationManager` ve arama `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

İOS 8.0, çağrı girişimi önce sürümlerinde unutmayın `RequestWhenInUseAuthorization` bir hatayla sonuçlanır. İOS sürüm 8'den önceki sürümleri desteklemek istiyorsanız, bu çağrıyı yapmadan önce kontrol ettiğinizden emin olun.

Kullanıcının konumuna erişim aynı zamanda yapılan değişiklikler gerektirir **Info.plist**. Konum verileri ile ilgili aşağıdaki anahtarları ayarlamanız gerekir:

- **NSLocationWhenInUseUsageDescription** - uygulamanızı ile etkileşim sırasında zaman kullanıcının konuma erişmek için.
- **NSLocationAlwaysUsageDescription** - ne zaman, uygulamanızın arka planda kullanıcının konumuna erişir.

Bu anahtarlar açarak ekleyebilirsiniz **Info.plist** ve seçerek *kaynak* düzenleyicisinin alt.

Güncelleştirdikten sonra **Info.plist** ve kullanıcıdan konumlarına erişmek izin istenir, ayarlayarak kullanıcının konumu haritada gösterebilir `ShowsUserLocation` özelliği true olarak:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "İzin ver konumu erişim Uyarısı")
 
## <a name="annotations"></a>Ek Açıklamalar

 `MKMapView` Ayrıca bir harita ek açıklamaları olarak bilinen görüntüleme görüntüleri destekler. Bunlar, özel resimler veya sistem tanımlı PIN'ler çeşitli renklerin olabilir. Örneğin, aşağıdaki ekran görüntüsü bir harita hem bir PIN ile gösterir ve özel bir görüntü:

 ![](images/03-annotations.png "Bu ekran görüntüsü, hem bir PIN ile bir eşleme gösterir ve özel bir görüntü")

### <a name="adding-an-annotation"></a>Ek açıklama ekleme

Bir eklenti iki bölümden oluşur:

-  `MKAnnotation` Başlık ve ek açıklamanın konumu gibi ek açıklamanın hakkında model verileri içeren nesne.
-  `MKAnnotationView` , Görüntünün görüntü ve isteğe bağlı olarak kullanıcı ek açıklamanın ne zaman dokunur gösterilen bir belirtme çizgisi içerir.


Harita Seti haritaya, ek açıklamaları eklemek için iOS temsilci deseni kullanır burada `Delegate` özelliği `MKMapView` örneğine ayarlanmış bir `MKMapViewDelegate`. Sorumlu döndürmek için bu temsilcinin uygulaması olan `MKAnnotationView` bir eklenti için.

Ek açıklama eklemek için önce ek açıklamanın çağrılarak eklenir `AddAnnotations` üzerinde `MKMapView` örneği:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Ek açıklamanın konumu harita üzerinde görünür hale geldiğinde `MKMapView` kendi temsilcinin çağıracak `GetViewForAnnotation` almak için yöntemi `MKAnnotationView` görüntülemek için.

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

Bellek, tasarruf etmek için `MKMapView` ek açıklama görünüm kullanıcının yeniden kullanmak üzere, benzer şekilde tablo hücreleri yeniden havuza için sağlar. Bir ek açıklama görünümü havuzdan alma çağrısıyla yapılır `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Belirtme çizgisi gösterme

Daha önce belirtildiği gibi ek açıklama belirtme çizgisi isteğe bağlı olarak gösterebilir. Belirtme çizgisi yalnızca göstermek için ayarlayın `CanShowCallout` true üzerinde `MKAnnotationView`. Ek açıklamanın dokunduğunuz, gösterildiği gibi görüntülenmesini açıklamanın başlığında sonuçları:

 ![](images/04-callout.png "Ek açıklamalar görüntülenmesini başlık")

### <a name="customizing-the-callout"></a>Belirtme çizgisi özelleştirme

Belirtme çizgisi sol ve sağ aksesuar görünümleri göstermek için aşağıda gösterildiği gibi özelleştirilebilir:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Bu kod, aşağıdaki çağrı sonuçları:

 ![](images/05-callout-accessories.png "Bir örnek belirtme")

Sağ donatıyı dokunarak kullanıcı işlemek için yalnızca uygulama `CalloutAccessoryControlTapped` yönteminde `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Yer paylaşımları

Bir harita katmanı grafik başka bir şekilde yer paylaşımları kullanıyor. Yer paylaşımları Haritası uzaklaştırılacağını gibi ölçeklendirilebilen çizim grafik içeriği destekler. iOS, yer paylaşımları dahil olmak üzere, çeşitli türleri için destek sağlar:

-  Çokgenler - harita üzerinde bazı bölge vurgulamak için yaygın olarak kullanılır.
-  Kullansa - genellikle bir rota gösterilirken görülür.
-  Daire - bir eşleme döngüsel bir alanını vurgulamak için kullanılır.


Ayrıca, özel yer paylaşımları ayrıntılı, özelleştirilmiş çizim koduyla rasgele geometri göstermek için oluşturulabilir. Örneğin, hava durumu radar özel bir katmana için iyi bir aday olur.

#### <a name="adding-an-overlay"></a>Bir katmana ekleme

Ek açıklamalar için benzer bir katmana ekleme 2 bölümleri içerir:

-  Katman için bir model nesnesi oluşturma ve eklemeyi `MKMapView` .
-  Katmanda için Görünüm oluşturma `MKMapViewDelegate` .


Katman modeli herhangi olabilir `MKShape` bir alt kümesi. Xamarin.iOS içeren `MKShape` çokgenler, kullansa ve daireler, alt sınıflarının aracılığıyla `MKPolygon`, `MKPolyline` ve `MKCircle` sırasıyla sınıfları.

Örneğin, aşağıdaki kodu eklemek için kullanılan bir `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Bir katmana görünümü bir `MKOverlayView` tarafından döndürülen örnek `GetViewForOverlay` içinde `MKMapViewDelegate`. Her `MKShape` karşılık gelen `MKOverlayView` verilen şeklin görüntülemek nasıl bilir. İçin `MKPolygon` yok `MKPolygonView`. Benzer şekilde, `MKPolyline` karşılık gelen `MKPolylineView`ve `MKCircle` yok `MKCircleView`.

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

 ![](images/06-circle-overlay.png "Haritada görüntülenmelerini bir daire")

## <a name="local-search"></a>Yerel arama

iOS API belirtilen bir coğrafi bölgede ilgilenilen noktaları için zaman uyumsuz aramaları sağlayan harita Seti ile yerel arama içerir.

Bir uygulama, bir yerel arama yapmak için şu adımları izlemelisiniz:

1.  Oluşturma `MKLocalSearchRequest` nesnesi.
1.  Oluşturma bir `MKLocalSearch` nesnesinin `MKLocalSearchRequest` .
1.  Çağrı `Start` yöntemi `MKLocalSearch` nesnesi.
1.  Alma `MKLocalSearchResponse` bir geri çağırma nesnesi.


Yerel arama API kendisini hiçbir kullanıcı arabirimi sağlar. Hatta kullanılması için bir harita olmasını gerektirmez. Ancak, yerel arama pratik kullanılmasını sağlamak için bir uygulama bir arama sorgusu belirtin ve sonuçları görüntülemek için bazı yol sağlaması gerekir. Sonuçları konum verileri içerecek olduğundan, ayrıca, bu genellikle bir haritada göstermek için anlamlı olacaktır.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Yerel arama kullanıcı Arabirimi ekleme

Arama girişi kabul etmek için bir yoldur ile bir `UISearchBar`, tarafından sağlanan bir `UISearchController` ve sonuçları bir tabloda görüntülenir.

Aşağıdaki kod ekler `UISearchController` (olan bir arama çubuğu özelliği) içinde `ViewDidLoad` yöntemi `MapViewController`:

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

### <a name="updating-the-search-results"></a>Arama sonuçlarını güncelleştiriliyor

`SearchResultsUpdater` İşletme arasında bir aracı gibi davranır `searchController`ait arama çubuğu ve arama sonuçları. 

Bu örnekte, biz ilk arama yönteminde oluşturmak zorunda `SearchResultsViewController`. Biz oluşturmalısınız bunun için bir `MKLocalSearch` nesne ve arama çıkarmak için kullan bir `MKLocalSearchRequest`, sonuçları geçirilen bir geri çağırma alınır `Start` yöntemi `MKLocalSearch` nesnesi. Sonuçları sonra döndürülür bir `MKLocalSearchResponse` içeren bir dizi nesnesi `MKMapItem` nesneler:

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

Ardından bizim `MapViewController` , özel bir uygulama oluşturacağız `UISearchResultsUpdating`, için atanan `SearchResultsUpdater` özelliği bizim `searchController` içinde [yerel arama kullanıcı Arabirimi ekleme](#Adding_a_Local_Search_UI) bölümü:

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

Yukarıdaki uygulama sonuçlarından bir öğe seçildiğinde açıklamanın aşağıda gösterildiği gibi eşlemesine ekler:

 ![](images/08-search-results.png "Sonuçları bir öğe seçildiğinde eşlemesine eklenen ek açıklama")
 
> [!IMPORTANT]
> `UISearchController` iOS 8 uygulanmıştır. Cihazları öncesindeki bu desteklemek istediğiniz sonra kullanmanız gerekecektir `UISearchDisplayController`.



## <a name="summary"></a>Özet

Bu makalede incelenmesi *harita* *Seti* framework iOS için. İlk olarak, ne Aranan `MKMapView` sınıfı, bir uygulamada dahil edilecek etkileşimli eşlemeleri olanak tanır. Ardından ek açıklamalar ve yer paylaşımları kullanarak eşlemeleri daha fazla özelleştirmek nasıl gösterilmektedir. Son olarak, 6.1, iOS ile harita pakete eklenen yerel arama yetenekleri incelenmesi nasıl kullanılacağını gösteren ilgilenilen noktaları için konum tabanlı sorgular gerçekleştirmek ve bunları bir harita Ekle.

## <a name="related-links"></a>İlgili bağlantılar

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (örnek)](https://developer.xamarin.com/samples/monotouch/MapDemo)
