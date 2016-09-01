
<properties
    pageTitle="Používání služby Azure AD Connect Health se synchronizací | Microsoft Azure"
    description="Toto je stránka o službě Azure AD Connect Health, která popisuje sledování synchronizace Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# Používání služby Azure AD Connect Health pro synchronizaci
Následující dokumentace se věnuje sledování služby Azure AD Connect (Sync) pomocí služby Azure AD Connect Health.  Informace o sledování služby AD FS pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md). Informace o sledování služby Active Directory Domain Services pomocí služby Azure AD Connect Health najdete v článku [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/sync.png)

## Upozornění služby Azure AD Connect Health pro synchronizaci
Část pojednávající o výstrahách služby Azure AD Connect Health pro synchronizaci uvádí seznam aktivních upozornění. Každé upozornění obsahuje důležité informace, postup řešení a odkazy na související dokumentaci. Výběrem aktivního nebo vyřešeného upozornění zobrazíte nové okno s doplňujícími informacemi, kroky, které můžete k vyřešení upozornění použít, a odkazy na další dokumentaci. Můžete si zobrazit i historické údaje o dříve vyřešených upozorněních.

Výběrem některého upozornění zobrazíte doplňující informace, kroky, které můžete k vyřešení upozornění použít, a odkazy na další dokumentaci.

![Chyba synchronizace služby Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### Omezené vyhodnocení upozornění
Pokud služba Azure AD Connect nepoužívá výchozí konfiguraci (například když je filtrování atributů změněné z výchozí konfigurace na vlastní), agent služby Azure AD Connect Health nebude odesílat chybové události související se službou Azure AD Connect. 

Služba tak bude při vyhodnocování upozornění omezená. Zobrazí se banner, který v rámci služby upozorňuje na tento stav na portálu Azure.

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner.png)

Můžete to změnit kliknutím na „Nastavení“ a povolením, aby agent služby Azure AD Connect Health mohl odesílat všechny protokoly chyb.

![Azure AD Connect Health pro synchronizaci](./media/active-directory-aadconnect-health-sync/banner2.png)

## Analýza synchronizace
V poslední verzi služby Azure AD Connect Health pro synchronizaci byly přidány následující nové funkce:

- sledování latence operací synchronizace
- sledování trendu změn objektů

### Latence synchronizace
Tato funkce poskytuje grafické zobrazení trendu latence operací synchronizace (import, export atd.) pro jednotlivé konektory.  Díky tomu se nejen rychle a snadno seznámíte s latencí operací (výborné pokud máte velké sady změn), ale budete moct i zjišťovat anomálie v latenci, které můžou vyžadovat další šetření.

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/synclatency.png)

Ve výchozím nastavení se zobrazuje jenom latence operace „exportu“ na konektoru Azure AD.  Pokud chcete zobrazit další operace na konektoru nebo zobrazit operace z jiných konektorů, klikněte pravým tlačítkem na graf a zvolte konkrétní operaci a konektor.

### Změny objektů synchronizace
Tato funkce nabízí grafické zobrazení trendu v počtu změn, které se vyhodnocují a exportují do služby Azure AD.  V současné době je snaha o shromáždění těchto informací z protokolů synchronizace obtížná.  Graf poskytuje nejen jednodušší způsob sledování počtu změn ve vašem prostředí, ale i vizuální zobrazení chyb, ke kterým dochází.

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/syncobjectchanges.png)

## Související odkazy

* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)




<!---HONumber=Aug16_HO4-->


