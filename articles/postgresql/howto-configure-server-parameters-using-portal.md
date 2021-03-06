---
title: Server parameters configureren-Azure Portal-Azure Database for PostgreSQL-één server
description: In dit artikel wordt beschreven hoe u de post gres-para meters in Azure Database for PostgreSQL kunt configureren via de Azure Portal.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 02/28/2018
ms.openlocfilehash: e1b40e3116d56e87a2f397350ef2ba5510e04c0c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "91707690"
---
# <a name="configure-server-parameters-in-azure-database-for-postgresql---single-server-via-the-azure-portal"></a>Server parameters configureren in Azure Database for PostgreSQL-één server via de Azure Portal 
U kunt configuratie parameters voor een Azure Database for PostgreSQL server weer geven, tonen en bijwerken via de Azure Portal.

## <a name="prerequisites"></a>Vereisten
Als u deze hand leiding wilt door lopen, hebt u het volgende nodig:
- [Azure Database for PostgreSQL server](quickstart-create-server-database-portal.md)

## <a name="viewing-and-editing-parameters"></a>Para meters weer geven en bewerken
1. Open [Azure Portal](https://portal.azure.com).

2. Selecteer uw Azure Database for PostgreSQL-server.

3. Selecteer in de sectie **instellingen** de optie **server parameters**. Op de pagina ziet u een lijst met para meters, hun waarden en beschrijvingen.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/3-overview-of-parameters.png" alt-text="Overzichts pagina voor para meters":::

4. Selecteer de **vervolg keuzelijst** om de mogelijke waarden voor enumerated-type para meters zoals client_min_messages weer te geven.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-enum-drop-down.png" alt-text="Overzichts pagina voor para meters":::

5. Selecteer of Beweeg de muis aanwijzer over de knop met het **i** (informatie) om het bereik van mogelijke waarden voor numerieke para meters zoals cpu_index_tuple_cost weer te geven.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/4-information-button.png" alt-text="Overzichts pagina voor para meters":::

6. Gebruik, indien nodig, het **zoekvak** om te beperken tot een specifieke para meter. De zoek opdracht bevindt zich op de naam en beschrijving van de para meters.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/5-search.png" alt-text="Overzichts pagina voor para meters":::

7. Wijzig de parameter waarden die u wilt aanpassen. Alle wijzigingen die u aanbrengt in een sessie, worden in paars gemarkeerd. Wanneer u de waarden hebt gewijzigd, kunt u **Opslaan**selecteren. Of u kunt uw wijzigingen **negeren** .
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/6-save-and-discard-buttons.png" alt-text="Overzichts pagina voor para meters":::

8. Als u nieuwe waarden voor de para meters hebt opgeslagen, kunt u altijd terugkeren naar de standaard waarden door **alles opnieuw instellen op de standaard**waarde te selecteren.
:::image type="content" source="./media/howto-configure-server-parameters-in-portal/7-reset-to-default-button.png" alt-text="Overzichts pagina voor para meters":::

## <a name="next-steps"></a>Volgende stappen
Meer informatie over:
- [Overzicht van server parameters in Azure Database for PostgreSQL](concepts-servers.md)
- [Para meters configureren met de Azure CLI](howto-configure-server-parameters-using-cli.md)
