---
title: 'Tutorial: Azure Active Directory-Integration mit OpenAthens | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und OpenAthens konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jeedes
ms.openlocfilehash: 269b216a94b1233c5f9f9a634fda3c05e46cac90
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435905"
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Tutorial: Azure Active Directory-Integration mit OpenAthens

In diesem Tutorial erfahren Sie, wie Sie OpenAthens in Azure Active Directory (Azure AD) integrieren.

Die Integration von OpenAthens in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf OpenAthens hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei OpenAthens anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit OpenAthens konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges OpenAthens-Abonnement

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von OpenAthens aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-openathens-from-the-gallery"></a>Hinzufügen von OpenAthens aus dem Katalog
Zum Konfigurieren der Integration von OpenAthens in Azure AD müssen Sie OpenAthens aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**So fügen Sie OpenAthens über den Katalog hinzu**

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

1. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Bereich „Unternehmensanwendungen“][2]
    
1. Klicken Sie im oberen Bereich des Dialogfelds auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

1. Geben Sie im Suchfeld die Zeichenfolge **OpenAthens** ein, wählen Sie im Ergebnisbereich die Option **OpenAthens** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**.

    ![OpenAthens in der Ergebnisliste](./media/openathens-tutorial/tutorial_openathens_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei OpenAthens mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in OpenAthens als Gegenstück des Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in OpenAthens muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in OpenAthens den Wert für **Benutzername** in Azure AD als Wert für **Username** (Benutzername) zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei OpenAthens müssen Sie die folgenden Schritte ausführen:

1. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on), um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
1. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user), um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
1. [Erstellen eines OpenAthens-Testbenutzers](#create-a-openathens-test-user), um in OpenAthens eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
1. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. [Testen des einmaligen Anmeldens](#test-single-sign-on), um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer OpenAthens-Anwendung.

**So konfigurieren Sie das einmalige Anmelden von Azure AD mit OpenAthens**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **OpenAthens** auf **Einmaliges Anmelden**.

    ![Link zum Konfigurieren des einmaligen Anmeldens][4]

1. Wählen Sie zum Aktivieren des einmaligen Anmeldens im Dialogfeld **Einmaliges Anmelden** für **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/openathens-tutorial/tutorial_openathens_samlbase.png)

1. Geben Sie im Abschnitt **Domäne und URLs für OpenAthens** den Wert `https://login.openathens.net/saml/2/metadata-sp` in das Textfeld **Bezeichner** ein.

    ![SSO-Informationen zur Domäne und zu den URLs für OpenAthens](./media/openathens-tutorial/tutorial_openathens_url.png)

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das SAML-Signaturzertifikat](./media/openathens-tutorial/tutorial_openathens_certificate.png) 

1. Wählen Sie die Schaltfläche **Speichern** aus.

    ![Schaltfläche „Speichern“ für einmaliges Anmelden](./media/openathens-tutorial/tutorial_general_400.png)

1. Melden Sie sich in einem anderen Webbrowserfenster bei der OpenAthens-Unternehmenswebsite als Administrator an.

1. Wählen Sie in der Liste auf der Registerkarte **Management** (Verwaltung) die Option **Connections** (Verbindungen) aus. 

    ![Einmaliges Anmelden konfigurieren](./media/openathens-tutorial/tutorial_openathens_application1.png)

1. Wählen Sie **SAML 1.1/2.0** aus, und klicken Sie anschließend auf die Schaltfläche **Configure** (Konfigurieren).

    ![Einmaliges Anmelden konfigurieren](./media/openathens-tutorial/tutorial_openathens_application2.png)
    
1. Klicken Sie zum Hinzufügen der Konfiguration auf die Schaltfläche **Browse** (Durchsuchen), um die Metadaten-XML-Datei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben, und klicken Sie anschließend auf **Add** (Hinzufügen).

    ![Einmaliges Anmelden konfigurieren](./media/openathens-tutorial/tutorial_openathens_application3.png)

1. Führen Sie auf der Registerkarte **Details** die folgenden Schritte aus.

    ![Einmaliges Anmelden konfigurieren](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. Wählen Sie für **Display name mapping** (Zuordnung des Anzeigenamens) die Option **Use Attribute** (Attribut verwenden) aus.

    b. Geben Sie im Textfeld **Display name attribute** (Anzeigenamenattribut) den Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.
    
    c. Wählen Sie für **Unique user mapping** (Eindeutige Benutzerzuordnung) die Option **Use Attribute** (Attribut verwenden) aus.

    d. Geben Sie im Textfeld **Unique user attribute** (Eindeutiges Benutzerattribut) den Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    e. Aktivieren Sie unter **Status** alle drei Kontrollkästchen.

    f. Wählen Sie in **Create local accounts** (Lokale Konten erstellen) die Option **automatically** (automatisch) aus.

    g. Klicken Sie auf **Save changes** (Änderungen speichern).

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, navigieren Sie zur Registerkarte **Einmaliges Anmelden**, und rufen Sie am unteren Rand im Abschnitt **Konfiguration** die eingebettete Dokumentation auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt wird über das Azure-Portal ein Testbenutzer namens Britta Simon erstellt.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**So erstellen Sie einen Testbenutzer in Azure AD**

1. Klicken Sie im linken Bereich des Azure-Portals auf **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/openathens-tutorial/create_aaduser_01.png)

1. Navigieren Sie zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/openathens-tutorial/create_aaduser_02.png)

1. Klicken Sie im oberen Bereich des Dialogfelds **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/openathens-tutorial/create_aaduser_03.png)

1. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/openathens-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse für Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Textfeld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-an-openathens-test-user"></a>Erstellen eines OpenAthens-Testbenutzers

OpenAthens unterstützt die Just-In-Time-Bereitstellung, und Benutzer werden nach erfolgreicher Authentifizierung automatisch erstellt. Sie müssen in diesem Abschnitt keine Aktion ausführen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf OpenAthens gewähren.

![Zuweisen der Benutzerrolle][200] 

**So weisen Sie Britta Simon OpenAthens zu**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Navigieren Sie zur Verzeichnisansicht, navigieren Sie zu **Unternehmensanwendungen**, und klicken Sie anschließend auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Liste **Anwendungen** die Option **OpenAthens** aus.

    ![OpenAthens-Link in der Anwendungsliste](./media/openathens-tutorial/tutorial_openathens_app.png)  

1. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

1. Wählen Sie die Schaltfläche **Hinzufügen** aus. Klicken Sie im Bereich **Zuweisung hinzufügen** auf **Benutzer und Gruppen**.

    ![Bereich „Zuweisung hinzufügen“][203]

1. Wählen Sie in der Liste **Benutzer und Gruppen** die Option **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie auf im Bereich **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel **OpenAthens** klicken, sollten Sie automatisch bei Ihrer OpenAthens-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* Eine Liste mit Tutorials für die Integration von SaaS-Apps in Azure Active Directory finden Sie unter [SaaS-Anwendungsintegration mit Azure Active Directory](tutorial-list.md).
* Weitere Informationen zu Anwendungszugriff und einmaligem Anmelden mit Azure Active Directory finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

<!--Image references-->

[1]: ./media/openathens-tutorial/tutorial_general_01.png
[2]: ./media/openathens-tutorial/tutorial_general_02.png
[3]: ./media/openathens-tutorial/tutorial_general_03.png
[4]: ./media/openathens-tutorial/tutorial_general_04.png

[100]: ./media/openathens-tutorial/tutorial_general_100.png

[200]: ./media/openathens-tutorial/tutorial_general_200.png
[201]: ./media/openathens-tutorial/tutorial_general_201.png
[202]: ./media/openathens-tutorial/tutorial_general_202.png
[203]: ./media/openathens-tutorial/tutorial_general_203.png

