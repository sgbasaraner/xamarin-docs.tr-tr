---
title: Xamarin.iOS iş parçacığı oluşturma
description: Bu belge, bir Xamarin.iOS uygulamasına System.Threading API'leri kullanmayı açıklar. Bu, görev paralel yanıt veren uygulamaları ve çöp toplama kitaplığı, ele alınmaktadır.
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/05/2017
ms.openlocfilehash: 8e4ee10fdabdcbb4c6cefe02b15dc93459708364
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350426"
---
# <a name="threading-in-xamarinios"></a>Xamarin.iOS iş parçacığı oluşturma

Xamarin.iOS çalışma zamanı geliştiriciler için .NET API'leri, açıkça iş parçacığı kullanırken hem de iş parçacığı erişmenizi sağlar (`System.Threading.Thread, System.Threading.ThreadPool`) ve örtük olarak tam farklı destekleyen API'yi yanı sıra zaman uyumsuz temsilci desenleri veya BeginXXX yöntemleri kullanılırken Görev paralel kitaplığı.



Xamarin yöntemi kesinlikle önermektedir, kullandığınız [görev paralel Kitaplığı](http://msdn.microsoft.com/library/dd460717.aspx) (TPL) birkaç nedeni uygulamaları oluşturmak için:
-  Varsayılan TPL Zamanlayıcı görevi yürütme akabinde işlem, burada CPU süresi için rekabet yedekleme çok fazla iş parçacığı sona senaryo kaçınarak gerçekleşir gibi gerekli iş parçacığı sayısını dinamik olarak büyüyecektir iş parçacığı havuzu için temsilci olarak seçer. 
-  TPL görev açısından işlemleri hakkında düşünmeye daha kolaydır. Kolayca üzerlerinde değişiklik, onları zamanlamak, yürütülmesi serileştirmek veya birçok zengin bir API ile paralel başlatın. 
-  Yeni C# zaman uyumsuz dil uzantıları ile programlama için bir altyapıdır. 


İş parçacığı havuzu, yavaş iş parçacığı sayısını gerektiği gibi sistem, sistem yükü ve uygulama Taleplerinizi kullanılabilir CPU çekirdek sayısı temel alınarak çıkarılır. Yöntemleri çağırarak bu iş parçacığı havuzu kullanabilirsiniz `System.Threading.ThreadPool` veya varsayılan kullanarak `System.Threading.Tasks.TaskScheduler` (parçası *paralel çerçeveleri*).

Genellikle geliştiriciler, yanıt veren uygulamaları oluşturmak ihtiyaç duydukları ve döngünün ana UI engellemek istediğinizde değil iş parçacığı kullanır.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Hızlı yanıt veren uygulamaları geliştirme

Uygulamanız için ana döngünün çalıştığı iş parçacığı UI öğelerine erişim sınırlandırılmalıdır. Ana kullanıcı Arabirimi için bir iş parçacığından değişiklik yapmak istiyorsanız, kullanarak kod kuyruk [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), şöyle:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

Yukarıda ana iş parçacığı bağlamı Temsilcide içinde kod, büyük olasılıkla uygulamanızı kilitlenebiliyordu herhangi bir yarış durumlarını neden olmadan çağırır.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Parçacıkları ve atık toplama

Yürütme sırasında Objective-C çalışma zamanı oluşturma ve nesneleri bırakın. Nesneleri "Otomatik-Sürüm Objective-C çalışma zamanı bu nesnelere serbest bırakılır" için işaretlenmiş geçerli iş parçacığı `NSAutoReleasePool`. Xamarin.iOS oluşturur bir `NSAutoRelease` havuzu için her bir iş parçacığından `System.Threading.ThreadPool` ve ana iş parçacığı için. Bu uzantı tarafından TaskScheduler varsayılan System.Threading.Tasks kullanılarak oluşturulan herhangi bir iş parçacığı kapsar.

Kullanarak kendi iş parçacığı oluşturursanız `System.Threading` olduğunuz sağlamanız gereken `NSAutoRelease` verilerin sızmasını engellemek için havuz. Bunu yapmak için aşağıdaki kod parçası, iş parçacığı kaydırma yeterlidir:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Not: Bu yana Xamarin.iOS 5.2 kendi sağlamak erişiminiz yok `NSAutoReleasePool` artık gibi bir otomatik olarak size sağlanacaktır.


## <a name="related-links"></a>İlgili bağlantılar

- [Kullanıcı Arabirimi İş Parçacığı ile Çalışma](~/ios/user-interface/ios-ui/ui-thread.md)
