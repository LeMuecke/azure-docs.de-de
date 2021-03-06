---
title: Synchronisieren der Netzwerkkonfiguration für Azure App Service
titleSuffix: Azure SQL Managed Instance
description: In diesem Artikel wird erläutert, wie Sie die Netzwerkkonfiguration für den Azure App Service-Hostingplan mit Ihrer Instanz von Azure SQL Managed Instance synchronisieren.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 12/13/2018
ms.openlocfilehash: a0192f204fdb0797d98947a174bf6d669034b666
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84695452"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan-with-azure-sql-managed-instance"></a>Synchronisieren der Netzwerkkonfiguration für den Azure App Service-Hostingplan mit Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Möglicherweise können Sie trotz vorhandener [Integration Ihrer App mit einem virtuellen Azure-Netzwerk](../../app-service/web-sites-integrate-with-vnet.md) keine Verbindung mit SQL Managed Instance herstellen. Sie können dieses Problem möglicherweise beheben, indem Sie die Netzwerkkonfiguration für Ihren Diensttarif aktualisieren oder synchronisieren. 

## <a name="sync-network-configuration"></a>Synchronisieren der Netzwerkkonfiguration 

Gehen Sie dazu folgendermaßen vor:  

1. Navigieren Sie zum App Service-Plan für Ihre Web-Apps.

   ![Screenshot des App Service-Plans](./media/azure-app-sync-network-configuration/app-service-plan.png)

2. Klicken Sie auf **Netzwerk** und dann auf **Klicken Sie zur Verwaltung hier**.

   ![Screenshot: Verwalten des Diensttarifs](./media/azure-app-sync-network-configuration/manage-plan.png)

3. Wählen Sie Ihr **VNET** aus, und klicken Sie auf **Netzwerk synchronisieren**.

   ![Screenshot: Synchronisierungsnetzwerk](./media/azure-app-sync-network-configuration/sync.png)

4. Warten Sie, bis die Synchronisierung abgeschlossen ist.
  
   ![Screenshot: abgeschlossene Synchronisierung](./media/azure-app-sync-network-configuration/sync-done.png)

Sie können jetzt versuchen, die Verbindung mit der Instanz von SQL Managed Instance erneut herzustellen.

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Konfigurieren Ihres VNET für SQL Managed Instance finden Sie unter [VNET-Architektur in SQL Managed Instance](connectivity-architecture-overview.md) und [Konfigurieren eines vorhandenen VNET](vnet-existing-add-subnet.md).
