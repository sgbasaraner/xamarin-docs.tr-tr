---
title: Görme Framework
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/31/2016
ms.openlocfilehash: 698bf829128cff1263e98b49d29a77b75ec32ad9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="vision-framework"></a>Görme Framework

Görme framework işleme özelliklerini 11 dahil olmak üzere, iOS için yeni görüntünün bir numara ekler:

- [Dikdörtgen algılama](#rectangles)
- [Yüz algılama](#faces)
- Machine Learning görüntü analiz (ele [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Barkod algılama
- Görüntü hizalama çözümleme
- Metin algılama
- Horizon Detection
- Nesne algılama & izleme

![Algılanan üç dikdörtgenler ile fotoğraf](vision-images/found-rectangles-tiny.png) ![Algılanan iki yüz ile fotoğraf](vision-images/xamarin-home-faces-tiny.png)

Dikdörtgen algılama ve yüz algılama aşağıdaki daha ayrıntılı olarak ele alınmıştır.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Dikdörtgen algılama

[VisionRects örnek](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) bir görüntüyü işlemek ve üzerinde algılanan dikdörtgenler çizme gösterilmektedir.

### <a name="1-initialize-the-vision-request"></a>1. Görme isteği başlatma

İçinde `ViewDidLoad`, oluşturma bir `VNDetectRectanglesRequest` başvuran `HandleRectangles` her istek sonunda çağrılacak yöntem:

`MaximumObservations` Özelliği de ayarlanması gerekir, aksi takdirde 1 varsayılan olur ve yalnızca tek bir sonuç döndürdü.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Görme işlemini Başlat

Aşağıdaki kod, isteği işlemeye başlar. İçinde **VisionRects** örnek, kullanıcının bir resim seçtiği sonra bu kodu çalıştırır:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Bu işleyici geçirir `ciImage` görme Framework `VNDetectRectanglesRequest` 1. adımda oluşturuldu.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Görüntü işleme sonuçlarını işleme

Dikdörtgen Algılama tamamlandıktan sonra çerçeve yürütür `HandleRectangles` yöntemi, bir özeti aşağıda gösterilmektedir:

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

`OverlayRectangles` Yönteminde **VisionRectangles** örnek üç işlevi vardır:

- Kaynak görüntü işleme
- Burada her biri algılandı, belirtmek için bir dikdörtgen çizme ve
- Her dikdörtgen CoreGraphics kullanarak metin etiketi ekleniyor.

Görünüm [örnek'ın kaynak](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/) tam CoreGraphics yöntemi.

![Algılanan üç dikdörtgenler ile fotoğraf](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Başka bir işleme

Dikdörtgen algılama durumdayken genellikle yalnızca ilk adımı, işlemlerinin zincirindeki gibi [bu CoreMLVision örnek](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision)dikdörtgenler el yazısı basamak ayrıştırmak için bir CoreML modeli burada geçirilir.


<a name="faces" />

## <a name="face-detection"></a>Yüz algılama

[VisionFaces örnek](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) benzer bir şekilde çalışır **VisionRectangles** örnek, farklı bir Vizyon isteği sınıfını kullanma.

### <a name="1-initialize-the-vision-request"></a>1. Görme isteği başlatma

İçinde `ViewDidLoad`, oluşturma bir `VNDetectFaceRectanglesRequest` başvuran `HandleRectangles` her istek sonunda çağrılacak yöntem.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Görme işlemini Başlat

Aşağıdaki kod, isteği işlemeye başlar. İçinde **VisionFaces** örnek kullanıcının bir resim seçtiği sonra bu kodu çalıştırır:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Bu işleyici geçirir `ciImage` görme Framework `VNDetectFaceRectanglesRequest` 1. adımda oluşturuldu.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Görüntü işleme sonuçlarını işleme

Yüz Algılama tamamlandıktan sonra işleyici yürütür `HandleRectangles` hata işleme gerçekleştirir ve algılanan yüzeyleri ve çağrıları sınırlarına görüntüler yöntemi `OverlayRectangles` özgün resmi sınırlayıcı dikdörtgen çizmek için:

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

`OverlayRectangles` Yönteminde **VisionFaces** örnek üç işlevi vardır:

- Kaynak görüntü işleme
- Algılandı, her yüz için dikdörtgen çizme ve
- CoreGraphics kullanarak her yüz için bir metin etiketi ekleniyor.

Görünüm [örnek'ın kaynak](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/) tam CoreGraphics yöntemi.

![Algılanan iki yüz ile fotoğraf](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Başka bir işleme

Görme framework gözler ve ağzınıza gibi yüz özellikleri algılamak için ek özellikler içerir. Kullanım `VNDetectFaceLandmarksRequest` döndürülecek türü `VNFaceObservation` sonuçları adım 3 yukarıdaki olduğu gibi ancak ek `VNFaceLandmark` veri.


## <a name="related-links"></a>İlgili bağlantılar

- [Görme dikdörtgenler (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/VisionRectangles/)
- [Görme yüzeyleri (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/VisionFaces/)
- [Gelişmeleri çekirdek Image - filtreleri, Kurtarma, görme ve daha (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/510/)
