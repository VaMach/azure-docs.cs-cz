---
title: "Integrace App Service Environment vaší ILB s služby Azure Application Gateway"
description: "Návod k integraci aplikace v App Service Environment vaší ILB s služby Azure Application Gateway"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a6a74f17-bb57-40dd-8113-a20b50ba3050
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: ccompy
ms.openlocfilehash: eedad8824add7fe425d34975dab640fbee82c2bc
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/19/2017
---
# <a name="integrating-your-ilb-ase-with-an-application-gateway"></a>Integrace App Service Environment vaší ILB s aplikační brány #

[Azure App Service prostředí řízení,](./intro.md) nasazení služby Azure App Service v podsíti virtuální sítě Azure zákazníka. Dá se nasadit pomocí veřejných nebo privátních koncový bod pro přístup k aplikaci. Nasazení sady App Service Environment privátní koncový bod se nazývá ILB App Service Environment.  
Azure Application Gateway je virtuální zařízení, která poskytuje Vyrovnávání zatížení vrstvy 7, snižování zátěže protokolu SSL a ochranu firewall webových aplikací. Můžete ho naslouchat na veřejné IP adresy a trasy přenosem do vaší aplikace koncového bodu. Následující informace popisují, jak integrovat firewall webových aplikací nakonfigurované aplikace v App Service Environment ILB Application Gateway.  

Integrace aplikační brány s ILB, App Service Environment se na úrovni aplikace.  Když nakonfigurujete službu Application Gateway s vaší App Service Environment ILB, byste ho pro konkrétní aplikace v App Service Environment vaší ILB. To umožňuje hostování zabezpečené víceklientským aplikacím v jednom ILB App Service Environment.  

![Aplikační brána odkazující na aplikaci na ILB App Service Environment][1]

Tento návod vám ukáže, jak:

* Vytvoření služby Application Gateway
* Nakonfigurujte aplikační bránu tak, aby odkazoval na aplikaci na vašem ILB App Service Environment
* Konfigurace aplikace přijmout vlastní název domény
* Upravte váš veřejný název hostitele DNS, který odkazuje na svoji službu Application Gateway

K integraci Application Gateway s vaší App Service Environment ILB, potřebujete:

* ILB App Service Environment
* aplikace spuštěné v App Service Environment ILB
* názvu internetové domény směrovatelné pro použití s vaší aplikace v App Service Environment ILB
* adresu ILB používané vaší ILB App Service Environment (Toto je App Service Environment portálu v části **Nastavení -> IP adresy**)

    ![IP adresy používané vaší ILB App Service Environment][9]
    
* veřejný název DNS, který se používá později tak, aby odkazoval na svoji službu application gateway 

Pro dokument číst informace o tom, jak vytvořit App Service Environment ILB [vytváření a používání ILB App Service Environment][ilbase]

Tato příručka předpokládá, že chcete aplikační brány ve stejné virtuální síti Azure, která je nasazena App Service Environment do. Před spuštěním vytvoření aplikační brány, vybrat nebo vytvořit podsíť, která se bude používat k hostování služby Application Gateway. Měli byste použít podsíť, která není jednu s názvem GatewaySubnet, nebo podsíť používá App Service Environment ILB.
Když vložíte Aplikační brána v na GatewaySubnet pak nebude možné vytvořit bránu virtuální sítě později. Můžete také nelze ho přesuňte do podsítě používané ve vaší App Service Environment ILB jako App Service Environment je jediné, co může být v této podsíti.

## <a name="steps-to-configure"></a>Postup konfigurace ##

1. Z portálu Azure, přejděte na **nový > sítě > Application Gateway** 
    1. Zadejte:
        1. Název služby Application Gateway
        1. Vyberte firewall webových aplikací
        1. Vyberte stejné předplatné použité pro virtuální síť App Service Environment
        1. Vytvořte nebo vyberte skupinu prostředků.
        1. Vyberte virtuální síť App Service Environment je v umístění

    ![Nové základy vytváření aplikací brány][2]   
    1. V sadě nastavení oblasti:
        1. Virtuální síť App Service Environment
        1. Musí být nasazena do podsítě služby Application Gateway. Provést bez použití na GatewaySubnet jako zabrání vytvoření brány VPN
        1. Vyberte veřejný
        1. Vyberte veřejnou IP adresu. Pokud nemáte jeden vytvořte jednu v tuto chvíli
        1. Konfigurace protokolu HTTP nebo HTTPS. Při konfiguraci pro protokol HTTPS, musíte zadat certifikát PFX
        1. Vyberte nastavení fireway webové aplikace. Zde můžete povolit bránu firewall a také nastavit pro buď zjišťování nebo prevence, jak můžete vidět přizpůsobit.

    ![Nové nastavení vytvoření služby application gateway][3]
    
    1. V části Souhrn kontrola, vyberte **Ok**. To může trvat o něco víc než 30 minut pro svoji službu Application Gateway k dokončení instalace.  

2. Po dokončení instalace Application Gateway přejděte do portálu Application Gateway. Vyberte **fond back-end**.  Přidáte adresu ILB pro vaše ILB App Service Environment.

    ![Nakonfigurujte fond back-end][4]

3. Po dokončení zpracování pro konfiguraci fondu back-end vyberte **testy stavu**. Vytvořte test stavu pro název domény, který chcete použít pro vaši aplikaci. 

    ![Konfigurace sond stavu][5]
    
4. Po dokončení zpracování pro konfiguraci vašeho sondy stavu vyberte **nastavení HTTP**.  Upravit existující nastavení existuje, vyberte **testu pomocí vlastního**a vyberte test, který jste nakonfigurovali.

    ![Konfigurace nastavení protokolu HTTP][6]
    
5. Přejděte do služby Application Gateway **přehled**a zkopírujte veřejnou IP adresu použitou pro službu Application Gateway.  Nastavit tuto IP adresu jako záznamu A pro název domény vaší aplikace, nebo použijte název DNS pro tuto adresu v záznamu CNAME.  Je jednodušší vyberte veřejnou IP adresu a zkopírujte jej z veřejné IP adresy uživatelského rozhraní než zkopírovat z odkazu v oddílu Přehled brány aplikace. 

    ![Portál Application Gateway][7]

6. Nastavte vlastní název domény pro vaši aplikaci v App Service Environment vaší ILB.  Přejděte do vaší aplikace na portálu a v části Nastavení vyberte **vlastní domény**

![Nastavit vlastní název domény v aplikaci][8]

Informace o nastavení vlastní názvy domén pro webové aplikace v tomto poli [nastavení vlastní názvy domén pro vaši webovou aplikaci][custom-domain]. Rozdíl oproti aplikaci v App Service Environment ILB a tento dokument je, že není k dispozici žádné ověření na názvu domény.  Vzhledem k tomu, že jste vlastníkem DNS, který spravuje aplikace koncových bodů, které můžete vložit všechno zde. Vlastní název domény, které přidáte v takovém případě nemusí být v DNS, ale stále nutné nakonfigurovat s vaší aplikací. 

Po dokončení instalace a jste povolili krátkou dobu pro změny DNS rozšíření, pak aplikace můžete přejít pomocí vlastní název domény, kterou jste vytvořili. 


<!--IMAGES-->
[1]: ./media/integrate-with-application-gateway/appgw-highlevel.png
[2]: ./media/integrate-with-application-gateway/appgw-createbasics.png
[3]: ./media/integrate-with-application-gateway/appgw-createsettings.png
[4]: ./media/integrate-with-application-gateway/appgw-backendpool.png
[5]: ./media/integrate-with-application-gateway/appgw-healthprobe.png
[6]: ./media/integrate-with-application-gateway/appgw-httpsettings.png
[7]: ./media/integrate-with-application-gateway/appgw-publicip.png
[8]: ./media/integrate-with-application-gateway/appgw-customdomainname.png
[9]: ./media/integrate-with-application-gateway/appgw-iplist.png

<!--LINKS-->
[appgw]: http://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
