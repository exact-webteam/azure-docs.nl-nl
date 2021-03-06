---
author: trevorbye
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 01/27/2020
ms.author: trbye
ms.openlocfilehash: a41c9dc888e85baf021712e3302da7ae6106db47
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2020
ms.locfileid: "81422243"
---
Voor het voltooien van de quickstart voor het herkennen van intenties moet u een LUIS-account en een project maken met behulp van de LUIS-previewportal. Voor deze quickstart hebt u alleen een LUIS-abonnement nodig. Een spraakserviceabonnement is *niet* vereist.

Eerst moet u een LUIS-account en -app maken in de LUIS-previewportal. Voor de LUIS-app die u maakt, wordt een vooraf samengesteld domein gebruikt voor automatisering van de startpagina. Dit domein biedt intenties, entiteiten en voorbeelduitingen. Wanneer u klaar bent, hebt u een LUIS-eindpunt in de cloud dat u kunt aanroepen met behulp van de Speech-SDK. 

Volg deze instructies om uw LUIS-app te maken:

* <a href="https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Snelstart: Een vooraf samengestelde domein-app maken <span class="docon docon-navigate-external x-hidden-focus"></span></a>

Wanneer u klaar bent, hebt u vier dingen nodig:

* Nieuwe publicatie met **Spraakvoorbereiding** ingeschakeld
* Uw **primaire LUIS-sleutel**
* Uw **LUIS-locatie**
* De **app-id** van uw LUIS-app

Hier is de locatie van deze informatie in de [LUIS-previewportal](https://preview.luis.ai/):

1. Selecteer uw app in de LUIS-previewportal en selecteer vervolgens de knop **Publiceren**.

2. Selecteer de **Productiesite**. Zet de optie **Spraakvoorbereiding** in de stand **Aan** als u `en-US` gebruikt. Selecteer vervolgens de knop **Publiceren**.

    > [!IMPORTANT]
    > **Spraakvoorbereiding** wordt ten zeerste aangeraden, aangezien de nauwkeurigheid van spraakherkenning hiermee wordt verbeterd.

    > [!div class="mx-imgBorder"]
    > ![LUIS naar eindpunt publiceren](../../../media/luis/publish-app-popup.png)

3. Selecteer **Beheren** in de LUIS-previewportal en selecteer vervolgens **Azure-resources**. Op deze pagina vindt u uw LUIS-sleutel en -locatie (ook wel _regio_ genoemd).

   > [!div class="mx-imgBorder"]
   > ![LUIS-sleutel en -locatie](../../../media/luis/luis-key-region.png)

4. Zodra u over uw sleutel en locatie beschikt, hebt u de app-id nodig. Selecteer **Toepassingsinstellingen** (uw app-id is op deze pagina beschikbaar).

   > [!div class="mx-imgBorder"]
   > ![LUIS-app-id](../../../media/luis/luis-app-id.png)
