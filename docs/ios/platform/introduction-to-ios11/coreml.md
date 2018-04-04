---
title: CoreML giriş
description: Makine 11 ios'ta mobil uygulamaları için öğrenme
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 412a534829349dbbc3f3b76b166882fa6e0e1cd1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-coreml"></a>CoreML giriş

_Makine 11 ios'ta mobil uygulamaları için öğrenme_

İOS için makine öğrenme CoreML getirir – uygulamaları görüntü tanıma için sorun giderme, her türlü görevleri gerçekleştirmek için eğitilmiş makine öğrenimi modellerini avantajından yararlanabilirsiniz.

Bu giriş aşağıdakileri kapsar:

- [CoreML ile çalışmaya başlama](#coreml)
- [CoreML görme framework ile kullanma](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML ile çalışmaya başlama

Bu adımları bir iOS projesi CoreML nasıl eklendiği açıklanmaktadır. Başvurmak [Mars Habitat Pricer örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) pratik örneğin.

![MARS Habitat fiyat tahmini örnek ekran görüntüsü](coreml-images/marspricer-heading.png)

### <a name="1-add-the-model-to-the-project"></a>1. Model projeye ekleyin

Derlenmiş model ekleme (sahip bir dizin **.modelc** uzantı) için **kaynakları** projenin dizin. Dizinin içeriğini tüm bir yapı eylemi olmalıdır **BundleResource**:

![Kaynaklar klasörünü derlenmiş model içermelidir](coreml-images/resources-modelc.png)

[Örnekleri](https://developer.xamarin.com/samples/monotouch/ios11/) Xcode 9'derlenmiş veya aşağıdaki terminal komutu kullanılarak el ile modelleri kullanın:

```csharp
xcrun coremlcompiler compile {model.mlmodel} {outputFolder}
```

> [!NOTE]
> **.model** dosyaları _gerekir_ derlenmesi için **.modelc** kullanılan önce tarafından CoreML

### <a name="2-load-the-model"></a>2. Model yüklenemiyor

Bir model kullanmadan önce kullanarak yük `MLModel.FromUrl` statik yöntemi:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.FromUrl(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Parametreler Ayarla

Model parametreleri, içeri ve dışarı uygulayan bir kapsayıcı sınıfını kullanarak geçirilir `IMLFeatureProvider`.

Özellik sağlayıcısı sınıfları dize sözlüğü gibi davranır ve `MLFeatureValue`s, burada her bir özellik değeri olabilir basit bir dize veya sayı, bir dizi veya veri veya bir görüntü içeren bir piksel arabelleği.

Tek değerli özellik sağlayıcısı için kod aşağıda verilmiştir:

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

Bu gibi sınıflarını giriş parametreleri CoreML tarafından anlaşılır şekilde sağlanabilir. Özellik adlarını (gibi `myParam` kod örneğinde) ne model bekliyor eşleşmesi gerekir.

### <a name="4-run-the-model"></a>4. Modeli Çalıştır

Model kullanılması örneğinin oluşturulması için özellik sağlayıcısı ve parametreleri, ardından ayarlamanız, `GetPrediction` yöntemi çağrılabilir:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Sonuçları Ayıkla

Tahmin sonuç `outFeatures` de örneği olan `IMLFeatureProvider`; çıkış değerleri kullanarak erişilebilir `GetFeatureValue` her çıkış parametresinin adı ile (gibi `theResult`), bu örnekteki gibi:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>CoreML görme Framework ile kullanma

CoreML de görme framework ile birlikte görüntü şekil tanıma, nesne kimliği ve diğer görevler gibi işlemleri gerçekleştirmek için kullanılabilir.

Aşağıdaki adımları nasıl CoreML ve görme birlikte kullanılan açıklamak [CoreMLVision örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Örnek birleştirir [dikdörtgenler tanıma](~/ios/platform/introduction-to-ios11/vision.md#rectangles) ile görme çerçeveden _MNINSTClassifier_ bir fotoğraf el yazısı basamakta tanımlamak için CoreML modeli.

![Sayının 3 görüntü tanıma](coreml-images/vision3.png) ![5 sayının görüntü tanıma](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Görme CoreML modeli oluşturma

CoreML modeli _MNISTClassifier_ yüklenir ve ardından sarmalanmış bir `VNCoreMLModel` hangi kullanılabilir hale getirir model görme görevler. Bu kod Ayrıca iki görme isteği oluşturur: bir görüntüde dikdörtgenler bulmak için ilk ve bir dikdörtgen CoreML modeli işlemek için:

```csharp
// Load the ML model
var assetPath = NSBundle.MainBundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
var mlModel = MLModel.FromUrl(assetPath, out NSError mlErr);
var vModel = VNCoreMLModel.FromMLModel(mlModel, out NSError vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(vModel, HandleClassification);
```

Sınıf halen uygulamak gereken `HandleRectangles` ve `HandleClassification` adım 3 ve 4 aşağıda gösterilen görme istekleri için yöntemleri.

### <a name="2-start-the-vision-processing"></a>2. Görme işlemini Başlat

Aşağıdaki kod, isteği işlemeye başlar. İçinde **CoreMLVision** örnek, kullanıcının bir resim seçtiği sonra bu kodu çalıştırır:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Bu işleyici geçirir `ciImage` görme Framework `VNDetectRectanglesRequest` 1. adımda oluşturuldu.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Görüntü işleme sonuçlarını işleme

Dikdörtgen Algılama tamamlandıktan sonra yürütür `HandleRectangles` ilk dikdörtgen ayıklamak için resmi kırpar, yöntemi için gri ölçeği dikdörtgen görüntüyü dönüştürür ve sınıflandırma için CoreML modeline geçirir.

`request` Bu yönteme geçirilen parametre görme isteğinin ayrıntılarını içerir ve kullanarak `GetResults<VNRectangleObservation>()` yöntemi, görüntüde bulunan dikdörtgenler listesini döndürür. İlk dikdörtgen `observations[0]` ayıklanır ve CoreML modeline geçirildi:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] { ClassificationRequest }, out NSError err);
  });
}
```

`ClassificationRequest` Kullanmak için 1. adımda başlatıldı `HandleClassification` sonraki adımda tanımlanan yöntemi.

### <a name="4-handle-the-coreml"></a>4. Tanıtıcı CoreML

`request` Bu yönteme geçirilen parametre CoreML isteğinin ayrıntılarını içerir ve kullanarak `GetResults<VNClassificationObservation>()` yöntemi, güvenirlik tarafından sıralanan olası sonuçlarının bir listesini döndürür (en yüksek güvenilirlik ilk):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```



## <a name="samples"></a>Örnekler

Denemek için üç CoreML örnekleri şunlardır:

* [Mars Habitat fiyat tahmini örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) basit sayısal girişleri ve çıkışları vardır.

* [Görme & CoreML örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) bir görüntü parametresini kabul edip görme framework tek basamak tanıdığı bir CoreML modeline iletilen kare görüntü bölgeleri tanımlamak için kullanır.

* Son olarak, [CoreML görüntü tanıma örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML fotoğraf özelliklerini tanımlamak için kullanır. Varsayılan olarak küçük kullanır **SqueezeNet** modeli (5MB), ancak yazılmış indirin ve büyük dahil **VGG16** modeli (553 MB). Daha fazla bilgi için bkz: [örnek'ın Benioku dosyası](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML örnek (Mars Habitat) (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML ve görme (sayı tanıma) (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML görüntü tanıma (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [Azure özel görme (örnek) ile CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Giriş CoreML (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/703/)
