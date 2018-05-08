---
title: Sağdan sola yerelleştirme
description: Sağdan sola yerelleştirme Xamarin.Forms uygulamalarına sağdan sola akış yönü için destek ekler.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/18/2018
ms.openlocfilehash: 001c9f6f26fc96ca0fd0d25e150c5ec9409e552a
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="right-to-left-localization"></a>Sağdan sola yerelleştirme

_Sağdan sola yerelleştirme Xamarin.Forms uygulamalarına sağdan sola akış yönü için destek ekler._

> [!NOTE]
> Sağdan sola yerelleştirme iOS 9 veya üstü ve API 17 ya da daha yüksek android'de kullanılmasını gerektirir.

Akış yönü kullanıcı Arabirimi öğeleri sayfada göz tarafından taranır yönüdür. Arapça ve İbranice gibi bazı diller kullanıcı Arabirimi öğeleri bir sağdan sola akış yönü düzenlenmiştir olmasını gerektirir. Bu ayarlanarak sağlanabilir [ `VisualElement.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği. Bu özellik alır veya kendi düzeni denetleyen ve birine ayarlamalıdır herhangi bir üst öğesi içinde hangi kullanıcı Arabirimi öğeleri akış yönünü ayarlar [ `FlowDirection` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FlowDirection/) numaralandırma değerlerinin:

- [`LeftToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.LeftToRight/)
- [`RightToRight`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.RightToLeft/)
- [`MatchParent`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.MatchParent/)

Ayarı [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliğine [ `RightToLeft` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.RightToLeft/) öğe üzerinde hizalama genellikle sağa, sağdan sola okuma sırası ve denetim düzenini gelen akış için ayarlar Sağdan sola:

[![Arapça sağdan sola akış yönü ile TodoItemPage](rtl-images/TodoItemPage-Arabic.png "Arapça sağdan sola akış yönü ile TodoItemPage")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage Arapça sağdan sola akış yönü ile")

> [!TIP]
> Yalnızca ayarlamalısınız [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) ilk düzeni özelliği. Çalışma zamanında bu değeri değiştirmeden performansını etkiler bir pahalı düzen işlemine neden olur.

Varsayılan [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) üst öğesi olmayan bir öğe için özellik değeri [ `LeftToRight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.LeftToRight/), ancak varsayılan `FlowDirection` bir üst öğeye sahip bir öğe için [ `MatchParent`](https://developer.xamarin.com/api/field/Xamarin.Forms.FlowDirection.MatchParent/). Bu nedenle, bir öğenin devralır `FlowDirection` görsel ağaç ve herhangi bir öğe içindeki kendi üst öğesinden özellik değeri üst öğesinden alır değeri geçersiz kılar.

> [!TIP]
> Bir uygulama sağdan sola yazılan diller için yerelleştirme ayarlanmadığında [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özellik sayfası veya kök bir düzen. Bu sayfa veya akış yönü uygun şekilde yanıt vermesi kök düzeni, içindeki tüm öğeleri neden olur.

## <a name="respecting-device-flow-direction"></a>Saygı aygıt akış yönü

Seçilen dile dayalı cihazın akış yönü saygı göstermek ve bölge bir açık Geliştirici seçimdir ve otomatik olarak gerçekleştirilmez. Ayarlanarak sağlanabilir [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özellik sayfasında veya kök düzeni için `static` [ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.FlowDirection/) değeri:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Tüm alt öğeler sayfası veya kök düzen, varsayılan olarak sonra devralır [ `Device.FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.FlowDirection/) değeri.

## <a name="platform-setup"></a>Platform Kurulumu

Belirli platform Kurulum sağdan sola yerel ayarlar etkinleştirmek için gereklidir.

### <a name="ios"></a>iOS

Gerekli sağdan sola yerel desteklenen bir dil için dizi öğeleri eklenmelidir `CFBundleLocalizations` anahtarını **Info.plist**. Aşağıdaki örnek, dizi için eklenen Arapça gösterir `CFBundleLocalizations` anahtarı:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Info.plist desteklenen dilleri](rtl-images/ios-locales.png "Info.plist desteklenen diller")

Daha fazla bilgi için bkz: [yerelleştirme temelleri iOS içinde](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Sağdan sola yerelleştirme sonra test belirtildi bir sağdan sola ayara aygıt/benzeticide üzerinde bölge ve dil değiştirerek **Info.plist**.

> [!WARNING]
> Lütfen bölge ve dil sağdan sola yerel iOS, her değiştirildiğinde unutmayın [ `DatePicker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/) görünümleri yerel ayarlar için gereken kaynakları eklemezseniz bir özel durum oluşturur. Örneğin, bir uygulama olan Arapça'test edilirken bir `DatePicker`, emin olun **mideast** seçildiyse **uluslararası** bölümünü **iOS yapı** bölmesi.

### <a name="android"></a>Android

Uygulamanın **AndroidManifest.xml** dosya güncelleştirilmelidir böylece `<uses-sdk>` düğüm kümeleri `android:minSdkVersion` özniteliği 17'ye ve `<application>` düğüm kümeleri `android:supportsRtl` özniteliğini `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Sağdan sola yerelleştirme sonra test sağdan sola dil kullanmak üzere Aygıt/Öykünücüsü değiştirerek veya etkinleştirme **zorla RTL Düzen yönünü** içinde **ayarlar > Geliştirici seçenekleri**.

### <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

Gerekli dil kaynakları belirtilmelidir `<Resources>` düğümünün **Package.appxmanifest** dosya. Aşağıdaki örnek, Arapça için eklenene gösterir `<Resources>` düğümü:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Ayrıca, uygulamanın varsayılan kültürü .NET standart Kitaplığı'nda açıkça tanımlanmış UWP gerektirir. Bu ayarlayarak gerçekleştirilebilir `NeutralResourcesLanguage` özniteliğini `AssemblyInfo.cs`, veya varsayılan kültürü için başka bir sınıf:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Sağdan sola yerelleştirme uygun sağdan sola yerel cihazda bölge ve dil değiştirerek sonra test edilmelidir.

## <a name="limitations"></a>Sınırlamalar

Xamarin.Forms sağdan sola yerelleştirme şu anda bir dizi sınırlama vardır:

- [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) Düğme konumu, araç çubuğu öğesi konumu ve geçiş animasyon aygıt yerel tarafından denetlenen yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.
- [`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/) Manyetik yönü Çevir değil.
- [`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) görsel içerik Çevir değil.
- [`DisplayAlert`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayAlert/p/System.String/System.String/System.String/) ve [ `DisplayActionSheet` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.DisplayActionSheet/p/System.String/System.String/System.String/System.String[]/) yönlendirmesini aygıt yerel tarafından denetlenen yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.
- [`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) İçerik saygı [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.
- A `TextDirection` eklemek için metin hizalamasını denetlemek için özellik olması gerekiyor.

### <a name="ios"></a>iOS

- [`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/) Yönlendirme aygıt yerel tarafından denetlenen yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.
- [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/) Metin hizalama aygıt bölgeye göre denetlenir yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.
- [`ContextActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) hareketleri ve hizalama ters değil.

### <a name="android"></a>Android

- [`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) Yönlendirme aygıt yerel tarafından denetlenen yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.
- [`ContextActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) yerleştirme aygıt yerel ayarı tarafından denetlenen yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.

### <a name="uwp"></a>UWP

- [`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/) Metin hizalama aygıt bölgeye göre denetlenir yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.
- [`FlowDirection`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği tarafından devralınan değil [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) alt.
- [`ContextActions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Cell.ContextActions/) Metin hizalama aygıt bölgeye göre denetlenir yerine [ `FlowDirection` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.FlowDirection/) özelliği.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Sağ sol dil için destek ile Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 sağdan sola desteği, göre [Xamarin Üniversitesi](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [TodoLocalizedRTL örnek uygulaması](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
