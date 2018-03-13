---
title: "Metin ve arama alanları ile çalışma"
description: "Bu makalede tasarlama ve metin ve arama alanlarını Xamarin.tvOS uygulama içinde çalışma kapsar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 7d58c30e745e26d1076e75470e527cbe95e85eb6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-text-and-search-fields"></a>Metin ve arama alanları ile çalışma

_Bu makalede tasarlama ve metin ve arama alanlarını Xamarin.tvOS uygulama içinde çalışma kapsar._



Gerekli olduğunda Xamarin.tvOS uygulamanızı küçük metin parçaları (örneğin, kullanıcı kimliklerini ve parolaları) kullanıcıdan isteyebilir bir metin alanı kullanarak ve ekran klavyesi:

[![](text-fields-and-search-images/intro01.png "Örnek arama alanı")](text-fields-and-search-images/intro01.png#lightbox)

İsteğe bağlı olarak, bir arama alanı kullanarak uygulamanın içeriği anahtar sözcüğü arama yeteneklerini sağlayabilirsiniz:

[![](text-fields-and-search-images/intro02.png "Örnek arama sonuçları")](text-fields-and-search-images/intro02.png#lightbox)

Bu belge, bir Xamarin.tvOS uygulamasında metin ve arama alanları ile çalışma ayrıntılarını ele alınacaktır.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>Metin ve arama alanları hakkında

Gerekli olursa, yukarıda belirtildiği gibi Xamarin.tvOS kullanan kullanıcı metni küçük miktarda toplamak için bir veya daha fazla metin alanları sunabilir bir ekran (veya isteğe bağlı bluetooth klavye kullanıcı yüklü tvOS sürümüne bağlı olarak). 

Ayrıca, uygulamanız (örneğin, bir müzik, filmler veya resim koleksiyonu) kullanıcıya içerik büyük miktarlarda sunarsa, küçük miktarda kullanılabilir öğeleri listesini filtrelemek için metin girmesini sağlayan bir arama alanı eklemek isteyebilirsiniz.

<a name="Text-Fields" />

## <a name="text-fields"></a>Metin alanları

TvOS içinde bir metin alanı getirir bir sabit yükseklik, yuvarlatılmış köşe giriş kutusu olarak sunulan bir kullanıcı üzerinde tıklattığında klavyesi:

[![](text-fields-and-search-images/text01.png "Metin alanları içinde tvOS")](text-fields-and-search-images/text01.png#lightbox)

Kullanıcı ne zaman hareket [odak](~/ios/tvos/app-fundamentals/navigation-focus.md) belirli bir metin alanı için bunu büyük büyür ve derin gölge görüntüler. Metin alanları diğer kullanıcı Arabirimi öğeleri odakta olduğunda binebilir olarak bu, kullanıcı arabiriminizi tasarlarken unutmayın gerekecektir.

Apple metin alanları ile çalışmak için aşağıdaki önerileri vardır:

- **Metin girişi tutumlu kullanmak** - doğası nedeniyle ekran klavyesi, uzun bölümler metin girerek veya birden çok metin alanları doldurma kullanıcıya can sıkıcı. Seçim listeleri kullanarak metin girişi miktarını sınırlamak için en iyi çözümdür veya [düğmeleri](~/ios/tvos/user-interface/buttons.md).
- **Amaç iletişim kurmak için ipuçlarını kullanma** -metin alanı boş olduğunda yer tutucu "ipuçları" görüntüleyebilir. Uygunsa, metin alanı ayrı bir etiket yerine amacı açıklamak için ipuçları kullanın.
- **Uygun varsayılan klavye türünü seçin** -tvOS metin alanı için belirttiğiniz birkaç farklı yerleşik amaçlı klavye türler sağlar. Örneğin, e-posta adresi klavye girdisi kullanıcının son girilen adresleri listesinden seçmesine olanak tanıyarak kolaylaştırabilir.
- **Uygun olduğunda, güvenli metin alanları kullanın** -güvenli metin alanı (yerine gerçek harf) noktaların girdiğiniz karakter sayısını gösterir. Her zaman güvenli metin alanı parolalar gibi hassas bilgileri toplama sırasında kullanın.

<a name="Keyboards" />

## <a name="keyboards"></a>Klavyeler

Klavye, ekran kullanıcının kullanıcı arabiriminde, bir doğrusal bir metin alanı tıkladığında görüntülenir. Touch yüzeyini kullanıcının kullandığı [Siri uzaktan](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) tek tek harfler klavyeden seçin ve istenen bilgileri girin:

[![](text-fields-and-search-images/keyboard01.png "Siri uzak klavye")](text-fields-and-search-images/keyboard01.png#lightbox)

Geçerli görünümde, birden fazla metin alanı ise bir **sonraki** düğmesi otomatik olarak gösterilecek sonraki metin alan kullanıcı. A **Bitti** bitiş metin girişi ve kullanıcı önceki ekrana dönün son metin alanı için düğmesi görüntülenir. 

Herhangi bir zamanda kullanıcı tuşlarına da basabilirsiniz **menü** metin girişi sonlandırmak ve yeniden önceki ekrana dönmek için Siri uzak düğmesi.

Apple ile çalışmak için aşağıdaki önerileri ekran klavyeler sahiptir:

- **Uygun varsayılan klavye türünü seçin** -tvOS metin alanı için belirttiğiniz birkaç farklı yerleşik amaçlı klavye türler sağlar. Örneğin, e-posta adresi klavye girdisi kullanıcının son girilen adresleri listesinden seçmesine olanak tanıyarak kolaylaştırabilir.
- **Uygun olduğunda, klavye donatıyı görünümleri kullanma** - standart yanı sıra her zaman görüntülenen, isteğe bağlı donatıyı görünümler (görüntü veya etiketleri gibi) bilgileri eklenebilir metin girişi veya çok amaçlı açıklamak için ekran klavyesi Kullanıcı, gerekli bilgileri girmenize yardımcı olmak.

İle çalışma hakkında daha fazla bilgi için Ekran Klavyesi, lütfen bkz. Apple'nın [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [klavye yönetme](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [veri girişi için özel görünümler](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) ve [ İOS için metin Programlama Kılavuzu](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) belgeleri.

<a name="Search" />

## <a name="search"></a>Ara

Bir arama alanı metin alanı sağlayan özel bir ekran sunmak ve, ekran klavyesi klavye görüntülenen öğeleri koleksiyonu filtre olanak tanır:

[![](text-fields-and-search-images/search01.png "Örnek arama sonuçları")](text-fields-and-search-images/search01.png#lightbox)

Kullanıcı Ara alanına harf girerken, sonuçları otomatik olarak arama sonuçlarını yansıtır. Kullanıcı herhangi bir zamanda odak sonuçları kaydırma ve sunulan öğelerden birini seçin.

Apple arama alanları ile çalışmak için aşağıdaki önerileri vardır:

- **Son aramalar sağlamak** - Siri uzaktan metin girerek can sıkıcı olabilir ve kullanıcıların eğilimindedir arama istekleri yinelemek, son arama sonuçları bir bölümünü geçerli sonuçları klavye alanı altında önce eklemeyi düşünün.
- **Mümkün olduğunda, sonuçlarını numarası sınırlamak** - büyük bir öğe listesi Kullanıcı ayrıştırma ve gidin, döndürülen sonuç sayısı sınırlandırabilirsiniz zor olabilir.
- **Uygunsa, sağlama arama sonucu filtreler** - uygulamanız tarafından sağlanan içerik kendisini uygundur daha fazla döndürülen arama sonuçlarını filtrelemek izin vermek için kapsam çubukları eklemeyi düşünün.

Daha fazla bilgi için lütfen Apple'nın bkz [UISearchController sınıf başvurusu](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html).

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>Metin alanları ile çalışma

Xamarin.tvOS uygulamada metin alanları ile çalışmak için en kolay yolu, onları iOS Tasarımcısı kullanarak kullanıcı arabirimi tasarımı eklemektir.

Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

1. İçinde **çözüm paneli**, çift `Main.storyboard` dosyayı düzenlemek için açın.
1. Bir veya daha fazla Sürükle **metin alanları** int bir görünüm tasarım yüzeyine: 

    [![](text-fields-and-search-images/text02.png "Bir metin alanı")](text-fields-and-search-images/text02.png#lightbox)
1. Seçin **metin alanları** ve her bir benzersiz vermek **adı** içinde **pencere öğesi** sekmesinde **özellikleri paneli**: 

    [![](text-fields-and-search-images/text03.png "Özellikler paneli pencere öğesi sekmesi")](text-fields-and-search-images/text03.png#lightbox)
1. İçinde **metin alanı** bölümünde gibi öğeleri tanımlayabilirsiniz **yer tutucu** ipucu ve varsayılan **değeri**: 

    [![](text-fields-and-search-images/text04.png "Metin alanı bölümü")](text-fields-and-search-images/text04.png#lightbox)
1. Özellikler gibi tanımlamak için aşağı kaydırmanız **yazım denetimi**, **büyük/küçük harf** ve varsayılan **klavye türü**: 

    [![](text-fields-and-search-images/text05.png "Yazım denetimi, büyük/küçük harf ve varsayılan klavye türü")](text-fields-and-search-images/text05.png#lightbox) 
1. Şeridinizin için değişiklikleri kaydedin.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosyayı düzenlemek için açın.
1. Bir veya daha fazla Sürükle **metin alanları** int bir görünüm tasarım yüzeyine: 

    [![](text-fields-and-search-images/text02-vs.png "Bir metin alanı")](text-fields-and-search-images/text02-vs.png#lightbox)
1. Seçin **metin alanları** ve her bir benzersiz vermek **adı** içinde **pencere öğesi** sekmesinde **özellikleri Explorer**: 

    [![](text-fields-and-search-images/text03-vs.png "Pencere öğesi sekmesi")](text-fields-and-search-images/text03-vs.png#lightbox)
1. İçinde **metin alanı** bölümünde gibi öğeleri tanımlayabilirsiniz **yer tutucu** ipucu ve varsayılan **değeri**: 

    [![](text-fields-and-search-images/text04-vs.png "Metin alanı bölümü")](text-fields-and-search-images/text04-vs.png#lightbox)
1. Özellikler gibi tanımlamak için aşağı kaydırmanız **yazım denetimi**, **büyük/küçük harf** ve varsayılan **klavye türü**: 

    [![](text-fields-and-search-images/text05-vs.png "Yazım denetimi, büyük/küçük harf ve varsayılan klavye türü")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. Şeridinizin için değişiklikleri kaydedin.
    
-----

Kodda, almak veya metin alanını kullanarak değerini ayarlamak, `Text` özelliği:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

İsteğe bağlı olarak kullanabileceğiniz `Started` ve `Ended` metin alanı olaylar için başlangıç ve bitiş metin girişi yanıt.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>Arama alanları ile çalışma

Arama alanlarla Xamarin.tvOS uygulamada çalışmaya en kolay yolu, onları arabirimi Tasarımcısı'nı kullanarak kullanıcı arabirimi tasarımı eklemektir.

Aşağıdakileri yapın:

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)
    
1. İçinde **çözüm paneli**, çift `Main.storyboard` dosyayı düzenlemek için açın.
1. Yeni bir koleksiyon görünümü denetleyicisi kullanıcının arama sonuçlarını sunmak için film şeridi sürükleyin: 

    [![](text-fields-and-search-images/search02.png "Bir koleksiyon görünümü denetleyicisi")](text-fields-and-search-images/search02.png#lightbox)
1. İçinde **pencere öğesi** sekmesinde **özellikleri paneli**, kullanın `SearchResultsViewController` için **sınıfı** ve `SearchResults` için **film şeridi kimliği**: 

    [![](text-fields-and-search-images/search03.png "Pencere öğesi sekmesi")](text-fields-and-search-images/search03.png#lightbox)
1. Seçin **hücre prototip** tasarım yüzeyine.
1. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, kullanın `SearchResultCell` için **sınıfı** ve `ImageCell` için **tanımlayıcısı**: 

    [![](text-fields-and-search-images/search04.png "Pencere öğesi sekmesi")](text-fields-and-search-images/search04.png#lightbox)
1. Düzen tasarımı, **hücre prototip** ve her benzersiz bir öğesiyle kullanıma **adı** içinde **pencere öğesi** sekmesinde **özellikleri Explorer**: 

    [![](text-fields-and-search-images/search05.png "Düzen hücre prototip tasarım")](text-fields-and-search-images/search05.png#lightbox)
1. Şeridinizin için değişiklikleri kaydedin.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. İçinde **Çözüm Gezgini**, çift `Main.storyboard` dosyayı düzenlemek için açın.
1. Yeni bir koleksiyon görünümü denetleyicisi kullanıcının arama sonuçlarını sunmak için film şeridi sürükleyin: 

    [![](text-fields-and-search-images/seach02-vs.png "Bir koleksiyon görünümü denetleyicisi")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, kullanın `SearchResultsViewController` için **sınıfı** ve `SearchResults` için **film şeridi kimliği**: 

    [![](text-fields-and-search-images/search03-vs.png "Pencere öğesi sekmesi")](text-fields-and-search-images/search03-vs.png#lightbox)
1. Seçin **hücre prototip** tasarım yüzeyine.
1. İçinde **pencere öğesi** sekmesinde **özellikleri Explorer**, kullanın `SearchResultCell` için **sınıfı** ve `ImageCell` için **tanımlayıcısı**: 

    [![](text-fields-and-search-images/search04-vs.png "Pencere öğesi sekmesi")](text-fields-and-search-images/search04-vs.png#lightbox)
1. Düzen tasarımı, **hücre prototip** ve her benzersiz bir öğesiyle kullanıma **adı** içinde **pencere öğesi** sekmesinde **özellikleri Explorer**: 

    [![](text-fields-and-search-images/search05-vs.png "Düzen hücre prototip tasarım")](text-fields-and-search-images/search05-vs.png#lightbox)
1. Şeridinizin için değişiklikleri kaydedin.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>Bir veri modeli sağlar

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Ardından, kullanıcı için arama veri modeli sonuçlar olarak görev yapması için bir sınıf sağlamanız gerekir. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **Ekle** > **yeni dosya...**   >  **Genel** > **boş sınıfı** ve sağlayan bir **adı**: 

[![](text-fields-and-search-images/search06.png "Boş sınıfı seçin ve bir ad sağlayın")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ardından, kullanıcı için arama veri modeli sonuçlar olarak görev yapması için bir sınıf sağlamanız gerekir. İçinde **Çözüm Gezgini**, proje adına sağ tıklayın ve seçin **Ekle** > **yeni öğe...**   >  **Apple** > **çeşitli** > **sınıfı** ve sağlayan bir **adı**: 

[![](text-fields-and-search-images/search06-vs.png "Sınıfı seçin ve bir ad sağlayın")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

Örnek olarak, resim başlık ve anahtar sözcüğü koleksiyonu arama yapmasına izin veren bir uygulama aşağıdakine benzeyebilir:

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>Koleksiyon görünümü hücresi

Yerinde veri modeli ile düzenleme **prototip hücre** (`SearchResultViewCell.cs`) ve hale görünüm bulunan aşağıdaki:

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

`UpdateUI` Yöntemi, bağımsız alanlarının listesini görüntülemek için kullanılacak **PictureInformation** öğeleri ( `PictureInfo` özelliği) adlandırılmış kullanıcı Arabirimi öğeleri özelliği her zaman güncelleştirilir. Örneğin, resim ve resimle ilişkili başlığı.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>Koleksiyon görünümü denetleyicisi

Ardından, arama sonuçları koleksiyon görünümü denetleyicisi Düzenle (`SearchResultsViewController.cs`) ve şu şekilde görünür yapın:

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

İlk olarak, `IUISearchResultsUpdating` arabirimi, kullanıcı tarafından güncelleştirilen arama denetleyicisi filtre işlemek için sınıfa eklenir:

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

Bir sabit ayrıca Kimliğini belirtmek için tanımlanan **prototip hücre** (yukarıdaki arabirimi Tasarımcısı'nda tanımlanan kimliği eşleşen) kullanılacak daha sonra koleksiyonu denetleyicisi yeni hücreye istediğinde:

```csharp
public const string CellID = "ImageCell";
```

Depolama arama filtresi terimi Aranmakta öğelerinin tam listesi için oluşturulur ve bu terimiyle eşleşen öğeleri listesi:

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

Zaman `SearchFilter` olan değişti, eşleşen öğeleri listesini güncelleştirilir ve koleksiyon görünümün içeriği yeniden yüklendi. `FindPictures` Yordamdır sorumlu yeni arama terimiyle eşleşen öğeleri bulmak için:

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

Değeri `SearchFilter` (hangi sonuçları koleksiyon görünümü güncelleştirir) güncelleştirilecek kullanıcı arama denetleyicisi filtrede değiştiğinde:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

`PopulatePictures` Yöntemi başlangıçta doldurur kullanılabilir öğeleri koleksiyonu:

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

Bu örnek amacıyla, tüm örnek verileri oluşturuluyor bellekte koleksiyon görünümü denetleyicisi yüklendiğinde. Gerçek bir uygulamada bu veri büyük olasılıkla bir veritabanı veya web hizmetinden okuma ve yalnızca gelen tutmak için gerektiğinde Apple TV's taşmasını bellek sınırlıdır.

`NumberOfSections` Ve `GetItemsCount` yöntemleri eşleşen öğe sayısını belirtin:

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

`GetCell` Yöntem yeni bir **prototip hücre** (temel `CellID` film şeridi yukarıda tanımlanan) koleksiyon görünümü her öğe için:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell` Yöntemi yapılandırılabilir şekilde görüntülenmesini hücre önce çağrılır:

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

`DidUpdateFocus` Yöntemi sonuçları koleksiyon görünümünde öğelerini vurgulayın gibi bu görsel geribildirim kullanıcıya sağlar:

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

Son olarak, `ItemSelected` yöntemi (Siri uzaktan Touch yüzeyiyle tıklayarak) bir öğeyi sonuçları koleksiyon görünümü seçerek kullanıcı işler:

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

Arama alanı kullanın (üst onu çağırma Görünüm), bir modal iletişim görünüm olarak sunulan varsa `DismissViewController` yöntemi kullanıcı bir öğeyi seçtiğinde arama görünümü yok sayın. Bu örnek için onu buraya kapatılan olmayan şekilde arama alanı sekmesi görünümü sekmesini içeriği olarak sunulur.

Koleksiyon görünümleri hakkında daha fazla bilgi için lütfen bkz bizim [koleksiyonu görünümlerle çalışma](~/ios/tvos/user-interface/collection-views.md) belgeleri.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>Arama alanı sunma

Başlıca iki yolu vardır, bir arama alanı (ve onun ilişkili Klavyesi ve arama sonuçlarında) tvOS kullanıcıya sunulan: 

- **Kalıcı iletişim kutusu görünümü** -arama alanı sunulabilir geçerli görünümü ve görünüm denetleyicisi olarak bir tam ekran modal iletişim kutusu görünümü. Bu, genellikle yanıt bir düğme veya diğer UI öğesini tıklatarak kullanıcı olarak gerçekleştirilir. Kullanıcı bir öğeyi Arama sonuçlarından seçtiğinde iletişim kutusu kapatılır.
- **İçeriği görüntülemek** -arama alanı bir parçasıdır doğrudan belirli bir görünüm. Örneğin, içeriği sekmesini View Controller arama sekmede.

Yukarıda verilen resimleri aranabilir listesi Örneğin, arama alanı arama sekmesinde içeriği görüntüle olarak sunulan ve arama sekmesini View Controller aşağıdaki gibi görünür:

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

Bir sabit eşleşen ilk olarak, tanımlı **film şeridi tanımlayıcı** arabirimi Tasarımcısı'nda arama sonuçları koleksiyon görünümü denetleyicisi atandı:

```csharp
public const string SearchResultsID = "SearchResults";
```

Ardından, `ShowSearchController` yöntemi yeni bir arama görünümü koleksiyonu denetleyicisi ve onu gerekli görüntüler oluşturur:

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

Yukarıdaki yöntemi, bir kez bir `SearchResultsViewController` film şeridi görünümünden örneği yeni bir `UISearchController` arama alanı sunmak ve kullanıcıya Klavyesi için oluşturulur. Arama sonuçları koleksiyonu (tarafından tanımlanan `SearchResultsViewController`) bu klavye altında görüntülenir.

Ardından, `SearchBar` bilgilerle gibi yapılandırılmış **yer tutucu** ipucu. Bu kullanıcıya preformed arama türü hakkında bilgi sağlar.

Ardından kullanıcı için iki yoldan biriyle arama alanı sunulur:

- **Kalıcı iletişim kutusu görünümü** - `PresentViewController` yöntemi, varolan bir görünümü arama sunmak için çağrılır tam ekran.
- **İçeriği görüntülemek** - A `UISearchContainerViewController` arama denetleyicisi içerecek şekilde oluşturulur. A `UINavigationController` arama kapsayıcı içerecek şekilde gezinti denetleyici görünüm denetleyiciye eklendikten sonra oluşturulan `AddChildViewController (navController)`, sunulan görünümü `View.Add (navController.View)`.

Son olarak ve yeniden sunu türüne ya da bağlı `ViewDidLoad` veya `ViewDidAppear` yöntemi çağıracaktır `ShowSearchController` arama kullanıcıya sunmak üzere yöntemi:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

Ne zaman uygulamayı çalıştırın ve arama sekmenin, kullanıcı tarafından seçilen öğelerin tam filtrelenmemiş listesini kullanıcıya sunulur:

[![](text-fields-and-search-images/intro02.png "Varsayılan arama sonuçları")](text-fields-and-search-images/intro02.png#lightbox)

Kullanıcı bir arama terimi girmeniz başladığında, sonuçlardan terimin tarafından filtre ve otomatik olarak güncelleştirilen:

[![](text-fields-and-search-images/intro03.png "Filtrelenmiş arama sonuçları")](text-fields-and-search-images/intro03.png#lightbox)

Kullanıcı herhangi bir zamanda arama sonuçlarında bir öğeye geçiş yapar ve dokunma yüzeyi Siri uzaktan seçmek için tıklatın.

<a name="Summary" />

## <a name="summary"></a>Özet

Bu makalede, tasarlama ve metin ve arama alanlarını Xamarin.tvOS uygulama içinde çalışma kapsamına. Arabirim Tasarımcısı'nda metin ve arama koleksiyonu içeriği nasıl oluşturulacağını gösterir ve arama alanı tvOS kullanıcıya sunulacak iki farklı yollarını gösterdi.



## <a name="related-links"></a>İlgili bağlantılar

- [tvOS Örnekleri](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS İnsan Arabirimi kılavuzları](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS için uygulama programlama kılavuzu](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
