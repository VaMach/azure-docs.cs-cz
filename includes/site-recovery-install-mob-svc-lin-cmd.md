1. Zkopírujte instalační službu do místní složky (například TMP) na serveru, který chcete chránit. V terminálu spusťte následující příkazy:
  ```
  cd /tmp ;

  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. Pro instalaci služby Mobility, spusťte následující příkaz:

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. Po dokončení instalace služby Mobility je potřeba získat zaregistrovaný na konfiguračním serveru. Spusťte následující příkaz pro registraci služba Mobility se konfigurační server.

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a>Nastavení mobilních zařízení služby Instalační program příkazového řádku

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|Parametr|Typ|Popis|Možné hodnoty|
|-|-|-|-|
|-r |Povinné|Určuje, zda by měly být nainstalovány služby Mobility (MS) nebo MasterTarget(MT) by měly být nainstalovány.|MS </br> MT|
|-d |Nepovinné|Umístění, kde bude nainstalován služba Mobility|/usr/local/ASR|
|-v|Povinné|Určuje platformu, na kterém služba Mobility je získávání nainstalovaná </br> </br>-** VMware: tuto hodnotu použijte, pokud k instalaci služby mobility na virtuálním počítači systémem *VMware vSphere hostitelích ESXi*, * hostitelů Hyper-V, a *fyzických serverů* </br> -** Azure: tuto hodnotu použijte, pokud instalujete agenta na virtuálním počítači Azure IaaS| VMware </br> Azure|
|-q|Nepovinné|Určuje, ke spuštění instalačního programu v bezobslužném režimu| neuvedeno|


#### <a name="mobility-service-configuration-command-line"></a>Nastavení mobilních zařízení služby konfigurace příkazového řádku

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|Parametr|Typ|Popis|Možné hodnoty|
|-|-|-|-|
|-i |Povinné|IP konfigurace serveru|Libovolná platná IP adresa|
|-P |Povinné|Úplná cesta k souboru, kde je uloží heslo připojení|Všechny platné složce.|
