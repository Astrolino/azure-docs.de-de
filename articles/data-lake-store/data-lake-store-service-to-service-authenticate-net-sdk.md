---
title: 'Dienst-zu-Dienst-Authentifizierung: .NET SDK mit Azure Data Lake Storage Gen1 und Azure Active Directory | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie mithilfe von Azure Active Directory und .NET SDK die Dienst-zu-Dienst-Authentifizierung mit Azure Data Lake Storage Gen1 implementieren.
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 61d9606ff14a48e6c224108596f0f01daa4813e1
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/24/2018
ms.locfileid: "49954285"
---
# <a name="service-to-service-authentication-with-azure-data-lake-storage-gen1-using-net-sdk"></a>Dienst-zu-Dienst-Authentifizierung mit Azure Data Lake Storage Gen1 unter Verwendung des .NET SDK
> [!div class="op_single_selector"]
> * [Verwenden von Java](data-lake-store-service-to-service-authenticate-java.md)
> * [Verwenden des .NET SDK](data-lake-store-service-to-service-authenticate-net-sdk.md)
> * [Verwenden von Python](data-lake-store-service-to-service-authenticate-python.md)
> * [Verwenden der REST-API](data-lake-store-service-to-service-authenticate-rest-api.md)
> 
>  

In diesem Artikel erfahren Sie, wie Sie mithilfe des .NET SDK die Dienst-zu-Dienst-Authentifizierung mit Azure Data Lake Storage Gen1 durchführen. Informationen zur Authentifizierung von Endbenutzern mit Data Lake Storage Gen1 unter Verwendung des .NET SDK finden Sie unter [Authentifizierung von Endbenutzern bei Data Lake Storage Gen1 mithilfe des .NET SDK](data-lake-store-end-user-authenticate-net-sdk.md).


## <a name="prerequisites"></a>Voraussetzungen
* **Visual Studio 2013, 2015 oder 2017** In der Anleitung unten wird Visual Studio 2017 verwendet.

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* **Erstellen einer Azure Active Directory-Webanwendung.** Sie müssen die Schritte unter [Dienst-zu-Dienst-Authentifizierung bei Azure Data Lake Storage Gen1 mithilfe von Azure Active Directory](data-lake-store-service-to-service-authenticate-using-active-directory.md) ausgeführt haben.

## <a name="create-a-net-application"></a>Erstellen einer .NET-Anwendung
1. Öffnen Sie Visual Studio, und erstellen Sie eine Konsolenanwendung.
2. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.
3. Unter **Neues Projekt** können Sie die folgenden Werte eingeben bzw. auswählen:

   | Eigenschaft | Wert |
   | --- | --- |
   | Category (Kategorie) |Vorlagen/Visual C#/Windows |
   | Vorlage |Konsolenanwendung |
   | NAME |CreateADLApplication |
4. Klicken Sie auf **OK**, um das Projekt zu erstellen.

5. Fügen Sie Ihrem Projekt die NuGet-Pakete hinzu.

   1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen und dann auf **NuGet-Pakete verwalten**.
   2. Stellen Sie auf der Registerkarte **NuGet-Paket-Manager** sicher, dass **Paketquelle** auf **nuget.org** festgelegt und das Kontrollkästchen **Vorabversion einbeziehen** aktiviert ist.
   3. Suchen und installieren Sie die folgenden NuGet-Pakete:

      * `Microsoft.Azure.Management.DataLake.Store`: In diesem Tutorial wird „v2.1.3-preview“ verwendet.
      * `Microsoft.Rest.ClientRuntime.Azure.Authentication`: In diesem Tutorial wird „v2.2.12“ verwendet.

        ![Hinzufügen einer NuGet-Quelle](./media/data-lake-store-get-started-net-sdk/data-lake-store-install-nuget-package.png "Erstellen eines neuen Azure Data Lake-Kontos")
   4. Schließen Sie den **NuGet-Paket-Manager**.

6. Öffnen Sie **Program.cs**, löschen Sie den vorhandenen Code, und fügen Sie dann die folgenden Anweisungen ein, um Verweise auf Namespaces hinzuzufügen.

        using System;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using System.Collections.Generic;
        using System.Security.Cryptography.X509Certificates; // Required only if you are using an Azure AD application created with certificates
                
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.DataLake.Store;
        using Microsoft.Azure.Management.DataLake.Store.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

## <a name="service-to-service-authentication-with-client-secret"></a>Dienst-zu-Dienst-Authentifizierung mit geheimem Clientschlüssel
Fügen Sie diesen Codeausschnitt der .NET-Clientanwendung hinzu. Ersetzen Sie die Platzhalterwerte mit den aus einer (als Voraussetzung angegebenen) Azure AD-Webanwendung abgerufenen Werten.  Mit diesem Codeausschnitt kann Ihre Anwendung unter Verwendung des Clientgeheimnisses/-schlüssels für die Azure AD-Webanwendung **nicht interaktiv** mit Data Lake Storage Gen1 authentifiziert werden. 

    private static void Main(string[] args)
    {    
        // Service principal / appplication authentication with client secret / key
        // Use the client ID of an existing AAD "Web App" application.
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        string secret_key = "<AAD_WEB_APP_SECRET_KEY>";
        var armCreds = GetCreds_SPI_SecretKey(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, secret_key);
        var adlCreds = GetCreds_SPI_SecretKey(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, secret_key);
    }

Im vorangehenden Ausschnitt wird die Hilfsfunktion `GetCreds_SPI_SecretKey` verwendet. Den Code für diese Hilfsfunktion finden Sie [hier auf GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_secretkey).

## <a name="service-to-service-authentication-with-certificate"></a>Dienst-zu-Dienst-Authentifizierung mit Zertifikat

Fügen Sie diesen Codeausschnitt der .NET-Clientanwendung hinzu. Ersetzen Sie die Platzhalterwerte mit den aus einer (als Voraussetzung angegebenen) Azure AD-Webanwendung abgerufenen Werten. Mit diesem Codeausschnitt kann Ihre Anwendung unter Verwendung des Zertifikats für eine Azure AD-Webanwendung **nicht interaktiv** mit Data Lake Storage Gen1 authentifiziert werden. Anweisungen zum Erstellen einer Azure AD-Anwendung finden Sie unter [Erstellen eines Dienstprinzipals mit selbstsigniertem Zertifikat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).

    
    private static void Main(string[] args)
    {
        // Service principal / application authentication with certificate
        // Use the client ID and certificate of an existing AAD "Web App" application.
        string TENANT = "<AAD-directory-domain>";
        string CLIENTID = "<AAD_WEB_APP_CLIENT_ID>";
        System.Uri ARM_TOKEN_AUDIENCE = new System.Uri(@"https://management.core.windows.net/");
        System.Uri ADL_TOKEN_AUDIENCE = new System.Uri(@"https://datalake.azure.net/");
        var cert = new X509Certificate2(@"d:\cert.pfx", "<certpassword>");
        var armCreds = GetCreds_SPI_Cert(TENANT, ARM_TOKEN_AUDIENCE, CLIENTID, cert);
        var adlCreds = GetCreds_SPI_Cert(TENANT, ADL_TOKEN_AUDIENCE, CLIENTID, cert);
    }

Im vorangehenden Ausschnitt wird die Hilfsfunktion `GetCreds_SPI_Cert` verwendet. Den Code für diese Hilfsfunktion finden Sie [hier auf GitHub](https://github.com/Azure-Samples/data-lake-analytics-dotnet-auth-options#getcreds_spi_cert).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie die Dienst-zu-Dienst-Authentifizierung verwenden, um sich mithilfe des .NET SDK mit Data Lake Storage Gen1 zu authentifizieren. In den folgenden Artikeln erfahren Sie, wie Sie das .NET SDK für die Arbeit mit Data Lake Storage Gen1 verwenden.

* [Kontoverwaltungsvorgänge für Data Lake Storage Gen1 mit dem .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Dateisystemvorgänge in Azure Data Lake Storage Gen1 mit dem .NET SDK](data-lake-store-data-operations-net-sdk.md)


