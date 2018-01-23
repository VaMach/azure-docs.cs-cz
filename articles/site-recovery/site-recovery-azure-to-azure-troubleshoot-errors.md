---
title: "Azure Site Recovery pro odstraňování potíží v Azure do Azure replikace problémy a chyby | Microsoft Docs"
description: "Řešení potíží s chyb a problémů při replikaci virtuálních počítačů Azure pro zotavení po havárii"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: sujayt
ms.openlocfilehash: 9e5719cd81408f6732826c90505a3ce8aa10f8ed
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Řešení problémů replikace virtuálního počítače Azure do Azure

Tento článek popisuje běžné problémy při Azure Site Recovery při replikaci a obnovení virtuálních počítačů Azure z jedné oblasti do jiné oblasti a vysvětluje, jak je vyřešit. Další informace o podporovaných konfiguracích najdete v tématu [matici podpory pro replikaci virtuálních počítačů Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problémy kvótu prostředků Azure (kód chyby 150097)
Vaše předplatné by měly být povoleny k vytvoření virtuálních počítačů Azure v oblasti cíl, který chcete použít jako svou oblast pro obnovení po havárii. Vaše předplatné navíc by měl mít dostatečná kvóta pro vytvoření virtuálních počítačů určité velikosti. Ve výchozím nastavení vybere Site Recovery pro cílový počítač stejnou velikost jako zdroj virtuálního počítače. Pokud odpovídající velikost není k dispozici, je automaticky vybrána nejbližší možné velikost. Pokud není k dispozici žádné odpovídající velikost, která podporuje konfiguraci zdrojového virtuálního počítače, zobrazí se tato chybová zpráva:

**Kód chyby** | **Možné příčiny** | **Doporučení**
--- | --- | ---
150097<br></br>**Zpráva**: nebylo možné povolit replikaci pro virtuální počítač VmName. | -Chcete-li vytvořit všechny virtuální počítače v cílovém umístění oblasti může být vypnutá svoje ID předplatného.</br></br>-Svoje ID předplatného nemusí být povoleno nebo nemá dostatečnou kvótu k vytvoření určité velikosti virtuálních počítačů v cílovém umístění oblasti.</br></br>-A vhodnou cílovou velikost virtuálního počítače, který odpovídá zdrojového virtuálního počítače síťový adaptér počet (2) nebyl nalezen pro ID předplatného v cílovém umístění oblasti.| Obraťte se na [Azure podporu fakturace](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) umožňující vytvoření virtuálních počítačů pro požadované velikosti virtuálních počítačů v cílovém umístění pro vaše předplatné. Když je tato funkce povolená, opakujte operaci se nezdařilo.

### <a name="fix-the-problem"></a>Opravte problém
Obraťte se na [Azure podporu fakturace](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) povolit vaše předplatné přístup k vytvoření požadovaných velikostí virtuální počítače v cílovém umístění.

Pokud cílové umístění obsahuje omezení kapacity, zakažte replikaci a povolte ho do jiného umístění, kde vaše předplatné má dostatečnou kvótu k vytvoření virtuálních počítačů pro požadované velikosti.

## <a name="trusted-root-certificates-error-code-151066"></a>Důvěryhodné kořenové certifikáty (kód chyby 151066)

Pokud nejsou k dispozici na virtuální počítač všechny nejnovější důvěryhodných kořenových certifikátů, může selhat úlohu "povolení replikace". Bez certifikáty ověřování a autorizace volání služby Site Recovery z virtuálního počítače nezdaří. Zobrazí se chybová zpráva pro nezdařené úlohy Site Recovery "povolit replikaci":

**Kód chyby** | **Možná příčina** | **Recommendations** (Doporučení)
--- | --- | ---
151066<br></br>**Zpráva**: Site Recovery konfigurace se nezdařila. | Požadované důvěryhodných kořenových certifikátů použít k ověřování a ověřování není na tomto počítači. | -Pro virtuální počítač s operačním systémem Windows zajistěte, aby byly na tomto počítači důvěryhodných kořenových certifikátů. Informace najdete v tématu [konfigurovat Důvěryhodné kořeny a zakázané certifikáty](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-Pro virtuální počítač s operačním systémem Linux postupujte podle pokynů pro důvěryhodných kořenových certifikátů, které zveřejnil distributora verze operačního systému Linux.

### <a name="fix-the-problem"></a>Opravte problém
**Windows**

Nainstalujte všechny nejnovější aktualizace systému Windows ve virtuálním počítači tak, aby byly všechny důvěryhodných kořenových certifikátů na tomto počítači. Pokud jste v odpojeném prostředí, postupujte podle standardního procesu aktualizace systému Windows ve vaší organizaci získat certifikáty. Pokud požadované certifikáty nejsou k dispozici na virtuální počítač, nezdaří se volání do služby Site Recovery z bezpečnostních důvodů.

Postupujte podle typické Windows update management nebo certifikát proces správy aktualizace ve vaší organizaci získat všechny nejnovější kořenové certifikáty a seznamu odvolaných certifikátů aktualizovaný certifikát na virtuálních počítačích.

Chcete-li ověřit, že byl problém vyřešen, přejděte na login.microsoftonline.com v virtuálního počítače z prohlížeče.

**Linux**

Postupujte podle pokynů uvedených ve vaší Linux distributora, chcete-li získat nejnovější důvěryhodných kořenových certifikátů a nejnovější seznam odvolaných certifikátů ve virtuálním počítači.

Protože SuSE Linux používá symbolických odkazů k údržbě seznamu certifikátů, postupujte takto:

1.  Přihlaste se jako kořenové uživatele.

2.  Spusťte tento příkaz:

      ``# cd /etc/ssl/certs``

3.  Pokud chcete zobrazit, pokud je certifikát kořenové certifikační Autority Symantec přítomné, spusťte tento příkaz:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  Pokud soubor nebyl nalezen, spusťte tyto příkazy:

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  Chcete-li vytvořit symlink s b204d74a.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem, spusťte tento příkaz:

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  Zkontrolujte, zda tento příkaz má následující výstup. Pokud ne, je nutné vytvořit symlink:

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. Pokud není přítomen symlink 653b494a.0, použijte tento příkaz k vytvoření symlink:

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Odchozí připojení pro rozsahy adres URL obnovení lokality nebo IP adresu (kód chyby 151037 nebo 151072)

U replikace Site Recovery k práci, odchozí připojení ke konkrétní adresy URL nebo IP adresu se rozsahy vyžaduje z virtuálního počítače. Pokud virtuální počítač je za bránou firewall nebo pravidla skupiny (NSG) zabezpečení sítě používá k řízení odchozí připojení, můžete sledovat některé z těchto chybových zpráv:

**Kódy chyb** | **Možné příčiny** | **Recommendations** (Doporučení)
--- | --- | ---
151037<br></br>**Zpráva**: Nepodařilo se zaregistrovat virtuální počítač Azure Site Recovery. | -Používáte NSG k řízení odchozí přístup na virtuální počítač a požadované IP adresy rozsahů nejsou povolené pro odchozí přístup.</br></br>-Používáte nástroji brány firewall třetích stran a požadované rozsahy IP nebo URL nejsou povolené.</br>| – Pokud používáte proxy server brány firewall pro řízení odchozí síťové připojení ve virtuálním počítači, ujistěte se, jestli jsou požadované adresy URL a rozsahy IP datacenter seznam povolených adres. Informace najdete v tématu [brány firewall proxy pokyny](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>– Pokud používáte pravidla NSG k řízení odchozí síťové připojení ve virtuálním počítači, zajistěte, aby rozsahy IP požadovaných datacenter seznam povolených adres. Informace najdete v tématu [sítě doprovodné materiály zabezpečení skupiny](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**Zpráva**: Site Recovery konfigurace se nezdařila. | Nelze navázat připojení ke koncovým bodům služby Site Recovery. | – Pokud používáte proxy server brány firewall pro řízení odchozí síťové připojení ve virtuálním počítači, ujistěte se, jestli jsou požadované adresy URL a rozsahy IP datacenter seznam povolených adres. Informace najdete v tématu [brány firewall proxy pokyny](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>– Pokud používáte pravidla NSG k řízení odchozí síťové připojení ve virtuálním počítači, zajistěte, aby rozsahy IP požadovaných datacenter seznam povolených adres. Informace najdete v tématu [sítě doprovodné materiály zabezpečení skupiny](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-the-problem"></a>Opravte problém
Do seznamu povolených IP adres [požadované adresy URL](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) nebo [požadované rozsahy IP adres](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges), postupujte podle kroků v [sítě pokyny dokumentu](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Nebyl nalezen v počítači (kód chyby 150039)

Nový disk připojen k virtuálnímu počítači musí být inicializován.

**Kód chyby** | **Možné příčiny** | **Recommendations** (Doporučení)
--- | --- | ---
150039<br></br>**Zpráva**: Azure datový disk (DiskName) (DiskURI) se logická jednotka (LUN) (LUNValue) nebyl namapovaný na odpovídající disk z hlásí v rámci virtuálního počítače, který má stejnou hodnotu, logickou jednotku. | -Nový datový disk se připojil k virtuálnímu počítači, ale nebyl inicializován.</br></br>-Datový disk ve virtuálním počítači není správně reporting hodnota logické jednotky, kdy byla disk připojen k virtuálnímu počítači.| Zajistěte, aby se inicializují datových disků a potom operaci opakujte.</br></br>Pro Windows: [připojení a inicializovat nový disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Pro Linux: [inicializovat nový datový disk v systému Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Opravte problém
Zajistěte, aby byly inicializovány datových disků a potom operaci opakujte:

- Pro Windows: [připojení a inicializovat nový disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- Pro Linux: [přidejte nový disk dat v systému Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Pokud potíže potrvají, obraťte se na podporu.


## <a name="unable-to-see-the-azure-vm-for-selection-in-enable-replication"></a>Nelze zobrazit virtuální počítač Azure pro výběr v "povolení replikace"

Nemusíte to vidět svého virtuálního počítače Azure pro výběr v [povolení replikace: Krok 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines). Tento problém může být z důvodu stálou konfiguraci Site Recovery ponecháno na virtuálním počítači Azure. Stálou konfiguraci může být ponecháno na virtuální počítač Azure v následujících případech:

- Povolit replikaci pro virtuální počítač Azure pomocí Site Recovery a pak odstranit trezor Site Recovery bez výslovně zakazuje replikace na virtuálním počítači.
- Povolit replikaci pro virtuální počítač Azure pomocí Site Recovery a pak odstranit skupinu prostředků obsahující trezoru Site Recovery bez výslovně zakazuje replikace na virtuálním počítači.

### <a name="fix-the-problem"></a>Opravte problém

Můžete použít [odebrat zastaralé konfigurační skript automatické obnovení systému](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) a odebrat zastaralé konfiguraci Site Recovery na virtuálním počítači Azure. Měli byste vidět virtuální počítač v [povolení replikace: Krok 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) po odebrání stálou konfiguraci.


## <a name="next-steps"></a>Další postup
[Replikace virtuálních počítačů Azure](site-recovery-replicate-azure-to-azure.md)
