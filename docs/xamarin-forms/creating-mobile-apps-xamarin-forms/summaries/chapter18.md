---
title: "Bölüm 18 özeti. MVVM"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: fa62ff952a4a8916a0c9603157d14948119d243d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-18-mvvm"></a>Bölüm 18 özeti. MVVM

Bir uygulama mimari için en iyi yöntemleri biri adlandırılır temel koddan kullanıcı arabirimi ayırarak *iş mantığı*. Birçok tekniği vardır, ancak XAML tabanlı ortamları için tasarlanmış bir Model-View-ViewModel veya MVVM olarak bilinir.

## <a name="mvvm-interrelationships"></a>MVVM kalmalarına

MVVM uygulamanın üç katmanı vardır:

- Web erişen veya Model temel alınan veri bazen dosyalar ile sağlar.
- XAML'de genellikle uygulanan kullanıcı arabirimi veya sunu katmanı, görünümdür
- Model ve görünüm ViewModel bağlanır

Model ViewModel kullanmayan ve ViewModel görünümünü kullanmayan. Bu üç katmanı genellikle aşağıdaki düzenekleri kullanarak birbirine:

![Görünümü, ViewModel ve görünümü](images/ch18fg03.png "MVVM")

Çok sayıda küçük program (ve daha da büyük olanlar), genellikle Model yok ya da işlevselliğini ViewModel tümleştirilmiştir.

## <a name="viewmodels-and-data-binding"></a>ViewModels ve veri bağlama

Veri bağlama devreye için bir ViewModel görünümü bildiren ViewModel bir özelliği değiştirildiğinde yeteneğinin olması gerekir. ViewModel uygulayarak bunu yapar [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) arabiriminde `System.ComponentModel` ad alanı. Bu Xamarin.Forms yerine .NET parçasıdır. (Genellikle ViewModels denemesi platform bağımsızlığı korumak.)

`INotifyPropertyChanged` Arabirimi bildirir adlı tek bir olay [ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/) değişti özelliğini gösterir.

### <a name="a-viewmodel-clock"></a>ViewModel saati

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) İçinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) kitaplığı tanımlar türünde bir özellik `DateTime` dayalı bir süreölçer değişir. Sınıf uygular `INotifyPropertyChanged` ve harekete `PropertyChanged` olay her `DateTime` özellik değişikliği.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) örnek bu ViewModel oluşturur ve veri bağlamaları ViewModel için güncelleştirilmiş tarih ve saat bilgilerini görüntülemek için kullanır.

### <a name="interactive-properties-in-a-viewmodel"></a>Bir ViewModel etkileşimli özellikleri

Tarafından gösterildiği gibi bir ViewModel özelliklerinde daha etkileşimli olabilir [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) parçasıdır sınıfı, [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) örnek. Veri bağlama iki multiplicand ve çarpanı değerlerini belirtin `Slider` öğeleri ve ürünle birlikte görüntüleme bir `Label`. Ancak, bu kullanıcı arabirimiyle XAML'de ViewModel veya arka plan kod dosyasına bilgilerde değişiklik için kapsamlı değişiklikler yapabilirsiniz.

### <a name="a-color-viewmodel"></a>Bir renk ViewModel

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) İçinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) kitaplığı RGB ve HSL renk modelleri tümleştirir. Örneklerde gösterildiği [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) örnek:

[![Üçlü ekran görüntüsü j](images/ch18fg08-small.png "HSL renk modeli")](images/ch18fg08-large.png "HSL renk modeli")

### <a name="streamlining-the-viewmodel"></a>ViewModel hızlandırma

ViewModels kodda tanımlayarak basitleştirilmesi bir `OnPropertyChanged` yöntemini kullanarak [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/) özniteliği arama özellik adı otomatik olarak alır. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) kitaplığı bunu yapar ve ViewModels için bir temel sınıf sağlar.

## <a name="the-command-interface"></a>Komut arabirimi

MVVM ile veri bağlamaları çalışır ve MVVM işleme için geldiğinde deficient görünüyor şekilde veri bağlamaları iş özellikleri ile bir `Clicked` olayı bir `Button` veya `Tapped` olayı bir `TapGestureRecognizer`. Bu tür olayları işlemek ViewModels izin vermek için Xamarin.Forms destekleyen *komut arabirimi*.

Komut arabirimi çıkmaktadır `Button` iki ortak özelliklere sahip:

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) tür [ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (tanımlanan `System.Windows.Input` ad alanı)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) türü `Object`

Komut arabirimi desteklemek için bir ViewModel türünde bir özellik tanımlamalısınız `ICommand` bağlı diğer bir deyişle sonra veri `Command` özelliği `Button`. `ICommand` Arabirimi bildirir iki yöntem ve bir olay:

- Bir [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/) türünde bir bağımsız değişken ile yöntemi `object`
- A [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/) türünde bir bağımsız değişken yöntemiyle `object` getirir `bool`
- A [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/) olayı

Dahili olarak, her bir özellik türü bir ViewModel ayarlar `ICommand` uygulayan bir sınıf örneği `ICommand` arabirimi. Veri bağlama aracılığıyla `Button` başlangıçta çağırır `CanExecute` yöntemi ve yöntem döndürüyorsa kendini devre dışı bırakır `false`. Ayrıca bir işleyici ayarlar `CanExecuteChanged` olay ve çağrıları `CanExecute` her bu olay tetiklenir. Varsa `Button` olan etkinse, çağıran `Execute` yöntemi her `Button` tıklandığında.

Xamarin.Forms geçerler bazı ViewModels olabilir ve bunlar zaten komut arabirimi desteklemiyor olabilir. Yeni ViewModels yalnızca Xamarin.Forms ile kullanılması için Xamarin.Forms sağlayan bir [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) sınıfı ve bir [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) uygulayan sınıf `ICommand` arabirimi. Genel tür bağımsız değişkeni türüdür `Execute` ve `CanExecute` yöntemleri.

### <a name="simple-method-executions"></a>Basit yöntem yürütmeleri

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) örnek bir ViewModel komutu arabiriminde kullanmayı gösterir. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Sınıfı tanımlayan iki türünün özelliklerini `ICommand` ve ayrıca en basit geçirir iki özel özelliklerini tanımlar [ `Command` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/). Bu ViewModel veri bağlamaları programı içeren `Command` iki özelliklerini `Button` öğeleri.

`Button` Öğeleri kolayca değiştirilebilir ile `TapGestureRecognizer` kod değişikliğine XAML'de nesneleri.

### <a name="a-calculator-almost"></a>Bir hesap makinesi neredeyse

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) örnek yaptığı her ikisini de kullanmanız `Execute` ve `CanExecute` yöntemlerini `ICommand`. Kullandığı bir [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) kitaplığı. ViewModel türünün altı özelliklerini içeren `ICommand`. Bu gelen başlatılır [ `Command` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/) ve [ `Command` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/) , `Command` ve [ `Command<T>` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) `Command<T>`. Sayı tuşları ekleme makinenin tüm ile başlatılmış özelliğine bağlı `Command<T>`ve bir `string` bağımsız değişkeni `Execute` ve `CanExecute` belirli anahtar tanımlar.

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels ve uygulama yaşam döngüsü

`AdderViewModel` Kullanılan **AddingMachine** örnek adlı iki yöntem de tanımlar `SaveState` ve `RestoreState`. Bu yöntemler, uyku moduna geçtiğinde ve yeniden başladığında uygulamadan adı verilir.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 18 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Bölüm 18 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
