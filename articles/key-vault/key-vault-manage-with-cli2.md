---
title: "Spravovat pomocí rozhraní příkazového řádku Azure Key Vault | Microsoft Docs"
description: "Tento kurz použijte k automatizaci běžných úkolů v Key Vault pomocí rozhraní příkazového řádku 2.0"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: ambapat
ms.openlocfilehash: 5da9f5eceda71ac85259193e0f183c72813e1679
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-key-vault-using-cli-20"></a>Spravovat pomocí rozhraní příkazového řádku 2.0 Key Vault
Azure Key Vault je dostupný ve většině oblastí. Další informace najdete na [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="introduction"></a>Úvod
Tento kurz vám pomůže začít s Azure Key Vault a ukáže vám, jak v Azure vytvořit zesílený kontejner (trezor), a jak ukládat a spravovat kryptografické klíče a tajné klíče v Azure. Provede vás provede procesem vytvoření trezoru, který obsahuje klíč nebo heslo, které pak můžete použít s aplikací Azure pomocí rozhraní příkazového řádku pro různé platformy Azure. Poté vám ukáže, jak aplikaci můžete potom použít tento klíč nebo heslo.

**Odhadovaný čas dokončení:** 20 minut

> [!NOTE]
> Tento kurz neobsahuje pokyny o tom, jak napsat aplikaci Azure, že jeden z kroků obsahuje, který ukazuje, jak k autorizaci aplikace pro použití klíče nebo tajného klíče v trezoru klíčů.
>
> Tento kurz používá nejnovější 2.0 rozhraní příkazového řádku Azure.
>
>

Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto kurzu potřebujete:

* Předplatné Microsoft Azure. Pokud jeden nemáte, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial).
* Rozhraní příkazového řádku verze 2.0 nebo novější. Nainstalujte nejnovější verzi a připojení k předplatnému Azure najdete v tématu [instalace a konfigurace Azure napříč platformami rozhraní příkazového řádku 2.0](/cli/azure/install-azure-cli).
* Aplikaci, kterou nakonfigurujete pro použití klíče nebo hesla, které v tomto kurzu vytvoříte. Vzorová aplikace je k dispozici ve službě [Microsoft Download Center](http://www.microsoft.com/download/details.aspx?id=45343). Pokyny naleznete v souvisejícím souboru Readme.

## <a name="getting-help-with-azure-cross-platform-command-line-interface"></a>Získání nápovědy pomocí rozhraní příkazového řádku Azure a platformy
V tomto kurzu se předpokládá, že jste obeznámeni s rozhraní příkazového řádku (Bash, terminálu, příkazového řádku)

--Parametr nápovědy nebo -h lze zobrazit nápovědu pro určité příkazy. Alternativně azure help [příkaz] [možnosti] formátu lze použít také k vrácení stejné informace. Například následující příkazy, které se všechny vrátí stejné informace:

```
az account set --help
az account set -h
```

Pokud máte pochybnosti o parametry vyžadované příkaz, v nápovědě k použití – Nápověda, – h nebo az help [příkaz].

Také si můžete přečíst následující kurzy a seznamte se s Azure Resource Manager v rozhraní příkazového řádku Azure napříč platformami:

* [Instalace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli)
* [Začínáme s Azure CLI 2.0](/cli/azure/get-started-with-azure-cli)

## <a name="connect-to-your-subscriptions"></a>Připojte se ke svým předplatným
K přihlášení pomocí účtu organizace, použijte následující příkaz:

```
az login -u username@domain.com -p password
```

nebo pokud se chcete přihlásit interaktivně zadáním

```
az login
```

Máte-li více předplatných a chcete určit jedno konkrétní, které se má použít pro Azure Key Vault, zadejte následující příkaz k zobrazení předplatných pro váš účet:

```
az account list
```

Poté pro určení předplatného, které se má použít, zadejte:

```
az account set --subscription <subscription name or ID>
```

Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [nainstalovat rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli).

## <a name="create-a-new-resource-group"></a>Vytvoření nové skupiny prostředků
Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků. Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.

```
az group create -n 'ContosoResourceGroup' -l 'East Asia'
```

První parametr je název skupiny prostředků a druhý parametr je umístění. Pro umístění, použijte příkaz `az account list-locations` zjišťují, jak zadat alternativní umístění na v tomto příkladu. Pokud potřebujete více informací, zadejte: `az account list-locations -h`.

## <a name="register-the-key-vault-resource-provider"></a>Registrace poskytovatele prostředků Key Vault
Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:

```
az provider register -n Microsoft.KeyVault
```

To jenom je nutné provést jednou za předplatné.

## <a name="create-a-key-vault"></a>Vytvořte trezor klíčů
Použití `az keyvault create` příkaz pro vytvoření trezoru klíčů. Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a zeměpisné umístění.

Například pokud použijete název trezoru ContosoKeyVault, název skupiny prostředků ContosoResourceGroup a umístění východní Asie, zadejte:
```
az keyvault create --name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů, který jste právě vytvořili. Dvě nejdůležitější vlastnosti jsou:

* **název**: V tomto příkladu to je ContosoKeyVault. Tento název pro jiné příkazy Key Vault budete používat.
* **vaultUri**: V tomto příkladu to je https://contosokeyvault.vault.azure.net. Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.

Váš účet Azure je nyní oprávněn provádět nad tímto trezorem klíčů všechny operace. Zatím k tomu není oprávněn nikdo jiný.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Přidejte k trezoru klíč nebo tajný klíč
Pokud chcete Azure Key Vault vytvořila softwarově chráněný klíč pro vás, použijte `az key create` příkaz a zadejte následující příkaz:
```
az keyvault key create --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --protection software
```
Nicméně pokud máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, který chcete nahrát do Azure Key Vault, zadejte následující příkaz pro import klíče z. Soubor PEM, který chrání klíč softwarem ve službě Key Vault:
```
az keyvault key import --vault-name 'ContosoKeyVault' --name 'ContosoFirstKey' --pem-file './softkey.pem' --pem-password 'PaSSWORD' --protection software
```
Nyní můžete odkazovat klíč, který jste vytvořili nebo nahrán do Azure Key Vault, pomocí jeho identifikátoru URI. Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy získáte aktuální verzi a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** Chcete-li získat konkrétní verzi.

Chcete-li přidat tajný klíč k úložišti, který je hesla s názvem SQLPassword a hodnotou z Pa$ $w0rd do Azure Key Vault, zadejte následující příkaz:
```
az keyvault secret set --vault-name 'ContosoKeyVault' --name 'SQLPassword' --value 'Pa$$w0rd'
```
Nyní na toto heslo, které jste přidali do Azure Key Vault, můžete odkazovat pomocí jeho identifikátoru URI. Pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy získáte aktuální verzi. Chcete-li získat konkrétní verzi, použijte **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Podívejme se na klíč nebo tajný klíč, který jste právě vytvořili:

* Pokud chcete zobrazit klíč, zadejte `az keyvault key list --vault-name 'ContosoKeyVault'`
* Pokud chcete zobrazit tajný klíč, zadejte `az keyvault secret list --vault-name 'ContosoKeyVault'`

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
2. Na levé straně klikněte na tlačítko **Azure Active Directory**a poté vyberte adresář, ve kterém bude registrace vaší aplikace. <br> <br> 

> [!Note] 
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
Chcete-li autorizovat aplikaci pro přístup ke klíči nebo tajný klíč v trezoru, použijte `az keyvault set-policy` příkaz.

Například pokud je název vaší trezoru ContosoKeyVault a aplikace, které chcete autorizovat má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete aplikaci autorizovat pro dešifrování a podepisování pomocí klíčů ve vašem trezoru, spusťte následující:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --key-permissions decrypt sign
```

Chcete-li tu samou aplikaci autorizovat pro čtení tajných klíčů v trezoru, spusťte následující:
```
az keyvault set-policy --name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --secret-permissions get
```
## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Pokud chcete použít modul hardwarového zabezpečení (HSM)
Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM. Moduly hardwarového zabezpečení jsou ověřené podle standardu FIPS 140-2 Level 2. Pokud se vás tento požadavek netýká, přeskočte tuto část a přejděte k části [Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).

Pro vytvoření těchto klíčů chráněných pomocí HSM, musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.

Když vytvoříte keyvault, přidejte parametr 'sku':

```
az keyvault create --name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```
Do tohoto trezoru můžete přidat klíče chráněné softwarem (jak jsme ukázali výše) a klíče chráněné pomocí HSM. Chcete-li vytvořit klíč chráněný HSM, nastavte parametr cílové na "HSM":

```
az keyvault key create --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --protection 'hsm'
```

Můžete následující příkaz pro import klíče ze souboru .pem ve vašem počítači. Tento příkaz importuje klíč do HSM ve službě Key Vault:

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --protection 'hsm' --pem-password 'PaSSWORD'
```
Další příkaz importuje balíček „přineste si vlastní klíč“ (BYOK). To vám umožní vygenerovat klíč v místním HSM a jeho přenesení do HSM ve službě Key Vault, aniž by klíč opustil hranice HSM.

```
az keyvault key import --vault-name 'ContosoKeyVaultHSM' --name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --protection 'hsm'
```
Podrobnější pokyny o tom, jak generovat tento balíček BYOK naleznete v tématu [použití HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů
Pokud již nepotřebujete trezor klíčů a klíč nebo tajný klíč, který obsahuje, můžete trezor klíčů odstranit pomocí `az keyvault delete` příkaz:

```
az keyvault delete --name 'ContosoKeyVault'
```

Nebo můžete odstranit celou skupinu prostředků Azure, která zahrnuje trezor klíčů a všechny další prostředky, které jste do skupiny zahrnuli.

```
az group delete --name 'ContosoResourceGroup'
```

## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Ostatní příkazy rozhraní příkazového řádku Azure a platformy
Další příkazy, že může využít ke správě Azure Key Vault.

Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:

AZ seznam klíčů keyvault – název trezoru ContosoKeyVault

Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč:

Zobrazit klíč keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.

Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:

AZ keyvault tajný seznamu – název trezoru ContosoKeyVault

Tady je příklad odebrání určitého klíče:

odstranění klíče keyvault az--název trezoru ContosoKeyVault – název 'ContosoFirstKey.

Tady je příklad odebrání určitého tajného klíče:

Odstranit tajný klíč keyvault az--název trezoru ContosoKeyVault – název 'SQLPassword.


## <a name="next-steps"></a>Další kroky
Úplný referenční rozhraní příkazového řádku Azure pro příkazy trezoru klíčů, najdete v části [odkaz klíč trezoru rozhraní příkazového řádku](/cli/azure/keyvault)

Programátorské reference najdete v [příručce pro vývojáře Azure Key Vault](key-vault-developers-guide.md).
