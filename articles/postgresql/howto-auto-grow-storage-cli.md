---
title: Opslag automatisch uitbreiden-Azure CLI-Azure Database for PostgreSQL-één server
description: In dit artikel wordt beschreven hoe u de automatische groei van opslag kunt configureren met behulp van de Azure CLI in Azure Database for PostgreSQL-één server.
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 8/7/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 6b6f3875af4c7f0eace327d810c632a6bb217092
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2020
ms.locfileid: "94534191"
---
# <a name="auto-grow-azure-database-for-postgresql-storage---single-server-using-the-azure-cli"></a>Azure Database for PostgreSQL storage automatisch uitbreiden-één server met behulp van de Azure CLI
In dit artikel wordt beschreven hoe u een Azure Database for PostgreSQL Server-opslag kunt configureren om te groeien zonder dat dit van invloed is op de werk belasting.

De server die [de opslag limiet bereikt](./concepts-pricing-tiers.md#reaching-the-storage-limit), wordt ingesteld op alleen-lezen. Als automatisch verg Roten 100 van de opslag is ingeschakeld, wordt de ingerichte opslag grootte met 5 GB verhoogd zodra de beschik bare opslag ruimte groter is dan 1 GB of 10% van de ingerichte opslag ruimte. Voor servers met meer dan 100 GB ingerichte opslag wordt de ingerichte opslag grootte verhoogd met 5% wanneer de beschik bare opslag ruimte lager is dan 5% van de ingerichte opslag grootte. De maximale opslag limieten die [hier](./concepts-pricing-tiers.md#storage) zijn opgegeven, zijn van toepassing.

## <a name="prerequisites"></a>Vereisten

- U hebt een [Azure database for postgresql-server](quickstart-create-server-database-azure-cli.md)nodig.

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

- Voor dit artikel is versie 2,0 of hoger van de Azure CLI vereist. Als u Azure Cloud Shell gebruikt, is de nieuwste versie al geïnstalleerd.

## <a name="enable-postgresql-server-storage-auto-grow"></a>Automatisch verg Roten van PostgreSQL-Server opslag inschakelen

Schakel de automatische groei van de server in op een bestaande server met de volgende opdracht:

```azurecli-interactive
az postgres server update --name mydemoserver --resource-group myresourcegroup --auto-grow Enabled
```

De automatische groei van de server inschakelen bij het maken van een nieuwe server met de volgende opdracht:

```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --auto-grow Enabled --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen5_2 --version 9.6
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [het maken van waarschuwingen over metrische gegevens](howto-alert-on-metric.md).