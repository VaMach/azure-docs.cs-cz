---
title: "Začínáme se soukromými šablonami | Dokumentace Microsoftu"
description: "Přidávejte, spravujte a sdílejte svoje soukromé šablony pomocí portálu Azure, rozhraní příkazového řádku Azure nebo PowerShellu."
services: marketplace-customer
documentationcenter: 
author: VybavaRamadoss
manager: asimm
editor: 
tags: marketplace, azure-resource-manager
keywords: 
ms.assetid: 6ec20778-b578-4885-acb5-104b0e51ea1a
ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar
ms.translationtype: Human Translation
ms.sourcegitcommit: a9b48f149427e5ceb69bcaa97b1bf08519499b6f
ms.openlocfilehash: 01657619cbe579c6818a790cc3ab95a33936a565
ms.contentlocale: cs-cz
ms.lasthandoff: 02/16/2017

---
# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Začínáme se soukromými šablonami na portálu Azure
Šablona [Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md) je deklarativní šablona, která slouží k definování nasazení. Můžete definovat, které prostředky se mají pro řešení nasadit, a určit parametry a proměnné, které vám umožní zadat hodnoty pro různá prostředí. Šablona se skládá z JSON a z výrazů, které můžete použít k vytvoření hodnot pro nasazení.

Můžete použít novou schopnost **Šablony** v [webu Azure Portal](https://portal.azure.com) spolu s poskytovatelem prostředků **Microsoft.Gallery** jako rozšíření [Azure Marketplace](https://azure.microsoft.com/marketplace/) a umožnit tak uživatelům vytváření, správu a nasazování soukromých šablon z osobní knihovny.

Tento dokument vás provede přidáním, správou a sdílením soukromé **šablony** pomocí webu Azure Portal.

## <a name="guidance"></a>Doprovodné materiály
Následující návrhy vám pomohou naplno využít výhody **šablon** při práci se svými řešeními:

* **Šablona** je zapouzdřující prostředek, který obsahuje šablonu Resource Manageru a další metadata. Chová se velmi podobně jako položka v Marketplace. Klíčovým rozdílem je, že se jedná o soukromou položku, na rozdíl od veřejných položek Marketplace.
* Knihovna **Šablony** dobře funguje pro uživatele, kteří potřebují přizpůsobit svá nasazení.
* **Šablony** dobře fungují pro uživatele, kteří potřebují jednoduché úložiště v rámci Azure.
* Začněte s existující šablonou Resource Manageru. Najděte šablony na [GitHubu](https://github.com/Azure/azure-quickstart-templates) nebo [exportujte šablonu](../azure-resource-manager/resource-manager-export-template.md) z existující skupiny prostředků.
* **Šablony** jsou vázané na uživatele, který je publikuje. Název vydavatele je viditelný každému, kdo k němu má přístup pro čtení.
* **Šablony** jsou prostředky Resource Manageru a po publikování je nelze přejmenovat.

## <a name="add-a-template-resource"></a>Přidání prostředku šablony
Existují dva způsoby, jak lze na portálu Azure vytvořit prostředek **šablony**.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Způsob 1: Vytvoření nového prostředku šablony ze spuštěné skupiny prostředků
1. Na portálu Azure přejděte do existující skupiny prostředků. V **Nastavení** vyberte **Exportovat šablonu**.
2. Když je šablona Resource Manageru exportovaná, uložte ji pomocí tlačítka **Uložit šablonu** do úložiště **Šablony**. Podrobné informace o exportování šablony najdete [zde](../azure-resource-manager/resource-manager-export-template.md).
   <br /><br />
   ![Export skupiny prostředků](media/rg-export-portal1.PNG)  <br />
3. Vyberte příkazové tlačítko **Uložit do šablony**.
   <br /><br />
4. Zadejte následující informace:
   
   * Název – Název objektu šablony (Poznámka: Jedná se o název založený na Azure Resource Manageru. Platí všechna omezení pro pojmenovávání a název nelze po vytvoření změnit).
   * Popis – Stručný popis šablony.
     
     ![Uložení šablony](media/save-template-portal1.PNG)  <br />
5. Klikněte na **Uložit**.
   
   > [!NOTE]
   > Obsahuje-li exportovaná šablona Resource Manageru chyby, v okně Exportovat šablonu se zobrazí oznámení, ale stále budete moci tuto šablonu Resource Manageru uložit do Šablon. Před tím, než znovu nasadíte exportovanou šablonu Resource Manageru, se ujistěte, že jste zkontrolovali a opravili všechny problémy šablony Resource Manageru.
   > 
   > 

### <a name="method-2--add-a-new-template-resource-from-browse"></a>Způsob 2: Přidání nového prostředku šablony z procházení
Můžete také přidat novou **šablonu** od začátku pomocí příkazového tlačítka +Přidat v **Procházet > Šablony**. Budete muset zadat Název, Popis a JSON šablony Resource Manageru.

![Přidání šablony](media/add-template-portal1.PNG)  <br />

> [!NOTE]
> Microsoft.Gallery je poskytovatel prostředků Azure umístěný na Tenantu. Prostředek šablony je vázán na uživatele, který ho vytvořil. Neváže se na konkrétní předplatné. Předplatné je potřeba zvolit pouze při nasazování šablony.
> 
> 

## <a name="view-template-resources"></a>Zobrazení prostředků šablony
Všechny **šablony**, které máte k dispozici, můžete zobrazit v **Procházet > Šablony**. To zahrnuje jak **šablony**, které jste vytvořili, tak i šablony, které jsou s vámi sdílené s různými úrovněmi oprávnění. Další podrobnosti naleznete níže v oddílu [Řízení přístupu](#access-control-for-a-tenant-resource-provider).

![Zobrazit šablonu](media/view-template-portal1.PNG)  <br />

Podrobnosti o **šabloně** zobrazíte kliknutím na položku v seznamu.

![Zobrazit šablonu](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Úprava prostředku šablony
Tok upravování **šablony** můžete zahájit kliknutím pravým tlačítkem na položku v seznamu Procházet nebo vybráním příkazového tlačítka Upravit.

![Úprava šablony](media/edit-template-portal1a.PNG)  <br />

Můžete upravit popis nebo text šablony Resource Manageru. Nemůžete upravit název, protože se jedná o název prostředku Resource Manageru. Pokud upravíte kód JSON šablony Resource Manageru, ověříme ho, abychom zajistili, že jde o platný kód JSON. Vyberte **OK** a poté aktualizovanou šablonu uložte kliknutím na **Uložit**.

![Úprava šablony](media/edit-template-portal2a.PNG)  <br />

Po uložení **šablony** se zobrazí oznámení o potvrzení.

![Úprava šablony](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Nasazení prostředku šablony
Můžete nasadit jakoukoli **šablonu**, ke které máte oprávnění ke **čtení**. Tok nasazení spustí standardní okno Nasazení šablony Azure. Vyplňte hodnoty parametrů šablony Resource Manageru a pokračujte s nasazováním.

![Nasazení šablony](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Úprava prostředku šablony
Prostředek **Šablony** můžete sdílet s ostatními uživateli. Sdílení se chová podobně jako [přiřazování rolí pro jakýkoli prostředek na Azure](../active-directory/role-based-access-control-configure.md). Vlastník **šablony** poskytuje oprávnění ostatním uživatelům, kteří mohou interagovat s prostředkem šablony. Osoba nebo skupina osob, se kterou **šablonu** sdílíte, uvidí šablonu Resource Manageru a její vlastnosti galerie.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Řízení přístupu pro prostředky Microsoft.Gallery
| Role | Oprávnění |
| --- | --- |
| Vlastník |Povoluje úplné řízení prostředku šablony včetně sdílení. |
| Čtenář |Povoluje čtení a spouštění (nasazování) prostředku šablony. |
| Přispěvatel |Povoluje upravování a právo odstranit prostředek šablony. Uživatel nemůže šablonu sdílet s ostatními. |

Kliknutím pravým tlačítkem nebo v okně zobrazení konkrétní položky vyberte na procházené položce **Sdílet** Spustí se Možnosti sdílení.

![Sdílení šablony](media/share-template-portal1a.png)  <br />

 Nyní můžete zvolit roli a uživatele nebo skupinu, kterým chcete poskytnout přístup ke konkrétní **šabloně**. Dostupné role jsou Vlastník, Čtenář a Přispěvatel. Další podrobnosti najdete výše v oddílu [Řízení přístupu](#access-control-for-a-tenant-resource-provider).

![Sdílení šablony](media/share-template-portal2b.png)  <br />

![Sdílení šablony](media/share-template-portal3b.png)  <br />

Klikněte na **Vybrat** a **Ok**. Nyní vidíte uživatele a skupiny, které jste přidali k prostředku.

![Sdílení šablony](media/share-template-portal4b.png)  <br />

> [!NOTE]
> Šablonu je možné sdílet pouze s uživateli a skupinami ve stejném tenantu služby Azure Active Directory. Pokud šablonu sdílíte s e-mailovou adresou, která není ve vašem tenantu, jejímu uživateli se odešle pozvánka s žádostí o připojení k tenantu v roli hosta.
> 
> 

## <a name="next-steps"></a>Další kroky
* Další informace o vytváření šablon Resource Manageru naleznete v tématu [Vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).
* Funkce, které můžete použít v šabloně Resource Manageru, jsou popsané v tématu [Funkce šablony](../azure-resource-manager/resource-group-template-functions.md).
* Doprovodné materiály o navrhování šablon naleznete v tématu [Osvědčené postupy pro navrhování šablon Azure Resource Manageru](../azure-resource-manager/best-practices-resource-manager-design-templates.md).


