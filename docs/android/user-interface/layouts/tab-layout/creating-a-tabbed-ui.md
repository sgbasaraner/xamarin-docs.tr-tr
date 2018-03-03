---
title: "İzlenecek yol - TabHost ile sekmeli kullanıcı Arabirimi oluşturma"
description: "Bu makalede TabHost API'yi kullanarak Xamarin.Android içinde sekmeli UI oluşturmada size yol gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 99a35705c408d16f5b4b0e71e53dd453ae377341
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>İzlenecek yol - TabHost ile sekmeli kullanıcı Arabirimi oluşturma

_Bu makalede TabHost API'yi kullanarak Xamarin.Android içinde sekmeli UI oluşturmada size yol gösterir._

> [!NOTE]
> **Not:** `TabHost` Google tarafından onaylanmaz eski bir API'dir. Geliştiriciler kullanarak sekmeli uygulamaları oluşturmak için kullanmaları [ActionBar](~/android/user-interface/controls/action-bar.md). `ActionBar` Tüm Android sürümünde kullanılabilir. Android 3.0 (API düzeyi 11) ilk sunulmuştur ve Android 2.2 (API düzeyi 8) ve Android 2.3 (API düzey 10) içinde geri alındığını [V7 uygulama Kitaplığı](http://developer.android.com/tools/support-library/features.html#v7-appcompat), Xamarin.Android kullanılabilir olduğu [Xamarin Android desteği Library - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) paket.

Bu makalede kullanarak Xamarin.Android içinde sekmeli UI oluşturmada size yol gösterir `TabHost` API. Android tüm sürümlerinde kullanılabilir daha eski bir API'dir. Bu örnek, bir etkinlikte kapsüllenmiş her sekme mantığı ile üç sekme ile bir uygulama oluşturacaksınız.
Aşağıdaki ekran görüntüsünde, oluşturacağız uygulama örneğidir:

![Birden fazla sekme uygulamayla örnek ekran görüntüsü](creating-a-tabbed-ui-images/image02.png)

<a name="Creating_the_Application" />

## <a name="creating-the-application"></a>Uygulama oluşturma

İndirip sıkıştırmasını [TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/).
Bu proje uygulamamız için başlangıç noktası olarak hizmet verir ve bazı görüntüleri içerir. Bu proje incelerseniz zaten sekmesini simgeler drawable kaynaklarını oluşturduk olduğunu görürsünüz.

İlk şimdi düzeni dosyasını güncelleştirme **Resources/Layout/Main.axml** sekmeleri gerçekleştirecektir. Düzen **Resources/Layout/Main.axml** ve aşağıdaki XML Ekle:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

Aşağıdaki ekran görüntüsünde Xamarin Tasarımcısı'nda düzeni gösterilir:

[![Xamarin Tasarımcısı'nda TabHost düzeninin ekran görüntüsü](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png)

TabHost içindeki iki alt görünüm olması gerekir: bir `TabWidget` ve `FrameLayout`. Konuma `TabWidget` ve `FrameLayout` dikey iç `TabHost`, `LinearLayout` kullanılır. İçeriğin nereye her sekmesi, boş olduğu gideceğini FrameLayout değil çünkü `TabHost` her etkinliğin çalışma zamanında otomatik olarak katıştırır. Sekmeli kullanıcı arabirimleri düzenini oluşturmak için geldiğinde incelenmelidir çeşitli kurallar şunlardır:

-   `TabHost` Bir kimliği olmalı `@android:id/tabhost`.

-   `TabWidget` Bir kimliği olmalı `@android:id/tabs`.

-   `FrameLayout` Bir kimliği olmalı `@android:id/tabcontent`.

-   `TabHost` devralınacak yönettiği herhangi bir etkinlik gerektirir `TabActivity`. Bu nedenle, bir alt kümesi için önemli olan `TabActivity` burada &ndash; normal etkinliğinden çalışmaz.

Dosyayı düzenlemek **MainActivity.cs** böylece sınıfı `MainActivity` alt sınıfların `TabActivity` aşağıdaki kod parçacığında gösterildiği gibi:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

Projenizde dört ayrı etkinlik sınıfları oluşturma: `MyScheduleActivity`, `SessionsActivity`, `SpeakersActivity`, ve `WhatsOnActivity`. Her etkinlik, bir sekme UI'yi hale getirir. Bu etkinlikler görüntüleyen bir saplama şimdi olacaktır bir `TextView` basit bir iletiyle. Aşağıdakileri içeren her bir etkinliği kodda Düzenle `OnCreate` uygulama:

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

Yukarıdaki kod bir düzen dosyasını kullanmayan dikkat edin. Yalnızca oluşturduğu bir `TextView` bazı metinle ve ayarlar `TextView` içerik görünümü olarak. Bu her kalan üç etkinlikler için yinelenen.

Sonraki biz her sekmeye simgeleri atar. Her sekme iki simgeler gerektirir &ndash; seçilen durumu, diğeri seçilmemiş durumu için için. Bu iki farklı simgeleri örneği (Bu uygulama için gerekli simgeler örnek projeye eklenmiş olan) aşağıdaki iki görüntü görülebilir:

![Seçili duruma ve seçili durumu simgeleri ekran görüntüsü](creating-a-tabbed-ui-images/tab-icons.png)


Biz tanımlayarak simgesi sekmelere drawable kaynakları atayacağınız bir *durumu listesi Drawable*. Durum listesi drawables XML içinde tanımlanır ve bu öğeye ait durumuna belirli farklı görüntüleri belirtmenizi sağlar özel bir drawable kaynaklardır. Bu örnekte bir sekmeye seçildikten sonra kullanılan bir görüntü ve sekmesi seçili olmadığında, kullanılan başka bir yoktur. Size zaman kazandırabilir gerekli durumu listesi drawables projeye eklendi. Aşağıdaki liste dosyaları gösterir ve her XML içeriyor:

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

Sekmeleri eklenir `TabHost` çok yinelenen bir görev programlı olarak olduğu. Bu konuda yardımcı olmak için sınıfına aşağıdaki yöntemi ekleyin `MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

Her bir sekmede `TabHost` bir örneği tarafından temsil edilen, `TabHost.TabSpec` sınıfı. Bu örnek meta verileri içeren sekmesinde, özel olarak işlemek gerekli:

-   **Metin ve simge** &ndash; görüntülenmesini `TabWidget`.

-   **Sekme içeriği** &ndash; bu ya da olabilir bir `Activity` veya `View` ve sekmesi seçili olduğunda görüntülenir.

-   **Benzersiz etiketi** &ndash; her sekme atanmış benzersiz bir etiketi olması gerekiyor.

Biz eklemelisiniz bir `TabHost.TabSpec` her bir sekmede uygulamamız için örneği. Sonraki adımda bunu sağlar.

Update yöntemi `OnCreate` içinde `MainActivity` böylece aşağıdaki kodu benzer:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

Uygulamayı çalıştırın. Uygulamanız bu kılavuzda, başında gösterilen ekran görüntüsüne benzer görünmelidir.

İşte bu kadar! Farklı bir uygulama bölümlerine kolay bir yol gidin kullanıcı sağlayan bir sekmeli uygulaması oluşturduk.


<a name="Summary" />

## <a name="summary"></a>Özet

Bu bölümde sekmeli düzenleri ele alınan ve sekmeli uygulaması oluşturma işleminde size kılavuzluk. İzlenecek yol nasıl kullanılacağını gösterilen bir `TabActivity` Şişir bir düzen dosyasını bu barındırma bir `TabHost` ve bir `TabWidget`. `TabHost` İçeren bir koleksiyonu sonra doldurulmuş olduğu `TabHost.TabSpec` tarafından kullanılacak nesneleri `TabHost` her bir sekmede kullanılacak etkinlikleri örneği oluşturmak için çalışma zamanında.



## <a name="related-links"></a>İlgili bağlantılar

- [TabHostWalkthrough (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android desteği kitaplığı v7 uygulama NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 uygulama kitaplığı](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
