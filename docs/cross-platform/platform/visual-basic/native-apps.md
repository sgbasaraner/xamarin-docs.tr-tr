---
title: Visual Basic.NET dilinde Xamarin iOS ve Android
description: "Visual Basic.NET dilinde yazılan iş mantığı kullanan yerel Xamarin.iOS ve Xamarin.Android uygulamalar oluşturmak nasıl XThis izlenecek yol gösterir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 455fda67-3879-4299-8036-b12840e6a498
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: dc107ee865ea93cdc12148a5498cf3d512f1dae9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="visual-basicnet-in-xamarin-ios-and-android"></a>Visual Basic.NET dilinde Xamarin iOS ve Android

[TaskyPortable](/samples/mobile/VisualBasic/TaskyPortableVB/) örnek uygulama Xamarin ile Visual Basic kodu taşınabilir Sınıf Kitaplığı'na derlenmiş nasıl kullanılabileceğini gösterir. Bazı ekran görüntüleri iOS, Android ve Windows Phone çalıştıran ortaya çıkan uygulamalar şunlardır:

 [ ![](native-apps-images/image5.png "iOS, Android ve Windows telefonlar Visual Basic ile oluşturulan bir uygulamayı çalıştırma")](native-apps-images/image5.png)

İOS, Android ve Windows Phone projeleri örnekte tüm C# dilinde yazılmıştır. Her uygulama için kullanıcı arabirimi yerel teknolojilerle oluşturulmuştur (film şeritleri, Xml ve Xaml sırasıyla), ancak `TodoItem` Yönetimi Visual Basic taşınabilir sınıf kitaplığı tarafından sağlanan kullanarak bir `IXmlStorage` tarafından sağlanan uygulama Yerel projesi.

## <a name="sample-walkthrough"></a>Örnek gözden geçirme

Bu kılavuz, Visual Basic nasıl uygulanmıştır anlatılmaktadır [TaskyPortableVB](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB) iOS ve Android için Xamarin örnek.

> ⚠️ Yönergeleri gözden [Visual Basic.NET PCLs](/guides/cross-platform/application_fundamentals/pcl/portable_visual_basic_net/) bu kılavuzla devam etmeden önce.

## <a name="visualbasicportablelibrary"></a>VisualBasicPortableLibrary

Visual Basic taşınabilir sınıf kitaplıkları yalnızca Visual Studio'da oluşturulabilir.
Örnek kitaplığı uygulamamız dört Visual Basic dosyalarında temel bilgileri içerir:

-  IXmlStorage.vb
-  TodoItem.vb
-  TodoItemManager.vb
-  TodoItemRepositoryXML.vb


### <a name="ixmlstoragevb"></a>IXmlStorage.vb

Dosya erişim davranışları şekilde önemli ölçüde platformları arasında farklılık gösterdiğinden, taşınabilir sınıf kitaplıkları sunmaz devam `System.IO` depolama API'leri herhangi bir profil dosyası. Bu bizim taşınabilir kodda dosya sistemi ile doğrudan etkileşim istiyoruz, biz bizim yerel projelerine her platformu üzerinde geri arama gerektiği anlamına gelir.  C# ' ta her platformda uygulanabilir basit bir arabirim karşı bizim Visual Basic kodu yazarak hala dosya sistemi erişimi paylaşılabilir Visual Basic kodu olabilir.

Örnek kod yalnızca iki yöntem içerir bu çok basit bir arabirim kullanır: Okuma ve yazma seri hale getirilmiş bir Xml dosyası.

```vb
Public Interface IXmlStorage
    Function ReadXml(filename As String) As List(Of TodoItem)
    Sub WriteXml(tasks As List(Of TodoItem), filename As String)
End Interface
```

iOS, Android ve Windows Phone uygulamalarında bu arabirim için kılavuzda daha sonra gösterilecek.

### <a name="todoitemvb"></a>TodoItem.vb

Bu sınıf uygulama genelinde kullanılacak iş nesnesi içerir. Visual Basic'te tanımlanmış olması ve iOS, Android ve Windows Phone paylaşılan C# dilinde yazılmıştır projeleri.

Sınıf tanımı aşağıda gösterilmiştir:

```vb
Public Class TodoItem
    Property ID() As Integer
    Property Name() As String
    Property Notes() As String
    Property Done() As Boolean
End Class
```

Örnek, yüklemek ve Todoıtem nesneleri kaydetmek için XML serileştirme ve serinin kullanır.

### <a name="todoitemmanagervb"></a>TodoItemManager.vb

Yönetici sınıfı 'API' için taşınabilir kodu gösterir. Temel CRUD işlemleri için sağlar `TodoItem` sınıfı, ancak bu işlem uygulaması.

```vb
Public Class TodoItemManager
    Private _repository As TodoItemRepositoryXML
    Public Sub New(filename As String, storage As IXmlStorage)
        _repository = New TodoItemRepositoryXML(filename, storage)
    End Sub
    Public Function GetTask(id As Integer) As TodoItem
        Return _repository.GetTask(id)
    End Function
    Public Function GetTasks() As List(Of TodoItem)
        Return New List(Of TodoItem)(_repository.GetTasks())
    End Function
    Public Function SaveTask(item As TodoItem) As Integer
        Return _repository.SaveTask(item)
    End Function
    Public Function DeleteTask(item As TodoItem) As Integer
        Return _repository.DeleteTask(item.ID)
    End Function
End Class
```

Oluşturucusu IXmlStorage örneği parametre olarak alır. Bu, hala paylaşılabilir diğer işlevlerini açıklayan taşınabilir kod izin vererek sırasında kendi çalışma uygulama sunmak amacıyla her bir platform sağlar.

### <a name="todoitemrepositoryvb"></a>TodoItemRepository.vb

Depo sınıfını Todoıtem nesnelerin listesini yönetmek için mantığını içerir. Tam kod aşağıda gösterilmektedir – eklenir ve koleksiyondan kaldırıldı gibi benzersiz bir kimlik değeri arasında Todoıtems çoğunlukla yönetmek için mantığı bulunmaktadır.

```vb
Public Class TodoItemRepositoryXML
    Private _filename As String
    Private _storage As IXmlStorage
    Private _tasks As List(Of TodoItem)

    ''' <summary>Constructor</summary>
    Public Sub New(filename As String, storage As IXmlStorage)
        _filename = filename
        _storage = storage
        _tasks = _storage.ReadXml(filename)
    End Sub
    ''' <summary>Inefficient search for a Task by ID</summary>
    Public Function GetTask(id As Integer) As TodoItem
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                Return _tasks(t)
            End If
        Next
        Return New TodoItem() With {.ID = id}
    End Function
    ''' <summary>List all the Tasks</summary>
    Public Function GetTasks() As IEnumerable(Of TodoItem)
        Return _tasks
    End Function
    ''' <summary>Save a Task to the Xml file
    ''' Calculates the ID as the max of existing IDs</summary>
    Public Function SaveTask(item As TodoItem) As Integer
        Dim max As Integer = 0
        If _tasks.Count > 0 Then
            max = _tasks.Max(Function(t As TodoItem) t.ID)
        End If
        If item.ID = 0 Then
            item.ID = ++max
            _tasks.Add(item)
        Else
            Dim j = _tasks.Where(Function(t) t.ID = item.ID).First()
            j = item
        End If
        _storage.WriteXml(_tasks, _filename)
        Return max
    End Function
    ''' <summary>Removes the task from the XMl file</summary>
    Public Function DeleteTask(id As Integer) As Integer
        For t As Integer = 0 To _tasks.Count - 1
            If _tasks(t).ID = id Then
                _tasks.RemoveAt(t)
                _storage.WriteXml(_tasks, _filename)
                Return 1
            End If
        Next
        Return -1
    End Function
End Class
```

> ℹ️ **Not:** Bu kod örneği çok temel veri depolama mekanizmasını yer almaktadır.
> Platforma özgü işlevselliği (yükleme ve kaydetme bir Xml dosyası bu durumda,) erişmek için bir arabirim karşı nasıl taşınabilir sınıf kitaplığı kod göstermek için sağlanmıştır.
> Bu, bir üretim kaliteli veritabanı alternatif olması amaçlanmamıştır.

## <a name="ios-android-and-windows-phone-application-projects"></a>iOS, Android ve Windows Phone Uygulama projeleri

Bu bölümde IXmlStorage arabirimi için platforma özel uygulamaları içerir ve her iki uygulamada nasıl kullanılacağını gösterir. Uygulama projeleri tüm C# dilinde yazılmıştır.

### <a name="ios-and-android-ixmlstorage"></a>iOS ve Android IXmlStorage

Xamarin.iOS ve Xamarin.Android sağlayan tam `System.IO` kolayca yükleyin ve aşağıdaki sınıfı kullanarak Xml dosyasını kaydetmek için işlevselliği:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        if (File.Exists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new FileStream(filename, FileMode.Open))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(filename))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

İOS uygulamada `TodoItemManager` ve `XmlStorageImplementation` oluşturulan **AppDelegate.cs** dosya bu kod parçacığında gösterildiği gibi. İlk dört satırı yalnızca veri depolanacağı dosyasının yolu oluşturmakta olduğunuz; Son iki satırı oluşturulmasını iki sınıfları gösterir.

```csharp
var xmlFilename = "TodoList.xml";
string documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine(documentsPath, "..", "Library"); // Library folder
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Android uygulamada `TodoItemManager` ve `XmlStorageImplementation` oluşturulan **Application.cs** dosya bu kod parçacığında gösterildiği gibi. İlk üç satırını yalnızca veri depolanacağı dosyasının yolu oluşturmakta olduğunuz; Son iki satırı oluşturulmasını iki sınıfları gösterir.

```csharp
var xmlFilename = "TodoList.xml";
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
var path = Path.Combine(libraryPath, xmlFilename);
var xmlStorage = new AndroidTodo.XmlStorageImplementation();
TaskMgr = new TodoItemManager(path, xmlStorage);
```

Kullanıcı arabirimi ile ilgilenen ve kullanarak uygulama kodunu kalan öncelikle `TaskMgr` yüklemek ve kaydetmek için sınıf `TodoItem` sınıfları.

### <a name="windows-phone-ixmlstorage"></a>Windows Phone IXmlStorage

Windows Phone, bunun yerine IsolatedStorage API gösterme cihazın dosya sistemi için tam erişim sağlamaz. `IXmlStorage` Windows Phone için uygulama aşağıdaki gibi görünür:

```csharp
public class XmlStorageImplementation : IXmlStorage
{
    public XmlStorageImplementation(){}
    public List<TodoItem> ReadXml(string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        if (fileStorage.FileExists(filename))
        {
            var serializer = new XmlSerializer(typeof(List<TodoItem>));
            using (var stream = new StreamReader(new IsolatedStorageFileStream(filename, FileMode.Open, fileStorage)))
            {
                return (List<TodoItem>)serializer.Deserialize(stream);
            }
        }
        return new List<TodoItem>();
    }
    public void WriteXml(List<TodoItem> tasks, string filename)
    {
        IsolatedStorageFile fileStorage = IsolatedStorageFile.GetUserStoreForApplication();
        var serializer = new XmlSerializer(typeof(List<TodoItem>));
        using (var writer = new StreamWriter(new IsolatedStorageFileStream(filename, FileMode.OpenOrCreate, fileStorage)))
        {
            serializer.Serialize(writer, tasks);
        }
    }
}
```

`TodoItemManager` Ve `XmlStorageImplementation` oluşturulan **App.xaml.cs** dosya bu kod parçacığında gösterildiği gibi.

```csharp
var filename = "TodoList.xml";
var xmlStorage = new XmlStorageImplementation();
TodoMgr = new TodoItemManager(filename, xmlStorage);
```

Windows Phone Uygulama kalan Xaml ve C# kullanıcı arabirimi oluşturma ve kullanma oluşan `TodoMgr` yüklemek ve kaydetmek için sınıf `TodoItem` nesneleri.

# <a name="visual-basic-pcl-in-visual-studio-for-mac"></a>Visual Basic PCL Mac için Visual Studio'da

Mac için Visual Studio, Visual Basic Dil desteklemez – oluşturamaz veya Mac için Visual Studio ile Visual Basic projeleri derleme

Visual Studio için taşınabilir sınıf kitaplıkları için Mac'in destek Visual Basic'ten oluşturulan PCL derlemeleri başvurusu yapabilir anlamına gelir.

Bu bölümde, Visual Studio PCL derlemede derlemek ve ardından onu bir sürüm denetim sisteminde depolanan diğer projeler tarafından başvurulan emin olun ve açıklanmaktadır.

## <a name="keeping-the-pcl-output-from-visual-studio"></a>Visual Studio'dan PCL çıkış tutma

Varsayılan olarak, çoğu sürüm denetim sistemleri (TFS ve Git gibi) yoksaymak için yapılandırılacak **/bin/** derlenmiş PCL derleme anlamı dizin depolanmayacak. Başka bir deyişle, el ile bir başvuru eklemek Mac için Visual Studio çalıştıran tüm bilgisayarlara kopyalamanız gerekir.

Sürüm denetim sisteminiz PCL derleme çıktı depolayabilir emin olmak için proje kök kopyalar oluşturma sonrası komut dosyası oluşturabilirsiniz. Bu oluşturma sonrası adımı derleme kolayca kaynak denetimine eklenebilir ve diğer projelerle paylaşılan sağlamaya yardımcı olur.

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Projeye sağ tıklayın ve seçin **Özellikler > Yapı olayları** bölümü.

2. Ekleme bir _oluşturma sonrası_ DLL çıkış bu projeden proje kök dizine kopyalar. komut dosyası (olduğu dışında **/bin/**). Sürüm denetimi yapılandırmanıza bağlı olarak, DLL artık kaynak denetimine eklenmesi mümkün olması gerekir.

  [ ![](native-apps-images/image6-vs-sml.png "VB DLL kopyalamak için son yapı betik derleme olayları")](native-apps-images/image6-vs.png)

### <a name="visual-studio-2015"></a>Visual Studio 2015

1.  Projeye sağ tıklayın ve seçin **Özellikler > derleme** , ardından tüm yapılandırmaları sol üst Tarak kutusunda seçildiğinden emin olun. Tıklatın **derleme olaylarını...**  sağ alt düğmesini.

  [ ![](native-apps-images/image6.png "Proje Özellikleri derleme bölümü")](native-apps-images/image6.png)

1.  DLL çıkış bu projeden proje kök dizine kopyalar oluşturma sonrası komut dosyası ekleme (olduğu dışında **/bin/** ). Sürüm denetimi yapılandırmanıza bağlı olarak, DLL artık kaynak denetimine eklenmesi mümkün olması gerekir.

  [ ![](native-apps-images/image7.png "Olayları penceresi oluşturma")](native-apps-images/image7.png)

### <a name="all-versions"></a>Tüm sürümler

Sonraki projeyi oluşturun, taşınabilir sınıf kitaplığı derleme proje kök ve eriştiğinizde, onay-içinde/commit/itme değişikliklerinizi DLL kopyalanacak (bunu Visual Studio ile bir Mac üzerine Mac için indirilebilir böylece) depolanır.

  [ ![](native-apps-images/image8-sml.png "Visual Basic derleme çıktı dosyası konumu")](native-apps-images/image8.png)


Visual Basic Dil Xamarin iOS veya Android projeleri desteklenmiyor olsa bile bu derleme Mac için Xamarin Visual Studio projelerinde sonra eklenebilir.

## <a name="referencing-the-pcl-in-visual-studio-for-mac"></a>Mac için Visual Studio PCL başvurma

Xamarin Visual Basic desteklemediğinden, PCL proje (veya Windows Phone Uygulama) bu ekran görüntüsünde gösterildiği gibi yüklenemedi:

 [ ![](native-apps-images/image9.png "Visual Studio Mac çözüm için")](native-apps-images/image9.png)

Visual Basic PCL derleme DLL hala Xamarin.iOS ve Xamarin.Android projelerinde içerebilir:

1.  Sağ tıklayın **başvuruları** düğümü ve select **başvuruları Düzenle...**

  [ ![](native-apps-images/image10.png "Proje düzenleme başvuruları menüsü")](native-apps-images/image10.png)

1.  Seçin **.Net derleme** sekmesinde ve Visual Basic proje dizininde DLL çıkış gidin. Mac için Visual Studio Proje açılamıyor olsa bile, tüm dosyaları kaynak denetiminden var olmalıdır. Tıklatın **Ekle** sonra **Tamam** iOS ve Android uygulamaları bu derleme eklemek için.

  [ ![](native-apps-images/image11-sml.png "Ekle Tamam iOS ve Android uygulamaları bu derleme eklemek için tıklatın")](native-apps-images/image11.png)

1.  Artık iOS ve Android uygulamaları Visual Basic taşınabilir sınıf kitaplığı tarafından sağlanan uygulama mantığını ekleyebilirsiniz. Bu ekran, Visual Basic PCL başvuran ve bu kitaplıktan işlevi kullanır koduna sahip bir iOS uygulaması gösterir.

  [ ![](native-apps-images/image12-sml.png "Düzen başvurular ekleyin .NET derleme penceresi")](native-apps-images/image12.png)


En son Visual Studio Visual Basic projesinde unutmayın projeyi oluşturmak için sonuçta elde edilen derleme DLL kaynak denetiminde depolamak ve böylece derlemeler Mac için Visual Studio bu yeni DLL Mac üzerine kaynak denetiminden çekme değişiklikler yapılırsa içerir işlevselliği.


## <a name="summary"></a>Özet

Bu makalede gösterilmiştir nasıl Visual Studio ve taşınabilir sınıf kitaplıkları kullanarak Xamarin uygulamaları Visual Basic kodunda kullanabilir. Xamarin Visual Basic doğrudan desteklemez olsa bile, PCL Visual Basic derleme iOS ve Android uygulamaları dahil edilecek Visual Basic ile yazılan kod sağlar.

## <a name="related-links"></a>İlgili bağlantılar

- [TaskyPortableVB (örnek)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [.NET Framework (Microsoft) ile platformlar arası geliştirme](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
