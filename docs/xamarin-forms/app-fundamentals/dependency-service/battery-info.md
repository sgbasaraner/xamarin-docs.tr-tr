---
title: Pil durumu denetleniyor
description: "DependencyService erişim pil bilgileri yerel olarak her platform için kullanın."
ms.topic: article
ms.prod: xamarin
ms.assetid: CF1C5A73-84ED-407D-BDC5-EB1D83D2D3DB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: 28c8ecc77aaeb00eff6f343ad41fed1c653362db
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="checking-battery-status"></a>Pil durumu denetleniyor

Bu makalede, pil durumu denetleyen bir uygulamayı oluşturulmasını anlatılmaktadır. Bu makale üzerinde pil eklentisi tarafından Ahmet Montemagno temel alır. Daha fazla bilgi için bkz: [GitHub deposuna](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery).

Xamarin.Forms geçerli pil durumunu denetleme işlevselliği içermediğinden bu uygulamayı kullanmaya ihtiyaç duyacaklardır [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) yerel API'leri yararlanmak için.  Bu makalede kullanmak için aşağıdaki adımları kapsar `DependencyService`:

- **[Arabirimi oluşturma](#Creating_the_Interface)**  &ndash; arabirimi paylaşılan kodda nasıl oluşturulduğunu anlayın.
- **[iOS uygulaması](#iOS_Implementation)**  &ndash; iOS için yerel kodda arabirimini uygulayan öğrenin.
- **[Android uygulaması](#Android_Implementation)**  &ndash; arabirimini yerel kodda Android için uygulama öğrenin.
- **[Windows Phone Uygulama](#Windows_Phone_Implementation)**  &ndash; arabirimini yerel kodda Windows Phone için uygulama öğrenin.
- **[Evrensel Windows Platform uygulaması](#UWPImplementation)**  &ndash; arabirimini yerel kodda Evrensel Windows Platformu (UWP) uygulaması öğrenin.
- **[Paylaşılan kod içinde uygulama](#Implementing_in_Shared_Code)**  &ndash; nasıl kullanacağınızı öğrenin `DependencyService` paylaşılan koddan yerel uygulama çağırmak için.

Tamamlandığında, kullanılarak uygulama `DependencyService` aşağıdaki yapı ayarlanmıştır:

![](battery-info-images/battery-diagram.png "DependencyService uygulama yapısı")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

İlk olarak, bir arabirim istenen işlevselliği ifade paylaşılan kod içinde oluşturun. Cihaz veya ücretlendirme, cihaz güç nasıl alıyor olup olmadığını ve uygulama denetimi pil söz konusu olduğunda, kalan pil yüzdesi ilgili bilgiler verilmiştir:

```csharp
namespace DependencyServiceSample
{
  public enum BatteryStatus
  {
    Charging,
    Discharging,
    Full,
    NotCharging,
    Unknown
  }

  public enum PowerSource
  {
    Battery,
    Ac,
    Usb,
    Wireless,
    Other
  }

  public interface IBattery
  {
    int RemainingChargePercent { get; }
    BatteryStatus Status { get; }
    PowerSource PowerSource { get; }
  }
}
```

Bu arabirim paylaşılan kodda karşı kodlama Xamarin.Forms uygulaması her platformda güç yönetimi API'leri erişmesine izin verir.

> [!NOTE]
> **Not**: arabirimini uygulayan sınıflar çalışmak için parametresiz bir oluşturucusu olmalıdır `DependencyService`. Kurucular arabirimleri tarafından tanımlanamıyor.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS uygulaması

`IBattery` Her platforma özgü uygulama projesinde arabirimi uygulanmadı. İOS uygulama yerel kullanacağı [ `UIDevice` ](https://developer.xamarin.com/api/type/UIKit.UIDevice/) pil bilgilere erişmek için API'ler. Aşağıdaki sınıf parametresiz bir oluşturucuya sahip Not böylece `DependencyService` yeni örnekleri oluşturabilirsiniz:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;

namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery
  {
    public BatteryImplementation()
    {
      UIDevice.CurrentDevice.BatteryMonitoringEnabled = true;
    }

    public int RemainingChargePercent
    {
      get
      {
        return (int)(UIDevice.CurrentDevice.BatteryLevel * 100F);
      }
    }

    public BatteryStatus Status
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return BatteryStatus.Charging;
          case UIDeviceBatteryState.Full:
            return BatteryStatus.Full;
          case UIDeviceBatteryState.Unplugged:
            return BatteryStatus.Discharging;
          default:
            return BatteryStatus.Unknown;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        switch (UIDevice.CurrentDevice.BatteryState)
        {
          case UIDeviceBatteryState.Charging:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Full:
            return PowerSource.Ac;
          case UIDeviceBatteryState.Unplugged:
            return PowerSource.Battery;
          default:
            return PowerSource.Other;
        }
      }
    }
  }
}
```

Son olarak, bu eklemek `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` deyimleri:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS;//necessary for registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.iOS
{
  public class BatteryImplementation : IBattery {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder `IBattery` anlamına arabirimi `DependencyService.Get<IBattery>` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir:

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android uygulaması

Android uygulaması kullanan [ `Android.OS.BatteryManager` ](https://developer.xamarin.com/api/type/Android.OS.BatteryManager/) API. Bu uygulama pil izinlerinde eksiklik işlemek için denetimleri gerektiren iOS sürümden daha karmaşık geçerlidir:

```csharp
using System;
using Android;
using Android.Content;
using Android.App;
using Android.OS;
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid;

namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery
  {
    private BatteryBroadcastReceiver batteryReceiver;
    public BatteryImplementation() { }

    public int RemainingChargePercent
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              var level = battery.GetIntExtra(BatteryManager.ExtraLevel, -1);
              var scale = battery.GetIntExtra(BatteryManager.ExtraScale, -1);

              return (int)Math.Floor(level * 100D / scale);
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }

      }
    }

    public DependencyServiceSample.BatteryStatus Status
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;
              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);
              if (isCharging)
                return DependencyServiceSample.BatteryStatus.Charging;

              switch(status)
              {
                case (int)BatteryStatus.Charging:
                  return DependencyServiceSample.BatteryStatus.Charging;
                case (int)BatteryStatus.Discharging:
                  return DependencyServiceSample.BatteryStatus.Discharging;
                case (int)BatteryStatus.Full:
                  return DependencyServiceSample.BatteryStatus.Full;
                case (int)BatteryStatus.NotCharging:
                  return DependencyServiceSample.BatteryStatus.NotCharging;
                default:
                  return DependencyServiceSample.BatteryStatus.Unknown;
              }
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }

    public PowerSource PowerSource
    {
      get
      {
        try
        {
          using (var filter = new IntentFilter(Intent.ActionBatteryChanged))
          {
            using (var battery = Application.Context.RegisterReceiver(null, filter))
            {
              int status = battery.GetIntExtra(BatteryManager.ExtraStatus, -1);
              var isCharging = status == (int)BatteryStatus.Charging || status == (int)BatteryStatus.Full;

              var chargePlug = battery.GetIntExtra(BatteryManager.ExtraPlugged, -1);
              var usbCharge = chargePlug == (int)BatteryPlugged.Usb;
              var acCharge = chargePlug == (int)BatteryPlugged.Ac;

              bool wirelessCharge = false;
              wirelessCharge = chargePlug == (int)BatteryPlugged.Wireless;

              isCharging = (usbCharge || acCharge || wirelessCharge);

              if (!isCharging)
                return DependencyServiceSample.PowerSource.Battery;
              else if (usbCharge)
                return DependencyServiceSample.PowerSource.Usb;
              else if (acCharge)
                return DependencyServiceSample.PowerSource.Ac;
              else if (wirelessCharge)
                return DependencyServiceSample.PowerSource.Wireless;
              else
                return DependencyServiceSample.PowerSource.Other;
            }
          }
        }
        catch
        {
          System.Diagnostics.Debug.WriteLine ("Ensure you have android.permission.BATTERY_STATS");
          throw;
        }
      }
    }
  }
}
```

Bu ekleme `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` deyimleri:

```csharp
...
using BatteryStatus = Android.OS.BatteryStatus;
using DependencyServiceSample.Droid; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.Droid
{
  public class BatteryImplementation : IBattery {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder `IBattery` anlamına arabirimi `DependencyService.Get<IBattery>` kullanılabilir paylaşılan kod bir örneğini oluşturabilirsiniz.

<a name="Windows_Phone_Implementation" />

## <a name="windows-phone-implementation"></a>Windows Phone uygulaması

Windows Phone güç API Android ve iOS eşdeğerleri daha az bilgi sağladığından bu uygulama Android ve iOS sürümlerinden daha sınırlıdır.

```csharp
using System;
using Windows.ApplicationModel.Core;
using DependencyServiceSample.WinPhone;

namespace DependencyServiceSample.WinPhone
{
  public class BatteryImplementation : IBattery
  {
    private int last;
    private BatteryStatus status = BatteryStatus.Unknown;

    public BatteryImplementation()
    {
      last = DefaultBattery.RemainingChargePercent;
    }

    Windows.Phone.Devices.Power.Battery battery;
    private Windows.Phone.Devices.Power.Battery DefaultBattery
    {
      get { return battery ?? (battery = Windows.Phone.Devices.Power.Battery.GetDefault()); }
    }
    public int RemainingChargePercent
    {
      get
      { return DefaultBattery.RemainingChargePercent; }
    }

    public  BatteryStatus Status
    {
      get { return status; }
    }

    public PowerSource PowerSource
    {
      get
      {
        if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
          return PowerSource.Ac;

        return PowerSource.Battery;
      }
    }
  }
}
```

Bu ekleme `[assembly]` öznitelik sınıfı yukarıda (ve tanımlanmış tüm ad alanlarını dışında) dahil olmak üzere tüm gerekli `using` deyimleri.

```csharp
using System;
using Windows.ApplicationModel.Core;
using DependencyServiceSample.WinPhone;

[assembly: Xamarin.Forms.Dependency (typeof (BatteryImplementation))]
namespace DependencyServiceSample.WinPhone {
  public class BatteryImplementation : IBattery {
    ...
```

Bu öznitelik sınıfı uygulaması kaydeder `IBattery` anlamına arabirimi `DependencyService.Get<IBattery>` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir.

<a name="UWPImplementation" />

## <a name="universal-windows-platform-implementation"></a>Evrensel Windows Platform uygulaması

UWP uygulaması kullanan `Windows.Devices.Power` API'leri pil durumu hakkında bilgi edinmek için:

```csharp
using DependencyServiceSample.UWP;
using Xamarin.Forms;

[assembly: Dependency(typeof(BatteryImplementation))]
namespace DependencyServiceSample.UWP
{
    public class BatteryImplementation : IBattery
    {
        private BatteryStatus status = BatteryStatus.Unknown;
        Windows.Devices.Power.Battery battery;

        public BatteryImplementation()
        {
        }

        private Windows.Devices.Power.Battery DefaultBattery
        {
            get
            {
                return battery ?? (battery = Windows.Devices.Power.Battery.AggregateBattery);
            }
        }

        public int RemainingChargePercent
        {
            get
            {
                var finalReport = DefaultBattery.GetReport();
                var finalPercent = -1;

                if (finalReport.RemainingCapacityInMilliwattHours.HasValue && finalReport.FullChargeCapacityInMilliwattHours.HasValue)
                {
                    finalPercent = (int)((finalReport.RemainingCapacityInMilliwattHours.Value /
                        (double)finalReport.FullChargeCapacityInMilliwattHours.Value) * 100);
                }
                return finalPercent;
            }
        }

        public BatteryStatus Status
        {
            get
            {
                var report = DefaultBattery.GetReport();
                var percentage = RemainingChargePercent;

                if (percentage >= 1.0)
                {
                    status = BatteryStatus.Full;
                }
                else if (percentage < 0)
                {
                    status = BatteryStatus.Unknown;
                }
                else
                {
                    switch (report.Status)
                    {
                        case Windows.System.Power.BatteryStatus.Charging:
                            status = BatteryStatus.Charging;
                            break;
                        case Windows.System.Power.BatteryStatus.Discharging:
                            status = BatteryStatus.Discharging;
                            break;
                        case Windows.System.Power.BatteryStatus.Idle:
                            status = BatteryStatus.NotCharging;
                            break;
                        case Windows.System.Power.BatteryStatus.NotPresent:
                            status = BatteryStatus.Unknown;
                            break;
                    }
                }
                return status;
            }
        }

        public PowerSource PowerSource
        {
            get
            {
                if (status == BatteryStatus.Full || status == BatteryStatus.Charging)
                {
                    return PowerSource.Ac;
                }
                return PowerSource.Battery;
            }
        }
    }
}
```

`[assembly]` Ad alanı bildiriminin üstüne öznitelik sınıfı uygulaması kaydeder `IBattery` anlamına arabirimi `DependencyService.Get<IBattery>` paylaşılan kodda bir örneğini oluşturmak için kullanılabilir.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Paylaşılan kod içinde uygulama

Her platform için arabirimi uygulanmıştır, paylaşılan uygulama onu yararlanmak için yazılabilir. Uygulama oluşacak bir düğmeyi içeren bir sayfa, o zaman güncelleştirmeleri dokunduğunuz geçerli pil durumu ile kendi metin. Kullandığı `DependencyService` örneğini almak için `IBattery` arabirimi. Çalışma zamanında, bu örneği yerel SDK tam erişime sahip platforma özgü uygulaması olacaktır.

```csharp
public MainPage ()
{
    var button = new Button {
        Text = "Click for battery info",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    button.Clicked += (sender, e) => {
        var bat = DependencyService.Get<IBattery>();

        switch (bat.PowerSource){
          case PowerSource.Battery:
            button.Text = "Battery - ";
            break;
          case PowerSource.Ac:
            button.Text = "AC - ";
            break;
          case PowerSource.Usb:
            button.Text = "USB - ";
            break;
          case PowerSource.Wireless:
            button.Text = "Wireless - ";
            break;
          case PowerSource.Other:
          default:
            button.Text = "Other - ";
            break;
        }
        switch (bat.Status){
          case BatteryStatus.Charging:
            button.Text += "Charging";
            break;
          case BatteryStatus.Discharging:
            button.Text += "Discharging";
            break;
          case BatteryStatus.NotCharging:
            button.Text += "Not Charging";
            break;
          case BatteryStatus.Full:
            button.Text += "Full";
            break;
          case BatteryStatus.Unknown:
          default:
            button.Text += "Unknown";
            break;
        }
    };
    Content = button;
}
```

Bu uygulamayı iOS, Android veya Windows platformları çalıştıran ve düğmesine basarak aygıtın geçerli güç durumunu yansıtacak şekilde güncelleştirme düğmesi metni neden olur.

![](battery-info-images/battery.png "Pil durumu örneği")


## <a name="related-links"></a>İlgili bağlantılar

- [DependencyService (örnek)](https://developer.xamarin.com/samples/DependencyService)
- [DependencyService (örnek) kullanma](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Xamarin.Forms örnekleri](https://github.com/xamarin/xamarin-forms-samples)
