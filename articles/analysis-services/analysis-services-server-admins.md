---
title: "Správa serveru admins ve službě Azure Analysis Services | Microsoft Docs"
description: "Naučte se spravovat správci serveru pro server služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: a1b58125dafdf73f245b6a8cd0f4917513b22ea9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="manage-server-administrators"></a>Spravovat správce serveru
Správci serveru musí být platný uživatel nebo skupina ve službě Azure Active Directory (Azure AD) pro klienta, ve kterém se nachází server. Můžete použít **Analysis Services Admins** v okně Ovládací prvek pro server na portálu Azure nebo vlastnosti serveru v aplikaci SSMS ke správě správci serveru. 

## <a name="to-add-server-administrators-by-using-azure-portal"></a>Přidat správce serveru pomocí portálu Azure
1. V okně Ovládací prvek pro váš server, klikněte na tlačítko **Analysis Services Admins**.
2. V  **\<servername >-Analysis Services Admins** okně klikněte na tlačítko **přidat**.
3. V **přidat správci serveru** okně vyberte uživatelské účty ze služby Azure AD a zvaní externí uživatele podle e-mailovou adresu.

    ![Správci serveru na portálu Azure](./media/analysis-services-server-admins/aas-manage-users-admins.png)

## <a name="to-add-server-administrators-by-using-ssms"></a>Přidat správce serveru pomocí aplikace SSMS
1. Pravým tlačítkem na server > **vlastnosti**.
2. V **vlastnosti serveru pro analýzu**, klikněte na tlačítko **zabezpečení**.
3. Klikněte na tlačítko **přidat**a potom zadejte e-mailovou adresu pro uživatele nebo skupiny ve službě Azure AD.
   
    ![Přidat správce serveru v aplikaci SSMS](./media/analysis-services-server-admins/aas-manage-users-ssms.png)

## <a name="next-steps"></a>Další kroky 
[Ověřování a uživatelská oprávnění](analysis-services-manage-users.md)  
[Spravovat role databáze a uživatele](analysis-services-database-users.md)  
[Řízení přístupu na základě rolí](../active-directory/role-based-access-control-what-is.md)  

