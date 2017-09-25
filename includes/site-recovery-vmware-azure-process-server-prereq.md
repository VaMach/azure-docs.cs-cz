V tomto článku se předpokládá následující:

1. Už bylo navázáno připojení **S2S VPN** nebo **Express Route** mezi vaší místní sítí a Azure Virtual Network.
2. Váš uživatelský účet má oprávnění k vytvoření nového virtuálního počítače v rámci předplatného Azure, do kterého bylo provedeno převzetí služeb při selhání virtuálních počítačů.
3. Vaše předplatné má minimálně 4 jádra, která jsou k dispozici pro rozběh nového virtuálního počítače procesového serveru.
4. Máte k dispozici **heslo konfiguračního serveru**.

> [!TIP]
> Ujistěte se, že se můžete připojit na port 443 konfiguračního serveru (spuštěného místně) z Azure Virtual Network, kam bylo provedeno převzetí služeb při selhání virtuálních počítačů.
