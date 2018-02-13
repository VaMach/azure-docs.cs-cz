---
title: "Příprava na místní servery VMware pro zotavení po havárii virtuálních počítačů VMware do Azure | Microsoft Docs"
description: "Zjistěte, jak připravit místní servery VMware pro zotavení po havárii do Azure pomocí služby Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 90a4582c-6436-4a54-a8f8-1fee806b8af7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: af09c5602c53be4377ba19e68ff3486bcfefe0ea
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="prepare-on-premises-vmware-servers-for-disaster-recovery-to-azure"></a>Příprava na místní servery VMware pro zotavení po havárii do Azure

V tomto kurzu se dozvíte, jak připravit na místní infrastruktuře VMware, pokud chcete replikovat virtuální počítače VMware do Azure. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Příprava účtu v systému vCenter server nebo vSphere ESXi hostiteli, a automatizovat zjišťování virtuálních počítačů
> * Příprava účtu pro automatické instalace služby Mobility na virtuálních počítačích VMware
> * Zkontrolujte požadavky na server VMware
> * Kontrola požadavků na virtuální počítač VMware

Tento kurz série ukážeme, jak zálohovat jeden virtuální počítač pomocí Azure Site Recovery. Pokud máte v úmyslu Ochrana více virtuálních počítačů VMware, byste měli stahovat [nástroj pro nasazení Planner](https://aka.ms/asr-deployment-planner) pro replikaci VMware. Tento nástroj shromažďovat informace o kompatibilitě virtuálních počítačů, disky na virtuální počítač a změn dat na disk. Nástroj platí i pro nároky na šířku pásma sítě a infrastrukturu Azure, které jsou potřebné pro úspěšné replikace a testovací převzetí služeb při selhání. [Další informace](site-recovery-deployment-planner.md) o spuštění nástroje.

Toto je druhý kurz v řadě. Ujistěte se, že máte [nastavit Azure komponenty](tutorial-prepare-azure.md) jak je popsáno v předchozí kurzu.

## <a name="prepare-an-account-for-automatic-discovery"></a>Příprava účtu pro automatické zjišťování

Site Recovery potřebuje přístup k serverům VMware:

- Automaticky vyhledá virtuální počítače. Je požadován alespoň účet jen pro čtení.
- Orchestraci replikace, převzetí služeb při selhání a navrácení služeb po obnovení. Je třeba účet, který můžete spustit operací, jako je vytváření a odebrání disků a pohánějící na virtuálních počítačích.

Vytvořte účet následujícím způsobem:

1. Pokud chcete použít vyhrazený účet, vytvořte roli na úrovni vCenter. Zadejte například název role **Azure_Site_Recovery**.
2. Přiřazení role oprávnění shrnuté v následující tabulce.
3. Vytvořte uživatele na hostiteli systému vCenter server nebo vSphere. Přiřazení role uživatele.

### <a name="vmware-account-permissions"></a>Oprávnění pro uživatelský účet VMware

**Úkol** | **Role nebo oprávnění** | **Podrobnosti**
--- | --- | ---
**Zjišťování virtuálních počítačů** | Uživatel alespoň jen pro čtení<br/><br/> Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).
**Úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení** |  Umožňuje vytvořit roli (Azure_Site_Recovery) s požadovanými oprávněními a potom přiřadit roli VMware uživatele nebo skupinu<br/><br/> Objekt datového centra –> Propagate pro podřízený objekt role = Azure_Site_Recovery<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Zdroj -> Přiřazení virtuálního počítače do fondu zdrojů, migrovat napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).

## <a name="prepare-an-account-for-mobility-service-installation"></a>Příprava účet pro instalaci služby Mobility

Služba Mobility musí být nainstalovaná na virtuálním počítači, které chcete replikovat. Site Recovery tato služba nainstaluje automaticky při povolení replikace pro virtuální počítač. Pro automatické instalaci je nutné připravit účet, který Site Recovery bude používat pro přístup k virtuálnímu počítači. Tento účet budete zadat při nastavování zotavení po havárii v konzole Azure.

1. Příprava domény nebo místní účet s oprávněními k instalaci na virtuálním počítači.
2. Chcete-li nainstalovat na virtuálních počítačích Windows, pokud nepoužíváte účet domény, zakažte řízení vzdáleného přístupu uživatele v místním počítači.
   - Z registru v části **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy**, s hodnotou 1.
3. Pokud chcete nainstalovat na virtuální počítače s Linuxem, připravte kořenový účet na zdrojovém serveru Linux.


## <a name="check-vmware-server-requirements"></a>Zkontrolujte požadavky na server VMware

Ujistěte se, že servery VMware splňovat následující požadavky.

**Komponenta** | **Požadavek**
--- | ---
**vCenter server** | vCenter 6.5, 6.0 nebo 5.5
**hostitelů vSphere** | vSphere 6.5, 6.0, 5.5

## <a name="check-vmware-vm-requirements"></a>Zkontrolujte požadavky na virtuální počítač VMware

Ujistěte se, že virtuální počítač splňuje požadavky na Azure shrnuté v následující tabulce.

**Požadavek na virtuální počítač** | **Podrobnosti**
--- | ---
**Velikost disku operačního systému** | Až 2048 GB.
**Počet disků operačního systému** | 1
**Počet datových disků** | 64 nebo méně
**Velikost datového disku virtuálního pevného disku** | Až 4095 GB
**Síťové adaptéry** | Několik adaptérů jsou podporovány.
**Sdílený virtuální pevný disk** | Nepodporuje se
**FC disku** | Nepodporuje se
**Formát pevného disku** | VHD nebo VHDX.<br/><br/> I když VHDX není aktuálně podporovaná v Azure, Site Recovery automaticky převede VHDX virtuálního pevného disku při selhání do Azure. Pokud selžou zpět na místní virtuální počítače nadále používat formát VHDX.
**Bitlocker** | Nepodporuje se. Zakážete, než povolíte replikaci pro virtuální počítač.
**Název virtuálního počítače.** | 1 až 63 znaků.<br/><br/> Omezen na písmena, číslice a pomlčky. Název virtuálního počítače musí začínat a končit písmenem nebo číslicí.
**Typ virtuálního počítače** | Generace 1 - Linux nebo Windows<br/><br/>Generace 2 – pouze v systému Windows

Virtuální počítač musí taky používat podporovaný operační systém. Najdete v článku [matici podpory Site Recovery](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) úplný seznam podporovaných verzí.

## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Příprava připojení k virtuálním počítačům Azure po převzetí služeb při selhání

Během převzetí služeb při selhání scénáři můžete připojit k replikované virtuální počítače v Azure z vaší místní sítě.

Pro připojení k virtuálním počítačům systému Windows pomocí protokolu RDP po převzetí služeb při selhání, postupujte takto:

1. Pro přístup přes internet, povolení protokolu RDP pro virtuální počítač na místě před převzetí služeb při selhání. Ujistěte se, že TCP a UDP pravidla jsou přidány k **veřejné** profilu a že je v povolené RDP **brány Windows Firewall** > **povolené aplikace** pro všechny profily.
2. Pro přístup prostřednictvím sítě site-to-site VPN, povolte RDP na místním počítači. RDP má být povoleno v **brány Windows Firewall** -> **povolené aplikace a funkce** pro **domény a privátní** sítě.
   Zkontrolujte, jestli zásada SAN operačního systému je nastavena na **OnlineAll**. [Další informace](https://support.microsoft.com/kb/3031135). Měla by existovat žádné Windows aktualizace čekající na vyřízení ve virtuálním počítači při aktivaci převzetí služeb při selhání. Pokud existují, nebudete moct přihlásit k virtuálnímu počítači, dokud se nedokončí aktualizace.
3. Windows Azure virtuálního počítače po převzetí služeb při selhání, zkontrolujte **spouštění diagnostiky** zobrazíte snímek virtuálního počítače. Pokud se nemůžete připojit, zkontrolujte, zda je virtuální počítač spuštěný a zkontrolujte tyto [tipy pro odstraňování potíží](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Pro připojení k virtuální počítače s Linuxem pomocí protokolu SSH po převzetí služeb při selhání, postupujte takto:

1. Na místním počítači před převzetí služeb při selhání zkontrolujte, že služba Secure Shell nastavena na automatické spuštění při spuštění systému. Zkontrolujte, jestli pravidla brány firewall umožňují připojení SSH.

2. Na virtuálním počítači Azure po převzetí služeb při selhání povolte příchozí připojení k portu SSH pro pravidel skupiny zabezpečení sítě na selhání virtuálních počítačů a v podsíti Azure, ke kterému je připojený.
   [Přidejte veřejnou IP adresu](site-recovery-monitoring-and-troubleshooting.md) pro virtuální počítač. Můžete zkontrolovat **spouštění diagnostiky** zobrazíte snímek virtuálního počítače.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Nastavit zotavení po havárii do Azure pro virtuální počítače VMware](tutorial-vmware-to-azure.md)
