
<properties
    pageTitle="Používání služby Azure AD Connect Health se službou AD DS | Microsoft Azure"
    description="Toto je stránka o službě Azure AD Connect Health, která popisuje monitorování služby AD DS."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="arluca"/>


# Používání služby Azure AD Connect Health se službou AD DS
Následující dokumentace se věnuje monitorování služby Active Directory Domain Services pomocí služby Azure AD Connect Health. Patří sem služby AD DS nainstalované v systému Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2.

Informace o monitorování služby AD FS pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md). Kromě toho informace o monitorování služby Azure AD Connect (synchronizace) pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md).

![Azure AD Connect Health pro službu AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## Upozornění služby Azure AD Connect Health pro službu AD DS
Část pojednávající o výstrahách v rámci služby Azure AD Connect Health pro AD DS obsahuje seznam aktivních a vyřešených výstrah, které souvisejí s řadiči domény. Výběrem aktivní nebo vyřešené výstrahy otevřete nové okno, které obsahuje další informace, postup řešení a odkazy na doprovodnou dokumentaci. Každý typ výstrahy může mít jednu nebo více instancí, které odpovídají jednotlivým řadičům domény, kterých se konkrétní výstraha týká. V dolní části okna s výstrahou můžete vybrat ovlivněné řadiče domény. Otevře se nové okno s dalšími podrobnosti o konkrétní instanci výstrahy.

V tomto okně můžete povolit e-mailová oznámení výstrah a také změnit zobrazení časového rozsahu. Zvětšení časového rozsahu vám umožní zobrazit předchozí vyřešené výstrahy.

![Chyba synchronizace služby Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## Řadiče domény
Tento řídicí panel nabízí topologické zobrazení prostředí společně s klíčovými provozními metrikami a stavem jednotlivých monitorovaných řadičů domény. Zobrazené metriky vám pomůžou rychle identifikovat libovolné řadiče domény, které můžou vyžadovat další šetření. Ve výchozím nastavení se zobrazuje jenom podmnožina sloupců. Kliknutím na příkaz sloupce si však můžete zobrazit celou sadu dostupných sloupců. Výběrem sloupců, které vás zajímají nejvíce, se tento řídicí panel promění v jedno přehledné místo, kde si zobrazíte stav prostředí služby AD DS. 

![Řadiče domény](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Řadiče domény můžete seskupit podle příslušné domény nebo webu, což je užitečné k pochopení topologie prostředí. Pokud dvakrát kliknete na záhlaví okna, řídicí panel se roztáhne na celou obrazovku, aby využil její plochu naplno. Hodí se to, když zobrazujete několik sloupců najednou. 

## Stav replikace
Tento řídicí panel zobrazuje stav replikace a topologii replikace pro řadiče domén, které monitorujete. Stav posledního pokusu o replikaci je uvedený spolu s užitečnou dokumentací pro jakékoli nalezené chyby. Po výběru doménového řadiče s chybou se otevře nové okno, které obsahuje další informace, postup řešení a odkazy na dokumentaci, která vám pomůže s řešením potíží. 

![Stav replikace](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## Monitorování
Tato funkce zajišťuje grafické zobrazení trendů různých čítačů výkonu, které se průběžně shromažďují ze všech monitorovaných řadičů domény. Výkon řadiče domény můžete snadno porovnat se všemi ostatními monitorovanými řadiči domény v doménové struktuře. Kromě toho můžete různé čítače výkonu zobrazit vedle sebe, což je užitečné při řešení problémů ve vašem prostředí. 

![Monitorování](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Ve výchozím nastavení jsme předem vybrali čtyři čítače výkonu. Kliknutím na příkaz filtru a výběrem nebo zrušením výběru libovolných požadovaných čítačů, můžete přidat nebo odebrat další čítače. Pokud navíc kliknete na konkrétní graf čítače výkonu, otevře se nové okno, které obsahuje příslušné datové body každého monitorovaného řadiče domény.

## Související odkazy

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Sep16_HO3-->


