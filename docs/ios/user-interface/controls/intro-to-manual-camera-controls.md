---
title: Xamarin.iOS el ile kamera denetimleri
description: Bu belgede nasıl iOS AVFoundation framework ile Xamarin.iOS el ile kamera denetimleri etkinleştirmek için kullanılabileceği açıklanır. El ile kamera denetimler denetim odağı, beyaz dengesi ve Etkilenme ayarları kullanıcıya izin verir.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a0f605a38117df87a03801c3b9d86b0b7361c232
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790831"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Xamarin.iOS el ile kamera denetimleri

Tarafından sağlanan el ile kamera denetimleri `AVFoundation Framework` iOS 8'de, bir iOS cihazın kamera üzerinde tam denetim olabilmesi bir mobil uygulama izin verin. Bu hassas denetim düzeyini profesyonel düzeyi kamera uygulamalar oluşturmak ve hala bir resim veya video önünde bulundurularak kamera parametrelerinin uyguladıkça tarafından sanatçı kompozisyonlarınıza sağlamak için kullanılabilir.

Bu denetimler de burada sonuçlar daha az doğruluk veya güzelliği görüntünün doğru sağlamıştır ve daha bazı özellik veya öğesi gerçekleştirilmesini görüntünün vurgulama doğrultusunda sağlamıştır bilimsel endüstriyel uygulamaları veya geliştirirken yararlı olabilir.

## <a name="avfoundation-capture-objects"></a>AVFoundation yakalama nesneleri

Video veya hala bir iOS cihazında kamera kullanarak görüntüleri alma olsun, bu görüntüleri yakalamak için kullanılan büyük ölçüde aynı işlemidir. Bu varsayılan otomatik kamera denetimleri veya yeni el ile kamera denetimleri yararlanmak olanları kullanan uygulamaları geçerlidir:

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation yakalama nesneleri genel bakış")](intro-to-manual-camera-controls-images/image1.png#lightbox)

Giriş öğesinden alınır bir `AVCaptureDeviceInput` içine bir `AVCaptureSession` tarafından yolu, bir `AVCaptureConnection`. Sonuç sabit bir resim veya video akışı olarak ya da çıktı olur. Tüm süreci tarafından denetlenen bir `AVCaptureDevice`.

## <a name="manual-controls-provided"></a>Sağlanan el ile denetimleri

İOS 8 tarafından sağlanan yeni API'leri kullanarak uygulama denetim aşağıdaki kamera özelliklerden birini gerçekleştirebilirsiniz:

-  **El ile odak** – son kullanıcının odağı doğrudan denetimini olanak vererek, bir uygulama gerçekleştirilecek görüntü üzerinde daha fazla denetim sağlayabilir.
-  **El ile Etkilenme** – Etkilenme üzerinde el ile denetim sağlayarak, bir uygulama kullanıcıların özgürlüğü sağlar ve bunları stilize bir görünüm elde etmek izin verebilirsiniz.
-  **El ile Beyaz Denge** – Beyaz Denge görüntüdeki rengi ayarlamak için kullanılan — gerçekçi görünmesi için genellikle. Farklı renk etme farklı açık kaynakları sahip ve bir görüntüyü yakalamak için kullanılan kamera ayarları için bu farklılıklar dengelemek için ayarlandı. Yeniden beyaz dengesi kullanıcı denetime izin vererek, kullanıcıların otomatik olarak yapılamaz ayarlamalar yapabilirsiniz.


var olan iOS API'ları bu görüntünün üzerinde ayrıntılı denetim sağlayan geliştirmeler yakalama işlemi ve iOS 8 uzantıları sağlar.

## <a name="bracketed-capture"></a>Köşeli parantez içindeki yakalama

Köşeli parantez içindeki yakalama el ile kamera yukarıda sunulan denetimleri ayarlarından temel alır ve biraz zaman, çeşitli farklı şekillerde yakalamak uygulama izin verir.

Kısacası, köşeli parantez içindeki yakalama veri bloğu resim resim ayarlarını çeşitli gerçekleştirilecek hala görüntülerinin olur.

## <a name="requirements"></a>Gereksinimler

Bu makalede sunulan adımları tamamlamak için aşağıdakiler gereklidir:

-  **Xcode 7 + ve iOS 8 veya daha yeni** – Apple'nın Xcode 7 ve iOS 8 veya daha yeni API'ler gereken yüklenmeli ve geliştirici bilgisayarda yapılandırılmış.
-  **Mac için Visual Studio** – Mac için Visual Studio en son sürümünü yüklenmeli ve kullanıcı cihazda yapılandırılmalıdır.
-  **iOS 8 aygıtı** – iOS 8 en son sürümünü çalıştıran bir iOS cihazı. Kamera özellikleri iOS Simulator'da sınanamıyor.


## <a name="general-av-capture-setup"></a>Genel AV yakalama Kurulumu

Bir iOS cihazında video kaydederken, her zaman gereklidir bazı genel kurulum kodu yok. Bu bölümde iOS cihazın kamerasının video kaydetmek ve bu videoda görüntülemek için gerekli olan en az Kurulum ele alınacaktır gerçek zamanlı olarak bir `UIImageView`.

### <a name="output-sample-buffer-delegate"></a>Çıkış örneği arabellek temsilci

İlk özelliklerinden biri olacak örnek çıkış arabelleği izlemek ve için arabellekteki gerçekleşti resim görüntülemek için bir temsilci bir `UIImageView` UI uygulamada.

Aşağıdaki yordam örnek arabellek izlemek ve kullanıcı arabirimini güncelleştirme:

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

Bu yordam, yerinde ile `AppDelegate` canlı bir video akışına kaydetmek için bir AV yakalama oturumu açmak için değiştirilebilir.

### <a name="creating-an-av-capture-session"></a>AV yakalama oturumu oluşturma

AV yakalama oturumunu iOS cihazın kameradan canlı video kaydı denetlemek için kullanılır ve bir iOS uygulamasına video almak için gereklidir. Örnek itibaren `ManualCameraControl` örnek uygulama birkaç farklı yerlerde yakalama oturumunu kullanarak, içinde yapılandırılacak `AppDelegate` ve tüm uygulama için kullanılabilir.

Uygulamanın değiştirmek için aşağıdakileri `AppDelegate` ve gerekli kodu ekleyin:


1. Çift `AppDelegate.cs` düzenlemek üzere açmak için Çözüm Gezgini'nde dosya.
1. Aşağıdaki using deyimlerini dosyanın en üstüne ekleyin:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. Aşağıdaki özel değişkenleri ekleyip hesaplanan özelliklerine `AppDelegate` sınıfı:
    
    ```
    #region Private Variables
    private NSError Error;
    #endregion
    
    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```
  
1. Tamamlanmış yöntemini geçersiz kılın ve şekilde değiştirin:
    
    ```
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;
    
        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }
    
        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }
    
        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);
    
        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();
    
        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }
    
        // Attach input to session
        Session.AddInput (Input);
    
        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;
    
        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);
    
        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```  
  
1. Değişiklikleri dosyaya kaydedin.


Yerinde bu kodu ile el ile kamera denetimleri kolayca deneme ve test için uygulanabilir.

## <a name="manual-focus"></a>El ile odak

Odağı denetimlerin doğrudan olabilmesi son kullanıcı izin vererek, bir uygulama üzerinde gerçekleştirilecek görüntü daha Artistik denetim sağlayabilir.

Örneğin, profesyonel fotoğrafı çeken bir görüntüsünü elde etmek için odak yumuşatma bir [Bokeh etkisi](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Bokeh etkisi")](intro-to-manual-camera-controls-images/image2.png#lightbox)

Ya da oluşturma bir [odak çekme etkisi](http://www.mediacollege.com/video/camera/focus/pull.html), gibi:

[![](intro-to-manual-camera-controls-images/image3.png "Odağı çekme etkisi")](intro-to-manual-camera-controls-images/image3.png#lightbox)

Bilimcilerine veya bir yazıcı tıbbi uygulamalar için uygulama Mercek denemeleri için programlı olarak hareket etmek isteyebilirsiniz. Son kullanıcı veya odak denetime zamanında resmin faydalanmak üzere uygulamayı gerçekleştirilecek hangisini yeni API sağlar.

### <a name="how-focus-works"></a>Odağı nasıl çalışır?

IOS 8 uygulamada odağını denetleme ayrıntılarını ele almadan önce. Bir iOS aygıtı odağı nasıl çalıştığı bir hızlı bakalım:

[![](intro-to-manual-camera-controls-images/image4.png "Bir iOS aygıtı odağı nasıl çalışır?")](intro-to-manual-camera-controls-images/image4.png#lightbox)

Açık kamera Mercek iOS cihazında girer ve bir görüntü algılayıcı üzerinde odaklanmıştır. Odak noktası (resmin en keskin nerede görüneceğini alanı), algılayıcı ilişkisinde olduğu algılayıcı Mercek uzaklığı denetler. Uzağına Mercek algılayıcı olduğunda, uzaklığı nesneleri en keskin gibi görünüyor ve nesneleri yakın daha yakın en keskin görünüyor.

Bir iOS aygıtı Mercek yakın için ya da dan, algılayıcı mıknatıs ve yaylar tarafından taşınır. Aygıt başka bir aygıt değişir ve cihaz yönünü veya cihaz ve yay yaşı gibi parametreleri etkilenen sonuç olarak, tam odağı konumlandırma imkansız aynıdır.

### <a name="important-focus-terms"></a>Önemli odak koşulları

Odak ile ilgilenirken, geliştirici konusunda bilgi sahibi olmanız birkaç koşulları vardır:

-  **Alan derinliği** – yakın ve sağdan odak nesneleri arasındaki uzaklığı. 
-  **Makro** -bu odak spektrumun yakın sonuna ve hangi Mercek odak en yakın olan uzaklık.
-  **Sonsuz** – bu odak spektrumun uzak sonuna ve hangi Mercek odak en uzak uzaklık.
-  **Hyperfocal uzaklığı** – çerçeve en uzak nesnesinde yalnızca uzak odak sonunda olduğu odak spektrumun noktasında budur. Diğer bir deyişle, alanın derinliği en üst düzeye çıkarır odak konumu budur. 
-  **Mercek konumu** – Yukarıdakilerin tümü kontrol olan diğer koşulları. Bu algılayıcı ve böylece Mercek uzaklığını odak denetleyicisidir.


Bu hüküm ve Bilgi Bankası aklınızda yeni el ile odak denetimleri başarıyla iOS 8 uygulamada uygulanabilir.

### <a name="existing-focus-controls"></a>Mevcut odak denetimleri

iOS 7 ve önceki sürümleri, sağlanan mevcut odak denetimleri aracılığıyla `FocusMode`özelliği olarak:

-   `AVCaptureFocusModeLocked` – Odak tek odak noktasına kilitlendi.
-   `AVCaptureFocusModeAutoFocus` – Sharp odak bulur ve orada kalacak kadar kamera tüm odak noktaları üzerinden Mercek taşır.
-   `AVCaptureFocusModeContinuousAutoFocus` – Odağı çıkış koşulu algıladığında kamera refocuses.


Mevcut denetimleri de ayarlanabilir noktası ilgi sağlanan`FocusPointOfInterest` özelliği, böylece kullanıcı belirli bir alandaki odaklanmak dokunabilirsiniz. Uygulama da Mercek taşıma izleyerek izleyebilirsiniz `IsAdjustingFocus` özelliği.

Ayrıca, aralık kısıtlama tarafından sağlanan `AutoFocusRangeRestriction` özelliği olarak:

-   `AVCaptureAutoFocusRangeRestrictionNear` – Autofocus yakındaki derinliklerine kısıtlar. Bir QR kodu veya barkod tarama gibi durumlarda yararlıdır.
-   `AVCaptureAutoFocusRangeRestrictionFar` – Autofocus uzak derinliklerine kısıtlar. Önemli olduğu bilinen nesneleri görünüm alanı içinde (örneğin, bir pencere çerçevesi) olduğu durumlarda faydalıdır.


Son olarak yoktur `SmoothAutoFocus` otomatik odak algoritması yavaşlatır ve video kaydederken yapıları taşınmasını engellemek için daha küçük artışlarla adımları özelliği.

### <a name="new-focus-controls-in-ios-8"></a>İOS 8 yeni odak denetimlerinde

İOS 7 ve üstünde zaten sağlanan özelliklerine ek olarak, aşağıdaki özellikleri artık iOS 8 odakta denetlemek kullanılabilir:

-  Tam el ile denetim odağı kilitlenirken Mercek konumu.
-  Anahtar-değer gözlem herhangi odak modunda Mercek konumu.


Yukarıdaki özellikleri uygulamak için `AVCaptureDevice` sınıfı, salt okunur içerecek şekilde değiştirilmiş `LensPosition` kamera Mercek geçerli konumunu almak için kullanılan özellik.

El ile denetim Mercek konumlarından yararlanmak için yakalama cihaz kilitli odak modunda olmalıdır. Örnek:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked` Yöntemi yakalama aygıtının kamera Mercek konumunu ayarlamak için kullanılır. Bir isteğe bağlı geri arama yordamı olmasını sağlayabilir değişikliği etkinleşir olduğunda bildirim alma. Örnek:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

Mercek konumda bir değişiklik yapılmadan önce yukarıdaki kodda görüldüğü gibi yakalama cihaz yapılandırması için kilitlenmiş olması gerekir. Geçerli Mercek konumu 0,0 ile 1,0 arasında değerlerdir.

### <a name="manual-focus-example"></a>El ile odak örneği

Genel AV yakalama Kurulum kodu, yerinde bir `UIViewController` için uygulamanın film şeridi eklendi ve aşağıdaki gibi yapılandırılmış:

[![](intro-to-manual-camera-controls-images/image5.png "Bir UIViewController uygulamaları film şeridi eklendi ve aşağıda gösterildiği gibi yapılandırılmış")](intro-to-manual-camera-controls-images/image5.png#lightbox)

Görünüm aşağıdaki ana öğeleri içerir:

-  A `UIImageView` video akışına görüntülenir.
-  A `UISegmentedControl` değiştirir odak modunda Otomatik'ten için kilitli.
-  A `UISlider` , Göster ve geçerli Mercek konumu güncelleştirin.


Görünüm Denetleyicisi'ni el ile odağı denetimi için kablo yukarı için aşağıdakileri yapın:


1. Aşağıdaki using deyimlerini:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Aşağıdaki özel değişkenleri ekleyin:

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```  
  
1. Aşağıdaki hesaplanan özellikleri ekleyin:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Geçersiz kılma `ViewDidLoad` yöntemi ve aşağıdaki kodu ekleyin:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Geçersiz kılma `ViewDidAppear` yöntemi ve görünüm yüklediğinde kaydı başlatmak için aşağıdakileri ekleyin:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Odağı kamera ayarlar gibi otomatik modda kamera ile kaydırıcı otomatik olarak Taşı:

    [![](intro-to-manual-camera-controls-images/image6.png "Bu örnek uygulama odakta kamera ayarlar gibi Kaydırıcı otomatik olarak geçer")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. Kilitli segment dokunun ve mercek konumu el ile ayarlamak için konum kaydırıcıyı sürükleyin:

    [![](intro-to-manual-camera-controls-images/image7.png "El ile Mercek konumu ayarlama")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. Uygulamayı durdurun.


Yukarıdaki kod kamera otomatik modunda olduğunda Mercek konum izlemenize veya kilitli modunda olduğunda Mercek konumunu denetlemek üzere kaydırıcıyı kullanmak nasıl göstermiştir.

## <a name="manual-exposure"></a>El ile Etkilenme

Etkilenme kaynak parlaklığını göre görüntünün parlaklığını başvuruyor ve uzun ve (ISO eşleme) algılayıcı kazanç düzeyine göre ne kadar açık tarafından nasıl algılayıcı isabetler belirlenir. Etkilenme üzerinde el ile denetim sağlayarak, uygulamanın son kullanıcıya özgürlüğü sağlar ve bunları stilize bir görünüm elde etmek izin verebilirsiniz.

El ile Etkilenme denetimlerini kullanarak, kullanıcı bir görüntüden unrealistically açık koyu ve moody alabilir:

[![](intro-to-manual-camera-controls-images/image8.png "Örnek unrealistically açık çıkarılmaktan koyu ve moody gösteren görüntü")](intro-to-manual-camera-controls-images/image8.png#lightbox)

Yeniden, bu programsal denetim Bilimsel uygulamalar için veya uygulama kullanıcı arabirimi tarafından sağlanan el ile denetimleri aracılığıyla kullanılarak otomatik olarak yapılabilir. Her iki durumda da, yeni iOS 8 Etkilenme API'leri kameranın Etkilenme ayarlar üzerinde hassas bir denetim sağlar.

### <a name="how-exposure-works"></a>Etkilenme nasıl çalışır?

IOS 8 uygulamada Etkilenme denetleme ayrıntılarını ele almadan önce. Etkilenme nasıl çalıştığı bir hızlı bakalım:

[![](intro-to-manual-camera-controls-images/image9.png "Etkilenme nasıl çalışır?")](intro-to-manual-camera-controls-images/image9.png#lightbox)

Etkilenme denetlemek için bir araya gelmesi üç temel öğeleri şunlardır:

-  **Perde hızı** – perde kamerayı sensörü üzerine açık izin vermek için açık olduğu süreyi budur. Daha kısa perde açıksa zaman daha az açık let ve canlı (daha az Hareket Bulanıklığı) görüntüsüdür. Uzun perde açık, daha fazla açık olarak ve daha oluşur Bulanıklaştırma hareket sağlar.
-  **ISO eşleme** – bu film fotoğraf ödünç bir terim ve açık için film içinde chemicals duyarlılığını başvuruyor. Film düşük ISO değerleri daha az çizgisi ve hassas renk çoğaltılması vardır; Sayısal algılayıcı düşük ISO değerlerine daha az parlaklığını ancak daha az algılayıcı gürültü vardır. ISO değer arttıkça, daha açık görüntü ancak daha fazla algılayıcı gürültü ile. Sayısal algılayıcı "ISO" ölçüsüdür [elektronik kazanç](http://en.wikipedia.org/wiki/Gain), fiziksel bir özellik değil. 
-  **Mercek açıklığı** – Mercek açılış boyutudur. Perde hızı ve ISO Etkilenme ayarlamak için kullanılan yalnızca iki değer olacak şekilde tüm iOS cihazlarda Mercek açıklığı, düzeltilmiştir.


### <a name="how-continuous-auto-exposure-works"></a>Nasıl sürekli otomatik Etkilenme çalışır

El ile Etkilenme nasıl çalışır önce öğrenme yarar fikir nasıl sürekli otomatik Etkilenme anlamak için bir iOS aygıtı çalışır.

[![](intro-to-manual-camera-controls-images/image10.png "Bir iOS aygıtı sürekli otomatik Etkilenme nasıl çalışır?")](intro-to-manual-camera-controls-images/image10.png#lightbox)

İlk otomatik Etkilenme bloğu ideal Etkilenme hesaplama işin ve sürekli olarak Ölçüm istatistikleri gönderilir. Bu bilgileri, ISO ve perde hızı iyi aydınlatma Sahne almak için en iyi bileşimi hesaplamak için kullanır. Bu döngü AE döngüsü olarak adlandırılır.

### <a name="how-locked-exposure-works"></a>Nasıl kilitli Etkilenme çalışır

Ardından, iOS cihazları nasıl kilitli Etkilenme çalışır inceleyelim.

[![](intro-to-manual-camera-controls-images/image11.png "Nasıl kilitli Etkilenme çalışır iOS cihazları")](intro-to-manual-camera-controls-images/image11.png#lightbox)

Yeniden, en iyi iOS ve Duration değerleri hesaplamak için çalışıyor otomatik Etkilenme blok sahip. Ancak, bu modda Ölçüm istatistikleri altyapısı AE blok kesilir.

### <a name="existing-exposure-controls"></a>Mevcut Etkilenme denetimleri

iOS 7 ve üzeri, aşağıdaki mevcut Etkilenme denetimleri aracılığıyla sağlamak `ExposureMode` özelliği:

-   `AVCaptureExposureModeLocked` – Sahne kez örnekleri ve Sahne boyunca bu değerler kullanır.
-   `AVCaptureExposureModeContinuousAutoExposure` – Sürekli olarak iyi aydınlatma emin olmak için Sahne örnekleri.


`ExposurePointOfInterest` Üzerinde kullanıma sunmak için bir hedef nesne seçerek Sahne kullanıma sunmak için dokunun için kullanılabilir ve uygulamayı izleyebilir `AdjustingExposure` zaman ayarlandığı görmek için özellik.

### <a name="new-exposure-controls-in-ios-8"></a>İOS 8 yeni Etkilenme denetimlerinde

İOS 7 ve üstünde zaten sağlanan özelliklerine ek olarak, aşağıdaki özellikleri artık iOS 8 Etkilenme denetlemek kullanılabilir:

-  Tam olarak el ile özel Etkilenme.
-  GET, kümesi ve anahtar-değer IOS ve perde hızı (süresi) inceleyin.


Yukarıdaki özellikleri uygulamak için yeni bir `AVCaptureExposureModeCustom` modu eklendi. Kameraya özel modda olduğunda, aşağıdaki kodu Etkilenme süresini ve ISO ayarlamak için kullanılabilir:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

Otomatik olarak ve kilitli modlarında uygulama aşağıdaki kodu kullanarak otomatik Etkilenme yordamı sapması ayarlayabilirsiniz:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

Bunlar hiçbir zaman sabit kodlanmış olmalıdır için minimum ve maksimum ayarı aralıkları uygulama üzerinde çalıştırıldığı cihaz bağlıdır. Bunun yerine, minimum ve maksimum değer aralıklarını alma için aşağıdaki özellikleri kullanın:

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


Etkilenme bir değişiklik yapılmadan önce yukarıdaki kodda görüldüğü gibi yakalama cihaz yapılandırması için kilitlenmiş olması gerekir.

### <a name="manual-exposure-example"></a>El ile Etkilenme örneği

Genel AV yakalama Kurulum kodu, yerinde bir `UIViewController` için uygulamanın film şeridi eklendi ve aşağıdaki gibi yapılandırılmış:

[![](intro-to-manual-camera-controls-images/image12.png "Bir UIViewController uygulamaları film şeridi eklendi ve aşağıda gösterildiği gibi yapılandırılmış")](intro-to-manual-camera-controls-images/image12.png#lightbox)

Görünüm aşağıdaki ana öğeleri içerir:

-  A `UIImageView` video akışına görüntülenir.
-  A `UISegmentedControl` değiştirir odak modunda Otomatik'ten için kilitli.
-  Dört `UISlider` gösteren ve uzaklık, süre, ISO ve sapması güncelleştirme denetimleri.


Görünüm Denetleyicisi'ni el ile Exposure Control kablo yukarı için aşağıdakileri yapın:


1. Aşağıdaki using deyimlerini:
    
    ```
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Aşağıdaki özel değişkenleri ekleyin:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```  
  
1. Aşağıdaki hesaplanan özellikleri ekleyin:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Geçersiz kılma `ViewDidLoad` yöntemi ve aşağıdaki kodu ekleyin:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;
    
        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;
    
        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;
    
        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });
    
            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });
    
            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });
    
            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };
    
        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {
    
            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
    
            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }
    
            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        ISO.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    
        Bias.ValueChanged += (object sender, EventArgs e) => {
    
            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;
    
            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```  
  
1. Geçersiz kılma `ViewDidAppear` yöntemi ve görünüm yüklediğinde kaydı başlatmak için aşağıdakileri ekleyin:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Kamera Etkilenme ayarlar gibi otomatik modda kamera ile kaydırıcılar otomatik olarak Taşı:

    [![](intro-to-manual-camera-controls-images/image13.png "Kamera Etkilenme ayarlar gibi kaydırıcılar otomatik olarak geçer")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. Kilitli segment dokunun ve otomatik Etkilenme sapması el ile ayarlamak için sapması kaydırıcıyı sürükleyin:

    [![](intro-to-manual-camera-controls-images/image14.png "Otomatik Etkilenme sapması el ile ayarlama")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. Özel segment dokunun ve el ile Etkilenme denetlemek için süre ve ISO kaydırıcılar sürükleyin:

    [![](intro-to-manual-camera-controls-images/image15.png "El ile denetim maruz kalma süresi ve ISO kaydırıcılar sürükleyin")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. Uygulamayı durdurun.


Yukarıdaki kod kamera otomatik modunda olduğunda Etkilenme ayarlarını izleme göstermiştir ve nasıl kilitli veya özel modda olduğunda Etkilenme denetlemek için kaydırma çubuklarını kullanın.

## <a name="manual-white-balance"></a>El ile beyaz dengesi

Beyaz Denge denetimleri, bunların daha gerçekçi görünmesini sağlamak için bir resim colosr bakiyesini ayarlamak kullanıcıların sağlar. Farklı renk etme farklı açık kaynakları sahip ve görüntüyü yakalamak için kullanılan kamera ayarları için bu farklılıklar dengelemek için ayarlanmalıdır. Yeniden beyaz dengesi kullanıcı denetime izin vererek bunlar Artistik efektler elde etmek için otomatik yordamları kuramadığı profesyonel ayarlamalar yapabilirsiniz.

[![](intro-to-manual-camera-controls-images/image16.png "El ile Beyaz Denge ayarları gösteren bir örnek görüntü")](intro-to-manual-camera-controls-images/image16.png#lightbox)

Örneği için daha sıcak, sarı turuncu TINT Tungsten göz alıcısın ışık sahip olurken gün ışığından yararlanma blueish cast ' var. (Confusingly, "soğuk" renkleri "sıcak" renkleri daha yüksek renk etme vardır. Renk etme Algısal bir tane fiziksel bir ölçü var.)

İnsan göz renk sıcaklığı farklılıkları Dengelemesi en çok iyi olduğu, ancak bu kamera yapamayacağı bir şey. Kamera rengi renk farkları ayarlamak için ters spektrumun artırmanın tarafından çalışır.

Yeni iOS 8 Etkilenme API işlemi denetimini ele geçirmek ve kameranın beyaz dengesi ayarlar üzerinde hassas bir denetim sağlamak için uygulamayı sağlar.

### <a name="how-white-balance-works"></a>Nasıl beyaz Bakiye çalışır

IOS 8 uygulamada beyaz dengesi denetleme ayrıntılarını ele almadan önce. Bir hızlı nasıl beyaz Bakiye works bakalım:

Renk algısına incelemesi içinde [CIE 1931 RGB renk alanı ve CIE 1931 XYZ renk alanını](http://en.wikipedia.org/wiki/CIE_1931_color_space) olan ilk renk alanları matematiksel olarak tanımlanmış. Uluslararası Komisyonu on aydınlatma (CIE) tarafından 1931 içinde oluşturuldukları.

[![](intro-to-manual-camera-controls-images/image17.png "Renk alanı CIE 1931 RGB renk alanını ve CIE 1931 XYZ")](intro-to-manual-camera-controls-images/image17.png#lightbox)

Yukarıdaki grafik bize tüm renkleri görülebilen İnsan göz için ayrıntılı açık yeşil açık bir kırmızı mavi gösterir. Herhangi bir noktasını diyagramdaki bir X ve Y değeri ile yukarıdaki grafikte gösterildiği gibi çizilen.

Grafikte görünür, İnsan görme aralığının dışında olacak grafikte çizilen X ve Y değerleri vardır ve bu renkleri kamera tarafından sonucunda yeniden oluşturulamaz.

Yukarıdaki grafikte küçük eğri adlı [Planckian Locus](http://en.wikipedia.org/wiki/Planckian_locus), renk sıcaklık (derece kelvin), (hotter) mavi taraftaki yüksek numaralarıyla ifade eder ve alt sayılar kırmızı tarafında (Soğutucu). Bu, tipik aydınlatma durumlarda faydalıdır.

Karma ışık koşullarında Beyaz Denge ayarları gerekli değişiklikleri yapmak için Planckian Locus sapma gerekecektir. Ayarlaması gerekir bu gibi durumlarda olması ya da yeşil gölgeye veya CIE tarafındaki kırmızı/macenta ölçeklendirin.

iOS cihazları için renk atamaları ters renk kazanç artırmanın tarafından dengelemek. Bir Sahne çok fazla mavi varsa, örneğin, ardından kırmızı kazanç dengelemek için boosted. Aygıta göre; bu nedenle, değerleri belirli aygıtlar için kalibre bu kazanç.

### <a name="existing-white-balance-controls"></a>Mevcut beyaz dengesi denetimleri

iOS 7 ve üzeri aşağıdaki mevcut Beyaz Denge denetimleri aracılığıyla sağlanan `WhiteBalanceMode` özelliği:

-   `AVCapture WhiteBalance ModeLocked` – Bir kez Sahne ve Sahne boyunca bu değerler kullanarak örnekleri.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – Sürekli olarak iyi dengelenmiş emin olmak için Sahne örnekleri.


Ve uygulama izleyebilirsiniz `AdjustingWhiteBalance` zaman ayarlandığı görmek için özellik.

### <a name="new-white-balance-controls-in-ios-8"></a>İOS 8 yeni beyaz Bakiye denetimlerinde

İOS 7 ve üstünde zaten sağlanan özelliklerine ek olarak, aşağıdaki özellikleri artık iOS 8 Beyaz Denge denetiminde kullanılabilir:

-  RGB aygıtının tam olarak el ile denetim kazanır.
-  GET, kümesi ve anahtar-değer gözlemlemek RGB aygıt kazanır.
-  Beyaz Gri kart kullanarak denge desteği.
-  Dönüştürme yordamları aygıt bağımsız renk alanları gelen ve giden.


Yukarıdaki özellikleri uygulamak için `AVCaptureWhiteBalanceGain` yapısı aşağıdaki üyeleriyle eklendi:

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


Şu anda en fazla beyaz dengesi kazanç olan dört (4) ve hazır'dan `MaxWhiteBalanceGain` özelliği. Yasal bir (1) için aralığı şekilde `MaxWhiteBalanceGain` (4) şu anda.

`DeviceWhiteBalanceGains` Özelliği, geçerli değerleri izlemek için kullanılabilir. Kullanım `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` Bakiye ayarlamak için kamera kilitli beyaz dengesi modunda olduğunda kazanır.

#### <a name="conversion-routines"></a>Dönüştürme yordamları

Dönüştürme yordamları için ve, dönüştürme ile yardımcı olmak için iOS 8 eklenmiştir aygıt bağımsız renk alanları. Dönüştürme yordamları uygulamak için `AVCaptureWhiteBalanceChromaticityValues` yapısı aşağıdaki üyeleriyle eklendi:

-   `X` -1 0 arasında bir değer.
-   `Y` -1 0 arasında bir değer.


Bir `AVCaptureWhiteBalanceTemperatureAndTintValues` yapısı ile aşağıdaki üyeleri de eklenmiştir:

-   `Temperature` -bir kayan nokta değeri derece Kelvin.
-   `Tint` -yeşil veya yeşil yöne doğru pozitif değerlere sahip 150 için 0'dan macenta ve negatif doğru macenta uzaklığı arasındadır.


Kullanım `CaptureDevice.GetTemperatureAndTintValues`ve `CaptureDevice.GetDeviceWhiteBalanceGains`sıcaklık ve TINT, renk ve RGB arasında dönüştürme yöntemleri elde renk alanları.

> [!NOTE]
> Dönüştürme yordamları daha doğru yakınsa dönüştürülecek Planckian Locus değerdir.




#### <a name="gray-card-support"></a>Gri kart desteği

Apple iOS 8 yerleşik gri kart desteği başvurmak için gri World terimini kullanır. Kullanıcının bir fiziksel gri çerçeve merkezi % 50'en az kapsayan ve beyaz denge ayarlamak için kullanan kartında odaklanmanıza olanak sağlar. Dilden bağımsız görünür beyaz elde etmek için gri kartı amacı budur.

Bir uygulamada bu uygulanabilir kamera önünde fiziksel bir gri kart yerleştirmek için kullanıcı isteyerek izleme `GrayWorldDeviceWhiteBalanceGains` özellik ve değerlerini kapatma tamamlanmasını bekleyin.

Uygulama sonra beyaz denge artışları için kilitleyebilirsiniz `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` değerleri kullanarak yöntemi `GrayWorldDeviceWhiteBalanceGains` değişiklikleri uygulamak için özellik.

Beyaz Denge bir değişiklik yapılmadan önce yakalama cihaz yapılandırması için kilitlenmiş olması gerekir.

### <a name="manual-white-balance-example"></a>El ile beyaz dengesi örneği

Genel AV yakalama Kurulum kodu, yerinde bir `UIViewController` için uygulamanın film şeridi eklendi ve aşağıdaki gibi yapılandırılmış:

[![](intro-to-manual-camera-controls-images/image18.png "Bir UIViewController uygulamaları film şeridi eklendi ve aşağıda gösterildiği gibi yapılandırılmış")](intro-to-manual-camera-controls-images/image18.png#lightbox)

Görünüm aşağıdaki ana öğeleri içerir:

-  A `UIImageView` video akışına görüntülenir.
-  A `UISegmentedControl` değiştirir odak modunda Otomatik'ten için kilitli.
-  İki `UISlider` gösteren ve sıcaklık ve TINT güncelleştirme denetimleri.
-  A `UIButton` gri kart (gri World) alanı örnek ve bu değerleri kullanarak Beyaz Denge ayarlamak için kullanılan.


Görünüm Denetleyicisi'ni el ile beyaz dengesi denetimini kablo yukarı için aşağıdakileri yapın:


1. Aşağıdaki using deyimlerini:

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```  
  
1. Aşağıdaki özel değişkenleri ekleyin:

    ```csharp
    #region Private Variables
    private NSError Error; 
    private bool Automatic = true;
    #endregion
    ```
  
1. Aşağıdaki hesaplanan özellikleri ekleyin:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```  
  
1. Yeni beyaz ayarlamak için aşağıdaki özel yöntemi ekleyin sıcaklık ve ton Bakiye:

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }
    
    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```   
  
1. Geçersiz kılma `ViewDidLoad` yöntemi ve aşağıdaki kodu ekleyin:

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ``` 
  
1. Geçersiz kılma `ViewDidAppear` yöntemi ve görünüm yüklediğinde kaydı başlatmak için aşağıdakileri ekleyin:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```  
  
1. Değişiklikleri kaydetmek kod ve uygulamayı çalıştırın.
1. Beyaz dengesi kamera ayarlar gibi otomatik modda kamera ile kaydırıcılar otomatik olarak Taşı:

    [![](intro-to-manual-camera-controls-images/image19.png "Beyaz dengesi kamera ayarlar gibi kaydırıcılar otomatik olarak geçer")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. Kilitli segment dokunun ve beyaz dengesi el ile ayarlamak için Temp ve ton kaydırıcıları sürükleyin:

    [![](intro-to-manual-camera-controls-images/image20.png "Beyaz dengesi el ile ayarlamak için Temp ve ton kaydırıcıları sürükleyin")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. Halen seçili kilitli segment ile fiziksel bir gri kartı önde kamera yerleştirin ve gri dünyaya beyaz dengesi ayarlamak için gri kartı düğmesine dokunun:

    [![](intro-to-manual-camera-controls-images/image21.png "Gri dünyaya beyaz dengesi ayarlamak için gri kartı düğmesine dokunun")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. Uygulamayı durdurun.

Yukarıdaki kod kamera otomatik modunda olduğunda beyaz dengesi ayarları izlemek veya kaydırıcıları kilitli modunda olduğunda beyaz dengesi denetlemek için kullandığı nasıl göstermiştir.

## <a name="bracketed-capture"></a>Köşeli parantez içindeki yakalama

Köşeli parantez içindeki yakalama el ile kamera yukarıda sunulan denetimleri ayarlarından temel alır ve biraz zaman, çeşitli farklı şekillerde yakalamak uygulama izin verir.

Kısacası, köşeli parantez içindeki yakalama veri bloğu resim resim ayarlarını çeşitli gerçekleştirilecek hala görüntülerinin olur.

[![](intro-to-manual-camera-controls-images/image22.png "Köşeli parantez içindeki yakalama nasıl çalışır")](intro-to-manual-camera-controls-images/image22.png#lightbox)

Köşeli parantez içindeki yakalama iOS 8 kullanarak, bir uygulama el ile kamera denetimleri bir dizi önceden, tek bir komut vermek ve bir dizi görüntü her el ile hazır için dönüş geçerli Sahne varsa olabilir.

### <a name="bracketed-capture-basics"></a>Köşeli parantez içindeki yakalama temelleri

Yeniden, köşeli parantez içindeki yakalama çok çeşitli resim ayarlarından resme ile gerçekleştirilecek hala görüntülerinin olur. Köşeli parantez içindeki yakalama kullanılabilir türleri şunlardır:

-  **Etkilenme köşeli ayraç otomatik** – tüm görüntüleri sahip olduğu çeşitli sapması tutar.
-  **El ile Etkilenme köşeli ayraç** – tüm görüntüleri bir değişken perde hızı (süresi) ve ISO sahip olduğu tutar.
-  **Basit veri bloğu ayraç** – hala hızlı art arda gerçekleştirilen görüntüleri bir dizi.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>İOS 8'de yeni köşeli parantez içindeki yakalama denetimleri

Tüm köşeli parantez içindeki yakalama komutları uygulanır `AVCaptureStillImageOutput` sınıfı. Kullanım `CaptureStillImageBracket`serisiyle ayarları verilen dizi görüntüleri almak için yöntemi.

İki yeni sınıflar, ayarları işlemek için uygulanmıştır:

-   `AVCaptureAutoExposureBracketedStillImageSettings` – Bunu bir özellik olan `ExposureTargetBias`, bir otomatik Etkilenme köşeli ayraç sapması ayarlamak için kullanılan. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` – Bu iki özelliğe sahip `ExposureDuration` ve `ISO`, ISO ve perde hızı için el ile Etkilenme köşeli ayraç ayarlamak için kullanılan. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>Köşeli parantez içindeki yakalama ilgilenmenin denetler

#### <a name="dos"></a>Yapılacak

İOS 8 denetimlerini köşeli parantez içindeki yakalama kullanma yükleyen yapılmalıdır bir listesi verilmiştir:

-  Uygulamayı çağırarak en kötü durum yakalama durum için hazırlayın `PrepareToCaptureStillImageBracket` yöntemi.
-  Örnek arabellekleri aynı paylaşılan havuzundan gelmesi olacak varsayalım.
-  Bir önceki hazırlama çağrısı tarafından ayrılmış bellek serbest bırakmak için arama `PrepareToCaptureStillImageBracket` yeniden ve bir nesne dizisi gönderin.


#### <a name="donts"></a>Yapılmaması gerekenler

İOS 8 denetimlerini köşeli parantez içindeki yakalama kullanma yükleyen yapılmalıdır olmayan bir listesi verilmiştir:

-  Ayarları türlerinde köşeli parantez içindeki yakalama tek bir yakalama bir arada kullanmayın.
-  İstek yok birden fazla `MaxBracketedCaptureStillImageCount` tek bir yakalama görüntüleri.


### <a name="bracketed-capture-details"></a>Köşeli parantez içindeki yakalama ayrıntıları

İOS 8 köşeli parantez içindeki yakalama ile çalışırken, aşağıdaki ayrıntıları dikkate gerçekleştirilmelidir:

-  Köşeli parantez içindeki ayarlarını geçici olarak devre dışı `AVCaptureDevice` ayarlar.
-  Flash ve hala görüntü sabitlemeyi ayarları göz ardı edilir.
-  Tüm görüntüleri aynı çıktı biçimi (jpeg, png, vb.) kullanmanız gerekir
-  Video Önizlemesi çerçeveler kaybolmasına neden olabilir.
-  Köşeli parantez içindeki yakalama iOS 8 ile uyumlu tüm cihazlarda desteklenir.


Aklınızda bu bilgi ile bir iOS 8 kullanma köşeli parantez içindeki yakalama örneği bakalım.

### <a name="bracket-capture-example"></a>Köşeli ayraç yakalama örneği

Genel AV yakalama Kurulum kodu, yerinde bir `UIViewController` için uygulamanın film şeridi eklendi ve aşağıdaki gibi yapılandırılmış:

[![](intro-to-manual-camera-controls-images/image23.png "Bir UIViewController uygulamaları film şeridi eklendi ve aşağıda gösterildiği gibi yapılandırılmış")](intro-to-manual-camera-controls-images/image23.png#lightbox)

Görünüm aşağıdaki ana öğeleri içerir:

-  A `UIImageView` video akışına görüntülenir.
-  Üç `UIImageViews` yakalama sonuçları görüntülenir.
-  A `UIScrollView` sonuç görünümleri ve video akışına barındırmak için.
-  A `UIButton` köşeli parantez içindeki yakalama bazı hazır ayarlara sahip olabilmesi için kullanılır.


Kablo görünüm denetleyicisini köşeli parantez içindeki yakalama için yukarı için aşağıdakileri yapın:


1. Aşağıdaki using deyimlerini:

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```  
  
1. Aşağıdaki özel değişkenleri ekleyin:

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```    
  
1. Aşağıdaki hesaplanan özellikleri ekleyin:

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```  
  
1. Gerekli çıktı görüntü görünümleri oluşturmak için aşağıdaki özel yöntemi ekleyin:

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {
    
        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));
    
        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");
    
        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);
    
        // Add to scrolling view
        ScrollView.AddSubview (imageView);
    
        // Return new image view
        return imageView;
    }
    #endregion
    ```  
  
1. Geçersiz kılma `ViewDidLoad` yöntemi ve aşağıdaki kodu ekleyin:
    
    ```
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
    
        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;
    
        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;
    
        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);
    
        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));
    
        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };
    
        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;
    
            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });
    
            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);
    
                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);
    
                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```
    
  
1. Geçersiz kılma `ViewDidAppear` yöntemi ve aşağıdaki kodu ekleyin:

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);
    
        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;
    
            ThisApp.Session.StartRunning ();
        }
    }
    
    ```  
    
1. Değişiklikleri kaydetmek kod ve uygulamayı çalıştırın.
1. Bir görünüm çerçeve ve yakalama köşeli ayraç düğmesine dokunun:

    [![](intro-to-manual-camera-controls-images/image24.png "Bir görünüm çerçeve ve yakalama köşeli ayraç düğmesine dokunun")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. Köşeli parantez içindeki yakalama tarafından gerçekleştirilen üç görüntüleri görmek için sağdan sola geçirme:

    [![](intro-to-manual-camera-controls-images/image25.png "Köşeli parantez içindeki yakalama tarafından gerçekleştirilen üç görüntüleri görmek için sağdan sola geçirme")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. Uygulamayı durdurun.


Yukarıdaki kod yapılandırmak ve iOS 8 bir otomatik Etkilenme köşeli parantez içindeki yakalama almak nasıl göstermiştir.

## <a name="summary"></a>Özet

Bu makalede biz iOS 8 tarafından sağlanan yeni el ile kamera denetimleri için giriş ele ve ne yaptıklarını ve nasıl çalıştıklarını temelleri kapsamına. Biz el ile odak, el ile Etkilenme ve el ile Beyaz Denge örnekleri verdiğiniz. Son olarak, size bir köşeli parantez içindeki daha önce ele alındığı el ile kamera denetimleri kullanarak yakalama alma örnek vermiş

## <a name="related-links"></a>İlgili bağlantılar

- [ManualCameraControls (örnek)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [iOS 8’e Giriş](~/ios/platform/introduction-to-ios8.md)
