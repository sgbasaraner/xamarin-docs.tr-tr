---
title: Xamarin.Essentials ile çalışmaya başlama
description: Xamarin.Essentials tüm iOS, Android veya UWP çalışır bir tek platformlar arası API sağlar erişilebilen uygulama paylaşılan kullanıcı arabirimini nasıl oluşturulduğunu olsun kodu.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: f0f6eebbd12041a7be2d8e2dc00a9146b40d675f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783080"
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials ile çalışmaya başlama

![Yayın öncesi NuGet](~/media/shared/pre-release.png)

Xamarin.Essentials tüm iOS, Android veya UWP çalışır bir tek platformlar arası API sağlar erişilebilen uygulama paylaşılan kullanıcı arabirimini nasıl oluşturulduğunu olsun kodu.

## <a name="platform-support"></a>Platform Desteği

Xamarin.Essentials aşağıdaki platformları ve işletim sistemlerini destekler:

| Platform | Sürüm |
| --- | --- |
| Android | 4.4 (API 19) veya üzeri |
| iOS |10.0 veya üzeri |
| UWP | 10.0.16299.0 veya üzeri |

## <a name="installation"></a>Yükleme

Xamarin.Essentials, Visual Studio kullanarak tüm mevcut veya yeni projeye eklenen bir NuGet paketi olarak kullanılabilir.

1. İndirme ve yükleme [Visual Studio](http://visualstudio.com) ile [Xamarin için Visual Studio Araçları](~/cross-platform/get-started/installation/index.md).

2. Varolan projeyi açın veya altında boş uygulama şablonu kullanarak yeni bir proje oluşturun **Visual Studio C#** (Android, iPhone ve iPad veya platformlar arası). **Önemli**: UWP projeye ekleme derleme 16299 ya da daha yüksek Proje Özellikleri'nde ayarlanır emin olmak değilse.

3. Ekleme **Xamarin.Essentials** her proje için NuGet paketi:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    Çözüm Gezgini panelinde çözüm adına sağ tıklayın ve seçin **NuGet paketlerini Yönet**. Arama **Xamarin.Essentials** pakete yükleyip **tüm** Android, iOS, UWP ve .NET standart kitaplıkları da dahil olmak üzere projeleri.

    > [!TIP]
    > Denetleme **INCLUDE yayın öncesi** kutusunu [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) Önizleme aşamasındadır.

    # <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

    Çözüm Gezgini panelinde proje adına sağ tıklayın ve seçin **Ekle > NuGet paketleri Ekle...** . Arama **Xamarin.Essentials** pakete yükleyip **tüm** Android, iOS ve .NET standart kitaplıkları da dahil olmak üzere projeleri.

    > [!TIP]
    > Denetleme **Göster ön sürüm paketlerini** kutusunu [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) Önizleme aşamasındadır.

    -----

4. Xamarin.Essentials başvuru API'leri başvurmak için hiçbir C# sınıfta ekleyin.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials platforma özgü Kurulum gerektirir:

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Android projenin `MainLauncher` veya tüm `Activity` diğer bir deyişle başlatılan Xamarin.Essentials başlatıldı, içinde `OnCreate` yöntemi:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Android çalışma zamanı izinlerini işlemek için Xamarin.Essentials herhangi almalıdır `OnRequestPermissionsResult`. Tümü için aşağıdaki kodu ekleyin `Activity` sınıfları:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Ek kurulumu gerekmez.

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    Ek kurulumu gerekmez.

    -----

6. İzleyin [Xamarin.Essentials kılavuzları](index.md) her özellik için kod parçacıkları kopyalayıp olanak tanır.

## <a name="other-resources"></a>Diğer Kaynaklar

Geliştiriciler için Xamarin ziyaret yeni öneririz [Xamarin geliştirme ile çalışmaya başlama](~/cross-platform/getting-started/index.md).

Ziyaret [Xamarin.Essentials GitHub depo](http://github.com/xamarin/Essentials) depo kapatın ve kaynak kod örnekleri, çalıştırmak İleri, yakında, geçerli görmek için. Topluluk Katkıları Hoş Geldiniz!

Göz atarak [API belgelerine](xref:Xamarin.Essentials) Xamarin.Essentials, her özellik için.
