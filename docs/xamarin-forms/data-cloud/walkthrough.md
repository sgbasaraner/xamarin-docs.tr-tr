---
title: "Örnek anlama"
description: "Bu konu, farklı bir web Hizmetleri ile iletişim gösterilmiştir Xamarin.Forms örnek uygulamayı bir kılavuz sağlar. Her web hizmeti ayrı örnek bir uygulamayı kullanırken, işlevsel olarak benzerdir ve ortak sınıfları paylaşın."
ms.topic: article
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: 6625edc1f661e5f9769de82ec48367e9f900e567
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="understanding-the-sample"></a>Örnek anlama

_Bu konu, farklı bir web Hizmetleri ile iletişim gösterilmiştir Xamarin.Forms örnek uygulamayı bir kılavuz sağlar. Her web hizmeti ayrı örnek bir uygulamayı kullanırken, işlevsel olarak benzerdir ve ortak sınıfları paylaşın._

Aşağıda açıklanan örnek Yapılacaklar listesi uygulaması, web hizmeti arka uçlarını Xamarin.Forms ile farklı türlerde erişmek nasıl göstermek için kullanılır. İşlevsellik sağlar:

- Görev listesini görüntüleyin.
- Ekleme, düzenleme ve görevleri silme.
- Bir görevin durumu 'Tamamlandı' olarak ayarlayın.
- Görev adı ve notlar alanlarını konuşun.

Her durumda, bir web hizmeti aracılığıyla erişilen bir arka uç görevleri depolanır.

Uygulama başlatıldığında, web hizmetinden alınan tüm görevleri listeler ve yeni bir görev oluşturmak kullanıcı izin veren bir sayfa görüntülenir. Bir görevi tıklatmadan nereye görev düzenlenemez, kaydedildi, silinmiş, konuşulan ve ikinci bir sayfa uygulamaya gider. Son uygulama aşağıda gösterilmiştir:

![](walkthrough-images/app-example-1.png "TODO uygulaması - ilk sayfa")
![](walkthrough-images/app-example-2.png "Todo uygulaması - ikinci sayfa")

Her konuda bu kılavuzdaki için karşıdan yükleme bağlantısı sağlayan bir *farklı* belirli bir web hizmeti arka uç türünü gösteren bir uygulama sürümü. Her web hizmeti stiline ilgili sayfasında ilgili örnek kodu indirin.

## <a name="understanding-the-application-anatomy"></a>Uygulama anatomisi anlama

Her bir örnek uygulama için PCL proje üç ana klasörleri oluşur:

<table>
    <thead>
        <tr><td><strong>Klasör</strong></td><td><strong>Amacı</strong></td></tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Veri</strong></td>
                        <td>Sınıflar ve arabirimler veri öğelerini yönetmek ve web hizmeti ile iletişim kurmak için kullanılan içerir. En azından, buna dahildir <code>TodoItemManager</code> bir özellik aracılığıyla sunulan sınıfı <code>App</code> web hizmeti işlemini çağırmak için sınıf.</td>
        </tr>
        <tr>
            <td><strong>Modelleri</strong></td>
                        <td>Uygulama için veri modeli sınıfları içerir. En azından, buna dahildir <code>TodoItem</code> tek bir uygulama tarafından kullanılan veri öğesi modeller sınıfı. Klasörü de modeli kullanıcı verileri için kullanılan ek sınıfları içerir.</td>
        </tr>
        <tr>
            <td><strong>Görünümler</strong></td>
                        <td>Uygulama için sayfalar içerir. Bu genellikle oluşan <code>TodoListPage</code> ve <code>TodoItemPage</code> sınıfları ve kimlik doğrulama amacıyla kullanılan ek sınıfları.</td>
                </tr>
    </tbody>
</table>

Her uygulama için PCL proje de önemli dosyaların sayısı oluşur:

<table>
    <thead>
      <tr><td><strong>Dosya</strong></td><td><strong>Amacı</strong></td></tr>
    <thead>
    <tbody>
        <tr>
            <td><strong>Constants.cs</strong></td>
            <td><code>Constants</code> Sınıfı web hizmetiyle iletişim kurmak için uygulama tarafından kullanılan sabitlerini belirtir. Bu sabitleri kişisel arka uç hizmetinize erişmek için güncelleştirme üzerinde bir sağlayıcı oluşturulan gerektirir.
        </tr>
        <tr>
            <td><strong>ITextToSpeech.cs</strong></td>
            <td><code>ITextToSpeech</code> Belirleyen arabirimi <code>Speak</code> yöntemi tüm sınıflar tarafından sağlanmalıdır.</td>
        </tr>
        <tr>
          <td><strong>TODO.cs</strong></td>
          <td><code>App</code> Her platformda uygulama tarafından görüntülenen her iki ilk sayfa başlatmasını için sorumlu sınıfı ve <code>TodoItemManager</code> web hizmeti işlemini çağırmak için kullanılan sınıf.</td>
        </tr>
    </tbody>
</table>

### <a name="viewing-pages"></a>Sayfalarını görüntüleme

Örnek uygulamalar çoğunluğu en az iki sayfa içerir:

- **TodoListPage** – bu sayfayı bir listesini görüntüler `TodoItem` örnekleri ve onay simgesi varsa `TodoItem.Done` özelliği `true`. Bir öğeyi tıklatarak gider `TodoItemPage`. Ayrıca, yeni öğeler tıklayarak oluşturulabilir  *+*  simgesi.
- **TodoItemPage** – bu sayfa seçilen ayrıntılarını görüntüler `TodoItem`ve düzenlenebilir, kaydedildi, silinen konuşulan ve izin verir.

Ayrıca, bazı örnek uygulamalar kullanıcı kimlik doğrulaması işlemini yönetmek için kullanılan ek sayfalar içerir.

### <a name="modeling-the-data"></a>Veri modelleme

Her örnek uygulamanın kullandığı `TodoItem` görüntülenir ve depolama için web hizmetine gönderilen veri modeli için sınıf. Aşağıdaki örnekte gösterildiği kod `TodoItem` sınıfı:

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

`ID` Özelliği her benzersiz şekilde tanımlamak için kullanılan `TodoItem` örneği ve her web hizmeti tarafından güncelleştirilemez veya verileri tanımlamak için kullanılır.

### <a name="invoking-web-service-operations"></a>Web hizmeti işlemleri çağırma

Web hizmeti işlemleri yoluyla erişilir `TodoItemManager` üzerinden sınıfı ve sınıfının bir örneği erişilebilir `App.TodoManager` özelliği. `TodoItemManager` Sınıfı, web hizmeti işlemini çağırmak için aşağıdaki yöntemleri sağlar:

- **GetTasksAsync** – bu yöntem doldurmak için kullanılan `ListView` denetlemek `TodoListPage` ile `TodoItem` örnekleri web hizmetinden alındı.
- **SaveTaskAsync** – bu yöntem oluşturmak veya güncelleştirmek için kullanılan bir `TodoItem` web hizmeti örneğinde.
- **DeleteTaskAsync** – bu yöntem silmek için kullanılan bir `TodoItem` web hizmeti örneğinde.

Ayrıca, bazı örnek uygulamaları ek yöntemleri içeren `TodoItemManager` kullanıcı kimlik doğrulaması işlemini yönetmek için kullanılan sınıf.

Web hizmeti işlemleri doğrudan çağırmak yerine `TodoItemManager` yöntemleri çağırma içine eklenen bağımlı bir sınıfı yöntemleri `TodoItemManager` Oluşturucusu. Örneğin, bir örnek uygulama yerleştirir `SimpleDBStorage` içine sınıf `TodoItemManager` Amazon'ın SimpleDB Hizmeti'ne karşı işlemlerini çağıran bir uygulama sağlamak için Oluşturucusu.

### <a name="translating-text-to-speech"></a>Metin okuma çevirme

Örnek uygulamalar çoğunu içeren değerlerini konuşma metin okuma (TTS) işlevselliği `TodoItem.Name` ve `TodoItem.Notes` özellikleri. Bu tarafından gerçekleştirilir `OnSpeakActivated` olay işleyicisini `TodoItemPage` aşağıdaki kod örneğinde gösterildiği gibi sınıfı:

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

Bu yöntem yalnızca çağırır `Speak` platforma özgü tarafından uygulanan yöntemi `Speech` sınıfı. Her `Speech` uygulayan sınıf `ITextToSpeech` arabirimi, ve platforma özgü başlatma kodunu oluşturur örneği `Speech` üzerinden erişilen sınıfı `App.Speech` özelliği.

## <a name="summary"></a>Özet

Bu konuda farklı web Hizmetleri ile iletişim kurma göstermek için kullanılan Xamarin.Forms örnek uygulamayı bir kılavuz sağlanır. Her web hizmeti ayrı örnek bir uygulama kullanıyor, ancak bunlar tüm aynı kullanıcı arabirimi ve iş mantığına yukarıda açıklandığı gibi temel alır - yalnızca web hizmetini veri depolama mekanizmasını farklıdır.


## <a name="related-links"></a>İlgili bağlantılar

- [ASMX sürüm (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [WCF sürüm (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [REST sürüm (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Azure sürümü (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
- [Amazon Web Services sürüm (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWS)
