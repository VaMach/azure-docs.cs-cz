---
title: "Nasazení řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak zřídit předkonfigurovaného řešení vzdáleného monitorování z azureiotsuite.com."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/22/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: fa08817c870372488b1e03a2c282add2eaa85478
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>Nasazení předkonfigurovaného řešení vzdáleného monitorování

V tomto kurzu se dozvíte, jak zřídit předkonfigurované řešení vzdáleného monitorování. Nasazení řešení od azureiotsuite.com. Můžete taky nasadit řešení pomocí rozhraní příkazového řádku, další informace o této možnosti najdete [nasadit předkonfigurované řešení z příkazového řádku](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploy-a-pcs-from-the-command-line).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurace předkonfigurované řešení
> * Nasadit předkonfigurované řešení
> * Přihlaste se k předkonfigurované řešení

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.

Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](http://azure.microsoft.com/pricing/free-trial/).

## <a name="deploy-the-preconfigured-solution"></a>Nasadit předkonfigurované řešení

Než nasadíte předkonfigurované řešení k předplatnému Azure, musíte zvolit některé možnosti konfigurace:

1. Přihlaste se k [azureiotsuite.com](https://www.azureiotsuite.com) pomocí vaší Azure přihlašovací údaje účtu a klikněte na tlačítko  **+**  k vytvoření nové řešení:

    ![Vytvořit nové řešení](media/iot-suite-remote-monitoring-deploy/createnewsolution.png)

1. Klikněte na tlačítko **vyberte** na **vzdálené monitorování preview** dlaždici.

    ![Zvolte vzdálené monitorování](media/iot-suite-remote-monitoring-deploy/remotemonitoring.png)

1. Na **vytvořit vzdálené monitorování řešení** stránky, zadejte **název řešení** pro předkonfigurované řešení vzdáleného monitorování.

1. Vyberte **základní** nebo **standardní** nasazení. Pokud nasazujete řešení se dozvíte, jak to funguje nebo chcete spustit ukázku, zvolte **základní** možnost minimalizovat náklady.

1. Vyberte buď **Java** nebo **.NET** jako jazyk. Všechny mikroslužeb jsou k dispozici jako implementace Java nebo .NET.

1. Zkontrolujte **podrobnosti řešení** panel pro další informace o možnostech konfigurace.

1. Vyberte **Předplatné** a **Oblast**, které chcete při zřizování řešení použít.

1. Kliknutím na tlačítko **Vytvořit řešení** zahájíte proces zřizování. Tento proces obvykle trvá několik minut:

    ![Podrobnosti o řešení vzdáleného](media/iot-suite-remote-monitoring-deploy/createform.png)

Informace o odstraňování potíží, najdete v části [co dělat, když se nasazení nezdaří](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) v úložišti GitHub.

## <a name="sign-in-to-the-preconfigured-solution"></a>Přihlaste se k předkonfigurované řešení

Po dokončení procesu zřizování se můžete přihlásit předkonfigurované řešení vzdáleného monitorování.

1. Na **zřízení řešení** vyberte vaší nové řešení vzdáleného monitorování:

    ![Zvolte nové řešení](media/iot-suite-remote-monitoring-deploy/choosenew.png)

1. Zobrazení informací o řešení vzdáleného monitorování v panelu, který se zobrazí. Zvolte **řídicí panel řešení** pro připojení k řešení vzdáleného monitorování.

    > [!NOTE]
    > Když skončíte s ním můžete odstranit řešení vzdáleného monitorování z tohoto panelu.

    ![Panel řešení](media/iot-suite-remote-monitoring-deploy/solutionpanel.png)

1. Řídicím panelu řešení vzdáleného monitorování se zobrazí v prohlížeči.

## <a name="next-steps"></a>Další postup

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Konfigurace předkonfigurované řešení
> * Nasadit předkonfigurované řešení
> * Přihlaste se k předkonfigurované řešení

Teď, když jste nasadili řešení vzdáleného monitorování, dalším krokem je [prozkoumejte možnosti řídicí panel řešení](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->