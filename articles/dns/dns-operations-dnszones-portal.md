---
title: "Správa zón DNS v Azure DNS - portálu Azure | Microsoft Docs"
description: "Můžete spravovat zóny DNS pomocí portálu Azure. Tento článek popisuje, jak k aktualizaci, odstranění a vytvoření zóny DNS na Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2017
ms.author: gwallace
ms.openlocfilehash: cfb1debf9447cd66856b73166a133d5d498fcc79
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="how-to-manage-dns-zones-in-the-azure-portal"></a>Správa zón DNS na portálu Azure

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Tento článek ukazuje, jak spravovat zóny DNS pomocí portálu Azure. Můžete také spravovat zóny DNS pomocí napříč platformami [rozhraní příkazového řádku Azure](dns-operations-dnszones-cli.md) nebo Azure [prostředí PowerShell](dns-operations-dnszones.md).

## <a name="create-a-dns-zone"></a>Vytvoření zóny DNS

1. Přihlášení k webu Azure Portal
2. V nabídce centra klikněte a klikněte na **vytvořit prostředek > sítě >** a pak klikněte na **zónu DNS** otevřete okno Vytvoření DNS zóny.

    ![Zóna DNS](./media/dns-operations-dnszones-portal/openzone650.png)

4. V okně **Vytvořit zónu DNS** zadejte následující hodnoty a pak klikněte na **Vytvořit**:


   | **Nastavení** | **Hodnota** | **Podrobnosti** |
   |---|---|---|
   |**Název**|contoso.com|Název zóny DNS|
   |**Předplatné**|[Vaše předplatné]|Vyberte předplatné, ve kterém chcete vytvořit zónu DNS.|
   |**Skupina prostředků**|**Vytvořit novou:** contosoDNSRG|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups).|
   |**Umístění**|Západní USA||

> [!NOTE]
> Skupina prostředků označuje umístění skupiny prostředků a nemá žádný vliv na zónu DNS. Umístění zóny DNS je vždy globální a není zobrazeno.

## <a name="list-dns-zones"></a>Seznam zón DNS

Na portálu Azure přejděte do **další služby** > **sítě** > **zóny DNS**. Každé zóny DNS se jedná vlastní prostředek, informace, jako je počet sad záznamů a názvové servery je možné zobrazit z tohoto zobrazení. Sloupec **NÁZVOVÉ servery** není ve výchozím zobrazení, klikněte na tlačítko Přidat **sloupce**, vyberte **názvové servery** a klikněte na tlačítko **provádí**.

![výpis zóny DNS](./media/dns-operations-dnszones-portal/listzones.png)

## <a name="delete-a-dns-zone"></a>Odstranit zónu DNS

Přejděte do zóny DNS na portálu. Na **zónu DNS** okně klikněte na tlačítko **odstranit zónu**. Zobrazí se výzva k potvrzení, zda že se chtějí odstranit zónu DNS. Odstranění zóny DNS také odstraní všechny záznamy, které jsou obsaženy v zóně.

## <a name="next-steps"></a>Další postup

Naučte se pracovat s zóny DNS a záznamy, navštivte stránky [Začínáme s Azure DNS pomocí portálu Azure](dns-getstarted-portal.md).