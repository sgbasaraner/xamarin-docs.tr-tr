---
title: Bir giriş özelleştirme
description: Xamarin.Forms giriş denetimi düzenlenmesi metnin tek bir satırı sağlar. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme giriş denetimi için özel Oluşturucu Oluşturma gösterilir.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 5f23b65fab24b447a9f534ed7403797a60cc284f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847231"
---
# <a name="customizing-an-entry"></a>Bir giriş özelleştirme

_Xamarin.Forms giriş denetimi düzenlenmesi metnin tek bir satırı sağlar. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işlemeyle geçersiz kılmasına etkinleştirme giriş denetimi için özel Oluşturucu Oluşturma gösterilir._

Yerel bir denetimi bir örneğini oluşturur her platform için eşlik eden bir oluşturucu her Xamarin.Forms denetleyebilir. Zaman bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetimi, bir Xamarin.Forms uygulaması iOS tarafından işlenir `EntryRenderer` sınıf örneği, tasarrufludur yerel başlatır `UITextField` denetim. Android platformunda `EntryRenderer` sınıfı başlatır bir `EditText` denetim. Üzerinde Evrensel Windows Platformu (UWP), `EntryRenderer` sınıfı başlatır bir `TextBox` denetim. Oluşturucu ve Xamarin.Forms denetimleri Eşle yerel denetim sınıfları hakkında daha fazla bilgi için bkz: [Oluşturucu taban sınıfları ve yerel denetimlere](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagram arasındaki ilişkiyi gösterir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) ve uyguladıktan karşılık gelen yerel denetimi:

![](entry-images/entry-classes.png "Giriş denetim ve yerel denetimler uygulamak arasındaki ilişki")

Oluşturma işlemi için özel Oluşturucu oluşturarak platforma özgü özelleştirmeler uygulamak için avantajlarından alınabilir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) her platformda denetim. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Entry_Control) Xamarin.Forms özel bir denetim.
1. [Tüketen](#Consuming_the_Custom_Control) Xamarin.Forms özel denetim.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda denetimi için özel Oluşturucu.

Her öğe artık sırasıyla uygulamak için incelenecektir bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) farklı arka plan rengi her platformda sahip denetimini.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Özel giriş denetimi oluşturma

Özel bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetim sınıflara tarafından oluşturulabilir `Entry` denetlemek, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` Denetim .NET standart kitaplığı projesinde oluşturulan ve sadece bir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetim. Denetimin özelleştirme gerçekleştirilmesini özel işleyicide ek uygulaması gereken şekilde `MyEntry` denetim.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Özel denetim kullanma

`MyEntry` Denetim başvurulabilir XAML'de .NET standart kitaplığı projesinde konumu için bir ad alanı bildirme ve denetim öğede ad alanı öneki kullanarak. Aşağıdaki örnekte gösterildiği kod nasıl `MyEntry` denetim XAML sayfası tarafından tüketilen:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Ad alanı öneki adlı bir şey. Ancak, `clr-namespace` ve `assembly` değerlerin özel denetim ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde öneki özel denetim başvurmak için kullanılır.

Aşağıdaki örnekte gösterildiği kod nasıl `MyEntry` denetim bir C# sayfası tarafından tüketilen:

```csharp
public class MainPage : ContentPage
{
  public MainPage ()
  {
    Content = new StackLayout {
      Children = {
        new Label {
          Text = "Hello, Custom Renderer !",
        },
        new MyEntry {
          Text = "In Shared Code",
        }
      },
      VerticalOptions = LayoutOptions.CenterAndExpand,
      HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
  }
}
```

Bu kod yeni bir örneğini oluşturur [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) görüntüler nesnesi bir [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) ve `MyEntry` denetim ortalanmış hem de dikey ve yatay olarak sayfasında.

Özel oluşturucu artık her platformda denetimin görünümünü özelleştirmek için her uygulama projesi eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Öğesinin bir alt kümesi oluşturmak `EntryRenderer` yerel denetimini işler sınıfı.
1. Geçersiz kılma `OnElementChanged` denetimi özelleştirmek için yerel denetimi ve yazma mantığı işleyen yöntemi. Karşılık gelen Xamarin.Forms Denetim oluşturulduğunda, bu yöntem çağrılır.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Her platform projesinde özel Oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse, varsayılan oluşturucu denetimin taban sınıfı için kullanılır.

Aşağıdaki diyagram, her proje örnek uygulamasında, aralarındaki ilişkilerin birlikte sorumlulukları gösterir:

![](entry-images/solution-structure.png "MyEntry özel Oluşturucu Proje Sorumlulukları")

`MyEntry` Denetim platforma özgü tarafından işlenen `MyEntryRenderer` tüm öğesinden türetilen sınıflar, `EntryRenderer` her platform için sınıf. Bu her sonuçları `MyEntry` aşağıdaki ekran görüntülerinde gösterildiği gibi bir platforma özgü arka plan rengiyle işlenen denetim:

![](entry-images/screenshots.png "Her platformda MyEntry denetimi")

`EntryRenderer` Sınıf çıkarır `OnElementChanged` Xamarin.Forms Denetim karşılık gelen yerel denetimi oluşturmak için oluşturulduğunda çağrılan yöntemi. Bu yöntem alır bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikleri Xamarin.Forms öğesini temsil, oluşturucu *olan* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olan* bağlı olarak, sırasıyla. Örnek uygulamasında `OldElement` özelliği `null` ve `NewElement` özelliği bir başvuru içerecek `MyEntry` denetim.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde `MyEntryRenderer` sınıfı, sistemi, yerel denetim özelleştirme gerçekleştirmek için yerdir. Platformda kullanılan yerel denetlemek için belirlenmiş bir başvuru üzerinden erişilen `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetlemek için bir başvuru aracılığıyla elde edilebilir `Element` özelliği, örnek uygulamasında kullanılmayan rağmen.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` Xamarin.Forms ile oluşturucuyu kaydeder özniteliği. Öznitelik iki parametre – işlenen Xamarin.Forms denetiminin türü adı ve özel Oluşturucu Tür adını alır. `assembly` Özniteliğine öneki belirtir. öznitelik tüm derlemesi için geçerlidir.

Aşağıdaki bölümlerde her platforma özgü uyarlamasını açıklanmaktadır `MyEntryRenderer` özel Oluşturucu sınıfı.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde iOS platformu için özel Oluşturucu gösterir:

```csharp
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer (typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.iOS
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged (ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged (e);

            if (Control != null) {
                // do whatever you want to the UITextField here!
                Control.BackgroundColor = UIColor.FromRGB (204, 153, 255);
                Control.BorderStyle = UITextBorderStyle.Line;
            }
        }
    }
}
```

Temel sınıfın çağrısı `OnElementChanged` yöntemi başlatır iOS `UITextField` denetimiyle Oluşturucu için kişinin atanmasını denetlemek için bir başvuru `Control` özelliği. Arka plan rengini sonra açık mor ile ayarlayın `UIColor.FromRGB` yöntemi.

### <a name="creating-the-custom-renderer-on-android"></a>Android özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde, Android platformu için özel Oluşturucu gösterir:

```csharp
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.Android
{
    class MyEntryRenderer : EntryRenderer
    {
        public MyEntryRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.SetBackgroundColor(global::Android.Graphics.Color.LightGreen);
            }
        }
    }
}
```

Temel sınıfın çağrısı `OnElementChanged` yöntemi bir Android başlatır `EditText` denetimiyle Oluşturucu için kişinin atanmasını denetlemek için bir başvuru `Control` özelliği. Arka plan rengini sonra ile açık yeşil ayarlayın `Control.SetBackgroundColor` yöntemi.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP üzerinde özel Oluşturucu Oluşturma

Aşağıdaki kod örneğinde UWP için özel Oluşturucu gösterir:

```csharp
[assembly: ExportRenderer(typeof(MyEntry), typeof(MyEntryRenderer))]
namespace CustomRenderer.UWP
{
    public class MyEntryRenderer : EntryRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
        {
            base.OnElementChanged(e);

            if (Control != null)
            {
                Control.Background = new SolidColorBrush(Colors.Cyan);
            }
        }
    }
}
```

Temel sınıfın çağrısı `OnElementChanged` yöntemi başlatır bir `TextBox` denetimiyle Oluşturucu için kişinin atanmasını denetlemek için bir başvuru `Control` özelliği. Arka plan rengini oluşturarak için mavi sonra ayarlanmış bir `SolidColorBrush` örneği.

## <a name="summary"></a>Özet

Bu makalede, bir özel denetim Oluşturucusu Xamarin.Forms oluşturma gösterilmiştir [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetim, geliştiricilerin kendi platforma özgü işleme ile varsayılan yerel işleme geçersiz kılmasına etkinleştirme. Özel oluşturucu Xamarin.Forms denetimlerin görünümünü özelleştirmek için güçlü bir yaklaşım sağlayın. Bunlar küçük stil değişiklikleri veya karmaşık platforma özgü düzeni ve davranışını özelleştirme için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererEntry (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
