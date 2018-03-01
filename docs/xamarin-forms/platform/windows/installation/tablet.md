---
title: "Bir Windows uygulaması ekleme"
ms.topic: article
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 369e8005c1d398c90b14a95ac36fc8659e462fbf
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="adding-a-windows-app"></a>Bir Windows uygulaması ekleme


1. **çözüm üzerinde sağ tıklayın > Ekle > Yeni proje...**  ve ekleme bir **boş uygulama (Windows)**

 ![](tablet-images/add-wu.png "Yeni Proje iletişim kutusu ekleme")

2. **projeye sağ tıklayın > NuGet paketlerini Yönet...**  ve ekleme **Xamarin.Forms** paket.

3. **projeye sağ tıklayın > Ekle > başvuru** ve bir proje başvurusu paylaşılan Xamarin.Forms uygulaması projesi oluşturun.

  ![](tablet-images/addref.png "Başvuru Yöneticisi iletişim kutusu")

4. Düzenleme **App.xaml.cs** içerecek şekilde `Init()` yöntemi çağrısı içinde `OnLaunched` satır 65 geçici yöntemi

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Düzen **MainPage.xaml** -kök öğesi değiştirme `<Page` için `<forms:WindowsPage` *ve* tanımlamak `xmlns:forms` kullandığı:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . Düzen **MainPage.xaml.cs** kaldırmak için `: Page` devralma belirleyici sınıf adı.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . Hala **MainPage.xaml.cs**, ekleme `LoadApplication` Çağır `MainPage` Oluşturucusu (etrafında satır 28) Xamarin.Forms uygulamanızı başlatmak için:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Çift **Package.appxmanifest** genellikle gereklidir bu özellikleri ayarlamak için:

  Özellikleri ayarlayın:

  * Internet (istemci)
  * Konum

9 . Son olarak, tüm yerel kaynakları (ör. Ekle görüntü dosyaları) gerekli varolan platform projelerden.

