---
title: "Nainstalujte MongoDB v systému Windows virtuálního počítače v Azure | Microsoft Docs"
description: "Informace o instalaci MongoDB ve virtuálním počítači Azure vytvořené pomocí modelu nasazení classic systémem Windows Server."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 4095df41-bb69-4bbe-9c1c-70923b0d84ba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: iainfou
ms.openlocfilehash: d70194f30b7866cab38cfbbe1ea06664b0ddc1f5
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/08/2017
---
# <a name="install-mongodb-on-a-windows-vm-in-azure"></a>Nainstalujte MongoDB v systému Windows virtuálního počítače v Azure
> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).  Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Instalace a konfigurace MongoDB pomocí modelu nasazení Resource Manager, najdete v části [v tomto článku](../install-mongodb.md).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

[MongoDB] [ MongoDB] je Oblíbené databáze NoSQL open source a vysoce výkonné. Tento článek vás provede vytvořením virtuální počítač (VM) Windows serveru pomocí [portál Azure][AzurePortal]. Vytvořit a připojit datový disk k virtuálnímu počítači před instalací a konfigurací MongoDB. Pokud máte existující virtuální počítač v Azure, který chcete použít, můžete přejít rovnou na [instalace a konfigurace MongoDB](#install-and-run-mongodb-on-the-virtual-machine).

## <a name="create-a-virtual-machine-running-windows-server"></a>Vytvoření virtuálního počítače se systémem Windows Server
Postupujte podle těchto pokynů můžete vytvořit virtuální počítač.

[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

> [!NOTE]
> Přidání koncového bodu pro MongoDB při vytváření virtuálního počítače a nakonfigurovat následujícím způsobem: pojmenujte ji jako **Mongo**, použijte **TCP** jako protokol a nastavte veřejné a privátní porty na **27017**.
>
>

## <a name="attach-a-data-disk"></a>Připojení datového disku
K poskytování úložiště pro virtuální počítač, připojit datový disk a potom inicializujte tak, aby Windows můžete používat. Pokud již máte datový disk, můžete připojit tento stávající disk, nebo můžete připojte prázdný disk.

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-windows-linux.md)]

Pokyny pro inicializaci disku, v tématu "postup: Inicializace nový datový disk ve Windows serveru" v [jak připojit datový disk k virtuálnímu počítači Windows](attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Nainstalujte a spusťte MongoDB na virtuálním počítači
[!INCLUDE [install-and-run-mongo-on-win2k8-vm](../../../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Souhrn
V tomto kurzu jste zjistili, jak k vytvoření virtuálního počítače se systémem Windows Server, vzdáleně připojit k němu a připojit datový disk.  Také jste zjistili, jak nainstalovat a nakonfigurovat MongoDB na virtuálním počítači systému Windows. Nyní můžete k MongoDB na virtuálním počítači systému Windows, pomocí následujících Pokročilá témata v [dokumentace k MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: https://portal.azure.com/

