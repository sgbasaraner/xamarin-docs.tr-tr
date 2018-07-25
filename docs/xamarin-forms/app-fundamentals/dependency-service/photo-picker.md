---
title: Resim Kitaplığı'ndan bir fotoğraf çekme
description: Bu makalede, bir fotoğraf telefonun Resim Kitaplığı'ndan seçmek için Xamarin.Forms DependencyService sınıfı kullanmayı açıklar.
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: dafa60ff57f34bd4169af48e380079d9637d8d26
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241113"
---
# <a name="picking-a-photo-from-the-picture-library"></a>Resim Kitaplığı'ndan bir fotoğraf çekme

Bu makale, kullanıcının telefonun Resim Kitaplığı'ndan bir fotoğraf sağlayan bir uygulama oluşturulmasını rehberlik yapacaktır. Xamarin.Forms bu işlevselliği içermediğinden kullanmak için gerekli [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) her platformda yerel API'lere erişmek için.  Bu makalede kullanmak için aşağıdaki adımları kapsar `DependencyService` bu görev için:

- **[Arabirimi oluşturma](#Creating_the_Interface)**  &ndash; arabirimi paylaşılan kodda nasıl oluşturulduğunu anlamanız.
- **[iOS uygulaması](#iOS_Implementation)**  &ndash; iOS için yerel kod içinde arabirim uygulamak hakkında bilgi edinin.
- **[Android uygulaması](#Android_Implementation)**  &ndash; arabirimi, Android için yerel koda uygulanması hakkında bilgi edinin.
- **[Evrensel Windows platformu uygulaması](#UWP_Implementation)**  &ndash; arabirimi, Evrensel Windows Platformu (UWP) için yerel koda uygulanması hakkında bilgi edinin.
- **[Paylaşılan kod içinde uygulama](#Implementing_in_Shared_Code)**  &ndash; nasıl kullanacağınızı öğrenin `DependencyService` paylaşılan kod yerel uygulamasından çağırmak için.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Arabirimi oluşturma

İlk olarak bir arabirim istenen işlevselliği ifade paylaşılan kod içinde oluşturun. Fotoğraf çekme uygulama söz konusu olduğunda, yalnızca bir yöntem gereklidir. İçinde tanımlanmıştır [ `IPicturePicker` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) arabirimi örnek kod, .NET Standard Kitaplığı'nda:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

`GetImageStreamAsync` Yöntemin tanımlanmış olarak zaman uyumsuz yöntemin çabuk dönmesini gerekir, ancak iade edemezsiniz bir `Stream` seçili fotoğrafın kadar kullanıcı resim kitaplığı taranan ve seçili tek bir nesne.

Bu arabirim, platforma özgü kod kullanarak tüm platformlarda uygulanır.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS uygulaması

İOS uygulaması `IPicturePicker` arabirim kullanımları [ `UIImagePickerController` ](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) açıklandığı [ **Galeriden bir fotoğraf seçin** ](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) tarif ve [örnek kod](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

İOS uygulaması bulunan [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) iOS projesi bir sınıfta örnek kod. Bu sınıf için görünür hale getirmek için `DependencyService` manager, sınıf gerekir ile tanımlanan bir [`assembly`] öznitelik türü `Dependency`, ve sınıfı genel olmalıdır ve açıkça uygulama `IPicturePicker` arabirimi:

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

`GetImageStreamAsync` Yöntemi oluşturur bir `UIImagePickerController` ve Fotoğraf Kitaplığı'ndan görüntülerini seçmek için başlatır. İki olay işleyicileri gereklidir: bir kullanıcı bir fotoğraf ve diğer kullanıcı Fotoğraf Kitaplığı görüntüsünü iptal ettiğinde için seçtiğinde. `PresentModalViewController` Sonra kullanıcıya Fotoğraf Kitaplığı görüntüler.

Bu noktada, `GetImageStreamAsync` yöntemi döndürmelidir bir `Task<Stream>` onu çağıran kod nesnesi. Bu görevi yalnızca kullanıcı fotoğraf kitaplığı ile etkileşim sona erdi ve biri olay işleyicileri, tamamlanır. Bu gibi durumlarda [ `TaskCompletionSource` ](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) sınıfı önemlidir. Sağlar sınıfını bir `Task` döndürüleceğini uygun genel türde nesne `GetImageStreamAsync` metot ve sınıf daha sonra sinyal görev tamamlandığında.

`FinishedPickingMedia` Olay işleyicisi, kullanıcı bir resim seçtiğinde çağrılır. Ancak, işleyici sağlar bir `UIImage` nesne ve `Task` .NET döndürmelidir `Stream` nesne. Bu iki adımda gerçekleştirilir: `UIImage` nesneyi bellek içinde depolanan bir JPEG dosyası için ilk dönüştürülür bir `NSData` nesnesi ve ardından `NSData` nesne için bir .NET dönüştürülür `Stream` nesne. Bir çağrı `SetResult` yöntemi `TaskCompletionSource` nesne sağlayarak görev tamamlandığında `Stream` nesnesi:

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

Bir iOS uygulamasına kullanıcıdan telefonun fotoğraf kitaplığınıza erişmesine izin gerektirir. Ekleyin `dict` Info.plist bölümünü:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android uygulaması

Android uygulaması açıklanan tekniği kullanan [ **bir görüntü seçin** ](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) tarif ve [örnek kod](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). Ancak, kullanıcı bir resim Resim Kitaplığı'ndan seçtiğinde çağrılan yöntem olan bir `OnActivityResult` öğesinden türetilen bir sınıfta geçersiz kılma `Activity`. Bu nedenle, normal [ `MainActivity` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) Android projesi sınıfında takıma bir alan, özellik ve geçersiz kılma `OnActivityResult` yöntemi:

```csharp
public class MainActivity : FormsAppCompatActivity
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

`OnActivityResult`Geçersiz kılma gösteren bir Android ile seçilen resim dosyası `Uri` nesne, ancak bir .NET Framework'e dönüştürülmesi bu can `Stream` çağırarak `OpenInputStream` yöntemi `ContentResolver` elde edildiği nesnesi Etkinliğin `ContentResolver` özelliği.

Android uygulaması iOS uygulaması gibi kullanan bir `TaskCompletionSource` görev tamamlandığında sinyal. Bu `TaskCompletionSource` ortak bir özellik olarak tanımlanan nesne `MainActivity` sınıfı. Bu özellik, başvurulabilmesi tanır [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) Android projesi sınıfta. Bu ile sınıftır `GetImageStreamAsync` yöntemi:

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

Bu yöntem erişir `MainActivity` çeşitli amaçlar için sınıf: için `Instance` özelliği için `PickImageId` için alan `TaskCompletionSource` özelliği ve çağrılacak `StartActivityForResult`. Bu yöntem tarafından tanımlanan `FormsAppCompatActivity` temel sınıf olan sınıf, `MainActivity`.

<a name="UWP_Implementation" />

## <a name="uwp-implementation"></a>UWP uygulaması

İOS ve Android uygulamalarında farklı olarak, Evrensel Windows platformu fotoğraf Seçici uygulamasını gerektirmez `TaskCompletionSource` sınıfı. [ `PicturePickerImplementation` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) Sınıfının kullandığı [ `FileOpenPicker` ](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) fotoğraf kitaplığına erişim elde etmek için sınıf. Çünkü `PickSingleFileAsync` yöntemi `FileOpenPicker` kendisi, zaman uyumsuzdur `GetImageStreamAsync` yöntemi yalnızca kullanabilir `await` bu yöntem (ve diğer zaman uyumsuz yöntemler) ve dönüş bir `Stream` nesnesi:


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

Her platform için arabirim uygulanmıştır, .NET Standard Kitaplığı'nda uygulama bunu avantajlarından yararlanabilirsiniz.

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

`Clicked` İşleyicisi kullanır `DependencyService` çağırmak için sınıf `GetImageStreamAsync`. Bu platform projesinde bir çağrı sonuçlanır. Yöntem döndürüyorsa bir `Stream` işleyicisi oluşturur. ardından, nesne bir `Image` öğesi ile bu resmin bir `TabGestureRecognizer`ve değiştirir `StackLayout` sayfasında, ile `Image`:

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

- [Fotoğraf galerisinden (iOS) seçin.](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [Bir görüntü (Android) seçin](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)
- [DependencyService (örnek)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
