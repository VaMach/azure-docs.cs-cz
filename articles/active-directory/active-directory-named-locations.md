---
title: "Umístění ve službě Azure Active Directory s názvem | Microsoft Docs"
description: "Zjistěte, co s názvem umístění jsou a způsob jejich konfigurace."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 126646d7460831f0235221595b8a93c88be6146d
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/07/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Pojmenované umístění v Azure Active Directory

Pomocí pojmenovaného umístění můžete označit důvěryhodné rozsahy IP adres ve vaší organizaci. S názvem umístění v rámci Azure Active Directory používá:

- Ke zjištění [rizik události](active-directory-reporting-risk-events.md) a snížit počet hlášených falešně pozitivních zjištění.  

- [Podmíněného přístupu na základě umístění](active-directory-conditional-access-azure-portal.md#locations).


Tento článek vysvětluje, jak můžete nakonfigurovat s názvem umístění ve vašem prostředí.


## <a name="entry-points"></a>Vstupní body

Dostanete na stránku konfigurace s názvem umístění **zabezpečení** části stránky Azure Active Directory kliknutím:

![Vstupní body](./media/active-directory-named-locations/34.png)

- **Podmíněný přístup:**

    - V **spravovat** klikněte na tlačítko **s názvem umístění**.
    
        ![Příkaz pojmenované umístění](./media/active-directory-named-locations/06.png)

- **Rizikové přihlášení:**

    - Na panelu nástrojů v horní části klikněte na tlačítko **přidat známé rozsahy IP adres**.

       ![Příkaz pojmenované umístění](./media/active-directory-named-locations/35.png)



## <a name="configuration-example"></a>Příklad konfigurace

**Konfigurace s názvem umístění:**

1. Přihlaste se k [portál Azure](https://portal.azure.com) jako globální správce.

2. V levém podokně klikněte na **Azure Active Directory**.

    ![V levém podokně na odkaz Azure Active Directory](./media/active-directory-named-locations/01.png)

3. Na **Azure Active Directory** stránky v **zabezpečení** klikněte na tlačítko **podmíněného přístupu**.

    ![Příkaz podmíněného přístupu](./media/active-directory-named-locations/05.png)


4. Na **podmíněného přístupu** stránky v **spravovat** klikněte na tlačítko **s názvem umístění**.

    ![Příkaz pojmenované umístění](./media/active-directory-named-locations/06.png)


5. Na **s názvem umístění** klikněte na tlačítko **nové umístění**.

    ![Příkaz nové umístění](./media/active-directory-named-locations/07.png)


6. Na **nový** proveďte následující:

    ![Nové okno](./media/active-directory-named-locations/56.png)

    a. V **název** zadejte název pro vaše s názvem umístění.

    b. V **rozsahy IP adres** zadejte rozsah adres IP. Rozsah IP adres musí být ve *směrování mezi doménami* formátu (CIDR).  

    c. Klikněte na možnost **Vytvořit**.



## <a name="what-you-should-know"></a>Důležité informace

**Hromadné aktualizace**: při vytváření nebo aktualizaci s názvem umístění pro hromadné aktualizace, můžete nahrát nebo stáhnout soubor CSV s rozsahy IP. Nahrávaný přidá rozsahy IP v souboru do seznamu místo přepsání seznamu.

![Nahrávání a stahování odkazy](./media/active-directory-named-locations/09.png)


**Omezení**: nesmí být delší než 60 s názvem umístění, můžete definovat s jeden rozsah IP adres přiřazené ke každému z nich. Pokud máte pouze jednu s názvem umístění nakonfigurovaný, můžete definovat maximálně 500 rozsahy IP adres pro ni.


## <a name="next-steps"></a>Další kroky

Další informace o:

- **Riziko události**, najdete v části [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).

- **Podmíněný přístup**, najdete v části [podmíněný přístup v Azure Active Directory](active-directory-conditional-access-azure-portal.md).

- **Sestavy rizikové přihlášení**, najdete v části [rizikové přihlášení sestav na portálu Azure Active Directory](active-directory-reporting-security-risky-sign-ins.md).  
