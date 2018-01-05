

## <a name="what-is-happening"></a>Co se děje?

Došlo ohrožení zabezpečení napříč celým průmyslem, hardwarové [odhalena na 3](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html). Zachování zákazníkům zabezpečené je vždy naše nejvyšší prioritu a jsme trvá active kroky k zajištění, že žádné Azure zákazníků je vystaven na tyto slabá místa zabezpečení.

S zveřejnit chyby zabezpečení jsme [accelerated načasování plánované údržby](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/) a začal automaticky restartování virtuálních počítačů, které stále potřebné aktualizace.
 
## <a name="how-can-i-see-which-of-my-vms-are-already-updated"></a>Jak můžu zjistit, které z virtuálních počítačů jsou již aktualizovat? 

Zobrazí stav virtuálních počítačů a pokud restartování dokončeny v rámci v [seznamu virtuálních počítačů na portálu Azure](https://aka.ms/T08tdc). Virtuální počítače se objeví jako "Již aktualizace" Pokud aktualizace byla použita, nebo "Plánovanou", pokud je aktualizace stále potřeba. Pokud chcete zobrazit jenom virtuální počítače "Naplánované" odkazovat na vaše [stavu služby Azure](https://portal.azure.com/).

## <a name="can-i-find-out-exactly-when-my-vms-will-be-rebooted"></a>Můžete zjistit přesně při virtuálních počítačů bude restartován?

Nejlepší způsob, jak zobrazit výstrahu o restartování, je konfigurace [naplánované události](https://docs.microsoft.com/azure/virtual-machines/windows/scheduled-events). To poskytuje oznámení 15 minut virtuálního počítače směrem dolů z důvodu údržby – umožňuje volání rozhraní API k virtuálnímu počítači a, kromě toho lze použít záznam protokolu aktivit k aktivační události monitorování Azure k odesílání e-mailů, SMS nebo webhooky. Není možné zajistit žádné dřívější oznámení.

## <a name="can-i-manually-redeploy-now-to-perform-the-required-maintenance"></a>I ručně znovu nasadit teď k provedení požadované údržby? 

Nemůžeme zaručit, že opakovaně nasazeném virtuálním počítači se přidělí aktualizované hostitele. Pokud je to možné, prostředky infrastruktury Azure se pokusí přidělit virtuálních počítačů pro hostitele, které jsou již aktualizován. Je možné, že opětovného nasazení virtuálního počítače může zobrazovat na hostiteli není aktualizován, v takovém případě může podléhat druhý restart, vynutit jako součást plánované údržby. V důsledku toho ruční opětovně nasadí není vhodné používat jako alternativní řešení.

## <a name="how-long-will-the-reboot-take"></a>Jak dlouho bude restartování trvat? 

Většina restartování trvá přibližně **30 minut**.

## <a name="does-the-guest-os-need-to-be-updated"></a>Podporuje hostované operačního systému je nutné aktualizovat? 

Tato aktualizace infrastruktury Azure adresy známé chyby zabezpečení na úrovni hypervisoru a nevyžaduje aktualizaci bitové kopie systému Windows nebo virtuálního počítače s Linuxem. Jako vždy, ale by měly pokračovat použít osvědčené postupy zabezpečení pro vaše Image virtuálních počítačů. Obraťte se na dodavatele vaše operační systémy pro aktualizace a pokyny, podle potřeby. Pokyny pro zákazníky, virtuální počítač Windows serveru, se teď publikoval a zde jsou k dispozici.

## <a name="will-there-be-a-performance-impact-as-a-result-of-resolving-this-update"></a>Bude k dispozici dopad na výkon v důsledku této aktualizace řešení?

Většina zákazníků Azure ještě neviděli znatelný dopad na výkon pomocí této aktualizace. Jsme pracovali k optimalizaci využití procesoru a disku vstupně-výstupní cestu a po použití opravu nejsou zobrazuje znatelný dopad na výkon. Malého zákazníků může zaznamenat některé síťové vlivu na výkon. To lze řešit pomocí Accelerated sítí Azure, pro [Windows](https://docs.microsoft.com/en-us/azure/virtual-network/create-vm-accelerated-networking-powershell) nebo [Linux](https://docs.microsoft.com/en-us/azure/virtual-network/create-vm-accelerated-networking-cli), který je k dispozici všem zákazníkům Azure bezplatné funkce.

## <a name="i-follow-your-recommendations-for-high-availability-will-my-environment-remain-highly-available-during-the-reboot"></a>I podle vaše doporučení pro vysokou dostupnost, bude Moje prostředí stále vysoce dostupný během restartování?

Ano, nastavte virtuálních počítačů nasazených v dostupnosti nebo sady škálování virtuálního počítače mít konstrukce aktualizace domény (UD). Při provádění údržby, Azure ctí UD omezení a nebude restartovat virtuální počítače z různých UD (v rámci stejné skupině dostupnosti). Další informace o vysoké dostupnosti, najdete v části [Správa dostupnosti virtuálních počítačích s Windows v Azure](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) nebo [Správa dostupnosti virtuálních počítačích s Linuxem v Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

## <a name="i-have-architected-my-business-continuitydisaster-recovery-plan-using-region-pairs-will-reboots-to-my-vms-occur-in-region-pairs-at-the-same-time"></a>I vytvořili po Moje obchodní kontinuity nebo zotavení po havárii pomocí párů oblast. Restartování do virtuálních počítačů dojde v oblasti páry ve stejnou dobu?

Za normálních okolností události plánované údržby Azure se nasazuje pro spárované oblasti jeden současně s cílem minimalizovat riziko přerušení v obou oblastí. Ale kvůli naléhavé povaha této aktualizace zabezpečení, jsme se zavedením aktualizace ke všem oblastem současně.

## <a name="what-about-paas-services-on-azure"></a>Co o službách PaaS v Azure?  

Služby platformy Azure včetně webové a mobilní, data služeb IoT, bez serveru, atd. vyřešili ohrožení zabezpečení. Je vyžadována pro zákazníky, kteří používají tyto služby žádná akce.

## <a name="next-steps"></a>Další postup

Další informace najdete v tématu [zákazníky, kteří zabezpečení Azure z procesoru ohrožení zabezpečení](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).