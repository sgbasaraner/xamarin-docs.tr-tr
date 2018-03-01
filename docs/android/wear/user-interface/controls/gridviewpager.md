---
title: GridViewPager
ms.topic: article
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: f156d802d807c4087331ca5796b046f8f5f2fa0d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) örnek Android takmak için 2B Seçici Gezinti desen uygulamak nasıl gösterir.

![Kare ekranındaki örnek ekran GridViewPager görüntüsü](gridviewpager-images/gridviewpager.png)

İlk ekleyin [Xamarin Android takmak desteği](http://www.nuget.org/packages/Xamarin.Android.Wear/) NuGet paketini projenize.

Düzen XML şöyle görünür:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Oluşturma bir [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (veya bir alt kümesi gibi [ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html) kullanıcı görüntülenecek görünüm sağlamak için.

[Örnek bağdaştırıcısı](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) geçersiz kılma işlemleri için de dahil olmak üzere gerekli yöntemleri gösterilmektedir `RowCount`, `GetColumnCount`, `GetBackground`, ve `GetFragment`

Bağdaştırıcısının gösterildiği gibi wire:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>İlgili bağlantılar

- [Google's 2D Picker doc](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Android.support.wearable belgeleri](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (sample)](https://developer.xamarin.com/samples/GridViewPager/)
