---
title: "Správa pracovních prostorů | Dokumentace Microsoftu"
description: "Správa pracovních prostorů v Azure Log Analytics pomocí různých úloh správy prováděných s uživateli, účty, pracovními prostory a účty Azure."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/17/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 9ee8f4aafcc35e43c4fcba5a3a72b043dd9fc32c
ms.openlocfilehash: 1d330362389ee690dc2942c9bb1bc32e1f10e08a
ms.lasthandoff: 02/21/2017


---
# <a name="manage-workspaces"></a>Správa pracovních prostorů

Pro správu přístupu k Log Analytics provádíte různé úlohy správy související s pracovními prostory. Tento článek obsahuje postupy a rady založené na osvědčených postupech pro správu pracovních prostorů. Pracovní prostor je v podstatě kontejner, který obsahuje informace o účtu a jednoduché konfigurační informace. Vy nebo další členové vaší organizace můžete používat víc pracovních prostorů ke správě různých sad dat, která se shromažďují ze všech částí vaší infrastruktury IT.

K vytvoření pracovního prostoru budete muset:

1. Mít předplatné Azure.
2. Zvolit název pracovního prostoru.
3. Přiřadit pracovní prostor ke svému předplatnému.
4. Vybrat zeměpisné umístění.

## <a name="determine-the-number-of-workspaces-you-need"></a>Určení potřebného počtu pracovních prostorů
Pracovní prostor je prostředek Azure v podobě kontejneru, ve kterém se data shromažďují, agregují, analyzují a zobrazují na webu Azure Portal.

Je možné vytvořit víc pracovních prostorů a přidělit uživatelům přístup k jednomu nebo několika z nich. Minimalizace počtu pracovních prostorů umožňuje dotazování a korelaci s většinou dat. Tato část popisuje případy, kdy je užitečné mít víc než jeden pracovní prostor.

V současné době pracovní prostor nabízí:

* Zeměpisné umístění úložiště dat
* Členitost fakturace
* Izolaci dat
* Obor pro konfiguraci

Na základě výše uvedených charakteristik můžete vytvořit víc pracovních prostorů, pokud:

* Jste globální společnost a potřebujete ukládat data v různých oblastech z důvodů suverenity dat nebo dodržování předpisů.
* Používáte Azure a chcete se vyhnout poplatkům za odchozí datové přenosy tím, že budete mít pracovní prostor ve stejné oblasti jako prostředky Azure, které spravuje.
* Chcete rozdělit náklady na různá oddělení nebo obchodní skupiny na základě využití. Když vytvoříte pracovní prostor pro každé oddělení nebo obchodní skupinu, vaše vyúčtování Azure a údaje o využití budou uvádět poplatky za každý pracovní prostor zvlášť.
* Jste poskytovatel spravované služby a potřebujete uchovávat analytická data pro každého zákazníka odděleně od dat ostatních zákazníků.
* Spravujete několik zákazníků a chcete, aby se každý zákazník, oddělení nebo pracovní skupina mohli podívat na svá vlastní data, ale ne na data ostatních.

Při používání agentů ke shromažďování dat můžete každého agenta konfigurovat tak, aby ukládal data do jednoho nebo více pracovních prostorů.

Pokud používáte System Center Operations Manager, můžete připojit každou skupinu nástroje Operations Manager jen do jednoho pracovního prostoru. Můžete nainstalovat Microsoft Monitoring Agent do počítačů spravovaných nástrojem Operations Manager a nastavit agenta tak, aby odesílal data do nástroje Operations Manager i do jiného pracovního prostoru Log Analytics.

### <a name="workspace-information"></a>Informace o pracovním prostoru

Podrobnosti o pracovním prostoru můžete zobrazit na webu Azure Portal. Podrobnosti je možné zobrazit také na portálu OMS.

#### <a name="view-workspace-information-the-azure-portal"></a>Zobrazení informací o pracovním prostoru na webu Azure Portal

1. Pokud jste to ještě neudělali, přihlaste se na webu [Azure Portal](https://portal.azure.com) pomocí svého předplatného Azure.
2. V nabídce **Centra** klikněte na **Další služby** a v seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Klikněte na** Log Analytics**.  
    ![Centrum Azure](./media/log-analytics-manage-access/hub.png)  
3. V okně Předplatná Log Analytics vyberte pracovní prostor.
4. V okně pracovního prostoru se zobrazí podrobnosti o pracovním prostoru a odkazy na další informace.  
    ![Podrobnosti o pracovním prostoru](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Správa účtů a uživatelů
Každý pracovní prostor může mít přiřazených víc uživatelských účtů, z nichž každý (účet Microsoft nebo účet organizace) může mít přístup k několika pracovním prostorům.

Ve výchozím se správcem pracovního prostoru stává účet Microsoft nebo účet organizace použitý při vytvoření pracovního prostoru. Správce pak může pozvat další účty Microsoft nebo vybrat uživatele ze služby Azure Active Directory.

Udělování přístupu k pracovnímu prostoru se provádí na dvou místech:

* V Azure můžete použít řízení přístupu na základě rolí k poskytnutí přístupu k předplatnému Azure a přidruženým prostředkům. Tato oprávnění se používají také pro přístup k PowerShellu a REST API.
* V portálu OMS se přiděluje přístup jen k samotnému portálu OMS, nikoli k přidruženému předplatnému Azure.

K zobrazení dat na dlaždicích řešení Backup a Site Recovery je potřeba oprávnění správce nebo spolusprávce k předplatnému Azure, se kterým je pracovní prostor propojený.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Správa přístupu ke službě Log Analytics pomocí webu Azure Portal
Pokud uživatelům umožníte přístup k pracovním prostoru protokolu Log Analytics prostřednictvím oprávnění Azure, například na webu Azure Portal, pak tito uživatelé budou mít přístup i k portálu Log Analytics. Pokud jsou uživatelé na webu Azure Portal, můžou přejít na portál OMS kliknutím na úlohu **OMS Portal** při prohlížení prostředku pracovního prostoru Log Analytics.

Ohledně webu Azure Portal je třeba pamatovat na několik věcí:

* Nejde o *řízení přístupu na základě rolí*. Pokud máte na webu Azure Portal přístup *čtenář* pro pracovní prostor Log Analytics, pak můžete provádět změny pomocí portálu OMS. Portál OMS má koncepce pouze pro uživatele Správce, Přispěvatel a Pouze pro čtení. Pokud je účet, ke kterému jste přihlášeni, propojený ve službě Azure Active Directory s pracovním prostorem, budete na portálu OMS jeho správcem, jinak budete přispěvatelem.
* Když se přihlásíte k portálu OMS přes stránku http://mms.microsoft.com, ve výchozím nastavení uvidíte seznam **Vybrat pracovní prostor**. Tento seznam obsahuje jen pracovní prostory, které byly přidány pomocí portálu OMS. Když chcete zobrazit pracovní prostory, ke kterým máte přístup s předplatným Azure, bude potřeba zadat tenanta jako část adresy URL. Například:

  `mms.microsoft.com/?tenant=contoso.com` Identifikátor tenanta je často poslední částí e-mailové adresy, se kterou se přihlašujete.
* Pokud je účet, se kterým se přihlašujete, v tenantu Azure Active Directory, pak jste *Správce* na portálu OMS. To většinou platí, pokud se nepřihlašujete jako odběratel CSP.  Pokud váš účet není účet tenanta Azure Active Directory, budete *Uživatel* portálu OMS.
* Pokud chcete přejít přímo na portál, ke kterému máte přístup s použitím oprávnění Azure, je třeba zadat prostředek jako část adresy URL. Tuto adresu URL můžete získat pomocí prostředí PowerShell.

  Například, `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Adresa URL vypadá takto: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

Pokud například uživatel chce přidat nebo odebrat řešení pro správu, musí být při používání webu Azure Portal administrátorem nebo přispěvatelem předplatného Azure. Navíc musí být uživatel členem skupiny přispěvatelů pracovního prostoru OMS nebo mít roli administrátora na portálu OMS.

### <a name="managing-users-in-the-oms-portal"></a>Správa uživatelů v portálu OMS
Uživatele a skupiny můžete spravovat na kartě **Manage Users** (Správa uživatelů) v rámci karty **Accounts** (Účty) na stránce nastavení.   

![Správa uživatelů](./media/log-analytics-manage-access/setup-workspace-manage-users.png)


#### <a name="add-a-user-to-an-existing-workspace"></a>Přidání uživatele do existujícího pracovního prostoru
Pomocí následujícího postupu přidáte k pracovnímu prostoru uživatele nebo skupinu.

1. Na portálu OMS klikněte na dlaždici **Nastavení**.
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

   Pokud přidáváte účet Microsoft, bude pozvánka k připojení k danému pracovnímu prostoru odeslána na zadaný e-mail. Až uživatel provede postup připojení k OMS popsaný v e-mailové pozvánce, bude mít přístup k tomuto pracovnímu prostoru.
   Pokud přidáváte účet organizace, uživatel bude mít přístup ke službě Log Analysis okamžitě.  

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
1. Na portálu OMS klikněte na dlaždici **Nastavení**.
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Manage Users** (Správa uživatelů).
3. Vyberte roli pro uživatele, kterého chcete upravit.
4. V potvrzovacím dialogovém okně klikněte na **Yes**.

### <a name="remove-a-user-from-a-workspace"></a>Odebrání uživatele z pracovního prostoru
Pomocí následujícího postupu můžete odebrat uživatele z pracovního prostoru. Odebrání uživatel nezavře pracovní prostor. Místo toho se jen odebere přidružení mezi pracovním prostorem a uživatelem. Pokud je uživateli přidruženo několik pracovních prostorů, tento uživatel se pořád bude moci přihlašovat do OMS a zobrazit své ostatní pracovní prostory.

1. Na portálu OMS klikněte na dlaždici **Nastavení**.
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
Všechny pracovní prostory vytvořené po 26. září 2016 musí být propojené s předplatným Azure v okamžiku vytvoření. Pracovní prostory vytvořené před tímto datem je potřeba propojit s pracovním prostorem při vašem dalším přihlášení. Když vytváříte pracovní prostor z webu Azure Portal nebo propojujete pracovní prostor s předplatným Azure, služba Azure Active Directory se propojí jako účet vaší organizace.

![propojení s předplatným Azure](./media/log-analytics-manage-access/required-link.png)

> [!IMPORTANT]
> K propojení s pracovním prostorem musí váš účet Azure už mít k tomuto pracovnímu prostoru přístup.  Jinými slovy – účet, který používáte pro přístup k webu Azure Portal, musí být **stejný** jako účet, který používáte pro přístup k pracovnímu prostoru. Pokud ne, přečtěte si část [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).
>
>

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Propojení pracovního prostoru s předplatným Azure v portálu OMS
Pokud chcete propojit pracovní prostor s předplatným Azure v portálu OMS, přihlášený uživatel už musí mít placený účet Azure.

1. Na portálu OMS klikněte na dlaždici **Nastavení**.
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Azure Subscription & Data Plan** (Předplatné a datový tarif Azure).
3. Klikněte na datový tarif, který chcete použít.
4. Klikněte na **Uložit**.  
   ![Předplatné a datové tarify](./media/log-analytics-manage-access/subscription-tab.png)

Váš nový datový tarif se zobrazí pásu karet portálu OMS v horní části webové stránky.

![Pás karet OMS](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Propojení pracovního prostoru s předplatným Azure na webu Azure Portal
1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
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
8. Klikněte na tlačítko **OK**. Pracovní prostor je teď propojený s vaším účtem Azure.

> [!NOTE]
> Pokud nevidíte pracovní prostor, s kterým chcete účet propojit, znamená to, že vaše předplatné Azure nemá přístup k pracovnímu prostoru, který jste vytvořili na webu OMS.  Informace o udělení přístupu k tomuto účtu z portálu OMS najdete v části [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Upgrade pracovního prostoru na placený tarif
Ve službě OMS existují tři tarify pracovního prostoru: **Free**, **Standalone** a **Premium**.  Pokud používáte tarif *Free*, existuje limit 500 MB pro odesílání dat do služby Log Analytics.  Pokud toto množství překročíte, budete muset přejít na placený tarif, aby se vám neshromažďovala data nad tento limit. Svůj tarif můžete kdykoli změnit.  Informace o cenách OMS najdete na stránce [podrobností o cenách](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing).

### <a name="using-entitlements-from-an-oms-subscription"></a>Používání nároků z předplatného OMS
Pokud chcete používat nároky z nákupu OMS E1, OMS E2 OMS nebo doplňku OMS pro System Center, zvolte tarif *OMS* služby OMS Log Analytics.

Když si koupíte předplatné OMS, nároky se přidají do vaší smlouvy Enterprise. Jakékoli předplatné vytvořené v rámci této smlouvy může tento nárok uplatnit. Díky tomu můžete například mít několik pracovních prostorů využívajících nárok vyplývající z předplatných OMS.

Pokud se chcete ujistit, že váš pracovní prostor využívá nárok plynoucí z předplatného OMS, proveďte následující:

1. Vytvořte pracovní prostor v předplatném Azure, které je součástí smlouvy Enterprise obsahující předplatné OMS.
2. Vyberte pro pracovní prostor tarif *OMS*.

> [!NOTE]
> Pokud jste pracovní prostor vytvořili před 26. zářím 2016 a cenový tarif služby Log Analytics je *Premium*, bude tento pracovní prostor používat nároky z doplňku OMS pro System Center. Své nároky můžete využít také tak, že přejdete na cenovou úroveň *OMS*.
>
>

Nároky na předplatné OMS nejsou viditelné na Azure nebo na portálu OMS. Tento nárok uvidíte jen na webu Enterprise Portal.  

Pokud potřebujete změnit předplatné Azure, se kterým je pracovní prostor propojený, můžete použít rutinu prostředí Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx).

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Využití závazků Azure ze smlouvy Enterprise
Pokud nemáte předplatné OMS, platíte za každou součást OMS zvlášť a využití se zobrazí na faktuře Azure.

Pokud jsou vaše předplatná Azure propojena se smlouvou Enterprise s finančním závazkem, jakékoli využití služby Log Analytics se bude automaticky strhávat ze zbývajícího finančního závazku.

Pokud potřebujete změnit předplatné Azure, se kterým je pracovní prostor propojený, můžete použít rutinu prostředí Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx).  

### <a name="change-a-workspace-to-a-paid-pricing-tier"></a>Změna pracovního prostoru na placenou cenovou úroveň
1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Vyberte pracovní prostor.  
4. V okně pracovního prostoru v části **Obecné** klikněte na **Cenová úroveň**.  
5. V části **Cenová úroveň** vyberte cenovou úroveň a klikněte na **Vybrat**.  
    ![výběr tarifu](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Po aktualizaci zobrazení na webu Azure Portal uvidíte položku **Cenová úroveň** aktualizovanou na vybranou úroveň.  
    ![aktualizovaný plán](./media/log-analytics-manage-access/manage-access-change-plan04.png)

> [!NOTE]
> Pokud je váš pracovní prostor propojený s účtem Automation, musíte před tím, než budete moci vybrat cenovou úroveň *Standalone (za GB)*, odstranit všechna řešení **Automation and Control** a zrušit propojení s účtem Automation. V okně pracovního prostoru v části **Obecné** klikněte na **Řešení**. Zobrazí se řešení a můžete je odstranit. Propojení s účtem Automation zrušíte kliknutím na název účtu Automation v okně **Cenová úroveň**.
>
>

## <a name="change-how-long-log-analytics-stores-data"></a>Změna doby, po kterou služba Log Analytics ukládá data

Na cenové úrovni Free služba Log Analytics zpřístupňuje data za posledních sedm dnů.
Na cenové úrovni Standard služba Log Analytics zpřístupňuje data za posledních 30 dnů.
Na cenové úrovni Premium služba Log Analytics zpřístupňuje data za posledních 365 dnů.
Na cenových úrovních Standalone a OMS služba Log Analytics ve výchozím nastavení zpřístupňuje data za posledních 31 dnů.

Při použití cenových úrovní Standalone a OMS můžete uchovávat až 2 roky dat (730 dnů). Za data uložená déle, než je výchozích 31 dnů, se účtuje poplatek za uchování. Další informace o cenách najdete v tématu věnovaném [poplatkům za nadlimitní využití](https://azure.microsoft.com/pricing/details/log-analytics/).

Pokud chcete změnit dobu uchování dat:

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Vyberte pracovní prostor.  
4. V okně pracovního prostoru v části **Obecné** klikněte na **Uchování**.  
5. Pomocí posuvníku zvyšte nebo snižte počet dní uchovávání a potom klikněte na **Uložit**
![změnit uchování](./media/log-analytics-manage-access/manage-access-change-retention01.png).

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Změna organizace Azure Active Directory pracovního prostoru

Můžete změnit organizaci Azure Active Directory pracovního prostoru. Změna organizace Azure Active Directory vám umožní přidávat uživatele a skupiny z tohoto adresáře do pracovního prostoru.

### <a name="to-change-the-azure-active-directory-organization-for-a-workspace"></a>Změna organizace Azure Active Directory pracovního prostoru

1. Na stránce Nastavení na portálu OMS klikněte na **Účty** a pak na kartu **Spravovat uživatele**.  
2. Zkontrolujte informace o účtech organizace a klikněte na **Změnit organizaci**.  
    ![změna organizace](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Zadejte informace o identitě správce vaší domény Azure Active Directory. Následně se zobrazí potvrzení o tom, že je váš pracovní prostor propojený s doménou Azure Active Directory.  
    ![Potvrzení o propojení pracovního prostoru](./media/log-analytics-manage-access/manage-access-add-adorg02.png)


## <a name="delete-a-log-analytics-workspace"></a>Odstranění pracovního prostoru Log Analytics
Když odstraníte pracovní prostor Log Analytics, odstraní se všechna data související s vaším pracovním prostorem ze služby OMS během 30 dní.

Pokud jste správce a k pracovnímu prostoru bylo přidruženo víc uživatelů, přidružení těchto uživatelů s pracovním prostorem se přeruší. Pokud byli tito uživatelé přidruženi s jinými pracovními prostory, můžou pokračovat v používání služby OMS s těmito prostory. Pokud ale s jinými pracovními prostory přidružení nejsou, budou muset pro další používání služby OMS vytvořit pracovní prostor.

### <a name="to-delete-a-workspace"></a>Odstranění pracovního prostoru
1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).
2. Přejděte na **Log Analytics** a vyberte tuto možnost.
3. Uvidíte svůj seznam existujících pracovních prostorů. Vyberte pracovní prostor, který chcete odstranit.
4. V okně pracovního prostoru klikněte na **Odstranit**.  
    ![odstranění](./media/log-analytics-manage-access/delete-workspace01.png)
5. V potvrzovacím dialogovém okně klikněte na **Ano**.

## <a name="next-steps"></a>Další kroky
* Postup přidání agentů a shromažďování dat obsahuje článek [Propojení počítačů s Windows se službou Log Analytics](log-analytics-windows-agents.md).
* Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
* Pokud vaše organizace používá proxy server nebo bránu firewall, postupujte podle článku [Konfigurace nastavení proxy serveru a brány firewall ve službě Log Analytics](log-analytics-proxy-firewall.md), aby agenti mohli se službou Log Analytics komunikovat.

