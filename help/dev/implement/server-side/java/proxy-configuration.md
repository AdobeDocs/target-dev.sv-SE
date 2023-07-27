---
title: Implementera proxykonfiguration i [!DNL Adobe Target] Java SDK
description: Lär dig hur du konfigurerar TargetClient-proxykonfigurationen i [!DNL Adobe Target] Java SDK.
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '88'
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
