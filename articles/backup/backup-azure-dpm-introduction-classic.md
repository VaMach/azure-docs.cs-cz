---
title: "Zálohování úloh DPM na portál Azure classic | Microsoft Docs"
description: "Úvod k zálohování serverů aplikace DPM pomocí služby zálohování Azure"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
keywords: "System Center Data Protection Manager, nástroje data protection manager, zálohy aplikace dpm"
ms.assetid: 8f23972b-d167-4231-b331-e198db3b18b4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;giridham;markgal
ms.openlocfilehash: a9a516cfdfaf4b95c4f0121a66e90f6e71206e9f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Příprava zálohování úloh do Azure pomocí DPM
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Server Azure Backup (klasické)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasické)](backup-azure-dpm-introduction-classic.md)
>
>

Tento článek obsahuje úvod do Microsoft Azure Backup používá k ochraně vašich serverů System Center Data Protection Manager (DPM) a úloh. Načtením ji budete porozumíte:

* Jak funguje Azure zálohování serveru
* Požadavky pro dosažení smooth zálohování prostředí
* Typické došlo k chybám a řešení problémů s nimi
* Podporované scénáře

System Center DPM zálohuje data souborů a aplikací. Data zálohovaná na DPM můžete uložit na pásce, na disku, nebo zálohovat do Azure se zálohováním Microsoft Azure. Aplikace DPM komunikuje s Azure Backup následujícím způsobem:

* **Aplikace DPM nasazená jako fyzický server nebo místní virtuální počítač** – Pokud aplikace DPM je nasazená jako fyzický server nebo jako virtuální počítač technologie Hyper-V místní data můžete zálohovat do trezoru služby Azure Backup kromě disku a pásky Zálohování.
* **Aplikace DPM nasazená jako virtuální počítač Azure** – ze System Center 2012 R2 s aktualizací 3, DPM dá nasadit jako virtuální počítač Azure. Pokud je aplikace DPM nasazená jako virtuální počítač Azure, které můžete zálohovat data na disky Azure připojené k virtuálnímu počítači DPM Azure nebo může přenést úložiště data prostřednictvím jejich zálohování až do trezoru zálohování Azure.

## <a name="why-backup-your-dpm-servers"></a>Proč zálohovat servery DPM?
Mezi výhody firmy pomocí služby Azure Backup k zálohování serverů aplikace DPM patří:

* Pro místní nasazení aplikace DPM můžete použít zálohování Azure jako alternativu k dlouhodobé nasazení na pásku.
* Pro nasazení aplikace DPM v Azure Azure Backup k přesměrování zpracování úloh úložiště z disku Azure vám umožní škálování uložením starších data v Azure Backup a nových dat na disku.

## <a name="how-does-dpm-server-backup-work"></a>Funkce Zálohování serveru aplikace DPM
Zálohování virtuálního počítače, nejprve v okamžiku snímek dat je potřeba. Služba Azure Backup v naplánovaném čase spustí úlohu zálohování a aktivuje rozšíření zálohování k pořízení snímku. Rozšíření zálohování koordinuje spolu se službou VSS v hosta k zajištění konzistence a vyvolá rozhraní API snímku objektu blob služby Azure Storage dosažení konzistence. To slouží k získání konzistentního snímku disků virtuálního počítače, aniž by bylo nutné ho vypnout.

Po provedení výpisu data se přenáší přes služba Azure Backup do trezoru záloh. Služba má na starosti identifikace a přenáší pouze bloky, které se změnily od poslední zálohy zvýšení efektivity sítě a úložiště záloh. Po dokončení přenosu dat se odebere snímku a vytvoří bod obnovení. Tento bod obnovení lze zobrazit v portálu Azure classic.

> [!NOTE]
> Pro virtuální počítače s Linuxem je možné pouze soubor zálohování s konzistentními.
>
>

## <a name="prerequisites"></a>Požadavky
Příprava Azure Backup k zálohování dat aplikace DPM následujícím způsobem:

1. **Vytvořte úložiště záloh**. Pokud jste dosud nevytvořili úložiště záloh ve vašem předplatném, najdete v části portálu Azure verzi tohoto článku - [Příprava zálohování úloh do Azure pomocí DPM](backup-azure-dpm-introduction.md).

  > [!IMPORTANT]
  > Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.
  > Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup. **Do 1. listopadu 2017:**
  >- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
  >- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
  >

2. **Stažení přihlašovacích údajů trezoru** – ve službě Azure Backup odešlete certifikát správy, který jste vytvořili do trezoru.
3. **Instalace agenta zálohování Azure a zaregistrujte serveru** – z Azure Backup, nainstalujte agenta na každém serveru DPM a registrace serveru DPM v trezoru záloh.

[!INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[!INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]

## <a name="requirements-and-limitations"></a>Požadavky (a omezení)
* Aplikace DPM může být spuštěná jako fyzický server nebo virtuální počítač technologie Hyper-V nainstalovaná v System Center 2012 SP1 nebo System Center 2012 R2. Můžete také používat jako virtuální počítač Azure používá aspoň na System Center 2012 R2 s kumulativní aktualizace 3 pro DPM 2012 R2 nebo virtuálního počítače s Windows v prostředí VMWare alespoň systémem System Center 2012 R2 s kumulativní aktualizací 5.
* Pokud spouštíte aplikaci DPM s nástrojem System Center 2012 SP1, nainstalujte kumulativní aktualizaci 2 pro System Center Data Protection Manager SP1. To je potřeba, před instalací programu Azure Backup Agent.
* Server aplikace DPM by měl mít prostředí Windows PowerShell a rozhraní .net Framework 4.5 nainstalované.
* Aplikace DPM můžete zálohovat většinu úloh do služby Azure Backup. Úplný seznam, co je podporováno najdete podporovat Azure Backup položky dole.
* Data uložená ve službě Azure Backup nelze obnovit pomocí možnosti "Kopírovat na pásku".
* Budete potřebovat účet Azure s povolenou funkcí zálohování Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Přečtěte si informace o [cenách zálohování Azure](https://azure.microsoft.com/pricing/details/backup/).
* Použití zálohování Azure vyžaduje Azure Backup Agent k instalaci na serverech, které chcete zálohovat. Každý server musí mít minimálně 10 % velikosti dat, která se zálohuje, k dispozici jako místního volného úložného. Například zálohování 100 GB dat vyžaduje minimálně 10 GB volného místa v pomocné umístění. Při minimální hodnota je 10 %, se doporučuje 15 % místního volného úložného místa pro umístění mezipaměti.
* Data budou uložena v úložišti Azure trezoru. Neexistuje žádné omezení množství dat, které jste můžete zálohovat Azure Backup trezoru, ale velikost zdroje dat (třeba virtuální počítač nebo databáze) by neměl být delší než 54,400 GB.

Tyto typy souborů jsou podporovány pro zálohování na Azure:

* Šifrované (pouze úplné zálohy)
* Komprimované (je podporováno přírůstkové zálohování)
* Zhuštěné (je podporováno přírůstkové zálohování)
* Komprimované a zhuštěné (zpracovány jako zhuštěné)

A tyto nejsou podporovány:

* Servery v systémech souborů s rozlišením velkých nejsou podporovány.
* Pevné odkazy (vynecháno)
* Body rozboru (vynecháno)
* Zašifrované a komprimované (vynecháno)
* Šifrované a zhuštěné (vynecháno)
* Komprimovaný datový proud
* Zhuštěný datový proud

> [!NOTE]
> Z v System Center DPM 2012 s aktualizací SP1 a vyšší, můžete zálohovat do úlohy, které jsou chráněné pomocí DPM do Azure pomocí služby Microsoft Azure Backup.
>
>
