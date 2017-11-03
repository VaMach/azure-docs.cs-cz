---
title: "Umístění ve službě Azure Active Directory s názvem | Microsoft Docs"
description: "Konfigurace s názvem umístění, nemusíte mít IP adresy, které jsou vlastněny organizaci generovat falešně pozitivních pro Impossible dostavit do netypických míst typ události riziko."
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
ms.date: 09/20/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: da437908509e40386ed23863648bd6956b308186
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="named-locations-in-azure-active-directory"></a>Pojmenované umístění v Azure Active Directory

Pomocí funkce s názvem umístění služby Azure Active Directory můžete označit důvěryhodné rozsahy IP adres ve vaší organizace. Ve vašem prostředí, můžete použít s názvem umístění v kontextu detekce [rizik události](active-directory-reporting-risk-events.md). Tato funkce pomáhá snížit počet hlášených falešně pozitivních pro *Impossible dostavit do netypických míst* rizik typ události. 

## <a name="configuration"></a>Konfigurace

Konfigurace s názvem umístění:

1. Přihlaste se k [portál Azure](https://portal.azure.com) jako globální správce.

2. V levém podokně klikněte na **Azure Active Directory**.

    ![V levém podokně na odkaz Azure Active Directory](./media/active-directory-named-locations/01.png)

3. Na **Azure Active Directory** okno v **zabezpečení** klikněte na tlačítko **podmíněného přístupu**.

    ![Příkaz podmíněného přístupu](./media/active-directory-named-locations/05.png)


4. Na **podmíněného přístupu** okno v **spravovat** klikněte na tlačítko **s názvem umístění**.

    ![Příkaz pojmenované umístění](./media/active-directory-named-locations/06.png)


5. Na **s názvem umístění** okně klikněte na tlačítko **nové umístění**.

    ![Příkaz nové umístění](./media/active-directory-named-locations/07.png)


6. Na **nový** okno, proveďte následující:

    ![Nové okno](./media/active-directory-named-locations/56.png)

    a. V **název** zadejte název pro vaše s názvem umístění.

    b. V **rozsahy IP adres** zadejte rozsah adres IP. Rozsah IP adres musí být ve *směrování mezi doménami* formátu (CIDR).  

    c. Klikněte na možnost **Vytvořit**.



## <a name="what-you-should-know"></a>Důležité informace

**Hromadné aktualizace**: při vytváření nebo aktualizaci s názvem umístění pro hromadné aktualizace, můžete nahrát nebo stáhnout soubor CSV s rozsahy IP. Nahrávaný přidá rozsahy IP v souboru do seznamu místo přepsání seznamu.

![Nahrávání a stahování odkazy](./media/active-directory-named-locations/09.png)


**Omezení**: nesmí být delší než 60 s názvem umístění, můžete definovat s jeden rozsah IP adres přiřazené ke každému z nich. Pokud máte pouze jednu s názvem umístění nakonfigurovaný, můžete definovat maximálně 500 rozsahy IP adres pro ni.


## <a name="next-steps"></a>Další kroky

Další informace o rizikových událostech najdete v tématu [Azure Active Directory rizikových událostech](active-directory-reporting-risk-events.md).

