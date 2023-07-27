---
keywords: anpassade händelser, at.js, begäran misslyckades, begäran slutfördes, innehållsåtergivning misslyckades, innehållsåtergivning slutfördes, biblioteket lästes in, begäranstart, innehållsåtergivning, inga erbjudanden, omdirigering av innehållsåtergivning, anpassade händelser2
description: Använd anpassade händelser för [!DNL Adobe Target] at.js JavaScript-bibliotek som ska meddelas när en mbox-begäran eller ett erbjudande misslyckas eller lyckas.
title: Hur använder jag anpassade at.js-händelser?
feature: at.js
exl-id: a4baed9a-9eb8-4343-9834-709b03e44ca2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 0%

---

# at.js, anpassade händelser

Information om `at.js custom events`som talar om för dig när en mbox-begäran eller ett erbjudande misslyckas eller lyckas.

Tidigare tillät inte mbox.js (nu föråldrat) annan JavaScript-kod som körs på sidan vad som händer bakom scenerna. I och med utvecklingen av at.js hade vi en unik möjlighet att åtgärda det här problemet.

Enligt våra kunder finns det flera scenarier som de vill bli informerade om, bland annat:

* En mbox-begäran misslyckades på grund av timeout, felaktig statuskod, JSON-tolkningsfel osv.
* En mbox-begäran lyckades.
* Det gick inte att återge erbjudandet på grund av att elementet i en omslutningsruta saknas, det går inte att hitta väljaren osv.
* Återgivningen av erbjudandet lyckades. DOM-ändringar har tillämpats.

Fördefinierade händelser har en struktur som gör att du kan extrahera nödvändiga data baserat på händelsetyp.

För att vara säker på att händelser kan användas i olika scenarier har de anpassade händelserna ett nyttolastobjekt som tilldelas till egenskapen detail för händelseobjektet (som skickas till hanteraren). För att undvika att skicka strängar som händelsenamn visas händelserna som konstanter med `adobe.target.event` namnutrymme.

## Struktur

| Nyckel | Typ | Beskrivning |
|--- |--- |--- |
| type | Sträng | Det finns flera scenarier där du vill bli informerad om hjälp med att spåra, felsöka och anpassa interaktion med at.js.<p>Varje anpassad händelse i listan nedan har två format: en &quot;konstant&quot; och ett &quot;strängvärde&quot;.<ul><li>**Konstanter**: Förväntades med `adobe.target.event.`, visas med versaler och innehåller understreck. Så här prenumererar du på anpassade händelser *efter* at.js laddas men *före* Använd konstanten när mbox-svaret har tagits emot.</li><li>**Strängvärden**: Gemener och innehåller streck. Så här prenumererar du på anpassade händelser *före* at.js läses in, använd strängvärdet.</li></ul>**Begäran misslyckades**<p>Konstant: `adobe.target.event.REQUEST_FAILED`<p>Strängvärde: `at-request-failed`<p>Beskrivning: En mbox-begäran misslyckades på grund av timeout, felaktig statuskod, JSON-tolkningsfel osv.<p>**Begäran lyckades**<p>Konstant: `adobe.target.event.REQUEST_SUCCEEDED`<p>Strängvärde: `at-request-succeeded`<p>Beskrivning: En mbox-begäran lyckades.<p>**Innehållsåtergivning misslyckades**<p>Konstant: `adobe.target.event.CONTENT_RENDERING_FAILED`<p>Strängvärde: `at-content-rendering-failed`<p>Beskrivning: Det gick inte att återge erbjudandet på grund av att elementet i omslutningsrutan saknas, det går inte att hitta väljaren osv.<p>**Innehållsåtergivning lyckades**<p>Konstant: `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`<p>Strängvärde: `at-content-rendering-succeeded`<p>Beskrivning: Återgivningen av erbjudandet lyckades. DOM-ändringar har tillämpats.<p>**Bibliotek inläst**<p>Konstant: `adobe.target.event.LIBRARY_LOADED`<p>Strängvärde: `at-library-loaded`<p>Beskrivning: Den här händelsen är idealisk för att spåra när at.js har lästs in helt. Du kan använda den här händelsen för att anpassa den globala mbox-körningen. Du kan också använda den här händelsen för att inaktivera den globala mbox-filen och sedan lyssna efter den här händelsen för att utlösa den globala mbox-filen senare.<p>**Begär start**<p>Konstant: `adobe.target.event.REQUEST_START`<p>Strängvärde: `at-request-start`<p>Beskrivning: Den här händelsen utlöses innan en HTTP-begäran körs. Du kan använda den här händelsen för prestandamätningar med hjälp av API:t för resurstimer.<p>**Start för innehållsåtergivning**<p>Konstant: `adobe.target.event.CONTENT_RENDERING_START`<p>Strängvärde: `at-content-rendering-start`<p>Beskrivning: Den här händelsen utlöses innan väljaravsökningen startas och innehållet återges på sidan. Du kan använda den här händelsen för att spåra återgivningsförloppet för innehåll.<p>**Innehållsåtergivning - inga erbjudanden**<p>Konstant: `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`<p>Strängvärde: `at-content-rendering-no-offers`<p>Beskrivning: Den här händelsen utlöses när inga erbjudanden returneras.<p>**Omdirigering av innehållsåtergivning**<p>Konstant: `adobe.target.event.CONTENT_RENDERING_REDIRECT`<p>Strängvärde: `at-content-rendering-redirect`<p>Beskrivning: Den här händelsen utlöses när ett erbjudande är en omdirigering och [!DNL Target] kommer att omdirigeras till en annan URL. |
| mbox | Sträng | mbox name |
| message | Sträng | Innehåller en beskrivning som kan läsas av människor, t.ex. vad som hände, felmeddelandet. |
| spårning | Objekt | Innehåller `sessionId` och `deviceId`. I vissa fall `deviceId` kan saknas eftersom [!DNL Target] det gick inte att hämta den från edge-servern. |
| type | Sträng | **Beslutsartifakt på enheten har slutförts**<p>Konstant:<p>`adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`<p>Strängvärde: `artifactDownloadSucceeded`<p>Beskrivning: Anropas när enhetens beslutsartefakt har hämtats.<p>**Avvikelsefakt på enheten misslyckades**<p>Konstant: `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`<p>Strängvärde: `artifactDownloadFailed`<p>Beskrivning: Anropas när det inte gick att hämta beslutsartefakten på enheten. |

## Användning

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(event) { 
  console.log('Event', event); 
});
```

## Utbildningsvideo: Response Tokens och at.js Custom Events ![Självstudiemärke](../../../assets/tutorial.png)

Titta på följande video för att lära dig hur du använder responstoken och anpassade at.js-händelser för att dela profilinformation från [!DNL Target] till system från tredje part.

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)
