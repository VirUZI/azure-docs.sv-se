---
title: Azures säkerhetsbaslinje för Cost Management
description: Säkerhetsbaslinjen för Cost Management erbjuder procedurvägledning och resurser för implementering av de säkerhetsrekommendationer som anges i Benchmark för Azure-säkerhet.
author: msmbaldwin
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 11/16/2020
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 3270dae1fd37913ba51ca1da63bbb44d715e7d31
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/21/2020
ms.locfileid: "95021787"
---
# <a name="azure-security-baseline-for-cost-management"></a>Azures säkerhetsbaslinje för Cost Management

Denna säkerhetsbaslinje gäller vägledning från [Benchmark för Azure-säkerhet version 2.0](../security/benchmarks/overview.md) till Azure Cost Management. Benchmark för Azure-säkerhet innehåller rekommendationer för hur du kan skydda dina molnlösningar på Azure. Innehållet grupperas efter de **säkerhetskontroller** som definieras av Benchmark för Azure-säkerhet och relaterad vägledning som avser Cost Management. **Kontroller** som inte gäller för Cost Management har uteslutits.

Mer information om hur Cost Management mappar fullständigt till Benchmark för Azure-säkerhet finns i [den fullständiga mappningsfilen för säkerhetsbaslinjen för Cost Management](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="identity-management"></a>Identitetshantering

*Mer information finns i [Benchmark för Azure-säkerhet: Identitetshantering](/azure/security/benchmarks/security-controls-v2-identity-management).*

### <a name="im-1-standardize-azure-active-directory-as-the-central-identity-and-authentication-system"></a>IM-1: Standardisera Azure Active Directory som centralt identitets- och autentiseringssystem

**Riktlinjer**: Azure Cost Management är integrerat med Azure Active Directory (Azure AD) som är Azures standardtjänst för identitets- och åtkomst hantering. Du bör standardisera Azure AD för att styra organisationens identitets- och åtkomsthantering i:

- Microsoft Cloud-resurser, till exempel Azure Portal, Azure Storage, Azure Virtual Machines (Linux och Windows), Azure Key Vault, PaaS och SaaS-program.

- Organisationens resurser, till exempel program i Azure eller företagets nätverksresurser.

Att skydda Azure AD bör vara en hög prioritet i din organisations rutiner för molnsäkerhet. Azure AD tillhandahåller en identitetssäkerhetspoäng för att hjälpa dig att utvärdera din status i fråga om identitetssäkerhet i relation till Microsofts rekommendationer för bästa praxis. Använd poängen för att mäta hur nära konfigurationen matchar rekommendationerna för bästa praxis och för att göra förbättringar i din säkerhetsstatus.

Obs! Azure AD har stöd för externa identitetsleverantörer, vilket gör det möjligt för användare utan Microsoft-konto att logga in på sina program och resurser med sin externa identitet.

- [Innehav i Azure AD](../active-directory/develop/single-and-multi-tenant-apps.md)

- [Så skapar och konfigurerar du en Azure AD-instans](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Definiera Azure AD-klientorganisationer](https://azure.microsoft.com/resources/securing-azure-environments-with-azure-active-directory/)  

- [Använda externa identitetsprovidrar för ett program](/azure/active-directory/b2b/identity-providers)

- [Vad är identitetssäkerhetspoäng i Azure AD](../active-directory/fundamentals/identity-secure-score.md)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="im-3-use-azure-ad-single-sign-on-sso-for-application-access"></a>IM-3: Använda enkel inloggning för Azure AD (SSO) för programåtkomst

**Riktlinjer**: Azure Cost Management använder Azure Active Directory för att tillhandahålla identitets- och åtkomsthantering för Azure-resurser, molnprogram och lokala program. Här ingår företagsidentiteter som anställda, samt externa identiteter som partner, säljare och leverantörer. Det här möjliggör enkel inloggning (SSO) för att hantera och skydda åtkomsten till din organisations data och resurser lokalt och i molnet. Anslut alla användare, program och enheter till Azure AD för sömlös, säker åtkomst och bättre synlighet och kontroll.

- [Förstå enkel programinloggning (Application SSO) med Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="im-4-use-strong-authentication-controls-for-all-azure-active-directory-based-access"></a>IM-4: Använda starka autentiseringskontroller för all Azure Active Directory-baserad åtkomst

**Riktlinjer**: Azure Cost Management är integrerat med Azure AD som stöder starka verifieringskontroller genom multifaktorautentisering (Multi-Factor Authentication, MFA) och starka metoder för lösenordskryptering.

- Multifaktorautentisering (MFA) – aktivera Azure AD MFA och följ Azure Security Centers rekommendationer för identitets- och åtkomsthantering för vissa rekommenderade metoder i din MFA-installation. MFA kan tillämpas på alla, på utvalda användare eller på per användare-nivå baserat på inloggningsvillkor och riskfaktorer.

- Lösenordsfri autentisering – det finns tre alternativ för lösenordsfri autentisering: Windows Hello for Business, Microsoft Authenticator-appen och lokala autentiseringsmetoder som smartkort.

För administratörer och privilegierade användare bör du se till att den högsta nivån för metoden stark autentisering används, och introducera därefter lämplig princip för stark autentisering för andra användare.

- [Så här aktiverar du MFA i Azure](../active-directory/authentication/howto-mfa-getstarted.md) 

- [Introduktion till alternativ för lösenordsfri autentisering för Azure Active Directory](../active-directory/authentication/concept-authentication-passwordless.md) 

- [Standardprincip för lösenord för Azure AD](../active-directory/authentication/concept-sspr-policy.md#password-policies-that-only-apply-to-cloud-user-accounts)

- [Eliminera felaktiga lösen ord med Azure AD-lösenordsskydd](../active-directory/authentication/concept-password-ban-bad.md)

**Azure Security Center-övervakning**: Ja

**Ansvar**: Kund

### <a name="im-5-monitor-and-alert-on-account-anomalies"></a>IM-5: Övervaka och varna vid kontoavvikelser

**Riktlinjer**: Azure Cost Management är integrerat med Azure Active Directory, där följande datakällor tillhandahålls:

- Inloggningsaktiviteter – Rapporten för inloggningsaktiviteter ger information om användningen av hanterade program och användares inloggningsaktiviteter.

- Granskningsloggar – Ger spårbarhet via loggar för alla ändringar som gjorts via olika funktioner i Azure AD. Exempel på granskningsloggar är de resursändringar som görs i Azure AD, som att lägga till eller ta bort användare, appar, grupper, roller och principer.

- Riskfyllda inloggningar – En riskfylld inloggning indikerar ett potentiellt inloggningsförsök av någon annan än användarkontots ägare.

- Användare som har flaggats för risk – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.

Dessa datakällor kan integreras med Azure Monitor, Azure Sentinel eller SIEM-system från tredje part.

Azure Security Center kan också avisera om vissa misstänkta aktiviteter, till exempel ett högt antal misslyckade autentiseringsförsök eller föråldrade konton i prenumerationen.

Azure Advanced Threat Protection (ATP) är en säkerhetslösning som kan använda Active Directory-signaler för att identifiera, upptäcka och undersöka avancerade hot, komprometterade identiteter och skadliga Insider-åtgärder.

- [Granska aktivitetsrapporter i Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Så visar du riskfyllda inloggningar för Azure AD](/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

- [Så här identifierar du Azure AD-användare som har flaggats för riskfylld aktivitet](/azure/active-directory/reports-monitoring/concept-user-at-risk) 

- [Så här övervakar du användarnas identitets- och åtkomstrelaterade aktiviteter i Azure Security Center](../security-center/security-center-identity-access.md) 

- [Aviseringar i skyddsmodulen för hotinformation i Azure Security Center](../security-center/alerts-reference.md) 

- [Så här integrerar du Azures aktivitetsloggar i Azure Monitor](../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

**Azure Security Center-övervakning**: Ja

**Ansvar**: Kund

## <a name="privileged-access"></a>Privilegierad åtkomst

*Mer information finns i [Benchmark för Azure-säkerhet: Privilegierad åtkomst](/azure/security/benchmarks/security-controls-v2-privileged-access).*

### <a name="pa-1-protect-and-limit-highly-privileged-users"></a>PA-1: Skydda och begränsa användare med höga privilegier

**Riktlinjer**: Cost Management för Enterprise-avtal (EA) har följande konto med höga privilegier.

- Företagsadministratör

Vi rekommenderar att du regelbundet granskar användare som tilldelats roller i ditt Enterprise-avtal (EA).

Vi rekommenderar också att du använder en regel för att begränsa antalet konton eller roller med höga privilegier och skyddar dessa konton på en utökad nivå, eftersom användare med den här behörigheten direkt eller indirekt kan läsa och ändra alla resurser i din Azure-miljö.

Du kan aktivera just-in-time (JIT)-privilegierad åtkomst till Azure-resurser och Azure AD med hjälp av Azure AD Privileged Identity Management (PIM). JIT beviljar temporära behörigheter för att utföra privilegierade uppgifter endast när användarna behöver det. PIM kan också generera säkerhetsaviseringar när det finns misstänkt eller osäker aktivitet i din Azure AD-organisation.

- [Hantera Azure Enterprise-roller](manage/understand-ea-roles.md) 

- [Behörigheter för administratörsrollen i Azure AD](/azure/active-directory/users-groups-roles/directory-assign-admin-roles) 

- [Använda säkerhetsaviseringar från Privileged Identity Management i Azure](../active-directory/privileged-identity-management/pim-how-to-configure-security-alerts.md) 

- [Skydda privilegierad åtkomst för hybrid- och molndistributioner i Azure AD](/azure/active-directory/users-groups-roles/directory-admin-roles-secure)

**Azure Security Center-övervakning**: Ja

**Ansvar**: Kund

### <a name="pa-3-review-and-reconcile-user-access-regularly"></a>PA-3: Granska och stäm av användaråtkomsten regelbundet

**Riktlinjer**: Azure Cost Management är beroende av rätt åtkomst för att kunna visa och hantera organisationers kostnadsdata. Åtkomsten kontrolleras med rollbaserad åtkomstkontroll i Azure (Azure RBAC) på prenumerationsnivå och genom administrativa roller med Enterprise-avtal (EA) eller Microsoft-kundavtal (Microsoft Customer Agreement, MCA) för faktureringsomfattningar. Granska den beviljade åtkomsten regelbundet för att se till att användarna eller grupperna har nödvändig åtkomst.

- [Hantera åtkomst till faktureringsinformation för Azure](manage/manage-billing-access.md)

- [Tilldela åtkomst till Cost Management-data](costs/assign-access-acm-data.md)

- [Hantera Azure Enterprise-roller](manage/understand-ea-roles.md)

- [Förstå administrativa roller för Microsoft-kundavtal i Azure](manage/understand-mca-roles.md)

**Azure Security Center-övervakning**: Ja

**Ansvar**: Kund

### <a name="pa-7-follow-just-enough-administration-least-privilege-principle"></a>PA-7: Följ JEA (Just Enough Administration, precis tillräcklig administration) (principen om minsta behörighet) 

**Riktlinjer**: Azure Cost Management integreras med rollbaserad åtkomstkontroll (RBAC) i Azure för att hantera resurser (t.ex. budgetar, sparade rapporter osv.). Med Azure RBAC kan du hantera åtkomst till Azure-resurser via rolltilldelningar. Du kan tilldela dessa roller till användare, grupper och tjänstens huvudnamn. Det finns fördefinierade inbyggda roller för vissa resurser och dessa roller kan inventeras eller frågas via verktyg som Azure CLI, Azure PowerShell eller Azure-portalen. De behörigheter som du tilldelar resurser via Azure RBAC bör alltid vara begränsade till vad som krävs av rollerna. Det här är ett komplement till JIT-metoden (just in time) för Azure AD Privileged Identity Management (PIM) och bör granskas regelbundet.

Använd inbyggda roller för att allokera behörighet och skapa bara en anpassad roll vid behov.

Azure Cost Management erbjuder inbyggda roller, läsare och deltagare.

- [Azure Cost Management-läsare](../role-based-access-control/built-in-roles.md#cost-management-reader)

- [Azure Cost Management-deltagare](../role-based-access-control/built-in-roles.md#cost-management-contributor)

Vad är Azure-baserad åtkomstkontroll (Azure RBAC) ../role-based-access-control/overview.md 

- [Så här konfigurerar du RBAC i Azure](../role-based-access-control/role-assignments-portal.md) 

- [Så här använder du identitets- och åtkomstgranskningar i Azure AD](../active-directory/governance/access-reviews-overview.md)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

## <a name="data-protection"></a>Dataskydd

*Mer information finns i [Benchmark för Azure-säkerhet: Dataskydd](/azure/security/benchmarks/security-controls-v2-data-protection).*

### <a name="dp-1-discovery-classify-and-label-sensitive-data"></a>DP-1: Identifiera, klassificera och märka känsliga data

**Riktlinjer**: Vi rekommenderar att du identifierar, klassificerar och märker känsliga data så att du kan utforma lämpliga kontroller för att se till att känslig information lagras, bearbetas och överförs säkert av organisationens tekniska system.

Använd Azure Information Protection (och dess associerade skanningsverktyg) för känslig information i Office-dokument på Azure, lokalt, i Office 365 och på andra platser.

Du kan använda Azure SQL-Information Protection för att hjälpa till med klassificering och märkning av information som lagras i Azure SQL-databaser.

- [Tagga känslig information med Azure Information Protection](/azure/information-protection/what-is-information-protection) 

- [Så här implementerar du identifiering av Azure SQL-data](/azure/sql-database/sql-database-data-discovery-and-classification)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="dp-2-protect-sensitive-data"></a>DP-2: Skydda känsliga data

**Riktlinjer**: Vi rekommenderar att du skyddar känsliga data genom att begränsa åtkomsten med hjälp av rollbaserad åtkomstkontroll (RBAC) i Azure, nätverksbaserade åtkomstkontroller och specifika kontroller i Azure-tjänster (till exempel kryptering i SQL och andra databaser).

För att säkerställa konsekvent åtkomstkontroll bör alla typer av åtkomstkontroll justeras mot din strategi för företagssegmentering. Strategin för företagssegmentering bör också informeras om platsen för känsliga eller affärskritiska data och system.

När det gäller den underliggande plattformen, som hanteras av Microsoft, behandlar Microsoft allt kundinnehåll som känsligt och skyddar det mot förlust och exponering av kunddata. För att säkerställa att kunddata i Azure alltid är skyddade har Microsoft implementerat vissa standardkontroller och funktioner för dataskydd.

- [Tilldela åtkomst till Cost Management-data](costs/assign-access-acm-data.md)

- [Rollbaserad åtkomstkontroll (RBAC) i Azure](../role-based-access-control/overview.md) 

- [Förstå skydd av kunddata i Azure](../security/fundamentals/protection-customer-data.md)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="dp-3-monitor-for-unauthorized-transfer-of-sensitive-data"></a>DP-3: Övervaka för obehörig överföring av känsliga data

**Riktlinjer**: Övervaka för att upptäcka obehörig överföring av data till platser som företag inte kan se och kontrollera. Detta omfattar vanligtvis övervakning av avvikande aktiviteter (stora eller ovanliga överföringar) som kan tyda på obehörig dataexfiltrering.

Azure Storage Advanced Threat Protection (ATP) och Azure SQL ATP kan varna vid avvikande överföring av information som kan tyda på obehörig överföring av känslig information.

Azure Information Protection (AIP) innehåller övervakningsfunktioner för information som har klassificerats och märkts.

Om det krävs för att förhindra skydd mot dataförlust (DLP) kan du använda en värdbaserad DLP-lösning för att genomdriva detektionskontroller och/eller förebyggande kontroller för att förhindra dataexfiltrering.

- [Aktivera Azure SQL ATP](../azure-sql/database/threat-detection-overview.md) 

- [Aktivera Azure Storage ATP](https://docs.microsoft.com/azure/storage/common/storage-advanced-threat-protection?tabs=azure-security-center)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="dp-5-encrypt-sensitive-data-at-rest"></a>DP-5: Kryptera känsliga vilande data

**Riktlinjer**: Som ett komplement till åtkomstkontrollerna stöder Azure Cost Managements exportfunktion Azure Storage-kryptering av vilande data för att skydda mot ”out of band”-attacker (till exempel åtkomst till underliggande lagring) med hjälp av Microsoft-hanterad nyckelkryptering. Dessa standardinställningar bidrar till att göra det svårt för angripare att läsa eller ändra data.

Mer information finns i:

- [Azure Storage-kryptering av vilande data](../storage/common/storage-service-encryption.md)

- [Förstå kryptering vid vila i Azure](../security/fundamentals/encryption-atrest.md#encryption-at-rest-in-microsoft-cloud-services)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

## <a name="asset-management"></a>Tillgångshantering

*Mer information finns i [Benchmark för Azure-säkerhet: Tillgångshantering](/azure/security/benchmarks/security-controls-v2-asset-management).*

### <a name="am-1-ensure-security-team-has-visibility-into-risks-for-assets"></a>AM-1: Se till att säkerhetsteamet har insyn i risker för tillgångar

**Riktlinjer**: Se till att säkerhetsteamen beviljas behörigheter för säkerhetsläsare i din Azure-klient och prenumerationer som gör att de kan övervaka säkerhetsrisker med hjälp av Azure Security Center. 

Beroende på hur säkerhetsteamets ansvarsområden är strukturerade kan ansvaret för övervakning av säkerhetsrisker ligga hos en central säkerhetsgrupp eller ett lokalt team. Detta innebär att säkerhetsinsikter och -risker alltid måste samlas centralt inom en organisation. 

Behörigheter för säkerhetsläsare kan tillämpas brett över en hel klientorganisation (rothanteringsgrupp) eller omfattas av hanteringsgrupper eller specifika prenumerationer. 

Obs! Ytterligare behörigheter kan krävas för att få insyn i arbetsbelastningar och tjänster. 

- [Översikt över säkerhetsläsarrollen](../role-based-access-control/built-in-roles.md#security-reader)

- [Översikt över Azure-hanteringsgrupper](../governance/management-groups/overview.md)

**Azure Security Center-övervakning**: Inte tillgänglig för tillfället

**Ansvar**: Kund

### <a name="am-4-ensure-security-of-asset-lifecycle-management"></a>AM-4: Garantera säker livscykelhantering för tillgångar

**Riktlinjer**: Upprätta eller uppdatera säkerhetsprinciper som kan användas för att hantera processer för livscykelhantering för ändringar med potentiellt stora konsekvenser. Sådana ändringar kan till exempel avse: identitetsleverantörer och åtkomst, datakänslighet, nätverkskonfiguration och tilldelning av administrativa privilegier.

Ta bort Azure-resurser när de inte längre behövs.

- [Ta bort Azure-resursgrupp och resurs](../azure-resource-manager/management/delete-resource-group.md)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

## <a name="logging-and-threat-detection"></a>Loggning och hotidentifiering

*Mer information finns i [Benchmark för Azure-säkerhet: Loggning och hotidentifiering](/azure/security/benchmarks/security-controls-v2-logging-threat-detection).*

### <a name="lt-2-enable-threat-detection-for-azure-identity-and-access-management"></a>LT-2: Aktivera hotidentifiering för identitets- och åtkomsthantering i Azure

**Riktlinjer**: Azure AD innehåller följande användarloggar som kan visas i Azure AD- rapportering eller integreras med Azure Monitor, Azure Sentinel eller andra SIEM/övervakningsverktyg för mer avancerade användningsfall med övervakning och analys:

- Inloggningsaktiviteter – Rapporten för inloggningsaktiviteter ger information om användningen av hanterade program och användares inloggningsaktiviteter.

- Granskningsloggar – Ger spårbarhet via loggar för alla ändringar som gjorts via olika funktioner i Azure AD. Exempel på granskningsloggar är de resursändringar som görs i Azure AD, som att lägga till eller ta bort användare, appar, grupper, roller och principer.

- Riskfyllda inloggningar – En riskfylld inloggning indikerar ett potentiellt inloggningsförsök av någon annan än användarkontots ägare.

- Användare som har flaggats för risk – En användare som har flaggats för risk indikerar att ett användarkonto kan ha komprometterats.

Azure Security Center kan också avisera om vissa misstänkta aktiviteter, till exempel ett högt antal misslyckade autentiseringsförsök eller föråldrade konton i prenumerationen. Utöver den grundläggande övervakningen av säkerhetsrutiner kan hotskyddsmodulen i Azure Security Center även samla in mer detaljerade säkerhetsaviseringar från enskilda Azure Compute-resurser (virtuella datorer, containrar, App Service), data resurser (SQL DB och lagring) och lager i Azure-tjänsten. Med den här funktionen kan du få insyn i kontoavvikelser i de enskilda resurserna.

Vi rekommenderar också att du regelbundet granskar användare som tilldelats roller i ditt Enterprise-avtal (EA). 

- [Hantera Azure Enterprise-roller](manage/understand-ea-roles.md)

- [Granska aktivitetsrapporter i Azure Active Directory](../active-directory/reports-monitoring/concept-audit-logs.md) 

- [Aktivera Azure Identity Protection](../active-directory/identity-protection/overview-identity-protection.md)

- [Skydd mot hot i Azure Security Center](/azure/security-center/threat-protection)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

## <a name="incident-response"></a>Incidenthantering

*Mer information finns i [Benchmark för Azure-säkerhet: Incidenthantering](/azure/security/benchmarks/security-controls-v2-incident-response).*

### <a name="ir-1-preparation--update-incident-response-process-for-azure"></a>IR-1: Förberedelse – uppdatera incidenthanteringsprocessen för Azure

**Riktlinjer**: Se till att din organisation har processer för att hantera säkerhetsincidenter, har uppdaterat dessa processer för Azure och utnyttjar dem regelbundet för att säkerställa beredskap.

- [Implementera säkerhet i företagsmiljön](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Referensguide för incidenthantering](/microsoft-365/downloads/IR-Reference-Guide.pdf)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="ir-2-preparation--setup-incident-notification"></a>IR-2: Förberedelser – konfigurera incidentmeddelande

**Riktlinjer**: Konfigurera kontaktinformation för säkerhetsincidenter i Azure Security Center. Microsoft använder kontaktinformationen för att kontakta dig om Microsoft Security Response Center (MRSC) upptäcker att en obehörig part har kommit åt dina data. Du har också alternativ för att anpassa incidentaviseringar och -meddelanden i olika Azure-tjänster baserat på dina behov av incidenthantering. 

- [Så här ställer du in säkerhetskontakt i Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Azure Security Center-övervakning**: Ja

**Ansvar**: Kund

### <a name="ir-3-detection-and-analysis--create-incidents-based-on-high-quality-alerts"></a>IR-3: Identifiering och analys – skapa incidenter baserat på aviseringar med hög kvalitet

**Riktlinjer**: Se till att du har en process för att skapa aviseringar med hög kvalitet och att mäta kvaliteten på aviseringar. På så sätt kan du dra slutsatser från tidigare incidenter och prioritera aviseringar för analytiker, så att de inte slösar tid på falska positiva identifieringar. 

Aviseringar med hög kvalitet kan byggas utifrån erfarenhet från tidigare incidenter, validerade community-källor och verktyg som utformats för att generera och rensa aviseringar genom att slå samman och korrelera olika signalkällor. 

Azure Security Center tillhandahåller aviseringar med hög kvalitet för flera olika typer av Azure-tillgångar. Du kan använda anslutningsprogrammet för ASC-data för att strömma aviseringarna till Azure Sentinel. Med Azure Sentinel kan du skapa avancerade aviseringsregler för att generera incidenter automatiskt för en undersökning. 

Exportera dina Azure Security Center-aviseringar och -rekommendationer med hjälp av exportfunktionen för att identifiera risker med Azure-resurser. Exportera aviseringar och rekommendationer antingen manuellt eller löpande.

- [Så här konfigurerar du export](../security-center/continuous-export.md)

- [Så här strömmar du aviseringar till Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Azure Security Center-övervakning**: Inte tillgänglig för tillfället

**Ansvar**: Kund

### <a name="ir-4-detection-and-analysis--investigate-an-incident"></a>IR-4: Identifiering och analys – undersöka en incident

**Riktlinjer**: Se till att analytiker kan fråga och använda olika datakällor när de undersöker potentiella incidenter, så att de kan skapa en fullständig överblick över vad som hände. Diverse loggar bör samlas in för att spåra en potentiell angripares aktiviteter över hela händelsekedjan för att undvika att blinda fläckar.  Du bör också se till att insikter och kunskaper fångas upp för andra analytiker och för framtida historiska referenser.  

Datakällorna för undersökningen innehåller de centraliserade loggningskällor som redan har samlats in från tjänster som omfattas och system som körs, men kan även innehålla:

- Nätverksdata – Använd nätverkssäkerhetsgruppers flödesloggar, Azure Network Watcher och Azure Monitor för att avbilda nätverksflödesloggar och annan analysinformation. 

- Ögonblicksbilder av system som körs: 

    - Använd funktionen för ögonblicksbilder på den virtuella Azure-datorn för att skapa en ögonblicksbild av det aktiva systemets disk. 

    - Använd operativsystemets interna minnesdumpningsfunktion för att skapa en ögonblicksbild av det aktiva systemets minne.

    - Använd funktionen för ögonblicksbilder i Azure-tjänsterna eller din programvaras egna funktion för att skapa ögonblicksbilder av de system som körs.

Azure Sentinel tillhandahåller omfattande dataanalyser i praktiskt taget alla loggkällor och en ärendehanteringsportal för att hantera hela livscykeln för incidenter. Affärsinformation under en undersökning kan associeras med en incident för spårning och rapportering. 

- [Ögonblicksbild av en Windows-dators disk](../virtual-machines/windows/snapshot-copy-managed-disk.md)

- [Ögonblicksbild av en Linux-dators disk](../virtual-machines/linux/snapshot-copy-managed-disk.md)

- [Microsoft Azure har stöd för diagnostikinformation och insamling av minnesdumpar](https://azure.microsoft.com/support/legal/support-diagnostic-information-collection/) 

- [Undersöka incidenter med Azure Sentinel](../sentinel/tutorial-investigate-cases.md)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="ir-5-detection-and-analysis--prioritize-incidents"></a>IR-5: Identifiering och analys – prioritera incidenter

**Riktlinjer**: Tillhandahålla en kontext till analytiker om vilka incidenter att fokusera på först baserat på aviseringens allvarlighetsgrad och tillgångens känslighet. 

Azure Security Center tilldelar en allvarlighetsgrad till varje avisering för att hjälpa dig att prioritera vilka aviseringar som bör undersökas först. Allvarlighetsgraden baseras på hur tillförlitligt Security Center är i sökandet eller på den analys som användes för att utfärda aviseringen, samt konfidensnivån att det fanns en skadlig avsikt bakom den aktivitet som ledde till aviseringen.

Markera även resurser med taggar och skapa ett namngivningssystem för att identifiera och kategorisera Azure-resurser, i synnerhet sådana som används för bearbetning av känsliga data.  Det är ditt ansvar att prioritera åtgärdandet av aviseringar baserat på allvarlighetsgraden för de Azure-resurser och den miljö där incidenten inträffade.

- [Säkerhetsaviseringar i Azure Security Center](../security-center/security-center-alerts-overview.md)

- [Använda taggar för att organisera dina Azure-resurser](/azure/azure-resource-manager/resource-group-using-tags)

**Azure Security Center-övervakning**: Inte tillgänglig för tillfället

**Ansvar**: Kund

### <a name="ir-6-containment-eradication-and-recovery--automate-the-incident-handling"></a>IR-6: Inneslutning, utrotning och återställning – automatisera incidenthanteringen

**Riktlinjer**: Automatisera manuella återkommande uppgifter för att förkorta svarstiden och minska belastningen på analytiker. Manuella uppgifter tar längre tid att köra, vilket saktar ned varje incident och minskar antalet incidenter som en analytiker kan hantera. Manuella uppgifter gör dessutom analytikern mer utmattad, vilket ökar risken för mänskliga fel som i sin tur orsakar fördröjningar, samt minskar analytikernas förmåga att fokusera på komplexa uppgifter på ett effektivt sätt. Använd automatiseringsfunktioner för arbetsflöde i Azure Security Center och Azure Sentinel för att automatiskt utlösa åtgärder eller köra en spelbok för att svara på inkommande säkerhetsaviseringar. Spelboken vidtar åtgärder som att skicka meddelanden, inaktivera konton och isolera problematiska nätverk. 

- [Konfigurera arbetsflödesautomation i Security Center](../security-center/workflow-automation.md)

- [Konfigurera automatiska svar på hot i Azure Security Center](../security-center/tutorial-security-incident.md#triage-security-alerts)

- [Konfigurera automatiska svar på hot i Azure Sentinel](../sentinel/tutorial-respond-threats-playbook.md)

**Azure Security Center-övervakning**: Inte tillgänglig för tillfället

**Ansvar**: Kund

## <a name="posture-and-vulnerability-management"></a>Status- och sårbarhetshantering

*Mer information finns i [Benchmark för Azure-säkerhet: Status- och sårbarhetshantering](/azure/security/benchmarks/security-controls-v2-posture-vulnerability-management).*

### <a name="pv-7-rapidly-and-automatically-remediate-software-vulnerabilities"></a>PV-7: Åtgärda sårbarheter för programvara snabbt och automatiskt

**Riktlinjer**: Distribuera programuppdateringar snabbt för att åtgärda programvarurelaterade sårbarheter i operativsystem och program.

Prioritera att använda ett gemensamt riskbedömningsprogram (t.ex. vanliga sårbarhetsbedömningssystem) eller standardiserade riskklassificeringar som tillhandahålls av ditt externa genomsökningsverktyg och anpassas efter din miljö med hjälp av kontext över vilka program som utgör en hög säkerhetsrisk och som kräver hög drifttid.

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="pv-8-conduct-regular-attack-simulation"></a>PV-8: Genomför regelbunden attacksimulering

**Riktlinjer**: Genomför vid behov genomslagstestning eller Red Team-aktiviteter på dina Azure-resurser och se till att alla kritiska säkerhetsbrister åtgärdas.
Följ Microsoft Clouds deltagarregler för genomslagstestning för att se till att genomslagstesterna inte strider mot Microsofts principer. Använd Microsofts strategi för och verkställande av Red Team-indelning och genomslagstester för live-webbplatser mot Microsoft-hanterad molninfrastruktur, tjänster och program.

- [Genomslagstestning i Azure](../security/fundamentals/pen-testing.md)

- [Deltagarregler för genomslagstestning](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1) 

- [Red Team-indelning i Microsoft Cloud](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Delad

## <a name="governance-and-strategy"></a>Styrning och strategi

*Mer information finns i [Benchmark för Azure-säkerhet: Styrning och strategi](/azure/security/benchmarks/security-controls-v2-governance-strategy).*

### <a name="gs-1-define-asset-management-and-data-protection-strategy"></a>GS-1: Definiera strategin för tillgångshantering och dataskydd 

**Riktlinjer**: Se till att dokumentera och förmedla en tydlig strategi för kontinuerlig övervakning och skydd av system och data. Prioritera upptäckt, utvärdering, skydd och övervakning av affärskritiska data och system. 

Strategin bör omfatta dokumenterad vägledning, principer och standarder för följande element: 

-   Standard för klassificering av data i enlighet med affärsriskerna

-   Insyn för säkerhetsorganisationen i risker och tillgångsinventering 

-   Säkerhetsorganisations godkännande av Azure-tjänster att använda 

-   Säkerhet för tillgångar via deras livscykel

-   Strategi för nödvändig åtkomstkontroll i enlighet med organisationens dataklassificering

-   Användning av inbyggda säkerhetsfunktioner för Azure- och tredje partsdata

-   Krypteringskrav för användningsfall med data under transport och i vila

-   Lämpliga kryptografiska standarder

Mer information finns i följande resurser:
- [Rekommendation för Azure Security Architecture – lagring, data och kryptering](https://docs.microsoft.com/azure/architecture/framework/security/storage-data-encryption?toc=/security/compass/toc.json&amp;bc=/security/compass/breadcrumb/toc.json)

- [Grundläggande Azure-säkerhet – säkerhet, kryptering och lagring av data i Azure](../security/fundamentals/encryption-overview.md)

- [Cloud Adoption Framework – bästa praxis för datasäkerhet och kryptering i Azure](https://docs.microsoft.com/azure/security/fundamentals/data-encryption-best-practices?toc=/azure/cloud-adoption-framework/toc.json&amp;bc=/azure/cloud-adoption-framework/_bread/toc.json)

- [Benchmark för Azure-säkerhet – tillgångshantering](/azure/security/benchmarks/security-controls-v2-asset-management)

- [Benchmark för Azure-säkerhet – dataskydd](/azure/security/benchmarks/security-controls-v2-data-protection)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="gs-2-define-enterprise-segmentation-strategy"></a>GS-2: Definiera strategi för företagssegmentering 

**Riktlinjer**: Upprätta en företagsomfattande strategi för att segmentera åtkomst till tillgångar med en kombination av identitet, nätverk, program, prenumeration, hanteringsgrupp och andra kontroller.

Du bör noggrant balansera behovet av säkerhetsseparation med behovet av att möjliggöra daglig drift av de system som behöver kommunicera med varandra och komma åt data.

Se till att segmenteringsstrategin implementeras konsekvent över olika kontrolltyper, inklusive nätverkssäkerhet, identitets- och åtkomstmodeller samt modeller för programbehörighet/-åtkomst och kontroller för mänskliga processer.

- [Vägledning om segmenteringsstrategi i Azure (video)](/security/compass/microsoft-security-compass-introduction#azure-components-and-reference-model-2151)

- [Vägledning om segmenteringsstrategi i Azure (dokument)](/security/compass/governance#enterprise-segmentation-strategy)

- [Justera nätverkssegmentering med strategi för företagssegmentering](/security/compass/network-security-containment#align-network-segmentation-with-enterprise-segmentation-strategy)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="gs-3-define-security-posture-management-strategy"></a>GS-3: Definiera strategi för hantering av säkerhetsstatus

**Riktlinjer**: Mät och minimera kontinuerligt risker för enskilda tillgångar och den miljö som de finns i. Prioritera tillgångar med högt värde och högexponerade attackytor, till exempel publicerade program, ingångs- och utgångspunkter för nätverk, slutpunkter för användare och administratörer osv.

- [Benchmark för Azure-säkerhet – status- och sårbarhetshantering](/azure/security/benchmarks/security-controls-v2-posture-vulnerability-management)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="gs-4-align-organization-roles-responsibilities-and-accountabilities"></a>GS-4: Justera roller, ansvarsområden och ansvar

**Riktlinjer**: Se till att dokumentera och förmedla en tydlig strategi för roller och ansvarsområden i din säkerhetsorganisation. Prioritera att presentera tydligt ansvar för säkerhetsbeslut, utbilda alla i den delade ansvarsmodellen och utbilda tekniska team om teknik för att skydda molnet.

- [Bästa praxis för Azure-säkerhet 1 – personer: Utbilda team i resan mot molnsäkerhet](/azure/cloud-adoption-framework/security/security-top-10#1-people-educate-teams-about-the-cloud-security-journey)

- [Bästa praxis för Azure-säkerhet 2 – personer: Utbilda team i teknik för molnsäkerhet](/azure/cloud-adoption-framework/security/security-top-10#2-people-educate-teams-on-cloud-security-technology)

- [Bästa praxis för Azure-säkerhet 3 – process: Tilldela ansvar för beslut kring molnsäkerhet](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="gs-5-define-network-security-strategy"></a>GS-5: Definiera en strategi för nätverkssäkerhet

**Riktlinjer**: Upprätta en metod för Azure-nätverkssäkerhet som en del av din organisations övergripande strategi för säker åtkomstkontroll.  

Strategin bör omfatta dokumenterad vägledning, principer och standarder för följande element: 

-   Centraliserad nätverkshantering och säkerhetsansvar

-   Modell för virtuell nätverkssegmentering i linje med företagets segmenteringsstrategi

-   Åtgärdsstrategi i olika hot- och attackscenarier

-   Strategi för Internets kanter, in- och utgångspunkter

-   Strategi för hybridmoln och lokala anslutningar

-   Aktuella artefakter för nätverkssäkerhet (t.ex. nätverksdiagram, arkitektur för referensnätverk)

Mer information finns i följande resurser:
- [Bästa praxis för Azure-säkerhet 11 – arkitektur. En enhetlig säkerhetsstrategi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Benchmark för Azure-säkerhet – nätverkssäkerhet](/azure/security/benchmarks/security-controls-v2-network-security)

- [Översikt över Azure-nätverkssäkerhet](../security/fundamentals/network-overview.md)

- [Arkitekturstrategi för företagsnätverk](/azure/cloud-adoption-framework/ready/enterprise-scale/architecture)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="gs-6-define-identity-and-privileged-access-strategy"></a>GS-6: Definiera strategi för identiteter och privilegierad åtkomst

**Riktlinjer**: Upprätta en metod för Azure-identitet och priviligierad åtkomst som en del av organisationens övergripande strategi för säker åtkomstkontroll.  

Strategin bör omfatta dokumenterad vägledning, principer och standarder för följande element: 

-   Ett centraliserat identitets- och autentiseringssystem och dess anslutningar till andra interna och externa identitetssystem

-   Starka autentiseringsmetoder i olika användnings fall och förhållanden

-   Skydd av användare med höga privilegier

-   Övervakning och hantering av avvikande användaraktiviteter  

-   Process för granskning och avstämning av användaridentitet och -åtkomst

Mer information finns i följande resurser:

- [Benchmark för Azure-säkerhet – identitetshantering](/azure/security/benchmarks/security-controls-v2-identity-management)

- [Benchmark för Azure-säkerhet – privilegierad åtkomst](/azure/security/benchmarks/security-controls-v2-privileged-access)

- [Bästa praxis för Azure-säkerhet 11 – arkitektur. En enhetlig säkerhetsstrategi](/azure/cloud-adoption-framework/security/security-top-10#11-architecture-establish-a-single-unified-security-strategy)

- [Säkerhetsöversikt för identitetshantering i Azure](../security/fundamentals/identity-management-overview.md)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

### <a name="gs-7-define-logging-and-threat-response-strategy"></a>GS-7: Definiera strategi för loggning och hothantering

**Riktlinjer**: Upprätta en strategi för loggning och hothantering för att snabbt upptäcka och åtgärda hot och samtidigt uppfylla kraven för efterlevnad. Prioritera att ge analytiker högkvalitativa varningar och sömlösa upplevelser så att de kan fokusera på hot i stället för på integrering och manuella åtgärder. 

Strategin bör omfatta dokumenterad vägledning, principer och standarder för följande element: 

-   SecOps-organisationens roll och ansvarsområden 

-   En väldefinierad process för incidenthantering i linje med NIST eller annat branschramverk 

-   Insamling och kvarhållning av loggar för att stödja hotidentifiering, incidentsvar och krav på efterlevnad

-   Centraliserad insyn i och korrelationsuppgifter om hot med hjälp av Säkerhetsinformation och händelsehantering, inbyggda Azure-funktioner och andra källor 

-   Kommunikations- och aviseringsplan för dina kunder, leverantörer och offentliga intressenter

-   Användning av inbyggda Azure- och tredjeparts-plattformar för incidenthantering, till exempel loggning och hotidentifiering, analys och attackåtgärder och -utrotning

-   Processer för hantering av incidenter och aktiviteter efter incidenter, till lärdomar och kvarhållning av bevis

Mer information finns i följande resurser:

- [Benchmark för Azure-säkerhet – loggning och hotidentifiering](/azure/security/benchmarks/security-controls-v2-logging-threat-detection)

- [Benchmark för Azure-säkerhet – incidenthantering](/azure/security/benchmarks/security-controls-v2-incident-response)

- [Bästa praxis för Azure-säkerhet 4 – process. Uppdatera incidenthanteringsprocesser för molnet](/azure/cloud-adoption-framework/security/security-top-10#4-process-update-incident-response-ir-processes-for-cloud)

- [Beslutsguide för ramverk för Azure-införande, loggning och rapportering](/azure/cloud-adoption-framework/decision-guides/logging-and-reporting/)

- [Azure i företagsskala, hantering och övervakning](/azure/cloud-adoption-framework/ready/enterprise-scale/management-and-monitoring)

**Azure Security Center-övervakning**: Inte tillämpligt

**Ansvar**: Kund

## <a name="next-steps"></a>Nästa steg

- Se [Översikt över Benchmark för Azure-säkerhet V2](/azure/security/benchmarks/overview)
- Läs mer om [säkerhetsbaslinjer för Azure](/azure/security/benchmarks/security-baselines-overview)
