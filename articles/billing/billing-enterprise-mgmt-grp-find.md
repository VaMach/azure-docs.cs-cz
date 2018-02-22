---
title: "Najít předplatné nebo správu skupiny Azure - Azure | Microsoft Docs"
description: "Jak se orientovat mezi několik adresářů, pokud chcete zobrazit skupiny pro správu a odběry"
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: f1b9c1ec2af8240ff71f6907516d8894c36ac9c3
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/08/2017
---
# <a name="find-an-azure-subscription-or-management-group"></a>Najít skupiny správy nebo předplatné Azure

Pokud máte problémy s vyhledáním předplatné nebo skupinu pro správu v Azure, můžete být vyhledávání v adresáři nesprávný. Tato situace může dojít v případě, že váš účet existuje v několika Azure Active Directory. Každý [služby active directory je nezávislá](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-directory-independence) a přístup není zděděna ve adresářích.      

![Přepínač adresáři nabídce](media/billing-enterprise-mgmt-groups/mgempty.png)



## <a name="switch-directories"></a>Přepínač adresáře 
Můžete snadno přepínat adresářů na portálu Azure.
1.  Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2.  Vyberte název v horním pravém rohu obrazovky. 
3.  Dolní části nabídky jsou uvedeny všechny adresáře, ke kterým máte přístup k.
4.  Vyberte název adresáře pro přístup. 

![Přepínač adresáři nabídce](media/billing-enterprise-mgmt-groups/switch-directory.png)

## <a name="asset-is-unavailable"></a>Prostředek není k dispozici? 
Pokud se vám zobrazí zpráva "Tento prostředek je k dispozici" při pokusu o přístup k odběru nebo správu skupinu a potom nemají správnou roli zobrazíte tuto položku.  

![prostředek není nalezena](media/billing-enterprise-mgmt-groups/asset-not-found.png)

Obraťte se na Spravovat předplatné nebo správu skupin má být poskytnut přístup.  
* Odběry, odkazovat [řízení řízení přístupu (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) dokumentu nápovědy, na kterém je vyžadována role.
* Pro skupiny pro správu přístupu RBAC není k dispozici a bude brzo. Obraťte se na podnikový portál spravovat tak, aby měl přístup přiřazen.   

## <a name="improve-your-experience-with-management-groups-and-subscriptions-in-the-same-directory"></a>Zlepšení vašich zkušeností s skupin pro správu a odběry ve stejném adresáři 
Předplatné nebo skupiny pro správu můžou přenášet do adresáře, který zvolíte, aby vše v jednom místě existuje.  Konsolidují předplatných a skupin pro správu do stejného adresáře pomáhá snižovat potřeba přepnout adresáře a povolit zásady na zděděná.  


### <a name="transfer-your-subscriptions"></a>Přenos odběrů 
Předplatné můžete přesunout k adresáři přidružené skupiny pro správu. Tento přesun lze dosáhnout tak, že správce registrace k účtu v cílovém adresáři přenos vašeho předplatného. Další informace, přihlaste se na [podnikový portál](https://ea.azure.com/helpdocs/changeAccountOwnerForASubscription).


 






