---
title: "Web hizmetlerine erişimi kimlik doğrulaması"
description: "Bu kılavuz, kimlik doğrulama Hizmetleri kullanıcılarının yalnızca kendi verilerine erişim yaparken bir arka uç paylaşmalarını sağlamak için bir Xamarin.Forms uygulaması şekilde entegre açıklanmaktadır. Yerleşik kimlik doğrulama mekanizmaları kullanılarak farklı sağlayıcıları tarafından sunulan ve OAuth kimlik sağlayıcısı karşı kimlik doğrulaması için Xamarin.Auth bileşenini kullanarak bir REST hizmeti temel kimlik doğrulaması kullanarak kapsanan konular içerir."
ms.topic: article
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
ms.openlocfilehash: 0139a7a921861b5d1c9a3639ee2c7e25ee6cf5fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="authenticating-access-to-web-services"></a>Web hizmetlerine erişimi kimlik doğrulaması

_Bu kılavuz, kimlik doğrulama Hizmetleri kullanıcılarının yalnızca kendi verilerine erişim yaparken bir arka uç paylaşmalarını sağlamak için bir Xamarin.Forms uygulaması şekilde entegre açıklanmaktadır. Yerleşik kimlik doğrulama mekanizmaları kullanılarak farklı sağlayıcıları tarafından sunulan ve OAuth kimlik sağlayıcısı karşı kimlik doğrulaması için Xamarin.Auth bileşenini kullanarak bir REST hizmeti temel kimlik doğrulaması kullanarak kapsanan konular içerir._

## <a name="authenticating-a-restful-web-servicerestmd"></a>[Bir RESTful Web hizmeti kimlik doğrulama](rest.md)

HTTP kaynaklara erişimi denetlemek için birden fazla kimlik doğrulama mekanizmaları kullanımını destekler. Temel kimlik doğrulaması doğru kimlik bilgilerine sahip bu istemcilere kaynaklara erişim sağlar. Bu makalede, temel kimlik doğrulaması RESTful web hizmeti kaynaklarına erişimi korumak için nasıl kullanılacağı gösterilmektedir.

## <a name="authenticating-users-with-an-identity-provideroauthmd"></a>[Bir kimlik sağlayıcısı ile kullanıcıların kimlik doğrulaması](oauth.md)

Xamarin.Auth platformlar arası SDK kullanıcıların kimlik doğrulaması ve hesaplarını depolamak içindir. Google, Microsoft, Facebook ve Twitter gibi kimlik sağlayıcıları kullanma için destek sağlayan OAuth kimlik doğrulayan içerir. Bu makalede Xamarin.Auth bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmaktadır.

## <a name="authenticating-users-with-azure-mobile-appsazuremd"></a>[Azure Mobile Apps kullanıcıların kimlik doğrulaması](azure.md)

Azure Mobile Apps Dış kimlik sağlayıcıları, çeşitli kimlik doğrulama ve uygulama kullanıcıları yetkilendirmek desteklemek üzere kullanır. İzinleri daha sonra tablolarda yalnızca kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için ayarlanabilir. Bu makalede Azure Mobile Apps bir Xamarin.Forms uygulaması kimlik doğrulama işleminde yönetmek için nasıl kullanılacağı açıklanmaktadır.

## <a name="authenticating-users-with-azure-active-directory-b2cazure-ad-b2cmd"></a>[Kullanıcıların Azure Active Directory B2C ile kimlik doğrulaması](azure-ad-b2c.md)

Azure Active Directory B2C bir tüketiciye yönelik web ve mobil uygulamaları için bulut kimlik yönetimi çözümüdür. Bu makalede Microsoft kimlik doğrulama kitaplığı (MSAL) ve Azure Active Directory B2C tüketici kimlik yönetimini Xamarin.Forms uygulamasına tümleştirmek için nasıl kullanılacağı gösterilmektedir.

## <a name="integrating-azure-active-directory-b2c-with-azure-mobile-appsazure-ad-b2c-mobile-appmd"></a>[Azure Active Directory B2C Azure mobil uygulamaları ile tümleştirme](azure-ad-b2c-mobile-app.md)

Azure Active Directory B2C, Azure Mobile Apps için kimlik doğrulama iş akışı yönetmek için kullanılabilir. Bu yaklaşım ile kimlik yönetimi deneyimi buluta tam olarak tanımlanır ve mobil uygulama kodunuzu değiştirmeden değiştirilebilir. Bu makalede, Azure Active Directory B2C kimlik doğrulaması ve yetkilendirme için bir Azure Mobile Apps örneğine Xamarin.Forms ile sağlamak için nasıl kullanılacağı gösterilmektedir.

## <a name="authenticating-users-with-an-amazon-simpledb-serviceawsmd"></a>[Kullanıcıların bir Amazon SimpleDB hizmeti ile kimlik doğrulaması](aws.md)

Amazon SimpleDB kendi kaynak tabanlı izinler sistem sağlamaz. Bunun yerine, bir kimlik sağlayıcısı kimlik doğrulaması, kullanıcıların yalnızca kendi verilerine erişim SimpleDB etki alanında olmasını sağlamak için kullanılabilir. Bu makalede, kullanıcıların kendi SimpleDB veriye erişimi kısıtlamak açıklanmaktadır.


## <a name="related-links"></a>İlgili bağlantılar

- [Web hizmetlerine giriş](~/cross-platform/data-cloud/web-services/index.md)
- [Zaman uyumsuz desteğine genel bakış](~/cross-platform/platform/async.md)
