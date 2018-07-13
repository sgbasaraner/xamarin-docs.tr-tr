---
title: Kurumsal uygulamaları doğrulama
description: Bu bölümde, kullanıcı girişini doğrulama hizmetine mobil uygulama performansını açıklanmaktadır. Bu, doğrulama kurallarını belirtme, doğrulama tetikleme ve doğrulama hatalarını gösterme içerir.
ms.prod: xamarin
ms.assetid: 56e4f0fc-48d9-4033-91ec-173bb46a5e4d
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: 2b4be17e3c96ee223433b435a7b1011eafa8e9db
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995833"
---
# <a name="validation-in-enterprise-apps"></a>Kurumsal uygulamaları doğrulama

Kullanıcıların girişi kabul eden herhangi bir uygulama, giriş geçerli olduğundan emin olmanız gerekir. Örneğin, bir uygulama için belirli bir aralıkta yalnızca karakterler içeriyor, belirli bir süre veya belirli bir biçimde eşleşen giriş denetleyebilirsiniz. Doğrulama, bir kullanıcı, uygulamanın başarısız olmasına neden olan veri sağlayabilirsiniz. Doğrulama, iş kuralları zorunlu kılar ve bir saldırgan kötü amaçlı veri ekleme öğesinden engeller.

Bağlam, Model ViewModel Model (MVVM) deseni, bir görünüm modeli veya model genellikle veri doğrulaması gerçekleştir ve böylece kullanıcı bunları düzeltmek için kullanabileceğiniz tüm doğrulama hatalarını görünümüne sinyal gerekecektir. Hizmetine mobil uygulaması, görünüm modeli özelliklerinin zaman uyumlu istemci tarafı doğrulama gerçekleştirir ve tüm doğrulama hatalarını kullanıcısı geçersiz veri içeren denetimi vurgulama ve kullanıcıyı bilgilendirmeniz hata iletilerini göstererek bildirir. neden verileri geçersiz. Şekil 6-1, hizmetine mobil uygulamada doğrulama gerçekleştiriliyor katılan sınıflarını göstermektedir.

[![](validation-images/validation.png "Hizmetine mobil uygulamadaki doğrulama sınıflarını")](validation-images/validation-large.png#lightbox "hizmetine mobil uygulamadaki doğrulama sınıfları")

**Şekil 6-1**: hizmetine mobil uygulamadaki doğrulama sınıfları

Doğrulama gerektiren bir görünüm modeli özellikleri, tür `ValidatableObject<T>`ve her `ValidatableObject<T>` örneğine sahip eklenen doğrulama kuralları, `Validations` özelliği. Doğrulama, görünüm modeli çağırarak çağrılır `Validate` yöntemi `ValidatableObject<T>` doğrulama alır. Örneğin, kurallar ve bunlara karşı yürütür `ValidatableObject<T>` `Value` özelliği. Tüm doğrulama hatalarını yerleştirilip `Errors` özelliği `ValidatableObject<T>` örneği ve `IsValid` özelliği `ValidatableObject<T>` örneği, doğrulama başarılı veya başarısız olduğunu göstermek için güncelleştirilir.

Özellik değişikliği bildirimi tarafından sağlanır `ExtendedBindableObject` sınıfı ve bu nedenle bir [ `Entry` ](xref:Xamarin.Forms.Entry) denetim bağlayabilirsiniz `IsValid` özelliği `ValidatableObject<T>` olsun veya olmasın bildirim almak için Görünüm modeli sınıfı örneği girilen verileri geçerli değil.

## <a name="specifying-validation-rules"></a>Doğrulama kurallarını belirtme

Doğrulama kuralları, türetilen bir sınıf oluşturarak belirtilir `IValidationRule<T>` aşağıdaki kod örneğinde gösterilen arabirimi:

```csharp
public interface IValidationRule<T>  
{  
    string ValidationMessage { get; set; }  
    bool Check(T value);  
}
```

Bu arabirim, bir doğrulama kuralı sınıfı sağlamalısınız belirtir bir `boolean` `Check` gerekli doğrulama gerçekleştirmek için kullanılan yöntem ve bir `ValidationMessage` özellik değeri olan doğrulama hata iletisi görüntülenir doğrulama başarısız olur.

Aşağıdaki örnekte gösterildiği kod `IsNotNullOrEmptyRule<T>` kullanıcı adı ve parola kullanıcı tarafından girilen doğrulama gerçekleştirmek için kullanılan doğrulama kuralı `LoginView` sahte services hizmetine mobil uygulamada kullanırken:

```csharp
public class IsNotNullOrEmptyRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        return !string.IsNullOrWhiteSpace(str);  
    }  
}
```

`Check` Yöntemi döndürür bir `boolean` değer bağımsız değişkeninde olup olmadığını gösteren `null`boş veya yalnızca boşluk karakterlerinden oluşur.

Hizmetine mobil uygulama tarafından kullanılmıyor olsa da, aşağıdaki kod örneği, e-posta adreslerini doğrulamak için bir doğrulama kuralı gösterir:

```csharp
public class EmailRule<T> : IValidationRule<T>  
{  
    public string ValidationMessage { get; set; }  

    public bool Check(T value)  
    {  
        if (value == null)  
        {  
            return false;  
        }  

        var str = value as string;  
        Regex regex = new Regex(@"^([\w\.\-]+)@([\w\-]+)((\.(\w){2,3})+)$");  
        Match match = regex.Match(str);  

        return match.Success;  
    }  
}
```

`Check` Yöntemi döndürür bir `boolean` değer bağımsız değişkeninde bir geçerli e-posta adresi olup olmadığını gösterir. Bu değer bağımsız değişkeninde belirtilen normal ifade deseni ilk oluşum için arama yaparak sağlanır `Regex` Oluşturucusu. Normal ifade deseni giriş dizesinde bulunamadı değerini denetleyerek belirlenebilir `Match` nesnenin `Success` özelliği.

> [!NOTE]
> Özellik doğrulamasını bazen bağımlı özellikler içerebilir. B. özelliğinde ayarlanmış belirli bir değeri özellik A için geçerli değerler kümesini bağlı olduğunda, bağımlı özellikleri örneğidir B. özelliğinin değeri alınırken içerir A özelliğinin değeri izin verilen değerlerden biri olduğunu denetlemek için Ayrıca, B özelliğinin değeri değiştiğinde, özellik A yeniden doğrulanır gerekir.

## <a name="adding-validation-rules-to-a-property"></a>Bir özelliğe doğrulama kuralları ekleme

Mobil uygulama hizmetine doğrulama gerektiren bir görünüm modeli özellikleri türünde olması bildirilir `ValidatableObject<T>`burada `T` doğrulanacak olan veri türü. Aşağıdaki kod örneği, iki özellik bir örneğini gösterir:

```csharp
public ValidatableObject<string> UserName  
{  
    get  
    {  
        return _userName;  
    }  
    set  
    {  
        _userName = value;  
        RaisePropertyChanged(() => UserName);  
    }  
}  

public ValidatableObject<string> Password  
{  
    get  
    {  
        return _password;  
    }  
    set  
    {  
        _password = value;  
        RaisePropertyChanged(() => Password);  
    }  
}
```

Doğrulama kuralları gerçekleşmesi doğrulama için eklenmesi gerekir `Validations` her koleksiyon `ValidatableObject<T>` , aşağıdaki kod örneğinde gösterildiği gibi örnek:

```csharp
private void AddValidations()  
{  
    _userName.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A username is required."   
    });  
    _password.Validations.Add(new IsNotNullOrEmptyRule<string>   
    {   
        ValidationMessage = "A password is required."   
    });  
}
```

Bu yöntemi ekler `IsNotNullOrEmptyRule<T>` doğrulama kuralı `Validations` her koleksiyon `ValidatableObject<T>` doğrulama kuralının için değerler belirten örneği `ValidationMessage` özelliği görüntülenir doğrulama hatası iletisini belirtir doğrulama başarısız olur.

## <a name="triggering-validation"></a>Tetikleyici doğrulama

Bir özellik değiştiğinde hizmetine mobil uygulamada kullanılan doğrulama yaklaşımı doğrulama özelliğinin ve otomatik olarak tetikleyici doğrulama el ile tetikleyebilirsiniz.

### <a name="triggering-validation-manually"></a>Doğrulama el ile tetikleme

Doğrulama için bir görünüm modeli özelliği el ile tetiklenebilir. Örneğin, bu gerçekleşir hizmetine mobil uygulamada kullanıcı dokunduğunda **oturum açma** düğmesini `LoginView`, sahte hizmetlerini kullanırken. Komut temsilci çağrılarını `MockSignInAsync` yönteminde `LoginViewModel`, hangi çağırır doğrulama yürüterek `Validate` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
private bool Validate()  
{  
    bool isValidUser = ValidateUserName();  
    bool isValidPassword = ValidatePassword();  
    return isValidUser && isValidPassword;  
}  

private bool ValidateUserName()  
{  
    return _userName.Validate();  
}  

private bool ValidatePassword()  
{  
    return _password.Validate();  
}
```

`Validate` Yöntemi kullanıcı adı ve kullanıcı tarafından girilen parola doğrulama gerçekleştirir `LoginView`, her doğrulama yöntemini çağırarak `ValidatableObject<T>` örneği. Aşağıdaki kod örneğinde elde edilen doğrulama yöntemi gösterir `ValidatableObject<T>` sınıfı:

```csharp
public bool Validate()  
{  
    Errors.Clear();  

    IEnumerable<string> errors = _validations  
        .Where(v => !v.Check(Value))  
        .Select(v => v.ValidationMessage);  

    Errors = errors.ToList();  
    IsValid = !Errors.Any();  

    return this.IsValid;  
}
```

Bu yöntem temizler `Errors` koleksiyonu ve tüm doğrulama kurallarını nesnenin eklendi alır `Validations` koleksiyonu. `Check` Yöntemi her alınan doğrulama kuralı için yürütülür ve `ValidationMessage` verileri doğrulamak için başarısız olan herhangi bir doğrulama kuralı için özellik değeri eklenir `Errors` koleksiyonunu `ValidatableObject<T>` örneği. Son olarak, `IsValid` özelliği ayarlanır ve doğrulama başarılı veya başarısız olduğunu belirten çağıran Metoda, bu değer döndürülür.

### <a name="triggering-validation-when-properties-change"></a>Doğrulama özelliklerini değiştirdiğinizde tetikleme

Bağlı bir özellik değiştiğinde doğrulamayı da tetiklenebilir. Örneğin, iki yönlü bir bağlama olduğunda `LoginView` ayarlar `UserName` veya `Password` özelliği, doğrulama tetiklenir. Aşağıdaki kod örneği, nasıl gerçekleştirildiğini gösterir:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    <Entry.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="TextChanged"  
            Command="{Binding ValidateUserNameCommand}" />  
    </Entry.Behaviors>  
    ...  
</Entry>
```

[ `Entry` ](xref:Xamarin.Forms.Entry) T:System.Windows.Forms.Binding için `UserName.Value` özelliği `ValidatableObject<T>` örneği ve denetimin `Behaviors` gruplandırmasında bir `EventToCommandBehavior` ona eklediğiniz örneği. Bu davranış yürütür `ValidateUserNameCommand` yanıt olarak [`TextChanged`] üzerinde tetikleme olay `Entry`, ne zaman tetiklenir metinde `Entry` değişiklikler. Buna karşılık, `ValidateUserNameCommand` temsilcisini yürütür `ValidateUserName` yürüten yöntemi `Validate` metodunda `ValidatableObject<T>` örneği. Bu nedenle, her seferinde kullanıcının girdiği karakter `Entry` kullanıcı adı olarak girilen veri doğrulama denetimi gerçekleştirilir.

Davranışlar hakkında daha fazla bilgi için bkz. [uygulama davranışları](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

<a name="displaying_validation_errors" />

## <a name="displaying-validation-errors"></a>Doğrulama hataları görüntüleme

Hizmetine mobil uygulama, kırmızı bir çizgi ile geçersiz veri içeren denetimi vurgulayarak tüm doğrulama hatalarını kullanıcı size bildirir ve kullanıcı neden bildiren bir hata iletisi görüntüleyerek veri denetimi içeren aşağıdaki geçersiz Geçersiz veri. Geçersiz veriler düzeltildiğinden, satır siyah olarak değiştirir ve hata iletisini kaldırılır. Doğrulama hataları mevcut olduğunda Şekil 6-2 LoginView hizmetine mobil uygulamada gösterir.

![](validation-images/validation-login.png "Oturum açma sırasında doğrulama hataları görüntüleme")

**Şekil 6-2:** oturum açma sırasında doğrulama hataları görüntüleme

### <a name="highlighting-a-control-that-contains-invalid-data"></a>Geçersiz veri içeren bir denetimi vurgulama

`LineColorBehavior` Ekli davranışı vurgulamak için kullanılan [ `Entry` ](xref:Xamarin.Forms.Entry) denetimleri nerede doğrulama hataları oluştu. Aşağıdaki kod örnekte gösterildiği nasıl `LineColorBehavior` bağlı davranışı için bağlı bir `Entry` denetimi:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">
    <Entry.Style>
        <OnPlatform x:TypeArguments="Style">
            <On Platform="iOS, Android" Value="{StaticResource EntryStyle}" />
            <On Platform="UWP" Value="{StaticResource UwpEntryStyle}" />
        </OnPlatform>
    </Entry.Style>
    ...
</Entry>
```

[ `Entry` ](xref:Xamarin.Forms.Entry) Aşağıdaki kod örneğinde gösterilen açık bir stil denetimi kullanır:

```xaml
<Style x:Key="EntryStyle"  
       TargetType="{x:Type Entry}">  
    ...  
    <Setter Property="behaviors:LineColorBehavior.ApplyLineColor"  
            Value="True" />  
    <Setter Property="behaviors:LineColorBehavior.LineColor"  
            Value="{StaticResource BlackColor}" />  
    ...  
</Style>
```

Bu stilini ayarlar `ApplyLineColor` ve `LineColor` özelliklerini bağlı `LineColorBehavior` davranışı üzerinde bağlı [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi. Stilleri hakkında daha fazla bilgi için bkz. [stilleri](~/xamarin-forms/user-interface/styles/index.md).

Zaman değerini `ApplyLineColor` ekli özellik kümesi veya değişiklik olduğunu `LineColorBehavior` ekli davranışı yürütür `OnApplyLineColorChanged` aşağıdaki kod örneğinde gösterilen yöntemi:

```csharp
public static class LineColorBehavior  
{  
    ...  
    private static void OnApplyLineColorChanged(  
                BindableObject bindable, object oldValue, object newValue)  
    {  
        var view = bindable as View;  
        if (view == null)  
        {  
            return;  
        }  

        bool hasLine = (bool)newValue;  
        if (hasLine)  
        {  
            view.Effects.Add(new EntryLineColorEffect());  
        }  
        else  
        {  
            var entryLineColorEffectToRemove =   
                    view.Effects.FirstOrDefault(e => e is EntryLineColorEffect);  
            if (entryLineColorEffectToRemove != null)  
            {  
                view.Effects.Remove(entryLineColorEffectToRemove);  
            }  
        }  
    }  
}
```

Bu yöntem parametrelerini davranışı bağlı denetim örneği ve eski ve yeni değerleri sağlayın `ApplyLineColor` ekli özellik. `EntryLineColorEffect` Sınıfı eklendi denetimin [ `Effects` ](xref:Xamarin.Forms.Element.Effects) koleksiyonu, `ApplyLineColor` ekli özelliği `true`, aksi takdirde denetimin kaldırılır `Effects` koleksiyonu. Davranışlar hakkında daha fazla bilgi için bkz. [uygulama davranışları](~/xamarin-forms/enterprise-application-patterns/mvvm.md#implementing_behaviors).

`EntryLineColorEffect` Kılabileceği [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) sınıfı ve aşağıdaki kod örneğinde gösterilmiştir:

```csharp
public class EntryLineColorEffect : RoutingEffect  
{  
    public EntryLineColorEffect() : base("eShopOnContainers.EntryLineColorEffect")  
    {  
    }  
}
```

[ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) Sınıfı, platforma özel efekt iç sarmalayan bir platformdan bağımsız etkisi temsil eder. Tür bilgilerini bir platforma özel efekt için hiçbir derleme zamanı erişimi olduğundan bu etkili temizleme işlemini kolaylaştırır. `EntryLineColorEffect` Çözümleme grup adı ve her platforma özel efekt sınıfı belirtilen benzersiz kimliği bir birleşimini içeren bir parametre olarak geçirerek, bir temel sınıf oluşturucusunu çağırır.

Aşağıdaki örnekte gösterildiği kod `eShopOnContainers.EntryLineColorEffect` iOS için uygulama:

```csharp
[assembly: ResolutionGroupName("eShopOnContainers")]  
[assembly: ExportEffect(typeof(EntryLineColorEffect), "EntryLineColorEffect")]  
namespace eShopOnContainers.iOS.Effects  
{  
    public class EntryLineColorEffect : PlatformEffect  
    {  
        UITextField control;  

        protected override void OnAttached()  
        {  
            try  
            {  
                control = Control as UITextField;  
                UpdateLineColor();  
            }  
            catch (Exception ex)  
            {  
                Console.WriteLine("Can't set property on attached control. Error: ", ex.Message);  
            }  
        }  

        protected override void OnDetached()  
        {  
            control = null;  
        }  

        protected override void OnElementPropertyChanged(PropertyChangedEventArgs args)  
        {  
            base.OnElementPropertyChanged(args);  

            if (args.PropertyName == LineColorBehavior.LineColorProperty.PropertyName ||  
                args.PropertyName == "Height")  
            {  
                Initialize();  
                UpdateLineColor();  
            }  
        }  

        private void Initialize()  
        {  
            var entry = Element as Entry;  
            if (entry != null)  
            {  
                Control.Bounds = new CGRect(0, 0, entry.Width, entry.Height);  
            }  
        }  

        private void UpdateLineColor()  
        {  
            BorderLineLayer lineLayer = control.Layer.Sublayers.OfType<BorderLineLayer>()  
                                                             .FirstOrDefault();  

            if (lineLayer == null)  
            {  
                lineLayer = new BorderLineLayer();  
                lineLayer.MasksToBounds = true;  
                lineLayer.BorderWidth = 1.0f;  
                control.Layer.AddSublayer(lineLayer);  
                control.BorderStyle = UITextBorderStyle.None;  
            }  

            lineLayer.Frame = new CGRect(0f, Control.Frame.Height-1f, Control.Bounds.Width, 1f);  
            lineLayer.BorderColor = LineColorBehavior.GetLineColor(Element).ToCGColor();  
            control.TintColor = control.TextColor;  
        }  

        private class BorderLineLayer : CALayer  
        {  
        }  
    }  
}
```

`OnAttached` Xamarin.Forms için yöntemi alır yerel denetim [ `Entry` ](xref:Xamarin.Forms.Entry) denetlemek ve çizgi rengini çağırarak güncelleştirmeleri `UpdateLineColor` yöntemi. `OnElementPropertyChanged` Geçersiz kılma üzerinde bağlanılabilir özellik değişiklikleri yanıt `Entry` , çizgi rengini güncelleştirerek denetimi ekli `LineColor` özellik değişiklikleri veya [ `Height` ](xref:Xamarin.Forms.VisualElement.Height) özelliği`Entry`değişiklikler. Etkileri hakkında daha fazla bilgi için bkz: [etkileri](~/xamarin-forms/app-fundamentals/effects/index.md).

Ne zaman geçerli veri girilen [ `Entry` ](xref:Xamarin.Forms.Entry) denetimi, geçerli siyah bir çizgi doğrulama hatası olduğunu göstermek için denetim altına. Şekil 6-3, bunun bir örneği gösterilmektedir.

![](validation-images/validation-blackline.png "Doğrulama hatası gösteren siyah çizgi")

**Şekil 6-3**: siyah bir çizgi belirten hiçbir doğrulama hatası

[ `Entry` ](xref:Xamarin.Forms.Entry) Denetimi de sahip bir [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) eklenen kendi [ `Triggers` ](xref:Xamarin.Forms.VisualElement.Triggers) koleksiyonu. Aşağıdaki örnekte gösterildiği kod `DataTrigger`:

```xaml
<Entry Text="{Binding UserName.Value, Mode=TwoWay}">  
    ...  
    <Entry.Triggers>  
        <DataTrigger   
            TargetType="Entry"  
            Binding="{Binding UserName.IsValid}"  
            Value="False">  
            <Setter Property="behaviors:LineColorBehavior.LineColor"   
                    Value="{StaticResource ErrorColor}" />  
        </DataTrigger>  
    </Entry.Triggers>  
</Entry>
```

Bu [ `DataTrigger` ](xref:Xamarin.Forms.DataTrigger) izleyiciler `UserName.IsValid` özellik ve değer ise olur `false`, yürütülmeden [ `Setter` ](xref:Xamarin.Forms.Setter), hangi değişikliklerin `LineColor` bağlı özelliği `LineColorBehavior` davranışı kırmızı bağlı. Şekil 6-4'te, bunun bir örneğini gösterir.

![](validation-images/validation-redline.png "Doğrulama hatası gösteren kırmızı çizgi")

**Şekil 6-4**: doğrulama hatası gösteren kırmızı çizgi

Satırda [ `Entry` ](xref:Xamarin.Forms.Entry) girilen verileri geçersiz olduğu sürece denetim kırmızı kalır, aksi takdirde girilen verilerin geçerli olduğunu belirtmek için siyah olarak değiştirir.

Tetikleyiciler hakkında daha fazla bilgi için bkz. [Tetikleyicileri](~/xamarin-forms/app-fundamentals/triggers.md).

### <a name="displaying-error-messages"></a>Hata iletilerini görüntüleme

Kullanıcı arabirimini etiket denetimleri verisini doğrulanamadı. her denetimin altında bir doğrulama hata iletisi görüntüler. Aşağıdaki örnekte gösterildiği kod [ `Label` ](xref:Xamarin.Forms.Label) görüntüleyen bir doğrulama hata iletisi kullanıcının geçerli bir kullanıcı adı girilmezse:

```xaml
<Label Text="{Binding UserName.Errors, Converter={StaticResource FirstValidationErrorConverter}"  
       Style="{StaticResource ValidationErrorLabelStyle}" />
```

Her [ `Label` ](xref:Xamarin.Forms.Label) bağlar `Errors` özelliği Doğrulanmakta olan Görünüm model nesnesi. `Errors` Özelliği tarafından sağlanan `ValidatableObject<T>` sınıfı ve tür `List<string>`. Çünkü `Errors` özelliği, birden çok doğrulama hataları içerebilir `FirstValidationErrorConverter` örneği görüntülemek için koleksiyondaki ilk hata almak için kullanılır.

## <a name="summary"></a>Özet

Hizmetine mobil uygulaması, görünüm modeli özelliklerinin zaman uyumlu istemci tarafı doğrulama gerçekleştirir ve tüm doğrulama hatalarını kullanıcısı geçersiz veri içeren denetimi vurgulama ve kullanıcıyı bilgilendirmeniz hata iletilerini göstererek bildirir. neden verileri geçersiz.

Doğrulama gerektiren bir görünüm modeli özellikleri, tür `ValidatableObject<T>`ve her `ValidatableObject<T>` örneğine sahip eklenen doğrulama kuralları, `Validations` özelliği. Doğrulama, görünüm modeli çağırarak çağrılır `Validate` yöntemi `ValidatableObject<T>` doğrulama alır. Örneğin, kurallar ve bunlara karşı yürütür `ValidatableObject<T>` `Value` özelliği. Tüm doğrulama hatalarını yerleştirilip `Errors` özelliği `ValidatableObject<T>`örneği ve `IsValid` özelliği `ValidatableObject<T>` örneği, doğrulama başarılı veya başarısız olduğunu göstermek için güncelleştirilir.


## <a name="related-links"></a>İlgili bağlantılar

- [(2 Mb PDF) e-kitabı indir](https://aka.ms/xamarinpatternsebook)
- [Hizmetine (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
