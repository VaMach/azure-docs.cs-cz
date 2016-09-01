<properties
   pageTitle="Časté otázky týkající se verze Public Preview služby Azure Backup | Microsoft Azure"
   description="Tato verze častých otázek podporuje verzi Public Preview služby Azure Backup. Odpovědi na časté otázky týkající se agenta zálohování, zálohování a uchovávání, obnovení, zabezpečení a další běžné otázky týkající se řešení Azure Backup."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="řešení zálohování; služba zálohování"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="07/01/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# Verze Public Preview služby Azure Backup – časté otázky

> [AZURE.SELECTOR]
- [Časté otázky týkající se režimu Classic](backup-azure-backup-faq.md)
- [Časté otázky týkající se režimu ARM](backup-azure-backup-ibiza-faq.md)

Tento článek obsahuje informace specifické pro verzi Public Preview služby Azure Backup. Tento článek se aktualizuje po příchodu nových častých otázek a doplňuje [Časté otázky týkající se Azure Backup](backup-azure-backup-faq). Časté otázky týkající se Azure Backup poskytují ucelený soubor otázek a odpovědí ohledně služby Azure Backup.  

Otázky týkající se služby Azure Backup můžete pokládat v části Disqus tohoto článku nebo v některém souvisejícím článku. Otázky týkající se služby Azure Backup můžete také publikovat na [diskusním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## Co obsahuje verze Public Preview?
Verze Public Preview zavádí trezor Služeb zotavení a podporu ARM v rámci ochrany virtuálních počítačů Azure. Trezor Služeb zotavení představuje novou generaci trezorů. Tento trezor využívají služby Azure Backup a Azure Site Recovery (ASR). Považujte ho za trezor v.2.

## Služby zotavení a trezory Backup

**Otázka č. 1: Trezory Služby zotavení jsou v.2. Jsou trezory Backup (v.1) stále podporovány?** <br/>
Odpověď 1. Ano, trezory Backup jsou stále podporovány. Trezory Backup vytvářejte na portálu Classic. Trezory Služeb zotavení vytvářejte na Portálu Azure.

**Otázka 2. Můžu migrovat trezor Backup do trezoru Služeb zotavení?** <br/>
Odpověď 2. Bohužel ne. Obsah trezoru Backup nyní nejde migrovat do trezoru Služeb zotavení. Ačkoli na přidání této funkce pracujeme, v rámci verze Public Preview není dostupná.

**Otázka 3. Podporují trezory Služeb zotavení virtuální počítače v.1 nebo v.2?** <br/>
 Odpověď 3. Trezory Služeb zotavení podporují virtuální počítače v.1 a v.2. Virtuální počítač vytvořený na portálu Classic (což je v.1) nebo virtuální počítač vytvořený na Portálu Azure (což je v.2) můžete zálohovat do trezoru Služeb zotavení.


## Podpora ARM pro virtuální počítače Azure

**Otázka 1. Existují ohledně podpory ARM pro virtuální počítače Azure nějaká omezení?** <br/>
Odpověď č. 1: Rutiny prostředí PowerShell pro ARM nejsou aktuálně dostupné. Pokud chcete přidat prostředky do skupiny prostředků, je nutné použít uživatelské rozhraní Portálu Azure.



<!--HONumber=Aug16_HO4-->


