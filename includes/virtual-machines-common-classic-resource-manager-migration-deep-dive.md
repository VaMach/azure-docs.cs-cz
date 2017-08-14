## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Smysl migrace prostředků IaaS z modelu Classic na Azure Resource Manager
Než se pustíme do podrobností, podívejme se na rozdíl mezi operacemi s prostředky IaaS v rovině dat a rovině správy.

* *Rovina správy/řízení* popisuje volání přicházející do roviny správy/řízení nebo rozhraní API za účelem úpravy prostředků. Například operace jako vytvoření virtuálního počítače, restartování virtuálního počítače a aktualizace virtuální sítě s použitím nové podsítě spravují spuštěné prostředky. Nemají přímý vliv na připojení k instancím.
* *Rovina dat* (aplikace) popisuje samotný běh aplikace a zahrnuje interakce s instancemi, které neprochází přes rozhraní Azure API. Za interakci v rovině dat neboli interakci s aplikací může být považován přístup k webu nebo načítání dat ze spuštěné instance SQL Serveru nebo serveru MongoDB. Kopírování objektu blob z účtu úložiště a přístup k veřejné IP adrese pro připojení k virtuálnímu počítači přes protokol RDP nebo SSH je také interakce v rovině dat. Tyto operace udržují spuštěnou aplikaci napříč výpočetními prostředky, síťovými službami a úložišti.

![Snímek obrazovky, který ukazuje rozdíl mezi rovinou správy/řízení a rovinou dat](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)

> [!NOTE]
> Platforma Azure v některých scénářích migrace zastaví, uvolní a restartuje vaše virtuální počítače. To způsobuje krátké výpadky roviny dat.
>
>

## <a name="the-migration-experience"></a>Možnosti migrace
Před zahájením migrace se doporučuje následující:

* Ujistěte se, že prostředky, které chcete migrovat, nepoužívají žádné nepodporované funkce ani konfigurace. Platforma obvykle tyto problémy rozpozná a vygeneruje chybu.
* Pokud máte virtuální počítače, které nejsou ve virtuální síti, jako součást operace přípravy se zastaví a uvolní. Pokud nechcete přijít o veřejnou IP adresu, před aktivací operace přípravy se podívejte na rezervaci IP adresy. Pokud jsou však virtuální počítače ve virtuální síti, nezastaví se ani neuvolní.
* Naplánujte migraci mimo pracovní dobu, abyste mohli vyřešit případné neočekávané chyby, ke kterým by mohlo při migraci dojít.
* Pro usnadnění ověření po dokončení kroku přípravy si stáhněte aktuální konfiguraci vašich virtuálních počítačů pomocí PowerShellu, příkazů rozhraní příkazového řádku nebo rozhraní REST API.
* Před zahájením migrace aktualizujte své skripty automatizace/operacionalizace s ohledem na model nasazení Resource Manager. Volitelně můžete provádět operace načtení, když jsou prostředky v připraveném stavu.
* Vyhodnoťte zásady řízení přístupu na základě role nakonfigurované pro prostředky IaaS v modelu Classic a naplánujte, jaké budou po dokončení migrace.

Pracovní postup migrace je následující

![Snímek obrazovky, který ukazuje pracovní postup migrace](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Všechny operace popsané v následujících částech jsou idempotentní. Pokud máte jiný problém, než je nepodporovaná funkce nebo chyba konfigurace, doporučuje se opakovat operaci přípravy, přerušení nebo potvrzení. Platforma Azure se pokusí akci provést znovu.
>
>

### <a name="validate"></a>Ověření
Operace ověření je prvním krokem v procesu migrace. Cílem tohoto kroku je na pozadí analyzovat data pro prostředky v rámci migrace a vrátit úspěch nebo chybu podle toho, jestli jsou prostředky schopné migrace, nebo ne.

Vyberete virtuální síť nebo hostovanou službu (pokud to není virtuální síť), kterou chcete ověřit pro migraci.

* Pokud prostředek není schopen migrace, platforma Azure uvede všechny důvody, proč se pro migraci nepodporuje.

Při ověřování služeb úložiště najdete migrovaný účet ve skupině prostředků se stejným názvem jako váš účet úložiště a připojeným řetězcem „-Migrated“.  Například pokud má váš účet úložiště název mystorage, najdete prostředek s podporou Azure Resource Manageru ve skupině prostředků mystorage-Migrated, která bude obsahovat účet úložiště mystorage.

### <a name="prepare"></a>Příprava
Operace přípravy je druhým krokem v procesu migrace. Cílem tohoto kroku je simulovat transformaci prostředků IaaS z modelu Classic na prostředky Resource Manageru a zobrazit je vedle sebe, abyste si je mohli prohlédnout.

Vyberete virtuální síť nebo hostovanou službu (pokud to není virtuální síť), kterou chcete připravit na migraci.

* Pokud prostředek není schopen migrace, platforma Azure zastaví proces migrace a uvede důvody, proč operace přípravy selhala.
* Pokud prostředek je schopen migrace, platforma Azure nejprve pro prostředky v rámci migrace uzamkne operace v rovině správy. Například nemůžete do virtuálního počítače v rámci migrace přidat datový disk.

Platforma Azure potom pro migrované prostředky spustí migraci metadat z modelu Classic na Resource Manager.

Po dokončení operace přípravy máte možnost vizualizace prostředků v modelu Classic i Resource Manager. Pro každou cloudovou službu v modelu nasazení Classic vytvoří platforma Azure název skupiny prostředků ve tvaru `cloud-service-name>-Migrated`.

> [!NOTE]
> Není možné vybrat název skupiny prostředků vytvořené pro migrované prostředky (tj. „-Migrated“), ale po dokončení migrace můžete pomocí funkce Azure Resource Manageru pro přesun prostředky přesunout do libovolné skupiny prostředků. Další informace najdete v článku [Přesun prostředků do nové skupiny prostředků nebo předplatného](../articles/resource-group-move-resources.md).

Následující dvě obrazovky ukazují výsledek úspěšné operace přípravy. První obrazovka ukazuje skupinu prostředků, která obsahuje původní cloudovou službu. Druhá obrazovka ukazuje novou skupinu prostředků „-Migrated“, která obsahuje ekvivalentní prostředky Azure Resource Manageru.

![Snímek obrazovky, který ukazuje portál s cloudovou službou modelu Classic](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Snímek obrazovky, který ukazuje portál s prostředky Azure Resource Manageru ve fázi přípravy](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

> [!NOTE]
> Virtuální počítače, které nejsou v klasické virtuální síti, se v této fázi migrace zastaví a uvolní.
>
>

### <a name="check-manual-or-scripted"></a>Kontrola (ruční nebo pomocí skriptu)
V tomto kroku kontroly můžete pro kontrolu, že migrace vypadá dobře, volitelně použít konfiguraci, kterou jste si stáhli dříve. Alternativně se můžete přihlásit k portálu a na místě zkontrolovat vlastnosti a prostředky, abyste ověřili, že migrace metadat vypadá dobře.

Pokud migrujete virtuální síť, většina konfigurací virtuálních počítačů se nerestartuje. U aplikací na těchto virtuálních počítačích můžete ověřit, že jsou stále spuštěné a funkční.

Můžete otestovat své skripty monitorování/automatizace a provozní skripty, abyste zjistili, jestli virtuální počítače fungují podle očekávání a jestli se vaše skripty správně aktualizovaly. Když jsou prostředky v připraveném stavu, podporují se pouze operace načtení.

Není stanoven žádný časový interval, během kterého musíte provést migraci. V tomto stavu máte času, kolik chcete. Pro tyto prostředky je však uzamčena vrstva správy, dokud je nepřerušíte nebo nepotvrdíte.

Pokud nastanou nějaké problémy, vždycky můžete migraci přerušit a vrátit se k modelu nasazení Classic. Po vrácení platforma Azure otevře pro prostředky operace v rovině správy, abyste mohli obnovit normální provoz na těchto počítačích v modelu nasazení Classic.

### <a name="abort"></a>Přerušení
Přerušení je volitelný krok, pomocí kterého můžete vrátit změny na model nasazení Classic a zastavit migraci.

> [!NOTE]
> Tuto operaci není možné provést po aktivaci operace potvrzení.     
>
>

### <a name="commit"></a>Potvrzení
Po dokončení ověření můžete migraci potvrdit. Prostředky se už nebudou zobrazovat v modelu Classic a budou dostupné pouze v modelu nasazení Resource Manager. Migrované prostředky je možné spravovat pouze na novém portálu.

> [!NOTE]
> Toto je idempotentní operace. Pokud selže, doporučuje se operaci opakovat. Pokud bude selhávat i nadále, vytvořte lístek podpory nebo na našem [fóru k virtuálním počítačům](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows) vytvořte příspěvek se značkou ClassicIaaSMigration.
>
>
<br>
Tady je vývojový diagram kroků během procesu migrace.

![Snímek obrazovky, který ukazuje kroky migrace](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-to-azure-resource-manager-resources"></a>Převod klasických prostředků na prostředky Azure Resource Manageru
Reprezentaci prostředků v modelu Classic a Resource Manager najdete v následující tabulce. Jiné funkce a prostředky se aktuálně nepodporují.

| Reprezentace v modelu Classic | Reprezentace v modelu Resource Manager | Podrobné poznámky |
| --- | --- | --- |
| Název cloudové služby |Název DNS |Během migrace se pro každou cloudovou službu vytvoří nová skupina prostředků s formátem pojmenování `<cloudservicename>-migrated`. Tato skupina prostředků obsahuje všechny vaše prostředky. Název cloudové služby se stane názvem DNS přidruženým k veřejné IP adrese. |
| Virtuální počítač |Virtuální počítač |Vlastnosti specifické pro virtuální počítače se migrují beze změny. Určité informace osProfile, jako je například název počítače, se v modelu nasazení Classic neukládají a zůstávají prázdné i po migraci. |
| Prostředky disku připojené k virtuálnímu počítači |Implicitní disky připojené k virtuálnímu počítači |Disky se v modelu nasazení Resource Manager nemodelují jako prostředky nejvyšší úrovně. Migrují se jako implicitní disky v rámci virtuálního počítače. Aktuálně se podporují pouze disky připojené k virtuálnímu počítači. Virtuální počítače Resource Manageru teď můžou používat klasické účty úložiště, což umožňuje snadnou migraci disků bez potřeby jakýchkoli aktualizací. |
| Rozšíření virtuálních počítačů |Rozšíření virtuálních počítačů |Z modelu nasazení Classic se migrují všechna rozšíření prostředků s výjimkou rozšíření XML. |
| Certifikáty virtuálních počítačů |Certifikáty v Azure Key Vault |Pokud cloudová služba obsahuje certifikáty služby, vytvoří se pro každou cloudovou službu trezor klíčů Azure a certifikáty se do něj přesunou. Virtuální počítače se aktualizují, aby odkazovaly na certifikáty z trezoru klíčů. <br><br> **POZNÁMKA:** Trezor klíčů neodstraňujte, protože to může způsobit, že virtuální počítač přejde do stavu selhání. Pracujeme na vylepšení back-endu, aby bylo možné trezory klíčů bezpečně odstranit nebo přesunout společně s virtuálním počítačem do nového předplatného. |
| Konfigurace WinRM |Konfigurace WinRM v rámci osProfile |Konfigurace vzdálené správy systému Windows se v rámci migrace přesune beze změny. |
| Vlastnost sady dostupnosti |Prostředek sady dostupnosti | V modelu nasazení Classic byla specifikace sady dostupnosti vlastnost na virtuálním počítači. Skupiny dostupnosti se v rámci migrace stanou prostředkem nejvyšší úrovně. Následující konfigurace se nepodporují: více skupin dostupnosti na cloudovou službu a jedna nebo více skupin dostupnosti společně s virtuálními počítači, které nejsou v žádné skupině dostupnosti v cloudové službě. |
| Konfigurace sítě na virtuálním počítači |Primární síťové rozhraní |Konfiguraci sítě na virtuálním počítači po migraci představuje prostředek primárního síťového rozhraní. U virtuálních počítačů, které nejsou ve virtuální síti, se při migraci změní interní IP adresa. |
| Několik síťových rozhraní na virtuálním počítači |Síťová rozhraní |Pokud je k virtuálnímu počítači přidruženo několik síťových rozhraní, z každého síťového rozhraní se v rámci migrace stane prostředek nejvyšší úrovně v modelu nasazení Resource Manager, a to společně se všemi vlastnostmi. |
| Sada koncových bodů s vyrovnáváním zatížení |Nástroj pro vyrovnávání zatížení |V modelu nasazení Classic platforma ke každé cloudové službě přiřadila implicitní nástroj pro vyrovnávání zatížení. Během migrace se vytvoří nový prostředek nástroje pro vyrovnávání zatížení a ze sady koncových bodů s vyrovnáváním zatížení se stanou pravidla nástroje pro vyrovnávání zatížení. |
| Příchozí pravidla NAT |Příchozí pravidla NAT |Vstupní koncové body definované na virtuálním počítači se během migrace převedou na pravidla příchozího překladu adres v rámi nástroje pro vyrovnávání zatížení. |
| Virtuální IP adresa |Veřejná IP adresa s názvem DNS |Z virtuální IP adresy se stane veřejná IP adresa, a ta se přidruží k nástroji pro vyrovnávání zatížení. Virtuální IP adresu je možné migrovat pouze v případě, že k ní je přidružený vstupní koncový bod. |
| Virtuální síť |Virtuální síť |Virtuální síť se migruje se všemi vlastnostmi na model nasazení Resource Manager. Vytvoří se nová skupina prostředků s názvem `-migrated`. |
| Vyhrazené IP adresy |Veřejná IP adresa s metodou statického přidělování |Vyhrazené IP adresy přidružené k nástroji pro vyrovnávání zatížení se migrují společně s migrací cloudové služby nebo virtuálního počítače. Migrace nepřidružených vyhrazených IP adres se aktuálně nepodporuje. |
| Veřejná IP adresa na virtuální počítač |Veřejná IP adresa s metodou dynamického přidělování |Veřejná IP adresa přidružená k virtuálnímu počítači se převede na prostředek veřejné IP adresy s nastavenou statickou metodou přidělování. |
| Skupiny NSG |Skupiny NSG |Skupiny zabezpečení sítě (NSG) přidružené k podsíti se v rámci migrace klonují do modelu nasazení Resource Manager. Skupina NSG v modelu nasazení Classic se během migrace neodebere. Operace se skupinou NSG v rovině správy jsou však v průběhu migrace blokované. |
| Servery DNS |Servery DNS |Servery DNS přidružené k virtuální síti nebo virtuálnímu počítači se migrují v rámci migrace odpovídajícího prostředku, a to společně se všemi vlastnostmi. |
| UDR |UDR |Trasy definované uživatelem (UDR) přidružené k podsíti se v rámci migrace klonují do modelu nasazení Resource Manager. UDR v modelu nasazení Classic se během migrace neodebere. V průběhu migrace jsou blokované operace s UDR v rovině správy. |
| Vlastnost předávání IP v konfiguraci sítě na virtuálním počítači |Vlastnost předávání IP na síťové kartě |Vlastnost předávání IP na virtuálním počítači se během migrace převede na vlastnost na síťových rozhraních. |
| Nástroj pro vyrovnávání zatížení s několika IP adresami |Nástroj pro vyrovnávání zatížení s několika prostředky veřejné IP adresy |Každá veřejná IP adresa přidružená k nástroji pro vyrovnávání zatížení se převede na prostředek veřejné IP adresy a po migraci se přidruží k nástroji pro vyrovnávání zatížení. |
| Interní názvy DNS na virtuálním počítači |Interní názvy DNS na síťové kartě |Během migrace se interní přípony DNS pro virtuální počítače migrují do vlastnosti jen pro čtení s názvem InternalDomainNameSuffix na síťové kartě. Přípona po migraci zůstane beze změny a překlad názvů virtuálních by ml fungovat stejně jako předtím. |
| Brána virtuální sítě |Brána virtuální sítě |Vlastnosti brány virtuální sítě se migrují beze změny. Virtuální IP adresa přidružená k bráně se také nemění. |
| Místní síťová lokalita |Brána místní sítě |Vlastnosti místní síťové lokality se migrují beze změny do nového prostředku označovaného jako Brána místní sítě. Představuje předpony místních adres a IP adresu vzdálené brány. |
| Odkazy na připojení |Připojení |Odkazy na možnosti připojení mezi bránou a místní síťovou lokalitou v konfiguraci sítě po migraci představuje nově vytvořený prostředek označovaný jako Připojení v Resource Manageru. Všechny vlastnosti odkazu na možnosti připojení v konfiguračních souborech sítě se beze změny zkopírují do nově vytvořeného prostředku Připojení. Možností připojení typu VNet-to-VNet v modelu Classic se dosahuje vytvořením dvou tunelů IPsec k místním síťovým lokalitám, které představují virtuální sítě. To se v modelu Resource Manager transformuje na typ připojení VNet2VNet, aniž by se vyžadovaly místní síťové brány. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Změny automatizace a nástrojů po migraci
V rámci migrace prostředků z modelu nasazení Classic do modelu nasazení Resource Manager je potřeba aktualizovat stávající automatizaci nebo nástroje, aby se zajistilo, že po migraci budou i nadále fungovat.
