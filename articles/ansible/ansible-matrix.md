---
title: Matice Ansible modul a verze pro Azure.
description: Matice Ansible modul a verze pro Azure.
ms.service: ansible
keywords: ansible, role, matice, verze, azure, devops
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/19/2018
ms.topic: article
ms.openlocfilehash: da5d1a8277d87a771b080ef9cefb3b40448d1563
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="ansible-module-and-version-matrix"></a>Matice Ansible modul a verze

## <a name="ansible-modules-for-azure"></a>Moduly Ansible pro Azure.
Ansible se dodává s číslem modulů, které mohou být provedeny přímo na vzdáleným hostitelům nebo prostřednictvím playbooks.
V tomto článku jsou uvedené moduly Ansible pro Azure, který můžete zřizovat Azure cloudové prostředky, jako je například virtuální počítač, sítě a služby kontejneru. Tyto moduly můžete získat z oficiálního vydání Ansible nebo z následujících rolí playbook publikovaný microsoftem.

| Modul Ansible pro Azure.                   |  Ansible 2.4 |  Playbook Role [azure_module](#introduction-to-azuremodule) |  Playbook Role [azure_preview_module](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|-----------------------------|-------------------------------------| 
| **Compute**                    |           |                          |                                  | 
| azure_rm_availabilityset                    | Ano          | Ano                         | Ano                                 | 
| azure_rm_availabilityset_facts              | Ano          | Ano                         | Ano                                 | 
| azure_rm_deployment                         | Ano          | Ano                         | Ano                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Ano          | Ano                         | Ano                                 | 
| azure_rm_virtualmachineimage_facts          | Ano          | Ano                         | Ano                                 | 
| azure_rm_resourcegroup                      | Ano          | Ano                         | Ano                                 | 
| azure_rm_resourcegroup_facts                | Ano          | Ano                         | Ano                                 | 
| azure_rm_virtualmachine                     | Ano          | Ano                         | Ano                                 | 
| azure_rm_virtualmachine_extension           | Ano          | Ano                         | Ano                                 | 
| azure_rm_virtualmachine_scaleset            | Ano          | Ano                         | Ano                                 | 
| **Sítě**                    |           |                          |                                  | 
| azure_rm_virtualnetwork                     | Ano          | Ano                         | Ano                                 | 
| azure_rm_virtualnetwork_facts               | Ano          | Ano                         | Ano                                 | 
| azure_rm_subnet                             | Ano          | Ano                         | Ano                                 | 
| azure_rm_networkinterface                   | Ano          | Ano                         | Ano                                 | 
| azure_rm_networkinterface_facts             | Ano          | Ano                         | Ano                                 | 
| azure_rm_publicipaddress                    | Ano          | Ano                         | Ano                                 | 
| azure_rm_publicipaddress_facts              | Ano          | Ano                         | Ano                                 | 
| azure_rm_dnsrecordset                       | Ano          | Ano                         | Ano                                 | 
| azure_rm_dnsrecordset_facts                 | Ano          | Ano                         | Ano                                 | 
| azure_rm_dnszone                            | Ano          | Ano                         | Ano                                 | 
| azure_rm_dnszone_facts                      | Ano          | Ano                         | Ano                                 | 
| **Úložiště**                    |           |                          |                                  | 
| azure_rm_loadbalancer                       | Ano          | Ano                         | Ano                                 | 
| azure_rm_loadbalancer_facts                 | Ano          | Ano                         | Ano                                 | 
| azure_rm_applicationgateway                 | -            | Ano                         |                                     | 
| azure_rm_applicationgateway_facts           | -            | -                           | Ano                                 | 
| azure_rm_securitygroup                      | -            | -                           | Ano                                 | 
| azure_rm_securitygroup_facts                | -            | -                           | Ano                                 | 
| azure_rm_storageaccount                     | Ano          | Ano                         | Ano                                 | 
| azure_rm_storageaccount_facts               | Ano          | Ano                         | Ano                                 | 
| azure_rm_storageblob                        | Ano          | Ano                         | Ano                                 | 
| azure_rm_managed_disk                       | Ano          | Ano                         | Ano                                 | 
| azure_rm_managed_disk_facts                 | Ano          | Ano                         | Ano                                 | 
| **Kontejnery**                    |           |                          |                                  | 
| azure_rm_acs                                | Ano          | Ano                         | Ano                                 | 
| azure_rm_containerinstance                  | -            | Ano                        |                                     | 
| azure_rm_containerinstance_facts            | -            | -                           | Ano                                 | 
| azure_rm_containerregistry                  | -            | Ano                         | Ano                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | Ano                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | Ano                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | Ano                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | Ano                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | Ano                                 | 
| **Azure Functions**                    |           |                          |                                  | 
| azure_rm_functionapp                        | Ano          | Ano                         | Ano                                 | 
| azure_rm_functionapp_facts                  | Ano          | Ano                         | Ano                                 | 
| **Databáze**                    |           |                          |                                  | 
| azure_rm_sqlserver                          | -            | Ano                         | Ano                                 | 
| azure_rm_sqlserver_facts                    | -            | -                           | Ano                                 | 
| azure_rm_sqldatabase                        | -            | -                           | Ano                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | Ano                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | Ano                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | Ano                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | Ano                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | Ano                                 | 
| azure_rm_mysqlserver                        | -            | Ano                         | Ano                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | Ano                                 | 
| azure_rm_mysqldatabase                      | -            | -                           | Ano                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | Ano                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | Ano                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | Ano                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | Ano                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | Ano                                 | 
| azure_rm_postgresqlserver                   | -            | Ano                         | Ano                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | Ano                                 | 
| azure_rm_postgresqldatabase                 | -            | -                           | Ano                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | Ano                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | Ano                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | Ano                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | Ano                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | Ano                                 | 

## <a name="introduction-to-azuremodule"></a>Úvod do azure_module
[Azure_module playbook role](https://galaxy.ansible.com/Azure/azure_modules/) zahrnuje nejnovější změny a opravy chyb pro Azure moduly z [devel větev úložiště Ansible](https://github.com/ansible/ansible/tree/devel). Pokud nemůžete počkat Ansible na další vydání, instalace azure_module role je vhodné použít.

Azure_module playbook role vydání každé tři týdny.

## <a name="introduction-to-azurepreviewmodule"></a>Úvod do azure_preview_module
[Azure_preview_module playbook role](https://galaxy.ansible.com/Azure/azure_preview_modules/) je nejúplnější role a obsahuje nejnovější Azure modulů. Aktualizace a opravy chyb hotovi včas více než oficiální Ansible verze. Pokud používáte Ansible pro účely zřizování prostředků Azure, můžete se doporučujeme nainstalovat roli azure_preview_module.

Azure_preview_module playbook role vydání každé tři týdny.

## <a name="next-steps"></a>Další postup
Další informace související se scénářem rolí, najdete na [vytváření opakovaně použitelného Playbooks](http://docs.ansible.com/ansible/latest/playbooks_reuse.html). 