---
title: Lizenzanforderung für die Verwendung von Privileged Identity Management – Azure Active Directory | Microsoft-Dokumentation
description: Beschreibt die Lizenzierungsanforderungen für die Verwendung von Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 01/10/2020
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 42d931206e99516e0320d0cbedd0812389ee41aa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84742163"
---
# <a name="license-requirements-to-use-privileged-identity-management"></a>Lizenzanforderungen für die Verwendung von Privileged Identity Management

Damit Sie Azure Active Directory (Azure AD) Privileged Identity Management (PIM) verwenden können, muss das Verzeichnis über eine gültige Lizenz verfügen. Darüber hinaus müssen den Administratoren und den erforderlichen Benutzern Lizenzen zugewiesen werden. In diesem Artikel werden die Lizenzanforderungen für die Verwendung von Privileged Identity Management beschrieben.

## <a name="valid-licenses"></a>Gültige Lizenzen

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="how-many-licenses-must-you-have"></a>Wie viele Lizenzen benötigen Sie?

Stellen Sie sicher, dass Ihr Verzeichnis über mindestens so viele Azure AD Premium P2-Lizenzen verfügt, wie Sie Mitarbeiter haben, die die folgenden Aufgaben ausführen:

- Als für Azure AD-Rollen berechtigt zugewiesene Benutzer, die mit PIM verwaltet werden
- Benutzer mit der Berechtigung zum Genehmigen oder Ablehnen von Aktivierungsanforderungen in PIM
- Benutzer mit einer Azure-Ressourcenrolle mit Just-in-Time- oder direkten (zeitbasierten) Zuweisungen  
- Benutzer mit einer Zugriffsüberprüfung
- Benutzer, die Zugriffsüberprüfungen ausführen

Für die folgenden Aufgaben sind **keine** Azure AD Premium P2-Lizenzen erforderlich:

- Für Benutzer mit den Rollen „Globaler Administrator“ oder „Administrator für privilegierte Rollen“, die PIM einrichten, Richtlinien konfigurieren, Warnungen empfangen oder Zugriffsüberprüfungen einrichten, sind keine Lizenzen erforderlich.

Weitere Informationen zu Lizenzen finden Sie unter [Zuweisen oder Entfernen von Lizenzen im Azure Active Directory-Portal](../fundamentals/license-users-groups.md).

## <a name="example-license-scenarios"></a>Beispielszenarien für Lizenzen

Anhand der folgenden Beispielszenarien für Lizenzen können Sie die Anzahl der benötigten Lizenzen bestimmen.

| Szenario | Berechnung | Anzahl der Lizenzen |
| --- | --- | --- |
| Die Woodgrove Bank hat 10 Administratoren für verschiedene Abteilungen und 2 globale Administratoren, die PIM konfigurieren und verwalten. Fünf Administratoren erhalten eine Berechtigung. | Fünf Lizenzen für die Administratoren, die berechtigt sind | 5 |
| Das Graphic Design Institute hat 25 Administratoren, von denen 14 über PIM verwaltet werden. Für die Rollenaktivierung ist eine Genehmigung erforderlich, und es gibt drei verschiedene Benutzer in der Organisation, die Aktivierungen genehmigen können. | 14 Lizenzen für die berechtigten Rollen + drei genehmigende Personen | 17 |
| Contoso hat 50 Administratoren, von denen 42 über PIM verwaltet werden. Für die Rollenaktivierung ist eine Genehmigung erforderlich, und es gibt fünf verschiedene Benutzer in der Organisation, die Aktivierungen genehmigen können. Außerdem überprüft Contoso monatlich die Benutzer, denen Administratorrollen zugewiesen sind. Bei den Prüfern handelt es sich um die Manager der Benutzer, von denen sechs keine von PIM verwaltete Administratorrolle haben. | 42 Lizenzen für die berechtigten Rollen + fünf genehmigende Personen + sechs Prüfer | 53 |

## <a name="what-happens-when-a-license-expires"></a>Was passiert, wenn eine Lizenz abläuft?

Wenn eine Lizenz vom Typ Azure AD Premium P2 oder EMS E5 bzw. eine Testlizenz abläuft, stehen die Funktionen von Privileged Identity Management in Ihrem Verzeichnis nicht mehr zur Verfügung:

- Permanente Rollenzuweisungen für Azure AD-Rollen sind nicht betroffen.
- Der Privileged Identity Management-Dienst im Azure-Portal sowie die Graph-API-Cmdlets und PowerShell-Schnittstellen von Privileged Identity Management sind für Benutzer nicht mehr verfügbar, um privilegierte Rollen zu aktivieren, den privilegierten Zugriff zu verwalten oder Zugriffsüberprüfungen privilegierter Rollen auszuführen.
- Berechtigte Rollenzuweisungen von Azure AD-Rollen werden entfernt, da Benutzer keine privilegierten Rollen mehr aktivieren können.
- Alle laufenden Zugriffsüberprüfungen von Azure AD-Rollen werden beendet, und die Privileged Identity Management-Konfigurationseinstellungen werden entfernt.
- Privileged Identity Management sendet keine E-Mails mehr zu Rollenzuweisungsänderungen.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von Azure AD Privileged Identity Management (PIM)](pim-deployment-plan.md)
- [Einstieg in Privileged Identity Management](pim-getting-started.md)
- [Rollen, die nicht in Privileged Identity Management verwaltet werden können](pim-roles.md)
