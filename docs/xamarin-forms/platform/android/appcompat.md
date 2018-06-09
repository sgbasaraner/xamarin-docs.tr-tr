---
title: Uygulama ve malzeme tasarım ekleme
description: Bu makalede, uygulama ve malzeme tasarım kullanmak için mevcut Xamarin.Forms Android uygulamaları dönüştürmek açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: c2eed44a7c684b91ceed4493a83ff3b4e1578b5f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242927"
---
# <a name="adding-appcompat-and-material-design"></a>Uygulama ve malzeme tasarım ekleme

_Uygulama ve malzeme tasarım kullanmak için mevcut Xamarin.Forms Android uygulamaları dönüştürmek için aşağıdaki adımları izleyin_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Genel Bakış

Bu yönergeler, uygulama kitaplığı kullanmasına ve malzeme tasarım Xamarin.Forms uygulamalarınızı Android sürümü etkinleştirmek için var olan Xamarin.Forms Android uygulamalarınızın güncelleştirmek açıklanmaktadır.

### <a name="1-update-xamarinforms"></a>1. Xamarin.Forms güncelleştir

2.0 veya daha yeni Xamarin.Forms çözümünü kullanarak emin olun. Xamarin.Forms Nuget Paketi 2.0 için gerekirse güncelleştirin.

### <a name="2-check-android-version"></a>2. Android sürümü denetleme

Android projenin hedef çerçevesi Android 6.0 (Marshmallow) olduğundan emin olun. Denetleme **Android projesi > Seçenekler > Yapı > Genel** corrent framework emin olmak için ayarları seçili:

 ![](appcompat-images/target-android-6-sml.png "Android genel derleme yapılandırması")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Malzeme tasarım desteklemek için yeni temalar ekleyebilirsiniz

Aşağıdaki üç dosyası Android projenize oluşturmak ve içeriğini yapıştırın. Google sağlayan bir [Stil Kılavuzu](http://www.google.com/design/spec/style/color.html#color-color-palette) ve [renk paletini Oluşturucu](http://www.materialpalette.com/) belirtilen bir alternatif renk düzeninin seçmenize yardımcı olmak için.

**Resources/Values/Colors.XML**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/Values/Style.XML**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Ek bir stil bulunması gerekir **değerleri v21** Android Lolipop ve daha yeni çalıştırırken belirli özellikleri uygulamak için klasör.

**Resources/Values-v21/Style.XML**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. AndroidManifest.xml güncelleştir

Bu yeni temayı sağlamak için kullanılan, ayarlanmış temada bilgidir **AndroidManifest** ekleyerek dosya `android:theme="@style/MyTheme"` (XML kalan olduğu gibi bırakın).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Araç çubuğu ve sekmesini düzenleri sağlayın

Oluşturma **Tabbar.axml** ve **Toolbar.axml** dosyalar **kaynakları/düzeni** dizin ve içeriklerini gelen altına yapıştırın:

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

Sekmenin yer çekimi için de dahil olmak üzere birkaç özelliği sekmeler için ayarlanan `fill` ve moduna `fixed`.
Çok sekme varsa, bu geçiş isteyebilirsiniz kaydırılabilir - Android okuma [TabLayout belgelerine](http://developer.android.com/reference/android/support/design/widget/TabLayout.html) daha fazla bilgi için.

**Resources/layout/Toolbar.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

Bu dosyalarda belirli tema, uygulamanız için değişebilir araç için oluşturuyoruz.
Başvurmak [Hello araç](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) blog gönderisi daha fazla bilgi edinin.


### <a name="6-update-the-mainactivity"></a>6. Güncelleştirme `MainActivity`

Varolan Xamarin.Forms uygulamalarda **MainActivity.cs** sınıf devralma `FormsApplicationActivity`. Bu ile değiştirilmelidir `FormsAppCompatActivity` yeni işlevselliğini etkinleştirmek için.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Son olarak, "yukarı yeni düzeni 5 adımda arasından wire" `OnCreate` aşağıda gösterildiği gibi yöntemi:

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
