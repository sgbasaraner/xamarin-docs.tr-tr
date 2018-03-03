---
title: "Konuşma tanıma API'si Bing konuşma kullanma"
description: "Bing konuşma API konuşulan dilinde işlemek için algoritmaları sağlayan bir bulut tabanlı bir API'dir. Bu makalede, bir Xamarin.Forms uygulaması metinde ses dönüştürmek için Bing konuşma tanıma REST API kullanımı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 186ea6277ec7bd4ceb52855186e6fd88344b1b86
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="speech-recognition-using-the-bing-speech-api"></a>Konuşma tanıma API'si Bing konuşma kullanma

_Bing konuşma API konuşulan dilinde işlemek için algoritmaları sağlayan bir bulut tabanlı bir API'dir. Bu makalede, bir Xamarin.Forms uygulaması metinde ses dönüştürmek için Bing konuşma tanıma REST API kullanımı açıklanmaktadır._

![](~/media/shared/preview.png "Bu API şu anda yayın öncesi")

> [!NOTE]
> Bing konuşma API hala önizlemede değil. Vardır API son serbest bırakmadan önce değişiklikler.

## <a name="overview"></a>Genel Bakış

Bing konuşma API iki bileşenden oluşur:

- Konuşma sözcükleri metne dönüştürmek için bir konuşma tanıma API'si. Konuşma tanıma bir REST API, istemci kitaplığı veya hizmeti gerçekleştirilebilir.
- Bir metin metin konuşulan sözcüklere dönüştürmek için okuma API. Metin okuma dönüştürme bir REST API'si gerçekleştirilir.

Bu makalede, konuşma tanıma REST API aracılığıyla gerçekleştirme odaklanır. Hizmet ve istemci kitaplıkları kısmi sonuçlar döndüren desteklerken, REST API yalnızca kısmi sonuç olmadan bir tek tanıma sonuç döndürebilir.

Bing konuşma tanıma API'si kullanmak için bir API anahtarı alınması gerekir. Bu, elde edilebilir [ücretsiz Başlarken](https://www.microsoft.com/cognitive-services/sign-up?ReturnUrl=/cognitive-services/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) Microsoft.com'daki.

Bing konuşma API'si hakkında daha fazla bilgi için bkz: [Bing konuşma belgelerine](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview) Microsoft.com'daki.

## <a name="authentication"></a>Kimlik doğrulaması

Bing konuşma tanıma REST API'sine yapılan her isteği sırasında bilişsel hizmetler belirteci hizmetinden alınan bir JSON Web Token (JWT) erişim belirteci gerektirir `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Belirteç hizmete bir POST isteği yaparak bir belirteç elde edilebilir belirten bir `Ocp-Apim-Subscription-Key` API anahtar değeri olarak içeren üstbilgi.

Aşağıdaki kod örneğinde bir erişim isteği belirteci Hizmeti'nden belirteç gösterilmektedir:

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

Base64 metindir, döndürülen erişim belirteci süre sonu zamanı 10 dakikalık sahiptir. Bu nedenle, örnek uygulama erişim belirtecini 9 dakikada yeniler.

Erişim belirteci her Bing konuşma tanıma REST API belirtilmelidir olarak çağrısı bir `Authorization` dizesiyle önekli üstbilgi `Bearer`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Bing konuşma tanıma REST API için geçerli bir erişim belirteci geçirmek için hata bir 403 yanıt hataya neden olur.

## <a name="performing-speech-recognition"></a>Konuşma tanıma gerçekleştirme

Konuşma tanıma bir POST isteğinin yaparak elde edilir `recognize` adresindeki API'sine `https://speech.platform.bing.com/recognize`. Tek bir istek 10 saniyeden fazla ses içeremez ve toplam istek süresi 14 saniye aşamaz.

Ses içeriği wav biçimde isteği POST gövdesinde yer almalıdır. Desteklenen codec bileşenleri hakkında daha fazla bilgi için bkz: [desteklenen codec bileşenleri](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs) Microsoft.com'daki.

Örnek uygulamasında `RecognizeSpeechAsync` yöntemini çağırır konuşma tanıma işlemi:

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

    // Read audio file to a stream
    var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
    var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

    // Send audio stream to Bing and deserialize the response
    string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
    string accessToken = authenticationService.GetAccessToken();
    var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
    var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

    fileStream.Dispose();
    return speechResults.results.FirstOrDefault();
}
```

Ses her platforma özgü projesi PCM wav verisi olarak kaydedilir ve `RecognizeSpeechAsync` yöntemi kullanan `PCLStorage` ses dosyası bir akış olarak açmak için NuGet paketi. Konuşma tanıma istek URI oluşturulur ve bir erişim belirteci belirteci Hizmeti'nden alınır. Konuşma tanıma isteği nakledilir `recognize` sonucu içeren bir JSON yanıtı döndürür API. Görüntü için arama yöntemi için döndürülen sonuç ile JSON yanıt serisi.

### <a name="configuring-speech-recognition"></a>Konuşma tanıma yapılandırma

Konuşma tanıma işlemi, HTTP sorgu parametrelerini belirterek yapılandırılabilir. Ayarlanmalıdır zorunlu parametreler gösteren aşağıdaki yöntemiyle zorunlu ve isteğe bağlı parametreler şunlardır:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    string requestUri = speechEndpoint;
    requestUri += @"?scenarios=ulm";                                    // websearch is the other option
    requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
    requestUri += @"&locale=en-US";                                     // Other languages supported
    requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
    requestUri += @"&version=3.0";                                      // Required value
    requestUri += @"&format=json";                                      // Required value
    requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
    requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
    return requestUri;
}
```

Tarafından gerçekleştirilen ana yapılandırma `GenerateRequestUri` yöntemdir ses içeriği yerel ayarlamak için. Desteklenen yerel ayarların listesi için bkz: [desteklenen yerel ayarlar ](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales) Microsoft.com'daki.

Zorunlu parametreler için olası değerler hakkında daha fazla bilgi için bkz: [gerekli parametreleri](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters) Microsoft.com'daki. İsteğe bağlı parametreler hakkında daha fazla bilgi için bkz: [isteğe bağlı parametreler](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) Microsoft.com'daki.

### <a name="sending-the-request"></a>İsteği gönderme

`SendRequestAsync` Yöntemi Bing konuşma tanıma REST API'sine POST isteği yapar ve yanıt verir:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);

    using (var httpClient = new HttpClient())
    {
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
        var response = await httpClient.PostAsync(url, content);

        return await response.Content.ReadAsStringAsync();
    }
}
```

Bu yöntem tarafından POST isteği oluşturur:

- Ses akışı kaydırma bir `StreamContent` örneği bir akışa göre HTTP içeriği sağlar.
- Ayarı `Content-Type` isteği üstbilgisinin `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Erişim belirteci ekleme `Authorization` dizesiyle öneki, üst `Bearer`.

POST isteğini sonra gönderilir `recognize` API. Yanıt ardından okuma ve çağıran yönteme döndürdü.

`recognize` API istek başarılı olduğunu belirten, isteğin geçerli olduğunu ve istenen bilgileri yanıtta sağlanan yanıt, HTTP durum kodu 200 (Tamam) gönderir. Olası hata yanıtları listesi için bkz: [hata yanıtları](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses) Microsoft.com'daki.

### <a name="processing-the-response"></a>Yanıt işleme

API yanıtını içinde bulunan tanınan metinle JSON biçiminde döndürülür `name` etiketi. Aşağıdaki JSON verilerini tipik başarılı yanıt iletisi gösterir:

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

Örnek uygulamasında JSON yanıt içine seri durumdan bir `SpeechResult` örneğiyle görüntülemek, arama yöntemine aşağıdaki ekran görüntülerinde gösterildiği gibi döndürülmesini sonucu:

![](speech-recognition-images/speech-recognition.png "Konuşma tanıma")

Her JSON etiket değerleri hakkında daha fazla bilgi için bkz: [konuşma tanıma yanıtları](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses) Microsoft.com'daki.

## <a name="summary"></a>Özet

Bu makalede Bing konuşma tanıma REST API'si bir Xamarin.Forms uygulaması metinde ses dönüştürmek için nasıl kullanılacağı açıklanmıştır. Konuşma tanıma gerçekleştirmenin yanı sıra Bing konuşma API da metin konuşulan sözcüklere dönüştürebilirsiniz.



## <a name="related-links"></a>İlgili bağlantılar

- [Bing konuşma belgeleri](https://www.microsoft.com/cognitive-services/Speech-api/documentation/overview)
- [Bir RESTful Web hizmetini kullanma](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing konuşma tanıma REST API'si](https://www.microsoft.com/cognitive-services/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
