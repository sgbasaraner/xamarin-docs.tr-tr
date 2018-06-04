---
title: Android sanal cihazı özelliklerini düzenleme
description: Bu makalede Android Aygıt Yöneticisi'ni bir Android sanal cihazı profil özelliklerini düzenlemek için nasıl kullanılacağı açıklanmaktadır.
ms.prod: xamarin
ms.assetid: 3E33C136-8042-4184-A40C-3200D8CD99CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 75ac85c67825e5db1b663d00f10eee6d093bfc1f
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734044"
---
# <a name="editing-android-virtual-device-properties"></a>Android sanal cihazı özelliklerini düzenleme

_Bu makalede Android Aygıt Yöneticisi'ni bir Android sanal cihazı profil özelliklerini düzenlemek için nasıl kullanılacağı açıklanmaktadır._


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**Android Aygıt Yöneticisi'ni** tek tek Android sanal cihazı profil özelliklerini düzenlemeyi destekler. **Yeni cihaz** ve **aygıt Düzenle** ekranlar listesinde ilk sütununda, sanal cihaz özelliklerini ikinci sütundaki her bir özellik karşılık gelen değerlerle (Bu örnekte görüldüğü gibi): 

[![Örnek yeni cihaz ekranı](device-properties-images/win/01-new-device-editor-sml.png)](device-properties-images/win/01-new-device-editor.png#lightbox)

Bir özellik seçtiğinizde, bu özellik için ayrıntılı bir açıklama sağ tarafta görüntülenir. Değiştirebileceğiniz *donanım profilinin özelliklerini* ve *AVD özellikleri*. Donanım profilinin özelliklerini (gibi `hw.ramSize` ve `hw.accelerometer`) benzetilmiş aygıt fiziksel özelliklerini açıklar. Bir accelerometer mevcut olup olmadığını ekran boyutu, kullanılabilir RAM miktarını bu özellikleri içerir. Çalıştırıldığında AVD işlemi AVD özelliklerini belirtin. Örneğin, AVD özellikleri AVD geliştirme bilgisayarınızın ekran kartı işleme için nasıl kullandığını belirlemek için yapılandırılabilir.

Aşağıdaki yönergeleri kullanarak özelliklerini değiştirebilirsiniz:

-   Bir boolean özelliği değiştirmek için boolean özelliği sağındaki onay işaretine tıklayın:

    ![Bir boolean özelliği değiştirme](device-properties-images/win/02-boolean-value.png)

-   Değiştirmek için bir *enum* (numaralandırılmış) özelliği, özelliğin sağındaki aşağı oka tıklayın ve yeni bir değer seçin.

    ![Bir liste özelliğini değiştirme](device-properties-images/win/04-enum-value.png)

-   Bir string veya tamsayı özelliğini değiştirmek için değer sütununda geçerli string veya tamsayı ayarını çift tıklatın ve yeni bir değer girin.

    ![Bir tamsayı özelliği değiştirme](device-properties-images/win/03-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Mac için Visual Studio](#tab/vsmac)

**Android Aygıt Yöneticisi'ni** tek tek Android sanal cihazı profil özelliklerini düzenlemeyi destekler. **Yeni cihaz** ve **aygıt Düzenle** ekranlar listesinde ilk sütununda, sanal cihaz özelliklerini ikinci sütundaki her bir özellik karşılık gelen değerlerle (Bu örnekte görüldüğü gibi): 

[![Örnek yeni cihaz ekranı](device-properties-images/mac/01-new-device-editor-sml.png)](device-properties-images/mac/01-new-device-editor.png#lightbox)

Bir özellik seçtiğinizde, bu özellik için ayrıntılı bir açıklama sağ tarafta görüntülenir. Değiştirebileceğiniz *donanım profilinin özelliklerini* ve *AVD özellikleri*. Donanım profilinin özelliklerini (gibi `hw.ramSize` ve `hw.accelerometer`) benzetilmiş aygıt fiziksel özelliklerini açıklar. Bir accelerometer mevcut olup olmadığını ekran boyutu, kullanılabilir RAM miktarını bu özellikleri içerir. Çalıştırıldığında AVD işlemi AVD özelliklerini belirtin. Örneğin, AVD özellikleri AVD geliştirme bilgisayarınızın ekran kartı işleme için nasıl kullandığını belirlemek için yapılandırılabilir.

Aşağıdaki yönergeleri kullanarak özelliklerini değiştirebilirsiniz:

-   Bir boolean özelliği değiştirmek için boolean özelliği sağındaki onay işaretine tıklayın:

    ![Bir boolean özelliği değiştirme](device-properties-images/mac/02-boolean-value.png)

-   Değiştirmek için bir *enum* (numaralandırılmış) özelliği, özellik sağındaki açılan menüsüne tıklayın ve yeni bir değer seçin.

    ![Bir liste özelliğini değiştirme](device-properties-images/mac/04-enum-value.png)

-   Bir string veya tamsayı özelliğini değiştirmek için değer sütununda geçerli string veya tamsayı ayarını çift tıklatın ve yeni bir değer girin.

    ![Bir tamsayı özelliği değiştirme](device-properties-images/mac/03-integer-value.png)

-----

Aşağıdaki tabloda listelenen özellikleri ayrıntılı bir açıklamasını sağlar **yeni cihaz** ve **aygıt Düzenleyicisi** ekranlar:

[!include[](~/android/includes/emulator-properties.md)]

Bu özellikler hakkında daha fazla bilgi için bkz: [donanım profilinin özelliklerini](https://developer.android.com/studio/run/managing-avds.html#hpproperties).

