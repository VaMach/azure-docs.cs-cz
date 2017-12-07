1. Zkopírujte instalační službu do místní složky (například C:\Temp) na serveru, který chcete chránit. Jako správce na příkazovém řádku spusťte následující příkazy:

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. Pro instalaci služby Mobility, spusťte následující příkaz:

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. Nyní agent musí být registrováno s konfiguračním serverem.

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a>Argumenty příkazového řádku Instalační program služby mobility

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| Parametr|Typ|Popis|Možné hodnoty|
|-|-|-|-|
|/ Role|Povinné|Určuje, zda by měly být nainstalovány služby Mobility (MS) nebo MasterTarget(MT) by měly být nainstalovány.|MS </br> MT –|
|/InstallLocation|Nepovinné|Umístění, kde je nainstalovaná služba Mobility|Libovolná složka v počítači|
|/ Platform|Povinné|Určuje platformu, na kterém služba Mobility je získávání nainstalovaná </br> </br>- **VMware** : tuto hodnotu použijte, pokud k instalaci služby mobility na virtuálním počítači systémem *VMware vSphere hostitelích ESXi*, *hostitelů Hyper-V* a *fyzických serverů* </br> - **Azure** : tuto hodnotu použijte, pokud instalujete agenta na virtuálním počítači Azure IaaS| VMware </br> Azure|
|/ Tichou|Nepovinné|Určuje, spusťte instalační program v bezobslužném režimu| Není k dispozici|

>[!TIP]
> Instalační protokoly naleznete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log

#### <a name="mobility-service-registration-command-line-arguments"></a>Argumenty příkazového řádku registrace služby mobility

```
Usage :
UnifiedAgentConfigurator.exe  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | Parametr|Typ|Popis|Možné hodnoty|
  |-|-|-|-|
  |/ CSEndPoint |Povinné|IP adresa konfiguračního serveru| Všechny platnou IP adresu|
  |/PassphraseFilePath|Povinné|Umístění heslo |Všechny platné UNC nebo místní cesta|


>[!TIP]
> Protokoly AgentConfiguration naleznete v části %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log
