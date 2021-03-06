---
title: Persoonlijke toegang inschakelen met persoonlijke koppeling (preview)-Portal
titleSuffix: Azure Digital Twins
description: Zie persoonlijke toegang inschakelen voor Azure Digital Apparaatdubbels-oplossingen met een persoonlijke koppeling met behulp van de Azure Portal.
author: baanders
ms.author: baanders
ms.date: 1/25/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 2545915edf9e39b63100a2bb16bd34fa6777675c
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2021
ms.locfileid: "100417836"
---
# <a name="enable-private-access-with-private-link-preview-azure-portal"></a>Persoonlijke toegang inschakelen met persoonlijke koppeling (preview): Azure Portal

[!INCLUDE [digital-twins-private-link-selector.md](../../includes/digital-twins-private-link-selector.md)]

In dit artikel worden de verschillende manieren beschreven om [persoonlijke koppelingen in te scha kelen met een persoonlijk eind punt voor een Azure Digital apparaatdubbels-exemplaar](concepts-security.md#private-network-access-with-azure-private-link-preview) (momenteel in preview-versie). Als u een persoonlijk eind punt voor uw Azure Digital Apparaatdubbels-exemplaar configureert, kunt u uw Azure Digital Apparaatdubbels-exemplaar beveiligen en de open bare bloot stelling elimineren, en gegevens exfiltration van uw [Azure-Virtual Network (VNet)](../virtual-network/virtual-networks-overview.md)voor komen.

In dit artikel wordt het proces beschreven met behulp van de [**Azure Portal**](https://portal.azure.com).

Hier volgen de stappen die in dit artikel worden behandeld: 
1. Schakel persoonlijke koppeling in en configureer een persoonlijk eind punt voor een Azure Digital Apparaatdubbels-exemplaar.
1. Schakel toegangs vlaggen voor open bare netwerken in of uit om de API-toegang tot verbindingen van particuliere koppelingen alleen te beperken.

## <a name="prerequisites"></a>Vereisten

Voordat u een persoonlijk eind punt kunt instellen, hebt u een [**Azure-Virtual Network (VNet)**](../virtual-network/virtual-networks-overview.md) nodig waar het eind punt kan worden geïmplementeerd. Als u nog geen VNet hebt, kunt u een van de Virtual Network [Snelstartgids](../virtual-network/quick-create-portal.md) van Azure volgen om dit in te stellen.

## <a name="add-a-private-endpoint-for-an-azure-digital-twins-instance"></a>Een persoonlijk eind punt toevoegen voor een Azure Digital Apparaatdubbels-exemplaar 

Wanneer u de [Azure Portal](https://portal.azure.com)gebruikt, kunt u ervoor kiezen om privé koppeling met een persoonlijk eind punt in te scha kelen voor een Azure Digital apparaatdubbels-exemplaar als onderdeel van de eerste installatie van het exemplaar, of om het later in te scha kelen voor een exemplaar dat al bestaat. 

Een van deze aanmaak methoden biedt dezelfde configuratie opties en hetzelfde eind resultaat voor uw exemplaar. In deze sectie wordt beschreven hoe u dit doet.

>[!TIP]
> U kunt ook een persoonlijk eind punt via de service voor persoonlijke koppelingen instellen, in plaats van via uw Azure Digital Apparaatdubbels-exemplaar. Dit geeft ook dezelfde configuratie opties en hetzelfde eind resultaat.
>
> Zie de documentatie over persoonlijke koppelingen voor de [Azure Portal](../private-link/create-private-endpoint-portal.md), [Azure cli](../private-link/create-private-endpoint-cli.md), [arm](../private-link/create-private-endpoint-template.md)of [Power shell](../private-link/create-private-endpoint-powershell.md)(Engelstalig) voor meer informatie over het instellen van persoonlijke koppelings bronnen.

### <a name="add-a-private-endpoint-during-instance-creation"></a>Een persoonlijk eind punt toevoegen tijdens het maken van een exemplaar

In deze sectie schakelt u privé-koppeling in met een persoonlijk eind punt op een Azure Digital Apparaatdubbels-exemplaar dat momenteel wordt gemaakt. Deze sectie richt zich op de netwerk stap van het aanmaak proces. voor een volledig overzicht van het maken van een nieuw exemplaar van de Azure Digital Apparaatdubbels raadpleegt u [*How to: een exemplaar en verificatie instellen*](how-to-set-up-instance-portal.md).

De opties voor de persoonlijke koppeling bevinden zich op het tabblad **netwerk** van Setup van exemplaar.

Op dit tabblad kunt u privé-eind punten inschakelen door de optie **privé-eind punt** te selecteren voor de **verbindings methode**.

Hiermee voegt u een sectie met de naam **privé-eindpunt verbindingen** toe, waar u de details van uw persoonlijke eind punt kunt configureren. Selecteer de knop **+ toevoegen** om door te gaan.

:::image type="content" source="media/how-to-enable-private-link/create-instance-networking-1.png" alt-text="Scherm afbeelding van de Azure Portal met het tabblad netwerken van het dialoog venster Resource maken voor Azure Digital Apparaatdubbels. Er is een hooglicht rond de naam van het tabblad, de optie voor het privé-eind punt voor de verbindings methode en de knop + toevoegen om een nieuwe verbinding voor een persoonlijk eind punt te maken." lightbox="media/how-to-enable-private-link/create-instance-networking-1.png":::

Hiermee opent u een pagina om de details van een nieuw persoonlijk eind punt in te voeren.

:::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-full.png" alt-text="Scherm opname van de Azure Portal de pagina persoonlijk eind punt maken weer te geven. Deze bevat de velden die hieronder worden beschreven.":::

1. Vul de selecties voor uw **abonnement** en **resource groep** in. Stel de **locatie** in op dezelfde locatie als de VNet die u gaat gebruiken. Kies een **naam** voor het eind punt en selecteer voor **doel-subbronnen** de *API*.

1. Selecteer vervolgens het **virtuele netwerk** en het **subnet** dat u wilt gebruiken om het eind punt te implementeren.

1. Selecteer ten slotte of u wilt **integreren met een privé-DNS-zone**. U kunt de standaard waarde **Ja** of gebruiken voor de Help bij deze optie kunt u de koppeling in de Portal volgen voor [meer informatie over persoonlijke DNS-integratie](../private-link/private-endpoint-overview.md#dns-configuration).

Nadat u de configuratie opties hebt ingevuld, klikt u op **OK** om te volt ooien.

Hiermee keert u terug naar het tabblad **netwerken** van de installatie van het Azure Digital apparaatdubbels-exemplaar, waarbij uw nieuwe eind punt zichtbaar moet zijn onder * * particuliere endpoint-verbindingen.

:::image type="content" source="media/how-to-enable-private-link/create-instance-networking-2.png" alt-text="Scherm afbeelding van de Azure Portal met het tabblad netwerken van het dialoog venster Resource maken voor Azure Digital Apparaatdubbels. Er is een hooglicht rond de nieuwe verbinding voor een persoonlijk eind punt en de navigatie knoppen (bekijken + maken, vorige, volgende: Geavanceerd)." lightbox="media/how-to-enable-private-link/create-instance-networking-2.png":::

U kunt vervolgens de onderste navigatie knoppen gebruiken om door te gaan met de rest van de installatie van het exemplaar.

### <a name="add-a-private-endpoint-to-an-existing-instance"></a>Een persoonlijk eind punt toevoegen aan een bestaand exemplaar

In deze sectie schakelt u privé-koppeling in met een persoonlijk eind punt voor een Azure Digital Apparaatdubbels-exemplaar dat al bestaat.

1. Ga eerst naar de [Azure Portal](https://portal.azure.com) in een browser. Ga naar het Azure Digital Apparaatdubbels-exemplaar en zoek de naam op in de zoek balk van de portal.

1. Selecteer **netwerken (preview)** in het menu aan de linkerkant.

1. Ga naar het tabblad **verbindingen van privé-eind punten** .

1. Selecteer **+ persoonlijk eind punt** om de instelling **een persoonlijk eind punt maken** te openen.

    :::image type="content" source="media/how-to-enable-private-link/add-endpoint-digital-twins.png" alt-text="Scherm afbeelding van de Azure Portal waarin de pagina netwerken (preview) wordt weer gegeven voor een Azure Digital Apparaatdubbels-exemplaar. Het tabblad verbindingen met het persoonlijke eind punt is gemarkeerd en de knop + privé-eind punt is ook gemarkeerd." lightbox="media/how-to-enable-private-link/add-endpoint-digital-twins.png":::

1. Op het tabblad  **basis beginselen**   voert u het **abonnement** en de **resource groep** van uw project in of selecteert u een **naam** en **regio** voor uw eind punt. De regio moet hetzelfde zijn als de regio voor het VNet dat u gebruikt.

    :::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-1.png" alt-text="Scherm afbeelding van de Azure Portal het eerste (basis) tabblad van het dialoog venster een persoonlijk eind punt maken weer gegeven. Deze bevat de velden die hierboven worden beschreven.":::

    Wanneer u klaar bent, selecteert u de knop **volgende: Resource >** om naar het volgende tabblad te gaan.

1. Op het tabblad **resource** voert u de volgende gegevens in of selecteert u deze: 
    * **Verbindings methode**: Selecteer **verbinding maken met een Azure-resource in mijn Directory** om te zoeken naar uw Azure Digital apparaatdubbels-exemplaar.
    * **Abonnement**: Voer uw abonnement in.
    * **Resource type**: Selecteer **micro soft. DigitalTwins/digitalTwinsInstances**
    * **Resource**: Selecteer de naam van uw Azure Digital apparaatdubbels-exemplaar.
    * **Doel-subresource**: Selecteer een **API**.

    :::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-2.png" alt-text="Scherm afbeelding van de Azure Portal het tweede tabblad (resource) van het dialoog venster een persoonlijk eind punt maken weer gegeven. Deze bevat de velden die hierboven worden beschreven.":::

    Wanneer u klaar bent, selecteert u de knop **volgende: configuratie >** om naar het volgende tabblad te gaan.    

1. Voer op het tabblad **configuratie** de volgende gegevens in of Selecteer deze:
    * **Virtueel netwerk**: Selecteer uw virtuele netwerk.
    * **Subnet**: Kies een subnet in het virtuele netwerk.
    * **Integreren met een privé-DNS-zone**: Selecteer of u wilt **integreren met een privé-DNS-zone**. U kunt de standaard waarde **Ja** of gebruiken voor de Help bij deze optie kunt u de koppeling in de Portal volgen voor [meer informatie over persoonlijke DNS-integratie](../private-link/private-endpoint-overview.md#dns-configuration).
    Als u **Ja** selecteert, kunt u de standaard configuratie-informatie laten staan.

    :::image type="content" source="media/how-to-enable-private-link/create-private-endpoint-3.png" alt-text="Scherm afbeelding van de Azure Portal het derde tabblad (configuratie) van het dialoog venster een persoonlijk eind punt maken weer gegeven. Deze bevat de velden die hierboven worden beschreven.":::

    Wanneer u klaar bent, kunt u de knop **beoordeling + maken** selecteren om de installatie te volt ooien. 

1. Controleer uw selecties op het tabblad **controleren en maken** en selecteer de knop  **maken** . 

Wanneer het eind punt is geïmplementeerd, wordt het weer gegeven in de privé-eindpunt verbindingen voor uw Azure Digital Apparaatdubbels-exemplaar.

>[!TIP]
> Het eind punt kan ook worden weer gegeven vanuit het persoonlijke koppelings centrum in de Azure Portal.

## <a name="disable--enable-public-network-access-flags"></a>Toegangs vlaggen voor open bare netwerken uitschakelen/inschakelen

U kunt uw Azure Digital Apparaatdubbels-exemplaar zo configureren dat alle open bare verbindingen worden geweigerd en alleen verbindingen via persoonlijke eind punten toestaan om de netwerk beveiliging te verbeteren. Deze actie wordt uitgevoerd met een **markering voor open bare toegang tot het netwerk**. 

Met dit beleid kunt u de API-toegang beperken tot verbindingen van een particuliere verbinding. Wanneer de vlag voor open bare netwerk toegang is ingesteld op *uitgeschakeld*, retour neren alle rest API naar het gegevens vlak van het Azure Digital apparaatdubbels-exemplaar vanuit de open bare Cloud `403, Unauthorized` . Als het beleid is ingesteld op *uitgeschakeld* en een aanvraag wordt ingediend via een persoonlijk eind punt, wordt de API-aanroep uitgevoerd.

In dit artikel wordt beschreven hoe u de waarde van de netwerk vlag bijwerkt met behulp van de [Azure Portal](https://portal.azure.com). Zie de [CLI-versie](how-to-enable-private-link-cli.md) van dit artikel voor instructies over hoe u dit doet met het opdracht regel programma Azure CLI of ARMClient.

### <a name="use-the-azure-portal"></a>De Azure-portal gebruiken

Als u open bare netwerk toegang wilt in-of uitschakelen in de [Azure Portal](https://portal.azure.com), opent u de portal en navigeert u naar uw Azure Digital apparaatdubbels-exemplaar.

1. Selecteer **netwerken (preview)** in het menu aan de linkerkant.

1. Stel op het tabblad **open bare toegang** **open bare netwerk toegang tot** **uitgeschakeld** of **alle netwerken** in.

    :::row:::
        :::column:::
            :::image type="content" source="media/how-to-enable-private-link/network-flag-portal.png" alt-text="Scherm afbeelding van de Azure Portal waarin de pagina netwerken (preview) wordt weer gegeven voor een Azure Digital Apparaatdubbels-exemplaar. Het tabblad open bare toegang is gemarkeerd en de optie om te kiezen of open bare netwerk toegang ook is gemarkeerd." lightbox="media/how-to-enable-private-link/network-flag-portal.png":::
        :::column-end:::
        :::column:::
        :::column-end:::
    :::row-end:::

    Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over persoonlijke koppelingen voor Azure: 
* [*Wat is Azure Private Link service?*](../private-link/private-link-service-overview.md)