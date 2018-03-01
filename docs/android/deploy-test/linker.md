---
title: "Android bağlama"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3528E195-AA74-90AF-B5F3-3B65FB4F0BB8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: 01a45f02d340effe69d1cb0cff7f0d8e5ca7bef6
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="linking-on-android"></a>Android bağlama

Xamarin.Android uygulamaları kullanan bir *bağlayıcı* uygulama boyutunu azaltmak için. Bağlayıcı employes statik çözümleme hangi derlemelerin gerçekte kullanılır, hangi türleri gerçekte kullanılır ve gerçekte kullanılan hangi üyeler belirlemek için uygulamanızın. Bağlayıcı sonra gibi davranır bir *atık toplayıcı*, derlemeleri, türleri ve başvurulan derlemelerin türleri, tüm kapatma kadar başvurulan üyeleri için sürekli olarak görünümlü ve üyeleri bulundu. Bu kapatma dışında her şey sonra *atılan*.

Örneğin, [Hello, Android](https://developer.xamarin.com/samples/HelloM4A/) örnek:

<table border="0" cellpadding="1" cellspacing="1">
  <tbody>
    <tr>
      <td>
        <strong>Yapılandırma</strong>
      </td>
      <td>
        <strong>1.2.0 boyutu</strong>
      </td>
      <td>
        <strong>4.0.1 boyutu</strong>
      </td>
    </tr>
    <tr>
      <td>
Bağlamadan sürüm: </td>
      <td>
14.0 MB </td>
      <td>
16.0 MB </td>
    </tr>
    <tr>
      <td>
Bağlama ile sürüm: </td>
      <td>
4.2 MB </td>
      <td>
2.9 MB </td>
    </tr>
  </tbody>
</table>

% 30 bir paket sonuçlarında 1.2.0 özgün (bağlantısız) paketinde boyutunu ve 4.0.1 bağlantısız paketinde %18 bağlanıyor.

 <a name="Control" />


## <a name="control"></a>Denetim

Bağlama temel *statik çözümleme*. Sonuç olarak, çalışma zamanı ortamı bağlıdır herhangi bir şey algılanmaz:

```csharp
// To play along at home, Example must be in a different assembly from MyActivity.
public class Example {
    // Compiler provides default constructor...
}

[Activity (Label="Linker Example", MainLauncher=true)]
public class MyActivity {
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Will this work?
        var o = Activator.CreateInstance (typeof (ExampleLibrary.Example));
    }
}
```

<a name="Linker_Behavior" />

### <a name="linker-behavior"></a>Bağlayıcı davranışı

Denetleme bağlayıcı için birincil mekanizma **bağlayıcı davranışı** (*bağlama* Visual Studio) içinde açılan **proje seçenekleri** iletişim kutusu. Üç seçenek vardır:

1.  **Bağlantı** (*hiçbiri* Visual Studio)
1.  **Bağlantı SDK derlemeleri** (*yalnızca Sdk derlemeleri*)
1.  **Tüm derlemelerde bağlantı** (*Sdk ve kullanıcı derlemeleri*)


**Olmayan bağlantı** seçeneği bağlayıcı devre dışı bırakır; Bu davranış yukarıdaki "Olmadan yayın bağlama" uygulama boyutu örnek kullanılır. Bu, bağlayıcı sorumlu olup olmadığını görmek için çalışma zamanı hataları gidermek için kullanışlıdır. Bu ayar genellikle üretim derlemeler için önerilmez.

**Bağlantı SDK derlemeleri** seçeneği yalnızca bağlantıları [Xamarin.Android ile gelen derlemeleri](~/cross-platform/internals/available-assemblies.md).
Diğer tüm derlemelerini (örneğin, kodunuzu) bağlı değil.

**Bağlantı tüm derlemelerde** seçeneği bağlanan tüm derlemelerde kodunuzu, ayrıca statik başvuru varsa kaldırılabilir anlamına gelir.

Yukarıdaki örnek ile çalışıp çalışmayacağını *olmayan bağlantı* ve *bağlantı SDK derlemeleri* seçenekleri ve başarısız *bağlantı tüm derlemelerde* davranışı, aşağıdaki hata oluşturmadan :

```shell
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): UNHANDLED EXCEPTION: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type,bool) <0x00180>
I/MonoDroid(17755): at System.Activator.CreateInstance (System.Type) <0x00017>
I/MonoDroid(17755): at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle) <0x00027>
I/MonoDroid(17755): at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (intptr,intptr,intptr) <0x00057>
I/MonoDroid(17755): at (wrapper dynamic-method) object.95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr) <0x00033>
E/mono    (17755): [0xafd4d440:] EXCEPTION handling: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):
E/mono    (17755): Unhandled Exception: System.MissingMethodException: Default constructor not found for type ExampleLibrary.Example.
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type, Boolean nonPublic) [0x00000] in <filename unknown>:0
E/mono    (17755):   at System.Activator.CreateInstance (System.Type type) [0x00000] in <filename unknown>:0
E/mono    (17755):   at LinkerScratch2.Activity1.OnCreate (Android.OS.Bundle bundle) [0x00000] in <filename unknown>:0
E/mono    (17755):   at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (IntPtr jnienv, IntPtr native__this, IntPtr native_savedInstanceState) [0x00000] in <filename unknown>:0
E/mono    (17755):   at (wrapper dynamic-method) object:95bb4fbe-bef8-4e5b-8e99-ca83a5d7a124 (intptr,intptr,intptr)
```

<a name="PreserveAttribute" />

### <a name="preserving-code"></a>Kod koruma

Bağlayıcı, korumak istediğiniz kod bazen kaldırır. Örneğin:

-   Dinamik olarak aracılığıyla çağrı kodu olabilir `System.Reflection.MemberInfo.Invoke`.

-   Dinamik olarak türleri örneği varsa, türlerinin varsayılan oluşturucu korumak isteyebilirsiniz.

-   XML serileştirme kullanırsanız, türlerinizi özelliklerini korumak isteyebilirsiniz.

Bu durumlarda, kullandığınız [Android.Runtime.Preserve](https://developer.xamarin.com/api/type/Android.Runtime.PreserveAttribute/) özniteliği. Uygulama tarafından statik olarak bağlantılı olmayan her üye tabidir kaldırılması, bu öznitelik, statik olarak başvurulan değil ancak hala uygulamanız tarafından gerekli üyeleri işaretlemek için kullanılabilmesi için. Bu öznitelik her üye bir türde veya türü uygulayabilirsiniz.

Aşağıdaki örnekte, bu öznitelik oluşturucusunun korumak için kullanılan `Example` sınıfı:

```csharp
public class Example
{
    [Android.Runtime.Preserve]
    public Example ()
    {
    }
}
```

Tüm türü korumak istiyorsanız, aşağıdaki öznitelik sözdizimini kullanabilirsiniz:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
```

Örneğin, aşağıdaki kodda tüm parçalara `Example` sınıfı XML serileştirmesi için korunur:

```csharp
[Android.Runtime.Preserve (AllMembers = true)]
class Example
{
    // Compiler provides default constructor...
}
```

Bazen, kapsayan tür korunur, ancak yalnızca belirli üyeleri korumak istediğiniz. Bu durumlarda aşağıdaki öznitelik sözdizimini kullanın:

```csharp
[Android.Runtime.Preserve (Conditional = true)]
```

Xamarin kitaplıkları'nı bir bağımlılık almak istemiyorsanız &ndash; Örneğin, platformlar arası taşınabilir sınıf kitaplığı (PCL) oluşturmakta olduğunuz &ndash; kullanmaya devam edebilirsiniz `Android.Runtime.Preserve` özniteliği. Bunu yapmak için bildirme bir `PreserveAttribute` içinde sınıf `Android.Runtime` ad alanı şuna benzer:

```csharp
namespace Android.Runtime
{
    public sealed class PreserveAttribute : System.Attribute
    {
        public bool AllMembers;
        public bool Conditional;
    }
}
```


<a name="falseflag" />

### <a name="falseflag"></a>falseflag

[Koru] öznitelik kullanılamaz, genellikle böylece çalışma zamanında yürütülen kod bloğunu önlerken türü kullanıldığını bağlayıcı düşündüğü bir kod bloğu sağlamak kullanışlıdır. Yapmak için yapabileceğimiz, bu teknik kullanın:

```csharp
[Activity (Label="Linker Example", MainLauncher=true)]
class MyActivity {

#pragma warning disable 0219, 0649
    static bool falseflag = false;
    static MyActivity ()
    {
        if (falseflag) {
            var ignore = new Example ();
        }
    }
#pragma warning restore 0219, 0649

    // ...
}
```


<a name="linkskip" />

### <a name="linkskip"></a>linkskip

Kullanıcı tarafından sağlanan derlemeler kümesini, ile atlanacak diğer kullanıcı derlemelerinde izin verirken bağlanmaması gerektiğinin olduğunu belirtmek mümkündür *bağlantı SDK derlemeleri* kullanarak davranışı [AndroidLinkSkip MSBuild özelliği](~/android/deploy-test/building-apps/build-process.md):

```xml
<PropertyGroup>
    <AndroidLinkSkip>Assembly1;Assembly2</AndroidLinkSkip>
</PropertyGroup>
```

<a name="LinkDescription" />

### <a name="linkdescription"></a>LinkDescription

[ `@(LinkDescription)` ](~/android/deploy-test/building-apps/build-process.md) 
 **Yapı eylemi** içeren dosyalarda kullanılabilir bir [özel bağlayıcı yapılandırma dosyası](~/cross-platform/deploy-test/linker.md).
dosya. Özel bağlayıcı yapılandırma gereken dosyaları korumak için `internal` veya `private` korunması gereken üyeleri.


<a name="Custom_Attributes" />

### <a name="custom-attributes"></a>Özel Öznitelikler

Bir derlemeyi bağlandığında, aşağıdaki özel öznitelik türlerini tüm üyelerinden kaldırılacak:

-  System.ObsoleteAttribute
-  System.MonoDocumentationNoteAttribute
-  System.MonoExtensionAttribute
-  System.MonoInternalNoteAttribute
-  System.MonoLimitationAttribute
-  System.MonoNotSupportedAttribute
-  System.MonoTODOAttribute
-  System.Xml.MonoFIXAttribute


Bir derlemeyi bağlandığında, sürümdeki tüm üyelerinden türleri kaldırılacak aşağıdaki özel öznitelik oluşturur:

-  System.Diagnostics.DebuggableAttribute
-  System.Diagnostics.DebuggerBrowsableAttribute
-  System.Diagnostics.DebuggerDisplayAttribute
-  System.Diagnostics.DebuggerHiddenAttribute
-  System.Diagnostics.DebuggerNonUserCodeAttribute
-  System.Diagnostics.DebuggerStepperBoundaryAttribute
-  System.Diagnostics.DebuggerStepThroughAttribute
-  System.Diagnostics.DebuggerTypeProxyAttribute
-  System.Diagnostics.DebuggerVisualizerAttribute


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Bağlayıcı yapılandırması](~/cross-platform/deploy-test/linker.md)
- [İos'ta bağlama](~/ios/deploy-test/linker.md)
