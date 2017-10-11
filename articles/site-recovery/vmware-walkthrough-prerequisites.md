---
title: "Předpoklady pro VMware do Azure replikaci s využitím Azure Site Recovery | Microsoft Docs"
description: "Shrnuje požadavky na replikaci úlohy běžící na virtuálních počítačích VMware do Azure pomocí služby Azure Site Recovery."
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
ms.openlocfilehash: 7a82e58d9ff9208130c43fcd11d03dcc3238696a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-2-review-the-prerequisites-for-vmware-to-azure-replication"></a>Krok 2: Přečtěte si požadavky pro VMware do Azure replikace

Přečtěte si požadavky shrnuté v následující tabulce.

**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | Další informace o [požadavky pro Azure](site-recovery-prereq.md#azure-requirements)
**Lokální konfigurační server** | Je třeba virtuální počítače VMware s Windows serverem 2012 R2 nebo novější. Nastavit tento server během nasazování Site Recovery.<br/><br/> Ve výchozím nastavení jsou procesový server a hlavní cílový server také nainstalované na tomto virtuálním počítači. Pokud jste vertikální navýšení kapacity, možná budete muset samostatný procesový server a má stejné požadavky jako konfigurační server.<br/><br/> Další informace o těchto součástí [sem](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Na místní servery VMware** | Jeden nebo více VMware vSphere serverů, 6.5, 6.0, 5.5, 5.1 s nejnovější aktualizace. Servery musí nacházet ve stejné síti jako konfigurační server (nebo samostatný procesový server).<br/><br/> Doporučujeme, abyste server vCenter pro správu hostitele, kde běží verze 6.5, 6.0 nebo 5.5 s nejnovějšími aktualizacemi.
**Místní virtuální počítače** | Na virtuálních počítačích, které chcete replikovat, musí běžet [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a počítače musí splňovat [požadavky na Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Virtuální počítač by měl mít spuštění nástroje VMware.
**Adresy URL** | Konfigurační server potřebuje přístup k tyto adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.<br/></br> Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).<br/><br/> Povolit tuto adresu URL pro stažení MySQL: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Služba mobility** | Nainstalovat na každý replikovaných virtuálních počítačů.




## <a name="limitations"></a>Omezení

Ujistěte se, že rozumíte omezení shrnuto v tabulce před nasazením.

**Omezení** | **Podrobnosti**
--- | ---
**Azure** | Účty úložiště a sítě musí být ve stejné oblasti jako trezor<br/><br/> Pokud používáte účet úložiště premium, potřebujete účet standardního úložiště pro ukládání protokolů replikace<br/><br/> Nelze se replikují na prémiové účty ve střední a – Jih, Indie.
**Lokální konfigurační server** | Typ adaptéru virtuálního počítače VMware musí být VMXNET3. Pokud tomu tak není, [tuto aktualizaci nainstalovat](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> vSphere PowerCLI 6.0 by měly být nainstalovány.<br/><br> Tento počítač by neměl být řadič domény. Tento počítač by měl mít statickou IP adresu.<br/><br/> Název hostitele musí být maximálně 15 znaků nebo méně, a operační systém by měl být v angličtině.
**VMware** | Site Recovery nepodporuje nové vCenter a vSphere 6.5 a 6.0 funkce, jako křížové řešení vMotion vCenter, virtuální svazky a úložiště DRS.
**Virtuální počítače** | Ověřte [omezení virtuálního počítače Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nelze replikovat virtuální počítače s šifrované disky nebo virtuální počítače pomocí rozhraní UEFI nebo EFI.<br/><br> Sdílený disk, clustery nejsou podporované. Pokud virtuální počítač má zdroj seskupování síťových adaptérů, jsou převedeny na jednu síťovou kartu po převzetí služeb při selhání.<br/><br/> Pokud virtuální počítače iSCSI disk, Site Recovery převede jej na soubor virtuálního pevného disku po převzetí služeb při selhání. Pokud cíle iSCSI, dosažitelný z virtuálního počítače Azure, se k němu připojuje a zobrazí se i virtuální pevný disk. Pokud k tomu dojde, odpojte cíle iSCSI.<br/><br/> Pokud chcete povolit konzistence více virtuálních počítačů, která umožňuje virtuální počítače běží stejné zatížení dohromady obnovit konzistentní datový bod, otevřete port 20004 ve virtuálním počítači.<br/><br/> Windows musí být nainstalován na jednotce C. Disk operačního systému musí být základní a nikoli dynamické. Datový disk může být dynamické.<br/><br/> Soubory Linux/etc/hosts na virtuálních počítačích by měly obsahovat položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry. Název hostitele, přípojné body, název zařízení, systémové cesty a názvy souborů (/ etc; USR) by měly být pouze v angličtině.<br/><br/> Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.<br/><br/>Vytvořit nebo nastavit **disk.enableUUID=true** v nastavení virtuálního počítače. To poskytuje konzistentní UUID VMDK, tak, aby správně připojí a zajišťuje, že jenom rozdílové změny jsou přenášeny zpět na místní během navrácení služeb po obnovení bez úplná replikace.


## <a name="next-steps"></a>Další kroky

- Pokud jste to úplné nasazení, přejděte na [krok 3: plánování kapacity](vmware-walkthrough-capacity.md)
- Pokud jste to jednoduchá testovací nasazení, přejděte na [krok 4: plánování sítě](vmware-walkthrough-network.md).
