---
title: "Příprava na místním serveru technologie Hyper-V pro zotavení po havárii virtuálních počítačů technologie Hyper-V do Azure | Microsoft Docs"
description: "Zjistěte, jak připravit virtuální počítače Hyper-v místě nejsou spravované přes System Center VMM pro zotavení po havárii do Azure se službou Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 02/14/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 9524ffde4a588d3ac029bc8a3df91726082e157d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="prepare-on-premises-hyper-v-servers-for-disaster-recovery-to-azure"></a>Příprava na místních serverech technologie Hyper-V pro zotavení po havárii do Azure

V tomto kurzu se dozvíte, jak připravit na místní infrastruktuře Hyper-V, pokud chcete replikovat virtuální počítače Hyper-V do Azure, za účelem zotavení po havárii. Hostitelé Hyper-V je možné spravovat pomocí System Center Virtual Machine Manager (VMM), ale není to nutné.  V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Zkontrolujte požadavky na Hyper-V a VMM požadavky, pokud je k dispozici.
> * Příprava VMM, pokud je k dispozici
> * Ověřte přístup k Internetu do Azure umístění
> * Připravit virtuální počítače tak, aby přístup je po převzetí služeb při selhání do Azure

Tento kurz je druhou částí série. Ujistěte se, že jste [nastavili komponenty Azure](tutorial-prepare-azure.md), jak je popsáno v předchozím kurzu.



## <a name="review-server-requirements"></a>Zkontrolujte požadavky na server

Zkontrolujte, zda hostitelé Hyper-V splňovat následující požadavky. Pokud spravujete hostitelů v cloudech System Center Virtual Machine Manager (VMM), zkontrolujte požadavky na nástroj VMM.


**Komponenta** | **Technologie Hyper-V spravován nástrojem VMM** | **Technologie Hyper-V bez VMM**
--- | --- | ---
**Operační systém hostitele technologie Hyper-V** | Windows Server 2016, 2012 R2 | Není k dispozici
**VMM** | VMM 2012, VMM 2012 R2 | Není k dispozici


## <a name="review-hyper-v-vm-requirements"></a>Kontrola požadavků na virtuální počítač Hyper-V

Ujistěte se, že virtuální počítač splňuje požadavky shrnuto v tabulce.

**Požadavek na virtuální počítač** | **Podrobnosti**
--- | ---
Hostovaný operační systém | Všechny hostovaný operační systém [nepodporuje v Azure](https://technet.microsoft.com/library/cc794868.aspx).
**Požadavky na Azure** | Místní, že virtuální počítače Hyper-V musí splňovat requirements(site-recovery-support-matrix-to-azure.md) virtuálního počítače Azure.

## <a name="prepare-vmm-optional"></a>Příprava VMM (volitelné)

Pokud hostitele Hyper-V jsou spravovány nástrojem VMM, je nutné připravit na místním serveru VMM. 

- Ujistěte se, že VMM server obsahuje minimálně jeden cloud, s jeden nebo více skupin hostitelů. Hostitel Hyper-V, na kterém běží virtuální počítače se musí nacházet v cloudu.
- Příprava serveru VMM pro mapování sítě.

### <a name="prepare-vmm-for-network-mapping"></a>Příprava VMM mapování sítě

Pokud používáte VMM, [mapování sítě](site-recovery-network-mapping.md) mapy mezi místními sítěmi virtuálních počítačů nástroje VMM a virtuálních sítí Azure. Mapování zajistí, že virtuální počítače Azure připojeni k správného síťového, když jste vytvořili po převzetí služeb při selhání.

Připravte VMM mapování sítě následujícím způsobem:

1. Zajistěte, aby byla [logická síť VMM](https://docs.microsoft.com/system-center/vmm/network-logical) který je spojen s cloudem, ve které se nacházejí hostitelů Hyper-V.
2. Zajistěte, abyste měli [síť virtuálních počítačů](https://docs.microsoft.com/system-center/vmm/network-virtual) propojené k logické síti.
3. V nástroji VMM připojte virtuální počítače k síti virtuálních počítačů.

## <a name="verify-internet-access"></a>Ověřte přístup k Internetu

1. Pro účely tohoto kurzu je nejjednodušší konfiguraci pro hostitele Hyper-V a VMM server, pokud je k dispozici, umožňuje mít přímý přístup k Internetu bez použití proxy serveru. 
2. Ujistěte se, který je hostitelem technologie Hyper-V a na serveru VMM, pokud je to relevantní, můžete přístup tyto adresy URL: 

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
3. Zajistěte, aby:
    - Všechna pravidla brány firewall založená na adresu IP by měl povolit komunikaci s Azure.
    - Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).
    - Povolte rozsahy IP adres pro oblast Azure svého předplatného a západní USA (používá se pro přístup k řízení a identity management).


## <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Příprava připojení k virtuálním počítačům Azure po převzetí služeb při selhání

Během převzetí služeb při selhání scénáři můžete připojit k replikované do místní sítě.

Pokud se po převzetí služeb při selhání chcete připojit k virtuálním počítačům s Windows pomocí protokolu RDP, postupujte následovně:

1. Pokud chcete mít přístup přes internet, před převzetím služeb při selhání povolte na místním virtuálním počítači protokol RDP. Ujistěte se, že jsou přidaná pravidla TCP a UDP pro **Veřejný** profil a že v části **Brána Windows Firewall** > **Povolené aplikace** je povolený protokol RDP pro všechny profily.
2. Pokud chcete mít přístup přes síť VPN typu Site-to-Site, povolte na místním počítači protokol RDP. Protokol RDP musí být povolený v části **Brána Windows Firewall** -> **Povolené aplikace a funkce** pro **doménovou a privátní** síť.
   Zkontrolujte, že je zásada SAN operačního systému nastavená na **OnlineAll**. [Další informace](https://support.microsoft.com/kb/3031135). Při aktivaci převzetí služeb při selhání by na virtuálním počítači neměly být žádné čekající aktualizace Windows. Jinak se nebudete moci k virtuálnímu počítači přihlásit, dokud se aktualizace nedokončí.
3. Po převzetí služeb při selhání na virtuálním počítači Azure s Windows zkontrolujte **diagnostiku spuštění**, kde se zobrazí snímek obrazovky virtuálního počítače. Pokud se nemůžete připojit, zkontrolujte, že je virtuální počítač spuštěný, a přečtěte si tyto [tipy pro řešení potíží](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Nastavení zotavení po havárii do Azure pro virtuální počítače Hyper-V](tutorial-hyper-v-to-azure.md)
> [nastavení zotavení po havárii do Azure pro virtuální počítače Hyper-V v cloudech VMM](tutorial-hyper-v-vmm-to-azure.md)
