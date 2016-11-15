---
title: "Správa přístupu ke službě Log Analytics | Dokumentace Microsoftu"
description: "Správa přístupu ke službě Log Analytics pomocí různých úloh správy prováděných s uživateli, účty, pracovními prostory OMS a účty Azure."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/02/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: fbd35f9fdd5165a2dd4bc7dd47bd70c890539e38


---
# <a name="manage-access-to-log-analytics"></a>Správa přístupu ke službě Log Analytics
Pro správu přístupu ke službě Log Analytics provádíte různé úlohy správy s uživateli, účty, pracovními prostory OMS a účty Azure. Při vytváření pracovního prostoru OMS (Operations Management Suite) zvolte název pracovního prostoru, přidružte ho ke svému účtu a zvolte zeměpisné umístění. Pracovní prostor je v podstatě kontejner, který obsahuje informace o účtu a jednoduché konfigurační informace. Vy nebo další členové vaší organizace můžete používat víc pracovních prostorů OMS ke správě různých dat, která se shromažďují ze všech částí vaší infrastruktury IT.

V článku [Začínáme se službou Log Analytics](log-analytics-get-started.md) se můžete podívat, jak všechno rychle nastavit a uvést do provozu. Zbývající část tohoto článku popisuje běžně používané úlohy:

* Určení potřebného počtu pracovních prostorů
* Správa účtů a uživatelů
* Přidání skupiny do existujícího pracovního prostoru
* Připojení existujícího pracovního prostoru k předplatnému Azure
* Upgrade pracovního prostoru na placený datový tarif
* Změna typu datového tarifu
* Přidání organizace Azure Active Directory k existujícímu pracovnímu prostoru
* Odstranění pracovního prostoru OMS

## <a name="determine-the-number-of-workspaces-you-need"></a>Určení potřebného počtu pracovních prostorů
Pracovní prostor je prostředek Azure v podobě kontejneru, ve kterém se data shromažďují, agregují, analyzují a zobrazují v portálu OMS.

Je možné vytvořit víc pracovních prostorů OMS Log Analytics a přidělit uživatelům přístup k jednomu nebo několika z nich. Minimalizace počtu pracovních prostorů umožňuje dotazování a korelaci s většinou dat. Tato část popisuje případy, kdy je užitečné mít víc než jeden pracovní prostor.

V současné době pracovní prostor Log Analytics nabízí:

* Zeměpisné umístění úložiště dat
* Členitost fakturace
* Izolaci dat

Na základě výše uvedených charakteristik můžete vytvořit víc pracovních prostorů, pokud:

* Jste globální společnost a potřebujete ukládat data v různých oblastech z důvodů suverenity dat nebo dodržování předpisů.
* Používáte Azure a chcete se vyhnout poplatkům za odchozí datové přenosy tím, že budete mít pracovní prostor Log Analytics ve stejné oblasti jako prostředky Azure, které spravuje.
* Chcete rozdělit náklady na různá oddělení nebo obchodní skupiny na základě využití. Když vytvoříte pracovní prostor pro každé oddělení nebo obchodní skupinu, vaše vyúčtování Azure a údaje o využití budou uvádět poplatky za každý pracovní prostor zvlášť.
* Jste poskytovatel spravované služby a potřebujete uchovávat analytická data pro každého zákazníka odděleně od dat ostatních zákazníků.
* Spravujete několik zákazníků a chcete, aby se každý zákazník, oddělení nebo pracovní skupina mohli podívat na svá vlastní data, ale ne na data ostatních.

Při používání agentů ke shromažďování dat můžete každého agenta konfigurovat tak, aby ukládal data do požadovaného pracovního prostoru.

Pokud používáte System Center Operations Manager, můžete připojit každou skupinu nástroje Operations Manager jen do jednoho pracovního prostoru. Můžete nainstalovat Microsoft Monitoring Agent do počítačů spravovaných nástrojem Operations Manager a nastavit agenta tak, aby odesílal data do nástroje Operations Manager i do jiného pracovního prostoru Log Analytics.

### <a name="workspace-information"></a>Informace o pracovním prostoru
Na portálu OMS můžete zobrazit informace o pracovním prostoru a určit, zda chcete zasílat informace od společnosti Microsoft.

#### <a name="view-workspace-information"></a>Zobrazení informací o pracovním prostoru
1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty).
3. Klikněte na kartu **Workspace Information** (Informace o pracovním prostoru).  
   ![Informace o pracovním prostoru](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Správa účtů a uživatelů
Každý prostor může mít více uživatelských účtů, které s ním spojená, a každý uživatelský účet (účet Microsoft nebo účtu organizace) mají přístup do více OMS pracovních prostorů.

Ve výchozím se správcem pracovního prostoru stává účet Microsoft nebo účet organizace použitý při vytvoření pracovního prostoru. Správce pak může pozvat další účty Microsoft nebo vybrat uživatele ze služby Azure Active Directory.

Udělování přístupu k pracovnímu prostoru OMS se provádí na dvou místech:

* V Azure můžete použít řízení přístupu na základě rolí k poskytnutí přístupu k předplatnému Azure a přidruženým prostředkům. Tato oprávnění se používají také pro přístup k PowerShellu a REST API.
* V portálu OMS se přiděluje přístup jen k samotnému portálu OMS, nikoli k přidruženému předplatnému Azure.

Pokud uživatelům udělíte přístup jenom k portálu OMS, ale ne k předplatnému Azure, které s ním je spojené, neuvidí data v dlaždicích řešení Backup a Site.
Pokud chcete, aby uživatelé mohli zobrazovat data v těchto řešeních, ujistěte se, že mají alespoň přístup **čtenář** pro účet Automation, trezor Backup a trezor Site Recovery, které jsou propojené s pracovním prostorem OMS.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Správa přístupu ke službě Log Analytics pomocí webu Azure Portal
Pokud uživatelům umožníte přístup k pracovním prostoru protokolu Log Analytics prostřednictvím oprávnění Azure, například na webu Azure Portal, pak tito uživatelé budou mít přístup i k portálu Log Analytics. Pokud jsou uživatelé na webu Azure Portal, můžou přejít na portál OMS kliknutím na úlohu **OMS Portal** při prohlížení prostředku pracovního prostoru Log Analytics.

Ohledně webu Azure Portal je třeba pamatovat na několik věcí:

* Nejde o *řízení přístupu na základě rolí*. Pokud máte na webu Azure Portal přístup *čtenář* pro pracovní prostor Log Analytics, pak můžete provádět změny pomocí portálu OMS. Portál OMS má koncepce pouze pro uživatele Správce, Přispěvatel a Pouze pro čtení. Pokud je účet, ke kterému jste přihlášení, propojený ve službě Azure Active Directory s pracovním prostorem, budete na portálu OMS jeho správcem, jinak budete přispěvatelem.
* Když se přihlásíte k portálu OMS přes stránku http://mms.microsoft.com, ve výchozím nastavení uvidíte seznam **Vybrat pracovní prostor**. Tento seznam obsahuje jen pracovní prostory, které byly přidány pomocí portálu OMS. Když chcete zobrazit pracovní prostory, ke kterým máte přístup s předplatným Azure, bude potřeba zadat tenanta jako část adresy URL. Například:
  
  `mms.microsoft.com/?tenant=contoso.com` Identifikátor tenanta je často poslední částí e-mailové adresy, se kterou se přihlašujete.
* Pokud je účet, se kterým se přihlašujete, v tenantu Azure Active Directory, pak jste *Správce* na portálu OMS. To většinou platí, pokud se nepřihlašujete jako odběratel CSP.  Pokud váš účet není účet tenanta Azure Active Directory, budete *Uživatel* portálu OMS.
* Pokud chcete přejít přímo na portál, ke kterému máte přístup s použitím oprávnění Azure, je třeba zadat prostředek jako část adresy URL. Tuto adresu URL můžete získat pomocí prostředí PowerShell.
  
  Například, `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.
  
  Adresa URL vypadá takto: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>Správa uživatelů v portálu OMS
Uživatele a skupiny můžete spravovat na kartě **Manage Users** (Správa uživatelů) v rámci karty **Accounts** (Účty) na stránce nastavení.   

![Správa uživatelů](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Přidání uživatele do existujícího pracovního prostoru
Pomocí následujícího postupu přidáte k pracovnímu prostoru OMS uživatele nebo skupinu.

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Manage Users** (Správa uživatelů).
3. V oddílu **Manage Users** (Správa uživatelů) vyberte typ účtu, který chcete přidat: **Organizational Account** (Účet organizace), **Microsoft Account** (Účet Microsoft), nebo **Microsoft Support** (Podpora Microsoft).
   
   * Pokud vyberete možnost Microsoft Account, zadejte e-mailovou adresu uživatele účtu Microsoft.
   * Pokud vyberete možnost Organizational Account, můžete zadat část e-mailového aliasu nebo jména uživatele nebo skupiny a v rozevíracím seznamu se zobrazí seznam vyhovujících uživatelů a skupin. Vyberte uživatele nebo skupinu.
   * Prostřednictvím podpory Microsoftu můžete pracovníkovi podpory nebo jinému zaměstnanci Microsoftu poskytnout dočasný přístup k vašemu pracovnímu prostoru kvůli odstraňování potíží.
     
     > [!NOTE]
     > Nejlepšího výkonu dosáhnete, když omezíte počet skupin Active Directory přidružených k jednomu účtu OMS na tři – jednu pro správce, jednu pro přispěvatele a jednu pouze pro čtení. Použití více skupin může ovlivnit výkon služby Log Analytics.
     > 
     > 
4. Vyberte typ uživatele nebo skupiny pro přidání: **Správce**, **Přispěvatel** nebo **Jen pro čtení**.  
5. Klikněte na tlačítko **Přidat**.
   
   Pokud přidáváte účet Microsoft, bude pozvánka k připojení k danému pracovnímu prostoru odeslána na zadaný e-mail. Až uživatel udělá postup v pozvání pro připojení k OMS, bude mít přístup do tohoto pracovního prostoru OMS.
   Pokud přidáváte účet organizace, uživatel bude mít přístup ke službě Log Analysis okamžitě.  
   ![e-mail s pozváním](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### <a name="edit-an-existing-user-type"></a>Úprava typu existujícího uživatele
Můžete změnit roli účtu pro uživatele přidruženého k účtu OMS. Máte následující možnosti rolí:

* *Správce*: může spravovat uživatele, zobrazovat a reagovat na všechny výstrahy a přidávat a odebírat servery.
* *Přispěvatel*: může zobrazovat a reagovat na všechny výstrahy a přidávat a odebírat servery.
* *Jen pro čtení*: takto označení uživatelé nemůžou:
  
  1. Přidávat a odebírat řešení. Galerie řešení je skrytá.
  2. Přidávat, upravovat nebo odebírat dlaždice na řídicím panelu **My Dashboard** (Můj řídicí panel).
  3. Zobrazit stránky **Settings** (Nastavení). Tyto stránky jsou skryté.
  4. V zobrazení hledání jsou skryté konfigurace Power BI, uložená hledání a úlohy výstrah.

#### <a name="to-edit-an-account"></a>Postup úpravy účtu
1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Manage Users** (Správa uživatelů).
3. Vyberte roli pro uživatele, kterého chcete upravit.
4. V potvrzovacím dialogovém okně klikněte na **Yes**.

### <a name="remove-a-user-from-an-oms-workspace"></a>Odebrání uživatele z pracovního prostoru OMS
Pomocí následujícího postupu můžete odebrat uživatele z pracovního prostoru OMS. Odebrání uživatel nezavře pracovní prostor. Místo toho se jen odebere přidružení mezi pracovním prostorem a uživatelem. Pokud je uživateli přidruženo několik pracovních prostorů, tento uživatel se pořád bude moci přihlašovat do OMS a zobrazit své ostatní pracovní prostory.

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Manage Users** (Správa uživatelů).
3. Klikněte na možnost **Remove** (Odebrat) vedle jména uživatele, kterého chcete odebrat.
4. V potvrzovacím dialogovém okně klikněte na **Yes**.

### <a name="add-a-group-to-an-existing-workspace"></a>Přidání skupiny do existujícího pracovního prostoru
1. Postupujte podle kroků 1-4 v části „Přidání uživatele do existujícího pracovního prostoru“ nahoře.
2. V části **Choose User/Group** (Vyberte uživatele/skupinu) vyberte **Group** (Skupina).
   ![Přidání skupiny do existujícího pracovního prostoru](./media/log-analytics-manage-access/add-group.png)
3. Zadejte zobrazovaný název nebo e-mailovou adresu pro skupinu, kterou chcete přidat.
4. Vyberte skupinu ze seznamu výsledků a klikněte na **Add** (Přidat).

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Připojení existujícího pracovního prostoru k předplatnému Azure
Všechny pracovní prostory vytvořené po 26. září 2016 musí být propojené s předplatným Azure v okamžiku vytvoření. Pracovní prostory vytvořené před tímto datem je potřeba propojit s pracovním prostorem při vašem dalším přihlášení. 

> [!IMPORTANT]
> K propojení s pracovním prostorem musí váš účet Azure už mít k tomuto pracovnímu prostoru přístup.  Jinými slovy – účet, který používáte pro přístup k webu Azure Portal, musí být **stejný** jako účet, který používáte pro přístup k pracovnímu prostoru OMS. Pokud ne, přečtěte si část [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).
> 
> 

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Propojení pracovního prostoru s předplatným Azure v portálu OMS
Pokud chcete propojit pracovní prostor s předplatným Azure v portálu OMS, přihlášený uživatel už musí mít placený účet Azure.

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Azure Subscription & Data Plan** (Předplatné a datový tarif Azure).
3. Klikněte na datový tarif, který chcete použít.
4. Klikněte na **Uložit**.  
   ![Předplatné a datové tarify](./media/log-analytics-manage-access/subscription-tab.png)

Váš nový datový tarif se zobrazí pásu karet portálu OMS v horní části webové stránky.

![Pás karet OMS](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Propojení pracovního prostoru s předplatným Azure na webu Azure Portal
1. Přihlaste se k [Azure Portal](http://portal.azure.com).
2. Přejděte k položce **Log Analytics (OMS)** a vyberte ji.
3. Uvidíte svůj seznam existujících pracovních prostorů. Klikněte na tlačítko **Přidat**.  
   ![seznam pracovních prostorů](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. V oddílu **OMS Workspace** klikněte na možnost **Or link existing** (Nebo propojit existující).  
   ![propojení existujícího](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Klikněte na **Configure required settings** (Konfigurovat požadovaná nastavení).  
   ![konfigurace požadovaných nastavení](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Uvidíte seznam pracovních prostorů, které ještě nejsou propojené s vaším účtem Azure. Vyberte pracovní prostor.  
   ![Výběr pracovního prostoru](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Je-li to třeba, můžete změnit hodnoty následujících položek:
   * Předplatné
   * Skupina prostředků
   * Umístění
   * Cenová úroveň  
     ![Změna hodnot](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Klikněte na možnost **Vytvořit**. Pracovní prostor je teď propojený s vaším účtem Azure.

> [!NOTE]
> Pokud nevidíte pracovní prostor, s kterým chcete účet propojit, znamená to, že vaše předplatné Azure nemá přístup k pracovnímu prostoru OMS, který jste vytvořili na webu OMS.  Přístup k tomuto účtu budete muset udělit z portálu OMS. Postup najdete v části [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).
> 
> 

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Upgrade pracovního prostoru na placený tarif
Ve službě OMS existují tři tarify pracovního prostoru: **Free**, **Standalone** a **Premium**.  Pokud používáte tarif *Free*, existuje limit 500 MB pro odesílání dat do služby Log Analytics.  Pokud toto množství překročíte, budete muset přejít na placený tarif, aby se vám neshromažďovala data nad tento limit. Svůj tarif můžete kdykoli změnit.  Informace o cenách OMS najdete na stránce [podrobností o cenách](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Používání nároků z předplatného OMS
Pokud chcete používat nároky z nákupu OMS E1, OMS E2 OMS nebo doplňku OMS pro System Center, zvolte tarif *OMS* služby OMS Log Analytics.

Když si koupíte předplatné OMS, nároky se přidají do vaší smlouvy Enterprise. Jakékoli předplatné vytvořené v rámci této smlouvy může tento nárok uplatnit. Díky tomu můžete například mít několik pracovních prostorů OMS využívajících nárok vyplývající z předplatných OMS.

Pokud se chcete ujistit, že váš pracovní prostor OMS využívá nárok plynoucí z předplatného OMS, proveďte následující:

1. Vytvořte pracovní prostor OMS v předplatném Azure, které je součástí smlouvy Enterprise obsahující předplatné Azure.
2. Vyberte pro pracovní prostor tarif *OMS*.

> [!NOTE]
> Pokud jste pracovní prostor vytvořili před 26. zářím 2016 a cenový tarif služby Log Analytics je *Premium*, bude tento pracovní prostor používat nároky z doplňku OMS pro System Center. Své nároky můžete využít také tak, že přejdete na cenovou úroveň *OMS*. 
> 
> 

Nároky na předplatné OMS nejsou viditelné na Azure nebo na portálu OMS. Tento nárok uvidíte jen na webu Enterprise Portal.  

Pokud potřebujete změnit předplatné Azure, se kterým je pracovní prostor OMS propojený, můžete použít rutinu prostředí Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx).

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Využití závazků Azure ze smlouvy Enterprise
Pokud nemáte předplatné OMS, platíte za každou součást OMS zvlášť a využití se zobrazí na faktuře Azure.

Pokud jsou vaše předplatná Azure propojena se smlouvou Enterprise s finančním závazkem, jakékoli využití služby Log Analytics se bude automaticky strhávat ze zbývajícího finančního závazku.

Pokud potřebujete změnit předplatné Azure, se kterým je pracovní prostor OMS propojený, můžete použít rutinu prostředí Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx).  

### <a name="change-a-workspace-to-a-paid-data-plan"></a>Změna pracovního prostoru na placený datový tarif
1. Přihlaste se k [Azure Portal](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Vyberte pracovní prostor.  
   ![Seznam pracovních prostorů](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4. V oblasti **Settings** (Nastavení) klikněte na možnost **Pricing tier** (Cenová úroveň).  
   ![cenová úroveň](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5. V oblasti **Pricing tier** (Cenová úroveň) vyberte datový tarif a klikněte na možnost **Select** (Vybrat).  
   ![výběr tarifu](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Po aktualizaci zobrazení na webu Azure Portal uvidíte položku **Pricing tier** (Cenová úroveň) aktualizovanou na vybraný tarif.  
   ![aktualizace cenové úrovně](./media/log-analytics-manage-access/manage-access-change-plan04.png)

## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Přidání organizace Azure Active Directory k existujícímu pracovnímu prostoru
Pracovní prostor Log Analytics můžete přidružit k doméně Azure Active Directory a přidat uživatele ze svého adresáře do pracovního prostoru OMS.

Když vytváříte pracovní prostor z webu Azure Portal nebo propojujete pracovní prostor s předplatným Azure, služba Azure Active Directory se propojí jako účet vaší organizace.

Když vytváříte pracovní prostor z portálu OMS, zobrazí se výzva k propojení předplatného Azure a účet organizace pro předplatné Azure se přidruží k pracovnímu prostoru Log Analytics.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Přidání organizace Azure Active Directory k existujícímu pracovnímu prostoru
Přidání organizace Azure Active Directory vám umožní přidávat uživatele a skupiny z tohoto adresáře do pracovního prostoru.

1. Na stránce Nastavení na portálu OMS klikněte na kartu **Accounts** (Účty) a pak na kartu **Manage Users** (Spravovat uživatele).  
2. Zkontrolujte informace o účtech organizace a klikněte na možnost **Add Organization** (Přidat organizaci).  
    ![přidání organizace](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Zadejte informace o identitě správce vaší domény Azure Active Directory. Následně se zobrazí potvrzení o tom, že je váš pracovní prostor propojený s doménou Azure Active Directory.
    ![potvrzení o propojení pracovního prostoru](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

Pokud potřebujete přidat uživatele z jiného adresáře, klikněte na tlačítko *Změnit organizaci*, aby se pracovní prostor přidružil k jinému adresáři.

## <a name="delete-your-log-analytics-workspace"></a>Odstranění pracovního prostoru Log Analytics
Když odstraníte pracovní prostor Log Analytics, odstraní se všechna data související s vaším pracovním prostorem ze služby OMS během 30 dní.

Pokud jste správce a k pracovnímu prostoru bylo přidruženo víc uživatelů, přidružení těchto uživatelů s pracovním prostorem se přeruší. Pokud byli tito uživatelé přidruženi s jinými pracovními prostory, můžou pokračovat v používání služby OMS s těmito prostory. Pokud ale s jinými pracovními prostory přidružení nejsou, budou muset pro další používání služby OMS vytvořit pracovní prostor.

### <a name="to-delete-an-oms-workspace"></a>Postup odstranění pracovního prostoru OMS
1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a pak na kartu **Workspace Information** (Informace o pracovním prostoru).
3. Klikněte na možnost **Close Workspace** (Zavřít pracovní prostor).
4. Vyberte jeden z přednastavených důvodů zavření pracovního prostoru nebo zadejte jiný důvod do textového pole.
5. Klikněte na **Close workspace** (Zavřít pracovní prostor).

## <a name="next-steps"></a>Další kroky
* Postup přidání agentů a shromažďování dat obsahuje článek [Propojení počítačů s Windows se službou Log Analytics](log-analytics-windows-agents.md).
* Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
* Pokud vaše organizace používá proxy server nebo bránu firewall, postupujte podle článku [Konfigurace nastavení proxy serveru a brány firewall ve službě Log Analytics](log-analytics-proxy-firewall.md), aby agenti mohli se službou Log Analytics komunikovat.




<!--HONumber=Nov16_HO2-->


