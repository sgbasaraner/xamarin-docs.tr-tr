---
title: Sağdan sola yerelleştirme
description: Sağdan sola yerelleştirme Xamarin.Forms uygulamalarını sağdan sola akış yönü için destek ekler.
ms.prod: xamarin
ms.assetid: 90E0CB16-C42A-4CC8-A70E-0C2CFB64A429
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 6c0de68f974c704b5f43232a1fc98065c90ee4f7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995729"
---
# <a name="right-to-left-localization"></a>Sağdan sola yerelleştirme

_Sağdan sola yerelleştirme Xamarin.Forms uygulamalarını sağdan sola akış yönü için destek ekler._

> [!NOTE]
> Sağdan sola yerelleştirme iOS 9 veya üzeri ve API 17 ya da daha yüksek Android kullanılmasını gerektirir.

Akış yönü sayfasında kullanıcı Arabirimi öğeleri gözle taranır yönüdür. Arapça ve İbranice gibi bazı diller, kullanıcı Arabirimi öğeleri bir sağdan sola akış yönü yerleşimlerini olduğunu gerektirir. Bu ayarlayarak gerçekleştirilebilir [ `VisualElement.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği. Bu özelliği alır veya düzenini denetler ve biri olarak ayarlanmalıdır, herhangi bir üst öğenin içinde hangi kullanıcı Arabirimi öğeleri akış yönünü ayarlar [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) sabit listesi değerleri:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

Ayarı [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliğini [ `RightToLeft` ](xref:Xamarin.Forms.FlowDirection.RightToLeft) bir öğede sağa, sağdan sola okuma düzeni ve denetimin düzenini gelen akış hizalama genel olarak ayarlar. Sağdan sola:

[![Sağdan sola akış yönü ile Arapça TodoItemPage](rtl-images/TodoItemPage-Arabic.png "TodoItemPage sağdan sola akış yönü ile Arapça")](rtl-images/TodoItemPage-Arabic-Large.png#lightbox "TodoItemPage Arapça ile sağdan sola akış yönü")

> [!TIP]
> Yalnızca ayarlamalısınız [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) ilk Düzen özelliği. Çalışma zamanında bu değerin değiştirilmesi, performansı etkileyen bir pahalı Düzen işlemi neden olur.

Varsayılan [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) olmadan bir üst öğenin özellik değeri [ `LeftToRight` ](xref:Xamarin.Forms.FlowDirection.LeftToRight), while varsayılan `FlowDirection` bir üst öğeye sahip bir öğe için [ `MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Bu nedenle, bir öğe devralır `FlowDirection` görsel ağacı ve herhangi bir öğenin üst öğesinden özellik değeri geçersiz kılma değeri üst öğesinden alır.

> [!TIP]
> Sağdan sola diller için bir uygulamayı yerelleştirme, ayarlama [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) bir sayfası veya kök Düzen özelliği. Bu sayfa veya akış yönü için uygun şekilde yanıt vermek için kök düzen, içerdiği tüm öğeleri neden olur.

## <a name="respecting-device-flow-direction"></a>Saygı cihaz akış yönü

Cihazın akış yönü uyarak seçilen dile dayalı ve bölge bir açık Geliştirici seçimdir ve otomatik olarak gerçekleştirilmez. Ayarlayarak gerçekleştirilebilir [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) sayfasında veya kök Düzen özelliği için `static` [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) değeri:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

```csharp
this.FlowDirection = Device.FlowDirection;
```

Tüm alt öğeleri sayfasının veya kök düzen, varsayılan olarak ardından devralır [ `Device.FlowDirection` ](xref:Xamarin.Forms.Device.FlowDirection) değeri.

## <a name="platform-setup"></a>Platform Kurulumu

Belirli bir platform kurulumu, sağdan sola yerel etkinleştirmek için gereklidir.

### <a name="ios"></a>iOS

Gerekli sağdan sola yerel desteklenen bir dil için dizi öğelerine eklenmesi gereken `CFBundleLocalizations` anahtarını **Info.plist**. Aşağıdaki örnekte dizi için eklenmiş Arapça `CFBundleLocalizations` anahtarı:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>ar</string>
</array>
```

![Info.plist desteklenen dilleri](rtl-images/ios-locales.png "Info.plist desteklenen diller")

Daha fazla bilgi için [iOS temel bilgileri yerelleştirme](https://docs.microsoft.com/en-gb/xamarin/ios/app-fundamentals/localization/#localization-basics-in-ios).

Sağdan sola yerelleştirme ardından edilebilirler belirtilen bir sağdan sola yerel, dil ve bölge cihaz/benzetici üzerinde değiştirerek **Info.plist**.

> [!WARNING]
> Dil ve bölge bir sağdan sola yerel iOS, tüm değiştirirken lütfen unutmayın [ `DatePicker` ](xref:Xamarin.Forms.DatePicker) görünümleri yerel ayarı için gereken kaynaklar eklemezseniz bir özel durum oluşturur. Örneğin, Arapça'olan bir uygulamayı test ederken bir `DatePicker`, emin **mideast** seçili **uluslararası duruma getirme** bölümünü **iOS derleme** bölmesi.

### <a name="android"></a>Android

Uygulamanın **AndroidManifest.xml** dosya güncelleştirilecek şekilde `<uses-sdk>` düğüm kümeleri `android:minSdkVersion` özniteliği 17'ye ve `<application>` düğüm kümeleri `android:supportsRtl` özniteliğini `true`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <uses-sdk android:minSdkVersion="17" ... />
    <application ... android:supportsRtl="true">
    </application>
</manifest>
```

Sağdan sola yerelleştirme, ardından sağdan sola dil kullanmak üzere cihaz/öykünücü değiştirerek veya etkinleştirerek edilebilirler **zorla sağdan sola düzen yönünü** içinde **Ayarları > Geliştirici seçenekleri**.

### <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

Gerekli dil kaynakları belirtilmelidir `<Resources>` düğümünün **Package.appxmanifest** dosya. Aşağıdaki örnek Arapça eklenmiş gösterir `<Resources>` düğüm:

```xml
<Resources>
    <Resource Language="x-generate"/>
    <Resource Language="en" />
    <Resource Language="ar" />
</Resources>
```

Ayrıca, UWP, uygulamanın varsayılan kültürü açıkça .NET Standard Kitaplığı'nda tanımlanan gerektirir. Bu ayarlayarak gerçekleştirilebilir `NeutralResourcesLanguage` özniteliğini `AssemblyInfo.cs`, veya başka bir sınıf, varsayılan kültür için:

```csharp
using System.Resources;

[assembly: NeutralResourcesLanguage("en")]
```

Sağdan sola yerelleştirme ardından dil ve bölge cihazdaki uygun sağdan sola yerel ayarı değiştirerek test edilebilir.

## <a name="limitations"></a>Sınırlamalar

Xamarin.Forms sağdan sola yerelleştirme şu anda bir dizi sınırlandırması vardır:

- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) Düğme konumu araç çubuğu öğesi konumu ve geçiş animasyon cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.
- [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) Manyetik yönü Çevir değil.
- [`Image`](xref:Xamarin.Forms.Image) görsel içerik Çevir değil.
- [`DisplayAlert`](xref:Xamarin.Forms.Page.DisplayAlert(System.String,System.String,System.String)) ve [ `DisplayActionSheet` ](xref:Xamarin.Forms.Page.DisplayActionSheet(System.String,System.String,System.String,System.String[])) yönü, cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.
- [`WebView`](xref:Xamarin.Forms.WebView) İçerik kayıtlı [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.
- A `TextDirection` eklenmesi için metin hizalamasını denetlemek için özellik olması gerekiyor.

### <a name="ios"></a>iOS

- [`Stepper`](xref:Xamarin.Forms.Stepper) Yönlendirme, cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.
- [`EntryCell`](xref:Xamarin.Forms.EntryCell) Metin hizalama, cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) hizalama ve hareketler ters değil.

### <a name="android"></a>Android

- [`SearchBar`](xref:Xamarin.Forms.SearchBar) Yönlendirme, cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) yerleştirme, cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.

### <a name="uwp"></a>UWP

- [`Editor`](xref:Xamarin.Forms.Editor) Metin hizalama, cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.
- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği tarafından devralınan değil [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage) alt öğeleri.
- [`ContextActions`](xref:Xamarin.Forms.Cell.ContextActions) Metin hizalama, cihaz yerel ayar tarafından denetlenir yerine [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) özelliği.

## <a name="right-to-left-language-support-with-xamarinuniversity"></a>Sağa sola dil desteği ile Xamarin.University

> [!VIDEO https://youtube.com/embed/f2lQ5yw3iiU]

**Xamarin.Forms 3.0 sağdan sola destek, göre [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>İlgili bağlantılar

- [TodoLocalizedRTL örnek uygulaması](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalizedRTL/)
