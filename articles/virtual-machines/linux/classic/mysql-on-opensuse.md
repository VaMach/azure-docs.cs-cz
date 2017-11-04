---
title: "Instalace databáze MySQL na virtuální počítač OpenSUSE | Microsoft Docs"
description: "Další informace pro instalaci databáze MySQL na počítači OpenSUSE Linux VMirtual v Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/19/2016
ms.author: cynthn
ms.openlocfilehash: 01b798a25575b66f89057315ce80d6cc0cde53b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Instalace MySQL do virtuálního počítače se spuštěným OpenSUSE Linuxem v Azure
[MySQL] [ MySQL] je Oblíbené databáze SQL open source. V tomto kurzu se dozvíte, jak vytvořit virtuální počítač s Linuxem OpenSUSE a pak nainstalujte MySQL.

> [!IMPORTANT] 
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Classic. Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.

<br>

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Vytvoření virtuálního počítače se systémem OpenSUSE Linux
[!INCLUDE [create-and-configure-opensuse-vm-in-portal](../../../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Nainstalujte a spusťte MySQL na virtuálním počítači
[!INCLUDE [install-and-run-mysql-on-opensuse-vm](../../../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Další kroky
Podrobnosti o MySQL najdete v tématu [MySQL dokumentaci][MySQLDocs].

[MySQLDocs]:http://dev.mysql.com/doc/index-topic.html
[MySQL]:http://www.mysql.com

