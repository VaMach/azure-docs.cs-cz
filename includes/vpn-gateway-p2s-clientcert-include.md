Každý klientský počítač, který se připojuje k virtuální síti pomocí připojení Point-to-Site, musí mít nainstalovaný klientský certifikát. Klientský certifikát se generuje z kořenového certifikátu a nainstaluje na každý klientský počítač. Pokud není nainstalovaný platný klientský certifikát a klient se pokusí připojit k virtuální síti, ověření se nezdaří.

Můžete buď vygenerovat jedinečný certifikát pro každého klienta, nebo můžete použít stejný certifikát pro více klientů. Výhodou generování jedinečných certifikátů pro klienty je možnost jednotlivý certifikát odvolat. V opačném případě, pokud více klientů používá stejný klientský certifikát a vy ho potřebujete odvolat, bude nutné vygenerovat a nainstalovat nové certifikáty pro všechny klienty, kteří používají tento certifikát k ověření.

Klientské certifikáty můžete vygenerovat následujícími způsoby:

- **Podnikový certifikát:**

  - Pokud používáte podnikové certifikační řešení, vygenerujte klientský certifikát s běžným názvem ve formátu name@yourdomain.com (namísto formátu název_domény\uživatelské_jméno).
  - Ujistěte se, že je certifikát založený na šabloně uživatelského certifikátu, která má jako první položku v seznamu používání Ověření klienta místo Přihlášení pomocí čipové karty atd. Certifikát můžete zkontrolovat dvojím kliknutím na klientský certifikát a zobrazením *Podrobnosti > Použití rozšířeného klíče*.

- **Kořenový certifikát podepsaný svým držitelem:** Pokud generujete klientský certifikát z kořenového certifikátu podepsaného svým držitelem podle pokynů v článku [Vytvoření kořenového certifikátu podepsaného svým držitelem pro připojení typu Point-to-Site](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientcert), automaticky se nainstaluje na počítač, který jste použili k jeho vygenerování. Pokud chcete klientský certifikát nainstalovat do jiného klientského počítače, musíte ho exportovat. Postupujte podle pokynů v článku týkajícím se [exportování certifikátu](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md#clientexport).