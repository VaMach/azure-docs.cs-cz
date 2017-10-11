Dále pokud všechny servery v clusteru se systémem Windows Server 2008 R2 nebo Windows Server 2012, musíte ověřit, že oprava hotfix [KB2854082](http://support.microsoft.com/kb/2854082) je nainstalován na všech místních serverů a virtuálních počítačích Azure, které jsou součástí clusteru. Libovolný server nebo virtuální počítač, který je v clusteru, ale není ve skupině dostupnosti, musí být také tato oprava hotfix nainstalovaná.

V této relaci vzdálené plochy pro každý z uzlů clusteru, stáhněte si [KB2854082](http://support.microsoft.com/kb/2854082) do místního adresáře. Potom nainstalujte opravu hotfix na všech uzlech clusteru postupně. Pokud je Clusterová služba aktuálně běží na uzlu clusteru, na konci instalace opravy hotfix restartování serveru.

> [!WARNING]
> Zastavením Clusterové služby nebo restartováním serveru ovlivňuje stavu kvora clusteru a skupinu dostupnosti a může způsobit clusteru do offline režimu. Aby se zachovala vysokou dostupnost clusteru během instalace, ujistěte se, že:
> 
> * Cluster je ve stavu optimální kvora. 
> * Než nainstalujete opravu hotfix v každém uzlu, jsou všechny uzly clusteru online.
> * Před instalací opravy hotfix do jiného uzlu v clusteru, povolit na na jeden uzel, včetně plně restartování serveru spusťte k dokončení instalace opravy hotfix.
> 
> 

