---
title: Uygulama dizini oluşturma ve ayrıntılı bağlantı sağlama
description: Bu makalede, uygulama dizini oluşturma, kullanma ve Xamarin.Forms uygulama içeriğinin iOS ve Android cihazlarda aranabilir hale getirmek için ayrıntılı bağlantı sağlama gösterir.
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 7a102765a3633b8abaf01b3f090d8253230bc16b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996102"
---
# <a name="application-indexing-and-deep-linking"></a>Uygulama dizini oluşturma ve ayrıntılı bağlantı sağlama

_Uygulama dizini oluşturma, arama sonuçlarında görünmesini tarafından kalmak için birkaç'ı kullandıktan sonra Aksi takdirde unutmuş uygulamalar sağlar. Ayrıntılı bağlantı sağlama uygulamalarının ayrıntılı bağlantı başvurulan sayfasına giderek uygulama verileri, genellikle içeren bir arama sonucunun yanıt olanak sağlar. Bu makalede, uygulama dizini oluşturma, kullanma ve Xamarin.Forms uygulama içeriğinin iOS ve Android cihazlarda aranabilir hale getirmek için ayrıntılı bağlantı sağlama gösterir._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Xamarin.Forms ve Azure ile ayrıntılı olarak bağlama [Xamarin University](https://university.xamarin.com/)**


Xamarin.Forms uygulaması dizin oluşturma ve ayrıntılı bağlantı sağlama kullanıcılar uygulamaları aracılığıyla gezinirken uygulama dizini oluşturma için meta veri yayımlama için bir API sağlar. Dizinlenmiş içeriği ardından için Spotlight arama, Google arama veya bir web araması aranabilir. Dokunma ayrıntılı bağlantısını içeren bir arama sonucunda bir uygulama tarafından işlenebilir ve genellikle derin bağlantıdan başvurulan sayfaya gitmek için kullanılan bir olay ateşlenir.

Örnek uygulama, aşağıdaki ekran görüntülerinde gösterildiği gibi yerel bir SQLite veritabanından verilerin depolandığı bir Yapılacaklar listesi uygulamasını gösterir:

![](deep-linking-images/screenshots.png "Yapılacaklar listesi uygulaması")

Her `TodoItem` örneği kullanıcı tarafından oluşturulan tarihine. Platforma özgü arama ardından uygulamadan dizinlenmiş verileri bulmak için de kullanılabilir. Kullanıcı bir uygulama için arama sonucu öğesi üzerinde dokunduğunda uygulamanın açıldığında `TodoItemPage` için gittiğinizde ve `TodoItem` derin başvurulan bağlantı görüntülenir.

Bir SQLite veritabanı kullanma hakkında daha fazla bilgi için bkz. [yerel bir veritabanı ile çalışmaya](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> Xamarin.Forms uygulaması dizin oluşturma ve bağlama işlevselliği derin yalnızca iOS ve Android platformları üzerinde kullanılabilir ve iOS 9 ve API 23 sırasıyla gerektirir.

## <a name="setup"></a>Kurulum

Aşağıdaki bölümler iOS ve Android platformları üzerinde bu özelliği kullanmak için ek kurulum yönergeleri sağlar.

### <a name="ios"></a>iOS

İOS platformunda bu işlevselliği kullanmak için gerekli ek kurulum yoktur.

### <a name="android"></a>Android

Android platformunda uygulama dizini oluşturma ve derin bağlama işlevselliği kullanmak için karşılanması gereken önkoşulları vardır:

1. Uygulamanızın sürümünü Google Play'den dinamik olması gerekir.
1. Google'nın Geliştirici Konsolu uygulamasında karşı Yardımcısı Web sitesi kayıtlı olması gerekir. Uygulama bir Web sitesi ile ilişkili olduğunda, URL'ler olabilir hem Web sitesi ve arama sonuçlarında sunulabilen uygulama için çalışma dizini. Daha fazla bilgi için [App Indexing Google aramada](https://support.google.com/googleplay/android-developer/answer/6041489) Google'nın Web sitesinde.
1. Uygulamanızın HTTP URL'si hedefleri üzerinde desteklemelidir `MainActivity` sınıfını, ki bu uygulamanın URL veri şemalarını hangi türde dizin uygulamaya bildirmek için yanıt verebilir. Daha fazla bilgi için [hedefi filtresini yapılandırma](~/android/platform/app-linking.md#configure-intent-filter).

Bu Önkoşullar sağlandığında, aşağıdaki ek kurulum Xamarin.Forms uygulaması dizin oluşturma ve ayrıntılı bağlantı sağlama Android platformunda kullanmak için gereklidir:

1. Yükleme [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) Android uygulaması projenizin NuGet paketi.
1. İçinde `MainActivity.cs` dosya, içeri aktarma `Xamarin.Forms.Platform.Android.AppLinks` ad alanı.
1. İçinde `MainActivity.OnCreate` geçersiz kılmak, kodun aşağıdaki satırı ekleyin `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Daha fazla bilgi için [ayrıntılı bağlantı içerik Xamarin.Forms URL gezintisi ile](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) Xamarin blogundan.

## <a name="indexing-a-page"></a>Sayfa dizini oluşturma

İçin Google ve Spotlight arama gösterme ve bir sayfa dizin oluşturma işlemi aşağıdaki gibidir:

1. Oluşturma bir [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) kullanıcı dizinlenmiş içeriği arama sonuçlarında seçtiğinde sayfasına dönmek için ayrıntılı bağlantı birlikte sayfanın dizini oluşturmak için gerekli meta veriler içerir.
1. Kayıt [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) aramada dizine eklemek örneği.

Aşağıdaki kod örneğinde nasıl oluşturulacağını gösterir. bir [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) örneği:

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

[ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Örneği içeren bir dizi özellik değerleri, sayfa dizini ve ayrıntılı bir bağlantı oluşturmak için gereklidir. [ `Title` ](xref:Xamarin.Forms.IAppLinkEntry.Title), [ `Description` ](xref:Xamarin.Forms.IAppLinkEntry.Description), Ve [ `Thumbnail` ](xref:Xamarin.Forms.IAppLinkEntry.Thumbnail) özellikleri dizinlenmiş içeriği arama sonuçlarında görüntülendiğinde tanımlamak için kullanılır. [ `IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) Özelliği `true` dizinlenmiş içeriği şu anda görüntülenen olduğunu belirtmek için. [ `AppLinkUri` ](xref:Xamarin.Forms.IAppLinkEntry.AppLinkUri) Özelliği bir `Uri` geçerli sayfaya geri dönün ve geçerli görüntülemek için gereken bilgileri içeren `TodoItem`. Aşağıdaki örnek, bir örnek gösterir `Uri` örnek uygulama için:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

Bu `Uri` başlatmak için gerekli tüm bilgileri içeren `deeplinking` uygulamayı gidin `DeepLinking.TodoItemPage`ve görüntüleme `TodoItem` olan bir `ID` , `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>İçerik dizinleme için kaydetme

Bir kez bir [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) örneği oluşturulan, arama sonuçlarında görünmesini dizin oluşturma için kaydedilmelidir. Bu ile gerçekleştirilir [ `RegisterLink` ](xref:Xamarin.Forms.IAppLinks.RegisterLink(Xamarin.Forms.IAppLinkEntry)) yöntemini aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Bu ekler [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) uygulamanın örneğine [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) koleksiyonu.

> [!NOTE]
> `RegisterLink` Yöntemi ayrıca bir sayfa için dizine alınmaz içeriği güncelleştirmek için kullanılabilir.

Bir kez bir [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) örneği kaydedilmedi dizin oluşturma işlemi için arama sonuçlarında görünebilir. Aşağıdaki ekran görüntüsünde, iOS platformunda arama sonuçlarında görünmesini dizinlenmiş içeriği gösterir:

![](deep-linking-images/ios-search.png "Dizinlenmiş içeriği arama sonuçlarında iOS")

## <a name="de-registering-indexed-content"></a>Kaydını içerik dizini

[ `DeregisterLink` ](xref:Xamarin.Forms.IAppLinks.DeregisterLink(Xamarin.Forms.IAppLinkEntry)) Yöntemi kullanılır dizinlenmiş içeriği Arama sonuçlarından kaldırmak için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Bu kaldırır [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) uygulama örneğinden [ `AppLinks` ](xref:Xamarin.Forms.Application.AppLinks) koleksiyonu.

> [!NOTE]
> Android'de dizinlenmiş içeriği Arama sonuçlarından kaldırmak mümkün değildir.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Ayrıntılı bağlantısını yanıt

Dizinlenmiş içeriği arama sonuçlarında görünür ve bir kullanıcı tarafından seçildiğinde `App` bir isteği işlemek için uygulamanın alacağı için sınıf `Uri` dizinlenmiş içeriği içeriyor. Bu istek işlenebilir [ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) aşağıdaki kod örneğinde gösterildiği gibi geçersiz:

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

[ `OnAppLinkRequestReceived` ](xref:Xamarin.Forms.Application.OnAppLinkRequestReceived(System.Uri)) Yöntemi denetler alınan `Uri` ayrıştırma önce uygulama için amaçlanan `Uri` için geçtiğiniz için sayfayı ve sayfaya geçirilecek parametre. Bir örneği oluşturulduğu için geçtiğiniz için sayfanın ve `TodoItem` temsil parametresi sayfa tarafından alınır. [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) İçin geçtiğiniz için sayfanın sonra ayarlanır `TodoItem`. Bu zaman sağlar `TodoItemPage` tarafından görüntülenen [ `PushAsync` ](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page)) yöntemi göstermeyi `TodoItem` olan `ID` ayrıntılı bağlantı içinde yer alır.

## <a name="making-content-available-for-search-indexing"></a>İçerik arama dizini oluşturma için kullanılabilir hale getirme

Ayrıntılı bağlantı tarafından temsil edilen sayfası görüntülenirse, her zaman [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) özelliği ayarlanabilir `true`. İOS ve Android'de bu kılar [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) Ayrıca örnek arama dizini oluşturma için ve yalnızca ios'ta kullanılabilir kılar `AppLinkEntry` örneği iletim için kullanılabilir. İletim hakkında daha fazla bilgi için bkz: [giriş animasyonuna](~/ios/platform/handoff.md).

Aşağıdaki kod örneği ayarı gösterir [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) özelliğini `true` içinde [ `Page.OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) geçersiz kıl:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Benzer şekilde, ne zaman derin bir bağlantı tarafından temsil edilen sayfası çıkıldığında liste kutusundan, [ `AppLinkEntry.IsLinkActive` ](xref:Xamarin.Forms.IAppLinkEntry.IsLinkActive) özelliği ayarlanabilir `false`. Bu, iOS ve Android üzerinde durdurur [ `AppLinkEntry` ](xref:Xamarin.Forms.AppLinkEntry) arama dizini oluşturma ve iOS yalnızca BT de tanıtılan örneği durdurur reklam `AppLinkEntry` iletim için örneği. Bu, içinde gerçekleştirilebilir [ `Page.OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) aşağıdaki kod örneğinde gösterildiği gibi geçersiz:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>İletim için veri sağlama

İOS, uygulamaya özgü veri sayfası dizin oluşturulurken depolanabilir. Bu verileri ekleyerek gerçekleştirilir [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) olan koleksiyon bir `Dictionary<string, string>` iletim içinde kullanılan anahtar-değer çiftlerini depolamak için. İletim kullanıcı cihazlarını birinde bir etkinlik başlatmak ve bu etkinliği başka kullanıcıların cihazlarını (kullanıcının iCloud hesabıyla tarafından tanımlandığı gibi) devam etmek için bir yoldur. Aşağıdaki kod, uygulamaya özgü anahtar-değer çiftlerini depolamak için bir örnek göstermektedir:

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Depolanan değerler [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) koleksiyon dizinli sayfası için meta veriler depolanır ve kullanıcı ayrıntılı bağlantısını içeren (veya başka içeriği görüntülemek için Handoff'a kullanıldığında bir arama sonucunda dokunduğunda geri yüklenecek cihaz) oturum açma.

Ayrıca şu anahtarlara yönelik değerler belirtilebilir:

- `contentType` – bir `string` dizinlenmiş içeriği Tekdüzen tür tanımlayıcısını belirtir. Bu değer için kullanılacak önerilen kuralı dizinlenmiş içeriği içeren bir sayfa türü adıdır.
- `associatedWebPage` – bir `string` dizinlenmiş içeriği web üzerinde de görüntülenebilir veya Safari'nin ayrıntılı bağlantılar uygulamanın desteklediği durumlarda ziyaret etmek için web sayfasını temsil eder.
- `shouldAddToPublicIndex` – bir `string` herhangi birinin `true` veya `false` uygulama iOS cihazında yüklü olmayan kullanıcılara sunulabilir Apple'nın genel bulut dizini, dizinlenmiş içeriği eklemek gerekip gerekmediğini denetler. Yalnızca içerik genel dizin oluşturma için ayarlandı, ancak bunu Apple'nın genel bulut dizini otomatik olarak eklenmeden bırakılır gelmez. Daha fazla bilgi için [genel arama dizini oluşturma](~/ios/platform/search/nsuseractivity.md). Bu anahtar için ayarlanmalıdır Not `false` kişisel verileri eklerken [ `KeyValues` ](xref:Xamarin.Forms.IAppLinkEntry.KeyValues) koleksiyonu.

> [!NOTE]
> `KeyValues` Koleksiyon Android platformunda kullanılmaz.

İletim hakkında daha fazla bilgi için bkz: [giriş animasyonuna](~/ios/platform/handoff.md).

## <a name="summary"></a>Özet

Bu makalede, uygulama dizini oluşturma, kullanma ve Xamarin.Forms uygulama içeriğinin iOS ve Android cihazlarda aranabilir hale getirmek için ayrıntılı bağlantı sağlama gösterilmiştir. Uygulama dizini oluşturma birkaç kullandıktan sonra ilgili unutmasına arama sonuçlarında görünmesini tarafından kalmak uygulamalar sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Ayrıntılı bağlantı (örnek)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS arama API'leri](~/ios/platform/search/index.md)
- [Android 6.0 uygulama bağlama](~/android/platform/app-linking.md)
- [AppLinkEntry](xref:Xamarin.Forms.AppLinkEntry)
- [IAppLinkEntry](xref:Xamarin.Forms.IAppLinkEntry)
- [IAppLinks](xref:Xamarin.Forms.IAppLinks)
