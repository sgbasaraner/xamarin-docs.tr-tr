---
title: Önsöz Kurumsal uygulama geliştirme
description: Bu bölümde, Xamarin.Forms kullanarak kurumsal uygulama düzenleri için bir önsöz sağlar.
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fd085d2fb12e82233f6d3be2e2773a84539f837b
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242374"
---
# <a name="preface-to-enterprise-app-development"></a>Önsöz Kurumsal uygulama geliştirme

Bu e-kitap Xamarin.Forms kullanarak platformlar arası Kurumsal uygulamaları oluşturma yönergeler sağlanmaktadır. Xamarin.Forms geliştiricilerin yerel kullanıcı iOS, Android ve evrensel Windows Platformu (UWP) dahil olmak üzere platformları arasında paylaşılabilir arabirimi düzenleri kolayca oluşturmanıza olanak tanır bir platformlar arası UI araç takımıdır. Çalışan (B2E) işletmeler için işletmeye (B2B) ve Tüketici (B2C) uygulamaları için kod tüm hedef platformlar arasında paylaşma olanağı sağlar ve daha düşük sahip olma maliyeti (TCO) yardımcı iş için kapsamlı bir çözüm sağlar.

Kılavuzu uyarlanabilir, sürdürülebilir ve sınanabilir Xamarin.Forms Kurumsal uygulamaları geliştirmek için Mimari Kılavuzu sağlar. Gevşek bağlantı korurken MVVM, bağımlılık ekleme, gezinti, doğrulama ve yapılandırma yönetimi, uygulama konusunda yönergeler sağlanır. Ayrıca, aynı zamanda bir yönerge yoktur gerçekleştirdiği kimlik doğrulama ve yetkilendirme kapsayıcılı mikro ve birim testi verilere IdentityServer ile.

Kaynak kodu Kılavuzu birlikte [eShopOnContainers mobil uygulama](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile)ve kaynak kodunu [eShopOnContainers başvuru uygulama](https://github.com/dotnet-architecture/eShopOnContainers). EShopOnContainers mobil uygulama kapsayıcılı mikro uygulama eShopOnContainers başvuru olarak bilinen bir dizi bağlayan Xamarin.Forms kullanılarak geliştirilen platformlar arası kurumsal bir uygulamadır. Ancak, eShopOnContainers mobil uygulama kapsayıcılı mikro dağıtma kaçınmak isteyen olanlar için sahte hizmetlerinden verileri kullanmak üzere yapılandırılabilir.

## <a name="whats-left-out-of-this-guides-scope"></a>Bu kılavuz kapsamı dışında kalan

Bu kılavuz, Xamarin.Forms ile bilginiz okuyucular adresindeki amaçlamaktadır. Xamarin.Forms ayrıntılı bir giriş için bkz: [Xamarin.Forms belgelerine](~/xamarin-forms/index.yml), ve [Xamarin.Forms ile Mobile Apps oluşturma](https://aka.ms/xamebook).

Kılavuzu için tamamlayıcı [.NET mikro: mimarisi kapsayıcılı .NET uygulamaları için](https://aka.ms/microservicesebook), geliştirme ve kapsayıcılı mikro dağıtma üzerine odaklanır. Diğer kılavuzlarını okuma değerinde dahil [Architecting ve ASP.NET Core ve Microsoft Azure ile Modern Web uygulamaları geliştirme](http://aka.ms/WebAppEbook), [Microsoft Platformu ve araçlarıilekapsayıcılıDockeruygulamayaşamdöngüsü](http://aka.ms/dockerlifecycleebook), ve [Microsoft Platformu ve araçları mobil uygulama geliştirme için](http://aka.ms/MobAppDev/StndPDF).

## <a name="who-should-use-this-guide"></a>Bu kılavuz kullanan

Bu kılavuz kitlesi çoğunlukla geliştiriciler ve mimari ve platformlar arası Kurumsal uygulamaları Xamarin.Forms kullanarak uygulama hakkında bilgi almak ister misiniz mimarları içindir.

İkincil bir izleyici ne yaklaşmak için platformlar arası Kurumsal uygulama geliştirme Xamarin.Forms kullanarak seçmek için karar vermeden önce bir mimari ve teknoloji genel bakış almak istediğiniz teknik karar alıcılar ' dir.

## <a name="how-to-use-this-guide"></a>Bu kılavuz kullanma

Bu kılavuz, Xamarin.Forms kullanarak platformlar arası Kurumsal uygulamaları oluşturma üzerine odaklanmıştır. Bu nedenle, bu tür uygulamalar ve bunların teknik konular anlamanın bir temel tamamının okunmalıdır. Kendi örnek uygulama ile birlikte kılavuzu ayrıca bir başlangıç noktası veya yeni bir kuruluş uygulaması oluşturmak için başvuru olarak hizmet verebilir. İlişkili örnek uygulaması, yeni uygulama için veya bir uygulamanın bileşen bölümleri düzenlemek nasıl görmek için bir şablon olarak kullanın. Daha sonra geri Mimari Kılavuzu için bu kılavuzun bakın.

Platformlar arası Kurumsal uygulama geliştirme Xamarin.Forms kullanarak ortak bir anlayış sağlamaya yardımcı olmak için takım üyeleri için bu kılavuzu iletmek çekinmeyin. Herkes ortak bir dizi terimler çalışma ve ilkeleri temelindeki sahip tutarlı bir uygulama mimari desenleri ve uygulamalar sağlamaya yardımcı olur.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
