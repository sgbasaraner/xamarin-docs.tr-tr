---
title: Xamarin.Forms bağlama yolu
description: Bu makalede, alt özellik ve koleksiyon üyelerini bağlama sınıfı Path özelliği ile erişmek için Xamarin.Forms veri bağlamaları kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887a20f1791a190c182e6d179cfabb46c6e0eb48
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998953"
---
# <a name="xamarinforms-binding-path"></a>Xamarin.Forms bağlama yolu

Örneklerde tüm önceki veri bağlama, [ `Path` ](xref:Xamarin.Forms.Binding.Path) özelliği `Binding` sınıfı (veya [ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path) özelliği `Binding` işaretleme uzantısı) ayarlandı tek bir özellik için. Ayarlanacak gerçekten mümkündür `Path` için bir *alt özellik* (bir özelliğin bir özelliği) veya bir koleksiyonun üyesi.

Örneğin, sayfanız içeriyor varsayalım. bir `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` Özelliği `TimePicker` türünde `TimeSpan`, ancak belki de başvuran bir veri bağlamayı oluşturmak istediğiniz `TotalSeconds` , söz konusu özellik `TimeSpan` değeri. Veri bağlama şu şekildedir:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time` Özelliği türüdür `TimeSpan`, sahip olduğu bir `TotalSeconds` özelliği. `Time` Ve `TotalSeconds` özelliklerdir simply nokta ile bağlı. Öğeleri `Path` dize her zaman başvuran özelliklere ve bu özelliklerin türleri.

Örnek ve diğer birçok içinde gösterilen **yolu çeşitlemeleri** sayfası:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />

        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

İkinci `Label`, sayfa bağlama kaynağıdır. `Content` Özelliği türüdür `StackLayout`, sahip olduğu bir `Children` türünün özelliği `IList<View>`, sahip olduğu bir `Count` çocukların sayısını gösteren özellik.

## <a name="paths-with-indexers"></a>Dizin oluşturucular ile yolları

Üçüncü bağlamasında `Label` içinde **yolu çeşitlemeleri** sayfaları başvuruları [ `CultureInfo` ](xref:System.Globalization.CultureInfo) sınıfını `System.Globalization` ad alanı:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Kaynak statik olarak ayarlanmış `CultureInfo.CurrentCulture` türünde bir nesne özelliğinin `CultureInfo`. Sınıf adlı bir özellik tanımlar `DateTimeFormat` türü [ `DateTimeFormatInfo` ](xref:System.Globalization.DateTimeFormatInfo) içeren bir `DayNames` koleksiyonu. Dizinin dördüncü öğeyi seçer.

Dördüncü `Label` Fransa ile ilişkilendirilmiş ancak kültür için benzer bir şey yok. `Source` Bağlama özelliği `CultureInfo` bir oluşturucusuna sahip:

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

Bkz: [oluşturucu bağımsız değişkenleri geçirme](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) XAML içinde oluşturucu bağımsız değişkenleri belirtme hakkında daha fazla ayrıntı için.

Bir alt öğelerinin başvurduğu son olarak, Son örnekte ikinci, benzer `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Bu alt bir `Label`, sahip olduğu bir `Text` türünün özelliği `String`, sahip olduğu bir `Length` özelliği. İlk `Label` raporları `TimeSpan` kümesinde `TimePicker`, metni değiştiğinde, bu nedenle en son `Label` de değişir.

Üç tüm platformlarda çalışan bir program şöyledir:

[![Yol çeşitlemeleri](binding-path-images/pathvariations-small.png "yolu çeşitlemeleri")](binding-path-images/pathvariations-large.png#lightbox "yolu farklılıkları")

## <a name="debugging-complex-paths"></a>Hata ayıklama karmaşık yolları

Karmaşık yol tanımları oluşturmak zor olabilir: her bir alt özellik türü veya doğru sonraki alt özellik eklemek için koleksiyondaki öğelerin türünü bilmeniz gerekir, ancak türleri yolunda görünmez. Bir yolu artımlı olarak derleme ve Ara sonuçları görme konularında tekniktir. Son örneğin olmadan başlatabilir `Path` hiç tanımı:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Bağlama kaynağı türü görüntüleyen veya `DataBindingDemos.PathVariationsPage`. Bildiğiniz `PathVariationsPage` türetildiği `ContentPage`var. Bu nedenle bir `Content` özelliği:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Türünü `Content` özelliği olarak açığa artık `Xamarin.Forms.StackLayout`. Ekleme `Children` özelliğini `Path` ve türü `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, bir Xamarin.Forms, ancak bir koleksiyon türü açıkça iç sınıf. Bir dizin ekleyin ve türü `Xamarin.Forms.Label`. Bu şekilde devam eder.

Xamarin.Forms bağlama yolu işlerken yükler bir `PropertyChanged` uygulayan yolundaki herhangi bir nesne üzerinde işleyici `INotifyPropertyChanged` arabirimi. Örneğin, son bağlama bir değişikliğe ilk tepki verdiğini `Label` çünkü `Text` özellik değişiklikleri.

Bağlama yolu özelliğinde uygulamıyor, `INotifyPropertyChanged`, bu özellik herhangi bir değişiklik yok sayılacak. Yalnızca dize özellikleri ve alt özellikleri hiçbir zaman kaldıklarında geçersiz bu tekniği kullanması gerekir, böylece bazı değişiklikleri tamamen bağlama yolu geçersiz.



## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama tanıtımları (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölümden Xamarin.Forms kitabı](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
