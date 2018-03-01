---
title: .NET Standard
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C30F8D3-1920-453E-9E8B-D40696736FF2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/12/2017
ms.openlocfilehash: 294d28c57978218986d62d1ee6579e8d283b8f72
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="net-standard"></a>.NET Standard

## <a name="using-net-standard-library-projects-to-share-code"></a>Kod paylaşmak için .NET standart Kitaplığı projelerinde kullanma

.NET standart kitaplığı, tüm .NET çalışma zamanları üzerinde kullanılabilir olması için hedeflenen .NET API'leri resmi bir özelliğidir. Standart Kitaplığı arkasında motivasyon büyük bütünlüğünü .NET ekosistemi saptamaktır.
[ECMA 335](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) .NET çalışma zamanı davranışı benzerliği kurmaya devam eder ancak hiçbir benzer spec .NET kitaplığı uygulamaları için .NET temel sınıf kitaplıkları (BCL) için.

Bunu bir Basitleştirilmiş, yeni nesil düşünebilirsiniz [taşınabilir sınıf kitaplığı](https://msdn.microsoft.com/library/gg597391.aspx).
.NET Core dahil olmak üzere tüm .NET platformları için Tekdüzen bir API ile tek bir kitaplıktır. Yalnızca tek bir .NET standart kitaplığı oluşturun ve .NET standart platformunu destekleyen herhangi bir çalışma zamanını şuradan kullanın.

# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

## <a name="xamarin-studio"></a>Xamarin Studio

.NET standart Kitaplığı projelerinde taşınabilir kitaplığı projesi oluşturarak Xamarin Studio 6.2 içinde oluşturulabilir:

[ ![](net-standard-images/xs01-sml.png "Yeni bir taşınabilir kitaplığı projesi oluşturma")](net-standard-images/xs01.png)

Proje oluşturulduktan sonra sağ tıklayın ve açın **proje seçenekleri** penceresi.
İçinde **genel** proje .NET standart dönüştürülür ve belirli bir sürümünü kullanmak üzere ayarlanmış bölüm **Platform** aşağı açılan listesi:

[ ![](net-standard-images/xs02-sml.png ".NET standart genel seçenekleri Dönüştür")](net-standard-images/xs02.png)

Daha sonra [bir NuGet paketi oluşturma](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/existing-library.md) diğer geliştiricilere bir kitaplık paylaşımı için.

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio Mac gözden geçirme

Bu bölümde nasıl oluşturulacağı ve .NET standart Mac için Visual Studio kullanarak kitaplığı kullanmak anlatılmaktadır Tam bir uygulama için .NET standart kitaplığı örnek bölümüne bakın.

### <a name="creating-a-net-standard-library"></a>.NET standart kitaplığı oluşturma

Çözümünüz için bir .NET standart Kitaplığı eklemek oldukça doğrudan ileriye doğru olur.

1. Yeni Proje Ekle iletişim kutusunda seçin `.NET Core` kategori ve ardından `Class Library(.NET Core)`.

  **Not:** bu şablonu adlandırılacak `.NET Standard` Mac için Visual Studio gelecek bir sürümünde

  ![Bir .NET Core sınıf kitaplığı oluşturun](net-standard-images/vsm01.png)

2. .NET standart kitaplığı proje Çözüm Gezgini'nde gösterildiği gibi görünür. Bağımlılıklar düğümü kitaplığı kullandığını belirtmek [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![Çözümdeki bağımlılıklar düğümü .NET standart gösterir](net-standard-images/vsm02.png)

#### <a name="editing-net-standard-library-settings"></a>.NET standart kitaplığı ayarlarını düzenleme

.NET standart kitaplık ayarları görüntülenebilir ve projeye sağ tıklayıp seçerek değiştirilen `Options` bu ekran görüntüsünde gösterildiği gibi:

![Proje seçenekleri .NET standart hedef çerçevede Düzenle](net-standard-images/vsm03.png)

Sürümünüz içinde değiştirebileceğiniz `netstandard` değiştirerek `Target Framework` aşağı açılan değer.

**Ek olarak:** düzenleyebileceğiniz `.csproj` doğrudan bu değeri değiştirmek için.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-windows-walkthrough"></a>Visual Studio'da (Windows) izlenecek yollar

Bu bölümde oluşturmak ve .NET standart Visual Studio kullanarak kitaplığı kullanmak nasıl aracılığıyla anlatılmaktadır. Tam bir uygulama için .NET standart kitaplığı örnek bölümüne bakın.

### <a name="creating-a-net-standard-library"></a>.NET standart kitaplığı oluşturma

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Çözümünüz için bir .NET standart Kitaplığı eklemek oldukça doğrudan ileriye doğru olur.

1. Yeni Proje Ekle iletişim kutusunda seçin `.NET Standard` kategori ve ardından `Class Library(.NET Standard)`.

  ![](net-standard-images/vs01.png "Yeni .NET standart sınıf kitaplığı oluşturun")

2. .NET standart kitaplığı proje Çözüm Gezgini'nde gösterildiği gibi görünür. Bağımlılıklar düğümü kitaplığı kullandığını belirtmek [NETStandard.Library](https://www.nuget.org/packages/NETStandard.Library/).

  ![](net-standard-images/vs02.png "Çözümde .NET standart proje")

#### <a name="editing-net-standard-library-settings"></a>.NET standart kitaplığı ayarlarını düzenleme

.NET standart kitaplık ayarları görüntülenebilir ve projeye sağ tıklayıp seçerek değiştirilen `Properties` bu ekran görüntüsünde gösterildiği gibi:

![](net-standard-images/vs03.png "Diğer projeler aynı şekilde .NET standart Kitaplığı Başvurusu")

Sürümünüz içinde değiştirebileceğiniz `netstandard` değiştirerek `Target Framework` aşağı açılan değer.

**Ek olarak:** düzenleyebileceğiniz `.csproj` doğrudan bu değeri değiştirmek için.

#### <a name="using-net-standard-library"></a>.NET standart kitaplığı kullanma

.NET standart kitaplığı oluşturulduktan sonra aynı şekilde normalde başvurular ekleyin herhangi uyumlu bir uygulama veya kitaplık projeye ait bir başvuru ekleyebilirsiniz. Visual Studio'da başvurular düğümünü sağ tıklatın ve seçin `Add Reference...` için geçiş `Solution : Projects` sekmesinde gösterildiği gibi:

![](net-standard-images/vs04.png "Visual Studio'da başvuruları düğümüne sağ tıklayın ve Başvuru Ekle seçin... ardından gösterildiği gibi çözüm projeleri sekmesine geçin")

-----


## <a name="related-links"></a>İlgili bağlantılar

- [Sürüm Notları](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#.NET_Standard_Support)
