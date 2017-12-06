# <a name="support-matrix"></a>Matice podpory

| | **Z VMware do Azure** |**Z Hyper-V do Azure**|**Z Azure do Azure**|**Technologie Hyper-V do sekundární lokality**|**Z VMware do sekundární lokality**
--|--|--|--|--|--
Podporované scénáře |Ano|Ano|Ne|Ano*|Ne
Podporovaná verze | vCenter 6.5, 6.0 nebo 5,5| Windows Server 2016, Windows Server 2012 R2 | Není k dispozici |Windows Server 2016, Windows Server 2012 R2|Není k dispozici
Podporované konfigurace|vCenter, ESXi| Cluster Hyper-V, hostiteli Hyper-V|Není k dispozici|Cluster Hyper-V, hostiteli Hyper-V|Není k dispozici|
Počet serverů, které může být profilovaným na kterém je spuštěna instance Planner nasazení Azure Site Recovery |Jeden (virtuální počítače, které patří do jednoho serveru vCenter nebo jeden server ESXi můžete profilovaným najednou)|Více (virtuální počítače napříč více hostitele nebo hostitelské clustery mohou být profil najednou)| Není k dispozici |Více (virtuální počítače napříč více hostitele nebo hostitelské clustery mohou být profil najednou)| Není k dispozici

* Nástroj je určen pro Hyper-V do Azure zotavení po havárii. Pro Hyper-V pro obnovení po havárii sekundární lokality můžete použít, pouze pochopit zdroje straně doporučení jako požadované šířku pásma sítě, požadované volného místa na každém serveru zdroj technologie Hyper-V a počáteční replikace dávkování čísla a batch definice.  Ignorujte Azure doporučení a náklady ze sestavy. Získat propustnosti operace není pro Hyper-V pro scénáře zotavení po havárii sekundární lokality.
