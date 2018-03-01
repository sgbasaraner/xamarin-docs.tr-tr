---
title: Tetikleyiciler
description: "XAML kullanıcı arabirimi değişikliklerle yanıtlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
ms.openlocfilehash: abfea1dae2699f7d40f2e27285cf8dab3db9400c
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="triggers"></a>Tetikleyiciler

Tetikleyiciler, olayları veya özellik değişikliklerini göre denetimlerin görünümünü değiştirme eylemleri bildirimli olarak XAML'de express olanak tanır.

Bir tetikleyici doğrudan denetime atama veya birden çok denetimlere uygulanması için bir sayfa düzeyinde veya uygulama düzeyinde kaynak sözlüğü ekleyin.

Tetikleyici dört tür vardır:

* [Özellik tetikleyici](#property) -bir denetim bir özellik için belirli bir değer ayarlandığında oluşur.

* [Veri tetikleyici](#data) - kullandığı veri başka bir denetim özelliklerine dayalı tetikleyici için bağlama.

* [Olay tetikleyicisi](#event) -denetimde bir olay ortaya çıktığında oluşur.

* [Birden çok tetikleyici](#multi) -bir eylem oluşmadan önce ayarlamak birden çok tetikleyici koşulları sağlar.

<a name="property" />

## <a name="property-triggers"></a>Özellik tetikleyicileri

Basit bir tetikleyici tamamen XAML'de ifade edilebilir ekleme bir `Trigger` bir denetimin öğesine koleksiyonu tetikler.
Bu örnek, değişen bir tetikleyici gösterir bir `Entry` arka plan rengi, odağı aldığında:

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

Tetikleyici bildirimi önemli bölümleri şunlardır:

* **TargetType** -tetikleyici uygulandığı denetim türü.

* **Özellik** -izlenen denetim özelliği.

* **Değer** -izlenen özelliği için oluştuğunda değeri, etkinleştirmek tetikleyici neden.

* **Ayarlayıcı** -bir koleksiyonu `Setter` öğeleri eklenebilir ve tetikleyici koşul karşılanıyorsa zaman. Belirtmeniz gerekir `Property` ve `Value` ayarlamak için.

* **EnterActions ve ExitActions** (gösterilmez) - kodda yazılır ve ek olarak (veya yerine) kullanılabilir `Setter` öğeleri. Bunlar [aşağıda açıklanan](#enterexit).

### <a name="applying-a-trigger-using-a-style"></a>Stil kullanarak bir tetikleyici uygulama

Tetikleyiciler da eklenebilir bir `Style` bir sayfa ya da bir uygulama bir denetim bildiriminde `ResourceDictionary`. Bu örnek örtülü bir stil bildirir (IE. hiçbir `Key` ayarlanır) yani tümüne uygulanacak `Entry` sayfadaki denetimleri.

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

Veri tetikleyicileri neden başka bir denetim izlemek için veri bağlama kullanmak `Setter`çağrılmadığı s. Yerine `Property` özniteliği özellik tetikleyici, ayarlama `Binding` özniteliği için belirtilen değer izlemek için.

Aşağıdaki örnek veri bağlama sözdizimini kullanan `{Binding Source={x:Reference entry}, Path=Text.Length}` olduğu nasıl biz başka bir denetimin özelliklerine bakın. Zaman uzunluğu `entry` sıfırsa, tetikleyici etkinleştirilir. Bu örnekte giriş boş olduğunda düğme tetikleyici devre dışı bırakır.

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

İpucu: değerlendirirken `Path=Text.Length` (ör. hedef özelliği için varsayılan değer her zaman sağla `Text=""`) Aksi durumda olacağından `null` ve tetikleyici beklendiği gibi çalışmaz.

Belirtme yanı sıra `Setter`da sağlayabilir s [ `EnterActions` ve `ExitActions` ](#enterexit).

<a name="event" />

## <a name="event-triggers"></a>Olay tetikleyicileri

`EventTrigger` Öğesi gerektiriyor yalnızca bir `Event` özelliği gibi `"Clicked"` aşağıdaki örnekte.

```xaml
<EventTrigger Event="Clicked">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Olduğuna dikkat edin hiçbir `Setter` öğeleri ancak bunun yerine bir sınıf tarafından tanımlanan bir başvuru `local:NumericValidationTriggerAction` gerektiren `xmlns:local` XAML kullanıcının sayfasında bildirilmesi için:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:WorkingWithTriggers;assembly=WorkingWithTriggers"
```

Sınıf uygulayan `TriggerAction` yani için bir geçersiz kılma sağlamalıdır `Invoke` tetikleyici olay oluştuğunda çağrılan yöntem.

Bir tetikleyici eylem uygulaması gerekir:

* Genel Uygulama `TriggerAction<T>` sınıfıyla tetikleyici uygulanacak denetim türüyle ilgili genel parametresi. Aktarılacağından gibi kullanabilir `VisualElement` denetimleri çeşitli iş ya da Denetim türü ister belirtin tetikleyici eylemleri yazmak için `Entry`.

* Geçersiz kılma `Invoke` yöntemi - tetikleme ölçütü karşılandığından her denir.

* İsteğe bağlı olarak tetikleyici bildirilmişse XAML'de ayarlanabilir özellikleri kullanıma (gibi `Anchor`, `Scale`, ve `Length` Bu örnekte).

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

Tetikleyici eylem tarafından kullanıma sunulan özellikler XAML bildiriminde aşağıdaki gibi ayarlanabilir:

```xaml
<EventTrigger Event="TextChanged">
    <local:NumericValidationTriggerAction />
</EventTrigger>
```

Tetikleyicileri paylaşırken dikkatli bir `ResourceDictionary`, bir örnek paylaşılabilecek arasında denetimleri kez yapılandırılmış herhangi bir durum tüm kendileri için geçerli şekilde.

Olay tetikleyicileri desteklemeyen Not `EnterActions` ve `ExitActions` [aşağıda açıklanan](#enterexit).   

<a name="multi" />

## <a name="multi-triggers"></a>Çoklu Tetikleyicileri

A `MultiTrigger` için benzer bir `Trigger` veya `DataTrigger` olabilir ancak birden fazla koşulu. Önce tüm koşulların karşılanması gerekir `Setter`s tetiklenir.

İki farklı girişleri bağlar bir düğme için bir tetikleyici bir örneği burada verilmiştir (`email` ve `phone`):

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

`Conditions` Koleksiyonu de içerebilir `PropertyCondition` öğeleri şöyle:

```xaml
<PropertyCondition Property="Text" Value="OK" />
```

### <a name="building-a-require-all-multi-trigger"></a>"Tüm gerektirir" birden çok tetikleyici oluşturma

Tüm koşullar doğru olduğunda çoklu yalnızca denetimiyle güncelleştirmeleri tetikleyebilir. "Tüm alan uzunlukları sıfır (burada tüm girişleri tamamlanmalıdır bir oturum açma sayfası gibi) için" test hassas bir koşul istediğinden "nerede Text.Length > 0" ancak bu yapılamıyor ifade edilir XAML'de.

Bu, yapılabilir bir `IValueConverter`. Dönüşümler dönüştürücü kodu `Text.Length` içine bağlama bir `bool` bir alanın boş olup olmadığını gösterir:


```csharp
public class MultiTriggerConverter : IValueConverter
{
    public object Convert(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        if ((int)value > 0) // length > 0 ?
            return true;            // some data has been entered
        else
            return false;           // input is empty
    }

    public object ConvertBack(object value, Type targetType,
        object parameter, CultureInfo culture)
    {
        throw new NotSupportedException ();
    }
}
```

Bu dönüştürücü birden çok tetikleyici kullanmak için önce bu sayfanın kaynak sözlüğe ekleyin (özel bir birlikte `xmlns:local` ad alanı tanımını):

```xaml
<ResourceDictionary>
   <local:MultiTriggerConverter x:Key="dataHasBeenEntered" />
</ResourceDictionary>
```

XAML aşağıda gösterilmiştir. İlk birden çok tetikleyici örnek aşağıdaki farkları dikkat edin:

* Düğmenin bulunduğu `IsEnabled="false"` varsayılan olarak ayarlayın.
* Birden çok tetikleyici koşulları dönüştürücü etkinleştirmek için kullanın. `Text.Length` bir Boole değeri.
* Tüm koşullar olduğunda `true`, kurucu düğmenin yapar `IsEnabled` özelliği `true`.

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

Bu ekran görüntüleri Yukarıdaki iki birden çok tetikleyici Örnekler arasındaki farkı gösterir. Ekranlar üst kısmında metin tek giriş `Entry` etkinleştirmek için yeterlidir **kaydetmek** düğmesi.
Ekranlar alt kısmında **oturum açma** her iki alan verileri içeren kadar düğmesi devre dışı kalır.


![](triggers-images/multi-requireall.png "MultiTrigger örnekleri")

<a name="enterexit" />

## <a name="enteractions-and-exitactions"></a>EnterActions ve ExitActions

Bir tetikleyici oluştuğunda değişiklikleri uygulamak için bir başka yolu eklemektir `EnterActions` ve `ExitActions` koleksiyonları ve belirterek `TriggerAction<T>` uygulamaları.

Size sağlayabilir *her ikisi de* `EnterActions` ve `ExitActions` yanı `Setter`s bir tetikleyici içinde ancak dikkat edin, `Setter`s hemen adlandırılır (için beklemez `EnterAction` veya `ExitAction` için tam). Alternatif olarak her şeyi kodda gerçekleştirmek ve kullanmaz `Setter`hiç s.

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

Her zaman, ne zaman bir sınıf XAML'de başvurulduğu bir ad alanı gibi bildirmelidir `xmlns:local` aşağıda gösterildiği gibi:

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

Not: `EnterActions` ve `ExitActions` üzerinde göz ardı edilir **olay tetikleyicileri**.



## <a name="related-links"></a>İlgili bağlantılar

- [Tetikleyiciler örnek](https://developer.xamarin.com/samples/WorkingWithTriggers)
- [Xamarin.Forms API belgeleri](https://developer.xamarin.com/api/type/Xamarin.Forms.TriggerAction%3CT%3E/)
