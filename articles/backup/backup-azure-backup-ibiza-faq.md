<properties
   pageTitle="Nejčastější dotazy k trezoru Recovery Services | Microsoft Azure"
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
     ms.date="08/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>


# Trezor služby Recovery Services – nejčastější dotazy

> [AZURE.SELECTOR]
- [Nejčastější dotazy týkající se klasického režimu ve službě Backup](backup-azure-backup-faq.md)
- [Nejčastější dotazy týkající se režimu Resource Manager](backup-azure-backup-ibiza-faq.md)

Tento článek poskytuje informace specifické pro trezor služby Recovery Services a doplňuje článek [Nejčastější dotazy k Azure Backup](backup-azure-backup-faq.md). Časté otázky týkající se Azure Backup poskytují ucelený soubor otázek a odpovědí ohledně služby Azure Backup.  

Otázky týkající se služby Azure Backup můžete pokládat v části Disqus tohoto článku nebo v některém souvisejícím článku. Otázky týkající se služby Azure Backup můžete také publikovat na [diskusním fóru](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## Trezory služby Recovery Services jsou založené na Resource Manageru. Jsou trezory služby Backup (v klasickém režimu) stále podporovány? <br/>
Ano, trezory Backup jsou stále podporovány. Trezory Backup vytvářejte na [portálu Classic](https://manage.windowsazure.com). Trezory Recovery Services vytvářejte na webu [Azure Portal](https://portal.azure.com). Důrazně ale doporučujeme, abyste si vytvořili trezor Recovery Services, protože všechna budoucí vylepšení budou dostupná jenom pro trezory Recovery Services. 

## Můžu migrovat trezor Backup do trezoru Služeb zotavení? <br/>
Bohužel ne. Obsah trezoru Backup nyní nejde migrovat do trezoru Služeb zotavení. Ačkoli na přidání této funkce pracujeme, v rámci verze Public Preview není dostupná.

## Podporují trezory Recovery Services klasické virtuální počítače nebo virtuální počítače využívající Resource Manager? <br/>
Trezory Recovery Services podporují oba modely.  Do trezoru Recovery Services můžete zálohovat virtuální počítač vytvořený na portálu Classic (to je virtuální počítač v klasickém režimu) nebo virtuální počítač vytvořený na webu Azure Portal (to je virtuální počítač založený Resource Manageru).

## Svoje klasické virtuální počítače jsem zálohoval v trezoru služby Backup. Teď chci svoje virtuální počítače migrovat z klasického režimu do režimu Resource Manageru.  Jakým způsobem je můžu zálohovat v trezoru Recovery Services?
Při migraci virtuálních počítačů z klasického režimu do režimu Resource Manageru se neprovede automatická migrace záloh klasických virtuálních počítačů do trezoru služby Backup. Při migraci záloh virtuálních počítačů použijte tento postup:

1. V trezoru služby Backup přejděte na kartu **Chráněné položky** a vyberte virtuální počítač. Klikněte na [Zastavit ochranu](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Políčko *Delete associated backup data* (Odstranit přidružená data záloh) ponechte **nezaškrtnuté**. 
2. Proveďte migraci virtuálního počítače z klasického režimu do režimu Resource Manageru. Ověřte, že se do režimu Resource Manager migruje také úložiště a síť odpovídající tomuto virtuálnímu počítači. 
3. Vytvořte trezor Recovery Services a ke konfiguraci zálohování migrovaného virtuálního počítače použijte akci **Zálohování** v horní části řídicího panelu trezoru. Další informace o postupu při [povolení zálohování v trezoru služby Recovery Services](backup-azure-vms-first-look-arm.md)



<!--HONumber=Sep16_HO5-->


