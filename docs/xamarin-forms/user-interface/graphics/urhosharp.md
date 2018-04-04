---
title: Xamarin.Forms içinde UrhoSharp kullanma
description: UrhoSharp Gelişmiş görselleştirme için uygulama 3B grafik eklemek için kullanılabilir
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: 8421355e0630a637589cb4f08c2fec4ea9cdab24
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="using-urhosharp-in-xamarinforms"></a>Xamarin.Forms içinde UrhoSharp kullanma

## <a name="what-is-urhosharp"></a>UrhoSharp nedir?

[UrhoSharp](~/graphics-games/urhosharp/index.md) Xamarin ve .NET geliştiricileri için güçlü bir 3B alt yapısıdır. [Giriş](~/graphics-games/urhosharp/introduction.md) UrhoSharp Kitaplığı hakkında daha fazla açıklar ve [bu notlar](~/graphics-games/urhosharp/using.md) program planda ve eylemleri açıklar.

UrhoSharp Xamarin.Forms uygulamalarında grafik oluşturmak için kullanılabilir.
Bu [örnek](https://github.com/xamarin/urho-samples/tree/master/FormsSample) nasıl UrhoSharp etkileşimli bir 3B grafik oluşturmak için kullanılabilecek gösterir:

![](urhosharp-images/ios-animation.gif "3B etkileşimli grafik iOS UrhoSharp")
![](urhosharp-images/android-animation.gif "3B etkileşimli grafik Android UrhoSharp")

## <a name="adding-the-urhosharp-nuget-packages"></a>UrhoSharp Nuget paketleri ekleme

UrhoSharp kullanmadan önce geliştiricilerin kendi çözüme UrhoSharp Nuget paketini eklemeniz gerekir. Bu kılavuz bir iOS, Android ve PCL Xamarin.Forms projeyle varsayar projesi. Tüm kod PCL projesinde yazılır; ancak UrhoSharp Nuget iOS ve Android projeleri çok eklenmesi gerekir.

UrhoSharp.Forms Nuget paketi tüm UrhoSharp nesneleri oluşturmak için gereken nesneleri içerir. UrhoSharp.Forms nuget paketini içeren `UrhoSurface` Xamarin.Forms UrhoSharp barındırmak için kullanılan sınıf.
Başlamak için PCL üzerinde 's sağ **paketleri** klasörü ve select **paketleri Ekle...** . Arama terimi girin **UrhoSharp.Forms**seçin **Xamarin.Forms UrhoSharp**, ardından **Paketi Ekle**.

[![](urhosharp-images/add-package-sml.png "Paketleri iletişim ekleyin")](urhosharp-images/add-package.png#lightbox "paketleri iletişim ekleyin")

UrhoSharp.Forms NuGet paketini projeye eklenecek:

![](urhosharp-images/packages.png "Paketler klasörü")

(Örneğin, iOS ve Android) platforma özgü projeleri için yukarıdaki adımları yineleyin.

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>İzlenecek yol: Bir Xamarin.Forms uygulaması UrhoSharp ekleme

Bu adımlar Xamarin.Forms UrhoSharp örnek kodda açıklamaktadır:

1. [Creat Xamarin Forms sayfası](#1)
2. [UrhoSurface Ekle](#2)
3. [Bir Urho uygulaması oluşturma](#3)
4. [UrhoSurface grafikleri sınıfı ekleme](#4)
5. [UrhoSharp ile etkileşim kurma](#5)

Örnek C# 6 özelliklerini kullanır ve Visual Studio'nun daha eski sürümleri derleme değil unutmayın.

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1. Xamarin Forms sayfa oluşturma

Aşağıdaki kod, bir Xamarin.Forms sayfası gösterir `UrhoPage` Urho ilgili kod eklenen önce:

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2. UrhoSurface Ekle

UrhoSharp içinde barındırılması bir `ContentPage` gibi diğer Xamarin.Forms denetimleri gibi.
Kod parçacığı aşağıda gösterildiği bir `UrhoSurface` Xamarin.Forms sayfasına eklendi:

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3. Bir Urho uygulaması oluşturma

Başvurmak `Charts` Bu örnekte kullanılan Urho 3B grafik uygulanması için sınıf. Temel kod anahat gösterilen aşağıda - sınıfı uyguladığını unutmayın `Urho.Application` için farklı `Xamarin.Forms.Application` uygulanan sınıfı **App.cs**.

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[UrhoSharp belgelerine](~/graphics-games/urhosharp/index.md) 3B görüntülerin ve Eylemler oluşturma hakkında daha fazla bilgi içerir.

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. UrhoSurface grafikleri sınıfı ekleme

Kullanım `UrhoSurface.Show<T>` Xamarin.Forms sayfa Urho uygulama eklemek için genel yöntem. Aşağıdaki kod parçacığında oluşturmak için gereken ek kod gösterir `Charts` sınıfı:

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

Not: `Show<T>` yöntemi zaman uyumsuz ve ile çağrılmalıdır `await` anahtar sözcüğü.

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5. UrhoSharp ile etkileşim kurma

Örnek seçili ve değişiklik grafik çubukları sağlar. `Charts` Sınıf çıkarır `Bars` ve `SelectedBar` bu etkileşimi etkinleştirmek için.

Sonra eklenmiş bir seçim olay işleyicisi her "çubuk" sahip `Charts` sınıfı çizilir, sunulan yineleme `Bars` koleksiyonu:

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

Olay işleyicisi Xamarin.Forms değerini kullanan `Slider` verilen çubuğu yüksekliğini ayarlamasına denetimi:

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

Son olarak, iki wire `Slider` böylece bunların değeri değiştiğinde UrhoSharp tuvale etkilenen denetler. 3B grafik görüntüsünün ilk kaydırıcıyı döner ve ikinci kaydırıcı seçili çubuğunun yüksekliği ayarlar.

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

Bir animasyon [sayfasının üst](#) örnek çalışan göster.

## <a name="summary"></a>Özet

Bu sayfa, 3B veri görselleştirme için Xamarin.Forms eklemek için UrhoSharp'ın nasıl kullanılabileceğini gösterir. Okuma [UrhoSharp belgelerine](~/graphics-games/urhosharp/index.md) yukarıda gösterilen yöntemi kullanarak Xamarin.Forms uygulamaları yer Urho planda derleme hakkında daha fazla bilgi için.


## <a name="related-links"></a>İlgili bağlantılar

- [Grafik örneği (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
