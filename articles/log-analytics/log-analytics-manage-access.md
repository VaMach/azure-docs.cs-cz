<properties
    pageTitle="Správa přístupu ke službě Log Analytics | Microsoft Azure"
    description="Správa přístupu ke službě Log Analytics pomocí různých úloh správy prováděných s uživateli, účty, pracovními prostory OMS a účty Azure."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>


# Správa přístupu ke službě Log Analytics

Pro správu přístupu ke službě Log Analytics můžete používat různé úlohy správy prováděné s uživateli, účty, pracovními prostory OMS a účty Azure. Při vytváření nového pracovního prostoru OMS (Operations Management Suite) zvolíte název pracovního prostoru, přidružíte ho ke svému účtu a zvolte zeměpisné umístění. Pracovní prostor je v podstatě kontejner, který obsahuje informace o účtu a jednoduché konfigurační informace. Vy nebo další členové vaší organizace můžete používat víc pracovních prostorů OMS ke správě různých dat, která se shromažďují ze všech částí vaší infrastruktury IT.

Článek [Začínáme se službou Log Analytics](log-analytics-get-started.md) popisuje, jak službu rychle zprovoznit a zbývající část tohoto článku popisuje podrobněji některé akce, které potřebujete pro správu přístupu k OMS.

I když zpočátku nemusíte potřebovat všechny úlohy správy, pokrýváme v následujících částech všechny běžné úlohy, které můžete potřebovat:

- Určení potřebného počtu pracovních prostorů
- Správa účtů a uživatelů
- Přidání skupiny do existujícího pracovního prostoru
- Připojení existujícího pracovního prostoru k předplatnému Azure
- Upgrade pracovního prostoru na placený datový tarif
- Změna typu datového tarifu
- Přidání organizace Azure Active Directory k existujícímu pracovnímu prostoru
- Zavření pracovního prostoru OMS

## Určení potřebného počtu pracovních prostorů

Pracovní prostor je prostředek Azure v podobě kontejneru, ve kterém se data shromažďují, agregují, analyzují a zobrazují v portálu OMS.

Je možné vytvořit víc pracovních prostorů OMS Log Analytics a přidělit uživatelům přístup k jednomu nebo několika z nich. Obecně je vhodné minimalizovat počet pracovních prostorů, protože vám to umožní dotazovat a korelovat nad větším objemem dat. Tato část popisuje případy, kdy je užitečné mít víc než jeden pracovní prostor.

V současné době pracovní prostor Log Analytics nabízí:

- Zeměpisné umístění úložiště dat
- Členitost fakturace
- Izolaci dat

Na základě výše uvedených charakteristik můžete vytvořit víc pracovních prostorů, pokud:

- Jste globální společnost a potřebujete ukládat data v různých oblastech z důvodů suverenity dat nebo dodržování předpisů.
- Používáte Azure a chcete se vyhnout poplatkům za odchozí datové přenosy tím, že budete mít pracovní prostor Log Analytics ve stejné oblasti jako prostředky Azure, které spravuje.
- Chcete rozdělit náklady na různá oddělení nebo obchodní skupiny na základě využití. Když vytvoříte pracovní prostor pro každé oddělení nebo obchodní skupinu, vaše vyúčtování Azure a údaje o využití budou uvádět poplatky za každý pracovní prostor zvlášť.
- Jste poskytovatel spravované služby a potřebujete uchovávat analytická data pro každého zákazníka odděleně od dat ostatních zákazníků.
- Spravujete víc zákazníků a chcete, aby každý zákazník, oddělení nebo obchodní skupina mohli prohlížet svá vlastní data, ale ne data jiných zákazníků, oddělení nebo obchodních skupin.

Při používání agentů ke shromažďování dat můžete každého agenta konfigurovat tak, aby ukládal data do požadovaného pracovního prostoru.

Pokud používáte System Center Operations Manager, můžete připojit každou skupinu nástroje Operations Manager jen do jednoho pracovního prostoru. Můžete nainstalovat Microsoft Monitoring Agent do počítačů spravovaných nástrojem Operations Manager a nastavit agenta tak, aby odesílal data do nástroje Operations Manager i do jiného pracovního prostoru Log Analytics.

### Informace o pracovním prostoru

Na portálu OMS můžete zobrazit informace o pracovním prostoru a určit, zda chcete zasílat informace od společnosti Microsoft.

#### Zobrazení informací o pracovním prostoru

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty).
3. Klikněte na kartu **Workspace Information** (Informace o pracovním prostoru).  
  ![Informace o pracovním prostoru](./media/log-analytics-manage-access/workspace-information.png)

## Správa účtů a uživatelů

Každý prostor může mít více uživatelských účtů, které s ním spojená, a každý uživatelský účet (účet Microsoft nebo účtu organizace) mají přístup do více OMS pracovních prostorů.

Ve výchozím se správcem pracovního prostoru stává účet Microsoft nebo účet organizace použitý při vytvoření pracovního prostoru. Správce pak může pozvat další účty Microsoft nebo vybrat uživatele ze služby Azure Active Directory.

Udělování přístupu k pracovnímu prostoru OMS se provádí na dvou místech:

- V Azure můžete použít řízení přístupu na základě rolí k poskytnutí přístupu k předplatnému Azure a přidruženým prostředkům. To se také používá pro přístup k prostředí PowerShell a rozhraní REST API.
- V portálu OMS se přiděluje přístup jen k samotnému portálu OMS, nikoli k přidruženému předplatnému Azure.

Pokud někomu udělíte přístup k portálu OMS, ale ne k předplatnému Azure, se kterým je propojený, pak dlaždice řešení Automation, Backup a Site Recovery nebudou takovému uživateli v portálu OMS zobrazovat žádná data.

Pokud chcete, aby uživatelé mohli zobrazovat data v těchto řešeních, Povolit všem uživatelům zobrazit data v těchto řešeních, ujistěte se, mají alespoň přístup **čtenář** pro účet Automation, trezor Backup a trezor Site Recovery, které jsou propojené s pracovním prostorem OMS.   

### Správa přístupu ke službě Log Analytics pomocí webu Azure Portal

Pokud uživatelům umožníte přístup k pracovním prostoru protokolu Log Analytics prostřednictvím oprávnění Azure, například na webu Azure Portal, pak tito uživatelé budou mít přístup i k portálu Log Analytics. Pokud jsou uživatelé na webu Azure Portal, můžou přejít na portál OMS kliknutím na úlohu **OMS Portal** při prohlížení prostředku pracovního prostoru Log Analytics.

Ohledně webu Azure Portal je třeba pamatovat na několik věcí:

- Nejde o *řízení přístupu na základě rolí*. Pokud máte na webu Azure Portal přístup *čtenář* pro pracovní prostor Log Analytics, pak můžete provádět změny pomocí portálu OMS. Portál OMS má koncepce pouze pro uživatele Správce, Přispěvatel a Pouze pro čtení. Pokud je účet, ke kterému jste přihlášení, propojený ve službě Azure Active Directory s pracovním prostorem, budete na portálu OMS jeho správcem, v opačném případě budete přispěvatelem.

- Když se přihlásíte k portálu OMS přes stránku http://mms.microsoft.com, ve výchozím nastavení uvidíte seznam **Select a workspace** (Vyberte pracovní prostor). Tento seznam obsahuje jen pracovní prostory, které byly přidány pomocí portálu OMS. Chcete-li zobrazit pracovní prostory, ke kterým máte přístup s předplatným Azure, bude třeba zadat tenanta jako část adresy URL. Příklad:

  `mms.microsoft.com/?tenant=contoso.com` Identifikátor tenanta je často poslední částí e-mailové adresy, se kterou se přihlašujete.

- Pokud je účet, ke kterému jste přihlášení, propojený ve službě Azure Active Directory s pracovním prostorem, budete na portálu OMS jeho *správcem*, v opačném případě budete přispěvatelem. Pokud váš účet není účet tenanta služby Azure Active Directory budete na portálu OMS *Uživatel*.

- Pokud chcete přejít přímo na portál, ke kterému máte přístup s použitím oprávnění Azure, je třeba zadat prostředek jako část adresy URL. Tuto adresu URL můžete získat pomocí prostředí PowerShell.

  Například `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  Adresa URL bude vypadat takto:
`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### Správa uživatelů v portálu OMS

Uživatele a skupiny můžete spravovat na kartě **Manage Users** (Správa uživatelů) v rámci karty **Accounts** (Účty) na stránce nastavení. Můžete tady provádět akce v následujících oddílech.  

![Správa uživatelů](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### Přidání uživatele do existujícího pracovního prostoru

Pomocí následujícího postupu přidáte k pracovnímu prostoru OMS uživatele nebo skupinu. Uživatel nebo skupina budou moci zobrazovat a reagovat na všechny výstrahy související s tímto pracovním prostorem.

>[AZURE.NOTE] Pokud chcete přidat uživatele nebo skupinu z účtu Azure Active Directory vaší organizace, je nutné nejdříve zkontrolovat, že máte účet OMS přidružený k doméně služby Active Directory. Viz [Přidání organizace Azure Active Directory k existujícímu pracovnímu prostoru](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Manage Users** (Správa uživatelů).
3. V oddílu **Manage Users** (Správa uživatelů) vyberte typ účtu, který chcete přidat: **Organizational Account** (Účet organizace), **Microsoft Account** (Účet Microsoft), nebo **Microsoft Support** (Podpora Microsoft).
    - Pokud vyberete možnost Microsoft Account, zadejte e-mailovou adresu uživatele účtu Microsoft.
    - Pokud vyberete možnost Organizational Account, můžete zadat část e-mailového aliasu nebo jména uživatele nebo skupiny a zobrazí se seznam vyhovujících uživatelů a skupin. Vyberte uživatele nebo skupinu.
    - Pomocí účtu Microsoft Support můžete pracovníkovi podpory Microsoftu poskytnout dočasný přístup k vašemu pracovnímu prostoru kvůli odstraňování potíží.

    >[AZURE.NOTE] Nejlepšího výkonu dosáhnete, když omezíte počet skupin Active Directory přidružených k jednomu účtu OMS na tři – jednu pro správce, jednu pro přispěvatele a jednu pouze pro čtení. Použití více skupin může ovlivnit výkon služby Log Analytics.

5. Vyberte typ uživatele nebo skupiny: **Správce**, **Přispěvatel** nebo **Jen pro čtení**.  
6. Klikněte na tlačítko **Přidat**.

  Pokud přidáváte účet Microsoft, bude pozvánka k připojení k danému pracovnímu prostoru odeslána na zadaný e-mail. Až uživatel provede postup připojení k OMS popsaný v e-mailové pozvánce, může zobrazit výstrahy a informace o účtu pro tento účet OMS a bude také moci zobrazit informace o uživateli na kartě **Accounts** (Účty) stránky **Settings** (Nastavení).
  Pokud přidáváte účet organizace, uživatel bude mít přístup ke službě Log Analysis okamžitě.  
  ![E-mailová pozvánka](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### Úprava typu existujícího uživatele

Můžete změnit roli účtu pro uživatele přidruženého k účtu OMS. Máte následující možnosti rolí:

 - *Správce*: může spravovat uživatele, zobrazovat a reagovat na všechny výstrahy a přidávat a odebírat servery.

 - *Přispěvatel*: může zobrazovat a reagovat na všechny výstrahy a přidávat a odebírat servery.

 - *Jen pro čtení*: takto označení uživatelé nemůžou:
   1. Přidávat a odebírat řešení. Galerie řešení je skrytá.
   2. Přidávat, upravovat nebo odebírat dlaždice na řídicím panelu **My Dashboard** (Můj řídicí panel).
   3. Zobrazit stránky **Settings** (Nastavení). Tyto stránky jsou skryté.
   4. V zobrazení hledání jsou skryté konfigurace Power BI, uložená hledání a úlohy výstrah.


#### Postup úpravy účtu

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Manage Users** (Správa uživatelů).
3. Vyberte roli pro uživatele, kterého chcete upravit.
2. V potvrzovacím dialogovém okně klikněte na **Yes**.

### Odebrání uživatele z pracovního prostoru OMS

Pomocí následujícího postupu můžete odebrat uživatele z pracovního prostoru OMS. Všimněte si, že se tím pracovní prostor uživatele nezavře. Místo toho se jen odebere přidružení mezi pracovním prostorem a uživatelem. Pokud je uživateli přidruženo několik pracovních prostorů, tento uživatel se stále bude moci přihlásit k OMS a zobrazit ostatní pracovní prostory.

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Manage Users** (Správa uživatelů).
3. Klikněte na možnost **Remove** (Odebrat) vedle jména uživatele, kterého chcete odebrat.
4. V potvrzovacím dialogovém okně klikněte na **Yes**.


### Přidání skupiny do existujícího pracovního prostoru

1.  Postupujte podle kroků 1-4 v části „Přidání uživatele do existujícího pracovního prostoru“ nahoře.
2.  V části **Choose User/Group** (Vyberte uživatele/skupinu) vyberte **Group** (Skupina).
    ![Přidání skupiny do existujícího pracovního prostoru](./media/log-analytics-manage-access/add-group.png)
3.  Zadejte zobrazovaný název nebo e-mailovou adresu pro skupinu, kterou chcete přidat.
4.  Vyberte skupinu ze seznamu výsledků a klikněte na **Add** (Přidat).

## Připojení existujícího pracovního prostoru k předplatnému Azure

Je možné vytvořit pracovní prostor z webu [microsoft.com/oms](https://microsoft.com/oms).  Pro tyto pracovní prostory ale existují určitá omezení, z nichž nejvýraznější je limit 500 MB uložených dat denně, pokud používáte bezplatný účet. Za účelem provedení změn tohoto pracovního prostoru bude třeba *propojit existující pracovní prostor s předplatným Azure*.

>[AZURE.IMPORTANT] K propojení s pracovním prostorem musí váš účet Azure již mít k tomuto pracovnímu prostoru přístup.  Jinými slovy – účet, který používáte pro přístup k webu Azure Portal, musí být **stejný** jako účet, který používáte pro přístup k pracovnímu prostoru OMS. Pokud tomu tak není, projděte si část [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).

### Propojení pracovního prostoru s předplatným Azure v portálu OMS

Pokud chcete propojit pracovní prostor s předplatným Azure v portálu OMS, přihlášený uživatel už musí mít placený účet Azure. Pracovní prostor, který právě používáte, se propojí s účtem Azure.

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a potom na kartu **Azure Subscription & Data Plan** (Předplatné a datový tarif Azure).
3. Klikněte na datový tarif, který chcete použít.
4. Klikněte na **Uložit**.  
  ![Předplatné a datové tarify](./media/log-analytics-manage-access/subscription-tab.png)

Váš nový datový tarif se zobrazí pásu karet portálu OMS v horní části webové stránky.

![Pás karet OMS](./media/log-analytics-manage-access/data-plan-changed.png)

### Propojení pracovního prostoru s předplatným Azure na webu Azure Portal

1.  Přihlaste se k [Azure Portal](http://portal.azure.com).
2.  Přejděte k položce **Log Analytics (OMS)** a vyberte ji.
3.  Uvidíte seznam existujících pracovních prostorů. Klikněte na tlačítko **Přidat**.  
    ![Seznam pracovních prostorů](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  V oddílu **OMS Workspace** klikněte na možnost **Or link existing** (Nebo propojit existující).  
    ![Propojení existujícího](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Klikněte na **Configure required settings** (Konfigurovat požadovaná nastavení).  
    ![Konfigurace požadovaných nastavení](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Uvidíte seznam pracovních prostorů, které ještě nejsou propojené s vaším účtem Azure. Vyberte pracovní prostor.  
    ![Výběr pracovního prostoru](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Je-li to třeba, můžete změnit hodnoty následujících položek:
    - Předplatné
    - Skupina prostředků
    - Umístění
    - Cenová úroveň  
        ![Změna hodnot](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Klikněte na možnost **Vytvořit**. Pracovní prostor je teď propojený s vaším účtem Azure.

>[AZURE.NOTE] Pokud nevidíte pracovní prostor, s kterým chcete účet propojit, znamená to, že vaše předplatné Azure nemá přístup k pracovnímu prostoru OMS, který jste vytvořili na webu OMS.  Účtu budete muset udělit přístup z pracovního prostoru OMS na webu OMS. Postup najdete v části [Přidání uživatele do existujícího pracovního prostoru](#add-a-user-to-an-existing-workspace).



## Upgrade pracovního prostoru na placený datový tarif

Ve službě OMS existují tři datové tarify pracovního prostoru: **Free**, **Standard** a **Premium**.  Pokud máte tarif *Free*, můžete narazit na omezení 500 MB dat denně.  Potřebujete-li shromažďovat víc dat, je třeba upgradovat pracovní prostor na ***průběžné platby***. Svůj datový tarif můžete změnit kdykoli.  Informace o cenách OMS najdete na stránce [podrobností o cenách](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

>[AZURE.IMPORTANT] Tarif pracovního prostoru je možné změnit, jen když je *propojený* s předplatným Azure.  Pokud jste vytvořili pracovní prostor v Azure nebo pokud jste *již propojili* pracovní prostor s účtem, můžete tuto zprávu ignorovat.  Pokud jste vytvořili pracovní prostor na [webu OMS](http://www.microsoft.com/oms), budete muset provést postup v části [Připojení existujícího pracovního prostoru k předplatnému Azure](#link-an-existing-workspace-to-an-azure-subscription).

### Použití nároků z doplňku OMS pro System Center

Doplněk OMS pro System Center poskytuje nárok na tarif Premium pro službu OMS Log Analytics, popsaný v [článku o cenách OMS](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

Při nákupu doplňku OMS pro System Center je doplněk OMS přidán jako nárok do vaší smlouvy na System Center. Jakékoli předplatné vytvořené v rámci této smlouvy může tento nárok uplatnit. Díky tomu můžete například mít několik pracovních prostorů OMS využívajících nárok vyplývající z doplňku OMS.

Pokud se chcete ujistit, že váš pracovní prostor OMS využívá nárok plynoucí z doplňku OMS, proveďte následující:

1. Propojte pracovní prostor OMS s předplatným Azure, které je součástí smlouvy Enterprise obsahující nákup doplňku OMS i využití předplatného Azure.
2. Vyberte pro pracovní prostor tarif Premium.

Když kontrolujete využití na webu Azure Portal nebo na portálu OMS, neuvidíte nárok plynoucí z doplňku OMS. Tento nárok uvidíte jen na webu Enterprise Portal.  

Pokud potřebujete změnit předplatné Azure, se kterým je pracovní prostor OMS propojený, můžete použít rutinu prostředí Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx).

### Využití závazků Azure ze smlouvy Enterprise

Pokud budete chtít využít samostatné ceny za součásti OMS, můžete platit za každou součást OMS zvlášť a jejich využití bude uvedeno na faktuře Azure.

Pokud jsou vaše předplatná Azure propojena se smlouvou Enterprise s finančním závazkem, jakékoli využití služby Log Analytics se bude automaticky strhávat ze zbývajícího finančního závazku.

Pokud potřebujete změnit předplatné Azure, se kterým je pracovní prostor OMS propojený, můžete použít rutinu prostředí Azure PowerShell [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx).  



### Změna pracovního prostoru na placený datový tarif

1.  Přihlaste se k [Azure Portal](http://portal.azure.com).
2.  Přejděte k položce **Log Analytics (OMS)** a vyberte ji.
3.  Uvidíte seznam existujících pracovních prostorů. Vyberte pracovní prostor.  
    ![Seznam pracovních prostorů](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  V oblasti **Settings** (Nastavení) klikněte na možnost **Pricing tier** (Cenová úroveň).  
    ![Cenová úroveň](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  V oblasti **Pricing tier** (Cenová úroveň) vyberte datový tarif a klikněte na možnost **Select** (Vybrat).  
    ![Výběr tarifu](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Po aktualizaci zobrazení na webu Azure Portal uvidíte položku **Pricing tier** (Cenová úroveň) aktualizovanou na vybraný tarif.  
    ![Změna cenové úrovně](./media/log-analytics-manage-access/manage-access-change-plan04.png)

Nyní můžete shromažďovat data nad rámec bezplatného objemu.


## Přidání organizace Azure Active Directory k existujícímu pracovnímu prostoru

Pracovní prostor Log Analytics (OMS) můžete přidružit k doméně Azure Active Directory. Díky tomu budete moci přidávat uživatele ze služby Active Directory přímo do pracovního prostoru OMS bez nutnosti použít samostatný účet Microsoft.

Když vytváříte pracovní prostor z webu Azure Portal nebo propojujete pracovní prostor s předplatným Azure, služba Azure Active Directory bude propojena jako účet vaší organizace.

Když vytváříte pracovní prostor z portálu OMS, budete vyzváni k propojení s předplatným Azure a s účtem organizace.

### Přidání organizace Azure Active Directory k existujícímu pracovnímu prostoru

1. Na stránce nastavení na portálu OMS klikněte na kartu **Accounts** (Účty) a pak na kartu **Workspace Information** (Informace o pracovním prostoru).  
2. Zkontrolujte informace o účtech organizace a klikněte na možnost **Add Organization** (Přidat organizaci).  
    ![Přidání organizace](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Zadejte informace o identitě správce vaší domény Azure Active Directory. Následně se zobrazí potvrzení o tom, že je váš pracovní prostor propojený s doménou Azure Active Directory.
    ![Potvrzení o propojení pracovního prostoru](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Jakmile bude váš účet propojený s účtem organizace, nebude možné toto propojení změnit ani odebrat.

## Zavření pracovního prostoru OMS

Při zavření pracovního prostoru OMS se ze služby OMS během 30 dnů odstraní všechna související data.

Pokud jste správce a k pracovnímu prostoru bylo přidruženo víc uživatelů, přidružení těchto uživatelů s pracovním prostorem se přeruší. Pokud byli tito uživatelé přidruženi s jinými pracovními prostory, můžou pokračovat v používání služby OMS s těmito prostory. Pokud ale s jinými pracovními prostory přidruženi nejsou, budou muset pro další používání služby OMS vytvořit nové pracovní prostory.

### Postup zavření pracovního prostoru OMS

1. V nástroji OMS klikněte na dlaždici **Settings** (Nastavení).
2. Klikněte na kartu **Accounts** (Účty) a pak na kartu **Workspace Information** (Informace o pracovním prostoru).
3. Klikněte na možnost **Close Workspace** (Zavřít pracovní prostor).
4. Vyberte jeden z přednastavených důvodů zavření pracovního prostoru nebo zadejte jiný důvod do textového pole.
5. Klikněte na **Close workspace** (Zavřít pracovní prostor).

## Další kroky

- Postup přidání agentů a shromažďování dat obsahuje článek [Propojení počítačů s Windows se službou Log Analytics](log-analytics-windows-agents.md).
- Článek [Přidání řešení Log Analytics z galerie řešení](log-analytics-add-solutions.md) popisuje přidání funkcí a shromažďování dat.
- Pokud vaše organizace používá proxy server nebo bránu firewall, postupujte podle článku [Konfigurace nastavení proxy serveru a brány firewall ve službě Log Analytics](log-analytics-proxy-firewall.md), aby agenti mohli se službou Log Analytics komunikovat.



<!--HONumber=Sep16_HO3-->


