---
title: Bing yazım denetimi API kullanarak yazım denetimi
description: Bing yazım denetimi, sağlama sözcüklerin için satır içi önerileri bağlamsal yazım metin için denetimi gerçekleştirir. Bu makalede, bir Xamarin.Forms uygulaması yazım hatalarını gidermek için Bing yazım denetleme REST API kullanımı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 259be743a706c9316e2e275ff305a0fe5ad97906
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049915"
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Bing yazım denetimi API kullanarak yazım denetimi

_Bing yazım denetimi, sağlama sözcüklerin için satır içi önerileri bağlamsal yazım metin için denetimi gerçekleştirir. Bu makalede, bir Xamarin.Forms uygulaması yazım hatalarını gidermek için Bing yazım denetleme REST API kullanımı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Bing yazım denetleme REST API iki çalışma modu vardır ve bir modu API için istekte belirtilmelidir:

- `Spell` kısa metin (9 adede kadar sözcükler) büyük/küçük harf değişiklikleri düzeltir.
- `Proof` uzun metin düzeltir, büyük/küçük harf düzeltmeleri ve temel noktalama sağlar ve agresif düzeltmeleri gizler.

Bing yazım denetleme API kullanmak için bir API anahtarı alınması gerekir. Bu, elde edilebilir [deneyin Bilişsel hizmetler](https://azure.microsoft.com/try/cognitive-services/)

Bing yazım denetleme API'si tarafından desteklenen dillerin bir listesi için bkz: [desteklenen diller](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/). Bing yazım denetleme API'si hakkında daha fazla bilgi için bkz: [Bing yazım denetleme belgelerine](/azure/cognitive-services/bing-spell-check/).

## <a name="authentication"></a>Kimlik doğrulaması

Bing yazım denetleme API'sine yapılan her isteği değeri olarak belirtilen bir API anahtarı gerektirir `Ocp-Apim-Subscription-Key` üstbilgi. Aşağıdaki kod örneği, API anahtarı eklemek gösterilmiştir `Ocp-Apim-Subscription-Key` isteği üstbilgisi:

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Bing yazım denetleme API için geçerli bir API anahtarı geçirmek için hata bir 401 yanıt hataya neden olur.

## <a name="performing-spell-checking"></a>Yazım denetimi gerçekleştiriliyor

Yazım denetimi elde edilebilir bir GET veya POST isteği yaparak `SpellCheck` adresindeki API'sine `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`. Bir GET isteği yapılırken yazım işaretli olmasını metin sorgu parametresi olarak gönderilir. Bir POST isteği yaparken işaretli yazım metni istek gövdesinde gönderilir. GET istekleri yazım denetimi 1500 karakter sorgu parametresi dize uzunluğu sınırlaması nedeniyle sınırlıdır. Bu nedenle, kısa dizelerle işaretli yazım yükleniyor sürece POST istekleri tipik olarak yapılmalıdır.

Örnek uygulamasında `SpellCheckTextAsync` yöntemini çağırır yazım işlem denetimi:

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync` Yöntemi bir istek URI oluşturur ve ardından isteği gönderir `SpellCheck` sonucu içeren bir JSON yanıtı döndürür API. Görüntü için arama yöntemi için döndürülen sonuç ile JSON yanıt serisi.

### <a name="configuring-spell-checking"></a>Yazım denetimi yapılandırma

HTTP sorgu parametrelerini belirterek işlemi denetleniyor yazım yapılandırılabilir:

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

Bing yazım denetleme REST API'si hakkında daha fazla bilgi için bkz: [yazım denetleme API v7 başvurusu](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/).

### <a name="sending-the-request"></a>İsteği gönderme

`SendRequestAsync` Metodu Bing yazım denetleme REST API'sine GET isteği yapar ve yanıt verir:

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Bu yöntem, GET isteği gönderir `SpellCheck` çevrilecek belirten metin istek URL'si ve yazım denetimi modu ile API. Yanıt ardından okuma ve çağıran yönteme döndürdü.

`SpellCheck` API istek başarılı olduğunu belirten, isteğin geçerli olduğunu ve istenen bilgileri yanıtta sağlanan yanıt, HTTP durum kodu 200 (Tamam) gönderir. Yanıt nesneleri listesi için bkz: [yanıt nesneleri](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects).

### <a name="processing-the-response"></a>Yanıt işleme

API yanıtını JSON biçiminde döndürülür. Aşağıdaki JSON verileri yanlış yazılmış metin için yanıt iletisi görüntüler `Go shappin tommorow`:

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
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

- [Bing yazım denetimi belgeleri](/azure/cognitive-services/bing-spell-check/)
- [Bir RESTful Web hizmetini kullanma](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing yazım denetleme API v7 başvurusu](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
