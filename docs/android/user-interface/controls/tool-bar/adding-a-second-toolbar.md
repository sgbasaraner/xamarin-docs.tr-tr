---
title: "İkinci bir araç çubuğu ekleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: FCE0AD27-8B6B-47C6-AD19-2B1C12E1BBBF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b471742ae9fb365d75e8dd3ca0f93f5e55208f19
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-a-second-toolbar"></a>İkinci bir araç çubuğu ekleme

<a name="overview" />

## <a name="overview"></a>Genel Bakış 

`Toolbar` Birden çok Değiştir eylemi çubuğu yapabilirsiniz &ndash; içinde bir etkinlik birden çok kez kullanılabilir, olması olabilir ekran üzerinde herhangi bir yere yerleştirme için özelleştirilmiş ve kısmi genişliğini ekran span şekilde yapılandırılabilir. Aşağıdaki örnekler bir ikinci oluşturmak nasıl çalışılacağını `Toolbar` ve ekranın alt kısmında yerleştirin. Bu `Toolbar` uygulayan **kopya**, **Kes**, ve **Yapıştır** menü öğeleri. 

<a name="define_second" />

## <a name="define-the-second-toolbar"></a>İkinci araç tanımlayın 

Düzen dosyasını düzenleyin **Main.axml** ve aşağıdaki XML içeriği ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:id="@+id/main_content"
        android:layout_below="@id/toolbar">
      <ImageView
          android:layout_width="fill_parent"
          android:layout_height="0dp"
          android:layout_weight="1" />
      <Toolbar
          android:id="@+id/edit_toolbar"
          android:minHeight="?android:attr/actionBarSize"
          android:background="?android:attr/colorAccent"
          android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
          android:layout_width="match_parent"
          android:layout_height="wrap_content" />
    </LinearLayout>
</RelativeLayout>
```

Bu XML saniyenin ekler `Toolbar` boş bir ekranın altına `ImageView` ekranın Orta doldurma. Bu yüksekliğini `Toolbar` bir eylem çubuğu yüksekliğini ayarlayın: 

```xml
android:minHeight="?android:attr/actionBarSize"
```

Bu arka plan rengini `Toolbar` sonraki tanımlanacağı bir Aksan rengi ayarlayın:

```xml
android:background="?android:attr/colorAccent
```

Bildirim bu `Toolbar` farklı bir temasına bağlı (**ThemeOverlay.Material.Dark.ActionBar**) tarafından kullanılan daha `Toolbar` oluşturulan [Eylem çubuğu değiştirme](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md) &ndash;etkinliğin penceresi dekorasyonu veya ilk kullanılan temayı bağlı değil `Toolbar`.

Düzen **Resources/values/styles.xml** ve aşağıdaki Aksan rengi stili tanımına ekleyin: 

```xml
<item name="android:colorAccent">#C7A935</item>
```

Bu alt kısımdaki araç koyu amber renk sağlar. Derleme ve uygulamayı çalıştıran boş ikinci araç çubuğunu ekranın alt kısmında görüntüler: 

[![Ekranın altındaki sarı ikinci araç ile uygulamasının ekran görüntüsü](adding-a-second-toolbar-images/01-second-toolbar-sml.png)](adding-a-second-toolbar-images/01-second-toolbar.png)


<a name="second_menus" />
 
## <a name="add-edit-menu-items"></a>Düzen menüsü öğeleri Ekle 

Bu bölümde altına Düzenle menü öğelerini eklemek açıklanmaktadır `Toolbar`. 

Menü öğeleri için ikincil kopya eklemek için `Toolbar`: 

1.  Menü simgeleri ekleme `mipmap-` (gerekliyse) uygulama projesi klasörleri.

2.  Bir ek menü kaynak dosyasına ekleyerek menü öğelerini içeriğini tanımlayın **kaynakları/menüsü**. 

3.  Etkinliğin içinde `OnCreate` yöntemi, bulma `Toolbar` (çağırarak `FindViewById`) ve Şişir `Toolbar`'s menüleri.

4.  Bir click işleyicisi uygulamak `OnCreate` yeni menü öğeleri için. 

Aşağıdaki bölümlerde bu süreci ayrıntılı gösteren: **Kes**, **kopya**, ve **Yapıştır** menü öğeleri altına eklenen `Toolbar`. 


<a name="second_resource" />

### <a name="define-the-edit-menu-resource"></a>Düzen menüsü kaynak tanımlayın

İçinde **kaynakları/menüsü** alt dizin adı verilen yeni bir XML dosyası oluşturma **edit_menus.xml** ve içeriği aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       android:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Bu XML oluşturur **Kes**, **kopya**, ve **Yapıştır** menü öğelerini (eklenme simgelerle `mipmap-` klasörlerde [Eylem çubuğu değiştirme ](~/android/user-interface/controls/tool-bar/replacing-the-action-bar.md)).


<a name="inflate_menus" />

### <a name="inflate-the-menus"></a>Menüleri Şişir

Sonunda `OnCreate` yönteminde **MainActivity.cs**, kod aşağıdaki satırları ekleyin: 

```csharp
var editToolbar = FindViewById<Toolbar>(Resource.Id.edit_toolbar);
editToolbar.Title = "Editing";
editToolbar.InflateMenu (Resource.Menu.edit_menus);
editToolbar.MenuItemClick += (sender, e) => {
    Toast.MakeText(this, "Bottom toolbar tapped: " + e.Item.TitleFormatted, ToastLength.Short).Show();
};
```

Bu kod bulur `edit_toolbar` tanımlanan Görünüm **Main.axml**, başlığını ayarlar **düzenleme**ve menü öğelerinden Şişir (tanımlanan **edit_menus.xml**). Bir menüyü tanımlar hangi düzenleme simgesine dokunduğunuz belirtmek için bir bildirim görüntüler işleyicisi'ı tıklatın. 

Derleme ve uygulamayı çalıştırın. Uygulama çalıştırıldığında metin ve yukarıya eklenen simgeler aşağıda gösterildiği gibi görünür: 

[![Alt araç kesme, kopyalama ve yapıştırma simgelerle diyagramı](adding-a-second-toolbar-images/02-bottom-toolbar-sml.png)](adding-a-second-toolbar-images/02-bottom-toolbar.png)

Dokunarak **Kes** menüsü simgesini görüntülenecek aşağıdaki bildirim neden olur: 

[![Gösteren bildirim ekran kesme menüsü simgesini dokunduğunuz](adding-a-second-toolbar-images/03-bottom-tapped-sml.png)](adding-a-second-toolbar-images/03-bottom-tapped.png)

Menü öğeleri ya da araç çubuğunda dokunarak elde edilen bildirimleri görüntüler: 

[![Ekran bildirimleri, kaydetme, kopyalama ve yapıştırma dokunduğunuz menü öğeleri](adding-a-second-toolbar-images/04-menu-action-sml.png)](adding-a-second-toolbar-images/04-menu-action.png)


<a name="up_button" />

## <a name="the-up-button"></a>Yukarı düğmesi 

Çoğu Android uygulamalarını Bel **geri** için uygulama gezinti düğmesini; tuşuna basarak **geri** düğmesi önceki ekrana kullanıcıya alır.
Ancak, siz de sağlamak isteyebilirsiniz bir **yukarı** uygulamanın ana ekranına "en" gitmek kullanıcıların kolaylaştırır düğmesi. Kullanıcı seçtiğinde **yukarı** düğmesi, uygulama hiyerarşisinde daha yüksek bir düzeye kullanıcı yukarı taşır &ndash; diğer bir deyişle, uygulamanın kullanıcı geri önceden ziyaret pencerelerinin geri yerine arka yığını birden çok etkinliği POP Etkinlik. 

Etkinleştirmek için **yukarı** kullanır ikinci bir etkinlik düğmesini bir `Toolbar` eylem çubuğunu çağrı `SetDisplayHomeAsUpEnabled` ve `SetHomeButtonEnabled` ikinci etkinliğin yöntemleri `OnCreate` yöntemi:

```csharp
SetActionBar (toolbar);
...
ActionBar.SetDisplayHomeAsUpEnabled (true);
ActionBar.SetHomeButtonEnabled (true);
```

[Destek v7 araç](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/) kod örneği gösterir **yukarı** eylem düğmesi. (Sonraki bölümde açıklandığı uygulama kitaplığı kullanır) Bu örnek araç kullanır ikinci bir etkinlik uygulayan **yukarı** önceki etkinliğin dön hiyerarşik gezinti düğmesi. Bu örnekte, `DetailActivity` giriş düğmesini etkinleştirir **yukarı** aşağıdakileri yaparak düğmesi `SupportActionBar` yöntem çağrıları: 

```csharp
SetSupportActionBar (toolbar);
...
SupportActionBar.SetDisplayHomeAsUpEnabled (true);
SupportActionBar.SetHomeButtonEnabled (true);
```

Kullanıcı ne zaman gider `MainActivity` için `DetailActivity`, `DetailActivity` görüntüler bir **yukarı** ekran görüntüsü gösterildiği gibi düğmesi (sol işaret ok):

[![Bir araç çubuğunda yukarı düğmesi sol ok örneği ekran görüntüsü](adding-a-second-toolbar-images/05-up-button-sml.png)](adding-a-second-toolbar-images/05-up-button.png)

Bu dokunun **yukarı** düğme neden geri dönmek uygulama `MainActivity`. Birden çok hiyerarşi düzeyi ile daha karmaşık bir uygulamada bu düğmesine kullanıcı uygulamasında bir sonraki en yüksek düzeye yerine önceki ekrana döndürecektir. 



## <a name="related-links"></a>İlgili bağlantılar

- [Lolipop araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Uygulama araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
