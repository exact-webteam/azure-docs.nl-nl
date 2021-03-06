---
title: Problemen met het indelen van pijp lijnen en triggers in Azure Data Factory oplossen
description: Gebruik verschillende methoden voor het oplossen van problemen met pijp lijn triggers in Azure Data Factory.
author: ssabat
ms.service: data-factory
ms.date: 12/15/2020
ms.topic: troubleshooting
ms.author: susabat
ms.reviewer: susabat
ms.openlocfilehash: 1a5f665627da1b08ec57b04863a58f227c673af4
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944902"
---
# <a name="troubleshoot-pipeline-orchestration-and-triggers-in-azure-data-factory"></a>Problemen met het indelen van pijp lijnen en triggers in Azure Data Factory oplossen

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Een pijplijnuitvoering in Azure Data Factory definieert een exemplaar van een pijplijnuitvoering. Stel bijvoorbeeld dat u een pijp lijn hebt die wordt uitgevoerd om 8:00 uur, 9:00 uur en 10:00 uur. In dit geval zijn er drie afzonderlijke pijplijn uitvoeringen. Elke pijplijnuitvoering heeft een unieke id. Een run-ID is een Globally Unique Identifier (GUID) die de specifieke pijplijn uitvoering definieert.

Pijplijnuitvoeringen worden doorgaans geïnstantieerd doordat argumenten worden doorgegeven aan parameters die u in de pijplijn definieert. U kunt een pijp lijn ofwel hand matig of via een trigger uitvoeren. Zie [pijp lijnen uitvoeren en triggers in azure Data Factory](concepts-pipeline-execution-triggers.md) voor meer informatie.

## <a name="common-issues-causes-and-solutions"></a>Veelvoorkomende problemen, oorzaken en oplossingen

### <a name="an-azure-functions-app-pipeline-throws-an-error-with-private-endpoint-connectivity"></a>Een Azure Functions app-pijp lijn genereert een fout met de connectiviteit van een persoonlijk eind punt
 
U hebt Data Factory en een Azure-functie-app die wordt uitgevoerd op een persoonlijk eind punt. U probeert een pijp lijn uit te voeren die samenwerkt met de functie-app. U hebt drie verschillende methoden geprobeerd, maar er is een fout ' ongeldige aanvraag ' geretourneerd en de andere twee methoden retour neren ' 103-fout verboden '.

**Oorzaak**: Data Factory momenteel geen ondersteuning voor een privé-eindpunt connector voor functie-apps. Azure Functions weigert aanroepen omdat het is geconfigureerd om alleen verbindingen van een persoonlijke koppeling toe te staan.

**Oplossing**: Maak een **PrivateLinkService** -eind punt en geef de DNS van uw functie-app op.

### <a name="a-pipeline-run-is-canceled-but-the-monitor-still-shows-progress-status"></a>Een pijplijn uitvoering wordt geannuleerd, maar de monitor wordt nog steeds de voortgangs status weer gegeven

Wanneer u de uitvoering van een pijp lijn annuleert, wordt in de pipeline-bewaking vaak nog steeds de voortgangs status weer gegeven. Dit gebeurt vanwege een probleem met de browser cache. Mogelijk hebt u ook niet de juiste controle filters.

**Oplossing**: Vernieuw de browser en pas de juiste bewakings filters toe.
 
### <a name="you-see-a-delimitedtextmorecolumnsthandefined-error-when-copying-a-pipeline"></a>U ziet de fout ' DelimitedTextMoreColumnsThanDefined ' bij het kopiëren van een pijp lijn
 
Als een map die u kopieert bestanden bevat met verschillende schema's, zoals een variabel aantal kolommen, een andere scheidings teken, instellingen voor de aanhalings tekens of een gegeven gegevens probleem, kan de Data Factory pijp lijn deze fout veroorzaken:

`
Operation on target Copy_sks  failed: Failure happened on 'Sink' side.
ErrorCode=DelimitedTextMoreColumnsThanDefined,
'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,
Message=Error found when processing 'Csv/Tsv Format Text' source '0_2020_11_09_11_43_32.avro' with row number 53: found more columns than expected column count 27.,
Source=Microsoft.DataTransfer.Common,'
`

**Oplossing**: Selecteer de optie voor **binaire kopieën** tijdens het maken van de Kopieer activiteit. Op deze manier kan Data Factory de bestanden niet openen om het schema te lezen, om uw gegevens te kopiëren of te migreren van de ene data Lake naar een andere. In plaats daarvan behandelt Data Factory elk bestand als binair en kopieert u het naar de andere locatie.

### <a name="a-pipeline-run-fails-when-you-reach-the-capacity-limit-of-the-integration-runtime"></a>Een pijplijn uitvoering mislukt wanneer u de capaciteits limiet van de Integration runtime bereikt

Foutbericht:

`
Type=Microsoft.DataTransfer.Execution.Core.ExecutionException,Message=There are substantial concurrent MappingDataflow executions which is causing failures due to throttling under Integration Runtime 'AutoResolveIntegrationRuntime'.
`

**Oorzaak**: u hebt de capaciteits limiet van de Integration runtime bereikt. U kunt een grote hoeveelheid gegevens stroom uitvoeren met behulp van dezelfde Integration runtime op hetzelfde moment. Zie [Azure-abonnement en service limieten, quota's en beperkingen](../azure-resource-manager/management/azure-subscription-service-limits.md#version-2) voor meer informatie.

**Oplossing**:
 
- Voer uw pijp lijnen uit op verschillende tijdstippen van de trigger.
- Maak een nieuwe Integration runtime en Splits uw pijp lijnen op meerdere integratie-Runtimes.

### <a name="you-have-activity-level-errors-and-failures-in-pipelines"></a>U hebt fouten en storingen op activiteit niveau in pijp lijnen

Met Azure Data Factory indeling wordt voorwaardelijke logica toegestaan en kunnen gebruikers verschillende paden maken op basis van het resultaat van een vorige activiteit. Er kunnen vier voorwaardelijke paden worden ingesteld: wanneer dit is **gelukt** (standaard geslaagd), na het **mislukken** van de **voltooiing** en **bij overs Laan**. 

Azure Data Factory evalueert het resultaat van alle activiteiten op Leaf-niveau. De resultaten van de pijp lijn slagen alleen als alle bladeren slagen. Als een Leaf-activiteit is overgeslagen, evalueren we de bovenliggende activiteit in plaats daarvan. 

**Oplossing**

1. Implementeer controles op activiteit niveau door [te volgen hoe er pijp lijn fouten en-fouten worden afgehandeld](https://techcommunity.microsoft.com/t5/azure-data-factory/understanding-pipeline-failures-and-error-handling/ba-p/1630459).
1. Gebruik Azure Logic Apps voor het bewaken van pijp lijnen met regel matige intervallen na het [uitvoeren van een query op Factory](/rest/api/datafactory/pipelineruns/querybyfactory).

## <a name="monitor-pipeline-failures-in-regular-intervals"></a>Pijp lijn fouten met regel matige intervallen bewaken

Mogelijk moet u de mislukte Data Factory pijp lijnen in intervallen controleren, bijvoorbeeld 5 minuten. U kunt de pijp lijn uitvoeringen opvragen en filteren vanuit een data factory met behulp van het eind punt. 

Stel een Azure Logic-app in om elke vijf minuten een query uit te zoeken op alle defecte pijp lijnen, zoals beschreven in [query door Factory](/rest/api/datafactory/pipelineruns/querybyfactory). Vervolgens kunt u incidenten melden aan ons ticket systeem.

Ga voor meer informatie naar [meldingen verzenden van Data Factory, deel 2](https://www.mssqltips.com/sqlservertip/5962/send-notifications-from-an-azure-data-factory-pipeline--part-2/).

## <a name="next-steps"></a>Volgende stappen

Probeer deze bronnen voor meer informatie over probleem oplossing:

*  [Data Factory Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory functie aanvragen](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure-video's](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Microsoft Q&A-vragenpagina](/answers/topics/azure-data-factory.html)
*  [Twitter-informatie over Data Factory](https://twitter.com/hashtag/DataFactory)