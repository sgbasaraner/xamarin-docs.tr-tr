---
title: Mac uygulama yapılandırması
description: Bu kılavuzda, yayının Xamarin.Mac uygulamanızı nasıl yapılandıracağınız anlatılmaktadır.
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: e47ff676b4dd02d5312a74fb699ed594b5e0f944
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="mac-app-configuration"></a>Mac uygulama yapılandırması

_Bu kılavuzda, yayının Xamarin.Mac uygulamanızı nasıl yapılandıracağınız anlatılmaktadır._


## <a name="mac-app-configuration"></a>Mac uygulama yapılandırması

Visual Studio'da Mac uygulama projesi Mac için sağ tıklatın ve seçin **seçenekleri**.


### <a name="application-settings"></a>Uygulama ayarları

Xamarin.Mac uygulamanın uygulama ayarlarını değiştirmek için çift **Info.plist** dosyasını **çözüm paneli**:

![Info.plist dosyasını seçerek](app-configuration-images/config04.png "Info.plist dosyasını seçme")

Bu uygulama için kullanılabilir seçenekler görüntülenir:

 [![Info.plist dosyasını düzenleyerek](app-configuration-images/config01.png "Info.plist dosyasını düzenleme")](app-configuration-images/config01-large.png#lightbox)

Xamarin.Mac ile oluşturulan çalışan Mac uygulamaları aşağıdaki sistem gereksinimleri vardır:

- Mac OS X 10.7 veya üzeri çalıştıran bir Mac bilgisayar.


### <a name="signing-settings"></a>Ayarları imzalama

**Mac imzalama** bölümünü **proje seçenekleri** iletişim kutusu self yayın için test için veya sürümü Apple App Store aracılığıyla Xamarin.Mac uygulamayı imzalamak Geliştirici sağlar:

[![Mac imzalama Düzenleyicisi'ni](app-configuration-images/config02.png "Mac imzalama penceresi")](app-configuration-images/config02-large.png#lightbox)

Burada arasından seçim kimlik sağlama profili ve derlendiğinde uygulamayı imzalamak için kullanılan herhangi bir özel yetkilendirmeler. Geliştirici başka bir Mac üzerindeki uygulamayı yüklemek için kullanılan yükleyici isteğe bağlı olarak oturum açın


### <a name="build-settings"></a>Yapı ayarları

**Mac yapı** bölümünü **proje seçenekleri** iletişim kutusu macOS hangi sürümünün app destekleyecek denetlemek ve isteğe bağlı olarak oluşturmak için bir Xamarin.Mac uygulama mimarisi seçmek Geliştirici sağlar Uygulama başarıyla derlendiğinde bir yükleme paketi:

 [![Yapı ayarları düzenleme](app-configuration-images/config03.png "derleme ayarlarını düzenleme")](app-configuration-images/config03-large.png#lightbox)


## <a name="related-links"></a>İlgili bağlantılar

- [Yükleme](/visualstudio/mac/installation/)
- [Merhaba, Mac örnek](~/mac/get-started/hello-mac.md)
- [Mac App Store, uygulamaları dağıtma](https://developer.apple.com/devcenter/mac/checklist/)
- [Geliştirici kimliği ve ağ geçidi](https://developer.apple.com/resources/developer-id/)
