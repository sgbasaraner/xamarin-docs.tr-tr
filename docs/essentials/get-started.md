---
title: Xamarin.Essentials ile çalışmaya başlama
description: Xamarin.Essentials sağlar, tüm iOS, Android veya UWP çalışır bir tek platformlar arası API erişilebilir uygulama paylaşılan kullanıcı arabirimi nasıl oluşturulduğunu fark etmeksizin kodu.
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: c72c1c66a465075770ce739270cb4b1f2c6fba7a
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353782"
---
# <a name="get-started-with-xamarinessentials"></a>Xamarin.Essentials ile çalışmaya başlama

![NuGet yayın öncesi](~/media/shared/pre-release.png)

Xamarin.Essentials sağlar, tüm iOS, Android veya UWP çalışır bir tek platformlar arası API erişilebilir uygulama paylaşılan kullanıcı arabirimi nasıl oluşturulduğunu fark etmeksizin kodu.

## <a name="platform-support"></a>Platform Desteği

Xamarin.Essentials aşağıdaki platformları ve işletim sistemlerini destekler:

| Platform | Sürüm |
| --- | --- |
| Android | 4.4 (API 19) veya üzeri |
| iOS |10.0 veya üzeri |
| UWP | 10.0.16299.0 veya üzeri |

## <a name="installation"></a>Yükleme

Xamarin.Essentials, Visual Studio kullanarak tüm mevcut veya yeni bir projeye eklenen bir NuGet paketi olarak kullanılabilir.

1. İndirme ve yükleme [Visual Studio](http://visualstudio.com) ile [Xamarin için Visual Studio Araçları](~/cross-platform/get-started/installation/index.md).

2. Varolan bir projeyi açın ya da altında boş uygulama şablonunu kullanarak yeni bir proje oluşturma **Visual Studio C#** (Android, iPhone ve iPad veya platformlar arası). **Önemli**: UWP projesine ekleme derleme 16299 veya üzeri, proje özelliklerinde ayarlandığından emin olun.

3. Ekleme **Xamarin.Essentials** her proje için NuGet paketi:

    # <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    Çözüm Gezgini panelinde çözüm adına sağ tıklayın ve seçin **NuGet paketlerini Yönet**. Arama **Xamarin.Essentials** pakete yükleyip **tüm** Android, iOS, UWP ve .NET standart kitaplıkları dahil olmak üzere projeleri.

    > [!TIP]
    > Denetleme **INCLUDE yayın öncesi** kutusunu [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) Önizleme aşamasındadır.

    # <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

    Çözüm Gezgini panelinde proje adına sağ tıklayın ve seçin **Ekle > NuGet paketleri Ekle...** . Arama **Xamarin.Essentials** pakete yükleyip **tüm** Android, iOS ve .NET standart kitaplıkları dahil olmak üzere projeleri.

    > [!TIP]
    > Denetleme **ön sürüm paketleri Göster** kutusunu [ **Xamarin.Essentials** NuGet](https://www.nuget.org/packages/Xamarin.Essentials) Önizleme aşamasındadır.

    -----

4. API'leri başvurmak için bir C# sınıfında Xamarin.Essentials bir başvuru ekleyin.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Platforma özgü Kurulum Xamarin.Essentials gerektirir:

    # <a name="androidtabandroid"></a>[Android](#tab/android)

    Xamarin.Essentials 4.4, API düzey 19 için karşılık gelen en düşük Android sürümü destekler ancak derleme için hedef Android sürümü 8.1 olmalıdır API düzeyi 27 karşılık gelen. (Visual Studio'da bu iki sürümü Android bildirim sekmesindeki Android projesi için Proje Özellikleri iletişim kutusunda ayarlanır. Mac için Visual Studio, proje Seçenekleri iletişim kutusunda, Android uygulama sekmesinde Android projesi için hazırsınız.) 
    
    Xamarin.Essentials gerektiren Xamarin.Android.Support kitaplıkları 27.0.2.1 sürümünü yükler. Uygulamanızın gerektirdiği diğer Xamarin.Android.Support kitaplıkları da NuGet Paket Yöneticisi'ni kullanarak 27.0.2.1 sürümüne güncelleştirilmesi gerekir. Uygulamanız tarafından kullanılan tüm Xamarin.Android.Support kitaplıkları aynı olmalıdır ve en az olmalıdır 27.0.2.1 sürümü. Başvurmak [sorun giderme sayfası](troubleshooting.md) Xamarin.Essentials NuGet ekleme veya çözümünüzdeki Nuget'i güncelleştirilirken bir sorun yaşarsanız.

    Android projenin `MainLauncher` veya tüm `Activity` diğer bir deyişle başlatılan Xamarin.Essentials başlatılmalı `OnCreate` yöntemi:

    ```csharp
    Xamarin.Essentials.Platform.Init(this, bundle);
    ```

    Android çalışma zamanı izinlerini işlemek için Xamarin.Essentials herhangi almalıdır `OnRequestPermissionsResult`. Tüm aşağıdaki kodu ekleyin `Activity` sınıflar:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # <a name="iostabios"></a>[iOS](#tab/ios)

    Ek kurulum gerekli.

    # <a name="uwptabuwp"></a>[UWP](#tab/uwp)

    Ek kurulum gerekli.

    -----

6. İzleyin [Xamarin.Essentials kılavuzları](index.md) her özellik için kod parçacıkları kopyalayıp olanak tanır.

## <a name="other-resources"></a>Diğer Kaynaklar

Geliştiricilerin Xamarin ziyaret yeni öneririz [Xamarin geliştirme ile çalışmaya başlama](~/cross-platform/getting-started/index.md).

Ziyaret [Xamarin.Essentials GitHub deposu](http://github.com/xamarin/Essentials) depo kapatın ve kaynak kod örnekleri, çalıştırma, yakında çıkacak, geçerli görmek için. Topluluk Katkıları Hoş Geldiniz!

Göz atarak [API belgeleri](xref:Xamarin.Essentials) Xamarin.Essentials her özelliği için.
