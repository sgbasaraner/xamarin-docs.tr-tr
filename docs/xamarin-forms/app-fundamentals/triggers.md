---
title: Xamarin.Forms Tetikleyicileri
description: Bu makalede, kullanıcı arabirimi değişiklikleri XAML ile yanıt vermek için Xamarin.Forms Tetikleyicileri kullanmayı açıklar. Tetikleyici olayları veya özellik değişikliklerine dayanan denetimlerin görünümünü değiştirme eylemleri bildirimli olarak XAML içinde express olanak sağlar.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: 954a0967e034e0321964e12ca0725ae2a85e3bc6
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995543"
---
# <a name="xamarinforms-triggers"></a>Xamarin.Forms Tetikleyicileri

Tetikleyici olayları veya özellik değişikliklerine dayanan denetimlerin görünümünü değiştirme eylemleri bildirimli olarak XAML içinde express olanak sağlar.

Bir tetikleyici bir denetime doğrudan atayabilir veya birden çok denetimlere uygulanması için bir sayfa düzeyinde veya uygulama düzeyinde bir kaynak sözlüğüne ekleyin.

Dört tür tetikleyici vardır:

* [Özellik tetikleyicisi](#property) -denetim üzerinde bir özelliği belirli bir değere ayarlandığında gerçekleşir.

* [Veri tetikleyici](#data) - tetiklenecek üzerinde başka bir denetimin özelliklerini bağlama veriler kullanıyor.

* [Olay tetikleyicisi](#event) -denetimde bir olay oluştuğunda gerçekleşir.

* [Birden çok tetikleyici](#multi) -bir eylem oluşmadan önce ayarlamak birden çok tetikleyici koşulu izin verir.

<a name="property" />

## <a name="property-triggers"></a>Özellik tetikleyicileri

Basit bir tetikleyici tamamen XAML ifade edilebilir ekleyerek bir `Trigger` denetim için öğe koleksiyonu tetikler.
Bu örnek, değişen bir tetikleyici gösterir. bir `Entry` arka plan rengi, odağı aldığında:

```xaml
<Entry Placeholder="enter name">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
             Property="IsFocused" Value="True">
            <Setter Property="BackgroundColor" Value="Yellow" />
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Tetikleyicinin bildirimi önemli bölümleri şunlardır:

* **TargetType** -tetikleyici uygulandığı bir denetim türü.

* **Özellik** -izlenen denetim özelliği.

* **Değer** -izlenen özelliği için meydana geldiğinde değeri, etkinleştirmek tetikleyici neden.

* **Ayarlayıcı** -bir koleksiyonu `Setter` öğeleri eklenebilir ve tetikleyici koşul karşılanıyorsa zaman. Belirtmelisiniz `Property` ve `Value` ayarlamak için.

* **EnterActions ve ExitActions** (gösterilmez) - kod halinde yazılmış ve ek olarak (veya yerine) kullanılabilir `Setter` öğeleri. Bunlar [aşağıda açıklanan](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Bir stil kullanarak bir tetikleyici uygulama

Tetikleyici de eklenebilir bir `Style` bir denetimi bir sayfası veya uygulama bildiriminde `ResourceDictionary`. Bu örnek, bir örtük stil bildirir (IE. hiçbir `Key` ayarlanır) anlamına gelir tümüne uygulanacak `Entry` sayfadaki denetimleri.

```xaml
<ContentPage.Resources>
    <ResourceDictionary>
        <Style TargetType="Entry">
                        <Style.Triggers>
                <Trigger TargetType="Entry"
                         Property="IsFocused" Value="True">
                    <Setter Property="BackgroundColor" Value="Yellow" />
                </Trigger>
            </Style.Triggers>
        </Style>
    </ResourceDictionary>
</ContentPage.Resources>
```

<a name="data" />

## <a name="data-triggers"></a>Veri tetikleyicileri

Veri tetikleyicilerini neden başka bir denetimde izlemek için veri bağlamasını kullanma `Setter`çağrılmadığı s. Yerine `Property` öznitelik bir özellik tetikleyicisi, Ayarla `Binding` özniteliği için belirtilen değer izlemek için.

Aşağıdaki örnekte veri bağlama sözdizimini kullanan `{Binding Source={x:Reference entry}, Path=Text.Length}` olduğu başka bir denetimin özelliklerini nasıl diyoruz. Zaman uzunluğunu `entry` sıfırsa, tetikleyici etkinleştirildi. Bu örnekte, giriş boş olduğunda düğme tetikleyici devre dışı bırakır.

```xaml
<!-- the x:Name is referenced below in DataTrigger-->
<!-- tip: make sure to set the Text="" (or some other default) -->
<Entry x:Name="entry"
       Text=""
       Placeholder="required field" />

<Button x:Name="button" Text="Save"
        FontSize="Large"
        HorizontalOptions="Center">
    <Button.Triggers>
        <DataTrigger TargetType="Button"
                     Binding="{Binding Source={x:Reference entry},
                                       Path=Text.Length}"
                     Value="0">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </Button.Triggers>
</Button>
```

İpucu: değerlendirirken `Path=Text.Length` bir varsayılan değer (ör. hedef özelliği için her zaman sağla `Text=""`) Aksi durumda olacağından `null` ve tetikleyici beklendiği gibi çalışmaz.

Yanı sıra belirtmeyi `Setter`da sağlayabilir s [ `EnterActions` ve `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Olay tetikleyicileri

`EventTrigger` Öğesi gerektiriyor yalnızca bir `Event` özelliği gibi `"Clicked"` aşağıdaki örnekte.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Olduğuna dikkat edin hiçbir `Setter` öğeleri ancak bunun yerine bir sınıf tarafından tanımlanan başvuru `local:NumericValidationTriggerAction` gerektiren `xmlns:local` XAML kullanıcının sayfasında bildirilmesi için:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Sınıfın uyguladığı `TriggerAction` anlamına gelir için bir geçersiz kılma sağlamalıdır `Invoke` tetikleyici olay her gerçekleştiğinde çağrılan yöntem.

Bir tetikleyici eylem uygulaması gerekir:

* Genel Uygulama `TriggerAction<T>` sınıfıyla tetikleyici uygulanacak denetim türü ile ilgili genel parametre. Aktarılabileceği gibi kullanabileceğiniz `VisualElement` çeşitli denetimleri ile çalışmak veya bir denetim türü ister belirtin tetikleyici eylemleri yazılacak `Entry`.

* Geçersiz kılma `Invoke` yöntemi - bu çağrılır tetikleyici ölçütleri karşılandığında olduğunda.

* Tetikleyici bildirildiğinde XAML içinde ayarlanabilir özellikleri isteğe bağlı olarak kullanıma sunma (gibi `Anchor`, `Scale`, ve `Length` Bu örnekte).

```csharp
public class NumericValidationTriggerAction : TriggerAction<Entry>
{
    protected override void Invoke (Entry entry)
    {
        double result;
        bool isValid = Double.TryParse (entry.Text, out result);
        entry.TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

Tetikleyici eylem tarafından kullanıma sunulan özellikleri XAML bildiriminde aşağıdaki gibi ayarlanabilir:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Tetikleyiciler paylaşırken dikkatli bir `ResourceDictionary`, bir kez yapılandırılmış herhangi bir durumu, tüm bunları uygulanır denetimler arasında paylaşılır.

Olay tetikleyicileri desteklemeyen unutmayın `EnterActions` ve `ExitActions` [aşağıda açıklanan](#enterexit).    

<a name="multi" />

## <a name="multi-triggers"></a>Çoklu Tetikleyicileri

A `MultiTrigger` için benzer bir `Trigger` veya `DataTrigger` olabilir ancak birden fazla koşul. Önce tüm koşulların karşılanması gerekir `Setter`s tetiklenir.

İşte bir örnek için iki farklı girişe bağlanan bir düğme için bir tetikleyici (`email` ve `phone`):

```xaml
<MultiTrigger TargetType="Button">
    <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference email},
                                   Path=Text.Length}"
                               Value="0" />
        <BindingCondition Binding="{Binding Source={x:Reference phone},
                                   Path=Text.Length}"
                               Value="0" />
    </MultiTrigger.Conditions>

  <Setter Property="IsEnabled" Value="False" />
    <!-- multiple Setter elements are allowed -->
</MultiTrigger>
```

`Conditions` Koleksiyonu içerebilir ayrıca `PropertyCondition` bu gibi öğeler:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>"Tüm gerektirir" çoklu tetiği oluşturma

Tüm koşullar doğru olduğunda birden çok tetikleyici yalnızca denetimiyle güncelleştirir. "Tüm alan uzunluğu sıfır (burada tüm girişleri tamamlanmalıdır bir oturum açma sayfası gibi) için" test zor bir koşul istediğinden "nerede Text.Length > 0" ancak bu ifade edilemez XAML içinde.

Bu ile yapılabilir bir `IValueConverter`. Dönüşümler aşağıda dönüştürücü kod `Text.Length` uygulamasına bağlama bir `bool` bir alan boş olup olmadığını gösterir:


```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;            // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Bu dönüştürücü birden çok tetikleyici kullanmak için önce bu sayfanın kaynak sözlüğüne ekleyin (özel bir birlikte `xmlns:local` ad alanı tanımını):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML aşağıda gösterilmiştir. İlk birden çok tetikleyici örnekte aşağıdaki farklılıklara dikkat edin:

* Düğmeye sahip `IsEnabled="false"` varsayılan olarak ayarlayın.
* Çoklu tetikleme koşullarını Dönüştürücüsü kullanmayı `Text.Length` bir Boole değeri.
* Tüm koşullar olduğunda `true`, ayarlayıcı düğmenin yapar `IsEnabled` özelliği `true`.

```xaml
<Entry x:Name="user" Text="" Placeholder="user name" />

<Entry x:Name="pwd" Text="" Placeholder="password" />

<Button x:Name="loginButton" Text="Login"
        FontSize="Large"
        HorizontalOptions="Center"
        IsEnabled="false">
  <Button.Triggers>
    <MultiTrigger TargetType="Button">
      <MultiTrigger.Conditions>
        <BindingCondition Binding="{Binding Source={x:Reference user},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
        <BindingCondition Binding="{Binding Source={x:Reference pwd},
                              Path=Text.Length,
                              Converter={StaticResource dataHasBeenEntered}}"
                          Value="true" />
      </MultiTrigger.Conditions>
      <Setter Property="IsEnabled" Value="True" />
    </MultiTrigger>
  </Button.Triggers>
</Button>
```

Bu ekran görüntüleri, yukarıdaki iki birden çok tetikleyici örnekleri arasındaki farkı gösterir. Ekranlar üst kısmında, metin girişi tek `Entry` etkinleştirmek için **Kaydet** düğmesi.
Ekranlar alt kısmında **oturum açma** hem alanların veri içermesini kadar düğmesini devre dışı kalır.


![](triggers-images/multi-requireall.png "MultiTrigger örnekleri")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions ve ExitActions

Ekleyerek bir tetikleyici oluştuğunda değişiklikleri uygulamak için başka bir yolu ise `EnterActions` ve `ExitActions` koleksiyonları ve belirterek `TriggerAction<T>` uygulamaları.

Sağlayabilirsiniz *hem* `EnterActions` ve `ExitActions` yanı `Setter`s bir tetikleyici ancak dikkat edin, `Setter`s hemen çağrılır (için beklemeyin `EnterAction` veya `ExitAction` için Tamamla). Alternatif olarak her şeyi kodda gerçekleştirmek ve kullanılmayan `Setter`hiç s.

```xaml
<Entry Placeholder="enter job title">
    <Entry.Triggers>
        <Trigger TargetType="Entry"
                 Property="Entry.IsFocused" Value="True">
            <Trigger.EnterActions>
                <local:FadeTriggerAction StartsFrom="0"" />
            </Trigger.EnterActions>

            <Trigger.ExitActions>
                <local:FadeTriggerAction StartsFrom="1" />
            </Trigger.ExitActions>
                        <!-- You can use both Enter/Exit and Setter together if required -->
        </Trigger>
    </Entry.Triggers>
</Entry>
```

Her zaman, bir sınıf içinde XAML ne zaman başvurulduğundan gibi bir ad alanı gibi bildirmelidir `xmlns:local` burada gösterildiği gibi:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

`FadeTriggerAction` Kod aşağıda gösterilmektedir:

```csharp
public class FadeTriggerAction : TriggerAction<VisualElement>
{
    public FadeTriggerAction() {}

    public int StartsFrom { set; get; }

    protected override void Invoke (VisualElement visual)
    {
            visual.Animate("", new Animation( (d)=>{
                var val = StartsFrom==1 ? d : 1-d;
                visual.BackgroundColor = Color.FromRgb(1, val, 1);

            }),
            length:1000, // milliseconds
            easing: Easing.Linear);
    }
}
```

Not: `EnterActions` ve `ExitActions` üzerinde göz ardı edilir **olay tetikleyicilerini**.



## <a name="related-links"></a>İlgili bağlantılar

- [Tetikleyiciler örnek](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms API belgeleri](xref:Xamarin.Forms.TriggerAction`1)
