---
title: "İş Parçacığı Oluşturma"
ms.topic: article
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8be599f5b6541ef738ffa47a01374fd7f90044a4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="threading"></a>İş Parçacığı Oluşturma

Xamarin.iOS çalışma zamanı .NET geliştiricilerinin API'leri iş parçacığı oluşturma erişmenizi sağlayan, açık hem de iş parçacıklarının kullanın ( `System.Threading.Thread, System.Threading.ThreadPool`), tam aralığı, görev destekler API'lerini yanı sıra zaman uyumsuz temsilci desenleri veya BeginXXX yöntemleri dolaylı olarak kullanırken Paralel kitaplığı.



Xamarin kesinlikle önerir, kullandığınız [görev paralel kitaplığı](http://msdn.microsoft.com/en-us/library/dd460717.aspx)

 (TPL) birkaç nedeni uygulamaları oluşturmak için:-varsayılan TPL Zamanlayıcı görev yürütme işlemi, burada çok fazla iş parçacığı sona bir senaryo kaçınarak gerçekleştirilir gibi gerekli iş parçacığı sayısını sırayla dinamik olarak büyüyecektir iş parçacığı havuzu için temsilci seçme CPU süresi için rekabet ayarlama. 
-  TPL görevleri açısından işlemleri hakkındaki görüşlerinizi daha kolaydır. Kolayca üzerlerinde değişiklik, onları zamanlamak, kendi yürütme serileştirmek veya birçok zengin bir dizi API ile paralel başlatın. 
-  Yeni C# zaman uyumsuz dil uzantıları ile programlama temelidir. 


İş parçacığı havuzu, yavaş gerektiği gibi iş parçacığı sayısı sistem, sistem yükü ve uygulamalarınızın talepleri kullanılabilir CPU çekirdeği sayısı temel alınarak büyüyecektir. Yöntemleri çağırma tarafından bu iş parçacığı havuzu kullanabilirsiniz `System.Threading.ThreadPool` veya varsayılan kullanarak `System.Threading.Tasks.TaskScheduler` (parçası *paralel çerçeveler*).

Genellikle geliştiriciler, esnek uygulamaları oluşturmak ihtiyaç duydukları ve döngü çalıştırmak ana UI engellemek istediğinizde değil iş parçacığı kullanır.

 <a name="Developing_Responsive_Applications" />


## <a name="developing-responsive-applications"></a>Esnek uygulamaları geliştirme

Kullanıcı Arabirimi öğeleri erişimi, uygulamanız için ana döngü çalışan iş parçacığı sınırlı olmalıdır. Bir iş parçacığından ana UI değişiklik yapmak istiyorsanız, kullanarak kod kuyruk [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/type/Foundation.NSObject/), şöyle:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    //
    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

Yukarıdaki kod bağlamında ana iş parçacığı temsilci içinde uygulamanızın kilitlenme herhangi yarış durumları neden olmadan çağırır.

 <a name="Threading_and_Garbage_Collection" />


## <a name="threading-and-garbage-collection"></a>Parçacıkları ve çöp toplama

Yürütme sırasında Objective-C çalışma zamanı oluşturmak ve nesneleri bırakın. Nesneleri "Otomatik-Sürüm Objective-C çalışma zamanı serbest bırakmak için bu nesneler" için işaretlenmiş geçerli iş parçacığının `NSAutoReleasePool`. Xamarin.iOS oluşturur bir `NSAutoRelease` havuzu için her bir iş parçacığından `System.Threading.ThreadPool` ve ana iş parçacığı için. Bu uzantı tarafından TaskScheduler varsayılan System.Threading.Tasks kullanılarak oluşturulan tüm iş parçacıklarının kapsar.

Kullanarak kendi iş parçacıklarını oluşturursanız `System.Threading` sahip olduğunuz sağlamak zorunda `NSAutoRelease` verilerin sızmasını engellemek için havuz. Bunu yapmak için yalnızca aşağıdaki kod parçası, iş parçacığı kaydır:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Not: Bu yana Xamarin.iOS 5.2, kendi sağlamak zorunda değilsiniz `NSAutoReleasePool` bir otomatik olarak sizin yerinize sağlanacak artık.


## <a name="related-links"></a>İlgili bağlantılar

- [Kullanıcı Arabirimi iş parçacığı ile çalışma](~/ios/user-interface/ios-ui/ui-thread.md)
