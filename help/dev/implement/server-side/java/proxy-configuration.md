---
title: Implementera proxykonfiguration i [!DNL Adobe Target] Java SDK
description: Lär dig hur du konfigurerar TargetClient-proxykonfigurationen i [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: 59ab3f53e2efcbb9f7b1b2073060bbd6a173e380
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Proxykonfiguration (Java)

## Grundläggande proxy

Om programmet som kör SDK kräver en proxy för att få åtkomst till Internet är `TargetClient` måste konfigureras med en proxykonfiguration enligt följande.

### Grundläggande proxykonfiguration

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Autentisering

Om en proxyautentisering krävs kan autentiseringsuppgifterna skickas som parametrar till `ClientProxyConfig` konstruktor, enligt exemplet nedan. Observera att detta endast fungerar för autentisering av användarnamn/lösenord.

### Grundläggande proxyautentisering

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port,username,password))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Enhetsbeslut

För förfrågningar om att hämta regelartefakten bör din proxy konfigureras att inte cachelagra svaret. Om det inte går att konfigurera proxyns cachelagringsmekanism för den begäran använder du ett konfigurationsalternativ som en tillfällig lösning för att kringgå cache på proxynivå. Den här lösningen lägger till `Authorization` header med ett tomt strängvärde till regelbegäran, som ska ange för proxyn att svaret inte ska cachelagras.

Ange följande för att du ska kunna aktivera den här lösningen:

```java {line-numbers="true"}
ClientConfig.builder()
    .shouldArtifactRequestBypassProxyCache(true)
    .build();
```


