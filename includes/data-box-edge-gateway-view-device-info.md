---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/04/2019
ms.author: alkohli
ms.openlocfilehash: 3312d1ec7c2535e103cf8959599c0d4c3014f520
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "86218151"
---
1. [Verbinding maken met de Power shell-interface](#connect-to-the-powershell-interface).
2. Gebruik de `Get-HcsApplianceInfo` om de informatie voor uw apparaat op te halen.

    In het volgende voor beeld ziet u het gebruik van deze cmdlet:

    ```
    [10.100.10.10]: PS>Get-HcsApplianceInfo
    
    Id                            : b2044bdb-56fd-4561-a90b-407b2a67bdfc
    FriendlyName                  : DBE-NBSVFQR94S6
    Name                          : DBE-NBSVFQR94S6
    SerialNumber                  : HCS-NBSVFQR94S6
    DeviceId                      : 40d7288d-cd28-481d-a1ea-87ba9e71ca6b
    Model                         : Virtual
    FriendlySoftwareVersion       : Data Box Gateway 1902
    HcsVersion                    : 1.4.771.324
    IsClustered                   : False
    IsVirtual                     : True
    LocalCapacityInMb             : 1964992
    SystemState                   : Initialized
    SystemStatus                  : Normal
    Type                          : DataBoxGateway
    CloudReadRateBytesPerSec      : 0
    CloudWriteRateBytesPerSec     : 0
    IsInitialPasswordSet          : True
    FriendlySoftwareVersionNumber : 1902
    UploadPolicy                  : All
    DataDiskResiliencySettingName : Simple
    ApplianceTypeFriendlyName     : Data Box Gateway
    IsRegistered                  : False
    ```

    Hier volgt een tabel met een overzicht van een aantal belang rijke informatie over apparaten:

    | Parameter | Beschrijving |
    |-----------|-------------|
    | FriendlyName                   | De beschrijvende naam van het apparaat zoals geconfigureerd via de lokale web-UI tijdens de implementatie van het apparaat. De standaard beschrijvende naam is het serie nummer van het apparaat.  |
    | SerialNumber                   | Het serie nummer van het apparaat is een uniek nummer dat is toegewezen aan de Factory.                                                                             |
    | Modelleren                          | Het model voor uw Azure Stack rand of Data Box Gateway apparaat. Het model is fysiek voor Azure Stack Edge en virtueel voor Data Box Gateway.                   |
    | FriendlySoftwareVersion        | De beschrijvende teken reeks die overeenkomt met de versie van de apparaatsoftware. Voor een systeem met Preview wordt de beschrijvende software versie Data Box Edge 1902. |
    | HcsVersion                     | De versie van de HCS-software die op uw apparaat wordt uitgevoerd. De HCS-software versie die overeenkomt met Data Box Edge 1902 is bijvoorbeeld 1.4.771.324.            |
    | LocalCapacityInMb              | De totale lokale capaciteit van het apparaat in megabits.                                                                                                        |
    | IsRegistered                   | Deze waarde geeft aan of uw apparaat is geactiveerd met de service.                                                                                         |


