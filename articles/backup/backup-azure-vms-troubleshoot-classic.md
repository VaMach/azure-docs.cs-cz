---
title: "Řešení Azure backup chyb portálu classic | Microsoft Docs"
description: "Řešení potíží s Azure zálohování a obnovení virtuální počítače, které jsou na klasickém portálu Azure."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: trinadhk;markgal;
ms.openlocfilehash: 284a1b64fbb15d0aa800182c6671d447e191b76a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Odstraňování potíží se zálohováním virtuálních počítačů Azure
> [!div class="op_single_selector"]
> * [Trezor služeb zotavení](backup-azure-vms-troubleshoot.md)
> * [Úložiště záloh](backup-azure-vms-troubleshoot-classic.md)
>
>

Řešení potíží s chyb zjištěných při pomocí Azure Backup informace uvedené v následující tabulce.

## <a name="discovery"></a>Zjišťování
| Operace zálohování | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zjišťování |Nepovedlo se zjistit nové body – Microsoft Azure Backup došlo k vnitřní chybě. Počkejte několik minut a opakujte operaci znovu. |Zkuste zopakujte proces zjišťování po 15 minutách. |
| Zjišťování |Nepovedlo se zjistit nové položky – další zjišťování operace již probíhá. Počkejte prosím na dokončení aktuální operace zjišťování. |Žádný |

## <a name="register"></a>Registrace
| Operace zálohování | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Registrace |Počet datových disků připojených k virtuálnímu počítači překročil podporovaný limit – prosím odpojte některé datových disků na tomto virtuálním počítači a operaci opakujte. Až 16 datových disků připojených k virtuální počítač Azure pro zálohování podporuje zálohování Azure |Žádný |
| Registrace |Microsoft Azure Backup se vyskytla vnitřní chyba: Počkejte několik minut a opakujte operaci znovu. Pokud potíže potrvají, kontaktujte prosím podporu Microsoftu. |Tato chyba kvůli jednomu z následujících nepodporované konfigurace virtuálních počítačů můžete získat na Premium LRS. <br> Virtuální počítače služby storage úrovně Premium můžete zálohovat pomocí trezoru služeb zotavení. [Další informace](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Registrace |Registrace s vypršením časového limitu operace instalace agenta se nezdařila. |Zkontrolujte, pokud se podporuje verze operačního systému virtuálního počítače. |
| Registrace |Příkaz se nezdařilo - jiná operace probíhá spuštění u této položky. Počkejte prosím na dokončení předchozí operace |Žádný |
| Registrace |Virtuální počítače s virtuální pevné disky uložené na storage úrovně Premium nejsou podporované pro zálohování |Žádný |
| Registrace |Agent virtuálního počítače není k dispozici na virtuálním počítači – nainstalujte požadované nezbytné, agent virtuálního počítače a restartujte operaci. |[Další informace](#vm-agent) o instalace agenta virtuálního počítače a postup ověření instalace agenta virtuálního počítače. |

## <a name="backup"></a>Zálohování
| Operace zálohování | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zálohování |S agentem virtuálního počítače nebylo možné komunikovat ohledně stavu snímku. Vypršel časový limit úloze dílčí snímků virtuálních počítačů. -Naleznete Průvodci odstraňováním potíží o tom, jak to vyřešit. |Tato chyba se vyvolá, pokud došlo k potížím s agenta virtuálního počítače, nebo blokovaný přístup k síti na infrastrukturu Azure nějakým způsobem. Další informace o [ladění si virtuální počítač snímek problémy](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Pokud agenta virtuálního počítače není způsobuje problémy, restartujte virtuální počítač. Nesprávný stav virtuálního počítače v některých případech může způsobit problémy a restartování virtuálního počítače obnoví tento "chybný stav" |
| Zálohování |Zálohování došlo k vnitřní chybě – opakujte operaci za několik minut. Pokud potíže potrvají, obraťte se na Microsoft Support |Zkontrolujte prosím, pokud je dočasný problém s přístupem ke úložiště virtuálního počítače. Zkontrolujte, zda [stavu Azure](https://azure.microsoft.com/en-us/status/) chcete zobrazit, pokud je jakýkoli průběžné problém související s výpočtů nebo úložiště nebo sítě v oblasti. Zkuste spustit znovu, které zmírnit problém zálohování post. |
| Zálohování |Operace se nedá provést, protože virtuální počítač už neexistuje. |Zálohování nelze provést, protože byla odstraněna konfigurovat pro zálohování virtuálních počítačů. Zastavit další zálohy přejdete na zobrazení chráněné položky, vyberte chráněné položky a klikněte na Zastavit ochranu. Vyberte možnost zachovat zálohu dat můžete zachovat data. Později můžete obnovit ochrany pro tento virtuální počítač kliknutím na Konfigurace ochrany z registrované položky zobrazení |
| Zálohování |Nepodařilo se nainstalovat služeb zotavení Azure rozšíření na vybranou položku - Agent virtuálního počítače je nezbytný předpoklad pro Azure Recovery Services rozšíření. Nainstalujte agenta virtuálního počítače Azure a restartujte operaci registrace |<ol> <li>Zkontrolujte, zda správně nainstalován agent virtuálního počítače. <li>Ujistěte se, zda je správně nastaven příznak na konfiguraci virtuálního počítače.</ol> [Další informace](#validating-vm-agent-installation) o instalace agenta virtuálního počítače a postup ověření instalace agenta virtuálního počítače. |
| Zálohování |Příkaz spuštění se nezdařilo - jiná operace právě probíhá u této položky. Počkejte prosím na dokončení předchozí operace a poté opakujte |Existující úlohy zálohování nebo obnovení pro virtuální počítač se systémem a novou úlohu nelze spustit, pokud existující úloha běží. |
| Zálohování |Instalace rozšíření se nezdařilo s chybou "modelu COM + se nepodařilo komunikovat Microsoft Distributed Transaction Coordinator |To obvykle znamená, zda není spuštěna služba modelu COM +. Nápovědu k opravě tohoto problému, kontaktujte podporu společnosti Microsoft. |
| Zálohování |Snímek operace se nezdařila s chybou operaci služby Stínová kopie svazku "Tato jednotka je uzamčen nástrojem BitLocker Drive Encryption. Je třeba odemknout tuto jednotku v Ovládacích panelech. |Vypnout nástroj BitLocker pro všechny disky na virtuálním počítači a zkontrolujte, zda je k odstranění problému služby Stínová kopie svazku |
| Zálohování |Virtuální počítače s virtuální pevné disky uložené na storage úrovně Premium nejsou podporované pro zálohování |Žádný |
| Zálohování |Virtuální počítač Azure nebyla nalezena. |To se stane, když dojde k odstranění primární virtuální počítač ale zásady zálohování nadále vypadat pro virtuální počítač k provedení zálohování. Chcete-li vyřešit tuto chybu: <ol><li>Znovu vytvořte virtuální počítač se stejným názvem a stejný název skupiny prostředků [název cloudové služby], <br>(NEBO) <li> Zakažte ochranu pro tento virtuální počítač tak, aby nebude získat aktivuje následné zálohy. </ol> |
| Zálohování |Agent virtuálního počítače není k dispozici na virtuálním počítači – nainstalujte požadované nezbytné, agent virtuálního počítače a restartujte operaci. |[Další informace](#vm-agent) o instalace agenta virtuálního počítače a postup ověření instalace agenta virtuálního počítače. |

## <a name="jobs"></a>Úlohy
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zrušení úlohy |Zrušení není podporován pro tento typ úlohy – počkejte na dokončení úlohy. |Žádný |
| Zrušení úlohy |Úloha není ve stavu, možné zrušit – počkejte na dokončení úlohy. <br>NEBO<br> Vybraná úloha není ve stavu, možné zrušit – počkejte na dokončení úlohy. |Ve všech pravděpodobnost téměř dokončení úlohy; Počkejte prosím na dokončení úlohy |
| Zrušení úlohy |Nelze zrušit úlohu, protože není v průběhu – zrušení je podporována pouze pro úlohy, které jsou v průběhu. Prosím pokus o zrušení na v průběhu úlohy. |K tomu dochází z důvodu přechodné stavu. Počkejte několik minut a opakujte operace zrušení |
| Zrušení úlohy |Nepodařilo se zrušit úlohy – počkejte na dokončení úlohy. |Žádný |

## <a name="restore"></a>Obnovení
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Obnovení |Obnovení se nezdařilo s cloudu vnitřní chyba |<ol><li>Cloudové služby, ke kterému se pokoušíte obnovit je nakonfigurováno nastavení DNS. Můžete zkontrolovat <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-slotu "Výroba" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Pokud je nakonfigurovanou adresu, znamená to, zda jsou nakonfigurovány nastavení DNS.<br> <li>Cloudové služby, ke kterému chcete se pokoušíte obnovit je nakonfigurován s vyhrazené IP adresy a stávající virtuální počítače v rámci cloudové služby jsou v zastaveném stavu.<br>Můžete zkontrolovat, že cloudové služby má vyhrazená IP adresa pomocí následující rutiny prostředí powershell:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-slotu $ "Výroba" dep. ReservedIPName <br><li>Pokoušíte se virtuální počítač s následující zvláštní síťové konfigurace v obnovit do stejné cloudové služby. <br>-Virtuální počítače v části Konfigurace služby Vyrovnávání zatížení (interní a externí)<br>-Virtuální počítače s víc vyhrazených IP adres<br>-Virtuální počítače s více síťovými kartami<br>Vyberte novou cloudovou službu v uživatelském rozhraní nebo prostudujte si prosím [obnovit aspekty](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) u virtuálních počítačů s zvláštní síťové konfigurace</ol> |
| Obnovení |Vybraný název DNS už je zabraný – Zadejte prosím jiný název DNS a zkuste to znovu. |Název DNS odkazuje na název cloudové služby (obvykle konče. cloudapp.net). Toto musí být jedinečný. Pokud dojde k této chybě, budete muset zvolit jiný název virtuálního počítače během obnovení. <br><br> Tato chyba se zobrazí pouze uživatelům na portálu Azure. Operace obnovení pomocí prostředí PowerShell úspěšné, protože pouze obnoví disky a nepodporuje vytvoření virtuálního počítače. Chyba se potýkají, když virtuální počítač je explicitně vytvořený můžete po operaci obnovení na disku. |
| Obnovení |Zadaný virtuální síťový konfigurace není správná – zadejte konfiguraci s jinou virtuální síť a zkuste to znovu. |Žádný |
| Obnovení |Zadaná Cloudová služba používá vyhrazenou IP adresu, která nebude odpovídat konfiguraci virtuálního počítače, který se má obnovit – zadejte jinou cloudovou službu, která není pomocí vyhrazené IP adresy nebo zvolte jiný bod obnovení pro obnovení z. |Žádný |
| Obnovení |Cloudové služby bylo dosaženo limitu počtu vstupní koncové body – zkuste operaci zopakovat zadáním jiné cloudové služby nebo pomocí existující koncový bod. |Žádný |
| Obnovení |Zálohování úložiště a cílový účet úložiště jsou ve dvou různých oblastech – zajistěte, aby byl účet úložiště zadaný v operaci obnovení ve stejné oblasti Azure jako úložiště záloh. |Žádný |
| Obnovení |Účet úložiště, zadaný pro operaci obnovení není podporována – účty úložiště pouze Basic nebo Standard s místně redundantní nebo geograficky redundantní replikaci nastavení nejsou podporovány. Vyberte prosím účet podporované úložiště |Žádný |
| Obnovení |Typ zadaný pro operaci obnovení účtu úložiště není online – Ujistěte se, že je účet úložiště zadaný v operaci obnovení online |K tomu může dojít z důvodu přechodné chyby ve službě Azure Storage nebo kvůli výpadku. Zvolte prosím jiný účet úložiště. |
| Obnovení |Bylo dosaženo kvóty skupina prostředků – prosím odstraňte některé skupiny prostředků pomocí portálu Azure, nebo požádejte podporu Azure o navýšení limitů. |Žádný |
| Obnovení |Vybraná podsíť neexistuje – vyberte podsíť, která již existuje |Žádný |

## <a name="policy"></a>Zásada
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Vytvoření zásad |Nepodařilo se vytvořit zásadu - snižte volby uchování před pokračováním konfigurace zásad. |Žádný |

## <a name="vm-agent"></a>Agent virtuálního počítače
### <a name="setting-up-the-vm-agent"></a>Nastavení agenta virtuálního počítače
Obvykle agenta virtuálního počítače již existuje ve virtuálních počítačích, které jsou vytvořené z Galerie Azure. Virtuální počítače, které jsou migrované z místních datových center však nebude mít Agent virtuálního počítače nainstalovaný. Pro tyto virtuální počítače musí být explicitně nainstalovaný Agent virtuálního počítače. Další informace o [instalace agenta virtuálního počítače na existující virtuální počítač](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Pro virtuální počítače Windows:

* Stáhněte si a nainstalujte [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). K dokončení instalace budete potřebovat oprávnění správce.
* [Aktualizujte vlastnost virtuálního počítače](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby indikovala, že je agent nainstalovaný.

Pro virtuální počítače Linux:

* Nainstalujte nejnovější [agenta systému Linux](https://github.com/Azure/WALinuxAgent) z githubu.
* [Aktualizujte vlastnost virtuálního počítače](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby indikovala, že je agent nainstalovaný.

### <a name="updating-the-vm-agent"></a>Aktualizace agenta virtuálního počítače
Pro virtuální počítače Windows:

* Aktualizace agenta virtuálního počítače je stejně jednoduchá, jako přeinstalace [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Ale je potřeba zajistit, že během aktualizace agenta virtuálního počítače neběží žádná operace zálohování.

Pro virtuální počítače Linux:

* Postupujte podle pokynů [aktualizace agenta virtuálního počítače Linux](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>Ověření instalace agenta virtuálního počítače
Jak zjistit, jestli verze agenta virtuálního počítače na virtuálních počítačích Windows:

1. Přihlaste se k virtuálnímu počítači Azure a přejděte do složky *C:\WindowsAzure\Packages*. Měl by být přítomný soubor WaAppAgent.exe.
2. Pravým tlačítkem myši klikněte na soubor, přejděte na **Vlastnosti** a poté vyberte kartu **Podrobnosti**. Pole verze produktu by mělo být 2.6.1198.718 nebo vyšší
