---
title: "Problém s vytvořením aplikace Proxy aplikace | Microsoft Docs"
description: "Řešení potíží při vytváření aplikací pro Proxy aplikace v portálu pro správu Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 5b8346ee2e02ea62b7a11b88a790cff56a7d13f4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="problem-creating-an-application-proxy-application"></a>Problém s vytvořením aplikace Proxy aplikace 

V následující tabulce jsou některé běžné problémy, uživatelé setkávají při vytváření nové aplikace proxy aplikace.

## <a name="recommended-documents"></a>Doporučené dokumenty 

Další informace o vytváření aplikací Proxy aplikace prostřednictvím portálu pro správu, najdete v části [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Pokud postupujete podle kroků v tomto dokumentu a dochází k výskytu k chybě při vytváření aplikace, najdete v části Podrobnosti o chybě informace a návrhy, jak opravit aplikaci. Většina chybové zprávy, obsahuje navrhované opravu. 

## <a name="specific-things-to-check"></a>Konkrétní co je potřeba zkontrolovat

Aby se zabránilo běžné chyby, ověřte:

-   Správci s oprávněním k vytvoření aplikace Proxy aplikace

-   Interní adresa URL je jedinečný

-   Externí adresa URL je jedinečný

-   Adresy URL začínat protokolu http nebo https a končit "/"

-   Adresa URL by měla být název domény a ne IP adresy

Chybová zpráva by měla zobrazit v pravém horním rohu při vytváření aplikace. Můžete také vybrat na ikonu oznámení naleznete v chybových zprávách.

   ![Oznámení řádku](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Další kroky
[Povolení Proxy aplikace na portálu Azure](active-directory-application-proxy-enable.md)
