---
title: "Řešení v Operations Management Suite (OMS) | Microsoft Docs"
description: "Řešení rozšířit funkce služby Operations Management Suite (OMS) tím, že poskytuje scénářů zabalené správy, které zákazníci mohou přidat do jejich pracovním prostorem OMS.  Tento článek poskytuje podrobné informace o tom, jak vlastní řešení vytvořené zákazníci a partneři."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 1f054a4e-6243-4a66-a62a-0031adb750d8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2443dd73fdf441721bd6f6f340da515d9f5a22a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-management-solutions-in-operations-management-suite-oms-preview"></a>Práce s řešení pro správu v Operations Management Suite (OMS) (Preview)
> [!NOTE]
> Toto je předběžná dokumentace pro řešení pro správu v OMS, které jsou aktuálně ve verzi preview.    
> 
> 

Řešení pro správu rozšířit funkce služby Operations Management Suite (OMS) tím, že poskytuje zabalené správu scénáře, které zákazníci můžete přidat do svého prostředí.  Kromě [řešení od společnosti Microsoft](../log-analytics/log-analytics-add-solutions.md), partnery a zákazníky, můžete vytvořit řešení pro správu k použití ve svém vlastním prostředí nebo přístupná zákazníkům prostřednictvím komunitou.

## <a name="finding-and-installing-management-solutions"></a>Hledání a instalace řešení pro správu
Existuje více metod pro vyhledání a instalace řešení pro správu, jak je popsáno v následujících částech.

### <a name="azure-marketplace"></a>Azure Marketplace
Řešení pro správu od společnosti Microsoft a důvěryhodným partnerům může být nainstalována v Azure Marketplace na portálu Azure.

1. Přihlaste se k portálu Azure.
2. V levém podokně vyberte **další služby**.
3. Buď přejděte dolů k položce **řešení** nebo typ *řešení* do **filtru** dialogové okno.
4. Klikněte **+ přidat** tlačítko.
5. Vyhledejte řešení, která vás zajímá buď procházením, kliknutím na **filtru** tlačítko nebo zadáním v **vyhledávání Everthing** pole.
6. Klikněte na položku marketplace zobrazíte její podrobné informace.
7. Klikněte na tlačítko **vytvořit** otevřete **přidat řešení** podokně.
8. Zobrazí se výzva k požadované informace, jako [OMS pracovní prostor a účet Automation](#oms-workspace-and-automation-account) kromě hodnoty všech parametrů v řešení.
9. Klikněte na tlačítko **vytvořit** instalací řešení.

### <a name="oms-portal"></a>Portálu OMS
Řešení pro správu od společnosti Microsoft může být nainstalována z Galerie řešení na portálu OMS.

1. Přihlaste se k portálu OMS.
2. Klikněte **řešení Galerie** dlaždici.
3. Na stránce Galerie řešení OMS informace o jednotlivých k dispozici řešení. Klikněte na název řešení, které chcete přidat do OMS.
4. Na stránce pro řešení, které jste vybrali zobrazí se podrobné informace o řešení. Klikněte na tlačítko **Přidat**.
5. Nová dlaždice pro řešení, které jste přidali, zobrazí se na Přehled stránky v OMS a vy můžete začít používat po OMS služba zpracovává vaše data.

### <a name="azure-quickstart-templates"></a>Rychlý úvod do šablon pro Azure
Členové komunity můžete odeslat řešení pro správu šablon Azure rychlý start.  Můžete stahovat tyto šablony pro pozdější instalaci nebo je další zkontrolovat postup [vytvářet vlastní řešení](#creating-a-solution).

1. Postupujte podle procesu popsaného v tématu [OMS pracovní prostor a účet Automation](#oms-workspace-and-automation-account) propojit pracovní prostor a účet.
2. Přejděte na [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).  
3. Hledání řešení, které vás zajímají.
4. Vyberte řešení z výsledků zobrazíte její podrobnosti.
5. Klikněte **nasadit do Azure** tlačítko.
6. Zobrazí se výzva k poskytují informace, jako je skupina prostředků a umístění kromě hodnoty všech parametrů v řešení.
7. Klikněte na tlačítko **nákupu** instalací řešení.

### <a name="deploy-azure-resource-manager-template"></a>Nasazení šablony Azure Resource Manageru
Řešení, které jste získali od komunity nebo [vytvořit sami](#creating-a-solution) jsou implementované jako šablony Resource Manageru, a můžete použít libovolnou metodu standardní pro [nasazení šablony](../azure-resource-manager/resource-group-template-deploy-portal.md).  Všimněte si, že před instalací řešení, musíte vytvořit a odkaz [OMS pracovní prostor a účet Automation](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>Pracovní prostor OMS a účet Automation.
Většina řešení pro správu vyžadují [pracovním prostorem OMS](../log-analytics/log-analytics-manage-access.md) tak, aby obsahovala zobrazení a [účet Automation](../automation/automation-security-overview.md#automation-account-overview) tak, aby obsahovala sady runbook a související prostředky. Pracovní prostor a účet, musí splňovat následující požadavky.

* Řešení lze použít pouze jeden pracovní prostor OMS a jeden účet Automation.  
* Pracovní prostor OMS a účet Automation používá řešení musí být propojena na sebe navzájem. Pracovní prostor služby OMS může propojit jen s jeden účet Automation a účet Automation může propojit jen s jeden pracovní prostor OMS.
* Propojení, tento pracovní prostor OMS a účet Automation musí být ve stejné skupině prostředků a oblast.  Jedinou výjimkou je pracovní prostor služby OMS v oblasti Východ USA a a účet Automation v oblasti Východ USA 2.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Vytváření propojení mezi pracovním prostorem OMS a účet Automation.
Jak je zadat pracovní prostor OMS a účet Automation závisí na metodě instalace pro vaše řešení.

* Při instalaci řešení společnosti Microsoft prostřednictvím portálu OMS je nainstalován v aktuálním pracovním prostorem OMS a není třeba žádný účet Automation.
* Když instalujete řešení prostřednictvím Azure Marketplace, budete vyzváni pracovním prostorem OMS a účet Automation a se vám vytvoří propojení mezi nimi.  
* Pro řešení mimo Azure Marketplace je nutné před instalací řešení propojit pracovní prostor OMS a účet Automation.  Můžete provést zvolením řešení v Azure Marketplace a výběr pracovním prostorem OMS a účet Automation.  Nemáte skutečně nainstalovat řešení, protože odkaz se vytvoří, jakmile jsou vybrané pracovní prostor OMS a účet Automation.  Po vytvoření odkazu, tento pracovní prostor OMS a účet Automation můžete použít pro žádné řešení. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Ověření propojení mezi pracovním prostorem OMS a účet Automation.
Můžete ověřit propojení mezi pracovním prostorem OMS a účtu Automation pomocí následujícího postupu.

1. Vyberte účet Automation na portálu Azure.
2. Přejděte do dolní části **nastavení** podokně.
3. Pokud je oddíl s názvem **OMS prostředky** v **nastavení** podokně a pak tento účet je připojen k pracovnímu prostoru OMS.
4. Vyberte **prostoru** k zobrazení podrobností o pracovním prostorem OMS propojené k tomuto účtu Automation.

## <a name="listing-management-solutions"></a>Výpis řešení pro správu
Chcete-li zobrazit řešení pro správu v pracovní prostory přidružený k vašemu předplatnému Azure se používá následující postup.

1. Přihlaste se k portálu Azure.
2. V levém podokně vyberte **další služby**.
3. Buď přejděte dolů k položce **řešení** nebo typ *řešení* do **filtru** dialogové okno.
4. Objeví se řešení, které jsou nainstalovány ve všech vašich pracovních prostorů.

Všimněte si, že se zobrazí pouze řešení společnosti Microsoft nainstalované v aktuálním pracovním prostoru pomocí portálu OMS.

## <a name="removing-a-management-solution"></a>Odebrání řešení pro správu
Pokud je odebrán řešení pro správu, budou odebrány také všechny prostředky v řešení.  

1. Vyhledejte řešení na portálu Azure pomocí postupu v [výpis řešení](#listing-solutions).
2. Vyberte řešení, které chcete odebrat.
3. Klikněte **odstranit** tlačítko.

## <a name="creating-a-management-solution"></a>Vytváření řešení pro správu
Úplné pokyny k vytváření řešení pro správu jsou k dispozici na [vytváření řešení v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md). 

## <a name="next-steps"></a>Další kroky
* Hledání [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates) ukázky různých šablonách Resource Manager.
* Vytvořit vlastní [řešení pro správu](operations-management-suite-solutions-creating.md).

