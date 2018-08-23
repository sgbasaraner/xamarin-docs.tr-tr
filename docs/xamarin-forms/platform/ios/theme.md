---
title: İOS özel biçimlendirme ekleme
description: Bu makalede, iOS özel görünüşünü Xamarin.Forms özel Oluşturucu kullanmadan ayarlamak açıklanmaktadır.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 3b8a440617dedfbe23f869e865b3cedae21d6c5b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241383"
---
# <a name="adding-ios-specific-formatting"></a>İOS özel biçimlendirme ekleme

İOS özel ayarlamak için tek yönlü biçimlendirme oluşturmaktır bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) için Denetim ve kümesi platforma özgü stili ve renklerini her platform için.

Xamarin.Forms iOS uygulamanızın görünümünü biçimini denetlemek için diğer seçenekler şunlardır:

* Yapılandırma seçenekleri görüntüleme [ **Info.plist**](#info-plist)
* Denetim stilleri aracılığıyla ayarlama [ `UIAppearance` API](#uiappearance)

Bu seçenekler aşağıda ele alınmıştır.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Info.plist özelleştirme

**Info.plist** dosyasının nasıl (ve olup olmadığı) durum çubuğunda gösterilen gibi bir iOS uygulamasının renderering, bazı yönlerini yapılandırmalarını izin verir.

Örneğin, [Yapılacaklar örneği](https://developer.xamarin.com/samples/xamarin-forms/Todo/) tüm platformlarda gezinti çubuğu rengi ve metin rengini ayarlamak için aşağıdaki kodu kullanır:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Ekran aşağıdaki kod parçacığında sonuçları gösterilmektedir. Durum çubuğu öğeleri siyah olduğuna dikkat edin (platforma özgü bir özellik olduğundan bu Xamarin.Forms içinde ayarlanamaz).

![](theme-images/status-default-sml.png "iOS Tema oluşturma")

İdeal olarak durum çubuğunu da beyaz olacaktır - sorun biz doğrudan iOS projesinde gerçekleştirebilirsiniz. Aşağıdaki girdileri ekleme **Info.plist** güvenilir listeye alınması için durum çubuğunu zorlamak için:

![](theme-images/info-plist.png "iOS Info.plist girişleri")

veya karşılık gelen düzenleme **Info.plist** doğrudan dahil edilecek dosyası:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Şimdi uygulamayı çalıştırdığınızda gezinti çubuğunda yeşil ve metin (Xamarin.Forms biçimlendirme nedeniyle) beyaz *ve* durum çubuğu metni ayrıca iOS özel yapılandırma beyaz teşekkür olan:

![](theme-images/status-white-sml.png "iOS Tema oluşturma")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) fazla iOS denetim görsel özelliklerini ayarlamak için kullanılan *olmadan* oluşturmak zorunda kalmadan bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Tek bir satır kod ekleyerek **AppDelegate.cs** `FinishedLaunching` yöntemi kullanarak bir belirtilen türle tüm denetimleri stilini belirlemek kendi `Appearance` özelliği. Aşağıdaki kodu içeren iki örnek - Genel sekmesi stil çubuk ve geçiş denetimi:

**AppDelegate.cs** iOS projesi

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>UITabBar

Varsayılan olarak, seçili Sekme çubuğu simgesinde bir [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) mavi olacaktır:

![](theme-images/tabbar-default.png "Varsayılan iOS TabbedPage Sekme çubuğu simgesi")

Bu davranışı değiştirmek için Ayarla `UITabBar.Appearance` özelliği:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Bu, seçilen sekmenin yeşil neden olur:

![](theme-images/tabbar-custom.png "İOS Sekme çubuğu TabbedPage simgesinde yeşil")

Bu API kullanarak Xamarin.Forms özelleştirme sağlar `TabbedPage` iOS çok az kod ile. Başvurmak [sekmeleri özelleştirme tarif](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) belirli bir yazı tipi için sekmesinde ayarlamak için özel Oluşturucu kullanma hakkında daha fazla bilgi.

### <a name="uiswitch"></a>UISwitch

`Switch` Denetimi, kolayca biçimlendirilebilir başka bir örnek verilmiştir:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Bu iki ekran görüntüsü almayı varsayılan Göster `UISwitch` sola ve özelleştirilmiş sürümü üzerinde denetim (ayar `Appearance`) sağdaki [Yapılacaklar örneği](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Varsayılan UISwitch rengi") ![](theme-images/switch-custom.png "özelleştirilmiş UISwitch rengi")

### <a name="other-controls"></a>Diğer denetimleri

Çok sayıda iOS kullanıcı arabirimi denetimleri, kendi varsayılan renkleri ve diğer özniteliklerini kullanarak olabilir [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>İlgili bağlantılar

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Sekmeleri özelleştirme](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
