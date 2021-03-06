---
title: Anmeldeinformationen für die Azure App Service-Bereitstellung | Microsoft Docs
description: Erfahren Sie, wie Sie Anmeldeinformationen für die Azure App Service-Bereitstellung verwenden.
services: app-service
documentationcenter: ''
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/05/2016
ms.author: dariagrigoriu
ms.openlocfilehash: a17260770f0b2e0a73585ce4108bd5625ac22229
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436147"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Konfigurieren von Anmeldeinformationen für die Azure App Service-Bereitstellung
[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) unterstützt zwei Arten von Anmeldeinformationen für [lokale Git-Bereitstellungen](app-service-deploy-local-git.md) und [FTP/S-Bereitstellungen](app-service-deploy-ftp.md). Diese sind nicht identisch mit Ihren Azure Active Directory-Anmeldeinformationen.

* **Anmeldeinformationen auf Benutzerebene**: ein Satz von Anmeldeinformationen für das gesamte Azure-Konto. Hiermit können Sie App Service für alle Apps in allen Abonnements bereitstellen, für die das Azure-Konto über Zugriffsberechtigungen verfügt. Dies ist der Standardsatz von Anmeldeinformationen, den Sie in **App Services** > **&lt;App-Name>** > **Anmeldeinformationen für Bereitstellung** konfigurieren. Dies ist auch der Standardsatz, der auf der Benutzeroberfläche des Portals angezeigt wird (z.B. **Übersicht** und **Eigenschaften** auf der [Ressourcenseite](../azure-resource-manager/resource-group-portal.md#manage-resources) Ihrer App).

    > [!NOTE]
    > Beim Delegieren des Zugriffs auf Azure-Ressourcen über die rollenbasierte Zugriffssteuerung (Role Based Access Control, RBAC) oder Co-Administratorberechtigungen kann jeder Azure-Benutzer, dem Zugriff auf eine App gewährt wird, seine eigenen Anmeldeinformationen auf Benutzerebene nutzen, bis der Zugriff aufgehoben wird. Diese Anmeldeinformationen für die Bereitstellung dürfen nicht mit anderen Azure-Benutzer geteilt werden.
    >
    >

* **Anmeldeinformationen auf App-Ebene**: ein Satz von Anmeldeinformationen für jede App. Er kann nur verwendet werden, um diese App bereitzustellen. Die Anmeldeinformationen für jede App werden automatisch bei der App-Erstellung generiert und befinden sich im Veröffentlichungsprofil der App. Sie können die Anmeldeinformationen nicht manuell konfigurieren, Sie können sie aber jederzeit für eine App zurücksetzen.

    > [!NOTE]
    > Um Benutzern Zugriff auf diese Anmeldeinformationen über die rollenbasierte Zugriffssteuerung zu gewähren, müssen Sie ihnen für die Web-App mindestens die Rolle „Mitwirkender“ zuweisen. Benutzer mit Leseberechtigung dürfen nicht veröffentlichen und haben deshalb keinen Zugriff auf diese Anmeldeinformationen.
    >
    >

## <a name="userscope"></a>Festlegen und Zurücksetzen der Anmeldeinformationen auf Benutzerebene

Sie können die Anmeldeinformationen auf Benutzerebene auf der [Ressourcenseite](../azure-resource-manager/resource-group-portal.md#manage-resources) einer App konfigurieren. Unabhängig von der App, in der Sie diese Anmeldeinformationen konfigurieren, gelten sie für alle Apps und für alle Abonnements in Ihrem Azure-Konto. 

So konfigurieren Sie die Anmeldeinformationen auf Benutzerebene

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf „App Service“ >  **&lt;eine_App>** > **Anmeldeinformationen für Bereitstellung**.

    > [!NOTE]
    > Im Portal muss mindestens eine App vorhanden sein, bevor Sie auf die Seite mit den Anmeldeinformationen für die Bereitstellung zugreifen können. Mit der [Azure-CLI](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set) können Sie die Anmeldeinformationen auf Benutzerebene jedoch ohne eine vorhandene App konfigurieren.

2. Konfigurieren Sie den Benutzernamen und das Kennwort, und klicken Sie dann auf **Speichern**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Wenn Sie Anmeldeinformationen für die Bereitstellung festgelegt haben, finden Sie den *Git*-Benutzernamen für Bereitstellungen in der **Übersicht** Ihrer App.

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

Den *FTP*-Benutzernamen für die Bereitstellung finden Sie in den **Eigenschaften** Ihrer App.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Ihr Kennwort auf Benutzerebene für die Bereitstellung wird von Azure nicht angezeigt. Wenn Sie das Kennwort vergessen haben, können Sie es nicht abrufen. Allerdings können Sie Ihre Anmeldeinformationen mit den Schritten in diesem Abschnitt zurücksetzen.
>
>  

## <a name="appscope"></a>Abrufen und Zurücksetzen der Anmeldeinformationen auf App-Ebene
Für jede App in App Service werden die Anmeldeinformationen auf App-Ebene im XML-Veröffentlichungsprofil gespeichert.

So rufen Sie die Anmeldeinformationen auf App-Ebene ab

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf „App Service“ >  **&lt;eine_App>** > **Übersicht**.

2. Klicken Sie auf **Mehr** > **Veröffentlichungsprofil abrufen**, und der Download einer .PublishSettings-Datei beginnt.

    ![](./media/app-service-deployment-credentials/publish_profile_get.png)

3. Öffnen Sie die .PublishSettings-Datei, und suchen Sie das Tag `<publishProfile>` mit dem Attribut `publishMethod="FTP"`. Rufen Sie dann die Attribute `userName` und `password` ab.
Dies sind die Anmeldeinformationen auf App-Ebene.

    ![](./media/app-service-deployment-credentials/publish_profile_editor.png)

    Ähnlich wie die Anmeldeinformationen auf Benutzerebene hat der FTP-Benutzername für die Bereitstellung das Format `<app_name>\<username>`, und der Git-Benutzername für die Bereitstellung ist einfach `<username>`, ohne den vorangestellten `<app_name>\`.

So setzen Sie die Anmeldeinformationen auf App-Ebene zurück

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf „App Service“ >  **&lt;eine_App>** > **Übersicht**.

2. Klicken Sie auf **Mehr** > **Veröffentlichungsprofil zurücksetzen**. Klicken Sie auf **Ja**, um das Zurücksetzen zu bestätigen.

    Durch das Zurücksetzens werden alle zuvor heruntergeladenen .PublishSettings-Dateien ungültig.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie diese Anmeldeinformationen zum Bereitstellen Ihrer App über [lokales Git](app-service-deploy-local-git.md) oder [FTP/S](app-service-deploy-ftp.md) verwenden.
