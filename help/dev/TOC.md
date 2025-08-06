---
user-guide-title: Adobe Target Developer Guide
breadcrumb-title: Target Developer Guide
user-guide-description: Lär dig hur du skräddarsyr och personanpassar dina kunders upplevelser för att maximera intäkterna från dina webbplatser och mobilsajter, appar, sociala medier och andra digitala kanaler.
source-git-commit: 8f24ffe82e16de0dbbd86d3baf0e76d826a98a9a
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 3%

---


# Adobe Target Developer Guide {#developer}

+ [Adobe Target Developer Guide](overview.md)
+ Komma igång {#implementation}
   + Innan du implementerar {#before-implement}
      + [Innan du implementerar](before-implement/considerations-before-you-implement-target.md)
      + [Förbered implementering av Target](before-implement/prepare-to-implement-target.md)
   + Integritet och säkerhet {#privacy}
      + [Sekretessöversikt](before-implement/privacy/privacy.md)
      + [Sekretess- och dataskyddsbestämmelser](before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)
      + [Målcookies](before-implement/privacy/cookie-behavior.md)
      + [Ta bort målcookien](before-implement/privacy/cookie-deleting.md)
      + [Inverkan av borttagning av cookies från tredje part på Target (at.js)](/help/dev/before-implement/privacy/third-party-cookie-deprecation.md)
      + [Google Chrome SameSite cookie-principer](before-implement/privacy/google-chrome-samesite-cookie-policies.md)
      + [Apple Intelligent Tracking Prevention (ITP) 2.x](before-implement/privacy/apple-itp-2x.md)
      + [CSP-direktiv (Content Security Policy)](before-implement/privacy/content-security-policy.md)
      + [Tillåtelselista: Hörnkantsnoder](before-implement/privacy/allowlist-edges.md)
   + Metoder för att hämta data till Target {#methods}
      + [Översikt över metoder](before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md)
      + [Sidparametrar](before-implement/methods-to-get-data-into-target/page-parameters.md)
      + [Profilattribut på sidan](before-implement/methods-to-get-data-into-target/in-page-profile-attributes.md)
      + [Skriptprofilattribut](before-implement/methods-to-get-data-into-target/script-profile-attributes.md)
      + [Dataleverantörer](before-implement/methods-to-get-data-into-target/data-providers.md)
      + [API för gruppprofiluppdatering](before-implement/methods-to-get-data-into-target/bulk-profile-update-api.md)
      + [API för enkel profiluppdatering](before-implement/methods-to-get-data-into-target/single-profile-update-api.md)
      + [Kundattribut](before-implement/methods-to-get-data-into-target/customer-attributes.md)
      + [Profil-API-inställningar](before-implement/methods-to-get-data-into-target/profile-api-settings.md)
   + [Översikt över målsäkerhet](before-implement/target-security-overview.md)
   + [Webbläsare som stöds](before-implement/supported-browsers.md)
   + [TLS-krypteringsändringar (Transport Layer Security)](before-implement/tls-transport-layer-security-encryption.md)
   + [CNAME och Adobe Target](before-implement/implement-cname-support-in-target.md)
+ Implementering på klientsidan {#client-side}
   + [Översikt: implementera Target för webben på klientsidan](implement/client-side/overview.md)
   + Implementering av Adobe Experience Platform Web SDK {#aep}
      + [Implementeringsöversikt för Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)
      + [Använd Adobe Target och Web SDK för personalisering](/help/dev/implement/client-side/aep-web-sdk/target-overview.md)
      + [Programimplementering på en sida](/help/dev/implement/client-side/aep-web-sdk/spa-implementation.md)
      + [Åtkomst till svarstoken](/help/dev/implement/client-side/aep-web-sdk/accessing-response-tokens.md)
      + [använd mbox3rdPartyId](/help/dev/implement/client-side/aep-web-sdk/using-mbox-3rdpartyid.md)
      + [Jämför at.js-biblioteket med Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md)
   + at.js-implementering {#at-js-implementation}
      + Hur at.js fungerar {#at-js}
         + [at.js Översikt över JavaScript Library](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)
         + [at.js - översikt](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
         + [Hur at.js hanterar flimmer](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
         + [at.js-integreringar](/help/dev/implement/client-side/atjs/how-atjs-works/target-atjs-integrations.md)
      + Så här distribuerar du at.js {#deploy-at-js}
         + [Så här distribuerar du at.js](implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md)
         + [Implementera mål med Adobe Experience Platform](implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)
         + [Implementera mål utan tagghanterare](implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)
         + [Implementera mål med Dynamic Tag Manager (DTM)](implement/client-side/atjs/how-to-deployatjs/implement-target-using-dtm.md)
         + [Implementera mål för SPA (Single Page Applications)](implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)
      + Enhetsbeslut {#on-device-decisioning}
         + [Översikt över beslut på enheter](implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md)
         + [Funktioner som stöds](implement/client-side/atjs/on-device-decisioning/supported-features.md)
         + [Regelartefakt](implement/client-side/atjs/on-device-decisioning/rule-artifact.md)
         + [Felsökning](implement/client-side/atjs/on-device-decisioning/troubleshooting-on-device-decisioning.md)
      + Funktionerna at.js {#functions-overview}
         + [at.js - funktionsöversikt](implement/client-side/atjs/atjs-functions/atjs-functions.md)
         + [adobe.target.getOffer()](implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md)
         + [adobe.target.getOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
         + [adobe.target.applyOffer()](implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md)
         + [adobe.target.applyOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)
         + [adobe.target.triggerView() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)
         + [adobe.target.trackEvent()](implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
         + [mboxCreate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)
         + [targetGlobalSettings()](implement/client-side/atjs/atjs-functions/targetglobalsettings.md)
         + [mboxDefine() och mboxUpdate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)
         + [targetPageParams()](implement/client-side/atjs/atjs-functions/targetpageparams.md)
         + [targetPageParamsAll()](implement/client-side/atjs/atjs-functions/targetpageparamsall.md)
         + [registerExtension() - at.js 1.x](implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)
         + [sendNotifications() - at.js 2.1](implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)
         + [at.js, anpassade händelser](implement/client-side/atjs/atjs-functions/atjs-custom-events.md)
         + [Felsöka at.js med Adobe Experience Cloud Debugger](implement/client-side/target-debugging-atjs/target-debugging-atjs.md)
         + [Använd molnbaserade instanser med Target](implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md)
      + [at.js Frågor och svar](implement/client-side/atjs/target-atjs-faq.md)
      + [versionsinformation för at.js](implement/client-side/atjs/target-atjs-versions.md)
      + [Uppgradera från at.js 1.x till at.js 2.x](implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)
      + [at.js cookies](implement/client-side/atjs/atjs-cookies.md)
   + [Tips för användaragent och klient](implement/client-side/atjs/user-agent-and-client-hints.md)
   + Förstå den globala mbox {#global-mbox}
      + [Förstå den globala mbox-översikten](implement/client-side/atjs/global-mbox/global-mbox-overview.md)
      + [Anpassa en global mbox](implement/client-side/atjs/global-mbox/customize-global-mbox.md)
      + [Använd en global mbox från en äldre implementering](implement/client-side/atjs/global-mbox/mbox-global-target-standard.md)
      + [Skicka parametrar till en global mbox](implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)
      + [Vanliga frågor om Global Mbox](implement/client-side/atjs/global-mbox/global-mbox-faq.md)
+ Implementering på serversidan {#server-side}
   + [Serversida: implementera Target overview](implement/server-side/server-side-overview.md)
   + [Komma igång med SDK:er för mål](implement/server-side/sdk-guides/getting-started/getting-started.md)
   + [Exempelappar](implement/server-side/sdk-guides/sample-apps/sample-apps.md)
   + [Övergång från äldre Target-API:er till Adobe I/O](implement/server-side/transition-from-target-classic-apis.md)
   + Grundprinciper {#core-principles}
      + [Översikt över grundläggande principer](implement/server-side/sdk-guides/core-principles/overview.md)
      + [Användar-ID och buckning](implement/server-side/sdk-guides/core-principles/user-identification-and-bucketing.md)
      + [Målgruppsanpassning](implement/server-side/sdk-guides/core-principles/audience-targeting.md)
      + [Händelsespårning](implement/server-side/sdk-guides/core-principles/event-tracking.md)
      + [Användarbehörigheter och egenskaper](implement/server-side/sdk-guides/core-principles/user-permissions-and-properties.md)
   + Integrering {#integration}
      + [Integrationsöversikt](implement/server-side/sdk-guides/integration-with-experience-cloud/overview.md)
      + [Experience Cloud ID Service (ECID)](implement/server-side/sdk-guides/integration-with-experience-cloud/ecid.md)
      + [Analyser för målrapportering (A4T)](implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md)
      + [AAM segment](implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md)
   + Beslut på enheten {#on-device-decisioning}
      + [Översikt över beslut på enheter](implement/server-side/sdk-guides/on-device-decisioning/overview.md)
      + Regelartefakt {#rule-artifact}
         + [Översikt över regelartefakt](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)
         + [Hämta via Adobe Target SDK](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-sdk.md)
         + [Ladda ned via JSON-nyttolast](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-json.md)
         + [Exempel på regelartefakt](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-example.md)
      + [Kör A/B-tester med funktionsflaggor](implement/server-side/sdk-guides/on-device-decisioning/execute-ab-tests-with-feature-flags.md)
      + [Utför funktionstester med attribut](implement/server-side/sdk-guides/on-device-decisioning/execute-feature-tests-with-attributes.md)
      + [Hantera utrullningar för funktionstester](implement/server-side/sdk-guides/on-device-decisioning/manage-rollouts-for-feature-tests.md)
      + [Leverera personalisering](implement/server-side/sdk-guides/on-device-decisioning/deliver-personalization.md)
      + [Översikt över funktioner som stöds](implement/server-side/sdk-guides/on-device-decisioning/supported-features.md)
      + [Felsökning av enhetsbeslut](implement/server-side/sdk-guides/on-device-decisioning/troubleshooting.md)
      + [Bästa praxis](implement/server-side/sdk-guides/best-practices/best-practices.md)
   + Node.js SDK Reference {#node-js}
      + [Node.js SDK - översikt](implement/server-side/node-js/overview.md)
      + [Installera Node.js SDK](implement/server-side/node-js/install-sdk.md)
      + [Initiera Node.js SDK](implement/server-side/node-js/initialize-sdk.md)
      + [Erbjudanden (Node.js)](implement/server-side/node-js/get-offers.md)
      + [Hämta attribut (Node.js)](implement/server-side/node-js/get-attributes.md)
      + [Skicka meddelanden (Node.js)](implement/server-side/node-js/send-notifications.md)
      + [SDK Events (Node.js)](implement/server-side/node-js/sdk-events.md)
      + [Logger (Node.js)](implement/server-side/node-js/logger.md)
      + [Proxykonfiguration (Node.js)](implement/server-side/node-js/proxy-configuration.md)
   + Java SDK Reference {#java}
      + [Java SDK - översikt](implement/server-side/java/overview.md)
      + [Installera Java SDK](implement/server-side/java/install-sdk.md)
      + [Initiera Java SDK](implement/server-side/java/initialize-sdk.md)
      + [Erbjudanden (Java)](implement/server-side/java/get-offers.md)
      + [Hämta attribut (Java)](implement/server-side/java/get-attributes.md)
      + [Skicka meddelanden (Java)](implement/server-side/java/send-notifications.md)
      + [SDK Events (Java)](implement/server-side/java/sdk-events.md)
      + [Logger (Java)](implement/server-side/java/logger.md)
      + [Asynkrona begäranden (Java)](implement/server-side/java/asynchronous-requests.md)
      + [Proxykonfiguration (Java)](implement/server-side/java/proxy-configuration.md)
      + [Anpassad Java (HTTP Client Configuration)](implement/server-side/java/custom-http-client.md)
      + [Verktygsmetoder (Java)](implement/server-side/java/utility-methods.md)
   + .NET SDK Reference {#net}
      + [.NET SDK - översikt](implement/server-side/net/overview.md)
      + [Installera .Net SDK](implement/server-side/net/install-sdk.md)
      + [Initiera .NET SDK](implement/server-side/net/initialize-sdk.md)
      + [Erbjudanden (.NET)](implement/server-side/net/get-offers.md)
      + [Hämta attribut (.NET)](implement/server-side/net/get-attributes.md)
      + [Skicka meddelanden (.NET)](implement/server-side/net/send-notifications.md)
      + [SDK Events (.NET)](implement/server-side/net/sdk-events.md)
      + [Asynkrona begäranden (.NET)](implement/server-side/net/asynchronous-requests.md)
   + Python SDK Reference {#python}
      + [Python SDK - översikt](implement/server-side/python/overview.md)
      + [Installera Python SDK](implement/server-side/python/install-sdk.md)
      + [Initiera Python SDK](implement/server-side/python/initialize-sdk.md)
      + [Erbjudanden (Python)](implement/server-side/python/get-offers.md)
      + [Hämta attribut (Python)](implement/server-side/python/get-attributes.md)
      + [Skicka meddelanden (Python)](implement/server-side/python/send-notifications.md)
      + [SDK Events (Python)](implement/server-side/python/sdk-events.md)
      + [Asynkrona begäranden (Python)](implement/server-side/python/asynchronous-requests.md)
      + [Logger (Python)](implement/server-side/python/logger.md)
+ [Hybrid-implementering](implement/hybrid/hybrid-overview.md)
+ [Rekommendationer och implementering](implement/recommendations/recommendations.md)
+ [Rekommendationer - betaversion](/help/dev/implement/recommendations/recommendations-beta.md)
+ Implementering av mobilappar {#mobile-apps}
   + [Mål för mobilappar - översikt](implement/mobile/overview.md)
   + [Förhandsvisning av målmobiler](implement/mobile/target-mobile-preview.md)
   + [Använd platstjänst](implement/mobile/use-location-service.md)
   + [Mål för mobilappar - frågor och svar](implement/mobile/mobile-faq.md)
   + [Implementera Target med AEP Mobile SDK i en inbyggd app med webbvyer](/help/dev/implement/mobile/native-app.md)
+ E-postimplementering {#implement-email}
   + [E-post: implementera målöversikt](implement/email/overview.md)
   + [Skapa en Adbox för en bild](implement/email/testing-content-with-the-adbox.md)
   + [Testa en e-postbild i Adbox](implement/email/testing-email-image-adbox.md)
   + [Arbeta med regissörer](implement/email/working-with-redirectors.md)
+ API-guider {#api}
   + [API-översikt för mål](/help/dev/before-administer/target-api-overview.md)
   + [Konfigurera autentisering för mål-API:er](/help/dev/before-administer/configure-authentication.md)
   + Guide för leverans-API {#delivery-api}
      + [Översikt över leverans-API](/help/dev/implement/delivery-api/overview.md)
      + [SDK för interaktion med leverans-API](/help/dev/before-implement/delivery-api-overview/sdks.md)
      + [Komma igång](/help/dev/before-implement/delivery-api-overview/getting-started.md)
      + [Användarbehörigheter (Premium)](/help/dev/before-implement/delivery-api-overview/user-permissions.md)
      + [Identifiera besökare](/help/dev/before-implement/delivery-api-overview/identifying-visitors.md)
      + [Enskild leverans eller gruppleverans](/help/dev/before-implement/delivery-api-overview/single-or-batch.md)
      + [Förhämtning](/help/dev/before-implement/delivery-api-overview/prefetch.md)
      + [Meddelanden](/help/dev/before-implement/delivery-api-overview/notifications.md)
      + [Integrering med Experience Cloud](before-implement/delivery-api-overview/integration.md)
      + [Överväganden och kända begränsningar](/help/dev/before-implement/delivery-api-overview/known-limitations.md)
      + [Klienttips](/help/dev/before-implement/delivery-api-overview/client-hints.md)
      + [Leverans-API](/help/dev/implement/delivery-api/delivery-api.md)
   + Admin-API {#admin-api}
      + [API-översikt för administratörer](before-administer/admin-api-overview/admin-api-overview.md)
      + [Adobe Target Admin API](/help/dev/administer/admin-api/admin-api-overview-new.md)
   + Profil-API {#profile-apis}
      + [API-översikt för profiler](/help/dev/administer/profile-api/profiles-api.md)
      + [Hämta profiler](/help/dev/administer/profile-api/profile-fetch.md)
      + [Uppdatera profiler](/help/dev/administer/profile-api/profile-api-overview.md)
      + [API för enkel profiluppdatering](/help/dev/administer/profile-api/profile-single-api.md)
      + [API för gruppprofiluppdatering](/help/dev/administer/profile-api/profile-bulk-api.md)
   + [Rapporterings-API](/help/dev/administer/reporting-api/reporting-api.md)
   + Rekommendations-API {#recommendations-api}
      + [API-översikt för rekommendationer](before-administer/recs-api/overview.md)
      + [Hantera katalogen med API:er](before-administer/recs-api/manage-catalog.md)
      + [Hantera anpassade villkor](before-administer/recs-api/manage-custom-criteria.md)
      + [Använd leverans-API med rekommendationer](before-administer/recs-api/fetch-recs-server-side-delivery-api.md)
      + [Rekommendations-API](/help/dev/administer/recommendations-api/recommendations-api.md)
   + API för modeller {#models-api}
      + [API-översikt för modeller (Blockeringslistning)](before-administer/models-api.md)
      + [API för modeller](/help/dev/administer/models-api/models-api-overview.md)
   + [Adobe Admin Console API:er](/help/dev/before-implement/delivery-api-overview/adobe-console-api.md)
   + [Adobe Experience Platform Edge Network Server API](/help/dev/before-implement/delivery-api-overview/aep-edge-network-server-api.md)
+ Implementeringsmönster {#implementation-patterns}
   + [Översikt över implementeringsmönster](/help/dev/patterns/pattern-overview.md)
   + Rekommendationer implementeringsmönster med at.js {#atjs}
      + [Rekommendationer implementeringsmönster med hjälp av översikten at.js](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md)
      + [Initiera SDK:er](/help/dev/patterns/recs-atjs/initialize-sdk.md)
      + [Konfigurera datainsamling](/help/dev/patterns/recs-atjs/data-collection.md)
      + [Återge upplevelser](/help/dev/patterns/recs-atjs/render-experiences.md)
      + [Meddela mål](/help/dev/patterns/recs-atjs/notify-target.md)