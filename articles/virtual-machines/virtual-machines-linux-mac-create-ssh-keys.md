---
title: "Vytvoření páru veřejného a privátního klíče SSH pro virtuální počítače s Linuxem | Dokumentace Microsoftu"
description: "Vytvoření páru veřejného a privátního klíče SSH pro virtuální počítače s Linuxem."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/12/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 330637f5b69ad95aef149d9fbde16f2151cde837
ms.openlocfilehash: 5c515dbe8e3030abf079e5ff47884fb04b9048ba


---

# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Vytvoření páru veřejného a privátního klíče SSH pro virtuální počítače s Linuxem

Tento článek ukazuje, jak vygenerovat pár veřejného a privátního klíče SSH pro použití s virtuálními počítači s Linuxem.  Pomocí páru klíčů SSH můžete v Azure vytvořit službu Virtual Machines, která ve výchozím nastavení používá klíče SSH k ověřování. Není potom potřeba používat k přihlášení hesla.  Hesla se dají uhodnout a jejich používání může vaše virtuální počítače vystavit útokům hrubou silou při pokusech o jejich uhodnutí. Virtuální počítače vytvářené pomocí šablon Azure nebo `azure-cli` mohou jako součást svého nasazení zahrnovat veřejný klíč SSH. Po nasazení potom není potřeba provádět krok konfigurace, při kterém se zakáže přihlašování pomocí hesla pro SSH.

## <a name="quick-commands"></a>Rychlé příkazy

Nahraďte příklady vlastními volbami a spusťte následující příkazy z prostředí Bash.

Klíče SSH jsou ve výchozím nastavení v adresáři `.ssh`.  

```bash
cd ~/.ssh/
```

Pokud adresář `~/.ssh` nemáte, vytvoří ho za vás příkaz `ssh-keygen` se správnými oprávněními.

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
```

Zadejte název souboru privátního klíče uloženého v adresáři `~/.ssh/`:

```bash
~/.ssh/id_rsa
```

Zadejte přístupové heslo pro id_rsa:

```bash
correct horse battery staple
```

V adresáři `~/.ssh` je teď dvojice klíčů SSH `id_rsa` a `id_rsa.pub`.

```bash
ls -al ~/.ssh
```

Přidejte nově vytvořený klíč do `ssh-agent`:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

Zkopírujte veřejný klíč SSH do svého virtuálního počítače s Linuxem:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

Otestujte přihlášení s použitím klíče místo hesla:

```bash
ssh -o PreferredAuthentications=publickey -o PubkeyAuthentication=yes -i ~/.ssh/id_rsa ahmet@myserver
Last login: Tue April 12 07:07:09 2016 from 66.215.22.201
$
```

Protokol SSH je úspěšně nakonfigurovaný, jestliže nejste vyzváni k zadání hesla privátního klíče SSH nebo přihlašovacího hesla k virtuálnímu počítači.

## <a name="detailed-walkthrough"></a>Podrobný postup

Použití veřejných a privátních klíčů SSH představuje nejjednodušší způsob, jak se přihlásit k linuxovým serverům. [Kryptografie využívající veřejný klíč](https://en.wikipedia.org/wiki/Public-key_cryptography) poskytuje mnohem bezpečnější způsob přihlašování k virtuálním počítačům s Linuxem nebo BSD v Azure než používání hesel, které je možné rozluštit (útokem hrubou silou) mnohem snadněji.

Veřejný klíč je možné s kýmkoli sdílet. Pouze vy (nebo vaše místní infrastruktura zabezpečení) ale máte privátní klíč.  Vytvořený privátní klíč SSH by měl mít [velmi zabezpečené heslo](https://www.xkcd.com/936/) pro svoji ochranu (zdroj:[xkcd.com](https://xkcd.com)).  Toto heslo se používá jenom pro přístup k privátnímu klíči SSH a **není** to heslo k uživatelskému účtu.  Když si ke svému klíči SSH přidáte heslo, zašifruje se pomocí něj privátní klíč, aby privátní klíč nebylo možné bez hesla odemknout.  Pokud by se útočníkovi povedlo odcizit privátní klíč a tento klíč by neměl heslo, mohl by ho použít k přihlášení k vašim serverům, které mají odpovídající veřejný klíč.  Pokud je privátní klíč chráněný heslem, útočník ho nemůže použít, což znamená další úroveň zabezpečení pro vaši infrastrukturu v Azure.

V tomto článku jsou vytvořené soubory klíčů s formátem *ssh-rsa*, které se doporučují pro nasazení v Resource Manageru.  Klíče *ssh-rsa* jsou vyžadované na [portálu](https://portal.azure.com) pro nasazení Classic i pro nasazení Resource Manageru.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Zakázání hesel SSH použitím klíčů SSH

Azure vyžaduje minimálně 2048bitové veřejné a privátní klíče ve formátu ssh-rsa. K vytvoření páru klíčů použijeme příkaz `ssh-keygen`, který se zeptá na několik otázek a pak zapíše privátní klíč a odpovídající veřejný klíč. Když se vytvoří virtuální počítač Azure VM, veřejný klíč se zkopíruje do `~/.ssh/authorized_keys`.  Klíče SSH v `~/.ssh/authorized_keys` vybízí klienta, aby pro přihlašovací připojení SSH použil odpovídající privátní klíč.  Při vytváření virtuálního počítače Azure s Linuxem, který používá ověřování pomocí klíčů SSH, Azure nakonfiguruje server SSHD tak, aby zakazoval přihlašování pomocí hesla a povoloval pouze klíče SSH.  Proto se při vytváření virtuálních počítačů Azure s Linuxem, které používají klíče SSH, nasadí virtuální počítače ve výchozím nastavení zabezpečené a tím vám ušetří obvyklý krok konfigurace po nasazení, při kterém se v konfiguračním souboru `sshd_config` zakáže používání hesel.

## <a name="using-ssh-keygen"></a>Použití příkazu ssh-keygen

Tento příkaz vytvoří pár klíčů SSH zabezpečený (zašifrovaný) heslem pomocí 2048bitového šifrování RSA a je pro snadnější identifikaci opatřený komentáři.  

Začněte změnou adresářů tak, aby se všechny vaše klíče SSH vytvořily v tomto adresáři.

```bash
cd ~/.ssh
```

Pokud adresář `~/.ssh` nemáte, vytvoří ho za vás příkaz `ssh-keygen` se správnými oprávněními.

```bash
ssh-keygen -t rsa -b 2048 -C "myusername@myserver"
```

*Vysvětlení příkazu*

`ssh-keygen`= program použitý k vytvoření klíčů

`-t rsa` = typ klíče, který se má vytvořit a který je [formát RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bity klíče

`-C "myusername@myserver"`= komentář připojený na konec souboru veřejného klíče pro snadnější identifikaci.  Standardně se jako komentář používá e-mail, můžete ale použít cokoli, co je pro vaši infrastrukturu vhodné.

### <a name="using-pem-keys"></a>Použití klíčů PEM

Pokud používáte klasický model nasazení (portál Azure Classic nebo rozhraní příkazového řádku Azure Service Management `asm`), možná budete pro přístup k linuxovým virtuálním počítačům potřebovat klíče SSH s formátem PEM.  Tady je postup pro vytvoření klíče PEM na základě stávajícího veřejného klíče SSH a certifikátu x509.

Postup vytvoření klíče s formátem PEM z existujícího veřejného klíče SSH:

```bash
ssh-keygen -f ~/.ssh/id_rsa.pub -e > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Příklad ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa.
Your public key has been saved in id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
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

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): id_rsa`

Název páru klíčů pro tento článek.  Výchozí je pár klíčů s názvem **id_rsa**, přičemž to, že se bude soubor privátního klíče jmenovat **id_rsa**, můžou očekávat i některé nástroje, takže je vhodné tento název použít. Výchozím umístěním pro páry klíčů SSH a konfigurační soubor SSH je adresář `~/.ssh/`.

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```
Výpis adresáře `~/.ssh` `ssh-keygen` vytvoří adresář `~/.ssh`, pokud neexistuje, a také nastaví správné režimy vlastnictví a souboru.

Heslo klíče:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` používá pro heslo označení přístupové heslo.  *Důrazně* doporučujeme přidat si do párů klíčů heslo. Pokud by pár klíčů nebyl chráněný heslem, mohl by kdokoli, kdo má soubor privátního klíče, použít tento klíč k přihlášení k jakémukoli serveru s odpovídajícím veřejným klíčem. Přidání hesla nabízí vyšší ochranu v případě, že by se někomu povedlo získat přístup k vašemu souboru s privátním klíčem, a poskytne vám čas změnit si klíče používané k vašemu ověření.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Použití příkazu ssh-agent k uložení hesla k soukromému klíči 

Abyste při každém přihlašování přes SSH nemuseli zadávat heslo k souboru s privátním klíčem, můžete si pomocí příkazu `ssh-agent` uložit heslo k souboru s privátním klíčem do mezipaměti. Pokud používáte počítač Mac, při vyvolání příkazu `ssh-agent` se hesla k privátním klíčům bezpečně uloží v řetězci klíčů v OSX.

Nejdřív ověřte, že `ssh-agent` běží.

```bash
eval "$(ssh-agent -s)"
```

Potom přidejte privátní klíč do `ssh-agent` pomocí příkazu `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Heslo privátního klíče je teď uložené v `ssh-agent`.

## <a name="create-and-configure-an-ssh-config-file"></a>Vytvoření a nakonfigurování konfiguračního souboru SSH

Vytvoření a konfigurace souboru `~/.ssh/config` patří mezi doporučené osvědčené postupy. Zrychluje přihlášení a optimalizuje chování klienta SSH.

V následujícím příkladu vidíte standardní konfiguraci.

### <a name="create-the-file"></a>Vytvoření souboru

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Upravením souboru přidejte novou konfiguraci SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Příklad souboru `~/.ssh/config`:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Tato konfigurace SSH obsahuje oddíly pro každý server, aby každý z nich mohl mít svůj vlastní vyhrazený pár klíčů. Výchozí nastavení (`Host *`) platí pro všechny hostitele, kteří neodpovídají žádnému konkrétních hostitelů uvedených v konfiguračním souboru výše.

### <a name="config-file-explained"></a>Vysvětlení konfiguračního souboru

`Host`= název hostitele volaného na terminálu.  `ssh fedora22` říká protokolu `SSH`, aby použil hodnoty v bloku nastavení s popiskem `Host fedora22`. POZNÁMKA: Hostitel může mít libovolný popisek, který pro vaše použití dává smysl a který nepředstavuje skutečný název hostitele žádného serveru.

`Hostname 102.160.203.241`= IP adresa nebo název DNS serveru, ke kterému přistupujete.

`User ahmet` = vzdálený uživatelský účet, který se má používat při přihlašování k serveru.

`PubKeyAuthentication yes`= říká protokolu SSH, že chcete k přihlášení použít klíč SSH.

`IdentityFile /home/ahmet/.ssh/id_id_rsa`= privátní klíč SSH a odpovídající veřejný klíč pro ověřování.

## <a name="ssh-into-linux-without-a-password"></a>Přihlášení do Linuxu bez hesla pomocí protokolu SSH

Teď máte pár klíčů SSH a nakonfigurovaný konfigurační soubor SSH a můžete se rychle a bezpečně přihlašovat ke svým virtuálním počítačům s Linuxem. Při prvním přihlášení k serveru pomocí klíče SSH vás příkaz vyzve k zadání přístupového hesla pro tento soubor klíče.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Vysvětlení příkazu

Při provádění příkazu `ssh fedora22` protokol SSH nejdříve vyhledá a načte všechna nastavení z bloku `Host fedora22` a pak načte všechna zbývající nastavení z posledního bloku, `Host *`.

## <a name="next-steps"></a>Další kroky

Dalším krokem je vytvoření virtuálního počítače Azure s Linuxem pomocí nového veřejného klíče SSH.  Vytvářené virtuální počítače Azure, pro které se v přihlašovacích údajích používá veřejný klíč SSH, jsou lépe zabezpečené než ty, které jsou vytvořené pomocí výchozí metody přihlašování s použitím hesel.  Virtuální počítače Azure vytvořené pomocí klíčů SSH jsou ve výchozím nastavení nakonfigurované se zakázaným heslem, aby se vyloučily pokusy o rozluštění hesla (útokem hrubou silou).

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí webu Azure Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



<!--HONumber=Dec16_HO2-->


