---
title: "Azure Disk Encryption – nejčastější dotazy | Microsoft Docs"
description: "Tento článek obsahuje odpovědi na nejčastější dotazy k Microsoft Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux."
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: devtiw;ejarvi;mayank88mahajan;vermashi;sudhakarareddyevuri;aravindthoram
ms.openlocfilehash: f3cb11bc13ad2561e62c096d840c0b8e4471d271
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="azure-disk-encryption-faq"></a>Nejčastější dotazy k Azure Disk Encryption

Tento článek obsahuje odpovědi na nejčastější dotazy (FAQ) o Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux. Další informace o této službě najdete v tématu [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="general-questions"></a>Obecné otázky
**Otázka:** je-li Azure Disk Encryption obecné dostupnosti (GA)?

**Odpověď:** Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux je obecně dostupnosti ve všech oblastech Azure veřejné.

**Otázka:** vyskytne uživatele, jsou k dispozici s Azure Disk Encryption?

**Odpověď:** Azure Disk Encryption GA podporuje šablony Azure Resource Manager, prostředí Azure PowerShell a rozhraní příkazového řádku Azure. To vám přináší značnou flexibilitu. Máte tři různé možnosti pro povolení šifrování disku pro virtuální počítače IaaS. Další informace o uživatelské rozhraní a podrobné pokyny k dispozici v Azure Disk Encryption najdete v tématu Azure Disk Encryption scénáře nasazení a prostředí.

**Otázka:** kolik Azure Disk Encryption náklady?

**Odpověď:** je bezplatná pro šifrování disky virtuálních počítačů s Azure Disk Encryption.

**Otázka:** které vrstvy virtuálního počítače Azure Disk Encryption podporuje?

**Odpověď:** Azure Disk Encryption je k dispozici na úrovni standard virtuálních počítačích, včetně [A, D, DS, G, GS a F](https://azure.microsoft.com/pricing/details/virtual-machines/) řady virtuální počítače IaaS. Je k dispozici pro virtuální počítače s storage úrovně premium. Není k dispozici na úroveň basic virtuálních počítačů.

**Otázka:** co Linuxových distribucích nepotřebuje podporu Azure Disk Encryption?

**Odpověď:** Azure Disk Encryption je podporováno v následujících Linuxových distribucích serveru a verze:

| Distribuce systému Linux | Verze | Typ svazku podporovaný pro šifrování|
| --- | --- |--- |
| Ubuntu | 16.04-DAILY-LTS | Disk operačního systému a dat |
| Ubuntu | 14.04.5-DAILY-LTS | Disk operačního systému a dat |
| RHEL | 7.4 | Datový disk * |
| RHEL | 7.3 | Datový disk * |
| RHEL | 7.2 | Datový disk * |
| RHEL | 6.8 | Datový disk * |
| RHEL | 6.7 | Datový disk * |
| CentOS | 7.3 | Disk operačního systému a dat |
| CentOS | 7.2n | Disk operačního systému a dat |
| CentOS | 6.8 | Disk operačního systému a dat |
| CentOS | 7.1 | Datový disk |
| CentOS | 7.0 | Datový disk |
| CentOS | 6.7 | Datový disk |
| CentOS | 6.6 | Datový disk |
| CentOS | 6.5 | Datový disk |
| openSUSE | 13.2 | Datový disk |
| SLES | 12 SP1 | Datový disk |
| SLES | Priorita: 12-SP1 | Datový disk |
| SLES | HPC 12 | Datový disk |
| SLES | Priorita: 11-SP4 | Datový disk |
| SLES | 11 SP4 | Datový disk |

*__ADE se podporuje pro RHEL pro datový disk. Aktuální implementace ADE funguje pro disk operačního systému, ale nepodporuje aktuálně společně. Společnosti Microsoft a Red Hat práce na společně podporované řešení. Do té doby můžete odkazovat v dokumentu White Paper ADE šifrování disku operačního systému Linux [zde](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption).__

**Otázka:** jak můžete začít používat Azure Disk Encryption?

**Odpověď:** Začínáme, přečtěte si téma [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) dokumentu white paper.

**Otázka:** můžete šifrovat spouštěcí a datové svazky s Azure Disk Encryption?

**Odpověď:** Ano, můžete šifrovat spouštěcí a datové svazky pro systém Windows a virtuálních počítačů IaaS Linux. U virtuálních počítačů Windows nelze zašifrovat data bez první šifrování svazku operačního systému. Pro virtuální počítače s Linuxem můžete šifrovat datový svazek bez nutnosti nejprve šifrování svazku operačního systému. Poté, co jste zašifrovali svazku operačního systému Linux, zakázáním šifrování na svazku operačního systému pro virtuální počítače IaaS Linux není podporována.

**Otázka:** nemá Azure Disk Encryption umožňují přineste si vlastní klíč (BYOK) funkce?

**Odpověď:** Ano, můžete zadat vlastní klíče šifrovací klíče. Tyto klíče jsou chráněné v Azure Key Vault, což je úložiště klíčů pro Azure Disk Encryption. Další informace o klíčových šifrovací klíče podporu scénářů najdete v tématu Azure Disk Encryption scénáře nasazení a prostředí.

**Otázka:** můžete použít Azure vytvořit klíče šifrovací klíč?

**Odpověď:** Azure Key Vault Ano, můžete použít ke generování klíče šifrovací klíč pro použití Azure disk encryption. Tyto klíče jsou chráněné v Azure Key Vault, což je úložiště klíčů pro Azure Disk Encryption. Další informace o scénářích klíče podpora šifrování pomocí klíče najdete v Azure Disk Encryption scénáře nasazení a prostředí.

**Otázka:** je možné používat na službu správy klíčů místní nebo modulu hardwarového zabezpečení pro zabezpečit šifrovacích klíčů?

**Odpověď:** nemůžete použít místní službu správy klíčů nebo modulu hardwarového zabezpečení k ochraně šifrovacích klíčů s Azure Disk Encryption. Aby se předešlo šifrovací klíče můžete použít pouze do služby Azure Key Vault. Další informace o scénářích klíče podpora šifrování pomocí klíče najdete v Azure Disk Encryption scénáře nasazení a prostředí.

**Otázka:** jaké jsou požadavky pro konfiguraci Azure Disk Encryption?

**Odpověď:** je požadovaných skript prostředí PowerShell. Pomocí tohoto skriptu můžete vytvořit aplikaci Azure Active Directory, vytvoření nového trezoru klíčů nebo nastavit existující trezor klíčů pro přístup k šifrování disku povolit šifrování a chránit tajných klíčů a klíče. Další informace o scénářích klíče podpora šifrování pomocí klíče najdete v části požadavky pro Azure Disk Encryption a scénáře nasazení a prostředí.

**Otázka:** kde může získat další informace o tom, jak pomocí prostředí PowerShell pro konfiguraci Azure Disk Encryption?

**Odpověď:** máme některé skvělé články v tom, jak můžete provádět základní úlohy Azure Disk Encryption, jakož i pokročilejší scénáře. Základní úlohách najdete v tématu [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Pro pokročilejší scénáře, najdete v části [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

**Otázka:** jakou verzi prostředí Azure PowerShell Azure Disk Encryption podporuje?

**Odpověď:** použít nejnovější verzi sady Azure PowerShell SDK ke konfiguraci Azure Disk Encryption. Stáhněte si nejnovější verzi [prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases). Azure Disk Encryption je *není* nepodporuje verze 1.1.0 sady Azure SDK.

> [!NOTE]
> Rozšíření Linux Azure šifrování disku preview je zastaralý. Podrobnosti najdete v tématu [místo začne Azure disk encryption preview rozšíření pro virtuální počítače IaaS Linux](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

**Otázka:** můžete použít Azure Disk Encryption na mé vlastní bitovou kopii systému Linux?

**Odpověď:** Azure Disk Encryption nelze použít na vaše vlastní bitovou kopii systému Linux. Pouze obrázky Linux Galerie jsme podporu pro podporované distribuce vyznačeny dříve. Vlastní Image Linux aktuálně nepodporujeme.

**Otázka:** můžete použít aktualizace pro Red Hat virtuálního počítače s Linuxem používající yum aktualizace?

**Odpověď:** Ano, můžete provést aktualizace nebo opravy virtuálního počítače s Linuxem Red Hat. Další informace najdete v tématu [použití aktualizací na šifrovaný IaaS Red Hat virtuálního počítače Azure pomocí aktualizace yum](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

**Otázka:** co je pracovní postup šifrování disku doporučený Azure pro Linux?

**Odpověď:** následující pracovní postup se doporučuje mít nejlepších výsledků v systému Linux:
* Spuštění z bitové kopie beze změny uložených Galerie odpovídající požadované distro operačního systému a verze
* Zálohujte všechny připojené jednotky, které se budou šifrovat.  To umožňuje obnovení v případě selhání, například pokud virtuální počítač je restartovat předtím, než byla dokončena šifrování.
* Šifrování (může trvat několik hodiny nebo i dny v závislosti na vlastnosti virtuálního počítače a velikost všech připojených datových disků)
* Přizpůsobit a přidat softwaru na bitovou kopii podle potřeby.

Pokud tento pracovní postup není možné, spoléhat na [šifrování služby úložiště](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE) na úložiště platformy vrstvy účet může být alternativu k použití dm-crypt šifrování celého disku.

**Otázka:** co je na disku "Svazek Bek" nebo "/ mnt/azure_bek_disk"?

**Odpověď:** "Bek svazek" pro Windows, nebo "/ mnt/azure_bek_disk" pro Linux je místní data svazek, který bezpečně ukládá šifrovací klíče pro šifrovaná virtuálních počítačů Azure IaaS.
> [!NOTE]
> Odstranit nebo upravit všechny obsah v tomto disku. Vzhledem k tomu, že šifrovací klíče přítomnosti je potřeba pro všechny operace šifrování pro virtuální počítač IaaS není odpojit disk.

**Otázka:** kde můžete přejít k nim máte nějaké otázky nebo poskytnout zpětnou vazbu?

**Odpověď:** můžete klást otázky nebo poskytnout zpětnou vazbu na [fórum pro Azure Disk Encryption](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Další postup
V tomto dokumentu jste zjistili, informace o nejčastější dotazy týkající se Azure Disk Encryption. Další informace o této služby a jeho funkce najdete v následujících článcích:

- [Použít šifrování disku v Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Šifrování virtuálního počítače Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Šifrování dat Azure v klidovém stavu](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
