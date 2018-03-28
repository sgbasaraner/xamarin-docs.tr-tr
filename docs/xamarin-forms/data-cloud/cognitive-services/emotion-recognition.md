---
title: API yüzeyi kullanarak duygu tanıma
description: Yüz API yüz ifade görüntünün bir girdi olarak alır ve her yüz görüntüdeki duygular kümesi arasında güven düzeyleri içeren verileri döndürür. Bu makalede, bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için yüz API kullanımı açıklanmaktadır.
ms.topic: article
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 0fc69fb1283ea2afd95900348cdecec5d6514ae0
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="emotion-recognition-using-the-face-api"></a>API yüzeyi kullanarak duygu tanıma

_Yüz API yüz ifade görüntünün bir girdi olarak alır ve her yüz görüntüdeki duygular kümesi arasında güven düzeyleri içeren verileri döndürür. Bu makalede, bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için yüz API kullanımı açıklanmaktadır._

## <a name="overview"></a>Genel Bakış

Yüz API anger, contempt, disgust, Korku, mutluluk, nötr algılamak için duygu algılama, sadness ve beklenmedik biçimde, yüz ifadesinde gerçekleştirebilirsiniz. Bu duygular evrensel ve cross-culturally aynı temel yüz ifadeleri bildirilir. Yüz bir ifadenin bir duygu sonucu döndürerek yanı sıra yüz API görüntüleyebilirsiniz. Ayrıca döndürür algılanan yüzeyleri için sınırlayıcı bir kutu. Bir API anahtarı, yüz API kullanmak için elde edilebilir olduğunu unutmayın. Bu, elde edilebilir [deneyin Bilişsel Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Duygu tanıma istemci kitaplığı aracılığıyla ve bir REST API'si aracılığıyla gerçekleştirilebilir. Bu makalede duygu tanıma yoluyla gerçekleştirme odaklanır [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) Nuget'ten indirilebilir istemci kitaplığı.

Yüz API kişilerin video yüz ifadeleri tanımak için de kullanılabilir ve bunların duygular özetini geri dönebilirsiniz. Daha fazla bilgi için bkz: [gerçek zamanlı analiz videoları nasıl](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

Yüz API'si hakkında daha fazla bilgi için bkz: [yüz API](/azure/cognitive-services/face/overview/).

## <a name="performing-emotion-recognition"></a>Duygu tanıma gerçekleştirme

Duygu tanıma yüz API'sine bir görüntü akışı yükleyerek elde edilir. Görüntü dosya boyutu 4 MB'den daha büyük olmamalıdır ve desteklenen dosya biçimleri JPEG, PNG, GIF ve BMP.

Aşağıdaki kod örneğinde duygu tanıma işlemi gösterilmektedir:

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

Bir `FaceServiceClient` örneği oluşturulan, duygu tanıma yüz API anahtarı ve bağımsız değişken olarak geçirilen bitiş noktası ile gerçekleştirmek için `FaceServiceClient` Oluşturucusu.

> [!NOTE]
> Abonelik anahtarlarınızı elde etmek için kullanılan yazarken, yüz API çağrıları, aynı bölgede kullanmanız gerekir. Örneğin, abonelik anahtarlarınızı aldıysanız `westus` bölge, yüz algılama uç nokta olacak `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

`DetectAsync` Çağrıldığı yöntemi `FaceServiceClient` örneği, yüz API görüntüye olarak yükleyen bir `Stream`. Bu işlem çağrıldığında API anahtarını yüz API için gönderilir. Geçerli bir API anahtarı göndermek için hata neden olur bir `Microsoft.ProjectOxford.Face.FaceAPIException` , özel durum iletisi geçersiz bir API anahtarı gönderildi belirten ile oluşturulan.

`DetectAsync` Yöntemi döndürür bir `Face` dizi koşuluyla bir yazıtipi tanınmıyor. Her yüz içeren bir dizi tarafından belirtilen isteğe bağlı yüz öznitelikleri birlikte konumunu belirtmek için bir dikdörtgen döndürülen `faceAttributes` bağımsız değişkeni `DetectAsync` yöntemi. Hiçbir yüz algılanırsa, boş bir `Face` dizi döndürülecek.

Puanları normalleştirilmiş olarak yüz API sonuçlarından yorumlanırken algılanan duygu yüksek puanı duygu olarak yorumlanıp bir toplanacak. Bu nedenle, örnek uygulama için en büyük algılanan yüz yüksek puanı tanınan duygu görüntüde aşağıdaki ekran görüntülerinde gösterildiği gibi görüntüler:

![](emotion-recognition-images/emotion-recognition.png "Duygu tanıma")

## <a name="summary"></a>Özet

Bu makalede yüz API bir Xamarin.Forms uygulaması değerlendirmek için duygu tanımak için nasıl kullanılacağı açıklanmıştır. Yüz API yüz ifade görüntünün bir girdi olarak alır ve her yüz görüntüdeki duygular kümesi arasında güven içeren verileri döndürür.

## <a name="related-links"></a>İlgili bağlantılar

- [Yüz API](/azure/cognitive-services/face/overview/).
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
