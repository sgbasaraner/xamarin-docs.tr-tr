---
title: Android ile çalışmaya başlama
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 1d50d7dbc53271a15e06a7a18d2fa95f91b76ea4
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="getting-started-with-android"></a>Android ile çalışmaya başlama

Gereksinimlerden yanı sıra [Java ile çalışmaya başlama](~/tools/dotnet-embedding/get-started/java/index.md) Kılavuzu de gerekir:

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) veya daha yenisi
* [Android Studio 3.x](https://developer.android.com/studio/index.html) Java 1.8 ile

Bir genel, şunları yapacağız:

* Bir C# Android kitaplığı projesi oluşturma
* .NET NuGet üzerinden katıştırma yükleyin
* Android Kitaplık derlemesi üzerinde çalıştırmak .NET katıştırma
* Android Studio'da bir Java projesi oluşturulan AAR dosyasında kullanın

## <a name="create-an-android-library-project"></a>Bir Android kitaplığıdır projesi oluşturma

Windows için Visual Studio veya Mac açın, yeni bir Android sınıf kitaplığı proje oluşturma, adlandırın **csharp gelen hello**ve kaydetmesi **~/Projects/hello-from-csharp** veya **%USERPROFILE%\ Csharp gelen Projects\hello**.

Yeni bir Android adlı etkinlik eklemek **HelloActivity.cs**, bir Android konumundaki ardından **Resource/layout/hello.axml**.

Yeni bir ekleme `TextView` düzeni ve değişiklik eğlenceli bir şeyler metni.

Düzen kaynağınız aşağıdakine benzer görünmelidir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

Etkinlik, çağırdığınız emin olun `SetContentView` yeni düzen ile:

```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```

> [!NOTE]
> Unutmayın `[Register]` özniteliği. Ayrıntılar için bkz [sınırlamalar](#current-limitations-on-android).

Projeyi oluşturun. Elde edilen derlemeyi kaydedileceği `bin/Debug/hello-from-csharp.dll`.

## <a name="installing-net-embedding-from-nuget"></a>.NET Nuget'ten katıştırma yükleme

Aşağıdaki adımları [yönergeleri](~/tools/dotnet-embedding/get-started/install/install.md) yüklemek ve .NET katıştırma projeniz için yapılandırmak için.

Yapılandırmanız komut çağırma şuna benzeyecektir:

### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Android Studio projesinde oluşturulan çıktı kullanın

1. Android Studio'yu açın ve yeni bir proje ile oluşturma bir **boş etkinlik**.
2. Sağ tıklayın, **uygulama** modülü ve **yeni > Modülü**.
3. Seçin **alma. JAR /. AAR paket**.
4. Dizin tarayıcısı bulmaya **~/Projects/hello-from-csharp/output/hello_from_csharp.aar** tıklatıp **son**.

![Android Studio AAR içe](android-images/androidstudioimport.png)

Bu AAR dosya adlı yeni bir modüle kopyalayacak **hello_from_csharp**.

![Android Studio projesi](android-images/androidstudioproject.png)

Yeni modülünden kullanmak için **uygulama**seçin ve sağ tıklatıp **açık modülü ayarları**. Üzerinde **bağımlılıkları** sekmesinde, yeni bir ekleme **modülü bağımlılık** ve **: hello_from_csharp**.

![Android Studio bağımlılıkları](android-images/androidstudiodependencies.png)

Etkinlik, yeni bir ekleme `onResume` yöntemi ve C# etkinlik başlatmak için aşağıdaki kodu kullanın:

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>Derleme sıkıştırma (*önemli*)

Bir başka değişiklik .NET katıştırmak için Android Studio projenizde gereklidir.

Uygulamanızın açmak **build.gradle** dosya ve aşağıdaki değişiklik ekleyin:

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin.Android .NET derlemelerini doğrudan APK şu anda yükler. ancak derlemeleri sıkıştırılmaz gerekir.

Bu kurulum yoksa, uygulama başlatılırken kilitlenme ve şöyle bir şey konsola yazdırma:

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>Uygulama Çalıştırma

Uygulamanızı başlatma bağlıdır:

![Merhaba örnekten öykünücüsünde çalıştıran C#](android-images/hello-from-csharp-android.png)

Burada ne dikkat edin:

* Bir C# sınıfını sahibiz `HelloActivity`, o alt sınıfların Java
* Android kaynak dosyaları sahibiz
* Android Studio'da kullandık bunlar Java'dan

Bu örnek çalışması, aşağıdakilerin tümü içinde son APK ayarlanır:

* Xamarin.Android uygulaması başlangıç üzerinde yapılandırılmış
* .NET derlemelerini dahil **varlıklar/derlemeler**
* **AndroidManifest.xml** C# etkinlikleri, vb. için değişiklikler.
* Android kaynakları ve .NET kitaplıklarına varlıklarından
* [Android aranabilir sarmalayıcılar](~/android/platform/java-integration/android-callable-wrappers.md) herhangi `Java.Lang.Object` alt sınıfı

Ek bir anlatım için arıyorsanız, Charles Petzold'un'ın katıştırma gösteren aşağıdaki video, denetleme [FingerPaint demo](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) Android Studio projesi içinde:

[![Android için Embeddinator 4000](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Java 1.8 kullanma

Bu yazma itibariyle Android Studio 3.0 kullanmak için en iyi seçenek olan ([buradan indirin](https://developer.android.com/studio/index.html)).

Uygulama modülünün 1.8 Java'da etkinleştirmek için **build.gradle** dosyası:

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

Bir göz atalım bir [Android Studio test projesi](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) daha fazla ayrıntı için. 

Android Studio 2.3.x kararlı kullanmak isteyen, kullanım dışı prizine zincirinin etkinleştirmeniz gerekir:

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Android geçerli sınırlamalar

Şimdi, sağ, alt `Java.Lang.Object`, Xamarin.Android .NET yerine Java saplama (Android aranabilir sarmalayıcısı) üretir. Bu nedenle, C# Java Xamarin.Android olarak verme için aynı kurallara uymalıdır. Örneğin:

```csharp
[Register("mono.embeddinator.android.ViewSubclass")]
public class ViewSubclass : TextView
{
    public ViewSubclass(Context context) : base(context) { }

    [Export("apply")]
    public void Apply(string text)
    {
        Text = text;
    }
}
```

* `[Register]` istediğiniz Java Paket adı için eşlemek için gereklidir
* `[Export]` bir yöntem Java görünür yapmak için gereklidir

Biz kullanabilirsiniz `ViewSubclass` Java sözlüğüdür:

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

Daha fazla bilgi edinin [Xamarin.Android ile Java tümleştirme](~/android/platform/java-integration/index.md).

## <a name="multiple-assemblies"></a>Birden çok derleme

Tek bir derleme katıştırma kolaydır; Ancak, bir C# derleme birden fazla olacaktır çok daha olasıdır. Birçok kez Android destek kitaplıkları veya Google Play Hizmetleri'nin şeyleri daha karmaşık hale gibi NuGet paketlerini üzerinde bağımlılıklara sahip olur.

.NET katıştırma gibi son AAR içine birçok dosya türü eklemek gerektiğinden bu bir ikilemini neden olur:

* Android varlıklar
* Android kaynakları
* Android yerel kitaplıkları
* Android java kaynak

Büyük olasılıkla bu dosyaları Android destek kitaplığı veya Google Play Hizmetleri, AAR eklemek istiyor musunuz, ancak Google resmi sürümünden Android Studio'da yerine kullanırsınız.

Önerilen yaklaşım şöyledir:

* Sahip olduğunuz derleme geçirmek .NET katıştırma (kaynağı varsa) ve Java aramak istediğiniz
* Android varlıklar, yerel kitaplıkları veya kaynaklardan gereksinim derleme geçirmek .NET katıştırma
* Android gibi Java bağımlılıkları kitaplığı veya Google Play Hizmetleri'nin Android Studio'da desteği ekleme

Bu nedenle, komut aşağıdaki gibi olabilir:

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

Herhangi bir şey Nuget'ten bırakmanız gerekir, sizin öğrenmek sürece içerdiği Android varlıklar, kaynaklar, Android Studio projenizde gerekir vb. Ayrıca Java ve bağlayıcı çağrı gerekmez bağımlılıkları atlayabilirsiniz _gereken_ gerekli kitaplığınızın parçalarını içerir.

Android Studio'da gerekli Java bağımlılıkları eklemek için **build.gradle** dosya gibi görünebilir:

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>Daha fazla bilgi

* [Android'de geri aramalar](~/tools/dotnet-embedding/android/callbacks.md)
* [Ön Android araştırma](~/tools/dotnet-embedding/android/index.md)
* [.NET katıştırma sınırlamaları](~/tools/dotnet-embedding/limitations.md)
* [Açık kaynak projesine katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Hata kodları ve açıklamaları](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>İlgili bağlantılar

- [Hava durumu örnek (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
