---
title: Konzepte – rollenbasierte Zugriffssteuerung (RBAC)
description: Erfahren Sie mehr über die wichtigsten Funktionen der rollenbasierten Zugriffssteuerung für Azure VMware Solution (AVS)
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 8628c88c300ef8ed271f5e06a8e8dfae40231fec
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85656754"
---
# <a name="role-based-access-control-rbac-for-azure-vmware-solution-avs"></a>Rollenbasierte Zugriffssteuerung (RBAC) für Azure VMware Solution (AVS)

In einer lokalen Bereitstellung von vCenter und ESXi hat der Administrator Zugriff auf das vCenter-Konto administrator@vsphere.local und kann zusätzliche Active Directory (AD)-Benutzer/-Gruppen zugewiesen haben. In einer Bereitstellung von Azure VMware Solution (AVS) hat der Administrator hingegen keinen Zugriff auf das Administratorbenutzerkonto, kann aber AD-Benutzer und -Gruppen zur CloudAdmin-Rolle in vCenter zuweisen.  Außerdem sind die Private Cloud-Benutzer von AVS nicht berechtigt, auf bestimmte von Microsoft unterstützte und verwaltete Verwaltungskomponenten zuzugreifen oder diese zu konfigurieren, wie z. B. Cluster, Hosts, Datenspeicher und verteilte virtuelle Netzwerke.


In AVS verfügt vCenter über einen integrierten lokalen Benutzer namens „cloudadmin“, der der integrierten CloudAdmin-Rolle zugewiesen ist. Der lokale cloudadmin-Benutzer wird zum Einrichten weiterer Benutzer in AD verwendet. Die CloudAdmin-Rolle verfügt im Allgemeinen über die Berechtigung zum Erstellen und Verwalten von Workloads in Ihrer privaten Cloud (VMs, Ressourcenpools, Datenspeicher und Netzwerke). Die CloudAdmin-Rolle in AVS verfügt über eine bestimmte Menge von vCenter-Berechtigungen, die von anderen VMware-Cloudlösungen abweicht.   

> [!NOTE]
> AVS bietet derzeit keine benutzerdefinierten Rollen in vCenter oder im AVS-Portal. 

## <a name="avs-cloudadmin-role-on-vcenter"></a>Rolle „AVS CloudAdmin“ in vCenter

Sie können die Berechtigungen anzeigen, die der Rolle „AVS CloudAdmin“ in vCenter Ihrer privaten AVS-Cloud erteilt wurden.

1. Melden Sie sich beim vSphere-Client des SDDC an, und gehen Sie zu **Menü** > **Verwaltung**.
1. Wählen Sie unter **Zugriffssteuerung** die Option **Rollen** aus.
1. Wählen Sie in der Liste der Rollen den Eintrag **CloudAdmin** aus, und wählen Sie dann **Berechtigungen** aus. 

   :::image type="content" source="media/rbac-cloudadmin-role-privileges.png" alt-text="Anzeigen der Berechtigungen der CloudAdmin-Rolle im vSphere-Client":::

Die CloudAdmin-Rolle in AVS verfügt über die folgenden Berechtigungen für vCenter. Eine ausführliche Erläuterung der einzelnen Berechtigungen finden Sie in der [VMware-Produktdokumentation](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html).

| Berechtigung | BESCHREIBUNG |
| --------- | ----------- |
| **Warnungen** | Warnung bestätigen<br />Warnung erstellen<br />Warnungsaktion deaktivieren<br />Warnung ändern<br />Warnung entfernen<br />Warnungsstatus festlegen |
| **Berechtigungen** | Berechtigungen ändern |
| **Inhaltsbibliothek** | Bibliothekselement hinzufügen<br />Abonnement für eine veröffentlichte Bibliothek erstellen<br />Lokale Bibliothek erstellen<br />Abonnierte Bibliothek erstellen<br />Bibliothekselement löschen<br />Lokale Bibliothek löschen<br />Abonnierte Bibliothek löschen<br />Abonnement für eine veröffentlichte Bibliothek löschen<br />Herunterladen von Dateien<br />Bibliothekselemente entfernen<br />Abonnierte Bibliothek entfernen<br />Speicher importieren<br />Abonnementinformationen testen<br />Bibliothekselement für seine Abonnenten veröffentlichen<br />Bibliothek für ihre Abonnenten veröffentlichen<br />Speicher lesen<br />Bibliothekselement synchronisieren<br />Abonnierte Bibliothek synchronisieren<br />Typintrospektion<br />Konfigurationseinstellungen aktualisieren<br />Dateien aktualisieren<br />Bibliothek aktualisieren<br />Bibliothekselement aktualisieren<br />Lokale Bibliothek aktualisieren<br />Abonnierte Bibliothek aktualisieren<br />Abonnement einer veröffentlichten Bibliothek aktualisieren<br />Konfigurationseinstellungen anzeigen |
| **Kryptografische Vorgänge** | Direktzugriff |
| **Datenspeicher** | Speicherplatz zuweisen<br />Durchsuchen des Datenspeichers<br />Datenspeicher konfigurieren<br />Low-Level-Dateivorgänge<br />Dateien entfernen<br />Metadaten virtueller Computer aktualisieren |
| **Ordner** | Ordner erstellen<br />Ordner löschen<br />Ordner verschieben<br />Ordner umbenennen |
| **Global** | Task abbrechen<br />Globales Tag<br />Health<br />Ereignis protokollieren<br />Benutzerdefinierte Attribute definieren<br />Dienst-Manager<br />Benutzerdefiniertes Attribut festlegen<br />Systemtag |
| **Host** | vSphere-Replikation<br />&#160;&#160;&#160;&#160;Replikation verwalten |
| **vSphere-Tagging** | vSphere-Tag zuweisen und Zuweisung aufheben<br />vSphere-Tag erstellen<br />vSphere-Tagkategorie erstellen<br />vSphere-Tag löschen<br />vSphere-Tagkategorie löschen<br />vSphere-Tag bearbeiten<br />vSphere-Tagkategorie bearbeiten<br />Feld „UsedBy“ für Kategorie ändern<br />Feld „UsedBy“ für Tag ändern |
| **Network** | Netzwerk zuweisen |
| **Ressource** | Anwenden einer Empfehlung<br />vApp dem Ressourcenpool zuweisen<br />Virtuellen Computer dem Ressourcenpool zuweisen<br />Ressourcenpool erstellen<br />Ausgeschalteten virtuellen Computer migrieren<br />Eingeschalteten virtuellen Computer migrieren<br />Ressourcenpool ändern<br />Ressourcenpool entfernen<br />vMotion abfragen<br />Ressourcenpool entfernen<br />Ressourcenpool umbenennen |
| **Geplante Aufgabe** | Erstellen der Aufgabe<br />Aufgabe ändern<br />Aufgabe entfernen<br />Aufgabe ausführen |
| **Sitzungen** | `Message`<br />Sitzung überprüfen |
| **Profil** | Profilgesteuerte Speicheransicht |
| **Speicheransicht** | Sicht |
| **vApp** | Virtuellen Computer hinzufügen<br />Ressourcenpool zuweisen<br />vApp zuweisen<br />Klon<br />Erstellen<br />Löschen<br />Exportieren<br />Importieren<br />Move<br />Ausschalten<br />Einschalten<br />Umbenennen<br />Angehalten<br />Unregister<br />OVF-Umgebung anzeigen<br />vApp-Anwendungskonfiguration<br />vApp-Instanzkonfiguration<br />vApp-managedBy-Konfiguration<br />vApp-Ressourcenkonfiguration |
| **Virtueller Computer** | Konfiguration ändern<br />&#160;&#160;&#160;&#160;Leasedauer des Datenträgers abrufen<br />&#160;&#160;&#160;&#160;Vorhandenen Datenträger hinzufügen<br />&#160;&#160;&#160;&#160;Neuen Datenträger hinzufügen<br />&#160;&#160;&#160;&#160;Gerät hinzufügen oder entfernen<br />&#160;&#160;&#160;&#160;Erweiterte Konfiguration<br />&#160;&#160;&#160;&#160;CPU-Anzahl ändern<br />&#160;&#160;&#160;&#160;Arbeitsspeicher ändern<br />&#160;&#160;&#160;&#160;Einstellungen ändern<br />&#160;&#160;&#160;&#160;Platzierung der Auslagerungsdatei ändern<br />&#160;&#160;&#160;&#160;Ressource ändern<br />&#160;&#160;&#160;&#160;Host-USB-Gerät konfigurieren<br />&#160;&#160;&#160;&#160;Unformatiertes Gerät konfigurieren<br />&#160;&#160;&#160;&#160;ManagedBy konfigurieren<br />&#160;&#160;&#160;&#160;Verbindungseinstellungen anzeigen<br />&#160;&#160;&#160;&#160;Virtuellen Datenträger erweitern<br />&#160;&#160;&#160;&#160;Geräteeinstellungen ändern<br />&#160;&#160;&#160;&#160;Kompatibilität der Fehlertoleranz abfragen<br />&#160;&#160;&#160;&#160;Nicht im Besitz befindliche Dateien abfragen<br />&#160;&#160;&#160;&#160;Aus Pfaden neu laden<br />&#160;&#160;&#160;&#160;Datenträger entfernen<br />&#160;&#160;&#160;&#160;Umbenennen<br />&#160;&#160;&#160;&#160;Gastinformationen zurücksetzen<br />&#160;&#160;&#160;&#160;Anmerkung festlegen<br />&#160;&#160;&#160;&#160;Änderungsnachverfolgung für Datenträger ein-/ausschalten<br />&#160;&#160;&#160;&#160;Übergeordneten Fork ein-/ausschalten<br />&#160;&#160;&#160;&#160;VM-Kompatibilität aktualisieren<br />Inventar bearbeiten<br />&#160;&#160;&#160;&#160;Aus vorhandenem Element erstellen<br />&#160;&#160;&#160;&#160;Neues Element erstellen<br />&#160;&#160;&#160;&#160;Verschieben<br />&#160;&#160;&#160;&#160;Registrieren<br />&#160;&#160;&#160;&#160;Entfernen<br />&#160;&#160;&#160;&#160;Registrierung aufheben<br />Gastvorgänge<br />&#160;&#160;&#160;&#160;Gastvorgang Aliasänderung<br />&#160;&#160;&#160;&#160;Gastvorgang Aliasabfrage<br />&#160;&#160;&#160;&#160;Gastvorgang Änderungen<br />&#160;&#160;&#160;&#160;Gastvorgang Programmausführung<br />&#160;&#160;&#160;&#160;Gastvorgang Abfragen<br />Interaktion<br />&#160;&#160;&#160;&#160;Frage beantworten<br />&#160;&#160;&#160;&#160;Sicherungsvorgang für VM<br />&#160;&#160;&#160;&#160;CD-Medien konfigurieren<br />&#160;&#160;&#160;&#160;Disketten konfigurieren<br />&#160;&#160;&#160;&#160;Geräte verbinden<br />&#160;&#160;&#160;&#160;Konsoleninteraktion<br />&#160;&#160;&#160;&#160;Screenshot erstellen<br />&#160;&#160;&#160;&#160;Alle Datenträger defragmentieren<br />&#160;&#160;&#160;&#160;Drag & Drop<br />&#160;&#160;&#160;&#160;Gastbetriebssystemverwaltung durch VIX-API<br />&#160;&#160;&#160;&#160;USB-HID-Scancodes einfügen<br />&#160;&#160;&#160;&#160;VMware-Tools installieren<br />&#160;&#160;&#160;&#160;Anhalten oder Anhalten aufheben<br />&#160;&#160;&#160;&#160;Lösch- oder Verkleinerungsvorgänge ausführen<br />&#160;&#160;&#160;&#160;Ausschalten<br />&#160;&#160;&#160;&#160;Einschalten<br />&#160;&#160;&#160;&#160;Sitzung auf VM aufzeichnen<br />&#160;&#160;&#160;&#160;Sitzung auf VM wiedergeben<br />&#160;&#160;&#160;&#160;Anhalten<br />&#160;&#160;&#160;&#160;Fehlertoleranz anhalten<br />&#160;&#160;&#160;&#160;Failover testen<br />&#160;&#160;&#160;&#160;Neustart der sekundären VM testen<br />&#160;&#160;&#160;&#160;Fehlertoleranz deaktivieren<br />&#160;&#160;&#160;&#160;Fehlertoleranz aktivieren<br />Bereitstellung<br />&#160;&#160;&#160;&#160;Datenträgerzugriff zulassen<br />&#160;&#160;&#160;&#160;Dateizugriff zulassen<br />&#160;&#160;&#160;&#160;Schreibgeschützten Datenträgerzugriff zulassen<br />&#160;&#160;&#160;&#160;VM-Download zulassen<br />&#160;&#160;&#160;&#160;Vorlage klonen<br />&#160;&#160;&#160;&#160;VM klonen<br />&#160;&#160;&#160;&#160;Vorlage aus VM erstellen<br />&#160;&#160;&#160;&#160;Gast anpassen<br />&#160;&#160;&#160;&#160;Vorlage bereitstellen<br />&#160;&#160;&#160;&#160;Als Vorlage markieren<br />&#160;&#160;&#160;&#160;Anpassungsspezifikation ändern<br />&#160;&#160;&#160;&#160;Datenträger höherstufen<br />&#160;&#160;&#160;&#160;Anpassungsspezifikationen lesen<br />Dienstkonfiguration<br />&#160;&#160;&#160;&#160;Benachrichtigungen zulassen<br />&#160;&#160;&#160;&#160;Abrufen globaler Ereignisbenachrichtigungen zulassen<br />&#160;&#160;&#160;&#160;Dienstkonfiguration verwalten<br />&#160;&#160;&#160;&#160;Dienstkonfiguration ändern<br />&#160;&#160;&#160;&#160;Dienstkonfigurationen abfragen<br />&#160;&#160;&#160;&#160;Dienstkonfiguration lesen<br />Momentaufnahmenverwaltung<br />&#160;&#160;&#160;&#160;Momentaufnahme erstellen<br />&#160;&#160;&#160;&#160;Momentaufnahme entfernen<br />&#160;&#160;&#160;&#160;Momentaufnahme umbenennen<br />&#160;&#160;&#160;&#160;Momentaufnahme wiederherstellen<br />vSphere-Replikation<br />&#160;&#160;&#160;&#160;Replikation konfigurieren<br />&#160;&#160;&#160;&#160;Replikation verwalten<br />&#160;&#160;&#160;&#160;Replikation überwachen |
| **vService** | Abhängigkeit erstellen<br />Abhängigkeit löschen<br />Abhängigkeitskonfiguration neu konfigurieren<br />Abhängigkeit aktualisieren |



## <a name="next-steps"></a>Nächste Schritte

Eine ausführliche Erläuterung der einzelnen Berechtigungen finden Sie in der [VMware-Produktdokumentation](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html).

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VMware product documentation]: https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html

