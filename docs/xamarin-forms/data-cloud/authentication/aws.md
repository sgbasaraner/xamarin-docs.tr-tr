---
title: Kullanıcıların bir Amazon SimpleDB hizmeti ile kimlik doğrulaması
description: Amazon SimpleDB kendi kaynak tabanlı izinler sistem sağlamaz. Bunun yerine, bir kimlik sağlayıcısı kimlik doğrulaması, kullanıcıların yalnızca kendi verilerine erişim SimpleDB etki alanında olmasını sağlamak için kullanılabilir. Bu makalede, kullanıcıların kendi SimpleDB veriye erişimi kısıtlamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 592e957e0c64e7189d6f01f1ba0f23da074c4bec
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="authenticating-users-with-an-amazon-simpledb-service"></a>Kullanıcıların bir Amazon SimpleDB hizmeti ile kimlik doğrulaması

_Amazon SimpleDB kendi kaynak tabanlı izinler sistem sağlamaz. Bunun yerine, bir kimlik sağlayıcısı kimlik doğrulaması, kullanıcıların yalnızca kendi verilerine erişim SimpleDB etki alanında olmasını sağlamak için kullanılabilir. Bu makalede, kullanıcıların kendi SimpleDB veriye erişimi kısıtlamak açıklanmaktadır._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) örnek uygulama kullanıcıların hesap ayrıntıları cihazda güvenli bir şekilde depolamak ve kullanıcı kimlik doğrulama işlemi yönetmek için kullanılır. Daha fazla bilgi için bkz: [bir kimlik sağlayıcısı ile kimlik doğrulaması yapan kullanıcıların](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="allowing-an-authenticated-user-access-to-simpledb-domain-data"></a>SimpleDB etki alanı verileri bir kimliği doğrulanmış kullanıcı erişimine izin verme

Örnek uygulama kullanır `TodoItem` model verileri için sınıf. Depolamak için bir `TodoItem` , ilk dönüştürülmesi gerekir içine SimpleDB service örneğinde bir `List` , `ReplaceableAttribute` nesneleri. Daha fazla bilgi için bkz: [oluşturma SimpleDB nesneleri](~/xamarin-forms/data-cloud/consuming/aws.md).

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

Kullanıcıların SimpleDB etki alanında yalnızca kendi verilere erişimi olmasını sağlamak için `ToSimpleDBReplaceableAttributes` yöntemi için ek öznitelik depolayan bir `TodoItem` , aşağıdaki kod örneğinde gösterildiği gibi örneği:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

Bu öznitelik her öğe SimpleDB etki alanında depolanan verileri ait kullanıcıyı benzersiz şekilde tanımlamak için kullanılan bir kullanıcı için bir ilişkili e-posta adresini sahip olmasını sağlar. Ne zaman bir etki alanı içeriğini alınır çağırarak `AmazonSimpleDBClient.SelectAsync` yöntemi, sorgu ifadesi sağlar yalnızca öğeleri kimliği doğrulanmış kullanıcı için olduğunu alınır, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

`SelectAsync` Yöntemi içeren bir koleksiyon sorgu ifadesi eşleşen ilişkili öznitelikleri ve öğelerinin bir yanıt döndürür. Sorgu ifadesi, yalnızca kullanıcının e-posta adresiyle eşleşen öğeleri alınmasını sağlar. Sorgu ifadeleri hakkında daha fazla bilgi için bkz: [kullanarak seçin Amazon SimpleDB sorguları oluşturmak için](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) Amazon'ın Web sitesinde.

> [!NOTE]
> Teklif sorgu ifadesi oluşturulurken kurallarına dikkat edin. Daha fazla bilgi için bkz: [tırnak içine almak kuralları seçin](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) Amazon'ın Web sitesinde.

## <a name="summary"></a>Özet

Bu makalede, kullanıcıların kendi SimpleDB veriye erişimi kısıtlamak nasıl açıklanmıştır. Amazon SimpleDB kendi kaynak tabanlı izinler sistem sağlamaz. Bunun yerine, bir kimlik sağlayıcısı kimlik doğrulaması, kullanıcıların SimpleDB etki alanında yalnızca kendi verilere erişimi olmasını sağlamak için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoAWSAuth (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Bir Amazon SimpleDB hizmetini kullanma](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Bir kimlik sağlayıcısı ile kullanıcıların kimlik doğrulaması](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Amazon Cognito kimliği](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB Geliştirici belgeleri](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [.NET için Amazon Web Hizmetleri SDK'sı](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
