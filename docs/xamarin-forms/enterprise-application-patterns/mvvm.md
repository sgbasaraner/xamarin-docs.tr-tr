---
title: Model-View-ViewModel düzeni
description: Bu bölümde MVVM düzeni eShopOnContainers mobil uygulama düzgün bir şekilde uygulama kullanıcı arabirimi iş ve sunu mantığını ayırmak için nasıl kullandığını açıklar.
ms.prod: xamarin
ms.assetid: dd8c1813-df44-4947-bcee-1a1ff2334b87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: fe2cace6a0fc3a1d901f55556eed09380f8f2006
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245438"
---
# <a name="the-model-view-viewmodel-pattern"></a>Model-View-ViewModel düzeni

Xamarin.Forms geliştirme deneyimi genellikle bir kullanıcı arabirimi XAML'de oluşturma ve kullanıcı arabiriminde işleyen arka plan kod ekleme içerir. Uygulamaları değiştirilir ve boyut ve kapsam büyümesine gibi karmaşık bakım sorunları ortaya çıkabilir. Bu sorunları UI denetimleri ve UI değişiklikleri ve birim testi kodun zorluk hale maliyetini artırır iş mantığı arasında sıkı bağ içerir.

Model-View-ViewModel (MVVM) deseni, uygulamanın kendi kullanıcı arabiriminden (UI) iş ve sunu mantığı düzgün bir şekilde ayırmak için yardımcı olur. Uygulama mantığı ve UI arasında temiz bir ayrım bakımı çok sayıda geliştirme sorunları gidermek için yardımcı olur ve bir uygulama sınamak için korumak ve gelişmesi daha kolay hale getirebilir. Kodu yeniden kullanın fırsatlar büyük ölçüde artırabilir ve geliştiricilerin sağlar ve bunların ilgili bölümleri Uygulama geliştirirken UI tasarımcıları daha kolayca işbirliği.

## <a name="the-mvvm-pattern"></a>MVVM düzeni

MVVM desende üç temel bileşeni vardır: model, Görünüm ve görünüm modeli. Her bir ayrı amaca hizmet eder. Şekil 2-1 üç bileşenleri arasındaki ilişkiler gösterilmektedir.

![](mvvm-images/mvvm.png "MVVM düzeni")

**Şekil 2-1**: MVVM düzeni

Her bileşenleri sorumluluklarını anlamanın yanı sıra, birbirleri ile nasıl etkileşim kurduklarını anlamak önemlidir. Yüksek bir düzeyde görünümü "Görünüm modeli bildiği" Görünüm modeli "modeli bildiği" ancak model görünüm modelinin farkında değildir ve görünüm modeli görünümünü farkında değildir. Bu nedenle, görünüm modeli model görünümünden yalıtır ve bağımsız olarak görünüm gelişmesi modeli sağlar.

MVVM düzeni kullanmanın avantajları şunlardır:

-   Var olan iş mantığı yalıtan varolan modeli uygulaması varsa, zor veya değiştirmek için riskli olabilir. Bu senaryoda, görünüm modeli modeli sınıfları için bir bağdaştırıcı görevi görür ve model kodunu önemli değişikliklere yapmaktan kaçınmak sağlar.
-   Geliştiriciler, görünümü kullanmadan görünüm modeli ve modeli için birim testleri oluşturabilirsiniz. Görünüm modeli için birim testleri tam olarak aynı işlevselliği görünümü tarafından kullanılan alıştırma yapabilirsiniz.
-   Görünüm tamamen XAML'de uygulanır koşuluyla UI uygulama koda dokunmadan tasarlanması. Bu nedenle, görünümü yeni bir sürümü mevcut görünüm model ile çalışması gerekir.
-   Tasarımcılar ve geliştiriciler bağımsız olarak ve aynı anda bileşenleri üzerinde geliştirme sürecinde çalışabilirsiniz. Geliştiriciler görünüm modeli ve modeli bileşenleri üzerinde çalışabilecek olmanıza karşın tasarımcıları görünümünde, odaklanabilirsiniz.

MVVM verimli bir şekilde kullanmak için anahtarı doğru sınıfları uygulama kodunuzda faktörü nasıl anlamak ve sınıfları nasıl etkileşim anlamak arasındadır. Aşağıdaki bölümlerde sorumluluklarını MVVM düzeninde sınıfların her biri açıklanmaktadır.

### <a name="view"></a>Görüntüle

Görünüm yapısını, Düzen ve hangi kullanıcı ekranda görür görünümünü tanımlamaktan sorumludur. İdeal olarak, her görünüm XAML içinde bir sınırlı iş mantığı içermeyen arka plan kodu ile tanımlanır. Ancak, bazı durumlarda, arka plan kodu animasyonları gibi XAML express zordur görsel davranışını uygulayan UI mantığı içerebilir.

Genellikle bir Xamarin.Forms uygulaması bir görünümü olan bir [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-türetilmiş veya [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)-türetilmiş sınıf. Ancak, görünümleri de görüntülendiğinde nesneyi görsel olarak sunmak üzere kullanılacak kullanıcı Arabirimi öğeleri belirtir ve veri şablonu tarafından temsil edilebilir. Bir görünüm olarak veri şablonu tüm arka plan kodu yok ve belirli görünüm model türü için bağlamak için tasarlanmıştır.

> [!TIP]
> Etkinleştirme ve arka plan kod UI öğelerini devre dışı bırakma kaçının. Görünüm modelleri görünümün görüntüleme, bir komut olup veya bir işlem bekleyen olduğunun bir göstergesi gibi bazı yönlerini etkilemek mantıksal durum değişikliklerini tanımlamak için sorumlu olduğundan emin olun. Bu nedenle, etkinleştirme ve kullanıcı Arabirimi öğeleri model özellikleri yerine etkinleştirme ve kod arkasında devre dışı bırakma görüntülemek için bağlama tarafından devre dışı.

Gibi bir düğmeye yanıt etkileşimleri görünüm, görünüm model üzerinde kod yürütmek veya öğe seçimi için birkaç seçeneğiniz vardır. Bir denetim komutları, Denetim 's destekliyorsa `Command` özelliği veri bağlama için bir `ICommand` görünüm modeli özelliği. Denetimin komutu çağrıldığında, görünüm modeli kodda yürütülür. Komutları, yanı sıra davranışları görünümünde bir nesneye iliştirilmiş ve çağrılacak komut veya çıkarılmasına olay için dinleme. Yanıt olarak davranışı ardından çağırabileceği bir `ICommand` görünüm modeli veya Görünüm modeli bir yöntem.

### <a name="viewmodel"></a>ViewModel

Görünüm modeli, özellikler ve görünüm için veri bağlamak için komutlar uygular ve görünüm durumu değişiklikleri değişiklik bildirimi olaylar ile uyarır. Özellikler ve görünüm modeli sağlar komutları UI tarafından sunulan işlevselliği tanımlayan ancak görünümü nasıl işlevselliğini görüntülenecek belirler.

> [!TIP]
> UI esnek zaman uyumsuz işlemleri ile koruyun. Mobil uygulamalar, kullanıcının algısına performansını artırmak için engellemesini kullanıcı Arabirimi iş parçacığı tutmanız gerekir. Bu nedenle, görünüm modeli g/ç işlemleri için zaman uyumsuz yöntemleri kullanın ve zaman uyumsuz olarak özellik değişikliklerini görünümlerini bildirmek için olayları Yükselt.

Görünüm modeli, gerekli tüm model sınıflarıyla görünümün etkileşimleri düzenlemekten sorumludur. Genellikle görünüm modeli model sınıflar arasında bir-çok ilişkisi yoktur. Görünüm modeli, böylece görünümünde denetimleri doğrudan için veri bağlama modeli sınıflarını görüntülemek için doğrudan kullanıma sunmak seçebilirsiniz. Bu durumda, model sınıfları veri bağlamayı destekleyen ve bildirim olayları değiştirmek için tasarlanmış gerekir.

Her görünüm modeli görünüme kolayca tüketebileceği bir formda bir modelden veri sağlar. Bunu başarmak için veri dönüştürme bazen görünüm modeli gerçekleştirir. Görünüm bağlayabilirsiniz özellikleri sağladığından bu veri dönüştürme görünüm modelinde yerleştirme iyi bir fikirdir. Örneğin, görünüm modeli görüntülemek için Görünüm tarafından kolaylaştırmak için iki özelliklerinin değerlerini birleştirebilirsiniz.

> [!TIP]
> Veri Dönüşümleri dönüştürme katmanda merkezileştirme. Görünüm modeli ve görünüm arasında bulunur ayrı veri dönüştürme katmanı olarak dönüştürücüler kullanmak da mümkündür. Örneğin, veri görünüm modeli sağlamaz özel biçimlendirme gerektirdiğinde bu gerekli olabilir.

Görünümü ile iki yönlü veri bağlaması katılmayı görünüm modeli için sırayla özelliklerini artırmalısınız `PropertyChanged` olay. Görünüm modelleri uygulayarak bu gereksinimi karşılamak `INotifyPropertyChanged` arabirim ve oluşturma `PropertyChanged` bir özelliği değiştirildiğinde olay.

Koleksiyon görünümü kolay `ObservableCollection<T>` sağlanır. Bu koleksiyon uygulamak zorunda gelen Geliştirici gereksinimini azaltır koleksiyonu değiştirildi bildirim uygulayan `INotifyCollectionChanged` koleksiyonlarda arabirimi.

### <a name="model"></a>Model

Model, uygulamanın verileri kapsüllemek görsel olmayan sınıfları sınıflarıdır. Bu nedenle, model, genellikle iş ve Doğrulama mantığı ile birlikte bir veri modeli içeren uygulamanın etki alanı modeli temsil eden olarak düşünülebilir. Veri aktarımı nesneleri (DTOs), düz eski CLR nesnelerini (POCOs) ve oluşturulan varlığı ve proxy nesneleri model nesneleri örneklerindendir.

Model sınıflar genellikle Hizmetleri ya da veri erişimi ve önbelleğe alma kapsülleyen depoları ile birlikte kullanılır.

## <a name="connecting-view-models-to-views"></a>Görünümlere bağlantı modelleri görüntüle

Görünüm modelleri görünümlerine Xamarin.Forms veri bağlama özelliklerini kullanarak bağlanabilir. Görünümleri oluşturmak ve modelleri görüntülemek ve çalışma zamanında ilişkilendirmek için kullanılan birçok yaklaşım vardır. Bu yaklaşım, Görünüm ilk oluşumunu ve görünüm modeli ilk oluşturma olarak bilinen iki kategoriye ayrılır. Görünüm ilk oluşumunu ve model ilk birleşim tercih ve karmaşıklık söz konusu görünüm arasında seçim yapma. Ancak, tüm yaklaşımlar kendi Bindingparameters'te özelliğine atanan bir görünüm modeli görünüme için aynı AIM paylaşır.

Görünümü ile ilk birleşim uygulama bağlı oldukları modelleri görüntüle bağlanmak görünümleri, kavramsal olarak oluşur. Bu yaklaşımın birincil avantajı modelleri görüntüle görünümleri kendilerini bir bağımlılığı olduğundan, bu geniş eşleşmiş oluşturmak kolay birim sınanabilir uygulamaları yapmasıdır. Görsel yapısını aşağıdaki yerine tarafından sınıfları nasıl oluşturulduğunu ve ilişkili anlamak için kod yürütmeyi izlemek zorunda kalmadan uygulama yapısını anlamak kolaydır. Ayrıca, Görünüm ilk yapım sistemiyle olmasını sağlayan bir görünüm modeli ilk birleşim karmaşık ve platformuyla uyuşmayan Gezinti ortaya çıktığında, dünyada sayfalar için sorumlu olan Xamarin.Forms Gezinti hizalar.

Görünümü ile modeli ilk birleşim uygulama kavramsal görünümü modellerinin, görünüm için Görünüm modeli bulmak için sorumlu olan bir hizmet ile oluşur. Görünüm oluşturma yerine koyma, uygulama UI olmayan mantıksal yapısını odaklanmak vermeden soyutlanması bu yana görünüm modeli ilk birleşim bazı geliştiriciler için daha doğal. Ayrıca, diğer görünüm modelleri tarafından oluşturulacak modelleri görüntüle sağlar. Ancak, bu yaklaşım genellikle karmaşık ve uygulama çeşitli kısımlarını nasıl oluşturulduğunu ve ilişkili anlaşılması güç olabilir.

> [!TIP]
> Görünüm modelleri ve görünümler bağımsız tutun. Bir özelliğe bir veri kaynağı görünümleri bağlama görünümün asıl karşılık gelen kendi görünüm modeli bağımlı olması gerekir. Özellikle, görünüm türleri gibi başvuru olmayan [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) ve [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/), görünüm modellerinden. Burada özetlenen ilkeleri izleyerek modelleri görüntüle haklarında, bu nedenle kapsamı sınırlayarak yazılım hataları olasılığını azaltır sınanabilir.

Aşağıdaki bölümlerde modelleri görüntüle görünümlerine bağlanma için ana yaklaşım açıklanmaktadır.

### <a name="creating-a-view-model-declaratively"></a>Görünüm Model bildirimli olarak oluşturma

XAML'de karşılık gelen kendi görünüm modeli bildirimli olarak örneği görünüme en kolay yaklaşım içindir. Görünüm oluşturulduğunda, karşılık gelen görünüm modeli nesnesi de oluşturulur. Bu yaklaşım, aşağıdaki kod örneğinde gösterilmiştir:

```xaml
<ContentPage ... xmlns:local="clr-namespace:eShop">  
    <ContentPage.BindingContext>  
        <local:LoginViewModel />  
    </ContentPage.BindingContext>  
    ...  
</ContentPage>
```

Zaman [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) oluşturulur örneği `LoginViewModel` otomatik olarak oluşturulur ve ayarlama görünümün [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/).

Bu bildirim temelli yapım ve görünümü tarafından görünüm modeli atanması, basittir ancak görünüm modeli varsayılan (parametre daha az) oluşturucuda gerektirdiğini dezavantajı sahip avantajı vardır.

### <a name="creating-a-view-model-programmatically"></a>Bir görünüm modeli program aracılığıyla oluşturma

Bir görünüm atanmasına görünüm modeli sonuçlanır arka plan kod dosyasına koda sahip kendi [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) özelliği. Bu genellikle görünümün oluşturucuda aşağıdaki kod örneğinde gösterildiği gibi gerçekleştirilir:

```csharp
public LoginView()  
{  
    InitializeComponent();  
    BindingContext = new LoginViewModel(navigationService);  
}
```

Program yapısı ve görünüm modeli görünümün arka plan kod içinde atanması basit avantajı vardır. Ancak, bu yaklaşımın dezavantajı görünümü Görünüm modeli gerekli bağımlılıkları ile sağlamak gereğidir. Bir bağımlılık ekleme kapsayıcısını kullanarak görünümü ve görünüm modeli arasında Kuplaj gevşek tutmanıza yardımcı olabilir. Daha fazla bilgi için bkz: [bağımlılık ekleme](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

### <a name="creating-a-view-defined-as-a-data-template"></a>Veri şablon olarak tanımlanan bir görünüm oluşturma

Bir görünüm veri şablon olarak tanımlanmış ve bir görünüm model türü ile ilişkili. Veri şablonları kaynaklar olarak tanımlanabilir veya satır içi olarak tanımlanan görünüm modeli görüntüler denetimi içinde olabilir. Görünüm model örneği içeriktir denetim ve veri şablonu görsel olarak sunmak üzere kullanılır. Bu teknik görünüm modeli görünüm oluşturma işlemi ve ardından ilk olarak, örneği bir durum örneğidir.

<a name="automatically_creating_a_view_model_with_a_view_model_locator" />

### <a name="automatically-creating-a-view-model-with-a-view-model-locator"></a>Otomatik olarak bir görünüm modeli bulucuyla görünüm Model oluşturma

Bir görünüm modeli Bulucu modelleri görüntüle ve görünümlere ilişkilerinin örneklemesi yöneten özel bir sınıftır. EShopOnContainers mobil uygulamaya `ViewModelLocator` sınıfı olan iliştirilmiş bir özellik `AutoWireViewModel`, görünüm modelleri görünümleri ile ilişkilendirmek için kullanılır. Görünümün XAML'de ekli bu özellik, görünüm modeli görünüme otomatik olarak bağlanmalıdır aşağıdaki kod örneğinde gösterildiği gibi göstermek için true olarak ayarlanır:

```xaml
viewModelBase:ViewModelLocator.AutoWireViewModel="true"
```

`AutoWireViewModel` Özelliği false olarak başlatılır ve değeri değiştiğinde bir özelliğidir bağlanabilirse `OnAutoWireViewModelChanged` olay işleyicisi çağrılır. Bu yöntem görünüm için Görünüm modeli çözümler. Aşağıdaki kod örneğinde bu nasıl başarıldığı gösterilmektedir:

```csharp
private static void OnAutoWireViewModelChanged(BindableObject bindable, object oldValue, object newValue)  
{  
    var view = bindable as Element;  
    if (view == null)  
    {  
        return;  
    }  

    var viewType = view.GetType();  
    var viewName = viewType.FullName.Replace(".Views.", ".ViewModels.");  
    var viewAssemblyName = viewType.GetTypeInfo().Assembly.FullName;  
    var viewModelName = string.Format(  
        CultureInfo.InvariantCulture, "{0}Model, {1}", viewName, viewAssemblyName);  

    var viewModelType = Type.GetType(viewModelName);  
    if (viewModelType == null)  
    {  
        return;  
    }  
    var viewModel = _container.Resolve(viewModelType);  
    view.BindingContext = viewModel;  
}
```

`OnAutoWireViewModelChanged` Yöntemi, kurala dayalı bir yaklaşım kullanarak görünüm modeli çözümlemeye çalışır. Bu kural varsayar:

-   Görünüm modelleri görünüm türleri aynı bütünleştirilmiş arasındadır.
-   Görünümleri olan bir. Görünümler alt ad alanı.
-   Görünüm modelleri olan bir. ViewModels alt ad alanı.
-   Görünüm model adları görünüm adlarıyla karşılık gelir ve "ViewModel" ile bitmelidir.

Son olarak, `OnAutoWireViewModelChanged` yöntemi kümeleri [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) çözülmüş görünüm model türü için Görünüm türü. Görünüm model türü çözümleme hakkında daha fazla bilgi için bkz: [çözümleme](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#resolution).

Bu yaklaşım, bir uygulama modelleri görüntüle ve görünümlere kendi bağlantısı oluşturmada sorumlu olduğu tek bir sınıf olduğunu avantajına sahiptir.

> [!TIP]
> Bir görünüm modeli Bulucu değiştirme kolaylığı için kullanın. Bir görünüm modeli Bulucu de bağımlılıkları, diğer uygulamaları değiştirme noktası olarak gibi birim sınama veya tasarım zamanı verileri için kullanılabilir.

## <a name="updating-views-in-response-to-changes-in-the-underlying-view-model-or-model"></a>Yanıt temel değişiklikleri olarak güncelleştirme görünümleri, Model veya Model görüntüleme

Tüm görünüm modeli ve bir görünüme erişilebilir modeli sınıfları uygulamalıdır `INotifyPropertyChanged` arabirimi. Bir görünüm modeli veya model sınıfı bu arabirimi uygulayan temel özellik değeri değiştiğinde görünümdeki tüm verilere bağlı denetimler için değişiklik bildirimleri sağlamak için sınıf sağlar.

Uygulamaları aşağıdaki gereksinimleri karşılayarak özellik değişikliği bildirimi doğru kullanımı için tasarlanmış:

-   Her zaman gerçekleştiren bir `PropertyChanged` ortak bir özelliğin değerini değişirse olay. Bu yükseltme varsayın değil `PropertyChanged` olay XAML bağlama nasıl gerçekleştiğini bilgisi nedeniyle yoksayıldı.
-   Her zaman gerçekleştiren bir `PropertyChanged` olayı için herhangi bir hesaplanan değerleri görünümünde diğer özellikleri tarafından kullanılan özellikler modeli veya modeli.
-   Her zaman oluşturma `PropertyChanged` değiştirme özelliği sağlar ya da zaman nesne bilinen güvenli bir durumda olmasını yöntemi sonunda olay. Olay işleyicileri zaman uyumlu olarak çağırarak olay oluşturma işlemi kesintiye uğratır. Bir işlem gerçekleştiriyorsanız aşması durumunda bir güvenli, kısmen güncelleştirilmiş durumda olduğunda geri arama işlevleri nesnesine doğurabilir. Ayrıca, geçişli değişikliklerin tarafından tetiklenen olası `PropertyChanged` olaylar. Geçişli değişiklikleri genellikle güncelleştirmeleri basamaklı değişiklik yürütmek güvenli hale gelmeden önce tamamlanması gerekir.
-   Asla oluşturma bir `PropertyChanged` özelliği değişmezse olay. Yükseltmeden önce eski ve yeni değerleri karşılaştırmanız gerekir yani `PropertyChanged` olay.
-   Asla oluşturma `PropertyChanged` olayı sırasında bir özellik başlatma, bir görünüm modelinin Oluşturucusu. Verilere bağlı denetimler görünümünde bu noktada değişiklik bildirimleri almak için abone değilsiniz.
-   Hiçbir zaman birden fazla yükseltme `PropertyChanged` olay ile aynı özellik adı bağımsız değişkeni içinde ortak bir yöntemin bir sınıfın tek bir zaman uyumlu çağırma. Örneğin, bir `NumberOfItems` özelliği, yedekleme deposu `_numberOfItems` yöntemi artış, alan `_numberOfItems` elli kez döngü yürütülmesi sırasında onu yalnızca özellik değişikliği bildirimi üzerinde tetiklemelidir `NumberOfItems` bir kez özelliği tüm iş tamamlandıktan sonra. Zaman uyumsuz yöntemleri için Yükselt `PropertyChanged` olayı için her zaman uyumlu bir zaman uyumsuz devamlılık zinciri parçası olarak belirtilen özellik adı.

EShopOnContainers mobil uygulamanın kullandığı `ExtendedBindableObject` sağlamak için sınıf değişiklik bildirimleri, aşağıdaki kod örneğinde görüntülendiği:

```csharp
public abstract class ExtendedBindableObject : BindableObject  
{  
    public void RaisePropertyChanged<T>(Expression<Func<T>> property)  
    {  
        var name = GetMemberInfo(property).Name;  
        OnPropertyChanged(name);  
    }  

    private MemberInfo GetMemberInfo(Expression expression)  
    {  
        ...  
    }  
}
```

Xamarin.Form'ın [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) uygulayan sınıf `INotifyPropertyChanged` arabirim ve sağlayan bir [ `OnPropertyChanged` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.OnPropertyChanged/p/System.String/) yöntemi. `ExtendedBindableObject` SAX `RaisePropertyChanged` özelliği çağrılacak yöntem bildirimi değiştirme ve bunu yaparken tarafından sağlanan işlevleri kullanır `BindableObject` sınıfı.

Her görünüm model sınıfı eShopOnContainers mobil uygulamaya türetilen `ViewModelBase` sırayla türeyen sınıf `ExtendedBindableObject` sınıfı. Bu nedenle, her görünüm model sınıfı kullanır `RaisePropertyChanged` yönteminde `ExtendedBindableObject` özellik değişikliği bildirimi sağlamak için sınıf. Aşağıdaki kod örneği, nasıl bir lambda ifadesi kullanarak özellik değişikliği bildirimi'de eShopOnContainers mobil uygulama'yı çağırır gösterir:

```csharp
public bool IsLogin  
{  
    get  
    {  
        return _isLogin;  
    }  
    set  
    {  
        _isLogin = value;  
        RaisePropertyChanged(() => IsLogin);  
    }  
}
```

Lambda ifadesi bu şekilde kullanarak lambda ifadesi her çağrı için değerlendirilecek bulunduğundan maliyet küçük bir performans içerdiğini unutmayın. Performans maliyeti küçüktür ve genellikle bir uygulama etkileyebilecek değil, ancak birçok değişiklik bildirimleri olduğunda maliyetleri tahakkuk. Ancak, bu yaklaşımın avantajı, derleme zamanı tür güvenliği ve destek özellikleri adlandırırken yeniden düzenleme sağlamasıdır.

## <a name="ui-interaction-using-commands-and-behaviors"></a>UI etkileşim komutları ve davranışları kullanma

Mobil uygulamalarda Eylemler yanıt arka plan kod dosyasına bir olay işleyicisi oluşturarak uygulanabilir düğmesini tıklatın gibi bir kullanıcı eylemi olarak genellikle çağrılır. Ancak, MVVM düzeni görünüm model ile eylemi uygulamak için sorumluluk arasındadır ve arka plan kodu yerleştirme kodda kaçınılmalıdır.

Komutları kullanıcı arabiriminde denetimlere bağlı eylemler temsil etmek için kolay bir yol sağlar. Bunlar eylemi uygulayan kod şifreleyebilir ve görünümünde visual kendi gösterimden ayrılmış kalmasını sağlamak için Yardım. Kullanıcı denetimi ile etkileşim kurduğunda bu denetimleri komut çağırma ve Xamarin.Forms bildirimli olarak komuta bağlı denetimler içerir.

Davranışları komut bildirimli olarak bağlı olması denetimler de izin verir. Ancak, bir dizi denetim tarafından başlatılan olayları ile ilişkili bir eylemi çağırmak için Davranışlar kullanılabilir. Bu nedenle, davranışları birçok komutu etkin denetimler, aynı senaryolarda bir büyük ölçüde esneklik ve denetim sağlarken adres. Ayrıca, davranışları komut nesneleri veya yöntemleri özellikle komutları ile etkileşim kurmak için tasarlanmamıştır denetimleri ile ilişkilendirmek için de kullanılabilir.

### <a name="implementing-commands"></a>Komutları uygulama

Görünüm modelleri genellikle uygulayan nesne örneklerini komut özellikleri, görünümünden bağlama için kullanıma `ICommand` arabirimi. Çok sayıda Xamarin.Forms denetimleri sağlar bir `Command` veri olabilir özelliği bağlı bir `ICommand` görünüm modeli tarafından sağlanan nesne. `ICommand` Arabirimi tanımlar bir `Execute` işlemi kapsülleyen yöntemi bir `CanExecute` komutu çağrılabilir olup olmadığını ve bir gösterir yöntemi `CanExecuteChanged` değişiklik etkileyen olup olduğunda oluşan olay komutun çalışacağını belirtir. [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Ve [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) Xamarin.Forms tarafından sağlanan sınıfları, uygulama `ICommand` arabirimi, burada `T` bağımsızdeğişkentürü`Execute`ve `CanExecute`.

Bir görünüm modeli içinde olması gerektiğini türünde bir nesne [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) veya [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) her bir genel özelliğinin türü, görünüm modeli için `ICommand`. `Command` Veya `Command<T>` Oluşturucusu gerektiren bir `Action` ne zaman çağırdı geri çağırma nesnesi `ICommand.Execute` yöntemi çağrılır. `CanExecute` Yöntemi bir isteğe bağlı Oluşturucu parametresini ve bir `Func` döndüren bir `bool`.

Aşağıdaki kodda gösterildiği nasıl bir [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) kayıt komutu temsil eder, örnek bir temsilciye belirterek yapılandırılmıştır `Register` modeli yöntemi görüntüleyin:

```csharp
public ICommand RegisterCommand => new Command(Register);
```

Komut görünüme bir başvuru döndürür bir özellik aracılığıyla kullanıma sunulan bir `ICommand`. Zaman `Execute` yöntemi çağrıldığında [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) nesnesi, onu yalnızca iletir belirtildi temsilci aracılığıyla görünüm modeli yöntemi çağrısı `Command` Oluşturucusu.

Zaman uyumsuz bir yöntem kullanarak bir komut tarafından çağrılabilir `async` ve `await` komutunun belirtirken anahtar sözcükleri `Execute` temsilci. Bu geri çağırma olduğunu belirten bir `Task` ve beklemenin. Örneğin, aşağıdaki gösterir nasıl kod bir [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) bir oturum açma komutu temsil eder, örnek bir temsilciye belirterek yapılandırılmıştır `SignInAsync` modeli yöntemi görüntüleyin:

```csharp
public ICommand SignInCommand => new Command(async () => await SignInAsync());
```

Parametreler için geçirilebilir `Execute` ve `CanExecute` kullanarak eylemleri [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) komutu örneği oluşturmak için sınıfı. Örneğin, aşağıdaki gösterir nasıl kod bir `Command<T>` belirtmek için kullanılan örnek `NavigateAsync` yöntemi türünde bir bağımsız değişken gerektirir `string`:

```csharp
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

Hem de [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) ve [ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/) sınıfları, temsilciye `CanExecute` her Oluşturucusu yöntemidir isteğe bağlıdır. Bir temsilci belirtilmezse, `Command` döndürülecek `true` için `CanExecute`. Ancak, görünüm modeli komutunun değişiklik gösterebilir `CanExecute` çağırarak durumu `ChangeCanExecute` yöntemi `Command` nesnesi. Bu neden `CanExecuteChanged` çıkarılmasına olay. Komuta bağlı UI denetimlerinde sonra etkin durumlarını verilere bağlı komutu kullanılabilirliğini yansıtacak şekilde güncelleştirilir.

#### <a name="invoking-commands-from-a-view"></a>Bir görünüm komutlarından çağırma

Aşağıdaki örnekte gösterildiği nasıl kod bir [ `Grid` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) içinde `LoginView` bağlar `RegisterCommand` içinde `LoginViewModel` sınıfı kullanarak bir [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) örneği:

```xaml
<Grid Grid.Column="1" HorizontalOptions="Center">  
    <Label Text="REGISTER" TextColor="Gray"/>  
    <Grid.GestureRecognizers>  
        <TapGestureRecognizer Command="{Binding RegisterCommand}" NumberOfTapsRequired="1" />  
    </Grid.GestureRecognizers>  
</Grid>
```

Bir komut parametresi de isteğe bağlı olarak kullanılarak tanımlanabilir [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.CommandParameter/) özelliği. Beklenen bağımsız değişken türü belirtilen `Execute` ve `CanExecute` hedef yöntemleri. [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/) Hedef komut kullanıcı ekli denetimi ile etkileşim kurduğunda otomatik olarak çağırır. Komutunun için komut parametresi verdiyse, bağımsız değişken olarak geçirilen `Execute` temsilci.

<a name="implementing_behaviors" />

### <a name="implementing-behaviors"></a>Davranışları uygulama

Davranışları alt kümesi için bunlara gerek kalmadan kullanıcı Arabirimi denetimlerini eklenecek işlevine izin verin. Bunun yerine, işlevi bir davranış sınıfta uygulanır ve denetim parçası boşmuş gibi denetime bağlı. Davranışları doğrudan yönelik olarak kısaca denetime bağlı ve olması birden çok görünüm veya uygulama için yeniden paketlenmiş olduğunu şekilde denetim API'si ile etkileşim olduğundan, normal olarak arka plan kod, yazmanız gerekir kodu uygulamak etkinleştirin. MVVM bağlamında davranışları komutları denetimleri bağlamak için yararlı bir yaklaşım ' dir.

Ekli özellikler üzerinden denetimine bağlı bir davranış olarak bilinen bir *davranışı bağlı*. Davranışı, daha sonra gösterilen API için bu denetim veya görsel ağaç görünümünün başka denetimler işlevsellik eklemek için bağlı olduğu öğesinin kullanabilirsiniz. EShopOnContainers mobil uygulamayı içeren `LineColorBehavior` ekli bir davranıştır sınıfı. Bu davranış hakkında daha fazla bilgi için bkz: [doğrulama hataları görüntüleme](~/xamarin-forms/enterprise-application-patterns/validation.md#displaying_validation_errors).

Öğesinden türeyen bir sınıf bir Xamarin.Forms davranıştır [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/) veya [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı, burada `T `davranışı uygulamalısınız denetim türü. Bu sınıfların sağlamak `OnAttachedTo` ve `OnDetachingFrom` davranışı bağlı ve ayrılmış denetimlerini yürütülür mantığı sağlamak için geçersiz kılınmalıdır yöntemleri.

EShopOnContainers mobil uygulamaya `BindableBehavior<T>` sınıfı türer [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/) sınıfı. Amacı `BindableBehavior<T>` sınıfı, bir taban sınıfı için gerektiren Xamarin.Forms davranışları sağlamak için [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) ekli denetimine ayarlanacak davranış.

`BindableBehavior<T>` SAX bir overridable `OnAttachedTo` ayarlar yöntemi [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) davranışı ve bir overridable `OnDetachingFrom` temizler yöntemi `BindingContext`. Ayrıca, ekli denetiminde başvuru sınıfı depolar `AssociatedObject` özelliği.

EShopOnContainers mobil uygulamayı içeren bir `EventToCommandBehavior` yanıt bir olayın gerçekleşmesi için bir komut yürütür sınıfı. Bu sınıfın türetildiği `BindableBehavior<T>` davranışı bağlamak ve yürütme böylece sınıfı bir `ICommand` tarafından belirtilen bir `Command` davranışı kullanıldığında özelliği. Aşağıdaki örnekte gösterildiği kod `EventToCommandBehavior` sınıfı:

```csharp
public class EventToCommandBehavior : BindableBehavior<View>  
{  
    ...  
    protected override void OnAttachedTo(View visualElement)  
    {  
        base.OnAttachedTo(visualElement);  

        var events = AssociatedObject.GetType().GetRuntimeEvents().ToArray();  
        if (events.Any())  
        {  
            _eventInfo = events.FirstOrDefault(e => e.Name == EventName);  
            if (_eventInfo == null)  
                throw new ArgumentException(string.Format(  
                        "EventToCommand: Can't find any event named '{0}' on attached type",   
                        EventName));  

            AddEventHandler(_eventInfo, AssociatedObject, OnFired);  
        }  
    }  

    protected override void OnDetachingFrom(View view)  
    {  
        if (_handler != null)  
            _eventInfo.RemoveEventHandler(AssociatedObject, _handler);  

        base.OnDetachingFrom(view);  
    }  

    private void AddEventHandler(  
            EventInfo eventInfo, object item, Action<object, EventArgs> action)  
    {  
        ...  
    }  

    private void OnFired(object sender, EventArgs eventArgs)  
    {  
        ...  
    }  
}
```

`OnAttachedTo` Ve `OnDetachingFrom` kaydolun ve içinde tanımlanan olayı için bir olay işleyicisi kaydını silmek için kullanılan yöntemleri `EventName` özelliği. Ardından, olay başlatıldığında `OnFired` yöntemi çağrıldığında, hangi komutu yürütür.

Kullanmanın avantajı `EventToCommandBehavior` bir olay başlatıldığında, bir komut çalıştırmak için olan komutları komutları ile etkileşim kurmak için tasarlanmış doğru denetimleri ile ilişkili olabilir. Ayrıca, bu olay işleme kod görünümü modelleri için burada birim test olabilir taşır.

#### <a name="invoking-behaviors-from-a-view"></a>Bir görünümden davranışları çağırma

`EventToCommandBehavior` Olan komutları desteklemiyor bir denetim için bir komut eklemek için özellikle yararlıdır. Örneğin, `ProfileView` kullanan `EventToCommandBehavior` yürütmek için `OrderDetailCommand` zaman [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) olay harekete [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) gösterildiği gibi kullanıcının siparişleri listeleyen Aşağıdaki kodda:

```xaml
<ListView>  
    <ListView.Behaviors>  
        <behaviors:EventToCommandBehavior             
            EventName="ItemTapped"  
            Command="{Binding OrderDetailCommand}"  
            EventArgsConverter="{StaticResource ItemTappedEventArgsConverter}" />  
    </ListView.Behaviors>  
    ...  
</ListView>
```

Çalışma zamanında `EventToCommandBehavior` etkileşim yanıtlar [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). İçinde bir öğe seçildiğinde `ListView`, [ `ItemTapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.ListView.ItemTapped/) olay yangın, hangi yürütecek `OrderDetailCommand` içinde `ProfileViewModel`. Varsayılan olarak, olayının olay değişkenlerini komutu geçirilir. Bu veriler, kaynak ve hedef arasında belirtilen dönüştürücü tarafından geçirilen gibi dönüştürülür `EventArgsConverter` döndürür özelliği [ `Item` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemTappedEventArgs.Item/) , `ListView` gelen [ `ItemTappedEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ItemTappedEventArgs/). Bu nedenle, `OrderDetailCommand` yürütüldüğünde, seçili `Order` parametre olarak kayıtlı eylem geçirilir.

Davranışları hakkında daha fazla bilgi için bkz: [davranışları](~/xamarin-forms/app-fundamentals/behaviors/index.md).

## <a name="summary"></a>Özet

Model-View-ViewModel (MVVM) deseni, uygulamanın kendi kullanıcı arabiriminden (UI) iş ve sunu mantığı düzgün bir şekilde ayırmak için yardımcı olur. Uygulama mantığı ve UI arasında temiz bir ayrım bakımı çok sayıda geliştirme sorunları gidermek için yardımcı olur ve bir uygulama sınamak için korumak ve gelişmesi daha kolay hale getirebilir. Kodu yeniden kullanın fırsatlar büyük ölçüde artırabilir ve geliştiricilerin sağlar ve bunların ilgili bölümleri Uygulama geliştirirken UI tasarımcıları daha kolayca işbirliği.

Desen MVVM kullanarak, uygulamanın kullanıcı arabirimini ve temel sunu ve iş mantığını üç ayrı sınıflara ayrılır: kullanıcı Arabirimi ve kullanıcı arabirimini Kapsüller görünümü mantığı; Sunum mantığı ve durumu yalıtır görünüm modeli; ve uygulamanın iş mantığı ve verileri yalıtır modeli.


## <a name="related-links"></a>İlgili bağlantılar

- [E-kitap (2 Mb PDF) indirin](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (örnek)](https://github.com/dotnet-architecture/eShopOnContainers)
