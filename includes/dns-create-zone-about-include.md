K hostování záznamů DNS v určité doméně se používá zóna DNS. Pokud chcete začít hostovat svoji doménu, musíte vytvořit zónu DNS. Každý záznam DNS vytvořený pro určitou doménu se bude nacházet v zóně DNS dané domény.

Třeba doména „contoso.com“ může obsahovat několik záznamů DNS, třeba „mail.contoso.com“ (pro poštovní server) a „www.contoso.com“ (pro webový server).

## <a name="a-namenamesaabout-dns-zone-names"></a><a name="names"></a>O názvech zón DNS
* Název zóny musí být v rámci skupiny prostředků jedinečný a zóna ještě nesmí existovat. Jinak se operace nezdaří.
* V jiné skupině prostředků nebo v rámci jiného předplatného Azure se dá znovu použít stejný název zóny.
* Pokud má víc zón stejný název, každé instanci se přiřadí různé adresy názvového serveru a jenom jedna instance může být delegovaná z nadřazené domény. Další informace najdete v tématu [Delegování domény do DNS Azure](../articles/dns/dns-domain-delegation.md).


<!--HONumber=Nov16_HO2-->


