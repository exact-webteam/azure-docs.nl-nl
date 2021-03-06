---
author: baanders
description: include-bestand voor Azure Digital Apparaatdubbels-limieten
ms.service: digital-twins
ms.topic: include
ms.date: 6/9/2020
ms.author: baanders
ms.openlocfilehash: 4a69b2ff15fc4857e9fb292d2f753aa68ed875d4
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2021
ms.locfileid: "100370092"
---
### <a name="functional-limits"></a>Functionele limieten

De volgende tabel geeft een overzicht van de functionele limieten van Azure Digital Apparaatdubbels. 

> [!TIP]
> Zie [Aanbevolen procedures voor het ontwerpen van modellen](../articles/digital-twins/concepts-models.md#best-practices-for-designing-models)voor het samen werken van aanbevelingen binnen deze functionele limieten.

| Gebied | Mogelijkheid | Standaardlimiet | Aanpasbaar? |
| --- | --- | --- | --- |
| Azure-resource | Aantal Azure Digital Apparaatdubbels-instanties in een regio, per abonnement | 10 | Yes |
| Digitale tweelingen | Aantal apparaatdubbels in een Azure Digital Apparaatdubbels-instantie | 200.000 | Yes |
| Digitale tweelingen | Aantal inkomende relaties met één dubbele waarde | 5\.000 | No |
| Digitale tweelingen | Aantal uitgaande relaties van een enkele dubbele | 5\.000 | No |
| Digitale tweelingen | Maximale grootte (van JSON-hoofd tekst in een PUT-of PATCH-aanvraag) van een enkele dubbele | 32 kB | No |
| Digitale tweelingen | Maximale grootte van de aanvraag lading | 32 kB | No | 
| Routering | Aantal eind punten voor één Azure Digital Apparaatdubbels-exemplaar | 6 | No |
| Routering | Aantal routes voor één Azure Digital Apparaatdubbels-exemplaar | 6 | Yes |
| Modellen | Aantal modellen binnen één Azure Digital Apparaatdubbels-exemplaar | 10.000 | Yes |
| Modellen | Aantal modellen dat kan worden geüpload in één API-aanroep | 250 | No |
| Modellen | Maximale grootte (van JSON-hoofd tekst in een PUT-of PATCH-aanvraag) van één model | 1 MB | No |
| Modellen | Aantal geretourneerde items op één pagina | 100 | No |
| Query’s uitvoeren | Aantal geretourneerde items op één pagina | 100 | Ja |
| Query’s uitvoeren | Aantal `AND`  /  `OR` expressies in een query | 50 | Ja |
| Query’s uitvoeren | Aantal matrix items in een `IN`  /  `NOT IN` component | 50 | Ja |
| Query’s uitvoeren | Aantal tekens in een query | 8,000 | Ja |
| Query’s uitvoeren | Aantal `JOINS` in een query | 5 | Ja |

### <a name="rate-limits"></a>Frequentielimieten

De volgende tabel geeft de frequentie limieten van verschillende Api's aan.

| API | Mogelijkheid | Standaardlimiet | Aanpasbaar? |
| --- | --- | --- | --- |
| Modellen-API | Aantal aanvragen per seconde | 100 | Yes |
| Digital Apparaatdubbels-API | Aantal aanvragen per seconde | 2.000 | Yes |
| Digital Apparaatdubbels-API | Aantal bewerkingen voor maken/verwijderen per seconde over **alle apparaatdubbels en relaties** | 50 | Yes |
| Digital Apparaatdubbels-API | Het aantal bewerkingen voor maken/bijwerken/verwijderen per seconde op een **enkele dubbele** of de relaties | 10 | No |
| Query-API | Aantal aanvragen per seconde | 500 | Yes |
| Query-API | [Query eenheden](../articles/digital-twins/concepts-query-units.md) per seconde | 4000 | Yes |
| API voor gebeurtenis routes | Aantal aanvragen per seconde | 100 | Yes |

### <a name="other-limits"></a>Andere limieten

Limieten voor gegevens typen en velden in DTDL documenten voor Azure Digital Apparaatdubbels-modellen vindt u in de documentatie van de bijbehorende specificaties in GitHub: [*Digital Apparaatdubbels Definition Language (DTDL)-versie 2*](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md).
 
Details van query latentie en andere query beperkingen vindt u in [*de instructies: Query's uitvoeren op de dubbele grafiek*](../articles/digital-twins/how-to-query-graph.md).
