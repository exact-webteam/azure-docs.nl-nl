---
title: Programmatisch Azure-abonnementen maken
description: In dit artikel worden de opties beschreven als u programmatisch Azure-abonnementen wilt maken.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 01/13/2021
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 546ed24b5f9e7892f40c9d425b668f60ad705f8f
ms.sourcegitcommit: 740698a63c485390ebdd5e58bc41929ec0e4ed2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2021
ms.locfileid: "99493885"
---
# <a name="create-azure-subscriptions-programmatically"></a>Azure-abonnementen maken via een programma

In dit artikel worden de opties beschreven als u programmatisch Azure-abonnementen wilt maken.

Met behulp van diverse REST API's kunt u een abonnement maken voor de volgende typen Azure-overeenkomsten:

- Enterprise Agreement (EA)
- Microsoft-klantovereenkomst (MCA)
- Microsoft Partner-overeenkomst (MPA)

Met REST API's kunt u geen extra abonnementen programmatisch maken voor andere typen overeenkomsten.

De vereisten en details voor het maken van abonnementen verschillen per overeenkomst en API-versie. Raadpleeg de volgende artikelen die van toepassing zijn op uw situatie:

Meest recente API's:

- [EA-abonnementen maken](programmatically-create-subscription-enterprise-agreement.md)
- [MCA-abonnementen maken](programmatically-create-subscription-microsoft-customer-agreement.md)
- [MPA-abonnementen maken](programmatically-create-subscription-microsoft-partner-agreement.md)

Als u nog steeds gebruikmaakt van [preview-API's](programmatically-create-subscription-preview.md), kunt u er abonnementen mee blijven maken. 

En u kunt [abonnementen maken met een ARM-sjabloon](create-subscription-template.md). Met een ARM-sjabloon kunt u het maken van abonnementen met REST API's automatiseren. 

## <a name="next-steps"></a>Volgende stappen

* Nadat u een abonnement hebt gemaakt, kunt u ervoor zorgen dat andere gebruikers en service-principals ook deze mogelijkheid hebben. Zie [Toegang verlenen voor het maken van Azure Enterprise-abonnementen (preview)](grant-access-to-create-subscription.md) voor meer informatie.
* Zie [Resources organiseren met Azure-beheergroepen](../../governance/management-groups/overview.md) voor meer informatie over het beheren van grote aantallen abonnementen met behulp van beheergroepen.
