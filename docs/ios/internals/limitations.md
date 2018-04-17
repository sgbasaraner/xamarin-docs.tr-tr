---
title: Sınırlamalar
ms.prod: xamarin
ms.assetid: 5AC28F21-4567-278C-7F63-9C2142C6E06A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/09/2018
ms.openlocfilehash: 8bd4ce464adf316517e2e1f2299006913bc68736
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="limitations"></a>Sınırlamalar

Xamarin.iOS kullanarak iPhone uygulamaları statik koda derlenmiş olduğundan, kod oluşturma zamanında gerektiren tüm özellikleri kullanmak mümkün değil.

Bu Masaüstü Mono karşılaştırıldığında Xamarin.iOS sınırlamalar vardır:

 <a name="Limited_Generics_Support" />


## <a name="limited-generics-support"></a>Sınırlı genel türler desteği

Geleneksel Mono/.NET, iPhone statik olarak isteğe bağlı bir JIT Derleyici tarafından derlenmiş yerine önceden derlenmiş kod.

Mono'nın [tam Uygulama Nesne AĞACI](http://www.mono-project.com/docs/advanced/aot/#full-aot) teknoloji genel türler göre birkaç sınırlamalara sahiptir, bu olası her genel olgu Önden derleme zamanında belirlenebilir nedeni. Her zaman yalnızca zaman derleyicide kullanarak çalışma zamanında derlenmiş kod gibi normal .NET ya da Mono çalışma zamanları için bir sorun değildir. Ancak bu bir statik derleyici Xamarin.iOS gibi için bir sınama oluşturur.

Bazı geliştiriciler, çalışan ortak sorunları şunları içerir:

 <a name="Generic_Subclasses_of_NSObjects_are_limited" />


### <a name="generic-subclasses-of-nsobjects-are-limited"></a>Genel alt sınıfların NSObjects sınırlıdır

Xamarin.iOS şu anda NSObject sınıfının genel yöntemler desteği gibi genel alt sınıflar oluşturmak için destek sınırlıdır. 7.2.1 itibariyle NSObjects Genel alt sınıflarının kullanarak bunu gibi mümkün değildir:

```csharp
class Foo<T> : UIView {
    [..]
}
```

> [!NOTE]
> NSObjects Genel alt sınıflarının mümkün olsa da, bazı sınırlamalar vardır. Okuma [NSObject Genel alt sınıflarının](~/ios/internals/api-design/nsobject-generics.md) belge daha fazla bilgi için



### <a name="pinvokes-in-generic-types"></a>Genel türlerde P/çağırır

Genel sınıflar P/Invokes desteklenmez:

```csharp
class GenericType<T> {
    [DllImport ("System")]
    public static extern int getpid ();
}
```

 <a name="Property.SetInfo_on_a_Nullable_Type_is_not_supported" />


### <a name="propertysetinfo-on-a-nullable-type-is-not-supported"></a>Boş değer atanabilir bir tür üzerinde Property.SetInfo desteklenmiyor

Yansıma'nın Property.SetInfo üzerinde null atanabilir değer ayarlamak için kullanma&lt;T&gt; şu anda desteklenmiyor.

 <a name="Value_types_as_Dictionary_Keys" />


### <a name="value-types-as-dictionary-keys"></a>Değer türleri olarak sözlük anahtarları

Değer türü bir sözlük olarak kullanarak&lt;TKey, TValue&gt; anahtarıdır sorunlu, varsayılan olarak Sözlük Oluşturucusu EqualityComparer kullanma girişiminde&lt;TKey&gt;. Varsayılan. EqualityComparer&lt;TKey&gt;. Varsayılan, sırayla çalışır yansıma IEqualityComparer uygulayan yeni bir tür örneklemek için kullanılacak&lt;TKey&gt; arabirimi.

Bu başvuru türleri için geçerlidir (yansıma olarak + yeni bir tür adım atlanır), ancak değeri için kilitlenme türleri ve cihazda kullanma girişimi sonra yerine hızlı bir şekilde yakar.

 **Geçici çözüm**: el ile uygulamak [IEqualityComparer&lt;TKey&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) arabirim yeni bir tür ve bu türünün bir örneğini sağlayın [sözlük&lt;TKey, TValue&gt; ](https://developer.xamarin.com/api/type/System.Collections.Generic.Dictionary%3CTKey,TValue%3E/) [(IEqualityComparer&lt;TKey&gt;)](https://developer.xamarin.com/api/type/System.Collections.Generic.IEqualityComparer%601/) Oluşturucusu.


 <a name="No_Dynamic_Code_Generation" />


## <a name="no-dynamic-code-generation"></a>Hiçbir dinamik kod oluşturma

İPhone ait çekirdek kodu dinamik olarak oluşturma bir uygulamanın engeller beri iPhone Mono herhangi bir dinamik kod oluşturma biçimi desteklemiyor. Bu güncelleştirmeler şunlardır:

-  System.Reflection.Emit kullanılabilir değil.
-  System.Runtime.Remoting için destek yok.
-  Dinamik olarak türleri oluşturmak için destek yok (hiçbir aktarýldýðý ("MyType ' 1")), mevcut türlerini (aktarýldýðý ("System.String") Örneğin, yalnızca iyi çalışır) arama rağmen. 
-  Geriye doğru geri aramalar derleme zamanında çalışma zamanı ile kayıtlı olması gerekir.


 
 <a name="System.Reflection.Emit" />


### <a name="systemreflectionemit"></a>System.Reflection.Emit

System.Reflection eksiği. **Yayma** üzerinde çalışma zamanı kodu oluşturma bağımlı olan hiçbir kod çalışacağı anlamına gelir. Bu gibi şeyleri içerir:

-  Dinamik dil çalışma zamanı.
-  Dinamik dil çalışma zamanı en üstünde oluşturulmuş tüm diller.
-  Remoting'ın TransparentProxy veya başka bir şey kodu dinamik olarak oluşturmak çalışma zamanı neden olacak. 


 **Önemli:** aynı şey **Reflection.Emit** ile **yansıma**. Reflection.Emit kodu dinamik olarak oluşturma hakkında ve bu kodu JITed ve yerel için derlenmiş kod sahip. İPhone (JIT derleme) sınırlamaları nedeniyle bu desteklenmiyor.

Ancak tüm yansıma özellikleri, öznitelikleri ve değerleri getirme listeleniyor yöntemleri, listeleme aktarýldýðý ("someClass") dahil olmak üzere API düzgün çalışır.

### <a name="using-delegates-to-call-native-functions"></a>Yerel işlevleri çağırmak için kullanma

Bir C# temsilci aracılığıyla yerel bir işlevi çağırmak için temsilcinin bildirimi aşağıdaki öznitelikler biri ile donatılmış gerekir:

- [UnmanagedFunctionPointerAttribute](https://developer.xamarin.com/api/type/System.Runtime.InteropServices.UnmanagedFunctionPointerAttribute/) (platformlar arası ve .NET standart 1.1 + ile uyumlu olduğundan, tercih edilen)
- [MonoNativeFunctionWrapperAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoNativeFunctionWrapperAttribute)

Bu öznitelikler birini sağlamak başarısız bir çalışma zamanı hatası gibi neden olur:

```
System.ExecutionEngineException: Attempting to JIT compile method '(wrapper managed-to-native) YourClass/YourDelegate:wrapper_aot_native(object,intptr,intptr)' while running in aot-only mode.
```
 
 <a name="Reverse_Callbacks" />


### <a name="reverse-callbacks"></a>Geri aramalar ters çevir

İçinde standart Mono geçişi C# temsilci örneklerinin bir işlev işaretçisi yerine yönetilmeyen kod için mümkündür. Çalışma zamanı genellikle bu işlev işaretçileri geri yönetilen koda çağrı yönetimsiz kod sağlayan küçük bir dönüştürücü dönüştürmek.

Mono olarak bu köprüleri sadece zaman tarafından uygulanan derleyici. Ne zaman, saat tamamlanan derleyici kullanarak tarafından iPhone iki önemli sınırlamalar vardır, bu aşamada gerekli:

-  Tüm geri çağırma yöntemleriyle bayrak [MonoPInvokeCallbackAttribute](https://developer.xamarin.com/api/type/ObjCRuntime.MonoPInvokeCallbackAttribute) 
-  Yöntem statik yöntemler olması, desteği yoktur örneği için yöntemleri. 
 
<a name="No_Remoting" />

## <a name="no-remoting"></a>Hiçbir uzaktan iletişim

Uzaktan iletişim yığını Xamarin.iOS üzerinde kullanılabilir değil.


 <a name="Runtime_Disabled_Features" />


## <a name="runtime-disabled-features"></a>Çalışma zamanı özellikleri devre dışı

Mono'nın iOS çalışma zamanı aşağıdaki özellikler devre dışı bırakıldı:

-  Profil Oluşturucu
-  Reflection.Emit
-  Reflection.Emit.Save işlevi
-  COM bağlamaları
-  JIT altyapısı
-  (Hiçbir JIT olmadığından) meta veri doğrulama


 <a name=".NET_API_Limitations" />


## <a name="net-api-limitations"></a>.NET API sınırlamaları

Her şeyin iOS kullanılabilir olduğu gibi kullanıma sunulan .NET API tam framework'ün bir alt kümesidir. İçin SSS Bölümüne bakın bir [şu anda desteklenen bir derleme listesi](~/cross-platform/internals/available-assemblies.md).



Özellikle, çalışma zamanı davranışını yapılandırmak için dış XML dosyalarını kullanmak mümkün değildir Xamarin.iOS tarafından kullanılan API profilini System.Configuration, içermez.
