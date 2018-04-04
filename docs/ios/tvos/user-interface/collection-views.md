---
title: Koleksiyon görünümleri ile çalışma
description: Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde koleksiyonu görünümlerle çalışma kapsar.
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7fa38aa81e5929bdc88ceebd153d86cfcd92f20e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-collection-views"></a>Koleksiyon görünümleri ile çalışma

_Bu makalede tasarlama ve Xamarin.tvOS uygulama içinde koleksiyonu görünümlerle çalışma kapsar._

Koleksiyon görünümlerini içerik için bir grubu rasgele düzenleri kullanarak görüntülenmesine izin verir. Yerleşik destek kullanarak, bunlar için kolay oluşturma kılavuz benzeri ya da doğrusal düzenleri, ayrıca özel düzenler desteklerken izin verin.

[![](collection-views-images/collection01.png "Örnek koleksiyon görünümü")](collection-views-images/collection01.png#lightbox)

Koleksiyon görünümü, kullanıcı etkileşimi ve koleksiyon içeriği sağlamak için bir temsilci ve veri kaynağı kullanarak öğeleri koleksiyonu tutar. Koleksiyon görünümü görünümünü bağımsız bir düzen alt sisteminde dayalı olduğundan farklı bir düzen sağlama koleksiyonu görünümün verileri üzerinde-çalışma sırasında sunumu kolayca değiştirebilirsiniz.

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>Koleksiyon görünümleri hakkında

Bir koleksiyon görünümü, yukarıda belirtildiği gibi (`UICollectionView`) öğelerinin sıralı bir koleksiyonunu yönetir ve özelleştirilebilir düzenleriyle öğelerin sayısını gösterir. Koleksiyon görünümlerini çalışma tablosu görünümleri ile benzer bir şekilde (`UITableView`), daha fazlasını tek bir sütunda öğeleri sunma düzenleri kullanabileceklerini dışında.

Bir koleksiyon görünümü içinde tvOS kullanırken, uygulamanızı bir veri kaynağını kullanan koleksiyonla ilişkili veri sağlamaktan sorumludur (`UICollectionViewDataSource`). Koleksiyon görünümü verileri isteğe bağlı olarak düzenlenir ve farklı gruplar halinde (bölümler) sunulan.

Koleksiyon görünümü bir hücre kullanmanın ekranda bireysel öğeleri gösterir (`UICollectionViewCell`), belirli bir bilgi (örneğin, bir görüntü ve başlığını) koleksiyonundan sunumu sağlar.

İsteğe bağlı olarak, ek görünümler bölümler ve hücreler için üstbilgi ve altbilgi davranacak şekilde koleksiyonu görünümün sunu eklenebilir. Koleksiyon görünümün düzenini, tek tek hücrelere yanı sıra Bu görünümlere yerleşimini tanımlamaktan sorumludur.

Koleksiyon görünümü, bir temsilci kullanarak kullanıcı etkileşimine yanıt (`UICollectionViewDelegate`). Bu temsilci, bir hücre vurgulanmış belli bir hücre odağı alabilirsiniz veya seçilmiş olması durumunda belirlemek için sorumludur. Bazı durumlarda, temsilci tek tek hücrelere boyutunu belirler.

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>Koleksiyon görünümü düzenleri

Önemli bir özelliği bir koleksiyon görünümü, kendi sunan veri ve onun düzenini arasında ayrılmasıdır. Bir koleksiyon görünümü Düzen (`UICollectionViewLayout`) kuruluş ve konumunu hücreleri (ve tüm tamamlayıcı görünümler) ile toplama görünümün ekranda sunusunda sağlamaktan sorumludur.

Tek tek hücrelere ekli veri kaynağından koleksiyon görünümü tarafından oluşturulan ve ardından düzenlenmiş ve verilen koleksiyon görünümü düzeni tarafından görüntülenir.

Koleksiyon görünümü oluşturulduğunda koleksiyon görünümü düzeni normal olarak sağlanır. Ancak, herhangi bir zamanda koleksiyon görünümü düzenini değiştirebilirsiniz ve koleksiyon görünümün verileri ekranda sunumu sağlanan yeni düzen kullanılarak otomatik olarak güncelleştirilir.

Koleksiyon görünümü düzenini (varsayılan olarak hiçbir animasyon yapılır) iki farklı düzenleri arasında geçiş animasyon için kullanılabilecek çeşitli yöntemler sağlar. Ayrıca, koleksiyon görünümü düzenleri, daha fazla düzeninde bir değişiklik sonuçlanır kullanıcı etkileşimi animasyon için hareketi tanıyıcılarla çalışabilir.

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>Hücreleri ve Tamamlayıcı görünümler oluşturma

Bir koleksiyon görünümün veri kaynağı yalnızca içeriği görüntülemek için kullanılan hücreleri koleksiyonunun öğesi, aynı zamanda yedekleme verileri sunmak için sorumlu değildir.

Koleksiyon görünümlerini öğelerinin büyük topluluklara işlemek üzere tasarlanmış olması nedeniyle, tek tek hücrelere kuyruktan çıkarıldı ve bellek kısıtlamaları taşmasını engellemenin yeniden kullanılabilir. Dequeueing görünümler için iki farklı yöntem vardır:

- `DequeueReusableCell` -Oluşturur veya (uygulamanın film şeridi içinde belirtildiği şekilde) belirtilen türde bir hücre döndürür.
- `DequeueReusableSupplementaryView` -Oluşturur veya (uygulamanın film şeridi içinde belirtildiği şekilde) belirtilen tür tamamlayıcı bir görünümünü verir.

Bu yöntemlerin her ikisi çağırmadan önce sınıfı kaydetmelisiniz film şeridi veya `.xib` koleksiyon görünümü ile hücrenin görünümü oluşturmak için kullanılan dosya. Örneğin:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

Burada `typeof(CityCollectionViewCell)` görünümü destekleyen sınıfı sağlar ve `CityViewDatasource.CardCellId` hücre (veya Görünüm) kuyruktan çıkarıldı zaman kullanılan kimliği sağlar.

Hücre kuyruktan çıkarıldı sonra onu temsil eden öğe için verileri ile yapılandırın ve koleksiyon görünümü görüntülemek için geri dönün.

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>Koleksiyon görünümü denetleyiciler hakkında

Bir koleksiyon View Controller (`UICollectionViewController`) özel bir görünüm denetleyicisidir (`UIViewController`), aşağıdaki davranış sağlar:

- Koleksiyon görünümü şeridini yüklemekten sorumlu veya `.xib` dosya ve görünümü örneği. Kodda oluşturduysanız, otomatik olarak yeni, yapılandırılmamış bir koleksiyon görünümü oluşturur.
- Koleksiyon görünümü yüklendikten sonra denetleyici veri kaynağına ve temsilci film şeridi görünümünden yüklemeyi dener veya `.xib` dosyası. Hiçbiri yoksa kendisini hem kaynak olarak ayarlar.
- Verilerin koleksiyon görünümü görüntülenen ilk doldurulur önce yüklenir ve yeniden yükler ve sonraki her görüntü Seç temizleyin sağlar.

Ayrıca, koleksiyon görünümü denetleyicisi koleksiyon görünümü yaşam döngüsü gibi yönetmek için kullanılan geçersiz kılınabilir yöntemleri sağlayan `AwakeFromNib` ve `ViewWillDisplay`.

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>Koleksiyon görünümlerini ve film şeritleri

Bir koleksiyon görünümü Xamarin.tvOS uygulamanızda çalışmak için en kolay yoludur şeridini birine eklemek için. Hızlı bir örnek olarak, size bir görüntü, başlık ve seçme düğmesi sunar ve örnek bir uygulama oluşturmak için adımıdır. Kullanıcı Seç düğmesine tıklarsanız, yeni bir görüntü seçmesine izin veren bir koleksiyon görünümü görüntülenir. Bir görüntü seçildiğinde, koleksiyon görünümü kapatılır ve yeni resim ve başlığı görüntülenir.

Şimdi aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

    
1. Yeni bir başlangıç **tek bir görünüm tvOS uygulama** Mac için Visual Studio'da
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve iOS Tasarımcısı açın.
1. Bir görüntü görünüm eklemek için varolan bir düğmeyi ve bir etiket görüntülemek ve bunları şuna benzer şekilde yapılandırabilirsiniz: 

    [![](collection-views-images/collection02.png "Örnek düzeni")](collection-views-images/collection02.png#lightbox)
1. Ata bir **adı** görüntü görüntüleme ve etiketinde **pencere öğesi sekmesini** , **özellikleri Explorer**. Örneğin: 

    [![](collection-views-images/collection03.png "Ayar adı")](collection-views-images/collection03.png#lightbox)
1. Ardından, bir koleksiyon görünümü denetleyicisi film şeridi sürükleyin: 

    [![](collection-views-images/collection04.png "Bir koleksiyon görünümü denetleyicisi")](collection-views-images/collection04.png#lightbox)
1. Control-sürükleyin düğmesinden koleksiyonu görünüm denetleyiciye ve seçin **anında** açılan gelen: 

    [![](collection-views-images/collection05.png "Anında iletme açılır penceresinden seçin")](collection-views-images/collection05.png#lightbox)
1. Uygulama çalıştırıldığında bu kullanıcı düğmesine tıkladığında Göster olması koleksiyon görünümü hale getirir.
1. Koleksiyon görünümü seçin ve aşağıdaki değerleri girin **Düzen sekmesini** , **özellikleri Explorer**: 

    [![](collection-views-images/collection06.png "Özellikler Gezgini")](collection-views-images/collection06.png#lightbox)
1. Bu, tek tek hücrelere ve hücreler ve koleksiyon görünümü dış ucunun arasındaki kenarlıkları boyutunu denetler.
1. Koleksiyon görünümü denetleyicisi seçin ve kendi sınıfı kümesine `CityCollectionViewController` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection07.png "CityCollectionViewController için set sınıfı")](collection-views-images/collection07.png#lightbox)
1. Koleksiyon görünümü seçin ve kendi sınıfı kümesine `CityCollectionView` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection08.png "CityCollectionView için set sınıfı")](collection-views-images/collection08.png#lightbox)
1. Koleksiyon görünümü hücreyi seçin ve kendi sınıfı kümesine `CityCollectionViewCell` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection09.png "CityCollectionViewCell için set sınıfı")](collection-views-images/collection09.png#lightbox)
1. İçinde **pencere öğesi sekmesini** emin **düzeni** olan `Flow` ve **kaydırma yönü** olan `Vertical` koleksiyon görünümü için: 

    [![](collection-views-images/collection10.png "Pencere öğesi sekmesi")](collection-views-images/collection10.png#lightbox)
1. Koleksiyon görünümü hücresi seçin ve ayarlayın, **kimlik** için `CityCell` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection11.png "Kimlik CityCell için ayarlama")](collection-views-images/collection11.png#lightbox)
1. Değişikliklerinizi kaydedin.
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. Yeni bir başlangıç **tek bir görünüm tvOS uygulama** Visual Studio.
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosya ve iOS Tasarımcısı açın.
1. Bir görüntü görünüm eklemek için varolan bir düğmeyi ve bir etiket görüntülemek ve bunları şuna benzer şekilde yapılandırabilirsiniz: 

    [![](collection-views-images/collection02vs.png "Düzeni Yapılandır")](collection-views-images/collection02vs.png#lightbox)
1. Ata bir **adı** görüntü görüntüleme ve etiketinde **pencere öğesi sekmesini** , **özellikleri Explorer**. Örneğin: 

    [![](collection-views-images/collection03vs.png "Özellikler Gezgini")](collection-views-images/collection03vs.png#lightbox)
1. Ardından, bir koleksiyon görünümü denetleyicisi film şeridi sürükleyin: 

    [![](collection-views-images/collection04vs.png "Bir koleksiyon görünümü denetleyicisi")](collection-views-images/collection04vs.png#lightbox)
1. Control-sürükleyin düğmesinden koleksiyonu görünüm denetleyiciye ve seçin **anında** açılan gelen: 

    [![](collection-views-images/collection05vs.png "Anında iletme açılır penceresinden seçin")](collection-views-images/collection05vs.png#lightbox)
1. Uygulama çalıştırıldığında bu kullanıcı düğmesine tıkladığında Göster olması koleksiyon görünümü hale getirir.
1. Koleksiyon görünümü seçin ve **Düzen sekmesini** , **özellikleri Explorer** girin **genişliği** olarak _361_ ve  **Yükseklik** olarak _256_ 
1. Bu, tek tek hücrelere ve hücreler ve koleksiyon görünümü dış ucunun arasındaki kenarlıkları boyutunu denetler.
1. Koleksiyon görünümü denetleyicisi seçin ve kendi sınıfı kümesine `CityCollectionViewController` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection07vs.png "CityCollectionViewController için set sınıfı")](collection-views-images/collection07vs.png#lightbox)
1. Koleksiyon görünümü seçin ve kendi sınıfı kümesine `CityCollectionView` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection08vs.png "CityCollectionView için set sınıfı")](collection-views-images/collection08vs.png#lightbox)
1. Koleksiyon görünümü hücreyi seçin ve kendi sınıfı kümesine `CityCollectionViewCell` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection09vs.png "CityCollectionViewCell için set sınıfı")](collection-views-images/collection09vs.png#lightbox)
1. İçinde **pencere öğesi sekmesini** emin **düzeni** olan `Flow` ve **kaydırma yönü** olan `Vertical` koleksiyon görünümü için: 

    [![](collection-views-images/collection10vs.png "Tgizli pencere öğesi sekmesi")](collection-views-images/collection10vs.png#lightbox)
1. Koleksiyon görünümü hücresi seçin ve ayarlayın, **kimlik** için `CityCell` içinde **pencere öğesi sekmesini**: 

    [![](collection-views-images/collection11vs.png "Kimlik CityCell için ayarlama")](collection-views-images/collection11vs.png#lightbox)
1. Değişikliklerinizi kaydedin.
    

-----

Tercih etmiş `Custom` koleksiyonu görünümün için **düzeni**, biz belirtilen özel bir düzen. Apple sağlayan yerleşik `UICollectionViewFlowLayout` ve `UICollectionViewDelegateFlowLayout` , kolayca sunabileceği veri kılavuz tabanlı bir düzende (bunlar tarafından kullanılan `flow` düzen stili). 

Film şeritleri ile çalışma hakkında daha fazla bilgi için lütfen bkz bizim [Hello, tvOS Hızlı Başlangıç Kılavuzu](~/ios/tvos/get-started/hello-tvos.md).

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>Veri toplama görünümünü sağlama

Bizim koleksiyon görünümü (ve koleksiyon görünümü denetleyicisi) bizim film şeridi eklendi sahibiz, biz koleksiyon için verileri sağlamanız gerekir. 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>Veri modeli

İlk olarak, biz görüntülemek için başlık ve seçilecek Şehir izin vermek için bir bayrak resmini filename tutan veri modeli oluşturmak için adımıdır.

Oluşturma bir `CityInfo` sınıfı ve şu şekilde görünür yapın:

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>Koleksiyon görünümü hücresi

Şimdi biz verilerin her bir hücre için nasıl sunulacak tanımlamanız gerekir. Düzenleme `CityCollectionViewCell.cs` (sizin için otomatik olarak film şeridi dosyanızdan oluşturulan) dosyası ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

Bizim tvOS uygulaması için size bir görüntü ve isteğe bağlı bir başlık görüntülüyor. Verilen Şehir seçeneği belirlerseniz, biz aşağıdaki kodu kullanarak görüntü görünüm karartma:

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

Görüntüyü içeren hücre odak kullanıcı tarafından duruma getirildiğinde, yerleşik kullanmak istiyoruz Parallax etkisi aşağıdaki özelliği ayarlama:

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

Gezinti ve odak hakkında daha fazla bilgi için lütfen bkz bizim [gezinti ve odak çalışma](~/ios/tvos/app-fundamentals/navigation-focus.md) ve [Siri uzak ve Bluetooth denetleyicileri](~/ios/tvos/platform/remote-bluetooth.md) belgeleri.


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>Koleksiyon görünümü veri sağlayıcısı

Oluşturulan veri modelimizi ve tanımlanan bizim hücre düzeni ile bir veri kaynağı için bizim koleksiyon görünümü oluşturalım. Veri kaynağı değil yalnızca yedekleme verileri, aynı zamanda dequeueing tek tek hücrelere ekranda görüntülemek için hücreleri sağlamaktan sorumlu olacaktır.

Oluşturma bir `CityViewDatasource` sınıfı ve şu şekilde görünür yapın:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

Bu sınıf ayrıntılı bakın sağlar. İlk olarak, biz devralınmalıdır `UICollectionViewDataSource` ve hücreleri (biz iOS Tasarımcısı atanır) kimliği için bir kısayol sağlayın:

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

Sonraki için koleksiyon veri depolama alanı sağlamak ve veri doldurmak için bir sınıf sağlar:

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

Biz geçersiz kılma sonra `NumberOfSections` yöntemi ve return bizim koleksiyonu Göster bölümleri (gruplar öğelerin) sayısı. Bu durumda, yalnızca bir olduğundan:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

Ardından, biz öğe sayısı aşağıdaki kodu kullanarak bizim koleksiyonunda döndürün:

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

Son olarak, koleksiyon görünümü aşağıdaki kodla istediğinde biz yeniden kullanılabilir bir hücre dequeue:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

Biz bir koleksiyon görünümü hücrenin aldıktan sonra bizim `CityCollectionViewCell` türü, biz doldurmak onu ile verilen öğe.

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>Kullanıcı olaylara yanıt verme

Bizim koleksiyonundan öğeyi seçmek kullanıcı istiyoruz çünkü Biz bu etkileşimi işlemek için bir koleksiyon görünümü temsilci sağlamanız gerekir. Ve biz kullanıcı öğesinin ne bilmeniz bizim arama görünümü izin vermek için bir yol seçilmiş sağlamanız gerekir.

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>Uygulama temsilcisi

Seçili öğenin koleksiyonu görünümünden geri çağırma görünümüne ilişkilendirmek için bir yol ihtiyacımız var. Biz bir özel özellik üzerinde kullanmaya başlayacağınız bizim `AppDelegate`. Düzen `AppDelegate.cs` dosya ve aşağıdaki kodu ekleyin:

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

Bu özelliği tanımlar ve başlangıçta gösterilecek varsayılan şehir ayarlar. Daha sonra biz kullanıcının seçimini görüntülemek ve değiştirilecek Seç izin vermek için bu özelliği kullanmak.

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>Koleksiyon görünümü temsilci

Ardından, yeni bir ekleme `CityViewDelegate` sınıf projeye ve aşağıdaki gibi görünmesi:


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

Bu sınıf daha yakın bir göz atalım. İlk olarak, biz devralınmalıdır `UICollectionViewDelegateFlowLayout`. Biz bu sınıftan devralınan nedenini ve `UICollectionViewDelegate` yerleşik kullanıyoruz olduğu `UICollectionViewFlowLayout` bizim öğeleri ve özel yerleşim türü sunmak için.

Ardından, bu kod kullanılarak tek tek öğelerin boyutu döndürün:

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

Ardından, belirli bir hücreye aşağıdaki kodu kullanarak odak alırsanız karar: 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

Belirli bir yedekleme verileri olup olmadığını denetleyin, `CanSelect` bayrağı ayarlanmış `true` ve bu değeri döndürür. Gezinti ve odak hakkında daha fazla bilgi için lütfen bkz bizim [gezinti ve odak çalışma](~/ios/tvos/app-fundamentals/navigation-focus.md) ve [Siri uzak ve Bluetooth denetleyicileri](~/ios/tvos/platform/remote-bluetooth.md) belgeleri.

Son olarak, aşağıdaki kod bir öğesiyle seçerek kullanıcıya yanıt vermemiz:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

Burada `SelectedCity` özelliği bizim `AppDelegate` öğesine seçilen kullanıcıya ve biz koleksiyon görünümü denetleyicisi bize adlı görünümüne döndürme kapatın. Tanımlı olmayan `ParentController` bizim koleksiyon görünümü henüz özelliği, biz gerçekleştirebilirsiniz, sonraki.

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>Koleksiyon görünümü yapılandırma

Şimdi biz bizim koleksiyon görünümü düzenleme ve bizim veri kaynağı ve temsilci atamak gerekir. Düzenleme `CityCollectionView.cs` (bize için otomatik olarak bizim film şeridi görünümünden oluşturulan) dosyası ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

Bir kısayol erişmek için ilk olarak, sağladığımız bizim `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

Ardından, bir kısayol koleksiyonu görünümün veri kaynağı ve bir özellik için koleksiyon View Controller (bizim temsilci yukarıdaki kullanıcı seçimi yaptığında koleksiyonu kapatmak için kullanılır) erişmek için sunuyoruz:

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

Ardından, biz koleksiyon görünümü başlatmak ve bizim hücre sınıfı, veri kaynağı ve temsilci atamak için aşağıdaki kodu kullanın:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

Son olarak, kullanıcı bunu vurgulanmış sahip olduğunda yalnızca görünür olmasını görüntüsü altında başlık (odak) istiyoruz. Aşağıdaki kod ile bunu:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Sıfır (0) odağı kaybeden önceki öğesinin transparence ayarlarız ve sonraki öğe transparence kazanmak için % 100 odak. Bu geçiş animasyon de.


## <a name="configuring-the-collection-view-controller"></a>Koleksiyon görünümü denetleyicisi yapılandırma

Şimdi biz bizim koleksiyon görünümü üzerinde son yapılandırma yapmanıza ve kullanıcı bir seçim yaptıktan sonra koleksiyon görünümü kapatılabilir, böylece biz tanımlanan özelliğini ayarlamak denetleyici izin vermeniz gerekir.

Düzenleme `CityCollectionViewController.cs` (bizim film şeridi görünümünden otomatik olarak oluşturulan) dosyası ve şu şekilde görünür yapın:

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>Tüm birlikte koyma 

Biz tüm sahip olduğunuza göre bir araya bölümlerini doldurmak ve bizim koleksiyon görünümü denetlemek için bir kez bizim ana görünüme her şeyi araya getirmek için son düzenlemeler gerekiyor.

Düzenleme `ViewController.cs` (bizim film şeridi görünümünden otomatik olarak oluşturulan) dosyası ve şu şekilde görünür yapın:

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

Aşağıdaki kod başlangıçta seçili öğesini görüntüler `SelectedCity` özelliği `AppDelegate` ve kullanıcı koleksiyonu görünümünden bir seçim yaptıktan sonra görüntüler:

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>Uygulamayı test etme

Yapı ve uygulama çalıştırırsanız herşeyi yerinde varsayılan şehir ile ana görünüm görüntülenir:

[![](collection-views-images/run01.png "Ana Ekran")](collection-views-images/run01.png#lightbox)

Kullanıcı tıklatırsanız **bir görünüm seçin** düğmesi, koleksiyon görünümü görüntülenir:

[![](collection-views-images/run02.png "Koleksiyon görünümü")](collection-views-images/run02.png#lightbox)

Sahip herhangi bir şehre kendi `CanSelect` özelliğini `false` görüntülenir görünüyorsa ve kullanıcı odak ayarlamak mümkün olmaz. Kullanıcı bir öğeyi zaman vurgular (kolaylaştırır odak) başlığı görüntülenir ve 3D subtlety Eğim görüntü Parallax efekti kullanabilirsiniz.

Kullanıcı görüntüyü seçin tıkladığında, koleksiyon görünümü kapatılır ve ana görünüme sahip yeni bir görüntü yeniden görüntülenir:

[![](collection-views-images/run03.png "Yeni bir görüntü ana ekran")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>Özel düzen oluşturma ve öğeleri yeniden sıralama

Bir koleksiyon görünümü kullanarak temel özellikleri de özel düzenler oluşturmak yeteneğidir. İOS tvOS devralır olduğundan, özel bir düzen oluşturma işlemi aynı kalır. Lütfen bakın bizim [koleksiyon görünümlerini giriş](~/ios/user-interface/controls/uicollectionview.md) daha fazla bilgi için.

Koleksiyon görünümlerini iOS için yeni eklenen 9 kolayca koleksiyondaki öğelerin yeniden sıralama izin becerisidir. İOS 9 kümesini tvOS 9 olduğuna göre yeniden, bu bunları yapılır aynı şekilde. Lütfen bakın bizim [koleksiyon görünümü değişikliklerini](~/ios/user-interface/controls/uicollectionview.md) daha fazla ayrıntı için belge.


<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve koleksiyon görünümlerini ile Xamarin.tvOS uygulama içinde çalışma ele. İlk olarak, bu koleksiyon görünümü oluşturan tüm öğeleri açıklanmıştır. Ardından, bir film şeridi kullanarak bir koleksiyon görünümü tasarlayıp nasıl oluşturulacağını gösterir. Son olarak, öğeleri yeniden sıralama ve özel düzenler oluşturma hakkındaki bilgilere bağlantılar sağlanır.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
