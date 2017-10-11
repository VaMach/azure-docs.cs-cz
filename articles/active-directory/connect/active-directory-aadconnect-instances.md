---
title: "Azure AD Connect: Instance služby synchronizace | Microsoft Docs"
description: "Tato stránka dokumenty zvláštní upozornění pro instancí služby Azure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e8321c3d16253226a5931cacbce6fa5d50b697bd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Zvláštní upozornění pro instance
Azure AD Connect se nejčastěji používá s celosvětové instance služby Azure AD a Office 365. Ale existují také další instance a ty mají různé požadavky na adresy URL a další důležité.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Germany
[Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) je svrchovaných Cloudová provozují zplnomocněnec němčině data.

| Adresy URL otevřete v proxy serveru |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Seznamy odvolaných certifikátů |

Když se přihlásíte ke klientovi Azure AD, musíte použít účet v doméně onmicrosoft.de.

Funkce aktuálně nejsou k dispozici v Německu cloudu společnosti Microsoft:

* **Azure AD Connect Health** není k dispozici.
* **Funkce Automatické aktualizace** není k dispozici.
* **Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.
* Nejsou k dispozici další služby Azure AD Premium.

## <a name="microsoft-azure-government-cloud"></a>Cloudu Microsoft Azure Government.
[Cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/) je cloud pro US government.

Tento cloud byl podporován ve starších verzích nástroje DirSync. Ze sestavení 1.1.180 služby Azure AD Connect je podporována nové generace cloudu. Generování používá jen USA založené na koncových bodů a mít jiný seznam adres URL, otevřete v proxy serveru.

| Adresy URL otevřete v proxy serveru |
| --- |
| \*.microsoftonline.com |
| \*. microsoftonline.us |
| \*. gov.us.microsoftonline.com |
| + Seznamy odvolaných certifikátů |

Azure AD Connect není schopna automaticky zjistit, že klientovi Azure AD je umístěný v cloudu Government. Místo toho musíte provést následující akce při instalaci Azure AD Connect.

1. Spusťte instalaci Azure AD Connect.
2. Když se zobrazí na první stránku, kde je mají přijetí smlouvy EULA, nebudete pokračovat, ale ponechte spuštění Průvodce instalací.
3. Spustit program regedit a změňte klíč registru `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` na hodnotu `2`.
4. Přejděte zpět na Průvodce instalací služby Azure AD Connect, přijetí smlouvy EULA a pokračovat. Během instalace, nezapomeňte použít **vlastní konfigurace** cesta instalace (a ne expresní instalaci). Potom pokračujte v instalaci jako obvykle.

Aktuálně nejsou k dispozici v cloudu Microsoft Azure Government funkce:

* **Azure AD Connect Health** není k dispozici.
* **Funkce Automatické aktualizace** není k dispozici.
* **Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.
* Nejsou k dispozici další služby Azure AD Premium.

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
