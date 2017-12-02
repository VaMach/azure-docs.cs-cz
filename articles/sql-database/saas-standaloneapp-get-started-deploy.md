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
ms.date: 11/30/2017
ms.author: genemi
ms.openlocfilehash: d38cd108821bce05824732bbdbdd322ae8563bde
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-and-explore-a-standalone-single-tenant-application-that-uses-azure-sql-database"></a>Nasazení a prozkoumejte samostatná jednoho klienta aplikace, která používá databázi SQL Azure

V tomto kurzu nasazení a prozkoumejte Wingtip lístky SaaS samostatné aplikace. Aplikace je navržená ke prezentují funkce databáze SQL Azure, které zjednodušují povolení SaaS scénáře.

Vzor samostatné aplikace nasadí skupinu prostředků Azure obsahující jednoho klienta aplikace a databáze klienta jednou pro každého klienta.  K zajištění víceklientské řešení se dá zřídit několik instancí aplikace.

V tomto kurzu nasadíte skupiny prostředků pro několik klientů do vašeho předplatného Azure.  Tento vzor umožňuje skupiny prostředků k nasazení do předplatného Azure klienta. Azure má partnera programy, které umožňují tyto skupiny prostředků, který bude spravován poskytovatele služeb jménem klienta. Poskytovatel služeb je správce v rámci předplatného klienta.

V části novější nasazení existují tři blue **nasadit do Azure** tlačítka. Každé tlačítko nasadí jinou instanci aplikace. Každá instance je přizpůsobené pro konkrétní klienta. Při stisknutí každé tlačítko odpovídající aplikace je nasazená plně do pěti minut.  Aplikace se nasadí ve vašem předplatném Azure.  Máte úplný přístup k prozkoumávání a pracovat jednotlivé součásti aplikace.

Zdrojový kód a správu skripty aplikace jsou k dispozici v [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp) úložiště GitHub.


V tomto kurzu se dozvíte:

> [!div class="checklist"]
> * Postup nasazení Wingtip lístky SaaS samostatné aplikace.
> * Kde lze získat zdrojovému kódu aplikace a skripty pro správu.
> * O servery a databáze, které tvoří aplikace.

Další kurzy, budou vydané. Se vám umožní zkoumat celou řadu scénářů správy na základě vzoru pro tuto aplikaci.   

## <a name="deploy-the-wingtip-tickets-saas-standalone-application"></a>Nasazení aplikace SaaS samostatné Wingtip lístky

Nasazení aplikace pro tři zadaný klienty:

1. Klikněte na každou blue **nasadit do Azure** tlačítko otevřete šablonu nasazení v [portál Azure](https://portal.azure.com). Každá šablona vyžaduje dvě hodnoty parametrů; název pro novou skupinu prostředků a uživatelské jméno, která odlišuje od jiných nasazení aplikace pro toto nasazení. Dalším krokem poskytuje podrobnosti pro tyto hodnoty nastavení.<br><br>
    <a href="http://aka.ms/deploywingtipsa-contoso" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Hall vzájemné součinnosti Contoso**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-dogwood" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Svída Dojo**
<br><br>
    <a href="http://aka.ms/deploywingtipsa-fabrikam" target="_blank"><img style="vertical-align:middle" src="media/saas-standaloneapp-get-started-deploy/deploy.png"/></a>&nbsp; **Křížovou Fabrikam Jazz kartou**

2. Zadejte požadovaný parametr hodnoty pro každé nasazení.

    > [!IMPORTANT]
    > Některé ověřování a server brány firewall jsou záměrně zabezpečená pro demonstrační účely. **Vytvořit novou skupinu prostředků** pro každé nasazení aplikace.  Nepoužívejte existující skupinu prostředků. Tato aplikace nebo všechny prostředky, který vytváří, nepoužívejte pro produkční prostředí. Až skončíte s aplikacemi zastavit související fakturace, odstraňte všechny skupiny prostředků.

    Je nejvhodnější použít jenom malá písmena, číslice a pomlčky v názvy prostředků.
    * Pro **skupiny prostředků** – vyberte **vytvořit nový**a pak zadejte jedno malé písmeno **název** pro skupinu prostředků.
        * Doporučujeme přidat pomlčkou, za nímž následuje vašimi iniciálami, za nímž následuje číslice: například *wingtip-sa-af1*.
        * Vyberte **umístění** z rozevíracího seznamu.

    * Pro **uživatele** -doporučujeme, abyste zvolili hodnota krátké uživatele, jako je například vašimi iniciálami plus číslice: například *af1*.


3. **Nasaďte aplikaci**.

    * Kliknutím na tlačítko přijmout podmínky a ujednání.
    * Klikněte na **Koupit**.

4. Monitorování stavu nasazení všech tří nasazení kliknutím **oznámení** (ikonu zvonku napravo od pole hledání). Nasazení aplikace trvá pět minut.


## <a name="run-the-application"></a>Spuštění aplikace

Aplikace umožňující prezentovat místa, které jsou hostiteli události. Typy místo obsahují vzájemné součinnosti místnosti, jazz kluby a sportu kluby. Místa jsou zákazníci Wingtip lístky aplikace. V Wingtip lístky místa jsou zapsány jako *klienty*. Probíhá klienta dává místo snadný způsob seznam událostí a prodej lístků zákazníkům. Každý místo získá přizpůsobené webu seznamu svoje události a prodej lístků. Každý klient je izolován od ostatních klientů a je nezávislý z nich. V pozadí každý klient získá instanci samostatné aplikace s vlastní samostatná databáze SQL.

1. Otevřete stránku události pro všechny tři klienty v záložkách samostatné prohlížeče:

    - http://events.contosoconcerthall. &lt;Uživatele&gt;. trafficmanager.net
    - http://events.dogwooddojo. &lt;Uživatele&gt;. trafficmanager.net
    - http://events.fabrikamjazzclub. &lt;Uživatele&gt;. trafficmanager.net

    (V každou adresu URL, nahraďte &lt;uživatele&gt; s hodnotou uživatele vaše nasazení.)

   ![Události](./media/saas-standaloneapp-get-started-deploy/fabrikam.png)

K řízení distribuce příchozí požadavky a používá aplikace [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Každá instance konkrétního klienta aplikace obsahuje název klienta jako součást názvu domény v adrese URL. Všechny klienta adresy URL zahrnují konkrétní **uživatele** hodnotu. Adresy URL použijte následující formát:
- http://events. &lt;venuename&gt;.&lt; Uživatel&gt;. trafficmanager.net

Každý klient databáze **umístění** je zahrnuta v nastavení aplikace odpovídající nasazené aplikace.

V produkčním prostředí, obvykle můžete vytvořit záznam CNAME DNS do [ *nasměrování internetové domény společnosti* ](../traffic-manager/traffic-manager-point-internet-domain.md) na adresu URL profil správce provozu.


## <a name="explore-the-servers-and-tenant-databases"></a>Prozkoumejte serverů a klientů databáze

Podívejme se na některé prostředky, které byly nasazeny:

1. V [portál Azure](http://portal.azure.com), přejděte do seznamu skupin prostředků.
2. Najdete v článku **wingtip-sa-katalogu -&lt;uživatele&gt;**  skupinu prostředků.
    - V této skupině prostředků **katalogu-sa -&lt;uživatele&gt;**  server nasazen. Na serveru **tenantcatalog** databáze.
    - Měli byste taky vidět tři klienta skupiny prostředků.
3. Otevřete **wingtip-sa-fabrikam -&lt;uživatele&gt;**  skupinu prostředků, která obsahuje prostředky pro nasazení křížovou kartou Jazz Fabrikam.  **Fabrikamjazzclub -&lt;uživatele&gt;**  server obsahuje **fabrikamjazzclub** databáze.

Každá databáze klienta je 50 DTU *samostatné* databáze.

## <a name="additional-resources"></a>Další zdroje

<!--
* Additional [tutorials that build on the Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* To learn about elastic pools, see [*What is an Azure SQL elastic pool*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* To learn about elastic jobs, see [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
-->

- Další informace o víceklientské aplikace SaaS najdete v tématu [vzory pro víceklientské aplikace SaaS návrhu](saas-tenancy-app-design-patterns.md).


## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli:

> [!div class="checklist"]
> * Postup nasazení Wingtip lístky SaaS samostatné aplikace.
> * O servery a databáze, které tvoří aplikace.
> * Postup odstranění ukázkové prostředky zastavit související fakturace.

