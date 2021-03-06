---
title: 'Tutorial: Integration von Azure Active Directory in LinkedIn Sales Navigator | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LinkedInSalesNavigator konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: f0e34a614251cf11c9547d749fef58dfa8ca623a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39425196"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Tutorial: Integration von Azure Active Directory in LinkedIn Sales Navigator

In diesem Tutorial erfahren Sie, wie Sie LinkedIn Sales Navigator in Azure Active Directory (Azure AD) integrieren.

Die Integration von LinkedIn Sales Navigator in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LinkedIn Sales Navigator hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LinkedIn Sales Navigator anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Integration von Azure AD in LinkedIn Sales Navigator konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LinkedIn Sales Navigator-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von LinkedIn Sales Navigator aus dem Katalog
1. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>Hinzufügen von LinkedIn Sales Navigator aus dem Katalog
Zum Konfigurieren der Integration von LinkedIn Sales Navigator in Azure AD müssen Sie LinkedIn Sales Navigator aus dem Katalog zu Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Um LinkedIn Sales Navigator aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

1. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![ANWENDUNGEN][2]
    
1. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![ANWENDUNGEN][3]

1. Geben Sie in das Suchfeld **LinkedIn Sales Navigator** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

1. Wählen Sie im Ergebnisbereich die Option **LinkedIn Sales Navigator** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei LinkedIn Sales Navigator mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LinkedIn Sales Navigator als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LinkedIn Sales Navigator muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in LinkedIn Sales Navigator zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei LinkedIn Sales Navigator über Azure AD müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
1. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
1. **[Erstellen eines LinkedIn Sales Navigator-Testbenutzers](#creating-a-linkedin-sales-navigator-test-user)**, um eine Entsprechung von Britta Simon in LinkedIn Sales Navigator zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
1. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
1. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden über Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer LinkedIn Sales Navigator-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei LinkedIn Sales Navigator über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LinkedIn Sales Navigator** auf **Einmaliges Anmelden**.

    ![Configure single sign-on][4]

1. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

1. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrer **LinkedIn Sales Navigator**-Website als Administrator an.

1. Klicken Sie im **Kontocenter** unter **Einstellungen** auf **Globale Einstellungen**. Wählen Sie außerdem **Sales Navigator** aus der Dropdownliste aus.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. Klicken Sie auf **ODER klicken Sie hier, um einzelne Felder aus dem Formular zu laden und zu kopieren**, und kopieren Sie die **Entitäts-ID** und **Assertion Consumer Access(ACS)-URL**.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

1. Führen Sie im Azure-Portal unter **Domäne und URLs für LinkedIn Sales Navigator** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** die **Entitäts-ID** ein, die Sie aus dem LinkedIn-Portal kopiert haben. 

    b. Geben Sie im Textfeld **Antwort-URL** die **Assertion Consumer Access(ACS)-URL** ein, die Sie aus dem LinkedIn-Portal kopiert haben.

1. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`.

1. Die **LinkedIn Sales Navigator**-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Im folgenden Screenshot wird ein Beispiel gezeigt. Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, LinkedIn Sales Navigator erwartet jedoch, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste oder den entsprechenden Attributwert gemäß Ihrer Organisationskonfiguration verwenden. 

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/updateusermail.png)
    
1. Klicken Sie im Abschnitt **Benutzerattribute** auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und legen Sie die Attribute fest. Der Benutzer muss vier Ansprüche mit den Namen **email**, **department**, **firstname** und **lastname** hinzufügen, und der Wert muss mit **user.mail**, **user.department**, **user.givenname** bzw. **user.surname** zugeordnet werden.

    | Attributname | Attributwert |
    | --- | --- |    
    | E-Mail| user.mail |
    | department| user.department |
    | firstname| user.givenname |
    | lastname| user.surname |
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld zum Attribut zu öffnen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

1. Führen Sie für das Attribut **Name** die folgenden Schritte aus:

    a. Klicken Sie auf das Attribut, um das Fenster **Attribut bearbeiten** zu öffnen.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/url_update.png)

    b. Löschen Sie den URL-Wert aus dem **Namespace**.
    
    c. Klicken Sie auf **OK**, um die Einstellung zu speichern.

1. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

1. Klicken Sie auf die Schaltfläche **Save** .

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_general_400.png)

1. Wechseln Sie zum Abschnitt **LinkedIn-Administratoreinstellungen**. Klicken Sie auf **XML-Datei hochladen**, um die Metadaten-XML-Datei hochzuladen, die Sie vom Azure-Portal heruntergeladen haben.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. Klicken Sie auf **Ein**, um das einmalige Anmelden zu aktivieren. Der Status des einmaligen Anmeldens wechselt von **Nicht verbunden** zu **Verbunden**.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das **Azure Active Directory**-Symbol.

    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

1. Klicken Sie auf **Benutzer und Gruppen** und **Alle Benutzer**.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

1. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

1. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Erstellen eines LinkedIn Sales Navigator-Testbenutzers

Die LinkedIn Sales Navigator-Anwendung unterstützt Just-in-Time(JIT)-Benutzerbereitstellungen. Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. Aktivieren Sie **Lizenzen automatisch zuweisen**, um dem Benutzer eine Lizenz zuzuweisen.
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf LinkedIn Sales Navigator gewähren.

![Benutzer zuweisen][200] 

**Um LinkedIn Sales Navigator Britta Simon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **LinkedIn Sales Navigator** aus.

    ![Configure single sign-on](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

1. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

1. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

1. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie auf die Kachel „LinkedIn Sales Navigator“ im Zugriffsbereich klicken, sollten Sie zur Seite der Organisation umgeleitet werden, auf der Sie Details Ihres persönlichen LinkedIn-Kontos angeben müssen. Hierdurch wird Ihr persönliches Konto mit Ihrem LinkedIn-Geschäftskonto verknüpft. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_203.png

