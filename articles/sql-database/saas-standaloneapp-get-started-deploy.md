---
title: "Kurz SaaS víceklientské – Azure SQL Database | Microsoft Docs"
description: "Nasazení a prozkoumejte samostatná aplikace SaaS jednoho klienta, která používá databázi SQL Azure."
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: sstein
ms.openlocfilehash: 2ca290dfcb23215ffd727500e76076ae8b14fa6f
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Nasazení a prozkoumejte samostatná jednoho klienta aplikace, která používá databázi SQL Azure

V tomto kurzu nasazení a prozkoumejte Wingtip lístky SaaS samostatné aplikace. Aplikace je navržená ke prezentují funkce databáze SQL Azure, které zjednodušují povolení SaaS scénáře.

Vzor samostatné aplikace nasadí skupinu prostředků Azure obsahující jednoho klienta aplikace a databáze klienta jednou pro každého klienta.  K zajištění víceklientské řešení se dá zřídit několik instancí aplikace.

Když v tomto kurzu nasadíte skupiny prostředků pro několik klientů do vašeho předplatného Azure, umožňuje tento vzor skupiny prostředků k nasazení do předplatného Azure klienta.  Azure má partnera programy, které umožňují tyto skupiny prostředků ke správě poskytovatelem služby jménem klienta jako správce předplatného klienta.

V níže uvedené části nasazení jsou tři nasadit do Azure tlačítka, z nichž každý nasadí jiné instance aplikace přizpůsobit pro konkrétní klienta. Při stisknutí tlačítka každé nasazení odpovídající aplikace plně pět minut později.  Aplikace se nasadí ve vašem předplatném Azure.  Máte úplný přístup k prozkoumávání a pracovat jednotlivé součásti aplikace.

Zdrojový kód a správu skripty aplikace jsou k dispozici v [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) úložiště GitHub.


V tomto kurzu se dozvíte:

> [!div class="checklist"]

> * Postup nasazení aplikace samostatné Wingtip lístky SaaS
> * Kde lze získat zdrojovému kódu aplikace a skripty pro správu
> * O serverů a databází, které tvoří aplikace

Další kurzy, budou vydané v přiměřené době který vám umožní zkoumat celou řadu scénářů správy na základě vzoru pro tuto aplikaci.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Nasazení aplikace SaaS samostatné Wingtip lístky

Nasazení aplikace pro tři zadaný klienty:

1. Klikněte na každou **nasadit do Azure** tlačítko otevřete šablonu nasazení na portálu Azure. Každá šablona vyžaduje dvě hodnoty parametrů; název pro novou skupinu prostředků a uživatelské jméno, která odlišuje od jiných nasazení aplikace pro toto nasazení. Dalším krokem poskytuje podrobnosti pro tyto hodnoty nastavení.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>     &nbsp;&nbsp;**Vzájemné součinnosti Hall contoso**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; &nbsp; **Svída Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>    &nbsp;&nbsp;**Společnost Fabrikam Jazz křížovou kartou**

2. Zadejte požadovaný parametr hodnoty pro každé nasazení.

    > [!IMPORTANT]
    > Některá ověřování a brány firewall serveru jsou pro demonstrační účely záměrně nezabezpečené. **Vytvořit novou skupinu prostředků** pro každé nasazení aplikace.  Nepoužívejte existující skupinu prostředků. Tato aplikace nebo všechny prostředky, který vytváří, nepoužívejte pro produkční prostředí. Až skončíte s aplikacemi zastavit související fakturace, odstraňte všechny skupiny prostředků.

    Je nejvhodnější použít jenom malá písmena, číslice a pomlčky v názvy prostředků.
    * Pro **skupiny prostředků** – vyberte Vytvořit nový a potom zadejte název pro skupinu prostředků (malá a velká písmena).
        * Doporučujeme vám, že všechna písmena ve vašem název skupiny prostředků být malými písmeny.
        * Doporučujeme přidat pomlčkou, za nímž následuje vašimi iniciálami, za nímž následuje číslice: například _wingtip-sa-af1_.
        * V rozevíracím seznamu vyberte umístění.

    * Pro **uživatele** -doporučujeme, abyste zvolili krátké hodnoty uživatele, jako je například vašimi iniciálami plus číslice: například _af1_.


1. **Nasaďte aplikaci**.

    * Kliknutím na tlačítko přijmout podmínky a ujednání.
    * Klikněte na **Koupit**.

1. Monitorování stavu nasazení všech tří nasazení kliknutím **oznámení** (ikonu zvonku napravo od pole hledání). Nasazení aplikace trvá přibližně pět minut.


## <a name="run-the-application"></a>Spuštění aplikace

Aplikace umožňující prezentovat místa, například vzájemné součinnosti místnosti, jazz kluby a kluby sportu, který hostitelem události. Místa zaregistrovat jako zákazníky (nebo klienty) Wingtip lístky pro snadný způsob, jak zobrazit seznam událostí a prodej lístků. Každý místo získá přizpůsobené webu ke správě a seznamu svoje události a prodej lístků, nezávislé a izolované od ostatních klientů. V pozadí každý klient získá samostatná databáze SQL a instanci samostatné aplikace.

1. Otevřete stránku události pro všechny tři klienty v záložkách samostatné prohlížeče:

    http://events.contosoconcerthall. &lt;Uživatele&gt;. trafficmanager.net <br>
    http://events.dogwooddojo. &lt;Uživatele&gt;. trafficmanager.net<br>
    http://events.fabrikamjazzclub. &lt;Uživatele&gt;. trafficmanager.net

    (Nahraďte &lt;uživatele&gt; s hodnotou uživatele vašeho nasazení).

   ![Události](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)


K řízení distribuce příchozí požadavky a používá aplikace [ *Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). Každá aplikace konkrétního klienta zahrnuje název klienta jako součást názvu domény v adrese URL. Všechny klienta adresy URL zahrnují konkrétní *uživatele* hodnotu a použijte tento formát: http://events.&lt; venuename&gt;.&lt; Uživatel&gt;. trafficmanager.net. Umístění databáze každého klienta je obsažena v nastavení aplikace odpovídající nasazené aplikace.

V produkčním prostředí, obvykle vytvořili byste záznam CNAME DNS do [ *nasměrování internetové domény společnosti* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) na adresu URL profil správce provozu.


## <a name="explore-the-servers-and-tenant-databases"></a>Prozkoumejte serverů a klientů databáze

Podívejme se na některé prostředky, které byly nasazeny:

1. V [portál Azure](http://portal.azure.com), přejděte do seznamu skupin prostředků.  Byste měli vidět **wingtip-sa-katalogu -&lt;uživatele&gt;**  skupinu prostředků, ve které **katalogu-sa -&lt;uživatele&gt;**  server nasazen se **tenantcatalog** databáze. Měli byste taky vidět tři klienta skupiny prostředků.

1. Otevřete **wingtip-sa-fabrikam -&lt;uživatele&gt;**  skupinu prostředků, která obsahuje prostředky pro nasazení křížovou kartou Jazz Fabrikam.  **Fabrikamjazzclub -&lt;uživatele&gt;**  server obsahuje **fabrikamjazzclub** databáze.


Každá databáze klienta je 50 DTU _samostatné_ databáze.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli:

> [!div class="checklist"]

> * Postup nasazení aplikace samostatné Wingtip lístky SaaS
> * O serverů a databází, které tvoří aplikace
> * Jak odstranit ukázkové prostředky k zastavení souvisejícího účtování


## <a name="additional-resources"></a>Další zdroje

<!--* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) -->
* Další informace o SaaS aplikacích s více tenanty najdete v tématu [*Vzory návrhu pro SaaS aplikace s více tenanty*](saas-tenancy-app-design-patterns.md).
