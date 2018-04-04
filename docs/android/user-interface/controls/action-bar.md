---
title: ActionBar
ms.prod: xamarin
ms.assetid: 84A79F1F-9E73-4E3E-80FA-B72E5686900B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 29726450e0899177f3223deeade1cb4766d933a5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="actionbar"></a>ActionBar


## <a name="overview"></a>Genel Bakış

Kullanırken `TabActivity`, sekme simgeler oluşturmak için kodu karşı Android 4.0 framework çalıştırdığınızda herhangi bir etkisi olmaz. 2.3 önce Android sürümlerinde olduğu gibi işlevsel olarak çalıştığını rağmen `TabActivity` sınıfının kendisi kullanım dışı bırakılmış 4. 0 '. Sekmeli arabirim oluşturmak için yeni bir yol biz sonraki ele alacağız Eylem çubuğu kullanan sunulmuştur.


## <a name="action-bar-tabs"></a>Eylem çubuğunda sekmeleri

Eylem çubuğunda sekmeli arabirimleri Android 4.0 ekleme desteği içerir.
Aşağıdaki ekran görüntüsünde, böyle bir arabirim örneği gösterilmektedir.

[![Uygulamasının ekran görüntüsü bir öykünücü çalıştıran; iki sekme gösterilir](action-bar-images/25-actionbartabs.png)](action-bar-images/25-actionbartabs.png#lightbox)

Eylem çubuğunda sekmeleri oluşturmak için öncelikle ayarlamak ihtiyacımız kendi `NavigationMode` sekmeleri desteklemek için özelliği. Android 4'te bir `ActionBar` özelliği ayarlamak için kullanabileceğiniz etkinlik sınıf üzerinde kullanılabilir `NavigationMode` şöyle:

```csharp
this.ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
```

Bunu yaptıktan sonra sekme çağırarak oluşturabiliriz `NewTab` eylem çubuğunda yöntemi. Bu sekme örneğiyle diyoruz `SetText` ve `SetIcon` sekmenin etiketi ve simge; ayarlamak için yöntemleri aşağıda gösterilen kodu sırada bu çağrılar yapılır:

```csharp
var tab = this.ActionBar.NewTab ();
tab.SetText (tabText);
tab.SetIcon (Resource.Drawable.ic_tab_white);
```

Sekme ancak ekleyebilmeniz için önce işlemek ihtiyacımız `TabSelected` olay. Bu işleyici, sekme için içerik oluşturabilir. Eylem çubuğunda sekme çalışmak üzere tasarlanmıştır *parçaları*, bir etkinlik kullanıcı arabiriminde bir bölümünü temsil eden sınıflar olduğu. Bu örnekte, tek bir parça'nın görünümü içerir `TextView`, hangi biz de Şişir bizim `Fragment` alt şöyle:

```csharp
class SampleTabFragment: Fragment
{           
    public override View OnCreateView (LayoutInflater inflater,
        ViewGroup container, Bundle savedInstanceState)
    {
        base.OnCreateView (inflater, container, savedInstanceState);
       
        var view = inflater.Inflate (
            Resource.Layout.Tab, container, false);

        var sampleTextView =
            view.FindViewById<TextView> (Resource.Id.sampleTextView);            
        sampleTextView.Text = "sample fragment text";


        return view;
    }
}
```

Olay bağımsız değişken geçirildi `TabSelected` olay türüdür `TabEventArgs`, içeren bir `FragmentTransaction` parça aşağıda gösterildiği gibi ekleyin kullanırız özelliği:

```csharp
tab.TabSelected += delegate(object sender, ActionBar.TabEventArgs e) {             
    e.FragmentTransaction.Add (Resource.Id.fragmentContainer,
        new SampleTabFragment ());
};
```

Son olarak, sekme eylemi çubuğuna çağırarak ekleyebiliriz `AddTab` bu kodda gösterildiği gibi yöntemi:

```csharp
this.ActionBar.AddTab (tab);
```

Tam bir örnek için bkz: *HelloTabsICS* bu belge için örnek kod projesinde.


## <a name="shareactionprovider"></a>ShareActionProvider

`ShareActionProvider` Sınıfı bir eylem çubuğundan gerçekleşmesi için bir paylaşım eylemi sağlar. Bu paylaşım hedefi işleyebilir ve kolay erişim için daha önce kullanılan uygulamaların geçmişini bunları daha sonra eylem çubuğundan tutar uygulamaların bir listesini içeren bir eylem görünüm oluşturmayı mvc'deki. Bu uygulamaların Android tutarlı bir kullanıcı deneyimi aracılığıyla veri paylaşmasını sağlar.


### <a name="image-sharing-example"></a>Resim paylaşımı örneği

Örneğin, bir görüntü paylaşmak için bir eylem çubuğunun menü öğesi ile bir ekran görüntüsü aşağıda verilmiştir (alınan [ShareActionProvider](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/) örnek). Menü öğesi eylem çubuğunda kullanıcı dokunur, ShareActionProvider ile ilişkili bir hedefi işlemek için uygulama yükler `ShareActionProvider`. Eylem çubuğunda sunulan şekilde bu örnekte, ileti uygulama daha önce kullanıldı.

[![Uygulama simgesi eylem çubuğunda Mesajlaşma ekran görüntüsü](action-bar-images/09-shareactionprovider.png)](action-bar-images/09-shareactionprovider.png#lightbox)


Kullanıcı eylem çubuğundaki öğeyi tıklattığında, paylaşılan görüntüsünü içeren Mesajlaşma uygulaması, aşağıda gösterildiği gibi başlatılır:

[![Monkey görüntü görüntüleme Mesajlaşma uygulamasının ekran görüntüsü](action-bar-images/10-messagewithimage.png)](action-bar-images/10-messagewithimage.png#lightbox)


### <a name="specifying-the-action-provider-class"></a>Sağlayıcı sınıfı eylem belirtme

Kullanılacak `ShareActionProvider`ayarlayın `android:actionProviderClass` menü öğesi eylem çubuğunun menüsü XML gibi özniteliği:

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:id="@+id/shareMenuItem"
      android:showAsAction="always"
      android:title="@string/sharePicture"
      android:actionProviderClass="android.widget.ShareActionProvider" />
</menu>
```


### <a name="inflating-the-menu"></a>Menü inflating

Menü Şişir için biz geçersiz kılma `OnCreateOptionsMenu` etkinlik alt. Menü başvuru sahibiz sonra biz elde edebilirsiniz `ShareActionProvider` gelen `ActionProvider` ayarlamak için SetShareIntent yöntemini kullanın ve menü öğesi özelliği `ShareActionProvider`'s aşağıda gösterildiği gibi hedefi:

```csharp
public override bool OnCreateOptionsMenu (IMenu menu)
{
    MenuInflater.Inflate (Resource.Menu.ActionBarMenu, menu);       
           
    var shareMenuItem = menu.FindItem (Resource.Id.shareMenuItem);           
    var shareActionProvider =
       (ShareActionProvider)shareMenuItem.ActionProvider;
    shareActionProvider.SetShareIntent (CreateIntent ());
}
```


### <a name="creating-the-intent"></a>Amacı oluşturma

`ShareActionProvider` Geçirilen amacı, kullanacağı `SetShareIntent` uygun etkinliği başlatmak için yukarıdaki koddaki yöntemi. Bu durumda aşağıdaki kodu kullanarak bir görüntü göndermek için bir hedefi oluşturun:

```csharp
Intent CreateIntent ()
{  
    var sendPictureIntent = new Intent (Intent.ActionSend);
    sendPictureIntent.SetType ("image/*");
    var uri = Android.Net.Uri.FromFile (GetFileStreamPath ("monkey.png"));          
    sendPictureIntent.PutExtra (Intent.ExtraStream, uri);
    return sendPictureIntent;
}
```

Görüntünün Yukarıdaki kod örneğinde uygulama ile bir varlık olarak eklenen ve etkinlik oluşturulduğunda, ileti uygulaması gibi diğer uygulamalar için erişilebilir olması için genel olarak erişilebilir bir konuma kopyalanır. Bu makalede eşlik örnek kod kullanımını gösteren Bu örnekte, tam kaynağını içerir.



## <a name="related-links"></a>İlgili bağlantılar

- [Merhaba sekmeleri ICS (örnek)](https://developer.xamarin.com/samples/HelloTabsICS/)
- [ShareActionProvider Demo (örnek)](https://developer.xamarin.com/samples/monodroid/ShareActionProviderDemo/)
- [Dondurma Sandwich Tanıtımı](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 platformu](http://developer.android.com/sdk/android-4.0.html)
