---
title: "Azure Databricks: Běžné otázky a pomoc | Microsoft Docs"
description: "Získejte odpovědi na časté otázky a informace o Azure Databricks odstraňování potíží."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/17/2017
ms.author: nitinme
ms.openlocfilehash: fb77ec001f9f52e0a974f8765f458f831fb63908
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="frequently-asked-questions-about-azure-databricks"></a>Časté otázky k Azure Databricks

Tento článek obsahuje seznam nejčastějších dotazů, které vám mohou mít s Azure Databricks. Jsou také uvedeny některé běžné problémy, které může mít při používání Databricks. Další informace najdete v tématu [co je Azure Databricks](what-is-azure-databricks.md). 

## <a name="can-i-use-my-own-keys-for-local-encryption"></a>Můžete použít vlastní klíče pro místní šifrování? 
Použití vlastní klíče z Azure Key Vault není podporováno v aktuální verzi. 

## <a name="can-i-use-azure-virtual-networks-with-databricks"></a>Můžete použít virtuální sítě Azure s Databricks?
Nové virtuální sítě se vytvoří jako součást Databricks zřizování. V této verzi nemůžete použít své vlastní virtuální síti Azure.

## <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Přístupu Azure Data Lake Store z Poznámkový blok 

Postupujte následovně:
1. V Azure Active Directory (Azure AD) zřídit hlavní název služby a zaznamenejte svůj klíč.
2. Přiřadíte potřebná oprávnění instančního objektu v Data Lake Store.
3. Získat přístup k souboru v Data Lake Store, použijte přihlašovací údaje pro hlavní služby v poznámkovém bloku.

Další informace najdete v tématu [použití Data Lake Store s Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="fix-common-problems"></a>Řešení běžných potíží

Tady je několik problémů, které se můžete setkat s Databricks.

### <a name="this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Toto předplatné není zaregistrované používání oboru názvů 'Microsoft.Databricks.

#### <a name="error-message"></a>Chybová zpráva

"Toto předplatné není zaregistrované používání oboru názvů 'Microsoft.Databricks'. V tématu https://aka.ms/rps-not-found pro postup registrace odběry. (Code: MissingSubscriptionRegistration) "

#### <a name="solution"></a>Řešení

1. Přejděte na [portál Azure](https://portal.azure.com).
2. Vyberte **odběry**, předplatné, které používáte, a potom **zprostředkovatelé prostředků**. 
3. V seznamu poskytovatelů prostředků proti **Microsoft.Databricks**, vyberte **zaregistrovat**. U předplatného registrace poskytovatele prostředků musí mít roli Přispěvatel nebo vlastníka.


### <a name="your-account-email-does-not-have-the-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>{E-mailu} váš účet nemá roli vlastníka nebo přispěvatele na prostředku prostoru Databricks na portálu Azure

#### <a name="error-message"></a>Chybová zpráva

"{E-mailu} váš účet nemá roli vlastníka nebo přispěvatele na prostředku prostoru Databricks na portálu Azure. Tato chyba může vyskytnout, pokud se uživatel guest v klientovi. Požádejte správce o udělení přístupu nebo můžete přidat jako uživatel přímo v pracovním prostoru Databricks." 

#### <a name="solution"></a>Řešení

Následuje několik řešení tohoto problému:

* Inicializace klienta, musíte být přihlášeni jako běžný uživatel klienta, nikoli jako uživatel guest. Role Přispěvatel musí mít také na prostředku prostoru Databricks. Můžete udělit přístup uživatelů z **přístup k ovládacímu prvku (IAM)** kartě v rámci pracovního prostoru Databricks na portálu Azure.

* Tato chyba může také dojít, pokud název domény e-mailu je přiřazen k více adresářů ve službě Azure AD. Chcete-li tento problém obejít, vytvořte nového uživatele v adresáři, který obsahuje předplatné s pracovního prostoru Databricks.

    a. Na portálu Azure přejděte do služby Azure AD. Vyberte **uživatelů a skupin** > **přidat uživatele**.

    b. Přidat uživatele, který má `@<tenant_name>.onmicrosoft.com` e-mailu místo `@<your_domain>` e-mailu. Můžete najít v **vlastní domény**, v rámci Azure AD na portálu Azure.
    
    c. Udělení tohoto nového uživatele **Přispěvatel** role na prostředku prostoru Databricks.
    
    d. Přihlaste se k portálu Azure pomocí nového uživatele a najít Databricks prostoru.
    
    e. Spusťte prostoru Databricks jako tohoto uživatele.


### <a name="your-account-email-has-not-been-registered-in-databricks"></a>Váš účet {e-mailu} nebyl zaregistrován v Databricks 

#### <a name="solution"></a>Řešení

Pokud jste nevytvořili pracovním prostoru, a jsou přidány jako uživatel, obraťte se na osobě, která vytvořila pracovním prostoru. Máte tato osoba, můžete přidat pomocí konzoly pro správu Databricks Azure. Pokyny najdete v tématu [přidání a Správa uživatelů](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Pokud jste vytvořili pracovní prostor a stále se tato chyba, zkuste vybrat **inicializovat prostoru** znovu z portálu Azure.

### <a name="cloud-provider-launch-failure-while-setting-up-the-cluster"></a>Selhání spuštění poskytovatele cloudu při nastavení clusteru

#### <a name="error-message"></a>Chybová zpráva

"Cloudu selhání spuštění poskytovatele: došlo k chybě zprostředkovatele cloudu při nastavení clusteru. Naleznete v Průvodci Databricks Další informace. Kód chyby Azure: PublicIPCountLimitReached. Chybová zpráva Azure: Nelze vytvořit více než 60 veřejné IP adresy pro toto předplatné v této oblasti. "

#### <a name="solution"></a>Řešení

Clustery Databricks použít jednu veřejnou IP adresu na jeden uzel. Pokud vaše předplatné již použit všechny jeho veřejné IP adresy, měli byste [žádost o zvýšení kvóty](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Zvolte **kvóty** jako **vydat typ**, a **sítě: ARM** jako **typ kvóty**. V **podrobnosti**, požádat o zvýšení kvóty veřejnou IP adresu. Například pokud limit je aktuálně 60 a chcete vytvořit 100 uzly clusteru, požádat o zvýšení limitu na 160.

### <a name="a-second-type-of-cloud-provider-launch-failure-while-setting-up-the-cluster"></a>Druhý typ spuštění selhání poskytovatele cloudové při nastavení clusteru

#### <a name="error-message"></a>Chybová zpráva

"Cloudu selhání spuštění poskytovatele: došlo k chybě zprostředkovatele cloudu při nastavení clusteru. Naleznete v Průvodci Databricks Další informace.
Kód chyby Azure: MissingSubscriptionRegistration Azure chybová zpráva: předplatné není zaregistrované používání oboru názvů 'Microsoft.Compute'. Viz https://aka.ms/rps-not-found pro postup registrace odběry."

#### <a name="solution"></a>Řešení

1. Přejděte na [portál Azure](https://portal.azure.com).
2. Vyberte **odběry**, předplatné, které používáte, a potom **zprostředkovatelé prostředků**. 
3. V seznamu poskytovatelů prostředků proti **Microsoft.Compute**, vyberte **zaregistrovat**. U předplatného registrace poskytovatele prostředků musí mít roli Přispěvatel nebo vlastníka.

Další podrobné pokyny naleznete v tématu [zprostředkovatelé prostředků a typy](../azure-resource-manager/resource-manager-supported-services.md).

## <a name="next-steps"></a>Další kroky

- [Rychlý úvod: Začínáme s Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Co je Azure Databricks?](what-is-azure-databricks.md)

