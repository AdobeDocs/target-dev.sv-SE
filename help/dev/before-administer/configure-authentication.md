---
title: Konfigurera autentisering för  [!DNL Adobe Target] API:er
description: Hur genererar jag autentiseringstoken som behövs för att kunna interagera med  [!DNL Adobe Target] API:er?
feature: APIs/SDKs, Administration & Configuration
exl-id: fc67363c-6527-40aa-aff1-350b5af884ab
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '1785'
ht-degree: 0%

---

# Konfigurera autentisering för [!DNL Adobe Target] API:er

Administratörs-API:erna [!DNL Adobe Target], inklusive [!DNL Recommendations Admin], skyddas av autentisering för att säkerställa att endast behöriga användare använder dem för åtkomst till [!DNL Adobe Target]. Använd [Adobe Developer Console](https://developer.adobe.com/console/home) för att hantera den här autentiseringen för alla [!DNL Adobe Experience Cloud solutions], inklusive [!DNL Adobe Target].

>[!IMPORTANT]
>
>De JWT-autentiseringsuppgifter (Service Account) som beskrivs i den här artikeln kommer att bli inaktuella till förmån för de nya OAuth Server-till-Server-autentiseringsuppgifterna.
>
>JWT-inloggningsuppgifterna (Service Account) kommer att fortsätta att fungera fram till 1 januari 2025. Du måste migrera programmet eller integreringen för att kunna använda de nya autentiseringsuppgifterna för OAuth Server-till-server före 1 januari 2025.
>
>Mer information och stegvisa instruktioner för att migrera din integrering finns i [Migrera från JWT-autentiseringsuppgifter (Service Account) till autentiseringsuppgifter för OAuth Server-till-server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/){target=_blank} i *Developer Console* -dokumentationen.
>
>Mer information om hur du konfigurerar nya OAuth-autentiseringsuppgifter finns i [Implementering av autentiseringsuppgifter för OAuth Server-till-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/){target=_blank} i *Developer Console* -dokumentationen.

Här följer de inledande stegen som krävs för att skapa de äldre JWT-autentiseringstoken som krävs för att kunna interagera med [!DNL Adobe Target] API:er:

1. Skapa ett projekt (som tidigare kallades integration) i [!DNL Adobe Developer Console].
1. Exportera projektinformation till Postman.
1. Generera en token för innehavaråtkomst.
1. Testa token för innehavaråtkomst.

## Krav

| Resurs | Information |
| --- | --- |
| Postman | Om du vill slutföra de här stegen hämtar du [Postman-appen](https://www.postman.com/downloads/) för ditt operativsystem. Postman Basic är kostnadsfritt när man skapar konto. Även om det inte krävs för att använda [!DNL Adobe Target] API:er i allmänhet, underlättar Postman API-arbetsflöden och [!DNL Adobe Target] innehåller flera Postman-samlingar som hjälper dig att köra API:erna och lära dig hur de fungerar. Resten av guiden förutsätter att man har kunskap om Postman. Mer information finns i [Postman-dokumentationen](https://learning.getpostman.com/). |
| Referenser | Du bör känna till följande resurser under resten av den här guiden:<ul><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Dokumentation för måladministratör och profil-API](../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API-dokumentation](https://developer.adobe.com/target/administer/recommendations-api/)</li></ul> |

## Skapa ett Adobe I/O-projekt

I det här avsnittet kommer du åt [!DNL Adobe Developer Console] och skapar ett projekt för [!DNL Adobe Target]. Mer information finns i [dokumentationen om projekt](https://developer.adobe.com/developer-console/docs/guides/projects/).

&lt;!—(1. Generera din privata nyckel och ditt offentliga certifikat enligt [dokumentationen för autentisering](https://developer.adobe.com/developer-console/docs/guides/authentication/). // [//]: # (enligt beskrivningen i **Steg 1** av [Så här konfigurerar du Adobe I/O: Autentisering - Steg för steg](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). När du är klar med steg 1 går du tillbaka till den här guiden och fortsätter med steg 2 nedan. // Resultatet av det här steget bör vara att en `private.key`-fil och en `certificate_pub.crt`-fil skapas. Återgå till den här guiden när du har skapat de här två filerna.)—>

1. I [Adobe Admin Console](https://adminconsole.adobe.com/) kontrollerar du att ditt [!DNL Adobe]-användarkonto har beviljats åtkomst på både [produktadministratörsnivå](https://helpx.adobe.com/se/enterprise/using/admin-roles.html) och [utvecklarnivå](https://helpx.adobe.com/se/enterprise/using/manage-developers.html) till [!DNL Target].

1. I [Adobe Developer Console](https://developer.adobe.com/console/home) väljer du den [!UICONTROL Experience Cloud Organization] som du vill skapa den här integreringen för. (Observera att du förmodligen bara har åtkomst till en enda [!UICONTROL Experience Cloud Organization].)

   ![configure-io-target-createProject2.png](assets/configure-io-target-createproject2.png)

1. Klicka på **[!UICONTROL Create new project]**.

   ![configure-io-target-createProject3.png](assets/configure-io-target-createproject3.png)

1. Klicka på **[!UICONTROL Add API]** om du vill lägga till ett REST API i projektet för att få tillgång till [!DNL Adobe]-tjänster och -produkter.

   ![Lägg till API](assets/configure-io-target-createproject4.png)

1. Välj **[!DNL Adobe Target]** som den [!DNL Adobe]-tjänst du vill integrera med. Klicka på knappen **[!UICONTROL Next]** som visas.

   ![configure-io-target-createProject5](assets/configure-io-target-createproject5.png)

1. Välj ett alternativ för att associera offentliga och privata nycklar med tjänstkontointegreringen som du skapar för [!DNL Target]. I det här exemplet väljer du **[!UICONTROL Option 1: Generate a key pair]** och klickar på **[!UICONTROL Generate keypair]**.

   ![configure-io-target-createProject6](assets/configure-io-target-createproject6.png)

1. Anteckna den automatiskt hämtade konfigurationsfilen (`config`) som innehåller din privata nyckel. Klicka på **[!UICONTROL Next]**.

   ![configure-io-target-createProject7](assets/configure-io-target-createproject7.png)

1. Kontrollera platsen för `config`, som är den komprimerade konfigurationsfilen som skapades i föregående steg, i filsystemet. Återigen innehåller den här `config`-filen din privata nyckel, som du behöver senare. Den exakta platsen i filsystemet kan skilja sig från den som visas här.

   ![configure-io-target-createProject8](assets/configure-io-target-createproject8.png)

1. Gå tillbaka till Adobe Developer Console och välj den [produktprofil(er)](https://helpx.adobe.com/se/enterprise/using/manage-products-and-profiles.html) som motsvarar de egenskaper som du använder Adobe Recommendations i. (Om du inte använder egenskaper väljer du standardalternativet för Workspace.) Klicka på **[!UICONTROL Save configured API]**.

   ![configure-io-target-createProject9](assets/configure-io-target-createproject9.png)

1. Klicka på **[!UICONTROL Create Integration]**. Du bör få ett tillfälligt meddelande om att ditt API har konfigurerats.
1. Som ett sista steg byter du namn på projektet till ett mer meningsfullt namn än det ursprungliga `Project 1`. Det gör du genom att navigera till projektet med navigeringssökvägen som visas, klicka på **[!UICONTROL Edit project]** för att komma åt den spärrade **[!UICONTROL Edit Project]** och byta namn på projektet.

   ![configure-io-target-createProject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
>
>I det här exemplet namnger vi projektet [!DNL Target]-integrering. Om du planerar att använda ditt projekt för mer än bara [!DNL Adobe Target] kan du ge det ett lämpligt namn. Du kan till exempel välja att ge den namnet&quot;Adobe API:er&quot; eller&quot;Experience Cloud API:er&quot;, eftersom den kan användas med andra lösningar i Adobe Experience Cloud.

## Exportera projektinformation

Nu när du har ett Adobe-projekt som du kan använda för att komma åt [!DNL Target] måste du se till att skicka information om det projektet tillsammans med dina Adobe API-begäranden. Dessa uppgifter krävs för att interagera med flera Adobe API:er, inklusive flera [!DNL Target] API:er. Integreringsinformationen innehåller till exempel autentiserings- och autentiseringsinformation som krävs av [!DNL Target] Admin API:er. Om du vill använda API:erna med Postman måste du därför hämta dessa uppgifter till Postman.

Det finns många sätt att ange projektinformation i Postman, men i det här avsnittet kan vi utnyttja vissa färdiga funktioner och samlingar. Först (i det här avsnittet) exporteras information om din integrering till en Postman-miljö. Därefter (i följande avsnitt) genererar du en token för innehavaråtkomst som ger dig tillgång till de nödvändiga Adobe-resurserna.

>[!NOTE]
>
>Om du vill se videoinstruktioner för en Experience Cloud-lösning, inklusive [!DNL Target], går du till [Använd Postman med Experience Platform API:er](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html?lang=sv-SE). Följande avsnitt är relevanta för [!DNL Target] API:er: 1. Skapa och exportera Experience Platform API till Postman 2. Generera en åtkomsttoken med Postman. De här stegen finns också nedan.

1. Navigera fortfarande i [Adobe Developer Console](https://developer.adobe.com/console/home) för att visa det nya projektets **[!UICONTROL Service Account (JWT)]** inloggningsuppgifter. Använd antingen den vänstra navigeringen eller **[!UICONTROL Credentials]**-avsnittet så som visas.

   ![JWT1](assets/configure-io-target-jwt1.png)

   I **[!UICONTROL Credential details]** kan du visa din **[!UICONTROL Public key(s)]**, **[!UICONTROL Client ID]** och annan information som rör ditt tjänstkonto.

   ![JWT1a](assets/configure-io-target-jwt1a.png)

1. Klicka för att navigera till information om API:t **[!DNL Adobe Target]**. Använd antingen den vänstra navigeringen eller **Anslutna produkter och tjänster** så som visas.

   ![JWT2](assets/configure-io-target-jwt2.png)

1. Klicka på **[!UICONTROL Download for Postman]** > **[!UICONTROL Service Account (JWT)]** för att skapa en JSON-fil som hämtar din autentiseringsinformation för en Postman-miljö.

   ![JWT3](assets/configure-io-target-jwt3.png)

   Observera JSON-filen i filsystemet.

   ![JWT3a](assets/configure-io-target-jwt3a.png)

1. I Postman klickar du på kugghjulsikonen för att hantera dina miljöer och sedan på **[!UICONTROL Import]** för att importera JSON-filen (miljö).

   ![JWT4](assets/configure-io-target-jwt4.png)

1. Välj filen och klicka på **[!UICONTROL Open]**.

   ![JWT5](assets/configure-io-target-jwt5.png)

1. I Postman **Hantera miljöer** modal klickar du på namnet på den nyligen importerade miljön för att inspektera den. (Ditt miljönamn kan skilja sig från det som visas här. Redigera namnet efter behov. Det behöver inte nödvändigtvis matcha namnet på projektet [!DNL Adobe].)

   ![JWT6](assets/configure-io-target-jwt6.png)

1. Obs! `CLIENT_SECRET` och `API_KEY` (tillsammans med andra variabler) har sina värden förifyllda, tagna från din integrering enligt definitionen i Adobe Developer Console. (Postman-variabeln `CLIENT_SECRET` ska matcha de `CLIENT SECRET` Adobe-autentiseringsuppgifter som visas i Developer Console, och `API_KEY` i Postman ska likaså matcha `CLIENT ID` i Developer Console.) Obs! `PRIVATE_KEY`, `JWT_TOKEN` och `ACCESS_TOKEN` är däremot tomma. Vi börjar med att ange värdet `PRIVATE_KEY`.

   ![JWT7](assets/configure-io-target-jwt7.png)

1. Öppna `config`-filen från filsystemet och öppna `private`-nyckelfilen.

   ![JWT8](assets/configure-io-target-jwt8.png)

1. Markera och kopiera hela innehållet i nyckelfilen `private`.

   ![JWT9](assets/configure-io-target-jwt9.png)

1. Klistra in värdet för din privata nyckel i fälten **[!UICONTROL INITIAL VALUE]** och **[!UICONTROL CURRENT VALUE]** i Postman.

   ![JWT10](assets/configure-io-target-jwt10.png)

1. Klicka på **[!UICONTROL Update]** och stäng modala miljöer.

## Generera token för innehavaråtkomst

I det här avsnittet genererar du din innehavaråtkomsttoken, vilket krävs för att autentisera din interaktion med [!DNL Adobe Target] API:er. Om du vill generera en token för innehavaråtkomst måste du skicka din integreringsinformation (som fastställts i de föregående avsnitten) till [Adobe Identity Management-tjänsten (IMS)](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md). Det finns några olika sätt att göra detta, men i den här guiden drar vi nytta av en Postman-samling som innehåller ett färdigbyggt IMS-anrop som gör processen direkt och enkel. När du har importerat samlingen kan du återanvända den när det behövs för att generera nya tokens inte bara för [!DNL Adobe Target], utan även för andra Adobe-API:er.

1. Navigera till [Adobe Identity Management Service API-exempelanrop](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims).

   ![token1](assets/configure-io-target-generatetoken1.png)

1. Klicka på **[!UICONTROL Adobe I/O Access Token Generation Postman collection]**.

   ![token2](assets/configure-io-target-generatetoken2.png)

1. Hämta rå-JSON för den här samlingen genom att klicka på **[!UICONTROL Raw]** och sedan kopiera den resulterande JSON-filen till Urklipp. (Du kan också spara raw-JSON som en .json-fil.)

   ![token3](assets/configure-io-target-generatetoken3.png)

1. I Postman importerar du samlingen genom att klistra in och skicka rå JSON från Urklipp. (Du kan också överföra den .json-fil som du sparade.) Klicka på **[!UICONTROL Continue]**.

   ![token4](assets/configure-io-target-generatetoken4.png)

1. Markera **[!UICONTROL IMS: JWT Generate + Auth via User Token]**-begäran i Postman-samlingen för generering av Adobe I/O Access-token, kontrollera att din miljö är markerad och klicka på **[!UICONTROL Send]** för att generera token.

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >Denna innehavaråtkomsttoken gäller i 24 timmar. Skicka begäran igen när du behöver generera en ny token.

1. Öppna Hantera miljöer modal igen och välj din miljö.

   ![token6](assets/configure-io-target-jwt11.png)

1. Observera att värdena `ACCESS_TOKEN` och `JWT_TOKEN` nu är ifyllda.

   ![token7](assets/configure-io-target-generatetoken7.png)

Fråga: Måste jag använda Postman-samlingen Adobe I/O Access Token Generation för att generera JSON Web Token (JWT) och innehavaråtkomsttoken?

Svar: Nej. Adobe I/O Access Token Generation Postman-samlingen är tillgänglig som ett bekvämt sätt att enklare generera JWT- och Bearer-åtkomsttoken i Postman. Du kan också använda funktionerna i Adobe Developer Console för att manuellt generera en token för innehavaråtkomst.

## Testa innehavaråtkomsttoken

I den här övningen använder du din nya innehavaråtkomsttoken genom att skicka en API-begäran som hämtar en lista över aktiviteter från ditt [!DNL Target]-konto. Ett lyckat svar indikerar att ditt [!DNL Adobe]-projekt och din autentisering fungerar som förväntat för att kunna använda API:t.

1. Importera [[!DNL Adobe Target] Admin-API:erna för Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection). Följ alla instruktioner tills samlingen har importerats till Postman.

   ![testtoken1](assets/configure-io-target-testtoken0.png)

1. Expandera samlingen och notera **[!UICONTROL List activities]**-begäran.

   ![testtoken1](assets/configure-io-target-testtoken1.png)

1. Observera att variabler som `{{access_token}}` från början inte har matchats. Du kan lösa detta på flera olika sätt - du kan till exempel definiera en ny samlingsvariabel som heter `{{access_token}}` - men i den här guiden ändrar du API-begäran så att den utnyttjar den Postman-miljö som du använde tidigare. På så sätt kan miljön fortsätta att fungera som en enda, konsekvent konsolidering av alla variabler som är gemensamma för olika API:er i Adobe.

   ![testtoken2](assets/configure-io-target-testtoken2.png)

1. Skriv som ska ersätta `{{access_token}}` med `{{ACCESS_TOKEN}}`.

   ![testtoken3](assets/configure-io-target-testtoken3.png)

1. Skriv som ska ersätta `{{api_key}}` med `{{API_KEY}}`.

   ![testtoken4](assets/configure-io-target-testtoken4.png)

1. Skriv som ska ersätta `{{tenant}}` med `{{TENANT_ID}}`. Anteckningen `{{TENANT_ID}}` känns inte igen än.

   ![testtoken4](assets/configure-io-target-testtoken4a.png)

1. Öppna modal Hantera miljöer och välj din miljö.

   ![JWT11](assets/configure-io-target-jwt11.png)

1. Skriv för att lägga till en ny `{{TENANT_ID}}`-miljövariabel. Kopiera och klistra in ditt klient-ID-värde i fälten **[!UICONTROL INITIAL VALUE]** och **[!UICONTROL CURRENT VALUE]** för den nya `TENANT_ID` -miljövariabeln.

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >Klient-ID:t skiljer sig från [!DNL Target] `clientcode`. Klient-ID finns i URL:en när du är inloggad på [!DNL Target]. Logga in på Adobe Experience Cloud, öppna [!DNL Target] och klicka på Target-kortet för att få ditt klient-ID. Använd det klient-ID som anges i URL-underdomänen. Om din URL när du är inloggad på [!DNL Adobe Target] till exempel är `<https://mycompany.experiencecloud.adobe.com/...>` är ditt klient-ID&quot;mincompany&quot;.

1. Skicka din begäran efter att du har valt rätt miljö. Du bör få ett svar med din lista över aktiviteter.

   ![testtoken6](assets/configure-io-target-testtoken6.png)

Nu när du har verifierat din Adobe-autentisering kan du använda den för att interagera med [!DNL Adobe Target] API:er (och andra Adobe API:er). Du kan till exempel [använda Recommendations API:er](recs-api/overview.md) för att skapa eller hantera rekommendationer, eller använda dem med [Target Delivery API](/help/dev/implement/delivery-api/overview.md).
