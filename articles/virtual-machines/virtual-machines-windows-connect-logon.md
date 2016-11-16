---
title: "Připojení k virtuálnímu počítači s Windows Serverem | Dokumentace Microsoftu"
description: "Zjistěte, jak se připojit a přihlásit k virtuálnímu počítači s Windows pomocí portálu Azure a modelu nasazení s využitím Resource Manageru."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: db417fb72442ea8a5cd4ef882eb657b08bebaa0a


---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Jak se připojit a přihlásit k virtuálnímu počítači s Windows v Azure
Pomocí tlačítka **Připojit** na Portálu Azure spustíte relaci Vzdálené plochy (protokol RDP). Nejdřív se k virtuálnímu počítači připojíte a pak se k němu přihlásíte.

## <a name="connect-to-the-virtual-machine"></a>Připojení k virtuálnímu počítači
1. Pokud jste to ještě neudělali, přihlaste se k [Portálu Azure](https://portal.azure.com/).
2. V nabídce centra klikněte na **Virtuální počítače**.
3. Ze seznamu vyberte virtuální počítač.
4. V okně pro virtuální počítač klikněte na **Připojit**.
   
    ![Snímek obrazovky Portálu Azure znázorňující způsob připojení k virtuálnímu počítači](./media/virtual-machines-windows-connect-logon/connect.png)
   
   > [!TIP]
   > Pokud je tlačítko **Připojit** na portálu zašedlé a nejste připojení k Azure přes [Express Route](../expressroute/expressroute-introduction.md) nebo připojení [S2S (Site-to-site) VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), musíte před použitím protokolu RDP vytvořit veřejnou IP adresu a přiřadit ji svému virtuálnímu počítači. Podle potřeby si můžete přečíst další informace o [veřejných IP adresách v Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a>Přihlášení k virtuálnímu počítači
[!INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Další kroky
Pokud při připojování nastanou problémy, přečtěte si článek [Řešení problémů s připojením ke Vzdálené ploše](virtual-machines-windows-troubleshoot-rdp-connection.md). Tento článek vás provede diagnostikou a řešením běžných problémů.




<!--HONumber=Nov16_HO2-->


