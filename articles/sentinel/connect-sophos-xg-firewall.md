---
title: Verbinding maken met Sophos XG-firewall gegevens met Azure Sentinel | Microsoft Docs
description: Meer informatie over het verbinden van Sophos XG-firewall gegevens met Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 0b8b6247a735bceaf98029740bf9d4f7e233069d
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/10/2021
ms.locfileid: "100097565"
---
# <a name="connect-your-sophos-xg-firewall-to-azure-sentinel"></a>Uw Sophos XG-firewall verbinden met Azure Sentinel

> [!IMPORTANT]
> De Sophos XG firewall-gegevens connector in azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze functie wordt zonder service level agreement gegeven en wordt niet aanbevolen voor productie werkbelastingen. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

In dit artikel wordt uitgelegd hoe u uw [SOPHOS XG-firewall](https://www.sophos.com/products/next-gen-firewall.aspx) apparaat verbindt met Azure Sentinel. Met de Sophos XG firewall Data Connector kunt u eenvoudig uw Sophos XG-firewall logboeken verbinden met Azure Sentinel, voor het weer geven van Dash boards, het maken van aangepaste waarschuwingen en het verbeteren van onderzoek. Integratie tussen Sophos XG firewall en Azure Sentinel maakt gebruik van syslog.

> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werk ruimte waarop u Azure Sentinel uitvoert.

## <a name="forward-sophos-xg-firewall-logs-to-the-syslog-agent"></a>XG-firewall logboeken door sturen naar de syslog-agent  

De Sophos XG-firewall configureren voor het door sturen van syslog-berichten naar uw Azure-werk ruimte via de syslog-agent.

1. Klik in de Azure-Sentinel Portal op **Data connectors** en selecteer **Sophos XG firewall** connector.

1. Selecteer de **pagina connector openen**.

1. Volg de instructies op de pagina **SOPHOS XG firewall** .

## <a name="find-your-data"></a>Uw gegevens zoeken

Nadat de verbinding tot stand is gebracht, worden de gegevens weer gegeven in Log Analytics onder syslog.

## <a name="validate-connectivity"></a>Connectiviteit valideren

Het kan Maxi maal 20 minuten duren voordat uw logboeken in Log Analytics worden weer gegeven.

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u een Sophos XG-firewall verbindt met Azure Sentinel. Zie de volgende artikelen voor meer informatie over Azure Sentinel:

- Meer informatie over het [verkrijgen van inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag met [het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Gebruik werkmappen](tutorial-monitor-your-data.md) om uw gegevens te bewaken.