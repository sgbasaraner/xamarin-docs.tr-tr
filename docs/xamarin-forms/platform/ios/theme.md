---
title: "İOS özel biçimlendirme ekleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: c84bc8286ad5f73acb2d7e9eb13467e2c3bfb9b9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="adding-ios-specific-formatting"></a>İOS özel biçimlendirme ekleme

İOS özel ayarlamak için tek yönlü biçimlendirme oluşturmaktır bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) bir denetim ve kümesi platforma özgü stilleri ve her platform için renk.

Xamarin.Forms iOS uygulamanızın görünüm dahil şeklini denetlemek için diğer seçenekleri:

* Yapılandırma seçenekleri görüntülemek [ **Info.plist**](#info-plist)
* Aracılığıyla denetim stilleri ayarlama [ `UIAppearance` API](#uiappearance)

Bu alternatifleri aşağıda ele alınmıştır.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Info.plist özelleştirme

**Info.plist** dosyası nasıl (ve olup olmadığı) durum çubuğu gösterildiği gibi iOS uygulamanın renderering bazı yönlerini yapılandırmalarını imkan tanır.

Örneğin, [Yapılacaklar örnek](https://developer.xamarin.com/samples/xamarin-forms/Todo/) tüm platformlarda gezinti çubuğu ve metin rengi ayarlamak için aşağıdaki kodu kullanır:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Sonuç ekran parçacığında gösterilir. Durum çubuğu öğeleri siyah olduğuna dikkat edin (bir platforma özgü özellik olduğundan bu Xamarin.Forms içinde ayarlanamaz).

![](theme-images/status-default-sml.png "iOS Tema oluşturma")

İdeal olarak durum çubuğu ayrıca beyaz olacaktır - bir şey biz doğrudan iOS projesinde gerçekleştirebilirsiniz. Aşağıdaki girdileri eklemek **Info.plist** beyaz olması için durum çubuğunda zorlamak için:

![](theme-images/info-plist.png "iOS Info.plist Entries")

veya karşılık gelen düzenleme **Info.plist** dosyasını doğrudan içerecek şekilde:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Uygulamayı çalıştırdığınızda, gezinti çubuğunda yeşil ve onun metin (Xamarin.Forms biçimlendirme nedeniyle) beyaz artık *ve* durum çubuğu da iOS özel yapılandırma beyaz teşekkür metindir:

![](theme-images/status-white-sml.png "iOS Tema oluşturma")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) fazla iOS denetim görsel özelliklerini ayarlamak için kullanılan *olmadan* oluşturmak zorunda kalmadan bir [özel Oluşturucu](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Tek satırlık bir kod ekleme **AppDelegate.cs** `FinishedLaunching` yöntemi kullanarak belirtilen tür, tüm denetimler stil kendi `Appearance` özelliği. Aşağıdaki kod iki örnek - Genel sekmesi stil oluşturma içeren çubuk ve geçiş denetimi:

**AppDelegate.cs** iOS projesi içinde

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

![](theme-images/tabbar-default.png "Varsayılan iOS TabbedPage sekmesini çubuğu simgesine")

Bu davranışı değiştirmek için ayarlanmış `UITabBar.Appearance` özelliği:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Bu, seçili sekme yeşil olması neden olur:

![](theme-images/tabbar-custom.png "İOS TabbedPage sekmesini çubuğu simgesinde yeşil")

Bu API kullanarak Xamarin.Forms görünüşünü özelleştirme sağlar `TabbedPage` çok az kod ile iOS. Başvurmak [özelleştirme sekmeleri tarif](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/) sekme için belirli bir yazı tipi ayarlamak için özel Oluşturucu kullanma hakkında daha fazla bilgi.

### <a name="uiswitch"></a>UISwitch

`Switch` Denetim kolayca biçimlendirilebilir başka bir örnek verilmiştir:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Bu iki Ekran yakalamalarını varsayılan Göster `UISwitch` solda ve özelleştirilmiş sürüm denetimi (ayar `Appearance`) içinde sağdaki [Yapılacaklar örnek](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Varsayılan UISwitch rengi") ![ ] (theme-images/switch-custom.png "UISwitch renk özelleştirilmiş")

### <a name="other-controls"></a>Diğer denetimleri

Birçok iOS kullanıcı arabirimi denetimlerini kendi varsayılan renkler ve kullanılarak ayarlanan diğer öznitelikleri olabilir [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>İlgili bağlantılar

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Sekmeleri özelleştirme](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
