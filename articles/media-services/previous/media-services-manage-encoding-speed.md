---
title: Snelheid en gelijktijdigheid van uw code ring beheren met Azure Media Services | Microsoft Docs
description: Dit artikel bevat een kort overzicht van hoe u de snelheid en gelijktijdigheid van uw coderings taken/taken kunt beheren met Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 676313f8-a158-4e3a-a99b-2c29a341ecc9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 4b6f843678d64bddd276f6123a432699efc89ad9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "89269281"
---
#  <a name="manage-speed-and-concurrency-of-your-encoding"></a>De snelheid en gelijktijdigheid van uw codering beheren

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

Dit artikel bevat een kort overzicht van hoe u de snelheid en gelijktijdigheid van uw coderings taken/taken kunt beheren.

## <a name="overview"></a>Overzicht

In Media Services bepaalt een **gereserveerd eenheids type** de snelheid waarmee uw media verwerkings taken worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2** en **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**). Het onderwerp voor het [schalen van coderings eenheden](media-services-scale-media-processing-overview.md) bevat een tabel waarmee u beslissingen kunt nemen bij het kiezen tussen verschillende coderings snelheden.

Naast het opgeven van het type gereserveerde eenheid kunt u opgeven om uw account in te richten met **gereserveerde eenheden**. Op basis van het aantal ingerichte, gereserveerde eenheden wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account. Als uw account bijvoorbeeld vijf gereserveerde eenheden heeft, zullen vijf media taken gelijktijdig worden uitgevoerd, zolang er taken moeten worden verwerkt. De resterende taken wachten in de wachtrij en worden opgehaald voor verwerking opeenvolgend wanneer een actieve taak wordt voltooid. Als er voor een account geen gereserveerde eenheden zijn ingericht, worden de taken sequentieel opgehaald. In dit geval is de wacht tijd tussen het volt ooien van een taak en de volgende begint afhankelijk van de beschik baarheid van resources in het systeem.

Zie [Dit](media-services-scale-media-processing-overview.md) onderwerp voor gedetailleerde informatie en voor beelden die laten zien hoe u coderings eenheden kunt schalen.

## <a name="next-step"></a>Volgende stap

[Schaal coderings eenheden](media-services-scale-media-processing-overview.md)

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

