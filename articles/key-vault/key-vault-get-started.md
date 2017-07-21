---
title: "Začínáme s Azure Key Vault | Dokumentace Microsoftu"
description: "Tento kurz vám pomůže začít s Azure Key Vault a ukáže vám, jak v Azure vytvořit zesílený kontejner, a jak ukládat a spravovat kryptografické klíče a tajné klíče v Azure."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 36721e1d-38b8-4a15-ba6f-14ed5be4de79
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/24/2016
ms.author: cabailey
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: c45695129c1795b44b933a6d60e2f21a98542f7d
ms.contentlocale: cs-cz
ms.lasthandoff: 05/25/2017


---
# <a name="get-started-with-azure-key-vault"></a>Začínáme s Azure Key Vault
Azure Key Vault je dostupný ve většině oblastí. Další informace najdete na [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod
Tento kurz vám pomůže začít s Azure Key Vault a ukáže vám, jak v Azure vytvořit zesílený kontejner (trezor), a jak ukládat a spravovat kryptografické klíče a tajné klíče v Azure. Provede vás procesem použití Azure PowerShell k vytvoření trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure. Poté vám ukáže, jak aplikace může tento klíč nebo heslo použít.

**Odhadovaný čas dokončení:** 20 minut

> [!NOTE]
> Tento kurz neobsahuje pokyny, jak napsat aplikaci Azure obsaženou v jednom z kroků, konkrétně Autorizace aplikace pro použití klíče nebo tajného klíče v trezoru klíčů.
>
> Tento kurz používá prostředí Azure PowerShell. Pokyny pro rozhraní příkazového řádku pro různé platformy najdete v [tomto ekvivalentním kurzu](key-vault-manage-with-cli2.md).
>
>

Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto kurzu potřebujete:

* Předplatné Microsoft Azure. Pokud žádné nemáte, můžete si zaregistrovat [bezplatný účet](https://azure.microsoft.com/pricing/free-trial/)
* Azure PowerShell v **minimální verzi 1.1.0**. Chcete-li nainstalovat Azure PowerShell a přidružit ho ke svému předplatnému Azure, prohlédněte si téma [Instalace a konfigurace Azure PowerShellu](/powershell/azure/overview). Pokud jste již Azure PowerShell nainstalovali, ale neznáte jeho verzi, z konzoly Azure PowerShell zadejte `(Get-Module azure -ListAvailable).Version`. Máte-li nainstalovaný Azure PowerShell ve verzi 0.9.1 až 0.9.8, stále můžete tento kurz použít, pouze s menšími změnami. Například musíte použít příkaz `Switch-AzureMode AzureResourceManager` a zároveň se změnily některé příkazy Azure Key Vault. Seznam rutin Key Vault pro verze 0.9.1 až 0.9.8 naleznete v tématu [Rutiny Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).
* Aplikaci, kterou nakonfigurujete pro použití klíče nebo hesla, které v tomto kurzu vytvoříte. Vzorová aplikace je k dispozici ve službě [Microsoft Download Center](http://www.microsoft.com/en-us/download/details.aspx?id=45343). Pokyny naleznete v souvisejícím souboru Readme.

Tento kurz je určen pro začátečníky v Azure PowerShellu, ale předpokládá, že chápete základní koncepty, jako jsou moduly, rutiny a relace. Další informace naleznete v tématu [Začínáme s Microsoft PowerShellem](https://technet.microsoft.com/library/hh857337.aspx).

Podrobnou nápovědu k jakékoli rutině v tomto kurzu získáte pomocí rutiny **Get-Help**.

    Get-Help <cmdlet-name> -Detailed

Například pro získání nápovědy pro rutinu **Login-AzureRmAccount** zadejte:

    Get-Help Login-AzureRmAccount -Detailed

Pro seznámení se s Azure Resource Managerem v Azure PowerShellu si také můžete přečíst následující kurzy:

* [Jak nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azure/overview)
* [Použití Azure PowerShellu s Resource Managerem](../powershell-azure-resource-manager.md)

## <a id="connect"></a>Připojení k předplatným
Spusťte relaci Azure PowerShellu a přihlaste se k účtu Azure pomocí následujícího příkazu:  

    Login-AzureRmAccount

Pamatujte, že pokud používáte konkrétní instanci Azure, například Azure Government, musíte v tomto příkazu použít parametr -Environment. Příklad: `Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)`

V automaticky otevřeném okně prohlížeče zadejte svoje uživatelské jméno a heslo k účtu Azure. Azure PowerShell získá všechna předplatná přidružená k tomuto účtu a ve výchozím nastavení použije první předplatné.

Máte-li více předplatných a chcete určit jedno konkrétní, které se má použít pro Azure Key Vault, zadejte následující příkaz k zobrazení předplatných pro váš účet:

    Get-AzureRmSubscription

Poté pro určení předplatného, které se má použít, zadejte:

    Set-AzureRmContext -SubscriptionId <subscription ID>

Další informace o konfiguraci prostředí Azure PowerShell najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a id="resource"></a>Vytvoření nové skupiny prostředků
Používáte-li Azure Resource Manager, pak se všechny související prostředky vytváří v uvnitř skupiny prostředků. Pro účely tohoto kurzu vytvoříme novou skupinu prostředků s názvem **ContosoResourceGroup**:

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'


## <a id="vault"></a>Vytvoření trezoru klíčů
Pro vytvoření trezoru klíčů použijte rutinu [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Tato rutina má tři povinné parametry: **název skupiny prostředků**, **název trezoru klíčů** a **zeměpisné umístění**.

Používáte-li například název trezoru **ContosoKeyVault**, název skupiny prostředků **ContosoResourceGroup** a umístění **Východní Asie**, zadejte:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Výstup této rutiny zobrazuje vlastnosti trezoru klíčů, který jste právě vytvořili. Dvě nejdůležitější vlastnosti jsou:

* **Název trezoru**: V tomto příkladu je to **ContosoKeyVault**. Tento název budete používat pro další rutiny Key Vault.
* **Identifikátor URI trezoru**: V tomto příkladu je to https://contosokeyvault.vault.azure.net/. Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.

Váš účet Azure je nyní oprávněn provádět nad tímto trezorem klíčů všechny operace. Zatím k tomu není oprávněn nikdo jiný.

> [!NOTE]
> Pokud se při pokusu o vytvoření nového trezoru klíčů zobrazí chyba **Předplatné není zaregistrované pro používání oboru názvů „Microsoft.KeyVault“**, spusťte `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"` a poté znovu spusťte váš příkaz New-AzureRmKeyVault. Další informace najdete v tématu [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider).
>
>

## <a id="add"></a>Přidání klíče nebo tajného klíče do trezoru
Chcete-li, aby pro vás služba Azure Key Vault vytvořila softwarově chráněný klíč, použijte rutinu [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey) a zadejte následující:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

Nicméně pokud už máte existující softwarově chráněný klíč v souboru .PFX uloženém na jednotce C:\ v souboru s názvem softkey.pfx a chcete ho nahrát do Azure Key Vault, zadejte následující kód, abyste nastavili proměnnou **securepfxpwd** na heslo **123** pro soubor .PFX:

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

Poté zadejte následující kód, abyste importovali klíč ze souboru .PFX, který chrání klíč softwarem, do služby Key Vault:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd


Na vytvořený nebo do Azure Key Vault nahraný klíč můžete odkazovat pomocí jeho identifikátoru URI. Pomocí **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy získáte aktuální verzi. Chcete-li získat konkrétní verzi, použijte **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87**.  

Identifikátor URI tohoto klíče zobrazíte zadáním:

    $Key.key.kid

Chcete-li do trezoru služby Azure Key Vault přidat tajný klíč v podobě hesla s názvem SQLPassword a hodnotou Pa$$w0rd, nejprve převeďte hodnotu Pa$$w0rd na zabezpečený řetězec zadáním následujícího:

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

Poté zadejte následující:

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Nyní na toto heslo, které jste přidali do Azure Key Vault, můžete odkazovat pomocí jeho identifikátoru URI. Pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy získáte aktuální verzi. Chcete-li získat konkrétní verzi, použijte **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Chcete-li zobrazit identifikátor URI tajného klíče, zadejte:

    $secret.Id

Podívejme se na klíč nebo tajný klíč, který jste právě vytvořili:

* Pokud chcete zobrazit klíč, zadejte `Get-AzureKeyVaultKey –VaultName 'ContosoKeyVault'`
* Pokud chcete zobrazit tajný klíč, zadejte `Get-AzureKeyVaultSecret –VaultName 'ContosoKeyVault'`

Nyní jsou váš trezor klíčů a klíč nebo tajný klíč připravené pro použití aplikacemi. Aby aplikace mohly klíče používat, musíte je nejdříve autorizovat.  

## <a id="register"></a>Registrace aplikace s Azure Active Directory
Tento krok obvykle provádí vývojář na samostatném počítači. Není nijak specifický pro Azure Key Vault, ale pro úplnost je zde uveden.

> [!IMPORTANT]
> Pro dokončení kurzu musí být váš účet, trezor i aplikace, kterou budete v tomto kroku registrovat, ve stejném adresáři Azure.
>
>

Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory. K tomu je potřeba, aby majitel aplikace nejprve zaregistroval aplikaci ve vlastním Azure Active Directory. Na konci registrace obdrží majitel aplikace následující hodnoty:

* **ID aplikace** (také označované jako ID klienta) a **ověřovací klíč** (také označovaný jako sdílený tajný klíč). Pro získání tokenu musí aplikace obě tyto hodnoty poskytnout do Azure Active Directory. Záleží na aplikaci, jak je k tomu nakonfigurovaná. Pro ukázkovou aplikaci Key Vault nastavuje majitel tyto hodnoty v souboru app.config.

Chcete-li zaregistrovat aplikaci v Azure Active Directory:

1. Přihlaste se k portálu Azure Classic.
2. Na levé straně klikněte na **Active Directory** a poté vyberte adresář, ve kterém zaregistrujete svoji aplikaci. <br> <br> **Poznámka:** Musíte vybrat stejný adresář, který obsahuje předplatné Azure, které jste použili pro vytvoření trezoru klíčů. Pokud si nejste jisti, který adresář to je, klikněte na **Nastavení**, určete předplatné, které jste použili při vytvoření trezoru klíčů, a poznamenejte si název adresáře v posledním sloupci.
3. Klikněte na **Aplikace**. Pokud do vašeho adresáře nebyly přidané žádné aplikace, na stránce se zobrazí pouze odkaz **Přidat aplikaci**. Klikněte na odkaz, případně klikněte na **Přidat** na panelu příkazů.
4. V průvodci **Přidat aplikaci** klikněte na stránce **Co si přejete udělat?** na **Přidat aplikaci, kterou vyvíjí moje organizace**.
5. Na stránce **Řekněte nám o své aplikaci** zadejte název vaší aplikace a poté vyberte **Webová aplikace nebo webové rozhraní API** (výchozí možnost). Klikněte na ikonu **Další**.
6. Na stránce **Vlastnosti aplikace** zadejte **URL pro přihlašování** a **Identifikátor ID URI Aplikace** pro svoji webovou aplikaci. Pokud vaše aplikace tyto hodnoty neobsahuje, v tomto kroku si je můžete vymyslet (například můžete do obou polí zadat http://test1.contoso.com). Nezáleží na tom, zda tyto weby existují. Důležité je, aby identifikátor URI ID aplikace byl pro každou aplikaci ve vašem adresáři jiný. Adresář tento řetězec používá k identifikaci vaší aplikace.
7. Změny provedené v průvodci uložíte kliknutím na ikonu **Dokončit**.
8. Na stránce **Rychlý start** klikněte na **Konfigurovat**.
9. Posuňte se k části **klíče**, vyberte dobu trvání a poté klikněte na **Uložit**. Stránka se obnoví a zobrazí hodnotu klíče. Vaši aplikaci je nutné nakonfigurovat s touto hodnotou klíče a s hodnotou **ID klienta**. (Pokyny k této konfiguraci se liší podle aplikace.)
10. Poznamenejte si hodnotu ID klienta z této stránky, v dalším kroku ji použijete pro nastavení oprávnění pro váš trezoru.

## <a id="authorize"></a>Autorizace aplikace pro použití klíče nebo tajného klíče
Chcete-li autorizovat aplikaci pro přístup ke klíči nebo k tajnému klíči, použijte rutinu  [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy).

Máte-li například trezor s názvem **ContosoKeyVault** a ID klienta aplikace, kterou chcete ověřit, má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed, můžete aplikaci autorizovat pro dešifrování a podepisování pomocí klíčů ve vašem trezoru spuštěním následujícího příkazu:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

Chcete-li tu samou aplikaci autorizovat pro čtení tajných klíčů v trezoru, spusťte následující:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>Pokud chcete použít modul hardwarového zabezpečení (HSM)
Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM. Moduly hardwarového zabezpečení jsou ověřené podle standardu FIPS 140-2 Level 2. Pokud se vás tento požadavek netýká, přeskočte tuto část a přejděte k části [Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů](#delete).

Pokud chcete vytvořit tyto klíče s ochranou HSM, musíte použít [vrstvu služby Azure Key Vault Premium pro podporu klíčů s ochranou HSM](https://azure.microsoft.com/pricing/free-trial/). Kromě toho tato funkce není dostupná pro Azure China.

Při vytváření trezoru klíčů přidejte parametr **-SKU**:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'



Do tohoto trezoru klíčů můžete přidat klíče chráněné softwarem (jak jsme ukázali výše) a klíče chráněné pomocí HSM. Chcete-li vytvořit klíč chráněný pomocí HSM, nastavte parametr **-Destination** na „HSM“:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

Pro import klíče ze souboru .PFX můžete ve vašem počítači použít následující příkaz. Tento příkaz importuje klíč do HSM ve službě Key Vault:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'


Další příkaz importuje balíček „přineste si vlastní klíč“ (BYOK). Tento scénář umožňuje vygenerovat klíč v místním modulu HSM a přenést ho do modulů HSM ve službě Key Vault, aniž by klíč opustil hranice modulu HSM:

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

Podrobnější pokyny o tom, jak generovat tento balíček BYOK naleznete v tématu [Generování a přenos klíčů chráněných pomocí HSM pro Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a id="delete"></a>Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů
Pokud již nepotřebujete trezor klíčů, ani klíče a tajné klíče, které obsahuje, můžete trezor klíčů odstranit pomocí rutiny [Remove-AzureRmKeyVault](/powershell/module/azurerm.keyvault/remove-azurermkeyvault):

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

Nebo můžete odstranit celou skupinu prostředků Azure, která zahrnuje trezor klíčů a všechny další prostředky, které jste do skupiny zahrnuli.

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'


## <a id="other"></a>Další rutiny Azure PowerShellu
Další užitečné příkazy pro správu Azure Key Vault:

* `$Keys = Get-AzureKeyVaultKey -VaultName 'ContosoKeyVault'`: Tento příkaz načte tabelární zobrazení všech klíčů a vybraných vlastnosti.
* `$Keys[0]`: Tento příkaz zobrazí úplný seznam vlastností zadaného klíče.
* `Get-AzureKeyVaultSecret`: Tento příkaz vypíše tabelární zobrazení všech názvů tajných klíčů a vybraných vlastností.
* `Remove-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey'`: Příklad odebrání určitého klíče.
* `Remove-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword'`: Příklad odebrání určitého tajného klíče.

## <a id="next"></a>Další kroky
Chcete-li používat Azure Key Vault ve webové aplikaci, podívejte se na navazující kurz [Použití Azure Key Vault z webové aplikace](key-vault-use-from-web-application.md).

Chcete-li mít přehled o využívání trezoru klíčů, podívejte se na kurz [Protokolování v Azure Key Vault](key-vault-logging.md).

Seznam nejnovějších rutin Azure PowerShellu pro Azure Key Vault naleznete v tématu [Rutiny Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).

Programátorské reference najdete v [příručce pro vývojáře Azure Key Vault](key-vault-developers-guide.md).

