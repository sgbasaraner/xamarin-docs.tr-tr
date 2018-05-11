---
title: Gerçek bir Xcode projesi kullanma örneği
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 4e30b07c10de439df8e1ec1706845150e8c54a41
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="real-world-example-using-an-xcode-project"></a>Gerçek bir Xcode projesi kullanma örneği


**Bu örnekte [Facebook POP kitaplığından](https://github.com/facebook/pop).**

Yeni sürüm 3.0 hedefi Sharpie Xcode projeleri giriş olarak destekler. Bu projeleri doğru üstbilgi dosyaları ve yerel kitaplığı derlemek gerekli ve bu nedenle çok bağlamak gerekli derleyici bayrakları belirtin. Amaç Sharpie ilk seçebilir _hedef_ ve aksi değil belirtilmedikçe bir projenin varsayılan yapılandırması.

Proje ve başlık dosyaları ayrıştırmak hedefi Sharpie çalışmadan önce onu yapılandırmanız gerekir. Projeleri genellikle her zaman bu bağlamak denemeden önce tam projeyi derlemek en iyisidir üstbilgi dosyaları dış tüketim ve tümleştirme, doğru yapısı derleme aşamaları sahiptir.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

