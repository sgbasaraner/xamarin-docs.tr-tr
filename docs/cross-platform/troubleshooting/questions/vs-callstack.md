---
title: Visual Studio işleminin geçerli çağrı yığınları nasıl topluyor mu?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 64c24b09-2c4a-43ad-b94d-6cd05a1aee44
author: asb3993
ms.author: amburns
ms.date: 03/30/2017
ms.openlocfilehash: e81c28f0610a0df2e4fe06349685ef5e0744071a
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2018
ms.locfileid: "33919767"
---
# <a name="how-do-i-collect-the-current-call-stacks-of-the-visual-studio-process"></a>Visual Studio işleminin geçerli çağrı yığınları nasıl topluyor mu?

Visual Studio'da (askıda donuyor) GUI kilitler, tanılama bilgilerini toplamak için önemli bir parçası Visual Studio işlemin tüm iş parçacıklarının gelen çağrı yığınları kümesidir. Visual Studio askıdaki örneği için bu bilgileri kaydetmek için Visual Studio ikinci bir örneğini kullanabilirsiniz:

1. İkinci bir örneğini (yeni bir pencerede) Visual Studio başlatın.

2. Visual Studio yeni bir örneğini açık herhangi çözümlerinde kapatın.

3. Seçin **hata ayıklama > işleme iliştirilemiyor**.

  ![](vs-callstack-images/image1.png "Hata ayıklama seçin > için işlem ekleme")

4. Özgün Askıdaki örneğini seçin `devenv.exe` listesinden **kullanılabilir işlemler**.

5. Seçin **hata ayıklama > tüm bölün**.

  ![](vs-callstack-images/image2.png "Hata ayıklama seçin > tüm bölün")

6. Seçin **hata ayıklama > dökümü olarak Kaydet**.

  ![](vs-callstack-images/image3.png "Hata ayıklama seçin > dökümü olarak Kaydet")

7. Değişiklik **farklı türde Kaydet** için **mini döküm (\*.dmp)**. Bu kadar küçük bir dosya daha üretecektir **mini döküm yığın ile**, ve Öbek genellikle donuyor tanılamak için ilgili değildir.

  ![](vs-callstack-images/image4.png "Bu kadar küçük bir dosya mini döküm yığın ile daha oluşturur ve Öbek genellikle donuyor tanılamak için ilgili değildir")

8. Döküm dosyasını kaydedin. Dosyayı çevrimiçi gönderme, boyutunu azaltmak için ZIP.
