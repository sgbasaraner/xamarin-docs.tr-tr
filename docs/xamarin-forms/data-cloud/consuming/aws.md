---
title: Bir Amazon SimpleDB hizmetini kullanma
description: "Amazon SimpleDB depolamak ve Amazon'ın bulut verilerde sorgu olanağı sağlayan bir web hizmetidir. Bu makalede, .NET için AWS SDK'sı sorgu, oluşturmak, değiştirmek ve SimpleDB hizmetinde depolanan verileri silmek için nasıl kullanılacağı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 823819AA-15F9-4144-B355-78A10AD37513
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 590e39deb7972df9e45064bb1a96e533a1fc9856
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="consuming-an-amazon-simpledb-service"></a>Bir Amazon SimpleDB hizmetini kullanma

_Amazon SimpleDB depolamak ve Amazon'ın bulut verilerde sorgu olanağı sağlayan bir web hizmetidir. Bu makalede, .NET için AWS SDK'sı sorgu, oluşturmak, değiştirmek ve SimpleDB hizmetinde depolanan verileri silmek için nasıl kullanılacağı açıklanmaktadır._

SimpleDB Hizmetleri REST Hizmetleri tüketicileri için tanıdık istek ve yanıt model kullanın. İşlem verileri içerebilir bir istek göndererek SimpleDB hizmette çağrılır. İstek işlendikten sonra SimpleDB hizmeti herhangi bir sonuç içeren bir yanıt döndürür. SimpleDB hizmeti programlı olarak oluşturulmuş olması gerekir ve aracılığıyla oluşturulamıyor [AWS Konsolu](https://aws.amazon.com). Ancak, bir AWS hesabı oluşturmak ve herhangi bir Amazon web hizmeti erişmek için gereklidir.

SimpleDB hizmetinde veri içinde hangi veri yerleştirilebilir ve veri karşı sorguları çalıştırmak etki alanları, halinde düzenlenir. Etki alanları özniteliği ad-değer çiftleri tarafından açıklanan öğeleri oluşur. Etki alanları, sütunları ve satırları benzer olan öğeleri benzer olan öznitelikleri olan tablolara, benzer olduğu düşünülebilir. SimpleDB veri modeli hakkında daha fazla bilgi için bkz: [veri modeli](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/DataModel.html) Amazon'ın Web sitesinde.

Gerekli Amazon hizmetler ayarlama yönergeleri örnek uygulama eşlik eden readme dosyasında bulunabilir. Örnek uygulamayı çalıştırdığınızda, onu bir Amazon Cognito kimlik havuzu SimpleDB hizmet erişim yetkisi vermek için aşağıdaki ekran görüntüsünde gösterildiği gibi bağlanır:

![](aws-images/portal.png "Örnek uygulama")

> [!NOTE]
> İOS 9 ve daha sonraki sürümleri, böylece hassas bilgilerin yanlışlıkla açığa önleme uygulama Aktarım güvenlik (ATS) Internet kaynakların (örneğin, uygulamanızın arka uç sunucusu) ve uygulama arasında güvenli bağlantı zorlar. ATS uygulamaları iOS 9 yerleşik varsayılan olarak etkin olduğundan, tüm bağlantıları ATS güvenlik gereksinimlerini tabi olacaktır. Bağlantıları bu gereksinimlerini karşılamıyorsa, bir özel durum ile başarısız olur.
> ATS kabul dışı kullanmak mümkün değilse `HTTPS` protokolü ve güvenli iletişim Internet kaynakların. Bu uygulamanın güncelleştirerek sağlanabilir **Info.plist** dosya. Daha fazla bilgi için bkz: [uygulama taşıma güvenliği](~/ios/app-fundamentals/ats.md).

## <a name="consuming-a-simpledb-service"></a>SimpleDB hizmetini kullanma

Amazon Cognito kimlik uygulamadan kodlamak AWS kimlik bilgileri olmadan uygulamaya çağrılacak SimpleDB gibi AWS hizmetleri sağlar. Bunun yerine, bir benzersiz kimlik havuzu oluşturulan [Amazon Cognito konsol](https://console.aws.amazon.com/cognito/home). Kimlik havuzu kimliğini erişebileceği SimpleDB gibi kaynakları belirtmek için rolleri kullanmak kimlikleri içerir.

[.NET için AWS SDK](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22) sağlar `CognitoAWSCredentials` ve `AmazonSimpleDBClient` bir Xamarin.Forms uygulaması tarafından SimpleDB hizmete erişmek için aşağıdaki kod örneğinde gösterildiği gibi kullanılan sınıflar:

```csharp
AmazonSimpleDBClient client;
...

public SimpleDBStorage ()
{
  var credentials = new CognitoAWSCredentials (
                      Constants.CognitoIdentityPoolId,
                      RegionEndpoint.USEast1);
  var config = new AmazonSimpleDBConfig ();
  config.RegionEndpoint = RegionEndpoint.USWest2;
  client = new AmazonSimpleDBClient (credentials, config);
  ...
}
```

Yeni bir örneğini `CognitoAWSCredentials` sınıfı benzersiz kimlik havuzu kimliği ve bölge Cognito kimlik hesabının sağlayarak oluşturulur. Yazma zaman Cognito kimliği yalnızca USEast1 ve EUWest1 bölgelerde kullanılabilir. Ancak, bu bölgeler dışında Amazon Hizmetleri ile iletişim kurabilir.

Zaman `AmazonSimpleDBClient` örneği oluşturulduğunda, `CognitoAWSCredentials` örneği girilmesi gerekir, bunların ile bir `AmazonSimpleDBConfig` SimpleDB hizmetin bulunduğu coğrafi bölge belirtir örneği. `CognitoAWSCredentials` Örneği erişilen SimpleDB hizmet içinde kimlik havuzu oluşturulduğu, AWS erişim anahtarı ve gizli anahtar uygulamada katıştırmak için gereken kaçınarak AWS hesabıyla ilişkili bir olmasını sağlar.

Çağrılarak oluşturulan bir SimpleDB hizmeti etki alanı `AmazonSimpleDBClient.CreateDomainAsync` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
string tableName = "Todo";
...

async Task CreateDomain ()
{
  ...
  await client.CreateDomainAsync (new CreateDomainRequest { DomainName = tableName });
  ...
}
```

`CreateDomainAsync` Yöntemi gerektiren bir `CreateDomainRequest` örneği parametre olarak. `CreateDomainRequest` Örneği başlatır `DomainName` özelliğinin etki alanını tanımlamak için kullanılacak değeri. Etki alanı oluşturmak için bu değer AWS hesabıyla ilişkili etki alanları arasında benzersiz olması gerekir. Aksi takdirde, etki alanı oluşturulmaz ve hiçbir hata yanıtı göstermek için gönderilir. Etki alanı adı karşı herhangi bir işlem, yeni oluşturulan bir etki alanı yerine var olan etki alanı karşı sonra ortaya çıkar.

### <a name="creating-simpledb-objects"></a>SimpleDB nesneleri oluşturma

Örnek uygulama kullanır `TodoItem` model verileri için sınıf. Depolamak için bir `TodoItem` , ilk dönüştürülmesi gerekir içine SimpleDB service örneğinde bir `List` , `ReplaceableAttribute` nesneleri. Bu tarafından gerçekleştirilir `ToSimpleDBReplaceableAttributes` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    new ReplaceableAttribute () {
      Name = "Name",
      Value = item.Name,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Notes",
      Value = item.Notes,
      Replace = true
    },
    new ReplaceableAttribute () {
      Name = "Done",
      Value = item.Done.ToString (),
      Replace = true
    }
  };
}
```

Bu yöntem oluşturur bir `List` , yeni `ReplaceableAttribute` örnekleri ile `List` tek bir temsil eden `TodoItem` örneği. Her `ReplaceableAttribute` örneğini gösteren tek bir özellikten `TodoItem` örneği. Hakkında daha fazla bilgi için `ReplaceableAttribute` sınıfı için bkz: [ReplaceableAttribute sınıfı](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_ReplaceableAttribute.htm) Amazon'ın Web sitesinde.

SimpleDB hizmetinden veri alınırken benzer şekilde, onu gelen dönüştürülmelidir bir `List` , `Attribute` için örnekler bir `TodoItem` örneği. Bu ile gerçekleştirilir `FromSimpleDBAttributes` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
TodoItem FromSimpleDBAttributes (List<Amazon.SimpleDB.Model.Attribute> attributeList, string id)
{
  var todoItem = new TodoItem ();
  todoItem.ID = id;
  todoItem.Name = attributeList.Where (attr => attr.Name == "Name").FirstOrDefault ().Value;
  todoItem.Notes = attributeList.Where (attr => attr.Name == "Notes").FirstOrDefault ().Value;
  todoItem.Done = Convert.ToBoolean (attributeList.Where (attr => attr.Name == "Done").FirstOrDefault ().Value);
  return todoItem;
}
```

Bu yöntem yalnızca her alır `Attribute` gelen örnek `List` ve yeni oluşturulan ayarlar `TodoItem` örneği.

Hakkında daha fazla bilgi için `Attribute` sınıfı için bkz: [öznitelik sınıfı](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_Model_Attribute.htm) Amazon'ın Web sitesinde.

### <a name="querying-data"></a>Veri sorgulama

Bir etki alanı içeriğini çağırarak alınabilir `AmazonSimpleDBClient.SelectAsync` yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
      SelectExpression = string.Format ("SELECT * from {0}", tableName)
  };
  var response = await client.SelectAsync (request);
  foreach (var item in response.Items) {
    Items.Add (FromSimpleDBAttributes (item.Attributes, item.Name));
  }
  ...
}
```

`SelectAsync` Yöntemi kabul eden bir `SelectRequest` örneği belirten bir parametre olarak bir `Select` sorgu, ifadesinde `SelectExpression` özelliği. Sorgu ifadesi biçimi standart SQL biçimine benzer `SELECT` deyimi. Sorgu ifadesi hakkında daha fazla bilgi için bkz: [kullanarak seçin Amazon SimpleDB sorguları oluşturmak için](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) Amazon'ın Web sitesinde.

> [!NOTE]
> **Not**: Teklif sorgu ifadesi oluşturulurken kurallarına dikkat edin. Daha fazla bilgi için bkz: [tırnak içine almak kuralları seçin](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) Amazon'ın Web sitesinde.

`SelectAsync` Yöntemi içeren bir koleksiyon sorgu ifadesi eşleşen ilişkili öznitelikleri ve öğelerinin bir yanıt döndürür. Bu koleksiyon için daha sonra dönüştürülür bir `List` , `TodoItem` örneklerini görüntülemek için.

### <a name="creating-and-replacing-data"></a>Oluşturma ve verileri değiştirme

`AmazonSimpleDBClient.PutAttributesAsync` Yöntemi oluşturun ve SimpleDB hizmeti etki alanındaki verileri değiştirmek için kullanılan aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task SaveTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBReplaceableAttributes (todoItem);
  var request = new PutAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.PutAttributesAsync (request);
  ...
}
```

`PutAttributesAsync` Yöntemi kabul eden bir `PutAttributesRequest` örneği parametre olarak. `PutAttributesRequest` Örnek yeni bir öğe olarak oluşturulan veya var olan öğenin yerini öznitelik ad-değer çiftlerini belirtir. `List` , `ReplaceableAttribute` Örnekleri tarafından oluşturulan `ToSimpleDBReplaceableAttributes` yöntemi. Bu yöntem aynı zamanda ayarlar `Replace` her özellik `ReplaceableAttribute` için `true`. Bu veri değiştirirse, var olan bir öznitelik değeri değiştirmek yeni öznitelik değeri neden olur. Ancak, mevcut öznitelik değerlerini değiştirmeye çalışırken bir hata yanıtı oluşturmayacaktır.

Değeri `PutAttributesRequest.ItemName` özelliği, etki alanına yeni bir öğe eklendiğinde veya varolan öğeyi olup değiştirilecek denetler. Uygulamanın yeni bir öğe oluşturduğunda, ayarlar `TodoItem.ID` yeni bir özellik `Guid`. Bu, her sağlar `TodoItem` örneğinin benzersiz bir tanımlayıcı vardır. Bu nedenle, varsa `PutAttributesRequest.ItemName` özelliği etki alanında var olmayan bir değere ayarlamak, SimpleDB hizmeti belirtilen öznitelik ad-değer çiftleri içeren yeni bir öğe oluşturacak. Varsa `PutAttributesRequest.ItemName` özelliği, etki alanında zaten bir değer ayarlanmışsa, SimpleDB hizmet öğesi belirtilen öznitelik ad-değer çiftleri ile güncelleştirir.

### <a name="deleting-data"></a>Verileri silme

`AmazonSimpleDBClient.DeleteAttributesAsync` Yöntemi kullanılır SimpleDB hizmet etki alanından verileri silmek için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public async Task DeleteTodoItemAsync (TodoItem todoItem)
{
  ...
  var attributeList = ToSimpleDBAttributes (todoItem);
  var request = new DeleteAttributesRequest () {
      DomainName = tableName,
      ItemName = todoItem.ID,
      Attributes = attributeList
  };
  await client.DeleteAttributesAsync (request);
  ...
}
```

`DeleteAttributesAsync` Yöntemi kabul eden bir `DeleteAttributesRequest` örneği parametre olarak.  `DeleteAttributesRequest` Örneği ile öğesinden silinecek öznitelikleri belirtiyor `List` , `Attribute` tarafından oluşturulan silinecek örnekleri `ToSimpleDBAttributes` yöntemi. Koşuluyla öğesinin tüm öznitelikleri silinmiş öğe silindi.

## <a name="summary"></a>Özet

Bu makalede, sorgu, oluşturmak ve değiştirmek için .NET için AWS SDK'yı kullanın ve SimpleDB hizmetinde depolanan verileri silmek anlatılmıştır. Bu SDK sağlar `CognitoAWSCredentials` ve `AmazonSimpleDBClient` , bir Xamarin.Forms uygulaması tarafından SimpleDB hizmete erişmek için kullanılan sınıfları.


## <a name="related-links"></a>İlgili bağlantılar

- [TodoAWS (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS/)
- [Amazon Web Hizmetleri SDK Xamarin Geliştirici Kılavuzu](http://docs.aws.amazon.com/mobile/sdkforxamarin/developerguide/)
- [Amazon Cognito kimliği](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB Geliştirici belgeleri](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [AmazonSimpleDBClient sınıfı](http://docs.aws.amazon.com/sdkfornet1/latest/apidocs/html/T_Amazon_SimpleDB_AmazonSimpleDBClient.htm)
- [.NET için Amazon Web Hizmetleri SDK'sı](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
