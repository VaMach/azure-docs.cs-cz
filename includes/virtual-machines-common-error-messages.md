>[!NOTE]
> Komentáře můžete nechat na této stránce pro zpětnou vazbu nebo prostřednictvím [Azure zpětné vazby](https://feedback.azure.com/forums/216843-virtual-machines) s #azerrormessage značkou.

## <a name="error-response-format"></a>Formát odpovědi chyby 
Virtuální počítače Azure pomocí následujícího formátu JSON pro odpovědi na chybu:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner evel error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Chybnou odpověď vždy obsahuje stavový kód a objekt chyby. Každý objekt chyba vždy obsahuje kód chyby a zprávu. Pokud virtuální počítač je vytvořen pomocí šablony, objekt chyba obsahuje také podrobnosti oddíl, který obsahuje vnitřní úroveň kódy chyb a zpráv. Za normálních okolností nejvíce vnitřní úroveň chybová zpráva je kořenový selhání.


## <a name="common-virtual-machine-management-errors"></a>Běžné chyby správy virtuálního počítače.

V této části jsou uvedeny běžné chybové zprávy, se můžete setkat při správě virtuálních počítačů:

|  Kód chyby  |  Chybová zpráva  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  Nepodařilo se získat zapůjčení při vytváření disku {0}' pomocí {1} identifikátor URI objektu blob. Objekt blob je již používán.  |  
|  AllocationFailed  |  Přidělení se nezdařilo. Zkuste zmenšit velikost virtuálního počítače nebo počet virtuálních počítačů, opakujte akci později nebo zkuste nasazení na jinou skupinu dostupnosti nebo jiném umístění Azure.  |  
|  AllocationFailed  |  Přidělení virtuálního počítače se nezdařilo z důvodu vnitřní chyby. Opakujte akci později nebo zkuste nasazení do jiného umístění.  |
|  ArtifactNotFound  |  Rozšíření virtuálního počítače vydavatele: {0} a typem objekt {1}' nebyl nalezen v umístění '{2}'.  |
|  ArtifactNotFound  |  Objekt {1}' typ rozšíření s vydavatelem: {0}, a verzí obslužné rutiny typu '{2}' nebyl nalezen v úložišti rozšíření.  |
|  ArtifactVersionNotFound  |  V úložišti artefaktů se nenašla žádná verze, která by splnila požadavek na verzi {0}.  |
|  ArtifactVersionNotFound  |  Žádná verze najít v úložišti artefaktů, který splňuje požadavek na verzi {0}' pro rozšíření virtuálního počítače s vydavatele objekt {1}' a typu '{2}'.  |
|  AttachDiskWhileBeingDetached  |  Nelze se připojit datový disk {0}' k objekt {1}' virtuální počítač, protože je disk momentálně odpojuje. Počkejte prosím, než se disk úplně odpojí a akci opakujte.  |
|  Struktura BadRequest  |  Zarovnání ' skupiny dostupnosti ještě nejsou v této oblasti nepodporuje.  |
|  Struktura BadRequest  |  Přidání virtuálního počítače s disky spravované na nespravované sadu dostupnosti nebo přidání virtuálního počítače s disky na základě objektů blob do spravované skupiny dostupnosti není podporována. Vytvořte skupiny dostupnosti s vlastností 'spravované' nastavit, aby bylo možné přidat virtuální počítač s spravovaných disků na ni.  |
|  Struktura BadRequest  |  Spravované disky nejsou podporovány v této oblasti.  |
|  Struktura BadRequest  |  Více rozšíření Vmextension na obslužnou rutinu není podporováno pro operační systém, zadejte: {0}. Rozšíření VMExtension objekt {1}' s obslužnou rutinou '{2}' je již přidáno nebo určeno ve vstupu.  |
|  Struktura BadRequest  |  Operace: {0} není podporována u prostředku objekt {1}' u spravovaných disků.  |
|  CertificateImproperlyFormatted  |  Reprezentace JSON tajného klíče získaná z {0} obsahuje datové pole, které není správně formátovaným souborem PFX, nebo poskytnuté heslo nedekóduje správně soubor PFX.  |
|  CertificateImproperlyFormatted  |  Data získaná z {0} nejde deserializovat do formátu JSON.  |
|  Konflikt  |  Změna velikosti disku je povolená jenom při vytvoření virtuálního počítače nebo při zrušení jeho přiřazení.  |
|  ConflictingUserInput  |  Disk {0}' nelze připojit, protože disk je již vlastněn virtuálním počítačem objekt {1}.  |
|  ConflictingUserInput  |  Zdrojová skupina prostředků je stejná jako cílová skupina prostředků.  |
|  ConflictingUserInput  |  Zdrojový účet úložiště pro disk {0} se liší od cílového účtu.  |
|  ContainerAlreadyOnLease  |  V kontejneru úložiště, který uchovává objekt blob s identifikátorem URI {0}, už existuje zapůjčení.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Žádost o přesunutí prostředků obsahuje prostředky KeyVault, na které odkazuje jeden nebo více {0} s v požadavku. Aktuálně nejsou podporované mezi předplatnými Move. Zkontrolujte podrobnosti chyby pro identifikátory prostředku KeyVault.  |
|  DiagnosticsOperationInternalError  |  Při zpracovávání diagnostického profilu virtuálního počítače {0} došlo k vnitřní chybě.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  {0} objektu BLOB se už používá jiný disk, který patří k virtuálnímu počítači objekt {1}. Můžete zkontrolovat metadata objektu blob pro referenční informace o disku.  |
|  DiskBlobNotFound  |  Nepodařilo se najít objekt blob VHD s {0} identifikátor URI pro disk objekt {1}.  |
|  DiskBlobNotFound  |  Nepodařilo se najít objekt blob VHD s identifikátorem URI {0}.  |
|  DiskEncryptionKeySecretMissingTags  |  tajný klíč {0} nemá značky {1}. Aktualizovat verzi tajného klíče, přidejte požadované značky a opakujte.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Rozbalení hodnoty tajného {0} hodnotu pomocí klíče {1} se nezdařilo.  |
|  DiskImageNotReady  |  {0} bitové kopie disku je ve stavu {1}. Opakujte akci po obrázek je připravený.  |
|  DiskPreparationError  |  Při přípravě disky virtuálních počítačů došlo k jedné nebo více chybám. Najdete v zobrazení instance disku podrobnosti.  |
|  DiskProcessingError  |  Zpracování disku zastavit virtuální počítač má jiné disky v přírůstcích u disků se nezdařilo.  |
|  ImageBlobNotFound  |  Nepodařilo se najít objekt blob VHD s {0} identifikátor URI pro disk objekt {1}.  |
|  ImageBlobNotFound  |  Nepodařilo se najít objekt blob VHD s identifikátorem URI {0}.  |
|  IncorrectDiskBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. {0} objektu BLOB pro objekt {1}' disk je typu objekt BLOB bloku.  |
|  IncorrectDiskBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. Objekt BLOB {0} je typu objekt {1}.  |
|  IncorrectImageBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. {0} objektu BLOB pro objekt {1}' disk je typu objekt BLOB bloku.  |
|  IncorrectImageBlobType  |  Diskové objekty BLOB mohou být pouze typu Objekt blob stránky. Objekt BLOB {0} je typu objekt {1}.  |
|  InternalOperationError  |  Nebylo možné přeložit účtu úložiště {0}. Zkontrolujte, zda že se vytvořil prostřednictvím poskytovatele prostředků úložiště ve stejném umístění jako nachází výpočetní prostředek.  |
|  InternalOperationError  |  úkolů hledání cíle {0} se nezdařilo.  |
|  InternalOperationError  |  Při ověřování síťového profilu virtuálního počítače {0} nastala chyba.  |
|  InvalidAccountType  |  AccountType {0} je neplatný.  |
|  InvalidParameter  |  Hodnota parametru {0} je neplatná.  |
|  InvalidParameter  |  Zadané heslo správce není povolené.  |
|  InvalidParameter  |  "Zadané heslo musí být mezi {0}-\ {1\} znaků a musí splňovat alespoň {2} požadavky na složitost hesla z následujícího: <ol><li> Obsahuje velké písmeno</li><li>Obsahuje malé písmeno.</li><li>Obsahuje číslici</li><li>Obsahuje speciální znak.</li></ol>  |
|  InvalidParameter  |  Zadané uživatelské jméno správce není povolené.  |
|  InvalidParameter  |  Pokud je virtuální počítač vytvořený z platformy nebo uživatelské image, nejde připojit existující disk s operačním systém.  |
|  InvalidParameter  |  Název kontejneru {0} je neplatný. Názvy kontejnerů musí být 3 až 63 znaků a může obsahovat jenom malé alfanumerické znaky a spojovníky. Pomlčka musí být dlouhé a následuje alfanumerický znak.  |
|  InvalidParameter  |  {0} název kontejneru v {1} adresa URL je neplatná. Názvy kontejnerů musí být 3 až 63 znaků a může obsahovat jenom malé alfanumerické znaky a spojovníky. Pomlčka musí být dlouhé a následuje alfanumerický znak.  |
|  InvalidParameter  |  Název objektu blob v adrese URL {0} obsahuje lomítko. V současné době není podporována pro disky.  |
|  InvalidParameter  |  Zdá se, že URI {0} není správný identifikátor URI objektu blob.  |
|  InvalidParameter  |  Disk s názvem: {0} už používá stejnou logickou jednotku: {1}.  |
|  InvalidParameter  |  Disk s názvem: {0} již existuje.  |
|  InvalidParameter  |  Pro disk, který už je v zadané referenci image definovaný, nejde zadat uživatelská přepsání image.  |
|  InvalidParameter  |  Disk s názvem: {0} už používá stejnou adresu URL VHD {1}.  |
|  InvalidParameter  |  {0} zadaný selhání domény počet musí spadat do {2} {1} rozsahu.  |
|  InvalidParameter  |  Licence typu {0} je neplatný. Platné typy licencí jsou: Windows_Client nebo Windows_Server, rozlišují velká a malá písmena.  |
|  InvalidParameter  |  Název hostitele Linux nemůže být delší než {0} znaků nebo obsahovat následující znaky: {1}.  |
|  InvalidParameter  |  Cílová cesta pro veřejné klíče SSH je aktuálně omezená na výchozí hodnotu {0} z důvodu známého problému s linuxovým agentem zřizování.  |
|  InvalidParameter  |  Disk na logické jednotce (LUN) {0} již existuje.  |
|  InvalidParameter  |  {0} předplatné žádosti se musí shodovat {1} předplatné obsažené v id spravovaného disku.  |
|  InvalidParameter  |  Vlastní data v OSProfile musí být v kódování Base64 a obsahovat nejvýš {0} znaků.  |
|  InvalidParameter  |  Název objektu BLOB v adrese URL {0} musí mít příponu objekt {1}.  |
|  InvalidParameter  |  {0}' není platná předpona názvu objektu blob zaznamenané virtuálního pevného disku. Platná předpona odpovídá regulárnímu výrazu objekt {1}.  |
|  InvalidParameter  |  Pokud není zřízený agent virtuálního počítače, nedají se k virtuálnímu počítači přidávat certifikáty.  |
|  InvalidParameter  |  Disk na logické jednotce (LUN) {0} již existuje.  |
|  InvalidParameter  |  Nelze vytvořit virtuální počítač protože požadovaná velikost {0} není k dispozici v clusteru, kde nastavení dostupnosti je právě přiděleno. Dostupné velikosti jsou: {1}. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  InvalidParameter  |  {0} požadovaný virtuální počítač velikost není k dispozici v aktuální oblasti. Velikosti, která je k dispozici v aktuální oblasti jsou: {1}. Získat další informace o dostupných velikostí virtuálních počítačů v každé oblasti v https://aka.ms/azure-regions.  |
|  InvalidParameter  |  {0} požadovaný virtuální počítač velikost není k dispozici v aktuální oblasti. Získat další informace o dostupných velikostí virtuálních počítačů v každé oblasti v https://aka.ms/azure-regions.  |
|  InvalidParameter  |  Uživatelské jméno správce Windows nemůže být delší než {0} znaků, tečkou končit ani obsahovat následující znaky: {1}.  |
|  InvalidParameter  |  Název počítače Windows nemůže být delší než {0} znaků, složen výhradně z číslic ani obsahovat následující znaky: {1}.  |
|  MissingMoveDependentResources  |  Žádost o přesun prostředků neobsahuje všechny závislé prostředky. Zkontrolujte podrobnosti o chybě pro ID chybějících prostředků.  |
|  MoveResourcesHaveInvalidState  |  Žádost o přesunutí prostředků obsahuje virtuální počítače, které jsou spojeny s účty úložiště nejsou platné. Zkontrolujte podrobnosti o těchto ID prostředků a názvy účtů odkazovaných úložiště.  |
|  MoveResourcesHavePendingOperations  |  Žádost o přesunutí prostředků obsahuje prostředky, pro které čeká na vyřízení operace. Zkontrolujte podrobnosti o těchto ID prostředků. Opakujte operaci, jakmile budou čekající operace dokončeny.  |
|  MoveResourcesNotFound  |  Žádost o přesunutí prostředků obsahuje prostředky, které nelze nalézt. Zkontrolujte podrobnosti o těchto ID prostředků.  |
|  NetworkingInternalOperationError  |  Neznámá chyba přidělení sítě.  |
|  NetworkingInternalOperationError  |  Neznámá chyba přidělení sítě  |
|  NetworkingInternalOperationError  |  Při zpracování síťového profilu virtuálního počítače nastala vnitřní chyba.  |
|  notFound  |  Skupina dostupnosti {0} nejde najít.  |
|  notFound  |  Zdrojový virtuální počítač {0}' zadaná v žádosti se v tomto umístění Azure neexistuje.  |
|  notFound  |  Klient s ID {0} se nenašel.  |
|  notFound  |  Nejde najít Image {0}.  |
|  NotSupported  |  Typ licence je {0}, ale {1} objektu blob bitové kopie není místní.  |
|  OperationNotAllowed  |  Dostupnost sady {0} nelze odstranit. Před odstraněním skupiny dostupnosti zkontrolujte, zda neobsahuje žádné virtuální počítače.  |
|  OperationNotAllowed  |  Změna nastavení dostupnosti SKU z "Pevně" na "Klasickém" není povolena.  |
|  OperationNotAllowed  |  Pokud virtuální počítač neběží, nedají se v něm upravovat rozšíření.  |
|  OperationNotAllowed  |  Akce zaznamenání je podporována pouze na virtuálním počítači s disky, na základě objektů blob. Použijte prosím prostředku 'Image' rozhraní API pro vytvoření Image z spravované virtuálního počítače.  |
|  OperationNotAllowed  |  {0} prostředek nelze vytvořit z Image {1}, dokud bitové kopie byla úspěšně vytvořena.  |
|  OperationNotAllowed  |  Aktualizace nastavení encryptionSettings nejsou povoleny, pokud je virtuální počítač přidělen. Zkuste to prosím znovu po zrušení jeho přidělení.  |
|  OperationNotAllowed  |  Přidání spravovaného disku k virtuálnímu počítači s disky, které jsou založené na objektech blob, se nepodporuje.  |
|  OperationNotAllowed  |  Maximální počet datových disků, připojí se k této velikosti virtuálního počítače je {0}.  |
|  OperationNotAllowed  |  Přidání disku, který je založený na objektech blob, k virtuálnímu počítači se spravovanými disky se nepodporuje.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na bitovou kopii objekt {1}' vzhledem k tomu, že obrázek je označena pro odstranění. Můžete pouze operaci odstranění (nebo počkejte některého probíhající Dokončit).  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' vzhledem k tomu, že virtuální počítač je zobecněný.  |
|  OperationNotAllowed  |  Operace: {0} není povolena, protože kolekce bodu obnovení objekt {1}' je označena pro odstranění.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na rozšíření virtuálního počítače objekt {1}' vzhledem k tomu, že je označena pro odstranění. Můžete pouze operaci odstranění (nebo počkejte některého probíhající Dokončit).  |
|  OperationNotAllowed  |  Operace: {0} není povolená, protože probíhá zřizování virtuálních počítačů objekt {1}' pomocí bitové kopie, {2}'.  |
|  OperationNotAllowed  |  Operace {0}' není povolena, protože virtuální počítač ScaleSet objekt {1}' právě používá, {2}, bitovou kopii.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' vzhledem k tomu, že virtuální počítač je označena pro odstranění. Můžete pouze operaci odstranění (nebo počkejte některého probíhající Dokončit).  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' vzhledem k tomu, že virtuální počítač je zrušeno přiřazení nebo je označený k zrušení přiřazení.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' vzhledem k tomu, že je virtuální počítač spuštěný. Prosím power vypnout explicitně v případě, že vypnout virtuální počítač z uvnitř hostovaného operačního systému.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' vzhledem k tomu, že virtuální počítač není navrácena.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}' vzhledem k tomu, že virtuální počítač má příponu '{2}' v chybovém stavu.  |
|  OperationNotAllowed  |  Operace: {0} není povolená na virtuálním počítači objekt {1}', protože probíhá jiná operace.  |
|  OperationNotAllowed  |  Operace: {0} vyžaduje virtuální počítač objekt {1}' Chcete-li být zobecněn.  |
|  OperationNotAllowed  |  Operace vyžaduje, aby virtuální počítač běžel (nebo aby byl nastaven na spuštění).  |
|  OperationNotAllowed  |  Disk s velikost {0} GB, což je menší než velikost {1}GB odpovídající disku v bitové kopii, není povolen.  |
|  OperationNotAllowed  |  Rozšíření obslužné rutiny {0} škálovací sady virtuálního počítače jde přidat jenom během vytváření škálovací sady virtuálního počítače.  |
|  OperationNotAllowed  |  Rozšíření obslužné rutiny {0} škálovací sady virtuálního počítače jde odstranit jenom během odstraňování škálovací sady virtuálního počítače.  |
|  OperationNotAllowed  |  Virtuální počítač {0}' již používá spravované disky.  |
|  OperationNotAllowed  |  Virtuální počítač {0}' patří do sady, Classic' dostupnosti objekt {1}. Aktualizujte prosím skupinu dostupnosti pomocí 'pevně, SKU a poté opakujte převod.  |
|  OperationNotAllowed  |  Virtuální počítač vytvořený z Image nemůže mít objekt blob na základě disky. Všechny disky musí být spravované disky.  |
|  OperationNotAllowed  |  Operaci zachycení nejde dokončit, protože virtuální počítač není zobecněný.  |
|  OperationNotAllowed  |  Operace správy na virtuálním počítači {0}' nejsou povoleny, protože se převedou disky virtuálních počítačů do spravovaných disků.  |
|  OperationNotAllowed  |  Probíhající operace je změna stavu napájení {0} virtuálního počítače na {1}. Proveďte operaci {2} po určité době.  |
|  OperationNotAllowed  |  Nelze přidat nebo aktualizovat virtuální počítač. {0} požadovaný virtuální počítač velikost nemusí být k dispozici v existující alokační jednotky. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Nelze změnit velikost virtuálního počítače protože požadovaná velikost {0} není k dispozici v clusteru, kde nastavení dostupnosti je právě přiděleno. Dostupné velikosti jsou: {1}. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  OperationNotAllowed  |  Nelze změnit velikost virtuálního počítače, protože požadovaná velikost {0} není k dispozici v clusteru, kde je aktuálně přidělené virtuální počítač. Ke změně velikosti virtuálního počítače do {1} prosím navrácení (to je zastavení operace na portálu Azure) a změny velikosti operaci opakujte. Přečtěte si další virtuální počítač na změnu velikosti strategie v https://aka.ms/azure-resizevm.  |
|  OSProvisioningClientError  |  Zřizování operačního systému se nezdařilo u virtuálního počítače {0}, protože se právě zřizuje hostitelský operační systém.  |
|  OSProvisioningClientError  |  Zřizování operačního systému pro virtuální počítač {0} se nezdařilo. Podrobnosti o chybě: {1} zkontrolujte image správně připravena (generalizována). <ul><li>Pokyny pro Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  Generování klíčů SSH hostitele se nezdařilo. Podrobnosti o chybě: {0}. Chcete-li vyřešit tento problém ověřit, pokud je správně nastavený agenta systému Linux. <ul><li>Můžete zkontrolovat, postupujte podle pokynů v: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-agent-user-guide/ </li></ul> |
|  OSProvisioningClientError  |  Uživatelské jméno pro virtuální počítač je neplatný pro této distribuce systému Linux. Podrobnosti o chybě: {0}.  |
|  OSProvisioningInternalError  |  Zřizování operačního systému pro virtuální počítač {0} se nepovedlo, protože nastala vnitřní chyba.  |
|  OSProvisioningTimedOut  |  Zřizování operačního systému pro virtuální počítač {0}' nebyl dokončen v přiděleném čase. Virtuální počítač může stále dokončit úspěšně. Zkontrolujte prosím stav zřizování později.  |
|  OSProvisioningTimedOut  |  Zřizování operačního systému pro virtuální počítač {0}' nebyl dokončen v přiděleném čase. Virtuální počítač může stále dokončit úspěšně. Zkontrolujte prosím stav zřizování později. Taky se ujistěte, image správně připravena (generalizována).   <ul><li>Pokyny pro Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Pokyny pro Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  Zřizování operačního systému pro virtuální počítač {0}' nebyl dokončen v přiděleném čase. Agenta hosta virtuálního počítače, ale byla zjištěna systémem. To naznačuje, že hostovaný operační systém nebyl správně připravené pro použití jako image virtuálního počítače (CreateOption = FromImage). Chcete-li vyřešit tento problém, buď použijte virtuální pevný disk, protože je s CreateOption = připojit nebo ji správně připravit pro použití jako obrázek:   <ul><li>Pokyny pro Windows: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Pokyny pro Linux: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Požadovaná velikost virtuálního počítače není v tuto chvíli na vybraném umístění dostupná.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Prostředek nelze aktualizovat, protože aktualizace probíhající platformy. Zkuste to prosím znova později.  |
|  StorageAccountLimitation  |  Účet úložiště {0} nepodporuje objekty blob stránek, které se ale vyžadují k vytváření disků.  |
|  StorageAccountLimitation  |  Účet úložiště {0} překročil jemu přidělenou kvótu.  |
|  StorageAccountLocationMismatch  |  Nebylo možné přeložit účtu úložiště {0}. Zkontrolujte, zda že se vytvořil prostřednictvím poskytovatele prostředků úložiště ve stejném umístění jako nachází výpočetní prostředek.  |
|  StorageAccountNotFound  |  Úložiště účet {0} nebyl nalezen. Zkontrolujte účet úložiště není odstraněný a že patří do stejného umístění Azure jako virtuální počítač.  |
|  StorageAccountNotRecognized  |  Použijte prosím účet úložiště spravovaný poskytovatelem prostředků úložiště. Použití {0} není podporováno.  |
|  StorageAccountOperationInternalError  |  Při přístupu k účtu úložiště {0} nastala vnitřní chyba.  |
|  StorageAccountSubscriptionMismatch  |  {0} účet úložiště nenáleží do předplatného {1}.  |
|  StorageAccountTooBusy  |  Účet úložiště {0}' je aktuálně zaneprázdněna. Zvažte použití jiného účtu.  |
|  StorageAccountTypeNotSupported  |  Disk {0} používá {1}, což je účet úložiště Blob. Zkuste to prosím znovu s účtem úložiště pro obecné účely.  |
|  StorageAccountTypeNotSupported  |  {0} účet úložiště je typu {1}. Diagnostika spouštění podporuje typy účtů úložiště {2}.  |
|  SubscriptionNotAuthorizedForImage  |  Předplatné není autorizováno.  |
|  TargetDiskBlobAlreadyExists  |  Objekt BLOB {0} již existuje. Zadejte jiný identifikátor URI pro vytvoření disku pro nový prázdný datový objekt {1}' objektu blob.  |
|  TargetDiskBlobAlreadyExists  |  Zaznamenat operace nemůže pokračovat, protože cíl image blob {0} již existuje a není nastavený příznak k přepsání objektů BLOB VHD. Objekt blob odstraňte nebo nastavte příznak k přepsání objektů BLOB virtuálního pevného disku a opakujte.  |
|  TargetDiskBlobAlreadyExists  |  Operace zachytávání nemůže pokračovat, protože cílový objekt blob bitové kopie {0} má aktivní zapůjčení.   |
|  TargetDiskBlobAlreadyExists  |  Objekt BLOB {0} již existuje. Zadejte jiný identifikátor URI objektu blob jako cíl pro disk objekt {1}.  |
|  TooManyVMRedeploymentRequests  |  Příliš mnoho požadavků na opětovné nasazení byly přijaty pro virtuální počítač {0}' nebo virtuální počítače ve stejné sadě dostupnosti tohoto virtuálního počítače. Zkuste to prosím znovu později.  |
|  VHDSizeInvalid  |  Zadaná hodnota velikosti disku {0} pro disk objekt {1}' s {2} objektu blob je neplatný. Velikost disku musí být až {3}, {4}.  |
|  VMAgentStatusCommunicationError  |  Virtuální počítač {0}' neohlásil stav pro agenta virtuálního počítače nebo rozšíření. Zkontrolujte, zda virtuální počítač je spuštěn agent virtuálního počítače a možné navázat odchozí připojení k úložišti Azure.  |
|  VMArtifactRepositoryInternalError  |  Při komunikaci s úložištěm artefaktů, aby se získaly podrobnosti o artefaktu virtuálního počítače, nastala chyba.  |
|  VMArtifactRepositoryInternalError  |  Při načítání dat artefaktů virtuálního počítače z úložiště artefaktů došlo k vnitřní chybě.  |
|  VMExtensionHandlerNonTransientError  |  Obslužná rutina: {0} oznámil chybu pro rozšíření virtuálního počítače objekt {1}' s kódem chyby terminálu, {2} a chybová zpráva: '{3}.  |
|  VMExtensionManagementInternalError  |  Při zpracování rozšíření virtuálního počítače {0} nastala  vnitřní chyba.  |
|  VMExtensionManagementInternalError  |  Při přípravě rozšíření virtuálního počítače došlo k několika chybám. Najdete v zobrazení instance virtuálního počítače rozšíření podrobnosti.  |
|  VMExtensionProvisioningError  |  Virtuální počítač nahlásil chybu při zpracování rozšíření: {0}. Chybová zpráva: "{1}".  |
|  VMExtensionProvisioningError  |  Několik rozšíření virtuálního počítače se nepovedlo zřídit ve virtuálním počítači. Podrobnosti viz zobrazení instance virtuálního počítače rozšíření podrobnosti.  |
|  VMExtensionProvisioningTimeout  |  Zřizování rozšíření virtuálního počítače {0}' časový limit. Instalace rozšíření možná trvá příliš dlouho, nebo nebylo možné získat stav rozšíření.  |
|  VMMarketplaceInvalidInput  |  Vytvoření virtuálního počítače z image pořízenou prostřednictvím Marketplace bez nemusí informace o plánu, odeberte prosím informace o plánu v požadavku. Název disku operačního systému je {0}.  |
|  VMMarketplaceInvalidInput  |  Informace o nákupu neodpovídá. Nelze nasadit z webu Marketplace bitové kopie. Název disku operačního systému je {0}.  |
|  VMMarketplaceInvalidInput  |  Vytvoření virtuálního počítače image pořízené na Marketplace vyžaduje informace o plánu v požadavku. Název disku operačního systému je {0}.  |
|  VMNotFound  |  Virtuální počítač {0} nebyl nalezen.  |
|  VMRedeploymentFailed  |  Opětovné nasazení virtuálního počítače {0} se nezdařila kvůli vnitřní chybě. Zkuste to prosím znovu později.  |
|  VMRedeploymentTimedOut  |  Opětovné nasazení virtuálního počítače {0}' neskončila v přiděleném čase. Se může úspěšně dokončit v určité době. Jinak můžete tento požadavek zopakovat.  |
|  VMStartTimedOut  |  Virtuální počítač {0} se nepodařilo spustit v přiděleném čase. Virtuální počítač stále může úspěšně spustit. Zkontrolujte prosím stav napájení později.  |


## <a name="next-steps"></a>Další kroky
Pokud potřebujete další pomoc, obraťte se na Azure odborníky na [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/support/options/) a vyberte **získat podporu**.