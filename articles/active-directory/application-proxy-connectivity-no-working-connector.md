---
title: "Pro aplikaci Proxy aplikace nalezeny žádná skupina konektor pracovní | Microsoft Docs"
description: "Řešení problémů, které se můžete setkat, když není žádný funkční konektor ve skupině pro vaši aplikaci s Azure AD Application Proxy Connector"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4945958deedc8a1d9989ff901192c03a5363b4dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Žádná pracovní skupina konektor najít pro aplikaci Proxy aplikace

Tento článek nápovědy, můžete použít k řešení běžných problémů potýkají při konektor pro aplikaci Proxy aplikace zjištěn není integrované s Azure Active Directory.

## <a name="overview-of-steps"></a>Přehled kroků
Pokud není žádný funkční konektor ve skupině konektor pro vaši aplikaci, existuje několik způsobů, jak problém vyřešit:

-   Pokud máte žádné konektory ve skupině, můžete:

    -   Stáhněte si nový konektor na pravém místního serveru a přiřaďte ho do této skupiny

    -   Konektor služby active přesuňte do skupiny

-   Pokud máte žádné aktivní konektory ve skupině, můžete:

    -   Z důvodu, kterou vaše konektor je neaktivní identifikovat a vyřešit

    -   Konektor služby active přesuňte do skupiny

Potřebujete vědět, který z nich je problém, otevřete nabídku "Proxy aplikace" v aplikaci a podívejte se na zprávu upozornění konektor skupiny. Ho zadat, že skupině potřebuje alespoň jeden konektor (nemáte žádné ve skupině), nebo že nemá žádné aktivní konektory (i když máte pravděpodobně neaktivní konektory).

   ![Výběr skupiny konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Podrobnosti o každá z těchto možností najdete v části odpovídající níže. Každá z těchto předpokládá, že začínáte na stránce Správa konektor. Pokud hledáte na výše uvedené chybová zpráva, můžete přejít na tuto stránku kliknutím na upozornění. V opačném případě lze najít na webu **Azure Active Directory**, kliknutím na na **podnikové aplikace, které**, pak **Proxy aplikace.**

   ![Správa skupin konektor portálu Azure](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Stáhněte si nový konektor

Pokud chcete stáhnout nový konektor, použijte tlačítko "Stáhnout konektor" v horní části stránky.

Poznámka: tento konektor musí být nainstalovaný na počítači s přímé směrem pohledu na back-end aplikace a je obvykle umístěny na stejném serveru jako aplikace. Po stažení, konektor objevit v této nabídky. Klikněte na konektoru a ujistěte se, že patří do správné skupiny pomocí "Konektor skupinu" rozevíracího seznamu. Uložte změnu.

   ![Stáhnout konektor z portálu Azure](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Přesunout konektoru služby Active

Pokud máte aktivní konektor, musí patřit do skupiny a má směrem pohledu do cílové back-end aplikace, můžete konektor přesunout do skupiny přiřazené. Chcete-li to provést, klikněte na tlačítko konektor. V poli "Konektor skupina" pomocí rozevíracího seznamu vyberte správné skupině, a klikněte na Uložit.

## <a name="resolve-an-inactive-connector"></a>Vyřešte neaktivní konektoru

Pokud se pouze konektory ve skupině jsou neaktivní, jsou pravděpodobně na počítači, který nemá všechny nezbytné porty odblokováno.

na odstranění příčin tohoto problému naleznete v dokumentu Poradce při potížích porty podrobnosti.

## <a name="next-steps"></a>Další kroky
[Pochopení konektory proxy aplikace služby Azure AD](application-proxy-understand-connectors.md)


