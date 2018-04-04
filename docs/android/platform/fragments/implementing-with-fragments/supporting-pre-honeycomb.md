---
title: Destek paketleri kullanılarak öncesi bal peteği Android destekleme
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 75e12821d96b98037c568fb5dac69ba53a507670
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>Destek paketleri kullanılarak öncesi bal peteği Android destekleme

*Android destek paketi* geri bazı yeni API'nin bağlantı noktası kitaplıklarının oluşur &ndash; parçaları gibi &ndash; Android eski sürümleri için. Bu nedenle Android destek paketi ekleyerek, biz uygulamamız Android 2.3 cihazlarda aşağıdaki ekranlar tarafından gösterildiği gibi çalıştırabilirsiniz:

[![İzlenecek yol ve ayrıntıları etkinliği ekran görüntüleri parça](supporting-pre-honeycomb-images/01-sml.png)](supporting-pre-honeycomb-images/01.png#lightbox)

## <a name="adding-the-support-package"></a>Destek paketi ekleme

Android destek paketi bir Xamarin.Android uygulaması için otomatik olarak eklenmez. Xamarin sağlar [Android destek kitaplığı v4 NuGet paketi](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) bir Xamarin.Android uygulamasına destek kitaplıkları ekleme basitleştirmek için.
Destek paketleri uygulamayı dahil, bir Xamarin.Android dahil etmek için [Android destek kitaplığı v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) Xamarin.Android projenize bileşen aşağıdaki ekran görüntüsünde gösterildiği gibi:

[![Android destek kitaplığı v4 paketi ekleme](supporting-pre-honeycomb-images/02-sml.png)](supporting-pre-honeycomb-images/02.png#lightbox)

Paket eklendikten sonra hedef çerçevesini Android 2.2 veya üzeri değiştirin:

[![Hedef Framework API düzeyini değiştirme işleminin ekran görüntüsü](supporting-pre-honeycomb-images/03-sml.png)](supporting-pre-honeycomb-images/03.png#lightbox)

Ayrıca, en düşük Android sürümü aynı API düzeyi hedefler emin olun:

[![Minimum Android sürümü ayarlama işleminin ekran görüntüsü](supporting-pre-honeycomb-images/04-sml.png)](supporting-pre-honeycomb-images/04.png#lightbox)

### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>MainActivity FragmentActivity türetilen değiştirme

Parçaları kullanan herhangi bir etkinlik öğesinden devralmalıdır `Xamarin.Support.V4.App.FragmentActivity`. Android sürümünden bağımsız olarak etkinlikler tarafından barındırılacak şekilde parçaları sağladığından Bu sınıf destek paketi gerekli bir parçasıdır. `MainActivity` yalnızca bir küçük değişiklikler gerektirir — şimdi öğesinden devralmalıdır `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```


### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>FragmentActivity türetilen DetailsActivity değiştirme

`DetailsActivity` gelen değiştirilmelidir bir `Activity` için bir `FragmentActivity`. Olarak `FragmentManager` olan Android öncesi bal peteği sürümleriyle uyumlu değil, Android destek paketi bir sarmalayıcı sınıfı içerir `SupportFragmentManager`, geriye dönük uyumluluk sağlar. Her `FragmentActivity` sahip bir `SupportFragmentManager` özelliği ve `DetailsActivity` kullanmak üzere değiştirilir `SupportFragmentManager` yerine `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

Biz bu değişiklikleri tamamladıktan sonra biz artık Android 1.6 ve daha yüksek çalıştırabilirsiniz ve kullanıcı arabirimimizi bizim hedef aygıt boyutunu ayarlamak için parçaları kullanan bir uygulamaya sahip.


## <a name="related-links"></a>İlgili bağlantılar

- [Android kitaplığı v4 destek](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
