---
title: "Použít výkonu doporučení – Azure SQL Database | Microsoft Docs"
description: "Použití portálu Azure k vyhledání doporučení výkonu, které můžete optimalizovat výkon vaší databáze SQL Azure."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 3c621fc557ed466ddf2b514136a32d98be454325
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="find-and-apply-performance-recommendations"></a>Najít a použít doporučení výkonu

Portálu Azure můžete použít k vyhledání doporučení výkonu, které můžete optimalizovat výkon vaší databáze SQL Azure nebo opravte některé problém uvedený v vaše úlohy. **Výkon doporučení** stránka na portálu Azure umožňuje najít nejvyšší doporučení podle jejich potenciální vliv. 

## <a name="viewing-recommendations"></a>Zobrazení doporučení

Pokud chcete zobrazit a použít doporučení pro optimální výkon, je nutné správný [řízení přístupu na základě role](../active-directory/role-based-access-control-what-is.md) oprávnění v Azure. **Čtečka**, **Přispěvatel databází SQL** zobrazíte doporučení, jsou vyžadována oprávnění a **vlastníka**, **Přispěvatel databází SQL** se vyžadují oprávnění k provádět žádné akce; Vytvořit nebo vyřadit indexy a zrušit vytváření indexu.

Najít výkonu doporučení na portálu Azure pomocí následujících kroků:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. Přejděte na **další služby** > **databází SQL**a vyberte svou databázi.
3. Přejděte na **výkonu doporučení** zobrazíte doporučení k dispozici pro vybranou databázi.

Výkon doporučení jsou uvedené v tabulce podobně jako na následujícím obrázku:

![Doporučení](./media/sql-database-advisor-portal/recommendations.png)

Doporučení jsou seřazené podle jejich potenciální dopad na výkon do následujících kategorií:

| Dopad | Popis |
|:--- |:--- |
| Vysoký |Vysoký dopad na doporučení by měl poskytovat nejvýraznější dopad na výkon. |
| Střednědobé používání |Střední dopad doporučení by měla zvýšit výkon, ale není podstatně. |
| Nízký |Nízký dopad doporučení by měl poskytovat lepší výkon než bez, ale nemusí být výrazné vylepšení. |


> [!NOTE]
> Databáze SQL Azure je potřeba sledovat aktivity alespoň za den za účelem zjištění několik doporučení. Databáze SQL Azure můžete snadněji optimalizovat konzistentní dotazu v případě vzorů, než je to možné na náhodných problematického shluky aktivity. Pokud nejsou momentálně k dispozici, doporučení **výkonu doporučení** stránka obsahuje zprávu s vysvětlením, proč.
> 

Můžete také zobrazit stav přehled činností. Vyberte doporučení nebo stav zobrazíte další podrobnosti.

Tady je příklad "Vytvořit index" doporučení na portálu Azure.

![Vytvoření indexu](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Uplatňovat doporučení
Databáze SQL Azure vám dává plnou kontrolu nad jak doporučení jsou povolit pomocí některé z následujících tří možností: 

* Používat jednotlivé doporučení jeden v daném okamžiku.
* Povolte automatické ladění pro automatické použití doporučení.
* Pokud chcete doporučení implementovat ručně, spusťte doporučené skript T-SQL databáze.

Vyberte všechny doporučení zobrazit jeho podrobnosti a potom klikněte na **zobrazit skript** ke kontrole přesné údaje o tom, jak vytvořit doporučení.

Databáze zůstane online, zatímco doporučení platí – pomocí výkonu doporučení nebo automatické ladění nikdy převede do režimu offline databáze.

### <a name="apply-an-individual-recommendation"></a>Jednotlivá doporučení použít
Můžete zkontrolovat a přijmout doporučení jeden najednou.

1. Na **doporučení** vyberte doporučení.
2. Na **podrobnosti** klikněte na tlačítko **použít** tlačítko.
   
    ![použít doporučení](./media/sql-database-advisor-portal/apply.png)

Vybrané doporučení se používají v databázi.

### <a name="removing-recommendations-from-the-list"></a>Odebrání doporučení ze seznamu
Pokud seznam doporučení obsahuje položky, které chcete odebrat ze seznamu, můžete zrušit doporučení:

1. V seznamu vyberte doporučení **doporučení** otevřete podrobnosti.
2. Klikněte na tlačítko **zahodit** na **podrobnosti** stránky.

V případě potřeby můžete přidat vyřazené položky zpět do **doporučení** seznamu:

1. Na **doporučení** klikněte na tlačítko **zobrazení zahozena**.
2. Vyberte položku zrušených ze seznamu zobrazíte její podrobnosti.
3. Volitelně klikněte na **vrátit zpět zahodit** přidat index zpět do hlavní seznam **doporučení**.


### <a name="enable-automatic-tuning"></a>Povolení automatického ladění
Můžete nastavit Azure SQL Database k implementaci doporučení automaticky. Doporučení jsou k dispozici, budou automaticky použita. Stejně jako u všech doporučení, spravováno službou, pokud je negativní dopad na výkon, je vrácen doporučení.

1. Na **doporučení** klikněte na tlačítko **automatické**:
   
    ![Nastavení služby Advisor](./media/sql-database-advisor-portal/settings.png)
2. Vyberte akce automatizovat:
   
    ![Doporučené indexy](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-the-recommended-t-sql-script"></a>Ručně spustit doporučené skriptu T-SQL
Vyberte všechny doporučení a pak klikněte na tlačítko **zobrazit skript**. Spusťte tento skript databázi doporučení použít ručně.

*Indexy, které jsou spouštěny ručně nejsou sledovat a ověřit pro vlivu na výkon službou* , doporučujeme sledovat tyto indexy po vytvoření ověřte se poskytují zvýšení výkonu a upravit nebo odstranit v případě potřeby. Podrobnosti o vytváření indexů najdete v tématu [CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Zrušení doporučení
Doporučení, které jsou v **čekající**, **ověření**, nebo **úspěch** stav se dají zrušit. Doporučení se stavem **zpracování** nelze zrušit.

1. Vyberte doporučení v **ladění historie** oblasti otevřete **podrobnosti doporučení** stránky.
2. Klikněte na tlačítko **zrušit** k přerušení proces použití doporučení.

## <a name="monitoring-operations"></a>Monitorování operací
Použití doporučeným nemusí dojít okamžitě. Portál obsahuje podrobné informace týkající se stavu doporučení. Toto jsou možné stavy, které může být index:

| Status | Popis |
|:--- |:--- |
| Čekající na vyřízení |Platí doporučení, příkaz byl přijat a je naplánováno spuštění. |
| Provádění |Doporučuje se právě používá. |
| Ověření |Bylo úspěšně aplikováno doporučení a službu je měření výhody. |
| Úspěch |Bylo úspěšně aplikováno doporučení a měří výhody. |
| Chyba |Při zavádění doporučení se stala chyba. Může to být dočasný problém, nebo které by mohly mít schéma změnit do tabulky a skript již není platný. |
| Návrat |Doporučení bylo použito, ale je považována za nenáročných a automaticky zrušeny. |
| Vrátit zpět |Doporučuje se vrátila. |

Kliknutím na doporučení v procesu v seznamu zobrazíte další podrobnosti:

![Doporučené indexy](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Při vrácení doporučení
Pokud jste použili výkonu doporučení k doporučení použít (což znamená, že nebyl spuštěn ručně skriptu T-SQL), se automaticky vrátí změnu Pokud najde negativní dopad na výkon. Pokud z nějakého důvodu chcete jednoduše obnovit doporučení, můžete provést následující:

1. Vyberte úspěšně použité doporučení v **ladění historie** oblasti.
2. Klikněte na tlačítko **vrátit** na **podrobnosti o doporučení** stránky.

![Doporučené indexy](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Monitorování vlivu na výkon doporučení indexu
Po doporučení jsou úspěšně implementováno (v současné době operace indexu a Parametrizace pouze dotazy doporučení), můžete kliknout na **přehled dotazu** na doporučení podrobnosti stránky otevřete [dotazu Přehled o výkonu](sql-database-query-performance.md) a zobrazit dopad na výkon nejčastějších dotazů.

![Dopad na výkon monitorování](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Souhrn
Databáze SQL Azure poskytuje doporučení pro zlepšení výkonu databáze SQL. Tím, že poskytuje skriptů T-SQL, získáte pomoc při optimalizaci vaši databázi a nakonec zlepšení výkonu dotazů.

## <a name="next-steps"></a>Další kroky
Sledovat vaše doporučení a pokračuje v používání jejich Upřesnit výkonu. Databázové úlohy jsou dynamické a průběžně změnu. Azure SQL Database i nadále monitorovat a poskytovat doporučení, které může zlepšit výkon vaší databáze. 

* V tématu [automatické ladění](sql-database-automatic-tuning.md) Další informace o automatické ladění ve službě Azure SQL Database.
* V tématu [výkonu doporučení](sql-database-advisor.md) přehled o výkonu doporučení Azure SQL Database.
* V tématu [Query Performance Insight](sql-database-query-performance.md) Další informace o zobrazení dopad na výkon nejčastějších dotazů.

## <a name="additional-resources"></a>Další zdroje
* [Úložiště dotazů](https://msdn.microsoft.com/library/dn817826.aspx)
* [VYTVOŘENÍ INDEXU](https://msdn.microsoft.com/library/ms188783.aspx)
* [Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md)

