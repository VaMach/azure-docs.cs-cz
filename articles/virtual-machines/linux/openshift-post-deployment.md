---
title: "OpenShift na úlohy nasazení Azure Post | Microsoft Docs"
description: "Úlohy nasazení OpenShift Post"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 12e6785358f5f412326418b0c64eeaeabdaa3b5f
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="post-deployment-tasks"></a>Úlohy po nasazení

Po nasazení clusteru OpenShift jsou přítomné další položky, které lze konfigurovat. Tento článek se zabývá následující:

- Konfigurovat jednotné přihlašování pomocí Azure Active Directory (AAD)
- Konfigurace monitorování OpenShift OMS
- Konfigurace metriky a protokolování

## <a name="single-sign-on-using-aad"></a>Jednotné přihlašování pomocí AAD

Chcete-li použít AAD pro ověřování, musí být nejprve vytvořený registrace aplikací Azure AD. Tento proces bude zahrnovat dva kroky – vytvoření registrace aplikací a pak nakonfigurujte oprávnění.

### <a name="create-app-registration"></a>Vytvoření aplikace registrace

Budeme používat rozhraní příkazového řádku Azure k vytvoření registrace aplikací a grafické uživatelské rozhraní (portál), chcete-li nastavit oprávnění. Pokud chcete vytvořit registrace aplikací, bude potřeba pět údaje.

- Zobrazovaný název: Název registrace aplikace (např: OCPAzureAD)
- Domovská stránka: Adresa URL konzoly OpenShift (např: https://masterdns343khhde.westus.cloudapp.azure.com:8443 nebo konzola)
- Identifikátor URI: Adresa URL konzoly OpenShift (např: https://masterdns343khhde.westus.cloudapp.azure.com:8443 nebo konzola)
- Adresa URL odpovědi: Hlavní veřejnou adresu URL a název registraci aplikace (např: https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD)
- Heslo: Zabezpečené heslo (použijte silné heslo)

Následující příklad vytvoří registraci aplikací pomocí informací z výše.

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com:8443/console --password {Strong Password}
```

Pokud je příkaz úspěšný, zobrazí se výstup JSON podobně jako:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com:8443/oauth2callback/OCPAzureAD"
  ]
}
```

Poznamenejte si vlastnost appId vrácená z příkazu pro později.

V **portál Azure**:

1.  Vyberte **Azure Active Directory** --> **registrace aplikací**
2.  Vyhledejte registrace aplikace (např: OCPAzureAD)
3.  Ve výsledcích klikněte na možnost registrace aplikací
4.  V okně nastavení, vyberte **požadovaná oprávnění**
5.  V okně požadovaných oprávnění, klikněte na tlačítko **přidat**

  ![Registrace aplikací](media/openshift-post-deployment/app-registration.png)

6.  Klikněte na krok 1: Vyberte rozhraní API a pak klikněte na tlačítko **Windows Azure Active Directory (Microsoft.Azure.ActiveDirectory)** a klikněte na tlačítko **vyberte** v dolní části

  ![Registrace aplikací vybrané rozhraní API](media/openshift-post-deployment/app-registration-select-api.png)

7.  Na krok 2: Vyberte oprávnění, vyberte **přihlášení a čtení profilu uživatele** pod **delegovaná oprávnění** a klikněte na tlačítko **vyberte**

  ![Registrace aplikace Access](media/openshift-post-deployment/app-registration-access.png)

8.  Klikněte na tlačítko **provést**

### <a name="configure-openshift-for-azure-ad-authentication"></a>Konfigurace OpenShift pro ověřování Azure AD

Ke konfiguraci OpenShift používat Azure AD jako zprostředkovatele ověřování, **/etc/origin/master/master-config.yaml** soubor musí upravovat na všech uzlech hlavní server.

Pomocí následujícího příkazu příkazového řádku najdete Id klienta:

```azurecli
az account show
```

V souboru yaml najdete následující řádky:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com:8443/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Vložte následující řádky ihned po řádcích výše:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Pomocí následujícího příkazu příkazového řádku najdete Id klienta:```az account show```

Restartujte služby OpenShift Master na všech uzlech hlavní

**OpenShift Origin**

```bash
sudo systemctl restart origin-master-api
sudo systemctl restart origin-master-controllers
```

**Platforma OpenShift kontejner s více hlavních serverů**

```bash
sudo systemctl restart atomic-openshift-master-api
sudo systemctl restart atomic-openshift-master-controllers
```

**Platforma kontejneru OpenShift jeden hlavní server**

```bash
sudo systemctl restart atomic-openshift-master
```

V konzole OpenShift se nyní zobrazí dvě možnosti pro ověřování – htpasswd_auth a **[registrace aplikací]**.

## <a name="monitor-openshift-with-oms"></a>Monitorování OpenShift s OMS

Monitorování OpenShift s OMS lze dosáhnout pomocí jedné ze dvou možností – instalace agenta OMS na hostiteli virtuálního počítače nebo kontejner OMS. Tento článek obsahuje pokyny k nasazení kontejneru OMS.

## <a name="create-an-openshift-project-for-oms-and-set-user-access"></a>Vytvoření projektu OpenShift pro OMS a nastavení přístupu uživatele

```bash
oadm new-project omslogging --node-selector='zone=default'
oc project omslogging
oc create serviceaccount omsagent
oadm policy add-cluster-role-to-user cluster-reader system:serviceaccount:omslogging:omsagent
oadm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent
```

## <a name="create-daemon-set-yaml-file"></a>Vytvořit soubor yaml démon sady

Vytvořte soubor s názvem ocp-omsagent.yml.

```yaml
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: oms
spec:
  selector:
    matchLabels:
      name: omsagent
  template:
    metadata:
      labels:
        name: omsagent
        agentVersion: 1.4.0-45
        dockerProviderVersion: 10.0.0-25
    spec:
      nodeSelector:
        zone: default
      serviceAccount: omsagent
      containers:
      - image: "microsoft/oms"
        imagePullPolicy: Always
        name: omsagent
        securityContext:
          privileged: true
        ports:
        - containerPort: 25225
          protocol: TCP
        - containerPort: 25224
          protocol: UDP
        volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        livenessProbe:
          exec:
            command:
              - /bin/bash
              - -c
              - ps -ef | grep omsagent | grep -v "grep"
          initialDelaySeconds: 60
          periodSeconds: 60
      volumes:
      - name: docker-sock
        hostPath:
          path: /var/run/docker.sock
      - name: omsagent-secret
        secret:
         secretName: omsagent-secret
````

## <a name="create-secret-yaml-file"></a>Vytvoření souboru tajný yaml

Chcete-li vytvořit soubor tajný yaml, bude potřeba dva údaje - ID pracovního prostoru OMS a sdílený klíč pracovního prostoru OMS. 

Ukázkový soubor ocp-secret.yml 

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: omsagent-secret
data:
  WSID: wsid_data
  KEY: key_data
```

Nahraďte wsid_data s Base64, pomocí kódování ID pracovního prostoru OMS a nahraďte key_data s Base64, pomocí kódování sdílený klíč pracovního prostoru OMS.

```bash
wsid_data='11111111-abcd-1111-abcd-111111111111'
key_data='My Strong Password'
echo $wsid_data | base64 | tr -d '\n'
echo $key_data | base64 | tr -d '\n'
```

## <a name="create-secret-and-daemon-set"></a>Vytvořit sdílený tajný klíč a démon

Nasazení tajný souboru

```bash
oc create -f ocp-secret.yml
```

Nasazení sady démon agenta OMS

```bash
oc create -f ocp-omsagent.yml
```

## <a name="configure-metrics-and-logging"></a>Konfigurace protokolování a metriky

Šablona OpenShift kontejneru platformy (OCP) Resource Manager poskytuje vstupní parametry pro povolení metrik a protokolování. Nemá šablonu OpenShift kontejneru platformy Marketplace nabízejí a OpenShift původu Resource Manager.

Pokud byla použita šablona OCP Resource Manager a metriky a protokolování nebyly povoleny v průběhu instalace nebo použila nabídku OCP Marketplace, to se dá snadno povolit ve skutečnosti. Pokud pomocí šablony Resource Manageru OpenShift původu, je třeba před práce, kterou.

### <a name="openshift-origin-template-pre-work"></a>Předběžné pracovní šablony OpenShift původu

SSH k prvním uzlu hlavní pomocí portu 2200

Příklad

```bash
ssh -p 2200 clusteradmin@masterdnsixpdkehd3h.eastus.cloudapp.azure.com 
```

Upravit **/etc/ansible/hosts soubor** a přidejte následující řádky po části zprostředkovatele Identity (# Povolit HTPasswdPasswordIdentityProvider)

```yaml
# Setup metrics
openshift_hosted_metrics_deploy=false
openshift_metrics_cassandra_storage_type=dynamic
openshift_metrics_start_cluster=true
openshift_metrics_hawkular_nodeselector={"type":"infra"}
openshift_metrics_cassandra_nodeselector={"type":"infra"}
openshift_metrics_heapster_nodeselector={"type":"infra"}
openshift_hosted_metrics_public_url=https://metrics.$ROUTING/hawkular/metrics

# Setup logging
openshift_hosted_logging_deploy=false
openshift_hosted_logging_storage_kind=dynamic
openshift_logging_fluentd_nodeselector={"logging":"true"}
openshift_logging_es_nodeselector={"type":"infra"}
openshift_logging_kibana_nodeselector={"type":"infra"}
openshift_logging_curator_nodeselector={"type":"infra"}
openshift_master_logging_public_url=https://kibana.$ROUTING
```

Nahraďte řetězec použitý pro $ROUTING **openshift_master_default_subdomain** možnost ve stejné **/etc/ansible/hosts** souboru.

### <a name="azure-cloud-provider-in-use"></a>Zprostředkovatel Azure Cloud používá

Na první hlavní uzel (původ) nebo Bastionu uzlu (OCP), SSH pomocí přihlašovacích údajů během nasazení zadána. Vydejte následující příkaz:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True \
-e openshift_hosted_logging_storage_kind=dynamic
```

### <a name="azure-cloud-provider-not-in-use"></a>Zprostředkovatel Azure Cloud není používán

Na první hlavní uzel (původ) nebo Bastionu uzlu (OCP), SSH pomocí přihlašovacích údajů během nasazení zadána. Vydejte následující příkaz:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-metrics.yml \
-e openshift_metrics_install_metrics=True 

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/byo/openshift-cluster/openshift-logging.yml \
-e openshift_logging_install_logging=True 
```

## <a name="next-steps"></a>Další kroky

- [Začínáme s platformou OpenShift kontejneru](https://docs.openshift.com/container-platform/3.6/getting_started/index.html)
- [Začínáme s OpenShiftem Origin](https://docs.openshift.org/latest/getting_started/index.html)