---
title: "Cloud App Discovery. nastavení registru pro Proxy služby | Microsoft Docs"
description: "Cílem tohoto tématu je poskytnout kroky, které je třeba provést nastavit požadovaný port v počítačích se systémem agenta Cloud App Discovery."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 8d78e925-e331-40ba-904a-e4ef14260cac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: f633e76ea7c0df456bff41c450eb136809de12a8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery. nastavení registru pro služby Proxy
Účelem tohoto tématu je poradit, jak provádět nastavit požadovaný port v počítačích se systémem agenta Cloud App Discovery. Ve výchozím nastavení agenta Cloud App Discovery konfigurován pro použití pouze porty 80 nebo 443. Pokud plánujete instalaci Cloud App Discovery v prostředí s proxy serveru, který používá vlastní port (80 ani 443), budete muset nakonfigurovat agenty na tento port použít. Konfigurace je založena na klíč registru.

> [!TIP] 
> Podívejte se na vylepšení nyní bez agenta Cloud App Discovery v Azure Active Directory (Azure AD), které jsou vylepšit určením [integraci s Microsoft Cloud App Security](https://portal.cloudappsecurity.com).

## <a name="modify-the-port-used-by-the-computer-running-the-cloud-app-discovery-agent"></a>Změnit port je používán počítači agenta Cloud App Discovery

1. Spusťte editor registru.
  ![Spustit](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)
2. Přejděte na nebo vytvořte následující klíč registru: **Discovery\Endpoint HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud aplikace**
3. Vytvořte novou **víceřetězcovou** hodnotu s názvem **porty**. 
  ![Nový](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)
4. Otevřete **Upravit víceřetězcovou** dialogové okno, dvakrát klikněte **porty** hodnotu.
5. V **údaj hodnoty**, zadejte následující hodnoty a přidejte všechny vlastní porty, které se používají ve vaší organizaci: <br><br>
   **80** <br>
   **8080** <br>
   **8118** <br>
   **8888** <br>
   **81** <br>
   **12080** <br>
   **6999** <br>
   **30606** <br>
   **31595** <br>
   **4080** <br>
   **443** <br>
   **1110** <br><br>
   ![Upravit víceřetězcovou](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)
6. Klikněte na tlačítko **OK** zavřete **Upravit víceřetězcovou** dialogové okno.

## <a name="next-steps"></a>Další kroky

* [Jak může zjišťovat nedovolené cloudové aplikace, které se používají v rámci Moje organizace](active-directory-cloudappdiscovery-whatis.md) 

