---
title: Çok dokunma parmak Xamarin.Android içinde izleme
description: Bu konu birden çok parmakları dokunma olayları izlemek nasıl gösterir
ms.prod: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 97e848e74a4513c55c0bf50258621c010b347fcd
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436612"
---
# <a name="multi-touch-finger-tracking"></a>Çok dokunma parmak izleme

_Bu konu birden çok parmakları dokunma olayları izlemek nasıl gösterir_

Aynı anda ekranda taşırken tek tek parmakları izlemek için çok touch uygulaması gerektiği zaman zamanlar vardır. Bir genel uygulama finger-paint programdır. İle tek bir parmak çizmek için aynı zamanda birden fazla parmakları ile aynı anda çizmek için kullanabilmek için kullanıcının istiyor. Programınızı birden çok dokunma olayları işler gibi hangi olayların her parmak karşılık gelen ayırmanız gerekir. Android bir kodu bu amaçla sağlar, ancak alma ve bu kodu işleme biraz zor olabilir.

Belirli bir parmak ile ilişkilendirilen tüm olayları için kodu aynı kalır. Bir parmak ilk ekrana dokunur ve parmak ekranından kaldırır sonra geçersiz hale gelir kodu atanır.
Bu kimlik kodları genellikle çok küçük tamsayı olan ve Android sonraki dokunma olayları için bunları yeniden kullanır.

Neredeyse her zaman, tek tek parmakları izleyen bir program dokunma izleme için bir sözlük tutar. Belirli bir parmak tanımlayan kodu sözlük anahtardır. Sözlük değeri uygulamaya bağlıdır. İçinde [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) program, her parmak vuruşun (Başlangıç yayımlamayı touch) ile bu parmak çizgi işlemek için gerekli tüm bilgileri içeren bir nesne ile ilişkili. Küçük bir programı tanımlayan `FingerPaintPolyline` sınıfı bu amaç için:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

Her çoklu çizgi rengi, vuruşun genişliğini ve Android grafik sahip [ `Path` ](https://developer.xamarin.com/api/type/Android.Graphics.Path/) biriktirmesi ve çizilen gibi birden çok noktası satırının işlemek için nesne.

Aşağıda gösterilen kodun geri kalanı yer bir `View` adlı türevi `FingerPaintCanvasView`. Sınıf bir sözlüğü nesne türü bulundurur `FingerPaintPolyline` bunlar etkin olarak bir veya daha fazla parmakları tarafından çizilen süre boyunca:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

Bu sözlük hızlı bir şekilde almak görünümü sağlar `FingerPaintPolyline` belirli bir parmak ile ilişkili bilgileri.

`FingerPaintCanvasView` Sınıfı ayrıca korur bir `List` nesne tamamlandı kullansa için:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

Bu nesneleri `List` bunlar çizilmiş sırayla.

`FingerPaintCanvasView` iki yöntem tarafından tanımlanan geçersiz kılmaları `View`: [ `OnDraw` ](https://developer.xamarin.com/api/member/Android.Views.View.OnDraw/p/Android.Graphics.Canvas/) ve [ `OnTouchEvent` ](https://developer.xamarin.com/api/member/Android.Views.View.OnTouchEvent/p/Android.Views.MotionEvent/).
İçinde kendi `OnDraw` geçersiz kılma, görünüm tamamlanmış kullansa çizer ve devam eden kullansa çizer.

Geçersiz kılma `OnTouchEvent` yöntemi başlar elde ederek bir `pointerIndex` değeri `ActionIndex` özelliği. Bu `ActionIndex` değeri arasında birden çok parmakları ayıran ancak birden çok olay arasında tutarlı değil. Bu nedenle, kullandığınız `pointerIndex` işaretçi almak için `id` değeri `GetPointerId` yöntemi. Bu kimliği *olan* birden çok olay genelinde tutarlı:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

Geçersiz kılma kullanan bildirimi `ActionMasked` özelliğinde `switch` deyimi yerine `Action` özelliği. Neden şöyledir:

Çok dokunma ile ilgilenirken `Action` özellik değerine sahip `MotionEventsAction.Down` ekran ve ardından değerlerini touch ilk parmak için `Pointer2Down` ve `Pointer3Down` ikinci ve üçüncü parmakları de dokunmatik ekran gibi. Dördüncü ve beşinci parmakları yaptıkça kişi, `Action` özelliğinin bile üyelerine karşılık gelen sayısal değerleri `MotionEventsAction` numaralandırma! Bit bayrakları ne anlama geldiklerini yorumlamak için değerler değerlerini incelemek gerekir.

Benzer şekilde, parmakları ekranla kişi çıkarken `Action` özelliğinin değerini `Pointer2Up` ve `Pointer3Up` ikinci ve üçüncü parmakları için ve `Up` ilk parmak için.

`ActionMasked` Özelliği alır üzerinde daha az sayıda ile birlikte kullanılması amaçlanan olduğundan değerleri sayısı `ActionIndex` arasında birden çok parmakları ayırt etmek için özellik. Özelliği, yalnızca parmakları ekrana dokunduğunuzda eşit olabilir `MotionEventActions.Down` ilk parmak için ve `PointerDown` sonraki parmakları için. Çıkış ekranı parmakları gibi `ActionMasked` değerlerini sahip `Pointer1Up` sonraki parmakları için ve `Up` ilk parmak için.

Kullanırken `ActionMasked`, `ActionIndex` dokunma ve diğer yöntemleri için bağımsız değişken olarak bu değer dışında kullanmak için genellikle ancak ekran gerekmez bırakın sonraki parmakları arasında ayırt `MotionEvent` nesnesi. En önemli biri çok dokunma için bu yöntemleri arasında `GetPointerId` Yukarıdaki kod çağrılır. Yöntemi bir değer döndüren olduğunu parmakları belirli olaylarla ilişkilendirmek üzere bir sözlük anahtarı için kullanabilirsiniz.

`OnTouchEvent` Geçersiz kılması [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) program işlemleri `MotionEventActions.Down` ve `PointerDown` aynı oluşturarak yeni olayları `FingerPaintPolyline` nesne ve sözlüğe ekleme:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

Dikkat `pointerIndex` görünümünde parmak konumunu almak için de kullanılır. Tüm dokunma bilgileri ile ilişkili `pointerIndex` değeri. `id` Sözlük girdisi oluşturmak için kullanılan böylece parmakları birden fazla ileti arasında benzersiz olarak tanımlar.

Benzer şekilde, `OnTouchEvent` tanıtıcısı da geçersiz `MotionEventActions.Up` ve `Pointer1Up` tamamlanmış çoklu çizgiye aktararak aynı `completedPolylines` sırasında çizilebilir şekilde koleksiyonu `OnDraw` geçersiz kılar. Kod ayrıca kaldırır `id` sözlük girişi:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

Şimdi hassas bölümü için.

Aşağı arasında ve olayları, genellikle vardır birçok `MotionEventActions.Move` olaylar. Bunlar için tek bir çağrı paketlendi `OnTouchEvent`, ve bunlar farklı gelen ele alınması gereken `Down` ve `Up` olaylar. `pointerIndex` Daha önce alınan değer `ActionIndex` özellik gözardı. Birden çok yöntem bunun yerine, edinmeniz gerekir `pointerIndex` 0 arasında döngü tarafından değerleri ve `PointerCount` özelliği ve ardından bir `id` bunların her biri için `pointerIndex` değerler:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

Bu tür bir işleme verir [FingerPaint](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint) tek tek parmakları izlemek ve sonuçlar ekranda çizmek için program:

[![FingerPaint örnek örnek ekran görüntüsü](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

Şimdi nasıl ekranda tek tek parmakları izlemek ve bunlar arasında ayrım gördünüz.


## <a name="related-links"></a>İlgili bağlantılar

- [Eşdeğer Xamarin iOS Kılavuzu](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
