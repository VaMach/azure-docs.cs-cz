---
title: "Vytvořte vlastní role řízení přístupu na základě Role a přiřaďte interních a externích uživatelů v Azure | Microsoft Docs"
description: "Přiřadit vlastní role RBAC vytvořené pomocí prostředí PowerShell a rozhraní příkazového řádku pro interních a externích uživatelů"
services: active-directory
documentationcenter: 
author: andreicradu
manager: catadinu
editor: kgremban
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2017
ms.author: a-crradu
ms.openlocfilehash: 213b02205bbe7f767b6aff6a0693bb34b97cb9ec
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/30/2017
---
# <a name="intro-on-role-based-access-control"></a>Úvod na řízení přístupu na základě rolí

Řízení přístupu na základě role je Azure portálu pouze funkce povolení vlastníky předplatného přiřadit granulární role jiným uživatelům, kteří mohou spravovat konkrétní prostředek obory ve svém prostředí.

RBAC umožňuje lepší zabezpečení správy pro velké organizace a pro SMB práce s externími spolupracovníky, dodavatele nebo freelancers, které potřebují přístup ke konkrétním prostředkům ve vašem prostředí, ale nemusí nutně prokázat celé infrastruktury nebo žádné související fakturace obory. RBAC umožňuje flexibilitu vlastnící jedno předplatné, které spravuje správce účtu (role Správce služby na úrovni předplatného) a pro práci v rámci stejného předplatného, ale bez jakékoli práva správce pro ni pozvali více uživatelů . Ze správy a fakturace perspektivy funkci RBAC prokáže, že se možnost efektivní čas a správy pro používání Azure v různých situacích.

## <a name="prerequisites"></a>Požadavky
Používání RBAC v prostředí Azure vyžaduje:

* Nutnosti samostatné předplatné přiřazeny uživateli jako vlastník (předplatné role)
* Mít roli vlastníka předplatného Azure
* K dispozici [portálu Azure](https://portal.azure.com)
* Zajistěte si následující zprostředkovatelé prostředků zaregistrovat pro předplatné uživatele: **Microsoft.Authorization**. Další informace o postupu při registraci zprostředkovatele prostředků najdete v tématu [zprostředkovatelé Resource Manager, oblastí, verzí rozhraní API a schémat](../azure-resource-manager/resource-manager-supported-services.md).

> [!NOTE]
> Předplatná Office 365 nebo Azure Active Directory licence (například: přístup k Azure Active Directory) zajištěného z O365 portál nemáte kvalifikaci pro pomocí RBAC.

## <a name="how-can-rbac-be-used"></a>RBAC použití
RBAC lze použít na tři různé rozsahy v Azure. Z oboru nejvyšší nejnižší tomu, že jsou následující:

* Předplatné (nejvyšší)
* Skupina prostředků
* Prostředek oboru (nejnižší úroveň přístupu nabídky cílové oprávnění v oboru jednotlivých prostředků Azure)

## <a name="assign-rbac-roles-at-the-subscription-scope"></a>Přiřadit role RBAC v oboru předplatného
Existují dvě běžných příkladů, když RBAC je použít (ale mimo jiné):

* Že externí uživatelé organizací pozvat (není součástí uživatele správce klienta Azure Active Directory) ke správě určitých prostředků nebo celý předplatného
* Práce s uživateli uvnitř organizace (jsou součástí klienta Azure Active Directory uživatele), ale součást různé týmy nebo skupin, které potřebují granulární přístup pro celé předplatné nebo pro určité skupiny prostředků nebo prostředek oborů v prostředí

## <a name="grant-access-at-a-subscription-level-for-a-user-outside-of-azure-active-directory"></a>Udělení přístupu na úrovni předplatného pro uživatele mimo Azure Active Directory
Role RBAC lze udělit pouze systémem **vlastníky** předplatného proto uživatel s oprávněními správce musíte být přihlášeni pomocí uživatelského jména, která má tato role předběžně zařazená nebo vytvořil předplatné Azure.

Z portálu Azure po přihlášení jako správce, vyberte možnost "Odběry" a vyberte požadované.
![okno odběru na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/0.png) ve výchozím nastavení, pokud uživatel s oprávněními správce koupil předplatné Azure, uživateli se zobrazí jako **správce účtu**, tím se roli předplatného. Další informace o rolích předplatné Azure, najdete v části [přidání nebo změna role Správce služby Azure, které spravují předplatné nebo služby](/billing/billing-add-change-azure-subscription-administrator.md).

V tomto příkladu uživatel "alflanigan@outlook.com" je **vlastníka** z "Bezplatnou zkušební verzi" předplatné v AAD klienta "Výchozí klienta Azure". Vzhledem k tomu, že je tento uživatel Tvůrce předplatného Azure se počáteční Account Microsoft "Outlook" (Account Microsoft = Outlook, Live atd.) bude výchozí název domény pro všechny uživatele přidán do tohoto klienta **"@alflaniganuoutlook.onmicrosoft.com"**. Návrh syntaxe nové domény je tvořen uvedení společně název uživatelské jméno a doménu uživatele, který vytvořil klienta a přidání rozšíření **". onmicrosoft.com"**.
Kromě toho uživatelé můžou přihlásit pomocí vlastního názvu domény v klientovi po přidání a ověření pro nového klienta. Další podrobnosti o tom, jak ověřit vlastní název domény v klienta služby Azure Active Directory najdete v tématu [přidání vlastního názvu domény do adresáře](/active-directory/active-directory-add-domain).

V tomto příkladu adresáři "Výchozí klient Azure" obsahuje pouze uživatele s názvem domény "@alflanigan.onmicrosoft.com".

Po výběru předplatného, musíte kliknout na uživatel s oprávněními správce **řízení přístupu (IAM)** a potom **přidat novou roli**.





![Funkce IAM řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/1.png)





![Přidání nového uživatele v IAM funkce řízení přístupu na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/2.png)

Dalším krokem je vybrat role, kterou chcete přiřadit a uživatel, kterému se přiřadí RBAC role. V **Role** rozevírací nabídce Uživatel s oprávněními správce vidí jenom integrovanou RBAC role, které jsou k dispozici v Azure. Podrobné vysvětlení jednotlivých rolí a jejich přiřaditelnými obory, najdete v části [předdefinované role pro řízení přístupu](role-based-access-built-in-roles.md).

Pak musí přidat e-mailovou adresu externího uživatele, uživatel s oprávněními správce. Očekávané chování je externí uživatel není zobrazena v existujícího klienta. Po pozval externí uživatel zadá budou viditelné v rámci **odběry > řízení přístupu (IAM)** s aktuální uživateli, které jsou přiřazeny role RBAC v obor předplatného.





![Přidejte oprávnění do nové role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/3.png)





![seznam rolí RBAC na úrovni předplatného](./media/role-based-access-control-create-custom-roles-for-internal-external-users/4.png)

Uživatel "chessercarlton@gmail.com" pozval být **vlastníka** pro předplatné "Bezplatnou zkušební verzi". Po odeslání pozvánky, obdrží externího uživatele potvrzení e-mailu s odkazem k aktivaci.
![e-mailová pozvánka pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/5.png)

Probíhá mimo organizaci, nový uživatel nemá žádné existující atributy v adresáři "Výchozí klient Azure". Budou vytvořeny po externího uživatele poskytl souhlas zaznamenávají v adresáři, který je přidružen k odběru, který byl přiřazen k roli.





![e-mailové zprávě pozvánky pro RBAC role](./media/role-based-access-control-create-custom-roles-for-internal-external-users/6.png)

Zobrazuje externí uživatel v klientovi Azure Active Directory od této chvíle jako externí uživatel a tato lze zobrazit na portálu Azure i na portálu classic.





![uživatelé okno azure active directory portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/7.png)





![uživatelé okno azure active directory portálu Azure classic](./media/role-based-access-control-create-custom-roles-for-internal-external-users/8.png)

V **uživatelé** zobrazení v obou portálů rozpoznal externí uživatele:

* Typ vlastní ikonu na portálu Azure
* Jiné zdrojové bod v portálu classic

Ale udělení **vlastníka** nebo **Přispěvatel** přístup k externím uživatelem v **předplatné** obor, neumožňuje přístup k adresáři uživatele správce, pokud **Globálního správce** to umožňuje. Ve vlastnosti uživatele **typ uživatele** jehož dvě společné parametry, **člen** a **hosta** lze identifikovat. Člen je uživatel, která je registrována v adresáři, zatímco hosta je uživatel vyzván k adresáři z externího zdroje. Další informace najdete v tématu [jak správci Azure Active Directory přidat uživatele spolupráce B2B](active-directory-b2b-admin-add-users.md).

> [!NOTE]
> Ujistěte se, že po zadání přihlašovacích údajů na portálu, externí uživatel vybere správný adresář, který má přihlášení k. Stejný uživatel můžete mít přístup k více adresářů a můžete vybrat některý z nich kliknutím uživatelské jméno v vpravo nahoře na portálu Azure a potom z rozevíracího seznamu vyberte příslušného adresáře.

Při se hostovaného v adresáři, externího uživatele můžete spravovat všechny prostředky pro předplatné Azure, ale nemůže získat přístup k adresáři.





![přístup omezen na portálu Azure azure active directory](./media/role-based-access-control-create-custom-roles-for-internal-external-users/9.png)

Azure Active Directory a předplatné Azure, nemají vztah nadřazený podřízený jako ostatní prostředky služby Azure (například: virtuálních počítačů, virtuálních sítí, webové aplikace, úložiště atd.) s předplatné Azure. Všechny pozdější je vytvořen, spravovat a účtují pod předplatným Azure, zatímco předplatné služby Azure se používá ke správě přístupu ke službě Azure directory. Další podrobnosti najdete v tématu [předplatné jak Azure souvisí s Azure AD](/active-directory/active-directory-how-subscriptions-associated-directory).

Ze všech předdefinovaných rolí RBAC **vlastníka** a **Přispěvatel** nabízejí úplné správy přístup ke všem prostředkům v prostředí, rozdíl, že Přispěvatel nelze vytvářet a odstraňovat nové role RBAC . Mezi integrované role jako **Přispěvatel virtuálních počítačů** nabízejí úplné správy přístup jen k prostředkům uvádí název, bez ohledu na to **skupiny prostředků** během vytváření do.

Přiřazení předdefinované role RBAC **Přispěvatel virtuálních počítačů** na úrovni předplatného, znamená, že uživatel přiřazenou roli:

* Můžete zobrazit všechny virtuální počítače bez ohledu na to datum jejich nasazení a skupiny prostředků, které jsou součástí
* Má úplné správy přístup k virtuálním počítačům v rámci předplatného
* Nelze zobrazit u jiných typů prostředků v předplatném
* Všechny změny z hlediska fakturační nemůže pracovat.

> [!NOTE]
> RBAC se portálu Azure pouze funkce, se nebude udělit přístup k portálu classic.

## <a name="assign-a-built-in-rbac-role-to-an-external-user"></a>Předdefinovaná role RBAC přiřadit externího uživatele
Pro různé scénáře v tomto testu, externí uživatele "alflanigan@gmail.com" se přidá jako **Přispěvatel virtuálních počítačů**.




![předdefinované role Přispěvatel virtuálního počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/11.png)

Normální chování pro tento externí uživatele s tato předdefinovaná role je chcete zobrazit a spravovat pouze virtuální počítače a jejich přiléhající Resource Manager pouze prostředky potřebné při nasazování. Podle návrhu, nabízí tyto role omezený přístup jenom k jejich příslušné prostředky, které jsou vytvořené na portálu Azure, bez ohledu na to některé můžete stále nasadit na klasickém portálu (například: virtuální počítače).





![Přehled role Přispěvatel virtuálních počítačů na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/12.png)

## <a name="grant-access-at-a-subscription-level-for-a-user-in-the-same-directory"></a>Udělení přístupu na úrovni předplatného pro uživatele ve stejném adresáři
Tok procesu je stejný jako při přidávání externího uživatele, z pohledu správce udělení RBAC role, jakož i uživatele i udělení přístupu k roli. Rozdíl je, že pozvané uživatele neobdrží žádné pozvánek e-mailu jako všechny obory prostředků v rámci předplatného. bude k dispozici v řídicím panelu po přihlášení.

## <a name="assign-rbac-roles-at-the-resource-group-scope"></a>Přiřazení role RBAC v oboru skupiny prostředků
Přiřazení na role RBAC **skupiny prostředků** oboru má identické proces pro přiřazení role na úrovni předplatného, pro oba typy uživatelů - externí nebo interní (součást stejný adresář). Uživatelé, které jsou přiřazeny RBAC role je zobrazíte ve svém prostředí pouze skupinu prostředků mají přiřazený přístup z **skupiny prostředků** ikonu na portálu Azure.

## <a name="assign-rbac-roles-at-the-resource-scope"></a>Přiřadit role RBAC v oboru prostředků
Přiřazení role RBAC v oboru prostředků v Azure má identické proces pro přiřazení role na úrovni předplatného nebo na úrovni skupiny prostředků, následující témže pracovním postupu pro oba scénáře. Znovu, můžete uživatele, pro které jsou přiřazené RBAC role zobrazení pouze těch položek, které mají přiřazený přístup k, buď v **všechny prostředky** kartě nebo přímo v jejich řídicího panelu.

Pro uživatele k přihlášení k adresáři správné zajistit je důležitým aspektem pro RBAC jak v oboru skupiny prostředků nebo prostředek oboru.





![přihlášení Directory na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/13.png)

## <a name="assign-rbac-roles-for-an-azure-active-directory-group"></a>Přiřazení role RBAC pro skupinu služby Azure Active Directory
Všechny scénáře pomocí RBAC na tři různé rozsahy v Azure nabízí oprávnění k správě, nasazení a správa různé prostředky jako uživatel s přiřazenou bez nutnosti správy osobních předplatné. Bez ohledu na to je pro předplatné, skupinu prostředků nebo prostředek oboru, všechny prostředky přiřazené uživatelé vytvořili na další se fakturují v rámci jednoho předplatného Azure, kde mají uživatelé přístup k přiřadit RBAC role. Tímto způsobem, uživatelů, kteří mají oprávnění správce pro toto předplatné celý Azure fakturace má úplný přehled o spotřebě, bez ohledu na to kdo spravuje prostředky.

Stejným způsobem jako pro skupiny Azure Active Directory s perspektivy, že uživatel s oprávněními správce chce zajistit granulární přístup pro týmy nebo celý oddělení, není jednotlivě pro každého uživatele, takže vzhledem k tomu lze použít pro větší organizace role RBAC jej jako velmi čas a správu efektivní možnost. Pro ilustraci v tomto příkladu **Přispěvatel** role je přidaný do jedné ze skupin v klientovi na úrovni předplatného.





![Přidání role RBAC pro skupiny AAD](./media/role-based-access-control-create-custom-roles-for-internal-external-users/14.png)

Tyto skupiny jsou skupiny zabezpečení, které jsou zřizovat a spravovat pouze v rámci Azure Active Directory.

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-powershell"></a>Vytvořit vlastní role RBAC otevření žádosti o podporu pomocí prostředí PowerShell
Předdefinované role RBAC, které jsou k dispozici v Azure zkontrolujte určité úrovně oprávnění na základě dostupných prostředků v prostředí. Pokud žádná z těchto rolí potřebám Správce uživatelů, existuje však možnost omezit přístup i další vytvořením vlastní role RBAC.

Vytvoření vlastní role RBAC vyžaduje trvat jednu předdefinovaná role, upravovat a importujte ji zpět do prostředí. Stažení a nahrání role se spravují pomocí prostředí PowerShell nebo rozhraní příkazového řádku.

Je důležité pochopit požadavky vytváření vlastní roli, které můžete udělit granulární přístup na úrovni předplatného a taky umožnit pozvané uživatele možnost otevření žádosti o podporu.

V tomto příkladu předdefinovaná role **čtečky** který uživatelům umožňuje přístup k zobrazení všech oborů prostředků, ale nechcete je upravit nebo vytvořit nové byl přizpůsoben, aby uživatel povolit možnost otevření žádosti o podporu.

Je první akcí exportu **čtečky** spustili role musí být dokončena v prostředí PowerShell se zvýšenými oprávněními jako správce.

```
Login-AzureRMAccount

Get-AzureRMRoleDefinition -Name "Reader"

Get-AzureRMRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json

```





![Snímek obrazovky prostředí PowerShell pro role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/15.png)

Budete potřebovat k extrakci šablona JSON role.





![Šablona JSON pro vlastní role RBAC čtečky](./media/role-based-access-control-create-custom-roles-for-internal-external-users/16.png)

Typické role RBAC se skládá ze tří hlavních částí **akce**, **NotActions** a **AssignableScopes**.

V **akce** části jsou uvedeny všechny operace, které jsou povolené pro tuto roli. Je důležité si uvědomit, že každá akce je přiřazený od zprostředkovatele prostředků. V takovém případě pro vytvoření lístky žádostí o podporu **Microsoft.Support** poskytovatele prostředků musí být uvedený.

Abyste mohli zobrazit všech poskytovatelů prostředků k dispozici a registrovaný v rámci vašeho předplatného, můžete použít PowerShell.
```
Get-AzureRMResourceProvider

```
Kromě toho můžete zkontrolovat všechny dostupné rutin prostředí PowerShell ke správě zprostředkovatelé prostředků.
    ![Snímek obrazovky prostředí PowerShell pro správu zprostředkovatele prostředků](./media/role-based-access-control-create-custom-roles-for-internal-external-users/17.png)

Pokud chcete omezit všechny akce pro konkrétní role RBAC, zprostředkovatelé prostředků jsou uvedeny v části **NotActions**.
Poslední je povinný, že RBAC role obsahuje explicitní předplatné ID, kde se používá. ID předplatného jsou uvedeny v seznamu **AssignableScopes**, jinak bude nebude povolen import role v rámci vašeho předplatného.

Po vytvoření a vlastní nastavení RBAC role, je nutné naimportovat zpátky prostředí.

```
New-AzureRMRoleDefinition -InputFile "C:\rbacrole2.json"

```

V tomto příkladu je vlastní název pro tuto roli RBAC "Čtečky podporu lístky úroveň přístupu" uživatel zobrazit vše, co v rámci předplatného a také otevření žádosti o podporu.

> [!NOTE]
> Jsou pouze dvě předdefinované role RBAC povolení akce otevření žádosti o podporu **vlastníka** a **Přispěvatel**. Uživatel nebude moci otevřít žádosti o podporu musí mohl být přiřazena role RBAC pouze v oboru předplatné, všechny žádosti o podporu se vytváří podle předplatného Azure.

Tato nová vlastní role byl přiřazen uživateli ze stejného adresáře.





![snímek obrazovky vlastní role RBAC importovat na portálu Azure](./media/role-based-access-control-create-custom-roles-for-internal-external-users/18.png)





![snímek obrazovky přiřazení vlastní importované role RBAC pro uživatele ve stejném adresáři](./media/role-based-access-control-create-custom-roles-for-internal-external-users/19.png)





![snímek obrazovky oprávnění pro vlastní importované role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/20.png)

V příkladu byla další podrobné zdůraznit omezení této vlastní role RBAC následujícím způsobem:
* Můžete vytvořit nové žádosti o podporu
* Nelze vytvořit nové obory prostředků (například: virtuálního počítače)
* Nelze vytvořit nové skupiny prostředků





![snímek obrazovky vytváření žádosti o podporu vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/21.png)





![snímek obrazovky vlastní role RBAC Nepodařilo se vytvořit virtuální počítače](./media/role-based-access-control-create-custom-roles-for-internal-external-users/22.png)





![snímek obrazovky vlastní role RBAC nelze vytvořit nové RGs](./media/role-based-access-control-create-custom-roles-for-internal-external-users/23.png)

## <a name="create-a-custom-rbac-role-to-open-support-requests-using-azure-cli"></a>Vytvořit vlastní role RBAC otevření žádosti o podporu pomocí rozhraní příkazového řádku Azure
Spouštění v Macu a bez nutnosti přístup k prostředí PowerShell, rozhraní příkazového řádku Azure je způsob, jak můžete přejít.

Postup vytvoření vlastních rolí jsou stejné, s jedinou výjimku, která pomocí rozhraní příkazového řádku roli nelze stáhnout v šabloně JSON, ale lze ji zobrazit v rozhraní příkazového řádku.

V tomto příkladu I vybrali integrované role **zálohování čtečky**.

```

azure role show "backup reader" --json

```





![Snímek obrazovky rozhraní příkazového řádku zálohování čtečky role zobrazit](./media/role-based-access-control-create-custom-roles-for-internal-external-users/24.png)

Úpravy roli v sadě Visual Studio po kopírování vlastnosti v šabloně JSON **Microsoft.Support** poskytovatele prostředků byla přidána do **akce** částech tak, aby tento uživatel může otevřít podpory požadavky můžete nadále být čtečku pro trezorů záloh. Akci je potřeba přidat kde tato role se použije v ID předplatného **AssignableScopes** části.

```

azure role create --inputfile <path>

```





![Rozhraní příkazového řádku snímek obrazovky importování vlastní role RBAC](./media/role-based-access-control-create-custom-roles-for-internal-external-users/25.png)

Nová role je nyní k dispozici na webu Azure portal a proces assignation je stejné jako v předchozích příkladech.





![Azure portálu snímek obrazovky vlastní role RBAC vytvořené pomocí rozhraní příkazového řádku 1.0](./media/role-based-access-control-create-custom-roles-for-internal-external-users/26.png)

Od verze nejnovější 2017 sestavení je všeobecně dostupná prostředí cloudové služby Azure. Prostředí Azure Cloud je doplněk k IDE a portálu Azure. S touto službou můžete získat prostředí založené na prohlížeči, který je ověřen a je hostovaná v Azure a můžete ji použít místo rozhraní příkazového řádku v počítači nainstalován.





![Azure Cloud Shell](./media/role-based-access-control-create-custom-roles-for-internal-external-users/27.png)
