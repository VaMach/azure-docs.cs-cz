---
title: "Jak používat přiřazený uživatelem spravovat Identity služby ze sady SDK služby Azure na virtuálním počítači"
description: "Ukázky kódu pro použití sady SDK služby Azure s MSI přiřazený uživatelem na virtuálním počítači."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 59d65e42c9b32bd0acd98645342833b4d57ad7a4
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="use-azure-sdks-with-a-user-assigned-managed-service-identity-msi"></a>Pomocí sady Azure SDK s přiřazený uživatelem spravované služby Identity (MSI)

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]
Tento článek obsahuje seznam ukázky sady SDK, které ukazují použití podpory jejich odpovídajících Azure SDK pro MSI přiřazený uživatelem.

## <a name="prerequisites"></a>Požadavky

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

> [!IMPORTANT]
> - Všechny ukázkový kód nebo skript v tomto článku předpokládá klienta běží na virtuálním počítači povolená MSI. Použijte funkci "Připojit" virtuální počítač na portálu Azure se vzdáleně připojit k virtuálnímu počítači. Informace o povolení MSI na virtuálním počítači, v [konfigurace virtuálních počítačů spravovaných služba Identity (MSI) pomocí rozhraní příkazového řádku Azure](msi-qs-configure-cli-windows-vm.md), nebo jeden z typu variant článků (pomocí prostředí PowerShell, portálu Azure, šablonu nebo Azure SDK). 

## <a name="sdk-code-samples"></a>Ukázky kódu SDK

| Sada SDK             | Ukázka kódu |
| --------------- | ----------- |
| Python          | [K ověření jednoduše z uvnitř virtuálního počítače použít MSI](https://azure.microsoft.com/resources/samples/resource-manager-python-manage-resources-with-msi/) |
| Ruby            | [Spravovat prostředky z virtuálního počítače povoleným MSI](https://azure.microsoft.com/resources/samples/resources-ruby-manage-resources-with-msi/) |

## <a name="next-steps"></a>Další postup

- V tématu [sady Azure SDK](https://azure.microsoft.com/downloads/) úplný seznam prostředků sady SDK Azure, včetně knihovny stahování, dokumentace a další.

Použijte následující sekci komentáře k poskytnutí zpětné vazby a Pomozte nám vylepšit a utvářejí náš obsah.








