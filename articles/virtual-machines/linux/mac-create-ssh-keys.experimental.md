---
title: "Vytvoření páru klíčů SSH pro virtuální počítače s Linuxem v Azure | Dokumentace Microsoftu"
description: "Vytvořte bezpečně dvojici veřejného a privátního klíče SSH pro virtuální počítače Azure s Linuxem."
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
ms.date: 03/08/2017
ms.author: rasquill
experiment_id: rasquill-ssh-20170308
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 19acd4efca7ef043f31b436b96f9129caee9591b
ms.lasthandoff: 04/03/2017



---

# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Vytvoření páru veřejného a privátního klíče SSH pro virtuální počítače s Linuxem

Tento článek ukazuje, jak vygenerovat pár souborů veřejného a privátního klíče protokolu SSH verze 2 RSA pro použití s virtuálními počítači s Linuxem.  Pomocí páru klíčů SSH můžete v Azure vytvořit službu Virtual Machines, která ve výchozím nastavení používá klíče SSH k ověřování. Není potom potřeba používat k přihlášení hesla.  Hesla se dají uhodnout a jejich používání může vaše virtuální počítače vystavit útokům hrubou silou při pokusech o jejich uhodnutí. Virtuální počítače vytvářené pomocí šablon Azure nebo `azure-cli` mohou jako součást svého nasazení zahrnovat veřejný klíč SSH. Po nasazení potom není potřeba provádět krok konfigurace, při kterém se zakáže přihlašování pomocí hesla pro SSH.

## <a name="quick-commands"></a>Rychlé příkazy

Nahraďte příklady vlastními volbami a spusťte následující příkazy z prostředí Bash.

Souboru veřejného klíče SSH se ve výchozím nastavení vytvoří v `~/.ssh/id_rsa.pub`. Pokud použijete následující příkaz, po zobrazení výzvy byste měli vytvořit přístupové heslo, které zabezpečí váš privátní klíč. (Přístupové heslo je heslo použité k šifrování privátního klíče.)

```bash
ssh-keygen -t rsa -b 2048 
```

Přidejte nově vytvořený klíč do `ssh-agent`:

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> Výše uvedené příkazy fungují v operačních systémech Linux téměř všech distribucí, ale nemusí pracovat v kontejnerech, protože prostředí může být výrazně omezené. 

## <a name="detailed-walkthrough"></a>Podrobný postup

Použití veřejných a privátních klíčů SSH představuje nejjednodušší způsob, jak se přihlásit k linuxovým serverům. [Kryptografie využívající veřejný klíč](https://en.wikipedia.org/wiki/Public-key_cryptography) poskytuje mnohem bezpečnější způsob přihlašování k virtuálním počítačům s Linuxem nebo BSD v Azure než používání hesel, které je možné rozluštit (útokem hrubou silou) mnohem snadněji.

> [!IMPORTANT]
> Veřejný klíč je možné s kýmkoli sdílet. Pouze vy (nebo vaše místní infrastruktura zabezpečení) ale máte privátní klíč.  Privátní klíč SSH musí mít [velmi zabezpečené heslo](https://www.xkcd.com/936/) (zdroj: [xkcd.com](https://xkcd.com)), který ho chrání.  Toto heslo se používá jenom pro přístup k privátnímu klíči SSH a **není** to heslo k uživatelskému účtu.  Když ke klíči SSH přidáte heslo, použije se k zašifrování klíče 128bitový standard AES, aby se privátní klíč bez tohoto hesla nedal dešifrovat.  Pokud by se útočníkovi povedlo odcizit privátní klíč a tento klíč by neměl heslo, mohl by ho použít k přihlášení k vašim serverům, které mají odpovídající veřejný klíč.  Pokud je privátní klíč chráněný heslem, útočník ho nemůže použít, což znamená další úroveň zabezpečení pro vaši infrastrukturu v Azure.

V tomto článku se vytvoří soubory veřejného a privátního klíče RSA protokolu SSH verze 2, které se doporučují pro nasazení v Resource Manageru.  Klíče *ssh-rsa* jsou na [portálu](https://portal.azure.com) potřeba jak při klasickém nasazení, tak při nasazení Resource Manageru.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Zakázání hesel SSH použitím klíčů SSH

Azure vyžaduje minimálně 2048bitové veřejné a privátní klíče ve formátu ssh-rsa. K vytvoření páru klíčů použijeme příkaz `ssh-keygen`, který se zeptá na několik otázek a pak zapíše privátní klíč a odpovídající veřejný klíč. Když se vytvoří virtuální počítač Azure VM, veřejný klíč se zkopíruje do `~/.ssh/authorized_keys`.  Klíče SSH v `~/.ssh/authorized_keys` vybízí klienta, aby pro přihlašovací připojení SSH použil odpovídající privátní klíč.  Při vytváření virtuálního počítače Azure s Linuxem, který používá ověřování pomocí klíčů SSH, Azure nakonfiguruje server SSHD tak, aby zakazoval přihlašování pomocí hesla a povoloval pouze klíče SSH.  Když vytvoříte virtuální počítače Azure s Linuxem a klíči SSH, zabezpečíte tím nasazené virtuální počítače a ušetříte si obvyklý konfigurační krok, který následuje po nasazení a spočívá v zakázání hesel v konfiguračním souboru sshd_config.

## <a name="using-ssh-keygen"></a>Použití příkazu ssh-keygen

Tento příkaz vytvoří pár klíčů SSH zabezpečený (zašifrovaný) heslem pomocí 2048bitového šifrování RSA a je pro snadnější identifikaci opatřený komentáři.  

Klíče SSH jsou ve výchozím nastavení v adresáři `~/.ssh`.  Pokud adresář `~/.ssh` nemáte, vytvoří ho za vás příkaz `ssh-keygen` se správnými oprávněními.

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

*Vysvětlení příkazu*

`ssh-keygen`= program použitý k vytvoření klíčů

`-t rsa` = typ vytvářeného klíče ve formátu RSA [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`= bity klíče


## <a name="classic-portal-and-x509-certs"></a>Klasický portál a certifikáty X.509

Pokud používáte [klasický portál](https://manage.windowsazure.com/) Azure, vyžaduje pro klíče SSH certifikáty X.509.  Jiné typy veřejných klíčů SSH nejsou povolené, *musí* jít o certifikáty X.509.

Vytvoření certifikátu X.509 ze stávajícího privátního klíče SSH-RSA:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Klasické nasazení pomocí `asm`

Pokud používáte klasický model nasazení (rozhraní CLI `asm` správy služby Azure), můžete v kontejneru **.pem** použít veřejný klíč SSH-RSA nebo klíč ve formátu RFC4716.  Veřejný klíč SSH-RSA byl vytvořen v předchozí části tohoto článku příkazem `ssh-keygen`.

Vytvoření klíče ve formátu RFC4716 z existujícího veřejného klíče SSH:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Příklad ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
The keys randomart image is:
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

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

Název páru klíčů pro tento článek.  Výchozí je pár klíčů s názvem **id_rsa**, přičemž to, že se bude soubor privátního klíče jmenovat **id_rsa**, můžou očekávat i některé nástroje, takže je vhodné tento název použít. Výchozím umístěním pro páry klíčů SSH a konfigurační soubor SSH je adresář `~/.ssh/`.  Pokud úplnou cestu nezadáte, `ssh-keygen` vytvoří klíče v aktuálním pracovním adresáři, nikoli ve výchozím adresáři `~/.ssh`.

Výpis adresáře `~/.ssh`

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Heslo klíče:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` označuje heslo použité k šifrování privátního klíče jako přístupové heslo.  *Důrazně* doporučujeme přidat si přístupové heslo do párů klíčů. Pokud by privátní klíč nebyl chráněný přístupovým heslem, mohl by kdokoli, kdo má soubor klíče, použít tento klíč k přihlášení k jakémukoli serveru s odpovídajícím veřejným klíčem. Přidání přístupového hesla nabízí vyšší ochranu v případě, že by se někomu povedlo získat přístup k vašemu souboru s privátním klíčem, a poskytne vám čas změnit si klíče používané k vašemu ověření.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Použití příkazu ssh-agent k uložení hesla k soukromému klíči 

Abyste při každém přihlašování přes SSH nemuseli zadávat přístupové heslo k souboru s privátním klíčem, můžete si pomocí příkazu `ssh-agent` uložit heslo k souboru s privátním klíčem do mezipaměti. Pokud používáte počítač Mac, při vyvolání příkazu `ssh-agent` se hesla k privátním klíčům bezpečně uloží v řetězci klíčů v OSX.

Ověřte a použijte příkazy `ssh-agent` a `ssh-add`, abyste informovali systém SSH o souborech s klíči. Potom se přístupové heslo nebude muset používat interaktivně.

```bash
eval "$(ssh-agent -s)"
```

Potom přidejte privátní klíč do `ssh-agent` pomocí příkazu `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Heslo privátního klíče je teď uložené v `ssh-agent`.

## <a name="using-ssh-copy-id-to-install-the-new-key"></a>Použití `ssh-copy-id` k instalaci nového klíče
Pokud jste virtuální počítač už vytvořili, můžete nainstalovat nový veřejný klíč SSH k virtuálním počítačům s Linuxem pomocí následujícího příkazu, ve kterém nahradíte uživatelské jméno virtuálního počítače a adresu serveru vlastními hodnotami:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Vytvoření a nakonfigurování konfiguračního souboru SSH

Vytvoření a konfigurace souboru `~/.ssh/config` patří mezi osvědčené postupy. Zrychluje přihlášení a optimalizuje chování klienta SSH.

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

Dalším krokem je vytvoření virtuálního počítače Azure s Linuxem pomocí nového veřejného klíče SSH.  Vytvářené virtuální počítače Azure, pro které se v přihlašovacích údajích používá veřejný klíč SSH, jsou lépe zabezpečené než ty, které jsou vytvořené pomocí výchozí metody přihlašování s použitím hesel.  Virtuální počítače Azure vytvořené pomocí klíčů SSH jsou ve výchozím nastavení nakonfigurované se zakázaným heslem, aby se vyloučily pokusy o rozluštění hesla (útokem hrubou silou). Pokud potřebujete další pomoc s vytváření páru klíčů SSH nebo potřebujete další certifikáty, například pro použití s portálem Azure Classic, přečtěte si téma [Podrobný postup vytvoření páru klíčů SSH a certifikátů](create-ssh-keys-detailed.md).

* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí šablony Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí webu Azure Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Vytvoření zabezpečeného virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

