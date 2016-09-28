<properties
   pageTitle="Konfigurace ověřování pomocí Amazon Web Services | Microsoft Azure"
   description="Tento článek popisuje postup vytvoření a ověření přihlašovacích údajů Amazon Web Services (AWS) pro runbooky ve službě Azure Automation, které spravují prostředky AWS."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="ověřování aws, konfigurace aws"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>


# Ověření runbooků pomocí Amazon Web Services
Automatizaci běžných úkolů pomocí prostředků ve službě Amazon Web Services můžete provést pomocí runbooků Automation v Azure.  V AWS můžete automatizovat celou řadu úloh pomocí runbooků Automation, stejně to děláte s prostředky v Azure.  Potřebujete jen dvě věci:

* Předplatné AWS a sadu přihlašovacích údajů.  Zejména přístupový klíč a tajný klíč AWS.  Další informace najdete v článku [Použití přihlašovacích údajů AWS](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Předplatné Azure a účet Automation.  Další informace o nastavení účtu Azure Automation najdete v článku [Konfigurace účtu Spustit v Azure jako](../automation/automation-sec-configure-azure-runas-account.md).  

Abyste mohli ověřovat pomocí AWS, musíte zadat sadu přihlašovacích údajů AWS a ověřit svoje runbooky spuštěné ve službě Azure Automation. Pokud už máte vytvořený účet Automation a chcete ho použít k ověřování pomocí AWS, postupujte podle návodu v následující části.  Pokud chcete mít vyhrazený účet pro runbooky, které se budou zaměřovat na prostředky AWS, vytvořte nejdřív nový [účet Spustit v Automation jako](../automation/automation-sec-configure-azure-runas-account.md) (vynechte možnost vytvořit objekt služby) a potom postupujte podle následujících kroků.

## Konfigurace účtu Automation
Pokud má Azure Automation komunikovat s AWS, musíte nejdřív načíst přihlašovací údaje AWS a uložit je jako assety ve službě Azure Automation.  Proveďte následující kroky, které jsou popsány v dokumentu AWS [Správa přístupových klíčů k vašemu účtu AWS](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html), vytvořte přístupový klíč a zkopírujte **Access Key ID** (ID přístupového klíče) a **Secret Access Key** (Tajný přístupový klíč) (případně si můžete soubor s klíčem stáhnout a uložit na bezpečné místo).

Po vytvoření a zkopírování bezpečnostních klíčů AWS vytvořte pomocí účtu Azure Automation asset přihlašovacích údajů, bezpečně klíče uložte a odkazujte na ně ve svých runboocích.  Postupujte podle kroků v části **Vytvoření nových přihlašovacích údajů** v článku [Assety přihlašovacích údajů ve službě Azure Automation](../automation/automation-certificates.md/###To create a new credential with the Azure portal) a zadejte následující informace:

1. Do pole **Název** zadejte **AWScred** nebo odpovídající hodnotu v souladu se svými standardy pro vytváření názvů.  
2. Do pole **Uživatelské jméno** zadejte svoje **Přístupové ID**, do polí **Heslo** a **Potvrzení hesla** zadejte svůj **Tajný přístupový klíč**.   

## Další kroky

- Další informace o vytváření runbooků pro automatizaci úloh v AWS najdete v článku [Automatizace nasazení virtuálního počítače ve službě Amazon Web Services](../automation/automation-scenario-aws-deployment.md).



<!--HONumber=Sep16_HO3-->


