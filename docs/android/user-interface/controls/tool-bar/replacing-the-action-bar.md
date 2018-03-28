---
title: Eylem çubuğunda değiştirme
ms.topic: article
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/27/2018
ms.openlocfilehash: f02f77eb45086d1d568b367b28163a4773dcd80d
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="replacing-the-action-bar"></a>Eylem çubuğunda değiştirme

## <a name="overview"></a>Genel Bakış

En yaygın birini kullanan için `Toolbar` ile özel bir varsayılan Eylem çubuğu değiştirmektir `Toolbar` (yeni bir Android projesi oluşturduğunuzda, onu varsayılan Eylem çubuğu kullanır). Çünkü `Toolbar` markalı logolar, başlıklar, menü öğeleri, gezinti düğmeleri ve hatta özel görünümler uygulama çubuk kısmına bir etkinliğin eklemenize olanak sağlayan varsayılan Eylem çubuğu üzerinde önemli bir yükseltme sunduğu UI,.

Bir uygulamanın varsayılan eylem çubuğunda değiştirmek için bir `Toolbar`: 

1.  Yeni özel bir tema oluşturun ve bu yeni temayı kullanır, böylece uygulamanın özelliklerini değiştirin. 

2.  Devre dışı `windowActionBar` özniteliği özel tema ve etkinleştirme `windowNoTitle` özniteliği.

3.  Tanımlamak için bir düzen `Toolbar`.

4.  Dahil `Toolbar` etkinliğin düzende **Main.axml** Düzen dosyası. 

5.  Etkinliğin kodu ekleyin `OnCreate` bulmaya yöntemi `Toolbar` ve arama `SetActionBar` yüklemek için `ToolBar` Eylem çubuğu olarak.

Aşağıdaki bölümlerde bu işlem ayrıntılı açıklanmıştır. Basit bir uygulama oluşturulur ve eylem çubuğunu değiştirilir özelleştirilmiş ile `Toolbar`. 



## <a name="start-an-app-project"></a>Bir uygulama projesi Başlat

Adlı yeni bir Android projesi oluşturma **ToolbarFun** (bkz [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) yeni bir Android projesi oluşturma hakkında daha fazla bilgi için). Bu Proje oluşturulduktan sonra hedef ve minimum Android API düzeylerini ayarlamak **Android 5.0 (API düzeyi 21 - Lolipop)** veya sonraki bir sürümü. Ayar Android sürümü düzeyleri hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md). Uygulama yerleşik ve çalıştırmak, bu ekran görüntüsünde görülen varsayılan eylem çubuğunda görüntülenir:

[![Varsayılan eylem çubuğunun ekran görüntüsü](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png#lightbox)



## <a name="create-a-custom-theme"></a>Özel bir tema oluşturun

Açık **kaynakları/değerleri** dizin ve yeni bir dosya olarak adlandırılan bir create **styles.xml**. İçeriği aşağıdaki XML ile değiştirin: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

Bu XML olarak adlandırılan yeni bir özel temayı tanımlayan **MyTheme** temel **Theme.Material.Light.DarkActionBar** Lolipop tema. `windowNoTitle` Özniteliği `true` başlık çubuğunu gizlemek için: 

```xml
<item name="android:windowNoTitle">true</item>
```

Varsayılan özel araç çubuğunu görüntüleme `ActionBar` devre dışı bırakılması gerekir: 

```xml
<item name="android:windowActionBar">false</item>
```

Bir olive-green `colorPrimary` ayarı araç çubuğunun arka plan rengini kullanılır: 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

## <a name="apply-the-custom-theme"></a>Özel tema uygula

Düzen **Properties/AndroidManifest.xml** ve aşağıdakileri ekleyin `android:theme` özniteliğini `<application>` öğesi uygulama kullanmayacağından `MyTheme` özel tema: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Bir uygulamaya özel bir tema uygulama hakkında daha fazla bilgi için bkz: [kullanarak özel Temalar](~/android/user-interface/material-theme.md#customtheme). 



## <a name="define-a-toolbar-layout"></a>Araç çubuğu düzeni tanımlayın

İçinde **kaynakları/düzeni** dizin adlı yeni bir dosya oluşturun **toolbar.xml**. İçeriği aşağıdaki XML ile değiştirin: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

Bu XML özel tanımlar `Toolbar` , varsayılan Eylem çubuğu yerini alır. En küçük yüksekliğini `Toolbar` yerini Eylem çubuğu boyutuna ayarlayın: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Arka plan rengini `Toolbar` daha önce tanımlanan olive-green renk ayarlanır **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

Lolipop ile başlayan `android:theme` özniteliği, tek bir görünüm stilini belirlemek için kullanılabilir. `ThemeOverlay.Material` Lolipop içinde sunulan Temalar olanaklı hale getirir varsayılan kaplamak `Theme.Material` temalar, onları açık veya koyu hale getirmek için ilgili öznitelikleri üzerine. Bu örnekte, `Toolbar` koyu Tema rengi ışığı içeriğinin; böylece kullanır: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

Bu ayar, böylece menü öğeleri koyu arka plan rengiyle Karşıtlık kullanılır.



## <a name="include-the-toolbar-layout"></a>Araç çubuğu düzeni içerir

Düzen dosyasını düzenleyin **Resources/layout/Main.axml** ve içeriğini aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
</RelativeLayout>
```

Bu düzeni içerir `Toolbar` tanımlanan **toolbar.xml** ve kullandığı bir `RelativeLayout` belirtmek için `Toolbar` (yukarıda düğme) kullanıcı Arabirimi çok üstündeki alınacaksa. 



## <a name="find-and-activate-the-toolbar"></a>Bul ve araç etkinleştirin

Düzenleme **MainActivity.cs** ve aşağıdaki ekleme deyimini kullanarak:

```csharp
using Android.Views;
```

Ayrıca, aşağıdaki kod satırlarını sonuna ekleyin `OnCreate` yöntemi:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Bu kod bulur `Toolbar` ve çağrıları `SetActionBar` böylece `Toolbar` varsayılan Eylem çubuğu özelliklerine sürer. Araç çubuğu başlığı olarak değiştirildiğinde **My araç**. Bu kod örneğinde görüldüğü gibi `ToolBar` doğrudan bir eylem çubuğu olarak başvurulabilir. Derleme ve bu uygulamayı çalıştırma &ndash; özelleştirilmiş `Toolbar` yerine varsayılan Eylem çubuğu görüntülenir: 

[![Yeşil renk düzenini içeren özelleştirilmiş bir araç çubuğu ekran görüntüsü](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png#lightbox)

Dikkat `Toolbar` bağımsız stilde `Theme.Material.Light.DarkActionBar` uygulama geri kalanı için uygulanan tema. 

Uygulama çalışırken bir özel durum oluşursa, bkz: [sorun giderme](#troubleshooting) bölümüne bakın.

 
## <a name="add-menu-items"></a>Menü öğeleri ekleme 

Bu bölümde, menüler eklenme `Toolbar`. Üst sağ alanı `ToolBar` menü öğeleri için ayrılmıştır &ndash; her bir menü öğesi (olarak da adlandırılan bir *eylem öğesi*) geçerli etkinlik içinde bir eylem gerçekleştirebilir veya tüm uygulama adına bir eylem gerçekleştirebilir. 

Menülere ekleme `Toolbar`: 

1.  Menü simgeleri (gerekliyse) eklemek `mipmap-` uygulama projesi klasörleri. Google üzerinde boş menü simgeleri kümesi sağlar [malzeme simgeler](https://design.google.com/icons/) sayfası. 

2.  Menü öğeleri içeriğini altında yeni bir menü kaynak dosyası ekleyerek tanımlarsınız **kaynakları/menüsü**. 

3.  Uygulama `OnCreateOptionsMenu` yöntemi etkinliğin &ndash; bu yöntem menü öğelerini Şişir. 

4.  Uygulama `OnOptionsItemSelected` yöntemi etkinliğin &ndash; menü öğesine dokunduğunuz olduğunda bu yöntem bir eylem gerçekleştirir. 

Aşağıdaki bölümlerde bu işlem ayrıntılı ekleyerek göstermek **Düzenle** ve **kaydetmek** özelleştirilmiş menü öğelerine `Toolbar`. 



### <a name="install-menu-icons"></a>Menü simgeleri yükleyin

Devam etmeden `ToolbarFun` örnek uygulama, menü simgeleri uygulaması projesine ekleyin. Karşıdan [araç çubuğu simgeleri](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons-plus.zip?raw=true)sıkıştırmasını açın ve ayıklanan içeriğini kopyalayın *mipmap -* projeye klasörleri *mipmap -* altındaki klasörler **ToolbarFun / Kaynakları** ve her eklenen simge dosyası projeye ekleyin.


### <a name="define-a-menu-resource"></a>Menü kaynağı tanımlayın

Yeni bir **menü** altında alt **kaynakları**. İçinde **menü** alt dizin adı verilen yeni bir menü kaynak dosyası oluşturma **top_menus.xml** ve içeriğini aşağıdaki XML ile değiştirin: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Bu XML üç menü öğeleri oluşturur:

-   Bir **Düzenle** kullanan menü öğesi `ic_action_content_create.png` simgesi (bir kalem). 

-   A **kaydetmek** kullanan menü öğesi `ic_action_content_save.png` simgesi (bir disket). 

-   A **Tercihler** simge yok menü öğesi.

`showAsAction` Özniteliklerini **Düzenle** ve **kaydetmek** menü öğeleri ayarlanır `ifRoom` &ndash; görünmesi şu menü öğeleri bu ayarı neden `Toolbar` varsa bunları görüntülenecek için yeterli yeri. **Tercihler** menü öğesi kümeleri `showAsAction` için `never` &ndash; bu neden olur **Tercihler** görünmesi menü *taşma* menü (üç Dikey noktalar). 


### <a name="implement-oncreateoptionsmenu"></a>Implement OnCreateOptionsMenu

Aşağıdaki yöntemi ekleyin **MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android çağrıları `OnCreateOptionsMenu` yöntemi uygulama menüsü kaynak bir etkinliğin belirtebilirsiniz. Bu yöntemde, **top_menus.xml** kaynak şişirileceğini geçirilen içine `menu`. Bu kod yeni neden **Düzenle**, **kaydetmek**, ve **Tercihler** görünmesi menü öğeleri `Toolbar`. 



### <a name="implement-onoptionsitemselected"></a>Uygulama OnOptionsItemSelected

Aşağıdaki yöntemi ekleyin **MainActivity.cs**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Bir kullanıcı bir menü öğesi dokunur, Android çağırır `OnOptionsItemSelected` yöntemi ve seçilen menü öğesi geçirir. Bu örnekte, uygulama yalnızca hangi menü öğesine dokunduğunuz belirtmek için bir bildirim görüntüler. 

Derleme ve çalıştırma `ToolbarFun` araç çubuğunda yeni menü öğelerini görmek için. `Toolbar` Şimdi bu ekran görüntüsünde görüldüğü gibi üç menü simgeleri görüntüler: 

[![Kaydetme, düzenleme, gösteren konumlarını Diyagram ve menü öğeleri taşması](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png#lightbox)

Bir kullanıcı dokunma zaman **Düzenle** belirtmek için menü öğesi, bir bildirim görüntülenir `OnOptionsItemSelected` yöntemi çağrıldı: 

[![Düzenleme öğesi dokunduğunuz olmadığında görüntülenen bildirim ekran görüntüsü](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png#lightbox)

Bir kullanıcı taşma menüsünü dokunur zaman **Tercihler** menü öğesi görüntülenir. Genellikle, daha az ortak Eylemler taşma menüde yerleştirilmelidir &ndash; Bu örnek için taşma menüsünü kullanır **Tercihler** kadar sık kullanılmadığından olarak **Düzenle** ve  **Kaydet**: 

[![Taşma menüsünde görüntülenen ekran görüntüsü, Tercihler menü öğesi](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png#lightbox)

Android Geliştirici Android menüleri hakkında daha fazla bilgi için bkz: [menüleri](https://developer.android.com/guide/topics/ui/menus.html) konu. 
 

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki ipuçları eylem çubuğunda bir araç çubuğu ile değiştirirken oluşabilecek sorunları hata ayıklamak için yardımcı olabilir.

### <a name="activity-already-has-an-action-bar"></a>Etkinlik bir eylem çubuğu zaten var.

Uygulama açıklandığı gibi özel bir tema kullanmak için düzgün şekilde yapılandırılmamışsa, [özel tema uygulama](#apply-the-custom-theme), uygulama çalıştırılırken şu özel durum ortaya çıkabilir:

![Özel tema kullanılmadığında oluşabilecek hata](replacing-the-action-bar-images/03-theme-not-defined.png)

Ayrıca, bir hata iletisi gibi aşağıdaki üretilen: _Java.Lang.IllegalStateException: Bu etkinlik zaten penceresi dekorasyonu tarafından sağlanan bir eylem çubuğu vardır._ 

Bu hatayı düzeltmek için aşağıdakileri doğrulayın `android:theme` özel tema eklenen özniteliğinin `<application>` (içinde **Properties/AndroidManifest.xml**) daha önce açıklandığı gibi [özel temauygulama](#apply-the-custom-theme). Ayrıca, bu hataya neden olabilir `Toolbar` düzeni veya özel tema düzgün yapılandırılmamış.


## <a name="related-links"></a>İlgili bağlantılar

- [Lolipop araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Uygulama araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
