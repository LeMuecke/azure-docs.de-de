---
title: 'Azure-VPN Gateway: Informationen zum P2S-Routing'
description: Anhand dieses Artikels können Sie das Verhalten von Point-to-Site-VPN-Routing nachvollziehen.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: kumud
ms.openlocfilehash: 536e1e57af16a87fba85323e9448bfb904d56384
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87027868"
---
# <a name="about-point-to-site-vpn-routing"></a>Informationen zu Point-to-Site-VPN-Routing

Anhand dieses Artikels können Sie das Verhalten von Azure Point-to-Site-VPN-Routing nachvollziehen. Das P2S VPN-Routingverhalten ist vom Clientbetriebssystem, dem für die VPN-Verbindung verwendeten Protokoll und der Art der Verbindung der virtuellen Netzwerke (VNets) untereinander abhängig.

Azure unterstützt zurzeit zwei Protokolle für den Remotezugriff: IKEv2 und SSTP. IKEv2 wird unter zahlreichen Betriebssystemen (einschließlich Windows, Linux, MacOS, Android und iOS) unterstützt. SSTP wird nur unter Windows unterstützt. Wenn Sie eine Änderung an der Topologie Ihres Netzwerks vornehmen und Windows-VPN-Clients verwenden, muss das VPN-Clientpaket für Windows-Clients heruntergeladen und erneut installiert werden, damit die Änderungen auf den Client angewendet werden.

> [!NOTE]
> Dieser Artikel gilt nur für IKEv2.
>

## <a name="about-the-diagrams"></a><a name="diagrams"></a>Informationen zu den Abbildungen

Es gibt eine Anzahl verschiedener Abbildungen in diesem Artikel. Jeder Abschnitt zeigt eine andere Topologie oder Konfiguration. Im Rahmen dieses Artikels funktionieren Site-to-Site- (S2S) und VNet-to-VNet-Verbindungen auf die gleiche Weise, weil beide IPSec-Tunnel darstellen. Alle VPN-Gateways in diesem Artikel sind routenbasiert.

## <a name="one-isolated-vnet"></a><a name="isolatedvnet"></a>Ein isoliertes VNet

Die Point-to-Site-VPN-Gatewayverbindung in diesem Beispiel gilt für ein VNet, das nicht mit einem anderen virtuellen Netzwerk (VNet1) verbunden ist und auch kein Peering aufweist. In diesem Beispiel können alle Clients nur auf VNet1 zugreifen.

![Weiterleitung in einem isolierten VNET](./media/vpn-gateway-about-point-to-site-routing/1.jpg "Weiterleitung in einem isolierten VNET")

### <a name="address-space"></a>Adressraum

* VNet1: 10.1.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

* Zu Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Windows-Clients können auf VNet1 zugreifen.

* Nicht-Windows-Clients können auf VNet1 zugreifen.

## <a name="multiple-peered-vnets"></a><a name="multipeered"></a>Mehrere VNets mit Peering

In diesem Beispiel gilt die Point-to-Site-VPN-Gatewayverbindung für VNet1. Für VNet1 erfolgt Peering mit VNet2. Für VNet2 erfolgt Peering mit VNet3. Für VNet1 erfolgt Peering mit VNet4. Es gibt kein direktes Peering zwischen VNet1 und VNet3. Für VNet1 ist „Gatewaytransit zulassen“ aktiviert. Für VNet2 und VNet4 ist „Remotegateways verwenden“ aktiviert.

Clients, die Windows verwenden, können auf VNets mit direktem Peering zugreifen, aber der VPN-Client muss erneut heruntergeladen werden, wenn Änderungen am VNet-Peering oder an der Netzwerktopologie vorgenommen werden. Nicht-Windows-Clients können auf VNets mit direktem Peering zugreifen. Der Zugriff ist nicht transitiv und nur auf VNets mit direktem Peering beschränkt.

![Mehrere VNETs mit Peering](./media/vpn-gateway-about-point-to-site-routing/2.jpg "Mehrere VNETs mit Peering")

### <a name="address-space"></a>Adressraum:

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* VNet4: 10.4.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

* Zu Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Windows-Clients können auf VNet1, VNet2 und VNet4 zugreifen, aber der VPN-Client muss erneut heruntergeladen werden, damit alle Topologieänderungen wirksam werden.

* Nicht-Windows-Clients können auf VNet1, VNet2 und VNet4 zugreifen.

## <a name="multiple-vnets-connected-using-an-s2s-vpn"></a><a name="multis2s"></a>Mehrere VNets, die über ein S2S-VPN verbunden sind

In diesem Beispiel gilt die Point-to-Site-VPN-Gatewayverbindung für VNet1. VNet1 ist mit VNet2 über eine Site-to-Site-VPN-Verbindung verbunden. VNet2 ist mit VNet3 über eine Site-to-Site-VPN-Verbindung verbunden. Direktes Peering oder eine Site-to-Site-VPN-Verbindung zwischen VNet1 und VNet3 ist nicht vorhanden. Alle Site-to-Site-Verbindungen führen nicht BGP für das Routing aus.

Clients, die Windows oder ein anderes unterstütztes Betriebssystem verwenden, können nur auf VNet1 zugreifen. Um auf zusätzliche VNets zugreifen zu können, muss BGP verwendet werden.

![Mehrere VNETs und S2S](./media/vpn-gateway-about-point-to-site-routing/3.jpg "Mehrere VNETs und S2S")

### <a name="address-space"></a>Adressraum

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

* Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 10.2.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Windows-Clients können nur auf VNet1 zugreifen.

* Nicht-Windows-Clients können nur auf VNet1 zugreifen.

## <a name="multiple-vnets-connected-using-an-s2s-vpn-bgp"></a><a name="multis2sbgp"></a>Mehrere VNets, die über ein S2S-VPN verbunden sind (BGP)

In diesem Beispiel gilt die Point-to-Site-VPN-Gatewayverbindung für VNet1. VNet1 ist mit VNet2 über eine Site-to-Site-VPN-Verbindung verbunden. VNet2 ist mit VNet3 über eine Site-to-Site-VPN-Verbindung verbunden. Direktes Peering oder eine Site-to-Site-VPN-Verbindung zwischen VNet1 und VNet3 ist nicht vorhanden. Alle Site-to-Site-Verbindungen führen BGP für das Routing aus.

Clients, die Windows oder ein anderes unterstütztes Betriebssystem verwenden, können auf alle VNets zugreifen, die über eine Site-to-Site-VPN-Verbindung verbunden sind, aber Routen zu verbundenen VNets müssen den Windows-Clients manuell hinzugefügt werden.

![Mehrere VNETs und S2S (BGP)](./media/vpn-gateway-about-point-to-site-routing/4.jpg "Mehrere VNETs und S2S (BGP)")

### <a name="address-space"></a>Adressraum

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16

* Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Windows-Clients können auf VNet1, VNet2 und VNet3 zugreifen, Routen zu VNet2 und VNet3 müssen allerdings manuell hinzugefügt werden.

* Nicht-Windows-Clients können auf VNet1, VNet2 und VNet3 zugreifen.

## <a name="one-vnet-and-a-branch-office"></a><a name="vnetbranch"></a>Ein VNet und eine Filiale

In diesem Beispiel gilt die Point-to-Site-VPN-Gatewayverbindung für VNet1. Für VNet1 ist keine Verbindung bzw. kein Peering mit einem beliebigen anderen virtuellen Netzwerk vorhanden, es ist jedoch mit einem lokalen Standort über eine Site-to-Site-VPN-Verbindung verbunden, die nicht BGP ausführt.

Windows-Clients und Nicht-Windows-Clients können nur auf VNet1 zugreifen.

![Weiterleitung mit einem VNET und einer Filiale](./media/vpn-gateway-about-point-to-site-routing/5.jpg "Weiterleitung mit einem VNET und einer Filiale")

### <a name="address-space"></a>Adressraum

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

* Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Windows-Clients können nur auf VNet1 zugreifen.

* Nicht-Windows-Clients können nur auf VNet1 zugreifen.

## <a name="one-vnet-and-a-branch-office-bgp"></a><a name="vnetbranchbgp"></a>Ein VNet und eine Filiale (BGP)

In diesem Beispiel gilt die Point-to-Site-VPN-Gatewayverbindung für VNet1. Für VNet1 ist keine Verbindung bzw. kein Peering mit einem beliebigen anderen virtuellen Netzwerk vorhanden, es ist jedoch mit einem lokalen Standort (Site1) über eine Site-to-Site-VPN-Verbindung verbunden, die BGP ausführt.

Windows-Clients können auf das VNet und die Filiale (Site1) zugreifen, die Routen zu Site1 müssen dem Client aber manuell hinzugefügt werden. Nicht-Windows-Clients können ebenfalls auf das VNet sowie die lokale Filiale zugreifen.

![Ein VNET und eine Filiale (BGP)](./media/vpn-gateway-about-point-to-site-routing/6.jpg "Ein VNET und eine Filiale")

### <a name="address-space"></a>Adressraum

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

* Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Windows-Clients können auf VNet1 und Site1 zugreifen, Routen zu Site1 müssen allerdings manuell hinzugefügt werden.

* Nicht-Windows-Clients können auf VNet1 und Site1 zugreifen.


## <a name="multiple-vnets-connected-using-s2s-and-a-branch-office"></a><a name="multivnets2sbranch"></a>Mehrere mithilfe von S2S verbundene VNets und eine Filiale

In diesem Beispiel gilt die Point-to-Site-VPN-Gatewayverbindung für VNet1. VNet1 ist mit VNet2 über eine Site-to-Site-VPN-Verbindung verbunden. VNet2 ist mit VNet3 über eine Site-to-Site-VPN-Verbindung verbunden. Direktes Peering oder ein Site-to-Site-VPN-Tunnel zwischen den VNet1- und VNet3-Netzwerken ist nicht vorhanden. VNet3 ist mit einer Filiale (Site1) über eine Site-to-Site-VPN-Verbindung verbunden. Alle VPN-Verbindungen führen BGP nicht aus.

Alle Clients können nur auf VNet1 zugreifen.

![S2S mit mehreren VNETs und einer Filiale](./media/vpn-gateway-about-point-to-site-routing/7.jpg "S2S mit mehreren VNETs und einer Filiale")

### <a name="address-space"></a>Adressraum

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

* Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Die Windows-Clients können nur auf VNet1 zugreifen.

* Nicht-Windows-Clients können nur auf VNet1 zugreifen.

## <a name="multiple-vnets-connected-using-s2s-and-a-branch-office-bgp"></a><a name="multivnets2sbranchbgp"></a>Mehrere mit S2S verbundene VNets und eine Filiale (BGP)

In diesem Beispiel gilt die Point-to-Site-VPN-Gatewayverbindung für VNet1. VNet1 ist mit VNet2 über eine Site-to-Site-VPN-Verbindung verbunden. VNet2 ist mit VNet3 über eine Site-to-Site-VPN-Verbindung verbunden. Direktes Peering oder ein Site-to-Site-VPN-Tunnel zwischen den VNet1- und VNet3-Netzwerken ist nicht vorhanden. VNet3 ist mit einer Filiale (Site1) über eine Site-to-Site-VPN-Verbindung verbunden. Alle VPN-Verbindungen führen BGP aus.

Clients mit Windows können auf VNets und Standorte zugreifen, die über eine Site-to-Site-VPN-Verbindung verbunden sind, die Routen zu VNet2, VNet3 und Site1 müssen dem Client aber manuell hinzugefügt werden. Nicht-Windows-Clients können ohne manuelle Eingriffe auf VNets und Standorte zugreifen, die über eine Site-to-Site-VPN-Verbindung verbunden sind. Der Zugriff ist transitiv, und Clients besitzen Zugriff auf Ressourcen in allen verbundenen VNets und Standorten (lokal).

![S2S mit mehreren VNETs und einer Filiale](./media/vpn-gateway-about-point-to-site-routing/8.jpg "S2S mit mehreren VNETs und einer Filiale")

### <a name="address-space"></a>Adressraum

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Hinzugefügte Routen

* Zu Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 192.168.0.0/24

* Nicht-Windows-Clients hinzugefügte Routen: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Zugriff

* Windows-Clients können auf VNet1, VNet2, VNet3 und Site1 zugreifen, die Routen zu VNet2, VNet3 und Site1 müssen dem Client aber manuell hinzugefügt werden.

* Nicht-Windows-Clients können auf VNet1, VNet2, VNet3 und Site1 zugreifen.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Erstellen eines P2S-VPNs mithilfe des Azure-Portals](vpn-gateway-howto-point-to-site-resource-manager-portal.md), um mit dem Erstellen Ihres P2S-VPNs zu beginnen.
