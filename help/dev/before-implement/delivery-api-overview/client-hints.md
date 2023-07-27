---
title: Klienttips för Adobe Target Delivery API
description: Hur använder jag klienttips i [!DNL Adobe Target] Leverans-API?
exl-id: 317b9d7d-5b98-464e-9113-08b899ee1455
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# Klienttips och [!UICONTROL Adobe Target Delivery API]

Klienttips måste skickas till [!DNL Adobe Target] på förfrågan.

I allmänhet rekommenderas du att skicka alla tillgängliga klienttips till [!DNL Target]. Mer information finns i [Tips för användaragent och klient](/help/dev/implement/client-side/atjs/user-agent-and-client-hints.md) i [Implementering på klientsidan](../../implement/client-side/overview.md) -avsnitt.

## Direktanrop till leverans-API

### Från webbläsaren

I det här fallet skickar webbläsaren lågentropi-klienttips till [!DNL Target] automatiskt via begäranrubriker. Men det finns ett par begränsningar på webbläsarnivå för den här implementeringen. Först skickas inga tips för klienttips från webbläsaren om inte begäran görs via https. Andra - Klienttips skickas inte vid den första begäran till [!DNL Target] på sidan. Klienttipsrubriker skickas endast för den andra begäran och alla begäranden därefter. Detta innebär att målgruppssegmentering och personalisering inte kan utföras av [!DNL Target] på första sidan. För att komma runt båda dessa begränsningar rekommenderar vi att du använder API:t för användaragentens klienttips i webbläsaren för att samla in klienttips direkt och skicka dem på nyttolasten för begäran.

### Från en server

I det här fallet måste klienttipsen vidarebefordras manuellt från webbläsaren till [!DNL Target] på förfrågan om leverans-API.

```
curl -X POST 'http://mboxedge28.tt.omtrdc.net/rest/v1/delivery?client=myClientCode&sessionId=abcdefghijkl00014' -d '{
  "context": {
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36",
    "clientHints": {
      "Sec-CH-UA-Model": "iPhone",
      "Sec-CH-UA-Mobile": true,
      "Sec-CH-UA-Platform": "iOS",
      "Sec-CH-UA": "[ { \"brand\": \"Chromium\", \"version\": \"91\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99\" } ]",
      "Sec-CH-UA-Full-Version-List": "[ { \"brand\": \"Chromium\", \"version\": \"91.1.1.1\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99.1.1.1\" } ]",
      "Sec-CH-UA-Platform-Version": "10.0.0",
      "Sec-CH-UA-Arch": "x86",
      "Sec-CH-UA-Bitness": "64"
    }
  },
  "execute": {
    "mboxes": [{
      "name": "home",
      "index": 1
    }]
  }
}'
```

## Formatering

Klienttipsrubrikerna Sec-CH-UA och Sec-CH-UA-Full-Version-List har ett annat format än resultaten från klienttipsens webbläsar-API (navigator.userAgentData.brands/navigator.userAgentData.getHighEntropyValues). Båda dessa format accepteras av leverans-API. Leverans-API:t normaliserar värdena till det format som används i begäranderubrikerna, vilket är viktigt att tänka på vid åtkomst av klienttips i profilskript.
