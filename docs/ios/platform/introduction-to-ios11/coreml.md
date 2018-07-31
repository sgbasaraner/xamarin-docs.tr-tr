---
title: Xamarin.iOS, CoreML giriş
description: Bu belgede, makine öğrenimi ios'ta sağlayan CoreML açıklanmaktadır. Bu belge, nasıl CoreML ile çalışmaya başlama ve Vision çerçevesi ile kullanmak nasıl ele alır.
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2017
ms.openlocfilehash: 13178d4530e3214c6cf31c1018b21815ccd2227f
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350697"
---
# <a name="introduction-to-coreml-in-xamarinios"></a>Xamarin.iOS, CoreML giriş

İOS için makine öğrenimi CoreML getirir: uygulamaları görevleri, her türlü sorun resim tanımadan çözmesini gerçekleştirmek için eğitilen makine öğrenimi modelleri yararlanabilirsiniz.

Bu giriş, aşağıdakileri içerir:

- [CoreML ile çalışmaya başlama](#coreml)
- [CoreML Vision çerçevesi ile kullanma](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>CoreML ile çalışmaya başlama

Bir iOS projesine CoreML ekleme adımları açıklanmaktadır. Başvurmak [Mars Habitat Pricer örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) pratik bir örnek.

![MARS Habitat fiyat tahmini örnek ekran görüntüsü](coreml-images/marspricer-heading.png)

### <a name="1-add-the-coreml-model-to-the-project"></a>1. CoreML modeli projeye ekleme

CoreML model ekleme (bir dosyayla **.mlmodel** uzantı) için **kaynakları** projenin dizin. 

Model dosyanın özelliklerinde, kendi **derleme eylemi** ayarlanır **CoreMLModel**. Bu, içinde derlenecek anlamına gelir. bir **.mlmodelc** uygulama yapılandırıldığında dosyası.

### <a name="2-load-the-model"></a>2. Model yüklenemiyor

Yükleme modeli kullanarak `MLModel.Create` statik yöntemi:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.Create(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Parametrelerini ayarlayın

Model parametreleri ve çıkış uygulayan bir kapsayıcı sınıfını kullanarak geçirilir `IMLFeatureProvider`.

Özellik sağlayıcısı sınıfları dize sözlüğü gibi davranır ve `MLFeatureValue`s, burada her bir özellik değeri olabilir basit bir dize veya sayı, bir dizi veya veri veya bir görüntü içeren bir piksel arabellek.

Tek değerli özellik sağlayıcısı için kodu aşağıda gösterilmiştir:

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

Böyle sınıflarını CoreML tarafından anlaşılır şekilde giriş parametresi sağlanabilir. Özellik adlarını (gibi `myParam` kod örneğinde) ne model bekliyor eşleşmelidir.

### <a name="4-run-the-model"></a>4. Model çalıştırın

Modeli kullanarak parametreleri ve oluşturulacak özellik sağlayıcısı, ardından ayarlamanızı gerektirir, `GetPrediction` yöntemi çağrılabilir:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Sonuçların ayıklanacağı

Tahmin sonuç `outFeatures` ayrıca örneğidir `IMLFeatureProvider`; çıkış değerleri kullanarak erişilebilir `GetFeatureValue` her çıkış parametresi adını (gibi `theResult`), bu örnekte olduğu gibi:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>CoreML Vision çerçevesi ile kullanma

CoreML ayrıca Vision çerçevesi ile birlikte resmi, şekli tanıma, nesne kimliği ve diğer görevler gibi işlemleri gerçekleştirmek için kullanılabilir.

Aşağıdaki adımları nasıl CoreML ve görüntü tanıma birlikte kullanıldığını açıklayan [CoreMLVision örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). Örnek birleştirir [dikdörtgenler tanıma](~/ios/platform/introduction-to-ios11/vision.md#rectangles) ile görme çerçeveden _MNINSTClassifier_ hello'nun el ile yazılmış bir basamak tanımlamak için CoreML modeli.

![Sayının 3 resim tanıma](coreml-images/vision3.png) ![5 sayısının resim tanıma](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Bir işleme CoreML model oluşturma

CoreML modeli _MNISTClassifier_ yüklenir ve ardından içinde sarmalanmış bir `VNCoreMLModel` getiren model işleme görevleri için kullanılabilir. Bu kod Ayrıca iki işleme isteği oluşturur: dikdörtgen bir görüntüde bulmak için ilk ve CoreML modeliyle bir dikdörtgen işlemek için:

```csharp
// Load the ML model
var bundle = NSBundle.MainBundle;
var assetPath = bundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
NSError mlErr, vnErr;
var mlModel = MLModel.Create(assetPath, out mlErr);
var model = VNCoreMLModel.FromMLModel(mlModel, out vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(model, HandleClassification);
```

Sınıf halen uygulamak gereken `HandleRectangles` ve `HandleClassification` adım 3 ve 4 aşağıda gösterilen işleme istekleri için yöntemleri.

### <a name="2-start-the-vision-processing"></a>2. Görme işlemini Başlat

Aşağıdaki kod, isteği işlemeye başlar. İçinde **CoreMLVision** örnek, bu kod, görüntü kullanıcının seçtiği sonra çalıştırılır:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Bu işleyici geçirir `ciImage` Vision çerçevesi için `VNDetectRectanglesRequest` 1. adımda oluşturulmuş.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Görüntü işleme sonuçlarını işleme

Dikdörtgen Algılama tamamlandıktan sonra yürütür `HandleRectangles` ilk dikdörtgen ayıklamak için resmi kırpar, yöntemi dikdörtgen görüntüyü gri tona dönüştürür ve CoreML modeline sınıflandırma geçirir.

`request` Bu yönteme geçirilen parametre işleme isteğinin ayrıntılarını içerir ve kullanarak `GetResults<VNRectangleObservation>()` yöntemi, görüntüde bulunan dikdörtgenler listesini döndürür. İlk dikdörtgen `observations[0]` ayıklanır ve CoreML modele geçti:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] {ClassificationRequest}, out NSError err);
  });
}
```

`ClassificationRequest` Kullanmak için 1. adımda başlatıldı `HandleClassification` sonraki adımda tanımlanan yöntemi.

### <a name="4-handle-the-coreml"></a>4. CoreML işleme

`request` Bu yönteme geçirilen parametre CoreML isteğinin ayrıntılarını içerir ve kullanarak `GetResults<VNClassificationObservation>()` yöntemi, olası sonuçlarını güvenle göre sıralanmış listesini döndürür (en yüksek güvenilirlik ilk):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  // ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```

## <a name="samples"></a>Örnekler

Denemek için üç CoreML örnekleri vardır:

* [Mars Habitat fiyat tahmini örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) basit sayısal girişler ve çıkışlar sahiptir.

* [Vizyonu ve CoreML örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) bir görüntü parametresini kabul edip Vision çerçevesi tek basamak tanıdığı bir CoreML modeline geçirilir görüntüde, kare bölgeleri belirlemek için kullanır.

* Son olarak, [CoreML resim tanıma örnek](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) CoreML fotoğraf özellikleri tanımlamak için kullanır. Varsayılan olarak küçük kullanır **SqueezeNet** modeli (5MB), ancak yazılır indirin ve büyük birleştirmek **VGG16** modeli (553 MB). Daha fazla bilgi için [örnek'ın Benioku](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).

## <a name="related-links"></a>İlgili bağlantılar

- [Machine Learning (Apple)](https://developer.apple.com/machine-learning/)
- [CoreML örnek (Mars Habitat) (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML ve görüntü tanıma (sayı tanıma) (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [CoreML resim tanıma (örnek)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [Azure özel görüntü (örnek) ile CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [CoreML (WWDC) (video) ile tanışın](https://developer.apple.com/videos/play/wwdc2017/703/)
