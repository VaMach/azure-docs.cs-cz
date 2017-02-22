UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP adresa použitá k přenosu dat] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parametry:

* /ServerMode: Povinné. Určuje, jestli se má nainstalovat konfigurační i procesový server, nebo jenom procesový server. Vstupní hodnoty: CS, PS.
* InstallLocation: Povinné. Složka, ve které jsou nainstalované komponenty.
* /MySQLCredsFilePath: Povinné. Cesta k souboru, ve kterém jsou uložené přihlašovací údaje serveru MySQL. Soubor by měl být v tomto formátu:
* [MySQLCredentials]
* MySQLRootPassword = "<Password>"
* MySQLUserPassword = "<Password>"
* /VaultCredsFilePath: Povinné. Umístění souboru s přihlašovacími údaji trezoru.
* /EnvType: Povinné. Typ instalace. Hodnoty: VMware, NonVMware.
* /PSIP a /CSIP: Povinné. IP adresa procesového serveru a konfiguračního serveru.
* /PassphraseFilePath: Povinné. Umístění souboru s heslem.
* /BypassProxy: Volitelné. Určuje, že se konfigurační server připojí k Azure bez proxy serveru.
* /ProxySettingsFilePath: Volitelné. Nastavení proxy serveru (výchozí proxy server vyžaduje ověření, nebo vlastní proxy server). Soubor by měl být v tomto formátu:
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "<IP adresa>"
* ProxyPort = "<Port>"
* ProxyUserName="<User Name>"
* ProxyPassword="<Password>"
* DataTransferSecurePort: Volitelné. Číslo portu pro data replikace.
* SkipSpaceCheck: Volitelné. Přeskočí kontrolu místa v mezipaměti.
* AcceptThirdpartyEULA: Povinné. Přijme smlouvy EULA třetích stran.
* ShowThirdpartyEULA: Povinné. Zobrazí smlouvy EULA třetích stran. Pokud je zadán jako vstup, všechny ostatní parametry budou ignorovány.


<!--HONumber=Feb17_HO2-->


