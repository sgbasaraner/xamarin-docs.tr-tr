---
title: Malzeme tema
description: "Tema nasıl Xamarin.Android uygulamanız ile malzeme tema"
ms.topic: article
ms.prod: xamarin
ms.assetid: DC4CDBD0-3DF9-4B7E-B876-29128985E2A7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: ba73e03d6bdeea64918e0232b2188bf8e3b65084
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="material-theme"></a>Malzeme tema

<a name="overview" />

## <a name="overview"></a>Genel Bakış

*Malzeme tema* görünümleri ve etkinlikleri Android 5.0 (Lolipop) ile başlayan Görünüm ve yapısını belirleyen bir kullanıcı arabirimi stili. Sistem kullanıcı Arabirimi tarafından ve aynı zamanda uygulamalar tarafından kullanılmak üzere malzeme tema Android 5.0 oluşturulmuştur. Malzeme tema, bir kullanıcı dinamik olarak ayarlar menüsünden seçebilir bir sistem genelinde görünüm seçeneği duygusu bir "tema" değil. Bunun yerine, malzeme tema, uygulamanızı görünümünü özelleştirmek için kullanabileceğiniz yerleşik ilgili temel stilleri kümesi olarak düşünülebilir.

Android üç malzeme tema özellikleri sağlar:

-  `Theme.Material` &ndash; Malzeme tema koyu sürümü; Varsayılan özellik Android 5.0 budur.

-  `Theme.Material.Light` &ndash; Malzeme tema hafif sürümü.

-  `Theme.Material.Light.DarkActionBar` &ndash; Hafif sürümü malzeme temasının ancak koyu Eylem çubuğu.

Bu malzeme tema özellikleri örnekleri burada görüntülenir:

[![Koyu tema, açık tema ve Eylem çubuğu koyu tema örnek ekran görüntüleri](material-theme-images/three-flavors-sml.png)](material-theme-images/three-flavors.png)

Bazı veya tüm renk öznitelikleri geçersiz kılma temayı kendi tema oluşturmak için malzeme türetilemeyeceğini. Örneğin, türetilen bir tema oluşturabilirsiniz `Theme.Material.Light`, ancak uygulama çubuğu rengini, marka rengini eşleştirmek için geçersiz kılar. Ayrıca, tek bir görünüm stil ekleyebilirsiniz; Örneğin, bir stil oluşturabilirsiniz [kart görünümü](~/android/user-interface/controls/card-view.md) daha yuvarlanmış köşeleri ve koyu arka plan rengi kullanır.

Tek bir tema için tüm bir uygulamayı kullanabilir veya farklı ekranları (aktiviteler) bir uygulama için farklı temaları kullanabilirsiniz. Yukarıdaki ekran görüntülerinde her etkinlik için farklı bir tema tek bir uygulamada yerleşik renk düzenleri göstermek için örneğin, kullanır. Radyo düğmeleri uygulama için farklı etkinlikleri geçer ve sonuç olarak, farklı tema görüntüler.

Malzeme tema yalnızca Android 5.0 ve daha sonra desteklenmediği için bunu (veya malzeme temayı türetilen özel bir tema) tema uygulamanızı Android önceki sürümlerinde çalışan için kullanamazsınız. Ancak ve Android eski sürümleri çalıştırdığında düzgün bir şekilde bir önceki tema için geri dönüş malzeme tema Android 5.0 cihazlarda kullanmak için uygulamanızı yapılandırabilirsiniz (bkz [Uyumluluk](#compatibility) Ayrıntılar için bu makalenin).

<a name="requirements" />

## <a name="requirements"></a>Gereksinimler

Aşağıdaki Xamarin tabanlı uygulamalarda yeni Android 5.0 malzeme tema özellikleri kullanmak için gereklidir:

-  **Xamarin.Android** &ndash; Xamarin.Android 4.20 or later must be installed and configured with either Visual Studio or Visual Studio for Mac. 

-  **Android SDK** &ndash; Android 5.0 (API 21) veya sonrası Android SDK Yöneticisi aracılığıyla yüklü olmalıdır.

-  **Java JDK 1.8** &ndash; JDK 1.7 özellikle atamak API düzey 23 ve önceki varsa kullanılabilir. JDK 1.8 edinilebilir [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

Bir Android 5.0 uygulama projesi yapılandırma konusunda bilgi edinmek için [ayarı oluşturan bir Android 5.0 proje](~/android/platform/lollipop.md).

<a name="builtinthemes" />

## <a name="using-the-built-in-themes"></a>Yerleşik Temalar kullanma

Malzeme teması kullanmak için en kolay yolu, yerleşik bir tema özelleştirme olmadan kullanmak için uygulamanızı yapılandırmaktır. Bir tema açıkça yapılandırmak istemiyorsanız, uygulamanızı varsayılan `Theme.Material` (Koyu tema). Uygulamanızı yalnızca bir etkinlik varsa, uygulama düzeyinde bir tema yapılandırabilirsiniz. Uygulamanız birden çok etkinliği varsa, böylece tüm etkinliklerde aynı Tema kullanan veya farklı etkinlikler için farklı temaları atayabilirsiniz tema uygulama düzeyinde yapılandırabilirsiniz. Aşağıdaki bölümlerde, temalar uygulama düzeyinde ve etkinlik düzeyinde nasıl yapılandırılacağı açıklanmaktadır.

<a name="themeanapp" />

### <a name="theming-an-application"></a>Bir uygulama Tema oluşturma

Bir malzeme tema özellik kullanmak için tüm bir uygulamayı yapılandırmak için ayarlamak `android:theme` uygulama düğümünde özniteliği **AndroidManifest.xml** şunlardan biri için:

-  `@android:style/Theme.Material` &ndash; Koyu tema.

-  `@android:style/Theme.Material.Light` &ndash; Açık tema.

-  `@android:style/Theme.Material.Light.DarkActionBar` &ndash; Açık tema koyu Eylem çubuğu.

Aşağıdaki örnek uygulamayı yapılandırır *Uygulamam* açık Tema kullanmak için:

```xml
<application android:label="MyApp" 
             android:theme="@android:style/Theme.Material.Light">
</application>
```

Alternatif olarak, uygulama ayarlayabilirsiniz `Theme` özniteliğini **AssemblyInfo.cs** (veya **Properties.cs**). Örneğin:

```C#
[assembly: Application(Theme="@android:style/Theme.Material.Light")]
```

Uygulama tema ayarlandığında `@android:style/Theme.Material.Light`, her etkinlik *Uygulamam* kullanılarak görüntülenen `Theme.Material.Light`.

<a name="activitytheme" />

### <a name="theming-an-activity"></a>Bir etkinlik Tema oluşturma

Eklediğiniz bir etkinlik tema için bir `Theme` ayarını `[Activity]` özniteliği etkinlik bildiriminin üstüne ve Ata `Theme` için kullanmak istediğiniz malzeme tema çeşidi. Aşağıdaki örnek Temalar etkinliği ile bir `Theme.Material.Light`:

```C#
[Activity(Theme = "@android:style/Theme.Material.Light",
          Label = "MyApp", MainLauncher = true, Icon = "@drawable/icon")]  
```

Bu uygulamadaki diğer etkinlikler varsayılan kullanacağı `Theme.Material` koyu renk şeması (veya yapılandırdıysanız, uygulama tema ayarı).

<a name="customtheme" />

## <a name="using-custom-themes"></a>Özel tema kullanma

Uygulamanız, markanızla stiller özel bir tema oluşturarak, marka geliştirebilirsiniz&rsquo;s renkleri. Özel bir tema oluşturmak için değiştirmek istediğiniz renk öznitelikleri geçersiz kılma yerleşik bir malzeme tema özellik türetilen yeni bir stil tanımlayın. Örneğin, türetilen özel bir tema tanımlayabilirsiniz `Theme.Material.Light.DarkActionBar` ve ekran arka plan rengi beyaz yerine bej değiştirir.

Malzeme tema özelleştirme için aşağıdaki düzen öznitelikleri gösterir:

-  `colorPrimary` &ndash; Uygulama çubuğunu rengi.

-  `colorPrimaryDark` &ndash; Durum çubuğu ve bağlamsal uygulama çubukları rengini; Bu normal bir koyu sürümüdür `colorPrimary`.

-  `colorAccent` &ndash; Onay kutuları, radyo düğmeleri ve düzenleme metin kutuları gibi kullanıcı Arabirimi denetimlerini rengi.

-  `windowBackground` &ndash; Ekran arka plan rengi.

-  `textColorPrimary` &ndash; Uygulama çubuğunda UI metin rengi.

-  `statusBarColor` &ndash; Durum çubuğu rengi.

-  `navigationBarColor` &ndash; Gezinti çubuğu rengi.

Bu ekran alanlar aşağıdaki şemada etiketlenmiştir:

[ ![Öznitelikleri ve bunların ilişkili ekran alanları diyagramı](material-theme-images/screen-attributes-sml.png)](material-theme-images/screen-attributes.png)

Varsayılan olarak, `statusBarColor` değerine ayarlanmış `colorPrimaryDark`. Ayarlayabileceğiniz `statusBarColor` düz renk için veya ayarlayabilirsiniz `@android:color/transparent` durum çubuğu saydam hale getirmek için. Gezinti çubuğu de ayarlayarak saydam hale getirilebilir `navigationBarColor` için `@android:color/transparent`.

<a name="customapptheme" />

### <a name="creating-a-custom-app-theme"></a>Özel uygulama Tema oluşturma

Oluşturma ve değiştirme dosyalarında bir özel uygulama tema oluşturabilirsiniz **kaynakları** uygulama projenizin klasör. Özel bir tema ile uygulamanızı stilini belirlemek için aşağıdaki adımları kullanın:

-   Oluşturma bir **colors.xml** dosyasını **kaynakları/değerleri** &mdash; özel tema renkleri tanımlamak için bu dosyasını kullanın. Örneğin, aşağıdaki kodu yapıştırabilirsiniz **colors.xml** başlamanıza yardımcı olmak için:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<resources>
    <color name="my_blue">#3498DB</color>
    <color name="my_green">#77D065</color>
    <color name="my_purple">#B455B6</color>
    <color name="my_gray">#738182</color>
</resources>
```

-   Bu örnek dosya adları ve özel tema kullanacağınız renk kaynaklar için renk kodları tanımlamak için değiştirin.

-   Oluşturma bir **değerleri/kaynakları-v21** klasör. Bu klasörde oluşturma bir **styles.xml** dosyası:

    [ ![Değerleri/kaynakları-21.xml klasöründeki styles.xml konumu](material-theme-images/values-v21-sml.png)](material-theme-images/values-v21.png)

    Unutmayın **değerleri/kaynakları-v21** Android 5.0 özgü &ndash; Android eski sürümleri değil Bu klasördeki dosyaların okuyun.

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

-   Bu noktada, kullanan bir uygulama *MyCustomTheme* stoğu görüntülenir `Theme.Material.Light` özelleştirmeleri olmadan tema:

    [ ![Özelleştirmeleri önce özel tema görünümü](material-theme-images/custom-theme-before-sml.png)](material-theme-images/custom-theme-before.png)

-   Renk özelleştirmeleri eklemek **styles.xml** renkleri değiştirmek istediğiniz düzeni özniteliklerin tanımlayarak. Örneğin, uygulama çubuğu rengini değiştirmek için `my_blue` ve kullanıcı Arabirimi denetimlerini rengini değiştirmek `my_purple`, renk geçersiz kılan eklemek **styles.xml** yapılandırılan renk kaynaklara başvuran **colors.xml**:

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

Bir uygulama yerinde bu değişikliklerle kullanan *MyCustomTheme* bir uygulama çubuğu renkte görüntülenir `my_blue` ve UI denetimleri `my_purple`, ancak `Theme.Material.Light` renk düzenini her yerde başka:

[ ![Özelleştirmeleri sonra özel tema görünümü](material-theme-images/custom-theme-after-sml.png)](material-theme-images/custom-theme-after.png)

Bu örnekte, *MyCustomTheme* renkten taşır `Theme.Material.Light` için arka plan rengi, durum çubuğu ve metin renklerini, ancak uygulama çubuğuna rengi değişir `my_blue` ve radyodüğmesinirenginiayarlar`my_purple`.

<a name="customview" />

### <a name="creating-a-custom-view-style"></a>Özel görünüm stili oluşturma

Android 5.0 ayrıca, tek bir görünüm stilini belirlemek mümkün kılar. Oluşturduktan sonra **colors.xml** ve **styles.xml** görüntüle stil ekleyebilirsiniz (açıklandığı gibi önceki bölümdeki), **styles.xml**.
Tek bir görünüm stilini belirlemek için aşağıdaki adımları kullanın:

-   Düzen **Resources/values-v21/styles.xml** ve ekleme bir `style` özel görünüm stili adını içeren düğüme. Özel renk özniteliklerini bu içinde görünümünüz için ayarlamak `style` düğümü. Örneğin, özel bir oluşturmak için [kart görünümü](~/android/user-interface/controls/card-view.md) daha köşeleri ve kullandığı yuvarlanmış stili `my_blue` kartı arka plan rengi ekleme bir `style` düğüme **styles.xml** ( içinde`resources`düğüm) ve arka plan rengi ve köşe RADIUS yapılandırın:

```xml
<!-- Theme an individual view: -->
<style name="CardView.MyBlue">

    <!-- Change the background color to Xamarin blue: -->
    <item name="cardBackgroundColor">@color/my_blue</item>

    <!-- Make the corners very round: -->
    <item name="cardCornerRadius">18dp</item>
</style>
```

-   Düzende ayarlamak `style` önceki adımda seçtiğiniz özel stil adı ile eşleşmesi bu görünümü özniteliği. Örneğin:

```xml
<android.support.v7.widget.CardView
    style="@style/CardView.MyBlue"
    android:layout_width="200dp"
    android:layout_height="100dp"
    android:layout_gravity="center_horizontal">
```

Aşağıdaki ekran görüntüsünde varsayılan bir örnek sağlar `CardView` (solunda gösterilen) ile karşılaştırıldığında gibi bir `CardView` , stilde özel `CardView.MyBlue` tema (sağ tarafta gösterilen):

[ ![Kart görünümü varsayılan ve özel kart görünümü örnekleri](material-theme-images/custom-cardview-sml.png)](material-theme-images/custom-cardview.png)

Bu örnekte, özel `CardView` arka plan rengiyle görüntülenen `my_blue` ve 18dp köşe RADIUS.

<a name="compatibility" />

## <a name="compatibility"></a>Uyumluluk

Böylece malzeme tema üzerinde Android 5.0 kullanır ancak eski Android sürümlerinde uyumlu aşağı stili için otomatik olarak döner uygulamanızı stilini belirlemek için aşağıdaki adımları kullanın:

-   Özel bir tema tanımlamak **Resources/values-v21/styles.xml** ' nden bir malzeme tema stil türetilir. Örneğin:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Material.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   Özel bir tema tanımlamak **Resources/values/styles.xml** , daha eski bir temayı türetilen, ancak aynı tema adı olarak yukarıdaki kullanır. Örneğin:

```xml
<resources>
    <style name="MyCustomTheme" parent="android:Theme.Holo.Light">
        <!-- Your customizations go here -->
    </style>
</resources>
```

-   İçinde **AndroidManifest.xml**, uygulamanızı özel tema adıyla yapılandırın. 
    Örneğin:

```xml
<application android:label="MyApp" 
             android:theme="@style/MyCustomTheme">
</application>
```

-   Alternatif olarak, belirli bir etkinliğe özel tema kullanarak stil ekleyebilirsiniz:

```C#
[Activity(Label = "MyActivity", Theme = "@style/MyCustomTheme")]
```

Tema renkleri tanımlanan kullanıyorsa, bir **colors.xml** dosya, bu dosyada yerleştirdiğinizden emin olun **kaynakları/değerleri** (yerine **değerleri/kaynakları-v21**) böylece her iki sürümü özel tema rengi tanımlarınızı erişebilir.

Uygulamanızı bir Android 5.0 cihazında çalıştırıldığında, belirtilen tema tanımı kullanacağı **Resources/values-v21/styles.xml**. Bu uygulamanın eski Android cihazlarda çalıştığında, onu otomatik olarak belirtilen tema tanımına döner **Resources/values/styles.xml**.

Önceki Android sürümlerle tema uyumluluğu hakkında daha fazla bilgi için bkz: [alternatif kaynaklar](~/android/app-fundamentals/resources-in-android/alternate-resources.md).

## <a name="summary"></a>Özet

Bu makalede Android 5.0 (Lolipop) dahil yeni malzeme tema kullanıcı arabirimi stili sunmuştur. Uygulamanızı stilini belirlemek için kullanabileceğiniz üç yerleşik malzeme tema özellikleri açıklanan, uygulamanızı markalama için özel bir tema oluşturma açıklanmıştır ve tek tek bir görünümünü nasıl bir örneği tema için sağlanan. Son olarak, bu makalede malzeme tema uygulamanızda Android eski sürümleriyle aşağı uyumluluk korurken kullanmayı açıklanmıştır.



## <a name="related-links"></a>İlgili bağlantılar

- [ThemeSwitcher (örnek)](https://developer.xamarin.com/samples/monodroid/android5.0/ThemeSwitcher)
- [Lolipop giriş](~/android/platform/lollipop.md)
- [CardView](~/android/user-interface/controls/card-view.md)
- [Diğer kaynaklar](~/android/app-fundamentals/resources-in-android/alternate-resources.md)
- [Android L Developer Preview](http://developer.android.com/preview/index.html)
- [Malzeme tasarım](http://developer.android.com/preview/material/index.html)
- [Malzeme tasarım ilkeleri](http://static.googleusercontent.com/media/www.google.com/en/us/design/material-design.pdf)
- [Uyumluluk koruma](http://developer.android.com/preview/material/compatibility.html)
