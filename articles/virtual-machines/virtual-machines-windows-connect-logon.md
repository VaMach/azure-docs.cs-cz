<properties
    pageTitle="Připojení k virtuálnímu počítači s Windows Serverem | Microsoft Azure"
    description="Zjistěte, jak se připojit a přihlásit k virtuálnímu počítači s Windows pomocí portálu Azure a modelu nasazení s využitím Resource Manageru."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/05/2016"
    ms.author="cynthn"/>

# Jak se připojit a přihlásit k virtuálnímu počítači s Windows v Azure 


Pomocí tlačítka **Připojit** na Portálu Azure spustíte relaci Vzdálené plochy (protokol RDP). Nejdřív se k virtuálnímu počítači připojíte a pak se k němu přihlásíte.

## Připojení k virtuálnímu počítači

1. Pokud jste to ještě neudělali, přihlaste se k [Portálu Azure](https://portal.azure.com/).

2.  V nabídce centra klikněte na **Virtuální počítače**.

3.  Ze seznamu vyberte virtuální počítač.

4. V okně pro virtuální počítač klikněte na **Připojit**.

    ![Snímek obrazovky Portálu Azure znázorňující způsob připojení k virtuálnímu počítači](./media/virtual-machines-windows-connect-logon/connect.png)
    
 > [AZURE.TIP] Pokud je tlačítko Připojit na portálu šedé (nedostupné) a nejste připojení k Azure přes [Express Route](../expressroute/expressroute-introduction.md) nebo připojení [S2S (Site-to-site) VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), musíte před použitím protokolu RDP vytvořit veřejnou IP adresu a přiřadit ji svému virtuálnímu počítači. Podle potřeby si můžete přečíst další informace o [veřejných IP adresách v Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## Přihlášení k virtuálnímu počítači

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]


## Další kroky

Pokud budete mít s připojením problémy, projděte si téma [Poradce při potížích s připojením k virtuálnímu počítači s Windows v Azure pomocí Vzdálené plochy](virtual-machines-windows-troubleshoot-rdp-connection.md). Tento článek vás provede diagnostikou a řešením běžných problémů.



<!--HONumber=Jun16_HO2-->


