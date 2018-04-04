---
title: Eclipse kitaplığı projesinde bağlama
description: Bu kılavuzda, Eclipse Android kitaplığı projesinde bağlamak için Xamarin.Android proje şablonları kullanımı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: CEE90F8A-164B-4155-813A-7537A665A7E7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 1c3c33f1b958aff5818b26e4408e60f449b46f41
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="binding-an-eclipse-library-project"></a>Eclipse kitaplığı projesinde bağlama

_Bu kılavuzda, Eclipse Android kitaplığı projesinde bağlamak için Xamarin.Android proje şablonları kullanımı açıklanmaktadır._


## <a name="overview"></a>Genel Bakış

Ancak. AAR dosyalarını giderek Android kitaplığı dağıtım norm haline gelmesini, bazı durumlarda için bir bağlama oluşturmak gerekli olan bir *Android kitaplığı projesi*. Android kitaplık projeleri paylaşılabilir kodu içeren özel Android projeleri ve Android uygulama projeleri tarafından başvurulan kaynaklar ' dir. Genellikle, kitaplık Eclipse IDE'de oluşturulduğunda bir Android kitaplığıdır projesine bağlar.
Bu kılavuz, bir Android kitaplığıdır projesi oluşturmak nasıl örnekler sağlar. Eclipse projenizin dizin yapısından ZIP.

Bir APK derlenmemiş ve, kendi olmayan, Android kitaplık projeleri normal Android projelerden farklı bir cihaza dağıtılabilir. Bunun yerine, bir Android kitaplığıdır proje bir Android uygulaması projesi tarafından başvurulan amaçlanmıştır. Bir Android uygulaması projesi yapılandırıldığında, Android Kitaplığı projesini ilk derlenir. Android uygulama projesi sonra derlenmiş bir Android kitaplığıdır projeye absorbed ve dağıtım için APK içine kaynakları ve kod içerir. Bu farklılık nedeniyle, bir Android kitaplığıdır proje için bir bağlama oluşturma bir Java için bir bağlama oluşturmaktan daha biraz farklıdır. JAR veya. AAR dosyası.



## <a name="walkthrough"></a>İzlenecek yol

Bir Android kitaplığıdır projesi Xamarin.Android Java bağlama bir projede bu Eclipse Android kitaplığı projesi oluşturmak için ilk gerekli kullanmaktır. Aşağıdaki ekran görüntüsünde derleme sonra bir Android kitaplığıdır proje örneği gösterilmektedir: 

[![Eclipse örnek kitaplığı projesi](binding-a-library-project-images/build-lib-in-eclipse.png)](binding-a-library-project-images/build-lib-in-eclipse.png#lightbox)

Geçici bir Android kitaplığıdır projeden kaynak kodu derlendikten dikkat edin. Adlı JAR dosyasını **android mapviewballoons.jar**, ve kaynakları kopyalandığını **res/bin/crunch** klasör. 

Android kitaplığı projesi Eclipse'te derlendikten sonra onu sonra Java Xamarin.Android bağlama project kullanarak bağlanabilir. İlk bir. ZIP dosyası içeren oluşturulmalıdır **bin** ve **res** klasörleri Android kitaplık projesinin. Araya giren kaldırmak önemlidir **crunch** alt kaynakları bulunan böylece **bin/res**. Aşağıdaki ekran içeriğini bir tür gösterir. ZIP dosyası: 

[![Android kitaplığının içeriğini .zip proje](binding-a-library-project-images/contents-of-zip-file.png)](binding-a-library-project-images/contents-of-zip-file.png#lightbox)

Bu. ZIP dosyası, ardından aşağıdaki ekran görüntüsünde gösterildiği gibi Java Xamarin.Android bağlama projeye eklenir:

[![Zip Java bağlama projesine eklendi](binding-a-library-project-images/zip-in-binding-project.png)](binding-a-library-project-images/zip-in-binding-project.png#lightbox)

Dikkat yapı eylemi. ZIP dosyası otomatik olarak ayarlanmış **LibraryProjectZip**.

Varsa. Android kitaplığı proje için gerekli olan JAR dosyalarını, bunlar eklenmesi için **Jar'lar** Java bağlama kitaplığı proje klasöründe ve **yapı eylemi** kümesine **ReferenceJar**. Bunun bir örneğini aşağıdaki ekran görüntüsünde görebilirsiniz: 

[![ReferenceJar için ayarlanmış bir eylem oluşturun](binding-a-library-project-images/set-to-referencejar.png)](binding-a-library-project-images/set-to-referencejar.png#lightbox)

Bu adımları tamamlandıktan sonra önceki üzerinde bu belgede açıklanan Java Xamarin.Android bağlama proje kullanılabilir.

> [!NOTE]
> Diğer IDE Android Kitaplığı projelerinde derleme şu anda desteklenmiyor. Diğer IDE aynı dizin yapısını veya dosyalar oluşturamaz **bin** klasörü Eclipse olarak. 


## <a name="summary"></a>Özet

Bu makalede, bir Android kitaplığıdır projesi bağlama sürecinde gitti. Eclipse Android kitaplığı projesinde oluşturduğumuz sonra zip dosyasından oluşturduğumuz **bin** ve **res** klasörleri Android kitaplık projesinin. Ardından, bu zip Java Xamarin.Android bağlama projesi oluşturmak için kullandık. 

