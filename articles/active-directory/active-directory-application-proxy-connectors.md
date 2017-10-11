---
title: "Klasického portálu konektory Proxy aplikace Azure AD | Microsoft Docs"
description: "Popisuje postup vytvoření a Správa skupin konektorů v Azure AD Application Proxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: fc65c4053c45d9c16c62ee0fe65924133a4bb94a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publikování aplikací na samostatných sítí a umístění pomocí konektoru skupin
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Portál Azure Classic](active-directory-application-proxy-connectors.md)
>
>

Konektor skupiny jsou užitečné pro různé scénáře, včetně:

* Weby s několik datových center vzájemně propojena. V takovém případě chcete zachovat tolik provoz v rámci datového centra nejdříve, protože datacenter křížové odkazy jsou nákladné a pomalé. Můžete nasadit konektorů v každé datové centrum, která bude sloužit pouze aplikace, které se nacházejí v datovém centru. Tento postup minimalizuje datacenter křížové odkazy a poskytuje zcela transparentní pro vaše uživatele.
* Správa aplikace nainstalované v izolovaných sítích, které nejsou součástí hlavní podnikové síti. Konektor skupiny můžete použít k instalaci vyhrazené konektory v izolovaných sítích také izolovat aplikace do sítě.
* Pro aplikace nainstalované v IaaS pro přístup do cloudu konektor skupiny poskytují běžné služby zabezpečený přístup ke všem aplikacím. Konektor skupiny nemáte vytvoření další závislosti na vaší podnikové síti nebo fragmentu aplikační prostředí. Konektory lze nainstalovat na každé cloudové datacentrum a sloužit pouze aplikace, které se nacházejí v této síti. Můžete nainstalovat několik konektorů k dosažení vysoké dostupnosti.
* Podpora pro prostředí s více doménovými strukturami ve kterých konkrétní konektory nasazení pro každou doménovou strukturu a nastavit poskytovat konkrétní aplikace.
* Konektor skupiny můžete používat v lokalitách zotavení po havárii a buď zjistit převzetí služeb při selhání nebo zálohu pro hlavní lokalitu.
* Konektor skupiny lze také sloužit více společností z jednoho klienta.

## <a name="prerequisite-create-your-connectors"></a>Předpoklad: Vytvoření vaší konektory
K seskupení konektory, [nainstalovat více konektorů](active-directory-application-proxy-enable.md), potom zadejte název a seskupovat je. Nakonec budete muset přiřadit konkrétní aplikace.

## <a name="step-1-create-connector-groups"></a>Krok 1: Vytvoření skupiny pro konektor
Můžete vytvořit libovolný počet skupin konektor. Vytvoření konektoru skupiny se provádí na portálu Azure classic.

1. Vyberte adresář a klikněte na tlačítko **konfigurace**.  
    ![Proxy aplikace nakonfigurovat snímek – kliknutím na Spravovat skupiny konektoru](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. V části Proxy aplikace, klikněte na **spravovat skupiny konektor** a vytvořte skupinu konektor tím, že název skupiny.  
    ![Snímek obrazovky skupin konektoru proxy aplikace – název nové skupiny](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Krok 2: Přiřazení do skupin konektory
Po vytvoření skupiny konektor přesunete konektory do příslušné skupiny.

1. V části **Proxy aplikace**, klikněte na tlačítko **spravovat konektory**.
2. V části **skupiny**, vyberte skupinu, do které chcete použít pro každý konektor. Konektory může trvat až 10 minut, než se stane aktivní do nové skupiny.  
    ![Snímek obrazovky konektory proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Krok 3: Přiřaďte aplikací do skupin konektoru
Posledním krokem je nastavení každou aplikaci, aby skupině konektor, který ji obsluhuje.

1. V portálu Azure classic, v adresáři, vyberte aplikaci, kterou chcete přiřadit ke skupině a klikněte na **konfigurace**.
2. V části **konektor skupiny**, vyberte skupinu, kterou má aplikace použít. Tato změna se použije okamžitě.  
    ![Snímek obrazovky skupiny konektoru proxy aplikace – vyberte skupinu z rozevírací nabídky](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Viz také
* [Povolení Proxy aplikace](active-directory-application-proxy-enable.md)
* [Povolení jednoduchého přihlášení](active-directory-application-proxy-sso-using-kcd.md)
* [Povolení podmíněného přístupu](active-directory-application-proxy-conditional-access.md)
* [Řešení problémů, které máte s pomocí Proxy aplikace](active-directory-application-proxy-troubleshoot.md)

Nejnovější novinky a aktualizace naleznete na [blogu proxy aplikace](http://blogs.technet.com/b/applicationproxyblog/)
