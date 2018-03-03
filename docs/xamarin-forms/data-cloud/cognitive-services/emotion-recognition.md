---
title: "API duygu kullanarak duygu tanıma"
description: "Duygu tanıma API'si bir girdi olarak görüntüdeki yüz ifadesi alır ve her yüz görüntüdeki duygular kümesi arasında güven düzeyleri döndürür. Bu makalede duygu tanıma API'si bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için nasıl kullanılacağı açıklanmaktadır."
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 159bd1b23eb7505c5d5629570a34d54e0525567e
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="emotion-recognition-using-the-emotion-api"></a>API duygu kullanarak duygu tanıma

_Duygu tanıma API'si bir girdi olarak görüntüdeki yüz ifadesi alır ve her yüz görüntüdeki duygular kümesi arasında güven düzeyleri döndürür. Bu makalede duygu tanıma API'si bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için nasıl kullanılacağı açıklanmaktadır._

![](~/media/shared/preview.png "Bu API şu anda yayın öncesi")

> [!NOTE]
> Duygu tanıma API'si hala önizlemede değil. Vardır API son serbest bırakmadan önce değişiklikler.

## <a name="overview"></a>Genel Bakış

Duygu tanıma API'si öfke, contempt, disgust, Korku, mutluluk, nötr, sadness ve beklenmedik biçimde, yüz ifadesinde algılayabilir. Bu duygular evrensel ve cross-culturally aynı temel yüz ifadeleri bildirilir. Yüz bir ifadenin bir duygu sonucu döndürerek yanı sıra duygu tanıma API'si de yüz API'si kullanılarak algılanan yüzeyleri için sınırlayıcı kutu döndürür. Bir kullanıcı zaten yüz API çağırdı, yüz dikdörtgen isteğe bağlı bir giriş olarak gönderebilirsiniz. Bir API anahtarı, duygu API kullanmak için elde edilebilir olduğunu unutmayın. Bu, elde edilebilir [ücretsiz Başlarken](https://www.microsoft.com/cognitive-services/sign-up) Microsoft.com'daki.

Duygu tanıma istemci kitaplığı aracılığıyla ve bir REST API'si aracılığıyla gerçekleştirilebilir. Bu makalede duygu tanıma yoluyla gerçekleştirme odaklanır [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) Nuget'ten indirilebilir istemci kitaplığı.

Duygu API kişilerin video yüz ifadeleri tanımak için de kullanılabilir ve bunların duygular özetini döndürür. Daha fazla bilgi için bkz: [videoda duygu](https://www.microsoft.com/cognitive-services/emotion-api/documentation#emotion-in-video) Microsoft.com'daki.

Duygu tanıma API'si hakkında daha fazla bilgi için bkz: [duygu API belgelerine](https://www.microsoft.com/cognitive-services/emotion-api/documentation) Microsoft.com'daki.

## <a name="performing-emotion-recognition"></a>Duygu tanıma gerçekleştirme

Duygu tanıma duygu tanıma API'si bir görüntü akışı yükleyerek elde edilir. Görüntü dosya boyutu 4 MB'den daha büyük olmamalıdır ve desteklenen dosya biçimleri JPEG, PNG, GIF ve BMP. Görüntüyü içinde algılanabilir yüz boyutu aralığı, 36 x 36 için 4096 x 4096 pikseldir. Bu aralığın dışında tüm yüzeyleri algılanmaz.

Aşağıdaki kod örneğinde duygu tanıma işlemi gösterilmektedir:

```csharp
using Microsoft.ProjectOxford.Emotion;
using Microsoft.ProjectOxford.Emotion.Contract;

var emotionClient = new EmotionServiceClient(Constants.EmotionApiKey);

using (var photoStream = photo.GetStream())
{
  Emotion[] emotionResult = await emotionClient.RecognizeAsync(photoStream);
  if (emotionResult.Any())
  {
    // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
    emotionResultLabel.Text = emotionResult.FirstOrDefault().Scores.ToRankedList().FirstOrDefault().Key;
  }
  // Store emotion as app rating
  ...
}
```

Bir `EmotionServiceClient` örneği oluşturulan, bağımsız değişken olarak geçirilen duygu API anahtarı ile duygu tanıma gerçekleştirmek için `EmotionServiceClient` Oluşturucusu.

`RecognizeAsync` Çağrıldığı yöntemi `EmotionServiceClient` örneği, duygu tanıma API'si görüntüye olarak yükleyen bir `Stream`. Bu işlem çağrıldığında API anahtarını duygu API için gönderilir. Geçerli bir API anahtarı göndermek için hata neden olur bir `Microsoft.ProjectOxford.Common.ClientException` , özel durum iletisi geçersiz bir API anahtarı gönderildi belirten ile oluşturulan.

`RecognizeAsync` Yöntemi döndürür bir `Emotion` dizi koşuluyla bir yazıtipi tanınmıyor. Her görüntüsünün algılanabilir yüzeyleri en fazla 64'tür ve yüzeyleri azalan düzende yüz dikdörtgen boyutuna göre sıralanır. Hiçbir yüz algılanırsa, boş bir `Emotion` dizi döndürülecek.

Puanları normalleştirilmiş olarak duygu tanıma API'si sonuçlarından yorumlanırken algılanan duygu yüksek puanı duygu olarak yorumlanması gerektiğini bir toplanacak. Bu nedenle, örnek uygulama için en büyük algılanan yüz yüksek puanı tanınan duygu görüntüde aşağıdaki ekran görüntülerinde gösterildiği gibi görüntüler:

![](emotion-recognition-images/emotion-recognition.png "Duygu tanıma")

## <a name="summary"></a>Özet

Bu makalede duygu tanıma API'si bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için nasıl kullanılacağı açıklanmıştır. Duygu tanıma API'si görüntünün bir girdi olarak yüz ifadesi alır ve her yüz görüntüdeki duygular kümesi arasında güven döndürür.


## <a name="related-links"></a>İlgili bağlantılar

- [Duygu API belgeleri](https://www.microsoft.com/cognitive-services/emotion-api/documentation)
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Emotion](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/)
- [Duygu tanıma API'si](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)
