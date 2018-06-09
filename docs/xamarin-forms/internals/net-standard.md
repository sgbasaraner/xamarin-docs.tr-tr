---
title: Standart 2.0 desteği Xamarin.Forms .NET
description: Bu makalede, .NET standart 2.0 kullanmak için bir Xamarin.Forms uygulaması dönüştürme açıklanmaktadır. Tüm .NET uygulamalarında kullanılabilir olacak şekilde tasarlanmıştır .NET API'leri, .NET standart özelliğidir.
ms.prod: xamarin
ms.assetid: 95805355-63a7-44e7-a3c6-6487a6276ab2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: dc90155c79d1d2850281744c4c9aac70cbd7ecc3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242361"
---
# <a name="net-standard-20-support-in-xamarinforms"></a>Standart 2.0 desteği Xamarin.Forms .NET

_Bu makalede, .NET standart 2.0 kullanmak için bir Xamarin.Forms uygulaması dönüştürme açıklanmaktadır._

Tüm .NET uygulamalarında kullanılabilir olacak şekilde tasarlanmıştır .NET API'leri, .NET standart özelliğidir. Bu kod Masaüstü uygulamaları, mobil uygulamaları ve oyunları arasında paylaşmak ve bulut Hizmetleri, farklı platformları için aynı API'leri getirerek kolaylaştırır. .NET standart tarafından desteklenen platformlar hakkında daha fazla bilgi için bkz: [.NET uygulama desteği](/dotnet/standard/net-standard#net-implementation-support/).

.NET standart kitaplıkları değiştirme taşınabilir sınıf kitaplıkları (PCL) var. Ancak, .NET standardını hedefleyen bir kitaplık hala bir PCL olduğunu ve .NET standart tabanlı PCL adlandırılır. Belirli PCL profilleri .NET standart sürümler için eşlenen ve bir eşlemesine sahip profiller için iki kitaplık türleri birbirine başvuru mümkün olacaktır. Daha fazla bilgi için bkz: [PCL Uyumluluk](/dotnet/standard/net-standard#pcl-compatibility).

Xamarin.Forms 2.4, bir .NET standart 2.0 kitaplıkla PCL değiştirerek hedef .NET standart 2.0 Xamarin.Forms uygulamalarına izin verir. Bu şekilde sağlanabilir:

- Olun [.NET Core 2.0](https://www.microsoft.com/net/download/core) yüklenir.
- Xamarin.Forms çözümünüzü Xamarin.Forms 2.4 ya da daha kullanacak şekilde güncelleştirin.
- .NET standart kitaplığı .NET standart 2.0 hedefler çözüme ekleyin.
- .NET standart kitaplığa eklenen sınıfı silin.
- Xamarin.Forms 2.4 (veya daha büyük) NuGet paketini .NET standart kitaplığına ekleyin.
- Platform projelerinde .NET standart kitaplığına bir başvuru ekleyin ve Xamarin.Forms kullanıcı arabirimi mantığı içeren PCL projeye başvurusunu kaldırın.
- Dosyaları PCL projeden .NET standart kitaplığına kopyalayın.
- Xamarin.Forms kullanıcı arabirimi mantığı içeren PCL projeyi kaldırın.


## <a name="related-links"></a>İlgili bağlantılar

- [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
- [Kod Paylaşma Seçenekleri](~/cross-platform/app-fundamentals/code-sharing.md)
