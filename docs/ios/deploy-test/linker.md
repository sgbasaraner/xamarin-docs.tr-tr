---
title: İos'ta bağlama
ms.prod: xamarin
ms.assetid: 3A4B2178-F264-0E93-16D1-8C63C940B2F9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/24/2017
ms.openlocfilehash: 1d83a152c0949abe0221f6eb6dfb42f4e79eaf38
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="linking-on-ios"></a>İos'ta bağlama

Uygulamanızı oluştururken, Mac veya Visual Studio için Visual Studio adı verilen bir aracı çağırır **mtouch** , yönetilen kod için bir bağlayıcı içerir. Sınıf kitaplıklarından uygulama kullanmıyorsa özellikleri kaldırmak için kullanılır. Yalnızca gerekli bitle sevk uygulama boyutunu azaltmak için belirtilir.

Bağlayıcı statik çözümleme uygulamanızı izlemek için açıktır farklı kod yolları belirlemek için kullanır. Her bulunabilirlik hiçbir şey kaldırıldığından emin olmak için her derleme ayrıntı gitmek olduğu gibi bir bit ağır. Bu varsayılan simulator derlemeleri olarak hata ayıklama sırasında yapı süresini hızlandırmak için etkin değil. Daha küçük uygulamaları üreten beri ancak bu uygulama nesne AĞACI derleme ve aygıta karşıya yükleme hızlandırabilirsiniz tüm *aygıtları (sürüm) derlemeler* bağlayıcı varsayılan olarak kullanarak.

Bağlayıcı statik bir aracı gibi onu ekleme türler ve yansıma yoluyla adlı veya dinamik olarak örneği yöntemler için işaretleyebilirsiniz değil. Çeşitli seçenekler bu sınırlamaya geçici çözüm yok.

<a name="Linker_Behavior" />

## <a name="linker-behavior"></a>Bağlayıcı davranışı

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

Bağlama işlemini bağlayıcı davranışı açılır listede aracılığıyla özelleştirilebilir **proje seçenekleri**. İOS projede bu çift erişip Gözat **iOS Yapı > bağlayıcı seçenekleri**aşağıda gösterildiği gibi:

[![](linker-images/image1.png "Bağlayıcı seçenekleri")](linker-images/image1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Bağlama işlemini bağlayıcı davranışı açılır listede aracılığıyla özelleştirilebilir **proje özelliklerini** Visual Studio.

Aşağıdakileri yapın:

1. Sağ **proje adı** içinde **Çözüm Gezgini** seçip **özellikleri**:

    ![](linker-images/linking01w.png "Çözüm Gezgini'nde proje adına sağ tıklayın ve Özellikler'i seçin")
2. İçinde **proje özelliklerini**seçin **IOS yapı**:

    ![](linker-images/linking02w.png "Select IOS derleme")
3. Bağlama seçeneklerini değiştirmek için aşağıdaki yönergeleri izleyin.

-----

Üç ana seçenekler aşağıda açıklanmıştır sunulur:


### <a name="dont-link"></a>Bağlantı yok

Bağlamayı devre dışı bırakma hiçbir derlemeleri değiştirildiğini emin olmanızı sağlar. Performansı artırmak için IDE'yi iOS simülatörü hedeflediğinde varsayılan ayar budur. Bağlayıcı çalıştırmak için uygulamanızın engelleyen bir hata içeren her aygıtları derlemeler için bu yalnızca geçici bir çözüm olarak kullanılmalıdır. Uygulamanız ile yalnızca çalışıyorsa *- nolink*, lütfen gönderin bir [hata raporu](http://bugzilla.xamarin.com).

Bu karşılık *- nolink* komut satırı aracı mtouch kullanırken seçeneği.

<a name="Link_SDK_assemblies_only" />

### <a name="link-sdk-assemblies-only"></a>Yalnızca bağlantı SDK derlemeleri

Bu modda, bağlayıcı derlemeleriniz dokunmadan bırakır ve uygulamanızı kullanmayan her şeyi kaldırarak (yani ne Xamarin.iOS ile birlikte sağlanır) SDK derlemeleri boyutunu azaltır. IDE'yi iOS cihazları hedeflediğinde varsayılan ayar budur.

Kodunuzda herhangi bir değişiklik gerektirmez gibi en basit seçenek budur. Her şeyi bağlama ile böylece her şeyi ve son uygulama boyutu bağlanmak için gereken çalışma arasında bir denge bağlayıcı birkaç en iyi duruma getirme bu modda, gerçekleştirebileceğiniz değil farktır.

Bu karşılık *- linksdk* komut satırı aracı mtouch kullanırken seçeneği.

<a name="Link_all_assemblies" />

### <a name="link-all-assemblies"></a>Tüm derlemelerde bağlantı

Her şeyi bağlanırken bağlayıcı uygulamayı olabildiğince küçük yapmak için en iyi duruma getirme tam kümesini kullanabilirsiniz. Kod bağlayıcı'nın statik çözümleme algılayamaz bir biçimde özellikleri kullandığında, kesilebilir kullanıcı kodu değiştirecektir. Böyle durumlarda, örneğin webservices'a, yansıma ya da serileştirme, bazı adjustements her şeyi bağlamak için uygulamanızda gerekli olabilir.

Bu karşılık *- linkall* seçeneğini komut satırı aracını kullanarak **mtouch**.

<a name="Controlling_the_Linker" />

## <a name="controlling-the-linker"></a>Denetleme bağlayıcı

Bağlayıcı kullandığınızda, bazen aradığınız dinamik olarak, hatta dolaylı olarak kod kaldıracak. Bu gibi durumlarda kapsayacak şekilde bağlayıcı birkaç yeni özellik ve kendi eylemler hakkında daha fazla denetime izin vermek için seçenekler sağlar.

<a name="Preserving_Code" />

### <a name="preserving-code"></a>Kod koruma

Bağlayıcı kullandığınızda bazı durumlarda, dinamik olarak çağrılır kod kaldırabilirsiniz System.Reflection.MemberInfo.Invoke kullanarak ya da göre dışarı aktarma yöntemlerinizi Objective-C kullanmaya `[Export]` özniteliği ve Seçici el ile çağırma .

Bu gibi durumlarda uygulayarak korunması kullanılmış veya tek tek üyeleri olması ya da tüm sınıflar dikkate alınması gereken bağlayıcı söyleyebilirsiniz `[Xamarin.iOS.Foundation.Preserve]` ya da sınıf düzeyinde veya üye düzeyi özniteliği. Uygulama tarafından statik olarak bağlantılı olmayan her üye tabidir kaldırılır. Bu öznitelik, bu nedenle değil, statik olarak başvurulur, ancak, uygulamanız tarafından hala gerekli olduğunu üyeleri işaretlemek için kullanılır.

Örneği için dinamik olarak türleri örneği varsa, türlerinin varsayılan oluşturucu korumak isteyebilirsiniz. XML serileştirme kullanırsanız, türlerinizi özelliklerini korumak isteyebilirsiniz.

Bu öznitelik her üye bir türde veya türü uygulayabilirsiniz. Tüm türü korumak istiyorsanız, söz dizimini kullanabilirsiniz `[Preserve
(AllMembers = true)]` türünde.

Bazen, kapsayan tür korunur, ancak yalnızca belirli üyeleri korumak istediğiniz. Bu gibi durumlarda kullanın `[Preserve (Conditional=true)]`

Xamarin kitaplıkları'nı bir bağımlılık almak istemiyorsanız-Örneğin, deyin platformlar arası taşınabilir sınıf kitaplığı (PCL) oluşturmakta olduğunuz - bu öznitelik kullanmaya devam edebilirsiniz.

Bunu yapmak için yalnızca bir PreserveAttribute sınıf bildirme ve gerekir şöyle kodunuzda kullanın:

```csharp
public sealed class PreserveAttribute : System.Attribute {
    public bool AllMembers;
    public bool Conditional;
}
```

Bu gerçekten bu hangi ad alanında tanımlı önemli değildir, bağlayıcı bu öznitelik türü ada göre arar.

 <a name="Skipping_Assemblies" />

### <a name="skipping-assemblies"></a>Derlemeleri atlama

Normal olarak bağlantılı diğer derlemelerden izin verirken bağlayıcı işleminden tutulması gereken derlemeleri belirlemek mümkündür. Bu kullanıyorsanız yararlıdır `[Preserve]` bazı derlemelerini mümkün değildir (örneğin taraf kodu 3) veya bir hata için geçici bir çözüm olarak.

Bu karşılık `--linkskip` komut satırı aracı mtouch kullanırken seçeneği.

Kullanırken **bağlantı tüm derlemelerde** seçeneği tüm derlemeleri atlamak için aşağıdaki koyun bağlayıcı bildirmek istiyorsanız **ek mtouch bağımsız değişkenleri** en üst düzey derlemenizi seçenekleri:

```csharp
--linkskip=NameOfAssemblyToSkipWithoutFileExtension
```

Birden çok derleme atlamak için bağlayıcı istiyorsanız, birden çok dahil `linkskip` bağımsız değişkenler:

```csharp
--linkskip=NameOfFirstAssembly --linkskip=NameOfSecondAssembly
```

Bu seçeneği kullanmak için kullanıcı arabirimi yoktur, ancak bunu Visual Studio'da Mac proje Seçenekleri iletişim kutusunu veya Visual Studio Proje Özellikler bölmesi için içinde sağlanabilir **ek mtouch bağımsız değişkenleri** metin alanı. (Örn. *--linkskip mscorlib =* mscorlib.dll bağlamayan ancak diğer derlemelerden çözümüne bağlayabilirsiniz).

<a name="Disabling_Link_Away" />

### <a name="disabling-link-away"></a>"Hemen bağlantı" devre dışı bırakma

Bağlayıcı aygıtlarda kullanılan, örneğin desteklenmeyen veya izin verilmeyen son derece düşüktür kodu kaldırır. Nadir durumlarda bir uygulama ya da kitaplık bu (veya çalışma) koduna bağlıdır mümkündür. Xamarin.iOS 5.0.1 itibaren bu en iyi duruma getirme atlamak için bağlayıcı sağlanabilir.

Bu karşılık *- nolinkaway* komut satırı aracı mtouch kullanırken seçeneği.

Bu seçeneği kullanmak için kullanıcı arabirimi yoktur, ancak bunu Visual Studio'da Mac proje Seçenekleri iletişim kutusunu veya Visual Studio Proje Özellikler bölmesi için içinde sağlanabilir **ek mtouch bağımsız değişkenleri** metin alanı. (Örn. *--nolinkaway* ek kod (yaklaşık 100 kb) kaldırmamanız).

### <a name="marking-your-assembly-as-linker-ready"></a>Derleme Bağlayıcı için hazır olarak işaretleme

Kullanıcılar yalnızca SDK derlemeleri bağlamak ve kendi kod bağlama yapmak için seçebilirsiniz.  Bu aynı zamanda SDK bağlanmaz Xamarin'ın çekirdek parçası olmayan herhangi bir üçüncü taraf kitaplıklar anlamına gelir.

Bu genellikle, el ile eklemek istediğiniz değil çünkü gerçekleşir `[Preserve]` öznitelikleri kendi kodu.  Bir üçüncü taraf olup olmadığını kitaplığı bağlayıcı kolay olup olmadığını öğrenmek mümkün değildir yan etkisi üçüncü taraf kitaplıklar bağlanamaz ve bu genel bir iyi varsayılan seçenektir aynıdır.

Projenizde, kitaplığınız veya yeniden kullanılabilir kitaplıkların Geliştiriciyseniz ve derlemenizi linkable olarak işlemek için bağlayıcı istediğiniz tüm yapmanız gereken varsa, derleme düzeyi özniteliğini ekleyin [ `LinkerSafe` ](https://developer.xamarin.com/api/type/Foundation.LinkerSafeAttribute/), şöyle:

```csharp
[assembly:LinkerSafe]
```

Kitaplığınızı gerçekten Xamarin bu kitaplıklarına gerekmez.  Örneğin, diğer platformlar çalışacak bir taşınabilir sınıf kitaplığı oluşturuluyorsa kullanmaya devam edebilirsiniz bir `LinkerSafe` özniteliği.
Xamarin bağlayıcı arayan `LinkerSafe` özniteliği ada göre gerçek tür tarafından değil.  Bunun anlamı, bu kod yazabilirsiniz ve ayrıca çalışır:

```csharp
[assembly:LinkerSafe]
// ... assembly attribute should be at top, before source
class LinkerSafeAttribute : System.Attribute {
    public LinkerSafeAttribute : System.base {}
}
```

## <a name="custom-linker-configuration"></a>Özel Bağlayıcı yapılandırması

İzleyin [bağlayıcı yapılandırma dosyası oluşturma için yönergeler](~/cross-platform/deploy-test/linker.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Özel Bağlayıcı Yapılandırması](~/cross-platform/deploy-test/linker.md)
- [Mac bağlama](~/mac/deploy-test/linker.md)
- [Android bağlama](~/android/deploy-test/linker.md)
