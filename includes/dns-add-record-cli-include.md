#### Vytvoření sady záznamů A s jedním záznamem
K vytvoření sady záznamů použijte příkaz `azure network dns record-set create`. Zadejte skupinu prostředků, název zóny, relativní název sady záznamů, typ záznamu a hodnotu TTL.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Po vytvoření sady záznamů A přidejte k této sadě záznamů adresu IPv4 pomocí příkazu `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### Vytvoření sady záznamů AAAA s jedním záznamem
    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### Vytvoření sady záznamů CNAME s jedním záznamem
Záznamy CNAME povolují jenom jednu řetězcovou hodnotu.

    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### Vytvoření sady záznamů MX s jedním záznamem
V tomto příkladu vytvoříme s využitím názvu sady záznamů „@“ záznam MX ve vrcholu zóny (v tomto „contoso.com“). To je u záznamů MX běžné.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### Vytvoření sady záznamů NS s jedním záznamem
    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### Vytvoření sady záznamů PTR s jedním záznamem
V tomto případě „my-arpa-zone.com“ představuje zónu ARPA představující váš rozsah IP adres.  Každá sada záznamů PTR v této zóně odpovídá IP adrese v rámci tohoto rozsahu IP adres.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### Vytvoření sady záznamů SRV s jedním záznamem
Pokud vytváříte záznam SRV v kořenu zóny, můžete v názvu záznamu uvést „_služba“ a „_protokol“. Do názvu záznamu není potřeba zahrnovat symbol „@“.

    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### Vytvoření sady záznamů TXT s jedním záznamem
    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"


<!--HONumber=Sep16_HO4-->


