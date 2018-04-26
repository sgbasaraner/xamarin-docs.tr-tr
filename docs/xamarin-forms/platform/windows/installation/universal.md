---
title: Evrensel Windows Platformu (UWP) uygulamasını ekleme
description: Bu makalede, Visual Studio'da Mac için oluşturulmuş bir Xamarin.Forms çözümünü UWP uygulaması projesi eklemek açıklanmaktadır
ms.prod: xamarin
ms.assetid: 34AAA045-64B8-4FDE-BB49-3FF0B4FFA17C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: f851c1ca241be9e3c94a70b1f63135a46575d471
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="adding-a-universal-windows-platform-uwp-app"></a>Evrensel Windows Platformu (UWP) uygulamasını ekleme

_Bu makalede, Visual Studio'da Mac için oluşturulmuş bir Xamarin.Forms çözümünü UWP uygulaması projesi eklemek açıklanmaktadır_

Çalışır durumda olmalıdır **Visual Studio 2017** üzerinde **Windows 10** UWP uygulamaları oluşturmak için. Evrensel Windows platformu hakkında daha fazla bilgi için bkz: [Evrensel Windows platformu giriş](/windows/uwp/get-started/universal-application-platform-guide/).

UWP Xamarin.Forms 2.1 içinde kullanılabilir ve sonraki sürümleri ve Xamarin.Forms.Maps Xamarin.Forms 2.2 ve sonraki sürümlerinde desteklenir.

Denetleme <a href="#troubleshooting">sorun giderme</a> yararlı ipuçları için bölüm.

Windows 10 telefonlar, tabletler ve masaüstü bilgisayarlar üzerinde çalışacak bir UWP uygulaması eklemek için aşağıdaki yönergeleri izleyin:

 1 . Sağ tıklatın ve çözüm **Ekle > Yeni proje...** ve ekleme bir **boş uygulama (Evrensel Windows)** proje:

  ![](universal-images/add-wu.png "Yeni Proje iletişim kutusu ekleme")

 2 . İçinde **yeni evrensel Windows platformu projesi** iletişim kutusunda, uygulama üzerinde çalışacağı Windows 10 en az ve hedef sürümlerini seçin:

  ![](universal-images/target-version.png "Evrensel Windows platformu yeni proje iletişim kutusu")

 3 . UWP projeye sağ tıklayıp **NuGet paketlerini Yönet...** ve ekleme **Xamarin.Forms** paket. Çözümdeki diğer projeler Xamarin.Forms paket aynı sürüme güncelleştirildiğinden emin olun.

 4 . Emin olun yeni UWP projesini yerleşik **Yapı > Configuration Manager** penceresi (Bu büyük olasılıkla olmaz işleminin gerçekleştiği bilgisayarlar varsayılan olarak). Değer çizgilerinin **yapı** ve **dağıtma** kutuları Evrensel projesi için:

  [![](universal-images/configuration-sml.png "Yapılandırma Yöneticisi penceresi")](universal-images/configuration.png#lightbox "Yapılandırma Yöneticisi penceresi")

 5 . Sağ tıklatın ve proje **Ekle > başvuru** ve bir başvuru (PCL, .NET Standard veya paylaşılan Proje) Xamarin.Forms uygulaması projesi oluşturun.

  ![](universal-images/addref-sml.png "Başvuru Yöneticisi iletişim kutusu")

 6 . UWP projesinde Düzenle **App.xaml.cs** içerecek şekilde `Init` yöntemi çağrısı içinde `OnLaunched` satır 52 geçici yöntemi:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7 . UWP projesinde Düzenle **MainPage.xaml** kaldırarak `Grid` içindeki `Page` öğesi.

 8 . İçinde **MainPage.xaml**, yeni bir ekleme `xmlns` için giriş `Xamarin.Forms.Platform.UWP`:

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 9 . İçinde **MainPage.xaml**, kök değiştirin `<Page` öğesine `<forms:WindowsPage`:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 10 . UWP projesinde Düzenle **MainPage.xaml.cs** kaldırmak için `: Page` devralma belirleyici sınıf adı için (bunu şimdi devralınacağı beri `WindowsPage` önceki adımda yaptığınız değişiklikleri nedeniyle):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 11 . İçinde **MainPage.xaml.cs**, ekleme `LoadApplication` Çağır `MainPage` Oluşturucusu Xamarin.Forms uygulaması başlatmak için:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

12 . Yerel kaynaklardan herhangi (ör. Ekle görüntü dosyaları) gerekli varolan platform projelerden.

<a name="troubleshooting" />

## <a name="troubleshooting"></a>Sorun giderme

<a name="target-invocation-exception" />

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"Hedef çağırma özel durum" ".NET yerel araç zinciri ile derleme" kullanırken

(Örneğin üçüncü taraf kontrol kitaplıkları veya birden çok kitaplıklara, uygulamanızın bölme) birden çok derleme UWP uygulamanızı başvuruyorsa Xamarin.Forms nesneleri (örneğin, özel Oluşturucu) Bu derlemelerden yükleyemedi.

Bu kullanılırken oluşabilir **olan .NET yerel araç zinciri derleme** UWP uygulamalar için bir seçenek olan **Özellikler > Yapı > Genel** projenin penceresine.

Bu bir UWP özgü aşırı yüklemesini kullanarak düzeltmek `Forms.Init` Çağır **App.xaml.cs** aşağıdaki kodda gösterildiği gibi (değiştirmeniz gerekir `ClassInOtherAssembly` gerçek bir sınıf kodunuzu başvuran):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Bir uygulama tarafından başvurulan her derlemesine başvuru ekleyin.

#### <a name="dependency-services-and-net-native-compilation"></a>Bağımlılık Hizmetleri ve .NET yerel derleme

Yayın derlemeleri .NET yerel derleme kullanarak ana uygulama yürütülebilir dosya (ayrı bir proje veya kitaplık olduğu gibi) dışında tanımlanan bağımlılık hizmetlerin çözümlenmesi başarısız olabilir.

Kullanım `DependencyService.Register<T>()` bağımlılık hizmet sınıfları el ile kaydetmek için yöntem. Yukarıdaki örnekte bağlı olarak, bu gibi kayıt yöntemi ekleyin:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
