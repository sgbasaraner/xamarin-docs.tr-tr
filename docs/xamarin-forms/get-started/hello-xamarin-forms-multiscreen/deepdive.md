---
title: Xamarin.Forms Multiscreen derinlemesine bakış
description: Bu makalede sayfa gezintisi ve veri bağlamanın bir Xamarin.Forms uygulaması tanıtır ve bunların kullanılması çok ekran platformlar arası uygulamasında gösterir.
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
ms.openlocfilehash: 1c7edff3c71b9d7530b2acf21acaa06149156d43
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242241"
---
# <a name="xamarinforms-multiscreen-deep-dive"></a>Xamarin.Forms Multiscreen derinlemesine bakış

İçinde [Xamarin.Forms Multiscreen Hızlı Başlangıç](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), Phoneword uygulama, uygulama için arama geçmişini ve ikinci bir ekran içerecek şekilde genişletildi. Bu makalede, ne, sayfa gezintisi anlaşılması ve veri bağlamanın bir Xamarin.Forms uygulaması geliştirmek için oluşturulduğunu gözden geçirir.

## <a name="navigation"></a>Gezinme

Xamarin.Forms bir sayfa yığınını kullanıcı deneyimi ve gezinti yöneten bir yerleşik gezinti modeli sağlar. Bu model son giren ilk çıkar (LIFO) yığınını uygulayan `Page` nesneleri. Bir sayfadan diğerine taşımak için bir uygulama bu yığını yeni bir sayfaya gönderir. Önceki sayfaya dönmek için uygulamanın geçerli sayfa yığından pop.

Xamarin.Forms sahip bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) yığınını yöneten sınıf [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) nesneleri. `NavigationPage` Sınıfı bir gezinti çubuğu bir başlık ve platform uygun görüntüler sayfanın üstüne eklemek de <span class="uiitem">geri</span> önceki sayfaya döner düğmesi. Aşağıdaki kod örneğinde nasıl kaydırılacağını gösterir bir `NavigationPage` uygulamanın ilk sayfa geçici:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

Tüm [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) örneğe sahip bir [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) sayfa yığını değiştirmek için yöntemlerini gösterir özelliği. Uygulama içeriyorsa, bu yöntemleri yalnızca çağrılan bir [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). Gitmek için `CallHistoryPage`, çağırmak gerekli olan [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) yöntemini aşağıdaki kod örneğinde gösterildiği gibi değiştirin:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

Bu yeni neden `CallHistoryPage` Gezinti yığına edilmesini nesnesi. Program aracılığıyla özgün sayfasına geri dönmek için `CallHistoryPage` gerekir nesne çağırma [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
await Navigation.PopAsync();
```

Phoneword uygulamada bu kodu olarak gerekli değildir ancak [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) sınıfı, sayfanın üst kısmındaki uygun bir platform içeren için gezinti çubuğu ekler <span class="uiitem">geri</span> döndürülecek düğmesi Önceki sayfaya.

## <a name="data-binding"></a>Veri Bağlama

Veri bağlama, nasıl bir Xamarin.Forms uygulaması görüntüler ve verileri ile etkileşim basitleştirmek için kullanılır. Kullanıcı arabirimi ve temel uygulama arasında bir bağlantı kurar. [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) Sınıfı, veri bağlamayı destekleyen altyapı çoğunu içerir.

Veri bağlama iki nesne arasındaki ilişkiyi tanımlar. *Kaynak* nesne veri sağlayacaktır. *Hedef* nesne tüketebilir (ve çoğunlukla görüntüler) kaynak nesne verileri. Phoneword uygulamada bağlama hedeflediği [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) telefon numaraları, görüntüleyen denetim sırada `PhoneNumbers` bağlama kaynağı koleksiyonudur.

`PhoneNumbers` Koleksiyonu bildirilir ve içinde başlatılan `App` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Örneği bildirilir ve içinde başlatılan `CallHistoryPage` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

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

Bu örnekte, [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) denetim görüntüler `IEnumerable` veri koleksiyonu, [ `ItemsSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) özelliği bağlanır. Veri koleksiyonunu nesneler herhangi bir türde, ancak varsayılan olarak, olabilir `ListView` kullanacağı `ToString` bu öğeyi görüntülemek için her öğenin yöntemi. [ `x:Static` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) Biçimlendirme uzantısı belirtmek için kullanılan `ItemsSource` özelliği statik olarak bağlı `PhoneNumbers` özelliği `App` içinde bulunan sınıfı `local` ad alanı .

Veri bağlama hakkında daha fazla bilgi için bkz: [veri bağlama Temelleri](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). XAML biçimlendirme uzantıları hakkında daha fazla bilgi için bkz: [XAML işaretleme uzantılarına](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## <a name="additional-concepts-introduced-in-phoneword"></a>Phoneword içinde sunulan ek kavramlar

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Öğeleri koleksiyonu ekranda görüntülemek için sorumludur. Her öğe `ListView` tek bir hücrede yer alır. Kullanma hakkında daha fazla bilgi için `ListView` denetlemek için bkz: [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="summary"></a>Özet

Bu makalede, sayfa gezintisi ve veri bağlamanın bir Xamarin.Forms uygulaması sunulur ve bunların kullanılması çok ekran platformlar arası uygulamada gösterilen.
