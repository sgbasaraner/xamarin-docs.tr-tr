---
title: Xamarin.iOS UI iş parçacığı ile çalışma
description: Bu belge, kullanıcı Arabirimi iş parçacığı Xamarin.iOS içinde çalışmak üzere açıklar. Kullanıcı Arabirimi iş parçacığı yürütmesi açıklar, bir arka plan iş parçacığı örnek sağlar ve zaman uyumsuz ve bekleme inceler.
ms.prod: xamarin
ms.assetid: 98762ACA-AD5A-4E1E-A536-7AF3BE36D77E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 4328b84625aff4c92d6e97029ced7dde747d4fc4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790415"
---
# <a name="working-with-the-ui-thread-in-xamarinios"></a>Xamarin.iOS UI iş parçacığı ile çalışma

Uygulama kullanıcı arabirimleri, her zaman tek daha çok iş parçacıklı cihazları iş parçacıklı, – ekran yalnızca bir gösterimini yoktur ve görüntülenenleri yapılan herhangi bir değişiklik tek 'erişim noktası' koordine olmanız gerekir. Bu, birden çok iş parçacığı aynı piksel (örneğin) aynı anda güncelleştirme girişiminde bulunmasını engeller.

Kodunuzun yalnızca kullanıcı arabirimi denetimlerini ana (veya kullanıcı Arabirimi) yapılan değişiklikler iş parçacığı olmanız gerekir. Farklı bir iş parçacığında (örneğin, bir geri çağırma veya arka plan iş parçacığı) oluşan herhangi bir kullanıcı Arabirimi güncelleştirme ekrana çizilir değil veya bir kilitlenme bile neden olabilir.

## <a name="ui-thread-execution"></a>Kullanıcı Arabirimi iş parçacığı yürütme

Bir görünümde denetimleri oluşturuyorsanız, veya bir touch gibi kullanıcı tarafından başlatılan bir olay işleme, kodu kullanıcı Arabirimi iş parçacığı bağlamında zaten yürütülüyor.

Kod bir arka plan iş parçacığı, bir görev veya bir geri çağırma yürütüyor sonra ana kullanıcı Arabirimi iş parçacığı üzerinde yürütme değil olasıdır. Bu durumda çağrıda kodu kaydırılacağını `InvokeOnMainThread` veya `BeginInvokeOnMainThread` şöyle:

```csharp
InvokeOnMainThread ( () => {
    // manipulate UI controls
});
```

`InvokeOnMainThread` Yöntemi tanımlanmış `NSObject` , gelen (örneğin, bir görünüm veya Görünüm denetleyicisini) herhangi bir Uıkit nesne üzerinde tanımlanan yöntemler içinde çağrılabilir şekilde.

Kodunuzu yanlış iş parçacığından UI denetimi erişmeyi denerse Xamarin.iOS uygulamalarında hata ayıklama sırasında bir hata oluşturulur. Bu, izlemek ve InvokeOnMainThread yöntemiyle bu sorunları düzeltmek için yardımcı olur. Bu yalnızca hata ayıklama sırasında oluşur ve bir hata yayın derlemelerde oluşturmadığını. Hata iletisi şöyle görünür:

 ![](ui-thread-images/image10.png "Kullanıcı Arabirimi iş parçacığı yürütme")

 <a name="Background_Thread_Example" />


## <a name="background-thread-example"></a>Arka plan iş parçacığı örneği

Bir kullanıcı arabirimi denetim erişmeye çalışan örnek aşağıda verilmiştir (bir `UILabel`) basit bir iş parçacığı kullanan bir arka plan iş:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    label1.Text = "updated in thread"; // should NOT reference UILabel on background thread!
})).Start();
```

Kod atar `UIKitThreadAccessException` hata ayıklama. Sorunu düzeltmek (ve kullanıcı arabirimi denetimini yalnızca ana kullanıcı Arabirimi iş parçacığından eriştiğinden emin olmak için), kullanıcı Arabirimi denetimlerini içinde başvuran herhangi bir kod sarmalamak bir `InvokeOnMainThread` ifade şuna benzer:

```csharp
new System.Threading.Thread(new System.Threading.ThreadStart(() => {
    InvokeOnMainThread (() => {
        label1.Text = "updated in thread"; // this works!
    });
})).Start();
```

Bunu olmaz bu bu belge, ancak örneklerde kalanı için uygulamanızı ağ istek yaptığında, anımsaması önemli bir kavram kullanmak bildirim merkezi veya başka bir çalışacak bir tamamlanma-işleyici gerektiren diğer yöntemleri kullanır iş parçacığı.

 <a name="Async_Await_Example" />


## <a name="asyncawait-example"></a>Zaman uyumsuz ve bekleme örneği

C# 5 zaman uyumsuz ve bekleme anahtar kullanırken `InvokeOnMainThread` awaited bir görev tamamlandığında çünkü yöntemini çağıran iş parçacığında devam gerekli değildir.

(Yalnızca tanıtım amacıyla bir gecikme yöntemi çağrıda bekler) Bu örnek kod (TouchUpInside işleyici olduğu) kullanıcı Arabirimi iş parçacığı olarak adlandırılan bir zaman uyumsuz yöntemi gösterilir. İçeren yöntemi kullanıcı Arabirimi iş parçacığı üzerinde çağrıldığı için kullanıcı Arabirimi işlemleri ister metin ayarını bir `UILabel` veya gösteren bir `UIAlertView` zaman uyumsuz işlemleri arka plan iş parçacıklarında tamamladıktan sonra güvenli bir şekilde çağrılabilir.

```csharp
async partial void button2_TouchUpInside (UIButton sender)
{
    textfield1.ResignFirstResponder ();
    textfield2.ResignFirstResponder ();
    textview1.ResignFirstResponder ();
    label1.Text = "async method started";
    await Task.Delay(1000); // example purpose only
    label1.Text = "1 second passed";
    await Task.Delay(2000);
    label1.Text = "2 more seconds passed";
    await Task.Delay(1000);
    new UIAlertView("Async method complete", "This method", 
               null, "Cancel", null)
        .Show();
    label1.Text = "async method completed";
}
```

Arka plan iş parçacığından (değil ana kullanıcı Arabirimi iş parçacığı) async yöntemi çağrıldıysa sonra `InvokeOnMainThread` hala gerekli olacaktır.


## <a name="related-links"></a>İlgili bağlantılar

- [Denetimleri (örnek)](https://developer.xamarin.com/samples/Controls/)
- [İş parçacığı oluşturma](~/ios/app-fundamentals/threading.md)
