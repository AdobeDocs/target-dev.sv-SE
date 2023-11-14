---
title: Konfigurera autentisering för [!DNL Adobe Target] API:er
description: Hur skapar jag autentiseringstoken som behövs för att kunna interagera med [!DNL Adobe Target] API?
feature: APIs/SDKs, Administration & Configuration
exl-id: fc67363c-6527-40aa-aff1-350b5af884ab
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '1865'
ht-degree: 2%

---

# Konfigurera autentisering för [!DNL Adobe Target] API:er

The [!DNL Adobe Target] Admin-API:er, inklusive [!DNL Recommendations Admin] API:er, skyddas av autentisering för att säkerställa att endast behöriga användare använder dem för åtkomst [!DNL Adobe Target]. Använd [Adobe Developer Console](https://developer.adobe.com/console/home) för att hantera denna autentisering för alla [!DNL Adobe Experience Cloud solutions], inklusive [!DNL Adobe Target].

>[!IMPORTANT]
>
>De JWT-autentiseringsuppgifter (Service Account) som beskrivs i den här artikeln kommer att bli inaktuella till förmån för de nya OAuth Server-till-Server-autentiseringsuppgifterna.
>
>JWT-inloggningsuppgifterna (Service Account) kommer att fortsätta att fungera fram till 1 januari 2025. Du måste migrera programmet eller integreringen för att kunna använda de nya autentiseringsuppgifterna för OAuth Server-till-server före 1 januari 2025.
>
>Mer information och stegvisa instruktioner för att migrera din integrering finns i [Migrerar från JWT-autentiseringsuppgifter (Service Account) till OAuth Server-till-Server-autentiseringsuppgifter](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/){target=_blank} i *Developer Console* dokumentation.
>
>Mer information om hur du konfigurerar nya OAuth-autentiseringsuppgifter finns i [Autentiseringsimplementering av OAuth Server-till-Server](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/){target=_blank} i *Developer Console* dokumentation.

Här följer de inledande stegen som krävs för att generera de äldre JWT-autentiseringstoken som behövs för att kunna interagera med [!DNL Adobe Target] API:

1. Skapa ett projekt (som tidigare kallades integration) i [!DNL Adobe Developer Console].
1. Exportera projektinformation till Postman.
1. Generera en token för innehavaråtkomst.
1. Testa token för innehavaråtkomst.

## Krav

| Resurs | Information |
| --- | --- |
| Postman | Om du vill slutföra de här stegen måste du skaffa [Postman](https://www.postman.com/downloads/) för ditt operativsystem. Postman Basic är kostnadsfritt när man skapar konto. Krävs inte för att använda [!DNL Adobe Target] API:er i allmänhet gör Postman API-arbetsflöden enklare och [!DNL Adobe Target] innehåller flera Postman-samlingar som hjälper dig att köra API:erna och lära dig hur de fungerar. Resten av guiden förutsätter att man har kunskap om Postman. Om du behöver hjälp kan du läsa [Postman-dokumentation](https://learning.getpostman.com/). |
| Referenser | Du bör känna till följande resurser under resten av den här guiden:<ul><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Dokumentation för Target Admin och Profile API](../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API-dokumentation](https://developer.adobe.com/target/administer/recommendations-api/)</li></ul> |

## Skapa ett Adobe I/O-projekt

I det här avsnittet kommer du åt [!DNL Adobe Developer Console] och skapa ett projekt för [!DNL Adobe Target]. Mer information finns i [dokumentation om projekt](https://developer.adobe.com/developer-console/docs/guides/projects/).

&lt;!---(1. Generera din privata nyckel och ditt offentliga certifikat, enligt [dokumentation om autentisering](https://developer.adobe.com/developer-console/docs/guides/authentication/). // [//]: # (enligt beskrivning i **Steg 1** av [Så här konfigurerar du Adobe IO: Autentisering - Steg för steg](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). När du är klar med steg 1 går du tillbaka till den här guiden och fortsätter med steg 2 nedan. // Resultatet av det här steget bör vara att en `private.key` och en `certificate_pub.crt` -fil. Återgå till den här guiden när du har skapat de här två filerna.)—>

1. I [Adobe Admin Console](https://adminconsole.adobe.com/), kontrollera att [!DNL Adobe] användarkontot har tilldelats båda [Produktadministratör](https://helpx.adobe.com/enterprise/using/admin-roles.html) och [Utvecklare](https://helpx.adobe.com/enterprise/using/manage-developers.html) behörighet till [!DNL Target].

1. I [Adobe Developer Console](https://developer.adobe.com/console/home)väljer du [!UICONTROL Experience Cloud Organization] som du vill skapa den här integreringen för. (Observera att du antagligen bara har tillgång till en enda [!UICONTROL Experience Cloud Organization].)

   ![configure-io-target-create-project2.png](assets/configure-io-target-createproject2.png)

1. Klicka på **[!UICONTROL Create new project]**.

   ![configure-io-target-create-project3.png](assets/configure-io-target-createproject3.png)

1. Klicka **[!UICONTROL Add API]** för att lägga till ett REST API i ditt projekt för att få åtkomst [!DNL Adobe] tjänster och produkter.

   ![Lägg till API](assets/configure-io-target-createproject4.png)

1. Välj **[!DNL Adobe Target]** som [!DNL Adobe] tjänster som du vill integrera med. Klicka på **[!UICONTROL Next]** som visas.

   ![configure-io-target-createproject5](assets/configure-io-target-createproject5.png)

1. Välj ett alternativ för att associera offentliga och privata nycklar med tjänstkontointegreringen som du skapar för [!DNL Target]. I detta exempel väljer du **[!UICONTROL Option 1: Generate a key pair]** och klicka **[!UICONTROL Generate keypair]**.

   ![configure-io-target-createproject6](assets/configure-io-target-createproject6.png)

1. Anteckna den automatiskt hämtade konfigurationsfilen (`config`), som innehåller din privata nyckel. Klicka på **[!UICONTROL Next]**.

   ![configure-io-target-createproject7](assets/configure-io-target-createproject7.png)

1. Kontrollera var i filsystemet `config`, som är den komprimerade konfigurationsfilen som skapades i föregående steg. Igen, den här `config` filen innehåller din privata nyckel, som du behöver senare. Den exakta platsen i filsystemet kan skilja sig från den som visas här.

   ![configure-io-target-createproject8](assets/configure-io-target-createproject8.png)

1. Gå tillbaka till Adobe Developer Console och välj [produktprofil(er)](https://helpx.adobe.com/enterprise/using/manage-products-and-profiles.html) motsvarar de egenskaper som du använder Adobe Recommendations i. (Om du inte använder egenskaper markerar du alternativet Standardarbetsyta.) Klicka på **[!UICONTROL Save configured API]**.

   ![configure-io-target-createproject9](assets/configure-io-target-createproject9.png)

1. Klicka på **[!UICONTROL Create Integration]**. Du bör få ett tillfälligt meddelande om att ditt API har konfigurerats.
1. Som ett sista steg byter du namn på projektet till ett namn som är mer meningsfullt än det ursprungliga `Project 1`. Det gör du genom att navigera till projektet med navigeringssökvägen som visas och klicka på **[!UICONTROL Edit project]** för att komma åt **[!UICONTROL Edit Project]** modal och ge projektet ett nytt namn.

   ![configure-io-target-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
>
>I det här exemplet kallar vi vårt projekt &quot;[!DNL Target] Integrering.&quot; Om du tänker använda ditt projekt för mer än bara [!DNL Adobe Target]kan du ge den ett namn. Du kan till exempel välja att ge den namnet&quot;Adobe API:er&quot; eller&quot;Experience Cloud API:er&quot;, eftersom den kan användas med andra lösningar i Adobe Experience Cloud.

## Exportera projektinformation

Nu när du har ett Adobe-projekt som du kan använda för att komma åt [!DNL Target]måste du se till att du skickar information om det projektet tillsammans med dina Adobe API-begäranden. Dessa uppgifter krävs för att interagera med flera Adobe-API:er, inklusive flera [!DNL Target] API. Integreringsinformationen innehåller till exempel information om autentisering och autentisering som krävs av [!DNL Target] Admin-API:er. Om du vill använda API:erna med Postman måste du därför hämta dessa uppgifter till Postman.

Det finns många sätt att ange projektinformation i Postman, men i det här avsnittet kan vi utnyttja vissa färdiga funktioner och samlingar. Först (i det här avsnittet) exporteras information om din integrering till en Postman-miljö. Därefter (i följande avsnitt) genererar du en token för innehavaråtkomst som ger dig tillgång till de nödvändiga Adobe-resurserna.

>[!NOTE]
>
>För videoinstruktioner för alla Experience Cloud-lösningar, inklusive [!DNL Target], se [Använd Postman med Experience Platform API:er](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html). Följande avsnitt gäller [!DNL Target] API: 1. Skapa och exportera Experience Platform API till Postman 2. Generera en åtkomsttoken med Postman. De här stegen finns också nedan.

1. Fortfarande i [Adobe Developer Console](https://developer.adobe.com/console/home)navigera till det nya projektets **[!UICONTROL Service Account (JWT)]** autentiseringsuppgifter. Använd antingen den vänstra navigeringen eller **[!UICONTROL Credentials]** som visas.

   ![JWT1](assets/configure-io-target-jwt1.png)

   I **[!UICONTROL Credential details]** kan du se **[!UICONTROL Public key(s)]**, **[!UICONTROL Client ID]** och annan information om ditt tjänstkonto.

   ![JWT1a](assets/configure-io-target-jwt1a.png)

1. Klicka för att navigera till information om **[!DNL Adobe Target]** API. Använd antingen den vänstra navigeringen eller **Anslutna produkter och tjänster** som visas.

   ![JWT2](assets/configure-io-target-jwt2.png)

1. Klicka **[!UICONTROL Download for Postman]** > **[!UICONTROL Service Account (JWT)]** för att skapa en JSON-fil som hämtar din autentiseringsinformation för en Postman-miljö.

   ![JWT3](assets/configure-io-target-jwt3.png)

   Observera JSON-filen i filsystemet.

   ![JWT3a](assets/configure-io-target-jwt3a.png)

1. I Postman klickar du på kugghjulsikonen för att hantera dina miljöer och sedan på **[!UICONTROL Import]** för att importera JSON-filen (miljö).

   ![JWT4](assets/configure-io-target-jwt4.png)

1. Välj filen och klicka på **[!UICONTROL Open]**.

   ![JWT5](assets/configure-io-target-jwt5.png)

1. I POSTMAN **Hantera miljöer** modal klickar du på namnet på den nyligen importerade miljön för att inspektera den. (Ditt miljönamn kan skilja sig från det som visas här. Redigera namnet efter behov. Det behöver inte nödvändigtvis matcha namnet på [!DNL Adobe] projekt.)

   ![JWT6](assets/configure-io-target-jwt6.png)

1. Anteckning `CLIENT_SECRET` och `API_KEY` (tillsammans med andra variabler) har sina värden ifyllda i förväg, som hämtats från integreringen enligt definitionen i Adobe Developer Console. (Postman `CLIENT_SECRET` variabeln ska matcha `CLIENT SECRET` autentiseringsuppgifter för Adobe som de visas i Developer Console, och `API_KEY` i Postman bör likaså matcha `CLIENT ID` i Developer Console.) Observera `PRIVATE_KEY`, `JWT_TOKEN`och `ACCESS_TOKEN` är tomma. Vi börjar med att erbjuda `PRIVATE_KEY` värde.

   ![JWT7](assets/configure-io-target-jwt7.png)

1. Öppna ditt filsystem `config` och öppna `private` nyckelfil.

   ![JWT8](assets/configure-io-target-jwt8.png)

1. Markera och kopiera hela innehållet i `private` nyckelfil.

   ![JWT9](assets/configure-io-target-jwt9.png)

1. I Postman klistrar du in ditt privata nyckelvärde i **[!UICONTROL INITIAL VALUE]** och **[!UICONTROL CURRENT VALUE]** fält.

   ![JWT10](assets/configure-io-target-jwt10.png)

1. Klicka **[!UICONTROL Update]** och stänger Miljöerna modal.

## Generera token för innehavaråtkomst

I det här avsnittet skapar du en token för innehavaråtkomst som krävs för att autentisera din interaktion med [!DNL Adobe Target] API. Om du vill generera en token för innehavaråtkomst måste du skicka din integreringsinformation (som anges i de föregående avsnitten) till [Adobe Identity Management Service (IMS)](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md). Det finns några olika sätt att göra detta, men i den här guiden drar vi nytta av en Postman-samling som innehåller ett färdigbyggt IMS-anrop som gör processen direkt och enkel. När du har importerat samlingen kan du återanvända den när det behövs för att generera nya tokens inte bara för [!DNL Adobe Target], men även andra Adobe-API:er.

1. Navigera till [Adobe Identity Management Service API-exempelanrop](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims).

   ![token1](assets/configure-io-target-generatetoken1.png)

1. Klicka på **[!UICONTROL Adobe I/O Access Token Generation Postman collection]**.

   ![token2](assets/configure-io-target-generatetoken2.png)

1. Hämta rå-JSON för den här samlingen genom att klicka **[!UICONTROL Raw]** och sedan kopiera den resulterande JSON-filen till Urklipp. (Du kan också spara raw-JSON som en .json-fil.)

   ![token3](assets/configure-io-target-generatetoken3.png)

1. I Postman importerar du samlingen genom att klistra in och skicka rå JSON från Urklipp. (Du kan också överföra den .json-fil som du sparade.) Klicka på **[!UICONTROL Continue]**.

   ![token4](assets/configure-io-target-generatetoken4.png)

1. Välj **[!UICONTROL IMS: JWT Generate + Auth via User Token]** begär Postman-samlingen för generering av Adobe I/O Access-token, kontrollera att din miljö är markerad och klicka sedan på **[!UICONTROL Send]** för att generera token.

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >Denna innehavaråtkomsttoken gäller i 24 timmar. Skicka begäran igen när du behöver generera en ny token.

1. Öppna Hantera miljöer modal igen och välj din miljö.

   ![token6](assets/configure-io-target-jwt11.png)

1. Anteckna `ACCESS_TOKEN` och `JWT_TOKEN` värden har nu fyllts i.

   ![token7](assets/configure-io-target-generatetoken7.png)

Fråga: Måste jag använda Postman-samlingen Adobe I/O Access Token Generation för att generera JSON Web Token (JWT) och innehavaråtkomsttoken?

Svar: Nej. Adobe I/O Access Token Generation Postman-samlingen är tillgänglig som ett bekvämt sätt att enklare generera JWT- och Bearer-åtkomsttoken i Postman. Du kan också använda funktionerna i Adobe Developer Console för att manuellt generera en token för innehavaråtkomst.

## Testa innehavaråtkomsttoken

I den här övningen använder du din nya innehavaråtkomsttoken genom att skicka en API-begäran som hämtar en lista över aktiviteter från din [!DNL Target] konto. Ett lyckat svar indikerar att [!DNL Adobe] projekt och autentisering fungerar som förväntat för att API:t ska kunna användas.

1. Importera [[!DNL Adobe Target] Admin-API:er för Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection). Följ alla instruktioner tills samlingen har importerats till Postman.

   ![testtoken1](assets/configure-io-target-testtoken0.png)

1. Expandera samlingen och notera **[!UICONTROL List activities]** begäran.

   ![testtoken1](assets/configure-io-target-testtoken1.png)

1. Observera att variabler som `{{access_token}}` är från början olösta. Du kan lösa detta på flera olika sätt - du kan till exempel definiera en ny samlingsvariabel som kallas `{{access_token}}`- men i den här handboken ändrar du i stället API-begäran så att du kan utnyttja den Postman-miljö du använde tidigare. På så sätt kan miljön fortsätta att fungera som en enda, konsekvent konsolidering av alla variabler som är gemensamma för olika API:er i Adobe.

   ![testtoken2](assets/configure-io-target-testtoken2.png)

1. Text som ska ersättas `{{access_token}}` med `{{ACCESS_TOKEN}}`.

   ![testtoken3](assets/configure-io-target-testtoken3.png)

1. Text som ska ersättas `{{api_key}}` med `{{API_KEY}}`.

   ![testtoken4](assets/configure-io-target-testtoken4.png)

1. Text som ska ersättas `{{tenant}}` med `{{TENANT_ID}}`. Anteckning `{{TENANT_ID}}` känns inte igen än.

   ![testtoken4](assets/configure-io-target-testtoken4a.png)

1. Öppna modal Hantera miljöer och välj din miljö.

   ![JWT11](assets/configure-io-target-jwt11.png)

1. Skriv för att lägga till en ny `{{TENANT_ID}}` miljövariabel. Kopiera och klistra in värdet för ditt klient-ID i **[!UICONTROL INITIAL VALUE]** och **[!UICONTROL CURRENT VALUE]** fält för dina nya `TENANT_ID` miljövariabel.

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >Klient-ID:t skiljer sig från din [!DNL Target] `clientcode`. Klient-ID:t finns i URL:en när du är inloggad på [!DNL Target]. Logga in på Adobe Experience Cloud och öppna för att få ditt klient-ID [!DNL Target]och klicka på Target-kortet. Använd det klient-ID som anges i URL-underdomänen. Om din URL-adress till exempel är inloggad på [!DNL Adobe Target] är `<https://mycompany.experiencecloud.adobe.com/...>` blir ditt klient-ID&quot;mincompany&quot;.

1. Skicka din begäran efter att du har valt rätt miljö. Du bör få ett svar med din lista över aktiviteter.

   ![testtoken6](assets/configure-io-target-testtoken6.png)

Nu när du har verifierat din autentisering från Adobe kan du använda den för att interagera med [!DNL Adobe Target] API:er (och andra Adobe-API:er). Du kan till exempel [Använda Recommendations API:er](recs-api/overview.md) för att skapa eller hantera rekommendationer, eller så kan du använda dem med [Målleverans-API](/help/dev/implement/delivery-api/overview.md).
