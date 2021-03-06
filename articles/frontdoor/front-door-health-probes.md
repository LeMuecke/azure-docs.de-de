---
title: 'Azure Front Door: Überwachung der Back-End-Integrität | Microsoft-Dokumentation'
description: In diesem Artikel wird erläutert, wie Azure Front Door die Integrität Ihrer Back-Ends überwacht.
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: e2e656c395f1a31c1f5ebbd46d5a18a046f854f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471573"
---
# <a name="health-probes"></a>Integritätstests

Um die Integrität und Entfernung der einzelnen Back-Ends von einer bestimmten Front Door-Umgebung zu ermitteln, sendet jede Front Door-Umgebung in regelmäßigen Abständen eine synthetische HTTP-/HTTPS-Anforderung an jedes konfigurierte Back-End. Anschließend ermittelt Front Door anhand der Antworten aus diesen Tests die „besten“ Back-Ends für die Weiterleitung echter Clientanforderungen. 

> [!WARNING]
> Da Front Door weltweit viele Edge-Umgebungen hat, kann das Volumen der Integritätstestanforderungen an Ihre Back-Ends recht hoch sein. Es variiert zwischen 25 Anforderungen pro Minute und bis zu 1.200 Anforderungen pro Minute abhängig von der konfigurierten Häufigkeit von Integritätstests. Bei der standardmäßigen Testfrequenz von 30 Sekunden sollte das Testvolumen in Ihrem Back-End etwa 200 Anforderungen pro Minute betragen.

## <a name="supported-protocols"></a>Unterstützte Protokolle

Front Door unterstützt das Senden von Tests über die Protokolle HTTP und HTTPS. Diese Tests werden über dieselben TCP-Ports gesendet, die für das Routen von Clientanforderungen konfiguriert wurden. Sie können nicht außer Kraft gesetzt werden.

## <a name="supported-http-methods-for-health-probes"></a>Für Integritätstests unterstützte HTTP-Methoden

Front Door unterstützt die folgenden HTTP-Methoden zum Senden der Integritätstests:

1. **GET:** Die GET-Methode bedeutet, dass alle Informationen (in Form einer Entität), die vom Anforderungs-URI identifiziert werden, abgerufen werden.
2. **HEAD:** Die HEAD-Methode ist identisch mit GET, außer dass der Server in der Antwort KEINEN Nachrichtentext zurückgeben darf. Bei neuen Front Door-Profilen ist die Testmethode standardmäßig auf HEAD festgelegt.

> [!NOTE]
> Um die Belastung und Kosten Ihrer Back-Ends zu senken, empfiehlt Front Door die Verwendung von HEAD-Anforderungen für Integritätstests.

## <a name="health-probe-responses"></a>Integritätstestantworten

| Antworten  | BESCHREIBUNG | 
| ------------- | ------------- |
| Ermitteln der Integrität  |  Der Statuscode „200 OK“ gibt an, dass das Back-End fehlerfrei ist. Alle anderen Antworten werden als Fehler betrachtet. Wenn für einen Test aus irgendeinem Grund (einschließlich Netzwerkausfall) keine gültige HTTP-Antwort eingeht, gilt der Test als fehlerhaft.|
| Messen der Latenzzeit  | Latenz ist die Gesamtbetrachtungszeit, gemessen ab dem Zeitpunkt unmittelbar vor dem Senden der Testanforderung bis zu dem Moment, in dem wir das letzte Byte der Antwort erhalten. Wir verwenden für jede Anforderung eine neue TCP-Verbindung, damit diese Messung nicht zugunsten von Back-Ends mit vorhandenen betriebsbereiten Verbindungen ausfällt.  |

## <a name="how-front-door-determines-backend-health"></a>Vorgehensweise beim Ermitteln der Back-End-Integrität mit Front Door

Azure Front Door verwendet in allen Algorithmen denselben aus drei Schritten bestehenden Prozess, um die Integrität zu bestimmen.

1. Deaktivierte Back-Ends werden ausgeschlossen.

2. Back-Ends mit fehlerhaften Integritätstests werden ausgeschlossen:
    * Diese Auswahl erfolgt anhand der letzten _n_ Integritätstestantworten. Wenn mindestens _x_ fehlerfrei sind, wird das Back-End als fehlerfrei betrachtet.

    * _n_ wird durch Ändern der SampleSize-Eigenschaft in den Einstellungen für den Lastenausgleich konfiguriert.

    * _x_ wird durch Ändern der SuccessfulSamplesRequired-Eigenschaft in den Einstellungen für den Lastenausgleich konfiguriert.

3. Aus der Menge der fehlerfreien Back-Ends im Back-End-Pool misst und verwaltet Front Door zusätzlich die Latenz (Roundtripzeit) für jedes Back-End.


## <a name="complete-health-probe-failure"></a>Vollständiger Integritätstestfehler

Wenn Integritätstests für alle Back-Ends in einem Back-End-Pool fehlerhaft sind, betrachtet Front Door alle Back-Ends als fehlerfrei und routet den Datenverkehr in einer Roundrobinverteilung über alle Back-Ends.

Sobald ein Back-End zu einem fehlerfreien Integritätsstatus zurückkehrt, wird der normale Lastenausgleichsalgorithmus von Front Door fortgesetzt.

## <a name="disabling-health-probes"></a>Deaktivieren von Integritätstests

Wenn Sie ein einzelnes Back-End in Ihrem Back-End-Pool haben, können Sie die Integritätstests deaktivieren, um die Last Ihres Anwendungs-Back-Ends zu verringern. Selbst wenn Sie über mehrere Back-Ends im Back-End-Pool verfügen, aber nur eines davon im aktivierten Zustand ist, können Sie Integritätstests deaktivieren.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Front Door-Instanz](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).
