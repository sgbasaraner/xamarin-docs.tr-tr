---
title: Xamarin.iOS koleksiyonu görünümleri
description: Koleksiyon görünümlerini rasgele düzenleri kullanarak görüntülenecek içerik izin verin. Ayrıca özel düzenler desteklerken kılavuz benzeri düzeni kutudan çıktığında, kolayca oluşturma sağlarlar.
ms.prod: xamarin
ms.assetid: F4B85F25-0CB5-4FEA-A3B5-D22FCDC81AE4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: b9ba2f885364084d6bee67c460b4831c00c7ae55
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790633"
---
# <a name="collection-views-in-xamarinios"></a>Xamarin.iOS koleksiyonu görünümleri

_Koleksiyon görünümlerini rasgele düzenleri kullanarak görüntülenecek içerik izin verin. Ayrıca özel düzenler desteklerken kılavuz benzeri düzeni kutudan çıktığında, kolayca oluşturma sağlarlar._

Koleksiyon görünümleri, kullanılabilir `UICollectionView` sınıfı, birden çok öğe düzenleri kullanarak ekranda sunan tanıtan yeni bir kavram iOS 6 olarak olur. Veri sağlamak için desenleri bir `UICollectionView` öğelerini oluşturabilir ve aynı temsilci seçme ve veri kaynağı iOS geliştirme yaygın olarak kullanılan desenleri bu öğeleri izleyin etkileşim için.

Ancak, koleksiyon görünümlerini bağımsız bir düzen alt iş `UICollectionView` kendisi. Bu nedenle, yalnızca farklı bir düzen sağlama kolayca koleksiyon görünümü sunumu değiştirebilirsiniz.

iOS sağlar adlı bir düzen sınıf `UICollectionViewFlowLayout` satırı tabanlı düzenleri gibi ek bir iş ile oluşturulacak bir kılavuz sağlar. Ayrıca, özel düzenler de hayal edebildiğiniz sunu sağlayan oluşturulabilir.

## <a name="uicollectionview-basics"></a>UICollectionView temelleri

`UICollectionView` Sınıfı oluşur üç farklı öğeleri:

-  **Hücreleri** – her öğe için veri tabanlı görünümleri
-  **Ek görünümler** – verilere görünümleri bir bölümle ilişkilendirilmiş.
-  **Decoration görünümleri** – olmayan veri düzeni tarafından oluşturulan görünümleri tabanlı

## <a name="cells"></a>Hücreleri

Hücreleri koleksiyon görünümü tarafından sunulan veri kümesindeki tek bir öğeyi temsil eden nesneleridir. Her bir hücre örneğidir `UICollectionViewCell` aşağıdaki şekilde gösterildiği gibi üç farklı görünümlerini oluşan sınıfı:

 [![](uicollectionview-images/01-uicollectionviewcell.png "Her bir hücre üç farklı görünümlerini aşağıda gösterildiği gibi oluşur")](uicollectionview-images/01-uicollectionviewcell.png#lightbox)

`UICollectionViewCell` Sınıfı bu görünümlerin her biri için aşağıdaki özelliklere sahiptir:

-   `ContentView` – Bu görünüm hücre sunar içeriği içerir. Ekranda en üstteki z-sırada işlenir.
-   `SelectedBackgroundView` – Hücre seçimi desteği yerleşik. Bu görünüm, görsel olarak bir hücre seçildiğini belirtmek için kullanılır. Hemen altına çizilir `ContentView` bir hücre seçili olduğunda.
-   `BackgroundView` – Hücreleri de tarafından sunulan bir arka plan görüntülemek `BackgroundView` . Bu görünüm altındaki işlenen `SelectedBackgroundView` .


Ayarlayarak `ContentView` değerinden küçük olacak şekilde, `BackgroundView` ve `SelectedBackgroundView`, `BackgroundView` görsel olarak içerik, while çerçeve için kullanılan `SelectedBackgroundView` bir hücre seçildiğinde, aşağıda gösterildiği gibi gösterilir:

 [![](uicollectionview-images/02-cells.png "Farklı bir hücre öğeleri")](uicollectionview-images/02-cells.png#lightbox)

Yukarıdaki ekran görüntüsü hücrelerde devralarak oluşturulan `UICollectionViewCell` ve ayarı `ContentView`, `SelectedBackgroundView` ve `BackgroundView` özellikleri, sırasıyla aşağıdaki kodda gösterildiği gibi:

```csharp
public class AnimalCell : UICollectionViewCell
{
        UIImageView imageView;

        [Export ("initWithFrame:")]
        public AnimalCell (CGRect frame) : base (frame)
        {
            BackgroundView = new UIView{BackgroundColor = UIColor.Orange};

            SelectedBackgroundView = new UIView{BackgroundColor = UIColor.Green};

            ContentView.Layer.BorderColor = UIColor.LightGray.CGColor;
            ContentView.Layer.BorderWidth = 2.0f;
            ContentView.BackgroundColor = UIColor.White;
            ContentView.Transform = CGAffineTransform.MakeScale (0.8f, 0.8f);

            imageView = new UIImageView (UIImage.FromBundle ("placeholder.png"));
            imageView.Center = ContentView.Center;
            imageView.Transform = CGAffineTransform.MakeScale (0.7f, 0.7f);

            ContentView.AddSubview (imageView);
        }

        public UIImage Image {
            set {
                imageView.Image = value;
            }
        }
}
```

 <a name="Supplementary_Views" />


## <a name="supplementary-views"></a>Tamamlayıcı görünümleri

Tamamlayıcı görünümleri, her bölüm ile ilgili bilgileri sunan görünümleri olan bir `UICollectionView`. Hücreleri gibi ek verilere görünümlerdir. Hücreleri bir veri kaynağından alınan öğe verileri görüntülemektedir burada tamamlayıcı görünümleri yerleştirilmek Defteri'nde kategorilerini veya müzik Müzik Kitaplığı'nda türünü gibi bölüm verileri sunar.

Örneğin, bir tamamlayıcı görünüm belirli bir bölüm için bir başlık sunmak için aşağıdaki çizimde gösterildiği gibi kullanılabilir:

 [![](uicollectionview-images/02a-supplementary-view.png "Aşağıda gösterildiği gibi belirli bir bölüm için bir başlık sunmak için tamamlayıcı görünüm kullanılan")](uicollectionview-images/02a-supplementary-view.png#lightbox)

Tamamlayıcı görünümü kullanmak için ilk olarak kayıtlı olması gerekir `ViewDidLoad` yöntemi:

```csharp
CollectionView.RegisterClassForSupplementaryView (typeof(Header), UICollectionElementKindSection.Header, headerId);
```

Ardından, görünümü kullanarak döndürülen gerekiyor `GetViewForSupplementaryElement`, kullanılarak oluşturulmuş `DequeueReusableSupplementaryView`ve devraldığı `UICollectionReusableView`. Aşağıdaki kod parçacığını yukarıdaki ekran görüntüsünde gösterildiği SupplementaryView üretir:

```csharp
public override UICollectionReusableView GetViewForSupplementaryElement (UICollectionView collectionView, NSString elementKind, NSIndexPath indexPath)
        {
            var headerView = (Header)collectionView.DequeueReusableSupplementaryView (elementKind, headerId, indexPath);
            headerView.Text = "Supplementary View";
            return headerView;
        }

```

Tamamlayıcı yalnızca üstbilgiler ve altbilgiler daha fazla genel görünümlerdir.
Bunlar koleksiyon görünümünde herhangi bir yere yerleştirilebilir ve görünümlerini tam olarak özelleştirilebilir yapmadan herhangi görünümlerini oluşur.

 <a name="Decoration_Views" />


## <a name="decoration-views"></a>Decoration görünümleri

Decoration görünümlerdir görüntülenebilen tamamen visual görünümleri bir `UICollectionView`. Hücreleri ve Tamamlayıcı görünümleri aksine, bunlar verilere değil. Bunlar her zaman bir düzen ait bir alt kümesi içinde oluşturulur ve sonradan içeriğinin düzenini değiştirebilirsiniz. Örneğin, bir düzenleme görünümü içeriği ile birlikte kayar bir arka plan görünüm sunmak için kullanılabilecek `UICollectionView`, aşağıda gösterildiği gibi:

 [![](uicollectionview-images/02c-decoration-view.png "Kırmızı bir arka plan ile düzenleme görünümü")](uicollectionview-images/02c-decoration-view.png#lightbox)

 Aşağıdaki kod parçacığı örnekleri kırmızıyla arka değişikliklerini `CircleLayout` sınıfı:

 ```csharp
 public class MyDecorationView : UICollectionReusableView
    {
        [Export ("initWithFrame:")]
        public MyDecorationView (CGRect frame) : base (frame)
        {
            BackgroundColor = UIColor.Red;
        }
    }
 ```


## <a name="data-source"></a>veri kaynağı

Diğer parçaları iOS, gibi gibi `UITableView` ve `MKMapView`, `UICollectionView` kendi verisinden alır bir *veri kaynağı*, Xamarin.iOS içinde sunulan **`UICollectionViewDataSource`** sınıfı. Bu sınıf, içeriği sağlamaktan sorumludur `UICollectionView` gibi:

-  **Hücreleri** – döndürülen `GetCell` yöntemi.
-  **Ek görünümler** – döndürülen `GetViewForSupplementaryElement` yöntemi.
-  **Bölüm sayısı** – döndürülen `NumberOfSections` yöntemi. Varsayılan olarak 1 uygulanmadı.
-  **Bölüm başına öğe sayısı** – döndürülen `GetItemsCount` yöntemi.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController
Kolaylık sağlamak için `UICollectionViewController` sınıfı kullanılabilir. Bu, sonraki bölümde açıklanan, iki temsilcinin, olacak şekilde otomatik olarak yapılandırılır ve veri kaynağı için kendi `UICollectionView` görünümü.

İle `UITableView`, `UICollectionView` sınıfı yalnızca, ekranda öğeleri için hücreleri almak için kendi veri kaynağı çağırın.
Aşağıdaki resimde gösterildiği gibi ekranı kaydırarak hücreleri yeniden, kuyruğuna yerleştirilir:

 [![](uicollectionview-images/03-cell-reuse.png "Ekranı kaydırarak hücreleri kuyruğuna yeniden kullanım için aşağıda gösterildiği gibi yerleştirilir")](uicollectionview-images/03-cell-reuse.png#lightbox)

Hücre yeniden ile basitleştirilmiştir `UICollectionView` ve `UITableView`. Artık hücreleri sistemiyle kayıtlı gibi bir yeniden sırada kullanılabilir durumda olmazsa bir hücre doğrudan veri kaynağında oluşturmanız gerekir. Bir hücrenin yeniden sıranın hücreden kuyruktan çağrı yapılırken kullanılabilir durumda değilse, iOS, tür veya kaydedildiği nib alarak otomatik olarak oluşturur.
Aynı tekniği de tamamlayıcı görünümler için kullanılabilir.

Örneğin, kaydeder aşağıdaki kod göz önünde bulundurun `AnimalCell` sınıfı:

```csharp
static NSString animalCellId = new NSString ("AnimalCell");
CollectionView.RegisterClassForCell (typeof(AnimalCell), animalCellId);
```

Zaman bir `UICollectionView` kendi öğesi ekranda olduğundan hücre gereken `UICollectionView` kendi veri kaynağının çağırır `GetCell` yöntemi. Bunun UITableView ile nasıl çalıştığı, bu yöntem sorumlu olacaktır yedekleme verileri hücreden yapılandırmak için benzer bir `AnimalCell` bu durumda sınıfı.

Aşağıdaki kod uygulaması gösterir `GetCell` döndüren bir `AnimalCell` örneği:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, Foundation.NSIndexPath indexPath)
{
        var animalCell = (AnimalCell)collectionView.DequeueReusableCell (animalCellId, indexPath);

        var animal = animals [indexPath.Row];

        animalCell.Image = animal.Image;

        return animalCell;
}
```

Çağrı `DequeReusableCell` hücre ya da yeniden sıradan XML'deki Sıraya alınacak burada veya bir hücre çağrısında kayıtlı türü temel alarak oluşturulan sıradaki kullanılabilir değilse `CollectionView.RegisterClassForCell`.

Bu durumda, kaydetme tarafından `AnimalCell` sınıfı, iOS oluşturacak yeni bir `AnimalCell` dahili ve bir hücre kuyruktan çağrısı yapıldığında, daha sonra yapılandırılmış hayvan sınıfında yer alan ve görüntülenmek üzere döndürülengörüntüsüiledöndürün`UICollectionView`.

 <a name="Delegate" />


### <a name="delegate"></a>Temsilci

`UICollectionView` Sınıfını kullanan bir temsilci türü `UICollectionViewDelegate` içeriği ile etkileşimi desteklemek için `UICollectionView`. Bu denetimin sağlar:

-  **Hücre seçimi** – bir hücre seçili olup olmadığını belirler.
-  **Hücre vurgulama** – bir hücre şu anda işlemdeki varsa belirler.
-  **Hücre menüleri** – uzun tuşuna hareketi yanıta hücrede görüntülenen menüsü.


Veri kaynağıyla olduğu gibi `UICollectionViewController` için temsilci olarak varsayılan olarak yapılandırılmış `UICollectionView`.

 <a name="Cell_HighLighting" />


#### <a name="cell-highlighting"></a>Hücre vurgulama

Bir hücre basıldığında, vurgulanan durumu hücre geçişleri ve kadar seçili kullanıcı kendi parmak hücreden kaldırır. Gerçekte seçilmeden önce bu geçici bir değişiklik hücrenin görünümünü sağlar. Seçimin, hücre üzerine `SelectedBackgroundView` görüntülenir. Yalnızca seçimi oluşmadan önce aşağıdaki şekilde vurgulanan durumunu gösterir:

 [![](uicollectionview-images/04-cell-highlight.png "Yalnızca seçimi gerçekleşmeden önce bu şekil vurgulanan durumunu gösterir.")](uicollectionview-images/04-cell-highlight.png#lightbox)

Vurgulama, uygulamak için `ItemHighlighted` ve `ItemUnhighlighted` yöntemlerinin `UICollectionViewDelegate` kullanılabilir. Örneğin, aşağıdaki kod, sarı bir arka plan uygulanır `ContentView` hücre vurgulanmış, beyaz arka plan beklemediğiniz vurgulanmış, yukarıdaki resimde gösterildiği gibi:

```csharp
public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.Yellow;
}

public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
{
        var cell = collectionView.CellForItem(indexPath);
        cell.ContentView.BackgroundColor = UIColor.White;
}
```

 <a name="Disabling_Selection" />


#### <a name="disabling-selection"></a>Seçimi devre dışı bırakma

Seçimi, varsayılan olarak etkindir `UICollectionView`. Seçimi devre dışı bırakmak için geçersiz kılma `ShouldHighlightItem` ve aşağıda gösterildiği gibi false döndürür:

```csharp
public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath)
{
        return false;
}
```

Vurgulama devre dışı bırakıldığında bir hücre seçme de devre dışı bırakılır. Ayrıca, ayrıca vardır bir `ShouldSelectItem` seçimi olsa da, doğrudan denetler yöntemi varsa `ShouldHighlightItem` uygulanır ve yanlış döndürür `ShouldSelectItem` çağrılmaz.

 `ShouldSelectItem` Kapalı bir öğe tarafından temelinde açık seçilmesine izin verir, `ShouldHighlightItem` uygulanmadı. Ayrıca seçim olmadan vurgulama, tanır `ShouldHighlightItem` uygulanır ve sırasında true değerini döndürür `ShouldSelectItem` false değerini döndürür.

 <a name="Cell_Menus" />


#### <a name="cell-menus"></a>Hücre menüleri

Her hücre bir `UICollectionView` kesme, kopyalama ve yapıştırma isteğe bağlı olarak desteklenen veren bir menü görüntüleyebiliyorsa. Düzen menüsü bir hücreyi oluşturmak için:

1.  Geçersiz kılma `ShouldShowMenu` ve bir menü öğesini göster, true döndürür.
1.  Geçersiz kılma `CanPerformAction` ve herhangi bir kesme, kopyalama veya yapıştırma olacağı öğesi gerçekleştirebilirsiniz, her eylem için true döndürür.
1.  Geçersiz kılma `PerformAction` düzenleme gerçekleştirmek için kopyalama yapıştırma işlemi.


Aşağıdaki ekran görüntüsünde bir hücre uzun basıldığında menüyü göster:

 [![](uicollectionview-images/04a-menu.png "Bu ekran bir hücre uzun basıldığında menüyü göster")](uicollectionview-images/04a-menu.png#lightbox)

 <a name="Layout" />


## <a name="layout"></a>Düzen

`UICollectionView` konumlandırma tüm alt öğeleri, hücre, tamamlayıcı görünümleri ve Decoration görünümlerini bağımsız olarak yönetilmesini sağlayan bir düzen sistemin desteklediği `UICollectionView` kendisi.
Düzen sistemi kullanarak, bir uygulama düzenleri Biz bu makalede gördünüz yanı sıra özel düzenler sağlamak kılavuz benzeri bir gibi destekleyebilir.

 <a name="Layout_Basics" />


### <a name="layout-basics"></a>Düzen temelleri

Düzenleri bir `UICollectionView` öğesinden devralınan bir sınıf içinde tanımlanan `UICollectionViewLayout`. Düzen uygulamasıdır her öğe için Düzen özniteliklerini oluşturmaktan sorumlu `UICollectionView`. Bir düzen oluşturmanın iki yolu vardır:

-  Yerleşik kullanmak `UICollectionViewFlowLayout` .
-  Özel bir düzen devralarak sağlamak `UICollectionViewLayout` .


 <a name="Flow_Layout" />


### <a name="flow-layout"></a>Akış düzeni

`UICollectionViewFlowLayout` Sınıfı, bir satırı tabanlı yerleşim anlatıldığı gibi bir kılavuz hücreleri içeriği düzenlemek için bu uygun sağlar.

Bir akış düzeni kullanmak için:

-  Bir örneğini oluşturmak `UICollectionViewFlowLayout` :


```csharp
var layout = new UICollectionViewFlowLayout ();
```

-  Örnek oluşturucusuna geçirmek `UICollectionView` :


```csharp
simpleCollectionViewController = new SimpleCollectionViewController (layout);
```

Bir kılavuz düzeni içeriği için gereken tek şey budur. Ayrıca, yönlendirmeyi değiştiğinde, `UICollectionViewFlowLayout` içeriği aşağıda gösterildiği gibi uygun şekilde yeniden düzenleme işler:

 [![](uicollectionview-images/05-layout-orientation.png "Yönlendirme değişiklikleri örneği")](uicollectionview-images/05-layout-orientation.png#lightbox)

 <a name="Section_Inset" />


#### <a name="section-inset"></a>Bölüm iç

Bazı boşluk sağlamak üzere `UIContentView`, düzenleri sahip bir `SectionInset` türündeki özelliği `UIEdgeInsets`. Örneğin, aşağıdaki kodu her bölümünün geçici 50 piksel arabellek sağlar `UIContentView` tarafından düzenlendiği zaman bir `UICollectionViewFlowLayout`:

```csharp
var layout = new UICollectionViewFlowLayout ();
layout.SectionInset = new UIEdgeInsets (50,50,50,50);
```

Bu, aşağıda gösterildiği gibi bölümü etrafında aralığı sonuçlanır:

 [![](uicollectionview-images/06-sectioninset.png "Aşağıda gösterildiği gibi bölüm aralığı")](uicollectionview-images/06-sectioninset.png#lightbox)

 <a name="Subclassing_UICollectionViewFlowLayout" />


#### <a name="subclassing-uicollectionviewflowlayout"></a>UICollectionViewFlowLayout alt sınıf yapma

Kullanarak Edition'da `UICollectionViewFlowLayout` doğrudan, bu da bir satır boyunca içeriğinin düzenini daha fazla özelleştirmek için sınıflandırma. Örneğin, bu aşağıda gösterildiği gibi hücreleri bir kılavuza sarmalamak değil, ancak bunun yerine tek bir satır yatay kaydırma efekti ile oluşturur bir düzen oluşturmak için kullanılabilir:

 [![](uicollectionview-images/07-line-layout.png "Yatay kaydırma etkisi olan tek bir satır")](uicollectionview-images/07-line-layout.png#lightbox)

Bu sınıflara göre uygulamak için `UICollectionViewFlowLayout` gerektirir:

-  Düzene geçerli düzen özelliklerini veya oluşturucuda düzeninde tüm öğeler başlatılıyor.
-  Geçersiz kılma `ShouldInvalidateLayoutForBoundsChange` , döndürmeyi bunu true olduğunda sınırları `UICollectionView` değişiklikleri, hücrelerin düzenini yeniden hesaplanması. Bu kullanılan centermost hücreye uygulanan dönüştürme kaydırma sırasında uygulanacak için kod bu durumda emin olun.
-  Geçersiz kılma `TargetContentOffset` centermost yapmak için hücre merkezine ek `UICollectionView` durdurur kaydırma olarak.
-  Geçersiz kılma `LayoutAttributesForElementsInRect` bir dizi döndürülecek `UICollectionViewLayoutAttributes` . Her `UICollectionViewLayoutAttribute` yerleşim özellikleri gibi belirli öğesi hakkında bilgi içeren kendi `Center` , `Size` , `ZIndex` ve `Transform3D` .


Aşağıdaki kod gibi bir uygulama gösterir:

```csharp
using System;
using CoreGraphics;
using Foundation;
using UIKit;
using CoreGraphics;
using CoreAnimation;

namespace SimpleCollectionView
{
    public class LineLayout : UICollectionViewFlowLayout
    {
        public const float ITEM_SIZE = 200.0f;
        public const int ACTIVE_DISTANCE = 200;
        public const float ZOOM_FACTOR = 0.3f;

        public LineLayout ()
        {
            ItemSize = new CGSize (ITEM_SIZE, ITEM_SIZE);
            ScrollDirection = UICollectionViewScrollDirection.Horizontal;
            SectionInset = new UIEdgeInsets (400,0,400,0);
            MinimumLineSpacing = 50.0f;
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            return true;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var array = base.LayoutAttributesForElementsInRect (rect);
            var visibleRect = new CGRect (CollectionView.ContentOffset, CollectionView.Bounds.Size);

            foreach (var attributes in array) {
                if (attributes.Frame.IntersectsWith (rect)) {
                    float distance = (float)(visibleRect.GetMidX () - attributes.Center.X);
                    float normalizedDistance = distance / ACTIVE_DISTANCE;
                    if (Math.Abs (distance) < ACTIVE_DISTANCE) {
                        float zoom = 1 + ZOOM_FACTOR * (1 - Math.Abs (normalizedDistance));
                        attributes.Transform3D = CATransform3D.MakeScale (zoom, zoom, 1.0f);
                        attributes.ZIndex = 1;
                    }
                }
            }
            return array;
        }

        public override CGPoint TargetContentOffset (CGPoint proposedContentOffset, CGPoint scrollingVelocity)
        {
            float offSetAdjustment = float.MaxValue;
            float horizontalCenter = (float)(proposedContentOffset.X + (this.CollectionView.Bounds.Size.Width / 2.0));
            CGRect targetRect = new CGRect (proposedContentOffset.X, 0.0f, this.CollectionView.Bounds.Size.Width, this.CollectionView.Bounds.Size.Height);
            var array = base.LayoutAttributesForElementsInRect (targetRect);
            foreach (var layoutAttributes in array) {
                float itemHorizontalCenter = (float)layoutAttributes.Center.X;
                if (Math.Abs (itemHorizontalCenter - horizontalCenter) < Math.Abs (offSetAdjustment)) {
                    offSetAdjustment = itemHorizontalCenter - horizontalCenter;
                }
            }
            return new CGPoint (proposedContentOffset.X + offSetAdjustment, proposedContentOffset.Y);
        }

    }
}
```

 <a name="Custom_Layout" />


### <a name="custom-layout"></a>Özel düzeni

Kullanmanın yanı sıra `UICollectionViewFlowLayout`, düzenlerini de tam olarak özelleştirilebilir doğrudan devralarak `UICollectionViewLayout`.

Geçersiz kılmak için anahtar yöntemler şunlardır:

-   `PrepareLayout` – Düzeni işlemi boyunca kullanılacak ilk geometrik hesaplamaları için kullanılır.
-   `CollectionViewContentSize` – İçeriği görüntülemek için kullanılan alanın boyutu döndürür.
-   `LayoutAttributesForElementsInRect` – UICollectionViewFlowLayout örnek daha önce gösterilen bilgiler sağlamak için bu yöntem kullanıldığı şekilde `UICollectionView` nasıl Düzen ilgili her bir öğe. Ancak, farklı `UICollectionViewFlowLayout` , seçtiğiniz ancak özel bir düzen oluştururken, öğeleri yerleştirebilirsiniz.


Örneğin, aynı içerik döngüsel bir düzende aşağıda gösterildiği gibi sunulması:

 [![](uicollectionview-images/08-circle-layout.png "Döngüsel özel aşağıda gösterildiği gibi düzeni")](uicollectionview-images/08-circle-layout.png#lightbox)

Güçlü bir şey düzenleri hakkında yatay kaydırma düzene kılavuz benzeri düzeninden değiştirmek için olan ve daha sonra bu döngüsel düzeni için sağlanan düzeni sınıfı gerektirir `UICollectionView` değiştirilmiş olabilir. Hiçbir şey içinde `UICollectionView`, temsilci veya veri kaynağı kod değişiklikleri hiç.


## <a name="changes-in-ios-9"></a>İOS 9 değişiklikleri


İOS 9, koleksiyon görünümü içinde (`UICollectionView`) destekler şimdi sürükleyin kutunun dışında öğelerinin yeni bir varsayılan hareketi tanıyıcı ve birkaç yeni destek yöntemleri ekleyerek yeniden sıralama.

Bu yeni yöntemleri kullanarak, koleksiyon görünümünüzde yeniden sıralamak ve sipariş işleminin herhangi aşaması sırasında öğeleri görünümünü özelleştirme seçeneğiniz Sürükle kolayca uygulayabilirsiniz.

[![](uicollectionview-images/intro01.png "Sipariş işlem örneği")](uicollectionview-images/intro01.png#lightbox)

Bu makalede, sizi bir Xamarin.iOS uygulaması aynı zamanda bazı iOS 9 koleksiyon görünümü denetime diğer değişikliklerinin Sürükle sipariş uygulama bir göz atalım:

- [Kolay öğelerinin yeniden sıralanmasını](#Easy-Reordering-of-Items)
    - [Basit sipariş örneği](#Simple-Reordering-Example)
    - [Bir özel hareketi tanıyıcıyı](#Using-a-Custom-Gesture-Recognizer)
    - [Özel düzenler ve yeniden sıralama](#Custom-Layouts-and-Reording)
- [Koleksiyon görünümü değişiklikleri](#Collection-View-Changes)

<a name="Easy-Reordering-of-Items" />

## <a name="reordering-of-items"></a>Öğelerinin yeniden sıralanmasını

Yukarıda belirtildiği gibi iOS 9 koleksiyonu görünümünde için en önemli değişikliklerden biri kutunun dışında kolay sürükle sipariş işlevselliği eklenmesi oluştu.

İOS 9'da, bir koleksiyon görünümü yeniden sıralama eklemek için en hızlı yoludur kullanmaktır bir `UICollectionViewController`.
Koleksiyon görünümü denetleyicisi şimdi sahip bir `InstallsStandardGestureForInteractiveMovement` standart ekler özelliği *tanıyıcı hareket* destekleyen koleksiyondaki öğelerin yeniden sıralamak için sürükleyin.
Varsayılan değer olduğundan `true`, yalnızca uygulamak zorunda `MoveItem` yöntemi `UICollectionViewDataSource` sürükleme sipariş desteklemek için sınıf. Örneğin:

```csharp
public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    // Reorder our list of items
    ...
}
```
<a name="Simple-Reordering-Example" />

### <a name="simple-reordering-example"></a>Basit sipariş örneği

Hızlı bir örnek olarak, yeni bir Xamarin.iOS projesi başlatın ve düzenleme **Main.storyboard** dosya. Sürükleme bir `UICollectionViewController` tasarım yüzeyine:

[![](uicollectionview-images/quick01.png "Bir UICollectionViewController ekleme")](uicollectionview-images/quick01.png#lightbox)

(Bu dosyasından belge anahattı bunu kolay olabilir) koleksiyon görünümü seçin. Özellikler paneli düzeni sekmesinde, aşağıdaki boyutları aşağıdaki ekran görüntüsünde gösterildiği gibi ayarlayın:

- **Hücre boyutu**: genişliği – 60 | Yükseklik – 60
- **Üstbilgi boyutu**: genişliği – 0 | Yükseklik – 0
- **Alt bilgi boyutu**: genişliği – 0 | Yükseklik – 0
- **Min aralığı**: hücrelerin – 8 | Satırlar – 8
- **Bölüm iç metinleri**: üst – 16 | Alt – 16 | Sol – 16 | Sağ – 16

[![](uicollectionview-images/quick04.png "Koleksiyon görünümü boyutlarını ayarlama")](uicollectionview-images/quick04.png#lightbox)

Ardından, varsayılan hücre düzenleyin:
    - Mavi ve arka plan rengini değiştirin
    - Hücre için başlık olarak davranmak üzere etiket ekleme
    - Yeniden tanımlayıcı kümesine **hücre**

[![](uicollectionview-images/quick02.png "Varsayılan hücre düzenleme")](uicollectionview-images/quick02.png#lightbox)

Boyutu değiştikçe hücre içinde ortalanmış etiket tutmak için kısıtlamalar ekleyin:

İçinde **özelliği paneli** için _CollectionViewCell_ ve **sınıfı** için `TextCollectionViewCell`:

[![](uicollectionview-images/quick05.png "TextCollectionViewCell için set sınıfı")](uicollectionview-images/quick05.png#lightbox)

Ayarlama **koleksiyon yeniden kullanılabilir görünümü** için `Cell`:

[![](uicollectionview-images/quick06.png "Koleksiyon yeniden kullanılabilir görünüm hücreye ayarlama")](uicollectionview-images/quick06.png#lightbox)

Son olarak, etiketi seçin ve adlandırın `TextLabel`:

[![](uicollectionview-images/quick07.png "ad etiketi TextLabel")](uicollectionview-images/quick07.png#lightbox)

Düzen `TextCollectionViewCell` sınıfı ve aşağıdaki özellikleri ekleyin.:

```csharp
using System;
using Foundation;
using UIKit;

namespace CollectionView
{
    public partial class TextCollectionViewCell : UICollectionViewCell
    {
        #region Computed Properties
        public string Title {
            get { return TextLabel.Text; }
            set { TextLabel.Text = value; }
        }
        #endregion

        #region Constructors
        public TextCollectionViewCell (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Burada `Text` etiketin özelliğini koddan ayarlanabilir şekilde hücre başlığı olarak gösterilir.

Yeni bir C# sınıf projeye ekleyin ve bunu `WaterfallCollectionSource`. Dosyasını düzenleyin ve aşağıdaki gibi yapar:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionSource : UICollectionViewDataSource
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        public List<int> Numbers { get; set; } = new List<int> ();
        #endregion

        #region Constructors
        public WaterfallCollectionSource (WaterfallCollectionView collectionView)
        {
            // Initialize
            CollectionView = collectionView;

            // Init numbers collection
            for (int n = 0; n < 100; ++n) {
                Numbers.Add (n);
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView) {
            // We only have one section
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section) {
            // Return the number of items
            return Numbers.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a reusable cell and set {~~it's~>its~~} title from the item
            var cell = collectionView.DequeueReusableCell ("Cell", indexPath) as TextCollectionViewCell;
            cell.Title = Numbers [(int)indexPath.Item].ToString();

            return cell;
        }

        public override bool CanMoveItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // We can always move items
            return true;
        }

        public override void MoveItem (UICollectionView collectionView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
        {
            // Reorder our list of items
            var item = Numbers [(int)sourceIndexPath.Item];
            Numbers.RemoveAt ((int)sourceIndexPath.Item);
            Numbers.Insert ((int)destinationIndexPath.Item, item);
        }
        #endregion
    }
}
```

Bu sınıf bizim koleksiyon görünümü için veri kaynağı olabilir ve her hücre koleksiyonu için bilgileri sağlayın.
Dikkat `MoveItem` yöntemi olarak koleksiyondaki öğelerin sürükleyin yeniden düzenlenen izin vermek için uygulanır.

Başka bir yeni C# sınıf projeye ekleyin ve bunu `WaterfallCollectionDelegate`. Bu dosyayı düzenlemek ve aşağıdaki gibi yapar:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;

namespace CollectionView
{
    public class WaterfallCollectionDelegate : UICollectionViewDelegate
    {
        #region Computed Properties
        public WaterfallCollectionView CollectionView { get; set;}
        #endregion

        #region Constructors
        public WaterfallCollectionDelegate (WaterfallCollectionView collectionView)
        {

            // Initialize
            CollectionView = collectionView;

        }
        #endregion

        #region Overrides Methods
        public override bool ShouldHighlightItem (UICollectionView collectionView, NSIndexPath indexPath) {
            // Always allow for highlighting
            return true;
        }

        public override void ItemHighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and change to green background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(183,208,57);
        }

        public override void ItemUnhighlighted (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get cell and return to blue background
            var cell = collectionView.CellForItem(indexPath);
            cell.ContentView.BackgroundColor = UIColor.FromRGB(164,205,255);
        }
        #endregion
    }
}
```

Bu bizim koleksiyon görünümü temsilcisi olarak hareket eder. Kullanıcı koleksiyonu görünümünde ile etkileşime giren bir hücre vurgulamak için yöntemleri geçersiz kılınmışsa.

Bir son C# sınıfı projeye ekleyin ve bunu `WaterfallCollectionView`. Bu dosyayı düzenlemek ve aşağıdaki gibi yapar:

```csharp
using System;
using UIKit;
using System.Collections.Generic;
using Foundation;

namespace CollectionView
{
    [Register("WaterfallCollectionView")]
    public class WaterfallCollectionView : UICollectionView
    {

        #region Constructors
        public WaterfallCollectionView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Initialize
            DataSource = new WaterfallCollectionSource(this);
            Delegate = new WaterfallCollectionDelegate(this);

        }
        #endregion
    }
}
```

Dikkat `DataSource` ve `Delegate` yukarıda oluşturduğumuz koleksiyon görünümü şeridini oluşturulduğunda ayarlayın (veya **.xib** dosyası).

Düzen **Main.storyboard** dosyasını yeniden, koleksiyon görünümü seçin ve geçiş **özellikleri**. Ayarlama **sınıfı** özel `WaterfallCollectionView` biz yukarıda tanımlanan sınıfı:

UI yaptığınız değişiklikleri kaydedin ve uygulamayı çalıştırın.
Kullanıcı bir öğeyi listeden seçer ve yeni bir konuma sürüklediği göz önünden öğesi'nin taşırken, diğer öğeler otomatik olarak animasyon.
Kullanıcı yeni bir konumda öğesi düştüğünde, bu konuma bağlı kalın. Örneğin:

[![](uicollectionview-images/intro01.png "Yeni bir konuma öğe sürükleme örneği")](uicollectionview-images/intro01.png#lightbox)

<a name="Using-a-Custom-Gesture-Recognizer" />

### <a name="using-a-custom-gesture-recognizer"></a>Bir özel hareketi tanıyıcıyı

Burada kullanamazsınız durumlarda bir `UICollectionViewController` ve bir normal kullanmalıdır `UIViewController`, ya da sürükle ve bırak hareketi üzerinde daha fazla denetim almak istiyorsanız, kendi özel hareketi tanıyıcınızı oluşturun ve görünüm yüklenirken koleksiyon görünümüne ekleyin. Örneğin:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Create a custom gesture recognizer
    var longPressGesture = new UILongPressGestureRecognizer ((gesture) => {

        // Take action based on state
        switch(gesture.State) {
        case UIGestureRecognizerState.Began:
            var selectedIndexPath = CollectionView.IndexPathForItemAtPoint(gesture.LocationInView(View));
            if (selectedIndexPath !=null) {
                CollectionView.BeginInteractiveMovementForItem(selectedIndexPath);
            }
            break;
        case UIGestureRecognizerState.Changed:
            CollectionView.UpdateInteractiveMovementTargetPosition(gesture.LocationInView(View));
            break;
        case UIGestureRecognizerState.Ended:
            CollectionView.EndInteractiveMovement();
            break;
        default:
            CollectionView.CancelInteractiveMovement();
            break;
        }

    });

    // Add the custom recognizer to the collection view
    CollectionView.AddGestureRecognizer(longPressGesture);
}
```

Koleksiyon görünümü eklenen birkaç yeni yöntemleri uygulamak ve sürükleme işlemi denetlemek için buraya kullanıyoruz:

 - `BeginInteractiveMovementForItem` -Bir taşıma işlemi başlangıcını işaretler.
 - `UpdateInteractiveMovementTargetPosition` -Öğenin konumu güncelleştirildikçe gönderilir.
 - `EndInteractiveMovement` -Bir öğe taşıma sonunu işaretler.
 - `CancelInteractiveMovement` -Taşıma işlemi iptal ediliyor kullanıcı işaretler.

Uygulamayı çalıştırdığınızda, tam olarak varsayılan sürükleyin gibi koleksiyon görünümü ile birlikte gelen hareketi tanıyıcı sürükleme işlemi çalışır.

<a name="Custom-Layouts-and-Reording" />

### <a name="custom-layouts-and-reordering"></a>Özel düzenler ve yeniden sıralama

İOS 9'da, sürükle sipariş ve özel düzenler koleksiyon görünümü içinde çalışmak için birkaç yeni yöntemler eklenmiştir. Bu özellik keşfetmek için özel bir düzen koleksiyona ekleyelim.

İlk olarak, adlı yeni bir C# sınıf ekleyin `WaterfallCollectionLayout` projeye. Düzenleyin ve aşağıdaki gibi yapar:

```csharp
using System;
using Foundation;
using UIKit;
using System.Collections.Generic;
using CoreGraphics;

namespace CollectionView
{
    [Register("WaterfallCollectionLayout")]
    public class WaterfallCollectionLayout : UICollectionViewLayout
    {
        #region Private Variables
        private int columnCount = 2;
        private nfloat minimumColumnSpacing = 10;
        private nfloat minimumInterItemSpacing = 10;
        private nfloat headerHeight = 0.0f;
        private nfloat footerHeight = 0.0f;
        private UIEdgeInsets sectionInset = new UIEdgeInsets(0, 0, 0, 0);
        private WaterfallCollectionRenderDirection itemRenderDirection = WaterfallCollectionRenderDirection.ShortestFirst;
        private Dictionary<nint,UICollectionViewLayoutAttributes> headersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private Dictionary<nint,UICollectionViewLayoutAttributes> footersAttributes = new Dictionary<nint, UICollectionViewLayoutAttributes>();
        private List<CGRect> unionRects = new List<CGRect>();
        private List<nfloat> columnHeights = new List<nfloat>();
        private List<UICollectionViewLayoutAttributes> allItemAttributes = new List<UICollectionViewLayoutAttributes>();
        private List<List<UICollectionViewLayoutAttributes>> sectionItemAttributes = new List<List<UICollectionViewLayoutAttributes>>();
        private nfloat unionSize = 20;
        #endregion

        #region Computed Properties
        [Export("ColumnCount")]
        public int ColumnCount {
            get { return columnCount; }
            set {
                WillChangeValue ("ColumnCount");
                columnCount = value;
                DidChangeValue ("ColumnCount");

                InvalidateLayout ();
            }
        }

        [Export("MinimumColumnSpacing")]
        public nfloat MinimumColumnSpacing {
            get { return minimumColumnSpacing; }
            set {
                WillChangeValue ("MinimumColumnSpacing");
                minimumColumnSpacing = value;
                DidChangeValue ("MinimumColumnSpacing");

                InvalidateLayout ();
            }
        }

        [Export("MinimumInterItemSpacing")]
        public nfloat MinimumInterItemSpacing {
            get { return minimumInterItemSpacing; }
            set {
                WillChangeValue ("MinimumInterItemSpacing");
                minimumInterItemSpacing = value;
                DidChangeValue ("MinimumInterItemSpacing");

                InvalidateLayout ();
            }
        }

        [Export("HeaderHeight")]
        public nfloat HeaderHeight {
            get { return headerHeight; }
            set {
                WillChangeValue ("HeaderHeight");
                headerHeight = value;
                DidChangeValue ("HeaderHeight");

                InvalidateLayout ();
            }
        }

        [Export("FooterHeight")]
        public nfloat FooterHeight {
            get { return footerHeight; }
            set {
                WillChangeValue ("FooterHeight");
                footerHeight = value;
                DidChangeValue ("FooterHeight");

                InvalidateLayout ();
            }
        }

        [Export("SectionInset")]
        public UIEdgeInsets SectionInset {
            get { return sectionInset; }
            set {
                WillChangeValue ("SectionInset");
                sectionInset = value;
                DidChangeValue ("SectionInset");

                InvalidateLayout ();
            }
        }

        [Export("ItemRenderDirection")]
        public WaterfallCollectionRenderDirection ItemRenderDirection {
            get { return itemRenderDirection; }
            set {
                WillChangeValue ("ItemRenderDirection");
                itemRenderDirection = value;
                DidChangeValue ("ItemRenderDirection");

                InvalidateLayout ();
            }
        }
        #endregion

        #region Constructors
        public WaterfallCollectionLayout ()
        {
        }

        public WaterfallCollectionLayout(NSCoder coder) : base(coder) {

        }
        #endregion

        #region Public Methods
        public nfloat ItemWidthInSectionAtIndex(int section) {

            var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
            return (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);
        }
        #endregion

        #region Override Methods
        public override void PrepareLayout ()
        {
            base.PrepareLayout ();

            // Get the number of sections
            var numberofSections = CollectionView.NumberOfSections();
            if (numberofSections == 0)
                return;

            // Reset collections
            headersAttributes.Clear ();
            footersAttributes.Clear ();
            unionRects.Clear ();
            columnHeights.Clear ();
            allItemAttributes.Clear ();
            sectionItemAttributes.Clear ();

            // Initialize column heights
            for (int n = 0; n < ColumnCount; n++) {
                columnHeights.Add ((nfloat)0);
            }

            // Process all sections
            nfloat top = 0.0f;
            var attributes = new UICollectionViewLayoutAttributes ();
            var columnIndex = 0;
            for (nint section = 0; section < numberofSections; ++section) {
                // Calculate section specific metrics
                var minimumInterItemSpacing = (MinimumInterItemSpacingForSection == null) ? MinimumColumnSpacing :
                    MinimumInterItemSpacingForSection (CollectionView, this, section);

                // Calculate widths
                var width = CollectionView.Bounds.Width - SectionInset.Left - SectionInset.Right;
                var itemWidth = (nfloat)Math.Floor ((width - ((ColumnCount - 1) * MinimumColumnSpacing)) / ColumnCount);

                // Calculate section header
                var heightHeader = (HeightForHeader == null) ? HeaderHeight :
                    HeightForHeader (CollectionView, this, section);

                if (heightHeader > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Header, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, heightHeader);
                    headersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);

                    top = attributes.Frame.GetMaxY ();
                }

                top += SectionInset.Top;
                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }

                // Calculate Section Items
                var itemCount = CollectionView.NumberOfItemsInSection(section);
                List<UICollectionViewLayoutAttributes> itemAttributes = new List<UICollectionViewLayoutAttributes> ();

                for (nint n = 0; n < itemCount; n++) {
                    var indexPath = NSIndexPath.FromRowSection (n, section);
                    columnIndex = NextColumnIndexForItem (n);
                    var xOffset = SectionInset.Left + (itemWidth + MinimumColumnSpacing) * (nfloat)columnIndex;
                    var yOffset = columnHeights [columnIndex];
                    var itemSize = (SizeForItem == null) ? new CGSize (0, 0) : SizeForItem (CollectionView, this, indexPath);
                    nfloat itemHeight = 0.0f;

                    if (itemSize.Height > 0.0f && itemSize.Width > 0.0f) {
                        itemHeight = (nfloat)Math.Floor (itemSize.Height * itemWidth / itemSize.Width);
                    }

                    attributes = UICollectionViewLayoutAttributes.CreateForCell (indexPath);
                    attributes.Frame = new CGRect (xOffset, yOffset, itemWidth, itemHeight);
                    itemAttributes.Add (attributes);
                    allItemAttributes.Add (attributes);
                    columnHeights [columnIndex] = attributes.Frame.GetMaxY () + MinimumInterItemSpacing;
                }
                sectionItemAttributes.Add (itemAttributes);

                // Calculate Section Footer
                nfloat footerHeight = 0.0f;
                columnIndex = LongestColumnIndex();
                top = columnHeights [columnIndex] - MinimumInterItemSpacing + SectionInset.Bottom;
                footerHeight = (HeightForFooter == null) ? FooterHeight : HeightForFooter(CollectionView, this, section);

                if (footerHeight > 0) {
                    attributes = UICollectionViewLayoutAttributes.CreateForSupplementaryView (UICollectionElementKindSection.Footer, NSIndexPath.FromRowSection (0, section));
                    attributes.Frame = new CGRect (0, top, CollectionView.Bounds.Width, footerHeight);
                    footersAttributes.Add (section, attributes);
                    allItemAttributes.Add (attributes);
                    top = attributes.Frame.GetMaxY ();
                }

                for (int n = 0; n < ColumnCount; n++) {
                    columnHeights [n] = top;
                }
            }

            var i =0;
            var attrs = allItemAttributes.Count;
            while(i < attrs) {
                var rect1 = allItemAttributes [i].Frame;
                i = (int)Math.Min (i + unionSize, attrs) - 1;
                var rect2 = allItemAttributes [i].Frame;
                unionRects.Add (CGRect.Union (rect1, rect2));
                i++;
            }

        }

        public override CGSize CollectionViewContentSize {
            get {
                if (CollectionView.NumberOfSections () == 0) {
                    return new CGSize (0, 0);
                }

                var contentSize = CollectionView.Bounds.Size;
                contentSize.Height = columnHeights [0];
                return contentSize;
            }
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForItem (NSIndexPath indexPath)
        {
            if (indexPath.Section >= sectionItemAttributes.Count) {
                return null;
            }

            if (indexPath.Item >= sectionItemAttributes [indexPath.Section].Count) {
                return null;
            }

            var list = sectionItemAttributes [indexPath.Section];
            return list [(int)indexPath.Item];
        }

        public override UICollectionViewLayoutAttributes LayoutAttributesForSupplementaryView (NSString kind, NSIndexPath indexPath)
        {
            var attributes = new UICollectionViewLayoutAttributes ();

            switch (kind) {
            case "header":
                attributes = headersAttributes [indexPath.Section];
                break;
            case "footer":
                attributes = footersAttributes [indexPath.Section];
                break;
            }

            return attributes;
        }

        public override UICollectionViewLayoutAttributes[] LayoutAttributesForElementsInRect (CGRect rect)
        {
            var begin = 0;
            var end = unionRects.Count;
            List<UICollectionViewLayoutAttributes> attrs = new List<UICollectionViewLayoutAttributes> ();


            for (int i = 0; i < end; i++) {
                if (rect.IntersectsWith(unionRects[i])) {
                    begin = i * (int)unionSize;
                }
            }

            for (int i = end - 1; i >= 0; i--) {
                if (rect.IntersectsWith (unionRects [i])) {
                    end = (int)Math.Min ((i + 1) * (int)unionSize, allItemAttributes.Count);
                    break;
                }
            }

            for (int i = begin; i < end; i++) {
                var attr = allItemAttributes [i];
                if (rect.IntersectsWith (attr.Frame)) {
                    attrs.Add (attr);
                }
            }

            return attrs.ToArray();
        }

        public override bool ShouldInvalidateLayoutForBoundsChange (CGRect newBounds)
        {
            var oldBounds = CollectionView.Bounds;
            return (newBounds.Width != oldBounds.Width);
        }
        #endregion

        #region Private Methods
        private int ShortestColumnIndex() {
            var index = 0;
            var shortestHeight = nfloat.MaxValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height < shortestHeight) {
                    shortestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int LongestColumnIndex() {
            var index = 0;
            var longestHeight = nfloat.MinValue;
            var n = 0;

            // Scan each column for the shortest height
            foreach (nfloat height in columnHeights) {
                if (height > longestHeight) {
                    longestHeight = height;
                    index = n;
                }
                ++n;
            }

            return index;
        }

        private int NextColumnIndexForItem(nint item) {
            var index = 0;

            switch (ItemRenderDirection) {
            case WaterfallCollectionRenderDirection.ShortestFirst:
                index = ShortestColumnIndex ();
                break;
            case WaterfallCollectionRenderDirection.LeftToRight:
                index = ColumnCount;
                break;
            case WaterfallCollectionRenderDirection.RightToLeft:
                index = (ColumnCount - 1) - ((int)item / ColumnCount);
                break;
            }

            return index;
        }
        #endregion

        #region Events
        public delegate CGSize WaterfallCollectionSizeDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, NSIndexPath indexPath);
        public delegate nfloat WaterfallCollectionFloatDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);
        public delegate UIEdgeInsets WaterfallCollectionEdgeInsetsDelegate(UICollectionView collectionView, WaterfallCollectionLayout layout, nint section);

        public event WaterfallCollectionSizeDelegate SizeForItem;
        public event WaterfallCollectionFloatDelegate HeightForHeader;
        public event WaterfallCollectionFloatDelegate HeightForFooter;
        public event WaterfallCollectionEdgeInsetsDelegate InsetForSection;
        public event WaterfallCollectionFloatDelegate MinimumInterItemSpacingForSection;
        #endregion
    }
}
```

Bu sınıf özel iki sütun, koleksiyon görünümü waterfall türü düzeni sağlamak için kullanılır.
Anahtar-değer kodlama kod kullanır (aracılığıyla `WillChangeValue` ve `DidChangeValue` yöntemleri) Bu sınıftaki hesaplanan bizim özellikler veri bağlama sağlanacak.

Ardından, düzenleme `WaterfallCollectionSource` ve aşağıdaki değişiklikler ve eklemeler yapın:

```csharp
private Random rnd = new Random();
...

public List<nfloat> Heights { get; set; } = new List<nfloat> ();
...

public WaterfallCollectionSource (WaterfallCollectionView collectionView)
{
    // Initialize
    CollectionView = collectionView;

    // Init numbers collection
    for (int n = 0; n < 100; ++n) {
        Numbers.Add (n);
        Heights.Add (rnd.Next (0, 100) + 40.0f);
    }
}
```

Liste görünümünde görüntülenen öğelerin her biri için bu rastgele yüksekliği oluşturur.

Ardından, düzenleme `WaterfallCollectionView` sınıfı ve aşağıdaki Yardımcısı özelliği ekleyin:

```csharp
public WaterfallCollectionSource Source {
    get { return (WaterfallCollectionSource)DataSource; }
}
```

Bu, veri kaynağı (ve madde yükseklikte) özel düzeninden daha kolay hale getirir.

Son olarak, görünüm denetleyicisini düzenleyin ve aşağıdaki kodu ekleyin:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    var waterfallLayout = new WaterfallCollectionLayout ();

    // Wireup events
    waterfallLayout.SizeForItem += (collectionView, layout, indexPath) => {
        var collection = collectionView as WaterfallCollectionView;
        return new CGSize((View.Bounds.Width-40)/3,collection.Source.Heights[(int)indexPath.Item]);
    };

    // Attach the custom layout to the collection
    CollectionView.SetCollectionViewLayout(waterfallLayout, false);
}
```

Bu bizim Özel düzen örneği oluşturur, her öğenin boyutunu sağlamak için olay ayarlar ve yeni düzene bizim koleksiyonu görünümüne ekler.

Biz Xamarin.iOS uygulaması tekrar çalıştırırsanız koleksiyon görünümü şimdi aşağıdaki gibi görünür:

[![](uicollectionview-images/custom01.png "Koleksiyon görünümü şimdi şöyle görünür")](uicollectionview-images/custom01.png#lightbox)

Biz önce Sürükle sipariş hala öğeleri olarak edebilirsiniz, ancak öğeleri şimdi bunların bırakıldığında yeni konumlarına uyacak şekilde boyutu değiştireceksiniz.

## <a name="collection-view-changes"></a>Koleksiyon görünümü değişiklikleri

Aşağıdaki bölümlerde, biz ayrıntılı koleksiyon görünümü her sınıf için iOS 9 tarafından yapılan değişiklikleri göz atın.

### <a name="uicollectionview"></a>UICollectionView

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionView` sınıfı iOS 9 için:

 - `BeginInteractiveMovementForItem` – Bir sürükleme işlemi başlangıcını işaretler.
 - `CancelInteractiveMovement` – Koleksiyonu bildirir görünümü kullanıcı bir sürükleme işlemi iptal etti.
 - `EndInteractiveMovement` – Koleksiyonu bildirir görünümü kullanıcı bir sürükleme işlemi tamamladı.
 - `GetIndexPathsForVisibleSupplementaryElements` – Döndürür `indexPath` üstbilgi ya da bir koleksiyon görünümü bölümündeki altbilgi.
 - `GetSupplementaryView` – Verilen üstbilgisinde veya altbilgisinde döndürür.
 - `GetVisibleSupplementaryViews` – Tüm görünür üstbilgi ve altbilgi listesini döndürür.
 - `UpdateInteractiveMovementTargetPosition` – Koleksiyonu bildiren kullanıcı taşınmış veya taşınması, bir sürükleme işlemi sırasında bir öğe görünümü.

### <a name="uicollectionviewcontroller"></a>UICollectionViewController

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewController` iOS 9 sınıfında:

 - `InstallsStandardGestureForInteractiveMovement` – IF `true` otomatik olarak sürükleme sipariş destekleyen yeni hareketi tanıyıcı kullanılır.
 - `CanMoveItem` – Belirli bir öğe kaldırılmasında Sürükle olabiliyorsa, koleksiyon görünümü bildirir.
 - `GetTargetContentOffset` – Belirli koleksiyon görünümü öğesi uzaklığını almak için kullanılır.
 - `GetTargetIndexPathForMove` – Alır `indexPath` belirli bir öğenin bir sürükleme işlemi için.
 - `MoveItem` – Belirli bir öğe sırasını listede taşır.


### <a name="uicollectionviewdatasource"></a>UICollectionViewDataSource

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewDataSource` iOS 9 sınıfında:

 - `CanMoveItem` – Belirli bir öğe kaldırılmasında Sürükle olabiliyorsa, koleksiyon görünümü bildirir.
 - `MoveItem` – Belirli bir öğe sırasını listede taşır.

### <a name="uicollectionviewdelegate"></a>UICollectionViewDelegate

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewDelegate` iOS 9 sınıfında:

 - `GetTargetContentOffset` – Belirli koleksiyon görünümü öğesi uzaklığını almak için kullanılır.
 - `GetTargetIndexPathForMove` – Alır `indexPath` belirli bir öğenin bir sürükleme işlemi için.

### <a name="uicollectionviewflowlayout"></a>UICollectionViewFlowLayout

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewFlowLayout` iOS 9 sınıfında:

 - `SectionFootersPinToVisibleBounds` – Görünür koleksiyon görünümü sınırları bölüm altbilgi sticks.
 - `SectionHeadersPinToVisibleBounds` – Görünür koleksiyon görünümü sınırları için bölüm üstbilgileri sticks.

### <a name="uicollectionviewlayout"></a>UICollectionViewLayout

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewLayout` iOS 9 sınıfında:

 - `GetInvalidationContextForEndingInteractiveMovementOfItems` – Kullanıcı Sürükle bittikten ya da iptal eder sürükleme işlemi sonunda geçersiz kılma bağlamını döndürür.
 - `GetInvalidationContextForInteractivelyMovingItems` – Bir sürükleme işlemi başlangıcında geçersiz kılma bağlamını döndürür.
 - `GetLayoutAttributesForInteractivelyMovingItem` – Yerleşim özniteliklerini belirli bir öğe için bir öğe sürükleme sırasında alır.
 - `GetTargetIndexPathForInteractivelyMovingItem` – Döndürür `indexPath` öğesinin belirli bir anda bir öğe sürüklendiğinde.

### <a name="uicollectionviewlayoutattributes"></a>UICollectionViewLayoutAttributes

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewLayoutAttributes` iOS 9 sınıfında:

 - `CollisionBoundingPath` – Bir sürükleme işlemi sırasında iki öğe çakışma yolunu döndürür.
 - `CollisionBoundsType` – Çakışma türünü döndürür (olarak bir `UIDynamicItemCollisionBoundsType`) bir sürükleme işlemi sırasında oluştu.

### <a name="uicollectionviewlayoutinvalidationcontext"></a>UICollectionViewLayoutInvalidationContext

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewLayoutInvalidationContext` iOS 9 sınıfında:

 - `InteractiveMovementTarget` – Sürükleme işleminin hedef öğeyi döndürür.
 - `PreviousIndexPathsForInteractivelyMovingItems` – Döndürür `indexPaths` diğer öğeler söz konusu işlemi yeniden sıralamak için sürükleyin.
 - `TargetIndexPathsForInteractivelyMovingItems` – Döndürür `indexPaths` sürükleme yeniden sıralama işlemi sonucunda kaldırılmasında öğelerinin.

### <a name="uicollectionviewsource"></a>UICollectionViewSource

Aşağıdaki değişiklikler veya eklemeler yapılmıştır `UICollectionViewSource` iOS 9 sınıfında:

 - `CanMoveItem` – Belirli bir öğe kaldırılmasında Sürükle olabiliyorsa, koleksiyon görünümü bildirir.
 - `GetTargetContentOffset` – Bir Sürükle yeniden sıralama işlemi taşınır öğeleri uzaklıklarını döndürür.
 - `GetTargetIndexPathForMove` – Döndürür `indexPath` öğenin Sürükle yeniden sıralama işlemi sırasında taşınır.
 - `MoveItem` – Belirli bir öğe sırasını listede taşır.

## <a name="summary"></a>Özet

Bu makalede, iOS 9 koleksiyonu görünümlerde değişiklikleri ele ve nasıl Xamarin.iOS içinde uygulanacağı açıklanmaktadır.
Bu basit bir Sürükle sipariş eylemi bir koleksiyon görünümü uygulama kapsamında; bir özel hareketi Sürükle sipariş ile tanıyıcıyı; ve özel koleksiyon görünümü düzeni Sürükle sipariş nasıl etkiler.

## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 örnekleri](https://developer.xamarin.com/samples/ios/iOS9/)
- [Koleksiyon görünümü örneği](https://developer.xamarin.com/samples/monotouch/ios9/CollectionView/)
- [SimpleCollectionView (örnek)](https://developer.xamarin.com/samples/SimpleCollectionView/)
- [Olaylar, Protokoller ve Temsilciler](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Tabloları ve hücreleri ile çalışma](~/ios/user-interface/controls/tables/index.md)
