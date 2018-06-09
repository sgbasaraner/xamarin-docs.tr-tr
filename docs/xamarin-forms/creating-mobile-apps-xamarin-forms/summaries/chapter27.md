---
title: Bölüm 27 özeti. Özel oluşturucu
description: 'Xamarin.Forms ile mobil uygulamaları oluşturma: Bölüm 27 özeti. Özel oluşturucu'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 844c9be72071cd307bd2330b54f8d70ddf28b9a3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240888"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Bölüm 27 özeti. Özel oluşturucu

Xamarin.Forms öğesi gibi `Button` adlı bir sınıf kapsüllenmiş bir platforma özgü düğmesi ile işlenen `ButtonRenderer`.  Burada [iOS sürümü `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [Android sürümü `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)ve [Windows çalışma zamanı sürümü `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ButtonRenderer.cs).

Bu bölümde, platforma özgü nesnelere eşleme özel görünümler oluşturmak için kendi Oluşturucu yazma biçimini açıklanır.

## <a name="the-complete-class-hierarchy"></a>Tam sınıf hiyerarşisi

Xamarin.Forms platforma özgü kodu içeren yedi derlemeleri vardır.
Bu bağlantıları kullanarak Github'da kaynak görüntüleyebilirsiniz:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (çok küçük)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.WinRT** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT) (sonraki üç daha büyük)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)
- [**Xamarin.Forms.Platform.WinRT.Tablet**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Tablet)
- [**Xamarin.Forms.Platform.WinRT.Phone**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.WinRT.Phone)

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) örnek yürütülen platformu için geçerli derlemeler için sınıf hiyerarşisi görüntüler.

Adlı önemli bir sınıftır fark edeceksiniz `ViewRenderer`. Bu, platforma özgü Oluşturucu oluştururken öğesinden türetilen sınıftır. Hedef platform görünüm sisteme bağlı olduğundan üç farklı sürümlerinde mevcuttur:

İOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L26) genel bağımsız değişkenlere sahiptir:

- `TView` kısıtlı için [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` kısıtlı için [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L14) genel bağımsız değişkenlere sahiptir:

- `TView` kısıtlı için [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeView` kısıtlı için [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

Windows çalışma zamanı [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.WinRT/ViewRenderer.cs#L12) genel bağımsız farklı adlandırılmış:

- `TElement` kısıtlı için [`Xamarin.Forms.View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)
- `TNativeElement` kısıtlı için [`Windows.UI.Xaml.FrameworkElement`](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx)

Bir işleyici yazılırken öğesinden bir sınıf türetme `View`ve birden çok yazma `ViewRenderer` sınıfları, desteklenen her platform için bir tane. Her platforma özgü uygulaması olarak belirttiğiniz türü türetilen yerel bir sınıf başvurur `TNativeView` veya `TNativeElement` parametresi.

## <a name="hello-custom-renderers"></a>Merhaba, özel Oluşturucu!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) program başvuran adlı özel bir görünüm `HelloView` içinde kendi [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) sınıfı.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Sınıfı dahil **HelloRenderers** proje ve yalnızca türeyen `View`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) Sınıfını **HelloRenderers.iOS** proje türer `ViewRenderer<HelloView, UILabel>`. İçinde `OnElementChanged` geçersiz kılma, yerel iOS oluşturduğu `UILabel` ve çağrıları `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) Sınıfını **HelloRenderers.Droid** proje türer `ViewRenderer<HelloView, TextView>`. İçinde `OnElementChanged` geçersiz kılma, bir Android oluşturduğu `TextView` ve çağrıları `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) Sınıfını **HelloRenderers.UWP** ve diğer Windows projeleri türer `ViewRenderer<HelloView, TextBlock>`. İçinde `OnElementChanged` geçersiz kılma, Windows oluşturduğu `TextBlock` ve çağrıları `SetNativeControl`.

Tüm `ViewRenderer` türevleri içeren bir `ExportRenderer` özniteliği ilişkilendirir derleme düzeyinde `HelloView` belirli sınıfıyla `HelloViewRenderer` sınıfı. Bu nasıl Xamarin.Forms oluşturucu bağımsız platform projelerinde bulur.

[![Üçlü ekran Hello görünümünün](images/ch27fg02-small.png "özel Oluşturucu")](images/ch27fg02-large.png#lightbox "özel Oluşturucu")

## <a name="renderers-and-properties"></a>Oluşturucu ve özellikleri

Oluşturucu bir sonraki kümesini elips çizme uygular ve çeşitli projelerinde bulunan [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) çözümü.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Sınıfı olan **Xamarin.FormsBook.Platform** platform. Sınıf benzer `BoxView` ve yalnızca tek bir özelliğini tanımlar: `Color` türü `Color`.

Oluşturucu özellik değerlerini ayarlayın aktarabilirsiniz bir `View` kılarak yerel nesnesine `OnElementPropertyChanged` işleyicide yöntemi. Bu yöntem içinde (ve oluşturucu çoğunu içinde), iki özellik bulunmaktadır:

- `Element`, Xamarin.Forms öğesi
- `Control`, yerel görünüm veya pencere öğesi veya denetim nesnesi

Bu özelliklerin türleri için genel parametreler tarafından belirlenir `ViewRenderer`. Bu örnekte, `Element` türü `EllipseView`.

`OnElementPropertyChanged` Geçersiz kılma dolayısıyla aktarım `Color` değerini `Element` yerel için `Control` nesnesiyle, büyük olasılıkla bir tür dönüştürme. Üç Oluşturucu şunlardır:

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), kullanan bir [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) elips için sınıf.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), kullanan bir [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) elips için sınıf.
- Windows çalışma zamanı: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), yerel Windows kullanabileceğiniz [ `Ellipse` ](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.ellipse.aspx) sınıfı.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) sınıfı görüntüler birkaç bu `EllipseView` nesneler:

[![Üçlü ekran görüntüsü elips Demo](images/ch27fg03-small.png "EllipseView özel Oluşturucu")](images/ch27fg03-large.png#lightbox "EllipseView özel Oluşturucu")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) geri dönmeler bir `EllipseView` ekran yanlarından kapalı.

## <a name="renderers-and-events"></a>Oluşturucu ve olaylar

Ayrıca, dolaylı olarak olaylar oluşturmak işleyiciler için de mümkündür. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Sınıfı normal Xamarin.Forms benzer `Slider` ancak ayrık adımları arasında sayısını belirterek verir `Minimum` ve `Maximum` değerleri.

Üç Oluşturucu şunlardır:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- Windows çalışma zamanı: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Oluşturucu yerel denetim değişiklikleri algılar ve ardından çağırın `SetValueFromRenderer`, tanımlanan bağlanabilirse özelliği başvuran `StepSlider`, hangi nedenler değişiklik `StepSlider` tetiklenecek bir `ValueChanged` olay.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) örneği, bu yeni kaydırıcı gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 27 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Bölüm 27 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
