---
title: Xamarin.iOS, Vision çerçevesi
description: Bu belge, iOS 11 kullanmayı açıklar Xamarin.iOS içinde Vision çerçevesi. Özellikle, dikdörtgen algılama ele alınmaktadır ve yüz algılama.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2017
ms.openlocfilehash: 4746de2f351e866fd72946b204f97e997c3e88c4
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350670"
---
# <a name="vision-framework-in-xamarinios"></a>Xamarin.iOS, Vision çerçevesi

Yeni görüntü işleme özelliklerini iOS 11'de dahil olmak üzere, bir dizi Vision çerçevesi ekler:

- [Dikdörtgen algılama](#rectangles)
- [Yüz algılama](#faces)
- Machine Learning görüntü analizi (ele [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Barkod algılama
- Görüntü hizalama analizi
- Metin algılama
- Ufuk algılama
- Nesne algılama ve izleme

![Fotoğraf sahip üç dikdörtgenler algılandı](vision-images/found-rectangles-tiny.png) ![Fotoğraf iki yüz algılandı](vision-images/xamarin-home-faces-tiny.png)

Dikdörtgen algılama ve yüz algılama, aşağıda daha ayrıntılı olarak ele alınmıştır.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Dikdörtgen algılama

[VisionRects örnek](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) görüntü işleme ve algılanan dikdörtgenler çizim gösterilmektedir.

### <a name="1-initialize-the-vision-request"></a>1. İşleme istek başlatılamıyor

İçinde `ViewDidLoad`, oluşturun bir `VNDetectRectanglesRequest` başvuran `HandleRectangles` her istek sonunda çağrılacak yöntemi:

`MaximumObservations` Özelliği ayarlanmalıdır, aksi takdirde 1 varsayılan olur ve yalnızca tek bir sonuç döndürdü.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Görme işlemini Başlat

Aşağıdaki kod, isteği işlemeye başlar. İçinde **VisionRects** örnek, bu kod, görüntü kullanıcının seçtiği sonra çalıştırılır:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Bu işleyici geçirir `ciImage` Vision çerçevesi için `VNDetectRectanglesRequest` 1. adımda oluşturulmuş.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Görüntü işleme sonuçlarını işleme

Dikdörtgen Algılama tamamlandıktan sonra framework yürütür `HandleRectangles` yöntemi, bir özeti aşağıda gösterilmektedir:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Sonuçları Görüntüle

`OverlayRectangles` Yönteminde **VisionRectangles** örnek üç işlev vardır:

- Kaynak görüntü işleme
- Her biri burada algılandı, belirtmek için bir dikdörtgen çizme ve
- Bir metin etiketi CoreGraphics kullanarak her bir dikdörtgen ekleniyor.

Görünüm [örnek'ın kaynak](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) tam CoreGraphics yöntemi.

![Fotoğraf sahip üç dikdörtgenler algılandı](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Daha fazla işleme

Dikdörtgen algılamadır genellikle yalnızca ilk adımı, işlemlerinin zincirindeki gibi ile [bu CoreMLVision örnek](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)burada dikdörtgenler el yazısı basamak ayrıştırılacak CoreML modeline geçirilir.


<a name="faces" />

## <a name="face-detection"></a>Yüz algılama

[VisionFaces örnek](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) benzer bir biçimde çalışır **VisionRectangles** örnek, farklı bir işleme isteği sınıf kullanarak.

### <a name="1-initialize-the-vision-request"></a>1. İşleme istek başlatılamıyor

İçinde `ViewDidLoad`, oluşturun bir `VNDetectFaceRectanglesRequest` başvuran `HandleRectangles` her istek sonunda çağrılacak yöntem.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Görme işlemini Başlat

Aşağıdaki kod, isteği işlemeye başlar. İçinde **VisionFaces** örnek bu kod, görüntü kullanıcının seçtiği sonra çalıştırılır:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Bu işleyici geçirir `ciImage` Vision çerçevesi için `VNDetectFaceRectanglesRequest` 1. adımda oluşturulmuş.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Görüntü işleme sonuçlarını işleme

Yüz Algılama tamamlandıktan sonra işleyici yürütür `HandleRectangles` hata işlemeyi gerçekleştirir ve algılanan yüzeylere ve iletileriniz sınırları görüntüler yöntemi `OverlayRectangles` özgün resmi sınırlayıcı bir dikdörtgen çizmek için:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Sonuçları Görüntüle

`OverlayRectangles` Yönteminde **VisionFaces** örnek üç işlev vardır:

- Kaynak görüntü işleme
- Algılanan her yüz için bir dikdörtgen çizme ve
- Bir metin etiketi CoreGraphics kullanarak her yüz için ekleniyor.

Görünüm [örnek'ın kaynak](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) tam CoreGraphics yöntemi.

![Fotoğraf iki yüz algılandı](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Daha fazla işleme

Vision çerçevesi algılamak gözler ve ağız gibi yüz özellikleri için ek özellikler içerir. Kullanım `VNDetectFaceLandmarksRequest` döndüreceği türü `VNFaceObservation` sonuçları Yukarıdaki 3. adım, ancak ek `VNFaceLandmark` veri.


## <a name="related-links"></a>İlgili bağlantılar

- [İşleme dikdörtgenler (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [İşleme yüzleri (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Gelişmeler çekirdek Image - filtreleri, tam, işleme ve daha (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/510/)
