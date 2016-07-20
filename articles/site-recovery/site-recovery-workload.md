<properties
    pageTitle="Jaké úlohy je možné chránit pomocí Azure Site Recovery?" 
    description="Azure Site Recovery chrání úlohy a aplikace tím, že koordinuje replikaci, přebírání služeb při selhání a obnovování lokálních virtuálních počítačů a fyzických serverů. Tyto procesy mohou využívat Azure nebo sekundární místní lokalitu." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="03/27/2016" 
    ms.author="raynew"/>

# Jaké úlohy je možné chránit pomocí Azure Site Recovery?


Tento článek popisuje, které úlohy a aplikace lze chránit pomocí Azure Site Recovery.

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## Přehled

Organizace potřebují strategii pro provozní kontinuitu a zotavení po havárii (BCDR), která určuje, jakým způsobem zůstanou aplikace, procesy a data dostupné během plánovaných a neplánovaných výpadků a jakým způsobem se co nejdříve obnoví normální pracovní podmínky.

Site Recovery je služba Azure, která přispívá ke strategii BCDR tím, že vám umožní nasazení řešení, která sledují aplikace s cílem zvýšit jejich odolnost a orchestrují replikaci lokálních fyzických serverů a virtuálních počítačů do cloudu (Azure) nebo do sekundární lokality. Ať již máte aplikace na bázi Windows nebo Linuxu a ať běží na fyzických serverech nebo na virtuálních počítačích VMware či Hyper-V, můžete pomocí Site Recovery orchestrovat replikaci, testovat zotavení po havárii, přebírat služby při selhání a vracet je po obnovení.


Site Recovery se integruje s aplikacemi Microsoftu, mezi které patří SharePoint, Exchange, Dynamics, SQL Server a Active Directory. Microsoft také úzce spolupracuje s předními dodavateli, jako je Oracle, SAP, IBM a Red Hat, s cílem zajistit, aby na platformách Microsoftu včetně Azure dobře fungovaly i jejich aplikace a služby. Své řešení můžete přizpůsobit na bázi jednotlivých aplikací.

## Proč pro ochranu aplikací používat Site Recovery?

Site Recovery pro ochranu a obnovení na úrovni aplikací přináší následující: 

- Téměř synchronní replikace i při úrovni RPO stanovené na 30 sekund, což odpovídá nárokům nejdůležitějších firemních aplikací
- Snímky konzistentní s jedno- i vícevrstvými aplikacemi
- Integrace s SQL Server AlwaysOn a partnerství s jinými technologiemi pro replikaci na úrovni aplikací, jako je replikace AD, SQL AlwaysOn, skupiny dostupnosti databáze Exchange (DAG) a Oracle Data Guard
- Flexibilní plány obnovení, které vám umožní obnovit celý zásobník aplikací jediným kliknutím, s možností zahrnout do plánu externí skripty a ručně prováděné akce 
- Pokročilá správa sítě v Site Recovery a Azure pro zjednodušení požadavků na aplikační síť, včetně možnosti rezervovat IP adresy, konfigurovat vyrovnávání zatížení a integrovat Azure Traffic Manager pro přepínání sítě s cílem dosáhnout nízké úrovně RTO
-  Bohatá automatizační knihovna obsahující předpřipravené skripty specifické pro aplikace, které je možné stáhnout a integrovat do plánů obnovení



##Souhrn úloh

Site Recovery dokáže replikovat jakoukoli aplikaci spuštěnou na podporovaném virtuálním počítači. Kromě toho jsme ve spolupráci s produktovými týmy provedli dodatečné testování specifické pro aplikace.

**Úloha** | **Replikace virtuálních počítačů Hyper-V do sekundární lokality** | **Replikace virtuálních počítačů Hyper-V do Azure** | **Replikace virtuálních počítačů VMware do sekundární lokality** | **Replikace virtuálních počítačů VMware do Azure**
---|---|---|---|---
Active Directory, DNS | Ano | Ano | Ano | Ano 
Webové aplikace (IIS, SQL) | Ano | Ano | Ano | Ano
SCOM | Ano | Ano | Ano | Ano
SharePoint | Ano | Ano | Ano | Ano
SAP<br/><br/>Replikace webu SAP do Azure k neclusterovému použití | Ano (testováno Microsoftem) | Ano (testováno Microsoftem) | Ano (testováno Microsoftem) | Ano (testováno Microsoftem)
Exchange (ne DAG) | Ano | Již brzy | Ano | Ano
Vzdálená plocha/VDI | Ano | Ano | Ano | – 
Linux (operační systém a aplikace) | Ano (testováno Microsoftem) | Ano (testováno Microsoftem) | Ano (testováno Microsoftem) | Ano (testováno Microsoftem) 
Dynamics AX | Ano | Ano | Ano | Ano
Dynamics CRM | Ano | Již brzy | Ano | Již brzy
Oracle | Ano (testováno Microsoftem) | Ano (testováno Microsoftem) | Ano (testováno Microsoftem) | Ano (testováno Microsoftem)
Souborový server systému Windows | Ano | Ano | Ano | Ano


## Replikace služby Active Directory a DNS

Pro většinu firemních aplikací má zásadní význam infrastruktura Active Directory a DNS. Během zotavování po havárii budete muset před obnovením aplikací a úloh ochránit a obnovit tyto součástí infrastruktury.

Pomocí Site Recovery můžete pro Active Directory a DNS vytvořit úplný automatizovaný plán zotavení po havárii. Pokud chcete například při selhání SharePointu a SAP převzít tyto služby z primární lokality do sekundární, můžete nastavit plán obnovení, který nejprve převezme Active Directory, a potom další plán specifický pro aplikace, na jehož základě se budou při selhání přebírat služby aplikací závislých na Active Directory.

[Zde jsou další informace](site-recovery-active-directory.md) o ochraně Active Directory a DNS.

## Ochrana SQL Serveru

SQL Server poskytuje základnu pro datové služby využívané mnoha firemními aplikacemi v lokálním datovém centru.  Site Recovery lze používat společně s technologiemi SQL Server HA/DR k ochraně vícevrstvých firemních aplikací, které používají SQL Server. Site Recovery poskytuje následující:

- Jednoduché a nákladově efektivní řešení zotavení po havárii pro SQL Server; možnost replikovat různé verze a edice samostatných serverů a clusterů SQL Server do Azure nebo do sekundární lokality  
- Integrace se skupinami dostupnosti SQL AlwaysOn pro správu převzetí služeb při selhání a jejich navrácení po obnovení na základě plánů obnovení Azure Site Recovery
- Komplexní plány obnovení pro všechny vrstvy v aplikaci, včetně databází na SQL Serveru
- Škálování SQL Serveru podle zatížení ve špičce pomocí Site Recovery, a to rozložením zátěže na větší virtuální počítače IaaS v Azure
- Snadné testování zotavení SQL Serveru po havárii: můžete spouštět testy převzetí služeb při selhání s cílem analyzovat data a kontrolovat dodržování předpisů, aniž by to mělo dopad na produkční prostředí.

[Zde jsou další informace](site-recovery-sql.md) o ochraně SQL Serveru.

##Ochrana SharePointu

Azure Site Recovery pomáhá chránit nasazení SharePointu následujícím způsobem:

- Eliminuje nutnost zajistit infrastrukturu (což by s sebou neslo náklady) pro záložní farmu k zotavení po havárii. Pomocí Site Recovery můžete replikovat celou farmu (webová, aplikační a databázová vrstva) do Azure nebo do sekundární lokality.
- Zjednodušuje nasazení a správu aplikace. Aktualizace zanesené do primární lokality se automaticky replikují, a jsou tedy k dispozici po převzetí služeb při selhání a obnovení farmy v sekundární lokalitě. Snižuje se také složitost správy a náklady spojené s udržováním záložní farmy v aktualizovaném stavu.
- Usnadňuje vývoj a testování aplikací SharePointu díky tomu, že je možné na vyžádání vytvořit repliku produkčního prostředí pro účely testování a ladění.
- Zjednodušuje přechod na cloud tím, že se pomocí Site Recovery provede migrace nasazení SharePointu do Azure.

[Zde jsou další informace](https://gallery.technet.microsoft.com/SharePoint-DR-Solution-f6b4aeae) o ochraně SharePointu.


## Ochrana Dynamics AX

Azure Site Recovery zvyšuje ochranu vašeho řešení Dynamics AX ERP tímto způsobem:

- Orchestrace replikace celého prostředí Dynamics AX (webová a AOS vrstva, databázové vrstvy, SharePoint) do Azure nebo do sekundární lokality
- Zjednodušení migrace nasazení Dynamics AX do cloudu (Azure)
- Usnadnění vývoje a testování aplikací Dynamics AX díky tomu, že je možné na vyžádání vytvořit kopii produkčního prostředí pro účely testování a ladění

[Zde jsou další informace](https://gallery.technet.microsoft.com/Dynamics-AX-DR-Solution-b2a76281) o ochraně Dynamic AX.

## Ochrana Vzdálené plochy 

Služba Vzdálená plocha (RDS) umožňuje nasazení infrastruktury virtuálních klientských počítačů (VDI), tedy stolních počítačů a aplikací na bázi relací, což umožňuje uživatelům pracovat odkudkoli. S Azure Site Recovery můžete:

- Replikovat spravované nebo nespravované virtuální desktopy ve fondu do sekundární lokality a vzdálené aplikace a relace do sekundární lokality nebo Azure
- Zde je seznam toho, co můžete replikovat:

**Vzdálená plocha** | **Replikace virtuálních počítačů Hyper-V do sekundární lokality** | **Replikace virtuálních počítačů Hyper-V do Azure** | **Replikace virtuálních počítačů VMware do sekundární lokality** | **Replikace virtuálních počítačů VMware do Azure** | **Replikace fyzických serverů do sekundární lokality** | **Replikace fyzických serverů do Azure**
---|---|---|---|---|---|---
**Virtuální desktop ve fondu (nespravovaný)** | Ano | Ne | Ano | Ne | Ano | Ne
**Virtuální desktop ve fondu (spravovaný a bez UPD)** | Ano | Ne | Ano | Ne | Ano | Ne
**Vzdálené aplikace a desktopové relace (bez UPD)** | Ano | Ano | Ano | Ano | Ano | Ano


[Zde jsou další informace](https://gallery.technet.microsoft.com/Remote-Desktop-DR-Solution-bdf6ddcb) o ochraně Vzdálené plochy.


## Ochrana Exchange

Site Recovery pomáhá chránit Exchange následujícím způsobem:

- Pro malá nasazení Exchange, jako jsou například jednotlivé nebo neclusterované servery, může Site Recovery provádět replikaci a přebírat služby při selhání do Azure nebo do sekundární lokality.
- U větších nasazení se Site Recovery integruje se skupinami dostupnosti databáze (DAG) Exchange.
- Tyto skupiny představují doporučené řešení pro zotavení Exchange ve velké firmě po havárii.  Plány obnovení Site Recovery mohou zahrnovat skupiny DAG, které orchestrují přebírání služeb při selhání mezi lokalitami.


[Zde jsou další informace](https://gallery.technet.microsoft.com/Exchange-DR-Solution-using-11a7dcb6) o ochraně Exchange.

## Ochrana nasazení SAP

Pomocí Site Recovery můžete své nasazení SAP chránit následujícím způsobem: 

- Aktivace ochrany u celého nasazení SAP nastavením replikace různých vrstev nasazení do Azure nebo do sekundární lokality
- Zjednodušení migrace na cloud pomocí Site Recovery migrací nasazení SAP do Azure
- Usnadnění vývoje a testování aplikací SAP díky tomu, že je možné na vyžádání vytvořit kopii produkčního prostředí pro testování a ladění aplikací

[Zde jsou další informace](http://aka.ms/asr-sap) o ochraně nasazení SAP.

## Další kroky

[Příprava](site-recovery-best-practices.md) pro nasazení Site Recovery




<!--HONumber=Jun16_HO2-->


