---
title: Özel oluşturucular giriş
description: Bu makalede, özel Oluşturucu tanıtır ve özel Oluşturucu oluşturma işlemi açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 180a196e0b95854815c8a74ef1d2df63407dd04f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998010"
---
# <a name="introduction-to-custom-renderers"></a>Özel oluşturucular giriş

_Özel oluşturucular Xamarin.Forms denetimleri davranışını ve görünümünü özelleştirmek için güçlü bir yaklaşım sağlar. Bunlar küçük stil değişikliklerini veya karmaşık platforma özel düzen ve davranışını özelleştirme için kullanılabilir. Bu makalede, özel Oluşturucu tanıtır ve özel Oluşturucu oluşturma işlemi açıklanmaktadır._

Xamarin.Forms [sayfaları, düzenler ve denetimleri](~/xamarin-forms/user-interface/controls/index.md) platformlar arası mobil kullanıcı arabirimlerini tanımlamak için ortak bir API sunar. Her platformda farklı işlenen her sayfa, Düzen ve denetimi kullanarak bir `Renderer` sırayla (Xamarin.Forms gösterimi için karşılık gelen), yerel bir denetim oluşturan sınıf ekranda düzenler ve belirtilen davranışı ekler Paylaşılan kod.

Geliştiriciler, kendi özel uygulayabilirsiniz `Renderer` sınıflar görünümünü ve davranışını özelleştirin. Özel oluşturucular verilen tür için varsayılan davranışı diğer platformlarda izin verirken tek bir yerde denetimi özelleştirmek için bir uygulama projesi eklenebilir; veya farklı özel oluşturucular, iOS, Android ve evrensel Windows Platformu (UWP) farklı bir görünüm oluşturmak için her bir uygulama projesine eklenebilir. Ancak, bir basit denetimi özelleştirme gerçekleştirmek için bir özel Oluşturucu sınıf uygulama genellikle ağır yanıt olur. Etkiler, bu işlemi basitleştireceksiniz ve genellikle küçük stil değişiklikler için kullanılır. Daha fazla bilgi için [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md).

## <a name="examining-why-custom-renderers-are-necessary"></a>İnceleniyor neden özel Oluşturucu gerekli olan

Xamarin.Forms denetiminin görünümünü değiştirme, özel bir oluşturucu kullanmadan sınıflara ve ardından özgün denetimin yerine özel denetim tüketen özel bir denetim oluşturulamaz içeren iki adımlı bir işlemdir. Aşağıdaki kod örneği, sınıflara örneği gösterir `Entry` denetimi:

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` Denetimi bir `Entry` denetim nerede `BackgroundColor` ayarlamak gri ve XAML'de konumu için bir ad alanı bildirmek ve ad alanı öneki üzerinde denetim öğesi kullanılarak başvurulabilir. Aşağıdaki kod örnekte gösterildiği nasıl `MyEntry` özel denetim tarafından kullanılabilir bir `ContentPage`:

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

`local` Ad alanı öneki her şey olabilir. Ancak, `namespace` ve `assembly` değerleri, özel denetimin ayrıntılarını eşleşmesi gerekir. Ad alanı bildirildiğinde, ön ek özel denetim başvurusu yapmak için kullanılır.

> [!NOTE]
> Tanımlama `xmlns` paylaşılan projeler .NET Standard Kitaplığı projelerinde çok daha kolaydır. Ne olacağını belirlemek kolay, bu nedenle .NET Standard kitaplığı bütünleştirilmiş kod içine derlenmiş `assembly=CustomRenderer` değeri olmalıdır. Paylaşılan projeler kullanırken (XAML dahil) paylaşılan tüm varlıklara her olması durumunda iOS, Android ve UWP anlamına gelir başvuran projeler, derlenen projeler sahip kendi *derleme adları* yazmak için mümkün değildir `xmlns` bildirimi değeri her uygulama için farklı olması gerektiğinden. Paylaşılan projeleri için XAML özel denetimlerinde her uygulama projesi, aynı derleme adı ile yapılandırılmasını gerektirir.

`MyEntry` Özel denetim ardından işlenen her platformda gri renkli bir arka plan ile aşağıdaki ekran görüntülerinde gösterildiği gibi:

![](introduction-images/screenshots.png "Her platformda MyEntry özel denetim")

Her platformda denetimin arka plan rengini değiştirme denetimini yalnızca alt sınıf yapma aracılığıyla gerçekleştirilir. Ancak, bu teknik, platforma özgü geliştirmeler ve özelleştirmeler yararlanmak mümkün olmadığından ne bunu başarabilirsiniz içinde sınırlıdır. Özel oluşturucular, bunlar gerekli olduğunda uygulanmalıdır.

## <a name="creating-a-custom-renderer-class"></a>Özel oluşturucu sınıfı oluşturma

Özel oluşturucu sınıfı oluşturma işlemi aşağıdaki gibidir:

1. Yerel kontrolünü icra eden Oluşturucu sınıfının oluşturun.
1. Yerel kontrolünü icra eden yöntemi yok sayın ve mantık denetimi özelleştirmek için yazma. Genellikle, `OnElementChanged` yöntemi, karşılık gelen Xamarin.Forms Denetim oluşturulurken çağrılan yerel denetimi oluşturmak için kullanılır.
1. Ekleme bir `ExportRenderer` bunu Xamarin.Forms denetimi oluşturmak için kullanılacak belirtmek için özel Oluşturucu sınıfı özniteliği. Bu öznitelik, özel Oluşturucu Xamarin.Forms ile kaydetmek için kullanılır.

> [!NOTE]
> Xamarin.Forms öğelerin çoğu, her platform projesinde özel bir oluşturucu sağlamak isteğe bağlıdır. Özel oluşturucu kayıtlı değilse denetimin taban sınıfı için varsayılan oluşturucu kullanılır. Ancak, özel Oluşturucu her platform projesinde işlenirken gereken bir [görünümü](xref:Xamarin.Forms.View) veya [Viewcell'i](xref:Xamarin.Forms.ViewCell) öğesi.

Tanıtımlar ve bu işlem açıklamalarını farklı Xamarin.Forms öğeleri için bu bölümdeki konular sağlar.

## <a name="troubleshooting"></a>Sorun giderme

Özel denetim bulunan bir özel durum (Mac/Visual Studio Xamarin.Forms uygulaması proje şablonu için Visual Studio tarafından oluşturulan yani .NET standart kitaplıkta değil), çözüme eklenmiş olan bir .NET Standard kitaplığı projesi iOS ortaya çıkabilir, özel denetim erişmeye çalışıyor. Bu sorun çözümleyen özel denetimden başvuru oluşturarak ortaya çıkarsa `AppDelegate` sınıfı:

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

Bu derleyici tanımak için zorlar `ClassInPCL` çözüm tarafından türü. Alternatif olarak, `Preserve` özniteliği eklenebilir `AppDelegate` sınıfı aynı sonucu elde etmek için:

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

Bu başvuru oluşturur `ClassInPCL` çalışma zamanında zorunludur gösteren türü. Daha fazla bilgi için [koruma kod](~/ios/deploy-test/linker.md).

## <a name="summary"></a>Özet

Bu makalede, özel Oluşturucu giriş ayarının ve özel Oluşturucu oluşturma süreci ana hatlarıyla belirtilen. Özel oluşturucular Xamarin.Forms denetimleri davranışını ve görünümünü özelleştirmek için güçlü bir yaklaşım sağlar. Bunlar küçük stil değişikliklerini veya karmaşık platforma özel düzen ve davranışını özelleştirme için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [Etkiler](~/xamarin-forms/app-fundamentals/effects/index.md)
