# <a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Jak používat nástroje příkazového řádku Azure pro Mac a Linux
Tato příručka popisuje, jak vytvářet a spravovat služby v Azure pomocí nástrojů příkazového řádku Azure pro Mac a Linux. Mezi popisované scénáře patří **instalace nástrojů**, **import nastavení publikování**, **vytvoření a správa Azure Websites** a **vytvoření a správa Azure Virtual Machines**. Komplexní referenční dokumentaci najdete v tématu [Dokumentace k nástroji příkazového řádku Azure pro Mac a Linux][reference-docs]. 

## <a name="table-of-contents"></a>Obsah
* [Co jsou nástroje příkazového řádku Azure pro Mac a Linux](#Overview)
* [Instalace nástrojů příkazového řádku Azure pro Mac a Linux](#Download)
* [Vytvoření účtu Azure](#CreateAccount)
* [Stažení a import nastavení publikování](#Account)
* [Vytvoření a správa webu Azure](#WebSites)
* [Vytvoření a správa virtuálního počítače Azure](#VMs)

<h2><a id="Overview"></a>Co jsou nástroje příkazového řádku Azure pro Mac a Linux</h2>

Nástroje příkazového řádku Azure pro Mac a Linux jsou sadou nástrojů příkazového řádku pro nasazení a správu služeb Azure.

Mezi podporované úlohy patří:

* Import nastavení publikování
* Vytvoření a správa Azure Websites
* Vytvoření a správa Azure Virtual Machines

Úplný seznam podporovaných příkazů získáte zadáním `azure -help` na příkazovém řádku po instalaci nástrojů nebo si přečtěte [referenční dokumentaci][reference-docs].

<h2><a id="Download">Instalace nástrojů příkazového řádku Azure pro Mac a Linux</a></h2>

Následující seznam obsahuje informace pro instalaci nástrojů příkazového řádku v závislosti na operačním systému:

* **Mac:** Stáhněte si [Instalační program sady Azure SDK][mac-installer]. Otevřete stažený soubor .pkg a na vyzvání dokončete postup instalace.
* **Linux:** Nainstalujte si nejnovější verzi [Node.js][nodejs-org] (viz [Instalace Node.js přes Správce balíčků][install-node-linux]), pak spusťte následující příkaz:
  
        npm install azure-cli -g
  
    **Poznámka:** Možná bude nutné spustit tento příkaz se zvýšenými oprávněními:
  
        sudo npm install azure-cli -g
* **Windows:** Spusťte instalační službu systému Windows (soubor .msi), která je k dispozici tady: [Nástroje příkazového řádku Azure][windows-installer].

Chcete-li otestovat instalaci, zadejte na příkazovém řádku `azure`. Pokud byla instalace úspěšná, zobrazí se seznam všech dostupných příkazů `azure`.

<h2><a id="CreateAccount"></a>Vytvoření účtu Azure</h2>

Pro použití nástrojů příkazového řádku Azure pro Mac a Linux budete potřebovat účet Azure.

Otevřete webový prohlížeč, přejděte na [http://www.windowsazure.com][windowsazuredotcom] a klikněte na **bezplatný účet** v pravém horním rohu.

![Web Azure][Azure Web Site]

Postupujte podle pokynů pro vytvoření účtu.

<h2><a id="Account"></a>Stažení a import nastavení publikování</h2>

Abyste mohli začít, musíte si nejprve stáhnout a naimportovat vlastní nastavení publikování. To vám umožní používat tyto nástroje k vytváření a správě služeb Azure. Ke stažení nastavení publikování použijte příkaz `account download`:

    azure account download

Otevře se váš výchozí prohlížeč a budete vyzváni k přihlášení k Portálu pro správu. Po přihlášení se stáhne váš soubor `.publishsettings`. Poznamenejte si, kam se soubor uloží.

Dále soubor `.publishsettings` naimportujte spuštěním následujícího příkazu, kde nahradíte `{path to .publishsettings file}` cestou k vašemu souboru `.publishsettings`:

    azure account import {path to .publishsettings file}

Všechny informace uložené příkazem <code>import</code> můžete odebrat pomocí příkazu <code>account clear</code>:

    azure account clear

Seznam možností pro příkazy `account` zobrazíte pomocí možnosti `-help`:

    azure account -help

Po importu nastavení publikování byste soubor `.publishsettings` měli z bezpečnostních důvodů odstranit.

> [!NOTE]
> Při importu nastavení publikování jsou přihlašovací údaje pro přístup k vašemu předplatnému Azure uloženy ve vaší složce `user`. Vaše složka `user` je chráněna operačním systémem. Přesto se doporučuje podniknout další kroky k zašifrování vaší složky `user`. Můžete tak učinit následujícími způsoby:    
> 
> * V systému Windows upravte vlastnosti složky nebo použijte nástroj BitLocker.
> * V systému Mac zapněte pro složku FileVault.
> * V Ubuntu použijte funkci Šifrovaný domovský adresář. Ostatní linuxové distribuce nabízí ekvivalentní funkce.
> 
> 

Nyní jste připraveni začít vytvářet a spravovat Azure Websites a Azure Virtual Machines.  

<h2><a id="WebSites"></a>Vytvoření a správa webu Azure</h2>

### <a name="create-a-website"></a>Vytvoření webu
Pokud chcete vytvořit web Azure, nejprve vytvořte prázdný adresář `MySite` a přejděte do něj.

Pak spusťte následující příkaz:

    azure site create MySite --git

Výstup tohoto příkazu bude obsahovat výchozí adresu URL pro nově vytvořený web. Možnost `--git` umožňuje použít k publikování na váš web Git, který vytvoří úložiště Git v místním adresáři aplikace i v datovém centru vašeho webu. Poznámka: Pokud již vaše místní složka je úložištěm Git, tento příkaz přidá k existujícímu úložišti nový vzdálený odkaz, které bude odkazovat na úložiště v datovém centru vašeho webu.

Všimněte si, že příkaz `azure site create` můžete spustit s některými z těchto možností:

* `--location [location name]`. Tato možnost umožňuje zadat umístění datového centra, ve které je vytvořený váš web (např. „Západní USA“). Pokud tuto možnost vynecháte, budete vyzváni k výběru umístění.
* `--hostname [custom host name]`. Tato možnost umožňuje zadat vlastní název hostitele pro váš web.

Potom můžete přidat obsah do adresáře vašeho webu. K potvrzení obsahu použijte běžný postup Gitu (`git add`, `git commit`). Pomocí následujícího příkazu nuceně vložte (push) obsah vašeho webu do Azure: 

    git push azure master

### <a name="set-up-publishing-from-github"></a>Nastavení publikování z GitHubu
Pokud chcete nastavit průběžné publikování z úložiště GitHub, při vytváření webu použijte možnost `--GitHub`:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Pokud máte místní klon úložiště GitHub nebo pokud máte úložiště s jedním vzdáleným odkazem na úložiště GitHub, tento příkaz automaticky publikuje kód v úložišti GitHub na váš web. Od této chvíle budou veškeré nuceně vložené (push) změny v úložišti GitHub automaticky publikovány na váš web.

Když nastavíte publikování z GitHubu, jako výchozí větev se použije hlavní větev. Pokud chcete zadat jinou větev, spusťte následující příkaz z místního úložiště:

    azure site repository <branch name>

### <a name="configure-app-settings"></a>Konfigurace nastavení aplikace
Nastavení aplikace jsou páry klíčů a hodnot, které jsou k dispozici vaší aplikaci za běhu. Pokud jsou nastavené pro web Azure, hodnoty nastavení aplikace přepíší nastavení se stejným klíčem definovaná v souboru Web.config vašeho webu. Pro aplikace v Node.js a PHP jsou nastavení aplikace k dispozici jako proměnné prostředí. Následující příklad ukazuje, jak nastavit pár klíče a hodnoty:

    azure site config add <key>=<value> 

Seznam všech párů klíčů a hodnot zobrazíte následovně:

    azure site config list 

Případně pokud znáte klíč a chcete zobrazit hodnotu, můžete použít:

    azure site config get <key> 

Pokud chcete změnit hodnotu existujícího klíče, musíte nejprve vymazat existující klíč a pak jej znovu přidat. Příkaz pro vymazání je:

    azure site config clear <key> 

### <a name="list-and-show-sites"></a>Seznam a zobrazení webů
K zobrazení seznamu vašich webů použijte následující příkaz:

    azure site list

Podrobné informace o webu získáte příkazem `site show`. Následující příklad zobrazí podrobnosti o webu `MySite`:

    azure site show MySite

### <a name="stop-start-or-restart-a-site"></a>Zastavení, spuštění nebo restartování webu
Zastavit, spustit nebo restartovat web můžete pomocí příkazů `site stop`, `site start`, nebo `site restart`:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

### <a name="delete-a-site"></a>Odstranění webu
Nakonec můžete web odstranit pomocí příkazu `site delete`:

    azure site delete MySite

Všimněte si, že pokud spouštíte některý z výše uvedených příkazů ze složky, ve které jste spustili příkaz `site create`, není nutné jako poslední parametr zadávat název webu `MySite`.

Úplný seznam příkazů `site` zobrazíte pomocí možnosti `-help`:

    azure site -help 

<h2><a id="VMs"></a>Vytvoření a správa virtuálního počítače Azure</h2>

Virtuální počítač Azure se vytvoří z image virtuálního počítače (soubor .vhd), který poskytnete nebo který je k dispozici v galerii imagí. Dostupné image zobrazíte pomocí příkazu `vm image list`:

    azure vm image list

Zřídit a spustit virtuální počítač z některé z dostupných imagí můžete pomocí příkazu `vm create`. Následující příklad ukazuje, jak vytvořit virtuální počítač s Linuxem (s názvem `myVM`) z image v galerii imagí (CentOS 6.2). Uživatelské jméno a heslo uživatele root pro virtuální počítač jsou `myusername`, respektive `Mypassw0rd`. (Všimněte si, že parametr `--location` určuje datové centrum, ve kterém se virtuální počítač vytváří. Pokud parametr `--location` vynecháte, budete vyzváni k výběru umístění.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Můžete zvážit předání příznaku `--ssh` (Linux) nebo `--rdp` (Windows) k příkazu `vm create` pro povolení vzdálených připojení k nově vytvořenému virtuálnímu počítači.

Pokud byste raději zřídili virtuální počítač z vlastní image, můžete vytvořit image ze souboru .vhd pomocí příkazu `vm image create` a pak pomocí příkazu `vm create` zřídit virtuální počítač. Následující příklad ukazuje, jak vytvořit image Linuxu (s názvem `myImage`) z místního souboru .vhd. (Parametr `--location` určuje datové centrum, ve kterém se image uloží.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Místo vytváření image z místního souboru .vhd můžete vytvořit image ze souboru .vhd uloženého ve službě Azure Blob Storage. Můžete to provést pomocí parametru `blob-url`:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Po vytvoření image z ní můžete zřídit virtuální počítač pomocí příkazu `vm create`. Následující příkaz vytvoří virtuálního počítač `myVM` z image vytvořené v předchozím kroku (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Po zřízení virtuálního počítače můžete vytvořit koncové body, které umožní vzdálený přístup k virtuálnímu počítači (například). Následující příklad pomocí příkazu `vm create endpoint` otevře externí port 22 a místní port 22 na virtuálním počítači `myVM`:

    azure vm endpoint create myVM 22 22

Podrobné informace o virtuálním počítači (včetně IP adresy, názvu DNS a informací o koncových bodech) získáte pomocí příkazu `vm show`:

    azure vm show myVM

Pokud chcete vypnout, spustit nebo restartovat virtuální počítač, požijte některý z následujících příkazů:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

A nakonec můžete virtuální počítač odstranit pomocí příkazu `vm delete`:

    azure vm delete myVM

Úplný seznam příkazů pro vytváření a správu virtuálních počítačů získáte pomocí možnosti `-h`:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com



<!--HONumber=Jan17_HO5-->


