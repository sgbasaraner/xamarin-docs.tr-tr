---
title: TvOS Xamarin tablo görünümleri ile çalışma
description: Bu makalede tasarlama ve tablo görünümleri ve Tablo görünümü denetleyicileri Xamarin.tvOS uygulama içinde çalışma kapsar.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8c74c2cc7598f50e57a6a450823e2b0ebca4b537
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789573"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>TvOS Xamarin tablo görünümleri ile çalışma

_Bu makalede tasarlama ve tablo görünümleri ve Tablo görünümü denetleyicileri Xamarin.tvOS uygulama içinde çalışma kapsar._

TvOS içinde bir tablo görünümü, isteğe bağlı olarak grupları veya bölümler düzenlenebilir satırları kaydırmanın tek bir sütun olarak sunulur. Büyük miktarda veri, kullanıcıya açık şekilde anlamak için verimli bir şekilde görüntülemek gerektiğinde tablosu görünümleri kullanılmalıdır.

Tablo görünümleri genellikle sütunlardan biri görüntülenen bir [bölünmüş görünümlü](~/ios/tvos/user-interface/split-views.md) ters tarafta görüntülenen seçili öğenin ayrıntılarını ile gezinti olarak:

[![](table-views-images/intro01.png "Örnek tablo görünümü")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>Tablo görünümleri hakkında

A `UITableView` isteğe bağlı olarak grupları veya bölümler düzenlenebilir bilgilerin hiyerarşik listesi olarak kaydırılabilir satır tek bir sütun görüntüler: 

[![](table-views-images/table01.png "Seçilen bir öğeyi")](table-views-images/table01.png#lightbox)

Apple tabloları ile çalışmak için aşağıdaki önerileri vardır:

- **Uyumlu genişliğinin olması** -, tablo genişlikleri doğru denge deneyin. Tablo çok genişse, bir mesafe tarama zor olabilir ve kullanılabilir içerik alanının çıktığınızda alabilir. Tablo çok dar ise kesilecek bilgileri neden olabilir veya kaydırma, yeniden bu arasında yer okuma kullanıcıya zor olabilir.
- **Tablo içeriği hızla Göster** - büyük listeler veri için içerik yavaş yük ve tablo kullanıcıya sunulan hemen bilgi gösteren başlatın. Tablo yüklemek için çok uzun sürerse, kullanıcı uygulamanızı veya kilitli yukarı düşünme ilgi kaybedebilirsiniz.
- **Kullanıcı, uzun içerik yükleri bildirmek** - uzun tablo yükleme süresi varsa kaçınılmaz, mevcut bir [ilerleme çubuğu veya etkinliği göstergesini](~/ios/tvos/user-interface/progress-indicators.md) uygulama bilmesi kilitlenmiş kurmadı.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Tablo görünümü hücre türleri

A `UITableViewCell` Tablo görünümünde veri tek tek satırı temsil etmek için kullanılır. Apple birkaç varsayılan tablo hücre türlerini tanımlanan:

- **Varsayılan** - bir seçeneği görüntü hücre ve sağdan sola hizalı başlığına sol tarafındaki bu türü sunar. 
- **Alt başlık** - ilk satırı ve daha küçük sola hizalı başlığında sola hizalı altyazısı sonraki satırında bu türü sunar.
- **1 değeri** -bu tür bir daha hafif renkli, sağa hizalı altyazısı sola hizalı başlıkla aynı çizgi üzerinde gösterir.
- **2 değerini** -bu tür bir daha hafif renkli, sola hizalı altyazısı sağa hizalı başlıkla aynı çizgi üzerinde gösterir.

Tüm varsayılan tablo görünümü hücre türleri de açığa göstergeleri veya onay işaretleri gibi grafik öğeleri destekler. 

Ayrıca, tanımlayabilirsiniz bir **özel** tablo görünüm hücre türü ve mevcut bir _prototip hücre_, ya da arabirimi Tasarımcısı'nda veya kod aracılığıyla oluşturduğunuz.

Apple Tablo görünümü hücrelerle çalışmak için aşağıdaki önerileri vardır:

- **Metin kırpma kaçının** -böylece bunlar sona ermez tek tek satırlık metin kısa kesilmiş tutun. Kesilmiş kelimeler ve ifadeler kullanıcının gelen arasında yer ayrıştırmak zordur.
- **Focused satır durumu göz önünde bulundurun** - bir satır ile yuvarlanmasını daha büyük, azaldığından köşeleri odakta olduğunda, gereken tüm durumlarda, hücrenin görünüm test. Görüntü veya metin kırpılmış hale veya Focused durumda yanlış bakın.
- **Tutumlu düzenlenebilir tabloları kullanmak** -taşıma veya tablo satırları silme daha tvOS üzerinde iOS zun. Bu özellik veya ekleme tvOS uygulamanızdan gelen rahatsız dikkatle karar vermeniz gerekir.
- **Özel hücre türleri burada uygun oluşturmak** - yerleşik tablo görünümü hücre türleri pek çok durumda için harika çalışırken daha fazla denetim sağlamak ve daha iyi bilgi sunmak için özel hücre türler standart bilgi oluşturmayı düşünün Kullanıcı.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Tablo görünümler ile çalışma

Xamarin.tvOS uygulamada tablosu görünümleri ile çalışmak için kolay oluşturmak ve değiştirmek görünümlerini arabirimi Tasarımcısı'nda için yoludur.

Başlamak için aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
1. Visual Studio'da Mac için yeni bir tvOS uygulama projesi başlatın ve seçin **tvOS** > **uygulama** > **Single View uygulaması** tıklatıp  **Sonraki** düğmesi: 

    [![](table-views-images/table02.png "Tek görünüm uygulaması seçin")](table-views-images/table02.png#lightbox)
1. Girin bir **adı** tıklatın ve uygulama için **sonraki**: 

    [![](table-views-images/table03.png "Uygulama için bir ad girin")](table-views-images/table03.png#lightbox)
1. Ya da ayarlamak **proje adı** ve **çözüm adı** veya Varsayılanları kabul edin ve **oluşturma** düğmesi yeni bir çözüm oluşturmak için: 

    [![](table-views-images/table04.png "Proje adı ve çözüm adı")](table-views-images/table04.png#lightbox)
1. İçinde **çözüm paneli**, çift tıklatın `Main.storyboard` dosyayı iOS Tasarımcısı açın: 

    [![](table-views-images/table05.png "Main.storyboard dosyası")](table-views-images/table05.png#lightbox)
1. Seçin ve delete **varsayılan görünüm denetleyicisi**: 

    [![](table-views-images/table06.png "Seçin ve varsayılan görünüm denetleyicisi silin")](table-views-images/table06.png#lightbox)
1. Seçin bir **bölme görünüm denetleyicisini** gelen **araç** ve tasarım yüzeyine sürükleyin.
1. Varsayılan olarak, elde edersiniz bir [bölünmüş görünümlü](~/ios/tvos/user-interface/split-views.md) ile bir **Gezinti View Controller** ve **tablo View Controller** sol tarafındaki ve **View Controller** sağ taraftaki içinde. Bu, Apple'nın önerilen tvOS Tablo görünümünde kullanımını oluşur: 

    [![](table-views-images/table08.png "Bölünmüş görünümü ekleme")](table-views-images/table08.png#lightbox)
1. Tablo görünümünde her bölümünü seçin ve özel bir Ata gerekecek **sınıf adı** içinde **pencere öğesi** sekmesinde **özellikleri Explorer** , bunu daha sonra C# dilinde erişebilmesi için kod. Örneğin, **tablo View Controller**: 

    [![](table-views-images/table09.png "Bir sınıf adı atayın")](table-views-images/table09.png#lightbox)
1. Özel bir sınıf için oluşturduğunuzdan emin olun **tablo View Controller**, **Tablo görünümü** ve tüm **prototip hücreleri**. Oluşturuldukları sırada Mac için visual Studio Proje ağacına özel sınıfları ekleyin: 

    [![](table-views-images/table10.png "Proje ağacında özel sınıflar")](table-views-images/table10.png#lightbox)
1. Ardından, Tablo görünümünde tasarım yüzeyine seçin ve onun özelliklerini gerektiği gibi ayarlayın. Sayısı gibi **prototip hücreleri** ve **stili** (düz veya Grouped): 

    [![](table-views-images/table11.png "Pencere öğesi sekmesi")](table-views-images/table11.png#lightbox)
1. Her **prototip hücre**, onu seçin ve benzersiz bir Ata **tanımlayıcısı** içinde **pencere öğesi** sekmesinde **özellikleri Explorer**. Bu adım _çok önemli_ bu tanımlayıcı daha sonra ihtiyacınız olacak şekilde zaman, doldurmak tablo. Örneğin `AttrCell`: 

    [![](table-views-images/table12.png "Pencere öğesi sekmesi")](table-views-images/table12.png#lightbox)
1. Hücre biri olarak sunmak için seçebilirsiniz [varsayılan tablo görünümü hücre türleri](#Table-View-Cell-Types) aracılığıyla **stili** açılır veya ayarlamak **özel** ve tasarım yüzeyi Düzen hücre kullanın diğer UI pencere öğeleri sürükleyerek **araç**: 

    [![](table-views-images/table13.png "Hücre düzeni")](table-views-images/table13.png#lightbox)
1. Benzersiz bir Ata **adı** prototip hücre tasarımında her kullanıcı Arabirimi öğesine **pencere öğesi** sekmesinde **özellikleri Explorer** daha sonra C# kodunda erişebilmesi için: 

    [![](table-views-images/table14.png "Bir ad atayın")](table-views-images/table14.png#lightbox)
1. Tüm Tablo görünümünde prototip hücrelerden biri için yukarıdaki adımı yineleyin.
1. Ardından, özel sınıflar UI tasarım, Düzen Ayrıntılar görünümünü ve ata benzersiz kalanını atayın **adları** her kullanıcı Arabirimi öğesi Ayrıntılar için bunları C# ' de erişebilmesi için görüntüleyin. Örneğin: 

    [![](table-views-images/table15.png "UI düzeni")](table-views-images/table15.png#lightbox)
1. Film şeridi için yaptığınız değişiklikleri kaydedin.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Visual Studio'da yeni bir tvOS uygulama projesi başlatın ve seçin **tvOS** > **Single View uygulaması** ve uygulamanız için bir ad girin. Tıklatın **Tamam** düğmesi yeni bir çözüm oluşturmak için: 

    [![](table-views-images/table02-vs.png "Tek görünüm uygulaması seçin")](table-views-images/table02-vs.png#lightbox)
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosyayı iOS Tasarımcısı açın: 

    [![](table-views-images/table05-vs.png "Main.storyboard dosyası")](table-views-images/table05-vs.png#lightbox)
1. Seçin ve delete **varsayılan görünüm denetleyicisi**: 

    [![](table-views-images/table06-vs.png "Seçin ve varsayılan görünüm denetleyicisi silin")](table-views-images/table06-vs.png#lightbox)
1. Seçin bir **bölme görünüm denetleyicisini** gelen **araç** ve tasarım yüzeyine sürükleyin: 

    [![](table-views-images/table07-vs.png "Bir bölme görünüm denetleyicisi")](table-views-images/table07-vs.png#lightbox)
1. Varsayılan olarak, elde edersiniz bir [bölünmüş görünümlü](~/ios/tvos/user-interface/split-views.md) ile bir **Gezinti View Controller** ve **tablo View Controller** sol tarafındaki ve **View Controller** sağ taraftaki içinde. Bu, Apple'nın önerilen tvOS Tablo görünümünde kullanımını oluşur: 

    [![](table-views-images/table08-vs.png "Düzen kullanıcı Arabirimi")](table-views-images/table08-vs.png#lightbox)
1. Tablo görünümünde her bölümünü seçin ve özel bir Ata gerekecek **sınıf adı** içinde **pencere öğesi** sekmesinde **özellikleri Explorer** , bunu daha sonra C# dilinde erişebilmesi için kod. Örneğin, **tablo View Controller**: 

    [![](table-views-images/table09-vs.png "Pencere öğesi sekmesi")](table-views-images/table09-vs.png#lightbox)
1. Özel bir sınıf için oluşturduğunuzdan emin olun **tablo View Controller**, **Tablo görünümü** ve tüm **prototip hücreleri**. Oluşturuldukları sırada Mac için visual Studio Proje ağacına özel sınıfları ekleyin: 

    [![](table-views-images/table10-vs.png "Proje ağacında özel sınıflar")](table-views-images/table10-vs.png#lightbox)
1. Ardından, Tablo görünümünde tasarım yüzeyine seçin ve onun özelliklerini gerektiği gibi ayarlayın. Sayısı gibi **prototip hücreleri** ve **stili** (düz veya Grouped): 

    [![](table-views-images/table11-vs.png "Pencere öğesi sekmesi")](table-views-images/table11-vs.png#lightbox)
1. Her **prototip hücre**, onu seçin ve benzersiz bir Ata **tanımlayıcısı** içinde **pencere öğesi** sekmesinde **özellikleri Explorer**. Bu adım _çok önemli_ bu tanımlayıcı daha sonra ihtiyacınız olacak şekilde zaman, doldurmak tablo. Örneğin `AttrCell`: 

    [![](table-views-images/table12-vs.png "Bir tanımlayıcıyı atayın")](table-views-images/table12-vs.png#lightbox)
1. Hücre biri olarak sunmak için seçebilirsiniz [varsayılan tablo görünümü hücre türleri](#Table-View-Cell-Types) aracılığıyla **stili** açılır veya ayarlamak **özel** ve tasarım yüzeyi Düzen hücre kullanın diğer UI pencere öğeleri sürükleyerek **araç**: 

    [![](table-views-images/table13-vs.png "Stil açılır")](table-views-images/table13-vs.png#lightbox)
1. Benzersiz bir Ata **adı** prototip hücre tasarımında her kullanıcı Arabirimi öğesine **pencere öğesi** sekmesinde **özellikleri Explorer** daha sonra C# kodunda erişebilmesi için: 

    [![](table-views-images/table14-vs.png "Pencere öğesi sekmesi")](table-views-images/table14-vs.png#lightbox)
1. Tüm Tablo görünümünde prototip hücrelerden biri için yukarıdaki adımı yineleyin.
1. Ardından, özel sınıflar UI tasarım, Düzen Ayrıntılar görünümünü ve ata benzersiz kalanını atayın **adları** her kullanıcı Arabirimi öğesi Ayrıntılar için bunları C# ' de erişebilmesi için görüntüleyin. Örneğin: 

    [![](table-views-images/table15.png "UI düzeni")](table-views-images/table15.png#lightbox)
1. Film şeridi için yaptığınız değişiklikleri kaydedin.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Bir veri modeli tasarlama

Sunulan Tablo görünümünde daha kolay görüntüleme bilgileri ile çalışma yapmak için ve ilgili ayrıntılı bilgileri (kullanıcı seçer veya Tablo görünümünde satırları vurgular gibi) sunan kolaylaştırır, özel bir sınıf veya veri modeli için bilgi olarak davranacak şekilde sınıfları oluşturmak için .

Bir listesini içeren bir seyahat kayıt uygulama örneği ele **Şehir**, benzersiz bir listesini içeren her **ilgi çekici Özellikler** , kullanıcı seçebilirsiniz. Kullanıcı bir çekim olarak işaretlemek açabilecektir bir *sık kullanılan*almak için seçin *yönergeleri* bir çekim için ve *bir uçuş kitap* verilen şehre.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Veri modeli için oluşturmak için bir **çekim**, proje adına sağ tıklayın **çözüm paneli** seçip **Ekle** > **yeni dosya...** . Girin `AttractionInformation` için **adı** tıklatıp **yeni** düğmesi: 

[![](table-views-images/data01.png "AttractionInformation için bir ad girin")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Veri modeli için oluşturmak için bir **çekim**, proje adına sağ tıklayın **Çözüm Gezgini** seçip **Ekle** > **yeni öğe ...** . Seçin **sınıfı** ve girin `AttractionInformation` için **adı** tıklatıp **Ekle** düğmesi: 

[![](table-views-images/data01-vs.png "Sınıfı seçin ve AttractionInformation için bir ad girin")](table-views-images/data01-vs.png#lightbox)

-----

Düzen `AttractionInformation.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

Özellikleri hakkında bilgi depolamak için bu sınıfın sağladığı bir verilen **çekim**.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Ardından, proje adına sağ tıklayın **çözüm paneli** yeniden seçip **Ekle** > **yeni dosya...** . Girin `CityInformation` için **adı** tıklatıp **yeni** düğmesi: 

[![](table-views-images/data02.png "CityInformation için bir ad girin")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ardından, proje adına sağ tıklayın **Çözüm Gezgini** yeniden seçip **Ekle** > **yeni öğe...** . Girin `CityInformation` için **adı** tıklatıp **Ekle** düğmesi: 

[![](table-views-images/data02-vs.png "CityInformation için bir ad girin")](table-views-images/data02-vs.png#lightbox)

-----

Düzen `CityInformation.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

Bu sınıfın hedef hakkında tüm bilgileri tutan **Şehir**, koleksiyonu **ilgi çekici Özellikler** bu şehre ait ve iki yardımcı yöntemler sağlar (`AddAttraction`) için ilgi çekici özellikler eklemek daha kolay Şehir.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>Tablo görünümü veri kaynağı

Her tablo görünümü, bir veri kaynağı gerektirir (`UITableViewDataSource`) tablosu için veri sağlar ve gerekli satırlar olarak oluşturmak için tablo görünüm tarafından gerekli.

Yukarıda verilen örnek için proje adına sağ tıklayın **Çözüm Gezgini**seçin **Ekle** > **yeni dosya...**  ve çağrısından `AttractionTableDatasource` tıklatıp **yeni** oluşturmak için düğmesi. Ardından, düzenleme `AttractionTableDatasource.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

Bir sınıfın birkaç bölümlerde ayrıntılı olarak bakalım.

İlk olarak, biz benzersiz tanımlayıcı (yukarıdaki arabirimi Tasarımcısı'nda atanan aynı tanımlayıcısı olduğu) prototip hücrenin tutmak için bir sabit tanımlanan, bir kısayol Tablo görünümü denetleyicisine ekledikten ve depolama verileri için oluşturulan:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Ardından, biz tablo View Controller Kaydet sonra yapı ve (yukarıda tanımlanan veri modelleri kullanarak), veri kaynağı doldurmak sınıfı oluşturulduğunda:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Örneğin, amacıyla `PopulateCities` yöntemi yalnızca veri modeli nesneler oluşturur bellekte bu gerçek bir uygulamada bir veritabanı veya web hizmetinden kolayca okunabilen ancak:

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

`NumberOfSections` Yöntemi tablosunda bölüm sayısı döndürür:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

İçin **düz** tablosu görünümleri stilde, her zaman 1 döndürür.

`RowsInSection` Yöntemi geçerli bölümdeki satır sayısını döndürür:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

Yeniden için **düz** tablosu görünümleri, veri kaynağında toplam öğe sayısını döndürür.

`TitleForHeader` Yöntemi döndürür başlığı verili bölümü:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

İçin bir **düz** Tablo görünümü yazın, başlık boş bırakın (`""`).

Son olarak, Tablo görünümünde tarafından istendiğinde, oluşturmak ve kullanarak bir prototip hücre doldurmak `GetCell` yöntemi: 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

İle çalışma hakkında daha fazla bilgi için bir `UITableViewDatasource`, lütfen bkz. Apple'nın [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) belgeleri.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Tablo görünümü temsilci

Her tablo görünümü, bir temsilci gerektirir (`UITableViewDelegate`) kullanıcı etkileşimi veya tablo diğer sistem olaylarına yanıt verecek şekilde.

Yukarıda verilen örnek için proje adına sağ tıklayın **Çözüm Gezgini**seçin **Ekle** > **yeni dosya...**  ve çağrısından `AttractionTableDelegate` tıklatıp **yeni** oluşturmak için düğmesi. Ardından, düzenleme `AttractionTableDelegate.cs` dosya ve şu şekilde görünür yapın:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

Bir çeşitli bölümleri Bu sınıf, Ayrıntılar bakalım.

İlk olarak, sınıf oluşturulduğunda, biz Tablo görünümü denetleyici için bir kısayol oluşturun:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Ardından, bir satır seçildiğinde (kullanıcı, Touch yüzeyinin üzerinde Apple uzak) işaretlemek istiyoruz **çekim** sık kullanılan olarak seçilen satırın tarafından temsil edilen:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

(Bunu Apple uzak Touch yüzeyini kullanarak odak vererek) bir satır kullanıcı vurgular, ardından, biz ayrıntılarını sunmak istediğiniz **çekim** bu satırının bizim bölme görünüm denetleyicisi tarafından Ayrıntıları bölümünde gösterilen:

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

`CanFocusRow` Yöntemi odak Tablo görünümünde hakkında bilgi almak için her satır için çağrılır. Dönüş `true` satır odak alırsanız başka dönmek `false`. Bu örnekte, özel bir oluşturduk `AttractionHighlighted` odağı alırken, her satırda geçirilen olay.

İle çalışma hakkında daha fazla bilgi için bir `UITableViewDelegate`, lütfen bkz. Apple'nın [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) belgeleri.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>Tablo görünümü hücresi

Her prototip arabirimi tasarımcısı Tablo görünümünde eklenen hücre için de özel bir tablo görünümü hücresi örneğini oluşturduğunuz (`UITableViewCell`) oluşturulduğu şekilde yeni hücreye (satır) doldurmak izin vermek için.

Örnek uygulama için çift `AttractionTableCell.cs` düzenlemek için açın ve aşağıdaki gibi görünmesini sağlamak için dosya:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

Bu sınıf, çekim veri modeli nesne için depolama sağlar (`AttractionInformation` yukarıda tanımlandığı gibi) belirli bir satırda görüntülenen:

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

`UpdateUI` Yöntemi doldurur UI Widgets gerektiği gibi (yani hücrenin prototip arabirimi Tasarımcısı'nda eklenen):

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

İle çalışma hakkında daha fazla bilgi için bir `UITableViewCell`, lütfen bkz. Apple'nın [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) belgeleri.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Tablo görünümü denetleyicisi

Bir tablo View Controller (`UITableViewController`) film şeridi arabirimi tasarımcısı aracılığıyla için eklenmiş olan bir tablo görünümü yönetir.

Örnek uygulama için çift `AttractionTableViewController.cs` düzenlemek için açın ve aşağıdaki gibi görünmesini sağlamak için dosya:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

Bu sınıf daha yakın bir göz atalım. İlk olarak, tablo görünümün erişimi kolay hale getirmek için kısayolları oluşturduk `DataSource` ve `TableDelegate`. Bunlar daha sonra Ayrıntılar görünümü sağ bölme görünüm sol tarafındaki Tablo görünümünde arasındaki iletişim kurmak için kullanacağız.

Tablo görünümünde belleğe yüklendiğinde, son olarak, örneklerini oluşturuyoruz `AttractionTableDatasource` ve `AttractionTableDelegate` (her ikisi de yukarıda oluşturduğunuz) ve Tablo görünümüne ekleyin.

İle çalışma hakkında daha fazla bilgi için bir `UITableViewController`, lütfen bkz. Apple'nın [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) belgeleri.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Birlikte tüm çekme

Bu belge listesinin başında belirtildiği gibi tablo görünümleri genellikle sütunlardan biri görüntülenir bir [bölünmüş görünümlü](~/ios/tvos/user-interface/split-views.md) ters tarafta görüntülenen seçili öğenin ayrıntılarını ile gezinti olarak. Örneğin: 

[![](table-views-images/intro01.png "Örnek uygulamayı çalıştırma")](table-views-images/intro01.png#lightbox)

Bu tvOS standart desende olduğuna göre her şeyi araya getirmek için son adımlar bakalım ve sahip bölünmüş görünümü sağ ve sol yanlarından etkileşim birbirleri ile.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>Ayrıntılı Görünüm

Seyahat uygulama örneği için özel bir sınıf, yukarıda sunulan (`AttractionViewController`) bölme görünüm ayrıntılı görünüm olarak sağ tarafındaki sunulan standart görünüm denetleyicinin tanımlanır:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

Burada, biz sağladığınız **çekim** (`AttractionInformation`) bir özellik olarak görüntülenir ve oluşturulan bir `UpdateUI` arabirim tasarımcısındaki görünüm UI pencere öğeleri doldurur yöntemi eklenir.

Biz de geri bölme görünüm denetleyiciye bir kısayol tanımladığınız (`SplitView`) biz iletişim kurmak için kullanacağınız Tablo görünümüne değiştirir (`AcctractionTableView`).

Özel Eylemler (olayları) için üç son olarak, eklenen `UIButton` arabirimi Tasarımcısı'nda oluşturulan örnek, bir çekim olarak işaretlemek kullanıcı izin bir _sık kullanılan_, get _yönergeleri_ için bir çekim ve _bir uçuş kitap_ verilen şehre.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Gezinti görünümü denetleyicisi

Bölünmüş Görünüm sol tarafındaki bir gezinti görünümü denetleyicisi tablo View Controller iç içe olduğundan, özel bir sınıf Gezinti View Controller atandı (`MasterNavigationController`) arabirimi Tasarımcısı'nda ve tanımı aşağıdaki gibidir:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Yeniden, bu sınıfın yalnızca bölme görünüm denetleyicisini iki taraf arasında iletişim kurmak kolaylaştırmak için birkaç kısayolları tanımlar:

* `SplitView` -Bölme görünüm denetleyicisini bağlantıdır (`MainSpiltViewController`) gezinti View Controller ait.
* `TableController` -Tablo görünümü denetleyici alır (`AttractionTableViewController`) gezinti View Controller üst görünümünde olarak sunulur.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Bölme görünüm denetleyicisi

Bölme görünüm denetleyicisini uygulamamız temeli olduğundan, özel bir sınıf oluşturduğumuz (`MasterSplitViewController`) için bu arabirimi Tasarımcısı'nda ve aşağıdaki şekilde tanımladık:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

İlk olarak, biz kısayollar oluşturmanıza **ayrıntıları** bölünmüş görünümün yan yana (`AttractionViewController`) ve **ana** yan (`MasterNavigationController`). Yeniden, bu, daha sonra iki taraf arasında iletişim kurmak kolaylaştırır.

Ardından, bölme görünüm belleğe yüklendiğinde, biz bölme görünüm denetleyicisini bölünmüş görünümü her iki tarafında ekleyin ve Tablo görünümünde bir çekim vurgulama kullanıcı yanıt (`AttractionHighlighted`) içinde yeni çekim görüntüleyerek **ayrıntıları**  bölünmüş görünümün yan.

Lütfen bakın [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) tablosu görünümleri bölünmüş görünümü içinde tam uygulamasını için örnek uygulama.

## <a name="table-views-in-detail"></a>Ayrıntılı tablosu görünümleri

TvOS dışına iOS tabanlı olduğundan, tablo görünümleri ve Tablo görünümü denetleyicileri tasarlanmıştır ve benzer şekilde davranır. Bizim iOS bir Xamarin uygulaması Tablo görünümü ile çalışma hakkında daha ayrıntılı bilgi için lütfen bkz [tabloları ve hücreleriyle çalışma](~/ios/user-interface/controls/tables/index.md) belgeleri.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve Xamarin.tvOS uygulama içinde tablo görünümleriyle çalışma ele. Ve tvOS uygulama Tablo görünümünde tipik kullanımını olan bir bölünmüş görünümlü içinde bir tablo görünümü ile çalışmaya ilişkin bir örnek sunulan.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
