
**Poslední aktualizace dokumentů**: 6 leden 6:30 PM PST.

Poslední zveřejnění [nová třída ohrožení zabezpečení procesoru](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) se označuje jako spekulativní provádění straně kanál útoky má za následek otázky z zákazníky, kteří potřebují další přehlednost.  

Je chráněn infrastrukturu, která používá Azure a izoluje úloh zákazníka od sebe navzájem.  To znamená, spuštěné v Azure se zákazníci navzájem nelze napadnout vaší aplikace pomocí těchto chyb zabezpečení.

## <a name="keeping-your-operating-systems-up-to-date"></a>Průběžná aktualizace operačních systémů

Při aktualizaci operačního systému není potřeba izolace aplikace od ostatních zákazníků spuštěné v Azure spuštěné v Azure, vždycky je osvědčeným postupem zachovat aktuální verze operačního systému. 

V následující nabídky Zde jsou naše doporučené akce k aktualizaci operačního systému: 

<table>
<tr>
<th>Nabídka</th> <th>Doporučená akce </th>
</tr>
<tr>
<td>Azure Cloud Services </td>  <td>Povolit automatickou aktualizaci nebo Ujistěte se, že používáte nejnovější hostovaného operačního systému.</td>
</tr>
<tr>
<td>Virtuální počítače s Linuxem v Azure</td> <td>Nainstalujte aktualizace od poskytovatele operačního systému, pokud je k dispozici. </td>
</tr>
<tr>
<td>Virtuální počítače s Azure Windows </td> <td>Ověřte, že používáte podporovanou antivirový aplikaci před instalací aktualizací operačního systému. Informace o kompatibilitě, obraťte se na dodavatele antivirového softwaru.<p> Nainstalujte [souhrn leden zabezpečení](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002). </p></td>
</tr>
<tr>
<td>Další služby Azure PaaS</td> <td>Je vyžadována pro zákazníky, kteří používají tyto služby žádná akce. Azure automaticky udržuje vaší verze operačního systému aktuálním stavu. </td>
</tr>
</table>

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Další pokyny, pokud používáte nedůvěryhodnými 

Pokud používáte nedůvěryhodnými není vyžadována žádná akce další odběratele. Pokud povolíte kód, který si nejste jisti, (například povolíte jednomu z vašich zákazníků nahrát binární nebo fragmentu, který potom spusťte v cloudu v rámci vaší aplikace), pak by měla být provedena následující kroky.  


### <a name="windows"></a>Windows 
Pokud používáte systém Windows a hostování nedůvěryhodnými, měli byste také povolit funkci Windows seznamu Zastínění virtuální adresy jádra (KVA), který zajišťuje zvýšenou ochranu zranitelná straně kanál spekulativní provádění. Tato funkce je ve výchozím nastavení vypnuté a mohou ovlivnit výkon, pokud je povoleno. Postupujte podle [Windows Server KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) pokyny pro povolení ochrany na serveru. Pokud používáte Azure Cloud Services, ověřte, že používáte WA-hosta-operačního systému-5.15_201801-01 nebo WA-hosta-operačního systému-4.50_201801-01 (dostupné od verze v lednu 10) a povolit registru klíče prostřednictvím úkolu spuštění.


### <a name="linux"></a>Linux
Pokud používáte Linux a hostování nedůvěryhodnými, měli byste také aktualizovat Linux na novější verzi, která implementuje izolace tabulky stránky jádra (KPTI), který odděluje tabulky stránky použít v jádru od těch náležící do prostoru uživatele. Tyto způsoby zmírnění vyžadují aktualizaci operační systém Linux a získáte od svého poskytovatele distribuce, pokud je k dispozici. Poskytovatel operačního systému lze zjistit, jestli jsou ochranu povolit nebo zakázat ve výchozím nastavení.








## <a name="next-steps"></a>Další postup

Další informace najdete v tématu [zákazníky, kteří zabezpečení Azure z procesoru ohrožení zabezpečení](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).