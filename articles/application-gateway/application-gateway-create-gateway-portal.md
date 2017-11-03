---
title: "Vytvoření služby Application Gateway - portálu Azure | Microsoft Docs"
description: "Informace o vytvoření služby Application Gateway pomocí portálu"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 54dffe95-d802-4f86-9e2e-293f49bd1e06
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: 17d09ce98c40717d1db0927f791a7c97ea7835e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-application-gateway-with-the-portal"></a>Vytvoření služby application gateway pomocí portálu

[Aplikační brána](application-gateway-introduction.md) je vyhrazené virtuální zařízení poskytuje aplikace doručení řadiče (ADC) jako služba nabízí různé vrstvy 7 možnosti vyrovnávání zatížení pro vaši aplikaci. Tento článek vás provede kroky k vytvoření služby Application Gateway pomocí portálu Azure a přidání existujícího serveru jako člen back-end.

## <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Přihlaste se k portálu Azure v [http://portal.azure.com](http://portal.azure.com)

## <a name="create-application-gateway"></a>Vytvořte aplikační bránu

K vytvoření služby application gateway, postupujte podle pokynů, které následují. Aplikační brána vyžaduje vlastní podsíti. Při vytváření virtuální sítě, ujistěte se, že necháte dostatek adresní prostor tak, aby měl více podsítí. Po nasazení služby application gateway k podsíti, můžete do ní přidat jenom jiné aplikace brány.

1. V podokně oblíbených položek na portálu, klikněte na tlačítko **nový**
1. V okně **Nový** klikněte na **Sítě**. V **sítě** okně klikněte na tlačítko **Application Gateway**, jak je znázorněno na následujícím obrázku:

    ![Vytvoření aplikační brány][1]

1. V **Základy** okno, které se zobrazí, zadejte následující hodnoty a pak klikněte na **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti**|
   |---|---|---|
   |**Název**|AdatumAppGateway|Název služby application gateway|
   |**Vrstvy**|Standard|Dostupné jsou hodnoty Standard a firewall webových aplikací. Navštivte [brány firewall webových aplikací](application-gateway-web-application-firewall-overview.md) Další informace o firewall webových aplikací.|
   |**Velikost SKU**|Střednědobé používání|Možnosti při výběru úrovně Standard jsou malé, střední a velká. Při výběru úrovně firewall webových aplikací, možnosti jsou střední a velké jenom.|
   |**Počet instancí**|2|Počet instancí služby application gateway pro vysokou dostupnost. Počty instancí 1 by měl použít pouze pro účely testování.|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém se má služba Application Gateway vytvořit.|
   |**Skupina prostředků**|**Vytvořit nový:** AdatumAppGatewayRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

1. V **nastavení** okno, které se zobrazí pod **virtuální síť**, klikněte na tlačítko **vyberte virtuální síť**. **Zvolte virtuální síť** otevře se okno.  Klikněte na tlačítko **vytvořit nový** otevřete **vytvořit virtuální síť** okno.

   ![Vyberte virtuální síť][2]

1. Na **okno vytvořit virtuální síť** zadejte následující hodnoty a potom klikněte na tlačítko **OK**. **Vytvořit virtuální síť** a **zvolte virtuální síť** okna zavřete. Tento krok naplní **podsíť** na **nastavení** okno s podsítí vybrali.

   | **Nastavení** | **Hodnota** | **Podrobnosti**|
   |---|---|---|
   |**Název**|AdatumAppGatewayVNET|Název služby application gateway|
   |**Adresní prostor**|10.0.0.0/16|Toto je adresní prostor virtuální sítě|
   |**Název podsítě**|AppGatewaySubnet|Název podsítě pro službu application gateway|
   |**Rozsah adres podsítě**|10.0.0.0/28|Tato podsíť umožňuje více další podsítě ve virtuální síti pro členy fondu back-end|

1. Na **nastavení** okno pod **konfigurace IP front-endu**, zvolte **veřejné** jako **typ IP adresy**

1. Na **nastavení** okno pod **veřejnou IP adresu** klikněte na tlačítko **zvolte veřejnou IP adresu**, **zvolte veřejnou IP adresu** okno otevře, klikněte na tlačítko **vytvořit nový**.

   ![Zvolte veřejnou IP adresu][3]

1. Na **vytvoření veřejné IP adresy** okno, přijměte výchozí hodnotu a klikněte na tlačítko **OK**. Okno se zavře a naplní **veřejnou IP adresu** s veřejnou IP adresou vybrali.

1. Na **nastavení** okno pod **konfiguraci naslouchacího procesu**, klikněte na tlačítko **HTTP** pod **protokol**. Zadejte port pro použití v **Port** pole.

2. Klikněte na tlačítko **OK** na **nastavení** okno pokračujte.

1. Zkontrolujte nastavení na **Souhrn** a klikněte na **OK** zahájíte vytvoření aplikační brány. Vytvoření služby application gateway je dlouhotrvající úlohy a trvá čas dokončení.

## <a name="add-servers-to-backend-pools"></a>Přidání serverů do back-endové fondy

Jakmile vytvoříte aplikační bránu, systémy, které hostují aplikaci, která bude s vyrovnáváním zatížení se stále potřeba přidat do služby application gateway. IP adresy, plně kvalifikovaný název domény nebo síťové adaptéry z těchto serverů se přidají do back-endové fondy adres.

### <a name="ip-address-or-fqdn"></a>IP adresa nebo plně kvalifikovaný název domény

1. Pomocí služby application gateway vytvořený na portálu Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte **AdatumAppGateway** Aplikační brána v okně všechny prostředky. Pokud jste vybrali, již předplatné neobsahuje několik prostředků, můžete zadat **AdatumAppGateway** v **filtrovat podle názvu...** pro snadný přístup ke službě Application Gateway.

1. Zobrazí se aplikační bránu, kterou jste vytvořili. Klikněte na tlačítko **back-endové fondy**a vyberte aktuálního fondu back-end **appGatewayBackendPool**, **appGatewayBackendPool** otevře se okno.

   ![Fondy aplikací s back-end brány][4]

1. Klikněte na tlačítko **přidat cíl** k přidání IP adres plně kvalifikovaný název domény. Zvolte **IP adresu nebo plně kvalifikovaný název domény** jako **typu** a do pole zadejte IP adresu nebo plně kvalifikovaný název domény. Tento krok opakujte pro členy fondu další back-end. Po dokončení klikněte na tlačítko **Uložit**.

### <a name="virtual-machine-and-nic"></a>Virtuální počítač a síťový adaptér

Síťové adaptéry virtuálního počítače můžete také přidat jako členy fondu back-end. Pouze virtuální počítače v rámci stejné virtuální síti jako aplikační brána jsou k dispozici prostřednictvím rozevíracího seznamu.

1. Pomocí služby application gateway vytvořený na portálu Azure **Oblíbené** podokně klikněte na tlačítko **všechny prostředky**. Klikněte **AdatumAppGateway** Aplikační brána v okně všechny prostředky. Pokud jste vybrali, již předplatné neobsahuje několik prostředků, můžete zadat **AdatumAppGateway** v **filtrovat podle názvu...** pro snadný přístup ke službě Application Gateway.

1. Zobrazí se aplikační bránu, kterou jste vytvořili. Klikněte na tlačítko **back-endové fondy**a vyberte aktuálního fondu back-end **appGatewayBackendPool**, **appGatewayBackendPool** otevře se okno.

   ![Fondy aplikací s back-end brány][5]

1. Klikněte na tlačítko **přidat cíl** k přidání IP adres plně kvalifikovaný název domény. Zvolte **virtuálního počítače** jako **typu** a vyberte virtuální počítač a síťovou kartu k použití. Po dokončení klikněte na tlačítko **uložit**

   > [!NOTE]
   > Jenom virtuální počítače ve stejné virtuální síti jako aplikační brána jsou k dispozici v rozevíracím seznamu.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud již nepotřebujete, odstraňte skupinu prostředků, aplikační brány a všechny související prostředky. To pokud chcete udělat, vyberte skupinu prostředků z okna aplikace brány a klikněte na tlačítko **odstranit**.

## <a name="next-steps"></a>Další kroky

V tomto scénáři nasazení služby application gateway, přidat server do back-end. Další kroky jsou konfigurace služby application gateway změny nastavení a úprava pravidla brány. Tyto kroky můžete navštívením v následujících článcích:

Naučte se vytvářet vlastní stavu sondy navštivte stránky [vytvořit vlastní stav testu](application-gateway-create-probe-portal.md)

Zjistěte, jak nakonfigurovat snižování zátěže protokolu SSL a trvat nákladná dešifrování SSL vypnout webových serverů, navštivte stránky [konfigurovat přesměrování zpracování SSL](application-gateway-ssl-portal.md)

Zjistěte, jak k ochraně vašich aplikací s [brány Firewall webových aplikací](application-gateway-webapplicationfirewall-overview.md) funkce aplikační brány.

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
