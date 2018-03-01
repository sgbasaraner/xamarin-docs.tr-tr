---
title: "Bölüm 23 özeti. Tetikleyiciler ve davranışları"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 72158cac683e46a37d2cbd537cdfd72bea78a336
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-23-triggers-and-behaviors"></a>Bölüm 23 özeti. Tetikleyiciler ve davranışları

Bunların her ikisi de öğesi etkileşimlerinin kullanımı ötesinde veri bağlamaları basitleştirmek ve XAML öğeleri işlevselliğini genişletmek için XAML dosyalarında kullanılacak tasarlanmıştır tetikleyiciler ve davranışları, benzer. Tetikleyiciler ve davranışları visual kullanıcı arabirimi nesneleri ile neredeyse her zaman kullanılır.

Tetikleyiciler ve davranışları desteklemek için her ikisini de `VisualElement` ve `Style` iki koleksiyon özellikleri destekler:

- [`VisualElement.Triggers`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Triggers/) ve [ `Style.Triggers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Triggers/) türü `IList<TriggerBase>`
- [`VisualElement.Behaviors`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) ve [ `Style.Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Behaviors/) türü `IList<Behavior>`

## <a name="triggers"></a>Tetikleyiciler

Bir tetikleyici (başka bir özellik değişikliği veya bazı kodları çalıştıran) bir yanıt sonuçları (özellik değişikliği veya bir olay tetikleme) bir durumdur. `Triggers` Özelliği `VisualElement` ve `Style` türü `IList<TriggersBase>`. [`TriggerBase`](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerBase/) dört sealed sınıfları kendisinden türetilen soyut bir sınıf ilgilidir:

- [`Trigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) özellik değişikliklere dayalı yanıtlar için
- [`EventTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/) Olay firings üzerinde temel yanıtlar için
- [`DataTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) veri bağlantılarına dayalı yanıtlar için
- [`MultiTrigger`](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) birden çok tetikleyici üzerinde temel yanıtlar için

Tetikleyici her zaman, özelliği tetik tarafından değiştirilen öğe üzerinde ayarlanır.

### <a name="the-simplest-trigger"></a>En basit tetikleyici

[ `Trigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Trigger/) Sınıfı bir özellik değeri için bir değişikliği denetler ve aynı öğenin başka bir özelliği ayarlanarak yanıt verir.

`Trigger` üç özelliklerini tanımlar:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Property/) türü `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Value/) türü `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Trigger.Setters/) tür `IList<SetterBase>`, içerik özelliği `Trigger`

Ayrıca, `Trigger` aşağıdaki özelliği kaynağından devralındı gerektirir `TriggerBase` ayarlanabilir:

- [`TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.TargetType/) öğede türünü belirtmek için `Trigger` bağlı

`Property` Ve `Value` koşulu oluşturan ve `Setters` yanıt koleksiyonudur. Zaman belirtilen `Property` tarafından belirtilen değere sahip `Value`, sonra `Setter` nesnelerini `Setters` koleksiyonu uygulanır. Zaman `Property` farklı bir değere sahip ayarlayıcıları kaldırılır. `Setter` ilk iki özellikleri ile aynı olan iki özelliklerini tanımlar `Trigger`:

- [`Property`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/) türü `BindableProperty`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/) türü `Object`

[ **EntryPop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPop) örnek gösterilmektedir nasıl bir `Trigger` uygulanan bir `Entry` boyutunu artırabilirsiniz `Entry` aracılığıyla `Scale` özelliği zaman `IsFocused`özelliği `Entry` olan `true`.

Ortak olmasa da `Trigger` kodda olarak ayarlanabilir [ **EntryPopCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntryPopCode) örnek gösterilmektedir.

[ **StyledTriggers** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/StyledTriggers) örnek gösterilmektedir nasıl `Trigger` ayarlanabilir bir `Style` birden çok uygulamak için `Entry` öğeleri.

### <a name="trigger-actions-and-animations"></a>Tetikleyici eylemleri ve animasyonları

Bir tetikleyiciye bağlı biraz kod çalıştırmak mümkündür. Bu kod bir özelliği hedefleyen bir animasyon olabilir. Bir ortak yoludur kullanmak için bir [ `EventTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.EventTrigger/), iki özellik tanımlar:

- [`Event`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Event/) tür `string`, bir olayın adı
- [`Actions`](https://developer.xamarin.com/api/property/Xamarin.Forms.EventTrigger.Actions/) tür `IList<TriggerAction>`, yanıt olarak çalıştırmak için eylemlerin bir listesi.

Bunu kullanmak için türeyen bir sınıf yazmanız gereken [ `TriggerAction<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/), genellikle `TriggerAction<VisualElement>`. Bu sınıf özelliklerini tanımlayabilirsiniz. Bağlanabilir özellikleri yerine düz CLR özellikleri çünkü bunlar `TriggerAction` öğesinden türetilen değil `BindableObject`. Geçersiz kılmanız gerekir [ `Invoke` ](https://developer.xamarin.com/api/member/Xamarin.Forms.TriggerAction%3CT%3E.Invoke/p/T/) eylem çağrıldığında çağrılan yöntem. Bağımsız değişken hedef öğedir.

[ `ScaleAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleAction.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı bir örneğidir. Çağırır `ScaleTo` animasyon özelliği `Scale` bir öğenin özelliği. Özelliklerinden biri türünde olduğundan `Easing`, [ `EasingConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/EasingConverter.cs) sınıfı standart kullanmanıza imkan tanır `Easing` XAML'de static alanlar.

[ **EntrySwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EntrySwell) örnek gösterilmektedir çağırmak nasıl `ScaleAction` gelen `EventTrigger` nesneleri izleyen `Focused` ve `Unfocused` olaylar.

[ **CustomEasingSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/CustomEasingSwell) örnek için özel bir hareket hızı işlev tanımlamak nasıl gösterir `ScaleAction` arka plan kod dosyasına.

Kullanarak eylemleri de çağırabileceği bir `Trigger` (olarak distinguished gelen `EventTrigger`). Bu farkında olmasını gerektirir, `TriggerBase` iki koleksiyon tanımlar:

- [`EnterActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.EnterActions/) türü `IList<TriggerAction>`
- [`ExitActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.TriggerBase.ExitActions/) türü `IList<TriggerAction>`

[ **EnterExitSwell** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EnterExitSwell) örnek bu koleksiyonların nasıl kullanılacağı gösterilmektedir.

### <a name="more-event-triggers"></a>Daha fazla olay tetikleyicileri

[ `ScaleUpAndDownAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ScaleUpAndDownAction.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı çağrıları `ScaleTo` iki kez yukarı ve aşağı doğru ölçeklendirme. [ **ButtonGrowth** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonGrowth) örnek kullanan bu bir stilde `EventTrigger` görsel geribildirim sağlamak için bir `Button` basıldığında. Bu çift animasyon türü koleksiyonunda iki eylemler kullanma olanağı da sağlar [`DelayedScaleAction`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DelayedScaleAction.cs)

[ `ShiverAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ShiverAction.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığı özelleştirilebilir shiver eylemi tanımlar. [ **ShiverButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverButtonDemo) örnek gösterilmektedir.

[ `NumericValidationAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationAction.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığıdır sınırlı `Entry` öğeleri ve kümelerini `TextColor` kırmızı IF özelliğine `Text` özelliği değil bir `double`. [ **TriggerEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TriggerEntryValidation) örnek gösterilmektedir.

### <a name="data-triggers"></a>Veri tetikleyicileri

[ `DataTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTrigger/) Benzer `Trigger` dışında bir özellik için değer değişikliklerini izleme yerine, veri bağlama izler. Bu özelliği başka bir öğe özelliğinde etkilemek için bir öğe sağlar.

`DataTrigger` üç özelliklerini tanımlar:

- [`Binding`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Binding/) türü `BindingBase`
- [`Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Value/) türü `Object`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.DataTrigger.Setters/) türü `IList<SetterBase>`

[ **GenderColors** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/GenderColors) örneği gerektirir [ **SchoolOfFineArt** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/SchoolOfFineArt) kitaplığı ve kümelerini mavi Öğrenciler adlarını renkleri veya Pembe temel alarak `Sex` özelliği:

[![Üçlü ekran görüntüsü cinsiyetiniz renkleri](images/ch23fg04-small.png "cinsiyetiniz renkleri")](images/ch23fg04-large.png "cinsiyetiniz renkleri")

[ **ButtonEnabler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ButtonEnabler) örnek kümeleri `IsEnabled` özelliği bir `Entry` için `False` varsa `Length` özelliği `Text` özelliği`Entry`0'a eşit. Dikkat `Text` özelliği, boş bir dize olarak başlatılır; varsayılan değer `null`ve `DataTrigger` düzgün çalışmaz.

### <a name="combining-conditions-in-the-multitrigger"></a>MultiTrigger birleştirme koşulları

[ `MultiTrigger` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MultiTrigger/) Koşullar koleksiyonudur. Olduklarında tüm `true`, ayarlayıcılar uygulanır. Sınıfı, iki özellik tanımlar:

- [`Conditions`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Conditions/) türü `IList<Condition>`
- [`Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.MultiTrigger.Setters/) türü `IList<Setter>`

[`Condition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/) bir Özet sınıf ve iki alt sınıfları içerir:

- [`PropertyCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.Condition/), sahip olduğu [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Property/) ve [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.PropertyCondition.Value/) gibi özellikleri `Trigger`
- [`BindingCondition`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingCondition/), sahip olduğu [ `Binding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Binding/) ve [ `Value` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingCondition.Value/) gibi özellikleri `DataTrigger`

İçinde [ **AndConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/AndConditions) örnek, bir `BoxView` yalnızca dört zaman renkli `Switch` öğeleri tüm açık olduğundan.

[ **OrConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/OrConditions) örnek gösterilmektedir nasıl yapabileceğiniz bir `BoxView` bir renk zaman *herhangi* dört `Switch` öğeleri yanar. Bu uygulamanın De Morgan'ın yasa ve tüm mantığı ters gerektirir.

Birleştirme ve ve veya mantığı kolay değildir ve genelde görünmez gerektirir `Switch` Ara sonuçların için öğeleri. [ **XorConditions** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/XorConditions) örnek gösterilmektedir nasıl bir `Button` her iki etkinleştirilebilir iki `Entry` öğeleri yazdığınız bazı metinler vardır, ancak her ikisi de sahip değilse, bazı metin kutusuna yazdığınız.

## <a name="behaviors"></a>Davranışlar

Bir tetikleyici ile herhangi bir şey yapmak için bir davranışa da yapabilirsiniz ancak davranışları her zaman türeyen bir sınıf gerektirir [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) ve aşağıdaki iki yöntemi geçersiz kılar:

- [`OnAttachedTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/T/)
- [`OnDetachingFrom`](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/T/)

Bağımsız değişken davranışı bağlı öğedir. Genellikle, `OnAttachedTo` yöntemi bazı olay işleyicileri iliştirir ve `OnDetachingFrom` bunları ayırır. Böyle bir sınıfın genellikle bazı durumu kaydettiğinden, bu genellikle, paylaşılamaz bir `Style`.

[**BehaviorEntryValidation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BehaviorEntryValidation) örnek benzer **TriggerEntryValidation** davranışı & #x 2014; kullanır ancak bu [ `NumericValidationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/NumericValidationBehavior.cs) sınıfında[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

### <a name="behaviors-with-properties"></a>Özellikleriyle davranışları

`Behavior<T>` türetilen `Behavior`, den türetilen `BindableObject`, bağlanabilir özellikleri bir davranış tanımlanabilir. Bu özellikler veri bağlamaları etkin olabilir.

Bu, gösterilmiştir [ **EmailValidationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationDemo) yapar program kullanımını [ `ValidEmailBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ValidEmailBehavior.cs) sınıfını [  **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı. `ValidEmailBehavior` bir salt okunur bağlanabilirse özelliğine sahiptir ve veri bağlamaları kaynağında görevi görür.

[ **EmailValidationConv** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationConv) örnek başka türde bir e-posta adresinin geçerli olduğunu göstermek için göstergeyi görüntülemek için bu aynı davranışı kullanır.

[ **EmailValidationTrigger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/EmailValidationTrigger) örnektir önceki örneği bir çeşididir. ButtonGlide kullanan bir `DataTrigger` bu davranışı birlikte.

### <a name="toggles-and-check-boxes"></a>Geçiş yapar ve onay kutuları

İki durumlu düğme sınıfında davranışını gibi kapsülleyen mümkündür [ `ToggleBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBehavior.cs) içinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı ve tüm tanımlayın iki durumlu tamamen XAML'de görsellerini.

[ **ToggleLabel** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ToggleLabel) örnek kullanır `ToggleBehavior` ile bir `DataTrigger` kullanmak için bir `Label` geçiş için iki metin dizesini ile.

[ **FormattedTextToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/FormattedTextToggle) örnek iki arasında geçiş yaparak bu kavramı genişletir `FormattedString` nesneleri.

[ `ToggleBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ToggleBase.cs) Sınıfını **Xamarin.FormsBook.Toolkit** kitaplığı türer `ContentView`, tanımlayan bir `IsToggled` özelliği ve bir araya getirir bir `ToggleBehavior` geçiş için mantığı. Bu iki durumlu düğme XAML'de tanımlamak tarafından gösterildiği gibi kolaylaştırır [ **TranditionalCheckBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalCheckBox) örnek.

[ **SwitchCloneDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/SwitchCloneDemo) içeren bir [ `SwitchClone` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter23/SwitchCloneDemo/SwitchCloneDemo/SwitchCloneDemo/SwitchClone.cs) öğesinden türetilen sınıf `ToggleBase` ve kullandığı bir [ `TranslateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TranslateAction.cs)Xamarin.Forms benzer iki durumlu düğme oluşturmak için sınıfı `Switch`.

A [ `RotateAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RotateAction.cs) içinde **Xamarin.FormsBook.Toolkit** bir animasyonlu seviyesini içinde yapmak için kullanılan bir animasyon sağlar [ **LeverToggle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/LeverToggle)örnek.

### <a name="responding-to-taps"></a>Dokunma için yanıt

Bir dezavantajı, `EventTrigger` kendisine eklenemez olan bir `TapGestureRecognizer` Tap'ları için yanıt. Bu soruna geçici alma amacı olan [ `TapBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/TapBehavior.cs) içinde [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)

[ **BoxViewTapShiver** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/BoxViewTapShiver) örnek kullanır `TapBehavior` önceki kullanılacak `ShiverAction` dokunduğunuz için `BoxView` öğeleri.

[ **ShiverViews** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/ShiverViews) örnek göstermektedir kapsülleyerek üzerinde biçimlendirme kesme nasıl bir `ShiverView` sınıfı.

### <a name="radio-buttons"></a>Radyo düğmeleri

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplık de sahip bir [ `RadioBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/RadioBehavior.cs) göre gruplandırılmış radyo düğmeleri sağlama sınıfı bir `string` grup adı.

[ **RadioLabels** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioLabels) program kendi radyo düğmesi için metin dizelerini kullanır. [ **RadioStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioStyle) kullanan örnek bir `Style` görünüm checked ve unchecked düğmeler arasındaki fark için. [ **RadioImages** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/RadioImages) örnek kendi radyo düğmeleri için paketlenmiş görüntüleri kullanır:

[![Radyo görüntülerinin Üçlü ekran](images/ch23fg17-small.png "radyo düğmesi görüntüleri")](images/ch23fg17-large.png "radyo düğmesi görüntüleri")

[ **TraditionalRadios** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/TraditionalRadios) örnek bir daire içinde bir nokta olan geleneksel görünmesini radyo düğmeleri çizer.

### <a name="fades-and-orientation"></a>Silinmeleri ve yönü

Son örnek [ **MultiColorSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23/MultiColorSliders) radyo düğmelerini kullanarak üç farklı renk seçim görünümleri arasında geçiş yapmanızı sağlar. Giriş ve çıkış kullanarak üç görünüm silinerek bir [ `FadeEnableAction` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/FadeEnableAction.cs) içinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) kitaplığı.

Program dikey ve yatay kullanma arasında yönlendirme değişiklikleri de yanıtlaması bir [ `GridOrientationBehavior` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/GridOrientationBehavior.cs) içinde **Xamarin.FormsBook.Toolkit** kitaplığı.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 23 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch23-Apr2016.pdf)
- [Bölüm 23 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter23)
- [Tetikleyiciler ile çalışma](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms davranışları](~/xamarin-forms/app-fundamentals/behaviors/creating.md)
