---
title: Touch ID
description: Touch ID Apple'nın Biyometrik parmak izi kimlik doğrulama teknolojisidir.
ms.prod: xamarin
ms.assetid: 4BC8EFD6-52FC-4793-BA69-D6BFF850FE5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 6ec46a5e098ba14925102211a27fcce8c27970e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2018
---
# <a name="touch-id"></a>Touch ID

Touch ID iOS 7 kullanıcı - bir geçiş kodu benzer kimlik doğrulaması için bir araç olarak sunulmuştur. Ancak, cihazın kilidini, uygulama mağazası kullanarak, kullanarak iTunes ve iCloud anahtarlık yalnızca kimlik doğrulaması sınırlıdır.

Şimdi yerel kimlik doğrulaması API'sini kullanarak bir iOS 8 uygulamasında kimlik doğrulama mekanizması olarak Touch ID kullanmanın iki yolu vardır. Şu anda yerel kimlik doğrulaması uzaktan kimlik doğrulaması için kullanmak mümkün değil.

Touch ID ve onun eşitleyeceğini tam olarak anlamak için şu Anahtarlık hizmetlerini keşfedin ve bu yeni değişiklikler için kullanıcının verileri anlamı. Anahtarlık erişimi de üzerine yeni erişim denetim listeleri (ACL'ler) özelliğini kullanarak iOS 8 içinde genişletilmiştir.

## <a name="keychain--secure-enclave"></a>Anahtarlık & güvenli Enclave

Anahtarlık tek tek bir Apple kimliği için parolalar, anahtarlar, sertifikalar ve notlar için güvenli depolama sağlama büyük bir veritabanı değil İOS 8 uygulama her zaman kendi benzersiz Anahtarlık öğelerine erişimi vardır ve diğer uygulamalarla Anahtarlıkta öğeleri erişemiyor. Bu işletim sistemi Anahtarlık tek bir parola ile kilidi olduğu X Anahtarlık kullanmak Anahtarlık hizmetleri kullanan bir uygulaması izin vererek farklıdır. Bu makalede Anahtarlık iOS 8 nasıl çalıştığını odaklanır.

Anahtarlık burada her satır bilinen olarak özel bir veritabanı olduğundan bir _Anahtarlık öğesi_. Her öğe Anahtarlık özniteliklerle tanımlanan ve şifrelenmiş değerlerden oluşur. Anahtarlık verimli şekilde kullanılmasına izin vermek için onu küçük öğeleri için optimize edilmiştir veya _gizli_.
Her Anahtarlık öğesi, kullanıcı geçiş kodunu ve benzersiz cihaz gizlilik tarafından korunur. Anahtarlık öğeleri bile kullanıcılar cihazlarını kullanmadığınızda korunmalıdır. Cihaz kilitli olduğunda kullanılabilir hale gelmesi öğeleri yalnızca vererek bu iOS uygulanır: cihaz kilitli olduğunda kullanılamaz duruma. Şifrelenmiş bir yedeklemeye da depolanabilir. Anahtar özelliklerini Anahtarlık erişim denetimi zorlamak için biridir; uygulamanın kendi kısmı Anahtarlık erişimi ve diğer tüm uygulamaların engellenir. Aşağıdaki diyagram, bir uygulaması Anahtarlık ile nasıl etkileşim kurduğunu gösterir:

[![](touchid-images/image1.png "Bu diyagramda uygulama Anahtarlık ile nasıl etkileşim kurduğu gösterilmektedir")](touchid-images/image1.png#lightbox)

### <a name="secure-enclave"></a>Güvenli Enclave

Anahtarlık Anahtarlık öğesi kendisi tarafından şifresi çözülemiyor; Bunun yerine, içinde yapılır *güvenli Enclave*. Güvenli enclave kayıtlı yazdırmayı karşı Touch ID algılayıcı parmak izi verilerden başarılı bir eşleşme belirlemek için sorumlu A7 yonga içinde ortak bir işlemci ' dir. Ardından Anahtarlık öğesi şifresini çözmek ve şifresi çözülmüş gizli Anahtarlığa dönüş.

### <a name="working-with-keychain"></a>Anahtarlık ile çalışma

İlk uygulamanızı bir parola olup olmadığını Anahtarlık sorgu. Yoksa, kullanıcı sürekli sorulan değil için bir parola sor gerekebilir. Parola güncelleştirilmesi gerekiyorsa, yeni bir parola kullanıcıdan ve Anahtarlığa güncelleştirilmiş değeri geçirin.

> [!NOTE]
> Bir gizli anahtarı kullanarak Anahtarlık aldıktan sonra verileri yapılan tüm başvuruları bellekten temizlenmelidir. Hiçbir zaman genel değişkenine atayın.

## <a name="keychain-acl-and-touch-id"></a>Anahtarlık ACL ve dokunma kimliği

Erişim denetim listesi, gerçekleşmesi için belirli bir işlemine izin vermek için ne gerekir ilgili bilgiler açıklanmaktadır iOS 8 içinde yeni bir Anahtarlık öğesi özniteliktir. Bu uyarı iletişim kutusu görüntüleme veya bir geçiş kodu isteme biçiminde olabilir. ACL erişilebilirlik ve kimlik doğrulaması için bir Anahtarlık öğesi ayarlamanıza olanak sağlar. Aşağıdaki diyagram, bu yeni öznitelik Anahtarlık öğenin geri kalanını oturum nasıl bağlar, gösterir:

[![](touchid-images/image2.png "Bu yeni öznitelik Anahtarlık öğenin geri kalanını oturum nasıl bağlar, bu diyagramda gösterilmektedir")](touchid-images/image2.png#lightbox)

İOS 8 itibariyle var. Şimdi yeni bir kullanıcı varlığı İlkesi `SecAccessControl`, güvenli enclave bir iPhone 5s'dir ve üstü tarafından zorlanır. Aşağıda, yalnızca cihaz yapılandırma İlkesi değerlendirme nasıl etkileyebilir tablosundaki görebilirsiniz:

|Aygıt Yapılandırması|İlke değerlendirmesi|Yedekleme mekanizması|
|--- |--- |--- |
|Cihaz geçiş kodu olmadan|Erişim yok|Yok.|
|Cihaz geçiş kodu|Geçiş kodu gerektirir|Yok.|
|Dokunma Kimliğine sahip cihazı|Touch ID tercih eder|Geçiş kodu sağlar|

Güvenli Enclave içindeki tüm işlemleri birbirine güvenebilir. Bu, biz Anahtarlık öğesi şifre çözme yetkilendirmek için Touch ID kimlik doğrulaması sonucu kullanabileceğiniz anlamına gelir. Güvenli Enclave da geçiş kodunu kullanmaya geri durumda kullanıcı gerekir başarısız Touch ID eşleşen bir sayaç tutar.
Adlı yeni bir çerçeve iOS 8 ' de _yerel kimlik doğrulaması_, kimlik doğrulama cihaz içinde bu işlemi destekler. Biz bu sonraki bölümde inceleyeceksiniz.

## <a name="local-authentication"></a>Yerel kimlik doğrulaması

Biz önceki bölümde belirlenen uygulamalar kullanıcı cihazda ayarlanmış güvenlik ilkesiyle uygun olarak kimlik doğrulaması için yerel kimlik doğrulaması kullanabilirsiniz.

Şu anda, API yalnızca iki yetenekleri sağlar: ilk olarak, yeni Anahtarlık erişim denetim listeleri (ACL'ler) kullanarak mevcut Anahtarlık Hizmetleri yardımcı olur. Anahtarlık veri kullanıcılar parmak izi başarılı kimlik doğrulaması ile kilidi açılabilir.

İkincisi, LocalAuthentication uygulamanızı yerel olarak kimlik doğrulaması yapmak için iki yöntem sunar. Geliştiriciler kullanması gereken `CanEvaluatePolicy` cihaz Touch ID kabul edebilen olup olmadığını belirlemek için ve ardından `EvaluatePolicy` kimlik doğrulama işlemini başlatmak için.

Her iki özelliği yerel kimlik doğrulaması sunarken, uygulama veya kullanıcı uzak bir sunucuya kimlik doğrulaması bir mekanizma sağlamaz.
Yerel kimlik doğrulaması kimlik doğrulaması için yeni bir standart kullanıcı arabirimi sağlar. Touch ID söz konusu olduğunda bir uyarı görünümü aşağıda gösterildiği gibi iki düğmelerle budur. Bir düğme iptal et ve bir geri dönüş kimlik doğrulaması – geçiş kodunu çeşit kullanın. Ayarlanmalıdır özel bir ileti yoktur. Bu neden Touch ID kimlik doğrulama gerekli olduğunu kullanıcıya açıklamak için kullanmak iyi bir uygulamadır.

[![](touchid-images/image12.png "Touch ID kimlik doğrulama uyarısı")](touchid-images/image12.png#lightbox)

### <a name="with-keychain-services"></a>Anahtarlık hizmetleriyle

Biz biraz daha önce bir Anahtarlık öğesi nasıl olduğuna Aranan şifresi, bizim geçiş kodu doğrulamak için güvenli enclave kullanarak. İOS 8'de, biz Touch ID doğrulama geri dönüş mekanizması ya da parola uygulamasını sağlar erişim denetim listeleri özelliği ile birlikte istemek için yerel kimlik doğrulaması kullanabilirsiniz.
Size kullanıyor ACL kullanılacak `SecAccessControl` İlkesi ve kullanarak aygıtın durumunu denetleme `SecAccessible.WhenPasscodeSetThisDeviceOnly` veya `SecAccessible.WhenUnlocked`.

#### <a name="considerations-with-acl"></a>ACL ile ilgili önemli noktalar

Biz ACL ile Anahtarlık kullanırken göz önünde bulundurmanız birçok şey vardır ve bunlardan bazıları aşağıda listelenmiştir:

-   Çağrı başarısız olur bir arka plan iş parçacığı üzerinde herhangi bir Anahtarlık işlemi çağırırsanız yalnızca ön plan uygulamayla – kullanın.
-   Kimlik doğrulaması ekleme ve Anahtarlık öğeleri güncelleştirme gerektirebilir.
-   Bir istek Anahtarlıkta birden çok eşleşen öğe döndürürse, kimlik doğrulaması gerekli olabilir.
-   ACL korunan öğeler salt cihaz ve bu nedenle değil eşitlenmiş veya yedeklenmiş olan.

### <a name="using-local-authentication-without-keychain-services"></a>Anahtarlık Hizmetleri olmadan yerel kimlik doğrulaması kullanma

Yerel kimlik doğrulaması, geçiş kodu veya Touch ID gibi bilgilerini toplamak ve güvenli kullanıcı kimlik doğrulaması tamamlanması Enclave ile çalışmak için bir yol olarak oluşturuldu. Bunu, uygulamanız ile güvenli hangi asla doğrudan birbiriyle iletişim kurabildiği Enclave, arasında bir köprü olarak düşünün. Ayrıca, uygulamanız için ilke değerlendirmesi için de kullanılabilir.

Bir uygulamanın bunu yapmak için yerel güvenli Enclave iç işlemi başlatan kimlik doğrulaması, iç ilke değerlendirmesi çağırır. Bu doğrudan sorgulama/güvenli Enclave erişmeden uygulamanıza kimlik doğrulaması sağlamak için yararlanabilirsiniz.

[![](touchid-images/image13a.png "Anahtarlık Hizmetleri olmadan yerel kimlik doğrulaması kullanma")](touchid-images/image13a.png#lightbox)

Uygulamanızda yerel kimlik doğrulaması kullanarak kullanıcı doğrulama, örneğin uygulamalar bankacılık gibi ya da ebeveyn denetimleri yardımcı olmak için cihaz sahibinin gözler için yalnızca bir özellik için tek tek kilidini açmak uygulama basit bir yol sağlar uygulama. Sizin de zaten var. kimlik doğrulaması genişletmek için bir yol kullanabilirsiniz – kullanıcılar kendi bilgilerini güvenli olacak şekilde benzer, ama bunlar ayrıca seçeneğiniz ister.

Yerel kimlik doğrulaması güvenliğini Anahtarlık farklı. Örneğin, Anahtarlık kullanılırken, işletim sistemi ve güvenli Enclave arasında güven olması. Yerel kimlik doğrulaması ile bu uygulama ve yalnızca güvenli Enclave değil güvenli Enclave kendisini sonuçlarını erişimi gerektiği anlamına gelir işletim sistemi arasındadır.

Güvenlik konuyla ilgili da olduğunu bilmek son derece önemlidir **erişim yok** kayıtlı parmakları ya da parmak izi görüntüler. Güvenli Enclave bu bilgi sahibi ve bu nedenle başka bir sistem bileşeninin ona erişimi vardır.

Yerel kimlik doğrulama API yararlanarak Anahtarlık Touch ID kullanmak için biz kullanabileceğiniz birkaç işlevleri vardır. Bunlar, aşağıda açıklanmıştır:

*   `CanEvaluatePolicy` – Bu yalnızca cihaz Touch ID kabul edebilen olup olmadığını kontrol eder
*   `EvaluatePolicy` – Bu kimlik doğrulama işlemini başlatır ve kullanıcı arabirimini görüntüler ve döndüren bir `true` veya `false` yanıt.
*   `DeviceOwnerAuthenticationWithBiometrics` – Bu Touch ID ekran göstermek için kullanılan ilkesidir. Hiçbir geçiş kodu geri dönüş mekanizması Burada, bunun yerine, uygulamanızda Touch ID kimlik doğrulamayı Atla yapmalarına izin vermek için bu geri dönüş uygulamalıdır'ı eşitlenmeyeceği olur.

Yerel, aşağıda listelenen kimlik doğrulamasını kullanarak birkaç uyarılar vardır:

*   Anahtarlık gibi ile yalnızca ön planda çalıştırabilirsiniz. Arka plan iş parçacığında çağırma başarısız olmasına neden olur.
*   İlke değerlendirmesi başarısız olabilir göz önünde bulundurun. Bir geçiş kodu düğmesi bir sıfırlamaya uygulanması gerekir.
*   Sağlamanız gerekir bir `localizedReason` neden kimlik doğrulaması gerekli açıklamak için. Bu kullanıcıyla güven oluşturmaya yardımcı olur.

Ardından, aşağıdaki bölümde, bu uyarılar göz önünde bulundurarak API uygulamak nasıl ele alacağız.

## <a name="adding-touch-id-to-your-application"></a>Uygulamanız için Touch ID ekleme

Önceki bölümlerde erişim ve Anahtarlık ve yerel kimlik doğrulamasını kullanarak kimlik doğrulaması arkasındaki teorik inceledik. Biz şimdi uygulamanıza Touch ID nasıl tümleştirebilir bir göz atalım.

### <a name="walkthrough"></a>İzlenecek yol

Bu nedenle uygulamamız için bazı Touch ID kimlik doğrulama ekleme konumundaki bakalım. Bu kılavuzda biz kullanacaksanız [film şeridi tablo](https://developer.xamarin.com/samples/StoryboardTable/) örnek. Cihaz sahibi bir şey bu listeye eklemek için yalnızca, herhangi bir öğe ekleyin izin vererek dağıtmayı istediğimizi yok emin olmak istiyoruz!

1.  Örneği indirmek ve Mac için Visual Studio'da çalıştırın
2.  Çift tıklatın `MainStoryboard.Storyboard` iOS Tasarımcısı örneği açın. Bu örnek ile kimlik doğrulaması denetleyecek uygulamamız için yeni bir ekran eklemek istiyoruz. Bu önce geçerli gidecek `MasterViewController`.
3.  Yeni bir sürükleyin **View Controller** gelen **araç** için **tasarım yüzeyi**. Bu olarak ayarlamak **kök View Controller** tarafından **Ctrl + Sürükle** gelen **Gezinti denetleyicisi**:

    [![](touchid-images/image4.png "Kök görünüm denetleyicisini ayarlama")](touchid-images/image4.png#lightbox)
4.  Yeni Görünüm denetleyicisini `AuthenticationViewController`.
5.  Ardından, bir düğme sürükleyin ve bunu Yerleştir `AuthenticationViewController`. Bu çağrı `AuthenticateButton`ve metin verin `Add a Chore`.
6.  Bir olay oluşturma `AuthenticateButton` adlı `AuthenticateMe`.
7.  El ile oluşturmak gelen ü `AuthenticationViewController` altındaki siyah bir çubuk tıklayarak ve **Ctrl + Sürükle** çubuğundan `MasterViewController` ve seçme **itme** (veya **Göster** boyutu sınıfları kullanıyorsanız):

    [![](touchid-images/image5.png "MasterViewController ve anında iletme seçme çubuğundan sürükleyin ya Göster")](touchid-images/image6.png#lightbox)
8.  Tıklayın yeni oluşturulan ü ve tanımlayıcı verin `AuthenticationSegue`aşağıda gösterildiği gibi:

    [![](touchid-images/image7.png "Kümesi için AuthenticationSegue segue tanımlayıcısı")](touchid-images/image7.png#lightbox)
9.  Aşağıdaki kodu ekleyin `AuthenticationViewController`:

    ```
    partial void AuthenticateMe (UIButton sender)
        {
            var context = new LAContext();
            NSError AuthError;
            var myReason = new NSString("To add a new chore");


            if (context.CanEvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, out AuthError)){
                var replyHandler = new LAContextReplyHandler((success, error) => {

                    this.InvokeOnMainThread(()=>{
                        if(success){
                            Console.WriteLine("You logged in!");
                            PerformSegue("AuthenticationSegue", this);
                        }
                        else{
                            //Show fallback mechanism here
                        }
                    });

                });
                context.EvaluatePolicy(LAPolicy.DeviceOwnerAuthenticationWithBiometrics, myReason, replyHandler);
            };
        }
    ```

Bu yerel kimlik doğrulaması kullanarak Touch ID kimlik doğrulama uygulamak için gereken tüm koddur. Aşağıdaki görüntü vurgulanan satırları yerel kimlik doğrulaması kullanımını göster:

[![](touchid-images/image8.png "Yerel kimlik doğrulaması kullanımını vurgulanan satırları göster")](touchid-images/image8.png#lightbox)

İlk olarak, aygıt Touch kullanarak giriş ID kabul edebilen olup olmadığını kurmak ihtiyacımız `CanEvaluatePolicy` ve ilkede geçirme `DeviceOwnerAuthenticationWithBiometrics`. Bu durum geçerlidir sonra kullanarak biz Touch ID UI görüntüleyebilirsiniz `EvaluatePolicy`. Üç parça bilgi uygulamasına geçirmek için sahip olduğumuz vardır `EvaluatePolicy` – ilke, kimlik doğrulama neden gerekli olduğunu belirten bir dize ve bir yanıt işleyici. Yanıt işleyici uygulama başarılı veya başarısız kimlik doğrulaması durumunda ne yapmanız gerektiğini bildirir. Yanıt işleyici yakın olarak inceleyelim:

[![](touchid-images/image9.png "Yanıt işleyicisi")](touchid-images/image9.png#lightbox)

Yanıt işleyici türü belirtildiğinde `LAContextReplyHandler`, parametreleri başarılı – aldığı bir `bool` değeri ve bir `NSError` adlı `error`. Başarılı olursa, burada gerçekten ne olursa olsun, kimlik doğrulaması – istiyoruz olmasından gerçekleştiririz, bu durumda yeni bir işi bize ekleyeceksiniz ekran görüntüleme budur. Yerel kimlik doğrulaması uyarılar biri bu olmalıdır, ön planda çalıştırmak, bu nedenle kullandığınızdan emin olun unutmayın `InvokeOnMainThread`:

[![](touchid-images/image10.png "Yerel kimlik doğrulaması için InvokeOnMainThread kullanın")](touchid-images/image10.png#lightbox)

Kimlik doğrulaması başarılı oldu, son olarak, geçiş istiyoruz `MasterViewController`. `PerformSegue` Yöntemi, bunu yapmak için kullanılabilir:

[![](touchid-images/image11.png "Geçiş için MasterViewController PerformSegue yöntemini çağırın")](touchid-images/image11.png#lightbox)

## <a name="summary"></a>Özet
Bu kılavuzda Anahtarlık ve iOS nasıl işlediğine inceledik. Biz de ACL, Anahtarlık incelediniz ve iOS bu değişiklikleri. Ardından, iOS 8 yenidir ve bizim uygulamada Touch ID kimlik doğrulama uygulanmasına Aranan yerel kimlik doğrulaması framework göz sürdü.

## <a name="related-links"></a>İlgili bağlantılar

- [Dokunma kimliği örneği](https://developer.xamarin.com/samples/StoryboardTable_LocalAuthentication)
- [Anahtarlık WWDC örnek](https://developer.xamarin.com/samples/KeychainTouchID/)
- [Anahtarlık (örnek)](https://developer.xamarin.com/samples/Keychain/)
