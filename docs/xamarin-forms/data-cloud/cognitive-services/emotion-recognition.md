---
title: API yüzeyi kullanarak duygu tanıma
description: Yüz API yüz ifade görüntünün bir girdi olarak alır ve her yüz görüntüdeki duygular kümesi arasında güven düzeyleri içeren verileri döndürür. Bu makalede, bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için yüz API kullanımı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 4dc04cb077b894b255eb496b2cb2983626573897
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049772"
---
# <a name="emotion-recognition-using-the-face-api"></a>API yüzeyi kullanarak duygu tanıma

_Yüz API yüz ifade görüntünün bir girdi olarak alır ve her yüz görüntüdeki duygular kümesi arasında güven düzeyleri içeren verileri döndürür. Bu makalede, bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için yüz API kullanımı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Yüz API anger, contempt, disgust, Korku, mutluluk, nötr algılamak için duygu algılama, sadness ve beklenmedik biçimde, yüz ifadesinde gerçekleştirebilirsiniz. Bu duygular evrensel ve cross-culturally aynı temel yüz ifadeleri bildirilir. Yüz bir ifadenin bir duygu sonucu döndürerek yanı sıra yüz API görüntüleyebilirsiniz. Ayrıca döndürür algılanan yüzeyleri için sınırlayıcı bir kutu. Bir API anahtarı, yüz API kullanmak için elde edilebilir olduğunu unutmayın. Bu, elde edilebilir [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Duygu tanıma istemci kitaplığı aracılığıyla ve bir REST API'si aracılığıyla gerçekleştirilebilir. Bu makalede, REST API aracılığıyla duygu tanıma gerçekleştirme odaklanır. REST API hakkında daha fazla bilgi için bkz: [yüz REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Yüz API kişilerin video yüz ifadeleri tanımak için de kullanılabilir ve bunların duygular özetini geri dönebilirsiniz. Daha fazla bilgi için bkz: [gerçek zamanlı analiz videoları nasıl](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Yüz API'si hakkında daha fazla bilgi için bkz: [yüz API](/azure/cognitive-services/face/overview/).

## <a name="authentication"></a>Kimlik doğrulaması

Yüz API'sine yapılan her isteği değeri olarak belirtilen bir API anahtarı gerektirir `Ocp-Apim-Subscription-Key` üstbilgi. Aşağıdaki kod örneği, API anahtarı eklemek gösterilmiştir `Ocp-Apim-Subscription-Key` isteği üstbilgisi:

```csharp
public FaceRecognitionService()
{
  _client = new HttpClient();
  _client.DefaultRequestHeaders.Add("ocp-apim-subscription-key", Constants.FaceApiKey);
}
```

Geçerli bir API anahtarı yüz API'sine geçirmek için hata bir 401 yanıt hataya neden olur.

## <a name="performing-emotion-recognition"></a>Duygu tanıma gerçekleştirme

Duygu tanıma görüntüye içeren bir POST isteği yaparak gerçekleştirilir `detect` adresindeki API'sine `https://[location].api.cognitive.microsoft.com/face/v1.0`, burada `[location]]` API anahtarınıza elde etmek için kullanılan bölgedir. İsteğe bağlı istek Parametreler şunlardır:

- `returnFaceId` – Algılanan yüzeyleri faceIds döndürülmeyeceğini. Varsayılan değer `true` şeklindedir.
- `returnFaceLandmarks` – Algılanan yazıtipleri, yüz işaretleri döndürülmeyeceğini. Varsayılan değer `false` şeklindedir.
- `returnFaceAttributes` – mi çözümlemek ve bir veya daha fazla belirtilen dönmek öznitelikler yüz. Desteklenen yüz öznitelikleri yer `age`, `gender`, `headPose`, `smile`, `facialHair`, `glasses`, `emotion`, `hair`, `makeup`, `occlusion`, `accessories`, `blur`, `exposure`, ve `noise`. Yüz özniteliği analiz ek hesaplama ve zaman maliyet olduğuna dikkat edin.

Görüntü içeriği, bir URL veya ikili veri olarak POST isteğinin gövdesinde yerleştirilmelidir.

> [!NOTE]
> Desteklenen görüntü dosyası biçimlerini JPEG, PNG, GIF ve BMP, ve izin verilen dosya boyutu 1 KB 4 MB.

Örnek uygulama çağırarak duygu tanıma işlemi çağrılan `DetectAsync` yöntemi:

```csharp
Face[] faces = await _faceRecognitionService.DetectAsync(photoStream, true, false, new FaceAttributeType[] { FaceAttributeType.Emotion });
```

Bu yöntem çağrısı, faceIds döndürülmesi, yüz işaretleri döndürülen döndürmemelidir ve görüntünün duygu analiz edilmesi gereken görüntü verilerini içeren akışı belirtir. Ayrıca sonuçları bir dizisi olarak döndüren belirtir `Face` nesneleri. Buna karşılık, `DetectAsync` yöntemini çağırır `detect` REST API, duygu tanıma gerçekleştirir:

```csharp
public async Task<Face[]> DetectAsync(Stream imageStream, bool returnFaceId, bool returnFaceLandmarks, IEnumerable<FaceAttributeType> returnFaceAttributes)
{
  var requestUrl =
    $"{Constants.FaceEndpoint}/detect?returnFaceId={returnFaceId}" +
    "&returnFaceLandmarks={returnFaceLandmarks}" +
    "&returnFaceAttributes={GetAttributeString(returnFaceAttributes)}";
  return await SendRequestAsync<Stream, Face[]>(HttpMethod.Post, requestUrl, imageStream);
}
```

Bu yöntem, bir istek URI oluşturur ve isteği gönderir `detect` API üzerinden `SendRequestAsync` yöntemi.

> [!NOTE]
> Abonelik anahtarlarınızı elde etmek için kullanılan yazarken, yüz API çağrıları, aynı bölgede kullanmanız gerekir. Örneğin, abonelik anahtarlarınızı aldıysanız `westus` bölge, yüz algılama uç nokta olacak `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

### <a name="sending-the-request"></a>İsteği gönderme

`SendRequestAsync` Yöntemi yüz API'sine POST isteği yapar ve sonuç olarak döndüren bir `Face` dizisi:

```csharp
async Task<TResponse> SendRequestAsync<TRequest, TResponse>(HttpMethod httpMethod, string requestUrl, TRequest requestBody)
{
  var request = new HttpRequestMessage(httpMethod, Constants.FaceEndpoint);
  request.RequestUri = new Uri(requestUrl);
  if (requestBody != null)
  {
    if (requestBody is Stream)
    {
      request.Content = new StreamContent(requestBody as Stream);
      request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
    }
    else
    {
      // If the image is supplied via a URL
      request.Content = new StringContent(JsonConvert.SerializeObject(requestBody, s_settings), Encoding.UTF8, "application/json");
    }
  }

  HttpResponseMessage responseMessage = await _client.SendAsync(request);
  if (responseMessage.IsSuccessStatusCode)
  {
    string responseContent = null;
    if (responseMessage.Content != null)
    {
      responseContent = await responseMessage.Content.ReadAsStringAsync();
    }
    if (!string.IsNullOrWhiteSpace(responseContent))
    {
      return JsonConvert.DeserializeObject<TResponse>(responseContent, s_settings);
    }
    return default(TResponse);
  }
  else
  {
    ...
  }
  return default(TResponse);
}
```

Görüntünün bir akış sağlanırsa, yöntemi POST isteğini görüntü akışta kaydırma tarafından derlemeler bir `StreamContent` örneği bir akışa göre HTTP içeriği sağlar. Görüntü URL aracılığıyla sağlanırsa, alternatif olarak, yöntemi POST isteğini URL'de kaydırma tarafından derlemeler bir `StringContent` örneği bir dizesine göre HTTP içeriği sağlar.

POST isteğini sonra gönderilir `detect` API. Yanıt okumak, seri durumdan ve çağıran yönteme döndürdü.

`detect` API istek başarılı olduğunu belirten, isteğin geçerli olduğunu ve istenen bilgileri yanıtta sağlanan yanıt, HTTP durum kodu 200 (Tamam) gönderir. Olası hata yanıtları listesi için bkz: [yüz REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

### <a name="processing-the-response"></a>Yanıt işleme

API yanıtını JSON biçiminde döndürülür. Aşağıdaki JSON verilerini örnek uygulama tarafından istenen veri sağlayan bir tipik başarılı yanıt iletisi gösterir:

```json
[  
   {  
      "faceId":"8a1a80fe-1027-48cf-a7f0-e61c0f005051",
      "faceRectangle":{  
         "top":192,
         "left":164,
         "width":339,
         "height":339
      },
      "faceAttributes":{  
         "emotion":{  
            "anger":0.0,
            "contempt":0.0,
            "disgust":0.0,
            "fear":0.0,
            "happiness":1.0,
            "neutral":0.0,
            "sadness":0.0,
            "surprise":0.0
         }
      }
   }
]
```

Başarılı yanıt iletisi boş bir yanıt algılandı hiçbir yüz gösterir, azalan düzende yüz dikdörtgen boyutuna göre derece yüz girişleri dizisi oluşur. Her yüz içeren bir dizi tarafından belirtilen isteğe bağlı yüz öznitelikleri tanınan `returnFaceAttributes` bağımsız değişkeni `DetectAsync` yöntemi.

Örnek uygulamasında JSON yanıt dizisi seri durumdan olan `Face` nesneleri. Puanları normalleştirilmiş olarak yüz API sonuçlarından yorumlanırken algılanan duygu yüksek puanı duygu olarak yorumlanıp bir toplanacak. Bu nedenle, örnek uygulamayı görüntüde tanınan duygu en büyük algılanan yüz için en yüksek puanı görüntüler. Bu, aşağıdaki kod ile sağlanır:

```csharp
emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
```

Aşağıdaki ekran görüntüsünde örnek uygulama duygu tanıma işlem sonucunu gösterir:

![](emotion-recognition-images/emotion-recognition.png "Duygu tanıma")

## <a name="summary"></a>Özet

Bu makalede yüz API bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için nasıl kullanılacağı açıklanmıştır. Yüz API yüz ifade görüntünün bir girdi olarak alır ve her yüz görüntüdeki duygular kümesi arasında güven içeren verileri döndürür.

## <a name="related-links"></a>İlgili bağlantılar

- [Yüz API](/azure/cognitive-services/face/overview/).
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [REST API yüzeyi](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
