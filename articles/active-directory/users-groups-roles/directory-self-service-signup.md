---
title: Self-Service-Registrierung für per E-Mail verifizierte Benutzer – Azure AD | Microsoft-Dokumentation
description: Verwenden der Self-Service-Registrierung in einer Azure Active Directory-Organisation (Azure AD)
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: overview
ms.workload: identity
ms.date: 04/29/2020
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a61bd162baf6f079b625dc07d4faa397493ba618
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87015866"
---
# <a name="what-is-self-service-sign-up-for-azure-active-directory"></a>Was ist die Self-Service-Registrierung für Azure Active Directory?

In diesem Artikel wird erläutert, wie Sie die Self-Service-Registrierung verwenden, um ein Unternehmen in Azure Active Directory (Azure AD) einzugeben. Wenn Sie einen Domänennamen von einem nicht verwalteten Azure AD-Organisation übernehmen möchten, lesen Sie [Übernehmen eines nicht verwalteten Verzeichnisses als Administrator](domains-admin-takeover.md).

## <a name="why-use-self-service-sign-up"></a>Gründe für das Verwenden der Self-Service-Registrierung
* Kunden erhalten schneller die gewünschten Dienste.
* Sie können E-Mail-basierte Angebote für einen Dienst erstellen.
* Sie können E-Mail-basierte Registrierungsabläufe erstellen, mit denen Benutzer schnell Identitäten mithilfe ihrer einfach zu merkenden geschäftlichen E-Mail-Aliase erstellen können.
* Ein per Self-Service erstelltes Azure AD-Verzeichnis kann in ein verwaltetes Verzeichnis konvertiert werden, das für andere Dienste verwendet werden kann.

## <a name="terms-and-definitions"></a>Begriffe und Definitionen
* **Self-Service-Registrierung**: Dies ist die Methode, mit der sich ein Benutzer für einen Clouddienst registriert, wobei für ihn basierend auf seiner E-Mail-Domäne automatisch eine Identität in Azure AD erstellt wird.
* **Nicht verwaltetes Azure-Verzeichnis:** Dies ist das Verzeichnis, in dem die Identität erstellt wird. Ein nicht verwaltetes Verzeichnis ist ein Verzeichnis ohne globalen Administrator.
* **Über E-Mail verifizierter Benutzer:** Dies ist ein Typ von Benutzerkonto in Azure AD. Ein Benutzer, für den nach der Registrierung für ein Self-Service-Angebot automatisch eine Identität erstellt wird, wird als über E-Mail verifizierter Benutzer bezeichnet. Ein über E-Mail verifizierter Benutzer ist ein normales Mitglied eines Verzeichnisses mit der Kennzeichnung "creationmethod=EmailVerified".

## <a name="how-do-i-control-self-service-settings"></a>Wie steuere ich Self-Service-Einstellungen?
Administratoren stehen derzeit zwei Self-Service-Steuerungsmöglichkeiten zur Verfügung. Sie können steuern, ob:

* Benutzer dem Verzeichnis per E-Mail beitreten können.
* Benutzer sich selbst für Anwendungen und Dienste lizenzieren können.

### <a name="how-can-i-control-these-capabilities"></a>Wie können diese Funktionen gesteuert werden?
Ein Administrator kann diese Funktionen mit den folgenden Parametern des Azure AD-Cmdlets „Set-MsolCompanySettings“ konfigurieren:

* **AllowEmailVerifiedUsers** steuert, ob ein Benutzer ein Verzeichnis erstellen oder diesem beitreten kann. Wenn Sie diesen Parameter auf „$false“ festlegen, können dem Verzeichnis keine über E-Mail verifizierten Benutzer beitreten.
* **AllowAdHocSubscriptions** steuert, ob Benutzern das Ausführen der Self-Service-Registrierung erlaubt ist. Wenn Sie diesen Parameter auf „$false“ festlegen, können Benutzer keine Self-Service-Registrierung ausführen.
  
„AllowEmailVerifiedUsers“ und „AllowAdHocSubscriptions“ sind verzeichnisweite Einstellungen, die auf ein verwaltetes oder nicht verwaltetes Verzeichnis angewendet werden können. Hier sehen Sie ein Beispiel:

* Ihr Administrator verwendet ein Verzeichnis mit einer verifizierten Domäne, z. B. „contoso.com“.
* Sie verwenden B2B-Zusammenarbeit aus einem anderen Verzeichnis, um einen Benutzer, der noch nicht vorhanden ist (userdoesnotexist@contoso.com), in das Basisverzeichnis von „contoso.com“ einzuladen.
* Für das Basisverzeichnis ist der Parameter „AllowEmailVerifiedUsers“ aktiviert.

Wenn die oben genannten Bedingungen erfüllt sind, wird ein Mitgliedsbenutzer im Basisverzeichnis und ein B2B-Gastbenutzer im einladenden Verzeichnis erstellt.

Testregistrierungen für Flow und PowerApps werden nicht über die Einstellung **AllowAdHocSubscriptions** gesteuert. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Wie kann ich verhindern, dass Benutzer mit der Verwendung von Power BI beginnen?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
* [Flow in Ihrer Organisation – häufig gestellte Fragen](https://docs.microsoft.com/flow/organization-q-and-a)

### <a name="how-do-the-controls-work-together"></a>Wie funktionieren diese Steuerungsmöglichkeiten zusammen?
Diese beiden Parameter können zusammen verwendet werden, um eine präzisere Steuerung der Self-Service-Registrierung zu erreichen. Der folgende Befehl erlaubt Benutzern beispielsweise die Self-Service-Registrierung, jedoch nur, wenn die Benutzer bereits über ein Konto in Azure AD verfügen (d. h. Benutzern, für die zuerst ein über E-Mail verifiziertes Konto erstellt werden müsste, ist die Self-Service-Registrierung nicht erlaubt):

```powershell
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
```

Im folgenden Flussdiagramm werden die verschiedenen Kombinationen für diese Parameter und die resultierenden Bedingungen für das Verzeichnis und die Self-Service-Registrierung erläutert.

![Flussdiagramm der Steuerelemente für die Self-Service-Registrierung](./media/directory-self-service-signup/SelfServiceSignUpControls.png)

Weitere Informationen und Beispiele zum Verwenden dieser Parameter finden Sie unter [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure AD](../fundamentals/add-custom-domain.md)
* [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/)
* [Azure PowerShell](/powershell/azure/)
* [Azure-Cmdlet-Referenz](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)
* [Schließen Ihres Geschäfts-, Schul- oder Uni-Kontos in einem nicht verwalteten Verzeichnis](users-close-account.md)
