---
title: Parçaları yönetme
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/07/2018
ms.openlocfilehash: b39067b0cb1bbb344866761042db0125fd4a4dc5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="managing-fragments"></a>Parçaları yönetme

Parçaları yönetmeye yardımcı olmak için Android sağlar `FragmentManager` sınıfı. Her etkinlik bir örneğine sahip `Android.App.FragmentManager` , bulma veya kendi parçaları dinamik olarak değiştirin. Bu değişiklikler her kümesi olarak bilinen bir *işlem*ve sınıfında bulunan API'leri birini kullanarak gerçekleştirilen `Android.App.FragmentTransation`, tarafından yönetilen `FragmentManager`. Bir etkinlik şuna benzer bir işlem başlangıç:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Parçaları yapılan bu değişiklikleri başlığında gerçekleştirilen `FragmentTransaction` gibi yöntemleri kullanarak örnek `Add()`, `Remove(),` ve `Replace().` kullanarak değişiklikler uygulanır `Commit()`. Bir işlemde değişiklikleri hemen gerçekleştirilmez.
Bunun yerine, bunlar etkinliğe ilişkin kullanıcı Arabirimi iş parçacığı üzerinde olabildiğince çabuk çalıştırmak üzere zamanlanır.

Aşağıdaki örnek, bir parça eklemek için var olan bir kapsayıcı gösterilmektedir:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Bir işlem sonra kaydedilmiş ise `Activity.OnSaveInstanceState()` olan adlı bir özel durum oluşturulur. Etkinlik durumu kaydettiğinde, Android de barındırılan tüm parçaları durumunu kaydeder olduğundan bu gerçekleşir. Bu noktadan sonra parça işlemler uygulanır, etkinlik geri yüklendiğinde bu işlemler durumu kaybolur.

Etkinliğin parça işlemleri kaydetmek mümkündür [geri yığın](http://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) yapılan bir çağrı yaparak `FragmentTransaction.AddToBackStack()`. Bu kullanıcı aracılığıyla geriye doğru gidin sağlar parça değiştirir **geri** düğmesine basıldığında. Bu yöntem çağrısı, olmadan kaldırılır parçaları yok edilmesi ve kullanıcı geri boyunca etkinlik giderse kullanılamayacaktır.

Aşağıdaki örnekte nasıl kullanılacağını gösterir `AddToBackStack` yöntemi bir `FragmentTransaction` ilk parça geri yığında durumunu korurken bir parçasını değiştirmek için:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```


## <a name="communicating-with-fragments"></a>Parçaları ile iletişim

*FragmentManager* bir etkinliğe bağlı parçaları tümünün bildiği ve bunlar parçaları bulmak için iki yöntem sunar:

-   **FindFragmentById** &ndash; bu yöntem bir parça parça bir işlemin bir parçası eklendiğinde Düzen dosyasında belirtilen kimliği veya kapsayıcı kimliği kullanarak bulur.

-   **FindFragmentByTag** &ndash; bu yöntem Düzen dosyasında sağlanmadığından veya bir işlem içinde eklenen bir etiketi olan bir parça bulmak için kullanılır.

Parçaları ve etkinlikleri başvurusu `FragmentManager`, aynı teknikleri ve geriye aralarında iletişim kurmak için kullanılır. Bir uygulama, bu iki yöntemden birini kullanarak bir başvuru parça bulmak, bu başvuruyu uygun türü cast ve yöntemleri parçasını doğrudan çağırmak. Aşağıdaki kod parçacığını bir örnek sağlar:

Ayrıca kullanmak üzere etkinlik için olası `FragmentManager` parçaları bulmak için:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```


### <a name="communicating-with-the-activity"></a>Etkinliği ile iletişim

Bir parça kullanmak mümkündür `Fragment.Activity` ana bilgisayar başvuru özelliği. Etkinliğe daha belirli bir tür atama tarafından aşağıdaki örnekte gösterildiği gibi çağrı yöntemleri ve özellikleri, ana bilgisayardaki bir etkinliğe mümkündür:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
