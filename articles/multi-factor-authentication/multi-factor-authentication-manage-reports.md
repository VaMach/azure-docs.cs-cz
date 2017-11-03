---
title: "Přístup a použití sestav pro Azure MFA | Microsoft Docs"
description: "Popisuje jak používat funkci Azure Multi-Factor Authentication - sestavy."
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: 3f6b33c4-04c8-47d4-aecb-aa39a61c4189
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: joflore
ms.reviewer: alexwe
ms.openlocfilehash: 77d6742faadfaf3d7afccfbe888b910c80278737
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Sestavy v Azure Multi-Factor Authentication

Azure Multi-Factor Authentication nabízí několik sestav, které mohou být využívána vám a vaší organizaci. Tyto sestavy lze přistupovat prostřednictvím portálu služby Multi-Factor Authentication Management Portal. V následující tabulce jsou uvedeny dostupné sestavy:

| Sestava | Popis |
|:--- |:--- |
| Využití |Využití zprávy obsahují informace na celkové využití: uživatelský souhrn a podrobnosti o uživateli. |
| Stav serveru |Tato sestava zobrazí stav aplikace Multi-Factor Authentication Server, která je spojená s vaším účtem. |
| Historie blokování uživatelů |Tyto sestavy zobrazit historii žádostí o blokování nebo odblokování uživatelů. |
| Historie přeskočených uživatelů |Zobrazí historii žádostí o obejití služby Multi-Factor Authentication pro telefonní číslo uživatele. |
| Upozornění na podvod |Zobrazí historii upozornění na podvod odeslaných během období, které jste zadali. |
| Ve frontě |Zobrazí sestavy zařazené do fronty pro zpracování a jejich stav. Po dokončení sestavy je k dispozici odkaz na stažení nebo zobrazení sestavy. |

## <a name="view-reports"></a>Zobrazení sestav

1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost Active Directory.
3. Proveďte jeden z těchto dvou možností, v závislosti na tom, zda používáte zprostředkovatelé ověřování:
   * **Možnost 1**: klikněte na kartu zprostředkovatelé vícefaktorového ověřování. Vyberte poskytovatele MFA a klikněte na tlačítko **spravovat** tlačítko dole.
   * **Možnost 2**: Vyberte adresář, přejděte na **konfigurace** kartě. V části ověřování vícefaktorového ověřování vyberte **Spravovat nastavení služby**. V dolní části stránky nastavení vícefaktorového ověřování služby kliknutím na Přejít do portálu odkaz.
4. V portálu Azure Multi-Factor Authentication Management Portal, vyberte typ sestavy, které chcete z **zobrazit sestavu** část v levém navigačním panelu.

<center>![Cloud](./media/multi-factor-authentication-manage-reports/report.png)</center>

## <a name="powershell-reporting"></a>Vytváření sestav prostředí PowerShell

Identifikujte uživatele, kteří mají zaregistrovaný pro MFA pomocí prostředí Powershell, který následuje.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifikujte uživatele, kteří se ještě nezaregistrovali pro MFA pomocí prostředí Powershell, který následuje dále.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

**Další zdroje**

* [Pro uživatele](end-user/multi-factor-authentication-end-user.md)
* [Azure Multi-Factor Authentication na webu MSDN](https://msdn.microsoft.com/library/azure/dn249471.aspx)
