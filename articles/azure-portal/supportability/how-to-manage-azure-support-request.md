---
title: Een Azure-ondersteuningsaanvraag beheren
description: Hierin wordt beschreven hoe u ondersteunings aanvragen weergeeft, berichten verzendt, het Ernst niveau van de aanvraag wijzigt, diagnostische gegevens deelt met Azure-ondersteuning, een gesloten ondersteunings aanvraag opnieuw opent en bestanden uploadt.
tags: billing
ms.assetid: 86697fdf-3499-4cab-ab3f-10d40d3c1f70
ms.topic: how-to
ms.date: 12/14/2020
ms.openlocfilehash: d6c68dd341e0794a690b41b73ecc4be954db7359
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2021
ms.locfileid: "100653846"
---
# <a name="manage-an-azure-support-request"></a>Een Azure-ondersteuningsaanvraag beheren

Nadat u [een ondersteunings aanvraag voor Azure hebt gemaakt](how-to-create-azure-support-request.md), kunt u deze beheren in de [Azure Portal](https://portal.azure.com), die in dit artikel wordt besproken. U kunt ook programmatisch aanvragen maken en beheren met behulp van de [Azure-ondersteunings ticket rest API](/rest/api/support)of met behulp van [Azure cli](/cli/azure/azure-cli-support-request).

## <a name="view-support-requests"></a>Ondersteuningsaanvragen weergeven

Bekijk de details en status van ondersteunings aanvragen door naar **Help en ondersteuning** te gaan voor  >   **alle ondersteunings aanvragen**.

:::image type="content" source="media/how-to-manage-azure-support-request/all-requests-lower.png" alt-text="Alle ondersteunings aanvragen":::

Op deze pagina kunt u ondersteunings aanvragen zoeken, filteren en sorteren. Selecteer een ondersteunings aanvraag om details weer te geven, inclusief de ernst en eventuele berichten die aan de aanvraag zijn gekoppeld.

## <a name="send-a-message"></a>Een bericht verzenden

1. Selecteer op de pagina **alle ondersteunings aanvragen** de ondersteunings aanvraag.

1. Selecteer op de pagina **ondersteunings aanvraag** **nieuwe berichten**.

1. Voer uw bericht in en selecteer **verzenden**.

## <a name="change-the-severity-level"></a>Het Ernst niveau wijzigen

> [!NOTE]
> Het maximale Ernst niveau is afhankelijk van uw [ondersteunings plan](https://azure.microsoft.com/support/plans).
>

1. Selecteer op de pagina **alle ondersteunings aanvragen** de ondersteunings aanvraag.

1. Selecteer op de pagina **ondersteunings aanvraag** **wijzigen**.

    :::image type="content" source="media/how-to-manage-azure-support-request/change-severity.png" alt-text="Ernst van de ondersteunings aanvraag wijzigen":::

1. De Azure Portal toont een van de twee schermen, afhankelijk van het feit of uw aanvraag al is toegewezen aan een ondersteunings technicus:

    - Als uw aanvraag niet is toegewezen, ziet u een scherm zoals het volgende. Selecteer een nieuw Ernst niveau en selecteer vervolgens **wijzigen**.

        :::image type="content" source="media/how-to-manage-azure-support-request/unassigned-can-change-severity.png" alt-text="Een nieuw Ernst niveau selecteren":::

    - Als uw aanvraag is toegewezen, ziet u een scherm zoals het volgende. Selecteer **OK** en vervolgens een [Nieuw bericht](#send-a-message) om een wijziging in het Ernst niveau aan te vragen.

        :::image type="content" source="media/how-to-manage-azure-support-request/assigned-cant-change-severity.png" alt-text="Kan geen nieuw Ernst niveau selecteren":::

## <a name="share-diagnostic-information-with-azure-support"></a>Diagnostische gegevens delen met ondersteuning voor Azure

Wanneer u een ondersteunings aanvraag maakt, wordt standaard de optie **Diagnostische gegevens delen** geselecteerd. Hierdoor kan ondersteuning voor Azure [Diagnostische gegevens](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) van uw Azure-resources verzamelen:

* U kunt deze optie niet wissen nadat een aanvraag is gemaakt.

* Als u de optie voor het maken van een aanvraag hebt uitgeschakeld, kunt u deze selecteren nadat de aanvraag is gemaakt.

    1. Selecteer op de pagina **alle ondersteunings aanvragen** de ondersteunings aanvraag.
    
    1. Selecteer op de pagina **ondersteunings aanvraag** de optie **machtiging verlenen** en selecteer vervolgens **Ja** en **OK**.
    
        :::image type="content" source="media/how-to-manage-azure-support-request/grant-permission-manage.png" alt-text="Machtigingen verlenen voor diagnostische gegevens":::

## <a name="upload-files"></a>Bestanden uploaden

U kunt de optie voor het uploaden van bestanden gebruiken om diagnostische bestanden of andere bestanden te uploaden die relevant zijn voor een ondersteunings aanvraag.

1. Selecteer op de pagina **alle ondersteunings aanvragen** de ondersteunings aanvraag.

1. Blader op de pagina **ondersteunings aanvraag** naar het bestand en selecteer vervolgens **uploaden**. Herhaal dit proces als u meerdere bestanden hebt.

    :::image type="content" source="media/how-to-manage-azure-support-request/file-upload.png" alt-text="Bestand uploaden":::

### <a name="file-upload-guidelines"></a>Richt lijnen voor het uploaden van bestanden

Volg deze richt lijnen wanneer u de optie voor het uploaden van bestanden gebruikt:

* Ter bescherming van uw privacy neemt u geen persoonlijke gegevens op in uw Upload.
* De bestands naam mag niet langer zijn dan 110 tekens.
* U kunt niet meer dan één bestand uploaden.
* Bestanden mogen niet groter zijn dan 4 MB.
* Alle bestanden moeten een bestandsnaam extensie hebben, zoals *. docx* of *. xlsx*. In de volgende tabel ziet u de bestands extensies die mogen worden geüpload.

| 0-9, A-C     | D-G   | H-M         | N-P   | R-T      | U-W        | X-Z     |
|-------------|-------|-------------|-------|----------|------------|---------|
| .7z         | . dat  | . har        | . odx  | . rar     | .tdb       | .xlam   |
| . een          | . db   | .hwl        | . oft  | . rdl     | .tdf       | .xlr    |
| . ABC        | . DMP  | . ICS        | . old  | .rdlc    | . tekst      | .xls    |
| . adm        | .do_  | . ini        | . One  | .re_     | .thmx      | .xlsb   |
| . aspx       | .doc  | .java       | . OSD  | . reg     | .tif       | .xlsm   |
| . ATF        | .docm | .jpg        | . AF  | . Verwijder  | . TRC       | .xlsx   |
| . b          | .docx | . LDF        | . P1   | . ren     | . TTD       | .xlt    |
| .ba_        | .dotm | . brief hoofd | .pcap | . rename  | .tx_       | .xltx   |
| . bak        | .dotx | . lnk        | . pdb  | .rft     | .txt       | .xml    |
| type        | .dtsx | .lo_        | .pdf  | . RPT     | .uccapilog | . XMLA   |
| . blg        | . eds  | . log        | .piz  | .rte     | .uccplog   | .xps    |
| .CA_        | . EMF  | . lpk        | .pmls | .rtf     | .udcx      | . XSD    |
| . -        | . eml  | . manifest   | .png  | . Voer     | .vb_       | . XSN    |
| . Cap        | .emz  | . Master     | .potx | .saz     | .vbs_      | . xxx    |
| .catx       | . err  | .mdmp       | .ppt  | .sql     | . vcf       | .z_     |
| . CFG        | . etl  | . MOF        | .pptm | .sqlplan | . vsd       | .z01    |
| . gecomprimeerd | . evt  | .mp3        | .pptx | STP     | . wdb       | .z02    |
| . Configuraties     | . evtx | .mpg        | . prn  | .svclog  | . WKS       | . Zi     |
| .cpk        | . KADE   | .ms_        | .psf  | -        | .wma       | .zi_    |
| . cpp        | .ex_  | . msg        | . PST  | -        | .wmv       | .zip    |
| .cs         | .ex0  | .msi        | . pub  | -        | . WMZ       | .zip_   |
| . BESTAND        | . FRD  | . mso        | -     | -        | . WPS       | .zipp   |
| .cvr        | .gif  | MSU        | -     | -        | . WPT       | . gezipt |
| -           | . GUID | . nfo        | -     | -        | . WSDL      | .zippy  |
| -           | . gz   | -           | -     | -        | . WSP       | .zipx   |
| -           | -     | -           | -     | -        | .wtl       | .zit    |
| -           | -     | -           | -     | -        | -          | .zix    |
| -           | -     | -           | -     | -        | -          | . zzz    |

## <a name="close-a-support-request"></a>Een ondersteunings aanvraag sluiten

Als u een ondersteunings aanvraag wilt sluiten, [verzendt u een bericht](#send-a-message) waarin wordt gevraagd of de aanvraag moet worden gesloten.

## <a name="reopen-a-closed-request"></a>Een gesloten aanvraag opnieuw openen

Als u een gesloten ondersteunings aanvraag opnieuw wilt openen, maakt u een [Nieuw bericht](#send-a-message), waarmee de aanvraag automatisch opnieuw wordt geopend.

## <a name="cancel-a-support-plan"></a>Een ondersteuningsplan annuleren

Zie [een ondersteunings plan annuleren](../../cost-management-billing/manage/cancel-azure-subscription.md#cancel-a-support-plan)als u een ondersteunings plan wilt annuleren.

## <a name="next-steps"></a>Volgende stappen

[Een ondersteuningsaanvraag maken voor Azure](how-to-create-azure-support-request.md)

[REST API voor Azure-ondersteuningstickets](/rest/api/support)
