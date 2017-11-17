---
title: "Spravovat pomocí rozhraní příkazového řádku Azure Key Vault | Microsoft Docs"
description: "Tento kurz použijte k automatizaci běžných úkolů v Key Vault pomocí rozhraní příkazového řádku"
services: key-vault
documentationcenter: 
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 66be6e44-684a-411b-802e-884628458ae7
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: barclayn
ms.openlocfilehash: 94ea95e7f40c8d47dd18422a9c0795655dae365b
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="manage-key-vault-using-cli"></a>Spravovat Key Vault pomocí rozhraní příkazového řádku

Pomocí tohoto kurzu můžete začít pracovat s Azure Key Vault v Azure vytvořit zesílený kontejner (trezor). Azure Key vault se používá k ukládání a spravovat kryptografické klíče a tajné klíče. Tento článek vás provede procesem vytvoření trezoru pomocí rozhraní příkazového řádku pro různé platformy Azure. Potom budou používat úložiště k provádění různých běžných operací. 

Azure Key Vault je dostupný ve většině oblastí. Další informace najdete na [stránce s cenami Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).

**Odhadovaný čas dokončení:** 20 minut

> [!NOTE]
> Tento kurz neobsahuje pokyny o tom, jak napsat aplikaci Azure, že obsahuje jeden z kroků. Tato ukázková aplikace se používá k zobrazení, jak aplikace může být autorizovaný k použití klíče nebo tajného klíče uložené v trezoru klíčů.
> Tento článek se zaměřuje na konfiguraci pomocí Multiplatformního rozhraní příkazového řádku Azure Key Vault. Prostředí Azure PowerShell pokyny najdete v tématu [tomto ekvivalentním kurzu](key-vault-get-started.md).

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

Také si můžete přečíst následující kurzy a seznamte se s Azure Resource Manager probíhající modelu v rozhraní příkazového řádku Azure napříč platformami:

* [Postup instalace a konfigurace rozhraní příkazového řádku Azure a platformy](../cli-install-nodejs.md)
* [Pomocí rozhraní příkazového řádku a platformy Azure s Azure Resource Manager](../xplat-cli-azure-resource-manager.md)

## <a name="connect-to-your-subscriptions"></a>Připojte se ke svým předplatným

Přihlásit se do Azure pomocí následujícího příkazu:

```azurecli-interactive
azure login -u username -p password
```

nebo pokud se chcete přihlásit interaktivně zadáním

```azurecli-interactive
azure login
```

Máte-li více předplatných a chcete určit jedno konkrétní, které se má použít pro Azure Key Vault, zadejte následující příkaz k zobrazení předplatných pro váš účet:

```azurecli-interactive
azure account list
```

Poté pro určení předplatného, které se má použít, zadejte:

```azurecli-interactive
azure account set <subscription name>
```

Další informace o konfiguraci rozhraní příkazového řádku pro různé platformy Azure najdete v tématu [postup instalace a konfigurace rozhraní příkazového řádku Azure napříč platformami](../cli-install-nodejs.md).

## <a name="switch-to-using-azure-resource-manager"></a>Přejít na používání Azure Resource Manager
Key Vault vyžaduje Azure Resource Manager, takže zadejte následující příkaz a přepněte do režimu Azure Resource Manager:

```azurecli-interactive
azure config mode arm
```

## <a name="create-a-new-resource-group"></a>Vytvoření nové skupiny prostředků
Pokud používáte Azure Resource Manager, všechny související prostředky vytváří v uvnitř skupiny prostředků. Pro účely tohoto kurzu vytvoříme novou skupinu prostředků, ContosoResourceGroup'.

```azurecli-interactive
azure group create 'ContosoResourceGroup' 'East Asia'
```

První parametr je název skupiny prostředků a druhý parametr je umístění. Pro umístění, použijte příkaz `azure location list` zjišťují, jak zadat alternativní umístění na v tomto příkladu. Pokud potřebujete více informací, zadejte:`azure help location`

## <a name="register-the-key-vault-resource-provider"></a>Registrace poskytovatele prostředků Key Vault
Ujistěte se, že poskytovatele prostředků Key Vault je zaregistrován v rámci vašeho předplatného:

```azurecli-interactive
azure provider register Microsoft.KeyVault
```

To jenom je nutné provést jednou za předplatné.

## <a name="create-a-key-vault"></a>Vytvořte trezor klíčů

Použití `azure keyvault create` příkaz pro vytvoření trezoru klíčů. Tento skript má tři povinné parametry: název skupiny prostředků, název trezoru klíčů a zeměpisné umístění.

Například:
- Pokud použijete název trezoru **ContosoKeyVault**
- Název skupiny prostředků **ContosoResourceGroup**
- Umístění **východní Asie** typu:

```azurecli-interactive
azure keyvault create --vault-name 'ContosoKeyVault' --resource-group 'ContosoResourceGroup' --location 'East Asia'
```

Výstup tohoto příkazu zobrazuje vlastnosti trezoru klíčů, který jste právě vytvořili. Dvě nejdůležitější vlastnosti jsou:

* **Název**: V tomto příkladu to je ContosoKeyVault. Tento název budete používat pro další rutiny Key Vault.
* **vaultUri**: V tomto příkladu to je https://contosokeyvault.vault.azure.net. Aplikace, které používají váš trezor prostřednictvím REST API musí používat tento identifikátor URI.

Váš účet Azure je nyní oprávněn provádět nad tímto trezorem klíčů všechny operace. Zatím k tomu není oprávněn nikdo jiný.

## <a name="add-a-key-or-secret-to-the-key-vault"></a>Přidejte k trezoru klíč nebo tajný klíč

Pokud chcete Azure Key Vault vytvořila softwarově chráněný klíč pro vás, použijte `azure key create` příkaz a zadejte následující příkaz:

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --destination software
````

Nicméně pokud máte existující klíč v soubor .pem uložený jako místního souboru do souboru s názvem softkey.pem, který chcete nahrát do Azure Key Vault, zadejte následující příkaz pro import klíče z. Soubor PEM, který chrání klíč softwarem ve službě Key Vault:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey' --pem-file './softkey.pem' --password 'PaSSWORD' --destination software
```

Nyní můžete odkazovat klíč, který jste vytvořili nebo nahrán do Azure Key Vault, pomocí jeho identifikátoru URI. Použít **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** vždy získáte aktuální verzi a používat **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** Chcete-li získat konkrétní verzi.

Chcete-li přidat tajný klíč k úložišti, který je hesla s názvem SQLPassword a hodnotou z Pa$ $w0rd do Azure Key Vault, zadejte následující příkaz:

```azurecli-interactive
azure keyvault secret set --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword' --value 'Pa$$w0rd'
```

Nyní na toto heslo, které jste přidali do Azure Key Vault, můžete odkazovat pomocí jeho identifikátoru URI. Pomocí **https://ContosoVault.vault.azure.net/secrets/SQLPassword** vždy získáte aktuální verzi. Chcete-li získat konkrétní verzi, použijte **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d**.

Podívejme se na klíč nebo tajný klíč, který jste právě vytvořili:

* Pokud chcete zobrazit klíč, zadejte `azure keyvault key list --vault-name 'ContosoKeyVault'`
* Pokud chcete zobrazit tajný klíč, zadejte `azure keyvault secret list --vault-name 'ContosoKeyVault'`

## <a name="register-an-application-with-azure-active-directory"></a>Registrujte aplikaci s Azure Active Directory

Tento krok obvykle provádí vývojář na samostatném počítači. Není specifické pro Azure Key Vault, ale je zahrnuté pro úplnost.

> [!IMPORTANT]
> K dokončení tohoto kurzu, váš účet, trezor a aplikace, která zaregistrujete v tomto kroku musí být ve stejné službě Azure Active directory.

Aplikace, které používají trezor klíčů, se musí ověřit pomocí tokenu z Azure Active Directory. K tomu je potřeba, aby majitel aplikace nejprve zaregistroval aplikaci ve vlastním Azure Active Directory. Na konci registrace obdrží majitel aplikace následující hodnoty:

- **ID aplikace** 
- **Ověřovací klíč** (také označovaný jako sdílený tajný klíč). 

Pro získání tokenu musí aplikace obě tyto hodnoty poskytnout do Azure Active Directory. Záleží na aplikaci, jak je k tomu nakonfigurovaná. Pro [ukázkovou aplikaci Key Vault](https://www.microsoft.com/download/details.aspx?id=45343), nastavuje majitel tyto hodnoty v souboru app.config.

Podrobné pokyny k registraci aplikace v Azure Active Directory měli přečtěte si článek s názvem [integrace aplikací s Azure Active Directory](../active-directory/develop/active-directory-integrating-applications.md) nebo [použití portálu k vytvoření Azure Active Aplikace Directory a objektu služby, které mají přístup k prostředkům](../azure-resource-manager/resource-group-create-service-principal-portal.md) zaregistrovat aplikaci v Azure Active Directory:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně klikněte na tlačítko **registrace aplikace**. Pokud nevidíte registrace aplikace kliknete na **další služby** a najdete je tam.  
    >[!NOTE]
    Musíte vybrat stejný adresář, který obsahuje předplatné Azure, který jste použili pro vytvoření trezoru klíčů. 
3. Klikněte na tlačítko **nové registrace aplikace**.
4. Na **vytvořit** okně zadejte název pro vaši aplikaci a pak vyberte **webové aplikace nebo webové rozhraní API** (výchozí) a zadejte **adresa URL přihlašování** pro váš web aplikace. Pokud tyto informace v tuto chvíli nemáte, můžete ho nastavit pro tento krok (je třeba zadat http://test1.contoso.com). Nezáleží na tom, zda tyto weby existují. 

   ![Registrace nové aplikace](./media/key-vault-manage-with-cli/new-application-registration.png)

5. Klikněte na tlačítko **Vytvořit**.
6. Po dokončení registrace aplikace zobrazí seznam registrovaných aplikací. Najděte aplikaci, který jste právě zaregistrován a klepněte na ni.
7. Klikněte na **registrovaná aplikace** okno kopie **ID aplikace**
8. Klikněte na **všechna nastavení**
9. Na **nastavení** okno kliknutím na tlačítko **klíče**
10. Zadejte popis do **klíče popis** pole a vyberte dobu trvání a poté klikněte na tlačítko **Uložit**. Stránka se obnoví a zobrazí hodnotu klíče. 
11. Budete používat **ID aplikace** a **klíč** informace v dalším kroku nastavit oprávnění pro svůj trezor.

## <a name="authorize-the-application-to-use-the-key-or-secret"></a>Autorizujte aplikaci pro použití klíče nebo tajného klíče
Chcete-li autorizovat aplikaci pro přístup ke klíči nebo tajný klíč v trezoru, použijte:

```azurecli-interactive
azure keyvault set-policy
```

Například pokud je název vaší trezoru ContosoKeyVault a aplikace, které chcete autorizovat má hodnotu 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed ID klienta a chcete aplikaci autorizovat pro dešifrování a podepisování pomocí klíčů ve vašem trezoru, spusťte následující:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-keys '[\"decrypt\",\"sign\"]'
```

> [!NOTE]
> Pokud používáte na příkazovém řádku Windows, doporučujeme nahradit jednoduchých uvozovek a být dvojité uvozovky a také řídicí interní dvojité uvozovky. Například: "[\"dešifrovat\",\"přihlašovací\"]".
> 

Chcete-li tu samou aplikaci autorizovat pro čtení tajných klíčů v trezoru, spusťte následující:

```azurecli-interactive
azure keyvault set-policy --vault-name 'ContosoKeyVault' --spn 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed --perms-to-secrets '[\"get\"]'
```

## <a name="if-you-want-to-use-a-hardware-security-module-hsm"></a>Pokud chcete použít modul hardwarového zabezpečení (HSM)
Pro lepší kontrolu můžete importovat nebo generovat klíče v modulech hardwarového zabezpečení (HSM), které nikdy neopustí hranice HSM. Moduly hardwarového zabezpečení jsou ověřené podle standardu FIPS 140-2 Level 2. Pokud se vás tento požadavek netýká, přeskočte tuto část a přejděte k části [Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů](#delete-the-key-vault-and-associated-keys-and-secrets).

Pro vytvoření těchto klíčů chráněných pomocí HSM, musíte mít předplatné trezoru s podporou klíčů chráněných pomocí HSM.

Když vytvoříte keyvault, přidejte parametr 'sku':

```azurecli-interactive
azure azure keyvault create --vault-name 'ContosoKeyVaultHSM' --resource-group 'ContosoResourceGroup' --location 'East Asia' --sku 'Premium'
```

Do tohoto trezoru můžete přidat klíče chráněné softwarem (jak jsme ukázali výše) a klíče chráněné pomocí HSM. Chcete-li vytvořit klíč chráněný HSM, nastavte parametr cílové na "HSM":

```azurecli-interactive
azure keyvault key create --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --destination 'HSM'
```

Můžete následující příkaz pro import klíče ze souboru .pem ve vašem počítači. Tento příkaz importuje klíč do HSM ve službě Key Vault:

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --pem-file '/.softkey.pem' --destination 'HSM' --password 'PaSSWORD'
```

Další příkaz importuje balíček „přineste si vlastní klíč“ (BYOK). To vám umožní vygenerovat klíč v místním HSM a jeho přenesení do HSM ve službě Key Vault, aniž by klíč opustil hranice HSM.

```azurecli-interactive
azure keyvault key import --vault-name 'ContosoKeyVaultHSM' --key-name 'ContosoFirstHSMKey' --byok-file './ITByok.byok' --destination 'HSM'
```

Podrobnější pokyny o tom, jak generovat tento balíček BYOK naleznete v tématu [použití HSM-Protected klíčů s Azure Key Vault](key-vault-hsm-protected-keys.md).

## <a name="delete-the-key-vault-and-associated-keys-and-secrets"></a>Odstranění trezoru klíčů, přidružených klíčů a tajných klíčů
Pokud již nepotřebujete trezor klíčů a klíč nebo tajný klíč, který obsahuje, můžete trezor klíčů odstranit pomocí příkazu delete azure keyvault:

```azurecli-interactive
azure keyvault delete --vault-name 'ContosoKeyVault'
```

Nebo můžete odstranit celou skupinu prostředků Azure, která zahrnuje trezor klíčů a všechny další prostředky, které jste do skupiny zahrnuli.

```azurecli-interactive
azure group delete --name 'ContosoResourceGroup'
```


## <a name="other-azure-cross-platform-command-line-interface-commands"></a>Ostatní příkazy rozhraní příkazového řádku Azure a platformy
Další příkazy, že může využít ke správě Azure Key Vault.

Tento příkaz vypíše tabulkové zobrazení všech klíčů a vybraných vlastností:

```azurecli-interactive
azure keyvault key list --vault-name 'ContosoKeyVault'
```

Tento příkaz zobrazí úplný seznam vlastností pro zadaný klíč:

```azurecli-interactive
azure keyvault key show --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Tento příkaz vypíše tabulkové zobrazení všech názvů tajných klíčů a vybraných vlastností:

```azurecli-interactive
azure keyvault secret list --vault-name 'ContosoKeyVault'
```

Tady je příklad odebrání určitého klíče:

```azurecli-interactive
azure keyvault key delete --vault-name 'ContosoKeyVault' --key-name 'ContosoFirstKey'
```

Tady je příklad odebrání určitého tajného klíče:

```azurecli-interactive
azure keyvault secret delete --vault-name 'ContosoKeyVault' --secret-name 'SQLPassword'
```


## <a name="next-steps"></a>Další kroky
- Programátorské reference najdete v [příručce pro vývojáře Azure Key Vault](key-vault-developers-guide.md).
- Souhrnné informace o Azure Key Vault naleznete v tématu [Co je Azure Key Vault?](key-vault-whatis.md).
- Jak pracovat s Azure Key Vault pomocí prostředí Powershell [Azure Key Vault Začínáme](key-vault-get-started.md).


