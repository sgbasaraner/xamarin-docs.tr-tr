---
title: Reusable EventToCommandBehavior
description: "Davranışları komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir. Bu makalede, bir olay başlatıldığında bir komut çağrılacak Xamarin.Forms davranış kullanarak gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: d82a1391feca9187cf2aca4394509447aeac6a18
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="reusable-eventtocommandbehavior"></a>Reusable EventToCommandBehavior

_Davranışları komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir. Bu makalede, bir olay başlatıldığında bir komut çağrılacak Xamarin.Forms davranış kullanarak gösterilmektedir._

## <a name="overview"></a>Genel Bakış

`EventToCommandBehavior` Sınıftır yanıt olarak bir komut yürüten yeniden kullanılabilir bir özel Xamarin.Forms davranışı *herhangi* olay tetikleme. Olay komutu geçirilir ve isteğe bağlı olarak olabilir varsayılan olarak, olay bağımsız dönüştürülen bir [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) uygulaması.

Aşağıdaki davranışı özellikleri davranışı kullanacak şekilde ayarlamanız gerekir:

- **EventName** – olayın adı davranışı dinler.
- **Komut** – **ICommand** yürütülecek. Bulunacak davranışı bekliyor `ICommand` üzerinde örnek [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) bir üst öğesinden devralınan ekli denetimi.

Aşağıdaki isteğe bağlı davranışı özellikler de ayarlayabilirsiniz:

- **CommandParameter** – bir `object` Command'e geçilecek.
- **Dönüştürücü** – bir [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) arasında geçirildiğinden, olay bağımsız değişken veri biçimi değişir uygulama *kaynak* ve *hedef*bağlama altyapısı tarafından.

## <a name="creating-the-behavior"></a>Davranışı oluşturma

`EventToCommandBehavior` Sınıfı türer `BehaviorBase<T>` sırayla türeyen sınıf [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı. Amacı `BehaviorBase<T>` sınıfı, bir taban sınıfı için gerektiren Xamarin.Forms davranışları sağlamak için [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) ekli denetimine ayarlanacak davranış. Bu davranış bağlamak ve yürütme sağlar `ICommand` tarafından belirtilen `Command` davranışı kullanıldığında özelliği.

`BehaviorBase<T>` SAX bir overridable [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) ayarlar yöntemi [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) davranışı ve bir overridable [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)temizler yöntemi `BindingContext`. Ayrıca, ekli denetiminde başvuru sınıfı depolar `AssociatedObject` özelliği.

### <a name="implementing-bindable-properties"></a>Bağlanabilir özelliklerini uygulama

`EventToCommandBehavior` Sınıfı tanımlayan dört [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/) bir olay başlatıldığında kullanıcı yürütme örnekleri, tanımlanan komutu. Bu özellikler, aşağıdaki kod örneğinde gösterilir:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  public static readonly BindableProperty EventNameProperty =
    BindableProperty.Create ("EventName", typeof(string), typeof(EventToCommandBehavior), null, propertyChanged: OnEventNameChanged);
  public static readonly BindableProperty CommandProperty =
    BindableProperty.Create ("Command", typeof(ICommand), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty CommandParameterProperty =
    BindableProperty.Create ("CommandParameter", typeof(object), typeof(EventToCommandBehavior), null);
  public static readonly BindableProperty InputConverterProperty =
    BindableProperty.Create ("Converter", typeof(IValueConverter), typeof(EventToCommandBehavior), null);

  public string EventName { ... }
  public ICommand Command { ... }
  public object CommandParameter { ... }
  public IValueConverter Converter { ...  }
  ...
}
```

Zaman `EventToCommandBehavior` sınıfı tüketilen, `Command` özelliği, bağlı veri olmalıdır bir `ICommand` yanıt olarak tanımlanan olay tetikleme yürütülecek `EventName` özelliği. Davranış bulmayı beklediğinize `ICommand` üzerinde [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) ekli denetimi.

Varsayılan olarak, olayının olay değişkenlerini komutu geçirilir. Arasında geçirildiğinden, isteğe bağlı olarak bu verileri dönüştürülebilir *kaynak* ve *hedef* belirterek bağlama altyapısı tarafından bir [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) olarak uygulaması `Converter` özellik değeri. Alternatif olarak, bir parametre komutu belirterek geçirilebilir `CommandParameter` özellik değeri.

### <a name="implementing-the-overrides"></a>Geçersiz kılmaları uygulama

`EventToCommandBehavior` Geçersiz kılmaları sınıf [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) ve [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) yöntemlerinin `BehaviorBase<T>` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  protected override void OnAttachedTo (View bindable)
  {
    base.OnAttachedTo (bindable);
    RegisterEvent (EventName);
  }

  protected override void OnDetachingFrom (View bindable)
  {
    DeregisterEvent (EventName);
    base.OnDetachingFrom (bindable);
  }
  ...
}
```

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/) Yöntemi çağrılarak kurulum gerçekleştirir `RegisterEvent` değerinde geçirerek yöntemini `EventName` bir parametre olarak özellik. [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) Yöntemi çağrılarak temizleme gerçekleştirir `DeregisterEvent` değerinde geçirerek yöntemini `EventName` bir parametre olarak özellik.

### <a name="implementing-the-behavior-functionality"></a>Davranış işlevlerini uygulama

Davranış amacı tarafından tanımlanan komutu yürütün etmektir `Command` tarafından tanımlanan olay tetikleme yanıta özelliğinde `EventName` özelliği. Çekirdek davranışı işlevselliği, aşağıdaki kod örneğinde gösterilir:

```csharp
public class EventToCommandBehavior : BehaviorBase<View>
{
  ...
  void RegisterEvent (string name)
  {
    if (string.IsNullOrWhiteSpace (name)) {
      return;
    }

    EventInfo eventInfo = AssociatedObject.GetType ().GetRuntimeEvent (name);
    if (eventInfo == null) {
      throw new ArgumentException (string.Format ("EventToCommandBehavior: Can't register the '{0}' event.", EventName));
    }
    MethodInfo methodInfo = typeof(EventToCommandBehavior).GetTypeInfo ().GetDeclaredMethod ("OnEvent");
    eventHandler = methodInfo.CreateDelegate (eventInfo.EventHandlerType, this);
    eventInfo.AddEventHandler (AssociatedObject, eventHandler);
  }

  void OnEvent (object sender, object eventArgs)
  {
    if (Command == null) {
      return;
    }

    object resolvedParameter;
    if (CommandParameter != null) {
      resolvedParameter = CommandParameter;
    } else if (Converter != null) {
      resolvedParameter = Converter.Convert (eventArgs, typeof(object), null, null);
    } else {
      resolvedParameter = eventArgs;
    }       

    if (Command.CanExecute (resolvedParameter)) {
      Command.Execute (resolvedParameter);
    }
  }
  ...
}
```

`RegisterEvent` Yanıt olarak yöntemi yürütüldüğünde `EventToCommandBehavior` değerini alan bir denetim ve bu iliştirilmekte `EventName` bir parametre olarak özellik. Yönteminin ardından tanımlanan olay bulmayı dener `EventName` ekli denetimi özelliği. Olay bulunabilir olması koşuluyla `OnEvent` yöntemi olay işleyicisi yöntemi olarak kaydedilir.

`OnEvent` Yanıt olarak tanımlanan olay tetikleme yöntemi yürütüldüğünde `EventName` özelliği. Koşuluyla `Command` özelliği geçerli bir başvurur `ICommand`, yöntemi bir parametre geçirilecek almaya çalışır `ICommand` gibi:

- Varsa `CommandParameter` özelliği tanımlayan bir parametre, onu alınır.
- Aksi halde, eğer `Converter` özelliği tanımlayan bir [ `IValueConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.IValueConverter/) uygulama, dönüştürücü yürütülür ve olay bağımsız değişken veri arasında geçti olarak dönüştürür *kaynak* ve *hedef* bağlama altyapısı tarafından.
- Aksi takdirde, olay bağımsız parametresi olarak kabul edilir.

Veri bağlama `ICommand` , parametre, sağlanan komutuna geçirme yürütülür [ `CanExecute` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Command.CanExecute/p/System.Object/) yöntemi döndürür `true`.

Burada, gösterilmese `EventToCommandBehavior` de içeren bir `DeregisterEvent` tarafından yürütülen yöntemi [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) yöntemi. `DeregisterEvent` Yöntemi bulun ve içinde tanımlanan olay kaydını silmek için kullanılan `EventName` tüm olası bellek sızıntıları temizleme özelliği.

## <a name="consuming-the-behavior"></a>Davranış kullanma

`EventToCommandBehavior` Sınıfı için eklenebilir [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/) aşağıdaki XAML kod örneğinde gösterildiği gibi bir denetim koleksiyonu:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilir:

```csharp
var listView = new ListView ();
listView.SetBinding (ItemsView<Cell>.ItemsSourceProperty, "People");
listView.Behaviors.Add (new EventToCommandBehavior {
  EventName = "ItemSelected",
  Command = ((HomePageViewModel)BindingContext).OutputAgeCommand,
  Converter = new SelectedItemEventArgsToSelectedItemConverter ()
});

var selectedItemLabel = new Label ();
selectedItemLabel.SetBinding (Label.TextProperty, "SelectedItemText");
```

`Command` Davranışı özelliğidir bağlı veri `OutputAgeCommand` ilişkili ViewModel özelliğinin sırada `Converter` özelliği ayarlanmış `SelectedItemConverter` örneği, döndüren [ `SelectedItem` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.SelectedItem/), [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) gelen [ `SelectedItemChangedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.SelectedItemChangedEventArgs/).

Çalışma zamanında denetimi ile etkileşim davranışı yanıtlar. İçinde bir öğe seçildiğinde [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), [ `ItemSelected` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemSelected/) olay yangın, hangi yürütecek `OutputAgeCommand` ViewModel içinde. Sırayla bu ViewModel güncelleştirmeleri `SelectedItemText` özelliği, [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) için aşağıdaki ekran görüntülerinde gösterildiği gibi bağlar:

[![](event-to-command-behavior-images/screenshots-sml.png "Örnek uygulama EventToCommandBehavior ile")](event-to-command-behavior-images/screenshots.png#lightbox "örnek EventToCommandBehavior ile uygulama")

Bir olay başlatıldığında, bir komut çalıştırmak için bu davranış kullanmanın avantajı olan komutları komutları ile etkileşim kurmak için tasarlanmış doğru denetimleri ile ilişkili olabilir. Ayrıca, bu kazan kalıbı olay işleme kodu arka plan kodu dosyalarından kaldırır.

## <a name="summary"></a>Özet

Bu makalede, bir olay başlatıldığında bir komut çağrılacak Xamarin.Forms davranış kullanarak gösterilmektedir. Davranışları komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [EventToCommand davranışı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Behavior](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [Davranışı<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
