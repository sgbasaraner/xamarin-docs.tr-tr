---
title: "Bir Android NUnit Test projesinin nasıl otomatikleştirmek?"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: a5f98fc351c879be55475808b5ab412449dadc7d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Bir Android NUnit Test projesinin nasıl otomatikleştirmek?

> [!NOTE]
> **Not:** bu kılavuz bir Android NUnit test projesi Xamarin.UITest proje adımlarını kapsar. Xamarin.UITest kılavuzları bulunabilir [burada](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

Bir Android birim testi projesi [Mac için Visual Studio] veya birim testi uygulama (Android) [Visual Studio] oluşturduğunuzda, varsayılan olarak otomatik olarak testlerinizi çalışmayacaktır.
Android birim testi otomatik hale getirmek için: hedef cihazda NUnit testleri çalıştırmak için kullanırız bir `Android.App.Instrumentation` oluşturulan ve kullanılarak gerçekleştirilen bir alt `adb shell am instrument` komutu.

İlk olarak, oluşturduğumuz **TestInstrumentation.cs** öğesinin bir alt kümesi oluşturur dosya `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (bildirilen `Xamarin.Android.NUnitLite.dll`). `TestInstrumentation(IntPtr, JniHandleOwnership)` Oluşturucusu _gerekir_ sağlanan ve sanal `AddTests()` yöntemi geçersiz.
`AddTests()` testleri denetimleri gerçekte yürütülür. Büyük ölçüde Demirbaş dosyasıdır.

Ardından, `.csproj` eklemek için değiştirilmelidir `TestInstrumentation.cs`.

İsteğe bağlı olarak, `.csproj` eklemek için değiştirilebilir `RunTests` olarak birim testleri çağırma belirleyebilmesini MSBuild hedef:

```shell
msbuild /t:RunTests Project.csproj
```

Yeni bir hedef kullanmak gerekli değildir; Bunun yerine karşılık gelen adb komutu kullanılabilir:

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

Değiştir `@PACKAGE\_NAME@` olarak uygun; mevcut değeri olan **AndroidManifest.xml** `/manifest/@package` özniteliği.

*Önemli Not*: ile [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) sürüm, varsayılan paket adları Android aranabilir sarmalayıcılar dışarı aktarılan türü bütünleştirilmiş kod tam adının MD5SUM üzerinde tabanlı. Bu, iki farklı derlemelerden sağlanması ve bir paketleme hatası almamış aynı tam ada izin verir. Bu nedenle, kullandığınızdan emin olun \`adı\` özelliği \`Araçları\` okunabilir bir ACW/sınıf adı oluşturmak için relationshipend.

_ACW adı kullanılmalıdır `adb` komutu_. Yeniden adlandırma/C# sınıfı yeniden düzenleme böylece gerektirecektir değiştirme `RunTests` doğru ACW adını kullanmak için komutu.

.Csproj dosya eklemeler:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

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

