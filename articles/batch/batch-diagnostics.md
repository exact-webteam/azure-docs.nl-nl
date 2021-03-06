---
title: Metrische gegevens, waarschuwingen en Diagnostische logboeken
description: Registreer en analyseer logboek gebeurtenissen van het diagnostische gegevens voor Azure Batch account resources, zoals Pools en taken.
ms.topic: how-to
ms.date: 10/08/2020
ms.custom: seodec18
ms.openlocfilehash: 83411d7018155955f5be71bd41803e510edbc9da
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/17/2021
ms.locfileid: "100592683"
---
# <a name="batch-metrics-alerts-and-logs-for-diagnostic-evaluation-and-monitoring"></a>Metrische batches, waarschuwingen en logboeken voor diagnostische evaluatie en bewaking

In dit artikel wordt uitgelegd hoe u een batch-account kunt bewaken met behulp van functies van [Azure monitor](../azure-monitor/overview.md). Azure Monitor verzamelt [metrische gegevens](../azure-monitor/essentials/data-platform-metrics.md) en [Diagnostische logboeken](../azure-monitor/essentials/platform-logs-overview.md) voor resources in uw batch-account. Verzamel en gebruik deze gegevens op verschillende manieren om uw batch-account te controleren en problemen op te sporen. U kunt ook [metrische waarschuwingen](../azure-monitor/alerts/alerts-overview.md) configureren zodat u meldingen ontvangt wanneer een metriek een opgegeven waarde bereikt.

## <a name="batch-metrics"></a>Metrische batches

Metrische gegevens zijn Azure-telemetriegegevens (ook wel prestatie meter items genoemd) die worden verzonden door uw Azure-resources en die worden gebruikt door de Azure Monitor service. Voor beelden van metrische gegevens in een batch-account zijn groep maken gebeurtenissen, Low-Priority aantal knoop punten en gebeurtenissen voltooid.

Bekijk de [lijst met ondersteunde metrische gegevens voor batches](../azure-monitor/essentials/metrics-supported.md#microsoftbatchbatchaccounts).

Metrische gegevens zijn:

- Standaard ingeschakeld in elk batch-account zonder aanvullende configuratie
- Elke 1 minuut gegenereerd
- Wordt niet automatisch gehandhaafd, maar heeft een rolling geschiedenis van 30 dagen. U kunt metrische gegevens van activiteiten persistent maken als onderdeel van de diagnostische logboek registratie.

## <a name="view-batch-metrics"></a>Metrische batches weer geven

Azure Portal op de pagina **overzicht** van het account worden standaard de metrische gegevens van het sleutel knooppunt, kernen en taak weer gegeven.

De metrische gegevens van een batch-account weer geven in de Azure Portal:

1. Selecteer in het Azure Portal **alle services**  >  **batch-accounts** en selecteer vervolgens de naam van uw batch-account.
2. Selecteer **Metrische gegevens** onder **Bewaking**.
3. Selecteer **metrische gegevens toevoegen** en kies vervolgens een waarde in de vervolg keuzelijst.
4. Selecteer een **aggregatie** optie voor de metriek. Gebruik de **gemiddelde** aggregatie voor metrische gegevens op basis van tellers (zoals ' toegewezen aantal kernen ' of ' aantal knoop punten met lage prioriteit '). Voor metrische gegevens op basis van gebeurtenissen (zoals ' pool formaat wijzigen voltooid ') gebruikt u de aggregatie **aantal**'.

   > [!WARNING]
   > Gebruik niet de aggregatie ' sum ', waarmee de waarden van alle gegevens punten worden toegevoegd die gedurende de periode van de grafiek worden ontvangen.

5. Herhaal stap 3 en 4 om extra metrische gegevens toe te voegen.

U kunt metrische gegevens ook programmatisch ophalen met de Azure Monitor-Api's. Zie [Azure monitor metrische gegevens ophalen met .net](https://azure.microsoft.com/resources/samples/monitor-dotnet-metrics-api/)voor een voor beeld.

### <a name="batch-metric-reliability"></a>Betrouw baarheid batch metriek

Metrische gegevens kunnen helpen bij het identificeren van trends en kunnen worden gebruikt voor het analyseren van data. Het is belang rijk te weten dat metrische levering niet gegarandeerd is en kan worden onderhevig aan een afdoende levering, gegevens verlies en/of duplicatie. Als gevolg hiervan wordt het gebruik van enkelvoudige gebeurtenissen naar waarschuwingen of trigger functies niet aanbevolen. Zie de volgende sectie voor meer informatie over het instellen van drempel waarden voor waarschuwingen.

De metrische gegevens die in de afgelopen drie minuten zijn verzonden, kunnen nog steeds worden geaggregeerd, zodat metrische waarden kunnen worden ondergerapporteerd tijdens deze periode.

## <a name="batch-metric-alerts"></a>Waarschuwingen voor batch metingen

U kunt bijna realtime *waarschuwingen voor metrische gegevens* configureren die activeren wanneer de waarde van een opgegeven metriek een drempel overschrijdt die u toewijst. De waarschuwing genereert een melding wanneer de waarschuwing ' geactiveerd ' is (wanneer de drempel wordt overschreden en er wordt voldaan aan de status van de waarschuwing), en wanneer deze ' opgelost ' is (wanneer de drempel waarde opnieuw wordt overschreden en de voor waarde niet meer wordt bereikt).

Waarschuwingen die op één gegevens punt geactiveerd worden, worden niet aanbevolen, omdat de metrieken afhankelijk zijn van een out-of-order-levering, gegevens verlies en/of duplicatie. Wanneer u uw waarschuwingen maakt, kunt u drempels gebruiken om deze inconsistenties te verantwoorden.

U kunt bijvoorbeeld een waarschuwing voor metrische gegevens configureren wanneer het aantal kernen op basis van de prioriteit een bepaald niveau heeft, zodat u de samen stelling van uw groeps aanpassen. Stel voor de beste resultaten een periode van 10 of meer minuten in, waarbij waarschuwingen worden geactiveerd als het gemiddelde aantal kernen met lage prioriteit lager is dan de drempel waarde voor de hele periode. Dit maakt het mogelijk dat er meer tijd in beslag wordt verzameld, zodat u nauw keurige resultaten krijgt.

Een waarschuwing voor metrische gegevens configureren in de Azure Portal:

1. Selecteer **Alle services** > **Batch-accounts** en selecteer vervolgens de naam van uw Batch-account.
2. Onder **bewaking** selecteert u **waarschuwingen** en selecteert u **nieuwe waarschuwings regel**.
3. Klik op **voor waarde selecteren** en kies vervolgens een metriek. Bevestig de waarden voor de **grafiek periode**, het **type drempel waarde**, de **operator** en het **aggregatie type** en voer een **drempel waarde** in. Selecteer vervolgens **Done**.
4. Voeg een actie groep toe aan de waarschuwing door een bestaande actie groep te selecteren of een nieuwe actie groep te maken.
5. Voer in de sectie **Details van waarschuwings regel** een naam en **Beschrijving** voor de **waarschuwings regel** in en selecteer de **Ernst**
6. Selecteer **Waarschuwingsregel maken**.

Zie [begrijpen hoe metrische waarschuwingen werken in azure monitor](../azure-monitor/alerts/alerts-metric-overview.md) en [metrische waarschuwingen met behulp van Azure monitor maken, weer geven en beheren](../azure-monitor/alerts/alerts-metric.md)voor meer informatie over het maken van metrische waarschuwingen.

U kunt ook een nabije realtime-waarschuwing configureren met behulp van de Azure Monitor [rest API](/rest/api/monitor/). Zie [overzicht van waarschuwingen in Microsoft Azure](../azure-monitor/alerts/alerts-overview.md)voor meer informatie. Zie de informatie over zoek query's in [reageren op gebeurtenissen met Azure monitor waarschuwingen](../azure-monitor/alerts/tutorial-response.md)om taak-, taak-of pool-specifieke informatie in uw waarschuwingen op te vragen.

## <a name="batch-diagnostics"></a>Batch-diagnose

Diagnostische logboeken bevatten informatie die wordt verzonden door Azure-resources die de werking van elke resource beschrijven. Voor batch kunt u de volgende logboeken verzamelen:

- **Service logboeken** gebeurtenissen die worden verzonden door de Azure batch-service tijdens de levens duur van een afzonderlijke batch resource, zoals een groep of taak.
- **Metrische gegevens** worden geregistreerd op account niveau.

De instellingen voor het inschakelen van het verzamelen van Diagnostische logboeken zijn standaard niet ingeschakeld. Schakel de diagnostische instellingen expliciet in voor elk batch-account dat u wilt bewaken.

### <a name="log-destinations"></a>Logboek bestemmingen

Een veelvoorkomend scenario is het selecteren van een Azure Storage-account als de logboek bestemming. Als u Logboeken in Azure Storage wilt opslaan, moet u het account maken voordat u het verzamelen van logboeken inschakelt. Als u een opslag account aan uw batch-account hebt gekoppeld, kunt u dat account kiezen als de logboek bestemming.

U kunt ook het volgende doen:

- Batch diagnostische logboek gebeurtenissen streamen naar een [Azure Event hub](../event-hubs/event-hubs-about.md). Event Hubs kan miljoenen gebeurtenissen per seconde opnemen, die u vervolgens kunt transformeren en opslaan met behulp van een real-time analyse provider.
- Diagnostische logboeken verzenden naar [Azure monitor-logboeken](../azure-monitor/logs/log-query-overview.md), waar u ze kunt analyseren of exporteren voor analyse in Power bi of Excel.

> [!NOTE]
> Mogelijk worden er extra kosten in rekening gebracht om diagnostische logboek gegevens met Azure-Services op te slaan of te verwerken.

### <a name="enable-collection-of-batch-diagnostic-logs"></a>Verzamelen van batch-diagnose logboeken inschakelen

Volg de onderstaande stappen om een nieuwe diagnostische instelling te maken in de Azure Portal.

1. Selecteer in het Azure Portal **alle services**  >  **batch-accounts** en selecteer vervolgens de naam van uw batch-account.
2. Selecteer **Diagnostische instellingen** onder **Controle**.
3. Selecteer in **Diagnostische instellingen** de optie **Diagnostische instelling toevoegen**.
4. Voer een naam in voor de instelling.
5. Selecteer een bestemming: **verzenden naar log Analytics**, **archiveren naar een opslag account** of **streamen naar een event hub**. Als u een opslag account selecteert, kunt u eventueel een Bewaar beleid instellen. Als u niet een aantal dagen opgeeft voor retentie, worden gegevens bewaard tijdens de levens duur van het opslag account.
6. Selecteer **ServiceLog**, **AllMetrics** of beide.
7. Selecteer **Opslaan** om de diagnostische instelling te maken.

U kunt ook [verzameling via Azure monitor in de Azure Portal inschakelen](../azure-monitor/essentials/diagnostic-settings.md) om Diagnostische instellingen te configureren, met behulp van een [Resource Manager-sjabloon](../azure-monitor/essentials/resource-manager-diagnostic-settings.md)of met Azure POWERSHELL of de Azure cli. Zie [overzicht van Azure platform-logboeken](../azure-monitor/essentials/platform-logs-overview.md)voor meer informatie.

### <a name="access-diagnostics-logs-in-storage"></a>Diagnostische logboeken openen in Storage

Als u batch-diagnose logboeken archiveert in een opslag account, wordt er een opslag container gemaakt in het opslag account zodra er een gerelateerde gebeurtenis plaatsvindt. Blobs worden gemaakt aan de hand van het volgende naamgevings patroon:

```json
insights-{log category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/
RESOURCEGROUPS/{resource group name}/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/{Batch account name}/y={four-digit numeric year}/
m={two-digit numeric month}/d={two-digit numeric day}/
h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Bijvoorbeeld:

```json
insights-metrics-pt1m/resourceId=/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/
RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/
BATCHACCOUNTS/MYBATCHACCOUNT/y=2018/m=03/d=05/h=22/m=00/PT1H.json
```

Elk `PT1H.json` blob-bestand bevat gebeurtenissen in JSON-indeling die zijn opgetreden binnen het uur dat is opgegeven in de BLOB-URL (bijvoorbeeld `h=12` ). Tijdens het huidige uur worden gebeurtenissen aan het `PT1H.json` bestand toegevoegd wanneer deze zich voordoen. De minuut waarde ( `m=00` ) is altijd `00` , omdat de diagnostische logboek gebeurtenissen per uur worden opgesplitst in afzonderlijke blobs. (Alle tijden zijn in UTC.)

Hieronder ziet u een voor beeld van een `PoolResizeCompleteEvent` vermelding in een `PT1H.json` logboek bestand. Het bevat informatie over het huidige en het doel aantal toegewezen knoop punten met een lage prioriteit, evenals de begin-en eind tijd van de bewerking:

```json
{ "Tenant": "65298bc2729a4c93b11c00ad7e660501", "time": "2019-08-22T20:59:13.5698778Z", "resourceId": "/SUBSCRIPTIONS/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.BATCH/BATCHACCOUNTS/MYBATCHACCOUNT/", "category": "ServiceLog", "operationName": "PoolResizeCompleteEvent", "operationVersion": "2017-06-01", "properties": {"id":"MYPOOLID","nodeDeallocationOption":"Requeue","currentDedicatedNodes":10,"targetDedicatedNodes":100,"currentLowPriorityNodes":0,"targetLowPriorityNodes":0,"enableAutoScale":false,"isAutoPool":false,"startTime":"2019-08-22 20:50:59.522","endTime":"2019-08-22 20:59:12.489","resultCode":"Success","resultMessage":"The operation succeeded"}}
```

Zie voor meer informatie over het schema van Diagnostische logboeken in het opslag account [Azure-resource logboeken archiveren in het opslag account](../azure-monitor/essentials/resource-logs.md#send-to-azure-storage). Als u via een programma toegang wilt krijgen tot de logboeken in uw opslag account, gebruikt u de opslag-Api's.

### <a name="service-log-events"></a>Service logboek gebeurtenissen

Azure Batch service logboeken, indien verzameld, bevatten gebeurtenissen die worden gegenereerd door de Azure Batch-service tijdens de levens duur van een afzonderlijke batch resource, zoals een groep of taak. Elke gebeurtenis die wordt verzonden door batch, wordt geregistreerd in JSON-indeling. Dit is bijvoorbeeld de hoofd tekst van een voor beeld van een **groep maken gebeurtenis**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "5",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "taskSlotsPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

De volgende gebeurtenissen van het service logboek worden verzonden door de batch-service:

- [Groep maken](batch-pool-create-event.md)
- [Groep verwijderen starten](batch-pool-delete-start-event.md)
- [Groep verwijderen is voltooid](batch-pool-delete-complete-event.md)
- [Begin grootte van groep wijzigen](batch-pool-resize-start-event.md)
- [Grootte van groep volt ooien voltooid](batch-pool-resize-complete-event.md)
- [Groep automatisch schalen](batch-pool-autoscale-event.md)
- [Taak starten](batch-task-start-event.md)
- [Taak voltooid](batch-task-complete-event.md)
- [Taak mislukt](batch-task-fail-event.md)
- [Taak planning mislukt](batch-task-schedule-fail-event.md)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [Batch-API's en -hulpprogramma's](batch-apis-tools.md) die beschikbaar zijn voor het bouwen van Batch-oplossingen.
- Meer informatie over het [bewaken van batch-oplossingen](monitoring-overview.md).