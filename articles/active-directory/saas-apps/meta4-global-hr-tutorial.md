---
title: 'Självstudie: Azure Active Directory integrering med META4 Global HR | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och META4 global personal.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/09/2019
ms.author: jeedes
ms.openlocfilehash: c2875afe597bd111f091ee1e4a53265154ca66b3
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/24/2020
ms.locfileid: "92516837"
---
# <a name="tutorial-azure-active-directory-integration-with-meta4-global-hr"></a>Självstudie: Azure Active Directory integrering med META4 Global HR

I den här självstudien får du lära dig att integrera META4 Global HR med Azure Active Directory (Azure AD).
Att integrera META4 Global HR med Azure AD ger följande fördelar:

* Du kan styra Azure AD som har åtkomst till META4 global personal.
* Du kan göra det möjligt för användarna att vara automatiskt inloggade att META4 globalt HR (enkel inloggning) med sina Azure AD-konton.
* Du kan hantera dina konton på en central plats – Azure-portalen.

Om du vill ha mer information om SaaS-appintegrering med Azure AD läser du avsnittet om [programåtkomst och enkel inloggning med Azure Active Directory](../manage-apps/what-is-single-sign-on.md).
Om du inte har en Azure-prenumeration kan du [skapa ett kostnadsfritt konto ](https://azure.microsoft.com/free/) innan du börjar.

## <a name="prerequisites"></a>Förutsättningar

Om du vill konfigurera Azure AD-integrering med META4 Global HR behöver du följande objekt:

* En Azure AD-prenumeration. Om du inte har en Azure AD-miljö kan du få ett [kostnads fritt konto](https://azure.microsoft.com/free/)
* META4 Global HR-aktiverad prenumeration för enkel inloggning

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien konfigurerar och testar du enkel inloggning med Azure AD i en testmiljö.



* META4 Global HR stöder **SP-och IDP** -INITIERAd SSO



## <a name="adding-meta4-global-hr-from-the-gallery"></a>Lägga till META4 Global HR från galleriet

Om du vill konfigurera integreringen av META4 global personal i Azure AD måste du lägga till META4 Global HR från galleriet till din lista över hanterade SaaS-appar.

**Utför följande steg för att lägga till META4 Global HR från galleriet:**

1. I **[Azure-portalen](https://portal.azure.com)** går du till den vänstra navigeringspanelen och klickar på **Azure Active Directory**-ikonen.

    ![Azure Active Directory-knappen](common/select-azuread.png)

2. Gå till **Företagsprogram** och välj alternativet **Alla program**.

    ![Bladet Företagsprogram](common/enterprise-applications.png)

3. Lägg till ett nytt program genom att klicka på knappen **Nytt program** längst upp i dialogrutan.

    ![Knappen Nytt program](common/add-new-app.png)

4. I rutan Sök skriver du **META4 Global HR**, väljer **META4 Global HR** från resultat panelen och klickar sedan på knappen **Lägg** till för att lägga till programmet.

    ![META4 Global HR i resultat listan](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurera och testa enkel inloggning med Azure AD

I det här avsnittet konfigurerar och testar du enkel inloggning med Azure AD med META4 Global HR baserat på en test användare som kallas **Britta Simon**.
För att enkel inloggning ska fungera måste en länk relation mellan en Azure AD-användare och den relaterade användaren i META4 Global HR upprättas.

Om du vill konfigurera och testa enkel inloggning med Azure AD med META4 Global HR måste du slutföra följande Bygg stenar:

1. **[Konfigurera enkel inloggning med Azure AD](#configure-azure-ad-single-sign-on)** – så att användarna kan använda den här funktionen.
2. **[Konfigurera META4 Global HR Single Sign-on](#configure-meta4-global-hr-single-sign-on)** -för att konfigurera de enskilda Sign-On inställningarna på program sidan.
3. **[Skapa en Azure AD-testanvändare](#create-an-azure-ad-test-user)** – för att testa enkel inloggning med Azure AD med Britta Simon.
4. **[Tilldela Azure AD-testanvändaren](#assign-the-azure-ad-test-user)** – så att Britta Simon kan använda enkel inloggning med Azure AD.
5. **[Skapa META4 Global HR test User](#create-meta4-global-hr-test-user)** – för att få en motsvarighet till Britta Simon i META4 Global HR som är länkad till Azure AD-representation av användare.
6. **[Testa enkel inloggning](#test-single-sign-on)** – för att verifiera om konfigurationen fungerar.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurera enkel inloggning med Azure AD

I det här avsnittet aktiverar du enkel inloggning med Azure AD i Azure-portalen.

Utför följande steg för att konfigurera enkel inloggning med Azure AD med META4 Global HR:

1. I [Azure Portal](https://portal.azure.com/)på sidan **META4 Global HR** Application Integration väljer du **enkel inloggning**.

    ![Konfigurera länk för enkel inloggning](common/select-sso.png)

2. I dialogrutan **Välj en metod för enkel inloggning** väljer du läget **SAML/WS-Fed** för att aktivera enkel inloggning.

    ![Välja läge för enkel inloggning](common/select-saml-option.png)

3. På sidan **Konfigurera enkel inloggning med SAML** klickar du på ikonen **Redigeringa** för att öppna dialogrutan **Grundläggande SAML-konfiguration**.

    ![Redigera grundläggande SAML-konfiguration](common/edit-urls.png)

4. I avsnittet **grundläggande SAML-konfiguration** , om du vill konfigurera programmet i **IDP** initierat läge, utför följande steg:

    ![Skärm bild som visar den grundläggande SAML-konfigurationen, där du kan ange Reply U R L och välja Spara.](common/both-replyurl.png)

    Skriv en URL i text rutan **svars-URL** med följande mönster:  `https://<SUBDOMAIN>.meta4globalhr.com/saml.sso/SAML2/POST`

5. Klicka på **Ange ytterligare URL:er** och gör följande om du vill konfigurera appen i **SP**-initierat läge:

    ![Skärm bild som visar ytterligare U R LS där du kan ange ett tecken på U R L.](common/both-signonurl.png)

    I text rutan **inloggnings-URL** skriver du en URL med följande mönster:  `https://<SUBDOMAIN>.meta4globalhr.com`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera värdena med faktisk svars-URL och inloggnings-URL. Kontakta [META4 global personal support team](mailto:victors@meta4.com) för att hämta dessa värden. Du kan även se mönstren som visas i avsnittet **Grundläggande SAML-konfiguration** i Azure-portalen.

6. På sidan **Set up Single Sign-On with SAML** (Konfigurera enkel inloggning med SAML) går du till avsnittet **SAML Signing Certificate** (SAML-signeringscertifikat), klickar på **Ladda ned** för att ladda ned **Federation Metadata-XML** från de angivna alternativen enligt dina behov och spara den på datorn.

    ![Länk för nedladdning av certifikatet](common/metadataxml.png)

7. I avsnittet **Konfigurera META4 global personal** kopierar du lämpliga URL: er enligt ditt krav.

    ![Kopiera konfigurations-URL:er](common/copy-configuration-urls.png)

    a. Inloggnings-URL

    b. Azure AD-identifierare

    c. Utloggnings-URL

### <a name="configure-meta4-global-hr-single-sign-on"></a>Konfigurera META4 Global HR Single Sign-On

Om du vill konfigurera enkel inloggning på **META4 Global HR** -sidan måste du skicka den hämtade **XML-metadata för federationsmetadata** och lämpliga kopierade url: er från Azure Portal till [META4 global personal support team](mailto:victors@meta4.com). De anger inställningen så att SAML SSO-anslutningen ställs in korrekt på båda sidorna.

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare 

Målet med det här avsnittet är att skapa en testanvändare i Azure-portalen med namnet Britta Simon.

1. Gå till den vänstra rutan i Azure-portalen och välj **Azure Active Directory**, välj **Users** och sedan **Alla användare**.

    ![Länkarna ”Användare och grupper” och ”Alla grupper”](common/users.png)

2. Välj **ny användare** överst på skärmen.

    ![Knappen Ny användare](common/new-user.png)

3. Genomför följande steg i Användaregenskaper.

    ![Dialogrutan Användare](common/user-properties.png)

    a. I fältet **Namn** anger du **BrittaSimon**.
  
    b. I fältet **användar namn** `brittasimon@yourcompanydomain.extension` . Till exempel BrittaSimon@contoso.com

    c. Markera kryssrutan **Visa lösenord** och skriv sedan ned det värde som visas i rutan Lösenord.

    d. Klicka på **Skapa**.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändaren

I det här avsnittet aktiverar du Britta Simon för att använda enkel inloggning med Azure genom att bevilja åtkomst till META4 global personal.

1. I Azure Portal väljer du **företags program**, väljer **alla program**och väljer sedan **META4 global personal**.

    ![Bladet Företagsprogram](common/enterprise-applications.png)

2. I listan program väljer du **META4 global personal**.

    ![META4 Global HR-länk i listan program](common/all-applications.png)

3. På menyn till vänster väljer du **Användare och grupper**.

    ![Länken ”Användare och grupper”](common/users-groups-blade.png)

4. Klicka på knappen **Lägg till användare** och välj sedan **Användare och grupper** i dialogrutan **Lägg till tilldelning**.

    ![Fönstret Lägg till tilldelning](common/add-assign-user.png)

5. I dialogrutan **Användare och grupper** väljer du **Britta Simon** i listan med användare och klickar på knappen **Välj** längst ned på skärmen.

6. Om du förväntar dig ett roll värde i SAML-kontrollen väljer du lämplig roll för användaren i listan i dialog rutan **Välj roll** och klickar sedan på knappen **Välj** längst ned på skärmen.

7. I dialogrutan **Lägg till tilldelning** klickar du på knappen **Tilldela**.

### <a name="create-meta4-global-hr-test-user"></a>Skapa META4 Global HR test User

I det här avsnittet skapar du en användare som heter Britta Simon i META4 Global HR. Arbeta med [META4 global personal support team](mailto:victors@meta4.com) för att lägga till användare i META4 Global HR-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

### <a name="test-single-sign-on"></a>Testa enkel inloggning 

I det här avsnittet testar du konfigurationen för enkel inloggning Azure AD med hjälp av åtkomstpanelen.

När du klickar på panelen META4 global personal på åtkomst panelen, bör du loggas in automatiskt på den globala HR-META4 som du ställer in SSO för. Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>Ytterligare resurser

- [Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory](./tutorial-list.md)

- [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Vad är villkorlig åtkomst i Azure Active Directory?](../conditional-access/overview.md)