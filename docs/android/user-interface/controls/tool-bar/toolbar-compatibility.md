---
title: "Araç çubuğu uyumluluk"
ms.topic: article
ms.prod: xamarin
ms.assetid: A0798CA1-2C7D-43B6-9E91-4435CC7B6683
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: d4d6e93bf3a755d9b48c9e096de87b4c89f2831f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="toolbar-compatibility"></a>Araç çubuğu uyumluluk

<a name="overview" />

## <a name="overview"></a>Genel Bakış

Bu bölümde nasıl kullanılacağı açıklanmaktadır `Toolbar` Android Android 5.0 Lolipop'den önceki sürümlerinde. Uygulamanızı Android Android 5.0'den önceki sürümleri desteklemiyorsa, bu bölümü atlayabilirsiniz. 

Çünkü `Toolbar` parçasıdır Android v7 destek Kitaplığı'nın bu Android 2.1 (API düzeyi 7) çalıştıran cihazlarda kullanılabilir ve daha yüksek. Ancak, [Android destek kitaplığı v7 uygulama](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet yüklü olmalıdır ve kod değiştirmiş kullandığı `Toolbar` bu Kitaplığı'nda sağlanan uygulama. Bu bölümde, bu NuGet yükleyin ve değiştirmek açıklanmaktadır **ToolbarFun** uygulamadan [ikinci bir araç çubuğu ekleme](~/android/user-interface/controls/tool-bar/adding-a-second-toolbar.md) Android Lolipop 5.0'den önceki sürümlerinde çalışır.

Araç çubuğu uygulama sürümünü kullanmak üzere bir uygulamayı değiştirmek için: 

1.  Uygulama için en az ve hedef Android sürümleri ayarlayın.

2.  Uygulama NuGet paketini yükleyin.

3.  Bir uygulama tema yerleşik Android tema yerine kullanın.

4.  Değiştirme `MainActivity` böylece, alt sınıfların `AppCompatActivity` yerine `Activity`. 

Bu adımların her biri aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.


<a name="android_version" />

## <a name="set-the-minimum-and-target-android-version"></a>En az ve hedef Android sürümü ayarlayın

Uygulamanın hedef Framework API düzeyi 21 veya daha büyük ayarlanmalı veya uygulamanın düzgün dağıtmaz. Gibi bir hata yoksa, **hiçbir kaynak tanımlayıcısı 'tileModeX' özniteliği için 'android' paketinde bulunan** görülen hedef Framework'ü ayarlanmazsa Uygulama dağıtırken, bunun nedeni **Android 5.0 (API düzeyi 21 - Lolipop)**  veya daha büyük. 

Hedef Framework'ü API düzeyi 21 düzeyini veya daha büyük ve en düşük Android desteklemek için uygulama olan sürüm için Android API düzey proje ayarları ayarlayın. Android API düzeylerini ayarlama hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md). İçinde `ToolbarFun` örnek, Minimum Android sürümü KitKat (API düzeyi 4.4) ayarlanır. 

<a name="install_nuget" />

## <a name="install-the-appcompat-nuget-package"></a>Uygulama NuGet paketini yükleyin

Ardından, eklemek [Android destek kitaplığı v7 uygulama](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) projeye paket. Visual Studio'da sağ **başvuruları** seçip **NuGet paketlerini Yönet...** . Tıklatın **Gözat** arayın ve **Android destek kitaplığı v7 uygulama**. Seçin **Xamarin.Android.Support.v7.AppCompat** tıklatıp **yükleme**: 

[![Manage NuGet paketlerine seçili V7 ekran uygulama paketi](toolbar-compatibility-images/01-appcompat-nuget-sml.png)](toolbar-compatibility-images/01-appcompat-nuget.png)

Bu NuGet yüklendiğinde, diğer bazı NuGet paketleri de yüklü zaten yüklü değilse (gibi **Xamarin.Android.Support.Animated.Vector.Drawable**, **Xamarin.Android.Support.v4**, ve **Xamarin.Android.Support.Vector.Drawable**). NuGet paketlerini yükleme hakkında daha fazla bilgi için bkz: [izlenecek yol: de dahil olmak üzere bir NuGet projenizdeki](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough). 

<a name="appcompat_theme" />

## <a name="use-an-appcompat-theme-and-toolbar"></a>Bir uygulama tema ve araç kullanın

Uygulama kitaplığı birkaç ile gelir `Theme.AppCompat` Android uygulama kitaplığı tarafından desteklenen herhangi bir sürümü kullanılabilir Temalar. `ToolbarFun` Örnek uygulama tema türetilir `Theme.Material.Light.DarkActionBar`, hangi Lolipop daha önceki Android sürümlerinde kullanılabilir değil. Bu nedenle, `ToolbarFun` Bu tema için uygulama karşılık gelen kullanmak için uyarlanmış olmalıdır `Theme.AppCompat.Light.DarkActionBar`. Ayrıca, çünkü `Toolbar` olan Android sürümlerinde kullanılabilir Lolipop,'den önceki biz uygulama sürümünü kullanmanız gerekir `Toolbar`. Bu nedenle, düzenleri kullanmalısınız `android.support.v7.widget.Toolbar` yerine `Toolbar`. 

<a name="update_layouts" />

### <a name="update-layouts"></a>Güncelleştirme düzenleri

Düzen **Resources/layout/Main.axml** ve değiştirme `Toolbar` aşağıdaki XML bir öğesiyle: 

```xml
<android.support.v7.widget.Toolbar
    android:id="@+id/edit_toolbar"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorAccent"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

Düzen **Resources/layout/toolbar.xml** ve içeriğini aşağıdaki XML ile değiştirin: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"/>
```

Unutmayın `?attr` değerlerini artık önekiyle `android:` (sözcüğünün `?` gösterimi başvuruda bulunan bir kaynak geçerli tema olarak). Varsa `?android:attr` hala kullanılan Burada, Android öznitelik değeri şu anda çalışan platform engellemeniz uygulama kitaplığı başvuru. Bu örnek kullandığından `actionBarSize` uygulama kitaplığı tarafından tanımlanan `android:` öneki bırakıldı. Benzer şekilde, `@android:style` değiştirilir `@style` böylece `android:theme` özniteliği için bir tema uygulama Kitaplığı'nda ayarlanmış &ndash; `ThemeOverlay.AppCompat.Dark.ActionBar` tema burada kullanılan yerine `ThemeOverlay.Material.Dark.ActionBar`. 

<a name="update_style" />

### <a name="update-the-style"></a>Güncelleştirme stili

Düzen **Resources/values/styles.xml** ve içeriğini aşağıdaki XML ile değiştirin: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="MyTheme.Base"> </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="windowNoTitle">true</item>
    <item name="windowActionBar">false</item>
    <item name="colorPrimary">#5A8622</item>
    <item name="colorAccent">#A88F2D</item>
  </style>
</resources>
```

Öğe adları ve bu örnekte üst tema artık ile önek `android:` çünkü uygulama kitaplığı kullanıyoruz. Üst tema için uygulama sürümü Ayrıca, değiştirilmiş `Light.DarkActionBar`. 


<a name="update_menus" />

### <a name="update-menus"></a>Güncelleştirme menüleri

Android önceki sürümleri desteklemek için uygulama kitaplığı özniteliklerini yansıtma özel öznitelikler kullanır `android:` ad alanı. Ancak, bazı öznitelikler (gibi `showAsAction` kullanılan öznitelik `<menu>` etiketi) eski cihazlarda Android Framework'teki yok &ndash; `showAsAction` Android API 11'de tanıtılan ancak Android API 7'de kullanılamaz. Bu nedenle, tüm destek kitaplığı tarafından tanımlanan özniteliklerini öneki için bir özel ad alanı kullanılmalıdır. Bir ad alanı menüsü kaynak dosyalarında adlı `local` önek için tanımlanan `showAsAction` özniteliği. 

Düzen **Resources/menu/top_menus.xml** ve içeriğini aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       local:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       local:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       local:showAsAction="never"
       android:title="Preferences" />
</menu>
```

`local` Ad alanı, bu satırıyla eklenir:

```xml
xmlns:local="http://schemas.android.com/apk/res-auto">
```

`showAsAction` Özniteliği bu başında `local:` ad alanı yerine `android:` 

```csharp
local:showAsAction="ifRoom"
```

Benzer şekilde, düzenleme **Resources/menu/edit_menus.xml** ve içeriğini aşağıdaki XML ile değiştirin:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:local="http://schemas.android.com/apk/res-auto">
  <item
       android:id="@+id/menu_cut"
       android:icon="@mipmap/ic_menu_cut_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Cut" />
  <item
       android:id="@+id/menu_copy"
       android:icon="@mipmap/ic_menu_copy_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Copy" />
  <item
       android:id="@+id/menu_paste"
       android:icon="@mipmap/ic_menu_paste_holo_dark"
       local:showAsAction="ifRoom"
       android:title="Paste" />
</menu>
```

Nasıl bu ad alanı anahtar sağlamak için destek `showAsAction` API düzeyi 11'den önceki Android sürümleri öznitelikte? Özel öznitelik `showAsAction` ve uygulama NuGet yüklendiğinde tüm olası değerlerinin uygulamada dahil edilir. 

<a name="subclass" />

## <a name="subclass-appcompatactivity"></a>Bir alt AppCompatActivity

Dönüştürme son adımda değiştirmektir `MainActivity` öğesinin bir alt kümesi olmasını `AppCompactActivity`. Düzen **MainActivity.cs** ve aşağıdakileri ekleyin `using` deyimleri: 

```csharp
using Android.Support.V7.App;
using Toolbar = Android.Support.V7.Widget.Toolbar;
```

Bu bildirir `Toolbar` uygulama sürümünde `Toolbar`. Ardından, sınıf tanımını değiştirin `MainActivity`: 

```csharp
public class MainActivity : AppCompatActivity
```

Uygulama sürümü için eylem çubuğunu ayarlamak için `Toolbar`, çağrısı yerine `SetActionBar` ile `SetSupportActionBar`. Bu örnekte, başlık de belirtmek için değiştirildiğinde uygulama sürümü `Toolbar` kullanılıyor:

```csharp
SetSupportActionBar (toolbar);
SupportActionBar.Title = "My AppCompat Toolbar";
```

Son olarak, Minimum Android düzeyi (örneğin, API 19) destekleneceği öncesi Lolipop değere değiştirin. 

Uygulamayı oluşturun ve ön Lolipop aygıt ya da Android öykünücüsünde çalıştırın. Aşağıdaki ekran görüntüsünde uygulama sürümünü gösterir **ToolbarFun** KitKat (API 19) çalıştıran bir Nexus 4: 

[![Tam ekran KitKat aygıtta çalışan uygulamanın her iki araç çubuğu gösterilir](toolbar-compatibility-images/02-running-on-kitkat-sml.png)](toolbar-compatibility-images/02-running-on-kitkat.png)

Uygulama Kitaplığı kullanıldığında, temalar geçirilmesi Android sürümlerine göre gerekmez &ndash; uygulama kitaplığı, desteklenen tüm Android sürümleri arasında tutarlı bir kullanıcı deneyimi sağlamak mümkün kılar. 




## <a name="related-links"></a>İlgili bağlantılar

- [Lolipop araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Uygulama araç çubuğu (örnek)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
