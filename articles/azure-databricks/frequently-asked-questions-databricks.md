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
ms.openlocfilehash: 6bb542537ec713be272f7e58e0b247763214ef4a
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/20/2017
---
# <a name="azure-databricks-preview-common-questions-and-help"></a>Azure Databricks ve verzi Preview: Běžné otázky a pomoc

Tento článek obsahuje seznam horní, které se týkají dotazy, které byste mohli Azure Databricks. Jsou také uvedeny některé běžné problémy, které se mohou vyskytnout při používání Azure Databricks. Další informace o Azure Databricks najdete v tématu [co je Azure Databricks?](what-is-azure-databricks.md) 

## <a name="common-questions"></a>Časté dotazy

V této části jsou uvedeny běžné otázky související s Azure Databricks.

### <a name="can-i-use-my-own-keys-for-local-encryption"></a>Můžete použít vlastní klíče pro místní šifrování? 
Použití vlastní klíče z Azure Keyvault není podporováno v aktuální verzi. 

### <a name="can-i-use-azure-vnets-with-azure-databricks"></a>Můžete použít virtuální sítě Azure s Azure Databricks?
Nové virtuální sítě se vytvoří v rámci Azure Databricks zřizování. V rámci této verze nemůžete použít své vlastní virtuální síť Azure.

### <a name="how-do-i-access-azure-data-lake-store-from-a-notebook"></a>Přístupu Azure Data Lake Store z Poznámkový blok 

1. V Azure Active Directory zřídit hlavní název služby a zaznamenejte svůj klíč.
2. Přiřaďte potřebná oprávnění k objektu služby v Azure Data Lake Store.
3. Získat přístup k souboru v Azure Data Lake Store, použijte přihlašovací údaje objektu služby v poznámkovém bloku.

Další informace najdete v tématu [použití Data Lake Store s Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html#azure-data-lake-store).

## <a name="troubleshooting"></a>Řešení potíží

Tato část popisuje postupy řešení běžných problémů s Azure Databricks.

### <a name="issue-this-subscription-is-not-registered-to-use-the-namespace-microsoftdatabricks"></a>Problém: Toto předplatné není zaregistrované používání oboru názvů 'Microsoft.Databricks.

**Chybová zpráva**

Toto předplatné není zaregistrované používání oboru názvů 'Microsoft.Databricks'. V tématu https://aka.ms/rps-not-found pro postup registrace odběry. (Kód: MissingSubscriptionRegistration)

**Řešení**

1. Přejděte na [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **odběry**, předplatné a pak klikněte na tlačítko **zprostředkovatelé prostředků**. 
3. V seznamu poskytovatelů prostředků proti **Microsoft.Databricks**, klikněte na tlačítko **zaregistrovat**. Role Přispěvatel nebo vlastníka musí mít v předplatném registrace poskytovatele prostředků.


### <a name="issue-your-account-email-does-not-have-owner-or-contributor-role-on-the-databricks-workspace-resource-in-the-azure-portal"></a>Problém: Účtu {e-mailu} nemá roli vlastníka nebo přispěvatele na prostředku prostoru Databricks na portálu Azure.

**Chybová zpráva**

Váš účet {e-mailu} nemá roli vlastníka nebo přispěvatele na prostředku prostoru Databricks na portálu Azure. Tato chyba může vyskytnout, pokud se uživatel guest v klientovi. Požádejte správce o udělení přístupu nebo můžete přidat jako uživatel přímo v pracovním prostoru Databricks. 

**Řešení**

Toto je několik řešení tohoto problému:

* Inicializace klienta, musíte být přihlášení jako běžný uživatel klienta není uživatel typu Host. Role Přispěvatel musí mít také na prostředku prostoru Databricks. Můžete udělit přístup uživatelů z **přístup k ovládacímu prvku (IAM)** kartě v rámci pracovního prostoru Azure Databricks na portálu Azure.

* Tato chyba může taky dojít, pokud název domény e-mailu je přiřazen k více Active Directory. Chcete-li tento problém obejít, vytvořte nového uživatele ve službě Active Directory obsahující předplatné s pracovního prostoru Databricks.

    a. V portálu Azure přejděte do služby Azure Active Directory, klikněte na tlačítko **uživatelů a skupin**, klikněte na tlačítko **přidat uživatele**.

    b. Přidat uživatele, který má `@<tenant_name>.onmicrosoft.com` e-mailu místo @< doména > e-mailu. Můžete najít < Název_klíče >. onmicrosoft.com, které jsou přidružené k vaší služby Active Directory v **vlastní domény** v rámci Azure Active Directory na portálu Azure.
    
    c. Udělení tohoto nového uživatele **Přispěvatel** role na prostředku prostoru Databricks.
    
    d. Přihlaste se k portálu Azure pomocí nového uživatele a najít Databricks prostoru.
    
    e. Spusťte prostoru Databricks jako tohoto uživatele.


### <a name="issue-your-account-email-has-not-been-registered-in-databricks"></a>Problém: Účtu {e-mailu} nebyl zaregistrován v Databricks 

**Řešení**

Pokud jste nevytvořili pracovním prostoru a jsou přidány jako uživatel pracovního prostoru, kontaktování dané osoby, vytvořené v pracovním prostoru můžete pomocí konzoly pro správu Azure Databricks přidat. Pokyny najdete v tématu [přidání a Správa uživatelů](https://docs.azuredatabricks.net/administration-guide/admin-settings/users.html). Pokud jste vytvořili pracovní prostor a stále se tato chyba, zkuste to znovu kliknutím na "Inicializovat prostoru" z portálu Azure.

### <a name="issue-cloud-provider-launch-failure-publicipcountlimitreached-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problém: Cloudu spusťte selhání poskytovatele (PublicIPCountLimitReached): došlo k chybě zprostředkovatele cloudu při nastavení clusteru

**Chybová zpráva**

Selhání spuštění poskytovatele cloudu: Při nastavení clusteru došlo k chybě poskytovatele cloudu. Naleznete v Průvodci Databricks Další informace. Kód chyby Azure: PublicIPCountLimitReached. Chybová zpráva Azure: Nelze vytvořit více než 60 veřejné IP adresy pro toto předplatné v této oblasti.

**Řešení**

Azure Databricks clustery použít jednu veřejnou IP adresu na jeden uzel. Pokud vaše předplatné již použit všechny jeho veřejné IP adresy, měli byste [žádost o zvýšení kvóty](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request). Zvolte **kvóty** jako **typ problému**, **sítě: ARM** jako **typ kvóty**a požádat o zvýšení kvóty veřejnou IP adresu v  **Podrobnosti o**. Například pokud limit je aktuálně 60 a chcete vytvořit 100 uzly clusteru, požádat o zvýšení limitu na 160.

### <a name="issue-cloud-provider-launch-failure-missingsubscriptionregistration-a-cloud-provider-error-was-encountered-while-setting-up-the-cluster"></a>Problém: Cloudu spusťte selhání poskytovatele (MissingSubscriptionRegistration): došlo k chybě zprostředkovatele cloudu při nastavení clusteru

**Chybová zpráva**

Selhání spuštění poskytovatele cloudu: Při nastavení clusteru došlo k chybě poskytovatele cloudu. Naleznete v Průvodci Databricks Další informace.
Kód chyby Azure: MissingSubscriptionRegistration Azure chybová zpráva: předplatné není zaregistrované používání oboru názvů 'Microsoft.Compute'. V tématu https://aka.ms/rps-not-found pro postup registrace odběrů

**Řešení**

1. Přejděte na [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **odběry**, předplatné a pak klikněte na tlačítko **zprostředkovatelé prostředků**. 
3. V seznamu poskytovatelů prostředků proti **Microsoft.Compute**, klikněte na tlačítko **zaregistrovat**. Role Přispěvatel nebo vlastníka musí mít v předplatném registrace poskytovatele prostředků.

V tématu [zprostředkovatelé prostředků a typy](../azure-resource-manager/resource-manager-supported-services.md) podrobnější pokyny.

## <a name="next-steps"></a>Další kroky
Podrobné pokyny pro vytváření dat verze 2 najdete v následujících kurzech:

- [Rychlý úvod: Začínáme s Azure Databricks](quickstart-create-databricks-workspace-portal.md)
- [Co je Azure Databricks?](what-is-azure-databricks.md)

