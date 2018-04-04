---
title: Bir Android NUnit Test projesinin nasıl otomatikleştirmek?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2018
ms.openlocfilehash: f63a25f36682038b7fcd85d711d980b9e3ec869d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Bir Android NUnit Test projesinin nasıl otomatikleştirmek?

> [!NOTE]
> Bu kılavuz, bir Android NUnit test projesi Xamarin.UITest proje otomatikleştirmek açıklanmaktadır. Xamarin.UITest kılavuzları bulunabilir [burada](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Oluştururken bir **birim testi uygulama (Android)** Visual Studio projesi (veya **Android birim testi** Mac için Visual Studio Proje), varsayılan olarak otomatik olarak testlerinizi bu proje olacaktır.
Hedef cihazda NUnit testleri çalıştırmak için oluşturabileceğiniz bir [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/) aşağıdaki komutu kullanarak tarafından başlatılan bir alt: 

```shell
adb shell am instrument 
```

Bu işlem aşağıdaki adımlarda açıklanmaktadır:

1.  Adlı yeni bir dosya oluşturun **TestInstrumentation.cs**: 

    ```cs 
    using System;
    using System.Reflection;
    using Android.App;
    using Android.Content;
    using Android.Runtime;
    using Xamarin.Android.NUnitLite;
     
    namespace App.Tests {
     
        [Instrumentation(Name="app.tests.TestInstrumentation")]
        public class TestInstrumentation : TestSuiteInstrumentation {
     
            public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
            {
            }
     
            protected override void AddTests ()
            {
                AddTest (Assembly.GetExecutingAssembly ());
            }
        }
    }
    ```
    Bu dosyadaki [Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (gelen **Xamarin.Android.NUnitLite.dll**) oluşturmak için sınıflandırma `TestInstrumentation`.

2.  Uygulama [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) oluşturucusu ve [AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29) yöntemi. `AddTests` Hangi testlerin gerçekte yürütülen yöntemi kontrol eder.

3.  Değiştirme `.csproj` eklemek için dosya **TestInstrumentation.cs**. Örneğin:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

3.  Birim testleri çalıştırmak için aşağıdaki komutu kullanın. Değiştir `PACKAGE_NAME` uygulamanın paket adı ile (paket adı uygulamanın içinde bulunabilir `/manifest/@package` özniteliği bulunan **AndroidManifest.xml**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  İsteğe bağlı olarak, değiştirebileceğiniz `.csproj` eklemek için dosya `RunTests` MSBuild hedef. Bu, aşağıdakine benzer bir komutu ile birim testleri çağırmak mümkün kılar:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (Bu yeni hedef kullanarak gerekli olmadığına dikkat edin; önceki `adb` komutu yerine kullanılabilir `msbuild`.)

Kullanma hakkında daha fazla bilgi için `adb shell am instrument` birim testleri çalıştırma, Android Geliştirici görmek için komutu [ADB ile testleri çalıştırma](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) konu.


> [!NOTE]
> İle [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) sürüm, varsayılan paket adları Android aranabilir sarmalayıcılar dışarı aktarılan türü bütünleştirilmiş kod tam adının MD5SUM üzerinde tabanlı. Bu, iki farklı derlemelerden sağlanması ve bir paketleme hatası almamış aynı tam ada izin verir. Bu nedenle, kullandığınızdan emin olun `Name` özellikte `Instrumentation` okunabilir bir ACW/sınıf adı oluşturmak için öznitelik.

_ACW adı kullanılmalıdır `adb` yukarıdaki komut_.
Yeniden adlandırma/C# sınıfı yeniden düzenleme böylece gerektirecektir değiştirme `RunTests` doğru ACW adını kullanmak için komutu.

