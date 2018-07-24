---
title: ListView etkileşim
description: Bu makalede, seçimleri, bağlam Eylemler ve çekme yenileme uygulayarak bir Xamarin.Forms ListView için etkileşim ekleme açıklanmaktadır.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/13/2018
ms.openlocfilehash: 77a48e36f33fc690306f5e590f9f4f3064fe1ddf
ms.sourcegitcommit: 4c0093ee5d4aeb16c0e6f0c740c4796736971651
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2018
ms.locfileid: "39202948"
---
# <a name="listview-interactivity"></a>ListView etkileşim

ListView aşağıdaki yaklaşımlardan sunan verilerle etkileşim destekler:

- [**Seçimi & dokunma** ](#selectiontaps) &ndash; Tap'ları ve seçimleri/kaldırılmasına öğelerinin yanıt. Etkinleştirmek veya devre dışı (varsayılan olarak etkindir), satır seçimi.
- [**Bağlam Eylemler** ](#Context_Actions) &ndash; sunmaya işlevi her öğesi, örneğin, manyetik Sil.
- [**Çekme yenileme** ](#Pull_to_Refresh) &ndash; kullanıcılar yerel deneyimler beklenir gelmiş çekme yenileme deyimi uygulayın.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Seçimi & Tap'ları

[ `ListView` ](xref:Xamarin.Forms.ListView) Seçim modunu ayarlama denetlenir [ `ListView.SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) bir değere [ `ListViewSelectionMode` ](xref:Xamarin.Forms.ListViewSelectionMode) sabit listesi:

- [`Single`](xref:Xamarin.Forms.ListViewSelectionMode.Single) tek bir öğe, vurgulanmış seçili öğe ile seçilebileceğini gösterir. Varsayılan değer budur.
- [`None`](xref:Xamarin.Forms.ListViewSelectionMode.None) öğeleri seçili gösterir.

Bir kullanıcı bir öğeyi dokunduğunda, iki olay harekete geçirilir:

- [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) Yeni bir öğe seçildiğinde ateşlenir.
- [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) bir öğe dokunulduğunda ateşlenir.

> [!NOTE]
> Aynı öğesini iki kez dokunarak yangın iki [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) olaylar, ancak olacak yalnızca yangın tek bir [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) olay.

Zaman [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) özelliği [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single), öğeler [ `ListView` ](xref:Xamarin.Forms.ListView) seçilebilir, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) ve [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) olayları harekete ve [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) özelliği seçilen öğenin değerine ayarlanır.

Zaman [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) özelliği [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), öğeler [ `ListView` ](xref:Xamarin.Forms.ListView) seçilemez, [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) olay harekete değil ve [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) özelliği kalır `null`. Ancak, [ `ItemTapped` ](xref:Xamarin.Forms.ListView.ItemTapped) hala olaylar ve dokunulan öğesi kısaca sırasında dokunun vurgulanır.

Bir öğe, belirlenmiş ve [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) özelliği değiştirildiğinde [ `Single` ](xref:Xamarin.Forms.ListViewSelectionMode.Single) için [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None), [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) özellik ayarlanacak `null` ve [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) olay harekete ile bir `null` öğesi.

Aşağıdaki ekran görüntüleri Göster bir [ `ListView` ](xref:Xamarin.Forms.ListView) varsayılan seçim modu:

![](interactivity-images/selection-default.png "Etkin seçim ile ListView")

### <a name="disabling-selection"></a>Seçimi devre dışı bırakma

Devre dışı bırakmak için [ `ListView` ](xref:Xamarin.Forms.ListView) seçimi kümesi [ `SelectionMode` ](xref:Xamarin.Forms.ListView.SelectionMode) özelliğini [ `None` ](xref:Xamarin.Forms.ListViewSelectionMode.None):

```xaml
<ListView ... SelectionMode="None" />
```

```csharp
var listView = new ListView { ... SelectionMode = ListViewSelectionMode.None };
```

<a name="Context_Actions" />

## <a name="context-actions"></a>Bağlam eylemleri
Genellikle, kullanıcı bir öğe üzerinde harekete geçmeye isteyecektir bir `ListView`. Örneğin, e-posta uygulamasında e-postaların listesini göz önünde bulundurun. İos'ta, ileti silme a doğru çekin:

![](interactivity-images/context-default.png "ListView bağlam eylemleri ile")

C# ve XAML İçerik Eylemler uygulanabilir. Aşağıda, belirli kılavuzları her ikisi için de ancak daha önce şimdi bulabilirsiniz her ikisi için bazı önemli uygulama ayrıntıları göz atın.

Bağlam eylemleri kullanılarak oluşturulur `MenuItem`s. Dokunma olayları MenuItems MenuItem tarafından kendisine ListView oluşturulur. Bu dokunun olayları ListView hücresi yerine olay burada başlatır üklenecek nasıl işleneceğini öğesinden farklıdır. ListView olay ortaya çıkarma olduğundan, kendi olay işleyicisine gibi madde işaretli veya dokunulan anahtar bilgileri verilir.

Varsayılan olarak, hangi hücre ait olduğunu bilmesinin imkanı MenuItem vardır. `CommandParameter` kullanılabilir `MenuItem` MenuItem'ın Viewcell'i arkasındaki nesneyi gibi nesneleri depolamak için. `CommandParameter` hem XAML ve C# içinde ayarlayabilirsiniz.

### <a name="c"></a>C#  

Birinde bağlam Eylemler uygulanabilir `Cell` (bir grup üstbilgisi olarak kullanılmıyor sürece) alt sınıf oluşturarak `MenuItem`s ve bunlara ekleme `ContextActions` hücre koleksiyonu. Şunlara sahip özellikler için bağlam eylem yapılandırılabilir:

* **Metin** &ndash; menü öğesi görünür bir dize.
* **Tıklandı** &ndash; öğe tıklatıldığında olay.
* **IsDestructive** &ndash; (isteğe bağlı) true olduğunda öğesi farklı iOS üzerinde oluşturulur.

Yalnızca biri olması gerekir ancak birden çok içerik eylemleri bir hücreye eklenebilir `IsDestructive` kümesine `true`. Aşağıdaki kod, bağlam eylemler için nasıl eklenir gösterir. bir `ViewCell`:

```csharp
var moreAction = new MenuItem { Text = "More" };
moreAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
moreAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("More Context Action clicked: " + mi.CommandParameter);
};

var deleteAction = new MenuItem { Text = "Delete", IsDestructive = true }; // red background
deleteAction.SetBinding (MenuItem.CommandParameterProperty, new Binding ("."));
deleteAction.Clicked += async (sender, e) => {
    var mi = ((MenuItem)sender);
    Debug.WriteLine("Delete Context Action clicked: " + mi.CommandParameter);
};
// add to the ViewCell's ContextActions property
ContextActions.Add (moreAction);
ContextActions.Add (deleteAction);
```

### <a name="xaml"></a>XAML

`MenuItem`s ayrıca XAML koleksiyonda bildirimli olarak oluşturulabilir. Aşağıdaki XAML iki bağlamı eylemi uygulanan özel bir hücre gösterir:

```xaml
<ListView x:Name="ContextDemoList">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
         <ViewCell.ContextActions>
            <MenuItem Clicked="OnMore" CommandParameter="{Binding .}"
               Text="More" />
            <MenuItem Clicked="OnDelete" CommandParameter="{Binding .}"
               Text="Delete" IsDestructive="True" />
         </ViewCell.ContextActions>
         <StackLayout Padding="15,0">
              <Label Text="{Binding title}" />
         </StackLayout>
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```

Arka plan kod dosyasında olun `Clicked` yöntemleri uygulanır:

```csharp
public void OnMore (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("More Context Action", mi.CommandParameter + " more context action", "OK");
}

public void OnDelete (object sender, EventArgs e) {
    var mi = ((MenuItem)sender);
    DisplayAlert("Delete Context Action", mi.CommandParameter + " delete context action", "OK");
}
```

> [!NOTE]
> `NavigationPageRenderer` İçin Android geçersiz kılınabilir bir `UpdateMenuItemIcon` bir özel simgeleri yüklemek için kullanılan yöntem `Drawable`. Bu geçersiz kılma üzerinde simgelerle SVG görüntüleri kullanmayı mümkün kılar `MenuItem` Android örneği.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Yenilemek için çekin
Kullanıcılar bir veri listesi kısmından aşağı doğru kaydırarak bu listeyi yenileyin beklenir ulaştık. `ListView` Bu çıkış,-hazır destekler. Çekme yenileme işlevselliğini etkinleştirmek için ayarlanmış `IsPullToRefreshEnabled` true:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Çekme yenileme kullanıcı olarak çekmek:

![](interactivity-images/refresh-start.png "ListView çekerek yenileme devam ediyor")

Çekme yenileme kullanıcı çekme kullanıma sundu. Liste güncelleştiriliyor sırada hangi kullanıcının gördüğü budur: ![ ] (interactivity-images/refresh-in-progress.png "ListView çekme tam Yenile")

ListView çekme yenileme olaylara yanıt olanak tanıyan birkaç olayları gösterir.

-  `RefreshCommand` Çağrılır ve `Refreshing` olay çağrılır. `IsRefreshing` Ayarlanacak `true`.
-  Hangi kod komut veya olay, liste görünümü içeriğini yenilemek için gerekli gerçekleştirmeniz gerekir.
-  Yenileme esnasında oluşacak olan tamamlamak, çağrı `EndRefresh` veya `IsRefreshing` için `false` işiniz liste görünümü söylemek için.

`CanExecute` Özelliği dikkate, çekme Yenile komutunu etkinleştirilmesi gerekip gerekmediğini, denetim için bir yol sağlar.



## <a name="related-links"></a>İlgili bağlantılar

- [ListView etkileşim (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [1.4 sürüm notları](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 sürüm notları](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
