---
title: "Bilişsel hizmetler zekasıyla ekleme"
description: "Bilişsel Hizmetler Microsoft API'leri, SDK'ları ve Hizmetleri yüz tanıma, konuşma tanıma ve dil anlama gibi özellikler ekleyerek uygulamalarını daha akıllı hale geliştiricilerin kullanımına kümesidir. Bu makalede bazı Microsoft Bilişsel hizmet API'leri çağırmak nasıl gösteren bir örnek uygulama için bir giriş sağlar."
ms.topic: article
ms.prod: xamarin
ms.assetid: 74121ADB-1322-4C1E-A103-F37257BC7CB0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 650f8dceebb088b3601c21c1f5373fc4ae8c76dc
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="adding-intelligence-with-cognitive-services"></a>Bilişsel hizmetler zekasıyla ekleme

_Bilişsel Hizmetler Microsoft API'leri, SDK'ları ve Hizmetleri yüz tanıma, konuşma tanıma ve dil anlama gibi özellikler ekleyerek uygulamalarını daha akıllı hale geliştiricilerin kullanımına kümesidir. Bu makalede bazı Microsoft Bilişsel hizmet API'leri çağırmak nasıl gösteren bir örnek uygulama için bir giriş sağlar._

## <a name="overview"></a>Genel Bakış

Eşlik eden örnek işlevsellik sağlayan bir Yapılacaklar listesi uygulamasıdır:

- Görev listesini görüntüleyin.
- Ekleyin ve yazılım klavyesini aracılığıyla veya konuşma tanıma Bing konuşma API ile gerçekleştirerek görevleri düzenleyin. Konuşma tanıma gerçekleştirme hakkında daha fazla bilgi için bkz: [konuşma tanıma Bing konuşma API kullanarak](speech-recognition.md).
- Bing yazım denetleme API kullanarak onay görevleri yazım. Daha fazla bilgi için bkz: [Bing yazım denetleme API kullanarak yazım denetimi](spell-check.md).
- İngilizce görevlere Çeviricisi API kullanarak Almanca çevir. Daha fazla bilgi için bkz: [metin çevirisini Çeviricisi API kullanarak](text-translation.md).
- Görevleri silme.
- Bir görevin durumu 'Tamamlandı' olarak ayarlayın.
- Uygulama duygu tanıma API'si kullanılarak duygu tanıma ile oranı. Daha fazla bilgi için bkz: [duygu tanıma duygu tanıma API'si kullanılarak](emotion-recognition.md).

Görevler yerel bir SQLite veritabanında depolanır. Yerel bir SQLite veritabanı kullanma hakkında daha fazla bilgi için bkz: [yerel bir veritabanı ile çalışan](~/xamarin-forms/app-fundamentals/databases.md).

`TodoListPage` Uygulaması başlatıldığında görüntülenir. Bu sayfa yerel veritabanında depolanan tüm görevler listesini görüntüler ve kullanıcının yeni bir görev oluşturmak için veya uygulama değerlendirmek için sağlar:

![](images/sample-application-1.png "TodoListPage")

Yeni öğeler tıklayarak oluşturulabilir  *+*  gider düğmesini `TodoItemPage`. Bu sayfa ayrıca için bir görev seçerek gittiğinizde:

![](images/sample-application-2.png "TodoItemPage")

`TodoItemPage` Çevrilmiş, kaydedilen ve silinmiş oluşturulacak düzenlenen, yazım denetimi görevleri sağlar. Konuşma tanıma oluşturmak veya bir görev düzenlemek için kullanılabilir. Bu kaydı başlatmak için mikrofon düğmesine basarak ve kaydı, durdurmak için ikinci kez aynı düğmesine basarak, kayıt için Bing konuşma tanıma API'si gönderdiği sağlanır.

Smilies düğmesine tıklayarak `TodoListPage` gider `RateAppPage`, yüz ifade görüntüde duygu tanıma gerçekleştirmek için kullanılır:

![](images/sample-application-3.png "RateAppPage")

`RateAppPage` Duygu tanıma API'si görüntülenmesini döndürülen duygu tanıma ile gönderilen kendi yüz fotoğrafı geçirmesine izin verir.

## <a name="understanding-the-application-anatomy"></a>Uygulama anatomisi anlama

Örnek uygulama için taşınabilir sınıf kitaplığı (PCL) proje beş ana klasörleri oluşur:

<table>
    <thead>
        <tr><td><strong>Klasör</strong></td><td><strong>Amacı</strong></td></tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Modelleri</strong></td>
            <td>Uygulama için veri modeli sınıfları içerir. Bu içerir <code>TodoItem</code> tek bir uygulama tarafından kullanılan veri öğesi modeller sınıfı. Klasörü ayrıca farklı Microsoft Bilişsel hizmet API'lerden döndürülen modeli JSON yanıtlar için kullanılan sınıfları içerir.</td>
        </tr>
        <tr>
            <td><strong>Depoları</strong></td>
                        <td>İçeren <code>ITodoItemRepository</code> arabirimi ve <code>TodoItemRepository</code> veritabanı işlemlerini gerçekleştirmek için kullanılan sınıfı.</td>
        </tr>
        <tr>
            <td><strong>Hizmetler</strong></td>
                        <td>Farklı Microsoft Bilişsel hizmet API'leri, tarafından kullanılan arabirimleri birlikte erişmek için kullanılan sınıfları ve arabirimleri içerir <code>DependencyService</code> platform projelerinde arabirimleri uygulayan sınıflar bulmak için sınıf.</td>
        </tr>
        <tr>
            <td><strong>Yardımcı programları</strong></td>
            <td>İçeren <code>Timer</code> tarafından kullanılan sınıf <code>AuthenticationService</code> sınıfı 9 dakikada bir JWT erişim belirtecini yenileyin.</td>
        </tr>
        <tr>
            <td><strong>Görünümler</strong></td>
            <td>Uygulama için sayfalar içerir.</td>
        </tr>
    </tbody>
</table>

PCL proje ayrıca bazı önemli dosyaları içerir:

<table>
    <thead>
      <tr><td><strong>Dosya</strong></td><td><strong>Amacı</strong></td></tr>
    <thead>
    <tbody>
        <tr>
            <td><strong>Constants.cs</strong></td>
            <td><code>Constants</code> Microsoft Bilişsel hizmet çağrılan API'leri için uç noktalar ve API anahtarlarını belirtir sınıfı. API anahtar sabitleri farklı Bilişsel hizmet API'leri erişmek için güncelleştirilmesi gerekir.
        </tr>
        <tr>
          <td><strong>App.xaml.cs</strong></td>
          <td><code>App</code> Sınıfı, her bir platformda uygulama tarafından görüntülenen her iki ilk sayfa başlatmasını için sorumlu ve <code>TodoManager</code> veritabanı işlemlerini çağırmak için kullanılan sınıf.</td>
        </tr>
    </tbody>
</table>

### <a name="nuget-packages"></a>NuGet paketleri

Örnek uygulama aşağıdaki NuGet paketlerini kullanır:

- `Microsoft.Net.Http` – sağlar `HttpClient` isteği HTTP üzerinden sağlama sınıfı.
- `Newtonsoft.Json` – .NET için bir JSON çerçeve sağlar.
- `Microsoft.ProjectOxford.Emotion` – Duygu API'ye erişmek için bir istemci kitaplığı.
- `PCLStorage` – platformlar arası yerel dosya g/ç API'leri kümesi sağlar.
- `sqlite-net-pcl` – SQLite veritabanı depolama sağlar.
- `Xam.Plugin.Media` – platformlar arası fotoğraf alma ve API çekme sağlar.

Ayrıca, bu NuGet paketleri ayrıca kendi bağımlılıkları yükler.

### <a name="modeling-the-data"></a>Veri modelleme

Örnek uygulama kullanır `TodoItem` görüntülenir ve yerel SQLite veritabanında depolanan veri modeli için sınıf. Aşağıdaki örnekte gösterildiği kod `TodoItem` sınıfı:

```csharp
public class TodoItem
{
  [PrimaryKey, AutoIncrement]
  public int ID { get; set; }
  public string Name { get; set; }
  public bool Done { get; set; }
}
```

`ID` Özelliği her benzersiz şekilde tanımlamak için kullanılan `TodoItem` örneği ve özellik otomatik artan bir birincil anahtar veritabanında SQLite öznitelikleri ile donatılmış.

### <a name="invoking-database-operations"></a>Veritabanı işlemlerini çağırma

`TodoItemRepository` Sınıfı veritabanı işlemlerini uygular ve sınıfının bir örneği üzerinden erişilen `App.TodoManager` özelliği. `TodoItemRepository` Sınıfı invoke veritabanını işlemleri için aşağıdaki yöntemleri sağlar:

- **GetAllItemsAsync** – tüm öğeleri yerel SQLite veritabanından alır.
- **GetItemAsync** – belirli bir öğeyi yerel SQLite veritabanından alır.
- **SaveItemAsync** – oluşturur veya yerel SQLite veritabanındaki bir öğe güncelleştirir.
- **DeleteItemAsync** – belirtilen öğeyi yerel SQLite veritabanından siler.

### <a name="platform-project-implementations"></a>Platform proje uygulamaları

`Services` PCL proje klasöründe içeren `IFileHelper` ve `IAudioRecorderService` tarafından kullanılan arabirimleri `DependencyService` platform projelerinde arabirimleri uygulayan sınıflar bulmak için sınıf.

`IFileHelper` Arabirimi tarafından gerçekleştirilir `FileHelper` her platform projesinde sınıfı. Bu sınıf tek bir yöntemi, oluşur `GetLocalFilePath`, SQLite veritabanı depolamak için bir yerel dosya yolu döndürür.

`IAudioRecorderService` Arabirimi tarafından gerçekleştirilir `AudioRecorderService` her platform projesinde sınıfı. Bu sınıf oluşan `StartRecording`, `StopRecording`ve platform API'leri ses kaydetmek için cihazın mikrofon kullanın ve wav dosyası olarak depolamak yöntemleri destekleme. İOS, `AudioRecorderService` kullanan `AVFoundation` ses kaydetmek için API. Android'de `AudioRecordService` kullanan `AudioRecord` ses kaydetmek için API. Üzerinde Evrensel Windows Platformu (UWP), `AudioRecorderService` kullanan `AudioGraph` ses kaydetmek için API.

### <a name="invoking-cognitive-services"></a>Bilişsel hizmetler çağırma

Örnek uygulama aşağıdaki Microsoft Bilişsel hizmetler çağırır:

- Bing konuşma API. Daha fazla bilgi için bkz: [konuşma tanıma Bing konuşma API kullanarak](speech-recognition.md).
- Bing yazım denetimi API'si. Daha fazla bilgi için bkz: [Bing yazım denetleme API kullanarak yazım denetimi](spell-check.md).
- API çevir. Daha fazla bilgi için bkz: [metin çevirisini Çeviricisi API kullanarak](text-translation.md).
- Duygu tanıma API'si. Daha fazla bilgi için bkz: [duygu tanıma duygu tanıma API'si kullanılarak](emotion-recognition.md).


## <a name="related-links"></a>İlgili bağlantılar

- [Microsoft Bilişsel hizmetler belgeleri](https://www.microsoft.com/cognitive-services/documentation)
- [Yapılacaklar Bilişsel hizmetler (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
