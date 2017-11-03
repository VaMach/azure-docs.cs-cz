---
title: "Pomocí privilegované koncový bod v zásobníku Azure | Microsoft Docs"
description: "Ukazuje, jak používat privilegované koncový bod v zásobníku Azure (pro operátor zásobník Azure)."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: twooley
ms.openlocfilehash: 9769b12064216680bb1b2db8c1fd7449927c7771
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Pomocí privilegované koncový bod v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Jako operátor zásobník Azure měli byste použít správce portálu, prostředí PowerShell nebo rozhraní API Správce Azure Resource Manageru pro nejvíce každodenní úlohy správy. Ale pro některé méně běžné operace, budete muset použít *privilegované koncový bod*. Tento koncový bod je předem nakonfigurovaný vzdálené konzoly prostředí PowerShell, který poskytuje akorát, funkce, které vám pomohou provést požadované úlohy. Koncový bod využívá prostředí PowerShell JEA (právě dostatečně správy) ke zveřejnění jenom omezenou sadu rutin. Pro přístup k privilegované koncový bod a vyvolání omezenou sadu rutin, se používá účet s nízkou úrovní oprávnění. Jsou vyžadovány žádné účty správce. Pro dodatečné zabezpečení není povoleno skriptování.

Privilegované koncového bodu můžete použít k provedení následujících úloh:

- Úkoly nízké úrovně, jako například [shromažďování diagnostických protokolů](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- K provádění úloh po nasazení datacenter integrace pro integrované systémy, jako je například přidávání serverů pro předávání systému DNS (Domain Name) po nasazení nastavení integrace grafu, integrace služby Active Directory Federation Services (AD FS) certifikátu otočení atd.
- Pro práci s podporou získat dočasný vysoké úrovně přístup pro odstraňování potíží integrovaného systému. 

Privilegované koncový bod zaznamená každou akci (a její výstup. odpovídající), které můžete provádět v relaci prostředí PowerShell. To poskytuje plnou průhlednost a dokončení auditování operací. Tyto soubory protokolu pro budoucí auditování můžete zachovat.

> [!NOTE]
> V Azure zásobníku Development Kit (ASDK), můžete spustit některé příkazy, které jsou k dispozici v koncovém bodě privilegované přímo z relace prostředí PowerShell na hostiteli development kit. Můžete však otestování některé operace pomocí privilegované koncový bod, jako je například kolekce protokolu, protože to je jedinou metodou dostupnou k provádění některých operací v prostředí, integrované systémy.

## <a name="access-the-privileged-endpoint"></a>Přístup k privilegované koncový bod

Privilegované koncového bodu přistupujete prostřednictvím vzdálené relace prostředí PowerShell na virtuálním počítači, který je hostitelem privilegované koncový bod. V ASDK je tento virtuální počítač s názvem AzS ERCS01. Pokud používáte integrovaný systém, existují tři instance privilegované koncového bodu, každý běžících v rámci virtuálního počítače (*předpony*-ERCS01, *předpony*-ERCS02, nebo *předpony*-ERCS03) na různých hostitelích pro odolnost proti chybám. 

Před zahájením tohoto postupu pro integrovaný systém, ujistěte se, že dostanete koncový bod privilegované podle IP adresy nebo přes DNS. Po počátečním nasazení Azure zásobníku mít přístup privilegované koncový bod jen pomocí IP adresy, protože integrace DNS se ještě není nastavený. Dodavatele hardwaru, od výrobců OEM můžete poskytnout soubor JSON s názvem "AzureStackStampDeploymentInfo", který obsahuje IP adresy privilegované koncový bod.

Doporučujeme vám, že se připojit k privilegované koncový bod pouze z hostitele životního cyklu hardwaru nebo z počítače vyhrazené, uzamčení dolů, jako je třeba [pracovní stanice privilegovaného přístupu](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. Proveďte jednu z následujících akcí v závislosti na vašem prostředí:

    - Integrovaný systém spusťte následující příkaz k přidání privilegované koncového bodu jako důvěryhodného hostitele na hardwaru životního cyklu hostitele nebo pracovní stanice privilegovaný přístup.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - Pokud spouštíte ADSK, přihlaste se k hostiteli development kit.

2. Na hostitele životního cyklu hardwaru nebo privilegovaného přístupu pracovní stanice otevřete relaci prostředí Windows PowerShell zvýšenými oprávněními. Spusťte následující příkazy k vytvoření vzdálené relace na virtuálním počítači, který je hostitelem privilegované koncový bod:
 
    - Integrované systému:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      `ComputerName` Parametr může být IP adresa nebo název DNS jednoho z virtuálních počítačů, které hostuje privilegované koncový bod. 
    - Pokud spouštíte ADSK:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   Pokud budete vyzváni, použijte následující pověření:

      - **Uživatelské jméno**: Zadejte účet CloudAdmin ve formátu  **&lt;* zásobník Azure domény*&gt;\cloudadmin**. (Pro ASDK, uživatelské jméno je **azurestack\cloudadmin**.)
      - **Heslo**: Zadejte stejné heslo, které jste zadali během instalace pro účet správce domény AzureStackAdmin.
    
3.  Když se připojíte, řádku se změní na  **[*ERCS virtuálního počítače nebo IP adresu název*]: PS > ** nebo **[azs-ercs01]: PS >**, v závislosti na prostředí. Odtud spustit `Get-Command` zobrazíte seznam dostupných rutin.

    ![Rutina Get-Command výstup zobrazuje seznam dostupných příkazů](media/azure-stack-privileged-endpoint/getcommandoutput.png)

    Řadu tyto rutiny jsou určena pouze pro prostředí integrovaný systém (například rutiny související s integrací datacenter). V ASDK ověření následující rutiny:

    - Clear hostitel
    - Zavřít PrivilegedEndpoint
    - Ukončení PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Get – příkaz
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Objekt míry
    - Nové CloudAdminUser
    - Odchozí výchozí
    - Odebrat CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Stop-AzureStack
    - Get-cluster, protokol

4.  Protože skriptování není povolená, nemůžete použít dokončování pomocí tabulátorů hodnoty parametrů. Chcete-li získat seznam parametrů pro danou rutinu, spusťte následující příkaz:

    ````PowerShell
    Get-Command <cmdlet_name> -Syntax
    ```` 
    Pokud se pokusíte žádný druh operace skriptu, operace selže s chybou **ScriptsNotAllowed**. Toto je očekávané chování.

## <a name="close-the-privileged-endpoint-session"></a>Ukončení relace privilegované koncový bod

 Jak už bylo zmíněno dříve, zaznamená privilegované koncový bod každou akci (a její výstup. odpovídající), které můžete provádět v relaci prostředí PowerShell. Zavřete relaci pomocí `Close-PrivilegedEndpoint` rutiny. Tato rutina správně koncový bod se zavře a přenese soubory protokolů do externí sdílené složky pro uchovávání informací.

K ukončení relace koncového bodu:

1. Vytvoření externí sdílené složky, která je přístupná pomocí privilegované koncový bod. Ve vývojovém prostředí sady právě vytvoříte sdílenou složku na hostiteli development kit.
2. Spustit `Close-PrivilegedEndpoint` rutiny. 
3. Se zobrazí výzva k zadání cesty na které chcete uložit soubor protokolu přepis. Zadejte sdílené složky, kterou jste vytvořili dříve, ve formátu &#92; &#92; *servername*&#92; *ShareName*. Pokud nezadáte cestu, rutina selže a relace zůstane otevřená. 

    ![Zavřít PrivilegedEndpoint výstupu rutiny, který ukazuje, kde zadáte cílovou cestu přepis](media/azure-stack-privileged-endpoint/closeendpoint.png)

Po přepis protokolové soubory jsou úspěšně přenést do sdílené složky, se automaticky odstraní z privilegovaných koncového bodu. Pokud zavřete privilegované koncový bod relace pomocí rutin `Exit-PSSession` nebo `Exit`, nebo jenom zavřete konzolu prostředí PowerShell, protokoly přepis nemáte přenést do sdílené složky. Zůstanou v privilegované koncový bod. Při příštím spuštění `Close-PrivilegedEndpoint` a zahrnují sdílené složky, přepis protokoly z předchozí relací se také přenosu.

## <a name="next-steps"></a>Další kroky
[Azure zásobníku diagnostické nástroje](azure-stack-diagnostics.md)







