---
title: "Konfigurace ověřování pomocí Amazon Web Services | Dokumentace Microsoftu"
description: "Tento článek popisuje postup vytvoření a ověření přihlašovacích údajů Amazon Web Services (AWS) pro runbooky ve službě Azure Automation, které spravují prostředky AWS."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: "ověřování aws, konfigurace aws"
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 2064c081da6d72ab15fb6cf72ba9d18a74a67fdd
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Ověření runbooků pomocí Amazon Web Services
Automatizaci běžných úkolů pomocí prostředků ve službě Amazon Web Services můžete provést pomocí runbooků Automation v Azure.  V AWS můžete automatizovat celou řadu úloh pomocí runbooků Automation, stejně to děláte s prostředky v Azure.  Potřebujete jen dvě věci:

* Předplatné AWS a sadu přihlašovacích údajů.  Zejména přístupový klíč a tajný klíč AWS.  Další informace najdete v článku [Použití přihlašovacích údajů AWS](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Předplatné Azure a účet Automation.  Další informace o nastavení účtu Azure Automation najdete v tématu popisujícím [plánování ověřování](automation-offering-get-started.md#authentication-planning).  

Abyste mohli ověřovat pomocí AWS, musíte zadat sadu přihlašovacích údajů AWS a ověřit svoje runbooky spuštěné ve službě Azure Automation. Pokud už máte vytvořený účet Automation a chcete ho použít k ověřování pomocí AWS, postupujte podle návodu v následující části.  Pokud chcete vyhradit účet pro runbooky, které se budou zaměřovat na prostředky AWS, vytvořte nejprve nový [účet Automation](automation-offering-get-started.md) (vynechte možnost vytvořit instanční objekt) a potom postupujte podle následujících kroků.

## <a name="configure-automation-account"></a>Konfigurace účtu Automation
Pokud má Azure Automation komunikovat s AWS, musíte nejdřív načíst přihlašovací údaje AWS a uložit je jako assety ve službě Azure Automation.  Proveďte následující kroky, které jsou popsány v dokumentu AWS [Správa přístupových klíčů k vašemu účtu AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html), vytvořte přístupový klíč a zkopírujte **Access Key ID** (ID přístupového klíče) a **Secret Access Key** (Tajný přístupový klíč) (případně si můžete soubor s klíčem stáhnout a uložit na bezpečné místo).

Po vytvoření a zkopírování bezpečnostních klíčů AWS vytvořte pomocí účtu Azure Automation asset přihlašovacích údajů, bezpečně klíče uložte a odkazujte na ně ve svých runboocích.  Postupujte podle kroků v části **Vytvoření nových přihlašovacích údajů** v článku [Assety přihlašovacích údajů ve službě Azure Automation](automation-credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) a zadejte následující informace:

1. Do pole **Název** zadejte **AWScred** nebo odpovídající hodnotu v souladu se svými standardy pro vytváření názvů.  
2. Do pole **Uživatelské jméno** zadejte své **ID přístupu** a do polí **Heslo** a **Potvrzení hesla** zadejte svůj **Tajný přístupový klíč**.   

## <a name="next-steps"></a>Další kroky
* Další informace o vytváření runbooků pro automatizaci úloh v AWS najdete v článku [Automatizace nasazení virtuálního počítače ve službě Amazon Web Services](automation-scenario-aws-deployment.md).

