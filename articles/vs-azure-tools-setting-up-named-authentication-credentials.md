---
title: "Nastavte pověření s názvem ověřování | Microsoft Docs"
description: "Zjistěte, jak k zadání přihlašovacích údajů, které Visual Studio můžete použít k ověřování žádostí Azure, takže můžete publikovat aplikaci do Azure ze sady Visual Studio nebo monitorování stávající cloudovou službu."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 61570907-42a1-40e8-bcd6-952b21a55786
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/22/2017
ms.author: kraigb
ms.openlocfilehash: 953b1aa459ddf5b7be00b9d32432e6dda97143e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-named-authentication-credentials"></a>Nastavení s názvem pověření pro ověření
K publikování aplikace do Azure ze sady Visual Studio nebo k monitorování stávající cloudovou službu, je nutné zadat přihlašovací údaje, které Visual Studio můžete použít k ověřování žádostí Azure. Existuje několik míst v sadě Visual Studio, kde se můžete přihlásit zadejte tyto přihlašovací údaje. Například z Průzkumníka serveru můžete otevřít místní nabídky **Azure** uzel a vyberte možnost **připojit k předplatnému Microsoft Azure**. Při přihlášení, informace o odběru, který je spojen s vaším účtem Azure je k dispozici v sadě Visual Studio. Není nic, které další je nutné provést.

Nástroje Azure také podporuje starší způsob, jak poskytnout pověření: pomocí souboru předplatné (soubor .publishsettings). Tento článek popisuje tuto metodu, který ještě není podporovaný v Azure SDK 2.2.

Následující položky jsou nutné k ověření do Azure:

* Vaše ID odběru
* Platný certifikát X.509 v3

> [!NOTE]
> Délka klíče certifikát X.509 v3 musí být alespoň 2 048 bitů. Azure odmítne libovolný certifikát, který nesplňuje tento požadavek nebo který není platný.
>
>

Visual Studio použije svoje ID předplatného společně s data certifikátu jako přihlašovací údaje. Příslušná pověření se odkazuje v souboru předplatné (soubor .publishsettings), který obsahuje veřejný klíč pro certifikát. Soubor odběru může obsahovat přihlašovací údaje pro více než jedno předplatné.

Můžete upravit informace o předplatném z **nové předplatné** nebo **upravit předplatné** dialogové okno, jak je popsáno dále v tomto článku.

Pokud chcete vytvořit certifikát, najdete pokyny v [vytvoření a nahrání certifikátu správy pro Azure](https://msdn.microsoft.com/library/windowsazure/gg551722.aspx) a ručně odeslat certifikát, který chcete [portál Azure classic](http://go.microsoft.com/fwlink/?LinkID=213885).

> [!NOTE]
> Tyto přihlašovací údaje, které sada Visual Studio vyžaduje ke správě vašich cloudových službách nejsou stejné přihlašovací údaje, které jsou nutné k ověření požadavku vůči služby Azure storage.
>
>

## <a name="import-set-up-or-edit-authentication-credentials-in-visual-studio"></a>Import, nastavit nebo upravit pověření pro ověření v sadě Visual Studio

1. V Průzkumníku serveru otevřete místní nabídku pro **Azure** uzel a vyberte možnost **spravovat a odběry filtru**.
2. Vyberte **certifikáty** kartě a pak použijte některou z následujících metod:

    * Vyberte **Import** otevřete **předplatná Microsoft Azure Import** dialogové okno. Zde si můžete stáhnout soubor odběry pro aktuálně načtených předplatné, přejděte do jeho umístění stahování a poté importovat pro použití při ověřování.
    * Vyberte **nový** otevřete **nové předplatné** dialogové okno. Zde můžete nastavit nový odběr pro použití v ověřování.
    * Vyberte **upravit** (po volbě aktivní předplatné) Chcete-li otevřít **upravit předplatné** dialogové okno. Zde můžete upravit stávající předplatné pro použití při ověřování. 

Následující postup předpokládá, že **nové předplatné** dialogového okna.

### <a name="to-set-up-authentication-credentials-in-visual-studio"></a>K nastavení pověření pro ověření v sadě Visual Studio
1. V **vybrat existující certifikát pro ověřování** vyberte certifikát.
2. Vyberte **zkopírovat úplnou cestu** odkaz. Cesta pro certifikát (soubor .cer) je zkopírován do schránky.

   > [!IMPORTANT]
   > Pokud chcete publikovat aplikaci Azure ze sady Visual Studio, musíte nahrát tohoto certifikátu [portál Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   >
   >
3. Na kterou certifikát nahrajete do portálu Azure:

   a. Otevřete web [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
   
   b. Pokud se zobrazí výzva, přihlaste se k portálu a potom vyhledejte **nastavení** > **certifikáty pro správu**.
   
   c. V **certifikáty pro správu** podokně, vyberte **nahrát**.
   
   d. Vyberte předplatné Azure, vložte úplnou cestu soubor .cer, který jste právě vytvořili a vyberte **nahrát**.

## <a name="next-steps"></a>Další kroky
* [Obecný přehled Web Apps](https://docs.microsoft.com/azure/app-service/)
* [Nasazení aplikace do služby Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/app-service-deploy-local-git) 
* [Nasazení WebJobs pomocí sady Visual Studio](https://docs.microsoft.com/en-us/azure/app-service/websites-dotnet-deploy-webjobs)
* [Vytvoření a nasazení cloudové služby](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy-portal)