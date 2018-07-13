---
title: Yeniden kullanılabilir EventToCommandBehavior
description: Davranışlar, komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir. Bu makalede, bir olay oluşturulduğunda çağırmak için bir Xamarin.Forms davranışı kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: EC7F6556-9776-40B8-9424-A8094482A2F3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 3151179b6ff6d26b74a87ded747310646b304603
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996327"
---
# <a name="reusable-eventtocommandbehavior"></a>Yeniden kullanılabilir EventToCommandBehavior

_Davranışlar, komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir. Bu makalede, bir olay oluşturulduğunda çağırmak için bir Xamarin.Forms davranışı kullanmayı gösterir._

## <a name="overview"></a>Genel Bakış

`EventToCommandBehavior` Sınıfı, yanıt olarak bir komut yürüttüğünde bir yeniden kullanılabilir Xamarin.Forms özel davranış *herhangi* olay tetikleyicisinin tetikleme. Varsayılan olarak, olay bağımsız değişkenleri için olay komutuna geçirilir ve isteğe bağlı olarak olabilir tarafından dönüştürülür. bir [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) uygulaması.

Davranışın kullanılabilmesi için aşağıdaki davranış özelliklerini ayarlamanız gerekir:

- **EventName** – davranışı dinleyen olayın adı.
- **Komut** – **ICommand** yürütülecek. Davranış bulmayı beklediği `ICommand` üzerinde örnek [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) bir üst öğeden devralınan ekli denetimi.

Aşağıdaki isteğe bağlı davranış özellikleri de ayarlayabilirsiniz:

- **CommandParameter** – bir `object` komutuna geçirilir.
- **Dönüştürücü** – bir [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) arasında geçirildiğinden, olay bağımsız değişkeni verilerin biçimini değiştirir uygulama *kaynak* ve *hedef*bağlama altyapısı tarafından.

## <a name="creating-the-behavior"></a>Davranışı oluşturma

`EventToCommandBehavior` Sınıf türetilir `BehaviorBase<T>` sırayla türetilen sınıf [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1) sınıfı. Amacı `BehaviorBase<T>` sınıfı, temel sınıf için gerekli Xamarin.Forms davranışları sağlamak için [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) ekli denetime ayarlamak için davranış. Bu davranışı bağlamak ve yürütme sağlar `ICommand` tarafından belirtilen `Command` davranışı kullanıldığında özelliği.

`BehaviorBase<T>` Geçersiz kılınabilir bir sağlar sınıfını [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) ayarlar yönteminin [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) davranış ve geçersiz kılınabilir bir [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))temizler yöntemi `BindingContext`. Ayrıca, eklenen denetimi başvuru sınıfı depolar `AssociatedObject` özelliği.

### <a name="implementing-bindable-properties"></a>Bağlanabilir Özellikler uygulama

`EventToCommandBehavior` Sınıfı tanımlar dört [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) bir olay oluşturulduğunda bir kullanıcının yürütme örnekleri, tanımlanan komutu. Bu özellikler aşağıdaki kod örneğinde gösterilmiştir:

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

Zaman `EventToCommandBehavior` sınıfı kullanılır, `Command` özelliği, bağlı veri olmalıdır bir `ICommand` yanıt olarak tanımlanan olay tetikleyicisinin tetikleme yürütülecek `EventName` özelliği. Davranış bulmayı beklediğinize `ICommand` üzerinde [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) ekli denetimi.

Varsayılan olarak, olayının olay değişkenlerini komutuna geçirilir. Bu veriler arasında geçirilen olarak isteğe bağlı olarak dönüştürülebilir *kaynak* ve *hedef* belirterek bağlama altyapısı tarafından bir [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) olarak uygulama `Converter` özellik değeri. Alternatif olarak, bir parametre komutu belirterek geçirilebilir `CommandParameter` özellik değeri.

### <a name="implementing-the-overrides"></a>Geçersiz kılmaları uygulama

`EventToCommandBehavior` Sınıf geçersiz kılmalarını [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) ve [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) yöntemlerinin `BehaviorBase<T>` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) Yöntemi çağırarak kurulum gerçekleştirir `RegisterEvent` yöntemini değerinde `EventName` özelliği bir parametre olarak. [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) Yöntemi çağırarak temizleme gerçekleştirir `DeregisterEvent` yöntemini değerinde `EventName` özelliği bir parametre olarak.

### <a name="implementing-the-behavior-functionality"></a>Davranış işlevselliği uygulama

Tarafından tanımlanan komutu yürütmek için davranış amacı olan `Command` özelliği tarafından tanımlanan olay tetikleyicisinin tetikleme yanıtta `EventName` özelliği. Çekirdek davranışı işlevselliği aşağıdaki kod örneğinde gösterilmiştir:

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

`RegisterEvent` Yanıt olarak yöntemi yürütüldüğünde `EventToCommandBehavior` değerini alan bir denetim ve bu iliştirilmekte `EventName` özelliği bir parametre olarak. Yöntemi ardından tanımlanan olay bulmaya çalışır `EventName` ekli denetimi özelliği. Olay bulunabilir olması koşuluyla `OnEvent` yöntemi için olay işleyicisi yöntemi olarak kaydedilir.

`OnEvent` Yanıt olarak tanımlanan olay tetikleyicisinin tetikleme yöntemi yürütüldüğünde `EventName` özelliği. Koşuluyla `Command` özelliğine geçerli bir başvuruyor `ICommand`, yöntemin bir parametre geçirmek için almayı dener `ICommand` gibi:

- Varsa `CommandParameter` özelliği tanımlayan bir parametre, elde edilir.
- Aksi takdirde `Converter` özelliği tanımlayan bir [ `IValueConverter` ](xref:Xamarin.Forms.IValueConverter) uygulama, dönüştürücü yürütülür ve arasında geçirilen olarak olay bağımsız değişkeni verileri dönüştürür *kaynak* ve *hedef* bağlama altyapısı tarafından.
- Aksi takdirde, olay bağımsız değişkenleri parametre olarak kabul edilir.

Veri bağlama `ICommand` , parametreyi koşuluyla komutuna geçirerek yürütülür [ `CanExecute` ](xref:Xamarin.Forms.Command.CanExecute(System.Object)) yöntemi döndürür `true`.

Burada, gösterilmese `EventToCommandBehavior` de içeren bir `DeregisterEvent` tarafından yürütülen yöntemi [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) yöntemi. `DeregisterEvent` Bulun ve içinde tanımlanan olay kaydını silmek için kullanılan yöntemi `EventName` özelliğini, diğer olası bellek sızıntısı temizleme.

## <a name="consuming-the-behavior"></a>Davranış kullanma

`EventToCommandBehavior` Sınıfı için eklenebilir [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors) aşağıdaki XAML kod örneğinde gösterildiği gibi bir denetim koleksiyonu:

```xaml
<ListView ItemsSource="{Binding People}">
  <ListView.Behaviors>
    <local:EventToCommandBehavior EventName="ItemSelected" Command="{Binding OutputAgeCommand}"
                                  Converter="{StaticResource SelectedItemConverter}" />
  </ListView.Behaviors>
</ListView>
<Label Text="{Binding SelectedItemText}" />
```

Eşdeğer C# kodu aşağıdaki kod örneğinde gösterilmiştir:

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

`Command` Özelliğidir davranışı, bağlı veri `OutputAgeCommand` ilişkili ViewModel özelliği sırada `Converter` özelliği `SelectedItemConverter` örneğini döndüren [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem), [ `ListView` ](xref:Xamarin.Forms.ListView) gelen [ `SelectedItemChangedEventArgs` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs).

Çalışma zamanında davranış etkileşim denetimi ile yanıt verecektir. İçinde bir öğe seçildiğinde [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) olay yangın, hangi yürütecek `OutputAgeCommand` ViewModel içinde. Sırayla bu ViewModel güncelleştirir `SelectedItemText` özelliği, [ `Label` ](xref:Xamarin.Forms.Label) için aşağıdaki ekran görüntülerinde gösterildiği gibi bağlanır:

[![](event-to-command-behavior-images/screenshots-sml.png "Örnek uygulama ile EventToCommandBehavior")](event-to-command-behavior-images/screenshots.png#lightbox "örnek EventToCommandBehavior ile uygulama")

Bir olay oluşturulduğunda, bir komut çalıştırmak için bu davranışı kullanmanın avantajı olan komutları komutları ile etkileşime geçmek için tasarlanmış olmayan denetimleri ile ilişkili olabilir. Ayrıca, bu kazan blondan olay işleme kodu arka plan kod dosyaları kaldırır.

## <a name="summary"></a>Özet

Bu makalede, bir olay oluşturulduğunda çağırmak için bir Xamarin.Forms davranışını kullanarak gösterdik. Davranışlar, komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri komutlarını ilişkilendirmek için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [EventToCommand davranışı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/eventtocommandbehavior/)
- [Davranışı](xref:Xamarin.Forms.Behavior)
- [Davranışı<T>](xref:Xamarin.Forms.Behavior`1)
