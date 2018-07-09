---
title: Gerçek bir Xcode projesini kullanarak örneği
description: Bu belge, Objective C# bağlamaları Objective-C kodu oluşturma işlemini basitleştirmek Sharpie, doğrudan bir giriş olarak bir Xcode projesini kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855253"
---
# <a name="real-world-example-using-an-xcode-project"></a>Gerçek bir Xcode projesini kullanarak örneği

**Bu örnekte [Facebook POP kitaplığından](https://github.com/facebook/pop).**

Yeni sürüm 3. 0'da, giriş olarak Xcode projeleri hedefi Sharpie destekler. Bu projeleri derleme yerel kitaplık için gereken ve bu nedenle, çok bağlamak gerekli derleyici bayraklarına ve doğru üst bilgi dosyaları belirtin. Amaç Sharpie ilk seçer _hedef_ ve aksi takdirde yapmak istenirse değil, bir projenin varsayılan yapılandırması.

Proje ve üstbilgi dosyalarını ayrıştırmak hedefi Sharpie çalışmadan önce onu yapılandırmanız gerekir. Projeleri, genellikle her zaman bu bağlama çalışmadan önce tam projeyi derlemek en iyisidir, dış tüketim ve tümleştirme, üst bilgi dosyaları doğru yapı derleme aşamaya sahiptir.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

## <a name="related-links"></a>İlgili bağlantılar

- [Xamarin University Ders: bir Objective-C bağlama kitaplığı oluşturma](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University Ders: derleme hedefi Sharpie ile bir Objective-C bağlama kitaplığı](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)