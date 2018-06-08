---
title: XAML'de bağımsız değişkenleri geçirme
description: Bu makalede, varsayılan olmayan kurucusuna, Fabrika yöntemlerini çağırmaya ve genel bir bağımsız değişken türünü belirtmek için bağımsız değişkenler geçirmek için kullanılan XAML öznitelikleri kullanma gösterilmektedir.
ms.prod: xamarin
ms.assetid: 8F3B267F-499E-4D79-9193-FCA99F199519
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2016
ms.openlocfilehash: c6ba3de9e50fd2ac452d9eeac169e4c1afd52ae0
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848440"
---
# <a name="passing-arguments-in-xaml"></a>XAML'de bağımsız değişkenleri geçirme

_Bu makalede, varsayılan olmayan kurucusuna, Fabrika yöntemlerini çağırmaya ve genel bir bağımsız değişken türünü belirtmek için bağımsız değişkenler geçirmek için kullanılan XAML öznitelikleri kullanma gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Genellikle, nesneleri bağımsız değişken gerektirir oluşturucular ile ya da statik oluşturma yöntemini çağırarak örneği oluşturmak gereklidir. XAML'de bu sağlanabilir kullanarak `x:Arguments` ve `x:FactoryMethod` öznitelikleri:

- `x:Arguments` Özniteliği, varsayılan olmayan bir oluşturucu ya da bir fabrika yöntemi nesne bildirimi oluşturucu bağımsız değişkenlerini belirtmek için kullanılır. Daha fazla bilgi için bkz: [oluşturucu bağımsız değişkenleri geçirme](#constructor_arguments).
- `x:FactoryMethod` Öznitelik, bir nesneyi başlatmak için kullanılan Üreteç yöntemi belirtmek için kullanılır. Daha fazla bilgi için bkz: [Fabrika yöntemleri çağırma](#factory_methods).

Ayrıca, `x:TypeArguments` özniteliği, genel bir tür oluşturucuya genel tür bağımsız değişkenlerini belirtmek için kullanılabilir. Daha fazla bilgi için bkz: [genel tür bağımsız değişkeni belirterek](#generic_type_arguments).

<a name="constructor_arguments" />

## <a name="passing-constructor-arguments"></a>Oluşturucu bağımsız değişkenleri geçirme

Bağımsız değişkenleri için varsayılan olmayan Oluşturucusu kullanılarak geçirilebilir `x:Arguments` özniteliği. Her oluşturucu bağımsız değişkenin türü ile temsil eden bir XML öğesi içinde ayrılmış gerekir. Xamarin.Forms aşağıdaki öğeleri için temel türlerini destekler:

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

Aşağıdaki kod örneğinde kullanımı gösterilir `x:Arguments` üç özniteliğiyle [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) oluşturucular:

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

İçindeki öğe sayısını `x:Arguments` etiketi ve bu öğeleri türleri eşleşmelidir birini [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) oluşturucular. `Color` [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/) ile tek bir parametre 0 (siyah) bir gri tonlamalı değerini 1 (beyaz) gerektirir. `Color` [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/) ile üç parametre 0 ile 1 arasında değişen bir kırmızı, yeşil ve mavi değer gerektirir. `Color` [Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Color.Color/p/System.Double/System.Double/System.Double/System.Double/) dört parametrelerle bir alfa kanal dördüncü parametre olarak ekler.

Aşağıdaki ekran görüntüleri her çağırma sonucu göster [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) belirtilen bağımsız değişken değerlerle Oluşturucusu:

![](passing-arguments-images/passing-arguments.png "X: Arguments ile belirtilen BoxView.Color")

<a name="factory_methods" />

## <a name="calling-factory-methods"></a>Fabrika yöntemleri çağırma

Fabrika yöntemleri çağırılabilir XAML'de yöntemin belirterek kullanarak ad `x:FactoryMethod` özniteliği ve kullanarak, bağımsız değişkenlerinin `x:Arguments` özniteliği. Fabrika yöntemi bir `public static` nesne veya sınıf ya da yöntemleri tanımlar yapısı olarak aynı türde değerler döndüren yöntemi.

[ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Yapısını tanımlayan çeşitli Fabrika yöntemler ve aşağıdaki kod örneğinde arama üç tanesi gösterilmektedir:

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

İçindeki öğe sayısını `x:Arguments` etiketini ve bu öğeleri türlerini çağrılan Üreteç yöntemi bağımsız değişkenleri aynı olmalıdır. [ `FromRgba` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgba/p/System.Int32/System.Int32/System.Int32/System.Int32/) Üreteç yöntemi gerektiren dört [ `Int32` ](https://docs.microsoft.com/dotnet/api/system.int32) sırasıyla 0 ile 255 arasında kırmızı, yeşil, mavi ve alfa değerleri temsil eden parametreleri. [ `FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/) Üreteç yöntemi gerektiren dört [ `Double` ](https://docs.microsoft.com/dotnet/api/system.double) ton, Doygunluk, parlaklığını ve sırasıyla 0 ile 1 arasında alfa değerleri temsil eden parametreleri. [ `FromHex` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHex/p/System.String/) Üreteç yöntemi gerektiren bir [ `String` ](https://docs.microsoft.com/dotnet/api/system.string) onaltılık temsil eden (A) RGB rengi.

Aşağıdaki ekran görüntüleri her çağırma sonucu göster [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) belirtilen bağımsız değişken değerlerle Üreteç yöntemi:

![](passing-arguments-images/factory-methods.png "X: FactoryMethod ve x: Arguments ile belirtilen BoxView.Color")

<a name="generic_type_arguments" />

## <a name="specifying-a-generic-type-argument"></a>Genel tür bağımsız değişkeni belirtme

Genel tür bağımsız değişkenleri için genel bir tür oluşturucu kullanılarak belirtilebilir `x:TypeArguments` aşağıdaki kod örneğinde gösterildiği gibi öznitelik:

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

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Sınıfı genel bir sınıftır ve ile örneğinin oluşturulması bir `x:TypeArguments` hedef türüyle eşleşen özniteliği. İçinde [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) sınıfı, [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) özniteliği tek bir kabul edebileceği `string` değeri veya virgülle ayrılmış birden çok `string` değerleri. Bu örnekte, [ `StackLayout.Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) özelliği platforma özgü ayarlanmış [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/).

## <a name="summary"></a>Özet

Bu makalede, varsayılan olmayan kurucusuna, Fabrika yöntemlerini çağırmaya ve genel bir bağımsız değişken türünü belirtmek için bağımsız değişkenler geçirmek için kullanılan XAML öznitelikleri kullanma gösterilmektedir.


## <a name="related-links"></a>İlgili bağlantılar

- [XAML Ad Alanları](~/xamarin-forms/xaml/namespaces.md)
- [Oluşturucu bağımsız değişkenleri (örnek) geçirme](https://developer.xamarin.com/samples/xamarin-forms/xaml/passingconstructorarguments/)
- [Fabrika yöntemleri (örnek) çağırma](https://developer.xamarin.com/samples/xamarin-forms/xaml/callingfactorymethods/)
