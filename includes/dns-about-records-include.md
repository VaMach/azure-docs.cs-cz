## Informace o záznamech

Každý záznam DNS má název a typ. Záznamy jsou uspořádány do různých typů podle dat, která obsahují. Nejběžnějším typem je záznam „A“, který mapuje název na adresu IPv4. Dalším typem je záznam „MX“, který mapuje název na poštovní server.

Azure DNS podporuje všechny běžné typy záznamů DNS včetně typů A, AAAA, CNAME, MX, NS, SOA, SRV a TXT. Sady záznamů SOA se vytváří automaticky s každou zónou. Nemůžete je vytvořit samostatně. Všimněte si, že záznamy SPF musí být vytvořené pomocí záznamu typu TXT. Další informace najdete na [této stránce](http://tools.ietf.org/html/rfc7208#section-3.1).

V Azure DNS se záznamy zadávají pomocí relativních názvů. „Plně kvalifikovaný“ název domény (FQDN) obsahuje název zóny, zatímco „relativní“ název ho neobsahuje. Například relativní název záznamu „www“ v zóně „contoso.com“ dává plně kvalifikovaný název záznamu www.contoso.com.

## Informace o sadách záznamů

Někdy můžete potřebovat vytvořit víc než jeden záznam DNS s daným názvem a typem. Předpokládejme například, že web „www.contoso.com“ je hostovaný na dvou různých IP adresách. Tento web vyžaduje dva různé záznamy A, jeden pro každou IP adresu. Tady je příklad sady záznamů:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS spravuje záznamy DNS pomocí sady záznamů. Sada záznamů je kolekcí záznamů DNS v zóně, které mají stejný název a jsou stejného typu. Většina sad záznamů obsahuje jediný záznam, ale příklady jako je ten náš, ve kterém sada záznamů obsahuje víc než jeden záznam, nejsou neobvyklé.

Sady záznamů SOA a CNAME jsou výjimkami. Normy DNS nepovolují více záznamů se stejným názvem pro tyto typy.

Hodnota TTL (Time to Live) určuje, jak dlouho budou klienti každý záznam uchovávat v mezipaměti před tím, než bude záznam znovu dotazován. V tomto příkladu je hodnota TTL 3 600 sekund nebo 1 hodina. Hodnota TTL se zadává pro sadu záznamů (ne pro jednotlivé záznamy), aby všechny záznamy v sadě pracovaly se stejnou hodnotou.

#### Sady záznamů se zástupným znakem

Azure DNS podporuje [záznamy se zástupným znakem](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Ty se vrátí po jakémkoli dotazu s odpovídajícím názvem (pokud nebude nalezena bližší shoda v sadě záznamů bez zástupného znaku). Sady záznamů se zástupnými znaky jsou podporované u všech typů záznamů s výjimkou NS a SOA.  

Pokud chcete vytvořit sadu záznamů se zástupným znakem, použijte název sady záznamů „\*“. Nebo použijte název s popiskem „\*“, například „\*.foo“.

#### Sady záznamů CNAME

Sady záznamů CNAME nemůžou existovat současně s jinými sadami záznamů se stejným názvem. Nemůžete například současně vytvořit sadu záznamů CNAME s relativním názvem „www“ a záznam A s relativním názvem „www“. Protože vrchol zóny (název = „@“) vždy obsahuje sady záznamů NS a SOA, které byly vytvořené při vytvoření zóny, nemůžete na vrcholu zóny vytvořit sadu záznamů CNAME. Tato omezení pocházejí z norem DNS a nepředstavují omezení Azure DNS.



<!--HONumber=Aug16_HO4-->


