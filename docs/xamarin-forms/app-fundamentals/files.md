---
title: Xamarin.Forms içinde dosya işleme
description: Dosya Xamarin.Forms ile işleme kod kullanarak bir .NET standart kitaplığında veya katıştırılmış kaynakları kullanarak elde edilebilir.
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 0be441a7be9777698212e690aca95fdd75e5050f
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310159"
---
# <a name="file-handling-in-xamarinforms"></a>Xamarin.Forms içinde dosya işleme

_Dosya Xamarin.Forms ile işleme kod kullanarak bir .NET standart kitaplığında veya katıştırılmış kaynakları kullanarak elde edilebilir._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms kodu her biri kendi dosya sistemine sahip birden çok platformda - çalışır. Daha önce bu dosyaları okuma ve yazma en kolay olduğunu her platformda yerel dosyanın API'leri kullanılarak gerçekleştirilen anlamına gelir. Alternatif olarak, ekli veri dosyalarıyla bir uygulama dağıtmak için daha basit bir çözüm kaynaklardır. Ancak, standart .NET 2.0 ile dosya erişim kodu .NET standart kitaplıklarında paylaşmak mümkündür.

Görüntü dosyaları işleme hakkında daha fazla bilgi için bkz [görüntülerle çalışma](~/xamarin-forms/user-interface/images.md) sayfa.

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Kaydetme ve dosyaları yükleme

`System.IO` Sınıfları, her platformda dosya sistemine erişmek için kullanılabilir. `File` Sınıfı, oluşturma, silme ve dosyaları okumasına olanak sağlar ve `Directory` sınıfı oluşturamaz, silemez veya dizinleri içeriğini listeleme olanak tanır. Aynı zamanda `Stream` alt sınıfların bir büyük ölçüde dosya işlemleri (örneğin, bir dosya içinde sıkıştırma veya konumu arama) üzerinde denetim sağlar.

Bir metin dosyası kullanarak yazılabilir `File.WriteAllText` yöntemi:

```csharp
File.WriteAllText(fileName, text);
```

Bir metin dosyası kullanarak okunabilir `File.ReadAllText` yöntemi:

```csharp
string text = File.ReadAllText(fileName);
```

Ayrıca, `File.Exists` yöntemi, belirtilen dosya var olup olmadığını belirler:

```csharp
bool doesExist = File.Exists(fileName);
```

Her platformda dosyasının yolunu değerini kullanarak bir .NET standart Kitaplığı'ndan belirlenebilir [ `Environment.SpecialFolder` ](xref:System.Environment.SpecialFolder) numaralandırması için ilk bağımsız değişken olarak `Environment.GetFolderPath` yöntemi. Bunun ardından bir dosya adı ile birleştirilebilir `Path.Combine` yöntemi:

```csharp
string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Bu işlemler kaydeder ve metin yükleyen bir sayfa içerir örnek uygulaması gösterilmiştir:

[![Kaydetme ve metin yükleme](files-images/saveandload-sml.png "kaydetme ve uygulama yükleme dosyalarında")](files-images/saveandload.png#lightbox "kaydetme ve uygulama yükleme dosyaları")

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Kaynaklar olarak ekli dosyaları yükleniyor

Bir dosyaya katıştırmak için bir **.NET standart** derlemesi oluşturun veya bir dosya eklemek ve emin **yapı eylemi: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Katıştırılmış kaynak yapı eylemi yapılandırma](files-images/vs-embeddedresource-sml.png "ayarı EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png#lightbox "ayarı EmbeddedResource BuildAction")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[![Metin dosyası katıştırılmış katıştırılmış kaynak yapı eylemi yapılandırma PCL içinde](files-images/xs-embeddedresource-sml.png "ayarı EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png#lightbox "ayarı EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` Katıştırılmış dosyasını kullanarak erişmek için kullanılan kendi **kaynak kimliği**. Kaynak kimliği olan katıştırılmış-proje için varsayılan ad alanı öneki filename varsayılan olarak bu durumda derlemesidir **WorkingWithFiles** ve dosya adı **PCLTextResource.txt**, kaynak kimliği olacak şekilde `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` Değişkeni kullanılabilecek metni görüntülemek veya aksi halde kodda kullanın. Bu ekran görüntüsü [örnek uygulaması](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) olarak işlenen metin gösteren bir `Label` denetim.

 [![Metin dosyası PCL katıştırılmış](files-images/pcltext-sml.png "katıştırılmış metin dosyasında PCL görüntülenen uygulama")](files-images/pcltext.png#lightbox "katıştırılmış metin dosyasında PCL görüntülenen uygulama")

Yükleme ve bir XML seri durumdan eşit olarak basit bir işlemdir. Aşağıdaki kod bir XML dosyası kurulduğunu yüklenen ve bir kaynaktan seri durumdan sonra bağlı gösterir bir `ListView` görüntülemek için. XML dosyasını içeren bir dizi `Monkey` nesneleri (örnek kodda sınıfı tanımlı).

```csharp
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(LoadResourceText)).Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [![XML dosyası katıştırılmış ListView içinde görüntülenen PCL içinde](files-images/pclxml-sml.png "katıştırılmış XML dosyasında PCL görüntülenen ListView içinde")](files-images/pclxml.png#lightbox "katıştırılmış XML dosyasında PCL görüntülenen ListView içinde")

<a name="Embedding_in_Shared_Projects" />

### <a name="embedding-in-shared-projects"></a>Paylaşılan projeleri katıştırma

Paylaşılan bir proje içeriğini başvuru projelere derlendiğinden için kullanılan önek katıştırılmış dosya kaynak kimlikleri değiştirebilirsiniz ancak paylaşılan projeleri dosyaları gömülü kaynaklar da içerebilir. Başka bir deyişle, katıştırılmış her dosya için kaynak kimliği her platform için farklı olabilir.

Paylaşılan projeleri ile bu sorunun iki çözümü vardır:

-  **Projeleri senkronize** -proje özelliklerini kullanmak her platform için düzenleme **aynı** derleme adı ve varsayılan ad alanı. Bu değer, ardından katıştırılmış kaynak kimlikleri paylaşılan projesinde öneki olarak "kodlanmış" olabilir.
-  **#if derleyici yönergeleri** -doğru kaynak kimliği öneki ayarlayın ve dinamik olarak doğru kaynak kimliği oluşturmak için bu değeri kullanmak için derleyici yönergeleri kullanın


İkinci seçenek gösteren kod aşağıda verilmiştir. Derleyici yönergeleri (normalde başvuru proje için varsayılan ad alanı ile aynı olduğu) sabit kodlanmış kaynak öneki seçmek için kullanılır. `resourcePrefix` Değişkeni katıştırılmış kaynak dosya adı ile birleştirerek geçerli kaynak kimliği oluşturmak için kullanılan sonra.

```csharp
#if __IOS__
var resourcePrefix = "WorkingWithFiles.iOS.";
#endif
#if __ANDROID__
var resourcePrefix = "WorkingWithFiles.Droid.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Kaynakları düzenleme

Yukarıdaki örneklerde, dosyanın durumda kaynak kimliği biçiminde olduğu .NET standart kitaplığı proje kök dizininde katıştırılır varsayılmaktadır **Namespace.Filename.Extension**, gibi `WorkingWithFiles.PCLTextResource.txt` ve `WorkingWithFiles.iOS.SharedTextResource.txt`.

Katıştırılmış kaynakları klasörlerde düzenlemek mümkündür. Katıştırılmış bir kaynağı bir klasörde yerleştirildiğinde, kaynak kimliği biçimi hale klasör adı kaynak kimliği (noktalarla ayrılmış olarak), bir parçası olur **Namespace.Folder.Filename.Extension**. Bir klasöre örnek uygulamasında kullanılan dosyaları yerleştirme **Klasörüm'ün** karşılık gelen kaynak kimlikleri yapacağı `WorkingWithFiles.MyFolder.PCLTextResource.txt` ve `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Gömülü kaynaklar hata ayıklama

Bazen belirli bir kaynak neden yüklenen değil anlaşılması zor olduğundan, aşağıdaki hata ayıklama kodu kaynakların doğru şekilde yapılandırıldığını doğrulamak için bir uygulama geçici olarak eklenebilir. Bilinen tüm kaynaklar için verilen derleme katıştırılmış çıkarır **hataları** kaynak sorunları yüklenirken hata ayıklama yardımcı olmak için paneli.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = IntrospectionExtensions.GetTypeInfo(typeof(SharedPage)).Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

## <a name="summary"></a>Özet

Bu makalede, bazı basit dosya işlemleri kaydetme ve cihazın metin yükleme ve katıştırılmış kaynaklar yükleniyor göstermiştir. .NET standart 2.0 ile dosya erişim kodu .NET standart kitaplıklarında paylaşmak da mümkündür.

## <a name="related-links"></a>İlgili bağlantılar

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Xamarin.Forms Örnekleri](https://github.com/xamarin/xamarin-forms-samples)
- [Xamarin.iOS dosya sisteminde ile çalışma](~/ios/app-fundamentals/file-system.md)
- [Dosyalar çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
