V systému DNS (Domain Name) slouží k vyhledání věcí na Internetu. Například pokud v prohlížeči zadejte adresu nebo kliknutím na odkaz na webové stránce, používá DNS přeložit domény na IP adresu. IP adresa je použít jako adresu, ale není velmi lidského popisný. Například je mnohem jednodušší pamatovat název DNS jako **contoso.com** než je mějte na paměti, jako je například 192.168.1.88 nebo 2001:0:4137:1f67:24a2:3888:9cce:fea3 IP adresu.

Systém DNS je založen na *záznamy*. Zaznamenává přidružit konkrétní *název*, jako například **contoso.com**, IP adresu nebo jiný název DNS. Při aplikaci, například webový prohlížeč, vyhledává název ve službě DNS, najde záznam a používá, ať odkazuje jako na adresu. Pokud je hodnota, které odkazuje na IP adresu, prohlížeč použije tuto hodnotu. Odkazuje na jiný název DNS, aplikace má rozlišení udělat znovu. Nakonec překlad všechny vyprší za IP adresu.

Pokud vytvoříte web Azure, název DNS je automaticky přiřazen k lokalitě. Tento název má formu  **&lt;yoursitename&gt;. azurewebsites.net**. Při přidání webu jako koncový bod Azure Traffic Manager, je pak web přístupné prostřednictvím  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domény.

> [!NOTE]
> Pokud váš web je nakonfigurovaný jako koncový bod Traffic Manager, použijte **. trafficmanager.net** adres při vytváření záznamů DNS.
> 
> Můžete použít pouze záznamy CNAME s nástrojem Traffic Manager
> 
> 

Existují také více typů záznamů, každá má své vlastní funkce a omezení, ale pro weby konfigurované pro jako koncové body Traffic Manager, jsme pouze pro vás k určitému; *CNAME* záznamy.

### <a name="cname-or-alias-record"></a>Záznam CNAME nebo Alias
Záznam CNAME se mapuje *konkrétní* název DNS, jako například **mail.contoso.com** nebo **www.contoso.com**, na jiný název domény (kanonický). V případě weby Azure s využitím Traffic Manager, je název kanonický domény  **&lt;Moje aplikace >. trafficmanager.net** název domény vašeho profilu Traffic Manageru. Po vytvoření CNAME tento alias  **&lt;Moje aplikace >. trafficmanager.net** název domény. Záznam CNAME se přeložit na IP adresu vašeho  **&lt;Moje aplikace >. trafficmanager.net** název domény automaticky, takže pokud se změní IP adresu webu, není nutné provádět žádnou akci.

Jakmile přenos dorazí na Traffic Manager, pak směruje provoz na váš web pomocí metody, který je nakonfigurován pro vyrovnávání zatížení. Toto je pro návštěvníky na svůj web zcela transparentní. Zobrazí jenom vlastní název domény v prohlížeči.

> [!NOTE]
> Některé domény registrátorů Povolit jenom vám umožní mapovat subdomény, když pomocí záznamu CNAME, jako třeba **www.contoso.com**a nejsou hlavní názvy, například **contoso.com**. Další informace o záznamy CNAME, naleznete v dokumentaci vaším registrátorem <a href="http://en.wikipedia.org/wiki/CNAME_record">položku Wikipedia na záznam CNAME</a>, nebo <a href="http://tools.ietf.org/html/rfc1035">IETF názvy domén – implementace a specifikace</a> dokumentu.
> 
> 

