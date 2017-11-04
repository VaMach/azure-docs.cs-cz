---
title: "Pomocí vzdálené plochy s rolemi Azure | Microsoft Docs"
description: "Pomocí Azure role vzdálené plochy"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: eab135d10c0d6df8ca72ac47d6804017a998a3d2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a>Pomocí Azure role vzdálené plochy
Pomocí sady Azure SDK a Vzdálená plocha, můžete přístup k Azure role a virtuálních počítačů, které jsou hostované v Azure. V sadě Visual Studio můžete nakonfigurovat vzdálené plochy z projektu Azure cloud service. Pokud chcete povolit vzdálenou plochu, musíte vytvořit pracovní projekt, který obsahuje jednu nebo více rolí a potom jej publikujte do Azure.

> [!IMPORTANT]
> By měl přístup Azure role pro řešení problémů nebo pouze vývoj. Účelem každého virtuálního počítače je spustit určité role v aplikaci Azure, aby se nespouštěla jiné klientské aplikace. Pokud chcete používat Azure jako hostitele virtuálního počítače, který můžete použít pro jakýkoli účel, najdete v části přístup virtuálních počítačů Azure z Průzkumníka serveru.
> 
> 

## <a name="to-enable-and-use-remote-desktop-for-an-azure-role"></a>Povolení a použití služby Vzdálená plocha pro Role v Azure
1. V Průzkumníku řešení otevřete místní nabídku pro projekt cloudové služby a potom zvolte **publikovat**.
   
    **Publikování aplikaci Azure** zobrazí se průvodce.
   
    ![Publikování příkaz pro projekt cloudové služby](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. V dolní části **nastavením publikování v Microsoft Azure** stránce průvodce vyberte **povolení vzdálené plochy** pro všechny role zaškrtávací políčko. 
   
    **Konfigurace vzdálené plochy** zobrazí se dialogové okno.
3. V dolní části **konfigurace vzdálené plochy** dialogovém okně vyberte **další možnosti** tlačítko. 
   
    Zobrazí rozevírací seznam, který umožňuje vytvořit nebo vybrat si certifikát tak, aby při připojování přes vzdálenou plochu můžete šifrovat přihlašovací údaje informace.
4. V rozevíracím seznamu vyberte  **&lt;vytvořit >**, nebo vyberte existující šablonu ze seznamu. 
   
    Pokud zvolíte existující certifikát, můžete přeskočte následující kroky.
   
   > [!NOTE]
   > Certifikáty, které potřebujete pro připojení ke vzdálené ploše se liší od certifikáty, které používáte pro jiné operace v Azure. Certifikát vzdáleného přístupu musí mít privátní klíč.
   > 
   > 
   
    **Vytvořit certifikát** zobrazí se dialogové okno.
   
   1. Zadejte popisný název pro nový certifikát a potom vyberte **OK** tlačítko. V rozevíracím seznamu se zobrazí nový certifikát.
   2. V **konfigurace vzdálené plochy** dialogovém okně zadejte uživatelské jméno a heslo.
      
       Nelze použít existující účet. Nezadávejte jako uživatelské jméno pro nový účet správce.
      
      > [!NOTE]
      > Pokud heslo nesplňuje požadavky na složitost, zobrazí se červená ikona vedle textového pole heslo. Heslo musí obsahovat velká písmena, malá písmena a čísla nebo symboly.
      > 
      > 
   3. Zvolte datum, na který platnosti účtu a po které připojení ke vzdálené ploše se zablokuje.
   4. Po všechny požadované informace, které jste zadali, klikněte **OK** tlačítko.
      
       Několik nastavení, které umožňují přístup k vzdálené se přidají do soubory .cscfg a .csdef.
5. V **nastavením publikování v Microsoft Azure** průvodce, vyberte **OK** tlačítko až budete připraveni k publikování cloudové služby.
   
    Pokud si nejste připravena k publikování, vyberte **zrušit** tlačítko. Konfigurace nastavení se ukládají a cloudové služby můžete publikovat později.

## <a name="connect-to-an-azure-role-by-using-remote-desktop"></a>Připojení do Azure Role pomocí vzdálené plochy
Po publikování cloudové služby na platformě Azure můžete Průzkumníka serveru pro přihlášení na virtuální počítače, který je hostitelem Azure. 

1. V Průzkumníku serveru rozbalte **Azure** uzel a pak rozbalte uzel pro cloudové služby a jeden z jeho role můžete zobrazit seznam instancí.
2. Otevřete místní nabídku pro uzel instanci a potom zvolte **připojit pomocí vzdálené plochy**.
   
    ![Připojování přes vzdálenou plochu](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Zadejte uživatelské jméno a heslo, které jste předtím vytvořili. Nyní jste přihlášeni ke vzdálené relaci.

