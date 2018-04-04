---
title: Merhaba, yıpranması
description: Android takmak ilk uygulamanızı oluşturma ve yıpranması öykünücü veya cihaz üzerinde çalıştırın. Bu kılavuzda düğme tıklamaları işler ve yıpranması aygıtta tıklatın sayaç görüntüleyen bir küçük Android takmak projesi oluşturmak için adım adım yönergeler sağlar. Bu uygulamayı yıpranması öykünücü veya bir Android telefonu Bluetooth bağlanan yıpranması aygıtı kullanarak hata ayıklama açıklanmaktadır. Ayrıca, Android takmak için bir dizi hata ayıklama ipuçları da sağlar.
ms.prod: xamarin
ms.assetid: 86BCD0E7-E9DC-40F1-9B44-887BC51BB48D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 742a10ce0042d2bbf6d5690cb7a7a6eca529a57e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="hello-wear"></a>Merhaba, yıpranması

_Android takmak ilk uygulamanızı oluşturma ve yıpranması öykünücü veya cihaz üzerinde çalıştırın. Bu kılavuzda düğme tıklamaları işler ve yıpranması aygıtta tıklatın sayaç görüntüleyen bir küçük Android takmak projesi oluşturmak için adım adım yönergeler sağlar. Bu uygulamayı yıpranması öykünücü veya bir Android telefonu Bluetooth bağlanan yıpranması aygıtı kullanarak hata ayıklama açıklanmaktadır. Ayrıca, Android takmak için bir dizi hata ayıklama ipuçları da sağlar._

![Bu öğreticinin tamamlanması uygulamasının ekran görüntüsü yıpranması](hello-wear-images/example.png)

## <a name="your-first-wear-app"></a>İlk yıpranması uygulamanızı

İlk Xamarin.Android yıpranması uygulamanızı oluşturmak için aşağıdaki adımları izleyin:

### <a name="1-create-a-new-android-project"></a>1. Yeni bir Android projesi oluşturma

Yeni bir **Android takmak uygulama**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Yeni Proje iletişim kutusuna takmak yeni Android uygulaması oluşturma](hello-wear-images/vs/new-solution-sml.png)](hello-wear-images/vs/new-solution.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Yeni bir çözüm iletişim kutusunda takmak yeni Android uygulaması oluşturma](hello-wear-images/xs/new-solution-sml.png)](hello-wear-images/xs/new-solution.png#lightbox)

-----


Bu şablon otomatik olarak içeren **Xamarin Android Wearable Kitaplığı** NuGet (ve bağımlılıklarını) şekilde yıpranması özgü pencere öğeleri erişiminiz olması. Yıpranması şablonunu görmüyorsanız gözden [yükleme ve Kurulum](~/android/wear/get-started/installation.md) kılavuz desteklenen Android SDK yüklü bir kez daha denetleyin. 

### <a name="2-choose-the-correct-target-framework"></a>2. Doğru seçin **hedef çerçevesi**

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Emin **Minimum Android hedef** ayarlanır **Android 5.0 (Lolipop)** ya da daha sonra: 

[![Visual Studio'da Android 5.0 hedef Framework'ü ayarlama](hello-wear-images/vs/target-framework-sml.png)](hello-wear-images/vs/target-framework.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Hedef Framework'ü ayarlandığından emin olun **Android 5.0 (Lolipop)** ya da daha sonra:

[![Mac için Visual Studio Android 5.0 hedef Framework'ü ayarlama](hello-wear-images/xs/target-framework-sml.png)](hello-wear-images/xs/target-framework.png#lightbox)

-----

Hedef Framework'ü ayarlama hakkında daha fazla bilgi için bkz: [anlama Android API düzeylerini](~/android/app-fundamentals/android-api-levels.md).


### <a name="3-edit-the-mainaxml-layout"></a>3. Düzen **Main.axml** düzeni

Düzen içerecek şekilde yapılandırmak bir `TextView` ve `Button` örnek: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
  <ScrollView
    android:id="@+id/scroll"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:background="#000000"
    android:fillViewport="true">
    <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:text="Main Activity"
        android:textSize="36sp"
        android:textColor="#006600" />
      <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="2dp"
        android:textColor="#cccccc"
        android:id="@+id/result" />
      <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="showNotification"
        android:text="Click Me!"
        android:id="@+id/click_button" />
    </LinearLayout>
  </ScrollView>
</FrameLayout>
```

### <a name="4-edit-the-mainactivitycs-source"></a>4. Düzen **MainActivity.cs** kaynağı

Bir sayaç artırmak ve her düğme tıklatıldığında görüntülemek için kodu ekleyin: 

```csharp
[Activity (Label = "WearTest", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
  int count = 1;

  protected override void OnCreate (Bundle bundle)
  {
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    Button button = FindViewById<Button> (Resource.Id.click_button);
    TextView text = FindViewById<TextView> (Resource.Id.result);

    button.Click += delegate {
      text.Text = string.Format ("{0} clicks!", count++);
    };
  }
}
```

### <a name="5-setup-an-emulator-or-device"></a>5. Bir öykünücü veya cihaz Kurulumu

Sonraki adım, dağıtmak ve uygulamayı çalıştırmak için bir öykünücü ve aygıtı ayarlanır. Yerel Yönetici değilseniz henüz dağıtma ve çalıştırma işleminde aşina Xamarin.Android uygulamaları genel olarak, bkz: [Hello, Android Hızlı Başlangıç](~/android/get-started/hello-android/hello-android-quickstart.md).

Android takmak Smartwatch gibi bir Android takmak cihaz yoksa bir öykünücü üzerinde uygulamayı çalıştırabilirsiniz. Bir öykünücü yıpranması uygulamalarında hata ayıklama hakkında daha fazla bilgi için bkz: [bir öykünücü üzerinde Android takmak hata ayıklama](~/android/wear/deploy-test/debug-on-emulator.md).

Android takmak Smartwatch gibi bir Android takmak cihazınız varsa, bir öykünücü kullanmak yerine cihazda uygulamayı çalıştırın. Yıpranması aygıtta hata ayıklama hakkında daha fazla bilgi için bkz: [takmak bir aygıtta hata ayıklama](~/android/wear/deploy-test/debug-on-device.md).


### <a name="6-run-the-android-wear-app"></a>6. Android yıpranması uygulamayı çalıştırma

Android takmak aygıt aygıt menülerle görüntülenmesi gerekir. Hata ayıklama başlamadan önce doğru Android takmak aygıt ya da AVD seçtiğinizden emin olun. Cihaz seçtikten sonra öykünücü veya aygıtı uygulamayı dağıtmak için Yürüt düğmesini tıklatın.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Visual Studio aygıt menüde takmak AVD seçme](hello-wear-images/vs/choose-wear-sim.png)](hello-wear-images/vs/choose-wear-sim.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Mac cihaz menü için Visual Studio'da takmak AVD seçme](hello-wear-images/xs/choose-wear-sim.png)](hello-wear-images/xs/choose-wear-sim.png#lightbox)

-----

Görebileceğiniz bir **yalnızca bir dakika...**  ilk ileti (veya başka bir Interstitial ekran): 

![Yalnızca bir dakika öykünücüsü görüntüler izleme...](hello-wear-images/please-wait.png)

Bir izleme öykünücüsü kullanıyorsanız, uygulamasını başlatmak için biraz zaman alabilir. Bluetooth kullanırken, USB üzerinden düğmelerden uygulama dağıtmak için daha uzun sürer. (Örneğin, yaklaşık 5 dakika için Nexus 5 telefon Bluetooth bağlı bir LG G izleme bu uygulamayı dağıtmak için sürer.)

Uygulama başarıyla dağıtıldıktan sonra yıpranması aygıt ekran aşağıdakine benzer bir ekran görüntülenir:

[![Yıpranması uygulamasının başlangıç ekranı](hello-wear-images/mainactivity-screen.png)](hello-wear-images/mainactivity-screen.png#lightbox)

Dokunun **BANA tıklayın!** düğmesi yıpranması cihaz ve bakın her dokunun ile sayısı artışı değişmiştir:

[![3 tıklama sonra ekran takmak uygulama](hello-wear-images/mainactivity-counts.png)](hello-wear-images/mainactivity-counts.png#lightbox)


## <a name="next-steps"></a>Sonraki Adımlar

Kullanıma [takmak örnekleri](https://developer.xamarin.com/samples/android/Android%20Wear/) Yardımcısı Phone uygulamalarını Android takmak uygulamalarla dahil olmak üzere.

Uygulamanızı dağıtmak hazır olduğunuzda bkz [paketle birlikte çalışma](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Bana uygulamayı (örnek) seçeneğine tıklayın](https://developer.xamarin.com/samples/monodroid/wear/WearTest/)
