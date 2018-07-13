---
title: Xamarin.Forms çoklu ekranı derinlemesine bakış
description: Bu makalede, sayfa gezintisi ve bir Xamarin.Forms uygulaması içinde veri bağlamayı sunar ve çoklu ekran platformlar arası uygulama kullanımları gösterir.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 355d050fea2516dfc8ad532675048c5c5293368a
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997562"
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Xamarin.Forms çoklu ekranı derinlemesine bakış

İçinde [Xamarin.Forms çoklu ekranı hızlı](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), Phoneword uygulama, uygulama için arama geçmişine ve ikinci bir ekran içerecek şekilde genişletildi. Bu makalede, ne, sayfa gezintisi bir anlayış ve veri bağlama bir Xamarin.Forms uygulaması geliştirmek için oluşturulduğundan incelenir.

## <a name="navigation"></a>Gezinti

Xamarin.Forms gezinti ve kullanıcı deneyimi sayfaların yığının yöneten bir yerleşik bir gezinti modeli sağlar. Bu model son giren ilk çıkar (LIFO) yığınını uygulayan `Page` nesneleri. Bir sayfadan diğerine taşımak için bir uygulama bu yığını yeni bir sayfaya gönderir. Önceki sayfaya geri dönmek için uygulamanın geçerli sayfa yığından açılır pencere görürsünüz.

Xamarin.Forms sahip bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) yığınını yöneten sınıf [ `Page` ](xref:Xamarin.Forms.Page) nesneleri. `NavigationPage` Sınıfı bir gezinti çubuğunu da bir başlık ve platform uygun gösteren sayfa üst kısmına eklenir <span class="uiitem">geri</span> önceki sayfasına döndürür düğmesine. Aşağıdaki kod örneği nasıl kaydırılacağını gösteren bir `NavigationPage` uygulamanın ilk sayfa geçici olarak:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Tüm [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) örneğe sahip bir [ `Navigation` ](xref:Xamarin.Forms.VisualElement.Navigation) sayfa yığın değiştirmek için yöntemlerini gösteren özellik. Uygulama içeriyorsa, bu yöntem yalnızca çağrılan bir [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Gidilecek `CallHistoryPage`, çağırmak gerekli olan [ `PushAsync` ](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

Bu yeni neden `CallHistoryPage` gezinme yığınına itilecek nesne. Program aracılığıyla geri geldikleri sayfaya geri dönmek için `CallHistoryPage` nesne çağırmalıdır [ `PopAsync` ](xref:Xamarin.Forms.NavigationPage.PopAsync) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await Navigation.PopAsync();
```

Phoneword uygulamada bu kod olarak gerekli değildir ancak [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) sınıfı uygun bir platform içeren sayfanın üst kısmına bir gezinti çubuğu ekler <span class="uiitem">geri</span> döndürür düğmesine Önceki sayfaya.

## <a name="data-binding"></a>Veri Bağlama

Veri bağlama, bir Xamarin.Forms uygulaması nasıl görüntüler ve kendi verilerle etkileşim kolaylaştırmak için kullanılır. Bu, kullanıcı arabirimi ve arka plandaki uygulama arasında bir bağlantı kurar. [ `BindableObject` ](xref:Xamarin.Forms.BindableObject) Sınıfı veri bağlamayı destekleyen bir altyapı çoğunu içerir.

Veri bağlama iki nesne arasındaki ilişkiyi tanımlar. *Kaynak* nesnesi veri sağlayın. *Hedef* nesne kullanma (ve çoğunlukla görüntüler) kaynak nesne verilerden. Bağlama hedefi Phoneword uygulamada olduğu [ `ListView` ](xref:Xamarin.Forms.ListView) telefon numaralarını gösteren Denetim sırasında `PhoneNumbers` bağlama kaynağı koleksiyonudur.

`PhoneNumbers` Koleksiyon bildirildi ve içinde başlatılan `App` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

[ `ListView` ](xref:Xamarin.Forms.ListView) Örneği bildirildi ve içinde başlatılan `CallHistoryPage` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             ...>
    ...
    <ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
    </ContentPage.Content>
</ContentPage>
```

Bu örnekte, [ `ListView` ](xref:Xamarin.Forms.ListView) denetim görüntüler `IEnumerable` veri koleksiyonu, [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) özelliği bağlanır. Veri koleksiyonu nesneleri varsayılan olarak, ancak herhangi bir türde olabilir `ListView` kullanacağı `ToString` her öğenin o öğe görüntülemek için yöntemi. [ `x:Static` ](xref:Xamarin.Forms.Xaml.StaticExtension) İşaretleme uzantısı belirtmek için kullanılan `ItemsSource` özelliğini statik olarak bağlı `PhoneNumbers` özelliği `App` içinde bulunan sınıf `local` ad alanı .

Veri bağlama hakkında daha fazla bilgi için bkz. [temel veri bağlama bilgileri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). XAML biçimlendirme uzantıları hakkında daha fazla bilgi için bkz: [XAML biçimlendirme uzantıları](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramları

[ `ListView` ](xref:Xamarin.Forms.ListView) Ekranda bir öğe koleksiyonu görüntülemek için sorumludur. Her öğe `ListView` tek bir hücrede yer alır. Kullanma hakkında daha fazla bilgi için `ListView` denetlemek için bkz: [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Özet

Bu makalede, sayfa gezintisi ve bir Xamarin.Forms uygulaması içinde veri bağlamayı ve çoklu ekran platformlar arası uygulama kullanımları gösterilmiştir.
