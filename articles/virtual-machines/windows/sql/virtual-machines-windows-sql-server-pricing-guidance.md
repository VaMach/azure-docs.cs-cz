---
title: "Efektivní správě náklady pro SQL Server na virtuálních počítačích Azure | Microsoft Docs"
description: "Obsahuje doporučené postupy pro výběr správné virtuálního počítače systému SQL Server cenový model."
services: virtual-machines-windows
documentationcenter: na
author: luisherring
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/18/2017
ms.author: jroth
ms.openlocfilehash: 29a92f0c70bffedeb75c50b7fc3b687ee5ee227d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pricing-guidance-for-sql-server-azure-vms"></a>Pokyny pro virtuální počítače SQL serveru Azure – ceny

Toto téma obsahuje cenovou pokyny pro virtuální počítače systému SQL Server v Azure. Existuje několik možností, které ovlivňují náklady a je důležité vybrat správné bitové kopie, který vyrovnává náklady obchodním požadavkům.

## <a name="free-licensed-sql-server-editions"></a>Uvolněte licenci edicích systému SQL Server

Pokud chcete vývoj, testování nebo sestavení testování konceptu, použijte volné aktivovaných **SQL Server Developer edition**. Tato edice obsahuje všechno, co v systému SQL Server Enterprise edition, proto můžete použít k vytvoření libovolnou aplikaci. Má jenom není povoleno spuštění v produkčním prostředí. Virtuální počítač systému SQL Server Developer vám bude účtovat pouze náklady na virtuální počítač není pro licencování SQL serveru.

Pokud chcete spustit prosté úlohy v produkčním prostředí (< 4 jádra, < 1 GB paměti, < 10 GB/databáze), pak použít volné aktivovaných **edice systému SQL Server Express**. Express virtuálního počítače s SQL se pouze účtují nákladů na virtuální počítač není licencování SQL.

Pro tyto vývoj/testování nebo lightweight produkčním prostředí můžete také šetřit peníze a vybrat menší velikost virtuálního počítače, který odpovídá těchto úloh. DS1v2 může být vhodný pro tyto úlohy.

Pokud chcete vytvořit virtuální počítač Azure SQL Server 2016 s jedním z těchto bitových kopií, v následujících tématech:

- [SQL Server 2016 vývojáře Azure virtuálních počítačů](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1DeveloperWindowsServer2016-ARM)
- [SQL Server 2016 Express virtuálních počítačů Azure](https://ms.portal.azure.com/#create/Microsoft.FreeLicenseSQLServer2016SP1ExpressWindowsServer2016-ARM)

## <a name="paid-sql-server-editions"></a>Placených edicích systému SQL Server

Pokud máte jiný lightweight produkční zatížení, použijte jednu z následujících edicích systému SQL Server:

| Edice systému SQL Server | Úloha |
|-----|-----|
| Web | Malé weby |
| Standard | Malé a střední zatížení |
| Enterprise | Velké soubory nebo důležité úlohy|

Máte dvě možnosti platit za licencování SQL serveru pro tyto edice: *platíte za použití* nebo *přineste si vlastní licenci*.

### <a name="pay-per-usage"></a>Platba za použití

**Licenci systému SQL Server za použití platícího** znamená, že za minutu náklady na provozování virtuálního počítače Azure obsahuje náklady na licence pro SQL Server. Zobrazí cenách pro různých edicích systému SQL Server (Web, Standard, Enterprise) v [virtuálního počítače Azure stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard). Náklady na je stejný pro všechny verze systému SQL Server (2008 R2 na 2016). Stejně jako u obecně licencování SQL serveru, náklady na licencování za minutu závisí na počtu jader virtuálního počítače.

Platícího systému SQL Server licencování za použití se doporučuje pro:

- Dočasné nebo pravidelné úlohy. Například aplikace, která musí podporovat událost pro několik měsíců každý rok nebo obchodní analýzy v pondělí.
- Úlohy s neznámé životnost nebo určený počet číslic. Například aplikace, nemusí být nutné několik měsíců, nebo které může být více nebo méně výpočetního výkonu, v závislosti na vyžádání.

Pokud chcete vytvořit virtuální počítač Azure SQL Server 2016 s jedním z těchto bitových kopií platím za použití, v následujících tématech:

- [SQL Server virtuálního počítače Azure webové 2016](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1WebWindowsServer2016)
- [SQL Server 2016 standardní virtuální počítač Azure](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1StandardWindowsServer2016)
- [SQL Server 2016 Enterprise Azure virtuálních počítačů](https://ms.portal.azure.com/#create/Microsoft.SQLServer2016SP1EnterpriseWindowsServer2016)

> [!IMPORTANT]
> Při vytváření virtuálního počítače s SQL serverem na portálu Azure, zobrazí odhadované měsíční náklady na **zvolte velikost** okno nezahrnuje náklady na licencování SQL serveru. Toto jsou náklady pouze na virtuální počítač.
>
> ![Vyberte virtuální počítač velikost okna](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>Volné Express a vývojáře edic systému SQL Server Toto je celkový počet odhadované náklady. Ale pro webové, Standard a Enterprise, najít další SQL licenční náklady na [virtuální počítače s Windows stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Na na stránce s cenami vyberte váš cílový edici systému SQL Server.

### <a name="bring-your-own-license-byol"></a>Používání vlastní licence (BYOL)

**Přináší vlastní licenci na SQL Server prostřednictvím mobilita licencí**, také nazývaný jako **BYOL**, znamená existující Volume License serveru SQL pomocí programu Software Assurance virtuální počítač Azure. Vzhledem k tomu, že jste už získali licence a programu Software Assurance prostřednictvím multilicenčního programu vám bude jenom virtuálního počítače s SQL serverem pomocí BYOL účtovat náklady na provozování virtuálního počítače, ne pro licencování SQL serveru.

Přináší vlastní SQL licencování prostřednictvím mobilita licencí se doporučuje pro:

- Nepřetržité úlohy. Například aplikace, která musí podporovat podnikové operace 24 x 7.
- Úlohy s známé životnost a škálování. Například aplikace, která se bude vyžadovat celý rok a který požadavek má byla naplánované.

Používat BYOL virtuálního počítače s SQL Server, musí mít licenci pro SQL Server Standard nebo Enterprise a [programu Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx#tab=1), což je požadovaná možnost prostřednictvím některé [Volume Licensing](https://www.microsoft.com/en-us/download/details.aspx?id=10585) programy a volitelné zakoupit s ostatními.  Cenové úrovně poskytované prostřednictvím multilicenčních programů se liší, na základě typu smlouvy a množství a nebo závazku k systému SQL Server. Ale jako existuje pravidlo, přináší vlastní licenci pro produkční nepřetržité úlohy má následující výhody:

| BYOL výhody | Popis |
|-----|-----|
| **Úspora nákladů** | Přináší vlastní licenci na SQL Server je nákladově efektivnější než platícího za použití, pokud zatížení se budou spouštět nepřetržitě SQL Server Standard nebo Enterprise pro *více než 10 měsíců*. |
| **Dlouhodobé úspory** | V průměru je *30 % levnější za jeden rok.* koupit nebo obnovit licenci SQL serveru pro první 3 roky. Kromě toho po 3 roky, nemusíte už obnovit licence, platit pouze pro Software Assurance. V tomto okamžiku je *200 % levnější*. |
| **Volné pasivní sekundární repliky** | Další výhodou přinesou vlastní licence je [volné licencování pro jeden pasivní sekundární repliky](https://azure.microsoft.com/pricing/licensing-faq/) na Server SQL pro vysokou dostupnost pro účely. To snižuje v poloviční licenční náklady na vysoce dostupné nasazení systému SQL Server (například použití skupin dostupnosti Always On). Práva pro spuštění pasivní sekundární jsou k dispozici prostřednictvím výhody Software Assurance servery převzetí služeb při selhání. |

Vytvoření virtuálního počítače s SQL Server 2016 Azure s jedním z těchto bitových kopií přineste si vlastníte license naleznete v tématu předponu "{BYOL}" virtuální počítače:

- [SQL Server 2016 Enterprise Azure virtuálních počítačů](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1EnterpriseWindowsServer2016)
- [SQL Server 2016 standardní virtuální počítač Azure](https://ms.portal.azure.com/#create/Microsoft.BYOLSQLServer2016SP1StandardWindowsServer2016)

> [!NOTE]
> Dejte nám vědět, do 10 dnů kolik licencí systému SQL Server, budete používat v Azure. Odkazy na předchozí bitové kopie jsou pokyny o tom, jak to udělat.

## <a name="avoid-unecessary-costs"></a>Vyhněte se unecessary náklady

Pokud používáte jakékoli úlohy, které nepoužívají nepřetržitě, vezměte v úvahu vypínání virtuálního počítače během období neaktivní. Platíte jenom za to, co používáte.

Pokud zkoušíte jednoduše na SQL Server na virtuální počítač Azure, by například chtít platit poplatky omylem ponecháte systémem týdny. Jeden řešením je použití [funkce automatického ukončení](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/).

![Virtuální počítač SQL autoshutdown](./media/virtual-machines-windows-sql-server-pricing-guidance/sql-vm-auto-shutdown.png)

Automatické ukončení je součástí větší sady podobné funkce poskytované službou [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab).

Pro jiné pracovní postupy, zvažte automaticky vypínání a restartování virtuálních počítačů Azure v rámci řešení pro skriptování, například [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!IMPORTANT]
> Vypíná a rušení přidělení virtuálního počítače je jedinou možností, aby se zabránilo poplatky. Jednoduše zastavení nebo pomocí možnosti napájení vypnout virtuální počítač stále způsobuje poplatky za používání.

## <a name="next-steps"></a>Další kroky

Obecné Azure ceny pokyny najdete v tématu [zabránit neočekávané náklady s Azure fakturace a náklady na správu](../../../billing/billing-getting-started.md).

Pro virtuální počítače, který nejnovější ceny, včetně systému SQL Server, najdete v článku [virtuálního počítače Azure stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-machines/sql-server-standard).

Přečtěte si další témata virtuálního počítače systému SQL Server na [SQL Server na Přehled virtuálních počítačů Azure](virtual-machines-windows-sql-server-iaas-overview.md).
