---
title: Overzicht van dynamische verpakking van Azure Media Services | Microsoft Docs
description: In deze artikelen vindt u een overzicht van Microsoft Azure Media Services dynamische verpakking.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: juliako
ms.openlocfilehash: 7c4f099df071bccb8a74f29a98953fe1e0323b12
ms.sourcegitcommit: 77afc94755db65a3ec107640069067172f55da67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2021
ms.locfileid: "98695574"
---
# <a name="dynamic-packaging"></a>Dynamische verpakking

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector" title1="Selecteer de versie van Media Services die u gebruikt:"]
> * [Versie 3](../latest/dynamic-packaging-overview.md)
> * [Versie 2](media-services-dynamic-packaging-overview.md)

> [!NOTE]
> Er worden geen nieuwe functies of functionaliteit meer aan Media Services v2. toegevoegd. <br/>Bekijk de nieuwste versie [Media Services v3](../latest/index.yml). Zie ook [migratie richtlijnen van v2 naar v3](../latest/migrate-v-2-v-3-migration-introduction.md)

Microsoft Azure Media Services kunnen worden gebruikt voor het leveren van veel media bron bestands indelingen, mediastreaming en indelingen voor inhouds beveiliging voor diverse client technologieën (bijvoorbeeld iOS, XBOX, Silverlight, Windows 8). Deze clients begrijpen verschillende protocollen, bijvoorbeeld iOS vereist een HTTP Live Streaming (HLS) v4-indeling en Silverlight en Xbox vereisen Smooth Streaming. Als u een set apparaatafhankelijke bitrate-bestanden (ISO base media 14496-12) hebt of een set adaptieve bitrate Smooth Streaming bestanden die u wilt gebruiken voor clients die een MPEG-DASH, HLS of Smooth Streaming begrijpen, moet u profiteren van Media Services dynamische verpakking.

Met dynamische pakketten moet u een Asset maken die een set Adaptive bitrate MP4-bestanden of een adaptieve bitsnelheid Smooth Streaming-bestanden bevat. Op basis van de opgegeven indeling in het manifest of de fragment aanvraag zorgt de server voor streaming op aanvraag ervoor dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client.

In het volgende diagram ziet u de traditionele code ring en statische pakket werk stroom.

![Statische code ring](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

In het volgende diagram ziet u de dynamische verpakkings werk stroom.

![Dynamische code ring](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>Gebruikelijk scenario

1. Upload een invoer bestand (een ' mezzanine-bestand '). Bijvoorbeeld: H. 264, MP4 of WMV (Zie [indelingen die worden ondersteund door de Media Encoder Standard](media-services-media-encoder-standard-formats.md)voor de lijst met ondersteunde indelingen.
2. Codeer uw mezzanine-bestand naar een adaptieve bitrate-set van H. 264 MP4.
3. Publiceer de Asset met de Adaptive bitrate MP4 die is ingesteld door de Locator op aanvraag te maken.
4. Bouw de streaming-Url's om uw inhoud te openen en te streamen.

## <a name="preparing-assets-for-dynamic-streaming"></a>Activa voorbereiden voor dynamische streaming

Als u uw asset wilt voorbereiden op dynamische streaming, hebt u de volgende opties:

- [Upload een hoofd bestand](media-services-dotnet-upload-files.md).
- [Gebruik de coderings Media Encoder Standard om H. 264 MP4-sets met een adaptieve bitsnelheid te maken](media-services-dotnet-encode-with-media-encoder-standard.md).
- [Uw inhoud streamen](media-services-deliver-content-overview.md).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Audio-codecs die worden ondersteund door dynamische pakketten

Dynamische verpakking ondersteunt MP4-bestanden, die audio gecodeerd met [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, hij-AAC v1, HE-AAC v2), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus)(Enhanced AC-3 of E-AC3), Dolby Atmos of [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, DTS HD, DTS HD lossless) bevatten. Streaming van Dolby Atmos-inhoud wordt ondersteund voor standaarden als MPEG-DASH protocol met gefragmenteerde MP4 (common Streaming Format) of CMAF (common Media Application Format) en via HTTP Live Streaming (HLS) met CMAF.

> [!NOTE]
> Dynamische verpakking biedt geen ondersteuning voor bestanden met [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) -audio (AC3) (dit is een verouderde codec).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
