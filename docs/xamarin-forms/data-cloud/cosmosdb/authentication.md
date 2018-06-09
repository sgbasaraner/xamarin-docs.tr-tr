---
title: Kullanıcıların bir Azure Cosmos DB belge veritabanı ile kimlik doğrulaması
description: Bir kullanıcı yalnızca kendi belgeleri bir Xamarin.Forms uygulaması erişebilmesi için bu makalede Azure Cosmos bölümlenmiş DB koleksiyonlarla erişim denetimi birleştirmek açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 11ED4A4C-0F05-40B2-AB06-5A0F2188EF3D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 031a48e5e10100b2c57ac067a0dda916c93d20da
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241617"
---
# <a name="authenticating-users-with-an-azure-cosmos-db-document-database"></a>Kullanıcıların bir Azure Cosmos DB belge veritabanı ile kimlik doğrulaması

_Birden çok sunucu ve bölümler, sınırsız depolama ve işleme desteklerken yayılabilir bölümlenmiş koleksiyonlar Azure Cosmos DB belge veritabanlarını destekler. Bu makalede, bir kullanıcı yalnızca kendi belgeleri bir Xamarin.Forms uygulaması erişebilmesi için erişim denetimi bölümlenmiş koleksiyonlar ile birleştirme açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Bölüm anahtarı, bölümlendirilmiş bir koleksiyon oluşturulurken belirtilen gerekir ve aynı bölüm anahtarına sahip belgelerin aynı bölümde depolanacak. Bu nedenle, kullanıcının kimliğini belirten bir bölüm anahtarı olarak yalnızca o kullanıcı için belgeleri depolar bölümlendirilmiş bir koleksiyon neden olur. Bu ayrıca Azure Cosmos DB belge veritabanı sayısı, kullanıcı ölçeklendirir ve öğeleri artırmak sağlar.

Herhangi bir koleksiyonu erişim verilmesi gerekir ve SQL API'yi erişim denetimi modeli iki tür erişim yapıları tanımlar:

- **Ana anahtarları** Cosmos DB hesabı içindeki tüm kaynaklar için tam yönetim erişimi etkinleştirmek ve Cosmos DB hesap oluşturulduğunda oluşturulur.
- **Kaynak belirteçleri** bir veritabanı kullanıcısı ve kullanıcının sahip bir koleksiyon veya bir belge gibi belirli bir Cosmos DB kaynak için izni arasındaki ilişkiyi yakalayın.

Bir ana anahtar gösterme kötü amaçlı veya ihmalkar kullanma olasılığını Cosmos DB hesabına açar. Ancak, Azure Cosmos DB kaynak belirteçleri okuma, yazma ve izin verilenler göre bir Azure Cosmos DB hesap belirli kaynakları silmek istemcileri izin vermek için güvenli bir mekanizma sağlar.

İsteyen tipik bir yaklaşım, oluşturma ve bir mobil uygulama için kaynak belirteçleri ileterek kaynak belirteci Aracısı kullanmaktır. Aşağıdaki diyagramda, nasıl belge veritabanı veri erişimi yönetmek için kaynak belirteci Aracısı örnek uygulamayı kullanan bir üst düzey genel bakış gösterilir:

![](authentication-images/documentdb-authentication.png "Belge veritabanı kimlik doğrulama işlemi")

Kaynak belirteci Aracısı Azure App hangi ana anahtar Cosmos DB hesabının sahip hizmetinde, barındırılan bir orta katmanda Web API hizmetidir. Örnek uygulama, belge veritabanı veri erişimini şu şekilde yönetmek için kaynak belirteci Aracısı'nı kullanır:

1. Oturum açma Xamarin.Forms uygulaması bir kimlik doğrulama akışı başlatmak için Azure App Service ile iletişim kurar.
1. Azure uygulama hizmeti bir OAuth kimlik doğrulama akışı Facebook ile gerçekleştirir. Kimlik doğrulaması akışı tamamlandıktan sonra Xamarin.Forms uygulaması bir erişim belirteci alır.
1. Xamarin.Forms uygulaması kaynak belirteci Aracısı'ndan bir kaynak belirteci istemek için erişim belirtecini kullanır.
1. Kaynak belirteci Aracısı erişim belirteci kullanıcının kimliğini Facebook istemek için kullanır. Kullanıcının kimliğini sonra kimliği doğrulanmış kullanıcının bölümlenmiş koleksiyonuna okuma/yazma erişimi vermek için kullanılan Cosmos DB gelen kaynak belirteç istemek için kullanılır.
1. Xamarin.Forms uygulaması kaynak belirteci doğrudan kaynak belirteci tarafından tanımlanan izinlerle Cosmos DB kaynaklara erişmek için kullanır.

> [!NOTE]
> Kaynak belirtecinin süresi dolduğunda, sonraki belge veritabanı isteklerinin bir 401 Yetkisiz özel durumu alır. Bu noktada, Xamarin.Forms uygulamaları kimliğini yeniden oluşturmak ve bu yeni bir kaynak belirteç istemeniz gerekir.

Cosmos DB bölümleme hakkında daha fazla bilgi için bkz: [bölümleme ve Azure Cosmos veritabanı ölçek](/azure/cosmos-db/partition-data/). Cosmos DB erişim denetimi hakkında daha fazla bilgi için bkz: [Cosmos DB verilere erişimin güvenliğini sağlama](/azure/cosmos-db/secure-access-to-data/) ve [erişim denetimi SQL API'sindeki](/rest/api/documentdb/access-control-on-documentdb-resources/).

## <a name="setup"></a>Kurulum

Kaynak belirteci Aracısı bir Xamarin.Forms uygulamayla tümleştirmek için işlem aşağıdaki gibidir:

1. Erişim denetimi kullanan bir Cosmos DB hesabı oluşturun. Daha fazla bilgi için bkz: [Cosmos DB yapılandırma](#cosmosdb_configuration).
1. Kaynak belirteci Aracısı'nı barındırmak için bir Azure uygulama hizmeti oluşturun. Daha fazla bilgi için bkz: [Azure uygulama hizmeti yapılandırma](#app_service_configuration).
1. Kimlik doğrulaması yapmak için bir Facebook uygulaması oluşturursunuz. Daha fazla bilgi için bkz: [Facebook uygulama yapılandırması](#facebook_configuration).
1. Facebook ile kolayca kimlik doğrulaması gerçekleştirmek için Azure App Service yapılandırın. Daha fazla bilgi için bkz: [Azure App Service kimlik doğrulama Yapılandırması](#app_service_authentication_configuration).
1. Azure App Service ve Cosmos DB ile iletişim kurmak için Xamarin.Forms örnek uygulamayı yapılandırın. Daha fazla bilgi için bkz: [Xamarin.Forms uygulaması yapılandırma](#forms_application_configuration).

<a name="cosmosdb_configuration" />

### <a name="azure-cosmos-db-configuration"></a>Azure Cosmos DB yapılandırma

Erişim denetimi kullanan bir Cosmos DB hesabı oluşturma işlemi aşağıdaki gibidir:

1. Cosmos DB hesabı oluşturun. Daha fazla bilgi için bkz: [Azure Cosmos DB hesap oluşturmak](/azure/cosmos-db/sql-api-dotnetcore-get-started#step-1-create-an-azure-cosmos-db-account).
1. Cosmos DB hesabında adlı yeni bir koleksiyon oluşturma `UserItems`, bölüm anahtarı belirterek `/userid`.

<a name="app_service_configuration" />

### <a name="azure-app-service-configuration"></a>Azure uygulama hizmeti yapılandırması

Azure App Service'te kaynak belirteci Aracısı barındırma işlemi aşağıdaki gibidir:

1. Azure portalında yeni bir App Service web uygulaması oluşturun. Daha fazla bilgi için bkz: [bir uygulama hizmeti ortamı'nda bir web uygulaması oluşturma](/azure/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase/).
1. Azure portal, web uygulaması için uygulama ayarları dikey penceresini açın ve aşağıdaki ayarları ekleyin:
    - `accountUrl` – değer Cosmos DB hesabının anahtarlar dikey penceresinden Cosmos DB hesap URL'si olmalıdır.
    - `accountKey` – değer Cosmos DB hesabının anahtarlar dikey penceresinden Cosmos DB ana anahtar (birincil veya ikincil) olmalıdır.
    - `databaseId` – değer Cosmos DB veritabanının adı olmalıdır.
    - `collectionId` – değer Cosmos DB koleksiyonunun adı olmalıdır (Bu durumda, `UserItems`).
    - `hostUrl` – Bu uygulama hizmeti hesabının genel bakış dikey penceresinden web uygulamasının URL'sini değer olmalıdır.

    Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

    [![](authentication-images/azure-web-app-settings.png "App Service Web uygulaması ayarları")](authentication-images/azure-web-app-settings-large.png#lightbox "App Service Web uygulaması ayarları")

1. Kaynak belirteci Aracısı çözüm Azure App Service web uygulamasını yayımlayın.

<a name="facebook_configuration" />

### <a name="facebook-app-configuration"></a>Facebook uygulama yapılandırması

Kimlik doğrulaması gerçekleştirmek üzere Facebook uygulaması oluşturmak için işlem aşağıdaki gibidir:

1. Bir Facebook uygulaması oluşturursunuz. Daha fazla bilgi için bkz: [kaydetme ve uygulama yapılandırma](https://developers.facebook.com/docs/apps/register) Facebook Geliştirici Merkezi'ndeki.
1. Facebook oturum açma ürün uygulamaya ekleyin. Daha fazla bilgi için bkz: [uygulamanızı veya Web sitesi için Facebook oturum açma ekleme](https://developers.facebook.com/docs/facebook-login) Facebook Geliştirici Merkezi'ndeki.
1. Facebook oturum açma gibi yapılandırın:
  - İstemci OAuth oturum açmayı etkinleştirme.
  - Web OAuth oturum açmayı etkinleştirme.
  - Geçerli OAuth yeniden yönlendirme URI'si App Service web uygulaması URI'sine kümesiyle `/.auth/login/facebook/callback` eklenir.

  Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

  ![](authentication-images/facebook-oauth-settings.png "Facebook oturum açma OAuth ayarları")

Daha fazla bilgi için bkz: [Facebook ile uygulamanızı kaydetme](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-nameregister-aregister-your-application-with-facebook).

<a name="app_service_authentication_configuration" />

### <a name="azure-app-service-authentication-configuration"></a>Azure uygulama hizmeti kimlik doğrulama yapılandırması

Uygulama hizmeti kolay kimlik doğrulamasını yapılandırma işlemi aşağıdaki gibidir:

1. Azure Portalı'nda, App Service web uygulaması'na gidin.
1. Azure Portalı'nda açık kimlik doğrulaması / yetkilendirme dikey ve aşağıdaki yapılandırma gerçekleştirin:
  - Uygulama hizmeti kimlik doğrulama açık olmalıdır.
  - Bir isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem ayarlanmalı **Facebook ile oturum açma**.

  Aşağıdaki ekran görüntüsünde, bu yapılandırma gösterilmektedir:

  [![](authentication-images/app-service-authentication-settings.png "App Service Web uygulaması kimlik doğrulama ayarları")](authentication-images/app-service-authentication-settings-large.png#lightbox "App Service Web uygulaması kimlik doğrulama ayarları")

App Service web uygulaması, ayrıca kimlik doğrulaması akışı etkinleştirmek için Facebook uygulaması ile iletişim kurmak için yapılandırılmalıdır. Bu Facebook kimlik sağlayıcısı seçip girerek gerçekleştirilebilir **uygulama kimliği** ve **uygulama gizli anahtarı** Facebook Geliştirici Merkezi Facebook uygulama ayarlarından değerleri. Daha fazla bilgi için bkz: [uygulamanıza eklemek Facebook bilgi](/azure/app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication#a-namesecrets-aadd-facebook-information-to-your-application).

<a name="forms_application_configuration" />

### <a name="xamarinforms-application-configuration"></a>Xamarin.Forms uygulama yapılandırması

Xamarin.Forms örnek uygulamayı yapılandırma işlemi aşağıdaki gibidir:

1. Xamarin.Forms çözümünü açın.
1. Açık `Constants.cs` ve aşağıdaki sabitleri değerlerini güncelleştirin:
  - `EndpointUri` – değer Cosmos DB hesabının anahtarlar dikey penceresinden Cosmos DB hesap URL'si olmalıdır.
  - `DatabaseName` – değer belge veritabanının adı olmalıdır.
  - `CollectionName` – değer belge veritabanı koleksiyonunun adı olmalıdır (Bu durumda, `UserItems`).
  - `ResourceTokenBrokerUrl` – değer uygulama hizmeti hesabının genel bakış dikey penceresinden kaynak belirteci Aracısı web uygulamasının URL'si olmalıdır.

## <a name="initiating-login"></a>Oturum açma başlatılıyor

Örnek uygulama Xamarin.Auth bir tarayıcı bir kimlik sağlayıcısı URL'ye yönlendirmek için aşağıdaki örnek kodda gösterildiği gibi kullanarak oturum açma işlemi başlatır:

```csharp
var auth = new Xamarin.Auth.WebRedirectAuthenticator(
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/facebook"),
  new Uri(Constants.ResourceTokenBrokerUrl + "/.auth/login/done"));
```

Bu, Azure App Service ve Facebook oturum açma sayfasını görüntüler Facebook arasında başlatılması için bir OAuth kimlik doğrulama akışı neden olur:

![](authentication-images/login.png "Facebook oturum açma")

Oturum açma tuşlarına basarak iptal **iptal** düğmesine basarak veya iOS **geri** ; bu durumda kullanıcı kimliği doğrulanmamış olarak kalır ve kimlik sağlayıcısı kullanıcı arabirimi android'de düğmesi ekranından kaldırıldı.

Xamarin.Auth hakkında daha fazla bilgi için bkz: [bir kimlik sağlayıcısı ile kimlik doğrulaması yapan kullanıcıların](~/xamarin-forms/data-cloud/authentication/oauth.md).

## <a name="obtaining-a-resource-token"></a>Kaynak belirteci alma

Başarılı kimlik doğrulaması, aşağıdaki `WebRedirectAuthenticator.Completed` olay etkinleşir. Aşağıdaki kod örneği, bu olay işleme gösterir:

```csharp
auth.Completed += async (sender, e) =>
{
  if (e.IsAuthenticated && e.Account.Properties.ContainsKey("token"))
  {
    var easyAuthResponseJson = JsonConvert.DeserializeObject<JObject>(e.Account.Properties["token"]);
    var easyAuthToken = easyAuthResponseJson.GetValue("authenticationToken").ToString();

    // Call the ResourceBroker to get the resource token
    using (var httpClient = new HttpClient())
    {
      httpClient.DefaultRequestHeaders.Add("x-zumo-auth", easyAuthToken);
      var response = await httpClient.GetAsync(Constants.ResourceTokenBrokerUrl + "/api/resourcetoken/");
      var jsonString = await response.Content.ReadAsStringAsync();
      var tokenJson = JsonConvert.DeserializeObject<JObject>(jsonString);
      resourceToken = tokenJson.GetValue("token").ToString();
      UserId = tokenJson.GetValue("userid").ToString();

      if (!string.IsNullOrWhiteSpace(resourceToken))
      {
        client = new DocumentClient(new Uri(Constants.EndpointUri), resourceToken);
        ...
      }
      ...
    }
  }
};
```

Başarılı bir kimlik doğrulaması kullanılabilir bir erişim belirteci sonucudur `AuthenticatorCompletedEventArgs.Account` özelliği. Erişim belirteci ayıklanan ve kaynak belirteci Aracısı'nın bir GET isteğine kullanılan `resourcetoken` API.

`resourcetoken` API facebook'taki sırayla Cosmos DB'den kaynak belirteç istemek için kullanılan, kullanıcının kimliğini istemek için erişim belirtecini kullanır. Geçerli izni belge belge veritabanındaki kullanıcı için zaten varsa, onu alınır ve kaynak belirteci içeren bir JSON belgesi Xamarin.Forms uygulaması döndürülür. Kullanıcı için geçerli izni belge yoksa, kullanıcı ve izin belge veritabanında oluşturulur ve kaynak belirteci izni belgeden ayıklanır ve Xamarin.Forms uygulaması bir JSON belgesi'na döndürdü.

> [!NOTE]
> Bir belge veritabanı kullanıcı bir belge veritabanı ile ilişkili bir kaynaktır ve her veritabanı sıfır veya daha fazla kullanıcı içerebilir. Bir belge database iznine sahip bir belge veritabanı kullanıcı ilişkili bir kaynaktır ve her kullanıcının sıfır veya daha fazla izinler içerebilir. İzni kaynak belge gibi bir kaynağa erişmeye çalışırken kullanıcının gerektiren bir güvenlik belirteci erişim sağlar.

Varsa `resourcetoken` API başarıyla tamamlandığında, HTTP durum kodu 200 (Tamam) yanıt kaynak belirteci içeren bir JSON belgesi birlikte gönderir. Aşağıdaki JSON verilerini tipik başarılı yanıt iletisi gösterir:

```csharp
{
  "id": "John Smithpermission",
  "token": "type=resource&ver=1&sig=zx6k2zzxqktzvuzuku4b7y==;a74aukk99qtwk8v5rxfrfz7ay7zzqfkbfkremrwtaapvavw2mrvia4umbi/7iiwkrrq+buqqrzkaq4pp15y6bki1u//zf7p9x/aefbvqvq3tjjqiffurfx+vexa1xarxkkv9rbua9ypfzr47xpp5vmxuvzbekkwq6txme0xxxbjhzaxbkvzaji+iru3xqjp05amvq1r1q2k+qrarurhmjzah/ha0evixazkve2xk1zu9u/jpyf1xrwbkxqpzebvqwma+hyyaazemr6qx9uz9be==;",
  "expires": 4035948,
  "userid": "John Smith"
}
```

`WebRedirectAuthenticator.Completed` Olay işleyicisi okur yanıttan `resourcetoken` API ve kaynak belirteci ve kullanıcı kimliği ayıklar. Kaynak belirteci daha sonra bir bağımsız değişken olarak geçirilen `DocumentClient` uç nokta, kimlik bilgileri ve Cosmos DB erişmek için kullanılan bağlantı İlkesi yalıtır ve yapılandırmak ve Cosmos DB isteklerini yürütmek için kullanılan Oluşturucu. Kaynak belirteci doğrudan bir kaynağa erişmek için her bir istekle gönderilir ve kimliği doğrulanmış kullanıcılar bölümlenmiş koleksiyonuna okuma/yazma erişimi izni olduğunu gösterir.

## <a name="retrieving-documents"></a>Belgeleri alınıyor

Yalnızca kimliği doğrulanmış bir kullanıcıya ait belgeler alma aşağıdaki kod örneğinde gösterilmiştir ve kullanıcının kimliğini bölüm anahtarı olarak içeren bir belge sorgu oluşturarak elde edilir:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink,
                        new FeedOptions
                        {
                          MaxItemCount = -1,
                          PartitionKey = new PartitionKey(UserId)
                        })
          .Where(item => !item.Id.Contains("permission"))
          .AsDocumentQuery();
while (query.HasMoreResults)
{
  Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
}
```

Sorgu zaman uyumsuz olarak tüm kimliği doğrulanmış kullanıcının belirtilen koleksiyonundan ait belgeleri alır ve bunları yerleştirir bir `List<TodoItem>` görüntü için koleksiyonu.

`CreateDocumentQuery<T>` Yöntemi belirtir. bir `Uri` belgeler için Sorgulanacak koleksiyonunu temsil eder bağımsız değişkeni ve bir `FeedOptions` nesnesi. `FeedOptions` Nesnesini belirtir öğeleri sınırsız sayıda sorgu ve bölüm anahtarı olarak kullanıcının kimliği tarafından döndürülebilir. Bu, yalnızca kullanıcının bölümlenmiş koleksiyonundaki belgelere sonuç döndürülür sağlar.

> [!NOTE]
> Kaynak belirteci aracısı tarafından oluşturulan izni belgeleri Xamarin.Forms uygulaması tarafından oluşturulan belgeler aynı belge koleksiyonu depolanan unutmayın. Bu nedenle, belge sorgu içeren bir `Where` belge koleksiyonunu sorgusu bir filtre koşulu uygulandığı yan tümcesi. Bu yan tümce izni belgeleri belge koleksiyonundan döndürülen olmayan sağlar.

Bir belge koleksiyonundan belgeleri alma hakkında daha fazla bilgi için bkz: [alma belge koleksiyonu belgeleri](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#document_query).

## <a name="inserting-documents"></a>Belgelere ekleme

Bir belgenin belge koleksiyona ekleme önce `TodoItem.UserId` özelliği güncelleştirilmelidir bölüm anahtarı olarak kullanılan değeri olan aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
item.UserId = UserId;
await client.CreateDocumentAsync(collectionLink, item);
```

Bu belge kullanıcının bölümlenmiş koleksiyona eklenecek sağlar.

Bir belgenin belge koleksiyona ekleme hakkında daha fazla bilgi için bkz: [bir belgenin belge koleksiyona ekleme](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#inserting_document).

## <a name="deleting-documents"></a>Belgeleri silme

Aşağıdaki kod örneğinde gösterildiği olarak bölümlenmiş bir koleksiyondan bir belgeyi silme bölüm anahtarı değerini belirtilmelidir:

```csharp
await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id),
                 new RequestOptions
                 {
                   PartitionKey = new PartitionKey(UserId)
                 });
```

Bu Cosmos DB hangi bölümlenmiş bilir sağlar koleksiyonu belgeden silin.

Bir belgenin belge koleksiyonundan silme hakkında daha fazla bilgi için bkz: [bir belgenin belge koleksiyonundan silme](~/xamarin-forms/data-cloud/cosmosdb/consuming.md#deleting_document).

## <a name="summary"></a>Özet

Bir kullanıcı yalnızca kendi bir Xamarin.Forms uygulaması belge veritabanı belgelerde erişebilmesi için bu makalede erişim denetimi bölümlenmiş koleksiyonlar ile birleştirme açıklanmıştır. Bölüm anahtarı olarak kullanıcının kimliğini belirtme bölümlendirilmiş bir koleksiyon yalnızca o kullanıcı için belgeleri depolamak sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Yapılacaklar Azure Cosmos DB Auth (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoDocumentDBAuth/)
- [Bir Azure Cosmos DB Belge Veritabanını Kullanma](~/xamarin-forms/data-cloud/cosmosdb/consuming.md)
- [Azure Cosmos DB verilere erişimin güvenliğini sağlama](/azure/cosmos-db/secure-access-to-data/)
- [Erişim denetimi SQL API'sindeki](/rest/api/documentdb/access-control-on-documentdb-resources/).
- [Bölüm ve ölçek Azure Cosmos veritabanı](/azure/cosmos-db/partition-data/)
- [Azure Cosmos DB istemci kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](https://msdn.microsoft.com/library/azure/dn948556.aspx)
