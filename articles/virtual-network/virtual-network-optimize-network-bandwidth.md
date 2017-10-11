---
title: "Optimalizovat propustnost sítě virtuálních počítačů | Microsoft Docs"
description: "Zjistěte, jak optimalizovat propustnost sítě virtuálního počítače Azure."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: steveesp
ms.openlocfilehash: 914747983d4d974810836be66d6c6af343f58b60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-network-throughput-for-azure-virtual-machines"></a>Optimalizovat propustnost sítě pro virtuální počítače Azure

Azure virtuální počítače (VM) mají výchozí nastavení sítě, které lze dále optimalizovat pro propustnost sítě. Tento článek popisuje, jak optimalizovat propustnost sítě pro Microsoft Azure Windows a virtuální počítače s Linuxem, včetně hlavních distribuce například Ubuntu a CentOS Red Hat.

## <a name="windows-vm"></a>Virtuální počítač s Windows

Pokud vaše virtuální počítač s Windows je podporovaná s [Accelerated sítě](virtual-network-create-vm-accelerated-networking.md), povolení této funkce by být optimální konfigurace propustnost. Pro všechny ostatní virtuální počítače Windows škálování na straně příjmu (RSS) pomocí dosáhnout vyšší maximální propustnost než virtuální počítač bez RSS. RSS může být nepovolený ve výchozím nastavení v systému Windows virtuálního počítače. Proveďte následující kroky k určení, zda je povoleno RSS a povolit, pokud je zakázána.

1. Zadejte `Get-NetAdapterRss` příkaz prostředí PowerShell, které chcete zobrazit, pokud byla povolená technologie RSS pro síťový adaptér. Ve výstupu v následujícím příkladu vrácená z `Get-NetAdapterRss`, RSS není povoleno.

    ```powershell
    Name                    : Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : False
    ```
2. Zadejte následující příkaz, který RSS povolte:

    ```powershell
    Get-NetAdapter | % {Enable-NetAdapterRss -Name $_.Name}
    ```
    Předchozí příkaz nemá výstup. Příkaz změnit nastavení síťový adaptér, což by způsobilo ztrátě dočasné připojení přibližně jednu minutu. Při ztrátě připojení se zobrazí dialogové okno Reconnecting. Obvykle se obnoví připojení k po třetí pokus.
3. Potvrďte, že byla povolená technologie RSS ve virtuálním počítači tak, že zadáte `Get-NetAdapterRss` příkaz znovu. V případě úspěchu se vrátí výstupu v následujícím příkladu:

    ```powershell
    Name                    :Ethernet
    InterfaceDescription    : Microsoft Hyper-V Network Adapter
    Enabled              : True
    ```

## <a name="linux-vm"></a>Virtuální počítač s Linuxem

RSS je vždy povolena ve výchozím nastavení v systému Linux virtuálního počítače Azure. Linux jádra vydané od ledna 2017 zahrnují nové možnosti optimalizace sítě, které umožňují virtuálního počítače s Linuxem k dosažení vyšší propustnost sítě.

### <a name="ubuntu"></a>Ubuntu

Chcete-li získat optimalizace, nejprve aktualizovat na nejnovější podporovanou verzi, od června 2017, což je:
```json
"Publisher": "Canonical",
"Offer": "UbuntuServer",
"Sku": "16.04-LTS",
"Version": "latest"
```
Po dokončení aktualizace zadejte následující příkazy a získat nejnovější jádra:

```bash
apt-get -f install
apt-get --fix-missing install
apt-get clean
apt-get -y update
apt-get -y upgrade
```

Volitelný příkaz:

`apt-get -y dist-upgrade`
#### <a name="ubuntu-azure-preview-kernel"></a>Ubuntu Azure Preview jádra
> [!WARNING]
> Tato jádra Azure Linux Preview nemusí mít stejnou úroveň dostupnost a spolehlivost jako obrázky Marketplace a verzí jádra, která jsou obecné dostupnosti. Funkce není podporována, může mít omezené možnosti a nemusí být stejně spolehlivá jako výchozí jádra. Nepoužívejte tuto jádra pro úlohy v produkčním prostředí.

Nainstalováním navrhované jádra Azure Linux jde dosáhnout výrazné propustnost. Pokud chcete vyzkoušet tuto jádra, přidejte následující řádek na /etc/apt/sources.list

```bash
#add this to the end of /etc/apt/sources.list (requires elevation)
deb http://archive.ubuntu.com/ubuntu/ xenial-proposed restricted main multiverse universe
```

Potom spuštěním těchto příkazů jako kořenového příkazu.
```bash
apt-get update
apt-get install "linux-azure"
reboot
```

### <a name="centos"></a>CentOS

Chcete-li získat optimalizace, nejprve aktualizovat na nejnovější podporovanou verzi, od července 2017, což je:
```json
"Publisher": "OpenLogic",
"Offer": "CentOS",
"Sku": "7.3",
"Version": "latest"
```
Po dokončení aktualizace nainstalujte nejnovější Linux integrační služby (LIS).
Optimalizace propustnosti se LIS, od 4.2.2-2. Zadejte následující příkazy pro instalaci LIS:

```bash
sudo yum update
sudo reboot
sudo yum install microsoft-hyper-v
```

### <a name="red-hat"></a>Red Hat

Chcete-li získat optimalizace, nejprve aktualizovat na nejnovější podporovanou verzi, od července 2017, což je:
```json
"Publisher": "RedHat"
"Offer": "RHEL"
"Sku": "7.3"
"Version": "7.3.2017071923"
```
Po dokončení aktualizace nainstalujte nejnovější Linux integrační služby (LIS).
Optimalizace propustnosti se LIS, od 4.2. Zadejte následující příkazy ke stažení a instalaci LIS:

```bash
mkdir lis4.2.2-2
cd lis4.2.2-2
wget https://download.microsoft.com/download/6/8/F/68FE11B8-FAA4-4F8D-8C7D-74DA7F2CFC8C/lis-rpms-4.2.2-2.tar.gz
tar xvzf lis-rpms-4.2.2-2.tar.gz
cd LISISO
install.sh #or upgrade.sh if prior LIS was previously installed
```

Další informace o Linux integrační služby verzi 4.2 pro Hyper-V zobrazením [stránce pro stažení](https://www.microsoft.com/download/details.aspx?id=55106).

## <a name="next-steps"></a>Další kroky
* Teď, když virtuální počítač je optimalizovaná, najdete v části výsledků [šířky pásma nebo propustnost testování virtuální počítač Azure](virtual-network-bandwidth-testing.md) pro váš scénář.
* Další informace s [Azure Virtual Network nejčastější dotazy (FAQ)](virtual-networks-faq.md)
