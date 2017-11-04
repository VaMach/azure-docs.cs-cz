---
title: "Nasazení řešení vzdáleného monitorování - Azure | Microsoft Docs"
description: "V tomto kurzu se dozvíte, jak zřídit předkonfigurovaného řešení vzdáleného monitorování z azureiotsuite.com."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 08/09/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: b16990d41b691c13b01d61aa0cde7d14e533e440
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="deploy-the-remote-monitoring-preconfigured-solution"></a>Nasadit předkonfigurované řešení vzdáleného monitorování

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

1. Přihlaste se k [azureiotsuite.com](https://www.azureiotsuite.com) pomocí vaší Azure přihlašovací údaje účtu a klikněte na tlačítko  **+**  vytvářet řešení.

1. Klikněte na tlačítko **Vybrat**, které se nachází na dlaždici **Vzdálené monitorování**.

1. Na **vytvořit vzdálené monitorování řešení** stránky, zadejte **název řešení** pro předkonfigurované řešení vzdáleného monitorování.

1. Vyberte **základní** nebo **standardní** nasazení. Pokud jsou deplying řešení se dozvíte, jak to funguje nebo chcete spustit ukázku, zvolte **základní** možnost minimalizovat náklady.

1. Vyberte buď **Java** nebo **.NET** jako jazyk. Všechny mikroslužeb jsou k dispozici jako implementace Java nebo .NET.

1. Zkontrolujte **podrobnosti řešení** panel pro další informace o možnostech konfigurace.

1. Vyberte **Předplatné** a **Oblast**, které chcete při zřizování řešení použít.

1. Kliknutím na tlačítko **Vytvořit řešení** zahájíte proces zřizování. Tento proces obvykle trvá několik minut.

Informace o odstraňování potíží, najdete v části [co dělat, když se nasazení nezdaří](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide#what-to-do-when-a-deployment-fails) v úložišti GitHub.

## <a name="sign-in-to-the-preconfigured-solution"></a>Přihlaste se k předkonfigurované řešení

Po dokončení procesu zřizování se můžete přihlásit předkonfigurované řešení vzdáleného monitorování.

1. Na **zřízení řešení** vyberte vaší nové řešení vzdáleného monitorování.

1. Zobrazení informací o řešení vzdáleného monitorování v panelu, který se zobrazí. Zvolte **řídicí panel řešení** pro připojení k řešení vzdáleného monitorování.

    > [!NOTE]
    > Když skončíte s ním můžete odstranit řešení vzdáleného monitorování z tohoto panelu.

1. Řídicím panelu řešení vzdáleného monitorování se zobrazí v prohlížeči.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Konfigurace předkonfigurované řešení
> * Nasadit předkonfigurované řešení
> * Přihlaste se k předkonfigurované řešení

Teď, když jste nasadili řešení vzdáleného monitorování, dalším krokem je [prozkoumejte možnosti řídicí panel řešení](./iot-suite-remote-monitoring-explore.md).

<!-- Next tutorials in the sequence -->