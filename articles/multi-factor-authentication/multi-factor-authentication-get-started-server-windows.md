<properties 
    pageTitle="Ověření Windows a server Azure Multi-Factor Authentication" 
    description="Toto je stránka Azure Multi-Factor Authentication, která vám pomůže při nasazení ověření Windows a serveru Azure Multi-Factor Authentication." 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtand"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/04/2016" 
    ms.author="billmath"/>

# Ověření Windows a server Azure Multi-Factor Authentication

Část ověřování systému Windows umožňuje správci povolit a konfigurovat ověřování systému Windows pro jednu nebo více aplikací.  Následuje seznam skutečností, které je potřeba si zapamatovat před nastavením ověřování systému Windows.

-  Před aktivací Azure Multi-Factor Authentication pro Terminálové služby je nutné restartovat počítač.
-  Pokud je zaškrtnuto políčko „Vyžadovat shodu uživatele Azure Multi-Factor Authentication“ a nejste v seznamu uživatelů, nebudete se po restartu moci přihlásit do počítače.
-  Důvěryhodné IP adresy jsou závislé na tom, zda aplikace může zajistit IP adresu klienta s ověřením. Momentálně jsou podporovány pouze terminálové služby.  







>[AZURE.NOTE]Tato funkce není podporována pro zabezpečené terminálové služby v systému Windows Server 2012 R2.
 



## Chcete-li zabezpečit aplikaci pomocí ověřování systému Windows, použijte následující postup.

1. Na serveru Azure Multi-Factor Authentication klikněte na ikonu ověřování systému Windows.
![Ověřování systému Windows](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)
2. Zaškrtněte políčko Povolit ověřování systému Windows. Ve výchozím nastavení je toto políčko zaškrtnuté.
3. Karta aplikace umožňuje správci konfigurovat jednu nebo více aplikací pro ověřování systému Windows.
4. Vyberte server nebo aplikaci – určete, zda je povolen server/aplikace. Klikněte na tlačítko OK.
5. Klikněte na tlačítko Přidat... .
6. Karta Důvěryhodné IP umožňuje přeskočit ověřování Azure Multi-Factor Authentication pro relace systému Windows pocházející z konkrétních IP adres. Například pokud zaměstnanci používají aplikace z kanceláře a z domova, můžete rozhodnout, že nechcete, aby jejich telefony vyzváněly pro ověřování Azure Multi-Factor Authentication, zatímco jsou v kanceláři. V takovém případě zadáte podsíť kanceláře jako položku důvěryhodných IP adres.
7. Klikněte na tlačítko Přidat... .
8. Vyberte jednu IP adresu, pokud chcete přeskočit jednu IP adresu.
9. Vyberte rozsah IP adres, pokud chcete přeskočit celý rozsah IP adres. Příklad 10.63.193.1–10.63.193.100.
10. Pokud chcete zadat rozsahu IP adres pomocí zápisu podsítě, vyberte podsíť. Zadejte počáteční IP adresu podsítě a vyberte příslušnou síťovou masku z rozevíracího seznamu. 
11. Klikněte na tlačítko OK.



<!--HONumber=Aug16_HO4-->


