---
title: "İzlenecek yol - etkinlik durumu kaydetme"
description: "Biz, etkinlik Yaşam Döngüsü Kılavuzu'nda durumu kaydedilmeden arkasında teorik ele aldıktan; Şimdi, şimdi bir örnek yol."
ms.topic: article
ms.prod: xamarin
ms.assetid: A6090101-67C6-4BDD-9416-F2FB74805A87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 36cabddc2439d64ad2d1135bbd0d453a7f411750
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---saving-the-activity-state"></a>İzlenecek yol - etkinlik durumu kaydetme

_Biz, etkinlik Yaşam Döngüsü Kılavuzu'nda durumu kaydedilmeden arkasında teorik ele aldıktan; Şimdi, şimdi bir örnek yol._

## <a name="activity-state-walkthrough"></a>Etkinlik durumu gözden geçirme

Açalım **ActivityLifecycle_Start** proje (içinde [ActivityLifecycle](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle) örnek), onu oluşturun ve çalıştırın. Bu etkinlik yaşam döngüsü ve çeşitli yaşam döngüsü yöntemleri nasıl adlı göstermek için iki etkinlik içeren çok basit bir projedir. Uygulama, ekranın başlattığınızda `MainActivity` görüntülenir: 

[ ![Etkinlik bir ekran](saving-state-images/01-activity-a-sml.png)](saving-state-images/01-activity-a.png)

### <a name="viewing-state-transitions"></a>Görüntüleme durumu geçişleri

Bu örnekte her yöntemin etkinlik durumu göstermek için IDE uygulama çıkış penceresine yazar. (Visual Studio çıkış penceresini açmak için şunu yazın **CTRL-ALT-O**; Visual Studio'da Mac için çıkış penceresini açmak için **Görünüm > klavye takımı > Uygulama çıktısı**.)

Uygulamayı ilk kez başlatıldığında, çıkış penceresi durum değişikliklerini görüntüler *etkinlik A*: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Biz tıklattığınızda **Başlat etkinliği B** düğmesi, biz bkz *etkinlik A* duraklatma ve sırasında durdurma *etkinlik B* , durum değişikliklerini gider: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.SecondActivity] Activity B - OnCreate
[ActivityLifecycle.SecondActivity] Activity B - OnStart
[ActivityLifecycle.SecondActivity] Activity B - OnResume
[ActivityLifecycle.MainActivity] Activity A - OnStop
```

Sonuç olarak, *etkinlik B* başlatıldığından ve yerine görüntülenen *etkinlik A*: 

[ ![Etkinlik B ekranı](saving-state-images/02-activity-b-sml.png)](saving-state-images/02-activity-b.png)

Biz tıklattığınızda **geri** düğmesini *etkinlik B* yok ve *etkinlik A* devam ettirildiğinde: 

```shell
[ActivityLifecycle.SecondActivity] Activity B - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnRestart
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
[ActivityLifecycle.SecondActivity] Activity B - OnStop
[ActivityLifecycle.SecondActivity] Activity B - OnDestroy
```
### <a name="adding-a-click-counter"></a>Tıklatın sayaç ekleme

Ardından, uygulama sayar ve tıklatıldığında sayısını görüntüleyen bir düğme sahibiz şekilde değiştirmek için yapacağız. İlk olarak, ekleyelim bir `_counter` örnek değişkeni `MainActivity`: 

```csharp
int _counter = 0;
```

Ardından, düzenleyelim **Resource/layout/Main.axml** düzeni dosya ve yeni bir ekleme `clickButton` kullanıcı düğmesi tıkladığını sayısını görüntüler. Elde edilen **Main.axml** aşağıdaki benzemelidir: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/myButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/mybutton_text" />
    <Button
        android:id="@+id/clickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/counterbutton_text" />
</LinearLayout>
```

Sonuna aşağıdaki kod ekleyelim [OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) yönteminde `MainActivity` &ndash; bu kodu tanıtıcıları tıklatın olaylarından `clickButton`:

```csharp
var clickbutton = FindViewById<Button> (Resource.Id.clickButton);
clickbutton.Text = Resources.GetString (
    Resource.String.counterbutton_text, _counter);
clickbutton.Click += (object sender, System.EventArgs e) =>
{
    _counter++;
    clickbutton.Text = Resources.GetString (
        Resource.String.counterbutton_text, _counter);
} ;
```

Ne zaman biz oluşturmak ve uygulamayı yeniden çalıştırın, yeni bir düğme görüntülenir artırır ve değerini görüntüler `_counter` üzerinde her tıklatın:

[![Dokunma sayım Ekle](saving-state-images/03-touched-sml.png)](saving-state-images/03-touched.png)

Ancak biz yatay modu için cihazı döndürdüğünüzde, bu sayı kaybolur:

[ ![Yatay olarak döndürme sayısını sıfır olarak ayarlar](saving-state-images/05-rotate-nosave-sml.png)](saving-state-images/05-rotate-nosave.png)

Uygulama çıkış inceleyerek, biz, bkz: *etkinlik A* duraklatıldı, durduruldu, yok, yeniden, yeniden ve sonra dikey döndürme yatay modu için sırasında sürdürüldü: 

```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
```

Çünkü *etkinlik A* yok ve aygıtın ne zaman Döndürülmüş yeniden yeniden örneği durumuna kaybolur. Ardından, biz kaydetmek ve örnek durumunu geri yüklemek için kod ekleyeceksiniz.

### <a name="adding-code-to-preserve-instance-state"></a>Preserve örnek durum için kod ekleme

Bir yönteme ekleyelim `MainActivity` örneği durumu. Önce *etkinlik A* olan yok, Android otomatik olarak çağırır [OnSaveInstanceState](https://developer.xamarin.com/api/member/Android.App.Activity.OnSaveInstanceState/p/Android.OS.Bundle/) ve içinde geçirir bir [paket](https://developer.xamarin.com/api/type/Android.OS.Bundle/) biz bizim örnek durum depolamak için kullanabilirsiniz. Şimdi bizim tıklatın sayısı bir tamsayı değeri kaydetmek için kullanın:

```csharp
protected override void OnSaveInstanceState (Bundle outState)
{
    outState.PutInt ("click_count", _counter);
    Log.Debug(GetType().FullName, "Activity A - Saving instance state");

    // always call the base implementation!
    base.OnSaveInstanceState (outState);    
}
```

Zaman *etkinlik A* yeniden ve sürdürüldü, Android geçirir bu `Bundle` uygulamasına geri bizim `OnCreate` yöntemi. Kod ekleyelim `OnCreate` geri yüklemek için `_counter` geçilen değerinden `Bundle`. Satır hemen önce aşağıdaki kodu ekleyin nerede `clickbutton` tanımlanır: 

```csharp
if (bundle != null)
{
    _counter = bundle.GetInt ("click_count", 0);
    Log.Debug(GetType().FullName, "Activity A - Recovered instance state");
}
```

Derleme ve uygulamayı yeniden çalıştırın, sonra birkaç kez ikinci düğmesini tıklatın. Biz yatay modu için cihazı döndürdüğünüzde sayısı korunur!

[ ![Ekran döndürme dört korunur sayısını gösterir](saving-state-images/06-rotate-save-sml.png)](saving-state-images/06-rotate-save.png)


Ne olduğunu görmek için çıkış penceresini bir göz atalım:
    
```shell
[ActivityLifecycle.MainActivity] Activity A - OnPause
[ActivityLifecycle.MainActivity] Activity A - Saving instance state
[ActivityLifecycle.MainActivity] Activity A - OnStop
[ActivityLifecycle.MainActivity] Activity A - On Destroy

[ActivityLifecycle.MainActivity] Activity A - OnCreate
[ActivityLifecycle.MainActivity] Activity A - Recovered instance state
[ActivityLifecycle.MainActivity] Activity A - OnStart
[ActivityLifecycle.MainActivity] Activity A - OnResume
``` 

Önce [OnStop](https://developer.xamarin.com/api/member/Android.App.Activity.OnStop/) yöntemi çağrıldı, bizim yeni `OnSaveInstanceState` yöntemi kaydetmek için çağrıldı `_counter` değeri bir `Bundle`. Android geçirilen bu `Bundle` dön bize bu çağrıldığında bizim `OnCreate` yöntemi ve biz kullanılan, geri yüklemek için mümkün `_counter` yerden değeri.


## <a name="summary"></a>Özet

Bu walkthough, biz durumu verilerini korumak için etkinlik yaşam döngüsünün bizim bilgi kullandınız. 



## <a name="related-links"></a>İlgili bağlantılar

- [ActivityLifecycle (örnek)](https://developer.xamarin.com/samples/monodroid/ActivityLifecycle)
- [Etkinlik yaşam döngüsü](~/android/app-fundamentals/activity-lifecycle/index.md)
- [Android Activity](https://developer.xamarin.com/api/type/Android.App.Activity/)
