---
title: Dosyalar
description: "Katıştırılmış kaynakları kullanarak veya yerel dosya sistemi API'leri karşı yazma dosya Xamarin.Forms ile işleme yapılabilir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9987C3F6-5F04-403B-BBB4-ECB024EA6CC8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/22/2017
ms.openlocfilehash: 605374c0f2bfe656e564e48d14ffe18ce5b7dfe5
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="files"></a>Dosyalar

_Katıştırılmış kaynakları kullanarak veya yerel dosya sistemi API'leri karşı yazma dosya Xamarin.Forms ile işleme yapılabilir._

## <a name="overview"></a>Genel Bakış

Xamarin.Forms kodu her biri kendi dosya sistemine sahip birden çok platformda - çalışır. Bu dosyaları okuma ve yazma çoğu olduğu anlamına gelir kolayca yapılan her platformda yerel dosya API kullanarak. Alternatif olarak, ekli veri dosyalarıyla bir uygulama dağıtmak için daha basit bir çözüm kaynaklardır.

Bu belge aşağıdaki ortak dosya senaryoları işlemeyi kapsar:

-  [ **Gömülü kaynaklar olarak dosyalar** ](#Loading_Files_Embedded_as_Resources) -dosyaları bir uygulama ve yüklenen yansıma API kullanarak bir parçası olarak sevk.
-  [ **Kaydetme ve dosyaları yükleme** ](#Loading_and_Saving_Files) -kullanıcı yazılabilir-depolama olabilir yerel olarak uygulanır ve kullanılarak erişilir `DependencyService` .


Bir üçüncü taraf bileşen çağrı **PCLStorage** okumak ve dosyaları PCL koddan kullanıcı-erişilebilir-depolama alanına yazmak için de kullanılabilir.

Görüntü dosyaları işleme hakkında daha fazla bilgi için bkz [görüntülerle çalışma](~/xamarin-forms/user-interface/images.md) sayfa.

<a name="Loading_Files_Embedded_as_Resources" />

## <a name="loading-files-embedded-as-resources"></a>Kaynaklar olarak ekli dosyaları yükleniyor

Bir dosyaya katıştırmak için bir **PCL** derlemesi oluşturun veya bir dosya eklemek ve emin **yapı eylemi: EmbeddedResource**.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![Katıştırılmış kaynak yapı eylemi yapılandırma](files-images/vs-embeddedresource-sml.png "ayarı EmbeddedResource BuildAction")](files-images/vs-embeddedresource.png "ayarı EmbeddedResource BuildAction")

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

[ ![Metin dosyası katıştırılmış katıştırılmış kaynak yapı eylemi yapılandırma PCL içinde](files-images/xs-embeddedresource-sml.png "ayarı EmbeddedResource BuildAction")](files-images/xs-embeddedresource.png "ayarı EmbeddedResource BuildAction")

-----

`GetManifestResourceStream` Katıştırılmış dosyasını kullanarak erişmek için kullanılan kendi **kaynak kimliği**. Kaynak kimliği olan katıştırılmış-proje için varsayılan ad alanı öneki filename varsayılan olarak bu durumda derlemesidir **WorkingWithFiles** ve dosya adı **PCLTextResource.txt**, kaynak kimliği olacak şekilde `WorkingWithFiles.PCLTextResource.txt`.

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLTextResource.txt");
string text = "";
using (var reader = new System.IO.StreamReader (stream)) {
    text = reader.ReadToEnd ();
}
```

`text` Değişkeni kullanılabilecek metni görüntülemek veya aksi halde kodda kullanın. Bu ekran görüntüsü [örnek uygulaması](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/) olarak işlenen metin gösteren bir `Label` denetim.

 [ ![Metin dosyası PCL katıştırılmış](files-images/pcltext-sml.png "katıştırılmış metin dosyasında PCL görüntülenen uygulama")](files-images/pcltext.png "katıştırılmış metin dosyasında PCL görüntülenen uygulama")

Yükleme ve bir XML seri durumdan eşit olarak basit bir işlemdir. Aşağıdaki kod bir XML dosyası kurulduğunu yüklenen ve bir kaynaktan seri durumdan sonra bağlı gösterir bir `ListView` görüntülemek için. XML dosyasını içeren bir dizi `Monkey` nesneleri (örnek kodda sınıfı tanımlı).

```csharp
var assembly = typeof(LoadResourceText).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream("WorkingWithFiles.PCLXmlResource.xml");
List<Monkey> monkeys;
using (var reader = new System.IO.StreamReader (stream)) {
    var serializer = new XmlSerializer(typeof(List<Monkey>));
    monkeys = (List<Monkey>)serializer.Deserialize(reader);
}
var listView = new ListView ();
listView.ItemsSource = monkeys;
```

 [ ![XML dosyası katıştırılmış ListView içinde görüntülenen PCL içinde](files-images/pclxml-sml.png "katıştırılmış XML dosyasında PCL görüntülenen ListView içinde")](files-images/pclxml.png "katıştırılmış XML dosyasında PCL görüntülenen ListView içinde")

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
#if WINDOWS_PHONE
var resourcePrefix = "WorkingWithFiles.WinPhone.";
#endif

Debug.WriteLine("Using this resource prefix: " + resourcePrefix);
// note that the prefix includes the trailing period '.' that is required
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
Stream stream = assembly.GetManifestResourceStream
    (resourcePrefix + "SharedTextResource.txt");
```

<a name="Organizing_Resources" />

### <a name="organizing-resources"></a>Kaynakları düzenleme

Yukarıdaki örneklerde, dosya durumda kaynak kimliği biçiminde olduğu PCL proje kök dizininde katıştırılır varsayılmaktadır **Namespace.Filename.Extension**, gibi `WorkingWithFiles.PCLTextResource.txt` ve `WorkingWithFiles.iOS.SharedTextResource.txt`.

Katıştırılmış kaynakları klasörlerde düzenlemek mümkündür. Katıştırılmış bir kaynağı bir klasörde yerleştirildiğinde, kaynak kimliği biçimi hale klasör adı kaynak kimliği (noktalarla ayrılmış olarak), bir parçası olur **Namespace.Folder.Filename.Extension**. Bir klasöre örnek uygulamasında kullanılan dosyaları yerleştirme **Klasörüm'ün** karşılık gelen kaynak kimlikleri yapacağı `WorkingWithFiles.MyFolder.PCLTextResource.txt` ve `WorkingWithFiles.iOS.MyFolder.SharedTextResource.txt`.

<a name="Debugging_Embedded_Resources" />

### <a name="debugging-embedded-resources"></a>Gömülü kaynaklar hata ayıklama

Bazen belirli bir kaynak neden yüklenen değil anlaşılması zor olduğundan, aşağıdaki hata ayıklama kodu kaynakların doğru şekilde yapılandırıldığını doğrulamak için bir uygulama geçici olarak eklenebilir. Bilinen tüm kaynaklar için verilen derleme katıştırılmış çıkarır **hataları** kaynak sorunları yüklenirken hata ayıklama yardımcı olmak için paneli.

```csharp
using System.Reflection;
// ...
// use for debugging, not in released app code!
var assembly = typeof(SharedPage).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames()) {
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

<a name="Loading_and_Saving_Files" />

## <a name="saving-and-loading-files"></a>Kaydetme ve dosyaları yükleme

Xamarin.Forms her biri kendi dosya sistemine sahip birden çok platformdaki çalıştığından yükleme ve kullanıcı tarafından oluşturulan dosyaları kaydetme için tek bir yaklaşım yoktur. Tamamlanmış ekran kaydetmek ve kaydeder ve bazı kullanıcı girişi - yükler ekran örnek uygulamasını içeren metin dosyalarını yüklemek nasıl göstermek için aşağıda gösterilmiştir:

 [ ![Kaydetme ve metin yükleme](files-images/saveandload-sml.png "kaydetme ve uygulama yükleme dosyalarında")](files-images/saveandload.png "kaydetme ve uygulama yükleme dosyaları")

Her platformun biraz farklı dizin yapısını ve farklı dosya sistemi özellikleri vardır - örneğin çoğu Xamarin.iOS ve Xamarin.Android desteği `System.IO` işlevselliği Windows Phone yalnızca destekler, ancak `IsolatedStorage` ve [ `Windows.Storage` ](http://msdn.microsoft.com/library/windowsphone/develop/jj681698(v=vs.105).aspx) API'leri.

Bu sorunla karşılaşmamak için örnek uygulama yüklemek ve dosyaları kaydetmek için Xamarin.Forms PCL bir arabirim tanımlar. Bu aygıtta yüklemek ve metin dosyaları kaydetmek için basit bir API depolanacağı sağlar.

```csharp
public interface ISaveAndLoad {
    void SaveText (string filename, string text);
    string LoadText (string filename);
}
```

PCL kod sonra [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) arabirimi yerel bir uygulama için bir başvuru elde edilir. Bu dosyaların her platforma özgü projeleri yazılmış sınıfına kaydetme ve yükleme temsilci için taşınabilir kodu sağlar. Örnekte, **kaydetmek** ve **yük** düğmeleri gösterildiği gibi yazılır:

```csharp
var saveButton = new Button {Text = "Save"};
saveButton.Clicked += (sender, e) => {
    DependencyService.Get<ISaveAndLoad>().SaveText("temp.txt", input.Text);
};
var loadButton = new Button {Text = "Load"};
loadButton.Clicked += (sender, e) => {
    output.Text = DependencyService.Get<ISaveAndLoad>().LoadText("temp.txt");
};
```

Ardından bir uygulama dosyalarını gerçekten kaydedilmiş ve yüklenen kullanılmadan önce her platforma özgü projeleri eklenmesi gerekir.

### <a name="ios-and-android"></a>iOS ve Android

Xamarin.iOS ve Xamarin.Android projeleri için arabiriminin uygulamasını aynı olabilir. Kod, dahil olmak üzere aşağıda verilmiştir `[assembly: Dependency (typeof (SaveAndLoad))]` için gerekli olan öznitelik `DependencyService` çalışmak için.

```csharp
[assembly: Dependency (typeof (SaveAndLoad))]
namespace WorkingWithFiles {
    public class SaveAndLoad : ISaveAndLoad {
        public void SaveText (string filename, string text) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            System.IO.File.WriteAllText (filePath, text);
        }
        public string LoadText (string filename) {
            var documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal);
            var filePath = Path.Combine (documentsPath, filename);
            return System.IO.File.ReadAllText (filePath);
        }
    }
}
```

### <a name="universal-windows-platform-uwp-windows-81-and-windows-phone-81"></a>Evrensel Windows Platformu (UWP), Windows 8.1 ve Windows Phone 8.1

Farklı bir dosya sistemi API – bu platformlar sahip [ `Windows.Storage` ](/windows/uwp/files/quickstart-reading-and-writing-files/) – başka bir deyişle kaydetmek ve dosyaları yüklemek için kullanılır.
`ISaveAndLoad` Arabirimi, aşağıda gösterildiği gibi uygulanabilir:

```csharp
using System;
using System.Threading.Tasks;
using Windows.Storage;
using WinApp;
using WorkingWithFiles;
using Xamarin.Forms;

[assembly: Dependency(typeof(SaveAndLoad_WinApp))]

namespace WindowsApp
{
    // https://msdn.microsoft.com/library/windows/apps/xaml/hh758325.aspx
    public class SaveAndLoad_WinApp : ISaveAndLoad
    {
        public async Task SaveTextAsync(string filename, string text)
        {
            StorageFolder localFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await localFolder.CreateFileAsync(filename, CreationCollisionOption.ReplaceExisting);
            await FileIO.WriteTextAsync(sampleFile, text);
        }
        public async Task<string> LoadTextAsync(string filename)
        {
            StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
            StorageFile sampleFile = await storageFolder.GetFileAsync(filename);
            string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
            return text;
        }
    }
}
```


<a name="Saving_and_Loading_in_Shared_Projects" />

### <a name="saving-and-loading-in-shared-projects"></a>Kaydetme ve paylaşılan projelerinde yükleme

Paylaşılan projeleri derleyici yönergeleri desteklediğinden, tek bir sınıf dosyası paylaşılan proje içindeki tüm platforma özgü kodu dahil mümkündür (kullanmadan `DependencyService`).
Tek bir `SaveAndLoad` sınıfı gibi derleyici yönergeleri kullanarak başvuru projelerine seçmeli olarak derlenmiş yukarıdaki her iki uygulamaları içeren yazılmaya `#if WINDOWS_PHONE`, `#if __IOS__`, ve `#if __ANDROID__`.

## <a name="additional-information"></a>Ek Bilgiler

Xamarin.Forms projeleri PCL tabanlı ayrıca yararlanabilir [PCLStorage NuGet](http://www.nuget.org/packages/pclstorage) ([kod &amp; belgelerine](https://pclstorage.codeplex.com/)) dosya işlemleri platformlar arası şekilde uygulamak yardımcı olmak için.


## <a name="summary"></a>Özet

Bu belge katıştırılmış kaynakları yükleme ve kaydetme ve cihazın metin yükleme için bazı basit dosya işlemleri göstermiştir. Geliştiriciler kendi yerel dosya API'lerini kullanarak uygulayabilirsiniz `DependencyService`, kendi dosya işleme gereksinimlerini karşılamak için gerektiği kadar karmaşık hale getirme.


## <a name="related-links"></a>İlgili bağlantılar

- [FilesSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFiles/)
- [Oluştur, yazma ve bir dosya (UWP) okuma](/windows/uwp/files/quickstart-reading-and-writing-files/)
- [Xamarin.Forms örnekleri](https://github.com/xamarin/xamarin-forms-samples)
- [Dosyalar çalışma kitabı](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/files/files.workbook)
