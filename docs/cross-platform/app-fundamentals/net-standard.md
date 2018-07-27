---
title: Kodu paylaşmak için .NET standart kitaplıkları kullanma
description: Bu belge, kodu paylaşmak için .NET standart kitaplıkları kullanmayı açıklar. Uygulamayı kullanarak bir .NET Standard kitaplığı oluşturmak ve ayarlarını düzenleme ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: conceptdev
ms.author: crdun
ms.date: 07/18/2018
ms.openlocfilehash: 65ba1915a2a968a14f0ce21bcada76e1b83531b0
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270639"
---
# <a name="net-standard-library-code-sharing"></a>.NET standard kitaplığı kod paylaşımı

.NET standart kitaplıkları, Xamarin ve .NET Core dahil olmak üzere tüm .NET platformları için Tekdüzen API vardır. Tek bir .NET Standard kitaplığı oluşturun ve .NET Standard platformu destekleyen herhangi bir çalışma zamanını şuradan kullanın. Başvurmak [bu grafik](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support) desteklenen platformlar hakkında ayrıntılar için.

.NET Standard sürümleri 1.0 1.6 aracılığıyla .NET Framework'ün artımlı olarak daha büyük alt kümelerini sunarken, .NET Standard 2.0 Xamarin uygulamaları için ve mevcut taşınabilir sınıf kitaplıkları taşıma için en iyi düzeyde desteği sağlar.

# <a name="visual-studio-for-mactabmacos"></a>[Mac için Visual Studio](#tab/macos)

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio

Bu bölüm .NET Standard Mac için Visual Studio kullanarak kitaplığı oluşturulacağı ve kullanılacağı konusunda yol göstermektedir

### <a name="creating-a-net-standard-library"></a>.NET Standard kitaplığı oluşturma

.NET Standard kitaplığı çözümünüze eklemek oldukça oldukça kolaydır.

1. İçinde **Yeni Proje Ekle** iletişim kutusunda **.NET Core** kategorisi ve ardından **.NET Standard Kitaplığı**:

    ![.NET Standard kitaplığı oluşturma](net-standard-images/vsm01-m157.png "yeni .NET Standard kitaplığı oluşturma")

2. Sonraki ekranda, hedef Framework'ü - seçin **.NET Standard 2.0** önerilir:

    [![.NET Standard 2.0 seçin](net-standard-images/vsm01a-m157-sml.png)](net-standard-images/vsm01a-m157.png#lightbox)

3. Proje adı son ekranında, yazın ve tıklayın **Oluştur**.

4. .NET Standard kitaplığı projesi, Çözüm Gezgini'nde görüldüğü gibi görünür. Bağımlılıklar düğümü kitaplığı kullandığını belirtmek [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![Çözümdeki bağımlılıklar düğümü .NET Standard gösterir](net-standard-images/vsm02-m157.png)

#### <a name="editing-net-standard-library-settings"></a>.NET Standard kitaplığı ayarlarını düzenleme

.NET Standard kitaplığı ayarları görüntülenebilir ve projeye sağ tıklayıp `Options` bu ekran görüntüsünde gösterildiği gibi:

![.NET Standard hedef çerçeve proje Seçenekleri Düzenle](net-standard-images/vsm03-m157.png ".NET standart hedef çerçeve proje seçenekleri sürümü Düzenle")

Sürümünüz içinde değiştirebilirsiniz `netstandard` değiştirerek `Target Framework` açılan değeri.

**Ek olarak:** düzenleyebileceğiniz `.csproj` doğrudan bu değeri değiştirmek için.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

Bu bölümde, oluşturma ve .NET Standard Visual Studio kullanarak kitaplığı kullanma hakkında bilgi vermektedir.

### <a name="creating-a-net-standard-library"></a>.NET Standard kitaplığı oluşturma

.NET Standard kitaplığı çözümünüze eklemek oldukça oldukça kolaydır.

1. İçinde **yeni proje** iletişim kutusunda **.NET Standard** kategorisi ve ardından **sınıf kitaplığı (.NET Standard)**.

    ![Yeni bir .NET standart sınıf kitaplığı oluşturma](net-standard-images/vs01-w157.png "yeni .NET Standard sınıf kitaplığı oluşturma")

2. .NET Standard kitaplığı projesi, Çözüm Gezgini'nde görüldüğü gibi görünür. Bağımlılıklar düğümü kitaplığı kullandığını belirtmek [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

    ![Proje klasöründeki NETStandard.Library](net-standard-images/vs02-w157.png "çözümde .NET Standard projesi")

### <a name="editing-net-standard-library-settings"></a>.NET Standard kitaplığı ayarlarını düzenleme

.NET Standard kitaplığı ayarları görüntülenebilir ve projeye sağ tıklayıp **özellikleri** bu ekran görüntüsünde gösterildiği gibi:

![.NET standard hedef çerçeve proje özelliklerinde Düzenle](net-standard-images/vs03-w157.png "diğer projeleri aynı şekilde .NET Standard Kitaplığı Başvurusu")

**Ek olarak:** düzenleyebileceğiniz `.csproj` doğrudan düzenlemek için `TargetFramework` hedeflenen öğesi ve hangi sürüm olarak değiştirin (ör.) `<TargetFramework>netstandard2.0</TargetFramework>`).

### <a name="using-a-net-standard-library-project"></a>.NET Standard kitaplığı projesi kullanarak

.NET Standard kitaplığı oluşturulduktan sonra ona bir başvuru uyumlu hiçbir uygulama veya kitaplık projesi başvuruları normalde eklediğiniz yolla ekleyebilirsiniz. Visual Studio'da başvurular düğümüne sağ tıklayın ve seçin **Başvuru Ekle...**  dönersiniz **projeleri > Çözüm** gösterildiği sekmesinde:

![.NET Standard kitaplığı başvuran](net-standard-images/vs04.png "Visual Studio'da, başvurular düğümüne sağ tıklayın ve Başvuru Ekle... seçin sonra gösterildiği gibi çözüm projelerinden sekmeye geçin")

-----

## <a name="related-links"></a>İlgili bağlantılar

* [.NET standard](https://docs.microsoft.com/dotnet/standard/net-standard) -ayrıntılı bilgiler ve PCL karşılaştırma.
