---
title: "Problém s vytvořením aplikace Proxy aplikace | Microsoft Docs"
description: "Řešení potíží při vytváření aplikací pro Proxy aplikace v portálu pro správu Azure AD"
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
ms.openlocfilehash: fe56f56162ba7186f1b660a5b44fcef38f1dee9d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
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
