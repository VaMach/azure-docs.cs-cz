## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>Jak APIM Proxy Server odpoví certifikáty SSL v TLS handshake

### <a name="clients-calling-with-sni-header"></a>Klienti s hlavičkou SNI volání
Pokud zákazník má jeden nebo více vlastních domén, které jsou nakonfigurované pro proxy server, APIM může reagovat na požadavky HTTPS z vlastní doménách (například contoso.com) a také výchozí doméně (například apim služby name.azure api.net). Na základě informací v hlavičce indikace názvu serveru (SNI), APIM odpoví certifikát příslušného serveru.

### <a name="clients-calling-without-sni-header"></a>Klienti volání bez hlavičky SNI
Pokud zákazník používá klient, který neodesílá [SNI](https://tools.ietf.org/html/rfc6066#section-3) záhlaví, APIM vytvoří odpovědi na základě logiky následující:

* Pokud služba má jenom jeden vlastní domény nakonfigurované pro proxy server, výchozí certifikát je certifikát, který byl vydán pro vlastní domény proxy serveru.
* Pokud službu pro proxy server nakonfigurován více vlastních domén (podporováno pouze v **Premium** vrstvy), Zákazník můžete určit, který certifikát by měl být výchozí certifikát. Chcete-li nastavit výchozí certifikát [defaultSslBinding](https://docs.microsoft.com/rest/api/apimanagement/apimanagementservice/createorupdate#hostnameconfiguration) musí být vlastnost nastavena na hodnotu true ("defaultSslBinding": "PRAVDA"). Pokud zákazník není nastavena vlastnost, je výchozí certifikát certifikát vydaný pro výchozí Proxy domény hostovanou na *.azure api.net.

## <a name="support-for-putpost-request-with-large-payload"></a>Podpora pro požadavek PUT/POST s velké datové části

APIM Proxy server podporuje žádost s velké datové části při použití klientské certifikáty v protokolu HTTPS (například datové části > 40 KB). Abyste zabránili zmrazení požadavek serveru, můžete nastavit zákazníkům vlastnost ["negotiateClientCertificate": "PRAVDA"](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/CreateOrUpdate#hostnameconfiguration) na název hostitele proxy serveru. Pokud je vlastnost nastavena na hodnotu true, klient certifikát je požadován v době připojení protokolem SSL/TLS, před všechny exchange požadavku HTTP. Vzhledem k tomu, že toto nastavení se vztahuje na **název hostitele Proxy** úrovně, všechny žádosti o připojení požádat o certifikát klienta. Zákazníci mohou nakonfigurovat až 20 vlastní domény pro Proxy (podporováno pouze v **Premium** vrstvy), tak i pracovní obejít toto omezení.

