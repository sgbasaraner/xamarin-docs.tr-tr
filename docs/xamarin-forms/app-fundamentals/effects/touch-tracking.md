---
title: Olayları etkileri çağırma
description: Efekt tanımlayabilir ve arka plandaki yerel görünümünde değişiklikleri sinyal bir olay çağırma. Bu makalede İzleme alt düzey çok dokunma parmak gerçekleştirme ve dokunma etkinlik sinyal olaylar üretmek nasıl gösterir.
ms.prod: xamarin
ms.assetid: 6A724681-55EB-45B8-9EED-7E412AB19DD2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/01/2017
ms.openlocfilehash: e363cae4dd72a25e4768395410d4e56a8db30eba
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="invoking-events-from-effects"></a>Olayları etkileri çağırma

_Efekt tanımlayabilir ve arka plandaki yerel görünümünde değişiklikleri sinyal bir olay çağırma. Bu makalede İzleme alt düzey çok dokunma parmak gerçekleştirme ve dokunma etkinlik sinyal olaylar üretmek nasıl gösterir._

Bu makalede açıklanan etkisi alt düzey dokunma olaylarına erişim sağlar. Bu alt düzey olayları varolan aracılığıyla kullanılabilir olmayan `GestureRecognizer` sınıfları, ancak bazı uygulama türleri için önemli. Örneğin, bir ekran üzerinde hareket ederken tek tek parmakları izlemek için uygulama gereksinimlerinde finger-paint. Müzik klavye Tap algılamak gereken ve bir anahtardan diğerine bir glissando gliding bir parmak yanı sıra tek tek anahtarları serbest bırakır.

Bir etkisi, herhangi bir Xamarin.Forms öğeye bağlı olduğundan çok dokunma parmak izlemek için idealdir.

## <a name="platform-touch-events"></a>Platform dokunma olayları

İOS, Android ve evrensel Windows platformu uygulamaları algılamak veren düşük düzeyli bir API dahil tüm etkinlik dokunun. Tüm üç temel türleri arasında ayrım yapmak için bu platformu dokunma olayları:

- *Basılı*, bir parmak ekran dokunduğunda
- *Taşınan*, ekran temas parmak taşındığında
- *Yayımlanan*, parmak ekranından serbest bırakıldığında

Bir çok dokunma ortamında birden çok parmakları aynı anda dokunmatik ekran. Çeşitli platformlar uygulamalar arasında birden çok parmakları ayırt etmek için kullanabileceğiniz bir kimlik numarasını (ID) içerir.

İOS içinde `UIView` sınıfı tanımlayan üç geçersiz kılınabilir yöntemleri `TouchesBegan`, `TouchesMoved`, ve `TouchesEnded` bu üç temel olaylara karşılık gelen. Makaleyi [çok dokunma parmak izleme](~/ios/app-fundamentals/touch/touch-tracking.md) bu yöntemleri kullanmayı açıklar. Ancak, bir iOS programı türeyen bir sınıf geçersiz kılma gerekmez `UIView` bu yöntemleri kullanmak için. İOS `UIGestureRecognizer` de bu aynı üç tanımlayan yöntemleri ve türeyen bir sınıf örneği iliştirebilirsiniz `UIGestureRecognizer` herhangi `UIView` nesnesi.

Android içinde `View` sınıfı tanımlayan adlı geçersiz kılınabilir bir yöntemi `OnTouchEvent` tüm dokunma etkinliği işlenecek. Dokunma etkinlik türü numaralandırma üyeleri tarafından tanımlanan `Down`, `PointerDown`, `Move`, `Up`, ve `PointerUp` makalesinde açıklandığı gibi [çok dokunma parmak izleme](~/android/app-fundamentals/touch/touch-tracking.md). Android `View` ayrıca adlı bir olay tanımlar `Touch` herhangi bir ağa bağlanması için bir olay işleyicisi sağlayan `View` nesnesi.

İçinde Evrensel Windows Platformu (UWP), `UIElement` sınıfı adlı olayları tanımlayan `PointerPressed`, `PointerMoved`, ve `PointerReleased`. Bu makalede açıklanan [işlemek işaretçi giriş makalesi MSDN'de](/windows/uwp/input-and-devices/handle-pointer-input/) ve API belgelerine [ `UIElement` ](/uwp/api/windows.ui.xaml.uielement/) sınıfı.

`Pointer` Evrensel Windows platformu API, fare, dokunma ve kalem giriş birleştirmek için tasarlanmıştır. Bu nedenle, `PointerMoved` olay Fare öğenin arasında taşındığında bile fare düğmesini basılı olduğunda çağrılır. `PointerRoutedEventArgs` Bu olayları eşlik nesne adlı bir özelliğe sahiptir `Pointer` adlı bir özellik olan `IsInContact` fare düğmesini basılı veya bir parmak iletişim kurmayan ekrandır gösterir.

Ayrıca, UWP adlı iki daha fazla olay tanımlar `PointerEntered` ve `PointerExited`. Bunlar, ne zaman fare ya da parmak bir öğeden diğerine taşır gösterir. Örneğin, A ve b adlı iki bitişik öğeleri göz önünde bulundurun İki öğeyi işaretçi olayları için işleyiciler yüklediniz. Bir parmak A'da bastığında `PointerPressed` olay çağrılır. Parmak taşınırken, A çağırır `PointerMoved` olaylar. Parmak A'dan B'ye taşınırsa A çağıran bir `PointerExited` olay ve B çağıran bir `PointerEntered` olay. Parmak sonra yayımlanırsa B çağıran bir `PointerReleased` olay.

İOS ve Android platformları UWP farklı: ilk çağrısı alır görünümü `TouchesBegan` veya `OnTouchEvent` bir parmak dokunduğunda görünümü parmak farklı görünümlere taşınırsa tüm dokunma etkinlik olsa bile almaya devam eder. İşaretçinin uygulama yakalar, UWP benzer şekilde davranabilir: içinde `PointerEntered` olay işleyicisi, öğesi çağrıları `CapturePointer` ve ardından bu parmak tüm dokunma etkinlik alır.

Bazı uygulamalar, örneğin, müzik klavye türleri için çok yararlı olacak UWP yaklaşım kanıtlar. Her anahtar için bu anahtarı dokunma olayları işlemek ve bir parmak başka bir kullanarak bir anahtarından slid algılayabilir `PointerEntered` ve `PointerExited` olaylar.

Bu nedenle, bu makalede açıklanan dokunma izleme etkisi UWP yaklaşım uygular.

## <a name="the-touch-tracking-effect-api"></a>Dokunma izleme etkisi API

[ **İzleme etkisi gösterileri Touch** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) örnek sınıflar (ve numaralandırma) alt düzey dokunma izleme uygulayan içerir. Bu tür ad alanına ait `TouchTracking` ve sözcüğüyle başlayan `Touch`. **TouchTrackingEffectDemos** .NET standart kitaplığı proje içerir `TouchActionType` numaralandırma dokunma olayların türünü için:

```csharp
public enum TouchActionType
{
    Entered,
    Pressed,
    Moved,
    Released,
    Exited,
    Cancelled
}
```

Tüm platformlar dokunma olay iptal edildi belirten bir olay da içerir.

`TouchEffect` .NET standart kitaplığı sınıfında türer `RoutingEffect` ve adlı bir olay tanımlar `TouchAction` ve adlı bir yöntem `OnTouchAction` , çağırır `TouchAction` olay:

```csharp
public class TouchEffect : RoutingEffect
{
    public event TouchActionEventHandler TouchAction;

    public TouchEffect() : base("XamarinDocs.TouchEffect")
    {
    }

    public bool Capture { set; get; }

    public void OnTouchAction(Element element, TouchActionEventArgs args)
    {
        TouchAction?.Invoke(element, args);
    }
}
```

Ayrıca fark `Capture` özelliği. Dokunma olaylarını yakalamak için bir uygulama bu özellik ayarlamalısınız `true` öncesinde bir `Pressed` olay. Aksi takdirde, dokunma olaylarına Evrensel Windows platformu de gibi davranır.

`TouchActionEventArgs` .NET standart kitaplığı sınıfında her olay eşlik tüm bilgileri içerir:

```csharp
public class TouchActionEventArgs : EventArgs
{
    public TouchActionEventArgs(long id, TouchActionType type, Point location, bool isInContact)
    {
        Id = id;
        Type = type;
        Location = location;
        IsInContact = isInContact;
    }

    public long Id { private set; get; }

    public TouchActionType Type { private set; get; }

    public Point Location { private set; get; }

    public bool IsInContact { private set; get; }
}
```

Bir uygulamanın kullanabileceği `Id` tek tek parmakları izleme özelliği. Bildirim `IsInContact` özelliği. Bu özellik her zaman olduğu `true` için `Pressed` olayları ve `false` için `Released` olaylar. Ayrıca her zaman olduğu `true` için `Moved` olayları iOS ve Android. `IsInContact` Özellik olabilir `false` için `Moved` program masaüstünde çalışan ve fare işaretçisi bir düğme hareket Evrensel Windows platformu olaylarına basılı.

Kullanabileceğiniz `TouchEffect` kendi uygulamalarınızı çözümün .NET standart kitaplığı projesinde dosyası dahil ve örneğine ekleyerek sınıfında `Effects` herhangi bir Xamarin.Forms öğe koleksiyonu. Bir işleyici ekleme `TouchAction` olay dokunma olaylarına elde edilir.

Kullanılacak `TouchEffect` , kendi uygulamanızda dahil platform uygulamaları da gerekir **TouchTrackingEffectDemos** çözümü.

## <a name="the-touch-tracking-effect-implementations"></a>Dokunma izleme etkin uygulamaları

İOS, Android ve UWP uygulamaları `TouchEffect` basit uygulama (UWP) ile başlayan ve diğerlerinden daha fazla yapısal olarak karmaşık olduğu için iOS uygulamasıyla biten aşağıda açıklanmıştır.

### <a name="the-uwp-implementation"></a>UWP uygulaması

UWP uygulaması `TouchEffect` en kolayıdır. Sınıfın türetildiği her zamanki gibi `PlatformEffect` ve iki derleme özniteliklerini içerir:

```csharp
[assembly: ResolutionGroupName("XamarinDocs")]
[assembly: ExportEffect(typeof(TouchTracking.UWP.TouchEffect), "TouchEffect")]

namespace TouchTracking.UWP
{
    public class TouchEffect : PlatformEffect
    {
        ...
    }
}
```

`OnAttached` Geçersiz kılma tüm işaretçi olayları alanları ve ekler işleyicileri olarak bazı bilgileri kaydeder:

```csharp
public class TouchEffect : PlatformEffect
{
    FrameworkElement frameworkElement;
    TouchTracking.TouchEffect effect;
    Action<Element, TouchActionEventArgs> onTouchAction;

    protected override void OnAttached()
    {
        // Get the Windows FrameworkElement corresponding to the Element that the effect is attached to
        frameworkElement = Control == null ? Container : Control;

        // Get access to the TouchEffect class in the .NET Standard library
        effect = (TouchTracking.TouchEffect)Element.Effects.
                    FirstOrDefault(e => e is TouchTracking.TouchEffect);

        if (effect != null && frameworkElement != null)
        {
            // Save the method to call on touch events
            onTouchAction = effect.OnTouchAction;

            // Set event handlers on FrameworkElement
            frameworkElement.PointerEntered += OnPointerEntered;
            frameworkElement.PointerPressed += OnPointerPressed;
            frameworkElement.PointerMoved += OnPointerMoved;
            frameworkElement.PointerReleased += OnPointerReleased;
            frameworkElement.PointerExited += OnPointerExited;
            frameworkElement.PointerCanceled += OnPointerCancelled;
        }
    }
    ...
}    
```

`OnPointerPressed` İşleyiciyi çağırır etkisi olay çağırarak `onTouchAction` alanındaki `CommonHandler` yöntemi:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerPressed(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Pressed, args);

        // Check setting of Capture property
        if (effect.Capture)
        {
            (sender as FrameworkElement).CapturePointer(args.Pointer);
        }
    }
    ...
    void CommonHandler(object sender, TouchActionType touchActionType, PointerRoutedEventArgs args)
    {
        PointerPoint pointerPoint = args.GetCurrentPoint(sender as UIElement);
        Windows.Foundation.Point windowsPoint = pointerPoint.Position;  

        onTouchAction(Element, new TouchActionEventArgs(args.Pointer.PointerId,
                                                        touchActionType,
                                                        new Point(windowsPoint.X, windowsPoint.Y),
                                                        args.Pointer.IsInContact));
    }
}
```

`OnPointerPressed` Ayrıca denetler `Capture` çağrıları ve .NET standart kitaplığı etkisi sınıfta bir özellik `CapturePointer` , `true`.

 Bir UWP olay işleyicileri bile basittir:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    void OnPointerEntered(object sender, PointerRoutedEventArgs args)
    {
        CommonHandler(sender, TouchActionType.Entered, args);
    }
    ...
}
```

### <a name="the-android-implementation"></a>Android uygulaması

Uygulamanız gerekir çünkü Android ve iOS uygulamaları mutlaka daha karmaşık `Exited` ve `Entered` bir parmak bir öğeden diğerine taşındığında olaylar. Her iki uygulamaları benzer şekilde yapılandırılmıştır.

Android `TouchEffect` sınıfı için bir işleyici yükler `Touch` olay:

```csharp
view = Control == null ? Container : Control;
...
view.Touch += OnTouch;
```

Sınıfı, ayrıca iki statik sözlük tanımlar:

```csharp
public class TouchEffect : PlatformEffect
{
    ...
    static Dictionary<Android.Views.View, TouchEffect> viewDictionary =
        new Dictionary<Android.Views.View, TouchEffect>();

    static Dictionary<int, TouchEffect> idToEffectDictionary =
        new Dictionary<int, TouchEffect>();
    ...
```

`viewDictionary` Her zaman yeni bir girdi alır `OnAttached` geçersiz kılma çağrılır:

```csharp
viewDictionary.Add(view, this);
```

Giriş sözlükte kaldırılır `OnDetached`. Her örneğinin `TouchEffect` etkisi bağlı belirli bir görünümü ile ilişkilidir. Statik sözlük herhangi verir `TouchEffect` tüm görünümleri ve bunların karşılık gelen Numaralandırılacak örneği `TouchEffect` örnekleri. Bu olayları bir görünümden diğerine aktarmak için izin vermek gereklidir.

Android tek tek parmakları izlemek bir uygulama sağlayan dokunma olayları için bir kodu atar. `idToEffectDictionary` Bu kodu ile ilişkilendiren bir `TouchEffect` örneği. Bu sözlüğe eklenen bir öğe olduğunda `Touch` işleyicisi için parmak tuşuna çağrılır:

```csharp
void OnTouch(object sender, Android.Views.View.TouchEventArgs args)
{
    ...
    switch (args.Event.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:
            FireEvent(this, id, TouchActionType.Pressed, screenPointerCoords, true);

            idToEffectDictionary.Add(id, this);

            capture = libTouchEffect.Capture;
            break;

```

Öğe kaldırılır `idToEffectDictionary` parmak ekranından serbest zaman. `FireEvent` Yöntemi çağırmak için gereken tüm bilgileri yalnızca öğelerden `OnTouchAction` yöntemi:

```csharp
void FireEvent(TouchEffect touchEffect, int id, TouchActionType actionType, Point pointerLocation, bool isInContact)
{
    // Get the method to call for firing events
    Action<Element, TouchActionEventArgs> onTouchAction = touchEffect.libTouchEffect.OnTouchAction;

    // Get the location of the pointer within the view
    touchEffect.view.GetLocationOnScreen(twoIntArray);
    double x = pointerLocation.X - twoIntArray[0];
    double y = pointerLocation.Y - twoIntArray[1];
    Point point = new Point(fromPixels(x), fromPixels(y));

    // Call the method
    onTouchAction(touchEffect.formsElement,
        new TouchActionEventArgs(id, actionType, point, isInContact));
}
```

Diğer tüm dokunma türleri iki farklı şekillerde işlenir: varsa `Capture` özelliği `true`, oldukça basit bir çeviri dokunma olaydır `TouchEffect` bilgi. Ne zaman daha karmaşık alır `Capture` olan `false` dokunma olaylarına bir görünümden diğerine taşınmasını gerektiğinden. Bu sorumluluğundadır `CheckForBoundaryHop` taşıma olayları sırasında çağrılan yöntemi. Bu yöntem her iki statik sözlükler kullanır. Aracılığıyla numaralandırır `viewDictionary` parmak şu anda temas ve onu kullanan görünüm belirlemek için `idToEffectDictionary` geçerli depolamak için `TouchEffect` örneği (ve bu nedenle, geçerli görünümü) belirli bir Kimlikle ilişkili:

```csharp
void CheckForBoundaryHop(int id, Point pointerLocation)
{
    TouchEffect touchEffectHit = null;

    foreach (Android.Views.View view in viewDictionary.Keys)
    {
        // Get the view rectangle
        try
        {
            view.GetLocationOnScreen(twoIntArray);
        }
        catch // System.ObjectDisposedException: Cannot access a disposed object.
        {
            continue;
        }
        Rectangle viewRect = new Rectangle(twoIntArray[0], twoIntArray[1], view.Width, view.Height);

        if (viewRect.Contains(pointerLocation))
        {
            touchEffectHit = viewDictionary[view];
        }
    }

    if (touchEffectHit != idToEffectDictionary[id])
    {
        if (idToEffectDictionary[id] != null)
        {
            FireEvent(idToEffectDictionary[id], id, TouchActionType.Exited, pointerLocation, true);
        }
        if (touchEffectHit != null)
        {
            FireEvent(touchEffectHit, id, TouchActionType.Entered, pointerLocation, true);
        }
        idToEffectDictionary[id] = touchEffectHit;
    }
}
```

Bir değişiklik yapıldıysa `idToEffectionDictionary`, büyük olasılıkla yöntemini çağırır `FireEvent` için `Exited` ve `Entered` bir görünümden aktarımı için. Ancak, parmak ekli bir olmadan bir görünümü tarafından dolu bir alanına taşınmış olabilecek `TouchEffect`, veya bir görünüme bağlı etkisi olmadan bu alanından.

Bildirim `try` ve `catch` görünümü erişildiğinde engelleyin. Daha sonra yeniden giriş sayfasına gider için gittiğinizde bir sayfasında `OnDetached` yöntemi çağrılmazsa ve öğeleri kalır `viewDictionary` ancak bunları atıldı Android dikkate alır.

### <a name="the-ios-implementation"></a>İOS uygulaması

İOS uygulaması hariç Android uygulaması benzer iOS `TouchEffect` sınıfının bir türevi örneği `UIGestureRecognizer`. Adlı iOS projesi sınıfında budur `TouchRecognizer`. Bu sınıf depolamak iki statik sözlük tutar `TouchRecognizer` örnekleri:

```csharp
static Dictionary<UIView, TouchRecognizer> viewDictionary =
    new Dictionary<UIView, TouchRecognizer>();

static Dictionary<long, TouchRecognizer> idToTouchDictionary =
    new Dictionary<long, TouchRecognizer>();
```

Bu yapısını kadar `TouchRecognizer` sınıfı için Android benzer `TouchEffect` sınıfı.

## <a name="putting-the-touch-effect-to-work"></a>Dokunma etkisi iş koyma

[ **TouchTrackingEffectDemos** ](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/) program ortak görevler için touch izleme test etme beş sayfalar içerir.

**BoxView sürükleyerek** sayfa eklemenizi sağlayan `BoxView` öğelerine bir `AbsoluteLayout` ve bunları ekran sürükleyin. [XAML dosyası](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml) iki başlatır `Button` eklemek için görünümleri `BoxView` öğelerine `AbsoluteLayout` ve Temizleme `AbsoluteLayout`.

Yönteminde [arka plan kod dosyasına](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BoxViewDraggingPage.xaml.cs) yeni ekler `BoxView` için `AbsoluteLayout` de ekler bir `TouchEffect` nesnesini `BoxView` ve bir olay işleyicisi efekti ekler:

```csharp
void AddBoxViewToLayout()
{
    BoxView boxView = new BoxView
    {
        WidthRequest = 100,
        HeightRequest = 100,
        Color = new Color(random.NextDouble(),
                          random.NextDouble(),
                          random.NextDouble())
    };

    TouchEffect touchEffect = new TouchEffect();
    touchEffect.TouchAction += OnTouchEffectAction;
    boxView.Effects.Add(touchEffect);
    absoluteLayout.Children.Add(boxView);
}
```

`TouchAction` Olay işleyicisi işler touch olayları tüm `BoxView` öğeleri, ancak bazı dikkatli gerekiyor: Bu iki parmakları tek bir izin veremez `BoxView` sürükleyerek programın yalnızca uygular ve iki parmakları gerekir çünkü birbiriyle müdahale. Bu nedenle, sayfa şu anda izlenen her parmak için katıştırılmış bir sınıf tanımlar:

```csharp
class DragInfo
{
    public DragInfo(long id, Point pressPoint)
    {
        Id = id;
        PressPoint = pressPoint;
    }

    public long Id { private set; get; }

    public Point PressPoint { private set; get; }
}

Dictionary<BoxView, DragInfo> dragDictionary = new Dictionary<BoxView, DragInfo>();
```

`dragDictionary` İçin bir giriş içerir her `BoxView` şu anda sürüklenen.

`Pressed` Dokunma eylem bu sözlüğe bir öğe ekler ve `Released` eylem kaldırır. `Pressed` Mantığı olup olmadığını zaten bir öğe sözlükte söz konusu kontrol gerekir `BoxView`. Bu durumda, `BoxView` zaten sürüklenen ve yeni olay saniyenin finger üzerinde aynı olduğundan `BoxView`. İçin `Moved` ve `Released` Eylemler, olay işleyicisi kontrol gerekir sözlük bir girdi için olup olmadığını `BoxView` ve dokunma `Id` özelliği, sürüklenen için `BoxView` sözlük girdisi yapısıyla eşleştiğini:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    BoxView boxView = sender as BoxView;

    switch (args.Type)
    {
        case TouchActionType.Pressed:
            // Don't allow a second touch on an already touched BoxView
            if (!dragDictionary.ContainsKey(boxView))
            {
                dragDictionary.Add(boxView, new DragInfo(args.Id, args.Location));

                // Set Capture property to true
                TouchEffect touchEffect = (TouchEffect)boxView.Effects.FirstOrDefault(e => e is TouchEffect);
                touchEffect.Capture = true;
            }
            break;

        case TouchActionType.Moved:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                Rectangle rect = AbsoluteLayout.GetLayoutBounds(boxView);
                Point initialLocation = dragDictionary[boxView].PressPoint;
                rect.X += args.Location.X - initialLocation.X;
                rect.Y += args.Location.Y - initialLocation.Y;
                AbsoluteLayout.SetLayoutBounds(boxView, rect);
            }
            break;

        case TouchActionType.Released:
            if (dragDictionary.ContainsKey(boxView) && dragDictionary[boxView].Id == args.Id)
            {
                dragDictionary.Remove(boxView);
            }
            break;
    }
}
```

`Pressed` Mantığı kümeleri `Capture` özelliği `TouchEffect` nesnesine `true`. Bu, bu parmak için tüm sonraki olaylarda aynı olay işleyicisi teslim etkisi vardır.

`Moved` Mantığı taşır `BoxView` değiştirilmesine tarafından `LayoutBounds` özelliği eklenmiş. `Location` Olay bağımsız değişkenlerinin özelliktir her zaman göreli `BoxView` sürüklenmekte olan ve `BoxView` sabit bir oranda sürüklenen `Location` ardışık olayları özelliklerini olacaktır yaklaşık aynı. Örneğin, bir parmak basarsa `BoxView` kendi Center'da `Pressed` eylem depoları bir `PressPoint` özelliği (50, 50), aynı kalır sonraki olayları için. Varsa `BoxView` sabit bir hızla, sonraki çapraz sürüklenip `Location` sırasında özellikleri `Moved` eylem değerini olabilir (55, 55); Bu durumda `Moved` mantığı Yatayvedikeykonumunu5ekler`BoxView`. Bu taşır `BoxView` yeniden merkezidir böylece doğrudan parmak altında.

Birden çok taşıyabilirsiniz `BoxView` aynı anda farklı parmakları kullanarak öğeleri.

[![](touch-tracking-images/boxviewdragging-small.png "Üçlü sayfasının ekran görüntüsü BoxView sürükleyerek")](touch-tracking-images/boxviewdragging-large.png#lightbox "Üçlü sayfasının ekran görüntüsü BoxView sürükleme")

### <a name="subclassing-the-view"></a>Görünüm alt sınıf yapma

Genellikle, bir Xamarin.Forms öğesi kendi dokunma olayları işlemek daha kolay olur. **Sürüklenebilir BoxView sürükleyerek** sayfa aynı işlevleri **BoxView sürükleyerek** sayfa, ancak kullanıcı drags örnekleri olan öğeler bir [ `DraggableBoxView` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/DraggableBoxView.cs) öğesinden türetilen sınıf `BoxView`:

```csharp
class DraggableBoxView : BoxView
{
    bool isBeingDragged;
    long touchId;
    Point pressPoint;

    public DraggableBoxView()
    {
        TouchEffect touchEffect = new TouchEffect
        {
            Capture = true
        };
        touchEffect.TouchAction += OnTouchEffectAction;
        Effects.Add(touchEffect);
    }

    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!isBeingDragged)
                {
                    isBeingDragged = true;
                    touchId = args.Id;
                    pressPoint = args.Location;
                }
                break;

            case TouchActionType.Moved:
                if (isBeingDragged && touchId == args.Id)
                {
                    TranslationX += args.Location.X - pressPoint.X;
                    TranslationY += args.Location.Y - pressPoint.Y;
                }
                break;

            case TouchActionType.Released:
                if (isBeingDragged && touchId == args.Id)
                {
                    isBeingDragged = false;
                }
                break;
        }
    }
}
```

Oluşturucusu oluşturur ve iliştirir `TouchEffect`ve ayarlar `Capture` bu nesnenin ilk örneği oluşturulduğunda özelliği. Sınıf depoladığından sözlük gereklidir `isBeingDragged`, `pressPoint`, ve `touchId` her parmak ile ilişkili değerler. `Moved` İşleme değiştirir `TranslationX` ve `TranslationY` mantığı çalışacak şekilde özellikleri olsa bile üst `DraggableBoxView` değil bir `AbsoluteLayout`.

### <a name="integrating-with-skiasharp"></a>SkiaSharp ile tümleştirme

Sonraki iki gösterim grafik gerektirir ve bu amaçla SkiaSharp kullanırlar. Hakkında bilgi edinmek isteyebilirsiniz [kullanarak SkiaSharp Xamarin.Forms içinde](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) Bu örnekler araştırmak önce. İlk iki makaleleri ("SkiaSharp çizim temel" ve "SkiaSharp satırları ve yolları"), burada gereken her şeyi kapsar.

**Elips çizme** sayfası, parmak ekranında geçirilerek elips çizme sağlar. Parmak taşıma nasıl bağlı olarak, sol üst sağ alt için veya herhangi bir köşesinin karşı köşe için üç nokta çizebilirsiniz. Elips bir rastgele bir renk ve opaklık çizilir.

[![](touch-tracking-images/ellipsedrawing-small.png "Üçlü sayfasının ekran görüntüsü elips çizme")](touch-tracking-images/ellipsedrawing-large.png#lightbox "Üçlü sayfasının ekran görüntüsü elips çizme")

Üç nokta birini sonra touch, başka bir konuma sürükleyin. Bu, "isabet-belirli bir noktada grafik nesnesi için arama kapsar test," olarak bilinen bir yöntem gerektirir. Bunlar, kendi gerçekleştiremezler SkiaSharp üç nokta Xamarin.Forms öğeleri olmayan `TouchEffect` işleme. `TouchEffect` Tüm uygulamalısınız `SKCanvasView` nesnesi.

[EllipseDrawPage.xaml](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml) dosya başlatır `SKCanvasView` tek hücreye `Grid`. `TouchEffect` Nesnesi bağlı olarak `Grid`:

```xaml
<Grid x:Name="canvasViewGrid"
        Grid.Row="1"
        BackgroundColor="White">

    <skia:SKCanvasView x:Name="canvasView"
                        PaintSurface="OnCanvasViewPaintSurface" />
    <Grid.Effects>
        <tt:TouchEffect Capture="True"
                        TouchAction="OnTouchEffectAction" />
    </Grid.Effects>
</Grid>
```

Android ve evrensel Windows platformu `TouchEffect` doğrudan bağlı `SKCanvasView`, ancak işe yaramazsa iOS. Dikkat `Capture` özelliği ayarlanmış `true`.

SkiaSharp işler her elips türünde bir nesne tarafından temsil edilen `EllipseDrawingFigure`:

```csharp
class EllipseDrawingFigure
{
    SKPoint pt1, pt2;

    public EllipseDrawingFigure()
    {
    }

    public SKColor Color { set; get; }

    public SKPoint StartPoint
    {
        set
        {
            pt1 = value;
            MakeRectangle();
        }
    }

    public SKPoint EndPoint
    {
        set
        {
            pt2 = value;
            MakeRectangle();
        }
    }

    void MakeRectangle()
    {
        Rectangle = new SKRect(pt1.X, pt1.Y, pt2.X, pt2.Y).Standardized;
    }

    public SKRect Rectangle { set; get; }

    // For dragging operations
    public Point LastFingerLocation { set; get; }

    // For the dragging hit-test
    public bool IsInEllipse(SKPoint pt)
    {
        SKRect rect = Rectangle;

        return (Math.Pow(pt.X - rect.MidX, 2) / Math.Pow(rect.Width / 2, 2) +
                Math.Pow(pt.Y - rect.MidY, 2) / Math.Pow(rect.Height / 2, 2)) < 1;
    }
}
```

`StartPoint` Ve `EndPoint` özellikleri program; dokunma işlerken kullanılır `Rectangle` özelliği elips çizme için kullanılır. `LastFingerLocation` Özelliği elips sürüklenen zaman oyuna gelir ve `IsInEllipse` isabet testi yöntemi yardımcı olur. Yöntem `true` noktası elips içinde ise.

[Arka plan kod dosyasına](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/EllipseDrawingPage.xaml.cs) üç koleksiyonları korur:

```csharp
Dictionary<long, EllipseDrawingFigure> inProgressFigures = new Dictionary<long, EllipseDrawingFigure>();
List<EllipseDrawingFigure> completedFigures = new List<EllipseDrawingFigure>();
Dictionary<long, EllipseDrawingFigure> draggingFigures = new Dictionary<long, EllipseDrawingFigure>();
```

`draggingFigure` Sözlüğünü içeren bir alt kümesini `completedFigures` koleksiyonu. SkiaSharp `PaintSurface` olay işleyicisi sadece bu nesne işler `completedFigures` ve `inProgressFigures` koleksiyonlar:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Fill
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (EllipseDrawingFigure figure in completedFigures)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
    foreach (EllipseDrawingFigure figure in inProgressFigures.Values)
    {
        paint.Color = figure.Color;
        canvas.DrawOval(figure.Rectangle, paint);
    }
}
```

Dokunma işleme trickiest parçası olan `Pressed` işleme. Bu yerdir isabet testi gerçekleştirilir, ancak kodu elips kullanıcının parmak altında algılarsa, şu anda başka bir parmak tarafından sürüklenen varsa bu elips yalnızca sürüklenebilir. Kullanıcının parmak altında hiçbir elips varsa, kodu yeni bir elips çizme işlemi başlar:

```csharp
case TouchActionType.Pressed:
    bool isDragOperation = false;

    // Loop through the completed figures
    foreach (EllipseDrawingFigure fig in completedFigures.Reverse<EllipseDrawingFigure>())
    {
        // Check if the finger is touching one of the ellipses
        if (fig.IsInEllipse(ConvertToPixel(args.Location)))
        {
            // Tentatively assume this is a dragging operation
            isDragOperation = true;

            // Loop through all the figures currently being dragged
            foreach (EllipseDrawingFigure draggedFigure in draggingFigures.Values)
            {
                // If there's a match, we'll need to dig deeper
                if (fig == draggedFigure)
                {
                    isDragOperation = false;
                    break;
                }
            }

            if (isDragOperation)
            {
                fig.LastFingerLocation = args.Location;
                draggingFigures.Add(args.Id, fig);
                break;
            }
        }
    }

    if (isDragOperation)
    {
        // Move the dragged ellipse to the end of completedFigures so it's drawn on top
        EllipseDrawingFigure fig = draggingFigures[args.Id];
        completedFigures.Remove(fig);
        completedFigures.Add(fig);
    }
    else // start making a new ellipse
    {
        // Random bytes for random color
        byte[] buffer = new byte[4];
        random.NextBytes(buffer);

        EllipseDrawingFigure figure = new EllipseDrawingFigure
        {
            Color = new SKColor(buffer[0], buffer[1], buffer[2], buffer[3]),
            StartPoint = ConvertToPixel(args.Location),
            EndPoint = ConvertToPixel(args.Location)
        };
        inProgressFigures.Add(args.Id, figure);
    }
    canvasView.InvalidateSurface();
    break;
```

Diğer SkiaSharp örnek **parmak boyama** sayfası. Vuruş rengini ve vuruşun genişliğini iki seçebilirsiniz `Picker` görünümleri ve bir veya daha fazla parmakları ile çizin:

[![](touch-tracking-images/fingerpaint-small.png "Üçlü sayfasının ekran görüntüsü parmak boyama")](touch-tracking-images/fingerpaint-large.png#lightbox "Üçlü sayfasının ekran görüntüsü parmak boyama")

Bu örnek ayrıca ekranında boyandığında her bir satır göstermek için ayrı bir sınıf gerektirir:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new SKPath();
    }

    public SKPath Path { set; get; }

    public Color StrokeColor { set; get; }

    public float StrokeWidth { set; get; }
}
```

Bir `SKPath` nesnesi, her satırın işlemek için kullanılır. [FingerPaint.xaml.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/FingerPaintPage.xaml.cs) dosyası bu nesneler, biri şu anda çizilen bu kullansa diğeri için tamamlanan kullansa için iki koleksiyonları sağlar:

```csharp
Dictionary<long, FingerPaintPolyline> inProgressPolylines = new Dictionary<long, FingerPaintPolyline>();
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

`Pressed` İşleme oluşturur Yeni bir `FingerPaintPolyline`, çağrıları `MoveTo` başlangıç noktasını depolamak için yol nesnesindeki ve bu nesneye ekler `inProgressPolylines` sözlük. `Moved` Çağrıları işleme `LineTo` yeni parmak konumu ile yol nesnesindeki ve `Released` işleme aktarımı tamamlanan çoklu çizgi gelen `inProgressPolylines` için `completedPolylines`. Bir kez daha, kod çizim gerçek SkiaSharp oldukça basittir:

```csharp
SKPaint paint = new SKPaint
{
    Style = SKPaintStyle.Stroke,
    StrokeCap = SKStrokeCap.Round,
    StrokeJoin = SKStrokeJoin.Round
};
...
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKCanvas canvas = args.Surface.Canvas;
    canvas.Clear();

    foreach (FingerPaintPolyline polyline in completedPolylines)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }

    foreach (FingerPaintPolyline polyline in inProgressPolylines.Values)
    {
        paint.Color = polyline.StrokeColor.ToSKColor();
        paint.StrokeWidth = polyline.StrokeWidth;
        canvas.DrawPath(polyline.Path, paint);
    }
}
```

### <a name="tracking-view-to-view-touch"></a>İzleme görünümü görünümü dokunma

Önceki örneklerde ayarladığınız `Capture` özelliği `TouchEffect` için `true`, ya da zaman `TouchEffect` oluşturuldu veya ne zaman `Pressed` olayı oluştu. Bu, aynı öğenin ilk görünümü basılı parmak ile ilişkilendirilen tüm olayları almasını sağlar. Son örnek mu *değil* ayarlamak `Capture` için `true`. Ekran iletişim kurmayan parmak bir öğeden diğerine taşındığında bu farklı davranışlara neden olur. Bir olay gelen parmak taşır öğe alır bir `Type` özelliğini `TouchActionType.Exited` ve ikinci öğeye sahip bir olay alan bir `Type` ayarıyla `TouchActionType.Entered`.

Bu tür bir touch işleme müzik klavye için çok kullanışlıdır. Bir anahtar bir parmak bir anahtarından slayt olduğunda bu, aynı zamanda basıldığında algılayabilir olmalıdır.

**Sessiz klavye** sayfayı tanımlayan küçük [ `WhiteKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/WhiteKey.cs) ve [ `BlackKey` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/BlackKey.cs) öğesinden türetilen sınıflar [ `Key` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/Key.cs), den türetilen `BoxView`.

`Key` Sınıfı bir gerçek müzik programında kullanılmaya hazır. Adlı ortak özelliklerini tanımlar `IsPressed` ve `KeyNumber`, MIDI standart tarafından oluşturulan anahtar kodu ayarlanması amaçlanan. `Key` Sınıfı ayrıca adlı bir olay tanımlayan `StatusChanged`, hangi zaman çağrılır `IsPressed` özellik değişikliği.

Birden çok parmakları her anahtarı izin verilir. Bu nedenle, `Key` sınıfı tutan bir `List` anahtara şu anda temas parmakları dokunma kimliği numaralarını:

```csharp
List<long> ids = new List<long>();
```

`TouchAction` Olay işleyicisi ekler bir Kimliğine `ids` listesi her ikisi için de bir `Pressed` olay türü ve bir `Entered` türü, ancak yalnızca `IsInContact` özelliği `true` için `Entered` olay. Kimliği kaldırılır `List` için bir `Released` veya `Exited` olay:

```csharp
void OnTouchEffectAction(object sender, TouchActionEventArgs args)
{
    switch (args.Type)
    {
      case TouchActionType.Pressed:
          AddToList(args.Id);
          break;

        case TouchActionType.Entered:
            if (args.IsInContact)
            {
                AddToList(args.Id);
            }
            break;

        case TouchActionType.Moved:
            break;

        case TouchActionType.Released:
        case TouchActionType.Exited:
            RemoveFromList(args.Id);
            break;
    }
}

```

`AddToList` Ve `RemoveFromList` yöntemleri iade `List` boş olmayan, boş arasında değişti ve bu durumda, çağırır `StatusChanged` olay.

Çeşitli `WhiteKey` ve `BlackKey` öğeleri sayfanın içinde düzenlenir [XAML dosyası](https://github.com/xamarin/xamarin-forms-samples/blob/master/Effects/TouchTrackingEffectDemos/TouchTrackingEffectDemos/TouchTrackingEffectDemos/SilentKeyboardPage.xaml), telefon yatay modunda tutulurken en iyi aradığı:

[![](touch-tracking-images/silentkeyboard-small.png "Üçlü sayfasının ekran görüntüsü sessiz klavye")](touch-tracking-images/silentkeyboard-large.png#lightbox "Üçlü sayfasının ekran görüntüsü sessiz klavye")

Anahtarlarını, parmak sweep varsa, renk hafif değişikliklerden dokunma olaylarına bir anahtardan diğerine aktarılmasını görürsünüz.

## <a name="summary"></a>Özet

Bu makalede, bir efekt olayları çağırmak nasıl ve yazmak ve alt düzey çok dokunma işleme uygulayan bir efekti kullanmak nasıl gösterilmiştir.


## <a name="related-links"></a>İlgili bağlantılar

- [İOS çok dokunma parmak izlemede](~/ios/app-fundamentals/touch/touch-tracking.md)
- [Çok dokunma parmak Android izleme](~/android/app-fundamentals/touch/touch-tracking.md)
- [Dokunma izleme etkin (örnek)](https://developer.xamarin.com/samples/xamarin-forms/effects/TouchTrackingEffectDemos/)
