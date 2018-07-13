---
title: Bir girdiyi özelleştirme
description: Xamarin.Forms giriş denetimi tek satırlık bir metin düzenlenmesine izin verir. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak girişi denetimi için özel Oluşturucu oluşturma işlemini gösterir.
ms.prod: xamarin
ms.assetid: 7B5DD10D-0411-424F-88D8-8A474DF16D8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 30326b8d52f39268015bdcbee1b84b9d9e5516b9
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998966"
---
# <a name="customizing-an-entry"></a>Bir girdiyi özelleştirme

_Xamarin.Forms giriş denetimi tek satırlık bir metin düzenlenmesine izin verir. Bu makalede, geliştiricilerin kendi platforma özgü özelleştirme varsayılan yerel işleme geçersiz kılmak girişi denetimi için özel Oluşturucu oluşturma işlemini gösterir._

Yerel bir denetimin bir örneği oluşturan her platform için eşlik eden bir işleyici her Xamarin.Forms denetleyebilir. Olduğunda bir [ `Entry` ](xref:Xamarin.Forms.Entry) bir Xamarin.Forms uygulaması iOS tarafından işlenen denetim `EntryRenderer` sınıf örneği, hangi sırayla yerel bir örneğini oluşturur `UITextField` denetimi. Android platformunda `EntryRenderer` sınıfı örnekleyen bir `EditText` denetimi. Üzerindeki Evrensel Windows Platformu (UWP), `EntryRenderer` sınıfı örnekleyen bir `TextBox` denetimi. Oluşturucu ve Xamarin.Forms denetimleri eşleyen yerel denetim sınıfları hakkında daha fazla bilgi için bkz. [oluşturucu temel sınıfları ve yerel denetimleri](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Aşağıdaki diyagramda arasındaki ilişkiyi gösterir [ `Entry` ](xref:Xamarin.Forms.Entry) ve uygulamaya karşılık gelen yerel denetimi:

![](entry-images/entry-classes.png "Giriş denetim ve yerel denetimleri uygulama arasındaki ilişki")

İşleme sürecini avantajlarından platforma özgü özelleştirmeler için özel Oluşturucu oluşturarak uygulamak için gerçekleştirilebilecek [ `Entry` ](xref:Xamarin.Forms.Entry) her platformda denetimi. Bunu yapmak için işlem aşağıdaki gibidir:

1. [Oluşturma](#Creating_the_Custom_Entry_Control) Xamarin.Forms özel bir denetim.
1. [Tüketen](#Consuming_the_Custom_Control) Xamarin.Forms özel denetimden.
1. [Oluşturma](#Creating_the_Custom_Renderer_on_each_Platform) her platformda denetimi için özel Oluşturucu.

Her öğe artık sırayla uygulanacağı açıklanmıştır bir [ `Entry` ](xref:Xamarin.Forms.Entry) her platformda farklı arka plan rengi olan denetim.

<a name="Creating_the_Custom_Entry_Control" />

## <a name="creating-the-custom-entry-control"></a>Özel giriş denetimi oluşturma

Özel bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi tarafından sınıflara oluşturulabilir `Entry` denetimi aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public class MyEntry : Entry
{
}
```

`MyEntry` Denetimi .NET Standard kitaplığı projesinde oluşturulduğunu ve yalnızca bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi. Denetimin özelleştirme gerçekleştirilmesini özel işleyicide ek uygulaması gerekli şekilde `MyEntry` denetimi.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Özel denetim kullanma

`MyEntry` Denetim başvurulabilir XAML içinde .NET Standard kitaplığı projesinde konumu için bir ad alanı bildirmek ve denetimi öğeye ad alanı öneki kullanarak. Aşağıdaki kod örnekte gösterildiği nasıl `MyEntry` denetimi bir XAML sayfası tarafından kullanılabilir:

```xaml
<ContentPage ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Ad alanı ön eki adı herhangi bir şey. Ancak, `clr-namespace` ve `assembly` değerleri, özel denetimin ayrıntılarını eşleşmesi gerekir. Özel denetim başvurusu yapmak için kullanılan önek için ad alanı bildirimi sonra.

Aşağıdaki kod örnekte gösterildiği nasıl `MyEntry` denetimi bir C# sayfası tarafından kullanılabilir:

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

Bu kod yeni bir örneğini oluşturur [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) görüntüleyecek nesneyi bir [ `Label` ](xref:Xamarin.Forms.Label) ve `MyEntry` denetim ortalanmış hem dikey ve yatay sayfasında.

Özel oluşturucu, artık her platformda denetimin görünümünü özelleştirmek için her bir uygulama projesine eklenebilir.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Her platformda özel Oluşturucu Oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir öğesinin `EntryRenderer` yerel kontrolünü icra eden sınıf.
1. Geçersiz kılma `OnElementChanged` denetimi özelleştirmek için yerel denetim ve Yaz mantığı işleyen yöntemi. Bu yöntem, karşılık gelen Xamarin.Forms Denetim oluşturulduğunda çağrılır.
1. Ekleme bir `ExportRenderer` bunu Xamarin.Forms denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Her platform projesinde özel bir oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse denetimin taban sınıfı için varsayılan oluşturucu kullanılır.

Örnek uygulamada, onlar arasındaki ilişkileri yanı sıra her bir proje sorumluluklarını Aşağıdaki diyagramda gösterilmektedir:

![](entry-images/solution-structure.png "MyEntry özel Oluşturucu Proje Sorumlulukları")

`MyEntry` Denetim platforma özgü tarafından işlenen `MyEntryRenderer` tüm türetilen sınıfları `EntryRenderer` her platform için sınıf. Bu her sonuçları `MyEntry` aşağıdaki ekran görüntülerinde gösterildiği gibi bir platforma özgü arka plan rengi ile işlenen denetim:

![](entry-images/screenshots.png "Her platformda MyEntry denetimi")

`EntryRenderer` Sınıfı kullanıma sunan `OnElementChanged` karşılık gelen yerel denetimi oluşturmak için Xamarin.Forms Denetim oluşturulurken çağrılan yöntemi. Bu yöntem bir `ElementChangedEventArgs` içeren parametre `OldElement` ve `NewElement` özellikleri. Bu özellikler Xamarin.Forms öğesini temsil, oluşturucu *olduğu* eklenmiş ve Xamarin.Forms öğesi, oluşturucu *olduğu* bağlı olarak, sırasıyla. Örnek uygulamada `OldElement` özelliği `null` ve `NewElement` özelliği, bir başvuru içerecek `MyEntry` denetimi.

Geçersiz kılınan bir sürümünü `OnElementChanged` yönteminde `MyEntryRenderer` sınıfı, nelerin yerel denetimi özelleştirme işlemi gerçekleştirin. Belirlenmiş bir başvuru platformunda kullanılan yerel denetlemek için erişilebilir `Control` özelliği. Ayrıca, işlenen Xamarin.Forms denetime başvuru aracılığıyla alınabilir `Element` özelliği, ancak örnek uygulama kullanılmaz.

Her özel Oluşturucu sınıfı ile donatılmış bir `ExportRenderer` özniteliği işleyici Xamarin.Forms ile kaydeder. Öznitelik, iki parametre – işlenen Xamarin.Forms denetimin tür adı ve özel Oluşturucu türü adını alır. `assembly` Özniteliğiyle önek öznitelik tüm derleme için geçerli olduğunu belirtir.

Aşağıdaki bölümlerde her platforma özel uygulanışı `MyEntryRenderer` özel Oluşturucu sınıfı.

### <a name="creating-the-custom-renderer-on-ios"></a>İOS özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, iOS platformu için özel Oluşturucu gösterir:

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

Temel sınıfın çağrısı `OnElementChanged` yöntemi örnekleyen bir iOS `UITextField` denetimiyle işleyiciye kişinin atanmasını denetimine bir başvuru `Control` özelliği. Arka plan rengini açık mor ile ayarlanır `UIColor.FromRGB` yöntemi.

### <a name="creating-the-custom-renderer-on-android"></a>Android'de özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, Android platformu için özel Oluşturucu gösterir:

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

Temel sınıfın çağrısı `OnElementChanged` yöntemi örnekleyen bir Android `EditText` denetimiyle işleyiciye kişinin atanmasını denetimine bir başvuru `Control` özelliği. Arka plan rengini açık yeşil ile ayarlanır `Control.SetBackgroundColor` yöntemi.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP üzerinde özel Oluşturucu Oluşturma

Aşağıdaki kod örneği, UWP için özel Oluşturucu gösterir:

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

Temel sınıfın çağrısı `OnElementChanged` yöntemi örnekleyen bir `TextBox` denetimiyle işleyiciye kişinin atanmasını denetimine bir başvuru `Control` özelliği. Arka plan rengi oluşturarak Camgöbeği için sonra ayarlanmış bir `SolidColorBrush` örneği.

## <a name="summary"></a>Özet

Bu makalede, bir özel denetim işleyici Xamarin.Forms için oluşturma gösterilmiştir [ `Entry` ](xref:Xamarin.Forms.Entry) geliştiricilerin kendi platforma özgü işleme varsayılan yerel işlemeyle geçersiz kılmak denetim. Özel oluşturucular Xamarin.Forms denetimlerin görünümünü özelleştirmek için güçlü bir yaklaşım sağlar. Bunlar küçük stil değişikliklerini veya karmaşık platforma özel düzen ve davranışını özelleştirme için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [CustomRendererEntry (örnek)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/entry/)
