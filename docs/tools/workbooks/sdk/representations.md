---
title: Xamarin çalışma kitaplarındaki temsili
description: Bu belgede bir değer döndürür herhangi bir kod için zengin sonuçları çizmeye sağlayan Xamarin çalışma kitaplarını gösterimi ardışık düzen açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: d4d8fa164b9f52e2c5331aa2c08fdddf232572d4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794169"
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin çalışma kitaplarındaki temsili

## <a name="representations"></a>temsili

Çalışma kitabı veya Inspector oturum içinde yürütülür ve bir sonuç (örn. bir değer veya ifade sonuç döndüren bir yöntem) verir kodu aracısındaki gösterimi ardışık düzeninden işlenir. Tamsayıları gibi temelleri hariç olmak üzere tüm nesneleri etkileşimli üye grafikleri üretmek için yansıtılır ve istemci daha zengin işleyebilen alternatif Beyanları sağlamak için bir sürecinden geçerler. Tüm boyutu ve derinliği nesnelerin güvenle (döngüleri ve sonsuz enumerables de dahil olmak üzere) yavaş ve etkileşimli yansıma ve Uzaktan iletişim nedeniyle desteklenir.

Xamarin çalışma kitapları, tüm aracılar ve sonuçları zengin işleme için izin istemcilere ortak birkaç türler sağlar. [`Color`][xir-color] Bu tür bir türünün bir örneği iOS örneğin aracı dönüştürmek için sorumlu olduğu `CGColor` veya `UIColor` içine nesneleri bir `Xamarin.Interactive.Representations.Color` nesnesi.

Ortak Beyanları yanı sıra, SDK tümleştirmesi aracısındaki özel Beyanları seri hale getirme ve istemci Beyanları işleme için API'ler sağlar.

## <a name="external-representations"></a>Dış temsili

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] kaydetme yeteneği sağlar bir [`RepresentationProvider`][repp], hangi rasgele bir nesneden işlemek için belirsiz bir forma dönüştürmek için bir tümleştirme uygulamalıdır. Bu belirsiz formlar uygulamalıdır [ `ISerializableObject` ] [ serobj] arabirimi.

Uygulama `ISerializableObject` arabirimi tam olarak nasıl nesneleri serileştirilir denetleyen serileştirme yöntemi ekler. `Serialize` Yönteminin beklediği bir geliştirici tam olarak hangi özelliklerdir serileştirilmesi için ve son adı ne olacağını belirteceksiniz. Bakarak `Person` nesnesinde bizim [`KitchenSink` örnek] [örnek], biz nasıl işlediğine görebilirsiniz:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Biz bir üst veya alt kümesini özgün nesne özelliklerinden sağlamak istiyorsanız, ile yapabiliriz `Serialize`. Örneğin, biz şuna önceden hesaplanan sağlar yapmak `Age` özellikte `Person`:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> Üretmek API'leri `ISerializableObject` nesneleri doğrudan gerek kalmaz tarafından işlenecek bir `RepresentationProvider`. Görüntülemek istediğiniz nesne ise **değil** bir `ISerializableObject`, içinde kaydırma işlemek istersiniz, `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Bir gösterimi işleme

Oluşturucu JavaScript'te uygulanır ve aracılığıyla temsil edilen nesne JavaScript sürümü erişebilir `ISerializableObject`. JavaScript kopya da sahip bir `$type` dize .NET türü adı gösteren özellik.

Elbette vanilla JavaScript derler istemci tümleştirme kodu için TypeScript kullanmanızı öneririz. Her iki durumda da SDK sağlar [typings][typings] hangi bulunabilir doğrudan TypeScript tarafından başvurulan veya yalnızca el ile JavaScript, tercih edilen vanilla yazarken göstermektedir.

İşleme için ana tümleştirme noktasıdır `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Burada, `PersonRenderer` uygulayan `Renderer` arabirimi. Bkz: [typings] [ typings] daha fazla ayrıntı için.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
