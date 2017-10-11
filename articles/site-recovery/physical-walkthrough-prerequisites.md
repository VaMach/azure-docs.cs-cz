---
title: "Požadavky fyzického serveru na Azure replikaci s využitím Azure Site Recovery | Microsoft Docs"
description: "Shrnuje požadavky na replikaci úlohy běžící na fyzických serverech Windows nebo Linuxem do Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 5f5cb4b9b6fcee6b56ccdcb6223afddd9de90fec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="step-2-review-the-prerequisites-for-physical-server-to-azure-replication"></a>Krok 2: Přečtěte si požadavky pro fyzický server na Azure replikaci

Přečtěte si požadavky shrnuto v tabulce.


**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | Další informace o [požadavky pro Azure](site-recovery-prereq.md#azure-requirements)
**Lokální konfigurační server** | Je třeba fyzického serveru (nebo virtuálních počítačů VMware) s Windows serverem 2012 R2 nebo novější. Nastavit tento server během nasazování Site Recovery.<br/><br/> Ve výchozím nastavení jsou procesový server a hlavní cílový server také nainstalované na tomto počítači. Pokud jste vertikální navýšení kapacity, bude pravděpodobně třeba samostatný procesový server. V takovém případě má stejné požadavky jako konfigurační server.<br/><br/> [Další informace](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Místní virtuální počítače** | Počítače, které chcete replikovat by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL** | Konfigurační server potřebuje přístup k tyto adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.<br/></br> Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).<br/><br/> Povolit tuto adresu URL pro stažení MySQL: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Služba mobility** | Nainstalovat na každý server replikované.




## <a name="limitations"></a>Omezení

Poznámka: omezení shrnuto v tabulce před nasazením.

**Omezení** | **Podrobnosti**
--- | ---
**Azure** | Účty úložiště a sítě musí být ve stejné oblasti jako trezor<br/><br/> Pokud používáte účet úložiště premium, potřebujete účet standardního úložiště pro ukládání protokolů replikace<br/><br/> Nelze se replikují na prémiové účty ve střední a – Jih, Indie.
**Lokální konfigurační server** | Pokud používáte jako počítač serveru konfigurace virtuálního počítače VMware, musí být typ adaptéru virtuálního počítače VMware VMXNET3. Pokud tomu tak není, [tuto aktualizaci nainstalovat](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> Pro virtuální počítače VMware by měly být nainstalovány vSphere PowerCLI 6.0.<br/><br> Tento počítač by neměl být řadič domény.<br/><br/> Tento počítač by měl mít statickou IP adresu.<br/><br/> Název hostitele musí být maximálně 15 znaků nebo méně, a operační systém by měl být v angličtině.
**Replikovat počítače** | Ověřte [omezení virtuálního počítače Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nelze replikovat virtuální počítače s šifrované disky nebo virtuální počítače pomocí rozhraní UEFI nebo EFI.<br/><br> Sdílený disk, clustery nejsou podporované. Pokud virtuální počítač má zdroj seskupování síťových adaptérů, jsou převedeny na jednu síťovou kartu po převzetí služeb při selhání.<br/><br/> Pokud virtuální počítače iSCSI disk, Site Recovery převede jej na soubor virtuálního pevného disku po převzetí služeb při selhání. Pokud cíle iSCSI, dosažitelný z virtuálního počítače Azure, se k němu připojuje a zobrazí se i virtuální pevný disk. Pokud k tomu dojde, odpojte cíle iSCSI.<br/><br/> Pokud chcete povolit konzistence více virtuálních počítačů, která umožňuje virtuální počítače běží stejné zatížení dohromady obnovit konzistentní datový bod, otevřete port 20004 ve virtuálním počítači.<br/><br/> Windows musí být nainstalován na jednotce C. Disk operačního systému musí být základní a nikoli dynamické. Datový disk může být dynamické.<br/><br/> Soubory Linux/etc/hosts na virtuálních počítačích by měly obsahovat položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry. Název hostitele, přípojné body, název zařízení, systémové cesty a názvy souborů (/ etc; USR) by měly být pouze v angličtině.<br/><br/> Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.<br/><br/>Vytvořit nebo nastavit **disk.enableUUID=true** v nastavení virtuálního počítače. To poskytuje konzistentní UUID VMDK, tak, aby správně připojí a zajišťuje, že jenom rozdílové změny jsou přenášeny zpět na místní během navrácení služeb po obnovení bez úplná replikace.


## <a name="next-steps"></a>Další kroky

- Pokud jste to úplné nasazení, přejděte na [krok 3: plánování kapacity](physical-walkthrough-capacity.md)
- Pokud jste to jednoduchá testovací nasazení, přejděte na [krok 4: plánování sítě](physical-walkthrough-network.md).
