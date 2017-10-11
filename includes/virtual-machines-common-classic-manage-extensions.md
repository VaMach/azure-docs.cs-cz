


## <a name="using-vm-extensions"></a>Pomocí rozšíření virtuálního počítače
Rozšíření virtuálního počítače Azure implementovat chování nebo funkce, které buď další programy fungovat na virtuálních počítačích Azure (například **WebDeployForVSDevTest** rozšíření umožňuje sadě Visual Studio pro řešení pro nasazení webu na vašem virtuálním počítači Azure) nebo zadejte možnost pro vás k interakci s virtuálním Počítačem pro podporu některé jiné chování (například můžete použít rozšíření přístupu virtuálních počítačů z prostředí PowerShell, rozhraní příkazového řádku Azure a REST klientů resetovat nebo upravte hodnoty vzdáleného přístupu na vašem virtuálním počítači Azure).

> [!IMPORTANT]
> Úplný seznam rozšíření funkce podporují, najdete v části [rozšíření virtuálního počítače Azure a funkce](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Protože každý rozšíření virtuálního počítače podporuje konkrétní funkci, přesně co můžete a nemůžete dělat s příponou závisí na rozšíření. Proto před změnou virtuálního počítače, zkontrolujte, zda že máte ke čtení v dokumentaci pro rozšíření virtuálního počítače, kterou chcete použít. Odebírání některé rozšíření virtuálního počítače není podporováno; ostatní uživatelé mají vlastnosti, které lze nastavit, které výrazně mění chování virtuálních počítačů.
> 
> 

Běžné úkoly, jsou:

1. Hledání dostupných rozšíření
2. Aktualizace načíst rozšíření
3. Přidání rozšíření
4. Odebrání rozšíření

## <a name="find-available-extensions"></a>Najít dostupných rozšíření
Můžete vyhledat rozšíření a použití rozšířené informace:

* PowerShell
* Rozhraní Azure napříč platformami příkazového řádku (Azure CLI)
* Rozhraní REST API pro správu služeb

### <a name="azure-powershell"></a>Azure PowerShell
Některá rozšíření mít rutiny prostředí PowerShell, které jsou specifické pro, který může zjednodušit jejich konfigurace z prostředí PowerShell; ale následující rutiny fungovat pro všechny rozšíření virtuálního počítače.

Následující rutiny můžete použít k získání informací o dostupných rozšíření:

* Pro instance webové role nebo rolí pracovního procesu, můžete použít [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) rutiny.
* Pro instance virtuálních počítačů, můžete použít [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) rutiny.
  
   Například následující příklad kódu ukazuje, jak uvádí informace k **IaaSDiagnostics** rozšíření pomocí prostředí PowerShell.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Rozhraní příkazového řádku Azure (Azure CLI)
Některá rozšíření mají příkazy rozhraní příkazového řádku Azure, které jsou specifické pro jejich (Docker rozšíření virtuálního počítače je jedním z příkladů), který může zjednodušit jejich konfigurace; ale pro následující příkazy pro všechna rozšíření virtuálního počítače.

Můžete použít **seznamu rozšíření virtuálních počítačů azure** příkaz získat informace o dostupných rozšíření a používat **–-json** možnosti se zobrazí všechny dostupné informace o jeden nebo více rozšíření. Pokud použijete název rozšíření, příkaz vrátí JSON popis všech dostupných rozšíření.

Například následující příklad kódu ukazuje, jak uvádí informace k **IaaSDiagnostics** rozšíření pomocí rozhraní příkazového řádku Azure **seznamu rozšíření virtuálních počítačů azure** příkaz a používá **–-json**  možnost vrátit úplné informace.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>Rozhraní REST API pro správu služeb
Následující rozhraní REST API můžete použít k získání informací o dostupných rozšíření:

* Pro instance webové role nebo rolí pracovního procesu, můžete použít [seznam dostupných rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci. Seznam verzí rozšíření k dispozici, můžete použít [verze rozšíření seznamu](https://msdn.microsoft.com/library/dn495437.aspx).
* Pro instance virtuálních počítačů, můžete použít [rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495441.aspx) operaci. Seznam verzí rozšíření k dispozici, můžete použít [verze rozšíření prostředků seznamu](https://msdn.microsoft.com/library/dn495440.aspx).

## <a name="add-update-or-disable-extensions"></a>Přidání, aktualizace nebo zakázat rozšíření
Rozšíření můžete přidat, pokud je vytvořena instance nebo mohou být přidány do spuštěné instance. Rozšíření můžete aktualizovat, zakázat nebo odebrat. Pomocí rutin prostředí Azure PowerShell nebo pomocí rozhraní API REST služby Management operace můžete provádět tyto akce. Parametry jsou potřebné k instalaci a nastavení některá rozšíření. Veřejné a privátní parametry jsou podporovány pro rozšíření.

### <a name="azure-powershell"></a>Azure PowerShell
Pomocí rutin prostředí Azure PowerShell je nejjednodušší způsob, jak přidat a aktualizovat rozšíření. Při použití rutiny rozšíření velká část konfigurace rozšíření se provádí za vás. Potřebujete v některých případech prostřednictvím kódu programu přidat rozšíření. Pokud budete potřebovat k tomu, je nutné zadat konfiguraci rozšíření.

Potřebujete vědět, zda rozšíření vyžaduje konfiguraci veřejné a privátní parametrů můžete použít následující rutiny:

* Pro instance webové role nebo rolí pracovního procesu, můžete použít **Get-AzureServiceAvailableExtension** rutiny.
* Pro instance virtuálních počítačů, můžete použít **Get-AzureVMAvailableExtension** rutiny.

### <a name="service-management-rest-apis"></a>Rozhraní REST API pro správu služeb
Když je načíst seznam dostupných rozšíření pomocí rozhraní REST API, zobrazí se informace o tom, jak rozšíření nakonfigurovat. Informace, které se vrátí může zobrazit informace o parametrech reprezentována na schéma veřejné a privátní schématu. Veřejné parametr hodnoty jsou vráceny v dotazech o instance. Hodnoty parametru privátní nebudou zobrazeny.

Potřebujete vědět, zda rozšíření vyžaduje konfiguraci veřejné a privátní parametrů můžete použít následující rozhraní REST API:

* Pro instance webové role nebo rolí pracovního procesu **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahovat informace v odpovědi z [seznamu dostupných Rozšíření](https://msdn.microsoft.com/library/dn169559.aspx) operaci.
* Pro instance virtuálních počítačů, **PublicConfigurationSchema** a **PrivateConfigurationSchema** elementy obsahovat informace v odpověď [seznamu prostředků Rozšíření](https://msdn.microsoft.com/library/dn495441.aspx) operaci.

> [!NOTE]
> Rozšíření můžete také použít konfigurace, které jsou definovány s JSON. Pokud tyto typy rozšíření se používají, jenom **SampleConfig** element se používá.
> 
> 

