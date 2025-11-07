---
keywords: gdpr, eu, europeisk union, sekretess, faq, vanliga frågor och svar, californias konsumentsekretesslag, ccpa, sekretess, dataskydd, opt-out, opt out, government, Regulation, gdpr5, gdpr6, gdpr7, gdpr8, gdpr9, eu0, eu1, eu2, eu3, eu4, eu5
description: Läs mer om Target och EU:s allmänna dataskyddsförordning (GDPR), Kaliforniens konsumentintegritetslag (CCPA) och andra integritetskrav.
title: Hur hanterar Target reglerna för sekretess och dataskydd?
feature: Privacy & Security
exl-id: 40bac3c5-8e6f-4a90-ac0c-eddce1dbe6c0
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '2329'
ht-degree: 0%

---

# Sekretess- och dataskyddsbestämmelser

Information om EU:s allmänna dataskyddsförordning (GDPR), Kaliforniens konsumentintegritetslag (CCPA) och andra internationella integritetskrav. Läs om hur dessa regler påverkar er organisation och Adobe Target.

## Översikt över dataskyddsförordningen och den allmänna dataskyddsförordningen

Den 25 maj 2018 trädde Europeiska unionens allmänna dataskyddsprogram i kraft. Mer information om vad den här förordningen betyder för dig finns i [GDPR och ditt företag](https://business.adobe.com/se/privacy/general-data-protection-regulation.html).

När Adobe tillhandahåller programvara och tjänster till ett företag fungerar Adobe som databehandlare för alla personuppgifter som bearbetas och lagras som en del av tillhandahållandet av dessa tjänster. Som databehandlare behandlar Adobe personuppgifter i enlighet med ditt företags tillstånd och instruktioner (till exempel enligt vad som anges i ditt avtal med Adobe).

Som personuppgiftsansvariga avgör du vilka personuppgifter som Adobe behandlar och lagrar åt dig. Om du använder Adobe Experience Cloud lösningar kan Adobe lagra personuppgifter åt dig, beroende på vilka lösningar du använder och vilken information du väljer att skicka till ditt Adobe Experience Cloud-konto. En detaljerad lista med exempel finns i [Adobe Experience Cloud sekretess](https://www.adobe.com/privacy/experience-cloud.html#collect).

Adobe Experience Cloud tillhandahåller GDPR-förberedda API:er för datakontroller som gör att de kan utföra följande uppgifter:

* Åtkomst till information om registrerade personer lagrad i mål
* Ta bort information om registrerade i mål

Mer information finns i:

* [Adobe Privacy Service - översikt](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=sv-SE)
* [Privacy Service API-guide](https://experienceleague.adobe.com/docs/experience-platform/privacy/api/overview.html?lang=sv-SE)
* [Översikt över användargränssnittet i Privacy Service](https://experienceleague.adobe.com/docs/experience-platform/privacy/ui/overview.html?lang=sv-SE)

## California Consumer Privacy Act (CCPA) - översikt

California Consumer Privacy Act (CCPA) ger Kaliforniens konsumenter nya rättigheter när det gäller deras personuppgifter och ålägger vissa enheter som bedriver verksamhet i Kalifornien dataskydd. CCPA trädde i kraft den 1 januari 2020.

På en hög nivå ger lagen Kalifornien flera nyckelrättigheter, bland annat rätten att

* Begär information (dataåtkomst)
* Avanmäl dig från försäljningen av personuppgifter (en vida definierad rätt att avanmäla delning av information med tredje part)
* Ta bort personlig information
* informeras om att personuppgifter röjs eller säljs

Om du var upptagen med att förbereda dig för Europas integritetslagstiftning (GDPR) förra året kan vissa av dessa rättigheter vara välkända och mycket av det arbete du har gjort kan återanvändas.

>[!NOTE]
>
>Åtkomst och borttagning av data som gäller för CCPA följer samma process som för GDPR.

## Anmäl dig till Adobe Target och Adobe Experience Platform

Target ger stöd för valmöjligheter via taggar i Adobe Experience Platform för att hjälpa er strategi för samtyckeshantering. Med avanmälningsfunktionen kan kunderna styra hur och när Target-taggen aktiveras. Det finns också ett alternativ via Adobe Experience Platform för att förgodkänna Target-taggen. Om du vill aktivera möjligheten att använda Opt-In i Target at.js-biblioteket bör du använda `targetGlobalSettings` och lägga till inställningen `optinEnabled=true`. I Adobe Experience Platform väljer du&quot;enable&quot; i listrutan GDPR-deltagande i installationsvyn för tillägget. Mer information finns i [Implementera mål med Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md).

I följande kodfragment visas hur du aktiverar inställningen `optinEnabled=true`:

```
window.targetGlobalSettings = {
  optinEnabled: true
};
```

>[!NOTE]
>
>Opt-in-funktionaliteten stöds i at.js version 1.7.0 och at.js 2.1.0 eller senare. Opt-in stöds inte i version 2.0.0 och 2.0.1 av at.js.

Vi rekommenderar att du använder Adobe Experience Platform för att hantera anmälan. Ytterligare detaljkontroll finns i Adobe Experience Platform för att dölja vissa delar av sidan före Target-utsändning som är användbara som en del av er strategi för samtycke.

Det finns tre scenarier att tänka på när du använder Opt-In:

1. **Target-taggen är förgodkänd via Adobe Experience Platform (eller den registrerade som tidigare godkänt Target):** Target-taggen behålls inte för samtycke och funktioner som förväntat.
1. **Måltaggen är INTE förgodkänd och `bodyHidingEnabled` är FALSE:** Måltaggen utlöses först efter det att samtycke har samlats in från kunden. Innan samtycke samlas in är endast standardinnehåll tillgängligt. När samtycke har mottagits anropas Target och personaliserat innehåll är tillgängligt för den registrerade (besökaren). Eftersom endast standardinnehåll är tillgängligt före samtycke är det viktigt att använda en lämplig strategi, till exempel en välkomstsida som täcker alla delar av sidan eller innehåll som kan personaliseras. Denna process säkerställer att upplevelsen är enhetlig för den registrerade (besökaren).
1. **Måltaggen är INTE förgodkänd och `bodyHidingEnabled` är TRUE:** Måltaggen utlöses endast efter att kunden har gett sitt samtycke. Innan samtycke samlas in är endast standardinnehåll tillgängligt. Men eftersom `bodyHidingEnabled` är inställt på true anger `bodyHiddenStyle` vilket innehåll på sidan som är dolt tills Target-taggen aktiveras (eller så avvisar den registrerade deltagande, vilket innebär att standardinnehåll visas). Som standard är `bodyHiddenStyle` inställd på `body { opacity:0;}`, vilket döljer HTML body-taggen. Adobe rekommenderade sidkonfiguration visas nedan så att hela sidans innehåll, förutom dialogrutan för hantering av samtycke, döljs genom att sidans innehåll placeras i en behållare och dialogrutan för hantering av samtycke i en separat behållare. Den här inställningen konfigurerar Target så att endast sidinnehållsbehållaren döljs. Se [Privacy Service - översikt](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=sv-SE&).

   Rekommenderad sidinställning för scenario 3 är:

   ```
   <html> 
   <head> 
   //visitor, at.js 
   </head> 
   
   <body> 
   <div id = "consentManagerDialog"> 
   
   //consent manager html dialog goes here 
   </div> 
   
   <div id="pageContent"> 
   // page content goes here 
   </div> 
   
   </body> 
   </html> 
   ```

   Antar `bodyHiddenStyle` av:

   ```
   #pageContent { opacity:0;}
   ```

## Frågor och svar om sekretess och dataskydd

Vanliga frågor och svar om EU:s allmänna dataskyddsförordning (GDPR), Kaliforniens konsumentintegritetslag (CCPA) och andra internationella integritetskrav som är specifika för Target.

### Vad har Adobe för policy för dessa bestämmelser?

Adobe uppfyller eller genomför redan sina skyldigheter som databehandlare. Adobe har en stark grund för certifierade säkerhets- och integritetskontroller genom design och har gjort produktförbättringar före tidsfristen i maj 2018. Företagskunder ansvarar för att implementera dessa förbättringar och uppdatera nödvändiga policyer och procedurer.

### Måste mitt företag, Data Controller, skicka en GDPR- eller CCPA-begäran till varje Adobe Experience Cloud-lösning som det använder?

Nej, Adobe tillhandahåller ett centralt sätt att hjälpa datakontroller att uppfylla sina GDPR- och CCPA-krav. Datakontroller behöver inte gå direkt till varje lösning.

Alla GDPR- och CCPA-begäranden i alla Experience Cloud-lösningar, inklusive Target, sker via ett centralt Adobe-API, som för närvarande kallas GDPR API. API:t slutför sedan begäran i Experience Cloud-lösningspaketet för Data Controller.

### Vilken information gör Adobe det möjligt för kunderna att radera som svar på en begäran från en registrerad användare/användare?

Informationen om en enskild besökare i Target finns i målbesökarprofilen. Med Target kan kunderna ta bort alla data som är kopplade till ett ID i sin besökarprofil. Exempel på målarkiv för profildata finns i [Besökarprofil](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=sv-SE).

Sammanställda eller anonyma data (t.ex. rapportdata) som inte identifierar en viss individ, eller data som inte är kopplade till en viss individ (t.ex. innehållsdata), omfattas inte av en begäran om att ta bort användare.

Målbesöksprofiler som har varit inaktiva i 90 dagar tas bort som standard, utan någon åtgärd.

### Vilka ID:n stöds för att hjälpa kunderna att slutföra en GDPR- eller CCPA-begäran om åtkomst och borttagning för Target?

Målet stöder följande ID-typer för att hitta en kundprofil:

| Användar-ID | Typ av namnutrymmes-ID | Namnområdes-ID | Definition |
|--- |--- |--- |--- |
| Experience Cloud ID (ECID) | Standard | 4 | Adobe Experience Cloud-id, f.d. besökar-ID eller Experience Cloud-id. Du kan använda JavaScript API för att hitta detta ID (se informationen nedan). |
| TnT ID/Cookie ID(TNTID) | Standard | 9 | Målidentifieraren anges som en cookie i besökarens webbläsare. Du kan använda JavaScript API för att hitta detta ID (se informationen nedan). |
| Tredjeparts-ID/CRM-ID (TREDJEPARTYID) | Målspecifik | Ej tillämpligt | Om du tillhandahåller Target tillsammans med din CRM eller annan unik identifieringsinformation för dina kunder. |

>[!NOTE]
>
>Även om Target har stöd för både cookies från första part och tredjeparts korsdomäner, rekommenderas endast cookies från första part för GDPR och CCPA.

### Hur hanterar Target samtyckeshantering?

GDPR och CCPA ändras inte när du måste få samtycke, utan hur du får det. Varje kunds strategi för samtycke är beroende av dess rutiner för datainsamling och -användning samt dess integritetspolicy. Samtalshantering stöds inte av och bör inte uppnås via Target för GDPR och CCPA.

Adobe erbjuder för närvarande inte någon lösning för hantering av samtycke, men det finns olika verktyg på marknaden som kan hantera vissa av de nya kraven. Mer information om sekretessverktyg i allmänhet, inklusive samtyckeshanterare, finns i [2017 Privacy Tech Vendor Report](https://iapp.org/media/pdf/resource_center/Tech-Vendor-Directory-1.4.1-electronic.pdf) på webbplatsen *International Association of Privacy Professionals (iaap)* .

Target ger support för tillvalsfunktionalitet via Adobe Experience Platform för att stödja er strategi för samtyckeshantering. Med avanmälningsfunktionen kan kunderna styra hur och när Target-taggen aktiveras. Det finns också ett alternativ via Adobe Experience Platform för att förgodkänna Target-taggen. Vi rekommenderar att du använder Adobe Experience Platform för att hantera anmälan. Det finns ytterligare detaljkontroll i Adobe Experience Platform för att dölja vissa delar av sidan före Target-lanseringen som kan vara användbara som en del av er strategi för samtycke.

Mer information om GDPR, CCPA och Adobe Experience Platform finns i [Adobe Privacy JavaScript Library och GDPR](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=sv-SE&). Se även avsnittet *Adobe Target och Adobe Experience Platform opt-in* ovan.

### Skickar `AdobePrivacy.js` information till GDPR API:t?

AdobePrivacy.js skickar *inte* den här informationen till API:t. Kunden måste göra det. Det här biblioteket innehåller bara de ID:n som lagras i webbläsaren för den specifika besökaren.

### Vad tar `removeIdentities` bort?

`removeIdentities` *only* tar bort dessa identiteter från webbläsaren, och det beror bara på om Adobe-lösningen har implementerat den.

Target tar till exempel bort cookies som lagrar dess ID, men Adobe Audience Manager (AAM) tar inte bort det demdex-ID som lagras i en cookie från tredje part.

### Vilken information måste ingå i en Target GDPR- eller CCPA-begäran?

Utöver kraven från Central Privacy Service innehåller ett giltigt GDPR- eller CCPA-meddelande för Target följande:

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            "namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            "namespace":"TNTID", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"THIRDPARTYID", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
}
```

### Vilka typer av svar kan jag förvänta mig från Target via GDPR API?

| Status för begäran | Målsvarsmeddelande | Scenario |
|--- |--- |--- |
| Bearbetar | Bearbetar | Målet tog emot GDPR- eller CCPA-begäran och bearbetar den. |
| Complete | Ej tillämpligt - företagskontexten är inte tillämplig | IMS-ID:t i GDPR- eller CCPA-begäran mappas inte till någon målklient.<br />Vissa företag har flera IMS-ID:n. Skicka det IMS-ID där Target har etablerats. |
| Complete | Ej tillämpligt - användarkontexten hittades inte | Det ID som anges i GDPR- eller CCPA-begäran för den specifika besökaren eller den angivna registrerade personen finns inte i målprofilens arkiv.<br />Det här resultatet returneras också om du försöker skicka en ID-typ för namnområde som inte stöds av Target (se ovan för ID som stöds). |
| Fel | Felmeddelande (informationen beror på feltypen) | Ett fel uppstod vid hämtning eller borttagning av den begärda dataobjektprofilen.<br />Fel vid överföring till Azure för åtkomstbegäran. |

### Vilket svar skickar Target till GDPR API för en åtkomstbegäran?

Svar på begäran om åtkomst av data innehåller en sammanfattning av målprofilen för besökaren i fråga. Denna retur skickas till Experience Cloud GDPR API, som i sin tur skickar ett svar till Data Controllers.

Ett exempel på Target-åtkomst-API-svar kan se ut så här:

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            ~"namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            ~"namespace":"tntId", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"thirdPartyId", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
} 
```

| Fält | Beskrivning |
|--- |--- |
| jobId | Anger GDPR- eller CCPA-jobb-ID från det centrala GDPR-API:t. |
| imsOrgID | Ger ditt företag en unik identifierare. |
| namespace | Kallas även datakälla. Se&quot;Vilka ID:n stöds för att hjälpa kunderna att slutföra en GDPR- eller CCPA-begäran om åtkomst och borttagning för Target?&quot; i det här avsnittet. |
| type | Den typ av ID som du begärde GDPR- eller CCPA-dataåtkomst för. Target accepterar flera ID-typer, varav vissa är standard och vissa är specifika för Target. Se&quot;Vilka ID:n stöds för att hjälpa kunderna att slutföra en GDPR- eller CCPA-begäran om åtkomst och borttagning för Target?&quot; i det här avsnittet. |
| value | ID för namnområdet/datakällan. Se&quot;Vilka ID:n stöds för att hjälpa kunderna att slutföra en GDPR- eller CCPA-begäran om åtkomst och borttagning för Target?&quot; för godkända värden. |
| integrationskod | Integrationskoder är egna namn för datakällorna och hjälper dig att spåra datakällorna enklare än att använda ID:n för datakällor. |

När flera värden anges för att identifiera profiler har varje giltig identifierare en profilfil. En eller flera profilfiler skickas till den centrala GDPR Azure-blobben via det centrala GDPR-API:t, i formatet JSON-svar för målprofil.

Ett exempel på en målprofil-JSON kan se ut som i följande exempel:

```
{"profileAttributes": 
 
"Sample_Parameter":{"value":"Gold Loyalty Status","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"user.ReturnTimeOfDay":{"value":"44.0","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"firstSessionStart":{"value":"1523497450602","modifiedAt":"2018-04-11T21:44:10.000-04:00"}, 
 
"user.sessionCountScript":{"value":"1","modifiedAt":"2018-04-11T21:44:14.000-04:00"} 
   } 
} 
```

Följande tabell innehåller en beskrivning av JSON-fälten för den illustrativa profilen:

| Fält | Beskrivning |
|--- |--- |
| sample_parameter | Många informationsdelar i målprofilen överförs eller tillhandahålls direkt av Data Controller. I det här exemplet överfördes en parameter till målprofilen med hjälp av API:t för profiluppdatering. Mer information finns i [Metoder för att hämta data till målet](/help/dev/before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md). |
| user.ReturnTimeOfDay | I det här standardfältet anges tiden på dagen för en användares senaste besök. |
| firstSessionStart | Detta standardfält innehåller den tid på dagen då användarens första session påbörjades. |
| user.sessionCountScript | Många informationsdelar i målprofilen överförs eller tillhandahålls direkt av Data Controller. I det här exemplet ökar ett profilskript antalet sessioner som den här besökaren har gjort på Data Controller-platsen. Mer information finns i [Profilskriptattribut](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=sv-SE). |

>[!NOTE]
>
>Det här kodexemplet är en förkortad version av en JSON för målprofil för illustration. Många av fälten i målprofilen är inte standard. Vad som returneras beror på vilken information som finns i den specifika besökarprofilen.

### Har Target stöd för IP-förfalskning?

Target har stöd för IP-obehörig åtkomst om du väljer att använda det som en del av GDPR- eller CCPA-implementeringsstrategin. Mer information finns i [Sekretess](privacy.md#replacement-of-last-octet-of-ip-addresses).

### Bör jag göra något för att förhindra att mina data delas eller säljs till tredje part?

Target tillåter inte kunder att dela eller sälja data direkt från Target till tredje part, så det finns ingen avanmälan om försäljning för Target.
