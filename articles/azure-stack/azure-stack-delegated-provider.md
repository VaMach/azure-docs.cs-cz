---
title: "Delegování nabídky v zásobníku Azure | Microsoft Docs"
description: "Zjistěte, jak se umístí jiní lidé starosti vytvoření nabídky a registrací uživatele."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 157f0207-bddc-42e5-8351-197ec23f9d46
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: brenduns
ms.reviewer: alfredop
ms.openlocfilehash: 287bc04660664facbe99d2cb80ae6c92e41c4111
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="delegate-offers-in-azure-stack"></a>Delegování nabídek v Azure Stacku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Jako operátor zásobník Azure budete chtít často put jiní lidé starosti vytvoření nabídky a přihlašování uživatelů. Například pokud jste poskytovatel služeb, můžete si zaregistrovat zákazníků a spravovat je vaším jménem prodejce. Nebo pokud jste součástí centrální skupinu IT v podniku, můžete chtít pobočky k registraci uživatelů bez zásahu uživatele.

Delegování vám pomůže se tyto úlohy tak, že je možné k dosažení a správě více uživatelů, než můžete přímo. Následující obrázek znázorňuje jednu úroveň delegování, ale zásobník Azure podporuje více úrovních. Delegované zprostředkovatelé (distribučních bodů) můžete zase delegovat na jiných poskytovatelů, až pěti úrovněmi.

![Úrovně delegování](media/azure-stack-delegated-provider/image1.png)

Operátory Azure zásobníku můžete delegovat vytvoření nabídky a uživatelů ke ostatními uživateli pomocí funkce delegování.

## <a name="roles-and-steps-in-delegation"></a>Role a kroky v delegování
Pochopit delegování, mějte na paměti, že existují tři role související se situací:

* *Zásobník Azure operátor* spravuje infrastrukturu Azure zásobníku, vytvoří šablonu nabídka a deleguje ostatním uživatelům nabízí svým uživatelům.

* Delegované operátory Azure zásobníku se nazývají *delegovaný zprostředkovatelé*. Můžete patří do jiných organizací, jako je například jiných uživatelů Azure Active Directory (Azure AD).

* *Uživatelé* zaregistrovat nabídky a použít je pro správu jejich úlohy vytváření virtuálních počítačů, ukládání dat a tak dále.

Jak je znázorněno na následujícím obrázku, existují dva kroky pro vytvoření delegování.

1. *Identifikovat delegované zprostředkovatele*. To provedete je se přihlásíte k odběru na základě plánu, který obsahuje pouze odběry služby nabídku. Uživatelé, kteří jsou přihlášení k odběru na tuto nabídku získat některé z možností operátor zásobník Azure, včetně možnosti rozšíření nabídky a uživatele je pro službu.

2. *Delegovat nabídku delegované poskytovatele*. Tato nabídka funguje jako šablonu pro co můžete nabízet delegovanou zprostředkovatele. Poskytovatele delegovanou teď může trvat nabídku. Zvolte název pro něj (ale neměnit jeho služeb a kvóty) a nabízet zákazníkům.

![Vytvoření poskytovatele delegovanou a umožňuje jim registrace uživatele](media/azure-stack-delegated-provider/image2.png)

Působit jako delegovaní zprostředkovatelé, uživatelé musí vytvořit vztah s hlavní zprostředkovatele. Jinými slovy musí vytvořit odběr. Toto předplatné v tomto scénáři identifikuje delegované zprostředkovatele tak, že má oprávnění k dispozici nabízí jménem hlavní zprostředkovatele.

Po navázání tuto relaci, operátor zásobník Azure můžete delegovat nabídku delegované poskytovatele. Delegované zprostředkovatel je teď možné provést nabídku, přejmenujte ji (ale neumožňuje změnit samé) a nabízí svým zákazníkům.

Následující části popisují, jak vytvořit poskytovatele delegovanou, delegovat nabídku a ověřte, zda uživatelé mohou zaregistrovat pro ni.

## <a name="set-up-roles"></a>Nastavit role

K poskytovatele delegovanou v práci, budete potřebovat další Azure AD účty kromě vašeho účtu operátor zásobník Azure. Pokud jste tyto dva účty, vytvořte je. Účty, můžou patřit do žádné uživatele Azure AD a jsou označovány jako poskytovatele delegovanou a uživatele.

| **Role** | **Organizační práva** |
| --- | --- |
| Poskytovatele delegovanou |Uživatel |
| Uživatel |Uživatel |

## <a name="identify-the-delegated-providers"></a>Identifikovat delegované zprostředkovatele
1. Přihlaste se jako operátor zásobník Azure.

2. Vytvořte nabídku, která umožní uživatelům stane delegované zprostředkovatelé:
   
   a.  [Vytvoření plánu](azure-stack-create-plan.md).
       Tento plán by měly obsahovat pouze službu odběry. Tento článek používá plán nazvaný **PlanForDelegation**.
   
   b.  [Vytvořit nabídku](azure-stack-create-offer.md) na základě tohoto plánu. Tento článek používá nabídku názvem **OfferToDP**.
   
   c.  Po dokončení vytvoření nabídky přidáte poskytovatele delegovanou jako odběratel v rámci této nabídky. To provedete zaškrtnutím **odběry** > **přidat** > **nové předplatné klienta**.
   
   ![Přidání poskytovatele delegovanou jako odběratel](media/azure-stack-delegated-provider/image3.png)

> [!NOTE]
> S nabídkami všechny zásobník Azure, máte možnost provedení nabídku veřejných a umožníte uživatelům zaregistrovat, nebo zachovat privátní, takže operátor zásobník Azure Spravovat registraci. Delegované poskytovatelé jsou obvykle malou skupinu. Chcete určovat, kdo bude umožněno, takže udržuje v rámci této nabídky privátní smysl ve většině případů.
> 
> 

## <a name="azure-stack-operator-creates-the-delegated-offer"></a>Nabídku delegované vytvoří Azure operátor zásobníku

Nyní jste vytvořili poskytovatele delegovanou. Dalším krokem je vytvoření plánu a nabídky, který chcete delegovat a který bude používat vaši zákazníci. Je vhodné definovat v rámci této nabídky přesně tak, jak chcete, aby zákazníci ji zobrazit, protože poskytovatele delegovanou nelze změnit plány a kvóty, které obsahuje.

1. Jakožto Obsluha zásobník Azure [vytvořit plán](azure-stack-create-plan.md) a [nabídku](azure-stack-create-offer.md) založených na. Tento článek používá nabídku názvem **DelegatedOffer.**
   
   > [!NOTE]
   > Tato nabídka nemusí být veřejné. Pokud si zvolíte, může být veřejné. Ve většině případů se však pouze chcete delegované zprostředkovatelé tak, aby měl přístup k němu. Po privátní nabídka delegovat, jak je popsáno v následujících krocích, poskytovatele delegovanou k němu má přístup.
   > 
   > 
1. Delegate nabídku. Přejděte na **DelegatedOffer.** Potom v **nastavení** podokně, vyberte **delegovaný zprostředkovatelé** > **přidat**.

2. Vyberte předplatné pro delegovanou zprostředkovatele z rozevíracího seznamu a pak vyberte **delegáta**.

> ![Přidání poskytovatele delegovanou](media/azure-stack-delegated-provider/image4.png)
> 
> 

## <a name="delegated-provider-customizes-the-offer"></a>Poskytovatele delegovanou přizpůsobí nabídky

Přihlaste se k portálu user portal jako poskytovatele delegovanou a poté vytvořit novou nabídku pomocí nabídku delegované jako šablona.

1. Vyberte **nové** > **klienta nabízí + plány** > **nabízejí**.

    ![Vytvoření nové nabídky](media/azure-stack-delegated-provider/image5.png)


1. Přiřadíte název nabídky. Tento článek používá **ResellerOffer**. Vyberte nabídku delegované na kterém chcete základní ho a pak vyberte **vytvořit**.
   
   ![Přiřadit název](media/azure-stack-delegated-provider/image6.png)

    >[!NOTE] 
    > Všimněte si rozdílu ve srovnání s nabízejí vytvoření jako zkušeného operátorem zásobník Azure. Poskytovatele delegovanou není vytvořit nabídku ze základní a plány rozšíření. Můžete volit pouze z nabídky, které mají oprávnění k jejich a nelze provádět změny těchto nabídek.

1. Zveřejnit nabídku výběrem **Procházet**a potom **nabízí**. Vyberte nabídku a pak vyberte **změny stavu**.

2. Delegované zprostředkovatel poskytuje tyto nabídky prostřednictvím svých vlastních portálu adresy URL. Tyto nabídky jsou viditelné pouze prostřednictvím portálu delegovaní. K vyhledání a změňte tuto adresu URL:
   
    a.  Vyberte **Procházet** > **další služby** > **odběry**. Potom vyberte poskytovatele delegovanou předplatné. Například **DPSubscription** > **vlastnosti**.
   
    b.  Zkopírujte portálu adresu URL do samostatných umístění, například Poznámkový blok.
   
    ![Vyberte poskytovatele delegovanou předplatné](media/azure-stack-delegated-provider/dpportaluri.png)  
   
   Nyní jste vytvořili na nabídku delegované jako delegovaný zprostředkovatele. Odhlaste se jako delegovaný zprostředkovatele. Zavřete okno prohlížeče, kterou používáte.

## <a name="sign-up-for-the-offer"></a>Zaregistrujte si nabídky
1. V nové okno prohlížeče, přejděte na portál delegované adresu URL, který jste uložili v předchozím kroku. Přihlaste se k portálu jako uživatel. 
   
   >[!NOTE]
   >Delegované nabídky se nezobrazí, pokud nepoužíváte delegované portálu. 

2. Na řídicím panelu, vyberte **získat předplatné**. Zobrazí sdělit pouze delegované nabídky, které byly vytvořeny poskytovatele delegovanou uživateli:

> ![Zobrazit a vybrat nabídky](media/azure-stack-delegated-provider/image8.png)
> 
> 

Proces delegování nabídka je dokončený. Uživatele můžete nyní zaregistrovat v rámci této nabídky získáním předplatné pro ni.

## <a name="multiple-tier-delegation"></a>Vícevrstvé delegování

Vícevrstvé delegování umožňuje poskytovateli delegované delegovat nabídku na ostatní entity. To umožňuje například vytvoření hlubší prodejce kanály, ve kterých zprostředkovatele, který spravuje zásobník Azure deleguje nabídku pro distributora. Distributor, pak deleguje prodejce. Azure zásobníku podporuje až pět úrovní delegování.

Pokud chcete vytvořit několik vrstev nabídka delegování, poskytovatele delegovanou zase deleguje nabídku dalšímu zprostředkovateli. Proces je stejný pro poskytovatele delegovanou stejně jako tomu bylo pro operátor zásobník Azure (viz [operátor zásobník Azure vytvoří nabídku delegované](#cloud-operator-creates-the-delegated-offer)).

## <a name="next-steps"></a>Další postup
[Zřízení virtuálního počítače](azure-stack-provision-vm.md)

