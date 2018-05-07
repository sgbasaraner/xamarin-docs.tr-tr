---
title: Xamarin ile sürekli tümleştirme giriş
description: Sürekli Tümleştirme otomatik derleme derler ve isteğe bağlı olarak kod eklendiğinde veya projenin sürüm denetimi deponuza geliştiriciler tarafından değiştirilen bir uygulama testleri bir yazılım mühendislik uygulamadır. Bu makalede, sürekli tümleştirme genel kavramlar ve bazı seçenekleri sürekli tümleştirme için Xamarin projelerle ele alınacaktır.
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/04/2017
ms.openlocfilehash: 54f3d3c475e506e7d451af5125e90a0f51aa7374
ms.sourcegitcommit: c9ebf456e1c6924956bedb13f4ea78ff09f7b1a0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="introduction-to-continuous-integration-with-xamarin"></a>Xamarin ile sürekli tümleştirme giriş

_Sürekli Tümleştirme otomatik derleme derler ve isteğe bağlı olarak kod eklendiğinde veya projenin sürüm denetimi deponuza geliştiriciler tarafından değiştirilen bir uygulama testleri bir yazılım mühendislik uygulamadır. Bu makalede, sürekli tümleştirme genel kavramlar ve bazı seçenekleri sürekli tümleştirme için Xamarin projelerle ele alınacaktır._

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]


##  <a name="introduction-to-continuous-integrationtoolsciintro-to-cimd"></a>[Sürekli Tümleştirme giriş](~/tools/ci/intro-to-ci.md)

Bu bölüm, sürekli tümleştirme ve bunların ilişkileri ile ilgili farklı bileşenleri kapsar. Aşağıdaki bölümler içinde ele alınmıştır sürekli tümleştirme ortamları özetlenmektedir.

[!include[](~/tools/ci/includes/firewall-information.md)]

## <a name="working-with-continuous-integration-environments"></a>Sürekli Tümleştirme ortamları ile çalışma


### <a name="using-app-center-build-with-xamarinappcenterbuildxamarin"></a>[Xamarin ile Uygulama Merkezi Derlemesini Kullanma](/appcenter/build/xamarin/)

Uygulama Merkezi ile Xamarin.iOS ve Xamarin.Android çözümlerini doğrudan GitHub, VSTS veya Bitbucket oluşturun.

### <a name="using-teamcity-with-xamarintoolsciteamcitymd"></a>[Xamarin ile TeamCity Kullanma](~/tools/ci/teamcity.md)

Bu kılavuz, mobil uygulamaları derlemek ve bunları App merkezi testine göndermek için TeamCity kullanma ile ilgili adımlar açıklanmaktadır.

### <a name="using-jenkins-with-xamarintoolscijenkins-walkthroughmd"></a>[Xamarin ile Jenkins Kullanma](~/tools/ci/jenkins-walkthrough.md)

Bu kılavuz, Jenkins sürekli tümleştirme sunucusu olarak ayarlayın ve Xamarin ile oluşturulan mobil uygulamaları derleme otomatikleştirmek göstermektedir. Jenkins OS x'te yüklemek, yapılandırmak ve sürüm denetimi sistemine değişiklikler uygulandıktan zaman Xamarin.iOS ve Xamarin.Android uygulamaları derlemek için işler ayarlamak nasıl açıklar.
