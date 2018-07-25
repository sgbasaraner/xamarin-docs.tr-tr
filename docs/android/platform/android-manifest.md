---
title: Android bildirimi ile çalışma
ms.prod: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: 0857b70e6e1d9104f62ec2e26f8edbab385d06f3
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242257"
---
# <a name="working-with-the-android-manifest"></a>Android bildirimi ile çalışma


## <a name="overview"></a>Genel Bakış

**AndroidManifest.xml** işlevleri ve Android için uygulamanızın gereksinimlerini tanımlamak olanak sağlayan Android platformunda güçlü bir dosyadır. Ancak, çalışmaya kolay değildir. Xamarin.Android bildirimini sizin için otomatik olarak oluşturmak için kullanılacak sınıflarınızı özel öznitelikler eklemenizi sağlayarak bu zorluk en aza indirmek için yardımcı olur. Hedefimiz kullanıcılarımızın 99 oranında hiçbir zaman el ile değiştirmeniz olmanızdır **AndroidManifest.xml**. 

**AndroidManifest.xml** derleme işlemi ve içinde bulunan XML parçası olarak oluşturulan **Properties/AndroidManifest.xml** özel öznitelikleri oluşturulan XML ile birleştirilir. Ortaya çıkan birleştirilmiş **AndroidManifest.xml** bulunan **obj** alt; Örneğin, konumunda bulunan **obj/Debug/android/AndroidManifest.xml** hata ayıklama yapıları için . Birleştirme işlemi Önemsiz: XML öğeleri oluşturmak için özel öznitelikler kodundaki kullanır ve *ekler* bu öğeleri eklemek **AndroidManifest.xml**. 



## <a name="the-basics"></a>Temeller

Derleme zamanında derlemeleri için taranan olmayan`abstract` öğesinden türetilen sınıfları [etkinlik](https://developer.xamarin.com/api/type/Android.App.Activity/) ve [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) öznitelik bunlar üzerinde bildirilen. Ardından bu sınıflar ve öznitelikler bildirimi oluşturmak için kullanır. Örneğin, aşağıdaki kodu düşünün: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

İçinde oluşturulan hiçbir şey sonuçlanır **AndroidManifest.xml**. İsterseniz bir `<activity/>` öğe oluşturulacak, kullanmanız gereken [ `[Activity]` ](https://developer.xamarin.com/api/type/Android.App.Activity/Attribute) özel öznitelik: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

Bu örnek eklenmesi aşağıdaki xml parçası neden **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

`[Activity]` Özniteliğine etkiye sahip `abstract` türleri; `abstract` türleri yok sayılır.



### <a name="activity-name"></a>Etkinlik Adı

Xamarin.Android 5.1 ile başlayarak, bir etkinlik türü adı verilen tür bütünleştirilmiş kodla nitelenen adını MD5SUM üzerinde temel alır. Bu, iki farklı derlemelerden sağlanması ve bir paketleme hata değil almak aynı olan tam adı sağlar. (Xamarin.Android 5.1 önce etkinliğin varsayılan türü adını dönüştürüldükten ad alanı ve sınıf adı oluşturuldu.) 

Bu varsayılanı geçersiz kılmak ve kullanmak, etkinliğin adı açıkça belirtmek istiyorsanız [ `Name` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Name/) özelliği: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

Bu örnek aşağıdaki xml parçası üretir:

```xml
<activity android:name="awesome.demo.activity" />
```

*Not*: kullanmalısınız `Name` özelliği yalnızca bu nedenle yeniden adlandırma geriye dönük uyumluluk nedenleriyle çalışma zamanında tür arama yavaşlamasına. Küçük harfli ad alanını temel alan etkinliğin varsayılan türü adını ve sınıf adı, eski kod varsa [Android çağrılabilir sarmalayıcı adlandırma](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) uyumluluk ipuçları için. 


### <a name="activity-title-bar"></a>Etkinlik başlık çubuğu

Çalıştırıldığında bir başlık çubuğu varsayılan olarak, Android uygulamanızı sağlar. Bunun için kullanılan değer [ `/manifest/application/activity/@android:label` ](http://developer.android.com/guide/topics/manifest/activity-element.html#label). Çoğu durumda, bu değer, sınıf adından farklı olacaktır. Başlık çubuğunda uygulamanızın etiket belirtmek için kullanın [ `Label` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Label/) özelliği.
Örneğin: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

Bu örnek aşağıdaki xml parçası üretir:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```


### <a name="launchable-from-application-chooser"></a>Uygulama Seçici başlatılabilir

Varsayılan olarak, etkinlik Android uygulama Başlatıcı ekranındaki gösterilmez. Büyük olasılıkla olacaktır birçok etkinlik uygulamanızda ve her biri için bir simge istemiyorsanız nedeni budur. Hangi uygulama başlatıcıdan başlatılabilir belirtmek için kullanın [ `MainLauncher` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.MainLauncher/) özelliği. Örneğin: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

Bu örnek aşağıdaki xml parçası üretir:

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

Varsayılan olarak, sistem tarafından sağlanan varsayılan Başlatıcısı simgesini etkinlik verilir. Özel bir simge kullanmak için önce ekleyin, **.png** için **kaynakları/drawable**, kendi derleme eylemi kümesine **AndroidResource**, ardından [ `Icon` ](https://developer.xamarin.com/api/property/Android.App.ActivityAttribute.Icon/) özelliğini kullanmak için simge belirtin. Örneğin: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

Bu örnek aşağıdaki xml parçası üretir:

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

Android bildirim için izinler eklediğinizde (açıklandığı [Android bildirim için izinler ekleme](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)), bu izinleri kaydedilir **Properties/AndroidManifest.xml**. Örneğin, ayarlarsanız `INTERNET` izni şu öğe eklendiğinde **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Hata ayıklama yapıları otomatik olarak daha kolay hata ayıklama yapmak için bazı izinleri ayarla (gibi `INTERNET` ve `READ_EXTERNAL_STORAGE`) &ndash; bu ayarlar yalnızca içinde oluşturulan **obj/Debug/android/AndroidManifest.xml** ve değil Etkin olarak gösterilen **gerekli izinler** ayarları. 

Örneğin, oluşturulan bildirim dosyasını incelerseniz **obj/Debug/android/AndroidManifest.xml**, aşağıdaki eklenen izni öğelerini görebilirsiniz: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

Yayın derleme bildiriminin sürümü (en **obj/Debug/android/AndroidManifest.xml**), bu izinler *değil* otomatik olarak yapılandırılmış. Bir yayın yapısı için geçiş uygulamanızı hata ayıklama derlemesinde kullanılabilir olan bir izin kaybetmenize neden olduğunu fark ederseniz, açıkça bu izni ayarladığınızdan doğrulayın **gerekli izinler** uygulamanızın (bkz: ayarları **Derleme > Android uygulaması** ; Mac için Visual Studio'da görmek **özellikleri > Android bildirim** Visual Studio'daki). 




## <a name="advanced-features"></a>Gelişmiş Özellikler


### <a name="intent-actions-and-features"></a>Hedefi Eylemler ve özellikleri

Android bildirimi etkinlik özelliklerini tanımlamak bir yol sağlar. Bu aracılığıyla yapılır [hedefleri](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) ve [ `[IntentFilter]` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) özel öznitelik. Hangi eylemler ile etkinlik için uygun olan belirtebilirsiniz [ `IntentFilter` ](https://developer.xamarin.com/api/constructor/Android.App.IntentFilterAttribute.IntentFilterAttribute/p/System.String[]/) oluşturucusu ve kategorilerini uygun [ `Categories` ](https://developer.xamarin.com/api/property/Android.App.IntentFilterAttribute.Categories/) özelliği. En az bir etkinlik (etkinlikleri oluşturucuda sağlanan neden olan) sağlanmalıdır. `[IntentFilter]` birden çok kez ve her kullanım ayrı bir sonuçları girilebilir `<intent-filter/>` öğesiyle `<activity/>`. Örneğin:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

Bu örnek aşağıdaki xml parçası üretir:

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

Android bildirimi ayrıca tüm uygulamanızın özelliklerini bildirmek bir yol sağlar. Bu aracılığıyla yapılır `<application>` öğesi ve kendisine karşılık gelen [uygulama](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) özel öznitelik. Bu etkinlik başına ayarlarını daha birçok farklı uygulama (derleme) ayarları olduğunu unutmayın. Genellikle, bildirdiğiniz `<application>` tüm uygulamanızın özelliklerini ve daha sonra bu ayarları bir etkinlik başına temelinde (gereken şekilde) geçersiz. 

Örneğin, aşağıdaki `Application` özniteliği eklenir **AssemblyInfo.cs** uygulamasında hata ayıklaması yapılmış olabilir, kullanıcı tarafından okunabilen adı olduğunu belirtmek için **Uygulamam**, ve kullanır`Theme.Light` tüm etkinlikler için varsayılan tema olarak stili: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

Bu bildirim oluşturulacağını belirtmek aşağıdaki XML parçası neden **obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```
Bu örnekte, uygulamadaki tüm etkinlikler için varsayılan `Theme.Light` stili. Bir etkinliğin tema ayarlamanız `Theme.Dialog`, yalnızca, etkinlik kullanacağı `Theme.Dialog` uygulamanızdaki diğer tüm etkinlikleri varsayılan sırada stil `Theme.Light` kümesinde stilinde `<application>` öğesi. 

`Application` Öğesi yapılandırmak için tek yolu değil `<application>` öznitelikleri. Alternatif olarak, doğrudan öznitelikler ekleyebilirsiniz `<application>` öğesinin **Properties/AndroidManifest.xml**. Bu ayarlar son birleştirilir `<application>` bulunan öğe **obj/Debug/android/AndroidManifest.xml**. Unutmayın, içeriğini **Properties/AndroidManifest.xml** her zaman özel öznitelikleri tarafından sağlanan verileri geçersiz kılar. 

Yapılandırabileceğiniz birçok uygulama çapında öznitelik `<application>` öğesi; bu ayarlar hakkında daha fazla bilgi için bkz. [ortak özellikler](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/#Public_Properties) bölümünü [ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/). 



## <a name="list-of-custom-attributes"></a>Özel öznitelikler listesi

-   [Android.App.ActivityAttribute](https://developer.xamarin.com/api/type/Android.App.ActivityAttribute/) : oluşturur bir [/manifest/application/activity](http://developer.android.com/guide/topics/manifest/activity-element.html) XML parçası 
-   [Android.App.ApplicationAttribute](https://developer.xamarin.com/api/type/Android.App.ApplicationAttribute/) : oluşturur bir [/bildirimi/uygulama](http://developer.android.com/guide/topics/manifest/application-element.html) XML parçası 
-   [Android.App.InstrumentationAttribute](https://developer.xamarin.com/api/type/Android.App.InstrumentationAttribute/) : oluşturur bir [/bildirimi/izleme](http://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML parçası 
-   [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/) : oluşturur bir [//intent-filter](http://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML parçası 
-   [Android.App.MetaDataAttribute](https://developer.xamarin.com/api/type/Android.App.MetaDataAttribute/) : oluşturur bir [//meta-data](http://developer.android.com/guide/topics/manifest/meta-data-element.html) XML parçası 
-   [Android.App.PermissionAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionAttribute/) : oluşturur bir [//permission](http://developer.android.com/guide/topics/manifest/permission-element.html) XML parçası 
-   [Android.App.PermissionGroupAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionGroupAttribute/) : oluşturur bir [//permission-group](http://developer.android.com/guide/topics/manifest/permission-group-element.html) XML parçası 
-   [Android.App.PermissionTreeAttribute](https://developer.xamarin.com/api/type/Android.App.PermissionTreeAttribute/) : oluşturur bir [//permission-tree](http://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML parçası 
-   [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/) : oluşturur bir [/manifest/application/service](http://developer.android.com/guide/topics/manifest/service-element.html) XML parçası 
-   [Android.App.UsesLibraryAttribute](https://developer.xamarin.com/api/type/Android.App.UsesLibraryAttribute/) : oluşturur bir [/manifest/application/uses-library](http://developer.android.com/guide/topics/manifest/uses-library-element.html) XML parçası 
-   [Android.App.UsesPermissionAttribute](https://developer.xamarin.com/api/type/Android.App.UsesPermissionAttribute/) : oluşturur bir [/manifest/uses-permission](http://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML parçası 
-   [Android.Content.BroadcastReceiverAttribute](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiverAttribute/) : oluşturur bir [/manifest/application/receiver](http://developer.android.com/guide/topics/manifest/receiver-element.html) XML parçası 
-   [Android.Content.ContentProviderAttribute](https://developer.xamarin.com/api/type/Android.Content.ContentProviderAttribute/) : oluşturur bir [/manifest/application/provider](http://developer.android.com/guide/topics/manifest/provider-element.html) XML parçası 
-   [Android.Content.GrantUriPermissionAttribute](https://developer.xamarin.com/api/type/Android.Content.GrantUriPermissionAttribute/) : oluşturur bir [/manifest/application/provider/grant-uri-permission](http://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML parçası

