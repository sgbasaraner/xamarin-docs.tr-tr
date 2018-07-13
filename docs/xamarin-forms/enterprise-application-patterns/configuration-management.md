---
title: Yapılandırma yönetimi
description: Bu bölümde, hizmetine mobil uygulama yapılandırma yönetimi, uygulama ayarları ve kullanıcı ayarlarını sağlamak için nasıl uyguladığını açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 6f32d8f328232bdfc644da57bdb3201c60010063
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995367"
---
# <a name="configuration-management"></a>Yapılandırma yönetimi

Ayarlar, uygulamanın derlenmeden değiştirilecek davranış sağlayan bir kodu uygulamadan davranışını yapılandıran veri ayrımı sağlar. Ayarları iki tür vardır: uygulama ayarları ve kullanıcı ayarları.

Uygulama ayarları, bir uygulama oluşturur ve yönetir verilerdir. Bu sabit web hizmeti uç noktalarını, API anahtarları ve çalışma zamanı durumu gibi veri içerebilir. Uygulama ayarları, uygulamanın varlığını bağlıdır ve yalnızca bu uygulama için anlamlı olan.

Kullanıcı, bir uygulama, uygulama davranışını etkiler ve sık sık yeniden ayarlama gerektirmeyen özelleştirilebilir ayarlarını ayarlarıdır. Örneğin, bir uygulama, verileri almak nereye ve ekranda görüntüleme şeklini belirttiğiniz kullanıcı sağlayabilir.

Xamarin.Forms ayarları verileri depolamak için kullanılan kalıcı bir sözlük içerir. Bu sözlük kullanılarak erişilebilir [ `Application.Current.Properties` ](xref:Xamarin.Forms.Application.Properties) özelliği ve içine yerleştirilir herhangi bir veri uygulama uyku moduna geçer ve uygulama devam ettirir veya yeniden başlatıldığı zaman geri kaydedilir. Ayrıca, [ `Application` ](xref:Xamarin.Forms.Application) sınıfı de sahip bir [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) gerektiğinde, kaydettiğiniz ayarlarına sahip bir uygulama sağlar yöntemi. Bu sözlük hakkında daha fazla bilgi için bkz: [özellikleri sözlüğüne](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Xamarin.Forms kalıcı sözlük kullanarak verileri depolamak için bir dezavantajı, kolayca bağlı veri olmamasıdır. Bu nedenle, hizmetine mobil uygulamada bulunan Xam.Plugins.Settings Kitaplığı'nı kullanan [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Bu kitaplık kalıcı hale getirmeniz ve her bir platform tarafından sağlanan yerel ayar Yönetimi'ni kullanırken uygulama ve kullanıcı ayarlarını almak için tutarlı, tür açısından güvenli, platformlar arası bir yaklaşım sağlar. Ayrıca, veri bağlama kitaplığı tarafından kullanıma sunulan ayarları verilere erişmek için kullanılacak oldukça basittir.

> [!NOTE]
> Xam.Plugin.Settings Kitaplığı hem uygulama hem de kullanıcı ayarları depolayabilir, ancak ikisi arasında bir ayrım yapar.

## <a name="creating-a-settings-class"></a>Ayarları sınıfı oluşturma

Xam.Plugins.Settings Kitaplığı kullanıldığında, tek bir statik sınıf, oluşturulması gereken uygulama tarafından gerekli olan uygulama ve kullanıcı ayarları içerir. Aşağıdaki kod örneği, hizmetine mobil uygulamada ayarları sınıfı gösterir:

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

Ayarları okumak ve aracılığıyla yazılan `ISettings` Xam.Plugins.Settings kitaplığı tarafından sağlanan API. Bu kitaplık API'ye erişmek için kullanılan tek sağlar `CrossSettings.Current`, ve bir uygulamanın ayarlar sınıfındaki bu singleton aracılığıyla açığa bir `ISettings` özelliği.

> [!NOTE]
> Using yönergeleri Plugin.Settings ve Plugin.Settings.Abstractions ad alanları için Xam.Plugins.Settings kitaplık türleri için erişim gerektiren bir sınıfa eklenmelidir.

## <a name="adding-a-setting"></a>Bir ayarı ekleme

Her bir ayar, bir anahtar, varsayılan bir değer ve bir özellik oluşur. Aşağıdaki kod örneği, tüm üç öğeler mobil uygulama hizmetine bağlanan Çevrimiçi Hizmetler için temel URL'yi temsil eden bir kullanıcı ayarı için gösterir:

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

Anahtarı her zaman anahtar adını tanımlayan bir sabit dize, gerekli türünde bir statik salt okunur değer ayarı için varsayılan değerine sahip olan. Varsayılan değer sağlayan geçerli bir değer ayarlanmamış bir ayarı almışsa kullanılabilir olmasını sağlar.

`UrlBase` Statik özelliği kullanan iki yöntemlerinden `ISettings` API ayar değeri okunamıyor veya yazılamıyor. `ISettings.GetValueOrDefault` Yöntemi, platforma özgü depolama biriminden bir ayarın değerini almak için kullanılır. Hiçbir değer ayarı tanımlanmazsa, varsayılan değerine yerine alınır. Benzer şekilde, `ISettings.AddOrUpdateValue` yöntemi, bir ayarın değerini platforma özgü depolama için kalıcı hale getirmek için kullanılır.

Varsayılan değer yerine tanımlayan `Settings` sınıfı `UrlBaseDefault` dize değerini alır `GlobalSetting` sınıfı. Aşağıdaki örnekte gösterildiği kod `BaseEndpoint` özelliği ve `UpdateEndpoint` bu sınıftaki yöntemi:

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

Her zaman `BaseEndpoint` özelliği ayarlanmış `UpdateEndpoint` yöntemi çağrılır. Bu yöntem bir dizi özellik, her biri temel güncelleştirmeler `UrlBase` tarafından sağlanan kullanıcı ayarı `Settings` hizmetine mobil uygulama bağlanan farklı uç noktalar temsil eden sınıf.

## <a name="data-binding-to-user-settings"></a>Veri bağlama için kullanıcı ayarları

Hizmetine mobil uygulamasında `SettingsView` iki kullanıcı ayarları kullanıma sunar. Bu ayarlar olup uygulama veri Docker kapsayıcıları olarak dağıtılmış bir mikro hizmetler almak veya uygulama bir Internet bağlantısı gerektirmez sahte hizmetlerinden verileri almak yapılandırmasını sağlar. Kapsayıcılı mikro hizmetler verileri almak üzere seçerken, mikro hizmetler için bir temel uç nokta URL'si belirtilmelidir. Şekil 7-1 gösterir `SettingsView` zaman kullanıcı seçmiştir veri kapsayıcılı mikro hizmetler.

![](configuration-management-images/settings-endpoint.png "Hizmetine mobil uygulama tarafından kullanıma sunulan kullanıcı ayarları")

**Şekil 7-1**: hizmetine mobil uygulama tarafından kullanıma sunulan kullanıcı ayarları

Veri bağlama, almak ve tarafından sunulan ayarlar için kullanılabilir `Settings` sınıfı. Bu sırayla özelliklerine erişmek görünüm modeli özellikleri görünümü bağlama denetimleri tarafından sağlanır `Settings` sınıf ve özellik yükseltme ayarları değeri değiştiyse bildirim değiştirildi. Modeller ve bunları görünümlerine ilişkilendirir nasıl hizmetine mobil uygulama görünüm yapıları hakkında bilgi için bkz. [otomatik olarak bir görünüm modeli bulucuyla bir görünüm modeli oluşturma](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Aşağıdaki örnekte gösterildiği kod [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi `SettingsView` kapsayıcılı mikro hizmetler için bir temel uç nokta URL'si girmesini sağlar:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Bu [ `Entry` ](xref:Xamarin.Forms.Entry) t:System.Windows.Forms.Binding için `Endpoint` özelliği `SettingsViewModel` sınıfı, iki yönlü bir bağlama kullanarak. Aşağıdaki kod örneği, uç nokta özelliğinde gösterir:

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

Zaman `Endpoint` özelliği `UpdateEndpoint` yöntemi çağrıldığında, sağlanan değer geçerli ve özelliği koşuluyla bildirim oluşturulur. Aşağıdaki örnekte gösterildiği kod `UpdateEndpoint` yöntemi:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Bu yöntem güncelleştirmeleri `UrlBase` özelliğinde `Settings` sınıfı temel uç nokta URL'si değeri ile girilen platforma özgü Depolama'da kalıcı hale neden olur kullanıcı tarafından.

Zaman `SettingsView` için çıkıldığında `InitializeAsync` yönteminde `SettingsViewModel` sınıfı yürütülür. Aşağıdaki kod örneği, bu yöntem gösterir:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Yöntem kümeleri `Endpoint` değere `UrlBase` özelliğinde `Settings` sınıfı. Erişim `UrlBase` özelliği, platforma özgü depolama alanından ayarları değerini almak Xam.Plugins.Settings kitaplığı neden olur. Hakkında nasıl bilgi `InitializeAsync` metodunu çağırmak için bkz: [gezinti sırasında parametre geçirme](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Bu mekanizma, bir kullanıcı için SettingsView gittiğinde her kullanıcı ayarları platforma özgü depolama alanından alınır ve veri bağlama aracılığıyla sunulan olmasını sağlar. Ardından kullanıcı ayarları değerlerini değiştirirse, veri bağlama, hemen platforma özgü depolama birimi kaybolacağından sağlar.

## <a name="summary"></a>Özet

Ayarlar, uygulamanın derlenmeden değiştirilecek davranış sağlayan bir kodu uygulamadan davranışını yapılandıran veri ayrımı sağlar. Uygulama ayarları, bir uygulama oluşturur ve yönetir verilerinin ve uygulama davranışını etkiler ve sık sık yeniden ayarlama gerektirmeyen özelleştirilebilir ayarları bir uygulamanın kullanıcı ayarlarıdır.

Tutarlı, tür açısından güvenli Xam.Plugins.Settings kitaplığı sağlar, kalıcı hale getirmeniz ve uygulama ve kullanıcı ayarlarını ve veri bağlama için platformlar arası bir yaklaşım kitaplığı ile oluşturulan ayarlara erişmek için kullanılabilir.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
