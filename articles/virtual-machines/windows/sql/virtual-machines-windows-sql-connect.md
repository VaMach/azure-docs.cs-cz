---
title: "Připojení k virtuálnímu počítači s SQL serverem (Resource Manager) | Microsoft Docs"
description: "Zjistěte, jak se připojit k serveru SQL, které jsou spuštěny na virtuálním počítači v Azure. Toto téma používá model nasazení classic. Scénáře se liší v závislosti na konfiguraci sítě a umístění klienta."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 67ba43f9456bbeffbf602067586143c4c68af672
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Připojení k virtuálnímu počítači s SQL Serverem v Azure (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-connect.md)
> * [Classic](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Přehled

Toto téma popisuje, jak se připojit k instanci systému SQL Server spuštěna na virtuálním počítači Azure. Některé pokrývá [obecné připojení scénáře](#connection-scenarios) a pak poskytuje [podrobné kroky pro konfiguraci připojení k systému SQL Server ve virtuálním počítači Azure](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Pokud máte by místo úplné návodu zřizování a připojení, najdete v části [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Scénáře připojení

Způsob, jakým se klient připojí k serveru SQL Server spuštěny na virtuálním počítači se liší v závislosti na umístění klienta a konfigurace sítí.

Pokud zřídíte virtuální počítač SQL Server na portálu Azure, máte možnost zadat typ **připojení SQL**.

![Veřejné možnosti připojení SQL během zřizování](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Možnosti připojení patří:

| Možnost | Popis |
|---|---|
| **Veřejné** | Připojení k systému SQL Server přes internet |
| **Privátní** | Připojení k systému SQL Server ve stejné virtuální síti |
| **Místní** | Připojení k systému SQL Server lokálně na jednom virtuálním počítači | 

Následující části popisují **veřejné** a **privátní** možnosti podrobněji.

## <a name="connect-to-sql-server-over-the-internet"></a>Připojení k systému SQL Server přes Internet

Pokud se chcete připojit k vaší databázový stroj SQL Server z Internetu, vyberte **veřejné** pro **připojení SQL** typ na portálu během zřizování. Na portálu automaticky provede následující kroky:

* Umožňuje protokol TCP/IP pro SQL Server.
* Nakonfiguruje pravidlo brány firewall pro otevření portu TCP systému SQL Server (standardně 1433).
* Umožňuje ověřování systému SQL Server, vyžaduje se pro veřejný přístup.
* Nakonfiguruje skupinu zabezpečení sítě na virtuálním počítači pro všechny přenosy TCP na portu SQL Server.

> [!IMPORTANT]
> Bitové kopie virtuálních počítačů pro SQL Server Developer a edice Express automaticky nepovolujte protokol TCP/IP. U edice Developer a Express, je nutné použít Správce konfigurace systému SQL Server na [ručně povolit protokol TCP/IP](#manualtcp) po vytvoření virtuálního počítače.

Libovolného klienta s přístupem k Internetu může připojit k instanci systému SQL Server zadáním buď veřejnou IP adresu virtuálního počítače nebo libovolný popisek DNS přiřazené tuto IP adresu. Pokud port serveru SQL Server je 1433, není potřeba zadat v připojovacím řetězci. Následující připojovací řetězec připojuje k virtuálnímu počítači SQL s jmenovkou DNS `sqlvmlabel.eastus.cloudapp.azure.com` pomocí ověřování SQL (můžete také použít veřejné IP adresy).

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

I když to umožňuje připojení klientů přes internet, to neznamená, že každý, kdo může připojit k systému SQL Server. Klienti nemusí mimo správné uživatelské jméno a heslo. Pro dodatečné zabezpečení se však vyhnout dobře známého portu 1433. Například pokud jste nakonfigurovali SQL serveru pro naslouchání na portu 1500 a zavedené správné brány firewall a pravidel skupiny zabezpečení sítě, může připojení připojením číslo portu na název serveru. Následující příklad mění předchozímu přidáním vlastní číslo portu, **1 500**, název serveru:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> Když dotazujete systému SQL Server ve virtuálním počítači přes internet, všechny odchozí data z datové centrum Azure podléhá normální [ceny na odchozí přenosy dat](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-to-sql-server-within-a-virtual-network"></a>Připojení k systému SQL Server v rámci virtuální sítě

Pokud vyberete **privátní** pro **připojení SQL** typ na portálu Azure nakonfiguruje většinu nastavení identické **veřejné**. Jeden rozdíl je, že neexistuje žádná pravidla skupiny zabezpečení sítě umožňuje mimo přenosy na portu systému SQL Server (standardně 1433).

> [!IMPORTANT]
> Bitové kopie virtuálních počítačů pro SQL Server Developer a edice Express automaticky nepovolujte protokol TCP/IP. U edice Developer a Express, je nutné použít Správce konfigurace systému SQL Server na [ručně povolit protokol TCP/IP](#manualtcp) po vytvoření virtuálního počítače.

Privátní připojení se často používá ve spojení s [virtuální sítě](../../../virtual-network/virtual-networks-overview.md), což umožňuje několik scénářů. I když tyto virtuální počítače existovat v různých skupinách prostředků se můžete připojit virtuální počítače ve stejné virtuální síti. A s [site-to-site VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), můžete vytvořit hybridní architekturu, která připojí virtuální počítače s místními sítěmi a počítače.

Virtuální sítě umožňují připojit virtuální počítače Azure k doméně. Toto je jediným způsobem, jak použít ověřování systému Windows na serveru SQL Server. Jiné připojení scénáře vyžadují ověřování SQL pomocí uživatelského jména a hesla.

Za předpokladu, že nakonfigurujete DNS ve virtuální síti, můžete se připojit k instanci systému SQL Server zadáním názvu počítače virtuální počítač s SQL serverem v připojovacím řetězci. Následující příklad předpokládá také ověřování systému Windows také nakonfigurován a že uživatel byl udělen přístup k instanci systému SQL Server.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a>Změňte nastavení připojení k SQL

Můžete změnit nastavení připojení pro virtuální počítač systému SQL Server na portálu Azure.

1. Na portálu Azure vyberte **virtuální počítače**.

2. Vyberte virtuální počítač systému SQL Server.

3. V části **nastavení**, klikněte na tlačítko **konfigurace systému SQL Server**.

4. Změna **úroveň připojení SQL** na požadované nastavení. Volitelně můžete této oblasti změnit port serveru SQL Server nebo nastavení ověřování SQL.

   ![Změnit připojení SQL](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Počkejte několik minut na dokončení aktualizace.

   ![Virtuální počítač SQL aktualizace oznámení](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a>Povolte protokol TCP/IP pro edice Developer a Express

Při změně nastavení připojení k systému SQL Server, Azure automaticky neumožňuje protokol TCP/IP pro vývojáře serveru SQL a edice Express. Následující kroky popisují ruční povolení protokolu TCP/IP, abyste se mohli vzdáleně připojit pomocí IP adresy.

Nejdřív připojte k počítači serveru SQL Server pomocí vzdálené plochy.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Dál povolte protokol TCP/IP s **SQL Server Configuration Manager**.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>Připojení přes SSMS

Následující kroky ukazují, jak vytvořit popisek volitelné DNS pro virtuální počítač Azure a potom se připojte pomocí SQL Server Management Studio (SSMS).

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Další kroky

Zřizování pokyny společně s postupem připojení najdete v tématu [zřizování virtuálního počítače systému SQL Server na platformě Azure](virtual-machines-windows-portal-sql-server-provision.md).

Další témata související se systémem SQL Server ve virtuálních počítačích Azure, najdete v části [systému SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-server-iaas-overview.md).