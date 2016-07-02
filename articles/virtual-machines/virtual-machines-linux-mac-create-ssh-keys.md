<properties
    pageTitle="Vytvoření klíčů SSH na Linuxu a Macu | Microsoft Azure"
    description="Zjistěte, jak vygenerovat a používat klíče SSH na Linux a Macu pro model nasazení pomocí Resource Manageru a model nasazení Classic na platformě Azure."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="v-livech"/>

# Vytvoření klíčů SSH na Linuxu a Macu pro virtuální počítače s Linuxem v Microsoft Azure

Abyste mohli vytvořit veřejný a privátní klíč zabezpečený přes SSH, budete na pracovní stanici potřebovat otevřený terminál.  Jakmile budete mít klíče SSH, budete moct pomocí tohoto klíče ve výchozím nastavení vytvořit nové virtuální počítače nebo přidat veřejný klíč do existujících virtuálních počítačů pomocí Azure CLI i šablon Azure.  To vám umožní přihlašovat se bez hesla přes SSH a místo hesel používat daleko bezpečnější způsob ověřování – klíče.

## Rychlý výpis příkazu

V následujících příkladech nahraďte hodnoty mezi závorkami (&lt; a &gt;) hodnotami ze svého vlastního prostředí.

```bash
ssh-keygen -t rsa -b 2048 -C "<your_user@yourdomain.com>"
```

Zadejte název souboru, který se uloží v adresáři `~/.ssh/`:

```bash
<azure_fedora_id_rsa>
```

Zadejte přístupové heslo pro azure_fedora_id_rsa:

```bash
<correct horse battery staple>
```

Přidejte nově vytvořený klíč do `ssh-agent` na Linuxu a Macu (přidáno také do Klíčenky v OSX):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/azure_fedora_id_rsa
```

Zkopírujte veřejný klíč SSH na Linux Server:

```bash
ssh-copy-id -i ~/.ssh/azure_fedora_id_rsa.pub <youruser@yourserver.com>
```

Otestujte přihlášení s použitím klíče místo hesla:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/azure_fedora_id_rsa <youruser@yourserver.com>
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

## Úvod

Používání veřejných a privátních klíčů SSH představuje nejjednodušší způsob, jak se přihlašovat ke svým linuxovým serverům. Kromě [kryptografie využívající veřejného klíče](https://en.wikipedia.org/wiki/Public-key_cryptography) ale navíc poskytuje mnohem bezpečnější způsob přihlašování k virtuálním počítačům s Linuxem nebo BSD v Azure než používání hesel, které je možné rozluštit (útokem hrubou silou) mnohem snadněji. Veřejný klíč je možné s kýmkoli sdílet. Pouze vy (nebo vaše místní infrastruktura zabezpečení) ale máte privátní klíč.  Vytvořený privátní klíč SSH bude mít vytvořené [zabezpečeného heslo](https://www.xkcd.com/936/) pro jeho ochranu a toto heslo se používá pouze pro přístup k privátnímu klíči SSH, tzn. **není** to heslo k uživatelskému účtu.  Když si ke svému klíči SSH přidáte heslo, zašifruje se pomocí něj soukromý klíč, aby privátní klíč nebylo možné bez hesla odemknout.  Pokud by se útočníkovi povedlo odcizit privátní klíč a neměl by k němu heslo, mohl by ho použít k přihlášení k vašim serverům, pro které je nainstalovaný odpovídající veřejný klíč.  Pokud je soukromý klíč chráněný heslem, útočník ho nemůže použít, což znamená další úroveň zabezpečení pro vaši infrastrukturu v Azure.


Tento článek vytvoří soubory klíčů ve formátu *ssh-rsa*, jak se doporučuje pro nasazení v Resource Manageru a vyžaduje na [portálu](https://portal.azure.com) jak pro nasazení pomocí modelu Classic, tak pro nasazení prostřednictvím Resource Manageru.


## Vytvoření klíčů SSH

Azure vyžaduje minimálně 2048bitové veřejné a privátní klíče ve formátu ssh-rsa. K vytvoření páru klíčů použijeme příkaz `ssh-keygen`, který se zeptá na několik otázek a pak zapíše privátní klíč a odpovídající veřejný klíč. Při vytváření svého virtuálního počítače Azure předáte obsah veřejného klíče, který se zkopíruje do virtuálních počítačů s Linuxem a používá se s vaším místním a bezpečně uloženým privátním klíčem k ověření při každém vašem přihlášení.

## Použití příkazu ssh-keygen

Tento příkaz vytvoří dvojici klíčů SSH zabezpečenou (zašifrovanou) heslem pomocí 2048bitového algoritmu RSA a bude okomentován pro snadnější identifikaci.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
```

_Vysvětlení příkazu_

`ssh-keygen` = program použitý k vytvoření klíčů

`-t rsa` = typ klíče, který se má vytvořit, který je [formát RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = bity klíče

`-C "ahmet@fedoraVMAzure"` = komentář připojený na konec souboru veřejného klíče pro snadnější identifikaci.  Standardně se jako komentář používá e-mail, můžete ale použít cokoli, co vhodné pro vaši infrastrukturu.

## Návod pro použití příkazu ssh-keygen

Podrobné vysvětlení jednotlivých kroků.  Začněte spuštěním příkazu `ssh-keygen`.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@fedoraVMAzure"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in azure_fedora_id_rsa.
Your public key has been saved in azure_fedora_id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@fedoraVMAzure
The key's randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Uložené soubory klíčů:

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): azure_fedora_id_rsa`

Název páru klíčů pro tento článek.  Výchozí je pár klíčů s názvem **id_rsa**, přičemž to, že se bude soubor privátního klíče jmenovat **id_rsa**, můžou očekávat i některé nástroje, takže je vhodné tento název použít. (Obvyklým výchozím umístěním pro všechny vaše páry klíčů SSH a konfigurační soubor SSH je umístění `~/.ssh/`.)

```bash
ahmet@fedora$ ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 azure_fedora_id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 azure_fedora_id_rsa.pub
```
Tím se zobrazí vaše nové páry klíčů a jejich oprávnění. `ssh-keygen` vytvoří adresář `~/.ssh`, pokud neexistuje, a také nastaví správné režimy vlastnictví a souboru.

Heslo klíče:

`Enter passphrase (empty for no passphrase):`

Doporučujeme si do párů klíčů přidat heslo (pro `ssh-keygen` se používá označení passphrase, tzn. přístupové heslo). Pokud by pár klíčů nebyl chráněný heslem, mohl by kdokoli, kdo má kopii souboru privátního klíče, použít tento klíč k přihlášení k jakémukoli (vašemu) serveru, který má odpovídající veřejný klíč. Přidání hesla tak nabízí daleko vyšší ochranu v případě, že by se někomu povedlo získat přístup k vašemu souboru se soukromým klíčem, a získáte tak čas změnit si klíče používané k vašemu ověření.

## Použití příkazu ssh-agent k uložení hesla k soukromému klíči 

Abyste při každém přihlašování přes SSH nemuseli zadávat heslo k souboru s privátním klíčem, můžete si pomocí příkazu `ssh-agent` uložit heslo k souboru s privátním klíčem do mezipaměti. To pak umožňuje rychle a bezpečně se přihlašovat k virtuálním počítačům s Linuxem.  Pokud používáte OSX, uloží se vám hesla k privátním klíčům při vyvolání příkazu `ssh-agent` bezpečně v Klíčence.

Nejdřív ověřte, že `ssh-agent` běží.

```bash
eval "$(ssh-agent -s)"
```

Teď do `ssh-agent` pomocí příkazu `ssh-add` přidejte privátní klíč (opět platí, že na OSX se tím spustí Klíčenka, do které se přihlašovací údaje uloží).

```bash
ssh-add ~/.ssh/azure_fedora_id_rsa
```

Heslo k privátnímu klíči je teď uložené, takže ho nebudete muset zadávat znovu při každém přihlašování přes SSH.

## Vytvoření a nakonfigurování konfiguračního souboru SSH

Přestože to není nezbytně nutné ke zprovoznění virtuálního počítače s Linuxem, je vhodné vytvořit a nakonfigurovat soubor `~/.ssh/config`, aby se vám zabránilo v nechtěném použití hesel k přihlášení pomocí vašich virtuálních počítačů a bylo možné automatizovat použití různých párů klíčů pro různé virtuální počítače Azure a nakonfigurovat další programy, jako **git** tak, aby také cílily na více serverů.

V následujícím příkladu vidíte standardní konfiguraci.

### Vytvoření souboru

```bash
touch ~/.ssh/config
```

### Upravením souboru přidejte novou konfiguraci SSH:

```bash
vim ~/.ssh/config
```

### Příklad souboru `~/.ssh/config`:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
  PubkeyAuthentication yes
  IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=no
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath /home/ahmet/.ssh/Connections/ssh-%r@%h:%p
  ControlPersist 4h
  StrictHostKeyChecking=no
  IdentityFile /home/ahmet/.ssh/id_rsa
  UseRoaming=no
```

Tato konfigurace SSH obsahuje oddíly pro každou službu, aby každá z nich mohla mít svůj vlastní vyhrazený pár klíčů. Výchozí nastavení platí pro všechny hostitele, ke kterým jste přihlášení a kteří neodpovídají žádné z výše uvedených skupin.


### Vysvětlení konfiguračního souboru

`Host` = název hostitele volaného na terminálu.  `ssh fedora22` říká protokolu `SSH`, aby použil hodnoty v bloku nastavení s popiskem `Host fedora22`. POZNÁMKA: Může to být libovolný popisek, který dává smysl pro vaše použití a nepředstavuje skutečný název hostitele žádného serveru.

`Hostname 102.160.203.241` = IP adresa nebo název DNS serveru, ke kterému se přihlašujete. Používá se ke směrování na server a může to být externí IP adresa, která se mapuje na interní IP adresu.

`User git` = vzdálený uživatelský účet, který se má používat při přihlašování k virtuálnímu počítači Azure

`PubKeyAuthentication yes` = říká protokolu SSH, že chcete k přihlášení použít klíč SSH.

`IdentityFile /home/ahmet/.ssh/azure_fedora_id_rsa` = říká protokolu SSH, který pár klíčů má předat serveru k ověření přihlašovacích údajů.


## Přihlášení do Linuxu bez hesla pomocí protokolu SSH

Teď, když máte pár klíčů SSH a nakonfigurovaný konfigurační soubor SSH, můžete se rychle a bezpečně přihlašovat ke svým virtuálním počítačům s Linuxem. Při prvním přihlášení k serveru pomocí klíče SSH vás příkaz požádá o přístupové heslo pro tento soubor klíče.

```bash
ssh fedora22
```

### Vysvětlení příkazu

Při provádění příkazu `ssh fedora22` protokol SSH nejdříve vyhledá a načte všechna nastavení z bloku `Host fedora22` a pak načte všechna zbývající nastavení z posledního bloku, `Host *`.

## Další kroky

Dalším krokem je vytvoření virtuálního počítače Azure s Linuxem pomocí nového veřejného klíče SSH.  Vytvářené virtuální počítače Azure, pro které se v přihlašovacích údajích používá veřejný klíč SSH, jsou lépe zabezpečené než ty, které jsou vytvořené pomocí výchozí metody přihlašování s použitím hesel.  Virtuální počítače Azure, které pro přihlášení používají klíče SSH, jsou ve výchozím nastavení nakonfigurované tak, aby zakazovaly přihlášení pomocí hesla, a vyloučily se tak pokusy o rozluštění hesla (útokem hrubou silou).

- [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí Portálu Azure](virtual-machines-linux-quick-create-portal.md)
- [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure CLI](virtual-machines-linux-quick-create-cli.md)



<!--HONumber=Jun16_HO2-->


