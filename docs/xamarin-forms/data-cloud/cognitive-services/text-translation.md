---
title: "Metin çevirisini Çeviricisi API kullanarak"
description: "Microsoft Çeviricisi API konuşma ve REST API'si aracılığıyla metin çevirmek için kullanılabilir. Bu makalede Microsoft metin çeviri API başka bir Xamarin.Forms uygulaması bir dilden diğerine metne çevirmek için nasıl kullanılacağı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: f403ebaffdf742c22e61b73aee7a42648fe597dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="text-translation-using-the-translator-api"></a>Metin çevirisini Çeviricisi API kullanarak

_Microsoft Çeviricisi API konuşma ve REST API'si aracılığıyla metin çevirmek için kullanılabilir. Bu makalede Microsoft metin çeviri API başka bir Xamarin.Forms uygulaması bir dilden diğerine metne çevirmek için nasıl kullanılacağı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Çevirici API iki bileşenden oluşur:

- Bir metin çeviri REST API'ın başka bir dilde metne bir dil metinden çevir. API çevirme önce gönderildiği metin dili otomatik olarak algılar.
- Konuşma çeviri REST API'ın başka bir dilde metne bir dilden diğerine konuşma transcribe. API geri çevrilen metin konuşma metin okuma özelliklerini de tümleşir.

Bu makalede, bir dil metinden başka bir metin çevirisi API'sini kullanmaya çevirirken odaklanır.

Metin çeviri API kullanmak için bir API anahtarı alınması gerekir. Bu yönergeleri izleyerek alınabilir [Başlarken](http://docs.microsofttranslator.com/text-translate.html) üzerinde [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

Microsoft Çeviricisi API'si hakkında daha fazla bilgi için bkz: [Microsoft Çeviricisi belgeleri](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview) Microsoft.com'daki.

## <a name="authentication"></a>Kimlik doğrulaması

Bilişsel hizmetler belirteç hizmetine penceresinden bir JSON Web Token (JWT) erişim belirteci metin çeviri API'sine yapılan her isteği gerektirir `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Belirteç hizmete bir POST isteği yaparak bir belirteç elde edilebilir belirten bir `Ocp-Apim-Subscription-Key` API anahtar değeri olarak içeren üstbilgi.

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

Erişim belirteci her metin çeviri API belirtilmelidir olarak çağrısı bir `Authorization` dizesiyle önekli üstbilgi `Bearer`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Bilişsel hizmetler belirteci hizmeti hakkında daha fazla bilgi için bkz: [kimlik doğrulama belirteci API](http://docs.microsofttranslator.com/oauth-token.html) üzerinde [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

## <a name="performing-text-translation"></a>Metin çeviri gerçekleştirme

Metin çeviri elde edilebilir bir GET isteğine yaparak `Translate` adresindeki API'sine `https://api.microsofttranslator.com/v2/http.svc/Translate`. Örnek uygulamasında `TranslateTextAsync` yöntemini çağırır metin çeviri işlemi:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

`TranslateTextAsync` Yöntemi bir istek URI oluşturur ve belirteci Hizmeti'nden bir erişim belirteci alır. Metin çeviri isteği sonra gönderilir `Translate` sonucu içeren bir XML yanıtı döndürür API. XML yanıtı ayrıştırılır ve görüntülemek için arama yöntemi için bir çeviri sonuç döndürdü.

Metin çeviri REST API'leri hakkında daha fazla bilgi için bkz: [örnek kod](http://docs.microsofttranslator.com/text-translate.html#/default) üzerinde [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="configuring-text-translation"></a>Metin çeviri yapılandırma

HTTP sorgu parametrelerini belirterek metin çeviri işlemi yapılandırılabilir. Ayarlanmalıdır zorunlu parametreler gösteren aşağıdaki yöntemiyle zorunlu ve isteğe bağlı parametreler şunlardır:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Bu yöntem Çevrilecek metin ve metne çevirmek için dili ayarlar. Microsoft Translator tarafından desteklenen dillerin bir listesi için bkz: [diller](https://www.microsoft.com/translator/languages.aspx) Microsoft.com'daki.

> [!NOTE]
> Bir uygulama metnin, hangi dilde bilmeniz gerekiyorsa `Detect` API, metin dizesi dilinin algılamak için çağrılabilir.

Zorunlu ve isteğe bağlı parametreler hakkında daha fazla bilgi için bkz: [metin çeviri API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) üzerinde [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="sending-the-request"></a>İsteği gönderme

`SendRequestAsync` Yöntemi metin çeviri REST API'sine GET isteği yapar ve yanıt verir:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
  using (var httpClient = new HttpClient())
  {
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
  }
}
```

Bu yöntem için erişim belirteci ekleyerek GET isteğini derlemeler `Authorization` dizesiyle öneki, üst `Bearer`. GET isteğini sonra gönderilir `Translate` çevrilecek belirten metin istek URL'si ve metne çevirmek için dil ile API. Yanıt ardından okuma ve çağıran yönteme döndürdü.

`Translate` API istek başarılı olduğunu belirten, isteğin geçerli olduğunu ve istenen bilgileri yanıtta sağlanan yanıt, HTTP durum kodu 200 (Tamam) gönderir. Yanıt iletilerini olası hata yanıtları listesi için bkz: [alma çevir](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Translate) üzerinde [docs.microsofttranslator.com](http://docs.microsofttranslator.com/).

### <a name="processing-the-response"></a>Yanıt işleme

API yanıtını XML biçiminde döndürülür. Aşağıdaki XML verileri tipik başarılı yanıt iletisi gösterir:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

Örnek uygulama içine XML yanıtı ayrıştırılır bir `XDocument` görüntülemek için arama yöntemine aşağıdaki ekran görüntülerinde gösterildiği gibi döndürülmesini XML kök değerine sahip örnek:

![](text-translation-images/text-translation.png "Almanca metni çevirme")

## <a name="summary"></a>Özet

Bu makalede Microsoft metin çeviri API bir Xamarin.Forms uygulaması başka bir dilde metne bir dil metinden çevirmek için nasıl kullanılacağı açıklanmıştır. Metin çevirme yanı sıra Microsoft Çeviricisi API ayrıca bir dilden diğerine konuşma başka bir dilde metne transcribe.



## <a name="related-links"></a>İlgili bağlantılar

- [Microsoft Çeviricisi belgeleri](https://www.microsoft.com/cognitive-services/translator-api/documentation/TranslatorInfo/overview)
- [Bir RESTful Web hizmetini kullanma](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Metin çeviri API](http://docs.microsofttranslator.com/text-translate.html)
