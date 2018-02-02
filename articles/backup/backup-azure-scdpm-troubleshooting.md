---
title: "Řešení potíží s System Center Data Protection Manager pomocí Azure Backup | Microsoft Docs"
description: "Řešení problémů v System Center Data Protection Manager."
services: backup
documentationcenter: 
author: adigan
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: pullabhk;markgal;adigan
ms.openlocfilehash: 2244a39217f54eb5906d05990f19fc8ca2fdeb0e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Řešení potíží s System Center Data Protection Manager

Můžete najít nejnovější poznámky k verzi pro aplikaci DPM SC [zde](https://docs.microsoft.com/en-us/system-center/dpm/dpm-release-notes?view=sc-dpm-2016).
Můžete najít na matici podpory ochrany [zde](https://docs.microsoft.com/en-us/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).

## <a name="replica-is-inconsistent"></a>Replika je nekonzistentní

K této chybě může dojít z různých důvodů, jako je například – úloha vytvoření repliky se nezdařila, problémy s deník změn svazku filtru úrovně rastrový obrázek chyby, zdrojového počítače byl neočekávaně vypnut, přetečení protokol synchronizace nebo replika je skutečně nekonzistentní. Postupujte podle těchto kroků k vyřešení tohoto problému:
- Pokud chcete odebrat nekonzistentní stav, spusťte kontrolu konzistence ručně nebo naplánovat každodenní kontrolu konzistence.
- Ujistěte se, že jste na nejnovější verzi MAB Server nebo System Center DPM
- Ujistěte se, že je povolena automatická kontrola konzistence
- Zkuste restartovat službu z příkazového řádku ("net stop dpmra" následovaný "net start dpmra")
- Ujistěte se, že jsou splněny požadavky na připojení a šířku pásma sítě
- Zkontrolujte, pokud byl neočekávaně vypnut zdrojový počítač
- Zkontrolujte disk je v pořádku a není dostatek místa pro repliky
- Zkontrolujte duplicitní zálohování běží souběžně

## <a name="online-recovery-point-creation-failed"></a>Vytvoření bodu obnovení online se nezdařilo.

Postupujte podle těchto kroků k vyřešení tohoto problému:
- Ujistěte se, že jste na nejnovější verzi Azure Backup Agent
- Zkuste ručně vytvořit bod obnovení v oblasti úloh ochrana
- Ujistěte se, že spustíte kontrolu konzistence na zdroji dat
- Ujistěte se, že jsou splněny požadavky na připojení a šířku pásma sítě
- Data repliky je v nekonzistentním stavu. Vytvořit bod obnovení disku tohoto zdroje dat
- Ujistěte se, že je replika přítomen a není chybí.
- Repliky má dostatek místa pro vytvoření deníku USN

## <a name="unable-to-configure-protection"></a>Ochranu nelze nakonfigurovat

Tato chyba se zobrazí, pokud je server aplikace DPM nemůže kontaktovat na chráněném serveru. Postupujte podle těchto kroků k vyřešení tohoto problému:
- Ujistěte se, že jste na nejnovější verzi Azure Backup Agent
- Ujistěte se, že je k dispozici připojení (sítě a brány firewall nebo proxy) mezi serverem DPM a chráněném serveru
- Pokud chráníte SQL Server, zajistěte, aby byl NT AUTHORITY\SYSTEM sysadmin povolené z vlastnosti přihlášení

## <a name="this-server-is-not-registered-to-the-vault-specified-by-the-vault-credential"></a>Tento server není registrovaný k úložišti určenému přihlašovacími údaji úložiště

Tato chyba se zobrazí, když soubor s přihlašovacími údaji trezoru vybrané nepatří do trezoru služeb zotavení přidruženou k aplikaci System Center DPM nebo serveru Azure Backup Server, na kterém dojde k pokusu o obnovení. Postupujte podle těchto kroků k vyřešení tohoto problému:
- Stažení souboru přihlašovacích údajů trezoru služeb zotavení trezoru, ke kterému System Center DPM / serveru Azure Backup je zaregistrován.
- Opakujte registraci serveru s úložištěm pomocí nejnovější soubor stažený úložiště přihlašovacích údajů.

## <a name="either-the-recoverable-data-is-not-available-or-the-selected-server-is-not-a-dpm-server"></a>Buď obnovitelná data nejsou k dispozici, nebo vybraný server není DPM server
Tato chyba se zobrazí, když nejsou žádné další DPM System Center / zaregistrované servery Azure Backup do trezoru služeb zotavení, nebo servery ještě jste zatím neodeslali metadata nebo vybraný server není System Center DPM nebo serveru Azure Backup.
- Pokud existují další System Center DPM / zaregistrované servery Azure Backup do trezoru služeb zotavení, zajistěte, aby nejnovější Azure Backup agent není nainstalován.
- Pokud existují další System Center DPM / zaregistrované servery Azure Backup do trezoru služeb zotavení, počkejte denně po instalaci zahájíte proces obnovení. Na noční úlohu ukládání metadata pro všechny chráněné zálohování do cloudu. Data jsou k dispozici pro obnovení.

## <a name="the-encryption-passphrase-provided-does-not-match-with-passphrase-associated-with-the-following-server"></a>Zadaná šifrovací přístupové heslo neodpovídá přístupovému heslu přidruženému k následujícímu serveru

> [!NOTE]
>Pokud jste zapomněli jste/ztratili šifrovací přístupové heslo, není žádná možnost obnovit data. Jedinou možností je použít k šifrování budoucí zálohování dat a znovu vygenerovat přístupové heslo.
>
>

Tato chyba se zobrazí, když šifrovací přístupové heslo používaného Probíhá šifrování dat z System Center DPM nebo Server Azure Backup dat, který obnovuje neodpovídá zadané šifrovací přístupové heslo. Agent se nepodařilo dešifrovat data. Proto se obnovení nezdaří. Postupujte podle těchto kroků k vyřešení tohoto problému:
- Zadejte přesně stejný šifrovací přístupové heslo přidružené k System Center DPM nebo serveru Azure Backup, jejichž data obnovena. 
