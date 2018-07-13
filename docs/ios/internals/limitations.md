---
title: Xamarin.iOS sınırlamaları
description: Bu belgede ele genel türler, Genel alt sınıfları NSObjects, P/Invoke'lar genel nesneleri ve daha fazlası Xamarin.iOS, sınırlamaları açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/09/2018
ms.openlocfilehash: e154e4e1688b8a3d03459956934409fa9d5aef35
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998091"
---
# <a name="limitations-of-xamarinios"></a>Xamarin.iOS sınırlamaları

Xamarin.iOS kullanarak iPhone uygulamaları için statik kod derlenmiş olduğundan, çalışma zamanında kod oluşturma gerektiren tüm özelliklerini kullanabilmeniz mümkün değildir.

Mono masaüstüne karşılaştırıldığında Xamarin.iOS kısıtlamaları şunlardır:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Sınırlı genel türler desteği

Geleneksel Mono/.NET, isteğe bağlı bir JIT derleyicisi tarafından derlenen yerine önceden statik olarak iPhone kod derlenir.

Mono'nın [tam AOT](http://www.mono-project.com/docs/advanced/aot/#full-aot) teknoloji genel türler ile ilgili birkaç sınırlama vardır, bunlar her olası genel örnek oluşturma, derleme zamanında Önden belirlenebilir nedeni. Kod her zaman tam zaman derleyicide kullanılarak çalışma zamanında derlenmiş gibi normal .NET ya da Mono çalışma zamanı için bir sorun değildir. Ancak bu Xamarin.iOS gibi statik bir derleme için bir sınama oluşturur.

Bazı geliştiriciler, çalışan yaygın sorunlar şunlardır:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Genel alt sınıfları NSObjects sınırlıdır

Xamarin.iOS şu anda sınırlı NSObject sınıfın genel metotlar için desteği yok gibi genel alt sınıfları oluşturmak için desteği içerir. 7.2.1 itibarıyla NSObjects Genel alt sınıfları kullanarak bunu gibi mümkündür:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> Genel alt sınıfları NSObjects mümkün olsa da, bazı sınırlamalar vardır. Okuma [nsobject'in Genel alt](~/ios/internals/api-design/nsobject-generics.md) belge için daha fazla bilgi



### <a name="pinvokes-in-generic-types"></a>P/çağırır, genel türler

P/Invoke'lar genel sınıflardaki desteklenmez:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Boş değer atanabilir bir tür üzerinde Property.SetInfo desteklenmiyor

Yansıma'nın Property.SetInfo değeri üzerinde bir boş değer ayarlamak için kullanarak&lt;T&gt; şu anda desteklenmiyor.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Değer türleri sözlük anahtarları

Bir sözlük olarak bir değer türü kullanarak&lt;TKey, TValue&gt; anahtardır sorunlu, varsayılan olarak Sözlük Oluşturucusu EqualityComparer kullanmayı dener&lt;TKey&gt;. Varsayılan. EqualityComparer&lt;TKey&gt;. Varsayılan, sırayla çalışır yansıma IEqualityComparer uygulayan yeni bir türü örneklemek için kullanılacak&lt;TKey&gt; arabirimi.

Bu başvuru türleri için geçerlidir (yansıtma olarak + yeni Oluştur türü adımı atlanır), ancak değeri kilitlenmeleri türleri ve bu cihazda kullanma girişimi sonra yerine hızlı bir şekilde harekete.

 **Geçici çözüm**: el ile uygulamak [IEqualityComparer&lt;TKey&gt; ](xref:System.Collections.Generic.IEqualityComparer`1) arabirim yeni türde ve sağlamak için bu türün bir örneği [sözlük&lt;TKey, TValue&gt; ](xref:System.Collections.Generic.Dictionary`2) [(IEqualityComparer&lt;TKey&gt;)](xref:System.Collections.Generic.IEqualityComparer`1) Oluşturucusu.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Dinamik kod üretme

İPhone ait çekirdek dinamik olarak kod oluşturma uygulamanın önlediğinden Mono iphone'daki herhangi bir biçimde dinamik kod oluşturmayı desteklemez. Bu güncelleştirmeler şunlardır:

-  System.Reflection.Emit kullanılamıyor.
-  System.Runtime.Remoting desteği yok.
-  Türleri dinamik olarak oluşturmak için destek yok (hiçbir aktarýldýðý ("MyType ' 1")), mevcut türlerini (aktarýldýðý ("System.String") gibi düzgün çalışır) arama rağmen. 
-  Ters geri çağırmaları derleme zamanında çalışma zamanıyla kaydedilmesi gerekir.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

System.Reflection eksiği. **Yayma** üzerinde çalışma zamanı kodu oluşturma bağımlı olan hiçbir kod anlamına gelir. Bu gibi şeyleri içerir:

-  Dinamik dil çalışma zamanı.
-  Dinamik dil çalışma zamanı üzerinde oluşturulan tüm diller.
-  Uzaktan iletişimi'nın TransparentProxy veya başka bir şey, çalışma zamanı dinamik olarak kod oluşturmasına neden olur. 


 **Önemli:** karıştırmayın **Reflection.Emit** ile **yansıma**. Reflection.Emit dinamik olarak kod oluşturma hakkında ve bu kodu Jıted ve yerel için derlenmiş kodu. İPhone (JIT derleme) sınırlamaları nedeniyle bu desteklenmez.

Ancak, öznitelikleri ve değerleri getirilirken özellikleri listeleme yöntemleri listeleme aktarýldýðý ("someClass") dahil olmak üzere tüm yansıma API'si, düzgün çalışır.

### <a name="using-delegates-to-call-native-functions"></a>Yerel işlevleri çağırmak için temsilcileri kullanma

Bir C# temsilci aracılığıyla yerel bir işlevi çağırmak için temsilcinin bildirimi aşağıdaki özniteliklerden birini ile donatılmış gerekir:

- [UnmanagedFunctionPointerAttribute](xref:System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute) (platformlar arası ve .NET Standard 1.1 + ile uyumlu olduğundan, tercih edilir)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Özniteliklerden birini sağlamak başarısız olduğu gibi çalışma zamanı hatasına neden olur:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Ters geri çağırmaları

Standart Mono içinde bir işlev işaretçisi yerine yönetilmeyen kod için C# temsilci örneklerini geçirmek mümkündür. Çalışma zamanı genellikle bu işlev işaretçileri geri yönetilen koda çağrı yapmak yönetilmeyen kod sağlayan küçük bir dönüştürücü dönüştürün.

Mono'da bu köprüleri Just-ın-Time tarafından uygulanan derleyici. Ne zaman, saat tamamlanan derleyici kullanarak iPhone tarafından iki önemli sınırlamalar vardır, bu aşamada gerekli:

-  Tüm, geri çağırma yöntemleri ile bayrak [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Yöntemleri statik yöntemler olması, desteği yoktur örneği için yöntemleri. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Hiçbir uzaktan iletişim

Uzaktan iletişim yığını üzerinde Xamarin.iOS kullanılabilir değil.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Çalışma zamanı özellikleri devre dışı

Mono'nın iOS çalışma zamanı aşağıdaki özellikler devre dışı bırakıldı:

-  Profil Oluşturucu
-  Reflection.Emit
-  Reflection.Emit.Save işlevi
-  COM bağlamaları
-  JIT altyapısına
-  (Hiçbir JIT olmadığından) meta veri doğrulama


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API sınırlamaları

Her şey iOS kullanılabilir olduğu gibi kullanıma sunulan .NET API tam framework'ün bir alt kümesidir. Bkz. SSS bir [şu anda desteklenen derlemelerin listesini](~/cross-platform/internals/available-assemblies.md).



Özellikle, Xamarin.iOS tarafından kullanılan API profilini System.Configuration, içermez, bu dış XML dosyaları çalışma zamanı davranışını yapılandırmak için kullanmak mümkün değildir.
