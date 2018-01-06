
 
Poslední zveřejnění [nová třída ohrožení zabezpečení procesoru](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) se označuje jako spekulativní provádění straně kanál útoky má za následek otázky z zákazníky, kteří potřebují další přehlednost. 

 
## <a name="azure-infrastructure"></a>Infrastrukturu Azure

Problémy popsané v zpřístupnění ohrožení zabezpečení by bylo možné obejít hranici hypervisoru a umožnit přístup k paměti mezi dvěma společně hostované virtuální počítače. Jsou uvedeny v předchozím kroku [příspěvku na blogu](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/), Azure má použít jejich zmírnění chránit zákazníky před toto ohrožení zabezpečení.  Společnost Microsoft doporučuje vždy zákazníkům instalaci osvědčené postupy zabezpečení pro vaše Image virtuálních počítačů, včetně instalace všech aktualizací zabezpečení od jejich dodavatele operačního systému.

## <a name="paas-services-on-azure"></a>PaaS služeb v Azure
Nabídky Azure PaaS mít ve výchozím nastavení nasazené způsoby zmírnění rizik. Zákazníci nevyžaduje žádnou akci. Azure Cloud Services výjimky jsou uvedeny níže.  


## <a name="azure-cloud-services"></a>Azure Cloud Services

[Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) s automatickou aktualizaci automaticky povoleno přijímat verzi hostovaného operačního systému, která zahrnuje jejich zmírnění pro tyto chyby zabezpečení. 

Následující verze hostovaného operačního systému zahrnují aktualizace s ochranná opatření proti ohrožení zabezpečení kanálu spekulativní provádění straně:

* WA-GUEST-OS-5.15_201801-01
* WA-GUEST-OS-4.50_201801-01


Malý počet zákazníků týkající se použití cloudových služeb Azure k hostování nedůvěryhodnými měli taky povolit [jádra virtuální adresu stínováním](#enabling-kernel-virtual-address-shadowing-on-windows-server) kromě aktualizace hostovaného operačního systému. To zajišťuje zvýšenou ochranu proti spekulativní provádění straně kanál ohrožení zabezpečení. Můžete to provést prostřednictvím úkolu spuštění. Další informace o tom, které zákazníci a scénáře použití vyžadují tato funkce a jak ji chcete povolit, najdete níže.


## <a name="azure-virtual-machines-windows--linux"></a>Virtuální počítače Azure (Windows a Linux)

Microsoft vždy doporučuje, aby zákazníci nainstalovat všechny aktualizace zabezpečení. 

Souhrn zabezpečení leden 2018 obsahuje opravy pro tyto chyby zabezpečení a další. Ověřte, že používáte podporovanou antivirový aplikaci před instalací aktualizací operačního systému. Kontaktujte dodavatele antivirového softwaru pro informace o kompatibilitě. 

Na adresu v spekulativní provádění slabá místa zabezpečení, aktualizace Linux jádra se bude vyžadovat a získáte od svého poskytovatele distribuce, pokud je k dispozici. 

Malý počet zákazníkům, kteří používají virtuální počítače Azure (Windows) pro hostování nedůvěryhodnými měli taky povolit [jádra virtuální adresu stínováním](#enabling-kernel-virtual-address-shadowing-on-windows-server) který zajišťuje zvýšenou ochranu proti spekulativní provádění straně kanálu chyby zabezpečení.  Další informace, o které zákazníků a využití scénáře vyžadují tato funkce a jak jej povolit najdete níže.


## <a name="enabling-kernel-virtual-address-shadowing-on-windows-server"></a>Povolení virtuální adresy jádra stínový provoz v systému Windows Server

Zákazníci, kteří používají Windows Server ke spouštění kódu vytvořeného nedůvěryhodné měli povolit funkci nazvanou jádra virtuální adresu stínový provoz, který poskytuje ochranu pro systémy, kde je prováděna nedůvěryhodnými s nízkou uživatelská oprávnění.

Tato dodatečná ochrana může mít vliv na výkon a je ve výchozím nastavení vypnuté. Jádra virtuální adresu stínováním chrání před zpřístupnění informací procesu proces a procesu jádra.

Vaše virtuální počítače, cloudové služby nebo místní servery jsou ohroženy v případě jejich spadat do jednoho z následujících kategorií:

* Azure vnořené virtualizace hostitele Hyper-V
* Vzdálené plochy (RDSH) hostitele
* Virtuální počítače nebo cloudové služby, které jsou spuštění nedůvěryhodné kódu jako kontejnery nebo nedůvěryhodné rozšíření pro databázi, nedůvěryhodné webového obsahu nebo úlohy, které spustit kód, který zajišťuje z externích zdrojů.

Příkladem scénáře, kde je nutné použít jádra virtuální adresu stínový provoz: 

|     |
|-----|
|Virtuální počítač Azure je spuštěna služba kam nedůvěryhodné uživatelé mohou odesílat kód JavaScript, který se spustí s omezenými oprávněními. Na stejný virtuální počítač existuje vysoce privilegované procesu, který obsahuje tajných dat, která nesmí být dostupný pro tyto uživatele. V takovém případě je potřebné k zajištění ochrany proti úniku mezi těmito dvěma jádra virtuální adresu stínový provoz.|
|     | 

Konkrétní pokyny k povolení jádra virtuální adresu stínováním jsou k dispozici prostřednictvím [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution).


> [!NOTE]
> V době publikace jádra virtuální adresu stínový provoz je k dispozici pouze v systému Windows Server 2016, Windows Server 2012 R2 a Windows Server 2008 R2.  
>
>

Pokud používáte Linux Server, obraťte se na dodavatele vaše operační systémy pro aktualizace a pokyny.

## <a name="branch-target-injection-mitigation-support-microcode"></a>Větev cíl vkládání zmírnění podporu (mikrokódu)

Zákazníci, kteří používají nástroje, které zjišťovat existenci způsoby zmírnění rizik na základě mikrokódu hlášení, že neopravené Azure. Toto je nesprávný. Během této publikace nebude vystavena větve cíl vkládání zmírnění podpory z Azure Hypervisor do virtuálních počítačů Azure nebo Azure Cloud Services. To znamená, že virtuální počítače nebudou o existenci mikrokódu a nemůžete použít jeho sada rozšířená instrukcí. To neznamená, že je Azure bude zranitelný vůči útokům kanál straně spekulativní provádění cross-VM.
 
Jak jsme pracovat s partnery v tomto oboru může dostupné další aktualizace.

## <a name="next-steps"></a>Další postup

Další informace najdete v tématu [zákazníky, kteří zabezpečení Azure z procesoru ohrožení zabezpečení](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).