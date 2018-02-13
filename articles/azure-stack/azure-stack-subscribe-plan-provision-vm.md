---
title: "Přihlášení k odběru na nabídku v zásobníku Azure | Microsoft Docs"
description: "Vytvořte odběr pro nabídky v Azure zásobníku"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 9b35b497c264fab2b8352eda82fe6d4e1fc274e9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Vytvoření odběrů nabídky v Azure zásobníku

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

Po jste [vytvořit nabídku](azure-stack-create-offer.md), musí uživatelé před použitím ji předplatné v rámci této nabídky.   
- Jako operátor cloudu můžete vytvořit odběr pro uživatele z portálu pro správce.  Odběry, které vytvoříte, může být pro veřejné a privátní nabídky.
- Jako uživatel klienta můžete přihlásit na nabídku veřejného při použití portálu user portal.  

Následující části obsahují pokyny pro operátorům cloudu při vytváření odběry pro uživatele a jak se přihlásit odběru na nabídku jako uživatel.

## <a name="create-a-subscription-as-a-cloud-operator"></a>Vytvoření odběru jako operátor cloudu
Operátoři cloudu můžete použít k vytvoření odběru na nabídku pro uživatele portálu pro správu.  Můžete vytvářet odběry pro členy klientovi služby directory.  Když [víceklientský](azure-stack-enable-multitenancy.md) je povoleno, můžete také vytvářet odběry pro uživatele v klientech další adresář.

Můžete vytvářet odběry pro veřejné a privátní nabídky.  Pokud nechcete, aby klienti vytvářet své vlastní odběry, proveďte všechny vaší privátní nabídky a pak vytvořte odběry jménem klientům. Tento přístup je běžné při integraci s externí fakturace nebo systémů katalogu služby Azure zásobníku.

Po vytvoření odběru pro uživatele, tento uživatel může přihlásit k portálu pro uživatele a zjistí jsou předplatné nabídku.  

### <a name="to-create-the-subscription-for-a-user"></a>Vytvoření odběru pro uživatele
1.  V portálu pro správu, přejděte do **odběry uživatele.**
2.  Vyberte **přidat** otevřete **nové předplatné uživatele** podokně. Sem zadejte následující podrobnosti:  

  a. **Zobrazovaný název** – popisný název pro identifikaci předplatné, které se zobrazí jako *název odběru uživatele*.

  b. **Uživatel** – zadejte uživatel z tenanta služby k dispozici adresáře pro toto předplatné. Uživatelské jméno se zobrazí jako *vlastníka*.  Formát uživatelského jména, závisí na řešení identity. Příklad:   

     -  **Azure AD:***&lt;uživatel1 > @&lt;contoso.onmicrosoft.com >* 

     -   **Službu AD FS:***&lt;uživatel1 > @&lt;azurestack.local >*      

  c.    **Klienta Directory** – vyberte adresář klienta, pokud je uživatelský účet členem. Pokud jste nepovolili víceklientský, je k dispozici pouze vašeho místního adresáře klienta.

3.  Vyberte **nabízejí** otevřete **nabízí** podokně a potom vyberte **nabízejí** pro toto předplatné. Vzhledem k tomu, že vytváříte odběr pro uživatele, můžete vybrat privátní nebo veřejné nabídky.

4.  Vyberte **vytvořit** vytvoření odběru. **Uživatele odběry** podokně teď zobrazuje nové předplatné.  Později když se uživatel přihlásí do portálu pro uživatele, že můžete zobrazit podrobnosti o toto předplatné.

### <a name="to-make-an-add-on-plan-available"></a>Chcete-li zpřístupnit plán rozšíření  
Operátor cloudu můžete přidat plán rozšíření na dříve vytvořenou předplatné kdykoli:   
1.  Přejděte v portálu pro správu **více služeb** > **uživatele odběry**a potom vyberte předplatné, kterou chcete změnit.   

2.  Vyberte **doplňky** > **přidat** otevřete **přidat plán** podokně.  

3.  Vyberte plán, který chcete přidat jako doplněk.  Konzole se pak zobrazí plány přidruženou k odběru.




## <a name="create-a-subscription-as-a-user"></a>Vytvoření odběru jako uživatel
Jako uživatel můžete se přihlásit k portálu user portal k vyhledání a přihlásit se k veřejné nabídky a plány rozšíření z vašeho adresáře klienta (organizace). Když prostředí zásobníku Azure podporuje [víceklientský](azure-stack-enable-multitenancy.md) můžete odebírat nabídky z klienta vzdáleného adresáře.

### <a name="to-subscribe-to-an-offer"></a>K odběru na nabídku
1. [Přihlaste se](azure-stack-connect-azure-stack.md) uživatelský portál Azure zásobníku (https://portal.local.azurestack.external) a klikněte na tlačítko **získat předplatné**.

   ![Získat předplatné](media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. V **zobrazovaný název** pole, zadejte název pro vaše předplatné, klikněte na **nabízejí**, klikněte na jednu z nabídky v **zvolte nabídku** podokně a pak klikněte na tlačítko  **Vytvoření**.

   ![Vytvoření nabídky](media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. Chcete-li zobrazit předplatné, které jste vytvořili, klikněte na tlačítko **další služby**, klikněte na tlačítko **odběry**, pak klikněte na nové předplatné.  

Jakmile se přihlásíte k nabídku, aktualizujte portálu, abyste zjistili, služby, které jsou součástí nové předplatné.

### <a name="to-subscribe-to-an-add-on-plan"></a>K odběru plánu rozšíření
Pokud nabídku plán rozšíření, můžete přidat plán k vašemu předplatnému kdykoli.  

1. V portálu pro uživatele, vyberte **další služby** > **odběry**a potom vyberte předplatné, že chcete změnit.

2. Vyberte **přidat plán** tlačítko a potom vyberte plán rozšíření chcete. Pokud **přidat plán** není k dispozici, pak nejsou žádné plány rozšíření nabídky přidružen toto předplatné.



## <a name="next-steps"></a>Další postup
[Zřízení virtuálního počítače](azure-stack-provision-vm.md)
