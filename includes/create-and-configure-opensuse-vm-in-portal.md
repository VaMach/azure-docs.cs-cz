1. Přihlaste se k webu [Azure Portal](http://portal.azure.com).  
2. Klikněte na tlačítko **nový** > **výpočetní** > **najdete v článku všechny** a pak zadejte **SUSE**.
   
3. Vyberte bitovou kopii virtuálního počítače OpenSUSE a pak klikněte na šipku pokračujte.
5. V prvním **konfigurace virtuálního počítače** stránky:
   
   * Zadejte **název virtuálního počítače**, jako je například "testlinuxvm". Název musí obsahovat 3 až 15 znaků, může obsahovat pouze písmena, číslice a pomlčky a musí začínat písmenem a končit písmenem nebo číslicí.
   * Ověřte **vrstvy** a vyberte **velikost**. Určuje vrstvu velikostí, které můžete vybrat z. Velikost ovlivní náklady na používání, stejně jako možnosti konfigurace, jako například kolik dat disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](../articles/virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
   * Zadejte **nové uživatelské jméno**, nebo přijměte výchozí nastavení, **azureuser**. Tento název se přidá do souboru Sudoers seznamu.
   * Rozhodněte, který typ **ověřování** používat. Obecné heslo, naleznete na adrese [silná hesla](http://msdn.microsoft.com/library/ms161962.aspx).
6. Při dalším **konfigurace virtuálního počítače** stránky:
   
   * Použít výchozí **vytvořte novou cloudovou službu**.
   * V **název DNS** zadejte jedinečný název DNS, který bude použit jako část adresy, jako je například "testlinuxvm".
   * V **oblasti nebo vztahů skupiny nebo virtuální síť** vyberte oblast, kde se bude hostovat tuto bitovou kopii virtuálního.
   * V části **koncové body**, zachovat koncový bod SSH. Můžete přidat další nyní, nebo přidat, změnit nebo odstranit, je po vytvoření virtuálního počítače.
     
     > [!NOTE]
     > Pokud chcete virtuální počítač používat virtuální síť, můžete **musí** při vytváření virtuálního počítače zadejte virtuální síť. Virtuální počítač nelze přidat k virtuální síti, po vytvoření virtuálního počítače. Další informace najdete v tématu [Přehled virtuálních sítí](../articles/virtual-network/virtual-networks-overview.md).
     > 
     > 
7. Na poslední **konfigurace virtuálního počítače** , potvrďte výchozí nastavení a pak klikněte na tlačítko zaškrtnutí dokončete.

Na portálu zobrazí nový virtuální počítač v rámci **virtuální počítače**. Když stav je uveden jako **(zřizování)**, virtuální počítač se nastavuje. Pokud je stav uveden jako **systémem**, můžete přesunout další krok.

## <a name="connect-to-the-virtual-machine"></a>Připojit k virtuálnímu počítači
SSH nebo PuTTY budete používat pro připojení k virtuálnímu počítači, v závislosti na operační systém na počítač, na kterém budete připojení z:

* Z počítače se systémem Linux pomocí SSH. Na příkazovém řádku zadejte:
  
    `$ ssh newuser@testlinuxvm.cloudapp.net -o ServerAliveInterval=180`
  
    Zadejte heslo uživatele.
* Z počítače se systémem Windows použití klienta PuTTY. Pokud nemáte nainstalováno, ji stáhnout z [stránku položek ke stažení PuTTY][PuTTYDownload].
  
    Uložit **putty.exe** do adresáře v počítači. Otevřete příkazový řádek, přejděte do této složky a spusťte **putty.exe**.
  
    Zadejte název hostitele, jako je například "testlinuxvm.cloudapp.net" a "22" zadejte **Port**.
  
    ![PuTTY obrazovky][Image6]  

## <a name="update-the-virtual-machine-optional"></a>Aktualizovat virtuální počítač (volitelné)
1. Poté, co jste připojení k virtuálnímu počítači, můžete volitelně nainstalovat opravy a aktualizace systému. Chcete-li spustit aktualizaci, zadejte:
   
    `$ sudo zypper update`
2. Vyberte **softwaru**, pak **Online aktualizace** seznam dostupných aktualizací. Vyberte **přijmout** spuštění instalace a použít všechny nové dostupných oprav (s výjimkou těch, volitelný).
3. Po dokončení instalace, vyberte **Dokončit**.  Systém je nyní aktuální.

[PuTTYDownload]: http://www.puttyssh.org/download.html

[Image6]: ./media/create-and-configure-opensuse-vm-in-portal/putty.png
