---
title: Smluv pro komunikace B2B - Azure Logic Apps | Microsoft Docs
description: "Vytvoření smlouvy, partneři může komunikovat v scénáře B2B Azure Logic Apps a Enterprise integračního balíčku"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 447ffb8e-3e91-4403-872b-2f496495899d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: LADocs
ms.openlocfilehash: 7ce0860272901f3b4e4cf3d63f7361d539f64741
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="partner-agreements-for-b2b-communication-with-azure-logic-apps-and-enterprise-integration-pack"></a>Partner smluv pro B2B komunikace se službou Azure Logic Apps a Enterprise integračního balíčku

Smlouvy umožňují obchodní entity komunikovat bez problémů pomocí standardních protokolů a jsou základním kamenem pro komunikaci business-to-business (B2B). Při povolování scénáře B2B pro logic apps s integračního balíčku Enterprise, smlouva je uspořádání komunikace mezi obchodními partnery B2B. Tato smlouva je podle komunikace, partnery, které chcete vytvořit a je protokol nebo přenos specifické.

Integrace Enterprise podporuje těchto standardů protokol nebo přenos:

* [AS2](logic-apps-enterprise-integration-as2.md)
* [X12](logic-apps-enterprise-integration-x12.md)
* [EDIFACT](logic-apps-enterprise-integration-edifact.md)

## <a name="why-use-agreements"></a>Proč používat smlouvy

Zde jsou některé běžné výhody při použití smlouvy:

* Umožňuje různým organizacím a podnikům k výměně informací o ve formátu, dobře známé.
* Zlepšení efektivity při provádění transakcí B2B
* Usnadňují vytváření, správu a použít při vytváření enterprise integrace aplikací

## <a name="how-to-create-agreements"></a>Postup vytvoření smluv

* [Vytvoření smlouvy AS2](logic-apps-enterprise-integration-as2.md)
* [Vytvoření X12 smlouvy](logic-apps-enterprise-integration-x12.md)
* [Vytvoření smlouvy EDIFACT](logic-apps-enterprise-integration-edifact.md)

## <a name="how-to-use-an-agreement"></a>Jak používat smlouvu

Můžete vytvořit [aplikace logiky](logic-apps-what-are-logic-apps.md "Další informace o Logic apps") s možnostmi B2B pomocí smlouvu, kterou jste vytvořili.

## <a name="how-to-edit-an-agreement"></a>Jak upravit smlouvu

Jakékoli smlouvy, můžete upravit pomocí následujících kroků:

1. Vyberte integrační účet, který má smlouvy, kterou chcete aktualizovat.

2. Vyberte **smlouvy** dlaždici.

3. Na **smlouvy** okně vyberte smlouvu.

4. Zvolte **upravit**. Provedené změny.

5. Pokud chcete uložit provedené změny, zvolte **OK**.

## <a name="how-to-delete-an-agreement"></a>Postup odstranění smlouvy

Jakékoli smlouvy, můžete odstranit pomocí následujících kroků:

1. Vyberte integrační účet, který má smlouvy, kterou chcete odstranit.
2. Vyberte **smlouvy** dlaždici.
3. Na **smlouvy** okně vyberte smlouvu.
4. Zvolte **odstranit**.
5. Potvrďte, že chcete vybranou smlouvu odstranit.

    V okně smlouvy přestane zobrazovat odstraněné smlouvy.

## <a name="next-steps"></a>Další kroky
* [Vytvoření smlouvy AS2](logic-apps-enterprise-integration-as2.md)
