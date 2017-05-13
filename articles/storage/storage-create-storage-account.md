---
title: "Jak vytvořit, spravovat nebo odstranit účet úložiště na webu Azure Portal | Dokumentace Microsoftu"
description: "Přečtěte si, jak vytvořit nový účet úložiště, spravovat klíče pro přístup k účtu nebo odstranit účet na webu Azure Portal. Získejte informace o účtech úložiště Standard a Premium."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 87c37da0-6cc6-4d88-a330-ef2896a1531d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/23/2017
ms.author: robinsh
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 33988eaaf926dcd91a1e63ae766e815cda93cf03
ms.contentlocale: cs-cz
ms.lasthandoff: 05/10/2017


---
# <a name="about-azure-storage-accounts"></a>Informace o účtech Azure Storage
[!INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Přehled
Účet úložiště Azure Storage poskytuje jedinečný obor názvů pro ukládání datových objektů Azure Storage a přístup k nim. Všechny objekty v rámci účtu úložiště se fakturují společně jako skupina. Ve výchozím nastavení jsou data ve vašem účtu dostupná pouze pro vás, vlastníka účtu.

[!INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Fakturace účtu úložiště
[!INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [!NOTE]
> Když vytvoříte virtuální počítač Azure, účet úložiště se vám vytvoří automaticky v umístění nasazení, pokud ještě nemáte účet úložiště v tomto umístění. Není proto podle následujících kroků nutné vytvořit účet úložiště pro disky virtuálního počítače. Název účtu úložiště bude založený na názvu virtuálního počítače. Další podrobnosti najdete v [dokumentaci ke službě Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/). 
> 
> 

## <a name="storage-account-endpoints"></a>Koncové body účtu úložiště
Každý objekt, který uložíte v úložišti Azure Storage, má jedinečnou adresu URL. Název účtu úložiště tvoří subdoménu dané adresy. Kombinace názvu domény a subdomény, která je pro každou službu specifická, tvoří *koncový bod* vašeho účtu úložiště.

Pokud je například název účtu úložiště *můj_účet_úložiště*, pak jsou výchozí koncové body pro váš účet úložiště tyto:

* Služba Blob service: http://*můj_účet_úložiště*.blob.core.windows.net
* Služba Table: http://*můj_účet_úložiště*.table.core.windows.net
* Služba front: http://*můj_účet_úložiště*.queue.core.windows.net
* Služba souborů: http://*můj_účet_úložiště*.file.core.windows.net

> [!NOTE]
> Účet úložiště Blob Storage zpřístupňuje pouze koncový bod Služby objektů Blob.
> 
> 

Adresa URL pro přístup k objektu v účtu úložiště je sestavena připojením umístění objektu v účtu úložiště ke koncovému bodu. Například adresa účtu pro objekty blob může mít tento formát: http://*můj_účet_úložiště*.blob.core.windows.net/*můj_kontejner*/*můj_objekt_blob*.

Můžete také nakonfigurovat vlastní název domény, který budete chtít se svým účtem úložiště používat. Pro účty úložiště Classic najdete informace v tématu věnovaném [konfiguraci vlastního názvu doménu pro koncový bod služby Blob Storage](storage-custom-domain-name.md). Pro účty úložiště Resource Manager ještě tato funkce nebyla přidána na [Portál Azure](https://portal.azure.com), můžete ji ale konfigurovat pomocí PowerShellu. Další informace najdete v tématu věnovaném rutině [Set AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx).  

## <a name="create-a-storage-account"></a>vytvořit účet úložiště
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra vyberte **Nový** -> **Úložiště** -> **Účet úložiště**.
3. Zadejte název účtu úložiště. Podrobnosti o tom, jak se bude název vašeho účtu úložiště používat k adresování vašich objektů v úložišti Azure Storage, najdete v tématu [Koncové body účtu úložiště](#storage-account-endpoints).
   
   > [!NOTE]
   > Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena.
   > 
   > Váš název účtu úložiště musí být jedinečný v rámci Azure. Pokud se už název účtu úložiště, který vyberete, používá, bude tato informace uvedena na Portálu Azure.
   > 
   > 
4. Zadejte model nasazení, který se má použít: **Resource Manager** nebo **Classic**. Doporučuje se používat model nasazení **Resource Manager**. Další informace najdete v tématu [Vysvětlení nasazení pomocí modelu nasazení Resource Manager a modelu Classic](../azure-resource-manager/resource-manager-deployment-model.md).
   
   > [!NOTE]
   > Účty úložiště Blob Storage je možné vytvářet pouze pomocí modelu nasazení Resource Manageru.
   > 
   > 
5. Vyberte typ účtu úložiště: **pro obecné účely** nebo **Blob Storage**. Ve výchozím nastavení je vybraný účet **pro obecné účely**.
   
    Pokud je vybraný typ **pro obecné účely**, zadejte úroveň výkonu: **Standard** nebo **Premium**. Výchozí nastavení je **Standard**. Další podrobnosti o účtech úložiště úrovně Standard a Premium najdete v tématu [Úvod do Microsoft Azure Storage](storage-introduction.md) a [Storage úrovně Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).
   
    Pokud bylo vybráno úložiště **Blob Storage**, zadejte úrovně přístupu: **Hot** nebo **Cool**. Výchozí hodnota je **Hot**. Další podrobnosti najdete v tématu [Azure Blob Storage: úrovně Cool a Hot](storage-blob-storage-tiers.md).
6. Vyberte možnost replikace pro účet úložiště: **LRS**, **GRS**, **RA-GRS** nebo **ZRS**. Výchozí hodnota je **RA-GRS**. Další informace o možnostech replikace pro Azure Storage najdete v tématu [Replikace Azure Storage](storage-redundancy.md).
7. Vyberte předplatné, ve kterém chcete vytvořit nový účet úložiště.
8. Zadejte novou skupinu prostředků nebo vyberte existující skupinu prostředků. Další informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).
9. Vyberte zeměpisné umístění účtu úložiště. V článku [Oblasti Azure](https://azure.microsoft.com/regions/#services) najdete další informace o tom, které služby jsou dostupné v daných oblastech.
10. Vytvořte účet úložiště kliknutím na **Vytvořit**.

## <a name="manage-your-storage-account"></a>Správa účtu úložiště
### <a name="change-your-account-configuration"></a>Změna konfigurace účtu
Po vytvoření účtu úložiště můžete upravit jeho konfiguraci, například můžete změnit možnost replikace používanou pro účet nebo změnit úroveň přístupu pro účet úložiště Blob Storage. Na [Portálu Azure](https://portal.azure.com) přejděte na svůj účet úložiště, klikněte na **Všechna nastavení** a potom klikněte na **Konfigurace**. Tady pak můžete zobrazit nebo změnit konfiguraci účtu.

> [!NOTE]
> V závislosti na úrovni výkonu, kterou jste zvolili při vytváření účtu úložiště, nemusí být některé možnosti replikace dostupné.
> 
> 

Změnou možnosti replikace se vám také změní cena za předplatné. Další informace najdete na stránce [s cenami za Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Pro účty úložiště Blob Storage se vám může nejenom změní cena za předplatné, ale můžou se vám také naúčtovat poplatky za změnu úrovně přístupu. Další podrobnosti najdete v části [Účty Blob Storage – ceny a fakturace](storage-blob-storage-tiers.md#pricing-and-billing).

### <a name="manage-your-storage-access-keys"></a>Správa přístupových klíčů k úložišti
Při vytváření účtu úložiště vygeneruje Azure dva 512bitové přístupové klíče k úložišti, které se používají pro ověřování přístupu k účtu úložiště. Poskytnutím dvou přístupových klíčů k úložišti vám Azure umožňuje znovu vygenerovat klíče bez přerušení poskytování vaší služby úložiště nebo přístupu k této službě.

> [!NOTE]
> Doporučujeme vám přístupové klíče k úložišti s nikým nesdílet. Pokud budete chtít povolit přístup k prostředkům úložiště bez poskytnutí přístupových klíčů, můžete použít *sdílený přístupový podpis*. Sdílený přístupový podpis poskytuje přístup k prostředku v rámci vašeho účtu pro časový interval, který nadefinujete, a s oprávněními, která zadáte. Další informace najdete v tématu [Použití sdílených přístupových podpisů (SAS)](storage-dotnet-shared-access-signature-part-1.md).
> 
> 

#### <a name="view-and-copy-storage-access-keys"></a>Zobrazení a zkopírování přístupových klíčů k úložišti
Na [Portálu Azure](https://portal.azure.com) přejděte na svůj účet úložiště, klikněte na **Všechna nastavení** a potom klikněte na **Přístupové klíče**. Tady si pak můžete zobrazit, zkopírovat a znovu vygenerovat přístupové klíče ke svému účtu. Okno **Přístupové klíče** také obsahuje předem nakonfigurované připojovací řetězce s využitím primárního a sekundárního klíče, které můžete zkopírovat a používat ve svých aplikacích.

#### <a name="regenerate-storage-access-keys"></a>Opětovné vygenerování přístupových klíčů k úložišti
Doporučujeme přístupové klíče k účtu úložiště pravidelně měnit, aby byla připojení k úložišti stále zabezpečená. Dva přístupové klíče se přiřazují proto, abyste mohli pro připojení k účtu úložiště používat jeden přístupový klíč, zatímco si znovu vygenerujete druhý přístupový klíč.

> [!WARNING]
> Opětovné generování přístupových klíčů může mít vliv na služby v Azure a také na vaše vlastní aplikace, které jsou na účtu úložiště závislé. Všichni klienti, kteří používají přístupový klíč pro přístup k účtu úložiště, se musí aktualizovat na používání nového klíče.
> 
> 

**Media Services** – Pokud máte služby Media Services, které jsou závislé na vašem účtu úložiště, musíte přístupové klíče znovu synchronizovat s touto službou po opětovném vygenerování klíčů.

**Aplikace** – Pokud máte webové aplikace nebo cloudové služby, které používají účet úložiště, dojde při opětovném vygenerování klíčů ke ztrátě připojení, pokud klíče nezaregistrujete.

**Průzkumníci úložiště** – Pokud používáte některou z [aplikací průzkumníka úložiště](storage-explorers.md), budete pravděpodobně muset aktualizovat klíč úložiště používaný těmito aplikacemi.

Tady je proces pro výměnu přístupových klíčů k úložišti:

1. Aktualizujte připojovací řetězce v kódu aplikace tak, aby odkazovaly na sekundární přístupový klíč účtu úložiště.
2. Znovu vygenerujte primární přístupový klíč pro účet úložiště. V okně **Přístupové klíče** klikněte na **Znovu vygenerovat klíč1** a potom kliknutím na **Ano** potvrďte, že chcete vygenerovat nový klíč.
3. Aktualizujte připojovací řetězce v kódu tak, aby odkazovaly na nový primární přístupový klíč.
4. Stejným způsobem pak opětovně vygenerujte sekundární přístupový klíč.

## <a name="delete-a-storage-account"></a>Odstranění účtu úložiště
Pokud chcete odebrat účet úložiště, který už nepoužíváte, přejděte na účet úložiště na [Portálu Azure](https://portal.azure.com) a klikněte na **Odstranit**. Odstraněním účtu úložiště se odstraní celý účet, včetně všech dat v účtu.

> [!WARNING]
> Odstraněný účet úložiště není možné obnovit ani není možné načíst žádný obsah, který byl součástí účtu před jeho odstraněním. Nezapomeňte si před odstraněním účtu zazálohovat všechno, co chcete uložit. To platí také pro všechny prostředky v rámci účtu – po odstranění jsou objekt blob, tabulka, fronta nebo soubor odstraněny trvale.
> 
> 

Pokud chcete odstranit účet úložiště, který je přidružený k virtuálnímu počítači Azure, musíte nejdřív zajistit, aby se odstranily všechny disky virtuálního počítače. Pokud nejdříve neodstraníte disky virtuálního počítače, zobrazí se vám pak při pokusu o odstranění účtu úložiště chybová zpráva podobná této:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Pokud účet úložiště používá model nasazení Classic, můžete disk virtuálního počítače odebrat pomocí následujících kroků na [Portálu Azure](https://manage.windowsazure.com):

1. Přejděte na [Portál Azure Classic](https://manage.windowsazure.com).
2. Přejděte na kartu Virtuální počítače.
3. Klikněte na kartu Disky.
4. Vyberte datový disk a potom klikněte na Odstranit disk.
5. Pokud chcete odstranit diskové image, přejděte na kartu Image a odstraňte všechny image, které jsou uložené v rámci účtu.

Další informace najdete v [dokumentaci ke službě Azure Virtual Machine](http://azure.microsoft.com/documentation/services/virtual-machines/). 

## <a name="next-steps"></a>Další kroky
* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je bezplatná samostatná aplikace od Microsoftu, která umožňuje vizuálně pracovat s daty Azure Storage ve Windows, macOS a Linuxu.
* [Azure Blob Storage: úrovně Cool a Hot](storage-blob-storage-tiers.md)
* [Účet replikace Azure Storage](storage-redundancy.md)
* [Nakonfigurování připojovacích řetězců Azure Storage](storage-configure-connection-string.md)
* [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
* Navštivte [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/).


