---
title: 'Azure-VPN Gateway: Konfigurieren von Paketerfassungen'
description: Hier erhalten Sie Informationen über Paketerfassungsfunktionen, die für VPN-Gateways verwendet werden können.
services: vpn-gateway
author: radwiv
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/15/2019
ms.author: radwiv
ms.openlocfilehash: 3ba3046367ceece6bf0ddf157451025c79977324
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077206"
---
# <a name="configure-packet-captures-for-vpn-gateways"></a>Konfigurieren der Paketerfassung für VPN-Gateways

Probleme Im Zusammenhang mit Konnektivität und Leistung sind häufig komplex, und das Eingrenzen der Ursache des Problems erfordert viel Zeit. Die Paketerfassung trägt wesentlich dazu bei, die für die Ursachensuche benötigte Zeit zu verkürzen, indem das Problem auf bestimmte Teile des Netzwerks eingeschränkt wird. So wird z. B. ermittelt, ob das Problem auf der Kundenseite des Netzwerks, auf der Azure-Seite des Netzwerks oder irgendwo dazwischen liegt. Sobald das Problem eingegrenzt wurde, ist es einfacher, ein effizientes Debugging und Maßnahmen zur Problembeseitigung durchzuführen.

Für die Paketerfassung stehen verschiedene gängige Tools zur Verfügung. Eine relevante Paketerfassung kann mit diesen Tools umständlich sein, insbesondere bei der Arbeit in Szenarien mit hohem Datenaufkommen. Die von einer Paketerfassung für VPN-Gateways bereitgestellten Filterfunktionen werden zu einem wichtigen Unterscheidungsmerkmal. Sie können zusätzlich zu den gängigen Tools für die Paketerfassung eine VPN-Gateway-Paketerfassung nutzen.

## <a name="vpn-gateway-packet-capture-filtering-capabilities"></a>Filterfunktionen für die VPN-Gateway-Paketerfassung

VPN-Gateway-Paketerfassungen können je nach Kundenanforderungen für das Gateway oder eine bestimmte Verbindung ausgeführt werden. Es ist außerdem möglich, Paketerfassungen für mehrere Tunnel gleichzeitig auszuführen. Sie können uni- oder bidirektionalen Datenverkehr, IKE- und ESP-Datenverkehr und interne Pakete gemeinsam mit einer Filterung für ein VPN-Gateway erfassen.

Die Verwendung von 5-Tupel-Filtern (Quellsubnetz, Zielsubnetz, Quellport, Zielport, Protokoll) und TCP-Flags (SYN, ACK, FIN, URG, PSH, RST) ist nützlich, um eine Problemisolierung bei einem hohen Datenverkehrsaufkommen durchzuführen.

Im Folgenden finden Sie ein Beispiel für JSON-Code und ein JSON-Schema mit Erläuterungen zu den einzelnen Eigenschaften. Beachten Sie auch einige Einschränkungen beim Ausführen der Paketerfassungen:
- Im Schema wird der Filter als ein Array angezeigt, aber derzeit kann jeweils nur ein Filter verwendet werden.
- Mehrere gatewayweite Paketerfassungen zur gleichen Zeit sind nicht zulässig.
- Mehrere Paketerfassungen über dieselbe Verbindung zur gleichen Zeit sind nicht zulässig. Sie können Paketerfassungen für verschiedene Verbindungen gleichzeitig ausführen.
- Pro Gateway können maximal fünf Paketerfassungen parallel ausgeführt werden. Diese Paketerfassungen können eine Kombination aus der gatewayweiten Paketerfassung und der Paketerfassung pro Verbindung sein.

### <a name="example-json"></a>JSON-Beispiel
```JSON-interactive
{
  "TracingFlags": 11,
  "MaxPacketBufferSize": 120,
  "MaxFileSize": 200,
  "Filters": [
    {
      "SourceSubnets": [
        "20.1.1.0/24"
      ],
      "DestinationSubnets": [
        "10.1.1.0/24"
      ],
      "SourcePort": [
        500
      ],
      "DestinationPort": [
        4500
      ],
      "Protocol": [
        6
      ],
      "TcpFlags": 16,
      "CaptureSingleDirectionTrafficOnly": true
    }
  ]
}
```
### <a name="json-schema"></a>JSON-Schema
```JSON-interactive
{
    "type": "object",
    "title": "The Root Schema",
    "description": "The root schema input JSON filter for packet capture",
    "default": {},
    "additionalProperties": true,
    "required": [
        "TracingFlags",
        "MaxPacketBufferSize",
        "MaxFileSize",
        "Filters"
    ],
    "properties": {
        "TracingFlags": {
            "$id": "#/properties/TracingFlags",
            "type": "integer",
            "title": "The Tracingflags Schema",
            "description": "Tracing flags that customer can pass to define which packets are to be captured. Supported values are CaptureESP = 1, CaptureIKE = 2, CaptureOVPN = 8. The final value is OR of the bits.",
            "default": 11,
            "examples": [
                11
            ]
        },
        "MaxPacketBufferSize": {
            "$id": "#/properties/MaxPacketBufferSize",
            "type": "integer",
            "title": "The Maxpacketbuffersize Schema",
            "description": "Maximum buffer size of each packet. The capture will only contain contents of each packet truncated to this size.",
            "default": 120,
            "examples": [
                120
            ]
        },
        "MaxFileSize": {
            "$id": "#/properties/MaxFileSize",
            "type": "integer",
            "title": "The Maxfilesize Schema",
            "description": "Maximum file size of the packet capture file. It is a circular buffer.",
            "default": 100,
            "examples": [
                100
            ]
        },
        "Filters": {
            "$id": "#/properties/Filters",
            "type": "array",
            "title": "The Filters Schema",
            "description": "An array of filters that can be passed to filter inner ESP traffic.",
            "default": [],
            "examples": [
                [
                    {
                        "Protocol": [
                            6
                        ],
                        "CaptureSingleDirectionTrafficOnly": true,
                        "SourcePort": [
                            500
                        ],
                        "DestinationPort": [
                            4500
                        ],
                        "TcpFlags": 16,
                        "SourceSubnets": [
                            "20.1.1.0/24"
                        ],
                        "DestinationSubnets": [
                            "10.1.1.0/24"
                        ]
                    }
                ]
            ],
            "additionalItems": true,
            "items": {
                "$id": "#/properties/Filters/items",
                "type": "object",
                "title": "The Items Schema",
                "description": "An explanation about the purpose of this instance.",
                "default": {},
                "examples": [
                    {
                        "SourcePort": [
                            500
                        ],
                        "DestinationPort": [
                            4500
                        ],
                        "TcpFlags": 16,
                        "SourceSubnets": [
                            "20.1.1.0/24"
                        ],
                        "DestinationSubnets": [
                            "10.1.1.0/24"
                        ],
                        "Protocol": [
                            6
                        ],
                        "CaptureSingleDirectionTrafficOnly": true
                    }
                ],
                "additionalProperties": true,
                "required": [
                    "SourceSubnets",
                    "DestinationSubnets",
                    "SourcePort",
                    "DestinationPort",
                    "Protocol",
                    "TcpFlags",
                    "CaptureSingleDirectionTrafficOnly"
                ],
                "properties": {
                    "SourceSubnets": {
                        "$id": "#/properties/Filters/items/properties/SourceSubnets",
                        "type": "array",
                        "title": "The Sourcesubnets Schema",
                        "description": "An array of source subnets that need to match the Source IP address of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                "20.1.1.0/24"
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/SourceSubnets/items",
                            "type": "string",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": "",
                            "examples": [
                                "20.1.1.0/24"
                            ]
                        }
                    },
                    "DestinationSubnets": {
                        "$id": "#/properties/Filters/items/properties/DestinationSubnets",
                        "type": "array",
                        "title": "The Destinationsubnets Schema",
                        "description": "An array of destination subnets that need to match the Destination IP address of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                "10.1.1.0/24"
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/DestinationSubnets/items",
                            "type": "string",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": "",
                            "examples": [
                                "10.1.1.0/24"
                            ]
                        }
                    },
                    "SourcePort": {
                        "$id": "#/properties/Filters/items/properties/SourcePort",
                        "type": "array",
                        "title": "The Sourceport Schema",
                        "description": "An array of source ports that need to match the Source port of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                500
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/SourcePort/items",
                            "type": "integer",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": 0,
                            "examples": [
                                500
                            ]
                        }
                    },
                    "DestinationPort": {
                        "$id": "#/properties/Filters/items/properties/DestinationPort",
                        "type": "array",
                        "title": "The Destinationport Schema",
                        "description": "An array of destination ports that need to match the Destination port of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                4500
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/DestinationPort/items",
                            "type": "integer",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": 0,
                            "examples": [
                                4500
                            ]
                        }
                    },
                    "Protocol": {
                        "$id": "#/properties/Filters/items/properties/Protocol",
                        "type": "array",
                        "title": "The Protocol Schema",
                        "description": "An array of protocols that need to match the Protocol of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                6
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/Protocol/items",
                            "type": "integer",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": 0,
                            "examples": [
                                6
                            ]
                        }
                    },
                    "TcpFlags": {
                        "$id": "#/properties/Filters/items/properties/TcpFlags",
                        "type": "integer",
                        "title": "The Tcpflags Schema",
                        "description": "A list of TCP flags. The TCP flags set on the packet must match any flag in the list of flags provided. FIN = 0x01,SYN = 0x02,RST = 0x04,PSH = 0x08,ACK = 0x10,URG = 0x20,ECE = 0x40,CWR = 0x80. An OR of flags can be provided.",
                        "default": 0,
                        "examples": [
                            16
                        ]
                    },
                    "CaptureSingleDirectionTrafficOnly": {
                        "$id": "#/properties/Filters/items/properties/CaptureSingleDirectionTrafficOnly",
                        "type": "boolean",
                        "title": "The Capturesingledirectiontrafficonly Schema",
                        "description": "A flags which when set captures reverse traffic also.",
                        "default": false,
                        "examples": [
                            true
                        ]
                    }
                }
            }
        }
    }
}
```

## <a name="setup-packet-capture-using-powershell"></a>Einrichten der Paketerfassung mithilfe von PowerShell

Nachfolgend finden Sie Beispiele für PowerShell-Befehle zum Starten und Beenden von Paketerfassungen. Weitere Informationen zu den Parameteroptionen finden Sie in diesem PowerShell-[Dokument](https://docs.microsoft.com/powershell/module/az.network/start-azvirtualnetworkgatewaypacketcapture).

### <a name="start-packet-capture-for-a-vpn-gateway"></a>Starten der Paketerfassung für ein VPN-Gateway

```azurepowershell-interactive
Start-AzVirtualnetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName"
```

Der optionale Parameter **-FilterData** kann verwendet werden, um einen Filter anzuwenden.

### <a name="stop-packet-capture-for-a-vpn-gateway"></a>Beenden der Paketerfassung für ein VPN-Gateway

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName" -SasUrl "YourSASURL"
```

### <a name="start-packet-capture-for-a-vpn-gateway-connection"></a>Starten der Paketerfassung für eine VPN-Gatewayverbindung

```azurepowershell-interactive
Start-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName"
```

Der optionale Parameter **-FilterData** kann verwendet werden, um einen Filter anzuwenden.

### <a name="stop-packet-capture-on-a-vpn-gateway-connection"></a>Beenden der Paketerfassung für eine VPN-Gatewayverbindung

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName" -SasUrl "YourSASURL"
```

## <a name="key-considerations"></a>Wichtige Aspekte

- Das Ausführen von Paketerfassungen kann sich auf die Leistung auswirken. Denken Sie daran, die Paketerfassung zu beenden, wenn sie nicht benötigt wird.
- Die empfohlene Mindestdauer für die Paketerfassung beträgt 600 Sekunden. Eine kürzere Paketerfassung führt möglicherweise zu unvollständigen Daten aufgrund von Synchronisierungsproblemen zwischen verschiedenen Komponenten im Pfad.
- Bei der Paketerfassung werden Datendateien im PCAP-Format generiert. Verwenden Sie Wireshark oder andere allgemein verfügbare Anwendungen, um PCAP-Dateien zu öffnen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu VPN-Gateways finden Sie unter [Was ist VPN Gateway?](vpn-gateway-about-vpngateways.md).
