---
title: Uw eerste functie maken in Azure Portal
description: Leer hoe u uw eerste serverloze Azure-functie kunt maken met behulp van Azure Portal.
ms.topic: how-to
ms.date: 03/26/2020
ms.custom: devx-track-csharp, mvc, devcenter, cc996988-fb4f-47
ms.openlocfilehash: 63e9c87d1d94d6b803c27862bc9f2755e02f3111
ms.sourcegitcommit: 706e7d3eaa27f242312d3d8e3ff072d2ae685956
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2021
ms.locfileid: "99980934"
---
# <a name="create-your-first-function-in-the-azure-portal"></a>Uw eerste functie maken in Azure Portal

Met Azure Functions kunt u uw code in een serverloze omgeving uitvoeren zonder dat u eerst een virtuele machine (VM) hoeft te maken of een webtoepassing moet publiceren. In dit artikel leert u hoe u Azure Functions kunt gebruiken om een ' Hallo wereld ' HTTP-trigger functie te maken in de Azure Portal.

>[!NOTE]
>Bewerken in de portal wordt alleen ondersteund voor Java script-, Power shell-, type script-en C#-scripts.<br><br>Voor C#-klassen bibliotheek-, Java-en python-functies kunt u de functie-app maken in de portal, maar u moet ook de functies lokaal maken en deze vervolgens publiceren naar Azure. 

In plaats daarvan wordt u aangeraden [uw functies lokaal te ontwikkelen](functions-develop-local.md) en te publiceren naar een functie-app in Azure.  
Gebruik een van de volgende koppelingen om aan de slag te gaan met de gekozen lokale ontwikkel omgeving en-taal:

| Visual Studio Code | Terminal/opdrachtprompt | Visual Studio |
| --- | --- | --- |
|  &bull;&nbsp;[Aan de slag met C #](./create-first-function-vs-code-csharp.md)<br/>&bull;&nbsp;[Aan de slag met Java](./create-first-function-vs-code-java.md)<br/>&bull;&nbsp;[Aan de slag met Java script](./create-first-function-vs-code-node.md)<br/>&bull;&nbsp;[Aan de slag met Power shell](./create-first-function-vs-code-powershell.md)<br/>&bull;&nbsp;[Aan de slag met python](./create-first-function-vs-code-python.md) |&bull;&nbsp;[Aan de slag met C #](./create-first-function-cli-csharp.md)<br/>&bull;&nbsp;[Aan de slag met Java](./create-first-function-cli-java.md)<br/>&bull;&nbsp;[Aan de slag met Java script](./create-first-function-cli-node.md)<br/>&bull;&nbsp;[Aan de slag met Power shell](./create-first-function-cli-powershell.md)<br/>&bull;&nbsp;[Aan de slag met python](./create-first-function-cli-python.md) | [Aan de slag met C #](functions-create-your-first-function-visual-studio.md) |

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-function-app"></a>Een functie-app maken

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functies. Met een functie-app kunt u functies groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren, schalen en delen.

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Maak vervolgens een functie in de nieuwe functie-app.

## <a name="create-an-http-trigger-function"></a><a name="create-function"></a>Een HTTP-triggerfunctie maken

1. Selecteer in het menu links van het venster **Functies** de optie **Functies** en selecteer vervolgens **Toevoegen** in het bovenste menu. 
 
1. Selecteer **http-trigger** in het venster **nieuwe functie** .

    ![HTTP-triggerfunctie kiezen](./media/functions-create-first-azure-function/function-app-select-http-trigger.png)

1. Accepteer in het venster **nieuwe functie** de standaard naam voor de **nieuwe functie** of voer een nieuwe naam in. 

1. Kies **anoniem** in de vervolg keuzelijst **autorisatie niveau** en selecteer vervolgens **functie maken**.

    Azure maakt de HTTP-activerings functie. U kunt de nieuwe functie nu uitvoeren door een HTTP-aanvraag te verzenden.

## <a name="test-the-function"></a>De functie testen

1. Selecteer in de nieuwe functie HTTP-trigger **code + test** in het menu links en selecteer **functie-URL ophalen** in het bovenste menu.

    ![Functie-URL ophalen selecteren](./media/functions-create-first-azure-function/function-app-select-get-function-url.png)

1. Selecteer in het dialoog venster **functie-URL ophalen** de optie **standaard** in de vervolg keuzelijst en selecteer vervolgens het pictogram **kopiëren naar klem bord** . 

    ![De functie-URL vanuit Azure Portal kopiëren](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

1. Plak de URL van de functie in de adresbalk van uw browser. Voeg de query reeks waarde `?name=<your_name>` toe aan het einde van deze URL en druk op ENTER om de aanvraag uit te voeren. 

    Het volgende voorbeeld toont het antwoord in de browser:

    ![Het antwoord van de functie in de browser.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    Als de aanvraag-URL een [toegangs sleutel](functions-bindings-http-webhook-trigger.md#authorization-keys) ( `?code=...` ) bevat, betekent dit dat u in plaats van het **anonieme** toegangs niveau de **functie** hebt gekozen bij het maken van de functie. In dit geval moet u in plaats daarvan toevoegen `&name=<your_name>` .

1. Wanneer uw functie wordt uitgevoerd, wordt traceringsinformatie naar de logboeken geschreven. Als u de uitvoer van de tracering wilt zien, gaat u terug naar de pagina **code en test** in de portal en vouwt u de pijl **Logboeken** onder aan de pagina uit.

   ![De viewer voor functielogboeken in Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
