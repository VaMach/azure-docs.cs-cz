## <a name="create-a-ruby-application"></a>Vytvoření Ruby aplikace
Pokyny najdete v tématu [vytvořit Ruby aplikace na platformě Azure](../articles/virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurace aplikace pro použití služby Service Bus
Použití služby Service Bus, stáhnout a použít balíček Azure Ruby, který obsahuje sadu knihoven pohodlí, které komunikují s služby REST úložiště.

### <a name="use-rubygems-to-obtain-the-package"></a>Použití RubyGems získat balíček
1. Pomocí rozhraní příkazového řádku, jako například **prostředí PowerShell** (Windows), **Terminálové** (Mac), nebo **Bash** (Unix).
2. "Gem instalace azure" zadejte v příkazovém okně instalace gem a závislostí.

### <a name="import-the-package"></a>Import balíčku
Na začátek souboru Ruby, ve kterém chcete používat úložiště pomocí svém oblíbeném textovém editoru, přidejte následující:

```ruby
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Nastavení připojení služby Service Bus
Pomocí následujícího kódu nastavit hodnoty oboru názvů a název klíče, klíče, podepisující osoba a hostitele:

```ruby
Azure.configure do |config|
  config.sb_namespace = '<your azure service bus namespace>'
  config.sb_sas_key_name = '<your azure service bus access keyname>'
  config.sb_sas_key = '<your azure service bus access key>'
end
signer = Azure::ServiceBus::Auth::SharedAccessSigner.new
sb_host = "https://#{Azure.sb_namespace}.servicebus.windows.net"
```

Nastavte hodnotu oboru názvů na hodnotu, kterou jste vytvořili místo celého adresy URL. Například použít **"yourexamplenamespace"**, není "yourexamplenamespace.servicebus.windows.net".
