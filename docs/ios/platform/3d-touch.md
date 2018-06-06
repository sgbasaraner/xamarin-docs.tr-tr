---
title: Xamarin.iOS 3B iletişim giriş
description: Bu makalede iPhone 6s ve iPhone 6s ile sunulan 3B dokunma hareketleri kullanmayı açıklar ve. Bu hareketleri baskısı duyarlılık, gözlem ve pop etkinleştir ve hızlı eylemler.
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f6eb71409317661cdd571c708db062e06e63ff55
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786589"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Xamarin.iOS 3B iletişim giriş

_Bu makale yeni kullanarak kapsamaktadır iPhone 6s ve iPhone 6s Plus 3B Touch uygulamanızda hareketleri._

[![](3d-touch-images/info01.png "3B dokunma örnekleri etkin uygulamalar")](3d-touch-images/info01.png#lightbox)

Bu makalede sağlar ve Xamarin.iOS uygulamalarınızı üzerinde yeni iPhone 6s ve iPhone 6s çalıştıran baskısı hassas hareketleri eklemek için yeni 3B Touch API'lerini kullanarak giriş cihazları artı.

3B dokunarak bir iPhone uygulamayı yalnızca kullanıcının cihazın ekran temas, ancak kullanıcı exerting ne kadar baskısı mantıklı yapabiliyor söyleyin ve farklı baskısı düzeylere yanıt sunulmuştur.

3B Touch uygulamanız için aşağıdaki özellikleri sağlar:

- [Baskı duyarlılık](#Pressure-Sensitivity) - uygulamalar şimdi ölçmek nasıl sabit veya ışık kullanıcı bu bilgileri ekran ve Al avantajlarından temas.
  Örneğin, bir boyama uygulama kalın veya kullanıcının ekran nasıl sabit temas thinner göre çizgi yapabilir.
- [Gözat ve Pop](#Peek-and-Pop) -uygulamanız, geçerli bağlam dışında gitmek zorunda kalmadan kendi verilerle etkileşimli kullanıcı artık olanak tanır. Ekran ekranı sabit basarak bunlar (bir ileti Önizleme gibi) ilginizi öğesindeki iletiye göz atabilirsiniz. Daha zor basarak bunlar öğesine pop.
- [Hızlı Eylemler](#Quick-Actions) -düşünüyorsanız, hızlı gibi eylemleri, kullanıcı bir masaüstü uygulamasının bir öğeyi tıklattığında açılan bağlamsal menüleri.
  Hızlı eylemleri kullanarak, kısayollar işlevleri için uygulamanızda doğrudan giriş ekranında uygulama simgesinden ekleyebilirsiniz.
- [3B dokunma Benzeticisinde sınama](#Testing-3D-Touch-in-the-Simulator) -doğru Mac donanım ile iOS Simulator'da 3B dokunma etkinleştirilmiş uygulamaları test edebilirsiniz.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Baskısı duyarlılık

Yeni özelliklerini kullanarak, yukarıda belirtildiği gibi [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) sınıf kullanıcı iOS cihazın ekranına uygulama baskısı miktarını ölçmek ve kullanıcı arabiriminde bu bilgileri kullanın. Örneğin, bir fırça vuruş daha saydam veya donuk baskısı miktarına göre yapılıyor.

[![](3d-touch-images/pressure01.png "Olarak daha saydam veya donuk çizilir fırça vuruş baskısı miktarına göre")](3d-touch-images/pressure01.png#lightbox)

3B dokunma sonucu olarak, uygulamanızı iOS 9 (veya daha büyük) çalışıyor ve iOS cihazı destekleyen 3B dokunarak özellikli baskısı değişiklikleri neden olur `TouchesMoved` çıkarılmasına olay.

Örneğin, izlerken `TouchesMoved` olayı bir [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/), kullanıcı ekrana uygulama geçerli baskısı almak için aşağıdaki kodu kullanabilirsiniz:

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

`MaximumPossibleForce` Özelliği için olası en yüksek değere döndürür `Force` özelliği [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/) uygulamayı çalıştıran iOS cihazı bağlı.

> [!IMPORTANT]
> Baskısı değişiklikleri neden olacak `TouchesMoved` oluşturulması için olay olsa bile X / Y koordinatları değişmemiştir. Bu davranış değişikliği nedeniyle, iOS uygulamaları için hazırlıklı olmalıdır `TouchesMoved` daha sık ve X için çağrılacak olay / Y koordinatları son aynı olacak şekilde `TouchesMoved` çağırın.




Daha fazla bilgi için lütfen Apple'nın bkz [TouchCanvas: UITouch etkili ve verimli şekilde kullanarak](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) örnek uygulama ve [UITouch sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Peek ve Pop

3B Touch uygulamanızı zamankinden daha hızlı içinde bilgilerle geçerli konumlarından gitmek zorunda kalmadan etkileşim kurmak bir kullanıcı için yeni yollar sağlar.

Uygulamanızı bir tablo iletilerin görüntüleme, örneğin, kullanıcı içeriği bir katmana görünümünde önizleme için bir öğe üzerinde sabit basabilirsiniz (hangi Apple olarak başvurduğu bir *Peek*).

[![](3d-touch-images/peekandpop01.png "İçerik adresindeki gözatma örneği")](3d-touch-images/peekandpop01.png#lightbox)

Kullanıcı daha zor basarsa normal ileti görünümü girer (hangi olarak adlandırılır *Pop*-ping görünümüne).

### <a name="checking-for-3d-touch-availability"></a>3B dokunma için kullanılabilirlik denetleniyor

İle çalışırken bir [UIViewController]() uygulama üzerinde çalışan iOS cihazı 3B dokunma destekleyip desteklemediğini görmek için aşağıdaki kodu kullanabilirsiniz:

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

Bu yöntem, önce çağrılabilir *veya sonra* `ViewDidLoad()`. 

### <a name="handling-peek-and-pop"></a>İşleme gözlem ve Pop

3B dokunma işleyebilecek bir iOS cihazında biz örneği kullanabilirsiniz `UIViewControllerPreviewingDelegate` görüntüsünü işlemek için sınıf **Peek** ve **Pop** öğe ayrıntıları. Örneğin, bir tablo görünümü denetleyicisi biz olsaydı adlı `MasterViewController ` biz desteklemek için aşağıdaki kodu kullanabilirsiniz **Peek** ve **Pop**:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

`GetViewControllerForPreview` Yöntemi gerçekleştirmek için kullanılan **Peek** işlemi. Tablo hücre ve yedekleme verilerine erişim kazanır ve ardından yükler `DetailViewController` geçerli film şeridi gelen. Ayarlayarak `PreferredContentSize` (0,0) sizden için varsayılan istiyoruz **Peek** görüntüleme boyutu. Son olarak, biz biz görüntüleme ile hücre dışındaki her şeyi ölçeklendirilmelidir `previewingContext.SourceRect = cell.Frame` ve görüntü için yeni bir görünümü döndürürüz.

`CommitViewController` Biz oluşturulan görünüm yeniden kullanır **Peek** için **Pop** kullanıcı zor bastığında görüntüleyin.

### <a name="registering-for-peek-and-pop"></a>Peek ve Pop kaydediliyor

Biz kullanıcıya izin vermek istediğiniz görünümü denetleyicisinden **Peek** ve **Pop** öğelerinden, ihtiyacımız için bu hizmeti kaydetmek. Bir tablo görünümü denetleyicisini yukarıda verilen örnekte (`MasterViewController`), aşağıdaki kodu kullanırız:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

Çağırma burada biz `RegisterForPreviewingWithDelegate` örneği yöntemiyle `PreviewingDelegate` yukarıda oluşturduğumuz. 3B Touch destekleyen iOS cihazlarda kullanıcı onu adresindeki gözlem için bir öğede sabit tuşlarına basabilirsiniz. Bunlar çok daha tuşuna basarsanız, öğe standart, Pop görünümü görüntüleyin.

Daha fazla bilgi için lütfen bkz bizim [iOS 9 ApplicationShortcuts örnek](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) ve Apple'nın [ViewControllerPreviews: API'leri önizlemesini UIViewController kullanarak](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) örnek uygulaması [ UIPreviewAction sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), [UIPreviewActionGroup sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) ve [UIPreviewActionItem Protokolü başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Hızlı Eylemler

3B dokunma ve hızlı Eylemler kullanarak, ortak, hızlı ve kolay erişim kısayolların işlevlere uygulamanızda iOS cihazında giriş ekranı simgesinden ekleyebilirsiniz.

Yukarıda belirtildiği gibi bir kullanıcı bir masaüstü uygulamasının bir öğeyi tıklattığında açılan bağlamsal menüler gibi hızlı eylemlerini düşünebilirsiniz. Hızlı Eylemler en yaygın işlevler veya uygulamanız özelliklerini kısayollar sağlamak için kullanmalısınız.

[![](3d-touch-images/quickactions01.png "Hızlı Eylemler menüsü örneği")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>Statik hızlı eylemleri tanımlama

Bir veya daha hızlı, uygulamanız tarafından gerekli eylemleri statik ve değişiklik gerekmez, bunları uygulamanın içinde tanımlayabilirsiniz `Info.plist` dosya. Harici bir düzenleyicide bu dosyasını düzenleyin ve aşağıdaki anahtarları ekleyin:

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

Burada biz iki statik hızlı eylem öğeleri aşağıdaki anahtarları tanımlama:

- `UIApplicationShortcutItemIconType` -Hızlı eylem öğesi tarafından aşağıdaki değerlerden biri olarak görüntülenecek simgesi tanımlar:
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle` -Öğenin altyazısı tanımlar.
* `UIApplicationShortcutItemTitle` -Öğenin başlığını tanımlar.
* `UIApplicationShortcutItemType` -Uygulamamıza öğesinde tanımlamak için kullanacağı bir dize değeridir. Daha fazla bilgi için aşağıdaki bölüme bakın.

> [!IMPORTANT]
> Ayarlanmış olan Hızlı kısayol kararları `Info.plist` dosya ile erişilemiyor `Application.ShortcutItems` özelliği. Bunlar yalnızca için iletilen `HandleShortcutItem` olay işleyicisi. 





### <a name="identifying-quick-action-items"></a>Hızlı eylem öğelerini tanımlama

Yukarıda gördüğünüz gibi ne zaman tanımladığınız hızlı Eylem öğeleriniz, uygulamanızın içinde `Info.plist`, bir dize değerine atanmış `UIApplicationShortcutItemType` bunları tanımlamak için anahtarı.

Kod içinde çalışmak bu tanımlayıcıları kolaylaştırmak için adlı bir sınıf ekleyin `ShortcutIdentifier` uygulamanızın için proje ve şu şekilde görünür yapın:

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>Hızlı bir eylem işleme

Ardından, uygulamanızın değiştirmenize gerek `AppDelegate.cs` giriş ekranında, uygulamanızın simgesinden hızlı eylem öğesini seçerek kullanıcı işlemek için dosya.

Aşağıdaki düzenlemelerini yapın:

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

İlk olarak, bir ortak tanımlarız `LaunchedShortcutItem` kullanıcı tarafından son seçilen hızlı eylem öğesi izlemek için özellik. Ardından, biz geçersiz kılma `FinishedLaunching` yöntemi ve bakın, `launchOptions` geçirildi ve bunlar içeriyorsa, hızlı eylem öğesi. İçermiyorlarsa hızlı eylemde depolarız `LaunchedShortcutItem` özelliği.

Ardından, biz geçersiz kılma `OnActivated` yöntemi ve Hızlı Başlat öğesine seçili geçişi `HandleShortcutItem` üzerinde işlem yapılması yöntemi. Şu anda biz yalnızca bir iletiye yazıyorsanız **konsol**. Gerçek bir uygulamada what ever eylem gerekli işlemesi. Eylem alındıktan sonra `LaunchedShortcutItem` özelliği temizlenir.

Son olarak, uygulamanızı zaten çalışıyorsa, `PerformActionForShortcutItem` yöntemi geçersiz kılmak ve çağırmak ihtiyacımız şekilde hızlı eylem öğesini işlemek için çağrılabilir bizim `HandleShortcutItem` yöntemi de burada.


### <a name="creating-dynamic-quick-action-items"></a>Dinamik hızlı eylem öğeleri oluşturma

Statik hızlı eylemi tanımlama ek olarak, uygulamanızın içinde öğelerini `Info.plist` dosyası, dinamik üzerinde-çalışma sırasında hızlı eylemler oluşturabilirsiniz. İki yeni dinamik hızlı eylemleri tanımlamak için düzenleme, `AppDelegate.cs` dosyasını yeniden ve değiştirme `FinishedLaunching` aramak için yöntemi aşağıdaki gibi:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

Biz, denetleniyor artık `application` zaten dinamik olarak oluşturulmuş bir dizi içeriyor `ShortcutItems`, içermiyorsa iki yeni oluşturacağız `UIMutableApplicationShortcutItem` yeni öğeleri tanımlamak ve bunlara eklemek için nesneleri `ShortcutItems` dizi.

Kodu önceden içinde eklediğimiz [hızlı bir eylem işleme](#Handling-a-Quick-Action) yukarıdaki bölümde bu dinamik hızlı Eylemler statik olanlar gibi işleyecek.

Bunu unutulmamalıdır (ki burada yapmakta olduğunuz gibi) statik ve dinamik hızlı eylem öğeleri bileşimi oluşturabilir, birini veya diğerini sınırlı değildir.

Daha fazla bilgi için lütfen bizim [iOS 9 ViewControllerPreview örnek](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/) ve Apple'nın [ApplicationShortcuts: kullanarak UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) örnek uygulaması [ UIApplicationShortcutItem sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), [UIMutableApplicationShortcutItem sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) ve [UIApplicationShortcutIcon sınıf başvurusu](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>3B dokunma Benzeticisinde test etme

İzleme paneli zorla Touch ile uyumlu Mac'te Xcode ve iOS simülatörü'nü en son sürümünü kullanarak etkinleştirdiğinizde, 3B dokunma işlevi Benzeticisinde test edebilirsiniz.

Bu işlevselliği etkinleştirmek için herhangi bir uygulama 3B Touch destekleyen benzetimli iPhone donanım çalıştırın (iPhone 6s ve büyük). Ardından, **donanım** iOS simülatörü ve etkinleştir menüde **3B dokunma için kullanım izleme paneli zorla** menü öğesi:

[![](3d-touch-images/simulator01.png "İOS Simulator'da donanım menüsünü seçin ve kullanım izleme paneli zorla 3B dokunma menü öğesi için etkinleştirme")](3d-touch-images/simulator01.png#lightbox)

Bu özellik active, gerçek iPhone donanımda gibi 3B dokunma etkinleştirmek için Mac'ın izleme paneli üzerinde daha zor basabilirsiniz.

## <a name="summary"></a>Özet

Bu makalede yeni 3B Touch iOS 9 iPhone 6s ve iPhone 6s için kullanılabilir hale API'leri anlatılmıştır artı. Uygulamaya ekleme baskısı duyarlılık kapsamdaki; Uygulama bilgileri Gezinti olmadan geçerli bağlamdan hızlı bir şekilde görüntülemek için gözlem ve Pop kullanarak; Uygulamanızı kısayollar sağlamak için hızlı eylemleri kullanarak en yaygın olarak kullanılan ve özelliklerine.



## <a name="related-links"></a>İlgili bağlantılar

- [iOS 9 ViewControllerPreview örnek](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts örnek](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [iOS 9 geliştiricileri için](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [İPhone uygulamalarınızı 3B dokunma hazırlanın](https://developer.apple.com/ios/3d-touch/)
