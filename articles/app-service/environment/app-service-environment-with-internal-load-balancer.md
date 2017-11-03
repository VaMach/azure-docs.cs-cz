---
title: "Vytváření a používání interní pro vyrovnávání zatížení s služby App Service Environment | Microsoft Docs"
description: "Vytvoření a použití s ILB App Service Environment"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: c0f942cb11edc6d6212914b3134e25c3fbc3d3ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Interní nástroj pomocí služby App Service Environment

> [!NOTE] 
> Tento článek je o v1 App Service Environment. Existuje novější verze App Service Environment, který je jednodušší použít a běží na výkonnější infrastruktury. Další informace o nové verzi spuštění s [Úvod do služby App Service Environment](intro.md).
>

Funkci prostředí App Service (App Service Environment) je možnost Premium služby Azure App Service, který doručí funkci rozšířené konfigurace, která není k dispozici v víceklientské razítka. Funkci App Service Environment v podstatě nasadí Azure App Service ve vaší virtuální Network(VNet) Azure. K získání lépe porozumět funkce nabízené číst prostředí App Service [novinky služby App Service Environment] [ WhatisASE] dokumentaci. Pokud nevíte, výhod pracuje v čtení virtuální sítě [Azure virtuální sítě, – nejčastější dotazy][virtualnetwork]. 

## <a name="overview"></a>Přehled
App Service Environment se dá nasadit pomocí Internetu přístupném koncovém bodu nebo IP adresu ve vaší virtuální síti. Chcete-li nastavit adresu IP adresu virtuální sítě je třeba nasadit vaší App Service Environment se vnitřní Balancer(ILB) zatížení. Pokud vaše App Service Environment je konfigurován s ILB zadáte:

* vaše vlastní doména nebo subdoména. Chcete-li snadno, tento dokument předpokládá subdomény, ale můžete ji nakonfigurovat v obou případech. 
* certifikát použitý pro protokol HTTPS
* Správa služby DNS pro vaše subdomény. 

Naopak může provádět akce, jako:

* aplikace v síti intranet hostitele, jako je obchodních aplikací, bezpečně v cloudu, které je k dispozici prostřednictvím webu k webu nebo ExpressRoute VPN
* aplikace pro hostitele v cloudu, které nejsou uvedené v veřejné servery DNS
* Vytvoření internet izolované back-end aplikace, které vaše aplikace front-endu může bezpečně integrovat

#### <a name="disabled-functionality"></a>Zakázané funkce
Existují některé věci, které nejde dělat, když pomocí ILB App Service Environment. Zahrnout všechny tyto věci:

* pomocí IPSSL
* přiřazení IP adres na konkrétní aplikace
* nakupovat a certifikát pomocí aplikace prostřednictvím portálu. Samozřejmě stále můžete získat certifikáty přímo s certifikační autority a jeho použití s aplikací, ne jenom prostřednictvím portálu Azure.

## <a name="creating-an-ilb-ase"></a>Vytváření ILB App Service Environment
Vytváření ILB App Service Environment není výrazně liší od vytvoření App Service Environment normálně. Podrobnější informace o vytvoření App Service Environment číst [postup vytvoření služby App Service Environment][HowtoCreateASE]. Proces vytvoření App Service Environment ILB je stejný mezi vytvoření virtuální sítě během vytváření App Service Environment nebo vyberte existující virtuální síť. Chcete-li vytvořit ILB App Service Environment: 

1. V portálu Azure vyberte **nový -> Web + mobilní zařízení -> Služba App Service Environment**
2. Vyberte předplatné
3. Vyberte nebo vytvořte skupinu prostředků.
4. Vyberte nebo vytvořte virtuální síť
5. Pokud vyberete virtuální síť vytvořit podsíť
6. Vyberte **virtuální sítě nebo umístění-> Konfigurace virtuální sítě** a nastavte typ VIP na interní
7. Zadejte název subdomény (to se stane subdoméně používá pro aplikace vytvořené v této App Service Environment)
8. Kliknutím na tlačítko Ok a poté vytvořit

![][1]

V rámci okna virtuální sítě existuje možnost konfigurace virtuální sítě. Díky tomu se můžete vybrat, jestli chcete externí virtuální IP adresy nebo interní virtuální IP adresy. Výchozí hodnota je externí. Pokud máte nastavené na externí vaší App Service Environment bude používat Internetu dostupné virtuální IP adresy. Pokud vyberete interní, vaše App Service Environment bude nakonfigurován s ILB na IP adresu ve virtuální síti. 

Po výběru interní, možnost přidávat další IP adresy k vaší App Service Environment odstraní a místo toho je třeba zadat subdoméně App Service Environment. V App Service Environment externí VIP název App Service Environment se používá v subdoméně pro aplikace vytvořené v této App Service Environment. Pokud vaše App Service Environment byla volána ***contosotest*** a aplikace v rámci této App Service Environment byla volána ***test*** subdoméně by být ve formátu ***contosotest.p.azurewebsites.net*** a adresa URL pro tuto aplikaci bude ***mytest.contosotest.p.azurewebsites.net***. Pokud nastavíte na interní typ VIP, váš název App Service Environment nepoužívá v subdoméně pro App Service Environment. Zadejte subdoméně explicitně. Pokud byl váš subdomény ***contoso.corp.net*** a k aplikaci v tom, že s názvem App Service Environment ***timereporting*** pak by měla adresa URL pro tuto aplikaci ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Aplikace v App Service Environment ILB
Vytvoření aplikace v App Service Environment ILB je stejný jako při vytváření aplikace v App Service Environment normálně. 

1. V portálu Azure vyberte **nový -> Web + mobilní zařízení -> Web** nebo **Mobile** nebo **aplikace API**
2. Zadejte název aplikace
3. Výběr předplatného
4. Vyberte nebo vytvořte skupinu prostředků
5. Vyberte nebo vytvořte Plan(ASP) aplikace služby. Vytvoření nové ASP pak vyberte vaše App Service Environment jako umístění a vyberte fond pracovních procesů chcete vaší ASP vytvořit v. Když vytvoříte ASP vyberete vaší App Service Environment jako umístění a fondu pracovních procesů. Když zadáte název aplikace se zobrazí, že je pro vaše App Service Environment subdoméně nahrazen subdoméně pod názvem vaší aplikace. 
6. Vyberte možnost vytvořit. Měli byste vybrat **připnout na řídicí panel** zaškrtávací políčko, pokud chcete, aby aplikace objeví na řídicím panelu. 

![][2]

V části název aplikace získá název subdomény aktualizovat tak, aby odrážela subdoméně vaší App Service Environment. 

## <a name="post-ilb-ase-creation-validation"></a>Ověření vytvoření POST ILB App Service Environment
App Service Environment ILB je trochu jiná než jiných - ILB App Service Environment. Jak již poznamenat, budete muset spravovat vlastní DNS a je nutné také zadat svůj vlastní certifikát pro připojení prostřednictvím protokolu HTTPS. 

Po vytvoření vaší App Service Environment si všimnete, že subdoméně ukazuje subdoméně jste zadali, a je novou položku v **nastavení** nabídky s názvem **ILB certifikát**. App Service Environment se vytvoří s certifikát podepsaný svým držitelem, který usnadňuje testování HTTPS. Na portálu se tak dozví, že budete muset zadat svůj vlastní certifikát pro protokol HTTPS, ale to je jednotka, abyste měli certifikát, který prochází s vaší subdomény. 

![][3]

Pokud jsou jednoduše vyzkoušení věcí a nemusíte vědět, jak vytvořit certifikát, můžete vytvořit certifikát podepsaný sám sebou aplikace konzoly MMC služby IIS. Po vytvoření můžete ho exportovat jako soubor .pfx a nahrajte ho v uživatelském rozhraní ILB certifikátu. Při přístupu k webu zabezpečenému protokolem certifikát podepsaný svým držitelem, prohlížeč bude zobrazovat upozornění, webu, ke které přistupujete není zabezpečený z důvodu nemůže ověřit certifikát. Pokud chcete, aby se zabránilo že upozornění musíte správně podepsaný certifikát, který odpovídá vaší subdomény a má řetěz certifikátů, který váš prohlížeč rozpozná.

![][6]

Pokud chcete opakujte postup s vlastní certifikáty a testování HTTP a HTTPS přístup k vaší App Service Environment:

1. Přejděte na uživatelské rozhraní App Service Environment po vytvoření App Service Environment **App Service Environment -> Nastavení -> ILB certifikáty**
2. Nastavte ILB certifikát tak, že vyberete soubor pfx certifikátu a zadejte heslo. Tento krok vezme chvíli zpracovat a zobrazí zprávu, která probíhá škálování operace.
3. Získat adresu ILB pro vaše App Service Environment (**App Service Environment -> Vlastnosti -> virtuální IP adresy**)
4. Vytvoření webové aplikace v App Service Environment po vytvoření 
5. Vytvoření virtuálního počítače, pokud nemáte v ní (ne ve stejné podsíti jako App Service Environment nebo věcí konec)
6. Nastavení DNS pro vaše subdomény. Můžete použít zástupný znak s vaší subdomény v DNS nebo pokud budete chtít provést několik jednoduchých testů, úprava souboru hostitelů na vašem virtuálním počítači nastavit název webové aplikace na virtuální IP adresy IP adresu. Pokud vaše App Service Environment měli název subdomény. ilbase.com a můžete provedeny mytestapp aplikaci web, aby by se řešit na mytestapp.ilbase.com a nastavte ho v souboru hostitelů. (V systému Windows soubor hostitele je v C:\Windows\System32\drivers\etc\)
7. Použít prohlížeč na tento virtuální počítač a přejděte na http://mytestapp.ilbase.com (nebo ať je název vaší webové aplikace s vaší subdomény)
8. Použít prohlížeč na tento virtuální počítač a přejděte k https://mytestapp.ilbase.com budete muset přijmout nedostatek zabezpečení, pokud používáte certifikát podepsaný svým držitelem. 

IP adresu pro vaše ILB je uveden ve vlastnostech jako virtuální IP adresy

![][4]

## <a name="using-an-ilb-ase"></a>Pomocí ILB App Service Environment
#### <a name="network-security-groups"></a>Network Security Groups (Skupiny zabezpečení sítě)
App Service Environment ILB umožňuje izolaci sítě pro vaše aplikace, jako jsou aplikace není přístupná nebo i známé pomocí Internetu. Toto je vynikající pro hostování intranetových serverů, například obchodních aplikací. Když je potřeba omezit přístup i další stále můžete Groups(NSGs) zabezpečení sítě pro řízení přístupu na úrovni sítě. 

Pokud chcete použít skupiny Nsg a dál omezit přístup budete muset Ujistěte se, že nedojde k narušení komunikaci, která App Service Environment potřebuje, aby provoz. I když je přístup protokolu HTTP nebo HTTPS pouze prostřednictvím ILB používané App Service Environment App Service Environment se stále závisí na prostředků mimo síť VNet. Zjistěte, jaký přístup k síti stále vyžadují vyhledejte informace v dokumentu o [řízení příchozí přenosy do služby App Service Environment] [ ControlInbound] a dokumentu na [podrobnosti konfigurace sítě pro prostředí App Service pomocí ExpressRoute][ExpressRoute]. 

Ke konfiguraci skupin Nsg je nutné znát IP adresu, která se používají v Azure ke správě vašeho App Service Environment. Tuto IP adresu je také odchozí IP adresu z vaší App Service Environment, pokud se zadá žádosti internetových. Odchozí IP adresu pro vaše App Service Environment bude zůstat statický po celou dobu životnosti vaší App Service Environment. Pokud odstraníte a znovu vytvořte vaše App Service Environment, obdržíte novou IP adresu. Tuto IP adresu najdete **Nastavení -> vlastnosti** a najděte **odchozí IP adresu**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Správa obecné ILB App Service Environment
Správa App Service Environment ILB je z velké části stejný jako správa App Service Environment normálně. Budete muset škálovat vaše fondy pracovních procesů pro hostování více instancí ASP a škálovat vaše servery front-end pro zpracování vyšší objemy přenosy HTTP/HTTPS. Pro obecné informace o správě konfigurace App Service Environment, čtení dokumentu na [konfigurace služby App Service Environment][ASEConfig]. 

Další správu položky jsou správy certifikátů a správu DNS. Budete muset získat a nahrát na server certifikát pro protokol HTTPS používá po vytvoření ILB App Service Environment a nahraďte ji, než vyprší její platnost. Protože Azure vlastní základní doménu můžete poskytujeme certifikáty ASEs externí VIP. Vzhledem k tomu, že subdomény používané ILB App Service Environment může být jakýkoli, budete muset přidat vlastní certifikát pro protokol HTTPS. 

#### <a name="dns-configuration"></a>Konfigurace služby DNS
Pokud používáte externí VIP DNS spravuje Azure. Všechny aplikace vytvořená v vaší App Service Environment se automaticky přidá do Azure DNS, který je veřejném DNS. V App Service Environment ILB, budete muset spravovat vlastní DNS. Pro danou subdoména například contoso.corp.net potřebujete vytvořit DNS A záznamy, které ukazují na vaši adresu ILB pro:

    * 
    publikování *.SCM ftp 


## <a name="getting-started"></a>Začínáme
Chcete-li začít pracovat s prostředí App Service, přečtěte si téma [Úvod do prostředí App Service][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
