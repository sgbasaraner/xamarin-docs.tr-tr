---
title: Malzeme teması
description: Tema nasıl Xamarin.Android uygulamanız ile malzeme teması
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 4b9c39a0ced9a264f501d78142c3bdfd556593ed
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "30771326"
---
# <a name="material-theme"></a>Malzeme teması

*Malzeme teması* görünümleri ve etkinlikleri Android 5.0 (Lollipop) ile başlayan Görünüm ve yapısını belirleyen bir kullanıcı arabirimi stili. Sistem kullanıcı Arabirimi veya uygulamalar tarafından kullanılmak üzere malzeme teması Android 5.0 içinde yerleşik olarak bulunur. Malzeme teması bir kullanıcı, dinamik olarak ayarlar menüsünden seçim yapabileceği bir sistem genelinde görünümü seçeneğinin anlamında bir "tema" değil. Bunun yerine, malzeme teması, uygulamanızın görünümünü özelleştirmek için kullanabileceğiniz yerleşik ilgili temel stilleri bir dizi düşünülebilir.

Android üç malzeme teması özellikleri sağlar:

-  `Theme.Material` &ndash; Malzeme teması koyu sürümü; Android 5.0 varsayılan özellik budur.

-  `Theme.Material.Light` &ndash; Malzeme teması açık sürümü.

-  `Theme.Material.Light.DarkActionBar` &ndash; Malzeme teması koyu Eylem çubuğu ile ancak açık sürümü.

Bu malzeme teması özellikleri örnekleri aşağıda görüntülenir:

[![Örnek ekran görüntüleri, koyu tema, açık tema ve Eylem çubuğu koyu tema](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png#lightbox)

Malzeme temayı kendi temanızı oluşturmak için bazılarını veya tümünü renk öznitelikleri geçersiz kılma türetebilirsiniz. Örneğin, türetilen bir tema oluşturabilirsiniz `Theme.Material.Light`, ancak uygulama çubuğu rengi markanızı rengini eşleşecek şekilde geçersiz kılar. Ayrıca, tek bir görünüm stil uygulayabilirsiniz; Örneğin, bir stil oluşturabilirsiniz [CardView](~/android/user-interface/controls/card-view.md) yuvarlatılmış köşelere daha ve koyu bir arka plan rengi kullanır.

Tüm uygulama için tek bir tema kullanabilir veya farklı temalar, bir uygulamada farklı ekran (etkinlik) kullanabilirsiniz. Yukarıdaki ekran görüntülerinde, her etkinlik için farklı bir tema tek bir uygulama yerleşik renk düzenlerini göstermek için örnek, kullanır. Radyo düğmeleri uygulama için farklı etkinlikleri geçin ve sonuç olarak, farklı tema görüntüler.

Malzeme teması yalnızca Android 5.0 ve sonraki sürümlerde desteklenir. çünkü bunu (veya malzeme teması türetilen özel bir tema) tema uygulamanızı önceki Android sürümlerinde çalıştırmak için kullanamazsınız. Ancak, uygulamanızın Android 5.0 cihazları malzeme teması kullanın ve eski Android sürümleri üzerinde çalıştığında düzgün bir şekilde bir önceki tema dönmesi yapılandırabilirsiniz (bkz [Uyumluluk](#compatibility) Ayrıntılar için bu makalenin).


## <a name="requirements"></a>Gereksinimler

Aşağıdaki yeni Android 5.0 malzeme teması özelliklerini Xamarin tabanlı uygulamalarında kullanmak için gereklidir:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 veya sonraki bir sürümü yüklü ve Mac için Visual Studio veya Visual Studio ile yapılandırılmış 

-  **Android SDK'sı** &ndash; Android 5.0 (API 21) veya daha sonra Android SDK Yöneticisi aracılığıyla yüklenmesi gerekir.

-  **Java JDK 1.8** &ndash; JDK 1.7 özellikle hedefleyen API düzeyi 23 ve eski olduğunda kullanılabilir. JDK 1.8 kullanılabilir [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Android 5.0 uygulama projesinde yapılandırma konusunda bilgi için bkz: [ayarı bir Android 5.0 projeyi](~/android/platform/lollipop.md).


## <a name="using-the-built-in-themes"></a>Yerleşik Temalar kullanma

Malzeme teması kullanmak için en kolay yolu, uygulamanızı özelleştirme olmadan, yerleşik bir tema kullanacak şekilde yapılandırmaktır. Bir tema açıkça yapılandırmak istemiyorsanız, uygulamanızın varsayılan `Theme.Material` (Koyu tema). Yalnızca bir etkinlik uygulamanız varsa, uygulama düzeyinde bir tema yapılandırabilirsiniz. Uygulamanız birden fazla etkinlik varsa, uygulama düzeyinde tüm etkinliklerde aynı temayı kullanır veya farklı bir tema için farklı etkinlikleri atayabilirsiniz böylece bir tema yapılandırabilirsiniz. Aşağıdaki bölümlerde, uygulama düzeyinde ve etkinlik düzeyinde tema yapılandırma işlemleri açıklanmaktadır.


### <a name="theming-an-application"></a>Uygulama teması oluşturma

Malzeme teması flavor kullanmak için tüm uygulama yapılandırmak için `android:theme` uygulama düğümünün özniteliği **AndroidManifest.xml** aşağıdakilerden birine:

-  `@android:style/Theme.Material` &ndash; Koyu tema.

-  `@android:style/Theme.Material.Light` &ndash; Açık tema.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Açık tema koyu Eylem çubuğu ile.

Aşağıdaki örnek bir uygulamayı yapılandırır *MyApp* açık Tema kullanmak için:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternatif olarak, uygulama ayarlayabilirsiniz `Theme` özniteliğini **AssemblyInfo.cs** (veya **Properties.cs**). Örneğin:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Uygulama teması ayarlandığında `@android:style/Theme.Material.Light`, her etkinlik *MyApp* kullanılarak gösterilecektir `Theme.Material.Light`.


### <a name="theming-an-activity"></a>Bir etkinlik Tema oluşturma

Bir etkinlik tema için eklediğiniz bir `Theme` ayarını `[Activity]` özniteliği, etkinlik bildiriminin üstüne ve atama `Theme` kullanmak istediğiniz malzeme teması flavor için. Aşağıdaki örnek temaları etkinliği ile bir `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Bu uygulamadaki diğer etkinlikleri varsayılan kullanacağı `Theme.Material` koyu renk (veya yapılandırdıysanız, uygulama teması ayarı).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Özel Temalar kullanma

Uygulamanızı markanızla stiller özel bir tema oluşturarak markanızı iyileştirebilecek&rsquo;s renkleri. Özel tema oluşturma için değiştirmek istediğiniz renk öznitelikleri geçersiz kılma yerleşik bir malzeme teması flavor türeyen yeni bir stil tanımlayın. Örneğin, türetilen özel bir tema tanımlayabilirsiniz `Theme.Material.Light.DarkActionBar` ve teknik yerine bej ekranı arka plan rengi değişir.

Malzeme teması özelleştirme için aşağıdaki düzeni özniteliklerine kullanıma sunar:

-  `colorPrimary` &ndash; Uygulama çubuğunda rengi.

-  `colorPrimaryDark` &ndash; Durum çubuğu ve bağlamsal uygulama çubuklarının rengini; Bu normalde bir koyu sürümüdür `colorPrimary`.

-  `colorAccent` &ndash; Onay kutularını ve radyo düğmeleri düzenleme metin kutuları gibi kullanıcı Arabirimi denetimleri rengi.

-  `windowBackground` &ndash; Ekranı arka plan rengi.

-  `textColorPrimary` &ndash; Uygulama çubuğunda UI metin rengi.

-  `statusBarColor` &ndash; Durum çubuğu rengi.

-  `navigationBarColor` &ndash; Gezinti çubuğu rengi.

Aşağıdaki diyagramda bu ekranı alanlardan etiketli:

[![Öznitelikler ve bunların ilişkili ekranın alanları diyagramı](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png#lightbox)

Varsayılan olarak, `statusBarColor` değerine ayarlanmış `colorPrimaryDark`. Ayarlayabileceğiniz `statusBarColor` düz bir renk için veya ayarlayabilirsiniz `@android:color/transparent` durum çubuğu saydam yapmak için. Gezinti çubuğunu da ayarlayarak saydam hale getirilebilir `navigationBarColor` için `@android:color/transparent`.


### <a name="creating-a-custom-app-theme"></a>Özel uygulama Tema oluşturma

Oluşturma ve değiştirme dosyalarında özel uygulama tema oluşturabilirsiniz **kaynakları** uygulama projenizin klasör. Özel bir tema uygulamanızla stilini belirlemek için aşağıdaki adımları kullanın:

-   Oluşturma bir **colors.xml** dosyası **kaynakları/değerleri** &mdash; özel tema renkleri tanımlamak için bu dosyayı kullanın. Örneğin, aşağıdaki koda yapıştırabilirsiniz **colors.xml** başlamanıza yardımcı olmak için:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Bu örnek dosya adlarını ve renk kodlarının özel temanızı kullanacağınız renk kaynakları tanımlamak için değiştirin.

-   Oluşturma bir **değerleri/kaynaklar-v21** klasör. Bu klasörde oluşturmak bir **styles.xml** dosyası:

    [![Kaynakları/değerleri-21.xml klasöründe styles.xml konumu](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png#lightbox)

    Unutmayın **değerleri/kaynaklar-v21** Android 5.0 özgü &ndash; Android eski sürümlerini değil Bu klasördeki dosyalar okuyun.

-   Ekleme bir `resources` düğüme **styles.xml** ve tanımlayan bir `style` özel tema adını içeren düğüme. Örneğin, bir **styles.xml** tanımlayan dosyası *MyCustomTheme* (yerleşik türetilmiş `Theme.Material.Light` tema stili):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Customizations go here -->
    </style>
</resources>
```

-   Bu noktada, kullanan bir uygulamayı *MyCustomTheme* hisse görüntüleyecektir `Theme.Material.Light` özelleştirmeleri olmadan teması:

    [![Özel tema görünüm özelleştirmeleri önce](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png#lightbox)

-   Renk özelleştirmeleri ekleme **styles.xml** renkleri değiştirmek istediğiniz düzen özniteliklerin tanımlayarak. Örneğin uygulama çubuğu rengi değiştirme `my_blue` ve UI denetimine rengini değiştirmek `my_purple`, renk geçersiz kılmaları ekleyin **styles.xml** yapılandırılan renk kaynaklara başvuran **colors.xml**:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <!-- Inherit from the light Material Theme -->
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Override the app bar color -->
        <item name="android:colorPrimary">@color/my_blue</item>
        <!-- Override the color of UI controls -->
        <item name="android:colorAccent">@color/my_purple</item>
    </style>
</resources>
```

Yerinde bu değişikliklerle, bir uygulamanın kullandığı *MyCustomTheme* bir uygulama çubuğunu renkte görüntülenir `my_blue` ve kullanıcı Arabirimi denetimleri `my_purple`, ancak `Theme.Material.Light` her yerde başka bir renk şeması:

[![Özel tema görünüm özelleştirmeleri sonra](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png#lightbox)

Bu örnekte, *MyCustomTheme* renkleri çözümümüz `Theme.Material.Light` için arka plan rengi, durum çubuğu ve metin renklerini, ancak bu uygulama çubuğuna rengini değiştirir `my_blue` ve radyodüğmesinirenginiayarlar`my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Özel görünüm stili oluşturma

Android 5.0 ayrıca, tek bir görünüm stilini belirlemek için mümkün kılar. Oluşturduktan sonra **colors.xml** ve **styles.xml** için görünüm stili ekleyebilirsiniz (açıklandığı gibi önceki bölümde), **styles.xml**.
Tek bir görünüm stilini belirlemek için aşağıdaki adımları kullanın:

-   Düzen **Resources/values-v21/styles.xml** ve ekleme bir `style` stilinizin adı olan özel görünüm düğümü. Özel renk öznitelikleri bu görünümünüz için ayarlamak `style` düğümü. Örneğin, bir özel oluşturma için [CardView](~/android/user-interface/controls/card-view.md) köşeler ve kullandığı daha yuvarlanmış stili `my_blue` kart arka plan renk olarak Ekle bir `style` düğüme **styles.xml** ( içinde`resources`düğümü) ve arka plan rengini ve köşe yarıçapı yapılandırın:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   Düzeninizi içinde ayarlamak `style` , önceki adımda seçtiğiniz özel bir stil adıyla eşleşecek şekilde bu görünüm için özniteliği. Örneğin:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Aşağıdaki ekran görüntüsünde varsayılan bir örnek sağlar `CardView` (gösterilen soldaki) ile karşılaştırıldığında gibi bir `CardView` , biçimlendirilmiş özel `CardView.MyBlue` tema (sağ tarafta gösterilir):

[![Varsayılan CardView ve özel CardView örnekleri](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png#lightbox)

Bu örnekte, özel `CardView` arka plan rengi görüntülenir `my_blue` ve bir 18dp köşe yarıçapı.


## <a name="compatibility"></a>Uyumluluk

Android 5.0 malzeme teması kullanır ancak daha eski Android sürümlerindeki aşağı uyumlu stil için otomatik olarak döner. böylece uygulamanızı stilini belirlemek için aşağıdaki adımları kullanın:

-   Özel bir tema olarak tanımlayın **Resources/values-v21/styles.xml** bir malzeme teması stil türetilir. Örneğin:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Özel bir tema olarak tanımlayın **Resources/values/styles.xml** , daha eski bir temayı türer, ancak aynı tema adı olarak yukarıdaki kullanır. Örneğin:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   İçinde **AndroidManifest.xml**, özel tema adı ile uygulamanızı yapılandırın. 
    Örneğin:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternatif olarak, özel temanızı kullanarak belirli bir etkinliği stil uygulayabilirsiniz:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Tema renkleri tanımlanan kullanıyorsa bir **colors.xml** dosya, bu dosyada yerleştirdiğinizden emin olun **kaynakları/değerleri** (yerine **değerleri/kaynaklar-v21**) böylece iki sürümü özel tema rengi tanımlarınızı erişebilirsiniz.

Uygulamanızı bir Android 5.0 cihazında çalıştırıldığında, belirtilen tema tanımı kullanacağı **Resources/values-v21/styles.xml**. Bu uygulamanın eski Android cihazlarda çalıştığında, otomatik olarak belirtilen tema tanımına döner **Resources/values/styles.xml**.

Tema uyumluluğu eski Android sürümleri hakkında daha fazla bilgi için bkz. [alternatif kaynaklar](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Özet

Bu makalede, Android 5.0 (Lollipop) dahil yeni malzeme teması kullanıcı arayüzü stilini kullanıma sunuldu. Uygulamanızı stilini belirlemek için kullanabileceğiniz üç yerleşik malzeme teması özellikleri açıklanan, uygulamanızı markalama için özel bir tema oluşturma açıklandığı ve tek bir görünüm ilişkin bir örnek tema için sağlanan. Son olarak, bu makalede, Android uygulamalarının eski sürümleriyle aşağı uyumluluğu korurken malzeme teması uygulamanızda kullanma açıklanmıştır.



## <a name="related-links"></a>İlgili bağlantılar

- [ThemeSwitcher (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Lolipop giriş](../platform/lollipop.md)
- [CardView](controls/card-view.md)
- [Alternatif Kaynaklar](../app-fundamentals/resources-in-android/alternate-resources.md)
- [Android Lollipop](https://developer.android.com/about/versions/lollipop)
- [Android pasta Geliştirici](https://developer.android.com/about/versions/pie/)
- [Malzeme tasarımı](https://developer.android.com/guide/topics/ui/look-and-feel/)
- [Malzeme tasarım ilkeleri](https://material.io/design/)
- [Uyumluluk](https://developer.android.com/training/backward-compatible-ui/)
