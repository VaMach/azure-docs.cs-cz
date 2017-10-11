---
title: "Koupit vlastní název domény pro Azure Web Apps"
description: "Zjistěte, jak koupit vlastní doménu s webovou aplikaci v Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 3cb22b935624041ab51e64028a1b668fd694f9b5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Koupit vlastní název domény pro Azure Web Apps

Domény nejvyšší úrovně, které jsou spravovány přímo v Azure jsou domény služby App Service (preview). Se snadno spravovat vlastní domény pro [Azure Web Apps](app-service-web-overview.md). V tomto kurzu se dozvíte, jak zakoupit domény služby App Service a názvy DNS přiřadit Azure Web Apps.

Tento článek je pro službu Azure App Service (webové aplikace, aplikace API, mobilní aplikace, Logic Apps). Virtuální počítač Azure nebo Azure Storage, najdete v části [domény přiřadit aplikační služby na virtuálním počítači Azure nebo Azure Storage](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Cloudové služby, najdete v části [konfigurace vlastního názvu domény pro cloudové služby Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* [Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.

## <a name="prepare-the-app"></a>Příprava aplikace

Použít vlastní domény ve službě Azure Web Apps, webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo  **Premium**). V tomto kroku je třeba zkontrolovat, že webová aplikace je v podporovaném cenová úroveň.

### <a name="sign-in-to-azure"></a>Přihlášení k Azure

Otevřete [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Přejděte na aplikaci na portálu Azure

V nabídce vlevo vyberte **App Services**a pak vyberte název aplikace.

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-domain/select-app.png)

Zobrazí na stránce Správa aplikace služby App Service.  

### <a name="check-the-pricing-tier"></a>Zkontrolujte cenové úrovně

V levém navigačním panelu stránku aplikace, přejděte na **nastavení** a vyberte **škálování (plán služby App Service)**.

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Aktuální úroveň aplikace je označený modré ohraničení. Zkontrolujte, zda není v aplikaci **volné** vrstvy. Vlastní DNS není podporována v **volné** vrstvy. 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Pokud plán služby App Service není **volné**ukončit **zvolte cenovou úroveň** stránky a přejít na [koupit domény](#buy-the-domain).

### <a name="scale-up-the-app-service-plan"></a>Škálovat plán služby App Service

Vyberte některé z vrstvy a bezplatnou (**sdílené**, **základní**, **standardní**, nebo **Premium**). 

Klikněte na **Vybrat**.

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Když se zobrazí následující oznámení, je dokončena operace škálování.

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Kupte si domény

### <a name="sign-in-to-azure"></a>Přihlášení k Azure
Otevřete [portál Azure](https://portal.azure.com/) a přihlaste se pomocí účtu Azure.

### <a name="launch-buy-domains"></a>Spusťte koupit domén
V **webové aplikace** , klikněte na název vaší webové aplikaci, vyberte **nastavení**a potom vyberte **vlastní domény**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

V **vlastní domény** klikněte na tlačítko **koupit domény**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-the-domain-purchase"></a>Konfigurace domény nákupu

V **aplikace služby domény** stránky v **hledat domény** pole, zadejte název domény, které chcete koupit a zadejte `Enter`. Navržené domény k dispozici se zobrazí pod textové pole. Vyberte jednu nebo více domén, které chcete koupit. 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

Klikněte **kontaktní údaje** a vyplňte formulář domény kontaktní informace. Po dokončení klikněte na tlačítko **OK** vrátit na stránku služby domény aplikace.
   
Je důležité, vyplňte všechna povinná pole s tolik přesnost míře. Nesprávná data kontaktní informace může způsobit selhání přikoupení domén. 

Potom vyberte požadované možnosti pro vaši doménu. V následující tabulce najdete vysvětlení:

| Nastavení | Navrhovaná hodnota | Popis |
|-|-|-|
|Automatické obnovení | **Povolení** | Obnovuje doménu služby aplikace automaticky každý rok. Platební karty je účtován za stejnou cenu nákupu v době obnovení. |
|Ochrana osobních údajů | Povolení | Vyjádřit výslovný souhlas pro "Ochrany osobních údajů", který je součástí kupní ceny _zdarma_ (s výjimkou domény nejvyšší úrovně, jejichž registru nepodporují ochrany osobních údajů, jako například _. co.in_, _. co.uk_ a tak dále). |
| Přiřadit výchozí názvy hostitelů | **Webová** a**@** | Vazby požadovaným názvem hostitele, vyberte v případě potřeby. Po dokončení nákupu operace domény vaší webové aplikace jsou přístupné na vybrané názvy hostitelů. Pokud webová aplikace je za [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), nevidíte možnost k přidělení kořenové domény (@), protože nemá podporu záznamů A Traffic Manager. Vám po dokončení nákupu domény provádět změny přiřazení názvu hostitele. |

### <a name="accept-terms-and-purchase"></a>Přijmout podmínky a nákupu

Klikněte na tlačítko **právní podmínky** přečtěte si podmínky a že poplatky a pak klikněte na **koupit**.

> [!NOTE]
> Aplikační služby domény použít Azure DNS k hostování domény. Kromě poplatek za zápis domain poplatky za používání pro Azure DNS použít. Informace najdete v tématu [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).
>
>

Zpět v **aplikace služby domény** klikněte na tlačítko **OK**. Když probíhá operace, zobrazí se následující oznámení:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>Testování názvy hostitelů

Pokud jste přiřadili výchozí názvy hostitelů do vaší webové aplikace, je také zobrazit upozornění na úspěch pro každý vybraný název hostitele. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Zobrazí vybraný názvy hostitelů v **vlastní domény** stránky v **názvy hostitelů** části. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

Chcete-li otestovat názvy hostitelů, přejděte na uvedené názvy hostitelů v prohlížeči. V příkladu v předchozí snímek obrazovky, zkuste přejdete na _kontoso.net_ a _www.kontoso.net_.

## <a name="assign-hostnames-to-web-app"></a>Přiřadit názvy hostitelů do webové aplikace

Pokud se rozhodnete přiřadit jeden nebo více názvy hostitelů výchozí webové aplikace během procesu nákupu, nebo pokud je třeba přiřadit název hostitele není uvedený, můžete kdykoli přiřadit název hostitele v.

Názvy hostitelů v doméně služby aplikaci můžete přiřadit také do jiné webové aplikace. Postup závisí na tom, jestli domény služby, aplikace a webové aplikace patří do stejného předplatného.

- Jiné předplatné: namapovat vlastní záznamy DNS od domény služby aplikace do webové aplikace jako externě zakoupené domény. Informace o přidání vlastní názvy DNS do domény služby App Service najdete v tématu [vlastní záznamy DNS spravovat](#custom). Mapování externí zakoupené domény do webové aplikace naleznete v tématu [mapovat existující vlastní název DNS pro službu Azure Web Apps](app-service-web-tutorial-custom-domain.md). 
- Stejného předplatného: použijte následující postup.

### <a name="launch-add-hostname"></a>Spuštění přidat název hostitele
V **App Services** vyberte název vaší webové aplikace, kterou chcete přiřadit názvů hostitelů, vyberte **nastavení**a potom vyberte **vlastní domény**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Ujistěte se, že zakoupené domény, je uvedena ve **doménami aplikací služby** tématu, ale nezaškrtnete políčko. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Všechny aplikace služby domény v rámci stejného předplatného jsou uvedeny ve webové aplikaci **vlastní domény** stránky. Pokud vaše doména je v rámci předplatného webové aplikace, ale nejde ji zobrazit ve webové aplikaci **vlastní domény** stránky, zkuste to znovu **vlastní domény** stránky nebo obnovení webové stránky. Zkontrolujte také, oznámení zvonku v horní části portálu Azure průběh nebo vytvoření chyby.
>
>

Vyberte **přidat název hostitele**.

### <a name="configure-hostname"></a>Nakonfigurujte název hostitele
V **přidat název hostitele** dialogové okno, zadejte plně kvalifikovaný název domény vaší domény služby aplikace nebo jakékoli subdomény. Například:

- kontoso.NET
- www.kontoso.NET
- ABC.kontoso.NET

Po dokončení vyberte **ověřením**. Typ záznamu název hostitele je automaticky vybrána pro vás.

Vyberte **přidat název hostitele**.

Po dokončení operace se zobrazí upozornění na úspěch přiřazené název hostitele.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Zavřete přidat název hostitele
V **přidat název hostitele** přiřaďte jiné název hostitele do webové aplikace, podle potřeby. Po dokončení zavřete **přidat název hostitele** stránky.

Teď byste měli vidět nově přiřazené hostname(s) ve vaší aplikaci **vlastní domény** stránky.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>Testování názvy hostitelů

Přejděte do seznamu názvy hostitelů v prohlížeči. V příkladu v předchozí snímek obrazovky, zkuste přejdete na _abc.kontoso.net_.

<a name="custom" />

## <a name="manage-custom-dns-records"></a>Spravovat vlastní záznamy DNS

V Azure, záznamy DNS pro doménu služby App Service se spravují pomocí [Azure DNS](https://azure.microsoft.com/services/dns/). Můžete přidat, odebrat a aktualizovat záznamy DNS, stejně jako pro externě zakoupené doménu.

### <a name="open-app-service-domain"></a>Otevřete aplikaci služby domény

Na portálu Azure v levé nabídce vyberte **více služeb** > **doménami aplikací služby**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Vyberte doménu pro správu. 

### <a name="access-dns-zone"></a>Zóna DNS přístup

V levé nabídce domény, vyberte **zónu DNS**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Tato akce otevře [zónu DNS](../dns/dns-zones-records.md) stránku vaší domény služby aplikace v Azure DNS. Informace o tom, jak upravit záznamy DNS najdete v tématu [správa zóny DNS na webu Azure portal](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Zrušit nákup (odstranit domény)

Po zakoupení služby doména aplikace máte zrušit nákup pro vrátit celou částku pět dní. Po pět dní můžete odstranit doménu služby aplikace ale nemůže přijímat náhrada.

### <a name="open-app-service-domain"></a>Otevřete aplikaci služby domény

Na portálu Azure v levé nabídce vyberte **více služeb** > **doménami aplikací služby**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Vyberte domény, ke které chcete zrušit nebo odstranit. 

### <a name="delete-hostname-bindings"></a>Odstranit vazby názvů hostitelů.

V levé nabídce domény, vyberte **vazby názvů hostitelů**. Vazby názvů hostitelů ze všech služeb Azure jsou zde uvedeny.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Doména aplikace služby nelze odstranit, dokud nejsou odstraněny všechny vazby názvů hostitelů.

Odstranit vazbu každý název hostitele tak, že vyberete **...**   >  **Odstranit**. Po odstranění se všechny vazby, vyberte **Uložit**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Zrušit nebo odstranění

V levé nabídce domény, vyberte **přehled**. 

Pokud nebyl uplynutí období zrušení na zakoupené doménu, vyberte **zrušit nákup**. Jinak se zobrazí **odstranit** tlačítko místo. Chcete-li odstranit doménu bez náhrady, vyberte **odstranit**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Vyberte **OK** k potvrzení operace. Pokud nechcete, aby bylo možné pokračovat, klikněte kamkoli mimo dialogové okno pro potvrzení.

Po dokončení operace doména je vydaná ze svého předplatného a k dispozici pro každý, kdo k nákupu znovu. 
