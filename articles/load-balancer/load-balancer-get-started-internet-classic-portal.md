---
title: "Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení v modelu nasazení Classic pomocí portálu Azure Classic | Microsoft Docs"
description: "Zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení v modelu nasazení Classic pomocí portálu Azure Classic"
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 7d8eb43fea032eb5aa72f448a7c1022be62a7b81
ms.openlocfilehash: bf71746d3c23fd69902f1a7af6cbab2ad1a65df4

---

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Začínáme vytvářet internetový nástroj pro vyrovnávání zatížení (Classic) na portálu Azure Classic

> [!div class="op_single_selector"]
> * [Portál Azure Classic](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Než začnete pracovat s prostředky Azure, je potřeba si uvědomit, že Azure má v současné době dva modely nasazení: Azure Resource Manager a klasický. Před zahájením práce s jakýmikoli prostředky Azure se ujistěte, že rozumíte [modelům nasazení a příslušným nástrojům](../azure-classic-rm.md). Dokumentaci k různým nástrojům můžete zobrazit kliknutím na karty v horní části tohoto článku. Tento článek se týká modelu nasazení Classic. Případně [zjistěte, jak vytvořit internetový nástroj pro vyrovnávání zatížení pomocí Azure Resource Manageru](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Nastavení internetového nástroje pro vyrovnávání zatížení pro virtuální počítače

Abyste mohli vyrovnávat zatížení síťového provozu z internetu napříč virtuálními počítači cloudové služby, musíte vytvořit sadu s vyrovnáváním zatížení. Tento postup předpokládá, že již máte vytvořené virtuální počítače, a že všechny jsou v rámci stejné cloudové služby.

**Konfigurace sady s vyrovnáváním zatížení pro virtuální počítače**

1. Na portálu Azure Classic klikněte na **Virtuální počítače** a potom klikněte na název virtuálního počítače v sadě s vyrovnáváním zatížení.
2. Klikněte na **Koncové body** a potom na **Přidat**.
3. Na stránce **Přidat koncový bod do virtuálního počítače** klikněte na šipku doprava.
4. Na stránce **Zadat podrobnosti o koncovém bodu**:

   * V části **Název** zadejte název koncového bodu nebo vyberte název ze seznamu předdefinovaných koncových bodů pro běžné protokoly.
   * V části **Protokol** vyberte podle potřeby protokol vyžadovaný typem koncového bodu, buď TCP, nebo UDP.
   * V části **Veřejný port a privátní port** zadejte podle potřeby čísla portů, které chcete, aby virtuální počítač používat. Můžete použít privátní port a pravidla brány firewall na virtuálním počítači k přesměrování provozu ve směru vhodném pro vaši aplikaci. Privátní port může být stejný jako veřejný port. Například koncovému bodu pro webový provoz (HTTP) můžete přiřadit port 80 jako veřejný i privátní port.

5. Vyberte možnost **Vytvořit sadu s vyrovnáváním zatížení** a potom klikněte na šipku doprava.
6. Na stránce **Konfigurovat sadu s vyrovnáváním zatížení** zadejte název sady s vyrovnáváním zatížení a potom přiřaďte hodnoty pro chování testů služby Azure Load Balancer. Služba Load Balancer pomocí testů určuje, jestli jsou virtuální počítače v sadě s vyrovnáváním zatížení dostupné k přijímání příchozího provozu.
7. Kliknutím na značku zaškrtnutí vytvořte koncový bod s vyrovnáváním zatížení. Na stránce **Koncové body virtuálního počítače** uvidíte ve sloupci **Název sady s vyrovnáváním zatížení** hodnotu **Ano**.
8. Na portálu klikněte na **Virtuální počítače**, klikněte na název dalšího virtuálního počítače v sadě s vyrovnáváním zatížení, klikněte na **Koncové body** a potom na **Přidat**.
9. Na stránce **Přidat koncový bod do virtuálního počítače** klikněte na **Přidat koncový bod do existující sady s vyrovnáváním zatížení**, vyberte název sady s vyrovnáváním zatížení a potom klikněte na šipku doprava.
10. Na stránce **Zadat podrobnosti o koncovém bodu** zadejte název koncového bodu a potom klikněte na značku zaškrtnutí.

U dalších virtuálních počítačů v sadě s vyrovnáváním zatížení opakujte kroky 8-10.

## <a name="next-steps"></a>Další kroky

[Začínáme s konfigurací interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurace distribučního režimu nástroje pro vyrovnávání zatížení](load-balancer-distribution-mode.md)

[Konfigurace nastavení časového limitu nečinnosti protokolu TCP pro nástroj pro vyrovnávání zatížení](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Nov16_HO2-->


