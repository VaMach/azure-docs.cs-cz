---
title: "Spravovat pomocí rozhraní příkazového řádku Azure Key Vault | Microsoft Docs"
description: "Tento kurz použijte k automatizaci běžných úkolů v Key Vault pomocí rozhraní příkazového řádku"
services: key-vault
documentationcenter: 
author: BrucePerlerMS
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: bruceper
ms.openlocfilehash: c2565a742ce4f6ab5f7639a54c4a475f00cbc260
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-key-vault-using-cli"></a>Spravovat Key Vault pomocí rozhraní příkazového řádku

Azure Key Vault je dostupný ve většině oblastí. Další informace najdete na [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod

Tento kurz vám pomůže začít s Azure Key Vault a ukáže vám, jak v Azure vytvořit zesílený kontejner (trezor), a jak ukládat a spravovat kryptografické klíče a tajné klíče v Azure. Provede vás provede procesem vytvoření trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure pomocí rozhraní příkazového řádku pro různé platformy Azure. Poté vám ukáže, jak aplikaci můžete potom použít tento klíč nebo heslo.

**Odhadovaný čas dokončení:** 20 minut

> [!NOTE]
> Tento kurz neobsahuje pokyny o tom, jak napsat aplikaci Azure, že jeden z kroků obsahuje, který ukazuje, jak k autorizaci aplikace pro použití klíče nebo tajného klíče v trezoru klíčů.
> 
> V současné době nelze Azure Key Vault konfigurovat na portálu Azure. Místo toho použijte tyto pokyny Multiplatformního rozhraní příkazového řádku. Nebo Azure PowerShell pokyny najdete v tématu [tomto ekvivalentním kurzu](key-vault-get-started.md).
> 
> 

Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).

## <a name="prerequisites"></a>Požadavky

K dokončení tohoto kurzu potřebujete:

* Předplatné Microsoft Azure. Pokud jeden nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).
* Rozhraní příkazového řádku verzi 0.9.1 nebo novější. Nainstalujte nejnovější verzi a připojení k předplatnému Azure najdete v tématu [instalace a konfigurace rozhraní příkazového řádku pro různé platformy Azure](../cli-install-nodejs.md).
* Aplikaci, kterou nakonfigurujete pro použití klíče nebo hesla, které v tomto kurzu vytvoříte. Vzorová aplikace je k dispozici ve službě [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Pokyny naleznete v souvisejícím souboru Readme.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Získání nápovědy pomocí rozhraní příkazového řádku Azure a platformy

V tomto kurzu se předpokládá, že jste obeznámeni s rozhraní příkazového řádku (Bash, terminálu, příkazového řádku)

--Parametr nápovědy nebo -h lze zobrazit nápovědu pro určité příkazy. Alternativně azure help [příkaz] [možnosti] formátu lze použít také k vrácení stejné informace. Například následující příkazy, které se všechny vrátí stejné informace:

    azure account set --help

    azure account set -h

    azure help account set

Pokud máte pochybnosti o parametry vyžadované příkaz, v nápovědě k použití – Nápověda, -h nebo azure nápovědy [příkaz].

Také si můžete přečíst následující kurzy a seznamte se s Azure Resource Manager v rozhraní příkazového řádku Azure napříč platformami:

* [Postup instalace a konfigurace rozhraní příkazového řádku Azure a platformy](../cli-install-nodejs.md)
* [Pomocí rozhraní příkazového řádku a platformy Azure s Azure Resource Manager](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Připojte se ke svým předplatným

K přihlášení pomocí účtu organizace, použijte následující příkaz:

    azure login -u username -p password

nebo pokud se chcete přihlásit interaktivně zadáním

    azure login

> [!NOTE]
> Přihlášení metodu lze použít pouze s účtem organizace. Účet organizace je uživatel, který je spravovaná vaší organizací a definované v klienta Azure Active Directory vaší organizace.
> 
> 

Pokud nyní není k dispozici účet organizace a používáte účet Microsoft k přihlášení k předplatnému Azure, můžete snadno vytvořit pomocí následujících kroků.

1. Přihlášení k přihlášení k [portálu pro správu Azure](https://manage.windowsazure.com/)a klikněte na Active Directory.
2. Pokud žádný adresář existuje, vyberte možnost vytvořit adresáře a zadejte požadované informace.
3. Vyberte adresář a přidání nového uživatele. Tento nový uživatel je účet organizace. Při vytváření uživatele bude nutné zadat obě e-mailovou adresu pro uživatele a dočasné heslo. Tyto informace uložte, protože se používá v jiném kroku.
4. Z portálu vyberte nastavení a pak vyberte správci. Vyberte Přidat a přidat nové uživatele jako spolusprávce. To umožňuje účet organizace ke správě vašeho předplatného Azure.
5. Nakonec odhlaste se z portálu Azure a potom znovu přihlásit pomocí nového účtu organizace. Pokud je to první čas přihlášení pomocí tohoto účtu, vyzve ke změně hesla.

Další informace o používání účet organizace v Microsoft Azure najdete v tématu [registrace pro Microsoft Azure jako organizace](../active-directory/sign-up-organization.md).

Máte-li více předplatných a chcete určit jedno konkrétní, které se má použít pro Azure Key Vault, zadejte následující příkaz k zobrazení předplatných pro váš účet:

    azure account list

Poté pro určení předplatného, které se má použít, zadejte:

    azure account set <subscription name>

Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [postup instalace a konfigurace rozhraní příkazového řádku Azure napříč platformami](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Přejít na používání Azure Resource Manager
Key Vault vyžaduje Azure Resource Manager, takže zadejte následující příkaz a přepněte do režimu Azure Resource Manager:

    azure config mode arm

## <a name="create-a-new-resource-group"></a>Vytvoření nové skupiny prostředků
Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků. Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.

    azure group create 'ContosoResourceGroup' 'East Asia'

První parametr je název skupiny prostředků a druhý parametr je umístění. Pro umístění, použijte příkaz `azure location list` zjišťují, jak zadat alternativní umístění na v tomto příkladu. Pokud potřebujete více informací, zadejte:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Registrace poskytovatele prostředků Key Vault
Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:

`azure provider register Microsoft.KeyVault`

To jenom je nutné provést jednou za předplatné.

## <a name="create-a-key-vault"></a>Vytvořte trezor klíčů

Použití `azure keyvault create` příkaz pro vytvoření trezoru klíčů. Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a zeměpisné umístění.

Například pokud použijete název trezoru ContosoKeyVault, název skupiny prostředků ContosoResourceGroup a umístění východní Asie, zadejte:

    azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'

Výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů, který jste právě vytvořili. Dvě nejdůležitější vlastnosti jsou:

* **Název**: V tomto příkladu to je ContosoKeyVault. Tento název budete používat pro další rutiny Key Vault.
* **vaultUri**: V tomto příkladu to je https://contosokeyvault.vault.azure.net. Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.

Váš účet Azure je nyní oprávněn provádět nad tímto trezorem klíčů všechny operace. Zatím k tomu není oprávněn nikdo jiný.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Přidejte k trezoru klíč nebo tajný klíč

Pokud chcete Azure Key Vault vytvořila softwarově chráněný klíč pro vás, použijte `azure key create` příkaz a zadejte následující příkaz:

    azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software

Nicméně pokud máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, který chcete nahrát do Azure Key Vault, zadejte následující příkaz pro import klíče z. Soubor PEM, který chrání klíč softwarem ve službě Key Vault:

    azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software

Nyní můžete odkazovat klíč, který jste vytvořili nebo nahrán do Azure Key Vault, pomocí jeho identifikátoru URI. Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy získáte aktuální verzi a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** Chcete-li získat konkrétní verzi.

Chcete-li přidat tajný klíč k úložišti, který je hesla s názvem SQLPassword a hodnotou z Pa$ $w0rd do Azure Key Vault, zadejte následující příkaz:

    azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'

Nyní na toto heslo, které jste přidali do Azure Key Vault, můžete odkazovat pomocí jeho identifikátoru URI. Pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy získáte aktuální verzi. Chcete-li získat konkrétní verzi, použijte **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Podívejme se na klíč nebo tajný klíč, který jste právě vytvořili:

* Pokud chcete zobrazit klíč, zadejte `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Pokud chcete zobrazit tajný klíč, zadejte `azure keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Registrujte aplikaci s Azure Active Directory

Tento krok obvykle provádí vývojář na samostatném počítači. Není specifické pro Azure Key Vault, ale je zahrnuté pro úplnost.

> [!IMPORTANT]
> Pro dokončení kurzu musí být váš účet, trezor i aplikace, kterou budete v tomto kroku registrovat, ve stejném adresáři Azure.
> 
> 

Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory. K tomu je potřeba, aby majitel aplikace nejprve zaregistroval aplikaci ve vlastním Azure Active Directory. Na konci registrace obdrží majitel aplikace následující hodnoty:

* **ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označovaný jako sdílený tajný klíč). Aplikace musí představovat obě tyto hodnoty do Azure Active Directory, chcete-li získat token. Záleží na aplikaci, jak je k tomu nakonfigurovaná. Pro ukázkovou aplikaci Key Vault nastavuje majitel tyto hodnoty v souboru app.config.

Chcete-li zaregistrovat aplikaci v Azure Active Directory:

1. Přihlaste se k portálu Azure.
2. Na levé straně klikněte na **Active Directory** a poté vyberte adresář, ve kterém zaregistrujete svoji aplikaci. <br> <br> 

>[!NOTE] 
> Musíte vybrat stejný adresář, který obsahuje předplatné Azure, který jste použili pro vytvoření trezoru klíčů. Pokud si nejste jisti, který adresář to je, klikněte na **Nastavení**, určete předplatné, které jste použili při vytvoření trezoru klíčů, a poznamenejte si název adresáře v posledním sloupci.

3. Klikněte na **Aplikace**. Pokud do vašeho adresáře nebyly přidané žádné aplikace, tato stránka se zobrazí pouze **přidat aplikaci** odkaz. Klikněte na odkaz, nebo také můžete klepnout **přidat** na panelu příkazů.
4. V průvodci **Přidat aplikaci** klikněte na stránce **Co si přejete udělat?** na **Přidat aplikaci, kterou vyvíjí moje organizace**.
5. Na **Řekněte nám o své aplikaci** , zadejte název pro vaši aplikaci a vyberte **webové aplikace nebo webové rozhraní API** (výchozí). Klikněte na ikonu Další.
6. Na stránce **Vlastnosti aplikace** zadejte **URL pro přihlašování** a **Identifikátor ID URI Aplikace** pro svoji webovou aplikaci. Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com). Není důležité, zda tyto stránky existují, důležité je, aby identifikátor ID URI aplikace byl pro každou aplikaci ve vašem adresáři jiný. Adresář tento řetězec používá k identifikaci vaší aplikace.
7. Klikněte na ikonu dokončení uložte provedené změny v průvodci.
8. Na stránce Rychlý Start klikněte na **konfigurace**.
9. Posuňte se k části **klíče**, vyberte dobu trvání a poté klikněte na **Uložit**. Stránka se obnoví a zobrazí hodnotu klíče. Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a s hodnotou **ID klienta**. (Pokyny k této konfiguraci se budou lišit podle aplikace.)
10. Poznamenejte si hodnotu ID klienta z této stránky, v dalším kroku ji použijete pro nastavení oprávnění pro váš trezoru.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorizujte aplikaci pro použití klíče nebo tajného klíče
Chcete-li autorizovat aplikaci pro přístup ke klíči nebo tajný klíč v trezoru, použijte `azure keyvault set-policy` příkaz.

Například pokud je název vaší trezoru ContosoKeyVault a aplikace, které chcete autorizovat má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete aplikaci autorizovat pro dešifrování a podepisování pomocí klíčů ve vašem trezoru, spusťte následující:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'

> [!NOTE]
> Pokud používáte na příkazovém řádku Windows, doporučujeme nahradit jednoduchých uvozovek a být dvojité uvozovky a také řídicí interní dvojité uvozovky. Například: "[\"dešifrovat\",\"přihlašovací\"]".
> 
> 

Chcete-li tu samou aplikaci autorizovat pro čtení tajných klíčů v trezoru, spusťte následující:

    azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Pokud chcete použít modul hardwarového zabezpečení (HSM)
Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM. Moduly hardwarového zabezpečení jsou ověřené podle standardu FIPS 140-2 Level 2. Pokud se vás tento požadavek netýká, přeskočte tuto část a přejděte k části [Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).

Pro vytvoření těchto klíčů chráněných pomocí HSM, musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.

Když vytvoříte keyvault, přidejte parametr 'sku':

    azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'

Do tohoto trezoru můžete přidat klíče chráněné softwarem (jak jsme ukázali výše) a klíče chráněné pomocí HSM. Chcete-li vytvořit klíč chráněný HSM, nastavte parametr cílové na "HSM":

    azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'

Můžete následující příkaz pro import klíče ze souboru .pem ve vašem počítači. Tento příkaz importuje klíč do HSM ve službě Key Vault:

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'

Další příkaz importuje balíček „přineste si vlastní klíč“ (BYOK). To vám umožní vygenerovat klíč v místním HSM a jeho přenesení do HSM ve službě Key Vault, aniž by klíč opustil hranice HSM.

    azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'

Podrobnější pokyny o tom, jak generovat tento balíček BYOK naleznete v tématu [použití HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů
Pokud již nepotřebujete trezor klíčů a klíč nebo tajný klíč, který obsahuje, můžete trezor klíčů odstranit pomocí příkazu delete azure keyvault:

    azure keyvault delete --vault-name 'ContosoKeyVault'

Nebo můžete odstranit celou skupinu prostředků Azure, která zahrnuje trezor klíčů a všechny další prostředky, které jste do skupiny zahrnuli.

    azure group delete --name 'ContosoResourceGroup'


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Ostatní příkazy rozhraní příkazového řádku Azure a platformy
Další příkazy, že může využít ke správě Azure Key Vault.

Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:

    azure keyvault key list --vault-name 'ContosoKeyVault'

Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč:

    azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:

    azure keyvault secret list --vault-name 'ContosoKeyVault'

Tady je příklad odebrání určitého klíče:

    azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'

Tady je příklad odebrání určitého tajného klíče:

    azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'


## <a name="next-steps"></a>Další kroky
Programátorské reference najdete v [příručce pro vývojáře Azure Key Vault](key-vault-developers-guide.md).

