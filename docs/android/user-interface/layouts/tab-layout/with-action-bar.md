---
title: "ActionBar sekmeli düzenleri"
description: "Bu kılavuz tanıtır ve ActionBar API'leri sekmeli kullanıcı arabirimi bir Xamarin.Android uygulaması oluşturmak için nasıl kullanılacağını açıklar."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 14abb7a4b85b493bb0ab96a982d989fad783fabd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="tabbed-layouts-with-the-actionbar"></a>ActionBar sekmeli düzenleri

_Bu kılavuz tanıtır ve ActionBar API'leri sekmeli kullanıcı arabirimi bir Xamarin.Android uygulaması oluşturmak için nasıl kullanılacağını açıklar._

<a name="Overview" />

## <a name="overview"></a>Genel Bakış

Eylem çubuk sekmeler, uygulama kimliği, menüleri ve arama gibi anahtar özellikleri için tutarlı bir kullanıcı arabirimi sağlamak için kullanılan bir Android UI düzeni olur. (API düzeyi 11) Android 3.0 sürümünde, Google Android platformu için ActionBar API'lerini kullanıma sunuldu. ActionBar API'ları tutarlı bir görünüm ve sekmeli kullanıcı arabirimleri için izin sınıfları sağlamak için kullanıcı Arabirimi Temalar tanıtır. Bu kılavuz, bir Xamarin.Android uygulaması eylem çubuğundaki sekmeler ekleme açıklanır. Ayrıca, Android destek kitaplığı v7 backport ActionBar sekmelere Android 2.3 için Android 2.1 hedefleme Xamarin.Android uygulamaları için kullanmak üzere nasıl anlatılmaktadır. 

Unutmayın `Toolbar` , yerine kullanması gereken bir daha yeni ve daha genelleştirilmiş Eylem çubuğu bileşeni `ActionBar` (`Toolbar` değiştirmek için tasarlanmış `ActionBar`). Daha fazla bilgi için bkz: [araç](~/android/user-interface/controls/tool-bar/index.md). 


<a name="Requirements" />

## <a name="requirements"></a>Gereksinimler

API düzeyi 11 (Android 3.0) hedefler ya da daha yüksek ActionBar API'leri yerel Android API'ları bir parçası olarak erişimi herhangi Xamarin.Android uygulaması. 

Bazı ActionBar API'leri 7 (Android 2.1) API düzeyine geri bağlantı noktalı ve aracılığıyla kullanılabilir [V7 uygulama Kitaplığı](http://developer.android.com/tools/support-library/features.html#v7-appcompat), hangi yoluyla Xamarin.Android uygulamaları için kullanılabilir hale [Xamarin Android destek kitaplığı - V7 ](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) paket.


<a name="Introducing_tabs_in_the_ActionBar" />

## <a name="introducing-tabs-in-the-actionbar"></a>ActionBar sekmeleri Tanıtımı

Eylem çubuğunda tüm sekmelerinin eşzamanlı olarak görüntülemek ve tüm sekmeleri geniş sekme etiketi genişliğini göre boyutu eşit olmak dener. Bu, aşağıdaki ekran görüntüsü tarafından gösterilmiştir: 

![Örnek penceresinin ekran görüntüsü ActionBar gösterilen tüm eşittir ölçekli sekmeleri](with-action-bar-images/image1.png)

ActionBar tüm sekmeleri görüntüleyemiyor, onu yatay olarak kaydırılabilir bir görünüm sekmeleri ayarlayacaksınız. Kullanıcı geri kalan sekmeleri görmek için sola veya sağa doğru çekin. Bu ekran görüntüsü Google play'den, bunun bir örneğini gösterir: 

![Yatay olarak kaydırılabilir görünümünde sekmelerinin örnek ekran görüntüsü](with-action-bar-images/image2.png)

Her sekme eylemi çubuğunda ilişkilendirileceği bir [ *parça*](~/android/platform/fragments/index.md). Kullanıcı bir sekme seçtiğinde, uygulama sekmesi ile ilişkili parça görüntüler. ActionBar uygun parça kullanıcıya görüntülemek için sorumlu değildir. Bunun yerine, ActionBar uygulamanın ActionBar.ITabListener arabirimini uygulayan bir sınıf sekmesinden durumu değişiklikleri hakkında uyarır. Bu arabirim sekmesini durumunu değiştirdiğinde, Android çağıracağı üç geri çağırma yöntemleri sağlar: 

-  **OnTabSelected** -kullanıcı sekmesini seçtiğinde bu yöntem çağrılır. Parça görüntülemelidir.

-  **OnTabReselected** -sekmesini zaten seçili ancak yeniden kullanıcı tarafından seçilen bu yöntem çağrılır. Bu geri çağırma genellikle görüntülenen parçasını yenileme ya da güncelleştirmek için kullanılır.

-  **OnTabUnselected** -kullanıcı başka bir sekmeye seçtiğinde bu yöntem çağrılır. Bu geri çağırma kaybolur önce görüntülenen parçadaki durumunu kaydetmek için kullanılır.

Xamarin.Android sarmalar `ActionBar.ITabListener` olaylarla `ActionBar.Tab` sınıfı. Uygulamalar, bir veya daha fazla bu olayları olay işleyicileri atayabilir. Üç olay vardır (her bir yöntemin için bir tane `ActionBar.ITabListener`), bir eylem çubuğunda sekme oluşturacak: 

-  TabSelected
-  TabReselected
-  TabUnselected


<a name="Adding_Tabs_to_the_ActionBar" />

### <a name="adding-tabs-to-the-actionbar"></a>ActionBar sekmeler ekleme

ActionBar Android 3.0 (API düzeyi 11) ve daha yüksek yerel ve en az olarak bu API hedefler herhangi bir Xamarin.Android uygulaması için kullanılabilir. 

Aşağıdaki adımları Android aktivite ActionBar sekmeler ekleme gösterilmektedir: 

1. İçinde `OnCreate` etkinliğin yöntemi &ndash; *tüm UI pencere öğeleri başlatma önce* &ndash; uygulama ayarlamalısınız `NavigationMode` üzerinde `ActionBar` için `ActionBar.NavigationModeTabs` bu kodda gösterildiği gibi kod parçacığında:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Kullanarak yeni bir sekme oluşturmak `ActionBar.NewTab()`.

3. Olay işleyicileri atayın veya özel bir sağlayın `ActionBar.ITabListener` uygulamasında, kullanıcı ActionBar sekmelerle etkileşim kurduğunda başlatılan olaylara yanıt verir.

4. Önceki adımda oluşturduğunuz sekme ekleme `ActionBar`.


Aşağıdaki kod sekmeleri durumu değişikliklerine yanıt vermelerini olay işleyicileri kullanan bir uygulama eklemek için aşağıdaki adımları kullanarak, bir örnek verilmiştir: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       }
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       }
    ActionBar.AddTab(tab);
}
```

<a name="Event_Handlers_vs_ActionBar.ITabListener" />

#### <a name="event-handlers-vs-actionbaritablistener"></a>Olay işleyicileri vs ActionBar.ITabListener

Uygulamalar, olay işleyicileri kullanmalıdır ve `ActionBar.ITabListener` farklı senaryolar için. Olay işleyicileri söz dizimi kolaylık belirli bir miktar sunar; Bunlar, bir sınıf oluşturun ve uygulama zorunluluğunu `ActionBar.ITabListener`. Bu kullanışlı bir ücret geliyor &ndash; Xamarin.Android gerçekleştirir bu dönüşüm, sizin için bir sınıf oluşturma ve uygulama `ActionBar.ITabListener` sizin için. Sekmeleri sınırlı sayıda uygulama sahip olduğunda bu sorun yoktur. 

Birçok sekmelerle ilgilenirken veya ActionBar sekmeler arasında ortak işlevselliği paylaşımı, bellek ve uygulayan özel bir sınıf oluşturmak için performans açısından daha verimli `ActionBar.ITabListener`ve sınıfın tek bir örnek paylaşımı. Bu, bir Xamarin.Android uygulaması kullanarak GREF'ın sayısını azaltır. 


<a name="Backwards_Compatibility_for_Older_Devices" />

### <a name="backwards-compatibility-for-older-devices"></a>Geriye dönük uyumluluk için eski aygıtları

[Android destek kitaplığı v7 uygulama](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) arka noktaları ActionBar sekmelere Android 2.1 (API düzeyi 7). Bu bileşen projeye eklendiğinde sekmeleri bir Xamarin.Android uygulaması'nda erişilebilir.

ActionBar kullanmak için bir alt kümesi bir etkinlik gerekir `ActionBarActivity` ve uygulama tema aşağıdaki kod parçacığında gösterildiği gibi kullanın:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

Bir etkinlik kendi ActionBar başvuru edinebilirsiniz `ActionBarActivity.SupportingActionBar` özelliği. Aşağıdaki kod parçacığını bir etkinlikte ActionBar ayarlama örneği gösterilmektedir:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```

<a name="Summary" />

## <a name="summary"></a>Özet

Bu kılavuzda ActionBar kullanarak bir Xamarin.Android sekmeli kullanıcı arabirimi oluşturmak nasıl ele. Biz ActionBar sekmeler ekleme ve bir etkinlik sekmesini olaylarla nasıl etkileşim kurabilen kapsamdaki `ActionBar.ITabListener` arabirimi. Android destek kitaplığı v7 uygulama paketi backports ActionBar Android eski sürümleri nasıl sekmeler gördük. 


## <a name="related-links"></a>İlgili bağlantılar

- [ActionBarTabs (örnek)](https://developer.xamarin.com/samples/monodroid/UserInterface/ActionBarTabs/)
- [Araç çubuğu](~/android/user-interface/controls/tool-bar/index.md)
- [Parçaları](~/android/platform/fragments/index.md)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](http://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)
- [Eylem çubuğu düzeni](http://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin.Android destek kitaplığı v7 uygulama NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
