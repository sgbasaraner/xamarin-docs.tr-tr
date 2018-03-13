---
title: "Uygulama dizin oluşturma ve derin bağlama"
description: "Uygulama dizin arama sonuçlarında görünmesini ilgili kalmak için birkaç kullandıktan sonra Aksi takdirde unutulursa uygulamaları sağlar. Derin bağlama uygulamaların uygulama verilerini içeren, genellikle derin bir bağlantıdan başvurulan sayfasına giderek arama sonucu yanıt izin verir. Bu makalede, uygulama dizin kullanmayı ve Xamarin.Forms uygulaması içeriği iOS ve Android cihazlarda aranabilir yapmak için derin bağlama gösterilmektedir."
ms.topic: article
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 38d3b6da0dd33e038f2d50209280f2983faf6013
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="application-indexing-and-deep-linking"></a>Uygulama dizin oluşturma ve derin bağlama

_Uygulama dizin arama sonuçlarında görünmesini ilgili kalmak için birkaç kullandıktan sonra Aksi takdirde unutulursa uygulamaları sağlar. Derin bağlama uygulamaların uygulama verilerini içeren, genellikle derin bir bağlantıdan başvurulan sayfasına giderek arama sonucu yanıt izin verir. Bu makalede, uygulama dizin kullanmayı ve Xamarin.Forms uygulaması içeriği iOS ve Android cihazlarda aranabilir yapmak için derin bağlama gösterilmektedir._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Xamarin.Forms ve Azure ile derin göre bağlama [Xamarin Üniversitesi](https://university.xamarin.com/)**


Xamarin.Forms uygulaması dizin oluşturma ve derin bağlama kullanıcılar uygulamaları aracılığıyla gidin gibi uygulama dizin oluşturma için meta veri yayımlama için bir API sağlar. Dizin oluşturulmuş içerik ardından için Spotlight arama, Google araması veya bir web araması aranabilir. Dokunma derin bir bağlantı içeren bir arama sonucu üzerinde bir uygulama tarafından yönetilebilir ve genellikle derin bağlantıdan başvurulan sayfasına gitmek için kullanılan bir olayı ateşlenir.

Örnek uygulama, aşağıdaki ekran görüntülerinde gösterildiği gibi yerel bir SQLite veritabanı verilerinin depolandığı bir Yapılacaklar listesi uygulaması gösterir:

![](deep-linking-images/screenshots.png "Yapılacaklar listesi uygulaması")

Her `TodoItem` kullanıcı tarafından oluşturulan örnek dizini. Platforma özgü arama sonra uygulamasından dizinlenmiş veri bulmak için de kullanılabilir. Uygulama için bir arama sonucu öğesinin kullanıcı dokunur, uygulama başlatıldığında, `TodoItemPage` için gittiğinizde ve `TodoItem` derin başvurulan bağlantı görüntülenir.

Bir SQLite veritabanı kullanma hakkında daha fazla bilgi için bkz: [yerel bir veritabanı ile çalışan](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> Xamarin.Forms uygulaması dizin oluşturma ve derin işlev bağlama yalnızca iOS ve Android platformları üzerinde kullanılabilir ve iOS 9 ve API 23 sırasıyla gerektirir.

## <a name="setup"></a>Kurulum

Aşağıdaki bölümler iOS ve Android platformları üzerinde bu özelliği kullanmak için ek kurulum yönergeleri sağlar.

### <a name="ios"></a>iOS

İOS platformunda bu işlevselliği kullanmak için gerekli ek kurulumu yok.

### <a name="android"></a>Android

Android platformunda uygulama dizin oluşturma ve işlevsellik bağlama derin kullanmak için karşılanması gereken önkoşulları vardır:

1. Uygulamanızın sürümünü Google Play'deki dinamik olması gerekir.
1. Bir yardımcı Web uygulamasını Google Developer konsolunda karşı kayıtlı olması gerekir. Uygulama bir Web sitesi ile ilişkili olduğunda, URL'ler olabilir Web sitesi ve ardından arama sonuçlarında hizmet uygulaması için çalışma dizini. Daha fazla bilgi için bkz: [uygulama dizin Google arama üzerinde](https://support.google.com/googleplay/android-developer/answer/6041489) Google Web sitesinde.
1. Uygulamanızın üzerinde HTTP URL'si hedefleri desteklemelidir `MainActivity` uygulamanın URL veri şemalarını ne tür dizin uygulamaya bildirmek sınıfı için yanıt verebilir. Daha fazla bilgi için bkz: [hedefi filtresi yapılandırma](~/android/platform/app-linking.md#configure-intent-filter).

Bu Önkoşullar sağlandığında, aşağıdaki ek kurulum Xamarin.Forms uygulaması dizin oluşturma ve derin Android platformunda bağlama kullanmak için gereklidir:

1. Yükleme [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) Android uygulaması projesine NuGet paketi.
1. İçinde `MainActivity.cs` dosya, içeri aktarma `Xamarin.Forms.Platform.Android.AppLinks` ad alanı.
1. İçinde `MainActivity.OnCreate` geçersiz kılma, kodun aşağıdaki satırı ekleyin `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Daha fazla bilgi için bkz: [ayrıntılı bağlantı içerik Xamarin.Forms URL Gezinti ile](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) Xamarin blogunda.

## <a name="indexing-a-page"></a>Bir sayfa dizin oluşturma

Bir sayfa dizin oluşturma ve Google ve Spotlight arama gösterme işlemi aşağıdaki gibidir:

1. Oluşturma bir [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) kullanıcı arama sonuçlarında dizin oluşturulmuş içerik seçtiğinde sayfasına dönmek için ayrıntılı bağlantı birlikte sayfanın dizini oluşturmak için gerekli meta veriler içeriyor.
1. Kayıt [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) arama için dizin örneği.

Aşağıdaki kod örneğinde nasıl oluşturulduğunu gösteren bir [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) örneği:

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

[ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Örneğini içeren bir dizi özellikleri değerleri, sayfa dizini oluşturmak ve ayrıntılı bir bağlantı oluşturmak için gereklidir. [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/), [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/), Ve [ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/) özellikler arama sonuçlarında görüntülendiğinde dizin oluşturulmuş içerik tanımlamak için kullanılır. [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) Özelliği ayarlanmış `true` dizin oluşturulmuş içerik şu anda görüntülenen olduğunu belirtmek için. [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/) Özelliği bir `Uri` geçerli sayfasına dönün ve geçerli görüntülemek için gereken bilgileri içeren `TodoItem`. Aşağıda bir örnek gösterilir `Uri` örnek uygulama için:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

Bu `Uri` başlatmak için gerekli tüm bilgileri içeren `deeplinking` uygulamayı gidin `DeepLinking.TodoItemPage`ve görüntüleme `TodoItem` olan bir `ID` , `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>Dizin oluşturma işlemi için içerik kaydetme

Bir kez bir [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) örneği oluşturulan, arama sonuçlarında görüntülenmesi için dizin oluşturma için kaydedilmelidir. Bu ile gerçekleştirilir [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/) yöntemi, aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Bu ekler [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) uygulamanın örneğine [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) koleksiyonu.

> [!NOTE]
> `RegisterLink` Yöntemi de bir sayfa için dizinlenir içeriği güncelleştirmek için kullanılabilir.

Bir kez bir [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) örneği kaydettirildi dizin oluşturma işlemi için arama sonuçlarında yer alabilir. Aşağıdaki ekran görüntüsünde, iOS platformunda arama sonuçlarında görünmesini dizin oluşturulmuş içerik gösterir:

![](deep-linking-images/ios-search.png "İOS arama sonuçlarında dizin oluşturulmuş içerik")

## <a name="de-registering-indexed-content"></a>XML'deki kaydetme içerik dizini

[ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/) Yöntemi kullanılır dizin oluşturulmuş içerik Arama sonuçlarından kaldırmak için aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Bu kaldırır [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) uygulamanın örneğinden [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) koleksiyonu.

> [!NOTE]
> Android dizin oluşturulmuş içerik Arama sonuçlarından kaldırmak mümkün değildir.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Yanıt için ayrıntılı bağlantı

Dizin oluşturulmuş içerik arama sonuçlarında görünür ve bir kullanıcı tarafından seçildiğinde `App` uygulamayı işlemek için bir istek alacak için sınıf `Uri` dizinli içeriği yer. Bu isteğin işlenmesi [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) , aşağıdaki kod örneğinde gösterildiği gibi geçersiz kıl:

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

[ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) Yöntemi denetler alınan `Uri` ayrıştırma önce bir uygulama için amaçlanan `Uri` için ilk sayfasına ve sayfasına iletilecek parametre. Bir örneği oluşturulduğu için ilk sayfasına ve `TodoItem` temsil parametresi sayfa tarafından alınır. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) İçin gittiğinizde sayfanın sonra ayarlanır `TodoItem`. Bu olduğunda sağlar `TodoItemPage` tarafından görüntülenen [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/) yöntemi, onu gösteren `TodoItem` , `ID` derin bağlantıya yer alır.

## <a name="making-content-available-for-search-indexing"></a>İçerik arama dizin oluşturma için kullanılabilir hale getirme

Ayrıntılı bağlantı tarafından temsil edilen sayfası görüntülenirse, her zaman [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) özelliği ayarlanabilir `true`. Bu, iOS ve Android sağlar [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) ayrıca örneği yalnızca iOS ve arama dizini oluşturma için kullanılabilir kılar `AppLinkEntry` örneği iletimi için kullanılabilir. İletim hakkında daha fazla bilgi için bkz: [iletimi giriş](~/ios/platform/handoff.md).

Aşağıdaki kod örneğinde ayarı gösterilmektedir [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) özelliğine `true` içinde [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) geçersiz kıl:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Benzer şekilde, ne zaman derin bir bağlantı tarafından temsil edilen sayfa gittiğinizde merkezden, [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) özelliği ayarlanabilir `false`. Bu, iOS ve Android cihazlarda durdurur [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) iOS yalnızca, BT ve arama dizini oluşturma için de tanıtılan örneği durdurur reklam `AppLinkEntry` iletimi için örneği. Bu, gerçekleştirilebilir [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) , aşağıdaki kod örneğinde gösterildiği gibi geçersiz kıl:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Veri iletimi için sağlama

İos'ta, uygulamaya özgü verileri sayfa dizin oluştururken depolanabilir. Bu veri ekleyerek sağlanır [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) olan koleksiyon, bir `Dictionary<string, string>` iletimi içinde kullanılan anahtar-değer çiftlerini depolamak için. İletim, bir etkinlik cihazlarını birini başlatın ve bu etkinliği başka cihazlarını (kullanıcının iCloud hesabıyla tanımlandığı gibi) devam etmek kullanıcı için bir yoldur. Aşağıdaki kod, uygulamaya özgü anahtar-değer çiftleri depolamanın bir örnektir:

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Depolanan değerleri [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) toplama dizini oluşturulmuş sayfa için olan meta verilerde depolanan ve ayrıntılı bir bağlantı içeren (veya başka bir içeriği görüntülemek için Handoff'a kullanıldığında arama sonucu üzerinde kullanıcı dokunur zaman geri yüklenecek oturum açmış aygıt).

Ayrıca, aşağıdaki anahtarları için değerler belirtilebilir:

- `contentType` – bir `string` dizin oluşturulmuş içerik Tekdüzen türü tanımlayıcısını belirtir. Bu değer için kullanılacak önerilen kuralı dizinli içeriğin bulunduğu sayfa türü adıdır.
- `associatedWebPage` – bir `string` dizin oluşturulmuş içerik Web'de de görüntülenebilir veya uygulama Safari'nin ayrıntılı bağlantılar destekliyorsa ziyaret etmek için bu web sayfasını temsil eder.
- `shouldAddToPublicIndex` – bir `string` herhangi birinin `true` veya `false` sonra uygulama iOS cihazında yüklemediniz kullanıcılara sunulabilir Apple'nın genel bulut dizini oluşturulmuş içerik eklemek gerekip gerekmediğini denetler. Yalnızca içeriği ortak dizin oluşturma işlemi için sahip ayarlanmadığından, ancak bunu Apple'nın genel bulut dizini otomatik olarak eklenen gelmez. Daha fazla bilgi için bkz: [ortak arama dizini oluşturma](~/ios/platform/search/nsuseractivity.md). Bu anahtar için ayarlanmalıdır Not `false` kişisel veri eklerken [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) koleksiyonu.

> [!NOTE]
> `KeyValues` Koleksiyonu Android platformunda kullanılan değil.

İletim hakkında daha fazla bilgi için bkz: [iletimi giriş](~/ios/platform/handoff.md).

## <a name="summary"></a>Özet

Bu makalede, uygulama dizin kullanmayı ve Xamarin.Forms uygulaması içeriği iOS ve Android cihazlarda aranabilir yapmak için derin bağlama gösterilmektedir. Uygulama dizin oluşturma işlemi birkaç kullandıktan sonra ilgili Aksi takdirde unutulursa arama sonuçlarında görünmesini tarafından ilgili kalmak uygulamaları sağlar.


## <a name="related-links"></a>İlgili bağlantılar

- [Derin bağlama (örnek)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [iOS arama API'leri](~/ios/platform/search/index.md)
- [Android 6.0 uygulama olarak bağlama](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
