---
title: Beveiligingsagents
description: Ga aan de slag met het leren, configureren, implementeren en gebruiken van Azure Defender voor IoT Security Service-agents op uw IoT-apparaten.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/24/2021
ms.author: shhazam
ms.openlocfilehash: a40b64dd3b8f898c961863c1d78a2a56642c44b9
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2021
ms.locfileid: "100521608"
---
# <a name="get-started-with-azure-defender-for-iot-device-micro-agents"></a>Aan de slag met Azure Defender voor IoT Device micro agents

Defender voor IoT-beveiligings agenten bieden verbeterde beveiligings mogelijkheden, zoals de aanbevolen procedures voor het controleren van de configuratie van besturings systemen. Neem de controle over uw apparaat veld beveiliging tegen bedreigingen en beveiligings postuur met één service.

De verdedigings medewerkers voor IoT-beveiligings agenten verwerken de onbewerkte gebeurtenis verzameling van het besturings systeem van het apparaat, het samen voegen van gebeurtenissen om de kosten te verlagen en de configuratie via een apparaat-module te verdubbelen. Beveiligings berichten worden via uw IoT Hub naar Defender voor IoT Analytics Services verzonden.

Gebruik de volgende werk stroom voor het implementeren en testen van uw Defender voor IoT-beveiligings agenten:

1. [Schakel Defender voor IOT-service in op uw IOT hub](quickstart-onboard-iot-hub.md).

1. Als uw IoT Hub geen geregistreerde apparaten heeft, [registreert u een nieuw apparaat](../iot-accelerators/iot-accelerators-device-simulation-overview.md).

1. [Maak een DefenderIotMicroAgent-module](quickstart-create-micro-agent-module-twin.md) voor uw apparaten.

1. Als u de agent wilt installeren op een gesimuleerd Azure-apparaat in plaats van op een echt apparaat te installeren, kunt u [een nieuwe virtuele Azure-machine (VM)](../virtual-machines/linux/quick-create-portal.md) in een beschik bare zone zetten.

1. [Implementeer een Defender voor IOT-beveiligings agent](how-to-deploy-linux-cs.md) op uw IOT-apparaat of op een nieuwe virtuele machine.

1. Volg de instructies voor [trigger_events](https://aka.ms/iot-security-github-trigger-events) een basislijn gebeurtenis van het besturings systeem uit te voeren.

1. Verifieer Defender voor IoT-aanbevelingen als reactie op de gesimuleerde controle van de basis lijn van de OS-fout in de vorige stap. Verificatie starten 30 minuten na het uitvoeren van het script.

## <a name="next-steps"></a>Volgende stappen

Uw [oplossing](quickstart-configure-your-solution.md)configureren 
 [beveiligings modules maken](quickstart-create-security-twin.md) [aangepaste waarschuwingen](quickstart-create-custom-alerts.md)configureren 
 [een beveiligings agent implementeren](how-to-deploy-agent.md)
