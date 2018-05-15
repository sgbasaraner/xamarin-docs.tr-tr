---
title: Resim Kitaplığı'ndan bir fotoğraf çekme
description: DependencyService telefonunuzun Resim Kitaplığı'ndan bir fotoğraf seçmek için kullanın
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: e552a0bf76572d50eb0d4618af69fc1179979f97
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="picking-a-photo-from-the-picture-library"></a>Resim Kitaplığı'ndan bir fotoğraf çekme

Bu makalede kullanıcıya, telefonunuzun Resim Kitaplığı'ndan bir fotoğraf çekme sağlayan bir uygulama oluşturulmasını adım adım anlatılmaktadır. Xamarin.Forms bu işlevselliği içermediğinden kullanmak gerekli olan [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) her platformda yerel API'leri erişmek için.  Bu makalede kullanmak için aşağıdaki adımları kapsar `DependencyService` bu görev için:

- **[Arabirimi oluşturma](#Creating_the_Interface)**  &ndash; arabirimi paylaşılan kodda nasıl oluşturulduğunu anlayın.
- **[iOS uygulaması](#iOS_Implementation)**  &ndash; iOS için yerel kodda arabirimini uygulayan öğrenin.
- **[Android uygulaması](#Android_Implementation)**  &ndash; arabirimini yerel kodda Android için uygulama öğrenin.
- **[Evrensel Windows Platform uygulaması](#UWP_Implementation)**  &ndash; arabirimini yerel kodda Evrensel Windows Platformu (UWP) uygulaması öğrenin.
- **[Paylaşılan kod içinde uygulama](#Implementing_in_Shared_Code)**  &ndash; nasıl kullanacağınızı öğrenin `DependencyService` paylaşılan koddan yerel uygulama çağırmak için.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

İlk olarak, bir arabirim istenen işlevselliği ifade paylaşılan kod içinde oluşturun. Fotoğraf çekme uygulama söz konusu olduğunda, yalnızca bir yöntem gereklidir. Bu tanımlanan [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) örnek kod, .NET standart kitaplığı arabiriminde:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` Yöntemi hızla döndürmesi gerekir, ancak geri dönemezsiniz yöntemi zaman uyumsuz olarak tanımlanır bir `Stream` kullanıcı resim kitaplığı göz atıp bir seçili kadar Seçili fotoğraf için nesnesi.

Bu arabirim, platforma özgü kodu kullanarak tüm platformlarda uygulanır.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS uygulaması

İOS uygulaması `IPicturePicker` arabirim kullanımları [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) açıklandığı gibi [ **Galeriden bir fotoğraf seçin** ](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) tarif ve [örnek koduna](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

İOS uygulaması bulunan [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) iOS projesi sınıfında örnek kod. Bu sınıf için görünür hale getirmek için `DependencyService` Yöneticisi, sınıf gereken ile tanımlanan bir [`assembly`] öznitelik türü `Dependency`, ve sınıfı ortak ve açıkça uygulama `IPicturePicker` arabirimi:

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

`GetImageStreamAsync` Yöntemi oluşturur bir `UIImagePickerController` ve Fotoğraf Kitaplığı'ndan görüntüleri seçmek için başlatır. İki olay işleyicileri gereklidir: kullanıcı bir fotoğraf ve ne zaman kullanıcı fotoğrafı kitaplığı görüntüsünü iptal diğer seçtiğinde için bir tane. `PresentModalViewController` Sonra Fotoğraf Kitaplığı kullanıcıya görüntüler.

Bu noktada, `GetImageStreamAsync` yöntemi döndürmelidir bir `Task<Stream>` çağırma kodu nesnesi. Bu görevi yalnızca kullanıcı fotoğraf kitaplığı ile etkileşim sona erdi ve olay işleyicileri biri olarak adlandırılır, tamamlanır. Bunun gibi durumlar için [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) sınıfı önemlidir. Sınıf sağladığı bir `Task` döndürmek için uygun genel türde nesne `GetImageStreamAsync` yöntemi ve sınıfı daha sonra işaret görev tamamlandığında.

`FinishedPickingMedia` Olay işleyicisi, kullanıcının bir resim seçildiğinde çağrılır. Ancak, işleyici sağlar bir `UIImage` nesne ve `Task` bir .NET döndürmelidir `Stream` nesnesi. Bu iki adımda gerçekleştirilir: `UIImage` nesne bir JPEG dosyasına depolanmış bellekte ilk dönüştürülür bir `NSData` nesnesi ve ardından `NSData` nesne için bir .NET dönüştürülür `Stream` nesnesi. Çağrı `SetResult` yöntemi `TaskComkpletionSource` nesne sağlayarak görev tamamlandıktan `Stream` nesnesi:

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data = image.AsJPEG(1);
                Stream stream = data.AsStream();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}

```

Bir iOS uygulaması telefonun Fotoğraf Kitaplığı erişim izni kullanıcıdan gerektirir. Aşağıdakileri ekleyin `dict` Info.plist dosyasının bölümü:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android uygulaması

Android uygulaması açıklanan teknikleri kullanan [ **bir görüntü seçin** ](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) tarif ve [örnek koduna](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Ancak, kullanıcı Resim Kitaplığı'ndan bir görüntü seçtiğinde çağrılan yöntem budur bir `OnActivityResult` türetilen bir sınıfta geçersiz kılma `Activity`. Bu nedenle, normal [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Android projesi sınıfında takıma bir alan, bir özellik ve geçersiz kılma `OnActivityResult` yöntemi:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
{
    ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}

```

`OnActivityResult`Geçersiz kılma gösteren bir Android seçilen resim dosyasıyla `Uri` nesnesi, ancak bir .NET dönüştürülüp olabilir `Stream` çağırarak nesne `OpenInputStream` yöntemi `ContentResolver` uygulamasından edinilen nesnesi Etkinliğin `ContentResolver` özelliği.

İOS uygulaması gibi Android uygulaması kullanan bir `TaskCompletionSource` görev tamamlandığında sinyal. Bu `TaskCompletionSource` nesne ortak özelliği olarak tanımlanır `MainActivity` sınıfı. Bu, başvurulacak özelliği sağlar [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Android projesi sınıfta. Bu sınıfı olan `GetImageStreamAsync` yöntemi:

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

Bu yöntem erişir `MainActivity` çeşitli amaçlarla sınıfı: için `Instance` özelliği için `PickImageId` için alan `TaskCompletionSource` özelliği ve çağırmak için `StartActivityForResult`. Bu yöntem tarafından tanımlanan `FormsApplicationActivity` sınıf temel sınıfını `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP uygulaması

İOS ve Android uygulamaları aksine, fotoğraf Seçici uygulama Evrensel Windows platformu için gerektirmez `TaskCompletionSource` sınıfı. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) Sınıfını kullanan [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) Fotoğraf Kitaplığı erişmek için sınıf. Çünkü `PickSingleFileAsync` yöntemi `FileOpenPicker` kendisi, uyumsuzdur `GetImageStreamAsync` yöntemi yalnızca kullanabilir `await` o yöntemi (ve diğer zaman uyumsuz yöntemleri) ve dönüş bir `Stream` nesnesi:


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Paylaşılan kod içinde uygulama

Her platform için arabirimi uygulanmıştır, .NET standart kitaplığı uygulamada bunu yararlanabilir.

[ `App` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) Sınıfı oluşturur bir `Button` fotoğraf seçmek için:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

`Clicked` İşleyici kullanan `DependencyService` çağırmak için sınıf `GetImageStreamAsync`. Bu platform projesindeki çağrıda sonuçlanır. Yöntem döndürüyorsa bir `Stream` nesne işleyicisi oluşturur sonra bir `Image` öğesi bu resimle için bir `TabGestureRecognizer`ve değiştirir `StackLayout` alanıyla sayfasında `Image`:

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

Dokunarak `Image` öğesi sayfa normal olarak döndürür.


## <a name="related-links"></a>İlgili bağlantılar

- [Fotoğraf Galerisi (iOS) seçin](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [Bir görüntü (Android) seçin](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
