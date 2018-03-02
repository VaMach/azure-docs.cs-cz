---
title: "Začínáme s DNS Azure pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Naučíte se vytvořit zónu a záznam DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat první zónu a záznam DNS pomocí webu Azure Portal."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: 22bf52f7452f182510c3714f7d1c2ca884446953
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Začínáme s DNS Azure pomocí webu Azure Portal

> [!div class="op_single_selector"]
> * [portál Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Tento článek vás provede kroky k vytvoření první zóny a záznamu DNS pomocí webu Azure Portal. Tyto kroky můžete provést také pomocí Azure PowerShellu nebo Azure CLI pro různé platformy.

K hostování záznamů DNS v určité doméně se používá zóna DNS. Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS. Nakonec, pokud chcete zónu DNS publikovat na internetu, bude potřeba nakonfigurovat pro doménu názvové servery. Každý z těchto kroků je popsán v následujících krocích.

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlaste se k portálu Azure.
2. V nabídce centra klikněte na **Vytvořit prostředek > Sítě >** a pak klikněte na **Zóna DNS**. Otevře se stránka **Vytvořit zónu DNS**.

    ![Zóna DNS](./media/dns-getstarted-portal/openzone650.png)

4. Na stránce **Vytvořit zónu DNS** zadejte následující hodnoty a pak klikněte na **Vytvořit**:


   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|contoso.com|Název zóny DNS|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém chcete vytvořit zónu DNS.|
   |**Skupina prostředků**|**Vytvořit novou:** contosoDNSRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků označuje umístění skupiny prostředků a nemá žádný vliv na zónu DNS. Umístění zóny DNS je vždy globální a není zobrazeno.

## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

Následující příklad vás provede procesem vytvoření nového záznamu A. Informace o dalších typech záznamů a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md). 

1. Když máte vytvořenou zónu DNS, na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**. Na stránce Všechny prostředky klikněte na zónu DNS **contoso.com**. Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat **contoso.com** do pole **Filtrovat podle názvu...** pro snadný přístup k zóně DNS.

1. V horní části stránky **Zóna DNS** vyberte **Sada záznamů**. Otevře se stránka **Přidat sadu záznamů**.

1. Na stránce **Přidat sadu záznamů** zadejte následující hodnoty a klikněte na **OK**. V tomto příkladu vytváříte záznam A.

   |**Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|www|Název záznamu|
   |**Typ**|A| Typ záznamu DNS, který se má vytvořit. Přípustné hodnoty jsou A, AAAA, CNAME, MX, NS, SRV, TXT, a PTR.  Další informace o typech záznamů najdete v tématu [Přehled záznamů a zón DNS](dns-zones-records.md).|
   |**Hodnota TTL**|1|Hodnota TTL (Time to Live) žádosti DNS.|
   |**Jednotka hodnoty TTL**|Hodiny|Měřené jednotky času pro hodnotu TTL.|
   |**IP adresa**|ipAddressValue| Tato hodnota je IP adresa, kterou záznam DNS překládá.|

## <a name="view-records"></a>Zobrazení záznamů

V dolní části stránky Zóna DNS se zobrazí záznamy pro příslušnou zónu DNS. Měli byste vidět výchozí záznamy DNS a SOA, které se vytvoří v každé zóně, a nové záznamy, které jste vytvořili.

![zóna](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Aktualizace názvových serverů

Jakmile budete spokojeni se správným nastavením zóny a záznamů DNS, bude potřeba nakonfigurovat váš název domény tak, aby používal názvové servery DNS Azure. Tím umožníte ostatním uživatelům na internetu najít vaše záznamy DNS.

Názvové servery vaší zóny můžete zobrazit na webu Azure Portal:

![zóna](./media/dns-getstarted-portal/viewzonens500.png)

Tyto názvové servery by měly být nakonfigurované u registrátora názvu domény (u kterého jste zakoupili název domény). Registrátor nabízí možnost nastavit názvové servery pro doménu. Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:

1. Na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**. Na stránce Všechny prostředky klikněte na skupinu prostředků **MyResourceGroup**. Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat **MyResourceGroup** do pole **Filtrovat podle názvu...** pro snadný přístup ke skupině prostředků.
1. Na stránce **MyResourceGroup** klikněte na tlačítko **Odstranit**.
1. Portál požaduje, abyste zadali název skupiny prostředků pro potvrzení, že ji skutečně chcete odstranit. Klikněte na **Odstranit**, jako název skupiny prostředků zadejte *MyResourceGroup*, a pak klikněte na **Odstranit**. Odstraněním skupiny prostředků se odstraní všechny prostředky v rámci dané skupiny prostředků. Proto nikdy nezapomeňte před odstraněním skupiny prostředků zkontrolovat její obsah. Portál odstraní všechny prostředky v rámci skupiny prostředků a potom odstraní samotnou skupinu prostředků. Tento proces trvá několik minut.


## <a name="next-steps"></a>Další kroky

Další informace o DNS Azure najdete v tématu [Přehled DNS Azure](dns-overview.md).

Další informace o správě záznamů DNS v DNS Azure najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md).

