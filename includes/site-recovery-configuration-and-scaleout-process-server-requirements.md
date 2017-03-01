| **Hardware** | |
| --- |---|
| Počet jader procesoru| 8 |
| Paměť RAM| 12 GB|
| Počet disků | 3 <br><br> – Disk s operačním systémem<br> – Disk mezipaměti procesového serveru<br> – Jednotka pro uchování (pro navrácení služeb po obnovení)|
| Volné místo na disku (mezipaměť procesového serveru) | 600 GB
| Volné místo na disku (disk pro uchování) | 600 GB|
| **Software** | |
| Verze operačního systému | Windows Server 2012 R2 |
| Národní prostředí operačního systému | Angličtina (en-us)|
| Verze VMware vSphere PowerCLI | [PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0 "PowerCLI 6.0")|
| Role Windows Serveru | Nepovolujte následující role: <br> – Active Directory Domain Services <br>– Internet Information Service <br> – Hyper-V |
| **Síť** | |
| Typ karty síťového rozhraní | VMXNET3 |
| Typ IP adresy | Statická |
| Přístup k internetu | Server by měl mít přístup k následujícím adresám URL, buď přímo, nebo prostřednictvím proxy serveru: <br> - \*.accesscontrol.windows.net<br> - \*.backup.windowsazure.com <br>- \*.store.core.windows.net<br> - \*.blob.core.windows.net<br> - \*.hypervrecoverymanager.windowsazure.com <br> - https://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi (nevyžaduje se pro procesový server pro horizontální navýšení kapacity) <br> - time.nist.gov <br> - time.windows.com |
| Porty | 443 (orchestrace řídicího kanálu)<br>9443 (přenos dat)|


<!--HONumber=Feb17_HO3-->


