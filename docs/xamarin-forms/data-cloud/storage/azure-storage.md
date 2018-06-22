---
title: Depolama ve Azure depolama biriminde bulunan verilere erişim
description: Azure Storage yapılandırılmamış ve yapılandırılmış verileri depolamak için kullanılan bir ölçeklenebilir bulut depolama çözümüdür. Bu makalede Xamarin.Forms metin ve ikili verileri Azure depolama alanında depolamak için nasıl kullanılacağı ve verilere nasıl erişileceğini gösterilmektedir.
ms.prod: xamarin
ms.assetid: 5B10D37B-839B-4CD0-9C65-91014A93F3EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
ms.openlocfilehash: 63afeec81eff350b034e8dd3a13da52801937826
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30789878"
---
# <a name="storing-and-accessing-data-in-azure-storage"></a>Depolama ve Azure depolama biriminde bulunan verilere erişim

_Azure Storage yapılandırılmamış ve yapılandırılmış verileri depolamak için kullanılan bir ölçeklenebilir bulut depolama çözümüdür. Bu makalede Xamarin.Forms metin ve ikili verileri Azure depolama alanında depolamak için nasıl kullanılacağı ve verilere nasıl erişileceğini gösterilmektedir._

## <a name="overview"></a>Genel Bakış

Azure depolama dört depolama hizmetleri sağlar:

- BLOB Depolama. Bir blob yedeklemeler, sanal makineler, medya dosyaları ya da belgeler gibi metin veya ikili veri olabilir.
- Table Storage bir NoSQL anahtar öznitelik deposudur.
- Kuyruk depolama, iş akışı işleme ve bulut hizmetleri arasında iletişim için Mesajlaşma bir hizmettir.
- Dosya depolama SMB protokolünü kullanarak paylaşılan depolama alanı sağlar.

İki tür depolama hesabı vardır:

- Genel amaçlı depolama hesapları tek bir hesaptan Azure Storage hizmetlerine erişim sağlar.
- Blob storage hesabı, blobları depolamak için bir özel depolama hesabıdır. Bu hesap türü, yalnızca blob verilerini depolamak gerektiğinde önerilir.

Bu makalede ve örnek uygulama ile birlikte gelen blob depolama ve bunları indirmek için karşıya yükleme resim ve metin dosyaları gösterir. Ayrıca, ayrıca blob depolama alanından dosyaların bir listesini almak ve dosya silme gösterir.

Azure Storage hakkında daha fazla bilgi için bkz: [depolama giriş](https://azure.microsoft.com/documentation/articles/storage-introduction/).

## <a name="introduction-to-blob-storage"></a>Blob Storage'a giriş

BLOB Depolama aşağıdaki çizimde gösterilen üç bileşenden oluşur:

![](azure-storage-images/blob-storage.png "BLOB Storage kavramları")

Azure Storage tüm erişimi bir depolama hesabıdır. Bir depolama hesabı sınırsız sayıda kapsayıcı içerebilir ve bir kapsayıcı depolama hesabının kapasite sınırını dolduracak kadar BLOB sınırsız sayıda depolayabilirsiniz.

Bir blob herhangi türde ve size bir dosyadır. Azure Storage üç farklı blob türlerini destekler:

- Blok blobları, akış ve bulut nesnelerini depolamak için en iyi duruma getirilir ve yedeklemeleri, ortam dosyaları ve belgeler vb. depolamak için iyi bir seçimdir. Blok blobları 195 Gb boyutunda olabilir.
- Ekleme blobları blok bloblarına benzer ancak için iyileştirilmiş ekleme günlüğe kaydetme gibi işlemleri. Ekleme blobları 195Gb boyutunda olabilir.
- Sayfa bloblarını sık okuma/yazma işlemleri için en iyi duruma getirilir ve genellikle sanal makineler ve bunların diskleri depolamak için kullanılır. Sayfa bloblarını boyutu 1 TB'ye kadar olabilir.

> [!NOTE]
> BLOB storage hesapları blok desteklemek ve BLOB'lar, ancak sayfa bloblarını desteklemez ilave unutmayın.

Bir blob, Azure depolama alanına yüklenir ve bir bayt akış Azure Storage'dan indirilen. Bu nedenle, dosyaları karşıya yükleme ve indirme sonra özgün kendi gösterimi dönüştürülen geri önce bayt akış dönüştürülmesi gerekir.

Azure depolama alanında depolanır her nesnenin benzersiz bir URL adresi vardır. Depolama hesabı adı o adresi ve alt etki alanı ve etki alanı adı forms birleşimi alt formları bir *endpoint* depolama hesabı için. Örneğin, depolama hesabınızın adı *mystorageaccount*, depolama hesabı için varsayılan blob uç noktası `https://mystorageaccount.blob.core.windows.net`.

Bir depolama hesabındaki bir nesneye erişilirken URL'si nesnenin konumda depolama hesabı uç noktaya eklenmesiyle oluşturulur. Örneğin, bir blob adresi biçimi olur `https://mystorageaccount.blob.core.windows.net/mycontainer/myblob`.

## <a name="setup"></a>Kurulum

Bir Azure depolama hesabı bir Xamarin.Forms uygulamayla tümleştirmek için işlem aşağıdaki gibidir:

1. Bir depolama hesabı oluşturun. Daha fazla bilgi için bkz: [depolama hesabı oluşturma](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account).
1. Ekleme [Azure Storage istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage/) Xamarin.Forms uygulaması için.
1. Depolama bağlantı dizesi yapılandırın. Daha fazla bilgi için bkz: [Azure depolama alanına bağlanılırken](#connecting).
1. Ekleme `using` yönergeleri için `Microsoft.WindowsAzure.Storage` ve `Microsoft.WindowsAzure.Storage.Blob` Azure Storage erişecek sınıfları için ad alanları.

> [!NOTE]
> Bu örnek bir paylaşılan erişim proje kullanırken, Azure Storage istemci kitaplığı şimdi de taşınabilir sınıf kitaplığı (PCL) projeden tüketilen destekler.

<a name="connecting" />

## <a name="connecting-to-azure-storage"></a>Azure depolama alanına bağlanma

Depolama hesabı kaynaklarına karşı yapılan her isteğin kimliğinin doğrulanması gerekir. Blobları anonim kimlik doğrulamayı destekleyecek şekilde yapılandırılmış olsa da, bir uygulama bir depolama hesabıyla kimlik doğrulaması yapmak için kullanabileceğiniz iki ana yaklaşım vardır:

- Paylaşılan anahtar. Bu yaklaşım, depolama hizmetlerine erişmek için Azure Storage hesabı adını ve hesap anahtarını kullanır. Bir depolama hesabı, paylaşılan anahtar kimlik doğrulaması için kullanılan oluşturulduktan iki özel anahtarları atanır.
- Paylaşılan erişim imzası. Bu, bir depolama kaynağına yetkilendirilmiş erişim sağlayan URL'ye eklenebilen bir belirteç, izinlere sahip, geçerli olduğu süre boyunca belirtir.

Bir uygulamadan Azure depolama kaynaklarına erişmek için gereken kimlik doğrulama bilgilerini içeren bağlantı dizeleri belirtilebilir. Ayrıca, bir bağlantı dizesi, Visual Studio'dan Azure Storage öykünücüsü bağlanmak için yapılandırılabilir.

> [!NOTE]
> Azure Storage HTTP ve HTTPS bağlantı dizesinde destekler. Ancak, HTTPS kullanılması önerilir.

### <a name="connecting-to-the-azure-storage-emulator"></a>Azure Storage öykünücüsü bağlanma

Azure depolama öykünücüsü Azure blob, kuyruk ve Tablo Hizmetleri Geliştirme amaçlı öykünen yerel bir ortam sağlar.

Aşağıdaki bağlantı dizesi için Azure Storage öykünücüsü bağlanmak için kullanılan:

```csharp
UseDevelopmentStorage=true
```

Azure Storage öykünücüsü hakkında daha fazla bilgi için bkz: [geliştirme ve sınama için Azure Storage öykünücüsünü kullanma](https://azure.microsoft.com/documentation/articles/storage-use-emulator/).

### <a name="connecting-to-azure-storage-using-a-shared-key"></a>Paylaşılan bir anahtar kullanarak Azure depolama alanına bağlanma

Paylaşılan anahtar ile Azure depolama birimine bağlamak için aşağıdaki bağlantı dizesi biçimi kullanılmalıdır:

```csharp
DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey
```

`myAccountName` Depolama hesabınızın adı ile değiştirilmelidir ve `myAccountKey` iki hesap erişim tuşlarınızı biri ile değiştirilmelidir.

> [!NOTE]
> Depolama hesabı tam okuma/yazma erişimi sağlayan uygulamanızı kullanan herkes kullanarak anahtar kimlik doğrulaması, hesap adı ve hesap anahtarı zaman paylaşılan dağıtılacaktır. Bu nedenle, yalnızca sınama amacıyla paylaşılan anahtar kimlik doğrulaması kullanmak ve hiçbir zaman anahtarları diğer kullanıcılara dağıtabilirsiniz.

### <a name="connecting-to-azure-storage-using-a-shared-access-signature"></a>Paylaşılan erişim imzası kullanarak Azure depolama birimine bağlama

Azure Storage bir SAS ile bağlanmak için aşağıdaki bağlantı dizesi biçimi kullanılmalıdır:

`BlobEndpoint=myBlobEndpoint;SharedAccessSignature=mySharedAccessSignature`

`myBlobEndpoint` blob uç noktanızı, URL ile değiştirilmelidir ve `mySharedAccessSignature` , SAS ile değiştirilmelidir. SAS protokolü, hizmet uç noktası ve kaynağa erişim için kimlik bilgilerini sağlar.

> [!NOTE]
> SAS kimlik doğrulaması, üretim uygulamaları için önerilir. Ancak, bir üretim uygulamasında SAS uygulama ile birlikte yerine bir arka uç hizmeti isteğe bağlı alınması.

Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [kullanarak paylaşılan erişim imzaları (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).

## <a name="creating-a-container"></a>Bir kapsayıcı oluşturma

`GetContainer` Yöntemi, daha sonra kapsayıcıdan BLOB'ları almak veya BLOB kapsayıcıya eklemek için kullanılabilir bir adlandırılmış kapsayıcı için bir başvuru almak için kullanılır. Aşağıdaki örnekte gösterildiği kod `GetContainer` yöntemi:

```csharp
static CloudBlobContainer GetContainer(ContainerType containerType)
{
  var account = CloudStorageAccount.Parse(Constants.StorageConnection);
  var client = account.CreateCloudBlobClient();
  return client.GetContainerReference(containerType.ToString().ToLower());
}
```

`CloudStorageAccount.Parse` Yöntemi bir bağlantı dizesi ayrıştırır ve döndüren bir `CloudStorageAccount` depolama hesabını temsil eden örneği. A `CloudBlobClient` kapsayıcılar ve bloblar almak için kullanılan örneği tarafından oluşturulan sonra `CreateCloudBlobClient` yöntemi. `GetContainerReference` Yöntemi alır belirtilen kapsayıcı olarak bir `CloudBlobContainer` çağıran yönteme döndürülmeden önce örneği. Bu örnekte, kapsayıcı addır `ContainerType` numaralandırma değeri, küçük bir dizeye dönüştürülür.

> [!NOTE]
> Kapsayıcı adları küçük harfli olması gerektiğini ve bir harf veya sayı ile başlamalıdır. Buna ek olarak, bunlar yalnızca harf, rakam ve tire karakter içerebilir ve 3 ila 63 karakter uzunluğunda olmalıdır.

`GetContainer` Yöntemi gibi çağrılır:

```csharp
var container = GetContainer(containerType);
```

`CloudBlobContainer` Örneği kullanılabilecek zaten yoksa, bir kapsayıcı oluşturmak için:

```csharp
await container.CreateIfNotExistsAsync();
```

Varsayılan olarak, yeni oluşturulan bir kapsayıcı özeldir. Başka bir deyişle, bir depolama erişim tuşu kapsayıcıdan BLOB'ları almak için belirtilmelidir. İçinde bir kapsayıcı genel BLOB'lar yapma hakkında daha fazla bilgi için bkz: [bir kapsayıcı oluşturmak](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#create-a-container).

## <a name="uploading-data-to-a-container"></a>Bir kapsayıcıya karşıya veri yükleme

`UploadFileAsync` Yöntemi bir BLOB depolamaya bayt veri akışı karşıya yüklemek için kullanılır ve aşağıdaki kod örneğinde gösterilir:

```csharp
public static async Task<string> UploadFileAsync(ContainerType containerType, Stream stream)
{
  var container = GetContainer(containerType);
  await container.CreateIfNotExistsAsync();

  var name = Guid.NewGuid().ToString();
  var fileBlob = container.GetBlockBlobReference(name);
  await fileBlob.UploadFromStreamAsync(stream);

  return name;
}
```

Bir kapsayıcı başvurusu aldıktan sonra zaten yoksa yöntemi, kapsayıcı oluşturur. Yeni bir `Guid` sonra benzersiz blob adı olarak davranacak şekilde oluşturulur ve bir blob blok başvurusu olarak alınan bir `CloudBlockBlob` örneği. Veri akışı blob kullanmaya sonra karşıya `UploadFromStreamAsync` önceden var olmayan veya mevcut değilse bu raporun üzerine blob oluşturur yöntemi.

BLOB depolamaya bu yöntemi kullanarak bir dosyayı karşıya önce ilk olarak bir bayt akışa dönüştürülmelidir. Bu, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
var byteData = Encoding.UTF8.GetBytes(text);
uploadedFilename = await AzureStorage.UploadFileAsync(ContainerType.Text, new MemoryStream(byteData));
```

`text` Veri sonra geçirilir bir akış olarak kaydırılan bir bayt dizisine dönüştürülür `UploadFileAsync` yöntemi.

## <a name="downloading-data-from-a-container"></a>Bir kapsayıcı veri indirme

`GetFileAsync` Yöntemi blob verilerini Azure Storage'dan indirmek için kullanılır ve aşağıdaki kod örneğinde gösterilir:

```csharp
public static async Task<byte[]> GetFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);

  var blob = container.GetBlobReference(name);
  if (await blob.ExistsAsync())
  {
    await blob.FetchAttributesAsync();
    byte[] blobBytes = new byte[blob.Properties.Length];

    await blob.DownloadToByteArrayAsync(blobBytes, 0);
    return blobBytes;
  }
  return null;
}
```

Yöntemi, bir kapsayıcı başvurusu aldıktan sonra depolanan veriler için bir blob başvurusu alır. Blob varsa özelliklerini tarafından alınır `FetchAttributesAsync` yöntemi. Bir bayt dizisi doğru boyutta oluşturulur ve blob çağıran yönteme döndürülmeden bayt dizisi olarak yüklenir.

Blob bayt veri indirdikten sonra kendi özgün gösterimine dönüştürülmesi gerekir. Bu, aşağıdaki kod örneğinde gösterilmiştir:

```csharp
var byteData = await AzureStorage.GetFileAsync(ContainerType.Text, uploadedFilename);
string text = Encoding.UTF8.GetString(byteData);
```

Bir bayt dizisi Azure Storage alınır `GetFileAsync` UTF8 için geri dönüştürülmeden önce yöntemi kodlanmış dize.

## <a name="listing-data-in-a-container"></a>Verileri bir kapsayıcıda listeleme

`GetFilesListAsync` Yöntemi BLOB'ları bir kapsayıcıda depolanan listesini almak için kullanılır ve aşağıdaki kod örneğinde gösterilir:

```csharp
public static async Task<IList<string>> GetFilesListAsync(ContainerType containerType)
{
  var container = GetContainer(containerType);

  var allBlobsList = new List<string>();
  BlobContinuationToken token = null;

  do
  {
    var result = await container.ListBlobsSegmentedAsync(token);
    if (result.Results.Count() > 0)
    {
      var blobs = result.Results.Cast<CloudBlockBlob>().Select(b => b.Name);
      allBlobsList.AddRange(blobs);
    }
    token = result.ContinuationToken;
  } while (token != null);

  return allBlobsList;
}
```

Bir kapsayıcı başvurusu aldıktan sonra kapsayıcının kullanmaktadır `ListBlobsSegmentedAsync` kapsayıcısı içinde BLOB'ları başvuruları alma yöntemi. Tarafından döndürülen sonuçlar `ListBlobsSegmentedAsync` yöntemi numaralandırılan sırada `BlobContinuationToken` örnek `null`. Her bir blob cast döndürülen gelen `IListBlobItem` için bir `CloudBlockBlob` sipariş Access'te `Name` özellik değeri önce BLOB eklenir `allBlobsList` koleksiyonu. Bir kez `BlobContinuationToken` örneği `null`son blob adı döndürdü ve yürütme döngüsü çıkar.

## <a name="deleting-data-from-a-container"></a>Bir kapsayıcı verileri silme

`DeleteFileAsync` Yöntemi kapsayıcıdan blob silmek için kullanılır ve aşağıdaki kod örneğinde gösterilir:

```csharp
public static async Task<bool> DeleteFileAsync(ContainerType containerType, string name)
{
  var container = GetContainer(containerType);
  var blob = container.GetBlobReference(name);
  return await blob.DeleteIfExistsAsync();
}
```

Yöntemi, bir kapsayıcı başvurusu aldıktan sonra belirtilen blob için bir blob başvurusu alır. Blob ile silinir `DeleteIfExistsAsync` yöntemi.

## <a name="summary"></a>Özet

Bu makalede Xamarin.Forms metin ve ikili verileri Azure depolama alanında depolamak için nasıl kullanılacağı ve verilere nasıl erişileceğini gösterilmektedir. Azure Storage yapılandırılmamış ve yapılandırılmış verileri depolamak için kullanılan bir ölçeklenebilir bulut depolama çözümüdür.


## <a name="related-links"></a>İlgili bağlantılar

- [Azure depolama (örnek)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/AzureStorage/)
- [Depolama giriş](https://azure.microsoft.com/documentation/articles/storage-introduction/)
- [BLOB depolama alanından Xamarin kullanma](https://azure.microsoft.com/documentation/articles/storage-xamarin-blob-storage/)
- [Paylaşılan erişim imzaları (SAS) kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)
- [Windows Azure depolama](https://www.nuget.org/packages/WindowsAzure.Storage/)
