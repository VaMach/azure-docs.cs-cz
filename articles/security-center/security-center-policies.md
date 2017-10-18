---
title: "Nastavení zásad zabezpečení ve službě Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s konfigurací zásad zabezpečení ve službě Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: yurid
ms.openlocfilehash: 67564e930310433bf4d51f7642bdd7ebf7e8e600
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Nastavení zásad zabezpečení ve službě Azure Security Center
Tento dokument vám pomůže nakonfigurovat zásady zabezpečení v Security Center a provede vás potřebnými kroky.


## <a name="how-security-policies-work"></a>Jak fungují zásady zabezpečení?
Security Center automaticky vytváří výchozí zásady zabezpečení pro každé z vašich předplatných Azure. Zásady můžete upravovat ve službě Security Center nebo můžete pomocí [zásad Azure](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) vytvářet nové definice, definovat další zásady, přiřazovat zásady napříč skupinami pro správu (ty mohou představovat celou organizaci, obchodní jednotku atd.) a monitorovat dodržování předpisů těchto zásad v příslušných oborech.

> [!NOTE]
> Zásady Azure jsou ve verzi Limited Preview. Pokud se chcete připojit, klikněte [sem](https://aka.ms/getpolicy). Další informace o zásadách Azure najdete v tématu [Vytvoření a správa zásad pro vynucování dodržování předpisů](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy).

## <a name="how-to-change-security-policies-in-security-center"></a>Jak změnit zásady zabezpečení ve službě Security Center?
Ve službě Security Center můžete upravit výchozí zásady zabezpečení pro každé z vašich předplatných Azure. Pokud chcete upravit zásady zabezpečení, musíte být vlastníkem, přispěvatelem nebo správcem zabezpečení daného předplatného nebo skupiny pro správu, která je obsahuje. Přihlaste se na webu Azure Portal a postupujte podle následujících kroků pro zobrazení zásad zabezpečení ve službě Security Center:

1. Na řídicím panelu **Security Center** v části **Obecné** klikněte na **Zásady zabezpečení**.
2. Vyberte předplatné, pro které chcete zásady zabezpečení povolit.

    ![Správa zásad](./media/security-center-policies/security-center-policies-fig10.png)

3. V části **SOUČÁSTI ZÁSAD** klikněte na **Zásady zabezpečení**.

    ![Součásti zásad](./media/security-center-policies/security-center-policies-fig12.png)

4. Toto jsou výchozí zásady přiřazené službě Security Center prostřednictvím zásad Azure. Můžete odstranit položky, které jsou v části **ZÁSADY A PARAMETRY**, nebo přidat další definice zásad, které jsou v části **DOSTUPNÉ MOŽNOSTI**. Pokud to chcete udělat, stačí kliknout na symbol plus vedle názvu definice.

    ![Definice zásad](./media/security-center-policies/security-center-policies-fig11.png)

5. Pokud chcete podrobnější vysvětlení nějaké zásady, klikněte na ni a otevře se další stránka s podrobnostmi a kódem JSON, který obsahuje strukturu [definice zásady](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy#policy-definition-structure):

    ![JSON](./media/security-center-policies/security-center-policies-fig13.png)

6. Jakmile budete s úpravami hotovi, klikněte na **Uložit**.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak ve službě Azure Security Center konfigurovat zásady zabezpečení. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md). Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](security-center-managing-and-responding-alerts.md). Zjistěte, jak spravovat a zpracovávat výstrahy zabezpečení.
* [Monitorování partnerských řešení pomocí služby Azure Security Center](security-center-partner-solutions.md). Zjistěte, jak monitorovat stav partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.
