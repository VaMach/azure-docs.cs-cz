[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Je možné používat vlastní interní kořenové certifikační autority infrastruktury veřejných klíčů pro připojení Point-to-Site?

Ano. Dříve bylo možné používat pouze kořenové certifikáty podepsané svým držitelem. I nyní je možné nahrát 20 kořenových certifikátů.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Jaké nástroje můžete použít k vytvoření certifikátů?

Můžete použít řešení infrastruktury veřejných KLÍČŮ organizace (interní infrastruktury veřejných KLÍČŮ), Azure PowerShell, MakeCert a OpenSSL.

### <a name="certsettings"></a>Jsou nějaké pokyny pro nastavení certifikátu a parametry?

* **Interní řešení infrastruktury veřejných KLÍČŮ nebo podnikové infrastruktury veřejných KLÍČŮ:** najdete v části postup [vygenerujete certifikáty](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Azure PowerShell:** najdete v článku [prostředí Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) najdete v článku kroky.

* **MakeCert:** najdete v článku [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) najdete v článku kroky.

* **OpenSSL:** 

    * Při exportu certifikátů, ujistěte se, zda je kořenový certifikát převést do formátu Base64.

    * Pro certifikát klienta:

      * Při vytváření privátního klíče, zadejte délku jako 4096.
      * Při vytváření certifikátu pro *-rozšíření* parametr, zadejte *usr_cert*.