---
title: Kod paylaşmak için .NET standart kitaplıkları kullanma
description: Bu belgede .NET standart kitaplıkları kod paylaşmak için nasıl kullanılacağı açıklanmaktadır. .NET standart kitaplığı oluşturma, ayarlarını düzenleme ve bir uygulama kullanarak açıklanır.
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 82a89309e6462462471f42c3504d109ff0722917
ms.sourcegitcommit: 5db075bdd0b62d5d1d1567c267303a6a1888c8f2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34806796"
---
# <a name="using-net-standard-libraries-to-share-code"></a>Kod paylaşmak için .NET standart kitaplıkları kullanma

## <a name="net-standard"></a>.NET standart

.NET standart kitaplığı, tüm .NET çalışma zamanları üzerinde kullanılabilir olması için hedeflenen .NET API'leri resmi bir özelliğidir. Standart Kitaplığı arkasında motivasyon büyük bütünlüğünü .NET ekosistemi saptamaktır.
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) .NET çalışma zamanı davranışı benzerliği kurmaya devam eder ancak hiçbir benzer spec .NET kitaplığı uygulamaları için .NET temel sınıf kitaplıkları (BCL) için.

Bunu bir Basitleştirilmiş, yeni nesil düşünebilirsiniz [taşınabilir sınıf kitaplığı](https://msdn.microsoft.com/library/gg597391.aspx).
.NET Core dahil olmak üzere tüm .NET platformları için Tekdüzen bir API ile tek bir kitaplıktır. Yalnızca tek bir .NET standart kitaplığı oluşturun ve .NET standart platformunu destekleyen herhangi bir çalışma zamanını şuradan kullanın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="visual-studio-for-mac"></a>Mac için Visual Studio

Bu bölümde nasıl oluşturulacağı ve .NET standart Mac için Visual Studio kullanarak kitaplığı kullanmak anlatılmaktadır Tam bir uygulama için .NET standart kitaplığı örnek bölümüne bakın.

### <a name="creating-a-net-standard-library"></a>.NET standart kitaplığı oluşturma

Çözümünüz için bir .NET standart Kitaplığı eklemek oldukça doğrudan ileriye doğru olur.

1. Yeni Proje Ekle iletişim kutusunda seçin `.NET Core` kategori ve ardından `Class Library(.NET Core)`.

  **Not:** bu şablonu adlandırılacak `.NET Standard` Mac için Visual Studio gelecek bir sürümünde

  ![.NET Core sınıf kitaplığı oluşturmak](net-standard-images/vsm01.png "yeni bir .NET çekirdek sınıf kitaplığı oluşturma")

2. .NET standart kitaplığı proje Çözüm Gezgini'nde gösterildiği gibi görünür. Bağımlılıklar düğümü kitaplığı kullandığını belirtmek [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Çözümdeki bağımlılıklar düğümü .NET standart gösterir](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>.NET standart kitaplığı ayarlarını düzenleme

.NET standart kitaplık ayarları görüntülenebilir ve projeye sağ tıklayıp seçerek değiştirilen `Options` bu ekran görüntüsünde gösterildiği gibi:

![Proje seçenekleri .NET standart hedef çerçevede Düzenle](net-standard-images/vsm03.png "proje seçenekleri .NET standart hedef Framework sürümü Düzenle")

Sürümünüz içinde değiştirebileceğiniz `netstandard` değiştirerek `Target Framework` aşağı açılan değer.

**Ek olarak:** düzenleyebileceğiniz `.csproj` doğrudan bu değeri değiştirmek için.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-2017-windows"></a>Visual Studio 2017 (Windows)

Bu bölümde oluşturmak ve .NET standart Visual Studio kullanarak kitaplığı kullanmak nasıl aracılığıyla anlatılmaktadır. Tam bir uygulama için .NET standart kitaplığı örnek bölümüne bakın.

### <a name="creating-a-net-standard-library"></a>.NET standart kitaplığı oluşturma

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Çözümünüz için bir .NET standart Kitaplığı eklemek oldukça doğrudan ileriye doğru olur.

1. Yeni Proje Ekle iletişim kutusunda seçin `.NET Standard` kategori ve ardından `Class Library(.NET Standard)`.

  ![Yeni bir .NET standart sınıf kitaplığı oluşturmak](net-standard-images/vs01.png "oluştur yeni .NET standart sınıf kitaplığı")

2. .NET standart kitaplığı proje Çözüm Gezgini'nde gösterildiği gibi görünür. Bağımlılıklar düğümü kitaplığı kullandığını belirtmek [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Proje klasöründeki NETStandard.Library](net-standard-images/vs02.png "çözümde .NET standart proje")

#### <a name="editing-net-standard-library-settings"></a>.NET standart kitaplığı ayarlarını düzenleme

.NET standart kitaplık ayarları görüntülenebilir ve projeye sağ tıklayıp seçerek değiştirilen `Properties` bu ekran görüntüsünde gösterildiği gibi:

![Proje Özellikleri'nde .NET standart hedef çerçeveyi Düzenle](net-standard-images/vs03.png "diğer projeler aynı şekilde .NET standart Kitaplığı Başvurusu")

Sürümünüz içinde değiştirebileceğiniz `netstandard` değiştirerek `Target Framework` aşağı açılan değer.

**Ek olarak:** düzenleyebileceğiniz `.csproj` doğrudan bu değeri değiştirmek için.

#### <a name="using-net-standard-library"></a>.NET standart kitaplığı kullanma

.NET standart kitaplığı oluşturulduktan sonra aynı şekilde normalde başvurular ekleyin herhangi uyumlu bir uygulama veya kitaplık projeye ait bir başvuru ekleyebilirsiniz. Visual Studio'da başvurular düğümünü sağ tıklatın ve seçin `Add Reference...` için geçiş `Solution : Projects` sekmesinde gösterildiği gibi:

![.NET standart kitaplığı başvuran](net-standard-images/vs04.png "Visual Studio'da, başvuruları düğümüne sağ tıklayın ve Başvuru Ekle seçin... ardından gösterildiği gibi çözüm projeleri sekmesine geçin")

-----

