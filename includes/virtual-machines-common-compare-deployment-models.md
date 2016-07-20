


## Výhody integrace výpočtů, sítě a úložiště v rámci modelu nasazení Azure Resource Manager

Model nasazení Azure Resource Manager nabízí možnost snadného využívání předem připravených šablon aplikací nebo vytvoření šablony aplikace pro nasazení a správu výpočetních a síťových prostředků a prostředků úložiště v Azure. V této části se budeme zabývat výhodami nasazení prostředků prostřednictvím modelu nasazení Azure Resource Manager.

-   Zjednodušená složitost – vytváření, integrace a spolupráce na složitých aplikacích, které můžou zahrnovat celou škálu prostředků Azure (například weby, databáze SQL, virtuální počítače nebo virtuální sítě) ze souboru šablony, který lze sdílet
-   Flexibilita pro opakovatelná nasazení pro vývoj, DevOps a správce systému, když používáte stejný soubor šablony
-   Těsná integrace rozšíření virtuálních počítačů (vlastní skripty, DSC, Chef, Puppet atd.) se správcem Azure Resource Manager v souboru šablony umožňuje snadnou orchestraci konfigurace při instalaci v rámci virtuálního počítače
-   Definování značek a šíření fakturace těchto značek pro výpočetní a síťové prostředky a prostředky úložiště
-   Jednoduchá a přesná správa přístupu k prostředkům organizace pomocí řízení přístupu na základě role v Azure (RBAC)
-   Zjednodušený scénář upgradu nebo aktualizace pomocí úpravy původní šablony a jejího opakovaného nasazení


## Rozvoj výpočetních a síťových rozhraní API a rozhraní API úložiště v rámci správce Azure Resource Manager

Kromě výše uvedených výhod se významně zvýšil výkon vydaných rozhraní API:

-   Možnost masivního a paralelního nasazení služeb Virtual Machines
-   Podpora pro 3 domény selhání ve skupinách dostupnosti
-   Vylepšené rozšíření pro vlastní skripty, které umožňuje specifikaci skriptů z jakékoli veřejně přístupné vlastní adresy URL
- Integrace služby Virtual Machines se službou Azure Key Vault pro vysoce zabezpečená úložiště a privátní nasazení tajných údajů z [hardwarových zabezpečovacích modulů](http://wikipedia.org/wiki/Hardware_security_module) [ověřených algoritmem FIPS](http://wikipedia.org/wiki/FIPS_140-2)
-   Poskytuje základní stavební bloky sítě prostřednictvím rozhraní API, aby zákazníkům umožnil vytvoření složitých aplikací, které obsahují síťová rozhraní, nástroje pro vyrovnávání zatížení a virtuální sítě
-   Síťová rozhraní jako nový objekt umožňují, aby složitější konfigurace sítí byly trvalejší a služba Virtual Machines je využívala opakovaně
-   Nástroje pro vyrovnávání zatížení jako prostředek první třídy, který umožňuje přiřazení IP adres
-   Podrobná rozhraní API služeb Virtual Network umožňují zjednodušit správu jednotlivých služeb Virtual Network

## Koncepční rozdíly přicházející se zavedením nových rozhraní API

V této části probereme několik nejdůležitějších koncepčních rozdílů mezi rozhraními API založenými na XML, která jsou dnes dostupná, a rozhraními API založenými na JSON, která jsou dostupná prostřednictvím správce Azure Resource Manager pro výpočty, sítě a úložiště.

 Položka | Azure Service Management (na základě XML)    | Poskytovatelé výpočtů, sítí a úložišť (na základě JSON)
 ---|---|---
| Cloudová služba pro službu Virtual Machines |  Cloudová služba byla kontejnerem pro uložení virtuálních počítačů, která vyžadovala dostupnost z platformy a vyrovnávání zatížení. | Cloudová služba už není objektem vyžadovaným pro vytvoření virtuálního počítače pomocí nového modelu. |
| Skupiny dostupnosti | Dostupnost pro platformu byla označovaná konfigurací stejného parametru „AvailabilitySetName“ ve službě Virtual Machines. Maximální počet domén selhání byl 2. | Skupina dostupnosti je prostředek vystavený poskytovatelem Microsoft.Compute. Služby Virtual Machines, které vyžadují vysokou dostupnost, musejí být součástí skupiny dostupnosti. Maximální počet domén selhání je teď 3. |
| Skupiny vztahů | Skupiny vztahů byly nezbytné k vytváření služeb Virtual Network. Se zavedením regionálních služeb Virtual Network přestaly být nutné. |Abychom to zjednodušili, koncept skupin vztahů neexistuje v rozhraních API, které se vystavují prostřednictvím správce Azure Resource Manager. |
| Vyrovnávání zatížení    | Vytvoření cloudové služby nabízí implicitní nástroj pro vyrovnávání zatížení nasazených služeb Virtual Machines. | Nástroj pro vyrovnávání zatížení je prostředek vystavený poskytovatelem Microsoft.Network. Primární síťové rozhraní služeb Virtual Machines, které potřebuje vyrovnávání zatížení, musí odkazovat na nástroj pro vyrovnávání zatížení. Nástroje pro vyrovnávání zatížení můžou být interní nebo externí. [Další informace.](../articles/resource-groups-networking.md) |
|Virtuální IP adresa | Služby Cloud Services získají výchozí VIP (virtuální IP adresu) po přidání virtuálního počítače do cloudové služby. Virtuální IP adresa je adresa přidružená k implicitnímu nástroji pro vyrovnávání zatížení.   | Veřejná IP adresa je prostředek vystavený poskytovatelem Microsoft.Network. Veřejná IP adresa může být statická (vyhrazená) nebo dynamická. Dynamické veřejné IP adresy můžete přiřadit k nástroji pro vyrovnávání zatížení. Veřejné IP adresy můžete zabezpečit pomocí skupin zabezpečení. |
|Vyhrazená IP adresa|   IP adresu můžete v Azure vyhradit a přidružit ji ke cloudové službě, abyste zajistili, že IP adresa zůstane dynamická.   | Veřejnou IP adresu můžete vytvořit v režimu „Statická“ a bude nabízet stejné funkce jako „vyhrazená IP adresa“. Statické veřejné IP adresy můžete k nástroji pro vyrovnávání zatížení přiřadit jenom teď. |
|Veřejná IP adresa (PIP) na virtuální počítač | Veřejné IP adresy můžete k virtuálnímu počítači přiřadit i přímo. | Veřejná IP adresa je prostředek vystavený poskytovatelem Microsoft.Network. Veřejná IP adresa může být statická (vyhrazená) nebo dynamická. Abyste právě teď získali veřejnou IP adresu na virtuální počítač, můžete k síťovému rozhraní přiřadit jenom dynamické veřejné IP adresy. |
|Koncové body| Vstupní koncové body je třeba konfigurovat na virtuálním počítači, aby se pro určité porty staly otevřeným připojením. Jeden z běžných režimů připojení k virtuálním počítačům se provádí nastavením vstupní koncových bodů. | Příchozí pravidla NAT můžete konfigurovat na nástrojích pro vyrovnávání zatížení, abyste dosáhli stejné možnosti povolování koncových bodů na konkrétních portech za účelem připojení k virtuálním počítačům. |
|Název DNS| Cloudová služba by získala implicitní, globálně jedinečný název DNS. Například: `mycoffeeshop.cloudapp.net`. | Názvy DNS jsou volitelné parametry, které můžete nastavit na prostředku veřejné IP adresy. Plně kvalifikovaný název bude v následujícím formátu – `<domainlabel>.<region>.cloudapp.azure.com`. |
|Síťová rozhraní | Primární a sekundární síťové rozhraní a jeho vlastnosti byly definované jako síťová konfigurace virtuálního počítače. | Síťové rozhraní je prostředek vystavený poskytovatelem Microsoft.Network. Životní cyklus síťového rozhraní není vázaný na virtuální počítač. |

## Začínáme s šablonami Azure pro službu Virtual Machines

Práci se šablonami Azure můžete zahájit využitím různých nástrojů, které máme pro vývoj a nasazení na platformu.

### Portál Azure

Portál Azure bude stále nabízet možnost pro nasazení služby Virtual Machines s klasickým modelem nasazení a služby Virtual Machines s modelem nasazení Resource Manageru současně. Portál Azure také umožní i nasazení vlastní šablony.

### Azure Powershell

Azure PowerShell bude mít dva režimy nasazení – režim **AzureServiceManagement** a režim **AzureResourceManager**.  Režim AzureResourceManager bude teď obsahovat i rutiny pro správu služeb Virtual Machines, Virtual Network a účtů služby Storage. Další informace si můžete přečíst [tady](../articles/powershell-azure-resource-manager.md).

### Azure CLI

Rozhraní příkazového řádku Azure (Azure CLI) bude mít dva režimy nasazení – režim **AzureServiceManagement** a režim **AzureResourceManager**. Režim AzureResourceManager bude teď obsahovat i příkazy pro správu služeb Virtual Machines, Virtual Network a účtů služby Storage. Další informace si můžete přečíst [tady](../articles/xplat-cli-azure-resource-manager.md).

### Visual Studio

V nejnovější verzi sady Azure SDK pro sadu Visual Studio můžete vytvářet a nasazovat služby Virtual Machines a složité aplikace přímo ze sady Visual Studio. Visual Studio nabízí možnost nasazení ze seznamu předdefinovaných šablon nebo zahájení z prázdné šablony.

### Rozhraní REST API

Podrobnou dokumentaci k REST API pro poskytovatele výpočetních a síťových prostředků a prostředků úložiště najdete [tady](https://msdn.microsoft.com/library/azure/dn790568.aspx).

## Nejčastější dotazy

**Můžu vytvořit virtuální počítač pomocí nového správce Azure Resource Manager, abych ho nasadil ve službě Virtual Network nebo účtu služby Storage, který byl vytvořený pomocí rozhraní Azure Service Management API?**

Toto se aktuálně nepodporuje. Pomocí nového správce Azure Resource Manager nemůžete nasadit službu Virtual Machine ve službě Virtual Network ve virtuální síti, která byla vytvořena pomocí rozhraní API pro správu služeb.

**Můžu vytvořit virtuální počítač pomocí nových rozhraní správce Azure Resource Manager z uživatelského image, který byl vytvořený pomocí rozhraní Azure Service Management API?**

Toto se aktuálně nepodporuje. Přesto můžete kopírovat soubory virtuálního pevného disku z účtu služby Storage, který byl vytvořený pomocí rozhraní API pro správu služeb, na nový účet vytvořený pomocí nových rozhraní API správce Azure Resource Manager.

**Jaký bude dopad na kvótu pro moje předplatné?**

Kvóty pro služby Virtual Machines, Virtual Network a účty služby Storage vytvořené pomocí nových rozhraní API správce Azure Resource Manager jsou oddělené od vašich aktuálních kvót. Každé předplatné získá nové kvóty pro vytvoření prostředků pomocí nových rozhraní API. Další informace o dodatečných kvótách najdete [tady](../articles/azure-subscription-service-limits.md).

**Můžu dál používat své automatizované skripty pro zřizování služeb Virtual Machines, Virtual Network, účtů služby Storage atd. prostřednictvím nových rozhraní API správce Azure Resource Manager?**

Všechny vámi vytvořené automatizace a skripty budou fungovat v existujících službách Virtual Machines a Virtual Network, které byly vytvořeny v rámci režimu Azure Service Management. Skripty je však potřeba aktualizovat, aby používaly nové schéma pro vytváření stejných prostředků prostřednictvím nového režimu správce Azure Resource Manager.

**Můžu služby Virtual vytvořené pomocí nových rozhraní API správce Azure Resource Manager připojit ke svému okruhu Express Route?**

Toto se aktuálně nepodporuje. Služby Virtual Network vytvořené pomocí nových rozhraní API správce Azure Resource Manager nemůžete připojit ke svému okruhu Express Route. Tato funkčnost bude podporovaná v budoucnu.

**Kde najdu příklady šablon správce Azure Resource Manager?**

Ucelenou sadu úvodních šablon najdete v [rychlých šablonách správce Azure Resource Manager](https://azure.microsoft.com/documentation/templates/).



<!--HONumber=Jun16_HO2-->


