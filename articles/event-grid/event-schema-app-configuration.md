---
title: Configuratie Azure-app als Event Grid bron
description: In dit artikel wordt beschreven hoe u Azure-app configuratie als Event Grid gebeurtenis bron gebruikt. Het bevat het schema en de koppelingen naar de zelf studie en de artikelen met procedures.
ms.topic: conceptual
ms.date: 02/11/2021
ms.openlocfilehash: a64c6fead5e6d95ba11bc98d7e9a52e3021c3be2
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2021
ms.locfileid: "100366769"
---
# <a name="azure-app-configuration-as-an-event-grid-source"></a>Configuratie Azure-app als Event Grid bron
In dit artikel vindt u de eigenschappen en het schema voor Azure-app configuratie gebeurtenissen. Zie [Azure Event grid-gebeurtenis schema](event-schema.md)voor een inleiding tot gebeurtenis schema's. Het biedt ook een lijst met snel starten en zelf studies om Azure-app configuratie als een gebeurtenis bron te gebruiken.

## <a name="available-event-types"></a>Beschik bare gebeurtenis typen

Azure-app configuratie worden de volgende gebeurtenis typen verzonden:

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Micro soft. AppConfiguration. KeyValueModified | Deze gebeurtenis treedt op wanneer een sleutel waarde wordt gemaakt of vervangen. |
| Micro soft. AppConfiguration. KeyValueDeleted | Deze gebeurtenis treedt op wanneer een sleutel waarde wordt verwijderd. |

## <a name="example-event"></a>Voorbeeld gebeurtenis

# <a name="event-grid-event-schema"></a>[Event Grid-gebeurtenisschema](#tab/event-grid-event-schema)
In het volgende voor beeld ziet u het schema van een gebeurtenis die door een sleutel waarde is gewijzigd: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Het schema voor een gebeurtenis die door een sleutel waarde is verwijderd, is vergelijkbaar: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
# <a name="cloud-event-schema"></a>[Cloudgebeurtenisschema](#tab/cloud-event-schema)

In het volgende voor beeld ziet u het schema van een gebeurtenis die door een sleutel waarde is gewijzigd: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "source": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "type": "Microsoft.AppConfiguration.KeyValueModified",
  "time": "2019-05-31T20:05:03Z",
  "specversion": "1.0"
}]
```

Het schema voor een gebeurtenis die door een sleutel waarde is verwijderd, is vergelijkbaar: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "source": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "type": "Microsoft.AppConfiguration.KeyValueDeleted",
  "time": "2019-05-31T20:05:03Z",
  "specversion": "1.0"
}]
```

---

## <a name="event-properties"></a>Gebeurtenis eigenschappen

# <a name="event-grid-event-schema"></a>[Event Grid-gebeurtenisschema](#tab/event-grid-event-schema)
Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| `topic` | tekenreeks | Volledige bronpad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| `subject` | tekenreeks | Het door de uitgever gedefinieerde pad naar het gebeurtenisonderwerp. |
| `eventType` | tekenreeks | Een van de geregistreerde gebeurtenistypen voor deze gebeurtenisbron. |
| `eventTime` | tekenreeks | Het tijdstip waarop de gebeurtenis is gegenereerd op basis van de UTC-tijd van de provider. |
| `id` | tekenreeks | De unieke id voor de gebeurtenis. |
| `data` | object | Gebeurtenis gegevens voor app-configuratie. |
| `dataVersion` | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| `metadataVersion` | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema voor de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |


# <a name="cloud-event-schema"></a>[Cloudgebeurtenisschema](#tab/cloud-event-schema)

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| `source` | tekenreeks | Volledige bronpad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| `subject` | tekenreeks | Het door de uitgever gedefinieerde pad naar het gebeurtenisonderwerp. |
| `type` | tekenreeks | Een van de geregistreerde gebeurtenistypen voor deze gebeurtenisbron. |
| `time` | tekenreeks | Het tijdstip waarop de gebeurtenis is gegenereerd op basis van de UTC-tijd van de provider. |
| `id` | tekenreeks | De unieke id voor de gebeurtenis. |
| `data` | object | Gebeurtenis gegevens voor app-configuratie. |
| `specversion` | tekenreeks | CloudEvents-schema specificatie versie. |

---

Het gegevens object heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| `key` | tekenreeks | De sleutel van de sleutel waarde die is gewijzigd of verwijderd. |
| `label` | tekenreeks | Het label, indien van toepassing, van de sleutel waarde die is gewijzigd of verwijderd. |
| `etag` | tekenreeks | Voor `KeyValueModified` de ETAG van de nieuwe sleutel waarde. Voor `KeyValueDeleted` de ETAG van de sleutel waarde die is verwijderd. |


## <a name="tutorials-and-how-tos"></a>Zelfstudies en handleidingen

|Titel | Beschrijving |
|---------|---------|
| [Reageren op Azure-app configuratie gebeurtenissen met behulp van Event Grid](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Overzicht van het integreren van Azure-app configuratie met Event Grid. |
| [Event Grid voor gegevens wijzigings meldingen gebruiken](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Meer informatie over het gebruik van Azure-app configuratie gebeurtenis abonnementen om de gebeurtenissen voor het wijzigen van de sleutel waarde naar een webeindpunt te verzenden. |

## <a name="next-steps"></a>Volgende stappen

* Zie [Wat is Event Grid?](overview.md) voor een inleiding tot Azure Event Grid.
* Zie [Event grid Subscription schema](subscription-creation-schema.md)voor meer informatie over het maken van een Azure Event grid-abonnement.
* Zie [Event grid voor gegevens wijzigings meldingen gebruiken](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json)voor een inleiding tot het werken met Azure-app configuratie gebeurtenissen. 