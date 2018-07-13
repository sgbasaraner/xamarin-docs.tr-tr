---
title: XAML bağımsız değişkenleri geçirme
description: Bu makalede, varsayılan olmayan Oluşturucular, Fabrika yöntemleri çağırmak için ve genel bir bağımsız değişken türünü belirtmek için bağımsız değişkenleri geçirmek için kullanılan XAML öznitelikleri kullanmayı gösterir.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: 51b72d9143895543715c519a65cf8c82aa4d12f7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996554"
---
# <a name="passing-arguments-in-xaml"></a>XAML bağımsız değişkenleri geçirme

_Bu makalede, varsayılan olmayan Oluşturucular, Fabrika yöntemleri çağırmak için ve genel bir bağımsız değişken türünü belirtmek için bağımsız değişkenleri geçirmek için kullanılan XAML öznitelikleri kullanmayı gösterir._

## <a name="overview"></a>Genel Bakış

Genellikle, bağımsız değişken gerektirir oluşturuculara sahip ya da statik oluşturma yöntemini çağırarak nesneleri somutlaştırmak gereklidir. XAML içinde bu gerçekleştirilebilir kullanarak `x:Arguments` ve `x:FactoryMethod` öznitelikleri:

- `x:Arguments` Özniteliği oluşturucu bağımsız varsayılan olmayan bir oluşturucu ya da bir fabrika yöntemi nesne bildirimi belirtmek için kullanılır. Daha fazla bilgi için [oluşturucu bağımsız değişkenleri geçirme](#constructor_arguments).
- `x:FactoryMethod` Öznitelik, bir nesneyi başlatmak için kullanılan bir Üreteç yöntemi belirtmek için kullanılır. Daha fazla bilgi için [Fabrika yöntemleri çağırma](#factory_methods).

Ayrıca, `x:TypeArguments` öznitelik, oluşturucuya genel bir türün genel tür bağımsız değişkenlerini belirtmek için kullanılabilir. Daha fazla bilgi için [genel tür bağımsız değişkeni belirterek](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Oluşturucu bağımsız değişkenleri geçirme

Bağımsız değişkenleri için varsayılan olmayan oluşturucu kullanılarak geçirilebilir `x:Arguments` özniteliği. Her oluşturucu bağımsız değişken bağımsız değişken türünü temsil eden bir XML öğesi içinde virgülle ayrılmış olmalıdır. Xamarin.Forms, temel türleri için şu öğeleri destekler:

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

Aşağıdaki kod örneği kullanmayı gösterir `x:Arguments` üç özniteliğiyle [ `Color` ](xref:Xamarin.Forms.Color) oluşturucular:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.9</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.25</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.75</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color>
      <x:Arguments>
        <x:Double>0.8</x:Double>
        <x:Double>0.5</x:Double>
        <x:Double>0.2</x:Double>
        <x:Double>0.5</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

İçindeki öğe sayısını `x:Arguments` etiketi ve bu öğe türleri eşleşmelidir birini [ `Color` ](xref:Xamarin.Forms.Color) oluşturucular. `Color` [Oluşturucusu](xref:Xamarin.Forms.Color.%23ctor(System.Double)) gri tonlamalı değeri 0 (siyah) 1 (beyaz) ile tek bir parametre gerektirir. `Color` [Oluşturucusu](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double)) ile üç parametreyi bir kırmızı, yeşil ve mavi değeri 0 ile 1 arasında gerektirir. `Color` [Oluşturucusu](xref:Xamarin.Forms.Color.%23ctor(System.Double,System.Double,System.Double,System.Double)) dört parametrelerle bir alfa kanalı dördüncü parametre olarak ekler.

Aşağıdaki ekran görüntüleri her çağırma sonucu göster [ `Color` ](xref:Xamarin.Forms.Color) Oluşturucusu ile belirtilen bağımsız değişken değerleri:

![](passing-arguments-images/passing-arguments.png "X: Arguments'ile belirtilen BoxView.Color")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Fabrika yöntemleri çağırma

Fabrika yöntemleri çağrıldığında XAML yöntemin belirterek kullanarak ad `x:FactoryMethod` özniteliği ve bağımsız değişkenleri kullanarak `x:Arguments` özniteliği. Bir Üreteç yöntemi olan bir `public static` nesneler veya değerleri aynı türde bir sınıf ya da yöntemlerini yapı döndüren yöntem.

[ `Color` ](xref:Xamarin.Forms.Color) Yapısını tanımlayan bir dizi Fabrika yöntemleri ve bunları çağırma üç aşağıdaki kod örneği gösterilmektedir:

```xaml
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromRgba">
      <x:Arguments>
        <x:Int32>192</x:Int32>
        <x:Int32>75</x:Int32>
        <x:Int32>150</x:Int32>                        
        <x:Int32>128</x:Int32>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHsla">
      <x:Arguments>
        <x:Double>0.23</x:Double>
        <x:Double>0.42</x:Double>
        <x:Double>0.69</x:Double>
        <x:Double>0.7</x:Double>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
<BoxView HeightRequest="150" WidthRequest="150" HorizontalOptions="Center">
  <BoxView.Color>
    <Color x:FactoryMethod="FromHex">
      <x:Arguments>
        <x:String>#FF048B9A</x:String>
      </x:Arguments>
    </Color>
  </BoxView.Color>
</BoxView>
```

İçindeki öğe sayısını `x:Arguments` etiketi ve bu öğeleri türde bağımsız değişkenleri çağrılan Üreteç yöntemi eşleşmesi gerekir. [ `FromRgba` ](xref:Xamarin.Forms.Color.FromRgba(System.Int32,System.Int32,System.Int32,System.Int32)) Üreteç yöntemi gerektiren dört [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) temsil eden 0 ile 255 sırasıyla arasında kırmızı, yeşil, mavi ve alfa değerleri, parametreleri. [ `FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) Üreteç yöntemi gerektiren dört [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) ton, Doygunluk, parlaklık ve alfa değerleri, 0-1 olarak sırasıyla arasında temsil eden parametreleri. [ `FromHex` ](xref:Xamarin.Forms.Color.FromHex(System.String)) Üreteç yöntemi gerektiren bir [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) temsil eden bir onaltılık RGB rengi (A).

Aşağıdaki ekran görüntüleri her çağırma sonucu göster [ `Color` ](xref:Xamarin.Forms.Color) Üreteç yöntemi ile belirtilen bağımsız değişken değerleri:

![](passing-arguments-images/factory-methods.png "X: FactoryMethod ve x: Arguments'ile belirtilen BoxView.Color")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Genel tür bağımsız değişkeni belirtme

Genel tür bağımsız değişkenleri için genel bir türün oluşturucu kullanılarak belirtilebilir `x:TypeArguments` aşağıdaki kod örneğinde gösterildiği gibi öznitelik:

```xaml
<ContentPage ...>
  <StackLayout>
    <StackLayout.Margin>
      <OnPlatform x:TypeArguments="Thickness">
        <On Platform="iOS" Value="0,20,0,0" />
        <On Platform="Android" Value="5, 10" />
        <On Platform="UWP" Value="10" />
      </OnPlatform>
    </StackLayout.Margin>
  </StackLayout>
</ContentPage>
```

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) Sınıf bir genel sınıftır ve ile Değişkensiz bir `x:TypeArguments` hedef türüyle eşleşen öznitelik. İçinde [ `On` ](xref:Xamarin.Forms.On) sınıfı [ `Platform` ](xref:Xamarin.Forms.On.Platform) özniteliği tek bir kabul edebilir `string` değeri veya virgülle ayrılmış birden çok `string` değerleri. Bu örnekte, [ `StackLayout.Margin` ](xref:Xamarin.Forms.View.Margin) özelliği, platforma özgü ayarlandığında [ `Thickness` ](xref:Xamarin.Forms.Thickness).

## <a name="summary"></a>Özet

Bu makalede, varsayılan olmayan Oluşturucular, Fabrika yöntemleri çağırmak için ve genel bir bağımsız değişken türünü belirtmek için bağımsız değişkenleri geçirmek için kullanılan XAML öznitelikleri kullanarak gösterdik.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Ad Alanları](~/xamarin-forms/xaml/namespaces.md)
- [Oluşturucu bağımsız değişkenleri (örnek) geçirme](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Arama Fabrika yöntemleri (örnek)](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
