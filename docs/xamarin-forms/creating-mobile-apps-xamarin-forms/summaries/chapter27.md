---
title: Bölüm 27 özeti. Özel oluşturucular
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 27 özeti. Özel oluşturucular'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 49961953-9336-4FD4-A42F-6D9B05FF52E7
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: c8b149cfeb814e2a1e0a0d1b38cca24ea096d112
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156531"
---
# <a name="summary-of-chapter-27-custom-renderers"></a>Bölüm 27 özeti. Özel oluşturucular

> [!NOTE] 
> Bu sayfadaki notları kitapta tanıtılan malzeme gelen Xamarin.Forms nerede ayrıldığını alanları gösterir.

Bir Xamarin.Forms öğesi gibi `Button` kapsüllenmiş adlı bir sınıfta bir platforma özgü düğme ile işlenen `ButtonRenderer`.  İşte [iOS sürümü `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/Renderers/ButtonRenderer.cs), [Android sürümünü `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/Renderers/ButtonRenderer.cs)ve [UWP sürümündeki `ButtonRenderer` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ButtonRenderer.cs).

Bu bölümde, kendi oluşturucular için platforma özgü nesnelerini eşleyen özel görünümler oluşturmak için nasıl yazılır açıklanır.

## <a name="the-complete-class-hierarchy"></a>Tam sınıf hiyerarşisi

Xamarin.Forms platforma özgü kod içeren dört derlemeler vardır.
Bu bağlantıları kullanarak GitHub üzerinde kaynak görüntüleyebilirsiniz:

- [**Xamarin.Forms.Platform** ](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform) (çok küçük)
- [**Xamarin.Forms.Platform.iOS**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.iOS)
- [**Xamarin.Forms.Platform.Android**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.Android)
- [**Xamarin.Forms.Platform.UAP**](https://github.com/xamarin/Xamarin.Forms/tree/master/Xamarin.Forms.Platform.UAP)

> [!NOTE]
> `WinRT` Kitap belirtilen derlemeleri misiniz artık bu çözümün bir parçası. 

[ **PlatformClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/PlatformClassHierarchy) örnek bir sınıf hiyerarşisi yürütülen platformu için geçerli olan derlemeler için görüntüler.

Adlı bir önemli sınıf fark edeceksiniz `ViewRenderer`. Bu, platforma özel Oluşturucu oluştururken öğesinden türetilen sınıftır. Hedef platform görünümü sisteme bağlı olmadığından üç farklı sürümde var:

İOS [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.iOS/ViewRenderer.cs#L25) genel bağımsız değişkenlere sahiptir:

- `TView` sınırlı [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` sınırlı [`UIKit.UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)

Android [ `ViewRenderer<TView, TNativeView>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.Android/ViewRenderer.cs#L17) genel bağımsız değişkenlere sahiptir:

- `TView` sınırlı [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeView` sınırlı [`Android.Views.View`](https://developer.xamarin.com/api/type/Android.Views.View/)

UWP [ `ViewRenderer<TElement, TNativeElement>` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Platform.UAP/ViewRenderer.cs#L6) genel bağımsız değişkenler farklı şekilde adlandırılmış:

- `TElement` sınırlı [`Xamarin.Forms.View`](xref:Xamarin.Forms.View)
- `TNativeElement` sınırlı [`Windows.UI.Xaml.FrameworkElement`](/uwp/api/Windows.UI.Xaml.FrameworkElement)

Bir oluşturucu yazılırken bir sınıftan türetme `View`ve ardından birden fazla yazma `ViewRenderer` sınıfları, desteklenen her platform için bir tane. Her platforma özgü uygulama olarak belirttiğiniz türünden türetilen bir yerel sınıf Bakacağınız `TNativeView` veya `TNativeElement` parametresi.

## <a name="hello-custom-renderers"></a>Özel oluşturucular Merhaba!

[ **HelloRenderers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/HelloRenderers) programı adlı özel bir görünüm başvuruyor `HelloView` içinde kendi [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/App.cs) sınıfı.

[ `HelloView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers/HelloView.cs) Sınıfı dahil **HelloRenderers** proje ve yalnızca türetilen `View`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.iOS/HelloViewRenderer.cs) Sınıfını **HelloRenderers.iOS** proje türetilir `ViewRenderer<HelloView, UILabel>`. İçinde `OnElementChanged` override, yerel iOS oluşturur `UILabel` ve çağrıları `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.Droid/HelloViewRenderer.cs) Sınıfını **HelloRenderers.Droid** proje türetilir `ViewRenderer<HelloView, TextView>`. İçinde `OnElementChanged` override, bir Android oluşturur `TextView` ve çağrıları `SetNativeControl`.

[ `HelloViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter27/HelloRenderers/HelloRenderers/HelloRenderers.UWP/HelloViewRenderer.cs) Sınıfını **HelloRenderers.UWP** ve diğer Windows projeleri türetilir `ViewRenderer<HelloView, TextBlock>`. İçinde `OnElementChanged` override, bir Windows oluşturur `TextBlock` ve çağrıları `SetNativeControl`.

Tüm `ViewRenderer` türevleri içeren bir `ExportRenderer` özniteliği derleme düzeyinde ilişkilendiren `HelloView` belirli sınıfıyla `HelloViewRenderer` sınıfı. Bu Xamarin.Forms oluşturucular tek bir platformu projelerinde nasıl bulur.

[![Üç ekran görüntüsü Hello görünümü](images/ch27fg02-small.png "özel Oluşturucu")](images/ch27fg02-large.png#lightbox "özel oluşturucular")

## <a name="renderers-and-properties"></a>Oluşturucular ve Özellikler

Oluşturucu sonraki kümesini elips çizme uygular ve çeşitli projelerde bulunan [ **Xamarin.FormsBook.Platform** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Platform) çözüm.

[ `EllipseView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/EllipseView.cs) Sınıfı **Xamarin.FormsBook.Platform** platform. Sınıf benzer `BoxView` ve yalnızca tek bir özelliğini tanımlar: `Color` türü `Color`.

Oluşturucu üzerinde ayarlanan özellik değerlerini aktarabilirsiniz bir `View` kılarak yerel nesnesine `OnElementPropertyChanged` işleyicide yöntemi. Bu yöntem içinde (ve çoğu oluşturucu içinde), iki özellik kullanılabilir:

- `Element`, Xamarin.Forms öğesi
- `Control`, yerel bir görünüm veya pencere öğesi veya denetim nesnesi

Bu özelliklerin türleri için genel parametreler tarafından belirlenir `ViewRenderer`. Bu örnekte, `Element` türünde `EllipseView`.

`OnElementPropertyChanged` Geçersiz kılma bu nedenle aktarılabileceği `Color` değerini `Element` yerel için `Control` büyük olasılıkla bir tür dönüştürme ile bir nesne. Üç Oluşturucu şunlardır:

- iOS: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseViewRenderer.cs), kullanan bir [ `EllipseUIView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/EllipseUIView.cs) elipsin sınıfı.
- Android: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseViewRenderer.cs), kullanan bir [ `EllipseDrawableView` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/EllipseDrawableView.cs) elipsin sınıfı.
- UWP: [ `EllipseViewRenderer` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/EllipseViewRenderer.cs), yerel Windows kullanabileceğiniz [ `Ellipse` ](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) sınıfı.

[ **EllipseDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/EllipseDemo) sınıfı bunlardan birkaç görüntüler `EllipseView` nesneler:

[![Üç ekran görüntüsü elips tanıtım](images/ch27fg03-small.png "EllipseView özel Oluşturucu")](images/ch27fg03-large.png#lightbox "EllipseView özel oluşturucular")

[ **BouncingBall** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/BouncingBall) geri dönmeler bir `EllipseView` ekran tarafında kapalı.

## <a name="renderers-and-events"></a>Oluşturucular ve olaylar

Ayrıca, dolaylı olarak olaylar oluşturmak Oluşturucu için de mümkündür. [ `StepSlider` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform/StepSlider.cs) Sınıfı için normal Xamarin.Forms benzer `Slider` arasında ayrı adımlarından belirtmeye izin verir, ancak `Minimum` ve `Maximum` değerleri.

Üç Oluşturucu şunlardır:

- iOS: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.iOS/StepSliderRenderer.cs)
- Android: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.Android/StepSliderRenderer.cs)
- UWP: [`StepSliderRenderer`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Platform/Xamarin.FormsBook.Platform.WinRT/StepSliderRenderer.cs)

Oluşturucu yerel denetim değişikliklerini algılamak ve sonra çağrı `SetValueFromRenderer`, tanımlı bir bağlanılabilir özellik başvuran `StepSlider`, bir değişiklik neden olur `StepSlider` ateşlenmesine bir `ValueChanged` olay.

[ **StepSliderDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27/StepSliderDemo) örnek, bu yeni kaydırıcı gösterir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 27 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch27-Apr2016.pdf)
- [Bölüm 27 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter27)
- [Özel Oluşturucular](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
