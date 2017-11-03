---
title: "Vytvoření instance služby Azure API Management | Microsoft Docs"
description: "Postupujte podle kroků tohoto kurzu k vytvoření nové instance Azure API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 6433ea1f0eb6ad375402b998b4dfa80bded35c4b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-new-azure-api-management-service-instance"></a>Vytvoření nové instance služby Azure API Management

Tento kurz popisuje kroky pro vytvoření nové instance API Management pomocí [portál Azure](https://portal.azure.com/).

## <a name="prerequisites"></a>Požadavky

+ Aktivní předplatné Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-new-service"></a>Vytvoření nové služby

1. V [portál Azure](https://portal.azure.com/), vyberte **nový** > **Enterprise integrace** > **API management**.

    Nebo zvolte **nový**, typ `API management` vyhledávacího pole a stiskněte Enter. Klikněte na možnost **Vytvořit**.

2. V **služba API Management** okno, zadejte jedinečný **název** služby API Management. Tento název není možné později změnit.

    > [!TIP]
    > Název služby se používá ke generování výchozí název domény ve formě *.azure-api.net {name}.* Pokud chcete použít vlastní název domény, najdete v části [nakonfigurovat vlastní doménu](configure-custom-domain.md). <br/>
    > Název služby slouží k odkazování na službu a odpovídající prostředků Azure.

5. Vyberte **předplatné** mezi různých předplatných Azure, které máte přístup.
6. V poli **Skupina prostředků** vyberte nový nebo existující prostředek.  Skupina prostředků je kolekce prostředků, které sdílejí životní cyklus, oprávnění a zásady. Další informace najdete [tady](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. V **umístění**, vyberte zeměpisnou oblast, kde se má vytvořit službu API Management. V rozevíracím seznamu se zobrazí pouze dostupné oblasti služby API Management. 
9. Zadejte **název organizace**. Tento název se používá v počet míst. Například název portál pro vývojáře a odesílatele oznámení e-mailů.
10. V **e-mailu správce**, nastavte e-mailovou adresu, na které všechna oznámení z **API Management** odešle.
11. V **cenová úroveň**, nastavte **vývojáře** vrstvy vyhodnotit službu. Tato úroveň není pro použití v provozním prostředí. Další informace o škálování úrovních služby API Management najdete v tématu [upgrade a škálování](upgrade-and-scale.md).
12. Zvolte **Vytvořit**.

    > [!TIP]
    > Obvykle trvá 20 až 30 minut pro vytvoření služby API Management. Výběr **připnout na řídicí panel** umožňuje hledání snadnější nově vytvořenou službu.

## <a name="next-steps"></a>Další kroky

[Publikování rozhraní API pomocí Azure API Management](#api-management-getstarted-publish-api.md)