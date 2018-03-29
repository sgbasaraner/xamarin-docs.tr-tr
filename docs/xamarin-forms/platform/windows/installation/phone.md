---
title: Windows Phone uygulama ekleme
ms.topic: article
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: bb3b7e101dad98a76ac6b8f55d190514d1bd599d
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="adding-a-windows-phone-app"></a>Windows Phone uygulama ekleme


İlk olarak, Xamarin.Forms PCL şablonu kullandıysanız [profilini güncelleştirmek](~/xamarin-forms/platform/windows/installation/index.md), aşağıdaki yönergeleri izleyin:

1. **çözüm üzerinde sağ tıklayın > Ekle > Yeni proje...**  ve ekleme bir **boş uygulama (Windows Phone)**

  ![](phone-images/add-wp81.png "Yeni Proje iletişim kutusu ekleme")

2. **Yeni oluşturulan projeye sağ tıklayın > NuGet paketlerini Yönet...**  ve ekleme **Xamarin.Forms** paket.

3. **projeye sağ tıklayın > Ekle > başvuru** ve bir proje başvurusu paylaşılan Xamarin.Forms uygulaması projesi oluşturun.

  ![](phone-images/addref.png "Başvuru Yöneticisi iletişim kutusu")

4. Düzen **App.xaml.cs** içerecek şekilde `Init()` yöntemini çağırın `OnLaunched` satır 67 geçici yöntemi:

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Düzen **MainPage.xaml** -kök öğesi değiştirme `<Page` için `<forms:WindowsPhonePage` *ve* tanımlamak `xmlns:forms` kullandığı:

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6 . Düzen **MainPage.xaml.cs** kaldırmak için `: PhonePage` devralma belirleyici sınıf adı.

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7 . Hala **MainPage.xaml.cs**, ekleme `LoadApplication` Çağır `MainPage` Oluşturucusu (etrafında satır 28) Xamarin.Forms uygulamanızı başlatmak için:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Çift **Package.appxmanifest** genellikle gereklidir bu özellikleri ayarlamak için:

  * Internet (istemci ve sunucu)

9 . Son olarak, tüm yerel kaynakları (ör. Ekle görüntü dosyaları) gerekli varolan platform projelerden.
