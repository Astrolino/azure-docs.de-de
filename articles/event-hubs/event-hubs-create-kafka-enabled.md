---
title: Erstellen von Apache Kafka-fähigen Azure Event Hubs | Microsoft-Dokumentation
description: Erstellen eines Kafka-fähigen Azure Event Hubs-Namespace mithilfe des Azure-Portals
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2018
ms.author: bahariri
ms.openlocfilehash: 7ce12f9dcaa15ade95274419f99c13d5915dbaaa
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42143642"
---
# <a name="create-apache-kafka-enabled-event-hubs"></a>Erstellen von Apache Kafka-fähigen Event Hubs

Azure Event Hubs ist ein Big Data-PaaS-Streamingdienst (Platform as a Service), der sekündlich Millionen von Ereignisse erfasst und Echtzeitanalysen sowie -visualisierung mit niedrigen Latenzen und einem hohen Durchsatz bietet.

Von Azure Event Hubs wird ein Kafka-Endpunkt bereitgestellt. Dieser Endpunkt ermöglicht Ihrem Event Hubs-Namespace die native Erkennung des [Apache Kafka](https://kafka.apache.org/intro)-Nachrichtenprotokolls und der APIs. Mit dieser Funktion können Sie wie bei Kafka-Themen mit Ihrem Event Hubs kommunizieren, ohne Ihre Protokollclients ändern oder eigene Cluster ausführen zu müssen. Event Hubs unterstützt [Apache Kafka Version 1.0.](https://kafka.apache.org/10/documentation.html) und höher.

In diesem Artikel wird beschrieben, wie ein Event Hubs-Namespace erstellt und die Verbindungszeichenfolge abgerufen werden kann, die erforderlich ist, um eine Verbindung zwischen Kafka-Anwendungen und Kafka-fähigen Event Hubs herzustellen.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen.

## <a name="create-a-kafka-enabled-event-hubs-namespace"></a>Erstellen eines Kafka-fähigen Event Hubs-Namespace

1. Melden Sie sich im [Azure-Portal][Azure portal] an, und klicken Sie oben links auf **Ressource erstellen**.

2. Suchen Sie nach Event Hubs, und wählen Sie die hier gezeigten Optionen aus:
    
    ![Suchen nach Event Hubs im Portal](./media/event-hubs-create-kafka-enabled/event-hubs-create-event-hubs.png)
 
3. Stellen Sie einen eindeutigen Namen bereit, und aktivieren Sie Kafka für den Namespace. Klicken Sie auf **Create**.
    
    ![Erstellen eines Namespace](./media/event-hubs-create-kafka-enabled/create-kafka-namespace.png)
 
4. Nachdem der Namespace erstellt wurde, klicken Sie auf der Registerkarte **Einstellungen** auf **SAS-Richtlinien**, um die Verbindungszeichenfolge abzurufen.

    ![Klicken auf „SAS-Richtlinien“](./media/event-hubs-create/create-event-hub7.png)

5. Sie können die Standardrichtlinie **RootManageSharedAccessKey** auswählen oder eine neue Richtlinie hinzufügen. Klicken Sie auf den Richtliniennamen, und kopieren Sie die Verbindungszeichenfolge. 
    
    ![Auswählen einer Richtlinie](./media/event-hubs-create/create-event-hub8.png)
 
6. Fügen Sie diese Verbindungszeichenfolge zur Konfiguration Ihrer Kafka-Anwendung hinzu.

Nun können Sie Ereignisse von Ihren Anwendungen streamen, die das Kafka-Protokoll in Event Hubs verwenden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Event Hubs finden Sie unter diesen Links:

* [Streamen von Ereignissen in Event Hubs aus Kafka-Anwendungen](event-hubs-quickstart-kafka-enabled-event-hubs.md)
* [Informationen zu Event Hubs für Kafka](event-hubs-for-kafka-ecosystem-overview.md)
* [Informationen zu Event Hubs](event-hubs-what-is-event-hubs.md)


[Azure portal]: https://portal.azure.com/
