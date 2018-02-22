---
title: "Správa služby Azure Analysis Services | Microsoft Docs"
description: "Zjistěte, jak ke správě serveru služby Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: 11aa42fbe15dd68dfac128b707fd6bdbfd0b24d1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="manage-analysis-services"></a>Správa služby Analysis Services
Po vytvoření serveru služby Analysis Services v Azure, mohou být některé úlohy správy a údržby, které je třeba provést hned nebo zopakovat dolů na cestách. Například spusťte zpracování aktualizace dat, určovat, kdo může získat přístup modely na vašem serveru, nebo sledování stavu vašeho serveru. Některé úlohy správy lze provést pouze na portálu Azure, jiné v SQL Server Management Studio (SSMS) a některé úlohy lze provádět buď.

## <a name="azure-portal"></a>Azure Portal
[Portál Azure](http://portal.azure.com/) je, kde můžete vytvořit a odstraněním serverů, monitorování prostředky serveru, změnit velikost a spravovat, kdo má přístup k serverům.  Pokud máte nějaké problémy, můžete také odeslat žádost o podporu.

![Získání názvu serveru v Azure](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Připojení k serveru v Azure je stejně jako připojení k instanci serveru ve vaší vlastní organizaci. Z aplikace SSMS můžete provádět mnoho úloh, jako je například zpracování dat nebo vytvořit skript zpracování, správě rolí a pomocí prostředí PowerShell.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Stažení a instalace aplikace SSMS
Chcete-li získat všechny nejnovější funkce a nejhladší zkušenosti při připojování k serveru Azure Analysis Services, ujistěte se, že používáte nejnovější verzi aplikace SSMS. 

[Stáhněte si SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="to-connect-with-ssms"></a>Připojit pomocí SSMS
 Při použití aplikace SSMS, před připojením k serveru poprvé, zkontrolujte, zda že vaše uživatelské jméno je zahrnutá ve skupině Admins služby Analysis Services. Další informace najdete v tématu [správci serveru](#server-administrators) dále v tomto článku.

1. Než připojíte, potřebujete získat název serveru. Na portálu **Azure Portal** > Server > **Přehled** > **Název serveru** zkopírujte název serveru.
   
    ![Získání názvu serveru v Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. V aplikaci SSMS > **Průzkumník objektů**, klikněte na tlačítko **připojit** > **služby Analysis Services**.
3. V **připojit k serveru** dialogové okno, vložte v názvu serveru, a potom v **ověřování**, vyberte jednu z následujících typů ověřování:
   
    **Ověřování systému Windows** používat přihlašovací údaje doména\uživatelské jméno a heslo systému Windows.

    **Ověřování hesla Active Directory** použít účet organizace. Například když připojení z jiné domény připojený počítač.

    **Univerzální ověřování služby Active Directory** používat [neinteraktivní nebo vícefaktorového ověřování](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![V aplikaci SSMS připojit](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Správci serveru a databáze uživatelů
V Azure Analysis Services existují dva typy uživatelů, správce serveru a uživatele databáze. Oba typy uživatelů, musí být ve vašem Azure Active Directory a musí být určena organizační e-mailovou adresu nebo hlavní název uživatele. Další informace najdete v tématu [Ověřování a uživatelská oprávnění](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Odstraňování potíží s připojením
Pokud se připojujete pomocí aplikace SSMS, pokud narazíte na problém, musíte vymazat mezipaměť přihlášení. Nic se uloží do mezipaměti na disku. Vymazání mezipaměti, ukončete a restartujte proces připojení. 

## <a name="next-steps"></a>Další postup
Pokud již jste nenasadili tabulkový model na nový server, teď je vhodná doba. Další informace najdete v tématu [Nasazení do služby Azure Analysis Services](analysis-services-deploy.md).

Pokud model jste nasadili na server, jste připraveni připojit se pomocí klienta nebo prohlížeče. Další informace najdete v tématu [načíst data ze serveru Azure Analysis Services](analysis-services-connect.md).

