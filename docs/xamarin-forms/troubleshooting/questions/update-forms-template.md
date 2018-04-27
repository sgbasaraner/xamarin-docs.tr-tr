---
title: Xamarin.Forms varsayılan şablonu için daha yeni bir NuGet paketi güncelleştirebilir miyim?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 6aea0faa65944f33783940178a1d2ce3ef65df1a
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2018
---
# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Xamarin.Forms varsayılan şablonu için daha yeni bir NuGet paketi güncelleştirebilir miyim?

Bu kılavuz Xamarin.Forms PCL şablon örnek olarak kullanılmıştır, ancak aynı genel yöntem Xamarin.Forms paylaşılan proje şablonu için de çalışır. Bu kılavuzu için 1.5.1.6471 2.1.0.6529 Xamarin.Forms güncelleştirme örneği ile yazılmıştır, ancak aynı adımları diğer sürümler yerine varsayılan olarak ayarlamak mümkündür.

1.  Özgün şablon kopyalama `.zip` gelen:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2.  Unzip `.zip` geçici bir konuma.

3.  Tüm Forms paketin eski sürümü oluşumları kullanmak istediğiniz yeni sürüm olarak değiştirin.
    *   `FormsTemplate\FormsTemplate.vstemplate`
    *   `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    *   `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Örnek: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4.  Ana "name" öğesini değiştirin [birden çok proje şablonu dosyası](http://msdn.microsoft.com/library/ms185308.aspx) (`Xamarin.Forms.PCL.vstemplate`) benzersiz hale getirmek için. Örneğin:
    > <Name>Boş uygulama (Xamarin.Forms taşınabilir) - 2.1.0.6529</Name>

5.  Tüm şablon klasörünü yeniden zip. Özgün dosya yapısı emin olun `.zip` dosya. `Xamarin.Forms.PCL.vstemplate` Dosyasının üst kısmında olması `.zip` dosya, herhangi bir klasör içinde değil.

6.  Kullanıcı başına Visual Studio şablonları klasörünüzde "Mobile Apps" alt oluşturun:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7.  Yeni daraltılmış yukarı şablon klasörünü yeni "Mobile Apps" dizinine kopyalayın.

8.  3. adımındaki eşleştiğinden NuGet paketini indirin. Örneğin, [ http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529 ](http://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (Ayrıca bkz. [ http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file ](http://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)) ve Xamarin Visual Studio uzantıları klasörünü uygun alt klasöre kopyalayın:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
