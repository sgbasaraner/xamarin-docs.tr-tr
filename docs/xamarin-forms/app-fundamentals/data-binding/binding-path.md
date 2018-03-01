---
title: "Bağlama yolu"
description: "Veri bağlamaları erişim alt özellikleri ve koleksiyon üyelerine kullanın"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 39d326714a6fee1abe242a7256888647784cdec3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="binding-path"></a>Bağlama yolu

Tüm önceki örneklerde veri bağlama, [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/) özelliği `Binding` sınıfı (veya [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/) özelliği `Binding` biçimlendirme uzantısı) ayarlandı tek bir özelliğe. Ayarlamak gerçekten mümkündür `Path` için bir *alt özellik* (bir özelliği bir özellik), veya bir koleksiyonun üyesi.

Örneğin, sayfanızın bulunduğunu varsayın bir `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time` Özelliği `TimePicker` türü `TimeSpan`, ancak başvuruda bulunan bir veri bağlama oluşturmak istediğiniz belki `TotalSeconds` özelliği, `TimeSpan` değeri. Veri bağlama şöyledir:

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```
         
`Time` Özelliği türüdür `TimeSpan`, sahip olduğu bir `TotalSeconds` özelliği. `Time` Ve `TotalSeconds` özellikleri simply bir nokta ile bağlı. Öğeleri `Path` dize her zaman başvurun özellikleri ve bu özelliklerin türleri için değil.

Örnek ve diğerleri birçok'nda gösterilen **yolu Çeşitlemeler** sayfa:

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

İkinci `Label`, sayfa bağlama kaynağıdır. `Content` Özelliği türüdür `StackLayout`, sahip olduğu bir `Children` türündeki özelliği `IList<View>`, sahip olduğu bir `Count` alt sayısını gösteren özellik.

## <a name="paths-with-indexers"></a>Dizin oluşturucular ile yolları

Üçüncü bağlamasında `Label` içinde **yolu Çeşitlemeler** sayfaları başvuruları [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/) sınıfını `System.Globalization` ad alanı:

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

Kaynak statik olarak ayarlamanız `CultureInfo.CurrentCulture` türünde bir nesne olan özelliği, `CultureInfo`. Sınıf adlı bir özelliğini tanımlar `DateTimeFormat` türü [ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/) içeren bir `DayNames` koleksiyonu. Dizin dördüncü öğeyi seçer.

Dördüncü `Label` Fransa ile ilişkili ancak kültür için benzer bir şey yapar. `Source` Bağlama özelliği ayarlanmış `CultureInfo` bir oluşturucuya sahip nesne:

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

Bkz: [oluşturucu bağımsız değişkenleri geçirme](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments) XAML'de oluşturucu bağımsız değişkenleri belirtme hakkında daha fazla ayrıntı için.

Bir alt öğelerinin başvurduğu son olarak, son örnek ikinci, benzerdir `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

Bu alt bir `Label`, sahip olduğu bir `Text` türündeki özelliği `String`, sahip olduğu bir `Length` özelliği. İlk `Label` raporları `TimeSpan` kümesinde `TimePicker`, bu nedenle bu metin değiştirdiğinde, en son `Label` de değişir.

Aşağıda, tüm üç platformlarında çalışan program verilmiştir:

[![Yol Çeşitlemeler](binding-path-images/pathvariations-small.png "yolu Çeşitlemeler")](binding-path-images/pathvariations-large.png "yolu farklılıkları")

## <a name="debugging-complex-paths"></a>Hata ayıklama karmaşık yolları

Karmaşık yolu tanımları oluşturmak zor olabilir: her alt özelliğin türünü veya doğru sonraki alt özellik eklemek için koleksiyondaki öğelerin türünü bilmeniz gerekir, ancak türleri kendilerini yolunda görünmez. Bir yolu artımlı olarak derleme ve Ara sonuçlarına bakın tekniktir. Son örneğin olmadan başlamasının `Path` hiç tanımı:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

Bağlama kaynak türünü görüntüler veya `DataBindingDemos.PathVariationsPage`. Bildiğiniz `PathVariationsPage` türetilen `ContentPage`, bu nedenle bir `Content` özelliği:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

Türü `Content` özelliği olarak açığa şimdi `Xamarin.Forms.StackLayout`. Ekleme `Children` özelliğine `Path` ve türü `Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`, bir sınıf Xamarin.Forms, ancak bir koleksiyon türü için açıkça iç olduğu. Bir dizin olarak ekleyin ve türü `Xamarin.Forms.Label`. Bu şekilde devam eder.

Xamarin.Forms bağlama yolu işlerken yükleyen bir `PropertyChanged` işleyicisi uygulayan yolda herhangi bir nesne üzerinde `INotifyPropertyChanged` arabirimi. Örneğin, son bağlama değişikliği ilk tepki verdiğini `Label` çünkü `Text` özellik değişikliği. 

Bağlama yolu özelliğinde uygulamayan varsa `INotifyPropertyChanged`, bu özellik herhangi bir değişiklik yok sayılacak. Yalnızca dize özellikleri ve alt özellikleri hiçbir zaman olduğunda geçersiz Bu teknik kullanması gereken şekilde bazı değişiklikler tamamen bağlama yolu geçersiz.



## <a name="related-links"></a>İlgili bağlantılar

- [Veri bağlama gösterileri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Veri bağlama bölüm Xamarin.Forms defterinden](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
