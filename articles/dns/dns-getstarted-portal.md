---
title: "Začínáme s DNS Azure pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Naučíte se vytvořit zónu a záznam DNS v DNS Azure. Pomocí tohoto podrobného průvodce můžete vytvořit a spravovat první zónu a záznam DNS pomocí webu Azure Portal."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 79f0c9297c4be70f705f325274f3d9241ea4bc3f
ms.lasthandoff: 03/29/2017

---

# <a name="get-started-with-azure-dns-using-the-azure-portal"></a>Začínáme s DNS Azure pomocí webu Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Tento článek vás provede kroky k vytvoření první zóny a záznamu DNS pomocí webu Azure Portal. Tyto kroky můžete provést také pomocí Azure PowerShellu nebo Azure CLI pro různé platformy.

K hostování záznamů DNS v určité doméně se používá zóna DNS. Pokud chcete začít hostovat svou doménu v DNS Azure, musíte vytvořit zónu DNS pro daný název domény. Všechny záznamy DNS pro vaši doménu se pak vytvoří v této zóně DNS. Nakonec, pokud chcete zónu DNS publikovat na internetu, bude potřeba nakonfigurovat pro doménu názvové servery. Jednotlivé kroky jsou popsány níže.

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlášení k webu Azure Portal
2. V nabídce centra klikněte na **Nový > Sítě >** a potom kliknutím na **Zóna DNS** otevřete okno Vytvořit zónu DNS.

    ![Zóna DNS](./media/dns-getstarted-portal/openzone650.png)

4. V okně **Vytvořit zónu DNS** pojmenujte zónu DNS. Například *contoso.com*.
 
    ![Vytvoření zóny](./media/dns-getstarted-portal/newzone250.png)

5. Dále určete skupinu prostředků, kterou chcete použít. Můžete vytvořit novou skupinu prostředků, nebo vybrat již existující. Pokud se rozhodnete vytvořit novou skupinu prostředků, pomocí rozevírací nabídky **Umístění** zadejte umístění skupiny prostředků. Pamatujte, že toto nastavení se týká umístění skupiny prostředků a nemá žádný vliv na zónu DNS. Umístění zóny DNS je vždy globální a není zobrazeno.

6. Políčko **Připnout na řídicí panel** můžete ponechat zaškrtnuté, pokud chcete novou zónu snadno najít na řídicím panelu. Poté klikněte na **Vytvořit**.

    ![Připnutí na řídicí panel](./media/dns-getstarted-portal/pindashboard150.png)

7. Po kliknutí na Vytvořit uvidíte na řídicím panelu probíhající konfiguraci nové zóny.

    ![Vytváření](./media/dns-getstarted-portal/creating150.png)

8. Po vytvoření nové zóny se na řídicím panelu otevře okno nové zóny.


## <a name="create-a-dns-record"></a>Vytvoření záznamu DNS

Následující příklad vás provede procesem vytvoření nového záznamu A. Informace o dalších typech záznamů a úpravě existujících záznamů najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md). 


1. V horní části okna **Zóna DNS** vyberte **Sada záznamů**. Otevře se okno **Přidat sadu záznamů**.

    ![Nová sada záznamů](./media/dns-getstarted-portal/newrecordset500.png)

4. V okně **Přidat sadu záznamů** pojmenujte sadu záznamů. Sadu záznamů můžete pojmenovat například **www**.

    ![Přidat sadu záznamů](./media/dns-getstarted-portal/addrecordset500.png)

5. Vyberte typ záznamu, který chcete vytvořit. Pro účely tohoto příkladu vyberte **A**.
6. Nastavte hodnotu **TTL**. Výchozí hodnota TTL (Time to Live) je jedna hodina.
7. Přidejte IP adresu záznamu.
8. Vytvořte záznam DNS výběrem **OK** v dolní části okna.


## <a name="view-records"></a>Zobrazení záznamů

V dolní části okna zóny DNS se zobrazí záznamy pro zónu DNS. Měli byste vidět výchozí záznamy DNS a SOA, které se vytvoří v každé zóně, a nové záznamy, které jste vytvořili.

![zóna](./media/dns-getstarted-portal/viewzone500.png)


## <a name="update-name-servers"></a>Aktualizace názvových serverů

Jakmile budete spokojeni se správným nastavením zóny a záznamů DNS, bude potřeba nakonfigurovat váš název domény tak, aby používal názvové servery DNS Azure. Tím umožníte ostatním uživatelům na internetu najít vaše záznamy DNS.

Názvové servery vaší zóny můžete zobrazit na webu Azure Portal:

![zóna](./media/dns-getstarted-portal/viewzonens500.png)

Tyto názvové servery by měly být nakonfigurované u registrátora názvu domény (u kterého jste zakoupili název domény). Registrátor vám nabídne možnost nastavit názvové servery pro doménu. Další informace najdete v tématu [Delegování domény do DNS Azure](dns-domain-delegation.md).


## <a name="next-steps"></a>Další kroky

Další informace o DNS Azure najdete v tématu [Přehled DNS Azure](dns-overview.md).

Další informace o správě záznamů DNS v DNS Azure najdete v tématu [Správa záznamů a sad záznamů DNS pomocí webu Azure Portal](dns-operations-recordsets-portal.md).


