---
title: Bölüm 18 özeti. MVVM
description: 'Xamarin.Forms ile mobil uygulamalar oluşturma: Bölüm 18 özeti. MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b512b15ea40d963cd35379cf3162856d132e38dc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995268"
---
# <a name="summary-of-chapter-18-mvvm"></a>Bölüm 18 özeti. MVVM

Bir uygulama oluşturmak için en iyi yollarından biridir bazen adlı temel alınan koddan kullanıcı arabirimi ayrılarak *iş mantığı*. Çeşitli teknikler vardır, ancak XAML tabanlı ortamlar için uyarlanmış bir Model-View-ViewModel veya MVVM olarak bilinir.

## <a name="mvvm-interrelationships"></a>MVVM kalmalarına

MVVM uygulamanın üç katmanı vardır:

- Web'e veya modeli temel alınan verileri bazen aracılığıyla dosyaları sağlar.
- XAML içinde genel olarak uygulanan kullanıcı arabirimi veya sunu katmanı, görünümdür
- Model ve görünümü ViewModel bağlanır

Model ViewModel ignorant ve ViewModel görünümünü ignorant. Bu üç katmanı genel olarak aşağıdaki düzenekleri kullanarak birbirine:

![Görünümü ViewModel ve Görünüm](images/ch18fg03.png "MVVM")

Model çok daha küçük programlar (ve daha da büyük olanlar), genellikle eksik veya işlevselliğini ViewModel tümleştirilmiştir.

## <a name="viewmodels-and-data-binding"></a>Viewmodel'lar ve veri bağlama

Veri bağlamalara etkileşim kurmak amacıyla bir ViewModel görünümü ViewModel özelliği değiştiğinde bildiren uyumlu olması gerekir. ViewModel uygulayarak yapar [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) arabiriminde `System.ComponentModel` ad alanı. Bu, Xamarin.Forms yerine .NET bölümüdür. (Genellikle Viewmodel'lar platform bağımsızlığı korumak dener.)

`INotifyPropertyChanged` Arabirimi adlı tek bir olay bildirir [ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged) değişti özelliğini gösterir.

### <a name="a-viewmodel-clock"></a>ViewModel saati

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) İçinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) kitaplığı tanımlar vlastnost typu `DateTime` değişiklikleri bir zamanlayıcıyı temel alan. Sınıfının Implements `INotifyPropertyChanged` ve harekete `PropertyChanged` olay olduğunda `DateTime` özellik değişiklikleri.

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) örnek bu ViewModel başlatır ve ViewModel veri bağlamaları güncelleştirilmiş tarih ve saat bilgilerini görüntülemek için kullanır.

### <a name="interactive-properties-in-a-viewmodel"></a>Etkileşimli bir ViewModel özellikleri

Tarafından gösterildiği gibi bir ViewModel özelliklerinde daha etkileşimli olabilir [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) parçası olan bir sınıf, [ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) örnek. Veri bağlamaları ikisinden çarpan ve çarpan değerleri sağlayın `Slider` öğeleri ve ürünle birlikte görünen bir `Label`. Ancak, bu XAML kullanıcı arabirimine ViewModel veya arka plan kod dosyası izleyen bir değişiklik yapmadan kapsamlı değişiklikler yapabilirsiniz.

### <a name="a-color-viewmodel"></a>Bir renk ViewModel

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) İçinde [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) kitaplığı RGB ve HSL renk modelleri tümleştirir. Örnekte gösterildiği [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) örnek:

[![Üç ekran görüntüsü j](images/ch18fg08-small.png "HSL renk modeli")](images/ch18fg08-large.png#lightbox "HSL renk modeli")

### <a name="streamlining-the-viewmodel"></a>ViewModel hızlandırma

Viewmodel'lar kodda tanımlayarak basitleştirilmesi bir `OnPropertyChanged` yöntemi kullanarak [ `CallerMemberName` ](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute) özniteliği arama özellik adı otomatik olarak alır. [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) Sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) kitaplık bunu yapar ve Viewmodel'lar için bir temel sınıf sağlar.

## <a name="the-command-interface"></a>Komut arabirimi

MVVM veri bağlamaları ile çalışır ve veri bağlamalarını çalışma özellikleri ile MVVM işlemede geldiğinde deficient olması gibi görünüyor. Bu nedenle bir `Clicked` olayı bir `Button` veya `Tapped` olayı bir `TapGestureRecognizer`. Bu tür olayları işlemek Viewmodel'lar izin vermek için Xamarin.Forms destekler *komut arabirimi*.

Komut arabirimi çıkmaktadır `Button` iki ortak özelliklere sahip:

- [`Command`](xref:Xamarin.Forms.Button.Command) tür [ `ICommand` ](xref:System.Windows.Input.ICommand) (tanımlanan `System.Windows.Input` ad alanı)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) türü `Object`

Komut arabirimi desteklemek için bir ViewModel vlastnost typu tanımlamalıdır `ICommand` bağlı diğer bir deyişle sonra veri `Command` özelliği `Button`. `ICommand` Arabirimi iki yöntem ve bir olay bildirir:

- Bir [ `Execute` ](xref:System.Windows.Input.ICommand.Execute(System.Object)) türünde bir bağımsız değişken ile yöntemi `object`
- A [ `CanExecute` ](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) türünde bir bağımsız değişken yöntemiyle `object` döndürür `bool`
- A [ `CanExecuteChanged` ](xref:System.Windows.Input.ICommand.CanExecuteChanged) olay

Dahili olarak, her bir özellik türü bir ViewModel ayarlar `ICommand` uygulayan bir sınıf örneğine `ICommand` arabirimi. Veri bağlama aracılığıyla `Button` başlangıçta çağırdığı `CanExecute` yöntemi ve yöntem döndürüyorsa kendini devre dışı bırakır `false`. Ayrıca bir işleyici için ayarlar `CanExecuteChanged` olay ve çağrıları `CanExecute` olduğunda bu olay tetiklenir. Varsa `Button` olan etkin çağrı `Execute` yöntemi her `Button` tıklandığında.

Xamarin.Forms geçerler bazı Viewmodel'lar olabilir ve bunlar zaten komut arabirimi desteklemiyor olabilir. Yalnızca Xamarin.Forms ile kullanılacak yeni Viewmodel'lar amaçlar için Xamarin.Forms sağlayan bir [ `Command` ](xref:Xamarin.Forms.Command) sınıfı ve [ `Command<T>` ](xref:Xamarin.Forms.Command`1) uygulayan sınıf `ICommand` arabirimi. Genel tür bağımsız değişkeninin türüdür `Execute` ve `CanExecute` yöntemleri.

### <a name="simple-method-executions"></a>Basit bir yöntem yürütme

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) örnek bir ViewModel komut arabiriminde kullanmayı gösterir. [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) Sınıf türünün iki özelliklerini tanımlar `ICommand` ve en kolay geçirir iki özel özellikleri de tanımlar [ `Command` Oluşturucusu](xref:Xamarin.Forms.Command.%23ctor(System.Action)). Programı için bu ViewModel veri bağlamaları içeren `Command` iki özelliklerini `Button` öğeleri.

`Button` Öğelerini kolayca değiştirilebilir ile `TapGestureRecognizer` kod değişikliğine gerek olmadan XAML nesneleri.

### <a name="a-calculator-almost"></a>Bir hesap makinesi neredeyse

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) örnek yaptığı her ikisini de kullanmanız `Execute` ve `CanExecute` yöntemlerinin `ICommand`. Bunu kullanan bir [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) sınıfını [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) kitaplığı. ViewModel türünün altı özelliklerini içeren `ICommand`. Bunlar başlatılır [ `Command` Oluşturucusu](xref:Xamarin.Forms.Command.%23ctor(System.Action)) ve [ `Command` Oluşturucusu](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean})) , `Command` ve [ `Command<T>` Oluşturucusu](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/) ' ın `Command<T>`. Ekleme makinesini sayısal anahtarları tüm ile başlatılan özelliğine bağlıdır `Command<T>`ve `string` bağımsız değişkeni `Execute` ve `CanExecute` belirli anahtar tanımlar.

## <a name="viewmodels-and-the-application-lifecycle"></a>Viewmodel'lar ve uygulama yaşam döngüsü

`AdderViewModel` Kullanılan **AddingMachine** örnek adlı iki yöntem de tanımlar `SaveState` ve `RestoreState`. Bu yöntemler uygulamadan uyku moduna geçer ve yeniden açıldığında çağrılır.



## <a name="related-links"></a>İlgili bağlantılar

- [Bölüm 18 tam metin (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [Bölüm 18 örnekleri](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
