---
title: "Özel oluşturucu giriş"
description: "Özel oluşturucu görünümünü ve Xamarin.Forms denetimleri davranışını özelleştirmek için güçlü bir yaklaşım sağlayın. Bunlar küçük stil değişiklikleri veya karmaşık platforma özgü düzeni ve davranışını özelleştirme için kullanılabilir. Bu makalede özel Oluşturucu bir giriş sağlar ve özel Oluşturucu oluşturma süreci özetlenmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: a9908429994f4575a9e41936d500bfd8906a843b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-custom-renderers"></a>Özel oluşturucu giriş

_Özel oluşturucu görünümünü ve Xamarin.Forms denetimleri davranışını özelleştirmek için güçlü bir yaklaşım sağlayın. Bunlar küçük stil değişiklikleri veya karmaşık platforma özgü düzeni ve davranışını özelleştirme için kullanılabilir. Bu makalede özel Oluşturucu bir giriş sağlar ve özel Oluşturucu oluşturma süreci özetlenmektedir._

Xamarin.Forms [sayfalar, Düzen ve denetimlerin](~/xamarin-forms/user-interface/controls/index.md) platformlar arası mobil kullanıcı arabirimleri açıklamak için ortak bir API sunar. Her platformda farklı işlenen her sayfası, Düzen ve denetim kullanarak bir `Renderer` sırayla (Xamarin.Forms gösterimine karşılık gelen), yerel bir denetimi oluşturur sınıfı ekranda düzenler ve belirtilen davranışı ekler Paylaşılan kod.

Geliştiriciler kendi özel uygulayabilirsiniz `Renderer` görünümünü ve/veya Denetim davranışını özelleştirmek için sınıflar. Özel oluşturucu için belirli bir türde tek bir yerde denetim diğer platformlarda varsayılan davranışı verirken özelleştirmek için bir uygulama projesi eklenebilir; ya da farklı özel Oluşturucu iOS, Android ve Windows Phone farklı bir görünüm oluşturmak için her uygulama projesi eklenebilir. Ancak, bir basit denetim özelleştirme gerçekleştirmek için özel Oluşturucu sınıfı uygulama ağır yanıt görülür. Etkiler, bu işlemi basitleştirmek ve genellikle küçük stil değişiklikler için kullanılır. Daha fazla bilgi için bkz: [efektler](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>İnceleniyor neden özel Oluşturucu gerekli olan

Xamarin.Forms denetiminin görünümünü değiştirme, özel bir oluşturucu kullanmadan sınıflara ve özgün denetim yerine özel denetim tüketen aracılığıyla özel bir denetim oluşturma içeren iki adımlı bir işlemdir. Aşağıdaki kod örneğinde sınıflara örneği gösterilmektedir `Entry` denetimi:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` Denetimi bir `Entry` kontrol nereye `BackgroundColor` ayarlamak gri ve XAML'de konumu için bir ad alanı bildirme ve denetim öğede ad alanı öneki kullanarak başvurulabilir. Aşağıdaki örnekte gösterildiği kod nasıl `MyEntry` özel denetim tüketilen tarafından bir `ContentPage`:

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

`local` Ad alanı öneki herhangi bir şey olabilir. Ancak, `namespace` ve `assembly` değerlerin özel denetim ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde öneki özel denetim başvurmak için kullanılır.

> [!NOTE]
> **Not**: tanımlama `xmlns` paylaşılan projeleri PCLs içinde çok daha kolaydır. Belirlemek kolay olacak şekilde bir PCL bütünleştirilmiş koda derlenmemiş `assembly=CustomRenderer` değeri olmalıdır. Paylaşılan projeleri kullanırken (XAML dahil) tüm paylaşılan varlıklar her olması durumunda iOS, Android ve Windows Phone anlamına gelir başvuran projeler derlenen projeleri sahip kendi *derleme adları* mümkün değildir çok yazmak için `xmlns` bildirimi değeri her uygulama için farklı olması gerektiğinden. Paylaşılan projeleri için XAML içinde özel denetimler her uygulama projesi aynı derleme adı ile yapılandırılmasını gerektirir.

`MyEntry` Özel denetim sonra işlenen her platformda gri bir arka planda aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](introduction-images/screenshots.png "Her platformda MyEntry özel denetimi")

Her platformda denetim arka plan rengini değiştirme zamanıyla ilgili denetimi sınıflara aracılığıyla gerçekleştirilir. Ancak, bu teknik platforma özgü geliştirmeler ve özelleştirmeler yararlanmak olası olmadığından ne bunu elde edebilirsiniz içinde sınırlıdır. Gerekli olduğunda özel Oluşturucu uygulanmalıdır.

## <a name="creating-a-custom-renderer-class"></a>Özel oluşturucu sınıfı oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Yerel denetimini işler Oluşturucu sınıfının oluşturun.
1. Yerel denetimini işler yöntemini geçersiz kılın ve denetimi özelleştirmek için mantığı yazma. Genellikle, `OnElementChanged` yöntemi karşılık gelen Xamarin.Forms Denetim oluşturulduğunda çağrılan yerel denetimi oluşturmak için kullanılır.
1. Ekleme bir `ExportRenderer` özniteliği, Xamarin.Forms denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> **Not**: çoğu Xamarin.Forms öğeleri için her platform projesinde özel Oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse, varsayılan oluşturucu denetimin taban sınıfı için kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [Görünüm](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) veya [ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) öğesi.

Bu bölümdeki konular gösterileri ve farklı Xamarin.Forms öğeler için bu işlemi açıklamalar sağlar.

## <a name="troubleshooting"></a>Sorun giderme

Özel denetimi (Mac/Visual Studio Xamarin.Forms uygulaması proje şablonu için Visual Studio tarafından oluşturulan yani değil PCL) çözümüne eklenmiş bir PCL projesinde içeriyorsa, iOS özel denetim erişilmeye çalışılırken bir özel durum oluşabilir. Bunu çözülmüş özel denetimden başvuru oluşturarak bu sorun ortaya çıkarsa `AppDelegate` sınıfı:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Bu tanımak için derleyici zorlar `ClassInPCL` çözüm tarafından türü. Alternatif olarak, `Preserve` özniteliği eklenebilir `AppDelegate` sınıfı aynı sonucu elde etmek için:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Bu başvuru oluşturur `ClassInPCL` çalışma zamanında istedi belirten türü. Daha fazla bilgi için bkz: [koruma kod](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Özet

Bu makalede, özel Oluşturucu giriş sağladı ve özel Oluşturucu oluşturma süreci ana hatlarıyla. Özel oluşturucu görünümünü ve Xamarin.Forms denetimleri davranışını özelleştirmek için güçlü bir yaklaşım sağlayın. Bunlar küçük stil değişiklikleri veya karmaşık platforma özgü düzeni ve davranışını özelleştirme için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Etkileri](~/xamarin-forms/app-fundamentals/effects/index.md)
