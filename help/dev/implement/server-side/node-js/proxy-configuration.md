---
title: Implementera proxykonfiguration i [!DNL Adobe Target] Node.js SDK
description: Lär dig hur du konfigurerar [!UICONTROL TargetClient] proxykonfiguration i [!DNL Adobe Target] Node.js SDK.
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 0%

---

# Proxykonfiguration (Node.js)

Om du vill konfigurera en proxy för Node SDK:s HTTP-begäranden åsidosätter du det hämtnings-API som används av SDK under initieringen.

Följande är ett grundläggande exempel som visar hur du åsidosätter `fetchApi` under `TargetClient` initiering för att lägga till en proxy:

```javascript {line-numbers="true"}
const { ProxyAgent } = require("undici");

const proxyAgent = new ProxyAgent("your proxy address here");

const fetchImpl = (url, options) => {
  const fetchOptions = options;
  fetchOptions.dispatcher = proxyAgent;
  return fetch(url, fetchOptions);
};

client = TargetClient.create({
    ...,
    fetchApi: fetchImpl
});
```

Observera att detta endast fungerar för nodversionerna 18.2+, där `undici.fetch` är standard `fetch` för nod.
Besök [Nod-SDK-exempel - repo](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
för exempel på proxykonfiguration för äldre versioner av noder och mer information.
