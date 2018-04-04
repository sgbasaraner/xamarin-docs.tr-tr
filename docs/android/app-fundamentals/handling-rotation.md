---
title: İşleme döndürme
description: Bu konu Xamarin.Android cihaz yönlendirmesini değişiklikleri nasıl ele alınacağını açıklar. Program aracılığıyla yönlendirmesini işlemek için değişiklikleri nasıl belirli cihaz yönlendirmesini de için kaynakları otomatik olarak yüklemek için Android kaynak sistemiyle çalışacak şekilde nasıl kapsar.
ms.prod: xamarin
ms.assetid: 6D33ADF7-ED81-0256-479D-D9E3787A76B0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 1cdc7928c45b99cdd8c8149b3ae9b06e790deeca
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="handling-rotation"></a>İşleme döndürme

_Bu konu Xamarin.Android cihaz yönlendirmesini değişiklikleri nasıl ele alınacağını açıklar. Program aracılığıyla yönlendirmesini işlemek için değişiklikleri nasıl belirli cihaz yönlendirmesini de için kaynakları otomatik olarak yüklemek için Android kaynak sistemiyle çalışacak şekilde nasıl kapsar._


## <a name="overview"></a>Genel Bakış

Mobil aygıtlar kolayca döndürülüp çünkü yerleşik döndürme mobil işletim sistemleri, standart bir özelliktir. Kullanıcı arabirimi XML veya programlama kodu bildirimli olarak oluşturulup oluşturulmadığını android uygulamaları içinde dönüş postalarla için Gelişmiş bir çerçeve sağlar. Otomatik olarak bildirim temelli düzen değişiklikleri Döndürülmüş aygıtta işlerken bir uygulamanın Android kaynak sistemi ile sıkı tümleştirme yararlanabilir. Programsal düzeni için değişiklikleri el ile yapılması gerekir. Bu daha hassas denetim çalışma zamanında, ancak daha fazla iş ödün verme pahasına geliştiricisi sağlar. Bir uygulama aynı zamanda etkinlik yeniden başlatma dışında iptal et ve yönlendirme değişiklikleri el ile denetimini ele seçebilirsiniz.

Bu kılavuz aşağıdaki yönlendirmesini konular inceler:

-   **Bildirim temelli düzeni döndürme** &ndash; nasıl Android kaynak sistem düzenleri ve drawables belirli yönleri için yükleme de dahil olmak üzere yönlendirme kullanan uygulamalar oluşturmak için kullanılır.

-   **Programsal düzeni döndürme** &ndash; denetimlerini programlı olarak nasıl ekleneceğini ve bunun yanı sıra yönlendirme değişiklikleri el ile nasıl ele alınacağını.


## <a name="handling-rotation-declaratively-with-layouts"></a>Döndürme bildirimli olarak düzenleri ile işleme

Yönlendirmeyi değiştiğinde adlandırma kurallarına uygun klasörlerde dosyaları dahil ederek, Android otomatik olarak uygun dosyalarını yükler.
İçin destek içerir:

-   *Düzen kaynakları* &ndash; hangi düzeni dosyaları için her bir yönlendirme şişirileceğini belirtme.

-   *Drawable kaynakları* &ndash; hangi drawables her yön için yüklenen belirtme.


### <a name="layout-resources"></a>Düzen kaynakları

Varsayılan olarak, Android XML (AXML) dosyaları dahil **kaynakları/düzeni** klasörü için bir etkinlik için işleme görünümleri kullanılır. Özellikle yatay için hiçbir ek Düzen kaynak sağlanırsa bu klasörün kaynakları dikey ve yatay yönlendirme için kullanılır. Varsayılan proje şablonu tarafından oluşturulan proje yapısını göz önünde bulundurun:

[![Varsayılan proje şablonu yapısı](handling-rotation-images/00.png)](handling-rotation-images/00.png#lightbox)

Bu proje tek bir oluşturur **Main.axml** dosyasını **kaynakları/düzeni** klasör. Zaman Etkinliğin `OnCreate` yöntemi çağrıldığında, görünümü içinde tanımlı Şişir **Main.axml,** XML aşağıda gösterildiği gibi bir düğme bildirir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:orientation="vertical"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
<Button  
  android:id="@+id/myButton"
  android:layout_width="fill_parent" 
  android:layout_height="wrap_content" 
  android:text="@string/hello"/>
</LinearLayout>
```

Cihaz, etkinlik 's yatay döndürülüp döndürülmeyeceğini `OnCreate` yöntemi tekrar çağrılır ve aynı **Main.axml** dosyası şişirileceğini, aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Aynı ekran ancak yatay yönde](handling-rotation-images/01-sml.png)](handling-rotation-images/01.png#lightbox)


#### <a name="orientation-specific-layouts"></a>Yönlendirme özgü düzenleri

Yerleşim klasörü yanı sıra (varsayılan olarak, dikey olarak ve açık olarak da adlandırılabilir *Düzen-port* adlı bir klasörü dahil `layout-land`), bir uygulama, gereken herhangi bir kod olmadan yatay zaman içinde görünümleri tanımlayabilirsiniz değiştirir.

Varsayalım **Main.axml** dosya aşağıdaki XML içeriyor:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is portrait"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Bir klasör içeren ek bir düzen kara adlı varsa **Main.axml** dosya yatay zaman düzende inflating şimdi neden olacak yeni eklenen yüklenirken Android projesine eklenir **Main.axml.** Yatay sürümünü göz önünde bulundurun **Main.axml** aşağıdaki kodu içeren dosyası (basitleştirmek için bu XML kodunu varsayılan dikey sürümü benzer, ancak farklı bir dizeyi kullanan `TextView`):

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="fill_parent"
  android:layout_height="fill_parent">
  <TextView
    android:text="This is landscape"
    android:layout_height="wrap_content"
    android:layout_width="fill_parent" />
</RelativeLayout>
```

Bu kodu çalıştırmak ve yatay olarak dikey aygıttan döndürme yeni XML yükleme aşağıda gösterildiği gibi gösterir:

[![Dikey moda yazdırma yatay ve dikey ekran görüntüleri](handling-rotation-images/02.png)](handling-rotation-images/02.png#lightbox)


### <a name="drawable-resources"></a>Drawable kaynakları

Döndürme sırasında Android drawable kaynakları düzeni kaynaklara benzer şekilde davranır. Bu durumda, gelen drawables sistem alır **kaynakları/drawable** ve **kaynakları/drawable-kara** klasörler, sırasıyla.

Örneğin, proje içinde Monkey.png adlı bir resim içeriyor deyin **kaynakları/drawable** klasörü, burada drawable başvurulmaktadır gelen bir `ImageView` XML şuna benzer:

```xml
<ImageView
  android:layout_height="wrap_content"
  android:layout_width="wrap_content"
  android:src="@drawable/monkey"
  android:layout_centerVertical="true"
  android:layout_centerHorizontal="true" />
```

Daha fazla, varsayalım farklı bir sürümünü **Monkey.png** altında bulunan **kaynakları/drawable-kara**. Düzen dosyalarla cihaz gibi yalnızca, belirli yönlendirme için drawable değişiklikleri aşağıda gösterildiği gibi döndürülmüş:

[![Dikey ve yatay modlarında gösterilen Monkey.png farklı sürümü](handling-rotation-images/03.png)](handling-rotation-images/03.png#lightbox)


## <a name="handling-rotation-programmatically"></a>Döndürme programlı olarak işleme

Bazen düzenleri kodda tanımlarız. Bu, çeşitli nedenlerle, teknik kısıtlamalar, geliştirici tercih vb. de dahil olmak üzere için meydana gelebilir. Biz denetimlerini programlı olarak eklediğinizde uygulamanın el ile XML kaynakları kullanırız bağlandığınızda otomatik olarak gerçekleştirilir cihaz yönlendirmesini için dikkate alması gerekir.


### <a name="adding-controls-in-code"></a>Kodda denetimler ekleme

Denetimlerini programlı olarak eklemek için aşağıdaki adımları gerçekleştirmek bir uygulama gerekir:

-  Bir düzen oluşturun.
-  Düzen parametrelerini ayarlayın.
-  Denetimleri oluşturun.
-  Denetim düzeni parametrelerini ayarlayın.
-  Denetimleri düzene ekleyin.
-  Düzenini içerik görünümü olarak ayarlayın.

Örneğin, bir tek oluşan bir kullanıcı arabirimi göz önünde bulundurun `TextView` eklenen denetim bir `RelativeLayout`aşağıdaki kodda gösterildiği gibi.

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
        
  // create TextView control
  var tv = new TextView (this);

  // set TextView's LayoutParameters
  tv.LayoutParameters = layoutParams;
  tv.Text = "Programmatic layout";

  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Bu kod örneği oluşturur bir `RelativeLayout` sınıfı ve kümelerini kendi `LayoutParameters` özelliği. `LayoutParams` Denetimleri yeniden kullanılabilir bir şekilde nasıl konumlandırılacağını Kapsüllenen Android'ın yolu bir sınıftır. Bir düzen örneği oluşturulduktan sonra denetimleri oluşturulur ve kendisine eklenir. Denetimler de `LayoutParameters`, gibi `TextView` Bu örnekte. Sonra `TextView` eklemeyi oluşturulur, `RelativeLayout` ve ayarı `RelativeLayout` uygulama görüntüleme içerik görünümü sonuçları olarak `TextView` gösterildiği gibi:

[![Dikey ve yatay modlarında gösterilen artışı sayaç düğmesi](handling-rotation-images/04.png)](handling-rotation-images/04.png#lightbox)


### <a name="detecting-orientation-in-code"></a>Kodda yönlendirmesini algılama

Bir uygulama her yön için farklı kullanıcı arabirimi yük çalışırsa zaman `OnCreate` olarak adlandırılır (Bu, bir cihaz Döndürülmüş her zaman gerçekleşir), yönlendirme algılamak ve istenen kullanıcı arabirimi kodu yük gerekir. Android sahip adlı bir sınıf `WindowManager`, geçerli bir aygıt döndürme aracılığıyla belirlemek için kullanılabilecek `WindowManager.DefaultDisplay.Rotation` özelliği, aşağıda gösterildiği gibi:

```csharp
protected override void OnCreate (Bundle bundle)
{
  base.OnCreate (bundle);
                        
  // create a layout
  var rl = new RelativeLayout (this);

  // set layout parameters
  var layoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.FillParent);
  rl.LayoutParameters = layoutParams;
                        
  // get the initial orientation
  var surfaceOrientation = WindowManager.DefaultDisplay.Rotation;
  // create layout based upon orientation
  RelativeLayout.LayoutParams tvLayoutParams;
                
  if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
  } else {
    tvLayoutParams = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    tvLayoutParams.LeftMargin = 100;
    tvLayoutParams.TopMargin = 100;
  }
                        
  // create TextView control
  var tv = new TextView (this);
  tv.LayoutParameters = tvLayoutParams;
  tv.Text = "Programmatic layout";
        
  // add TextView to the layout
  rl.AddView (tv);
        
  // set the layout as the content view
  SetContentView (rl);
}
```

Bu kod ayarlar `TextView` pikseller yukarıdan konumlandırılmış 100 olacak şekilde otomatik olarak yeni düzene yatay olarak aşağıda gösterildiği gibi döndürülmüş zaman animasyon ekranın sol:

[![Görünüm durumu üzerinde dikey ve yatay modları korunur](handling-rotation-images/05.png)](handling-rotation-images/05.png#lightbox)


### <a name="preventing-activity-restart"></a>Etkinlik yeniden önleme

Her şeyi işleme yanı sıra `OnCreate`, bir uygulama aynı zamanda bir etkinlik yönlendirmesini ayarlayarak değiştiğinde yeniden engelleyebilirsiniz `ConfigurationChanges` içinde `ActivityAttribute` gibi:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
```
Şimdi cihaz döndürüldüğünde, etkinlik yeniden başlatılmaz. El ile yönlendirmesini değiştirme bu durumda işlemek için bir etkinlik kılabilirsiniz `OnConfigurationChanged` yöntemi ve yönünü belirlemek `Configuration` etkinlik yeni uygulama olduğu gibi geçirilen nesnesi:

```csharp
[Activity (Label = "CodeLayoutActivity", ConfigurationChanges=Android.Content.PM.ConfigChanges.Orientation | Android.Content.PM.ConfigChanges.ScreenSize)]
public class CodeLayoutActivity : Activity
{
  TextView _tv;
  RelativeLayout.LayoutParams _layoutParamsPortrait;
  RelativeLayout.LayoutParams _layoutParamsLandscape;
                
  protected override void OnCreate (Bundle bundle)
  {
    // create a layout
    // set layout parameters
    // get the initial orientation

    // create portrait and landscape layout for the TextView
    _layoutParamsPortrait = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
                
    _layoutParamsLandscape = new RelativeLayout.LayoutParams (ViewGroup.LayoutParams.FillParent, ViewGroup.LayoutParams.WrapContent);
    _layoutParamsLandscape.LeftMargin = 100;
    _layoutParamsLandscape.TopMargin = 100;
                        
    _tv = new TextView (this);
                        
    if (surfaceOrientation == SurfaceOrientation.Rotation0 || surfaceOrientation == SurfaceOrientation.Rotation180) {
      _tv.LayoutParameters = _layoutParamsPortrait;
    } else {
      _tv.LayoutParameters = _layoutParamsLandscape;
    }
                        
    _tv.Text = "Programmatic layout";
    rl.AddView (_tv);
    SetContentView (rl);
  }
                
  public override void OnConfigurationChanged (Android.Content.Res.Configuration newConfig)
  {
    base.OnConfigurationChanged (newConfig);
                        
    if (newConfig.Orientation == Android.Content.Res.Orientation.Portrait) {
      _tv.LayoutParameters = _layoutParamsPortrait;
      _tv.Text = "Changed to portrait";
    } else if (newConfig.Orientation == Android.Content.Res.Orientation.Landscape) {
      _tv.LayoutParameters = _layoutParamsLandscape;
      _tv.Text = "Changed to landscape";
    }
  }
}
```

Burada `TextView's` düzeni parametreleri yatay ve dikey için başlatılır. Sınıf değişkenleri ile birlikte parametreleri tutun `TextView` kendisi, yönlendirme değiştiğinde etkinlik yeniden oluşturulmayacak beri. Kodu hala kullanan `surfaceOrientartion` içinde `OnCreate` ilk düzenini ayarlamak için `TextView`. Bundan sonra `OnConfigurationChanged` tüm sonraki düzen değişiklikleri işler.

Biz uygulamayı çalıştırdığınızda, Android cihaz rotasyonu oluşur ve etkinlik yeniden başlatmamasını gibi kullanıcı arabirimi değişiklikleri yükler.


## <a name="preventing-activity-restart-for-declarative-layouts"></a>Bildirim temelli düzenleri için engelleme etkinliğini yeniden başlatma

Biz düzeni XML'de tanımlarsanız etkinlik yeniden aygıt döndürme neden de engellenebilir. Örneğin, bir etkinlik yeniden başlatmayı engellemek isterseniz Biz bu yaklaşım kullanabilirsiniz (performans nedenleriyle, belki de) ve yeni kaynaklar için farklı yönleri yüklemek gerekli değil.

Bunu yapmak için biz programlı bir düzen kullanırız aynı yordamı izleyin. Basitçe `ConfigurationChanges` içinde `ActivityAttribute`yaptığımız gibi `CodeLayoutActivity` daha önce. Yönlendirmesini değiştirme yeniden uygulanabilir için çalıştırmak için gereken herhangi bir kod `OnConfigurationChanged` yöntemi.


## <a name="maintaining-state-during-orientation-changes"></a>Yönlendirme değişiklikleri sırasında durum koruma

Döndürme işleme bildirimli olarak veya program aracılığıyla olsun, tüm Android uygulamalarını cihaz yönlendirmesini değiştiğinde durumunu yönetmek için aynı teknikleri uygulamanız gerekir. Bir Android cihaz döndürüldüğünde sistem çalışan bir etkinlik yeniden durumunu yönetme önemlidir. Android düzenleri ve belirli bir yön için özellikle tasarlanmış drawables gibi diğer kaynaklar yük kolaylaştırmak için bunu yapar. Başlatıldığında, etkinlik yerel sınıfı değişkenlerde sakladığınız herhangi bir geçici durum kaybeder. Bu nedenle, bir etkinlik durumu bağımlı ise, uygulama düzeyinde durumu kalıcı gerekir. Bir uygulamayı işlemek için gereken kaydetme ve geri yükleme arasında yönlendirme değişiklikleri korumak için istediği herhangi bir uygulama durumu.

Android kalıcı durumda üzerinde daha fazla bilgi için bkz [etkinlik yaşam döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md) Kılavuzu.


## <a name="summary"></a>Özet

Bu makalede ele alınan Android'ın yerleşik özellikleri döndürme ile çalışmak için nasıl kullanılacağını. İlk olarak, Android kaynak sistem yönlendirme kullanan uygulamalar oluşturmak için nasıl kullanılacağı açıklanmıştır. Ardından kodda denetimleri ekleme ve bunun yanı sıra yönlendirme değişiklikleri el ile nasıl ele alınacağını sunulmuştur.



## <a name="related-links"></a>İlgili bağlantılar

- [Döndürme Demo (örnek)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/RotationDemo/)
- [Etkinlik Yaşam Döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Çalışma zamanı değişiklikleri işleme](http://developer.android.com/guide/topics/resources/runtime-changes.html)
- [Hızlı ekran yönünü değiştirme](http://android-developers.blogspot.com/2009/02/faster-screen-orientation-change.html)
