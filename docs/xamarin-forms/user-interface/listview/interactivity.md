---
title: ListView etkileşim
description: Etkileşim seçimleri, manyetik Sil ve çekme yenileme uygulayarak, ListView ekleyin.
ms.prod: xamarin
ms.assetid: CD14EB90-B08C-4E8F-A314-DA0EEC76E647
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/01/2018
ms.openlocfilehash: 5fe821e7e5254da8febbbde518b9fd42526bf262
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848128"
---
# <a name="listview-interactivity"></a>ListView etkileşim

ListView aşağıdaki yollarla sunar verilerle etkileşimi destekler:

- [**Seçimi & Tap** ](#selectiontaps) &ndash; dokunma ve seçimleri/kaldırılmasına öğelerinin nasıl yanıt. Etkinleştirmek veya devre dışı (varsayılan olarak etkindir) satır seçimi.
- [**Bağlam Eylemler** ](#Context_Actions) &ndash; sunmaya işlevselliği her öğe, örneğin, manyetik Sil.
- [**Çekme yenileme** ](#Pull_to_Refresh) &ndash; kullanıcılar beklenir yerel deneyimleri gelmiş çekme yenileme deyim uygulayın.

<a name="selectiontaps" />

## <a name="selection--taps"></a>Seçimi & dokunma
`ListView` bir öğe seçimi birer birer destekler. Seçim varsayılan olarak açıktır. Bir kullanıcı bir öğeyi dokunur, iki olaylar: `ItemTapped` ve `ItemSelected`. Aynı öğe iki kez dokunarak birden çok ateşlenir değil, Not `ItemSelected` olayları, ancak birden çok ateşlenir `ItemTapped` olaylar. Ayrıca `ItemSelected` bir öğe seçili değilse çağrılır.

Unutmayın, `ItemSelected` öğeleri kaldırıldığında hem seçildiklerinde olarak adlandırılır. Null için denetleyin gerekir anlamına `SelectedItem` içinde `ItemSelected` kullanabilmeniz için önce olay işleyicisi:

```csharp
void OnSelection (object sender, SelectedItemChangedEventArgs e)
{
  if (e.SelectedItem == null) {
    return; //ItemSelected is called on deselection, which results in SelectedItem being set to null
  }
  DisplayAlert ("Item Selected", e.SelectedItem.ToString (), "Ok");
  //((ListView)sender).SelectedItem = null; //uncomment line if you want to disable the visual selection state.
}
```

### <a name="disabling-selection"></a>Seçimi devre dışı bırakma

Seçimi devre dışı bırakmak istiyorsanız, işleme `ItemSelected` olay ve kümesi `SelectedItem` özelliği null:

```csharp
SelectionDemoList.ItemSelected += (sender, e) => {
    ((ListView)sender).SelectedItem = null;
};
```

Seçimi etkin:

![](interactivity-images/selection-default.png "ListView etkin seçimle")

<a name="Context_Actions" />

## <a name="context-actions"></a>Bağlam Eylemler
Genellikle, kullanıcıların bir öğe üzerinde eyleme isteyeceksiniz bir `ListView`. Örneğin, e-postaları için posta uygulama listesini göz önünde bulundurun. İOS, ileti silmek için doğru çekin::

![](interactivity-images/context-default.png "ListView bağlam eylemleri ile")

C# ve XAML bağlam Eylemler uygulanabilir. Aşağıda, belirli kılavuzları her ikisi için ancak daha önce şimdi bulabilirsiniz bazı anahtar uygulama ayrıntıları her ikisi için göz atın.

Bağlam eylemleri kullanılarak oluşturulan `MenuItem`s. MenuItems için dokunun olaylar MenuItem tarafından kendisine ListView oluşturulur. Bu, burada ListView hücre yerine olayı başlatır hücreler için dokunun olayları nasıl işleneceğini gelen farklıdır. ListView olayı tanımlamaz çünkü, olay işleyicisi gibi öğesi seçili veya dokunduğunuz anahtar bilgi verilir.

Varsayılan olarak, MenuItem ait hangi hücrenin bilmesinin yolu vardır. `CommandParameter` kullanılabilir `MenuItem` nesnenin MenuItem'ın ViewCell arkasında gibi nesneleri depolamak için. `CommandParameter` hem XAML ve C# içinde ayarlanabilir.

### <a name="c"></a>C#  

Hiçbirinde içerik Eylemler uygulanabilir `Cell` (bir grup üstbilgisi olarak kullanılmadığından sürece) bir alt kümesi oluşturarak `MenuItem`s ve bunlara ekleme `ContextActions` hücre koleksiyonu. Aşağıdakilere sahip özellikler için bağlam eylemi yapılandırılabilir:

* **Metin** &ndash; menü öğesi görünür dize.
* **Tıklanan** &ndash; öğe tıklatıldığında olay.
* **IsDestructive** &ndash; (isteğe bağlı) doğru olduğunda öğeyi farklı İos'ta işlenir.

Yalnızca biri olmalıdır ancak birden fazla bağlam Eylemler bir hücreye eklenebilir `IsDestructive` kümesine `true`. Aşağıdaki kodu bağlamı eylemler için nasıl ekleneceği gösterilmektedir bir `ViewCell`:

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

`MenuItem`s de XAML koleksiyonunda bildirimli olarak oluşturulabilir. XAML iki bağlam Eylemler uygulanan özel bir hücre gösterir:

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

Arka plan kod dosyasına olun `Clicked` yöntemleri uygulanır:

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
> `NavigationPageRenderer` Android bir overridable için `UpdateMenuItemIcon` bir özel akıştan simgeler yüklemek için kullanılan yöntem `Drawable`. Bu geçersiz kılma üzerinde simgelerle SVG görüntüleri kullanmayı mümkün kılar `MenuItem` Android örneklerinde.

<a name="Pull_to_Refresh" />

## <a name="pull-to-refresh"></a>Yenileme için çekme
Kullanıcıların bir veri listesi çekme o listesinin yenileneceği beklenir geldi. `ListView` Bu out-of--box destekler. Çekme yenileme işlevselliğini etkinleştirmek üzere ayarlamak `IsPullToRefreshEnabled` true:

```csharp
listView.IsPullToRefreshEnabled = true;
```

Çekme yenileme kullanıcı olarak çekiyor:

![](interactivity-images/refresh-start.png "ListView sürüyor yenilemek için çekme")

Çekme yenileme kullanıcı olarak çekme yayımladı. Liste güncelleştiriliyor sırada ne kullanıcının gördüğü budur: ![ ] (interactivity-images/refresh-in-progress.png "ListView isteme yenileme tamamlandı")

ListView çekme yenileme olaylara yanıt olanak sağlayan birkaç olayları gösterir.

-  `RefreshCommand` Çağrılır ve `Refreshing` adlı olay. `IsRefreshing` Ayarlanacak `true`.
-  Liste görünümü, komut veya olay içeriğini yenilemek için hangi kod gereklidir gerçekleştirmeniz gerekir.
-  Yenilerken tamamlamak ise çağrı `EndRefresh` veya `IsRefreshing` için `false` bitirdiniz liste görünümü bildirmek için.

`CanExecute` Özelliği dikkate, çekme refresh komutu etkinleştirilmesi gerekip gerekmediğini hangi, denetim için bir yol sağlar.



## <a name="related-links"></a>İlgili bağlantılar

- [ListView etkileşim (örnek)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [1.4 sürüm notları](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 sürüm notları](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
