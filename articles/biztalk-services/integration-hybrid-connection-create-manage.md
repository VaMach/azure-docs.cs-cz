---
title: "Vytvoření a správa hybridních připojení | Microsoft Docs"
description: "Naučte se vytvořit hybridní připojení, Správa připojení a instalace správce hybridního připojení. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: erikre
editor: 
ms.assetid: aac0546b-3bae-41e0-b874-583491a395ea
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: ccompy
ms.openlocfilehash: 7b8b9072d0e2fd054ca07873c0a9ce772dc2941e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-hybrid-connections"></a>Vytvoření a správa hybridních připojení

> [!IMPORTANT]
> Hybridní připojení BizTalk jsou vyřazena z provozu a nahrazena hybridními připojeními App Service. Další informace, včetně informací o tom, jak spravovat existující hybridní připojení BizTalk, najdete v tématu [Hybridní připojení Azure App Service](../app-service/app-service-hybrid-connections.md).


## <a name="overview-of-the-steps"></a>Přehled kroků
1. Vytvořit hybridní připojení tak, že zadáte **název hostitele** nebo **plně kvalifikovaný název domény** místní prostředku v privátní síti.
2. Webové aplikace Azure nebo Azure mobilní aplikace propojte hybridní připojení.
3. Nainstalovat správce hybridního připojení na vaše místní prostředek a připojte se k určité hybridní připojení. Portál Azure poskytuje možnosti jedním kliknutím, nainstalovat a připojit.
4. Spravovat hybridní připojení a jejich klíče připojení.

Toto téma obsahuje tyto kroky. 

> [!IMPORTANT]
> Je možné nastavit koncový bod hybridní připojení k IP adrese. Pokud chcete použít IP adresu, může nebo nemusí dosáhnout místnímu prostředku, v závislosti na vašeho klienta. Hybridního připojení závisí na klientovi provádění vyhledávání DNS. Ve většině případů **klienta** je kód aplikace. Pokud klient nebude provádět vyhledávání DNS (nepokouší pro překlad adres IP, jako by to byl platný název domény (x.x.x.x)), pak provoz nejsou odesílány prostřednictvím daného hybridního připojení.
> 
> Pro příklad (pseudokódu), můžete definovat **10.4.5.6** jako místního hostitele:
> 
> **V následujícím scénáři funguje:**  
> `Application code -> GetHostByName("10.4.5.6") -> Resolves to 127.0.0.3 -> Connect("127.0.0.3") -> Hybrid Connection -> on-prem host`
> 
> **V následujícím scénáři nefunguje:**  
> `Application code -> Connect("10.4.5.6") -> ?? -> No route to host`
> 
> 

## <a name="CreateHybridConnection"></a>Vytvořit hybridní připojení.
Hybridní připojení lze vytvořit na portálu Azure pomocí webových aplikací **nebo** pomocí služby BizTalk Services. 

<!-- **To create Hybrid Connections using Web Apps**, see [Connect Azure Web Apps to an On-Premises Resource](../app-service-web/web-sites-hybrid-connection-get-started.md). You can also install the Hybrid Connection Manager (HCM) from your web app, which is the preferred method.  -->

**Chcete-li vytvořit hybridní připojení ve službě BizTalk Services**:

1. Přihlaste se do [portál Azure Classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně, vyberte **BizTalk Services** a potom vyberte svoji službu BizTalk. 
   
    Pokud nemáte existující službu BizTalk, můžete [vytvoření služby BizTalk](biztalk-provision-services.md).
3. Vyberte **hybridní připojení** karty:  
   ![Karta hybridní připojení][HybridConnectionTab]
4. Vyberte **vytvořit hybridní připojení** nebo vyberte **přidat** tlačítko na hlavním panelu. Zadejte následující příkaz:
   
   | Vlastnost | Popis |
   | --- | --- |
   | Name (Název) |Hybridní připojení. název musí být jedinečné a nesmí být stejný název jako službu BizTalk. Můžete zadat libovolný název, ale buďte konkrétní s jeho účel. Příklady obsahují:<br/><br/>Mzdy*SQLServer*<br/>SupplyList*SharepointServer*<br/>Zákazníci*OracleServer* |
   | Název hostitele |Zadejte název hostitele plně kvalifikovaný název hostitele nebo adresu IPv4 místnímu prostředku. Příklady obsahují:<br/><br/>Můjsqlserver<br/>*Můjsqlserver*. *Domény*. corp.*společnost*.com<br/>*myHTTPSharePointServer*<br/>*myHTTPSharePointServer*. *Společnost*.com<br/>10.100.10.10<br/><br/>Pokud chcete použít adresu IPv4, Všimněte si, že kódu klienta nebo aplikace nemusí přeložit IP adresu. Viz důležitá poznámka na začátku tohoto tématu. |
   | Port |Zadejte číslo portu místnímu prostředku. Například pokud používáte webové aplikace, zadejte port 80 nebo 443. Pokud používáte systém SQL Server, zadejte port 1433. |
5. Výběrem symbolu zaškrtnutí dokončete nastavení. 

#### <a name="additional"></a>Další
* Můžete vytvořit více hybridní připojení. Najdete v článku [BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md) počet povolených připojení. 
* Každé hybridní připojení se vytvoří dvojice připojovacích řetězců: aplikace klíče tohoto SEND a místní klíče, které NASLOUCHAT. Každý pár má primární a sekundární klíč. 

## <a name="LinkWebSite"></a>Propojte mobilní aplikace nebo webové aplikace Azure App Service
Chcete-li odkaz na webovou aplikaci nebo mobilní aplikace v Azure App Service na stávající hybridní připojení, vyberte **použít stávající hybridní připojení** v okně hybridní připojení. 
<!-- See [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md). -->

## <a name="InstallHCM"></a>Instalace správce hybridního připojení na místě
Po vytvoření hybridní připojení, nainstalujte na prostředek místní správce hybridního připojení. Ho můžete stáhnout z vaší webové aplikace Azure nebo z vaší služby BizTalk. BizTalk Services kroky: 

1. Přihlaste se do [portál Azure Classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně, vyberte **BizTalk Services** a potom vyberte svoji službu BizTalk. 
3. Vyberte **hybridní připojení** karty:  
   ![Karta hybridní připojení][HybridConnectionTab]
4. Na hlavním panelu vyberte **místní instalace**:  
   ![Místní instalace][HCOnPremSetup]
5. Vyberte **instalace a konfigurace** spustit nebo stáhnout správce hybridního připojení na místní systém. 
6. Výběrem symbolu zaškrtnutí zahájíte instalaci. 

<!--
You can also download the Hybrid Connection Manager MSI file and copy the file to your on-premises resource. Specific steps:

1. Copy the on-premises primary Connection String. See [Manage Hybrid Connections](#ManageHybridConnection) in this topic for the specific steps.
2. Download the Hybrid Connection Manager MSI file. 
3. On the on-premises resource, install the Hybrid Connection Manager from the MSI file. 
4. Using Windows PowerShell, type: 
> Add-HybridConnection -ConnectionString “*Your On-Premises Connection String that you copied*” 
--> 

#### <a name="additional"></a>Další
* Správce hybridního připojení lze nainstalovat na následující operační systémy:
  
  * Windows Server 2008 R2 (rozhraní .NET Framework 4.5 + a Windows Management Framework 4.0 + požadované)
  * Windows Server 2012 (Windows Management Framework 4.0 + požadované)
  * Windows Server 2012 R2
* Když nainstalujete správce hybridního připojení, dojde k následující položky: 
  
  * Hybridní připojení hostované v Azure je automaticky konfigurují pro použití primární připojovací řetězec aplikace. 
  * Místní prostředek je automaticky konfigurují pro použití primární připojovací řetězec místní.
* Správce hybridního připojení musí používat platný místní připojovací řetězec pro ověřování. Azure Web Apps nebo Mobile Apps musí platnou aplikaci připojovací řetězec pro autorizaci použít.
* Hybridní připojení je možné škálovat nainstalováním jiná instance správce hybridního připojení na jiném serveru. Nakonfigurujte místní naslouchací proces používat stejnou adresu jako první místní naslouchací proces. V takovém případě je provoz náhodně distribuované (kruhové dotazování) mezi active místní naslouchací procesy. 

## <a name="ManageHybridConnection"></a>Správa hybridních připojení
Chcete-li spravovat hybridní připojení, můžete:

* Pomocí portálu Azure, přejděte na svoji službu BizTalk. 
* Použití [rozhraní REST API](http://msdn.microsoft.com/library/azure/dn232347.aspx).

#### <a name="copyregenerate-the-hybrid-connection-strings"></a>Kopírování nebo znovu vygenerovat hybridní připojovací řetězce
1. Přihlaste se do [portál Azure Classic](http://go.microsoft.com/fwlink/p/?LinkID=213885).
2. V levém navigačním podokně, vyberte **BizTalk Services** a potom vyberte svoji službu BizTalk. 
3. Vyberte **hybridní připojení** karty:  
   ![Karta hybridní připojení][HybridConnectionTab]
4. Vyberte hybridní připojení. Na hlavním panelu vyberte **spravovat připojení**:  
   ![Spravovat možnosti][HCManageConnection]
   
    **Správa připojení** obsahuje připojovací řetězce aplikace a místně. Můžete kopírovat připojovací řetězce nebo znovu vygenerovat přístupový klíč použít v připojovacím řetězci. 
   
    **Pokud vyberete znovu vygenerovat**, se změní sdílený přístupový klíč použít v připojovacím řetězci. Udělejte toto:
   
   * Na portálu Azure classic, vyberte **synchronizace klíčů** v aplikaci Azure.
   * Znovu spustit **místní instalace**. Když znovu spustíte instalační program On-Premises, místnímu prostředku automaticky konfigurují pro použití aktualizované primární připojovací řetězec.

#### <a name="use-group-policy-to-control-the-on-premises-resources-used-by-a-hybrid-connection"></a>Použití zásad skupiny k řízení místní prostředky využívané třídou hybridní připojení
1. Stažení [šablony pro správu Správce hybridního připojení](http://www.microsoft.com/download/details.aspx?id=42963).
2. Extrahujte soubory.
3. Na počítači, který upravuje zásad skupiny postupujte takto:  
   
   * Kopírování. ADMX souborů do *%WINROOT%\PolicyDefinitions* složky.
   * Kopírování. ADML souborů do *%WINROOT%\PolicyDefinitions\en-us* složky.

Po zkopírování, můžete změnit zásady Editor zásad skupiny.

## <a name="next"></a>Další
[Přehled hybridních připojení](integration-hybrid-connection-overview.md)

## <a name="see-also"></a>Viz také
[REST API pro správu služby BizTalk Services v Microsoft Azure](http://msdn.microsoft.com/library/azure/dn232347.aspx)  
[BizTalk Services: Tabulka edic](biztalk-editions-feature-chart.md)  
[Vytvoření služby BizTalk pomocí portálu Azure classic](biztalk-provision-services.md)  
[BizTalk Services: Karty Řídicí panel, Sledování a Škálování](biztalk-dashboard-monitor-scale-tabs.md)

[HybridConnectionTab]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionTab.png
[HCOnPremSetup]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionOnPremSetup.png
[HCManageConnection]: ./media/integration-hybrid-connection-create-manage/WABS_HybridConnectionManageConn.png 
