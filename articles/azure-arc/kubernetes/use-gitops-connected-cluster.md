---
title: Configuraties implementeren met behulp van GitOps in Kubernetes-cluster met Arc (preview)
services: azure-arc
ms.service: azure-arc
ms.date: 02/17/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: GitOps gebruiken voor het configureren van een Azure Arc enabled Kubernetes-cluster (preview)
keywords: GitOps, Kubernetes, K8s, azure, Arc, Azure Kubernetes service, AKS, containers
ms.openlocfilehash: b30ecde0e128a955967638828dcb7bec008205ea
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652476"
---
# <a name="deploy-configurations-using-gitops-on-an-arc-enabled-kubernetes-cluster-preview"></a>Configuraties implementeren met behulp van GitOps op een Kubernetes-cluster met Arc-functionaliteit (preview)

In dit artikel wordt beschreven hoe u configuraties toepast op een Azure Arc enabled Kubernetes-cluster. Zie het [artikel configuraties en GitOps-Azure-Arc Kubernetes](./conceptual-configurations.md)voor een conceptueel overzicht van dit proces.

## <a name="before-you-begin"></a>Voordat u begint

* Controleer of er een bestaand Kubernetes-verbonden Azure-Arc-cluster is ingeschakeld. Als u een verbonden cluster nodig hebt, gaat u naar de [Snelstartgids een Azure-Kubernetes-cluster verbinden](./connect-cluster.md).

* Controleer de [configuraties en GitOps met Arc for Kubernetes-artikel](./conceptual-configurations.md) om inzicht te krijgen in de voor delen en architectuur van deze functie.

## <a name="create-a-configuration"></a>Een configuratie maken

De [voor beeld-opslag plaats](https://github.com/Azure/arc-k8s-demo) die in dit artikel wordt gebruikt, is gestructureerd rond de persoon van een cluster operator die een paar naam ruimten wil inrichten, een gemeen schappelijke werk belasting moet implementeren en een bepaalde specifieke configuratie voor het team moet bieden. Met deze opslag plaats worden de volgende resources op het cluster gemaakt:

* **Naam ruimten:** `cluster-config` , `team-a` , `team-b`
* **Implementatie:**`cluster-config/azure-vote`
* **ConfigMap:**`team-a/endpoints`

De `config-agent` polls van Azure voor New of Updated `sourceControlConfiguration` . Het duurt Maxi maal 30 seconden voordat deze taak is uitgevoerd.

Als u een persoonlijke opslag plaats koppelt aan de `sourceControlConfiguration` , voltooit u de stappen in [configuratie Toep assen vanuit een persoonlijke Git-opslag plaats](#apply-configuration-from-a-private-git-repository).

### <a name="using-azure-cli"></a>Azure CLI gebruiken

Gebruik de Azure CLI-extensie om `k8sconfiguration` een verbonden cluster te koppelen aan het [voor beeld van de Git-opslag plaats](https://github.com/Azure/arc-k8s-demo). 
1. Geef deze configuratie een naam `cluster-config` .
1. Geef de agent de opdracht om de operator in de `cluster-config` naam ruimte te implementeren.
1. Geef de operator `cluster-admin` machtigingen.

```azurecli
az k8sconfiguration create --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name cluster-config --operator-namespace cluster-config --repository-url https://github.com/Azure/arc-k8s-demo --scope cluster --cluster-type connectedClusters
```

**Uitvoer**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Pending",
    "lastConfigApplied": "0001-01-01T00:00:00",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": null,
  "id": "/subscriptions/<sub id>/resourceGroups/<group name>/providers/Microsoft.Kubernetes/connectedClusters/<cluster name>/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-k8s-demo",
  "resourceGroup": "MyRG",
  "sshKnownHostsContents": "",
  "systemData": {
    "createdAt": "2020-11-24T21:22:01.542801+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-11-24T21:22:01.542801+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
  },
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
  ```

#### <a name="use-a-public-git-repository"></a>Een open bare Git-opslag plaats gebruiken

| Parameter | Indeling |
| ------------- | ------------- |
| `--repository-url` | http [s]://Server/repo [. git] of git://Server/repo [. git]

#### <a name="use-a-private-git-repository-with-ssh-and-flux-created-keys"></a>Een persoonlijke Git-opslag plaats met SSH-en met stromen gemaakte sleutels gebruiken

| Parameter | Indeling | Notities
| ------------- | ------------- | ------------- |
| `--repository-url` | ssh://user@server/repo[. git] of user@server:repo [. git] | `git@` vervangen `user@`

> [!NOTE]
> De open bare sleutel die wordt gegenereerd door stroom moet worden toegevoegd aan het gebruikers account in uw Git-service provider. Als de sleutel wordt toegevoegd aan de opslag plaats in plaats van het gebruikers account, gebruikt u `git@` in plaats van `user@` in de URL. Ga naar de sectie [configuratie Toep assen vanuit een persoonlijke Git-opslag plaats](#apply-configuration-from-a-private-git-repository) voor meer informatie.

#### <a name="use-a-private-git-repository-with-ssh-and-user-provided-keys"></a>Een persoonlijke Git-opslag plaats gebruiken met SSH en door de gebruiker verschafte sleutels

| Parameter | Indeling | Notities |
| ------------- | ------------- | ------------- |
| `--repository-url`  | ssh://user@server/repo[. git] of user@server:repo [. git] | `git@` vervangen `user@` |
| `--ssh-private-key` | met base64 gecodeerde sleutel in de [PEM-indeling](https://aka.ms/PEMformat) | Sleutel rechtstreeks opgeven |
| `--ssh-private-key-file` | volledig pad naar lokaal bestand | Geef het volledige pad naar het lokale bestand dat de PEM-indelings sleutel bevat

> [!NOTE]
> Geef rechtstreeks of in een bestand uw eigen persoonlijke sleutel op. De sleutel moet de [indeling PEM](https://aka.ms/PEMformat) hebben en eindigen op een nieuwe regel (\n).  De bijbehorende open bare sleutel moet worden toegevoegd aan het gebruikers account in uw Git-service provider. Als de sleutel wordt toegevoegd aan de opslag plaats in plaats van het gebruikers account, gebruikt u `git@` in plaats van `user@` . Ga naar de sectie [configuratie Toep assen vanuit een persoonlijke Git-opslag plaats](#apply-configuration-from-a-private-git-repository) voor meer informatie.

#### <a name="use-a-private-git-host-with-ssh-and-user-provided-known-hosts"></a>Een privé Git-host gebruiken met SSH en door de gebruiker verschafte bekende hosts

| Parameter | Indeling | Notities |
| ------------- | ------------- | ------------- |
| `--repository-url`  | ssh://user@server/repo[. git] of user@server:repo [. git] | `git@` vervangen `user@` |
| `--ssh-known-hosts` | Base64-gecodeerd | Direct bekende hosts-inhoud leveren |
| `--ssh-known-hosts-file` | volledig pad naar lokaal bestand | Bekende hosts-inhoud in een lokaal bestand opgeven |

> [!NOTE]
> Als u de Git-opslag plaats wilt verifiëren voordat u de SSH-verbinding tot stand brengt, houdt de stroom operator een lijst met algemene Git-hosts in het bekende hosts-bestand bij. Als u een ongebruikelijke Git-opslag plaats of uw eigen Git-host gebruikt, moet u mogelijk de host-sleutel opgeven om ervoor te zorgen dat stroom uw opslag plaats kan identificeren. U kunt uw known_hosts inhoud rechtstreeks of in een bestand opgeven. Gebruik de [indelings specificaties](https://aka.ms/KnownHostsFormat) voor de known_hosts-inhoud in combi natie met een van de hierboven beschreven SSH-sleutel scenario's voor het leveren van uw eigen inhoud.

#### <a name="use-a-private-git-repository-with-https"></a>Een persoonlijke Git-opslag plaats met HTTPS gebruiken

| Parameter | Indeling | Notities |
| ------------- | ------------- | ------------- |
| `--repository-url` | https://server/repo[. git] | HTTPS met basis verificatie |
| `--https-user` | RAW of Base64-gecodeerd | HTTPS-gebruikers naam |
| `--https-key` | RAW of Base64-gecodeerd | HTTPS persoonlijk toegangs token of wacht woord

> [!NOTE]
> De persoonlijke verificatie van de HTTPS helm-versie wordt alleen ondersteund met de helm-operator grafiek versie 1.2.0 + (standaard).
> De HTTPS-verificatie van de helm-versie wordt momenteel niet ondersteund voor door Azure Kubernetes Services beheerde clusters.
> Als u stroom nodig hebt om toegang te krijgen tot de Git-opslag plaats via uw proxy, moet u de Azure Arc-agents bijwerken met de proxy instellingen. Zie [verbinding maken met een uitgaande proxy server](./connect-cluster.md#connect-using-an-outbound-proxy-server)voor meer informatie.

#### <a name="additional-parameters"></a>Aanvullende para meters

Pas de configuratie aan met de volgende optionele para meters:

| Parameter | Beschrijving |
| ------------- | ------------- |
| `--enable-helm-operator`| Schakel deze optie in om ondersteuning voor helm-grafiek implementaties in te scha kelen. |
| `--helm-operator-params` | Grafiek waarden voor de operator helm (indien ingeschakeld). Bijvoorbeeld `--set helm.versions=v3`. |
| `--helm-operator-version` | Grafiek versie voor de helm-operator (indien ingeschakeld). Gebruik versie 1.2.0 +. Standaard: ' 1.2.0 '. |
| `--operator-namespace` | Naam voor de operator naam ruimte. Standaard: standaard. Max.: 23 tekens. |
| `--operator-params` | Para meters voor operator. Moet binnen enkele aanhalings tekens worden opgegeven. Bijvoorbeeld: ```--operator-params='--git-readonly --sync-garbage-collection --git-branch=main' ``` 

##### <a name="options-supported-in----operator-params"></a>Opties die worden ondersteund in  `--operator-params` :

| Optie | Beschrijving |
| ------------- | ------------- |
| `--git-branch`  | Vertakking van de Git-opslag plaats die moet worden gebruikt voor Kubernetes-manifesten. De standaard waarde is ' Master '. Nieuwere opslag plaatsen hebben een hoofd vertakking `main` met de naam, in welk geval u moet instellen `--git-branch=main` . |
| `--git-path`  | Relatief pad binnen de Git-opslag plaats voor stroom om Kubernetes-manifesten te zoeken. |
| `--git-readonly` | Git-opslag plaats wordt als alleen-lezen beschouwd. Er wordt geen poging gedaan om te schrijven naar de stroom. |
| `--manifest-generation`  | Als deze functie is ingeschakeld, zoekt stroom. stroom. yaml en voert u Kustomize of andere manifest generatoren uit. |
| `--git-poll-interval`  | Periode waarbij de Git-opslag plaats wordt gecontroleerd op nieuwe door voeringen. De standaard waarde is `5m` (5 minuten). |
| `--sync-garbage-collection`  | Als deze functie is ingeschakeld, worden door stroom gemaakte resources verwijderd, maar zijn ze niet meer aanwezig in Git. |
| `--git-label`  | Label om de voortgang van de synchronisatie bij te houden. Wordt gebruikt om de Git-vertakking te labelen.  De standaardinstelling is `flux-sync`. |
| `--git-user`  | Gebruikers naam voor git-door voer. |
| `--git-email`  | E-mail die moet worden gebruikt voor git-door voer. 

Als u niet wilt dat stroom naar de opslag plaats schrijft en `--git-user` of `--git-email` niet is ingesteld, `--git-readonly` wordt automatisch ingesteld.

Zie de documentatie van de [stroom](https://aka.ms/FluxcdReadme)voor meer informatie.

> [!TIP]
> U kunt een `sourceControlConfiguration` in het Azure portal maken op het tabblad **GitOps** van de Kubernetes-resource van Azure Arc ingeschakeld.

## <a name="validate-the-sourcecontrolconfiguration"></a>De sourceControlConfiguration valideren

Gebruik de Azure CLI om te controleren of de `sourceControlConfiguration` is gemaakt.

```azurecli
az k8sconfiguration show --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

De `sourceControlConfiguration` resource wordt bijgewerkt met de nalevings status, berichten en informatie over fout opsporing.

**Uitvoer**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2020-12-10T18:26:52.801000+00:00",
    "message": "...",
    "messageLevel": "Information"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": {
    "chartValues": "",
    "chartVersion": ""
  },
  "id": "/subscriptions/<sub id>/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "...",
  "repositoryUrl": "git://github.com/Azure/arc-k8s-demo.git",
  "resourceGroup": "AzureArcTest",
  "sshKnownHostsContents": null,
  "systemData": {
    "createdAt": "2020-12-01T03:58:56.175674+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-12-10T18:30:56.881219+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
},
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

Wanneer een `sourceControlConfiguration` wordt gemaakt of bijgewerkt, vinden er een aantal dingen plaats op de schermen:

1. De Azure-Arc `config-agent` bewaakt Azure Resource Manager voor nieuwe of bijgewerkte configuraties ( `Microsoft.KubernetesConfiguration/sourceControlConfigurations` ) en kennisgevingen de nieuwe `Pending` configuratie.
1. De `config-agent` configuratie-eigenschappen worden gelezen en de doel naam ruimte wordt gemaakt.
1. De Azure-Arc `controller-manager` bereidt een Kubernetes-service account voor met de juiste machtiging ( `cluster` of `namespace` bereik) en implementeert vervolgens een exemplaar van `flux` .
1. Als u de optie SSH gebruikt met door stromen gegenereerde sleutels, `flux` genereert een SSH-sleutel en registreert de open bare sleutel.
1. De `config-agent` status rapporteert terug naar de `sourceControlConfiguration` resource in Azure.

Terwijl het inrichtings proces plaatsvindt, `sourceControlConfiguration` wordt de status van een aantal wijzigingen gewijzigd. Bewaak de voortgang met de `az k8sconfiguration show ...` bovenstaande opdracht:

| Wijziging fase | Description |
| ------------- | ------------- |
| `complianceStatus`-> `Pending` | Hiermee worden de initiële en in uitvoering zijnde statussen aangegeven. |
| `complianceStatus` -> `Installed`  | `config-agent` het cluster kan worden geconfigureerd en `flux` zonder fouten worden geïmplementeerd. |
| `complianceStatus` -> `Failed` | `config-agent` Er is een fout opgetreden bij het implementeren `flux` ; de gegevens moeten beschikbaar zijn in de `complianceStatus.message` antwoord tekst. |

## <a name="apply-configuration-from-a-private-git-repository"></a>Configuratie Toep assen vanuit een persoonlijke Git-opslag plaats

Als u een privé Git-opslag plaats gebruikt, moet u de open bare SSH-sleutel configureren in uw opslag plaats. De open bare SSH-sleutel is het certificaat dat door stroom wordt gegenereerd of het het account dat u opgeeft. U kunt de open bare sleutel configureren in de specifieke Git-opslag plaats of op de Git-gebruiker die toegang heeft tot de opslag plaats. 

### <a name="get-your-own-public-key"></a>Uw eigen open bare sleutel ophalen

Als u uw eigen SSH-sleutels hebt gegenereerd, beschikt u al over de persoonlijke en open bare sleutels.

#### <a name="get-the-public-key-using-azure-cli"></a>De open bare sleutel ophalen met behulp van Azure CLI 

Het volgende is handig als de sleutels door stroom worden gegenereerd.

```console
$ az k8sconfiguration show --resource-group <resource group name> --cluster-name <connected cluster name> --name <configuration name> --cluster-type connectedClusters --query 'repositoryPublicKey' 
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAREDACTED"
```

#### <a name="get-the-public-key-from-the-azure-portal"></a>De open bare sleutel ophalen uit de Azure Portal

Het volgende is handig als de sleutels door stroom worden gegenereerd.

1. Navigeer in het Azure Portal naar de verbonden cluster bron.
2. Selecteer op de pagina Resource ' GitOps ' en Bekijk de lijst met configuraties voor dit cluster.
3. Selecteer de configuratie die gebruikmaakt van de persoonlijke Git-opslag plaats.
4. Kopieer in het context venster dat wordt geopend onder aan het venster de **open bare sleutel van de opslag plaats**.

#### <a name="add-public-key-using-github"></a>Open bare sleutel toevoegen met behulp van GitHub

Gebruik een van de volgende opties:

* Optie 1: de open bare sleutel toevoegen aan uw gebruikers account (is van toepassing op alle opslag plaatsen in uw account):  
    1. Open GitHub en klik op uw profiel pictogram in de rechter bovenhoek van de pagina.
    2. Klik op **Instellingen**.
    3. Klik op **SSH-en GPG-sleutels**.
    4. Klik op **nieuwe SSH-sleutel**.
    5. Geef een titel op.
    6. Plak de open bare sleutel zonder omringende aanhalings tekens.
    7. Klik op **SSH-sleutel toevoegen**.

* Optie 2: Voeg de open bare sleutel als een Deploy-sleutel toe aan de Git-opslag plaats (alleen van toepassing op deze opslag plaats):  
    1. Open GitHub en navigeer naar uw opslag plaats.
    1. Klik op **Instellingen**.
    1. Klik op **sleutels implementeren**.
    1. Klik op **Deploy-sleutel toevoegen**.
    1. Geef een titel op.
    1. Controleer **toegang voor schrijven toestaan**.
    1. Plak de open bare sleutel zonder omringende aanhalings tekens.
    1. Klik op **sleutel toevoegen**.

#### <a name="add-public-key-using-an-azure-devops-repository"></a>Open bare sleutel toevoegen met behulp van een Azure DevOps-opslag plaats

Gebruik de volgende stappen om de sleutel toe te voegen aan uw SSH-sleutels:

1. Klik onder **gebruikers instellingen** in de rechter bovenhoek (naast de profiel afbeelding) op **open bare SSH-sleutels**.
1. Selecteer  **+ nieuwe sleutel**.
1. Geef een naam op.
1. Plak de open bare sleutel zonder omringende aanhalings tekens.
1. Klik op **Add**.

## <a name="validate-the-kubernetes-configuration"></a>De Kubernetes-configuratie valideren

Nadat `config-agent` het exemplaar is geïnstalleerd `flux` , moeten de resources in de Git-opslag plaats naar het cluster gaan. Controleer of de naam ruimten, implementaties en resources zijn gemaakt met de volgende opdracht:

```console
kubectl get ns --show-labels
```

**Uitvoer**

```console
NAME              STATUS   AGE    LABELS
azure-arc         Active   24h    <none>
cluster-config    Active   177m   <none>
default           Active   29h    <none>
itops             Active   177m   fluxcd.io/sync-gc-mark=sha256.9oYk8yEsRwWkR09n8eJCRNafckASgghAsUWgXWEQ9es,name=itops
kube-node-lease   Active   29h    <none>
kube-public       Active   29h    <none>
kube-system       Active   29h    <none>
team-a            Active   177m   fluxcd.io/sync-gc-mark=sha256.CS5boSi8kg_vyxfAeu7Das5harSy1i0gc2fodD7YDqA,name=team-a
team-b            Active   177m   fluxcd.io/sync-gc-mark=sha256.vF36thDIFnDDI2VEttBp5jgdxvEuaLmm7yT_cuA2UEw,name=team-b
```

We kunnen zien dat de,, en `team-a` `team-b` `itops` `cluster-config` naam ruimten zijn gemaakt.

De `flux` operator is geïmplementeerd in de `cluster-config` naam ruimte, zoals is gericht op onze `sourceControlConfig` :

```console
kubectl -n cluster-config get deploy  -o wide
```

**Uitvoer**

```console
NAME             READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES                         SELECTOR
cluster-config   1/1     1            1           3h    flux         docker.io/fluxcd/flux:1.16.0   instanceName=cluster-config,name=flux
memcached        1/1     1            1           3h    memcached    memcached:1.5.15               name=memcached
```

## <a name="further-exploration"></a>Verdere verkenning

U kunt de andere resources die zijn geïmplementeerd als onderdeel van de configuratie opslagplaats verkennen met behulp van:

```console
kubectl -n team-a get cm -o yaml
kubectl -n itops get all
```

## <a name="delete-a-configuration"></a>Een configuratie verwijderen

Een `sourceControlConfiguration` gebruiken van Azure CLI of Azure Portal verwijderen.  Nadat u de opdracht verwijderen initieert, `sourceControlConfiguration` wordt de resource direct verwijderd in Azure. Het volledig verwijderen van de bijbehorende objecten uit het cluster moet binnen 10 minuten plaatsvinden. Als het `sourceControlConfiguration` zich in een mislukte status bevindt, kan het een uur duren voordat de gekoppelde objecten zijn verwijderd.

> [!NOTE]
> Wanneer een `sourceControlConfiguration` met- `namespace` bereik is gemaakt, kunnen gebruikers met `edit` een functie binding in de naam ruimte werk belastingen implementeren op deze naam ruimte. Wanneer `sourceControlConfiguration` het bereik van de naam ruimte wordt verwijderd, blijft de naam ruimte intact en wordt deze niet verwijderd om te voor komen dat deze andere workloads worden opgesplitst. Indien nodig kunt u deze naam ruimte hand matig verwijderen met `kubectl` .  
> Wijzigingen in het cluster die het resultaat zijn van implementaties uit de getraceerde Git-opslag plaats, worden niet verwijderd wanneer het `sourceControlConfiguration` wordt verwijderd.

```azurecli
az k8sconfiguration delete --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

**Uitvoer**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
```

## <a name="next-steps"></a>Volgende stappen

- [Helm gebruiken met broncode beheer configuratie](./use-gitops-with-helm.md)
- [Azure Policy gebruiken om de cluster configuratie te bepalen](./use-azure-policy.md)
