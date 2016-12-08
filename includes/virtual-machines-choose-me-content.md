<a name="tellmevm"></a>

## <a name="tell-me-about-virtual-machines"></a>Informace o virtuálních počítačích
Služba Azure Virtual Machine vám umožní vytvořit a používat virtuální počítače v cloudu. Díky tomu, co se nazývá *IaaS (infrastruktura jako služba)*, může být technologie virtuálních počítačů použita mnoha různými způsoby. Tady je několik příkladů:

* **Virtuální počítače pro vývoj a testování.** Vývojářské skupiny virtuální počítače běžně používají, protože nabízejí rychlý a snadný způsob, jak vytvořit počítač s konkrétní konfigurací pro kódování a testování aplikací. Služba Azure Virtual Machine poskytuje snadný a nákladově výhodný způsob, jak tyto virtuální počítače vytvářet, používat a nakonec je odstranit, když už nejsou potřeba.
* **Spouštění aplikací v cloudu.** Spouštění některých aplikací ve veřejném cloudu přináší úspory. Jedním z příkladů je aplikace, která má extrémní hodnoty poptávky. I když můžete vlastní datové centrum vybavit dostatečně výkonným hardwarem pro zpracování poptávky ve špičce, bude takový hardware většinu času značně nevyužitý. Spuštění této aplikace v Azure umožní platit peníze navíc za virtuální počítače jen v případě, kdy je potřebujete, a v ostatních případech je prostě vypnete. Nebo předpokládejme, že jste startup, který potřebuje výpočetní prostředky na vyžádání rychle a bez závazků. I tady platí, že Azure může být správnou volbou.
* **Rozšíření vlastního datového centra do veřejného cloudu.** Když používáte službu Azure Virtual Network, může si vaše organizace vytvořit virtuální síť (VNET), která je rozšířením místní sítě, a do ní může virtuální počítače přidat. To umožňuje spouštění aplikací [SharePoint](../articles/virtual-machines/virtual-machines-windows-sharepoint-farm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), [server SQL Server](../articles/virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a dalších ve virtuálním počítači Azure. Tento přístup může nasazování usnadnit nebo zlevnit ve srovnání se spouštěním aplikací ve virtuálních počítačích vlastního datového centra.   
* **Zotavení po havárii.** Proč neustále platit za zálohovací datové centrum, který se používá jen občas, když vám zotavení po havárii založené na IaaS umožní platit za výpočetní prostředky jenom v případě, kdy je skutečně potřebujete?  Pokud se vaše primární datové centrum ocitne mimo provoz, můžete vytvořit virtuální počítače spuštěné v Azure, v nich můžete spustit základní aplikace, které vypnete v okamžiku, když už je nepotřebujete.

Podobně jako jiné virtuální počítače používají i virtuální počítače v Azure operační systém, úložiště a sítě a můžete v nich spustit celou řadu aplikací. Můžete použít image poskytovaný Azure nebo jedním z jeho partnerů, popřípadě můžete použít vlastní. Příklady představují různé verze, vydání a konfigurace následujících systémů:

* Linuxové servery, například Suse, Ubuntu a CentOS
* Windows Server 
* SQL Server
* BizTalk Server 
* SharePoint Server

Virtuální počítače používají virtuální pevné disky (VHD), na které ukládají svůj operační systém (OS) a data. Virtuální pevné disky se používají i pro image, ze kterých si můžete nainstalovat operační systém. Následující obrázek to znázorňuje spolu se dvěma nástroji pro vytváření a správu virtuálních počítačů.

<a name="fig_createvms"></a>
![vm_diagram](./media/virtual-machines-choose-me-content/diagram.png)

**Obrázek: Služba Azure Virtual Machine poskytuje infrastrukturu jako službu.**

Virtuální počítače můžete spravovat pomocí portálu (přes prohlížeč), nástrojů příkazového řádku s podporou pro skriptování, nebo přímo prostřednictvím REST API. Partneři Microsoftu, například RightScale a ScaleXtreme také poskytují služby pro správu, které jsou závislé na REST API. 

Kromě volby operačního systému máte v případě virtuálních počítačů i další konfigurační možnosti:

* Velikost, která určuje faktory rozhodující o tom, kolik disků můžete připojit a jak velký bude výpočetní výkon. Azure nabízí širokou škálu velikostí, které podporují mnoho typů použití. Podrobnosti najdete v článku [Velikosti služeb Virtual Machines](../articles/virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  
* Oblast Azure, kde bude nový virtuální počítač hostovaný, například USA, Evropa nebo Asie. 
* Rozšíření virtuálních počítačů, která virtuálním počítačům dávají další možnosti, například spuštění antivirového programu nebo použití funkce prostředí Windows PowerShell Konfigurace požadovaného stavu.

Mezi další výhody, které hovoří pro využívání virtuálních počítačů, patří následující:

**Průběžné platby** – Azure účtuje hodinovou cenu na základě velikosti virtuálního počítače a na jeho operačním systému. V případě neúplných hodin Azure účtuje jenom využité minuty. Služba Storage je oceněna a účtována samostatně. Podrobnosti najdete v článku [Ceny služeb Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).

**Odolnost** – Azure sleduje fyzický hardware, který je hostitelem každého spuštěného virtuálního počítače. Pokud fyzický server se spuštěným virtuálním počítačem selže, Azure to zaznamená, přesune virtuální počítač na nový hardware a restartuje virtuální počítač. Tento proces se někdy nazývá službou opravy. Azure také chrání data virtuálního počítače tím, že udržuje redundantní kopie virtuálních pevných disků ve službě Blob Storage. 



<!--HONumber=Nov16_HO2-->


