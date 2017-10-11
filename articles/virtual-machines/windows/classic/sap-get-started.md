---
title: "Pomocí SAP na virtuálních počítačích s Windows | Microsoft Docs"
description: "Zrušte o používání SAP na Windows virtuální počítače (VM) v Microsoft Azure"
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: MSSedusch
manager: timlt
editor: 
tags: azure-service-management
keywords: 
ms.assetid: 1b455be4-c02f-43e3-8d39-c2d5f216e646
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: campaign-page
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/04/2016
ms.author: sedusch
ms.openlocfilehash: 1d6326d5f79ea4e975abd1aa90b0d4a635f4c31a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Pomocí SAP na virtuálních počítačích s Windows v Azure
Cloud Computing je často používaný termín, který získává v oblasti IT čím dál větší význam – od malých firem až po velké a nadnárodní společnosti. Microsoft Azure je platforma cloudových služeb od společnosti Microsoft, která nabízí široké spektrum nových možností. Nyní mohou zákazníci rychle zřizovat a zrušit aplikace jako cloudové služby, takže už nejsou omezeni technickými nebo rozpočtovými limity. Namísto investování času a prostředků do hardwarové infrastruktury se společností můžou zaměřit na aplikace, obchodní procesy a jejich výhody pro zákazníky a uživatele.

S virtuálními počítači Microsoft Azure společnost Microsoft nabízí komplexní infrastruktury jako služby (IaaS) platformu. Služba Azure Virtual Machines teď v rámci IaaS podporuje aplikace využívající SAP NetWeaver. Dokumenty White Paper níže popisují, jak naplánovat a implementovat aplikace SAP NetWeaver založené na virtuálních počítačích s Windows v Azure. Můžete taky implementovat SAP NetWeaver na základě aplikací na [virtuální počítače s Linuxem](../../linux/classic/sap-get-started.md).

[!INCLUDE [virtual-machines-common-classic-sap-get-started](../../../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver ve službě Azure - HA
Title: SAP NetWeaver ve službě Azure - Clustering SAP ASC nebo SCS instancí pomocí clusteru převzetí služeb při selhání systému Windows Server v Azure s DataKeeper s

Souhrn: ' Tento dokument popisuje, jak nastavit konfiguraci s vysokou dostupností SAP ASC nebo SCS v Azure pomocí DataKeeper s. SAP chrání jejich jediný bod selhání komponenty, jako jsou služby zařazování replikace nebo SAP ASC nebo SCS s konfigurace clusteru převzetí služeb při selhání systému Windows Server, které vyžadují sdílené disky. Tyto součásti SAP jsou nezbytné pro funkce systému SAP. Proto je funkce vysoké dostupnosti zavede a ujistěte se, že tyto součásti tolerovat selhání serveru nebo na virtuální počítač, protože dokončení konfigurace clusteru se systémem Windows pro úplné obnovení a prostředí Hyper-V. K srpnu 2015 nemůže Azure sám na sobě poskytovat vysoce dostupné konfigurace požadované pro tyto důležité součásti SAP základě sdílených disků, které by byla zapotřebí pro Windows. Ale za pomoci produktu DataKeeper podle SIOS konfigurace clusteru převzetí služeb při selhání Windows serveru podle potřeby pro SAP ASC nebo SCS se dají vytvářet na platformě Azure IaaS. Tento dokument popisuje v kroku krok přístupu k instalaci konfiguraci clusteru převzetí služeb při selhání systému Windows Server pomocí sdíleného disku poskytované s Datakeeper v Azure. Dokumentu vysvětluje podrobnosti v konfigurace na straně Azure, Windows a SAP, které usnadňují konfiguraci vysoké dostupnosti pracovních optimální způsobem. Dokumentu doplňuje SAP instalace dokumentace a poznámky k SAP, které představují primární prostředky pro instalace a nasazení SAP softwaru na dané platformy.

Aktualizované: Srpen 2015

[Stáhnout teď tuto příručku](http://go.microsoft.com/fwlink/?LinkId=613056)

