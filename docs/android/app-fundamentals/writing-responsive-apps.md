---
title: Esnek uygulamaları yazma
ms.prod: xamarin
ms.assetid: 452DF940-6331-55F0-D130-002822BBED55
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: b8c113b67b3fbfa57ca86c72e11ddeb0e4e1a9ab
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763507"
---
# <a name="writing-responsive-applications"></a>Esnek uygulamaları yazma

Esnek GUI koruma anahtarları uzun süre çalışan görevler arka plan iş parçacığında GUI engellenen değil Bunu yapmak için biridir. Değeri hesaplamak için 5 saniye alır ancak bu kullanıcıya görüntülemek için bir değeri hesaplamak istiyoruz varsayalım:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        SlowMethod ();
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Bu çalışır, ancak "değeri hesaplanırken uygulama 5 saniye bağlantıyı". Bu süre boyunca, uygulama kullanıcı etkileşimi yanıt vermez. Bu sorunu almak için bir arka plan iş parçacığı bizim hesaplamaları yapmak istiyoruz:

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        textview.Text = "Method Complete";
    }
}
```

Şimdi bizim GUI hesaplama sırasında yanıt verebilir durumda kalması biz arka plan iş parçacığında değeri hesaplayın. Hesaplama yapılır, ancak, uygulamamıza, bu oturum açma bırakarak çöküyor:

```shell
E/mono    (11207): EXCEPTION handling: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):
E/mono    (11207): Unhandled Exception: Android.Util.AndroidRuntimeException: Exception of type 'Android.Util.AndroidRuntimeException' was thrown.
E/mono    (11207):   at Android.Runtime.JNIEnv.CallVoidMethod (IntPtr jobject, IntPtr jmethod, Android.Runtime.JValue[] parms)
E/mono    (11207):   at Android.Widget.TextView.set_Text (IEnumerable`1 value)
E/mono    (11207):   at MonoDroidDebugging.Activity1.SlowMethod ()
```

Bu durum, GUI iş GUI güncelleştirmeniz gerekir çünkü. Bizim kodu uygulamanın kilitlenmesine neden GUI iş parçacığı havuzu iş güncelleştirir. Arka plan iş parçacığı bizim değerini hesaplamak, ancak bizim güncelleştirme ile işlenen GUI parçacığında yapmak için ihtiyacımız [Activity.RunOnUIThread](https://developer.xamarin.com/api/member/Android.App.Activity.RunOnUiThread/(System.Action)):

```csharp
public class ThreadDemo : Activity
{
    TextView textview;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Create a new TextView and set it as our view
        textview = new TextView (this);
        textview.Text = "Working..";

        SetContentView (textview);

        ThreadPool.QueueUserWorkItem (o => SlowMethod ());
    }

    private void SlowMethod ()
    {
        Thread.Sleep (5000);
        RunOnUiThread (() => textview.Text = "Method Complete");
    }
}
```

Bu kod, beklendiği gibi çalışır. Bu GUI yanıt verebilir durumda kalır ve hesaplama gö olduğunda düzgün şekilde güncelleştirilir.

Bu yöntem yalnızca pahalı bir değeri hesaplamak için kullanılan değil unutmayın. Bir web hizmeti çağrısı veya indirme Internet verileri gibi arka planda yapılan herhangi bir uzun süre çalışan görev için kullanılabilir.
