---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: fe5daa38c43723c85fb464e191ee4a3e85700e0b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2020
ms.locfileid: "67175991"
---
1. Wanneer de virtuele Azure-machine is gemaakt en wordt uitgevoerd, klikt u in Azure Portal op het pictogram Virtuele machines om uw VM's te bekijken.

1. Klik op het beletselteken (**...**) voor de nieuwe virtuele machine.

1. Klik op **Verbinding maken**.

   ![In de portal verbinding maken met de virtuele machine](./media/virtual-machines-sql-server-remote-desktop-connect/azure-virtual-machine-connect.png)

1. Open het **RDP**-bestand dat in de browser is gedownload voor de VM.

1. Verbinding met extern bureaublad meldt dat de uitgever van deze externe verbinding niet kan worden geïdentificeerd. Klik op **Verbinden** om door te gaan.

1. Klik in het dialoogvenster **Windows-beveiliging** op **Een ander account gebruiken**. Mogelijk moet u op **Meer opties** klikken om de optie weer te geven. Geef de gebruikersnaam en het wachtwoord op die u hebt geconfigureerd tijdens het maken van de VM. U moet een backslash toevoegen voor de gebruikersnaam.

   ![Extern bureaublad-verificatie](./media/virtual-machines-sql-server-remote-desktop-connect/remote-desktop-connect.png)

1. Klik op **OK** om verbinding te maken.
