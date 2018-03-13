---
title: "Android derleme bildirimi ile çalışma"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: aa2d2ce6cabe9c394b9807ca3d6328da5b4ba311
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-the-android-manifest"></a>Android derleme bildirimi ile çalışma


## <a name="overview"></a>Genel Bakış

**AndroidManifest.xml** işlevsellik ve Android uygulamanıza gereksinimlerini tanımlamak izin veren bir Android platformda güçlü bir dosyadır. Ancak, çalışmaya kolay değildir. Xamarin.Android bildirim sizin için otomatik olarak oluşturmak için kullanılacak sınıflarınızı özel öznitelikler eklemenize olanak sağlayarak bu zorluk en aza indirmenize yardımcı olur. Amacımız kullanıcılarımızın % 99 hiçbir zaman el ile değiştirmeniz olmanızdır **AndroidManifest.xml**. 

**AndroidManifest.xml** yapı işlemi ve içinde bulunan XML parçası olarak oluşturulan **Properties/AndroidManifest.xml** özel öznitelikleri oluşturulmuş XML ile birleştirilir. Elde edilen birleştirilmiş **AndroidManifest.xml** bulunan **obj** alt; Örneğin, en bulunuyorsa **obj/Debug/android/AndroidManifest.xml** hata ayıklama derlemeleri . Birleştirme işlemi Önemsiz: XML öğelerini oluşturmak için özel öznitelikler kodundaki kullanır ve *ekler* bu elemanlara **AndroidManifest.xml**. 



## <a name="the-basics"></a>Temeller

Derleme zamanında derlemeleri için taranır olmayan`abstract` öğesinden türetilen sınıflar [etkinlik](https://developer.xamarin.com/api/type/Android.App.Activity/) ve [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) özniteliği üzerlerinde bildirilmedi. Ardından bu sınıfları ve öznitelikleri bildirimi oluşturmak için kullanır. Örneğin, aşağıdaki kodu göz önünde bulundurun: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

Bu, oluşturulan hiçbir şey sonuçlanır **AndroidManifest.xml**. İsterseniz bir `<activity/>` öğesinin oluşturulması için kullanmanız gereken [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) özel öznitelik: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

Bu örnek için eklemek aşağıdaki xml parçası neden **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` Özniteliği hiçbir etkisi vardır `abstract` türleri; `abstract` türleri yok sayılır.



### <a name="activity-name"></a>Etkinlik Adı

Xamarin.Android 5.1 ile başlayarak, bir etkinlik türünün adı verilen tür bütünleştirilmiş kod tam adının MD5SUM üzerinde temel alır. Bu, iki farklı derlemelerden sağlanması ve bir paketleme hatası almamış aynı tam ada izin verir. (Xamarin.Android 5.1 önce etkinliğin varsayılan türü adını dönüştürüldükten ad alanı ve sınıf adı oluşturuldu.) 

Bu varsayılanı geçersiz kılabilir ve kullanım etkinliklerinizi adını açıkça belirtmek isterseniz, [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) özelliği: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

Bu örnekte aşağıdaki xml parçası üretir:

```xml
<activity android:name="awesome.demo.activity" />
```

*Not*: kullanması gereken `Name` özelliği yalnızca şekilde yeniden adlandırma geriye dönük uyumluluk nedenleriyle çalışma zamanında tür arama yavaşlaması. Bkz: dönüştürüldükten ad temel etkinliğin varsayılan türü adını ve sınıf adı bekliyor eski kodunuz varsa [Android aranabilir sarmalayıcısı adlandırma](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) uyumluluk koruma ile ilgili ipuçları için. 


### <a name="activity-title-bar"></a>Etkinlik başlık çubuğu

Çalışırken varsayılan olarak, bir başlık çubuğu Android uygulamanızı sağlar. Bunun için kullanılan değer [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label). Çoğu durumda, bu değer sınıfı adından farklılık gösterir. Başlık çubuğunda, uygulamanızın etiket belirtmek için kullanın [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) özelliği.
Örneğin: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

Bu örnekte aşağıdaki xml parçası üretir:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Uygulama Seçici gelen launchable

Varsayılan olarak, etkinliklerinizi Android'ın uygulama Başlatıcısı ekranında gösterilmez. Bu büyük olasılıkla olacaktır birçok etkinlik uygulamanızda ve her biri için bir simge istemediğiniz kaynaklanır. Hangisinin uygulama Başlatıcısı'ndan launchable belirtmek için kullanın [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) özelliği. Örneğin: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

Bu örnekte aşağıdaki xml parçası üretir:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```



### <a name="activity-icon"></a>Etkinlik simgesi

Varsayılan olarak, sistem tarafından sağlanan varsayılan Başlatıcı simgesini etkinliklerinizi verilir. Özel bir simge kullanmak için önce ekleyin, **.png** için **kaynakları/drawable**, yapı eylemi kümesine **AndroidResource**, ardından [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) özelliğini kullanmak için simge belirtin. Örneğin: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

Bu örnekte aşağıdaki xml parçası üretir:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```


### <a name="permissions"></a>İzinler

Android derleme bildirimi için izinleri eklediğinizde (açıklandığı gibi [ekleme izni Android derleme bildirimi](https://developer.xamarin.com/recipes/android/general/projects/add_permissions_to_android_manifest/)), bu izinleri kaydedilir **Properties/AndroidManifest.xml**. Örneğin, ayarlarsanız `INTERNET` izni, aşağıdaki öğeyi eklenen **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Hata ayıklama derlemelerinde, daha kolay hata ayıklama yapmak için bazı izinler otomatik olarak ayarlanır (gibi `INTERNET` ve `READ_EXTERNAL_STORAGE`) &ndash; bu ayarlar yalnızca içinde oluşturulan **obj/Debug/android/AndroidManifest.xml** ve değil Etkin olarak gösterilen **gerekli izinleri** ayarlar. 

Örneğin, oluşturulan bildirim dosyasını incelerseniz **obj/Debug/android/AndroidManifest.xml**, aşağıdaki eklenen izin öğelerini görebilirsiniz: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Sürümde bildiriminin sürümü yapı (adresindeki **obj/Debug/android/AndroidManifest.xml**), bu izinler *değil* otomatik olarak yapılandırılmış. Yayın derlemesine geçme uygulamanızı hata ayıklama derlemesi kullanılabilir bir izin kaybetmenize neden olduğunu fark ederseniz, açıkça bu izni ayarladığınızdan doğrulayın **gerekli izinleri** (bkz: uygulamanıziçinayarları **Derleme > Android uygulaması** for Mac; Visual Studio'da görmek **Özellikler > Android derleme bildirimi** Visual Studio). 




## <a name="advanced-features"></a>Gelişmiş Özellikler


### <a name="intent-actions-and-features"></a>Hedefi Eylemler ve Özellikler

Android derleme bildirimi etkinlik özelliklerini tanımlamak bir yol sağlar. Bu aracılığıyla yapılır [hedefleri](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) ve [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) özel öznitelik. Hangi eylemler ile etkinliklerinizi için uygun olan belirtebilirsiniz [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) oluşturucu ve kategorilerini uygun [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) özelliği. En az bir etkinlik (etkinlikleri oluşturucuda sağlanan neden olan) sağlanmalıdır. `[IntentFilter]` birden çok kez ve her kullanım ayrı bir sonuçları girilebilir `<intent-filter/>` öğesi içinde `<activity/>`. Örneğin:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

Bu örnekte aşağıdaki xml parçası üretir:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```


### <a name="application-element"></a>Uygulama öğesi

Android derleme bildirimi ayrıca tüm uygulamanız için özellikler bildirmek bir yol sağlar. Bu aracılığıyla yapılır `<application>` öğesi ve kendisine karşılık gelen [uygulama](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) özel öznitelik. Bu etkinlik başına ayarlarını yerine uygulama genelinde (derleme genelinde) ayarları olduğunu unutmayın. Genellikle, bildirdiğiniz `<application>` tüm uygulamanız için özellikler ve daha sonra bu ayarları bir etkinlik başına temelinde (gerektiğinde) geçersiz. 

Örneğin, aşağıdaki `Application` özniteliği eklenir **AssemblyInfo.cs** uygulama hata ayıklaması olabilir, kullanıcı tarafından okunabilen adını olduğunu belirtmek için **Uygulamam**, ve kullanır`Theme.Light` stili tüm etkinlikler için varsayılan tema olarak: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Bu bildirim içinde oluşturulacak aşağıdaki XML parçası neden **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
Bu örnekte, uygulamadaki tüm etkinlikler için varsayılan `Theme.Light` stili. Bir etkinliğin tema ayarlarsanız `Theme.Dialog`, yalnızca, etkinlik kullanacağı `Theme.Dialog` uygulamanızda tüm diğer etkinlikler varsayılan sırada stil `Theme.Light` stili kümesinde olarak `<application>` öğesi. 

`Application` Öğesi yapılandırmak için tek yolu değil `<application>` öznitelikleri. Alternatif olarak, doğrudan öznitelikler ekleyebilirsiniz `<application>` öğesinin **Properties/AndroidManifest.xml**. Bu ayarlar son birleştirilir `<application>` bulunan öğe **obj/Debug/android/AndroidManifest.xml**. Unutmayın, içeriğini **Properties/AndroidManifest.xml** her zaman özel öznitelikleri tarafından sağlanan verileri geçersiz kılar. 

Yapılandırabileceğiniz birçok uygulama çapında öznitelik `<application>` öğesi; bu ayarları hakkında daha fazla bilgi için bkz: [ortak özellikler](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) bölümünü [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Özel öznitelikler listesi

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : oluşturan bir [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) XML parçası 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : oluşturan bir [/bildirimi/uygulama](http://developer.android.com/guide/topics/manifest/application-element.html) XML parçası 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : oluşturan bir [/bildirimi/Araçları](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML parçası 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : oluşturan bir [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML parçası 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : oluşturan bir [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) XML parçası 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : oluşturan bir [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) XML parçası 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : Generates a  [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) XML fragment 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : Generates a  [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML fragment 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : oluşturan bir [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) XML parçası 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : Generates a  [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) XML fragment 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : oluşturan bir [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML parçası 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : Generates a  [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) XML fragment 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : Generates a  [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) XML fragment 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : Generates a  [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML fragment

