---
title: Yapılandırma yönetimi
description: Bu bölümde, uygulama ayarları ve kullanıcı ayarlarını sağlamak için yapılandırma yönetimi eShopOnContainers mobil uygulamayı nasıl uyguladığını açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d6cd9771760bc2932345fec24887842ce1c47376
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243957"
---
# <a name="configuration-management"></a>Yapılandırma yönetimi

Ayarlar, uygulama derlenmeden değiştirilecek davranışı izin vererek uygulama kodundan davranışını yapılandırır veri ayrımı sağlar. İki tür ayar vardır: uygulama ayarları ve kullanıcı ayarları.

Uygulama ayarları, bir uygulama oluşturur ve yönetir verilerdir. Sabit web hizmeti uç noktaları, API anahtarlarını ve çalışma zamanı durumu gibi verileri içerebilir. Uygulama ayarları, uygulamanın varlığını bağlıdır ve yalnızca bu uygulama için anlamlıdır.

Kullanıcı, uygulamanın davranışı etkiler ve sık sık yeniden ayarlama gerektirmeyen özelleştirilebilir ayarları, bir uygulamanın ayarlardır. Örneğin, bir uygulama kullanıcının verileri almak nereye ve ekranda görüntülemek nasıl belirtmesine sağlayabilir.

Xamarin.Forms ayarları verilerini depolamak için kullanılan kalıcı bir sözlük içerir. Bu sözlüğü kullanılarak erişilebilir [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) özelliği ve içine yerleştirilen herhangi bir veri uygulama uyku moduna geçer ve uygulama sürdürür veya yeniden başlatıldığı zaman geri kaydedilir. Ayrıca, [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) sınıfı ayrıca sahip bir [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) gerektiğinde kaydedilmiş ayarlarını sağlamak bir uygulamanın sağlar yöntemi. Bu sözlük hakkında daha fazla bilgi için bkz: [özellikleri sözlüğüne](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Xamarin.Forms kalıcı sözlüğünü kullanarak veri depolama için bir dezavantajı, kolayca bağlı veri olmamasıdır. Bu nedenle, kullanılabilir Xam.Plugins.Settings kitaplığı eShopOnContainers mobil uygulamanın kullandığı [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Bu kitaplık kalıcı yapma ve her platformu tarafından sağlanan yerel ayarları yönetimi kullanırken uygulama ve kullanıcı ayarlarını almak için tutarlı, tür kullanımı uyumlu, platformlar arası bir yaklaşımdır. Ayrıca, veri bağlama kitaplığı tarafından sunulan ayarlar verilere erişmek için kullanılacak basittir.

> [!NOTE]
> Xam.Plugin.Settings kitaplığı uygulama ve kullanıcı ayarlarını saklarken iki arasında ayrım sağlar.

## <a name="creating-a-settings-class"></a>Ayarları sınıfı oluşturma

Xam.Plugins.Settings kitaplığı kullanırken, tek bir statik sınıf, oluşturulmalıdır uygulama tarafından gerekli uygulama ve kullanıcı ayarlarını içerir. Aşağıdaki kod örneğinde eShopOnContainers mobil uygulamaya ayarları sınıfı gösterir:

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Ayarları okumak ve aracılığıyla yazılmış `ISettings` Xam.Plugins.Settings kitaplığı tarafından sağlanan API. Bu API erişmek için kullanılan bir singleton sağlar `CrossSettings.Current`, ve bir uygulamanın ayarları sınıfı aracılığıyla bu singleton maruz bırakmamalısınız bir `ISettings` özelliği.

> [!NOTE]
> Xam.Plugins.Settings kitaplık türleri erişim gerektiren bir sınıf Plugin.Settings ve Plugin.Settings.Abstractions ad alanları için yönergeleri kullanarak eklenmelidir.

## <a name="adding-a-setting"></a>Bir ayarı ekleme

Her ayar, bir anahtar, varsayılan bir değer ve bir özellik oluşur. Aşağıdaki kod örneği, tüm üç öğeler eShopOnContainers mobil uygulama bağlandığı Çevrimiçi Hizmetler için temel URL'yi temsil eden bir kullanıcı ayarı için gösterir:

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

Anahtarı her zaman anahtar adını tanımlayan bir const dizedir, ayar için varsayılan değer ile gerekli tür statik salt okunur değeri olması. Varsayılan değer sağlayan geçerli bir değer ayarlanmamış bir ayarı alındıysa kullanılabilir olmasını sağlar.

`UrlBase` Statik özelliği kullanan iki yöntemleri `ISettings` API ayar değeri okunamıyor veya yazılamıyor. `ISettings.GetValueOrDefault` Yöntemi platforma özgü depolama biriminden bir ayarın değerini almak için kullanılır. Hiçbir değer ayar için tanımlanmış olması durumunda, varsayılan değer yerine alınır. Benzer şekilde, `ISettings.AddOrUpdateValue` yöntemi bir ayarın değerini platforma özgü depolama için kalıcı hale getirmek için kullanılır.

Varsayılan değer yerine tanımlayan `Settings` sınıfı, `UrlBaseDefault` dize değerini alır `GlobalSetting` sınıfı. Aşağıdaki örnekte gösterildiği kod `BaseEndpoint` özelliği ve `UpdateEndpoint` bu sınıftaki yöntemi:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Her zaman `BaseEndpoint` özelliği ayarlanmış `UpdateEndpoint` yöntemi çağrılır. Bu yöntem, temel özellikleri, bir dizi güncelleştirmeleri `UrlBase` tarafından sağlanan kullanıcı ayarı `Settings` eShopOnContainers mobil uygulama bağlandığı farklı uç noktalar temsil eden sınıf.

## <a name="data-binding-to-user-settings"></a>Veri bağlama kullanıcı ayarları

EShopOnContainers mobil uygulamaya `SettingsView` iki kullanıcı ayarlarını gösterir. Bu ayarlar olup uygulama veri Docker kapsayıcılar olarak dağıtılan mikro almak veya uygulama bir Internet bağlantısı gerektirmeyen sahte hizmetlerinden veri almak yapılandırılmasını sağlar. Kapsayıcılı mikro verileri almak üzere seçerken, mikro hizmetler için bir temel uç nokta URL'si belirtilmelidir. Şekil 7-1 gösterir `SettingsView` zaman kullanıcı seçmiştir kapsayıcılı mikro verileri.

![](configuration-management-images/settings-endpoint.png "EShopOnContainers mobil uygulama tarafından kullanıma sunulan kullanıcı ayarları")

**Şekil 7-1**: eShopOnContainers mobil uygulama tarafından kullanıma sunulan kullanıcı ayarları

Veri bağlama, almak ve tarafından sunulan ayarlar için kullanılabilir `Settings` sınıfı. Bu sırayla özelliklerinde erişim görünüm model özellikleri görünümü bağlamayı denetimlere tarafından sağlanır `Settings` sınıfı ve bir özellik oluşturma ayarları değer değiştiyse bildirim değiştirildi. Modeller ve görünümlere ilişkilendirir nasıl eShopOnContainers mobil uygulama görünümü oluşturur hakkında bilgi için bkz: [otomatik olarak bir görünüm modeli bulucuyla görünüm Model oluşturma](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Aşağıdaki örnekte gösterildiği kod [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) gelen denetim `SettingsView` bir temel uç nokta URL'si için kapsayıcılı mikro girmesini sağlar:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Bu [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) denetim bağlanır `Endpoint` özelliği `SettingsViewModel` sınıfı, iki yönlü bir bağlama işlemini kullanma. Aşağıdaki kod örneğinde, uç nokta özelliği gösterilmektedir:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Zaman `Endpoint` özelliği ayarlanmış `UpdateEndpoint` yöntemi çağrıldığında, sağlanan değer geçerli ve özelliği değiştirildi koşuluyla bildirim tetiklenir. Aşağıdaki örnekte gösterildiği kod `UpdateEndpoint` yöntemi:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Bu yöntem güncelleştirmeleri `UrlBase` özelliğinde `Settings` temel uç nokta URL'si değeri sınıfıyla girilen platforma özgü depolama alanına kalıcı neden olur kullanıcı tarafından.

Zaman `SettingsView` için gittiğinizde `InitializeAsync` yönteminde `SettingsViewModel` sınıfı gerçekleştirilir. Aşağıdaki kod örneği, bu yöntem gösterilmektedir:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Yöntem kümeleri `Endpoint` özellik değerine `UrlBase` özelliğinde `Settings` sınıfı. Erişme `UrlBase` özellik, platforma özgü depolama biriminden ayarları değerini almak Xam.Plugins.Settings kitaplığı eklenmesine neden olur. Ne hakkında bilgi için `InitializeAsync` yöntemi çağrıldığında, bkz: [gezinti sırasında geçirme parametreleri](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Bu mekanizma bir kullanıcı için SettingsView gider olduğunda, kullanıcı ayarları platforma özgü depolama biriminden alınan ve veri bağlama aracılığıyla sunulan sağlar. Ardından, kullanıcı ayarları değerlerini değiştirirse, veri bağlama bunlar hemen platforma özgü depolama birimi kalıcı yapıldığını sağlar.

## <a name="summary"></a>Özet

Ayarlar, uygulama derlenmeden değiştirilecek davranışı izin vererek uygulama kodundan davranışını yapılandırır veri ayrımı sağlar. Uygulama ayarları, bir uygulama oluşturur ve yönetir verileri ve kullanıcı ayarları, uygulamanın davranışı etkiler ve sık sık yeniden ayarlama gerektirmeyen özelleştirilebilir bir uygulamanın ayarlardır.

Tutarlı bir tür kullanımı uyumlu Xam.Plugins.Settings kitaplığı sağlar, uygulama ve kullanıcı ayarlarını ve veri bağlama alma ve sürdürmek için platformlar arası yaklaşımı kitaplığı ile oluşturduğunuz ayarları erişmek için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
