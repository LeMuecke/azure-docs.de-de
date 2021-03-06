---
title: include file
description: include file
services: azure-resource-manager
author: tfitzmac
ms.service: cost-management-billing
ms.topic: include
ms.date: 03/26/2020
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 94fd7e692be31ba247e3342246d3940ed08ef9b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334984"
---
| Resource | Begrenzung |
| --- | --- |
| Abonnements pro Azure Active Directory-Mandant | Unbegrenzt. |
| [Co-Admins](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) pro Abonnement |Unbegrenzt. |
| [Ressourcengruppen](../articles/azure-resource-manager/management/overview.md) pro Abonnement |980 |
| Anforderungsgröße für Azure Resource Manager-API |4\.194.304 Byte |
| Tags pro Abonnement<sup>1</sup> |50 |
| Eindeutige Tag-Berechnungen pro Abonnement<sup>1</sup> | 10.000 |
| [Bereitstellungen auf Abonnementebene](../articles/azure-resource-manager/templates/deploy-to-subscription.md) pro Standort | 800<sup>2</sup> |

<sup>1</sup> Sie können bis zu 50 Tags direkt auf ein Abonnement anwenden. Das Abonnement kann jedoch eine unbegrenzte Anzahl von Tags enthalten, die auf Ressourcengruppen und Ressourcen innerhalb des Abonnements angewendet werden. Die Anzahl von Tags pro Ressource oder Ressourcengruppe ist auf 50 beschränkt. Resource Manager gibt nur eine [Liste mit eindeutigen Tagnamen und Werten](/rest/api/resources/tags) im Abonnement zurück, wenn die Anzahl der Tags 10.000 oder weniger beträgt. Sie können eine Ressource jedoch auch anhand des Tags finden, wenn die Anzahl 10.000 überschreitet.  

<sup>2</sup>Wenn der Grenzwert von 800 Bereitstellungen erreicht ist, löschen Sie nicht mehr benötigte Bereitstellungen aus dem Verlauf. Bereitstellungen auf Abonnementebene können mithilfe von [Remove-AzDeployment](/powershell/module/az.resources/Remove-AzDeployment) oder [az deployment sub delete](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-delete) gelöscht werden.
