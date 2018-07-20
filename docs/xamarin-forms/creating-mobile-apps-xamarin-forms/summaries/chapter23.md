---
title: Bölüm 23 özeti. Tetikleyiciler ve davranışlar
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 23 özeti. Tetikleyiciler ve davranışlar'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 19E84B5D-46B4-4B6D-A255-87BEFB011261
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 83a445555f9f184f735c105370de20665ad704a3
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156762"
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Bölüm 23 özeti. Tetikleyiciler ve davranışlar

Bunların her ikisi de veri bağlama öğesi etkileşimleri kullanımı dışında basitleştirmek ve XAML öğeleri genişletmek için XAML dosyalarında kullanılacak yöneliktir tetikleyiciler ve davranışlar, benzer. Tetikleyiciler ve davranışlar hem visual kullanıcı arabirimi nesneleri ile neredeyse her zaman kullanılır.

Tetikleyiciler ve davranışlar, desteklemek için her ikisi de `VisualElement` ve `Style` iki koleksiyon özellikleri destekler:

- [`VisualElement.Triggers`](xref:Xamarin.Forms.VisualElement.Triggers) ve [ `Style.Triggers` ](xref:Xamarin.Forms.Style.Triggers) türü `IList<TriggerBase>`
- [`VisualElement.Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) ve [ `Style.Behaviors` ](xref:Xamarin.Forms.Style.Behaviors) türü `IList<Behavior>`

## <a name="triggers"></a>Tetikleyiciler

Bir tetikleyici (başka bir özellik değişikliğini veya bazı kodları çalıştıran) bir yanıt sonuçları (özellik değişikliği veya bir olay tetikleyicisinin tetikleme) bir durumdur. `Triggers` Özelliği `VisualElement` ve `Style` türünde `IList<TriggersBase>`. [`TriggerBase`](xref:Xamarin.Forms.TriggerBase) dört sealed sınıfları türettiğiniz bir soyut sınıfı şöyledir:

- [`Trigger`](xref:Xamarin.Forms.Trigger) özellik değişikliklere dayalı yanıtlar için
- [`EventTrigger`](xref:Xamarin.Forms.EventTrigger) Olay firings üzerinde temel yanıtlar için
- [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) yanıtları bağlamaları verileri temel alan için
- [`MultiTrigger`](xref:Xamarin.Forms.MultiTrigger) birden çok Tetikleyicileri temel alan yanıtlar için

Tetikleyici her zaman, özelliği Tetikleyici tarafından değiştirilmiş öğe üzerinde ayarlanır.

### <a name="the-simplest-trigger"></a>En basit tetikleyici

[ `Trigger` ](xref:Xamarin.Forms.Trigger) Sınıfı için bir özellik değeri, bir değişiklik denetler ve aynı öğenin başka bir özelliği ayarlayarak yanıt verir.

`Trigger` üç özellik tanımlar:

- [`Property`](xref:Xamarin.Forms.Trigger.Property) türü `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Trigger.Value) türü `Object`
- [`Setters`](xref:Xamarin.Forms.Trigger.Setters) tür `IList<SetterBase>`, içerik özelliği `Trigger`

Ayrıca, `Trigger` şu özellik öğesinden devralınan gerektirir `TriggerBase` ayarlanabilir:

- [`TargetType`](xref:Xamarin.Forms.TriggerBase.TargetType) öğeyi türünü belirtmek için `Trigger` eklenir

`Property` Ve `Value` koşul oluşturan ve `Setters` yanıt koleksiyonudur. Zaman belirtilen `Property` tarafından belirtilen değere sahip `Value`, ardından `Setter` nesneler `Setters` koleksiyon uygulanır. Zaman `Property` farklı bir değere sahip ayarlayıcılar kaldırılır. `Setter` ilk iki özelliklerini aynı olan iki özellikleri tanımlayan `Trigger`:

- [`Property`](xref:Xamarin.Forms.Setter.Property) türü `BindableProperty`
- [`Value`](xref:Xamarin.Forms.Setter.Value) türü `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) örnek gösterir nasıl bir `Trigger` uygulanan bir `Entry` boyutunu artırabilir `Entry` aracılığıyla `Scale` özelliği olduğunda `IsFocused`özelliği `Entry` olduğu `true`.

Ortak olmasa da `Trigger` kodda olarak ayarlanabilir [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) örnek gösterir.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) örnek gösterir nasıl `Trigger` ayarlanabilir bir `Style` birden çok uygulamak için `Entry` öğeleri.

### <a name="trigger-actions-and-animations"></a>Eylem tetikleyici ve animasyonları

Bir tetikleyiciye bağlı olarak biraz kodu çalıştırmak mümkündür. Bu kod bir özelliği hedefleyen bir animasyon olabilir. Tek bir ortak yolu bir [ `EventTrigger` ](xref:Xamarin.Forms.EventTrigger), iki özellik tanımlar:

- [`Event`](xref:Xamarin.Forms.EventTrigger.Event) tür `string`, bir olayın adı
- [`Actions`](xref:Xamarin.Forms.EventTrigger.Actions) tür `IList<TriggerAction>`, yanıt olarak çalıştırmak üzere eylemleri bir listesi.

Bunu kullanmak için türetilen bir sınıf yazmanız gereken [ `TriggerAction<T>` ](xref:Xamarin.Forms.TriggerAction`1), genellikle `TriggerAction<VisualElement>`. Bu sınıf özelliklerini tanımlayabilirsiniz. Bağlanabilir özellikler yerine düz CLR özellikleri çünkü bunlar `TriggerAction` öğesinden türetilen değil `BindableObject`. Geçersiz kılmanız gerekir [ `Invoke` ](xref:Xamarin.Forms.TriggerAction`1.Invoke*) eylem çağırıldığında çağrılan yöntem. Bağımsız değişken hedef öğenin ' dir.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı, bir örnek verilmiştir. Çağrı `ScaleTo` animasyon uygulamak için özellik `Scale` bir öğenin özelliği. Özelliklerinden birini türünde olduğu için `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) sınıfı standart kullanmanıza imkan tanır `Easing` XAML statik alanları.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) örnek nasıl çağrılacağını gösterir `ScaleAction` gelen `EventTrigger` izleyen nesneleri `Focused` ve `Unfocused` olayları.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) örnek özel kolaylaştırıcı bir işlev için tanımlamak üzere nasıl gösterir `ScaleAction` bir arka plan kod dosyasında.

Eylemleri kullanarak da çağırabilirsiniz bir `Trigger` (olarak distinguished gelen `EventTrigger`). Bu uyumlu olmasını gerektirir, `TriggerBase` iki koleksiyonu tanımlar:

- [`EnterActions`](xref:Xamarin.Forms.TriggerBase.EnterActions) türü `IList<TriggerAction>`
- [`ExitActions`](xref:Xamarin.Forms.TriggerBase.ExitActions) türü `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) örnek bu koleksiyonların nasıl kullanılacağı gösterilmektedir.

### <a name="more-event-triggers"></a>Daha fazla olay tetikleyicileri

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplık çağrıları `ScaleTo` iki kez ölçeği artırma ve azaltma. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) örnek kullanır bu bir stilde `EventTrigger` görsel geri bildirim sağlamak için bir `Button` basıldığında. Bu çift animasyon türü koleksiyonunda iki eylemlerini kullanma olanağı da sağlar [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığı özelleştirilebilir shiver eylemi tanımlar. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) örnek gösterir.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığı sınırlı olduğundan `Entry` öğeleri ve kümeleri `TextColor` özelliğini kırmızı ise `Text` özelliği olmayan bir `double`. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) örnek gösterir.

### <a name="data-triggers"></a>Veri tetikleyicileri

[ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) Benzer `Trigger` dışında bir özellik değeri değişiklikleri izlemek yerine, bir veri bağlamayı izler. Bu özellik başka bir öğe özelliğinde etkilemek için bir öğe sağlar.

`DataTrigger` üç özellik tanımlar:

- [`Binding`](xref:Xamarin.Forms.DataTrigger.Binding) türü `BindingBase`
- [`Value`](xref:Xamarin.Forms.DataTrigger.Value) türü `Object`
- [`Setters`](xref:Xamarin.Forms.DataTrigger.Setters) türü `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) örnek gerektirir [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) kitaplığı ve kümeleri rengini Mavi Öğrenciler adlarını veya Pembe temel alarak `Sex` özelliği:

[![Cinsiyet renkler üç ekran](images/ch23fg04-small.png "cinsiyet renkleri")](images/ch23fg04-large.png#lightbox "cinsiyet renkleri")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) örnek kümeleri `IsEnabled` özelliği bir `Entry` için `False` varsa `Length` özelliği `Text` özelliği`Entry`0 değerine eşittir. Dikkat `Text` özelliği boş dize olarak yeniden başlatılır; varsayılan değer `null`ve `DataTrigger` düzgün çalışmaz.

### <a name="combining-conditions-in-the-multitrigger"></a>SE birleştirme koşulları

[ `MultiTrigger` ](xref:Xamarin.Forms.MultiTrigger) Koşullar oluşan bir koleksiyondur. Olduklarında tüm `true`, ayarlayıcılar uygulanır. Sınıfı iki özelliklerini tanımlar:

- [`Conditions`](xref:Xamarin.Forms.MultiTrigger.Conditions) türü `IList<Condition>`
- [`Setters`](xref:Xamarin.Forms.MultiTrigger.Setters) türü `IList<Setter>`

[`Condition`](xref:Xamarin.Forms.Condition) bir soyut sınıftır ve iki alt sınıfları içerir:

- [`PropertyCondition`](xref:Xamarin.Forms.Condition), sahip olduğu [ `Property` ](xref:Xamarin.Forms.PropertyCondition.Property) ve [ `Value` ](xref:Xamarin.Forms.PropertyCondition.Value) gibi özellikleri `Trigger`
- [`BindingCondition`](xref:Xamarin.Forms.BindingCondition), sahip olduğu [ `Binding` ](xref:Xamarin.Forms.BindingCondition.Binding) ve [ `Value` ](xref:Xamarin.Forms.BindingCondition.Value) gibi özellikleri `DataTrigger`

İçinde [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) örnek, bir `BoxView` yalnızca dört olduğunda renkli `Switch` öğeleri tüm açık olduğundan.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) örneği, nasıl kullanabileceğinizi gösterir bir `BoxView` bir renk olduğunda *herhangi* dört `Switch` öğeleri etkinleştirilir. Bu uygulamanın De Morgan'ın yasa ve tüm mantığı ters gerektirir.

Birleştirme ve ve mantıksal çok kolay değildir ve genelde görünmez gerektirir `Switch` Ara sonuçlar için öğeleri. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) örnek gösterir nasıl bir `Button` ya da etkinleştirilebilir iki `Entry` öğeleri yazdığınız bazı metinler vardır, ancak her ikisi de sahip değilse metin yazdığınız.

## <a name="behaviors"></a>Davranışlar

Bir tetikleyici ile her şeyi yapabilirsiniz, davranışı ile de yapabilirsiniz, ancak davranışları türetildiği bir sınıf her zaman gerektirir [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) ve aşağıdaki iki yöntemi geçersiz kılar:

- [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo*)
- [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom*)

Bağımsız değişken davranışı bağlı bir öğedir. Genellikle, `OnAttachedTo` yöntemi bazı olay işleyicileri ekler ve `OnDetachingFrom` bunları ayırır. Bu sınıf, genellikle bazı durumunu kaydeder, bu genellikle, paylaşılamıyor bir `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) örnek benzer **TriggerEntryValidation** bir davranış kullanması hariç, &mdash; [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) sınıfı [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

### <a name="behaviors-with-properties"></a>Özellikleri içeren davranışlar

`Behavior<T>` öğesinden türetilen `Behavior`, öğesinden türetildiğini `BindableObject`, bağlanabilir özellikler davranışı tanımlanabilir. Bu özellikler veri bağlamalara etkin olabilir.

Bu gösterilmiştir [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) kılan bir program kullanımını [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) sınıfını [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. `ValidEmailBehavior` salt okunur bağlanılabilir özellik vardır ve veri bağlamalarını kaynağı olarak görev yapar.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) örnek bir e-posta adresinin geçerli olduğunu göstermek için göstergesinin başka bir tür görüntülemek için bu aynı davranışı kullanır.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) önceki örnek bir değişim örnektir. ButtonGlide kullanan bir `DataTrigger` birlikte bu davranışı.

### <a name="toggles-and-check-boxes"></a>Geçiş yapar ve onay kutuları

Bir sınıf içinde iki durumlu bir düğmenin davranışını gibi yalıtılacak mümkündür [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) içinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı ve tüm tanımlayın görseller için tamamen XAML içinde Aç/Kapat.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) kullanan örnek `ToggleBehavior` ile bir `DataTrigger` kullanmak için bir `Label` geçiş için iki metin dizeleriyle.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) örnek, iki yer arasındaki geçiş yaparak bu kavramı genişletir `FormattedString` nesneleri.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığı türetilir `ContentView`, tanımlayan bir `IsToggled` özelliği ve içerir bir `ToggleBehavior` için iki durumlu düğme mantığı. Bu sayede daha kolay ve iki durumlu düğmeyi XAML içinde tanımlanacağını tarafından gösterildiği şekilde [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) örnek.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) içeren bir [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) türetilen sınıf `ToggleBase` ve kullandığı bir [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)Xamarin.Forms benzeyen iki durumlu düğme oluşturmak için sınıf `Switch`.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) içinde **Xamarin.FormsBook.Toolkit** bir animasyonlu seviyesini içinde yapmak için kullanılan bir animasyon sağlar [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)örnek.

### <a name="responding-to-taps"></a>Tap'ları için yanıtlama

Bir dezavantajı, `EventTrigger` kendisine eklenemez olduğu bir `TapGestureRecognizer` Tap'ları için yanıtlayın. Bu sorunu geçici olarak alma amacı olan [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) içinde [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) kullanan örnek `TapBehavior` önceki kullanılacak `ShiverAction` dokunulduğunda için `BoxView` öğeleri.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) örnek kapsülleyerek işaretleme üzerinde kesin nasıl gösterir bir `ShiverView` sınıfı.

### <a name="radio-buttons"></a>Radyo düğmeleri

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplık de sahip bir [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) sınıfı göre gruplandırılmış radyo düğmeleri oluşturmaya yönelik bir `string` grup adı.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) program, radyo düğmesini metin dizelerini kullanır. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) kullanan örnek bir `Style` görünüm checked ve unchecked düğmeler arasındaki fark için. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) örnek için radyo düğmeleri paketlenmiş görüntüleri kullanır:

[![Üç ekran radyo görüntülerin](images/ch23fg17-small.png "radyo düğmesine görüntü")](images/ch23fg17-large.png#lightbox "radyo düğmesine görüntü")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) örnek bir daire içinde bir nokta olan geleneksel görünen radyo düğmeleri çizer.

### <a name="fades-and-orientation"></a>Yavaşça ve yönü

Son örnek [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) radyo düğmelerini kullanarak üç farklı renk seçimi görünümler arasında geçiş yapmanızı sağlar. Giriş ve çıkış kullanarak üç görünüm Soldurma bir [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) içinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

Program ayrıca dikey ve yatay kullanarak arasında yönlendirme değişikliklere yanıt bir [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) içinde **Xamarin.FormsBook.Toolkit** kitaplığı.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 23 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Bölüm 23 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Tetiklerle çalışma](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms Davranışları](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
