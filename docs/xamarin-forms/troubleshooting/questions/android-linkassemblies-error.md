---
title: Android derleme hatası – LinkAssemblies görevi beklenmedik şekilde başarısız oldu
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: de6e0b66cac688955d27ba2d0165d5a059d36c38
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30789546"
---
# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android derleme hatası – LinkAssemblies görevi beklenmedik şekilde başarısız oldu

Bir hata iletisi görebilirsiniz `The "LinkAssemblies" task failed unexpectedly` bir Xamarin.Android projesi oluşturma kullandığında Forms. Bağlayıcı etkin olmadığında bu olur (genellikle açık bir *sürüm* uygulama paketi boyutunu azaltmak için yapı); ve Android hedefleri son framework güncelleştirilmemiş oluşur. (Daha fazla bilgi: [Xamarin.Forms Android gereksinimleri için](~/xamarin-forms/get-started/installation.md#android))

Bu sorun için çözüm en son desteklenen Android SDK sürümlerine sahip ve ayarlayın emin olmaktır **hedef Framework** için **son yüklenmiş platform**. Ayrıca ayarlamanız önerilir **hedef Android sürümü** için **kullanım hedef Framework sürümü** ve **minimum Android sürümü** API 15 veya daha yüksek. Desteklenen yapılandırma olarak kabul edilir.

## <a name="setting-in-visual-studio-for-mac"></a>Visual Studio'da Mac için ayarlama

1.  Android projeye sağ tıklayın.
2.  Git **Yapı > Genel > hedef çerçevesi**.
3.  Ayarlama **hedef Framework: son yüklenmiş platform**.
4.  Proje seçenekleri hala, Git **Yapı > Android uygulaması**.
5.  Ayarlama **Minimum Android sürümü** API düzeyi 15 veya daha yüksek & **hedef Android sürümü** için **otomatik - kullanım hedef framework sürümü**.

## <a name="setting-in-visual-studio"></a>Visual Studio'da ayarlama

1.  Android projeye sağ tıklayın.
2.  Git **uygulama** proje seçenekleri.
3.  Ayarlama **Android sürümüyle derleme** & **hedef Android sürümü** ayarlar **kullanımı son Platform** / **kullanın SDK sürümüyle derleme**.
4.  Ayarlama **Minimum Android hedef** API 15 veya daha yüksek ayarlama.

Bu ayarları güncelleştirdikten sonra temizleme ve değişikliklerinizi toplanma emin olmak için projenizi yeniden derleyin.
