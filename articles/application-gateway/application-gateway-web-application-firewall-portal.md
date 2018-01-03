---
title: "Vytvořit nebo aktualizovat aplikační bránu pomocí brány firewall webových aplikací | Microsoft Docs"
description: "Informace o vytvoření služby application gateway pomocí brány firewall webových aplikací pomocí portálu"
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: b561a210-ed99-4ab4-be06-b49215e3255a
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: davidmu
ms.openlocfilehash: bfc06c1b44974fd17a3794654503d21d6407a917
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-by-using-the-portal"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací pomocí portálu

> [!div class="op_single_selector"]
> * [portál Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Azure CLI](application-gateway-web-application-firewall-cli.md)

Naučte se vytvářet brány firewall webových aplikací (firewall webových aplikací)-Povolit aplikační brány.

Firewall webových aplikací v Azure Application Gateway chrání webových aplikací z běžných útoky založenými na web jako Injektáž SQL, útoky skriptování mezi weby a hijacks relace. Firewall webových aplikací chrání před řadu OWASP top 10 běžné webové chyb zabezpečení.

## <a name="scenarios"></a>Scénáře

Tento článek představuje dva scénáře. V prvního scénáře, zjistíte, jak [vytvoření služby application gateway s firewall webových aplikací](#create-an-application-gateway-with-web-application-firewall). V druhém scénáři se dozvíte, jak [existující aplikační brány přidat firewall webových aplikací](#add-web-application-firewall-to-an-existing-application-gateway).

![Příklad scénáře][scenario]

> [!NOTE]
> Aplikační brány můžete přidat vlastní stavu sondy, fond back-end adresy a dalších pravidlech. Tyto aplikace jsou konfigurovány po dokončení konfigurace aplikační brány a ne během počátečního nasazení.

## <a name="before-you-begin"></a>Než začnete

 Služby application gateway vyžaduje vlastní podsíti. Když vytvoříte virtuální síť, ujistěte se, ponechat dostatek místa na adresu tak, aby měl více podsítí. Po nasazení služby application gateway k podsíti, lze přidat pouze další aplikačních bran do podsítě.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Přidání brány firewall webových aplikací do existující aplikační brány

Aktualizuje existující aplikační bránu. pro podporu firewall webových aplikací v tomto příkladu **prevence** režimu.

1. Na portálu Azure **Oblíbené** podokně, vyberte **všechny prostředky**. Na **všechny prostředky** okně vyberte existující aplikační brány. Pokud jste vybrali, již předplatné neobsahuje několik prostředků, zadejte název v **filtrovat podle názvu** pole snadno přístupu zóny DNS.

   ![Výběr brány existující aplikace][1]

2. Vyberte **brány firewall webových aplikací**a aktualizovat nastavení aplikační brány. Po dokončení aktualizace. Vyberte **Uložit**. 

3. Chcete-li aktualizovat existující aplikační brány pro podporu firewall webových aplikací, použijte následující nastavení:

   | **Nastavení** | **Hodnota** | **Podrobnosti**
   |---|---|---|
   |**Upgradujte na úroveň firewall webových aplikací**| Zaškrtnuté | Tento parametr nastaví úroveň služby application gateway k vrstvě firewall webových aplikací.|
   |**Stav brány firewall**| Povoleno | Toto nastavení umožňuje v bráně firewall na firewall webových aplikací.|
   |**Režimu brány firewall** | Prevention (Prevence) | Toto nastavení je, jak má zacházet s škodlivý přenos firewall webových aplikací. **Detekce** režimu pouze protokoly událostí. **Prevence** režim protokoly událostí a zastaví škodlivý přenos.|
   |**Sada pravidel**|3.0|Toto nastavení určuje, [základní sada pravidel](application-gateway-web-application-firewall-overview.md#core-rule-sets) používané k ochraně členy fondu back-end.|
   |**Konfigurace zakázaná pravidla**|Je to různé.|Abyste zabránili falešně pozitivních zjištění možných, můžete toto nastavení zakázat určité [pravidla a pravidla skupiny](application-gateway-crs-rulegroups-rules.md).|

    >[!NOTE]
    > Když upgradujete existující aplikační brány pro SKU firewall webových aplikací, změny velikosti SKU **střední**. Po dokončení konfigurace, můžete změnit konfiguraci tohoto nastavení.

    ![Základní nastavení][2-1]

    > [!NOTE]
    > Zobrazit protokoly firewall webových aplikací, zapněte diagnostiku a vyberte **ApplicationGatewayFirewallLog**. Vyberte počet instancí **1** pouze pro testovací účely. Nedoporučujeme počet instancí v rámci **2** vzhledem k tomu, že není předmětem smlouvě SLA. Malé brány nejsou k dispozici při použití firewall webových aplikací.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Vytvoření služby application gateway pomocí brány firewall webových aplikací

Tento scénář se:

* Vytvořte střednědobého používání Aplikační brána firewall webových aplikací se dvěma instancemi.
* Vytvořte virtuální síť s názvem AdatumAppGatewayVNET s vyhrazeným blokem CIDR 10.0.0.0/16.
* Vytvoříte podsíť s názvem Appgatewaysubnet, která používá blok CIDR 10.0.0.0/28.
* Konfigurujte certifikát pro přesměrování zpracování SSL.

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com). Pokud účet nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/free).

2. V **Oblíbené** podokně na portálu, vyberte **nový**.

3. Na **nový** vyberte **sítě**. Na **sítě** vyberte **Application Gateway**, jak je znázorněno na následujícím obrázku:

    ![Vytváření služby Application gateway][1]

4. Na **Základy** okno, které se zobrazí, zadejte následující hodnoty a potom vyberte **OK**:

   | **Nastavení** | **Hodnota** | **Podrobnosti**
   |---|---|---|
   |**Název**|AdatumAppGateway|Název služby application gateway.|
   |**Vrstvy**|WAF|Dostupné jsou hodnoty Standard a firewall webových aplikací. Další informace o firewall webových aplikací najdete v tématu [brány firewall webových aplikací](application-gateway-web-application-firewall-overview.md).|
   |**Velikost SKU**|Střednědobé používání|Možnosti úrovně Standard jsou **malé**, **střední**, a **velké**. Možnosti vrstvy firewall webových aplikací jsou **střední** a **velké** pouze.|
   |**Počet instancí**|2|Počet instancí služby application gateway pro vysokou dostupnost. Počty instancí 1 použijte jenom pro účely testování.|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné sloužící k vytvoření aplikační brány.|
   |**Skupina prostředků**|**Vytvořit nový:** AdatumAppGatewayRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

   ![Základní nastavení konfigurace][2-2]

5. Na **nastavení** okno, které se zobrazí v části **virtuální síť**, vyberte **vyberte virtuální síť**. Na **zvolte virtuální síť** vyberte **vytvořit nový**.

   ![Výběr virtuální sítě][2]

6. Na **okno vytvořit virtuální síť**, zadejte následující hodnoty a potom vyberte **OK**. **Podsíť** na **nastavení** okno se zobrazí v podsíti, které jste zvolili.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|AdatumAppGatewayVNET|Název služby application gateway.|
   |**Adresní prostor**|10.0.0.0/16| Tato hodnota je adresní prostor virtuální sítě.|
   |**Název podsítě**|AppGatewaySubnet|Název podsítě pro službu application gateway.|
   |**Rozsah adres podsítě**|10.0.0.0/28 | Tato podsíť pro členy fondu back-end umožňuje více další podsítě ve virtuální síti.|

7. Na **nastavení** okno pod **konfigurace IP front-endu**, vyberte **veřejné** jako **IP adres jako typu**.

8. Na **nastavení** okno pod **veřejnou IP adresu**, vyberte **zvolte veřejnou IP adresu**. Na **zvolte veřejnou IP adresu** vyberte **vytvořit nový**.

   ![Veřejná IP adresa výběru][3]

9. Na **vytvoření veřejné IP adresy** okno, přijměte výchozí hodnotu a vyberte **OK**. **Veřejnou IP adresu** se vyplní pole s veřejnou IP adresu, které jste zvolili.

10. Na **nastavení** okno pod **konfiguraci naslouchacího procesu**, vyberte **HTTP** pod **protokol**. Certifikát je potřeba použít **HTTPS**. Privátní klíč pro certifikát je potřeba. Zadejte .pfx exportu certifikátu a zadejte heslo pro soubor.

11. Konfigurovat konkrétní nastavení pro **firewall webových aplikací**.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Stav brány firewall**| Povoleno| Toto nastavení se změní firewall webových aplikací zapnout nebo vypnout.|
   |**Režimu brány firewall** | Prevention (Prevence)| Toto nastavení určuje akce, které firewall webových aplikací, převezme škodlivý přenos. **Detekce** režimu pouze protokoly provoz. **Prevence** režim protokoly a zastaví provoz 403 neoprávněným odpovědi.|


12. Zkontrolujte **Souhrn** a vyberte **OK**. Službu application gateway je nyní zařazen do fronty a vytvořit.

13. Po použití vytvoření brány, přejděte na ni na portálu pokračujte konfigurace aplikační brány.

    ![Zobrazení prostředků brány aplikace][10]

Tyto kroky vytvořit základní aplikační brána s výchozím nastavením pro naslouchací proces, fond back-end, nastavení HTTP back-end a pravidla. Po dokončení zřizování úspěšně, můžete změnit tato nastavení tak, aby vyhovovala vašemu nasazení.

> [!NOTE]
> Application Gateway vytvoří s základní konfigurací firewall webových aplikací jsou nakonfigurovány s řádku 3.0 pro ochranu.

## <a name="next-steps"></a>Další kroky

Konfigurace vlastní domény alias pro funkci [veřejnou IP adresu](../dns/dns-custom-domain.md#public-ip-address), můžete použít Azure DNS nebo jiného poskytovatele DNS.

Konfigurace protokolování diagnostiky do protokolu událostí, které jsou zjištěna nebo zabránit s firewall webových aplikací najdete v tématu [diagnostics Application Gateway](application-gateway-diagnostics.md).

Vytvoření vlastní stavu sondy naleznete v tématu [vytvořit sondu vlastní stavu](application-gateway-create-probe-portal.md).

Ke konfiguraci snižování zátěže protokolu SSL a trvat nákladná předplatné SSL vypnout webových serverů, najdete v části [přesměrování zpracování SSL konfigurace](application-gateway-ssl-portal.md).

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[2-1]: ./media/application-gateway-web-application-firewall-portal/figure2-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png
