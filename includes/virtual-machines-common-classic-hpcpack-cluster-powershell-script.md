



V závislosti na prostředí a možnosti můžete vytvořit skript všechny clusteru infrastruktury, včetně virtuální síť Azure, účty úložiště, cloudové služby, řadič domény, vzdálení nebo místní databáze SQL, hlavního uzlu a další uzly clusteru. Skript můžete alternativně použít existující infrastrukturu Azure a vytvořit pouze uzly clusteru prostředí HPC.

Základní informace o plánování clusteru služby HPC Pack, najdete v článku [období pro vyzkoušení produktu a plánování](https://technet.microsoft.com/library/jj899596.aspx) a [Začínáme](https://technet.microsoft.com/library/jj899590.aspx) obsahu v knihovně TechNet HPC Pack 2012 R2.

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**: předplatné můžete použít ve službě Azure globální nebo Azure China. Vaše předplatné omezení ovlivnit počet a typ uzly clusteru, který můžete nasadit. Informace najdete v tématu [předplatného Azure a omezení služby, kvóty a omezení](../articles/azure-subscription-service-limits.md).
* **Klientský počítač s Windows v prostředí Azure PowerShell 0.8.10 nebo později nainstalovaný a nakonfigurovaný**: najdete v části [Začínáme s Azure PowerShell](/powershell/azureps-cmdlets-docs) pokyny k instalaci a kroky pro připojení k předplatnému Azure.
* **Skript nasazení HPC Pack IaaS**: Stáhněte a rozbalte nejnovější verzi skript z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Zkontrolujte verzi skript spuštěním `New-HPCIaaSCluster.ps1 –Version`. Tento článek vychází z verze 4.5.2 skriptu.
* **Konfigurační soubor skriptu**: Vytvořte soubor XML, který používá skript ke konfiguraci clusteru HPC. Informace a příklady najdete v částech dále v tomto článku a soubor Manual.rtf, který doprovází skript nasazení.

## <a name="syntax"></a>Syntaxe
```PowerShell
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
> [!NOTE]
> Spusťte skript jako správce.
> 
> 

### <a name="parameters"></a>Parametry
* **ConfigFile**: Určuje cestu k souboru konfiguračního souboru k popisu clusteru prostředí HPC. Další informace o konfigurační soubor v tomto tématu najdete v článku nebo v souboru Manual.rtf ve složce obsahující skript.
* **AdminUserName**: Určuje uživatelské jméno. Pokud domény doménové struktury se vytvoří skript, to se změní na uživatelské jméno místního správce pro všechny virtuální počítače a jméno správce domény. Pokud doménová struktura domény už existuje, určuje uživatele domény jako uživatelské jméno místního správce k instalaci sady HPC Pack.
* **AdminPassword**: Určuje heslo správce. Pokud není zadaný v příkazovém řádku, skript zobrazí výzvu k zadejte heslo.
* **HPCImageName** (volitelné): Určuje název bitové kopie prostředí HPC Pack virtuálního počítače použít k nasazení clusteru HPC. Musí to být image poskytovaný společností Microsoft HPC Pack z Azure Marketplace. Není-li zadána (doporučeno obvykle), skript vybere nejnovější publikované [image HPC Pack 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/). Nejnovější bitové kopie je založená na Windows Server 2012 R2 Datacenter s HPC Pack 2012 R2 Update 3 nainstalován.
  
  > [!NOTE]
  > Nasazení se nezdaří, pokud neurčíte platnou bitovou kopií HPC Pack.
  > 
  > 
* **Soubor protokolu** (volitelné): Určuje cestu k souboru protokolu nasazení. Pokud není zadaný, skript vytvoří soubor protokolu v adresáři temp počítače se systémem skript.
* **Platnost** (volitelné): potlačí všechny výzvy potvrzení.
* **NoCleanOnFailure** (volitelné): Určuje, že nejsou odstranit virtuální počítače Azure, které nejsou úspěšně nasazena. Odebrat tyto virtuální počítače ručně před opětovným spuštěním skriptu pokračujte nasazení nebo nasazení může selhat.
* **PSSessionSkipCACheck** (volitelné): pro všechny cloudové služby s virtuálními počítači nasadit pomocí tohoto skriptu, je certifikát podepsaný svým držitelem automaticky generuje služba Azure, a všechny virtuální počítače v rámci cloudové služby použít tento certifikát jako výchozí vzdáleného systému Windows Certifikát správy (WinRM). Pokud chcete nasadit HPC funkcí v těchto virtuálních počítačích Azure, skript ve výchozím nastavení dočasně nainstaluje tyto certifikáty v místním počítači\\úložiště Důvěryhodné kořenové certifikační autority klientského počítače k potlačení chyby "není důvěryhodné certifikační Autority" zabezpečení. Při provádění skriptu. Certifikáty jsou odebrány, po dokončení skriptu. Pokud je tento parametr zadán, v klientském počítači nejsou nainstalovány certifikáty a potlačeno upozornění zabezpečení.
  
  > [!IMPORTANT]
  > Tento parametr se nedoporučuje pro nasazení v produkčním prostředí.
  > 
  > 

### <a name="example"></a>Příklad
Následující příklad vytvoří cluster služby HPC Pack pomocí konfiguračního souboru *MyConfigFile.xml*a určuje přihlašovací údaje správce pro instalaci clusteru.

```PowerShell
.\New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### <a name="additional-considerations"></a>Další aspekty
* Skript Volitelně můžete umožnit odesílání úlohy prostřednictvím webového portálu HPC Pack nebo HPC Pack REST API.
* Skript můžete případně spustit vlastní skripty před a po konfiguraci z hlavního uzlu Pokud chcete nainstalovat další software nebo konfigurovat další nastavení.

## <a name="configuration-file"></a>Konfigurační soubor
Konfigurační soubor nasazení skriptu je soubor XML. Soubor schématu HPCIaaSClusterConfig.xsd je ve složce HPC Pack IaaS nasazení skriptu. **IaaSClusterConfig** je kořenový prvek konfiguračního souboru, který obsahuje podřízené elementy podrobně popsány v souboru Manual.rtf ve složce nasazení skriptu.

