---
title: 'Zelf studie: een privécloud van Azure VMware-oplossing maken en implementeren'
description: Meer informatie over het maken en implementeren van een Azure VMware-oplossing privécloud
ms.topic: tutorial
ms.date: 11/19/2020
ms.openlocfilehash: c8383e987e13e43ea9bc9ba5be196538a259aa8c
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2021
ms.locfileid: "100653126"
---
# <a name="tutorial-create-an-azure-vmware-solution-private-cloud"></a>Zelfstudie: Een Azure VMware Solution-privécloud maken

In deze zelf studie leert u hoe u een privécloud van Azure VMware-oplossingen maakt en implementeert. De minimale initiële implementatie van hosts is drie. Extra hosts kunnen een voor een worden toegevoegd, maximaal 16 hosts per cluster. 

Met Azure VMware Solution kunt u bij het starten uw privécloud niet met uw on-premises vCenter beheren. Daarom moet u aanvullende configuratie uitvoeren. Deze procedures en gerelateerde vereisten worden behandeld in deze zelfstudie.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure VMware Solution-privécloud maken
> * Controleer of de privécloud is geïmplementeerd

## <a name="prerequisites"></a>Vereisten

- Een Azure-account met een actief abonnement. [Gratis een account maken](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- De juiste beheerdersrechten en machtigingen voor het maken van een privécloud. U moet mini maal een bijdrager niveau in het abonnement hebben.
- Volg de informatie die u hebt verzameld in het [plannings](production-ready-deployment-steps.md) artikel om Azure VMware-oplossing te implementeren.
- Zorg ervoor dat de juiste netwerken zijn geconfigureerd zoals beschreven in [ Zelfstudie: Controlelijst voor netwerken](tutorial-network-checklist.md).
- Er zijn hosts ingericht en de resource provider micro soft. AVS is geregistreerd zoals beschreven in [de aanvragen hosts en de resource provider micro soft. AVS in te scha kelen](enable-azure-vmware-solution.md).

## <a name="create-a-private-cloud"></a>Een privécloud maken

U kunt een Azure VMware Solution-privécloud maken met behulp van [Azure Portal](#azure-portal) of door de [Azure CLI](#azure-cli) te gebruiken.

### <a name="azure-portal"></a>Azure Portal

[!INCLUDE [create-avs-private-cloud-azure-portal](includes/create-private-cloud-azure-portal-steps.md)]

### <a name="azure-cli"></a>Azure CLI

U kunt ook een Azure VMware Solution-privécloud maken met de Azure CLI met behulp van de Azure Cloud Shell in plaats van met Azure Portal.  Zie [Azure VMware-opdrachten](/cli/azure/ext/vmware/vmware) voor een lijst met opdrachten die u kunt gebruiken met Azure VMware Solution.

#### <a name="open-azure-cloud-shell"></a>Azure Cloud Shell openen

Selecteer **Nu proberen** in de rechterbovenhoek van een codeblok. U kunt Cloud Shell ook openen in een afzonderlijk browsertabblad door naar [https://shell.azure.com/bash](https://shell.azure.com/bash) te gaan. Klik op **Kopiëren** om de codeblokken te kopiëren, plak deze in Cloud Shell en druk vervolgens op **Enter** om de code uit te voeren.

#### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht `[az group create](/cli/azure/group)`. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive

az group create --name myResourceGroup --location eastus
```

#### <a name="create-a-private-cloud"></a>Een privécloud maken

Geef een naam voor de resourcegroep op en een naam voor de privécloud, een locatie en de grootte van het cluster.

| Eigenschap  | Beschrijving  |
| --------- | ------------ |
| **-g** (Naam van resourcegroep)     | De naam van de resourcegroep voor uw privécloud-resources.        |
| **-n** (Naam van privécloud)     | De naam van uw Azure VMware Solution-privécloud.        |
| **--location**     | De locatie die voor uw privécloud wordt gebruikt.         |
| **--cluster-size**     | De grootte van de cluster. De minimumwaarde is 3.         |
| **--network-block**     | Het netwerkblok voor het CIDR IP-adres dat moet worden gebruikt voor uw privécloud. Het adresblok mag geen overlap vertonen met de in andere virtuele netwerken gebruikte adresblokken die zich in uw abonnement en on-premises netwerken bevinden.        |
| **--sku** | De SKU-waarde: AV36 |

```azurecli-interactive
az vmware private-cloud create -g myResourceGroup -n myPrivateCloudName --location eastus --cluster-size 3 --network-block xx.xx.xx.xx/22 --sku AV36
```

## <a name="azure-vmware-commands"></a>Azure VMware-opdrachten

Zie [Azure VMware-opdrachten](/cli/azure/ext/vmware/vmware) voor een lijst met opdrachten die u kunt gebruiken met Azure VMware Solution.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure VMware Solution-privécloud maken
> * Controleer of de privécloud is geïmplementeerd
> * Een Azure VMware Solution-privécloud verwijderen

Ga door naar de volgende zelfstudie voor informatie over het maken van een jumpbox. U gebruikt de jumpbox om verbinding te maken met uw omgeving, zodat u uw privécloud lokaal kunt beheren.


> [!div class="nextstepaction"]
> [Toegang tot een privécloud van Azure VMware Solution](tutorial-access-private-cloud.md)