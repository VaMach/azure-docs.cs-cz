---
title: "Instalace jazyka Python a sady SDK – Azure"
description: "Informace o instalaci Pythonu a sady SDK pro použití s Azure."
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.openlocfilehash: e69fff29be5b12c3c0004b4101eba69c7da87d3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="installing-python-and-the-sdk"></a>Instalace jazyka Python a sady SDK
Python je možné snadno nastavit v systému Windows a obsahuje předem nainstalovaná v systému Mac, Linux, a [Bash pro systém Windows](https://msdn.microsoft.com/commandline/wsl/about). Tento průvodce vás provede procesem instalace a získávání váš počítač připravený k použití s Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Co je v Python Azure SDK?
Azure SDK pro jazyk Python obsahuje součásti, které vám umožní vyvíjet, nasazení a Správa aplikací Python pro Azure. Konkrétně sadu Azure SDK pro jazyk Python zahrnuje následující:

* **Správa knihovny**. Tyto knihovny tříd poskytují rozhraní správu prostředků Azure, například účty úložiště, virtuální počítače.
* **Knihovny za běhu**. Tyto knihovny tříd poskytují rozhraní pro přístup k Azure funkce, jako je například úložiště a service bus.

## <a name="which-python-and-which-version-to-use"></a>Které Python a která verze se má použít
Nejsou k dispozici několik typů Python překladače – mezi příklady patří:

* CPython – standard a nejčastěji používané překladač Pythonu
* PyPy - rychlé, kompatibilní s jinou implementaci na CPython
* IronPython - překladač Pythonu, která běží na rozhraní .net/CLR
* Jython - překladač Pythonu, který běží na virtuálním počítači Java

**CPython** v2.7 nebo v3.3 + a PyPy 5.4.0 jsou testovány a podporovány pro sady Azure SDK pro Python.

## <a name="where-to-get-python"></a>Kde získat jazyk Python?
Chcete-li získat CPython několika způsoby:

* Přímo z [www.python.org][www.python.org]
* Z důvěryhodných distro jako [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] nebo [www.activestate.com][www.activestate.com]
* Sestavení ze zdroje!

Pokud máte konkrétní potřebu, doporučujeme první dvě možnosti.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>Instalace sady SDK v systému Windows, Linux a systému MacOS (pouze klientské knihovny)
Pokud je již nainstalován jazyk Python, můžete k instalaci sady všechny knihovny klienta ve vaší stávající Python 2.7 nebo prostředí Python 3.3 + pip. Tato akce stáhne balíčky ze [indexu balíčků Pythonu] [ Python Package Index] (úložiště PyPI).

Budete pravděpodobně potřebovat oprávnění správce:

* Linux a systému MacOS, použijte `sudo` příkaz: `sudo pip install azure-mgmt-compute`.
* Windows: Otevřete prostředí PowerShell nebo příkazový řádek jako správce

Můžete nainstalovat jednotlivě každý knihovny pro každou službu Azure:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Náhled balíčky můžete nainstalovat pomocí `--pre` příznak:

```console
   $ pip install --pre azure-mgmt-compute # installs only the latest Compute Management library
```

Můžete taky nainstalovat sadu Azure knihovny v jediném řádku pomocí `azure` metabalíček. Vzhledem k tomu, že ne všechny balíčky v tomto balíčku meta publikují se jako stabilní ještě `azure` metabalíček je stále ve verzi preview.
Však balíčky jádra, z perspektivy kvality/úplnost kód lze považovat za "stabilní" v tuto chvíli

* oficiálně označen jako takový synchronizované s jinými jazyky co nejdříve.
  Jsme nejsou plánování na všechny další hlavní změny do té doby.

Vzhledem k tomu, že je verze preview, budete muset použít `--pre` příznak:

```console
   $ pip install --pre azure
```

nebo přímo

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Získávání další balíčky
[Indexu balíčků Pythonu] [ Python Package Index] (úložiště PyPI) má bohaté výběr Python knihovny.  Pokud jste vybrali k instalaci Distro, budete již mít většinu zajímavé bitů pro různé scénáře vývoje webů na technické projekty.

## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio
[Python Tools pro Visual Studio][Python Tools pro Visual Studio] (PTVS) je plug-in bez/OSS od společnosti Microsoft, která převede VS do plnohodnotný IDE Python:

![How-k-install-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Pomocí PTVS je nepovinný, ale doporučuje se jako nabízí podporu jazyka Python a webový projekt nebo řešení, ladění, profilace, interaktivních okna, úprav šablony a technologii Intellisense.

PTVS také usnadňuje nasazení do služby Microsoft Azure s podporou pro nasazení do [cloudové služby](cloud-services/cloud-services-python-ptvs.md) a [weby](app-service/app-service-web-overview.md).

PTVS funguje s vaší stávající instalaci sady Visual Studio 2013, 2015 nebo 2017.  Dokumentace, stahování a diskusí najdete v tématu [Python Tools pro Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scénáře pro systémy Linux a systému MacOS
Pro Linux ani systému MacOS, hlavní scénáře Azure, které jsou podporovány:

1. Využívání služeb Azure pomocí klientské knihovny pro jazyk Python
2. Spuštění aplikace v virtuálního počítače s Linuxem
3. Vývoj a publikování na weby Azure pomocí Git

První scénář umožňuje vytvářet bohaté webových aplikací, které využít výhod funkcí Azure PaaS, jako [úložiště objektů blob](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [fronty úložiště](storage/queues/storage-python-how-to-use-queue-storage.md), [tabulky úložiště](cosmos-db/table-storage-how-to-use-python.md) apod prostřednictvím Pythonic obálek pro rozhraní API REST Azure. Fungují stejně jako v systému Windows, Mac a Linux.  Můžete taky tyto knihovny klienta z místní vývojovém počítači nebo virtuálního počítače s Linuxem spuštěné v Azure.

Pro tento scénář virtuální počítač jednoduše spustit virtuální počítač s Linuxem podle vašeho výběru (Ubuntu, CentOS, Suse) a spouštět a spravovat co se vám líbí.  Například můžete spustit [IPython] [ IPython] REPL/poznámkového bloku na vašem systému Windows nebo Mac/Linux počítač a přejděte v prohlížeči na systému Linux nebo Windows více procesorů virtuálního počítače modul IPython v Azure.

Informace o tom, jak nastavit virtuální počítač s Linuxem najdete v tématu [vytvořit virtuálním počítači systémem Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kurzu.

Pomocí nasazení Git, můžete vyvíjet webovou aplikaci Python a publikování na web Azure ve všech operačních systémech.  Když stisknete úložiště do Azure, automaticky vytvoří virtuální prostředí a pip nainstaluje požadované balíčky.

Další informace o použití žádné kompatibilní se standardem WSGI framework najdete v tématu [konfigurace Python s weby Azure](app-service/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>Další Software a prostředky:
* [Azure SDK pro Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK pro Python Githubu](https://github.com/Azure/azure-sdk-for-python)
* [Oficiální ukázek Azure pro jazyk Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Distribuci jazyka Python poměrně Analytics][Continuum Analytics Python Distribution]
* [Distribuci jazyka Python Enthought][Enthought Python Distribution]
* [Distribuci jazyka Python ActiveState][ActiveState Python Distribution]
* [SciPy - sada knihoven Scientific Python][SciPy - A suite of Scientific Python libraries]
* [NumPy - číslice knihovny pro jazyk Python][NumPy - A numerics library for Python]
* [Projekt Django – vyspělá webové framework/CMS][Django Project - A mature web framework/CMS]
* [IPython - k pokročilé REPL/Poznámkový blok pro jazyk Python][IPython - an advanced REPL/Notebook for Python]
* [Python Tools pro Visual Studio na Githubu][Python Tools for Visual Studio on GitHub]
* [Středisko pro vývojáře programující v Pythonu](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[Python Tools pro Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
