---
title: "Řešení potíží s přidělováním virtuálního počítače s Linuxem | Microsoft Docs"
description: "Řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálního počítače s Linuxem v Azure"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/03/2016
ms.author: cjiang
ms.openlocfilehash: 8cf421afe9b41af050152dc965d42e7809b5f2d9
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálních počítačů Linux v Azure
Při vytvoření virtuálního počítače, restartujte zastaveném (deallocated) virtuálních počítačů nebo změnit velikost virtuálního počítače Microsoft Azure přiřadí výpočetní prostředky do vašeho předplatného. Příležitostně zobrazí chyby při provádění těchto operací – ještě před dosažením limity předplatného Azure. Tento článek vysvětluje příčiny některých běžných chyb přidělení a navrhne možné nápravu. Informace mohou být užitečné také při plánování nasazení vašich služeb. Můžete také [řešení potíží s přidělením při vytváření, restartování nebo změně velikosti virtuálních počítačů Windows v Azure](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

