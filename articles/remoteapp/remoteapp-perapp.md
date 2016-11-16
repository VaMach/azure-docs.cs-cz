---
title: "Publikování aplikací pro jednotlivé uživatele v kolekci Azure RemoteAppu (verze Preview) | Dokumentace Microsoftu"
description: "Přečtěte si, jak v Azure RemoteAppu publikovat aplikace místo skupin pro jednotlivé uživatele."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 08/15/2016
ms.author: piotrci
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 07cadd332edc4c55d87ca76aabeaba824d1e2673


---
# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Publikování aplikací pro jednotlivé uživatele v kolekci Azure RemoteAppu (verze Preview)
> [!IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Tento článek vysvětluje, jak publikovat aplikace pro jednotlivé uživatele v kolekci Azure RemoteAppu. Toto je nová funkce v Azure RemoteAppu, která je aktuálně ve verzi Preview a je k dispozici pouze vybraným uživatelům se zájmem o nové funkce pro účely vyhodnocení.

Původně byl v Azure RemoteAppu povolen pouze jeden způsob „publikování“ aplikací: správce publikoval aplikace z image a ty se pak staly viditelnými pro všechny uživatele v kolekci.

Obvyklým scénářem je zahrnout mnoho aplikací do jediného image a nasadit jednu kolekci, aby byly náklady na správu minimální. Často ale nejsou všechny aplikace relevantní pro všechny uživatele a správci by raději publikovali aplikace pro jednotlivé uživatele, aby ti pak ve svém kanálu aplikací neviděli aplikace, které nepotřebují.

A právě to je nyní v Azure RemoteAppu možné – aktuálně jako omezeně dostupná funkce ve verzi Preview. Zde je stručný popis nové funkce:

1. Kolekce lze nastavit do jednoho ze dvou režimů:
   
   * Původní „režim kolekce“, ve kterém všichni uživatelé v kolekci vidí všechny publikované aplikace. To je výchozí režim.
   * Nový „režim aplikací“, ve kterém uživatelé vidí pouze aplikace, které jim byly explicitně přiřazeny.
2. V současnosti lze režim aplikací povolit pouze pomocí rutin prostředí Azure RemoteApp PowerShell.
   
   * Když je nastavený režim aplikací, přiřazení uživatelů v kolekci nelze spravovat prostřednictvím portálu Azure. Přiřazení uživatelů se musí spravovat pomocí rutin prostředí PowerShell.
3. Uživatelům se zobrazí pouze aplikace, které byly publikovány přímo jim. Uživatel však stále může spustit i další aplikace, které jsou dostupné v imagi, když k nim bude přistupovat přímo přes operační systém.
   
   * Tato funkce nenabízí zabezpečené uzamčení aplikací. Omezuje pouze jejich viditelnost v kanálu aplikací.
   * Pokud potřebujete uživatele od určitých aplikací izolovat, musíte pro tento účel použít samostatné kolekce.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Jak získat rutiny prostředí Azure RemoteApp PowerShell
Pokud chcete vyzkoušet tuto novou funkci ve verzi Preview, budete muset použít rutiny prostředí Azure PowerShell. Nový režim publikování aplikací aktuálně není možné povolit pomocí portálu pro správu Azure.

Nejprve zkontrolujte, zda máte nainstalovaný [modul Azure PowerShell](../powershell-install-configure.md).

Potom spusťte konzolu PowerShellu v režimu správce a spusťte následující rutinu:

        Add-AzureAccount

Zobrazí se výzva k zadání vašeho uživatelského jména a hesla pro Azure. Jakmile se přihlásíte, budete moci spouštět rutiny Azure RemoteAppu pro svá předplatná Azure.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Jak zjistit, ve kterém režimu kolekce je
Spusťte následující rutinu:

        Get-AzureRemoteAppCollection <collectionName>

![Zkontrolujte režim kolekce](./media/remoteapp-perapp/araacllelvel.png)

Vlastnost AclLevel může mít následující hodnoty:

* Collection: původní režim publikování. Všichni uživatelé vidí všechny publikované aplikace.
* Application: nový režim publikování. Uživatelé vidí pouze aplikace, které byly publikovány přímo pro ně.

## <a name="how-to-switch-to-application-publishing-mode"></a>Jak přepnout na režim publikování aplikací
Spusťte následující rutinu:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Stav publikování jednotlivých aplikací bude zachován: na počátku všichni uživatelé uvidí všechny původně publikované aplikace.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Jak zobrazit seznam uživatelů, kteří mohou vidět konkrétní aplikaci
Spusťte následující rutinu:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Rutina vypíše seznam všech uživatelů, kteří mohou vidět danou aplikaci.

Poznámka: Aliasy aplikací (v syntaxi výše „app alias“) si můžete zobrazit spuštěním příkazu Get-AzureRemoteAppProgram -NázevKolekce <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Jak přiřadit aplikaci uživateli
Spusťte následující rutinu:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Uživatel nyní uvidí aplikaci v klientovi Azure RemoteAppu a bude se k ní moci připojit.

## <a name="how-to-remove-an-application-from-a-user"></a>Jak odebrat aplikaci uživateli
Spusťte následující rutinu:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Poskytnutí zpětné vazby
Oceníme vaše názory a návrhy týkající se této funkce ve verzi Preview. Vyplňte prosím náš [průzkum](http://www.instant.ly/s/FDdrb) a dejte nám vědět, co si myslíte.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Neměli jste možnost vyzkoušet si funkci ve verzi Preview?
Pokud jste se dosud neúčastnili žádného vyhodnocování verzí Preview, použijte prosím tento [průzkum](http://www.instant.ly/s/AY83p) a požádejte o přístup.




<!--HONumber=Nov16_HO2-->


