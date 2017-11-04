---
title: "Přihlaste se k Azure z rozhraní příkazového řádku | Microsoft Docs"
description: "Připojení k předplatnému Azure z rozhraní příkazového řádku (Azure CLI) pro Mac, Linux a Windows"
editor: tysonn
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: ed856527-d75e-4e16-93fb-253dafad209d
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 10/04/2016
ms.author: rasquill
"\"/": 
ms.openlocfilehash: 31efab60690b54faf7992251fcd01e307c4464f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="log-in-to-azure-from-the-azure-cli"></a>Přihlaste se k Azure z rozhraní příkazového řádku Azure
Rozhraní příkazového řádku Azure je sada příkazů open source, a platformy pro práci s prostředky Azure. Tento článek popisuje různé způsoby, jak poskytnout přihlašovací údaje účtu Azure připojit rozhraní příkazového řádku Azure k předplatnému Azure:

* Spustit `azure login` rozhraní příkazového řádku příkaz k ověření pomocí služby Azure Active Directory. Tato metoda umožňuje přístup k rozhraní příkazového řádku v obou [příkaz režimy](#cli-command-modes). Při spuštění příkazu bez dalších možností, `azure login` vás vyzve, abyste pokračovat interaktivní přihlašování prostřednictvím webového portálu. Pro další `azure login` příkaz Možnosti najdete v tématu scénáře v tomto článku nebo typ `azure login --help`.
* Pokud potřebujete použít příkazy rozhraní příkazového řádku v režimu Azure Service Management (nedoporučuje se většina nových nasazení), můžete stáhnout a nainstalovat soubor nastavení publikování ve vašem počítači.

Pokud jste ještě nenainstalovali rozhraní příkazového řádku, přečtěte si téma [nainstalovat Azure CLI](cli-install-nodejs.md). Pokud nemáte předplatné Azure, můžete si [bezplatně vytvořit účet](http://azure.microsoft.com/free/) během několika minut.

Informace o jiný účet identity a předplatná Azure, najdete v části [asociování předplatných Azure se službou Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

## <a name="scenario-1-azure-login-with-interactive-login"></a>Scénář 1: přihlášení k azure s interaktivní přihlášení
S určitým účtům rozhraní příkazového řádku vyžaduje, abyste spustili `azure login` a potom pokračujte v procesu přihlášení s webovým prohlížečem, přes webový portál, procesu označovaného jako *interaktivní přihlášení*. Obvyklým důvodem je, když máte pracovní nebo školní účet (také nazývané *účet organizace*), je nastavena na požadavek vícefaktorového ověřování. Pokud chcete používat příkazy v režimu Resource Manager také používejte interaktivní přihlášení pomocí účtu Microsoft.

Interaktivní přihlášení je snadné: typ `azure login` – bez jakékoli možnosti – jak je znázorněno v následujícím příkladu:

```
azure login
```                                                                                             

Výstup vypadá přibližně takto:

```         
info:    Executing command login
info:    To sign in, use a web browser to open the page http://aka.ms/devicelogin. Enter the code XXXXXXXXX to authenticate.
```
Zkopírujte kód nabízena ve výstupu příkazu a otevřete prohlížeč na http://aka.ms/devicelogin nebo jinou stránku, pokud zadaný. (Můžete otevřít prohlížeč na stejném počítači nebo na jiný počítač nebo zařízení.) Zadejte kód a potom zobrazí výzva k zadání uživatelského jména a hesla pro identitu, kterou chcete použít. Po dokončení tohoto procesu příkazové okno dokončení přihlášení. Ho může vypadat podobně jako:

    info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Added subscription Azure Free Trial
    info:    Setting subscription "Visual Studio Ultimate with MSDN" as default
    +
    info:    login command OK

> [!NOTE]
> S interaktivní přihlášení ověřování a autorizace se provádí pomocí služby Azure Active Directory. Pokud chcete použít identitu účtu Microsoft, přistupuje k procesu přihlášení výchozí doménu služby Azure Active Directory. (Pokud jste zaregistrovali bezplatný účet Azure, Azure Active Directory automaticky vytvoří výchozí doménu pro účet.)
>
>

## <a name="scenario-2-azure-login-with-a-username-and-password"></a>Scénář 2: azure přihlášení pomocí uživatelského jména a hesla
Použít `azure login` s uživatelské jméno (`-u`) parametr k ověření, když chcete použít pracovní nebo školní účet, který nevyžaduje vícefaktorového ověřování. Zobrazení výzvy na příkazovém řádku hesla (nebo můžete volitelně předat heslo jako další parametr `azure login` příkaz). Následující příklad předá uživatelské jméno účtu organizace:

    azure login -u myUserName@contoso.onmicrosoft.com

Potom budete vyzváni k zadání hesla:

    info:    Executing command login
    Password: *********

Potom dokončí proces přihlášení.

    info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

Pokud je vaše první protokolování v čase se pomocí těchto přihlašovacích údajů, zobrazí se výzva k ověření, které chcete ověřovací token do mezipaměti. Tato výzva také nastat, pokud jste dříve používali `azure logout` příkazu (popsaný v článku později). Chcete-li vynechat tuto výzvu pro scénáře automatizace, spusťte `azure login` s `-q` parametr.

## <a name="scenario-3-azure-login-with-a-service-principal"></a>Scénář 3: přihlášení k azure pomocí objektu služby
Pokud vytvoříte objekt služby pro aplikaci služby Active Directory a objektu služby má oprávnění pro vaše předplatné, můžete použít `azure login` příkaz k ověření objektu služby. V závislosti na scénáři, je možné zadat přihlašovací údaje objektu služby jako explicitní parametry `azure login` příkaz. Například následující příkaz předává hlavní název služby a služby Active Directory ID klienta:

    azure login -u https://www.contoso.org/example --service-principal --tenant myTenantID

Zobrazí se výzva k zadání hesla. Můžete také zadat přihlašovací údaje prostřednictvím rozhraní příkazového řádku skriptu nebo aplikace kódu nebo používat certifikát pro ověření instanční objekt neinteraktivně pro scénáře automatizace. Podrobnosti a příklady naleznete v tématu [ověřování hlavní název služby pomocí Azure Resource Manageru](resource-group-authenticate-service-principal-cli.md).

## <a name="scenario-4-use-a-publish-settings-file"></a>Scénář 4: Použijte soubor nastavení publikování
Pokud potřebujete použít příkazy rozhraní příkazového řádku režimu Azure Service Management (například k nasazení virtuálních počítačů Azure v modelu nasazení classic), se můžete připojit pomocí soubor nastavení publikování. Tato metoda nainstaluje certifikát na místním počítači, který umožňuje provádět úlohy správy pro, dokud jsou platné předplatné a certifikát.

* **Chcete-li stáhnout soubor nastavení publikování** pro váš účet, zajistěte, aby rozhraní příkazového řádku v režimu správy služby zadáním `azure config mode asm`. Spusťte následující příkaz:

        azure account download

To otevře výchozí prohlížeč a zobrazí výzvu pro přihlášení k aplikaci [portál Azure classic](https://manage.windowsazure.com). Po přihlášení, `.publishsettings` stahování souborů. Poznamenejte si, kam se soubor uloží.

> [!NOTE]
> Pokud váš účet je spojen s více klienty Azure Active Directory, budete vyzváni k výběru služby Active Directory, které chcete stáhnout soubor nastavení publikování pro.
>
>

Po výběru pomocí stránce pro stahování, nebo pomocí portálu Azure classic, vybrané služby Active Directory se změní na výchozí hodnotu použitou classic stránka portálu a stahování. Po vytvoření výchozí uvidíte text '**kliknutím sem se vrátíte na stránku výběr**se v horní části stránky pro stažení. Použijte zadaný odkaz a návrat na stránku výběr.

* **Chcete-li importovat soubor nastavení publikování**, spusťte následující příkaz:

        azure account import <path to your .publishsettings file>

> [!IMPORTANT]
> Po importu vaše nastavení publikování, měli byste odstranit `.publishsettings` souboru. To už není nutné pomocí rozhraní příkazového řádku Azure a představuje bezpečnostní riziko, protože ho může použít k získání přístupu k vašemu předplatnému.
>
>

## <a name="cli-command-modes"></a>Režimy rozhraní příkazového řádku příkaz
Rozhraní příkazového řádku Azure obsahuje dva režimy příkaz pro práci s prostředky Azure, se sadami jiný příkaz:

* **Režimu Resource Manager** – pro práci s prostředky Azure v modelu nasazení Resource Manager. Pokud chcete nastavit tento režim, spusťte `azure config mode arm`.
* **Režim správy služby** – pro práci s prostředky Azure v modelu nasazení classic. Pokud chcete nastavit tento režim, spusťte `azure config mode asm`.

Při první instalaci aktuální verzi rozhraní příkazového řádku je v režimu Resource Manager.

> [!NOTE]
> Režim Resource Manager a Service Management režim se vzájemně vylučují. To znamená nelze spravovat prostředky vytvořené v jednom režimu z jiných režimu.
>
>

## <a name="multiple-subscriptions"></a>Více předplatných
Pokud máte víc předplatných Azure, připojení k Azure udělí přístup k Všechna předplatná spojená s přihlašovacích údajů. Jedno předplatné, je vybrána jako výchozí a používat rozhraní příkazového řádku Azure při provádění operací. Můžete zobrazit předplatná, včetně aktuální výchozí předplatné, pomocí `azure account list` příkaz. Tento příkaz vrátí informace podobná této:

    info:    Executing command account list
    data:    Name              Id                                    Current
    data:    ----------------  ------------------------------------  -------
    data:    Azure-sub-1       ####################################  true
    data:    Azure-sub-2       ####################################  false

V předchozím seznamu **aktuální** sloupec zobrazuje informace o aktuálním předplatném výchozí jako Azure-sub-1. Chcete-li změnit výchozí předplatné, použijte `azure account set` příkaz a určete předplatné, které chcete nastavit jako výchozí. Například:

    azure account set Azure-sub-2

Tato operace změní výchozí předplatné na Azure-sub-2.

> [!NOTE]
> Změna výchozí předplatné se okamžitě projeví a globální změně; nové příkazy rozhraní příkazového řádku Azure, zda je spouštět z příkazového řádku stejnou instanci nebo jinou instanci, použijte nové předplatné výchozí.
>
>

Pokud chcete použít jiné než výchozí předplatné s Azure CLI, ale nechcete změnit aktuální výchozí nastavení, můžete použít `--subscription` možnost pro příkaz a zadejte název odběru, které chcete použít pro operaci.

Jakmile se připojíte ke svému předplatnému Azure, budete moct začít, používat příkazy rozhraní příkazového řádku Azure pro práci s prostředky Azure.

## <a name="storage-of-cli-settings"></a>Ukládání nastavení rozhraní příkazového řádku
Jestli přihlášení s `azure login` příkaz nebo importu nastavení publikování, profil rozhraní příkazového řádku a protokoly jsou uloženy v `.azure` adresář umístěný ve vaší `user` adresáře. Vaše `user` adresář je chráněn váš operační systém. Doporučujeme však, že je provést další kroky k šifrování vašeho `user` adresáře. Můžete tak učinit následujícími způsoby:

* V systému Windows upravit vlastnosti directory nebo pomocí nástroje BitLocker.
* V systému Mac zapněte FileVault pro adresář.
* V Ubuntu použijte funkci Šifrovaný domovský adresář. Další Linuxových distribucích nabízí podobné funkce.

## <a name="logging-out"></a>Protokolování se
Chcete-li odhlášení, použijte následující příkaz:

    azure logout -u <username>

Pokud předplatné přidružené k účtu jsou pouze ověření pomocí služby Active Directory, protokolování se odstraní informace o předplatném z místního profilu. Ale pokud soubor nastavení publikování byl importován také pro odběry, protokolování jen odstranění služby Active Directory související informace z místního profilu.

## <a name="next-steps"></a>Další kroky
* Pomocí rozhraní příkazového řádku Azure naleznete v části [rozhraní příkazového řádku Azure v režimu Resource Manager](virtual-machines/azure-cli-arm-commands.md) a [rozhraní příkazového řádku Azure v režimu správy služby](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).
* Další informace o Azure CLI, stažení zdrojový kód, problémy sestavy, nebo můžete přispět k projektu, najdete [úložiště GitHub pro rozhraní příkazového řádku Azure](https://github.com/azure/azure-xplat-cli).
* Pokud narazíte na problémy pomocí rozhraní příkazového řádku Azure nebo Azure, přejděte [fóra Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).
