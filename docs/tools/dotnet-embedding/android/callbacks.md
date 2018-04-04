---
title: Android'de geri aramalar
ms.prod: xamarin
ms.assetid: F3A7A4E6-41FE-4F12-949C-96090815C5D6
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 72786ac4bceca2635747ebcc844a98b0ce60383f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="callbacks-on-android"></a>Android'de geri aramalar

Java için C# üzerinden çağırma olduğu biraz bir *riskli iş*. Yani var olan bir *düzeni* C# üzerinden Java; geri aramalar için ancak onu isteriz daha çok daha karmaşıktır.

Java için en anlamlı geri aramalar yapmak için üç seçenek şu konulara değineceğiz:

- Soyut sınıflar
- Arabirimler
- Sanal yöntemler

## <a name="abstract-classes"></a>Soyut sınıflar

Kullanmanızı öneririz böylece geri çağırmalar için en kolay yolu budur `abstract` yalnızca basit biçiminde çalışma geri almaya çalışıyorsanız.

Java'yı uygulamak ister misiniz bir C# sınıfı ile başlayalım:

```csharp
[Register("mono.embeddinator.android.AbstractClass")]
public abstract class AbstractClass : Java.Lang.Object
{
    public AbstractClass() { }

    public AbstractClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public abstract string GetText();
}
```

Bunun çalışmasını sağlamak için ayrıntıları aşağıdadır:

- `[Register]` iyi paket adı oluşturur Java'da--bir paketi otomatik olarak oluşturulan adı olmadan alırsınız.
- Sınıflara `Java.Lang.Object` sınıfı Xamarin.Android'ın Java oluşturucuyu çalıştırmak için Embeddinator işaret eder.
- Boş Oluşturucusu: Java koddan kullanmak istediğiniz.
- `(IntPtr, JniHandleOwnership)` Oluşturucusu: Xamarin.Android C# oluşturmak için kullanır-Java nesnelerini eşdeğeridir.
- `[Export]` Java yönteme kullanıma sunmak için Xamarin.Android işaret eder. Java world küçük yöntemleri kullanın istemeyebilir bu yana biz de yöntem adını değiştirebilirsiniz.

Sonraki senaryoyu test etmek için C# yöntem olalım:

```csharp
[Register("mono.embeddinator.android.JavaCallbacks")]
public class JavaCallbacks : Java.Lang.Object
{
    [Export("abstractCallback")]
    public static string AbstractCallback(AbstractClass callback)
    {
        return callback.GetText();
    }
}
```
`JavaCallbacks` Bunu test etmek için herhangi bir sınıf olduğu sürece olabilir bir `Java.Lang.Object`.

Şimdi, Embeddinator bir AAR oluşturmak için .NET derleme çalıştırın. Bkz: [Getting Started guide](~/tools/dotnet-embedding/get-started/java/android.md) Ayrıntılar için.

Android Studio uygulamasına AAR dosya alındıktan sonra birim testi yazalım:

```java
@Test
public void abstractCallback() throws Throwable {
    AbstractClass callback = new AbstractClass() {
        @Override
        public String getText() {
            return "Java";
        }
    };

    assertEquals("Java", callback.getText());
    assertEquals("Java", JavaCallbacks.abstractCallback(callback));
}
```
Böylece biz:

- Uygulanan `AbstractClass` Java anonim bir tür ile
- Bizim örnek bulduğundan emin yapılan `"Java"` Java'dan
- Bizim örnek bulduğundan emin yapılan `"Java"` C# üzerinden
- Eklenen `throws Throwable`, C# oluşturucular şu anda işaretlenir beri `throws`

Karşılaştık varsa, bu birim olarak test-olduğundan, bu başarısız olurdu hatayla gibi:

```csharp
System.NotSupportedException: Unable to find Invoker for type 'Android.AbstractClass'. Was it linked away?
```

Eksik olana burada bir `Invoker` türü. Bu sınıfıdır `AbstractClass` , C# Java çağrılarını iletir. C# world bir Java nesnesi girer ve eşdeğer C# türünün soyut olup durumunda Xamarin.Android otomatik olarak arar bir C# türü soneki `Invoker` C# kod içinde kullanmak için.

Xamarin.Android kullanan bu `Invoker` başka şeylerin Java bağlama projelerde düzeni.

Bizim uyarlamasını işte `AbstractClassInvoker`:
```csharp
class AbstractClassInvoker : AbstractClass
{
    IntPtr class_ref, id_gettext;

    public AbstractClassInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(AbstractClassInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public override string GetText()
    {
        if (id_gettext == IntPtr.Zero)
            id_gettext = JNIEnv.GetMethodID(class_ref, "getText", "()Ljava/lang/String;");
        IntPtr lref = JNIEnv.CallObjectMethod(Handle, id_gettext);
        return GetObject<Java.Lang.String>(lref, JniHandleOwnership.TransferLocalRef)?.ToString();
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Burada, giden oldukça bit yoktur biz:

- Bir sınıf soneki eklenen `Invoker` o alt sınıflar `AbstractClass`
- Eklenen `class_ref` tutmak için JNI başvuru Java sınıfı için o alt sınıfların bizim C# sınıfı
- Eklenen `id_gettext` Java JNI başvuru tutmak için `getText` yöntemi
- Dahil bir `(IntPtr, JniHandleOwnership)` Oluşturucusu
- Uygulanan `ThresholdType` ve `ThresholdClass` Xamarin.Android hakkında ayrıntılı bilgi edinmek bir zorunluluk olarak `Invoker`
- `GetText` Java arama için gereken `getText` yöntemi uygun JNI imzalı ve onu çağırın
- `Dispose` yalnızca başvuru temizlemek için gereklidir `class_ref`

Bu sınıf ekleme ve yeni AAR oluşturma sonra geçişleri bizim birim testi. Geri aramalar için bu deseni değil gördüğünüz *ideal*, ancak ortak.

Java birlikte çalışma hakkında daha fazla bilgi için harika bkz [Xamarin.Android belgelerine](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/working_with_jni/) Bu konu hakkında.

## <a name="interfaces"></a>Arabirimler

Arabirimleri aynıdır çok bir ayrıntı dışında soyut sınıflar: Xamarin.Android oluşturmaz Java kendileri için. .NET katıştırma önce olmadığından Java bir C# arabirimi burada uygulamak birçok senaryo budur.

Aşağıdaki C# arabirimi sahibiz varsayalım:

```csharp
[Register("mono.embeddinator.android.IJavaCallback")]
public interface IJavaCallback : IJavaObject
{
    [Export("send")]
    void Send(string text);
}
```
`IJavaObject` Xamarin.Android arabirimi budur ancak Aksi durumda bu tam olarak aynı olduğundan Embeddinator sinyalleri bir `abstract` sınıfı.

Xamarin.Android şu anda bu arabirim için Java kod oluşturmaz olduğundan, aşağıdaki Java C# projenize ekleyin:

```java
package mono.embeddinator.android;

public interface IJavaCallback {
    void send(String text);
}
```
Dosyanın herhangi bir yere koyun, ancak yapı eylemi ayarlamak için emin olun `AndroidJavaSource`. Bu Embeddinator AAR dosyanıza derlenmiş için uygun bir dizine kopyalayın işaret.

Ardından, `Invoker` uygulaması oldukça aynı olacaktır:

```csharp
class IJavaCallbackInvoker : Java.Lang.Object, IJavaCallback
{
    IntPtr class_ref, id_send;

    public IJavaCallbackInvoker(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer)
    {
        IntPtr lref = JNIEnv.GetObjectClass(Handle);
        class_ref = JNIEnv.NewGlobalRef(lref);
        JNIEnv.DeleteLocalRef(lref);
    }

    protected override Type ThresholdType
    {
        get { return typeof(IJavaCallbackInvoker); }
    }

    protected override IntPtr ThresholdClass
    {
        get { return class_ref; }
    }

    public void Send(string text)
    {
        if (id_send == IntPtr.Zero)
            id_send = JNIEnv.GetMethodID(class_ref, "send", "(Ljava/lang/String;)V");
        JNIEnv.CallVoidMethod(Handle, id_send, new JValue(new Java.Lang.String(text)));
    }

    protected override void Dispose(bool disposing)
    {
        if (class_ref != IntPtr.Zero)
            JNIEnv.DeleteGlobalRef(class_ref);
        class_ref = IntPtr.Zero;

        base.Dispose(disposing);
    }
}
```

Bir AAR dosyasını oluşturduktan sonra aşağıdaki geçirme birim yazma Android Studio'da test edin:

```java
class ConcreteCallback implements IJavaCallback {
    public String text;
    @Override
    public void send(String text) {
        this.text = text;
    }
}

@Test
public void interfaceCallback() {
    ConcreteCallback callback = new ConcreteCallback();
    JavaCallbacks.interfaceCallback(callback, "Java");
    assertEquals("Java", callback.text);
}
```

## <a name="virtual-methods"></a>Sanal yöntemler

Geçersiz kılma bir `virtual` Java'da olası ancak harika bir deneyim değildir.

Aşağıdaki C# sınıfı sahip varsayalım:

```csharp
[Register("mono.embeddinator.android.VirtualClass")]
public class VirtualClass : Java.Lang.Object
{
    public VirtualClass() { }

    public VirtualClass(IntPtr handle, JniHandleOwnership transfer) : base(handle, transfer) { }

    [Export("getText")]
    public virtual string GetText() { return "C#"; }
}
```

İzlediyseniz `abstract` sınıfı yukarıdaki örnekte, bir ayrıntı dışında çalışması: _olmaz Xamarin.Android arama `Invoker`_ .

Bu sorunu gidermek için olması için C# sınıfını değiştirme `abstract`:

```csharp
public abstract class VirtualClass : Java.Lang.Object
```

Bu ideal değildir, ancak bu senaryo çalışma alır. Xamarin.Android çekme `VirtualClassInvoker` ve Java kullanabilir `@Override` yöntemi.

## <a name="callbacks-in-the-future"></a>Geri aramalar gelecekte

Var olan birkaç şey Biz bu senaryoları geliştirmek olabilir:

1. `throws Throwable` C# üzerinde oluşturucular sabit bu [PR](https://github.com/xamarin/java.interop/pull/170).
1. Destek arabirimler Xamarin.Android Java üreteci olun.
    - Bu yapı eylemiyle Java kaynak dosyası ekleme gereksinimini ortadan kaldırır `AndroidJavaSource`.
1. Xamarin.Android yüklemek için bir yol sağlamak bir `Invoker` sanal sınıflar için.
    - Bu sınıfta işaretlemek için gereksinimini ortadan kaldırır bizim `virtual` örnek `abstract`.
1. Oluştur `Invoker` Embeddinator için otomatik olarak sınıfları
    - Bu karmaşık, ancak ortak olacak. Xamarin.Android zaten Java bağlama projeleri için buna benzer bir şey yapıyor.

Burada yapılacak işleri çok yoktur, ancak bu geliştirmeler Embeddinator mümkündür.

## <a name="further-reading"></a>Daha Fazla Bilgi

* [Android Başlarken](~/tools/dotnet-embedding/get-started/java/android.md)
* [Ön Android araştırma](~/tools/dotnet-embedding/android/index.md)
* [Embeddinator sınırlamaları](~/tools/dotnet-embedding/limitations.md)
* [Açık kaynak projesine katkıda bulunan](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Hata kodları ve açıklamaları](~/tools/dotnet-embedding/errors.md)
