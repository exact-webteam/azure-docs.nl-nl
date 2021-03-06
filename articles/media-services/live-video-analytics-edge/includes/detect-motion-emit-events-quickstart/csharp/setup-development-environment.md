---
ms.openlocfilehash: 2afe3e29c5717eb8d4921f82f35083ecbb14828c
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2021
ms.locfileid: "99531436"
---
1. Kloon de opslagplaats vanuit deze locatie: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp.
1. Open in Visual Studio Code de map waarin de opslagplaats is gedownload.
1. Ga in Visual Studio Code naar de map *src/cloud-to-device-console-app*. Maak daar een bestand en geef het de naam *appsettings.json*. Dit bestand bevat de instellingen die nodig zijn om het programma uit te voeren.
1. Kopieer de inhoud van het bestand *~/clouddrive/lva-sample/appsettings.json* dat u eerder in deze quickstart hebt gegenereerd.

    De tekst moet lijken op de volgende uitvoer.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. Ga naar de map *src/edge* en maak een bestand met de naam *.env*.
1. Kopieer de inhoud van het bestand */clouddrive/lva-sample/edge-deployment/.env*. De tekst moet lijken op de volgende code.

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    VIDEO_INPUT_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    VIDEO_OUTPUT_FOLDER_ON_DEVICE="/var/media"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"      
    ```
    