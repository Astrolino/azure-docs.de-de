---
title: 'Schnellstart: Cassandra-API mit .NET – Azure Cosmos DB'
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe der Cassandra-API von Azure Cosmos DB eine Profilanwendung mit dem Azure-Portal und .NET erstellen.
services: cosmos-db
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
author: SnehaGunda
ms.author: sngun
ms.custom: quick start connect, mvc
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: 3708027b766ff9070831dd3677e821565e0dd4fa
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994954"
---
# <a name="quickstart-build-a-cassandra-app-with-net-and-azure-cosmos-db"></a>Schnellstartanleitung: Erstellen einer Cassandra-App mit .NET und Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](create-cassandra-dotnet.md)
> * [Java](create-cassandra-java.md)
> * [Node.js](create-cassandra-nodejs.md)
> * [Python](create-cassandra-python.md)
>  

In dieser Schnellstartanleitung erfahren Sie, wie Sie mithilfe von .NET und der [Cassandra-API](cassandra-introduction.md) von Azure Cosmos DB eine Profil-App erstellen, indem Sie ein Beispiel von GitHub klonen. Außerdem wird in dieser Schnellstartanleitung gezeigt, wie Sie das webbasierte Azure-Portal verwenden, um ein Azure Cosmos DB-Konto zu erstellen.

Azure Cosmos DB ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Tabellen-, Schlüssel-Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets die Vorteile der globalen Verteilung und der horizontalen Skalierung nutzen, die Azure Cosmos DB bietet. 

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] Alternativ können Sie [Azure Cosmos DB ohne Azure-Abonnement testen](https://azure.microsoft.com/try/cosmosdb/) – kostenlos und ohne Verpflichtung.

Zudem benötigen Sie: 
* Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.
* Installieren Sie [Git](https://www.git-scm.com/), um das Beispiel zu klonen.

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]


## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Beginnen wir nun mit der Verwendung von Code. Wir klonen eine Cassandra-API-App aus GitHub, legen die Verbindungszeichenfolge fest und führen die App aus. Sie werden feststellen, wie einfach Sie programmgesteuert mit Daten arbeiten können. 

1. Öffnen Sie eine Eingabeaufforderung. Erstellen Sie einen neuen Ordner namens `git-samples`. Schließen Sie dann die Eingabeaufforderung.

    ```bash
    md "C:\git-samples"
    ```

2. Öffnen Sie ein Git-Terminalfenster (z.B. git bash), und verwenden Sie den Befehl `cd`, um in den neuen Ordner zu gelangen und dort die Beispiel-App zu installieren.

    ```bash
    cd "C:\git-samples"
    ```

3. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. Dieser Befehl erstellt eine Kopie der Beispiel-App auf Ihrem Computer.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-dotnet-getting-started.git
    ```

4. Öffnen Sie anschließend die Projektmappendatei „CassandraQuickStartSample“ in Visual Studio. 

## <a name="review-the-code"></a>Überprüfen des Codes

Dieser Schritt ist optional. Wenn Sie erfahren möchten, wie der Code die Datenbankressourcen erstellt, können Sie sich die folgenden Codeausschnitte ansehen. Die Codeausschnitte stammen alle aus der Datei `Program.cs`, die im Ordner `C:\git-samples\azure-cosmos-db-cassandra-dotnet-getting-started\CassandraQuickStartSample` installiert wurde. Andernfalls können Sie mit [Aktualisieren der Verbindungszeichenfolge](#update-your-connection-string) fortfahren.

* Initialisieren Sie die Sitzung, indem Sie eine Verbindung mit einem Cassandra-Clusterendpunkt herstellen. Die Cassandra-API von Azure Cosmos DB unterstützt nur TLSv1.2. 

  ```csharp
   var options = new Cassandra.SSLOptions(SslProtocols.Tls12, true, ValidateServerCertificate);
   options.SetHostNameResolver((ipAddress) => CassandraContactPoint);
   Cluster cluster = Cluster.Builder().WithCredentials(UserName, Password).WithPort(CassandraPort).AddContactPoint(CassandraContactPoint).WithSSL(options).Build();
   ISession session = cluster.Connect();
   ```

* Erstellen Sie einen neuen Keyspace.

    ```csharp
    session.Execute("CREATE KEYSPACE uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 };"); 
    ```

* Erstellen Sie eine neue Tabelle.

   ```csharp
  session.Execute("CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)");
   ```

* Fügen Sie Benutzerentitäten ein. Verwenden Sie hierzu das IMapper-Objekt mit einer neuen Sitzung, die eine Verbindung mit dem Keyspace „uprofile“ herstellt.

    ```csharp
    mapper.Insert<User>(new User(1, "LyubovK", "Dubai"));
    ```
    
* Fragen Sie alle Benutzerinformationen ab.

    ```csharp
   foreach (User user in mapper.Fetch<User>("Select * from user"))
   {
      Console.WriteLine(user);
   }
    ```
    
* Führen Sie eine Abfrage zum Abrufen der Informationen eines einzelnen Benutzers aus.

    ```csharp
    mapper.FirstOrDefault<User>("Select * from user where user_id = ?", 3);
    ```

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren. Die Angabe der Verbindungszeichenfolgeninformationen ermöglicht Ihrer App die Kommunikation mit Ihrer gehosteten Datenbank.

1. Wählen Sie im [Azure-Portal](http://portal.azure.com/) die Option **Verbindungszeichenfolge** aus.

    Verwenden Sie die Schaltfläche ![Schaltfläche „Kopieren“](./media/create-cassandra-dotnet/copy.png) auf der rechten Seite des Bildschirms, um den Benutzernamen zu kopieren.

    ![Anzeigen und Kopieren eines Zugriffsschlüssels im Azure-Portal auf der Seite „Verbindungszeichenfolge“](./media/create-cassandra-dotnet/keys.png)

2. Öffnen Sie in Visual Studio 2017 die Datei „Program.cs“. 

3. Ersetzen Sie `<FILLME>` in Zeile 13 durch den Benutzernamen aus dem Portal.

    Zeile 13 von „Program.cs“ sollte nun in etwa wie folgt aussehen: 

    `private const string UserName = "cosmos-db-quickstart";`

3. Kehren Sie zum Portal zurück, und kopieren Sie das Kennwort. Ersetzen Sie `<FILLME>` in Zeile 14 durch das Kennwort aus dem Portal.

    Zeile 14 von „Program.cs“ sollte nun in etwa wie folgt aussehen: 

    `private const string Password = "2Ggkr662ifxz2Mg...==";`

4. Kehren Sie zum Portal zurück, und kopieren Sie den Kontaktpunkt. Ersetzen Sie `<FILLME>` in Zeile 15 durch den Kontaktpunkt aus dem Portal.

    Zeile 15 von „Program.cs“ sollte nun in etwa wie folgt aussehen: 

    `private const string CassandraContactPoint = "cosmos-db-quickstarts.cassandra.cosmosdb.azure.com"; //  DnsName`

5. Speichern Sie die Datei "Program.cs".
    
## <a name="run-the-net-app"></a>Ausführen der .NET-App

1. Klicken Sie in Visual Studio auf **Tools** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.

2. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um das NuGet-Paket des .NET-Treibers zu installieren: 

    ```cmd
    Install-Package CassandraCSharpDriver
    ```
3. Drücken Sie STRG+F5, um die Anwendung auszuführen. Ihre App wird im Konsolenfenster angezeigt. 

    ![Anzeigen und Überprüfen der Ausgabe](./media/create-cassandra-dotnet/output.png)

    Drücken Sie STRG+C, um die Programmausführung zu beenden und das Konsolenfenster zu schließen. 
    
4. Öffnen Sie im Azure-Portal den **Daten-Explorer**, um diese neuen Daten abzufragen, zu ändern und zu verwenden.

    ![Anzeigen der Daten im Daten-Explorer](./media/create-cassandra-dotnet/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Überprüfen von SLAs im Azure-Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie gelernt, wie Sie ein Azure Cosmos DB-Konto erstellen, einen Container mit dem Daten-Explorer erstellen und eine Web-App ausführen. Jetzt können Sie zusätzliche Daten in Ihr Cosmos DB-Konto importieren. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB: Import Cassandra data](cassandra-import-data.md) (Azure Cosmos DB: Importieren von Cassandra-Daten)
