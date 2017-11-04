## <a name="migrate-iaas-resources-from-the-classic-deployment-model-to-azure-resource-manager"></a>Migrovat prostředky infrastruktury z modelu nasazení classic do Azure Resource Manageru
Nejprve je důležité si uvědomit rozdíl mezi dat a správu rovinu operací na infrastrukturu jako službu (IaaS) prostředky.

* *Rovině řízenísprávy/* popisuje volání, které jsou do roviny správy a ovládání nebo rozhraní API pro úpravy prostředků. Například operace jako vytvoření virtuálního počítače, restartování virtuálního počítače a aktualizace virtuální sítě s použitím nové podsítě spravují spuštěné prostředky. Neovlivňují přímo připojení k virtuálním počítačům.
* *Data roviny* Popisuje modul runtime vlastní aplikace (aplikace) a zahrnuje interakci s instancí, které nechcete přesunout prostřednictvím rozhraní API služby Azure. Například přístup k webu nebo stahování dat z spuštěné instance systému SQL Server nebo server s MongoDB, jsou data interakce ploché nebo aplikace. Další příklady zahrnují kopírovat objekt blob z účtu úložiště a přístup k veřejné IP adresy používat protokol RDP (Remote Desktop) nebo Secure Shell (SSH) do virtuálního počítače. Tyto operace udržují spuštěnou aplikaci napříč výpočetními prostředky, síťovými službami a úložišti.

Roviny dat je stejný mezi modelem nasazení classic a Resource Manager zásobníky. Rozdílem je, že během procesu migrace překládá Microsoft reprezentace prostředky z modelu nasazení classic do, v zásobníku Resource Manager. V důsledku toho musíte použít nové nástroje, rozhraní API a sady SDK ke správě prostředků v zásobníku Resource Manager.

![Diagram, který ukazuje rozdíl mezi správy a ovládání a rovinu dat](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> Platforma Azure v některých scénářích migrace zastaví, uvolní a restartuje vaše virtuální počítače. To způsobí, že s krátkou prodlevou mezi roviny data.
>

## <a name="the-migration-experience"></a>Možnosti migrace
Před zahájením migrace:

* Ujistěte se, že prostředky, které chcete migrovat, nepoužívají žádné nepodporované funkce ani konfigurace. Platforma obvykle tyto problémy rozpozná a vygeneruje chybu.
* Pokud máte virtuální počítače, které nejsou ve virtuální síti, jsou zastavena a v rámci operace Příprava navrácena. Pokud nechcete ztratit veřejnou IP adresu, vezměte v úvahu před spuštěním operace přípravy rezervace IP adresu. Pokud jsou virtuální počítače ve virtuální síti, že nejsou zastavena a navrácena.
* Naplánujte migraci mimo pracovní dobu, abyste mohli vyřešit případné neočekávané chyby, ke kterým by mohlo při migraci dojít.
* Pro usnadnění ověření po dokončení kroku přípravy si stáhněte aktuální konfiguraci vašich virtuálních počítačů pomocí PowerShellu, příkazů rozhraní příkazového řádku nebo rozhraní REST API.
* Aktualizujte automatizace a operationalization skripty pro zpracování modelu nasazení Resource Manager, než začnete s migrací. Volitelně můžete provádět operace načtení, když jsou prostředky v připraveném stavu.
* Vyhodnocení zásad řízení přístupu na základě Role (RBAC), které jsou nakonfigurované s prostředky IaaS v modelu nasazení classic a plánování po dokončení migrace.

Pracovní postup migrace je následující:

![Diagram znázorňující pracovní postup migrace](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Operace popsané v následujících částech jsou všechny idempotent. Pokud došlo k potížím. než nepodporované funkce nebo chyby v konfiguraci, Příprava, opakujte zrušení nebo potvrzení operace. Azure se pokusí akci znovu.
>
>

### <a name="validate"></a>Ověření
Operace ověření je prvním krokem v procesu migrace. Cílem tohoto kroku je k analýze stavu prostředky, které chcete migrovat v modelu nasazení classic. Operaci vyhodnotí, zda jsou prostředky umožňující migrace (úspěch nebo neúspěch).

Vyberte virtuální síť nebo cloudové služby (Pokud není ve virtuální síti), kterou chcete ověřit migraci. Pokud prostředek není schopné migrace, uvádí Azure důvodů, proč proč.

#### <a name="checks-not-done-in-the-validate-operation"></a>Kontroluje, není provedené v operaci ověřit

Operace ověření pouze analyzuje stav prostředky v modelu nasazení classic. Můžete vyhledat všechny chyby a nepodporované scénáře z důvodu různých konfiguracích v modelu nasazení classic. Není možné zkontrolovat všechny problémy, které zásobník Azure Resource Manager může uložit prostředky během migrace. Tyto problémy se kontroluje jenom při prostředky podstoupit transformaci v dalším kroku migrace (Příprava operace). Následující tabulka uvádí všechny problémy nevráceno operaci ověřit:


|Kontroly sítě není v operaci ověřit|
|-|
|Virtuální síť s ER i VPN Gateway.|
|Připojení brány virtuální sítě v odpojeném stavu.|
|Všechny okruhy ER jsou předem migrovat na Azure Resource Manager zásobníku.|
|Azure Resource Manager kvóty zkontroluje síťové prostředky. Příklad: statickou veřejnou IP adresu, dynamické veřejné IP adresy, zatížení vyrovnávání, skupiny zabezpečení sítě, směrovací tabulky a síťových rozhraní. |
| Všechna pravidla pro vyrovnávání zatížení jsou platné v nasazení a virtuální sítě. |
| Konfliktní privátních IP adres mezi navrácena zastavit virtuální počítače ve stejné virtuální síti. |

### <a name="prepare"></a>Příprava
Operace přípravy je druhým krokem v procesu migrace. Cílem tohoto kroku je k simulaci transformace prostředky IaaS z modelu nasazení classic do Resource Manager prostředky. Příprava operaci Další, uvede tento-souběžného pro vás k vizualizaci.

> [!NOTE] 
> V tomto kroku se nemění vaše prostředky v modelu nasazení classic. Je bezpečné krok spustit, pokud se pokoušíte se migrace. 

Vyberte virtuální síť nebo cloudové služby (Pokud není virtuální sítě), které chcete připravit pro migraci.

* Pokud prostředek není schopné migrace, Azure zastaví proces migrace a uvádí důvod, proč Příprava operace se nezdařila.
* Pokud prostředek schopný migrace, Azure zamyká roviny management operace pro prostředky v rámci migrace. Například nemůžete do virtuálního počítače v rámci migrace přidat datový disk.

Azure pak spustí migrace metadata z modelu nasazení classic do Resource Manager migrací prostředků.

Po dokončení operace Příprava máte možnost vizualizace prostředky v modelu nasazení classic i Resource Manager. Pro každou cloudovou službu v modelu nasazení Classic vytvoří platforma Azure název skupiny prostředků ve tvaru `cloud-service-name>-Migrated`.

> [!NOTE]
> Není možné vybrat název skupiny prostředků vytvořené pro migrované prostředky (tedy "-migrovat"). Po dokončení migrace, ale můžete přesunout funkce služby Správce prostředků Azure k přesunu prostředků do žádné skupiny prostředků, které chcete. Další informace najdete v tématu, které se zabývá [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../articles/resource-group-move-resources.md).

Následující dva snímky obrazovky ukazují výsledek po úspěšné připravte operaci. První z nich zobrazuje skupinu prostředků, která obsahuje původní cloudové služby. Druhý zobrazí nové "-migrovat" skupinu prostředků, která obsahuje ekvivalentní prostředky Azure Resource Manager.

![Snímek obrazovky zobrazující původní cloudové služby](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Snímek obrazovky zobrazující prostředky Azure Resource Manager v operaci Příprava](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Zde je servisní pohled na vaše prostředky po dokončení fázi přípravy. Všimněte si, že prostředků v rovině dat je stejný. Je zobrazena v rovině správy (modelu nasazení classic) i v rovině řízení (Resource Manager).

![Diagram fázi přípravy](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Virtuální počítače, které nejsou ve virtuální síti v modelu nasazení classic jsou zastavena a navrácena v této fázi migrace.
>

### <a name="check-manual-or-scripted"></a>Kontrola (ruční nebo pomocí skriptu)
V kroku zkontrolujte máte možnost použít konfiguraci, kterou jste předtím stáhli k ověření, že migrace vypadá v pořádku. Alternativně můžete přihlásit na portál a kontrola vlastnosti a prostředky pro ověření spokojeni metadata migrace.

Pokud migrujete virtuální síť, většina konfigurací virtuálních počítačů se nerestartuje. U aplikací na těchto virtuálních počítačů můžete ověřit, že aplikace je stále spuštěna.

Můžete otestovat skripty monitorování a provozní zda virtuálních počítačích fungují podle očekávání, a pokud aktualizované skripty fungovat správně. Když jsou prostředky v připraveném stavu, podporují se pouze operace načtení.

Není okno sady času, před kterou je potřeba provést migrace. V tomto stavu máte času, kolik chcete. Pro tyto prostředky je však uzamčena vrstva správy, dokud je nepřerušíte nebo nepotvrdíte.

Pokud nastanou nějaké problémy, vždycky můžete migraci přerušit a vrátit se k modelu nasazení Classic. Po přejděte zpět Azure otevře operations management rovině s prostředky, ve kterém můžete obnovit normální činnost na těchto virtuálních počítačích v modelu nasazení classic.

### <a name="abort"></a>Přerušení
Toto je volitelný krok, pokud chcete vrátit zpět všechny změny modelu nasazení classic a zastavit migrace. Tato operace odstraní metadata Resource Manager (vytvořili v kroku Příprava) pro vaše prostředky. 

![Diagram kroku přerušení](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Tuto operaci nelze provést po mít spustí operace potvrzení.     
>

### <a name="commit"></a>Potvrzení
Po dokončení ověření můžete migraci potvrdit. Prostředky se už nezobrazují v modelu nasazení classic a jsou k dispozici pouze v modelu nasazení Resource Manager. Migrované prostředky je možné spravovat pouze na novém portálu.

> [!NOTE]
> Toto je idempotentní operace. Pokud se nezdaří, opakujte operaci. Pokud nadále nezdaří, vytvořit lístek podpory nebo vytvořit příspěvek ve fóru s "ClassicIaaSMigration" značky našem [fórum virtuálních počítačů](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![Diagram kroku potvrzení](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="migration-flowchart"></a>Vývojový diagram migrace

Tady je Vývojový diagram, který ukazuje, jak pokračovat v migraci:

![Snímek obrazovky, který ukazuje kroky migrace](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-the-classic-deployment-model-to-resource-manager-resources"></a>Překlad modelu nasazení classic do Resource Manager prostředky
Model nasazení classic a Resource Manager reprezentace prostředků najdete v následující tabulce. Jiné funkce a prostředky se aktuálně nepodporují.

| Reprezentace v modelu Classic | Reprezentace v modelu Resource Manager | Poznámky |
| --- | --- | --- |
| Název cloudové služby |Název DNS |Během migrace se pro každou cloudovou službu vytvoří nová skupina prostředků s formátem pojmenování `<cloudservicename>-migrated`. Tato skupina prostředků obsahuje všechny vaše prostředky. Název cloudové služby se stane názvem DNS přidruženým k veřejné IP adrese. |
| Virtuální počítač |Virtuální počítač |Vlastnosti specifické pro virtuální počítače se migrují beze změny. Určité osProfile informace, jako je název počítače, není uložen v modelu nasazení classic a zůstane prázdný po migraci. |
| Prostředky disku připojené k virtuálnímu počítači |Implicitní disky připojené k virtuálnímu počítači |Disky se v modelu nasazení Resource Manager nemodelují jako prostředky nejvyšší úrovně. Migrují se jako implicitní disky v rámci virtuálního počítače. Aktuálně se podporují pouze disky připojené k virtuálnímu počítači. Virtuální počítače správce prostředků teď můžete použít účty úložiště v modelu nasazení classic, což umožňuje snadno migrovat bez jakékoli aktualizace disky. |
| Rozšíření virtuálních počítačů |Rozšíření virtuálních počítačů |Z modelu nasazení Classic se migrují všechna rozšíření prostředků s výjimkou rozšíření XML. |
| Certifikáty virtuálních počítačů |Certifikáty v Azure Key Vault |Pokud Cloudová služba obsahuje certifikáty služby, migrace vytvoří nový trezor Azure klíčů za cloudové služby a přesouvá certifikáty do trezoru klíčů. Virtuální počítače se aktualizují, aby odkazovaly na certifikáty z trezoru klíčů. <br><br> Neodstraňujte trezoru klíčů. To může způsobit virtuálního počítače do stavu selhání. |
| Konfigurace WinRM |Konfigurace WinRM v rámci osProfile |Konfigurace vzdálené správy systému Windows se v rámci migrace přesune beze změny. |
| Vlastnost sady dostupnosti |Prostředek sady dostupnosti | Specifikace skupina dostupnosti je vlastnost na virtuálním počítači v modelu nasazení classic. Skupiny dostupnosti se v rámci migrace stanou prostředkem nejvyšší úrovně. Následující konfigurace se nepodporují: více skupin dostupnosti na cloudovou službu a jedna nebo více skupin dostupnosti společně s virtuálními počítači, které nejsou v žádné skupině dostupnosti v cloudové službě. |
| Konfigurace sítě na virtuálním počítači |Primární síťové rozhraní |Konfiguraci sítě na virtuálním počítači po migraci představuje prostředek primárního síťového rozhraní. U virtuálních počítačů, které nejsou ve virtuální síti, se při migraci změní interní IP adresa. |
| Několik síťových rozhraní na virtuálním počítači |Síťová rozhraní |Pokud virtuální počítač má přidruženo více síťových rozhraní, každé síťové rozhraní se změní na prostředek nejvyšší úrovně v rámci migrace, spolu se všechny vlastnosti. |
| Sada koncových bodů s vyrovnáváním zatížení |Nástroj pro vyrovnávání zatížení |V modelu nasazení Classic platforma ke každé cloudové službě přiřadila implicitní nástroj pro vyrovnávání zatížení. Během migrace se vytvoří nový prostředek nástroje pro vyrovnávání zatížení a ze sady koncových bodů s vyrovnáváním zatížení se stanou pravidla nástroje pro vyrovnávání zatížení. |
| Příchozí pravidla NAT |Příchozí pravidla NAT |Vstupní koncové body definované na virtuálním počítači se během migrace převedou na pravidla příchozího překladu adres v rámi nástroje pro vyrovnávání zatížení. |
| Virtuální IP adresa |Veřejná IP adresa s názvem DNS |Virtuální IP adresy se změní na veřejnou IP adresu a souvisí s nástrojem pro vyrovnávání zatížení. Virtuální IP adresu je možné migrovat pouze v případě, že k ní je přidružený vstupní koncový bod. |
| Virtuální síť |Virtuální síť |Virtuální síť se migruje se všemi vlastnostmi na model nasazení Resource Manager. Vytvoří se nová skupina prostředků s názvem `-migrated`. |
| Vyhrazené IP adresy |Veřejná IP adresa s metodou statického přidělování |Vyhrazené IP adresy přidružené k nástroji pro vyrovnávání zatížení se migrují společně s migrací cloudové služby nebo virtuálního počítače. Migrace nepřidružených vyhrazených IP adres se aktuálně nepodporuje. |
| Veřejná IP adresa na virtuální počítač |Veřejná IP adresa s metodou dynamického přidělování |Veřejná IP adresa přidružená k virtuálnímu počítači se převede na prostředek veřejné IP adresy s nastavenou statickou metodou přidělování. |
| Skupiny NSG |Skupiny NSG |Skupiny zabezpečení sítě (NSG) přidružené k podsíti se v rámci migrace klonují do modelu nasazení Resource Manager. Skupina NSG v modelu nasazení Classic se během migrace neodebere. Operace se skupinou NSG v rovině správy jsou však v průběhu migrace blokované. |
| Servery DNS |Servery DNS |Servery DNS přidružené k virtuální síti nebo virtuálnímu počítači se migrují v rámci migrace odpovídajícího prostředku, a to společně se všemi vlastnostmi. |
| UDR |UDR |Trasy definované uživatelem (UDR) přidružené k podsíti se v rámci migrace klonují do modelu nasazení Resource Manager. UDR v modelu nasazení Classic se během migrace neodebere. V průběhu migrace jsou blokované operace s UDR v rovině správy. |
| Vlastnost předávání IP v konfiguraci sítě na virtuálním počítači |Vlastnost předávání IP na síťové kartě |Vlastnost předávání IP na virtuálním počítači se během migrace převede na vlastnost na síťových rozhraních. |
| Nástroj pro vyrovnávání zatížení s několika IP adresami |Nástroj pro vyrovnávání zatížení s několika prostředky veřejné IP adresy |Všechny veřejné IP adresy přidružené k vyrovnávání zatížení je převést na prostředek veřejné IP a související s nástrojem pro vyrovnávání zatížení po migraci. |
| Interní názvy DNS na virtuálním počítači |Interní názvy DNS na síťové kartě |Během migrace se interní přípony DNS pro virtuální počítače migrují do vlastnosti jen pro čtení s názvem InternalDomainNameSuffix na síťové kartě. Přípona zůstává beze změny po migraci, a virtuálních počítačů řešení by měly být nadále fungovat jako dříve. |
| Brána virtuální sítě |Brána virtuální sítě |Vlastnosti brány virtuální sítě se migrují beze změny. Virtuální IP adresa přidružená k bráně se také nemění. |
| Místní síťová lokalita |Brána místní sítě |Vlastnosti serveru místní sítě se migrují beze změny na nový prostředek s názvem bránu místní sítě. Reprezentuje předpony adres místní a Brána vzdálené IP adresy. |
| Odkazy na připojení |Připojení |Odkazy na připojení mezi bránou rozhraní a na místní síťový web v konfiguraci sítě je reprezentována nový prostředek s názvem připojení. Všechny vlastnosti připojení odkaz v síti konfigurační soubory se zkopírují na prostředek připojení beze změny. Připojení mezi virtuálním sítím v modelu nasazení classic je dosaženo vytvořením dvou tunelových propojení IPsec na místní sítě reprezentující virtuální sítě. Transformuje je typ sítě na virtuální-připojení virtuální sítě v modelu Resource Manager, bez nutnosti brány místní sítě. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Změny automatizace a nástrojů po migraci
Jako součást migrace vašich prostředků z modelu nasazení classic do modelu nasazení Resource Manager musíte aktualizovat existující automatizace nebo nástrojů pro zajištění, že nadále fungovat po migraci.
