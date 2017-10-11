---
title: "Použití certifikátů s Enterprise integračního balíčku | Microsoft Docs"
description: "Další informace o použití certifikátů s Enterprise integračního balíčku | Azure Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0570aab14283b38f9efcc50636f0c0c1c8e3ed13
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Další informace o certifikátech a řešení Enterprise Integration Pack
## <a name="overview"></a>Přehled
Integrace Enterprise používá certifikáty k zabezpečení komunikace B2B. Můžete provádět dva typy certifikátů ve vaší podnikové integrace aplikace:

* Veřejné certifikáty, které je třeba zakoupit od certifikační autority (CA).
* Privátní certifikáty, které můžete zadat sami. Tyto certifikáty se někdy označují jako certifikáty podepsané svým držitelem.

## <a name="what-are-certificates"></a>Co jsou certifikáty?
Certifikáty jsou digitální dokumenty, ověření identity účastníky v elektronické komunikace a která taky zabezpečené elektronické komunikace.

## <a name="why-use-certificates"></a>Proč používat certifikáty?
Někdy komunikace B2B musí být důvěrné. Integrace Enterprise používá certifikáty k zabezpečení této komunikace dvěma způsoby:

* Šifrování obsahu zpráv
* Podle digitálnímu podepisování zpráv  

## <a name="upload-a-public-certificate"></a>Nahrát veřejného certifikátu

Použít *veřejný certifikát* ve vašich logic apps možnosti B2B, musíte nejprve nahrát na server certifikát ke svému účtu integrace.  

Po odeslání certifikát, je k dispozici a pomáhá vám zabezpečit zpráv B2B, když definujete jejich vlastnosti v [smlouvy](logic-apps-enterprise-integration-agreements.md) , kterou vytvoříte.  

Tady jsou podrobné kroky pro nahrávání veřejné certifikáty do účtu integrace po přihlášení k portálu Azure:

1. Vyberte **další služby** a zadejte **integrace** do vyhledávacího pole filtru. Vyberte **účty pro integraci** ze seznamu výsledků     
![Vyberte procházení](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Vyberte účet integrace, do které chcete přidat certifikát.  
![Vyberte účet integrace, do které chcete přidat certifikát](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Vyberte **certifikáty** dlaždici.  
![Vyberte dlaždici certifikáty](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. V **certifikáty** okno, které se otevře, vyberte **přidat** tlačítko.   
![Kliknutím na tlačítko Přidat](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Zadejte **název** certifikát, a pak vyberte certifikát, zadejte jako **veřejné** z rozevíracího seznamu.  
6. Vyberte ikonu složky na pravé straně **certifikát** textové pole. Po otevření nástroje pro výběr souborů, najděte a vyberte soubor certifikátu, který chcete odeslat do svého účtu integrace.
7. Vyberte certifikát a pak vyberte **OK** v nástroje pro výběr souborů. To se ověří a odešle certifikát ke svému účtu integrace.
8. Nakonec zpět na **přidat certifikát** okně, vyberte **OK** tlačítko.  
![Vyberte tlačítko OK](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Vyberte **certifikáty** dlaždici. Měli byste vidět nově přidané certifikát.  
![V tématu nový certifikát](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Nahrát privátní certifikát

Použít *privátní certifikát* ve vašich logic apps s B2B možnosti, můžete odeslat privátní certifikát do svého účtu integrace provedením následujících kroků

1. [Nahrát svůj privátní klíč do Key Vault](../key-vault/key-vault-get-started.md "Další informace o Key Vault") a poskytují **název klíče** 
   
   > [!TIP]
   > Je nutné autorizovat Logic Apps k provádění operací v Key Vault. Objekt služby Logic Apps můžete poskytnout přístup pomocí následujícího příkazu Powershellu:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Poté, co jste jste udělali v předchozím kroku, přidejte privátní certifikát k integraci účtu.

Následující části jsou podrobné kroky pro nahrávání privátní certifikáty do účtu integrace po přihlášení k portálu Azure:  
 
1. Vyberte účet integrace, do které chcete přidat certifikát a vyberte **certifikáty** dlaždici.  
![Vyberte dlaždici certifikáty](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. V **certifikáty** okno, které se otevře, vyberte **přidat** tlačítko.   
![Kliknutím na tlačítko Přidat](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Zadejte **název** certifikát, a vyberte certifikát, zadejte jako **privátní** z rozevíracího seznamu.   
4. Vyberte ikonu složky na pravé straně **certifikát** textové pole. Po otevření nástroje pro výběr souborů, najděte odpovídající veřejný certifikát, který chcete odeslat do svého účtu integrace.   
   
   > [!Note]
   > Při přidávání privátního certifikátu je potřeba přidat odpovídající veřejný certifikát, které se zobrazí v [smlouvy AS2](logic-apps-enterprise-integration-as2.md) příjem a odesílání nastavení pro podepisování a šifrování zpráv.
   > 
   >   

5. Vyberte **skupiny prostředků**, **Key Vault**, **název klíče** a vyberte **OK** tlačítko.  
![Přidání certifikátu](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Vyberte **certifikáty** dlaždici. Měli byste vidět nově přidané certifikát.
![V tématu nový certifikát](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [Vytvoření smlouvy s B2B](logic-apps-enterprise-integration-agreements.md)  
* [Další informace o Key Vault](../key-vault/key-vault-get-started.md "Další informace o Key Vault")  

