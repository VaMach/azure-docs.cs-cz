---
title: "Vypršení platnosti pro Office 365 skupin v Azure Active Directory | Microsoft Docs"
description: "Jak nastavit vypršení platnosti pro skupiny Office 365 ve službě Azure Active Directory (preview)"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 6b454ed7257e8d3f91e585cee2b559c54371fb15
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="configure-expiration-for-office-365-groups-preview"></a>Nakonfigurovat vypršení platnosti pro skupiny Office 365 (preview)

Teď můžete spravovat životní cyklus skupiny Office 365 nastavením vypršení platnosti funkce pro ně. V Azure Active Directory (Azure AD) můžete nastavit dobu platnosti pro pouze skupiny Office 365. Jakmile jednou nastavíte skupinu vyprší:
-   Vlastníci skupiny jsou upozorněni obnovit skupinu jako doba vypršení platnosti přiblíží
-   Je-li odstranit všechny skupiny, který není obnoven
-   Všechny skupiny Office 365, který je odstraněn může být obnovena do 30 dní, podle vlastníků skupiny nebo správce

> [!NOTE]
> Nastavení vypršení platnosti pro skupiny Office 365 vyžaduje licenci Azure AD Premium pro všechny členy skupiny, u které se použijí nastavení vypršení platnosti.

Informace o tom, jak stáhnout a nainstalovat rutiny Azure AD PowerShell najdete v tématu [Azure Active Directory PowerShell pro grafu – veřejné verze Preview 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

## <a name="set-group-expiration"></a>Sada skupiny vypršení platnosti

1. Otevřete [centra pro správu Azure AD](https://aad.portal.azure.com) pomocí účtu, který je globálním správcem v klientovi služby Azure AD.

2. Otevřete Azure AD, vyberte **uživatelů a skupin**.

3. Vyberte **nastavení skupiny** a pak vyberte **vypršení platnosti** otevře se nastavení vypršení platnosti.
  
  ![Okno vypršení platnosti](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. Na **vypršení platnosti** okno, můžete:

  * Nastavení doby platnosti skupiny ve dnech. Můžete třeba vybrat jednu z přednastavení hodnoty, nebo vlastní hodnota (by měl být 31 dnů nebo déle). 
  * Zadejte e-mailovou adresu, kde odeslat oznámení vypršení platnosti a obnovení v případě, že skupina má bez vlastníka. 
  * Vyberte skupiny Office 365, které vyprší. Můžete povolit vypršení platnosti pro **všechny** skupiny Office 365, můžete vybrat z skupiny Office 365, nebo můžete vybrat **žádné** zakázat vypršení platnosti pro všechny skupiny.
  * Uložit nastavení, když jste hotovi výběrem **Uložit**.


E-mailová oznámení, jako je tato jsou odesílány vlastníků skupiny Office 365 30 dní, 15 dní a 1 den před vypršením platnosti skupiny.

![Vypršení platnosti e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

Vlastník skupiny pak můžete vybrat **obnovit skupinu** obnovit Office 365 skupinu, nebo můžete vybrat **přejděte do skupiny** zobrazíte členů a další podrobnosti o této skupině.

Když vyprší platnost skupinu, je skupinu odstranit jeden den po datu vypršení platnosti. E-mailové oznámení, jako je tato posílá informace o vypršení platnosti a následné mazání jejich skupiny Office 365 vlastníci skupiny Office 365.

![Skupiny odstranění e-mailových oznámení](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

Skupinu lze obnovit tak, že vyberete **obnovení skupiny** nebo pomocí rutin prostředí PowerShell, jak je popsáno v [obnovení odstraněné Office 365 skupiny ve službě Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).
    
Pokud na skupinu, kterou jste obnovení obsahuje dokumenty, weby Sharepointu nebo jiné trvalé objekty, může trvat až 24 hodin plně obnovit skupiny a její obsah.

> [!NOTE]
> * Při prvním nastavení vypršení platnosti všech skupin, které jsou starší než interval vypršení platnosti jsou nastaveny na 30 dní do vypršení platnosti. První e-mail s oznámením obnovení odeslání během dne. 
>   Například skupiny A byl vytvořen před 400 dny a interval vypršení platnosti je nastaven na 180 dní. Při aplikování nastavení vypršení platnosti, skupiny A má 30 dní, než je odstraní, pokud vlastník obnovuje ho.
> * Když dynamická skupina je odstranit a obnovit, je považovat za novou skupinu a znovu vyplněna podle pravidla. Tento proces může trvat až 24 hodin.

## <a name="next-steps"></a>Další postup
Tyto články poskytují další informace o skupinách Azure AD.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
