---
title: My Mac nerede?
description: Mac Xamarin.iOS projeler derlemek Visual Studio için yapılandırma yönergeleri
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: f3e2988c9700549db24ad69277df9e412c99caed
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="wheres-my-mac"></a>My Mac nerede?

_Mac Xamarin.iOS projeler derlemek Visual Studio için yapılandırma yönergeleri_

Geçerli **kararlı** Mac farklı önceki sürümler için Visual Studio için Xamarin sürümü etkileşime[^](#earlier-versions).

Mac Xamarin Mac aracısı olarak kullanmak için etkinleştirmelisiniz **uzaktan oturum açma** Mac üzerinde

1. Açık *Spotlight* (`Cmd-Space`) arayın ve **uzaktan oturum açma** seçin ve ardından **paylaşım** sonucu. Bu açılır **sistem tercihleri** adresindeki **paylaşım** paneli.

  ![](visual-studio-ssh-images/spotlight.png "Uzaktan oturum açma için Spotlight arama")

2. Değer çizgilerinin **uzaktan oturum açma** seçeneğini **hizmet** Mac bağlanmak Visual Studio için Xamarin izin vermek üzere soldaki listesi

  ![](visual-studio-ssh-images/sharing.png "Değer çizgilerinin hizmeti listesinde uzak oturum seçeneği")

3. Olduğundan emin olun **uzaktan oturum açma** erişim için izin verilecek şekilde ayarlandığını **tüm kullanıcılar**, ya da Mac kullanıcı adınız veya Grup listesinde eklenir, sağdaki listeden kullanıcılar izin verilen.

Aynı ağda olması durumunda, Mac artık Visual Studio tarafından bulunabilir olması gerekir.
Visual Studio mac'e bağlanma girişiminde bulunduğunda, Visual Studio'dan Mac kullanıcı kimlik bilgilerinizi kullanarak oturum açmak için istenir.

## <a name="where-can-i-find-more-information"></a>Daha fazla bilgi nereden bulabilirim?

Kılavuzları ayarlamak ve aşağıda listelenen yeni aracı anlamanıza yardımcı olması için kullanılabilir vardır:

- [Mac bilgisayara bağlayarak](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Sorun giderme](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University - Xamarin Mac arası](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## <a name="migrating-from-previous-versions"></a>Önceki sürümlerden geçirme

Visual Studio için önceki sürümlerinde Xamarin kullandıysanız tanımanız **Xamarin.iOS yapı konağı** Mac'inizde başlatılması için gereken uygulama ve ardından *eşleştirilmiş* bir PIN numarası ile yoluyla, Visual Studio örneği.

Yapı konak uygulama artık Visual Studio için Xamarin bu sürümü ile gerekli değildir.
