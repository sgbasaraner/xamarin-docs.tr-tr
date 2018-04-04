---
title: Konuşma tanıma Microsoft konuşma API'SİNDE kullanma
description: Microsoft konuşma API konuşulan dilinde işlemek için algoritmaları sağlayan bir bulut tabanlı bir API'dir. Bu makalede Microsoft konuşma tanıma REST API'si bir Xamarin.Forms uygulaması metinde ses dönüştürmek için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 81e645749d239f8964047e92255e786c9b35409d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="speech-recognition-using-the-microsoft-speech-api"></a>Konuşma tanıma Microsoft konuşma API'SİNDE kullanma

_Microsoft konuşma API konuşulan dilinde işlemek için algoritmaları sağlayan bir bulut tabanlı bir API'dir. Bu makalede Microsoft konuşma tanıma REST API'si bir Xamarin.Forms uygulaması metinde ses dönüştürmek için nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Microsoft konuşma API iki bileşenden oluşur:

- Konuşma sözcükleri metne dönüştürmek için bir konuşma tanıma API'si. Konuşma tanıma bir REST API, istemci kitaplığı veya hizmeti gerçekleştirilebilir.
- Bir metin metin konuşulan sözcüklere dönüştürmek için okuma API. Metin okuma dönüştürme bir REST API'si gerçekleştirilir.

Bu makalede, konuşma tanıma REST API aracılığıyla gerçekleştirme odaklanır. Hizmet ve istemci kitaplıkları kısmi sonuçlar döndüren desteklerken, REST API yalnızca kısmi sonuç olmadan bir tek tanıma sonuç döndürebilir.

Microsoft konuşma API kullanmak için bir API anahtarı alınması gerekir. Bu, elde edilebilir [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/).

Microsoft konuşma API'si hakkında daha fazla bilgi için bkz: [Microsoft konuşma API belgelerine](/azure/cognitive-services/speech/home/).

## <a name="authentication"></a>Kimlik doğrulaması

Bilişsel hizmetler belirteç hizmetine penceresinden bir JSON Web Token (JWT) erişim belirteci Microsoft konuşma REST API'sine yapılan her isteği gerektirir `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Belirteç hizmete bir POST isteği yaparak bir belirteç elde edilebilir belirten bir `Ocp-Apim-Subscription-Key` API anahtar değeri olarak içeren üstbilgi.

Aşağıdaki kod örneğinde bir erişim isteği belirteci Hizmeti'nden belirteç gösterilmektedir:

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Base64 metindir, döndürülen erişim belirteci süre sonu zamanı 10 dakikalık sahiptir. Bu nedenle, örnek uygulama erişim belirtecini 9 dakikada yeniler.

Erişim belirteci her Microsoft konuşma REST API'si belirtilmelidir olarak çağrısı bir `Authorization` dizesiyle önekli üstbilgi `Bearer`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Microsoft konuşma REST API için geçerli bir erişim belirteci geçirmek için hata bir 403 yanıt hataya neden olur.

## <a name="performing-speech-recognition"></a>Konuşma tanıma gerçekleştirme

Konuşma tanıma bir POST isteğinin yaparak elde edilir `recognition` adresindeki API'sine `https://speech.platform.bing.com/speech/recognition/`. Tek bir istek 10 saniyeden fazla ses içeremez ve toplam istek süresi 14 saniye aşamaz.

Ses içeriği wav biçimde isteği POST gövdesinde yer almalıdır.

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
    var speechResult = JsonConvert.DeserializeObject<SpeechResult>(response);

    fileStream.Dispose();
    return speechResult;
}
```

Ses her platforma özgü projesi PCM wav verisi olarak kaydedilir ve `RecognizeSpeechAsync` yöntemi kullanan `PCLStorage` ses dosyası bir akış olarak açmak için NuGet paketi. Konuşma tanıma istek URI oluşturulur ve bir erişim belirteci belirteci Hizmeti'nden alınır. Konuşma tanıma isteği nakledilir `recognition` sonucu içeren bir JSON yanıtı döndürür API. Görüntü için arama yöntemi için döndürülen sonuç ile JSON yanıt serisi.

### <a name="configuring-speech-recognition"></a>Konuşma tanıma yapılandırma

Konuşma tanıma işlemi, HTTP sorgu parametrelerini belirterek yapılandırılabilir:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
    // To build a request URL, you should follow:
    // https://docs.microsoft.com/en-us/azure/cognitive-services/speech/getstarted/getstartedrest
    string requestUri = speechEndpoint;
    requestUri += @"dictation/cognitiveservices/v1?";
    requestUri += @"language=en-us";
    requestUri += @"&format=simple";
    System.Diagnostics.Debug.WriteLine(requestUri.ToString());
    return requestUri;
}
```

Tarafından gerçekleştirilen ana yapılandırma `GenerateRequestUri` yöntemdir ses içeriği yerel ayarlamak için. Desteklenen yerel ayarların listesi için bkz: [desteklenen diller ](/azure/cognitive-services/speech/api-reference-rest/supportedlanguages/).

### <a name="sending-the-request"></a>İsteği gönderme

`SendRequestAsync` Yöntemi Microsoft konuşma REST API'sine POST isteği yapar ve yanıt verir:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var content = new StreamContent(fileStream);
    content.Headers.TryAddWithoutValidation("Content-Type", contentType);
    var response = await httpClient.PostAsync(url, content);
    return await response.Content.ReadAsStringAsync();
}
```

Bu yöntem tarafından POST isteği oluşturur:

- Ses akışı kaydırma bir `StreamContent` örneği bir akışa göre HTTP içeriği sağlar.
- Ayarı `Content-Type` isteği üstbilgisinin `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Erişim belirteci ekleme `Authorization` dizesiyle öneki, üst `Bearer`.

POST isteğini sonra gönderilir `recognition` API. Yanıt ardından okuma ve çağıran yönteme döndürdü.

`recognition` API istek başarılı olduğunu belirten, isteğin geçerli olduğunu ve istenen bilgileri yanıtta sağlanan yanıt, HTTP durum kodu 200 (Tamam) gönderir. Olası hata yanıtları listesi için bkz: [sorun giderme](/azure/cognitive-services/speech/troubleshooting).

### <a name="processing-the-response"></a>Yanıt işleme

API yanıtını içinde bulunan tanınan metinle JSON biçiminde döndürülür `name` etiketi. Aşağıdaki JSON verilerini tipik başarılı yanıt iletisi gösterir:

```json
{  
   "RecognitionStatus":"Success",
   "DisplayText":"Go shopping tomorrow.",
   "Offset":16000000,
   "Duration":17100000
}
```

Örnek uygulamasında JSON yanıt içine seri durumdan bir `SpeechResult` örneğiyle görüntülemek, arama yöntemine aşağıdaki ekran görüntülerinde gösterildiği gibi döndürülmesini sonucu:

![](speech-recognition-images/speech-recognition.png "Konuşma tanıma")

## <a name="summary"></a>Özet

Bu makalede Microsoft konuşma REST API'si bir Xamarin.Forms uygulaması metinde ses dönüştürmek için nasıl kullanılacağı açıklanmıştır. Konuşma tanıma gerçekleştirmenin yanı sıra Microsoft konuşma API da metin konuşulan sözcüklere dönüştürebilirsiniz.

## <a name="related-links"></a>İlgili bağlantılar

- [Microsoft konuşma API belgelerine](/azure/cognitive-services/speech/home/).
- [Bir RESTful Web hizmetini kullanma](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
