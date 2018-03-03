---
title: "Bing yazım denetimi API kullanarak yazım denetimi"
description: "Bing yazım denetimi, sağlama sözcüklerin için satır içi önerileri bağlamsal yazım metin için denetimi gerçekleştirir. Bu makalede, bir Xamarin.Forms uygulaması yazım hatalarını gidermek için Bing yazım denetleme REST API kullanımı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: ad2bdf27323fd7d7e108a25387cd6aea6d442098
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Bing yazım denetimi API kullanarak yazım denetimi

_Bing yazım denetimi, sağlama sözcüklerin için satır içi önerileri bağlamsal yazım metin için denetimi gerçekleştirir. Bu makalede, bir Xamarin.Forms uygulaması yazım hatalarını gidermek için Bing yazım denetleme REST API kullanımı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Bing yazım denetleme REST API iki çalışma modu vardır ve bir modu API için istekte belirtilmelidir:

- `Spell` kısa metin (9 adede kadar sözcükler) büyük/küçük harf değişiklikleri düzeltir.
- `Proof` uzun metin düzeltir, büyük/küçük harf düzeltmeleri ve temel noktalama sağlar ve agresif düzeltmeleri gizler.

Bing yazım denetleme API kullanmak için bir API anahtarı alınması gerekir. Bu, elde edilebilir [ücretsiz Başlarken](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) Microsoft.com'daki.

Bing yazım denetleme API'si tarafından desteklenen dillerin bir listesi için bkz: [dil desteği](https://www.microsoft.com/cognitive-services/Bing-Spell-check-API/documentation#language-support) Microsoft.com'daki. Bing yazım denetleme API'si hakkında daha fazla bilgi için bkz: [Bing yazım denetleme API](https://www.microsoft.com/cognitive-services/bing-spell-check-api/documentation) Microsoft.com'daki.

## <a name="authentication"></a>Kimlik doğrulaması

Bing yazım denetleme API'sine yapılan her isteği değeri olarak belirtilen bir API anahtarı gerektirir `Ocp-Apim-Subscription-Key` üstbilgi. Aşağıdaki kod örneği, API anahtarı eklemek gösterilmiştir `Ocp-Apim-Subscription-Key` isteği üstbilgisi:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
  ...
}
```

Bing yazım denetleme API için geçerli bir API anahtarı geçirmek için hata bir 401 yanıt hataya neden olur.

## <a name="performing-spell-checking"></a>Yazım denetimi gerçekleştiriliyor

Yazım denetimi elde edilebilir bir GET veya POST isteği yaparak `SpellCheck` adresindeki API'sine `https://api.cognitive.microsoft.com/bing/v5.0/SpellCheck`. Bir GET isteği yapılırken yazım işaretli olmasını metin sorgu parametresi olarak gönderilir. Bir POST isteği yaparken işaretli yazım metni istek gövdesinde gönderilir. GET istekleri yazım denetimi 1500 karakter sorgu parametresi dize uzunluğu sınırlaması nedeniyle sınırlıdır. Bu nedenle, kısa dizelerle işaretli yazım yükleniyor sürece POST istekleri genellikle yapılacaktır.

Örnek uygulamasında `SpellCheckTextAsync` yöntemini çağırır yazım işlem denetimi:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
  string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
  var response = await SendRequestAsync(requestUri, Constants.BingSpellCheckApiKey);
  var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
  return spellCheckResults;
}
```

`SpellCheckTextAsync` Yöntemi bir istek URI oluşturur ve ardından isteği gönderir `SpellCheck` sonucu içeren bir JSON yanıtı döndürür API. Görüntü için arama yöntemi için döndürülen sonuç ile JSON yanıt serisi.

Bing yazım denetleme REST API'si hakkında daha fazla bilgi için bkz: [yazım denetleme API](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) Microsoft.com'daki.

### <a name="configuring-spell-checking"></a>Yazım denetimi yapılandırma

HTTP sorgu parametrelerini belirterek işlemi denetleniyor yazım yapılandırılabilir. Bir GET isteği için ayarlanması gereken zorunlu parametreler gösteren aşağıdaki yöntemiyle zorunlu ve isteğe bağlı parametreler şunlardır:

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

Bu yöntem kullanıma yazım ve yazım denetimi modu metni ayarlar.

Zorunlu ve isteğe bağlı parametreler hakkında daha fazla bilgi için bkz: [yazım denetleme API](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) Microsoft.com'daki.

### <a name="sending-the-request"></a>İsteği gönderme

`SendRequestAsync` Metodu Bing yazım denetleme REST API'sine GET isteği yapar ve yanıt verir:

```csharp
async Task<string> SendRequestAsync(string url, string apiKey)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

API anahtar değeri olarak ekleyerek bu yöntem GET isteğini derlemeler `Ocp-Apim-Subscription-Key` üstbilgi. GET isteğini sonra gönderilir `SpellCheck` çevrilecek belirten metin istek URL'si ve yazım denetimi modu ile API. Yanıt ardından okuma ve çağıran yönteme döndürdü.

`SpellCheck` API istek başarılı olduğunu belirten, isteğin geçerli olduğunu ve istenen bilgileri yanıtta sağlanan yanıt, HTTP durum kodu 200 (Tamam) gönderir. Yanıtlar en olası hata yanıtları listesi için bkz [yazım denetleme API](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358) Microsoft.com'daki.

### <a name="processing-the-response"></a>Yanıt işleme

API yanıtını JSON biçiminde döndürülür. Aşağıdaki JSON verileri yanlış yazılmış metin için yanıt iletisi görüntüler `Go shappin tommorow`:

```csharp
{
  "_type": "SpellCheck",
  "flaggedTokens": [
    {
      "offset": 3,
      "token": "shappin",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "shopping",
          "score": 1
        }
      ]
    },
    {
      "offset": 11,
      "token": "tommorow",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "tomorrow",
          "score": 1
        }
      ]
    }
  ]
}
```

`flaggedTokens` Dizi metni doğru yazılmış değil veya olan dilbilgisi açısından hatalı olarak işaretlenmiş sözcükleri dizisi içerir. Dizi hiçbir yazım ve dilbilgisi hataları bulunamazsa, boş olur. Dizi içinde etiketler aşağıdaki gibidir:

- `offset` – işaretlenen word metin dizesinin başından sıfır tabanlı bir uzaklık.
- `token` – word yanlış yazılmış veya dilbilgisi açısından hatalı metin dizesi içinde.
- `type` – işaretlenmesini Word'ün neden hata türü. İki olası değerler – `RepeatedToken` ve `UnknownToken`.
- `suggestions` – Yazım ve dilbilgisi hatayı düzeltmek üzere sözcükler dizisi. Dizi oluşan bir `suggestion` ve `score`, önerilen düzeltmeyi doğru olduğunu güvenirlik düzeyini belirtir.

Örnek uygulamasında JSON yanıt içine seri durumdan bir `SpellCheckResult` örneğiyle görüntülemek için arama yöntemi için döndürülen sonuç. Aşağıdaki örnekte gösterildiği kod nasıl `SpellCheckResult` örneği görüntülenmek üzere işlenen:

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

Bu kod dolaşır `FlaggedTokens` koleksiyonu ve her yanlış yazılmış değiştirir veya dilbilgisi açısından hatalı sözcükleri ilk öneriyle kaynak metni. Aşağıdaki ekran görüntüleri önce ve sonra yazım denetimi göster:

![](spell-check-images/before-spell-check.png "Önce yazım denetimi")

![](spell-check-images/after-spell-check.png "Sonra yazım denetimi")

## <a name="summary"></a>Özet

Bu makalede Bing yazım denetleme REST API'si bir Xamarin.Forms uygulaması yazım hatalarını gidermek için nasıl kullanılacağı açıklanmıştır. Bing yazım denetimi, sağlama sözcüklerin için satır içi önerileri bağlamsal yazım metin için denetimi gerçekleştirir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bing yazım denetimi belgeleri](https://www.microsoft.com/cognitive-services/bing-spell-check-api/documentation)
- [Bir RESTful Web hizmetini kullanma](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing yazım denetimi API'si](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358)
