<properties
   pageTitle="Přehled Azure Resource Manageru | Microsoft Azure"
   description="Popisuje, jak Azure Resource Manager využívat k nasazení, správě a řízení přístupu k prostředkům v Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="tomfitz"/>

# Přehled Azure Resource Manageru

Infrastrukturu aplikace obvykle tvoří celá řada komponent, může to být třeba virtuální počítač, účet úložiště a virtuální síť nebo webová aplikace, databáze, databázový server a služby jiných výrobců. Tyto komponenty nevidíte jako samostatné entity, ale jako související a vzájemně provázané části jedné entity. Chcete je nasadit, spravovat a monitorovat jako skupinu. Azure Resource Manager umožňuje pracovat s prostředky ve vašem řešení jako se skupinou. Všechny prostředky pro vaše řešení můžete nasadit, aktualizovat nebo odstranit v rámci jediné koordinované operace. Pro nasazení použijete šablonu a tato šablona může fungovat v různých prostředích, jako je testovací, přípravné nebo produkční prostředí. Resource Manager poskytuje funkce zabezpečení, auditování a označování, které vám po nasazení pomohou prostředky spravovat. 

## Terminologie

Pokud je pro vás Azure Resource Manager novinkou, následuje pár termínů, se kterými byste nemuseli být obeznámeni.

- **prostředek** - Spravovatelná položka, která je k dispozici prostřednictvím služby Azure. Mezi běžné prostředky patří virtuální počítač, účet úložiště, webová aplikace, databáze nebo virtuální síť, ale existuje i mnoho dalších.
- **skupina prostředků** - Kontejner, který obsahuje související prostředky pro aplikaci. Skupina prostředků může zahrnovat všechny prostředky pro aplikaci nebo jenom ty prostředky, které sami seskupíte dohromady. To, jakým způsobem se prostředky přidělí do skupin prostředků, můžete rozhodnout na základě toho, co je pro vaši organizaci nejvhodnější. Viz [Skupiny prostředků](#resource-groups).
- **poskytovatel prostředků** - Služba poskytující prostředky, které můžete nasadit a spravovat prostřednictvím Resource Manageru. Každý poskytovatel prostředků nabízí operace pro práci s nasazenými prostředky. Mezi běžné poskytovatele prostředků patří Microsoft.Compute, který dodává virtuální počítače, Microsoft.Storage, který dodává účet úložiště, a Microsoft.Web, který dodává prostředky vztahující se k webovým aplikacím. Viz [Poskytovatelé prostředků](#resource-providers).
- **šablona Resource Manageru** - Soubor formátu JavaScript Object Notation (JSON), který definuje jeden nebo více prostředků k nasazení do skupiny prostředků. Definuje také závislosti mezi nasazenými prostředky. Šablony lze použít k nasazení prostředků konzistentně a opakovaně. Viz [Nasazení šablon](#template-deployment).
- **deklarativní syntaxe** - Syntaxe, která umožňuje prohlásit „Toto mám v úmyslu vytvořit“, aniž by k tomu bylo nutno psát sekvence programových příkazů. Šablona Resource Manageru je příkladem deklarativní syntaxe. V souboru definujete vlastnosti pro infrastrukturu k nasazení do Azure. 

## Výhody použití Resource Manageru

Resource Manager poskytuje několik výhod:

- Můžete všechny prostředky pro vaše řešení nasadit, spravovat a monitorovat jako skupinu a nemusíte je zpracovávat jednotlivě.
- Můžete svoje řešení opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně.
- Infrastrukturu můžete spravovat pomocí deklarativních šablon místo skriptů.
- Můžete definovat závislosti mezi prostředky, takže se nasadí ve správném pořadí.
- Můžete využít řízení přístupu pro všechny služby ve vaší skupině prostředků, protože do platformy pro správu je nativně integrováno řízení přístupu na základě role (RBAC).
- Můžete označit prostředky pomocí značek a logicky tak uspořádat všechny prostředky ve vašem předplatném.
- Můžete zpřehlednit fakturaci pro vaši organizaci tím, že zobrazíte souhrnné náklady pro celou skupinu nebo pro skupinu prostředků, které sdílejí stejnou značku.  

Resource Manager poskytuje nový způsob nasazení a správy vašich řešení. Pokud jste využívali dřívější model nasazení a chcete se dozvědět o změnách, přečtěte si článek věnovaný [nasazení Resource Manageru a klasickému nasazení](resource-manager-deployment-model.md).

## Doprovodné materiály

Následující návrhy vám pomohou při práci s vašimi řešeními plně využít výhod Resource Manageru.

1. K definování a nasazení infrastruktury využijte deklarativní syntaxi v šablonách Resource Manageru, nikoli imperativní příkazy.
2. V šabloně definujte všechny kroky nasazení a konfigurace. K nastavení svého řešení byste neměli využívat žádné ruční kroky.
3. Ke správě vašich prostředků využijte imperativní příkazy, například pro spuštění nebo zastavení aplikace nebo počítače.
4. Prostředky se stejným životním cyklem uspořádejte do skupiny prostředků. K ostatnímu uspořádání prostředků využijte značky.

Další doporučení najdete v tématu [Osvědčené postupy pro vytváření šablon Azure Resource Manageru](resource-manager-template-best-practices.md).

## Skupiny prostředků

Při definování skupin prostředků byste měli vzít v úvahu některé důležité faktory:

1. Všechny prostředky ve skupině by měly sdílet stejný životní cyklus. Budete je nasazovat, aktualizovat a odstraňovat společně. Pokud některý z prostředků, například databázový server, musí mít jiný cyklus nasazení, měl by být v jiné skupině prostředků.
2. Každý prostředek může být jenom v jedné skupině prostředků.
3. Prostředky je možné do skupiny prostředků kdykoli přidat nebo naopak odebrat.
4. Prostředky je možné přesouvat mezi skupinami. Další informace najdete v tématu, které se zabývá [přesunutím prostředků do nové skupiny prostředků nebo předplatného](resource-group-move-resources.md).
4. Skupina prostředků může obsahovat prostředky, které se nacházejí v různých oblastech.
5. Skupinu prostředků lze využít k určení rozsahu řízení přístupu pro akce správy.
6. Prostředek může mít interakci s prostředkem v jiné skupině prostředků, pokud spolu tyto dva prostředky souvisejí, ale nesdílejí stejný životní cyklus (například několik aplikací, které se připojují k databázi).

## Poskytovatelé prostředků

Každý poskytovatel prostředků nabízí sadu prostředků a operací pro práci v technické oblasti. Pokud například chcete ukládat klíče a tajné klíče, budete využívat poskytovatele prostředků **Microsoft.KeyVault**. Tento poskytovatel prostředků nabízí typ prostředku nazvaný **vaults** pro vytvoření trezoru klíčů a typ prostředku s názvem **vaults/secrets** pro vytvoření tajných údajů v trezoru klíčů. Umožňuje také operace prostřednictvím rozhraní [REST API služby Key Vault ](https://msdn.microsoft.com/library/azure/dn903609.aspx). Můžete volat přímo rozhraní REST API, nebo můžete pro správu trezoru klíčů použít [rutiny prostředí PowerShell služby Key Vault](https://msdn.microsoft.com/library/dn868052.aspx) a [rozhraní příkazového řádku Azure Key Vault](./key-vault/key-vault-manage-with-cli.md). K práci s většinou prostředků můžete také použít řadu programovacích jazyků. Další informace najdete v tématu [Sady SDK a ukázky](#sdks-and-samples). 

K nasazení a správě infrastruktury budete potřebovat, abyste o poskytovateli prostředků věděli víc, třeba typy prostředků, které nabízí, čísla verzí pro operace REST API, operace, které podporuje, a schéma, které se má použít při nastavení hodnot vytvářeného typu prostředku. Další informace o podporovaných poskytovatelích prostředků najdete v tématu věnovaném [poskytovatelů, oblastem, verzím API a schématům Resource Manageru](resource-manager-supported-services.md).

## Nasazení šablon

Pomocí Resource Manageru můžete vytvořit jednoduchou šablonu (ve formátu JSON), která definuje nasazení a konfiguraci vaší aplikace. Pomocí šablony můžete aplikaci opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně. Azure Resource Manager analyzuje závislosti a zajistí, že se prostředky vytvoří ve správném pořadí. Další informace najdete v tématu [Definování závislostí v šablonách Azure Resource Manageru](resource-group-define-dependencies.md).

Když vytvoříte řešení z portálu, bude toto řešení automaticky zahrnovat šablonu nasazení. Šablonu nemusíte vytvářet od začátku, protože můžete začít se šablonou pro svoje řešení a upravit ji tak, aby vyhovovala vašim konkrétním potřebám. Šablonu pro stávající skupinu prostředků můžete načíst tak, že vyexportujete aktuální stav této skupiny do šablony, nebo zobrazíte šablonu, která byla pro toto konkrétní nasazení použita. Zobrazení vyexportované šablony vám pomůže blíže se seznámit se syntaxí šablony. Další informace o práci s exportovanými šablonami najdete v článku, který popisuje [export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).

Není nutné definovat celou infrastrukturu v jediné šabloně. Často má smysl rozdělit požadavky nasazení do několika cílených šablon, které jsou zaměřené na konkrétní účel. Tyto šablony můžete snadno opakovaně využít pro různá řešení. Když chcete nasadit konkrétní řešení, vytvoříte hlavní šablonu, která propojí všechny požadované šablony. Další informace najdete v tématu věnovaném [použití propojených šablon s Azure Resource Managerem](resource-group-linked-templates.md).

Šablony můžete také využít pro aktualizace infrastruktury. Můžete například ke své aplikaci přidat nový prostředek a konfigurační pravidla pro prostředky, které jsou už nasazené. Pokud šablona specifikuje vytvoření nového assetu, ale tento asset již existuje, Azure Resource Manager místo vytvoření nového assetu provede jeho aktualizaci. Azure Resource Manager aktualizuje stávající asset do stejného stavu, jako kdyby byl nový. Můžete ale také určit, aby Resource Manager odstranil všechny prostředky, které nejsou v šabloně zadané. Vysvětlení různých možností při nasazení najdete v tématu, které popisuje [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md). 

V šabloně můžete zadat parametry, které umožňují její přizpůsobení a flexibilní nasazení. Je třeba možné předat hodnoty parametrů, které nasazení přizpůsobí vašemu testovacímu prostředí. Díky zadání parametrů můžete stejnou šablonu využít pro nasazení do všech prostředí vaší aplikace.

Resource Manager poskytuje rozšíření pro scénáře, kdy potřebujete další operace, jako je třeba instalace konkrétního softwaru, který není zahrnutý v původní instalaci. Pokud již využíváte službu pro správu konfigurace, jako je DSC, Chef nebo Puppet, můžete tuto službu s pomocí rozšíření používat i nadále.

Šablona se také stane součástí zdrojového kódu vaší aplikace. Můžete ji vrátit se změnami do vašeho úložiště zdrojového kódu a aktualizovat ji podle toho, jak se bude vaše aplikace vyvíjet. K úpravě šablony můžete použít Visual Studio.

Další informace o definování šablony najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).

Podrobné pokyny k vytvoření šablony najdete v [názorném průvodci k šablonám Resource Manageru](resource-manager-template-walkthrough.md).

Pokyny pro nasazení řešení do různých prostředí najdete v článku věnovaném [testovacím a vývojovým prostředím v Microsoft Azure](solution-dev-test-environments.md). 

## Značky

Resource Manager nabízí funkci označování, která umožňuje kategorizovat prostředky podle požadavků na správu nebo fakturaci. Značky pravděpodobně budete chtít použít v případě, že máte komplexní kolekci prostředků a skupin prostředků a potřebujete tyto assety vizualizovat co nejsmysluplnějším způsobem. Můžete například označit prostředky, které mají v rámci organizace podobnou roli nebo které patří do stejného oddělení. Uživatelé ve vaší organizaci mohou vytvořit různé prostředky, které se bez použití značek budou později těžko identifikovat a spravovat. Představte si třeba, že potřebujete odstranit všechny prostředky pro konkrétní projekt. Pokud ale nejsou označené značkami, budete je muset vyhledat ručně. Označení může také hrát důležitou roli při omezení zbytečných nákladů ve vašem předplatném. 

Prostředky se stejnou značkou nemusí být umístěné ve stejné skupině prostředků. Můžete vytvořit vlastní taxonomii značek a zajistit tak, že všichni uživatelé ve vaší organizaci budou využívat společné značky a nebudou neúmyslně zavádět vlastní (třeba odd. místo oddělení).

Další informace o značkách najdete v článku o [použití značek k uspořádání prostředků Azure](resource-group-using-tags.md). Můžete vytvořit [vlastní zásady](#manage-resources-with-customized-policies), které vyžadují doplnění značek k prostředkům během nasazení.

## Řízení přístupu

Resource Manager umožňuje určit, kdo má přístup ke konkrétním akcím pro vaši organizaci. Nativně integruje OAuth a řízení přístupu na základě role (RBAC) do platformy pro správu a toto řízení přístupu využívá pro všechny služby ve vaší skupině prostředků. Uživatele můžete přidat k předdefinovaným platformám a rolím pro konkrétní prostředky a potom tyto role použít pro předplatné, skupinu prostředků nebo prostředek a omezit tak přístup. Můžete například s výhodou využít předdefinovanou roli s názvem Přispěvatel databází SQL, která uživatelům umožňuje spravovat databáze, ale ne databázové servery nebo zásady zabezpečení. Uživatele ve vaší organizaci, kteří potřebují tento typ přístupu, přidáte k roli Přispěvatel databází SQL a tuto roli použijete pro předplatné, skupinu prostředků nebo prostředek.

Resource Manager automaticky protokoluje akce uživatelů pro účely auditování. Informace o využití protokolů auditu najdete v tématu věnovaném [operacím auditu s využitím Resource Manageru](resource-group-audit.md).

Další informace o řízení přístupu na základě rolí najdete v tématu [Řízení přístupu na základě role v Azure](./active-directory/role-based-access-control-configure.md). Téma věnované [integrovaným rolím RBAC](./active-directory/role-based-access-built-in-roles.md) obsahuje seznam integrovaných rolí a povolených akcí. Mezi integrované role patří obecné role, jako je Vlastník, Čtenář a Přispěvatel, ale také role pro konkrétní služby, jako je Přispěvatel virtuálních počítačů, Přispěvatel sítě a Správce zabezpečení SQL (to jsou jen některé z dostupných rolí).

Je také možné explicitně zamknout důležité prostředky a zabránit tak uživatelům v jejich změně nebo odstranění. Další informace najdete v tématu [Zamknutí prostředků pomocí Azure Resource Manageru](resource-group-lock-resources.md).

Doporučené postupy najdete v části věnované [požadavkům na zabezpečení pro Azure Resource Manager](best-practices-resource-manager-security.md).

## Správa prostředků pomocí vlastních zásad

Resource Manager umožňuje vytvářet vlastní zásady pro správu prostředků. Vytvářené zásady mohou zahrnovat nejrůznější scénáře, od vynucení zásad vytváření názvů u prostředků, přes omezení, které typy a instance prostředků je možné nasadit, nebo omezení, které oblasti mohou hostovat konkrétní typ prostředku, až po vyžadování značek u jednotlivých prostředků, aby se zajistila fakturace po odděleních. Zásady pomáhají omezit náklady a zajistit konzistenci v rámci předplatného. Další informace najdete v tématu [Použití zásad ke správě prostředků a řízení přístupu](resource-manager-policy.md).

## Konzistentní vrstva správy

Resource Manager poskytuje plně kompatibilní operace, ať už používáte Azure PowerShell, rozhraní příkazového řádku Azure pro Mac, Linux a Windows, portál Azure nebo REST API. Můžete využít rozhraní, které vám nejvíce vyhovuje, a rychle přecházet mezi rozhraními, aniž by to vedlo k nějakým nejasnostem. Portál dokonce zobrazuje oznámení pro akce, které se provedou mimo portál.

Informace o použití PowerShellu najdete v tématech [Použití Azure PowerShellu s Resource Managerem](powershell-azure-resource-manager.md) a [Rutiny Azure Resource Manageru](https://msdn.microsoft.com/library/azure/dn757692.aspx).

Informace o rozhraní příkazového řádku Azure najdete v tématu věnovaném [použití rozhraní příkazového řádku Azure CLI pro Mac, Linux a Windows se správou prostředků Azure](xplat-cli-azure-resource-manager.md).

Informace o rozhraní REST API najdete v [referenčním přehledu rozhraní REST Azure Resource Manageru](https://msdn.microsoft.com/library/azure/dn790568.aspx). Pokud chcete zobrazit operace REST pro vaše nasazené prostředky, přečtěte si téma věnované [použití Průzkumníku prostředků Azure k zobrazení a úpravám prostředků](resource-manager-resource-explorer.md).

Informace o použití portálu najdete v tématu [Nasazení prostředků pomocí šablon Resource Manageru a webu Azure Portal](resource-group-template-deploy-portal.md).

Azure Resource Manager podporuje technologii CORS (sdílení prostředků mezi zdroji). Díky CORS můžete volat rozhraní REST API Resource Manageru nebo služby Azure z webové aplikace, která je umístěná v jiné doméně. Bez podpory CORS by webový prohlížeč zabránil aplikaci z jedné domény v přístupu k prostředkům v jiné doméně. Resource Manager umožňuje použití CORS pro všechny požadavky s platnými přihlašovacími údaji pro ověřování.

## Sady SDK a ukázky

Sady Azure SDK jsou k dispozici pro různé jazyky a platformy.
Každá z těchto implementací jazyka je k dispozici prostřednictvím správce balíčků jejího ekosystému a GitHubu.

Kód v každé z těchto sad SDK je vygenerován ze specifikací rozhraní Azure RESTful API.
Tyto specifikace jsou open source a jsou založeny na specifikaci Swagger 2.0.
Kód SDK je generován prostřednictvím open source projektu s názvem AutoRest.
AutoRest transformuje tyto specifikace rozhraní RESTful API do klientských knihoven v několika jazycích.
Pokud chcete zlepšit některé aspekty generovaného kódu v sadách SDK, je celá sada nástrojů pro vytváření sad SDK otevřená, volně přístupná a založená na rozšířeném formátu specifikace rozhraní API.

**Ukázky**: Rychlý úvod ve zvoleném jazyce.

- [.NET](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=dotnet)
- [Java](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=java)
- [Node.js](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=nodejs)
- [Python](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=python)
- [PHP](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=php) *již brzy*
- [Ruby](https://azure.microsoft.com/documentation/samples/?service=azure-resource-manager&platform=ruby)

**Úložiště open source sad SDK**: Vítáme zpětnou vazbu, otázky a žádosti o získání dat.

- [.NET](https://github.com/Azure/azure-sdk-for-net)
- [Java](https://github.com/Azure/azure-sdk-for-java)
- [Node.js](https://github.com/Azure/azure-sdk-for-node)
- [PHP](https://github.com/Azure/azure-sdk-for-php)
- [Python](https://github.com/Azure/azure-sdk-for-python)
- [Ruby](https://github.com/Azure/azure-sdk-ruby)

> [AZURE.NOTE] Pokud sada SDK neobsahuje požadovanou funkci, můžete také přímo zavolat [rozhraní Azure REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx).

## Další kroky

- Jednoduchý úvod k práci se šablonami najdete v článku, který popisuje [export šablony Azure Resource Manageru ze stávajících prostředků](resource-manager-export-template.md).
- Podrobnější pokyny k vytvoření šablony najdete v [názorném průvodci k šablonám Resource Manageru](resource-manager-template-walkthrough.md).
- Funkce, které můžete v šabloně, využít, jsou popsané v článku [Funkce šablon](resource-group-template-functions.md)
- Informace o použití sady Visual Studio s Resource Managerem najdete v tématu [Vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
- Informace o použití VS Code pomocí Resource Manageru najdete v tématu [Práce s šablonami Azure Resource Manageru ve Visual Studio Code](resource-manager-vs-code.md).

Zde je videoukázka tohoto přehledu:

[AZURE.VIDEO azure-resource-manager-overview]



<!--HONumber=Aug16_HO4-->


