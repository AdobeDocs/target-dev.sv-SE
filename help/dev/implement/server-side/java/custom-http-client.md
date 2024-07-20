---
title: Lär dig hur du konfigurerar den anpassade HTTP-klienten
description: Lär dig konfigurera TargetClient med ClientConfig.builder().httpClient().
feature: APIs/SDKs
exl-id: 7615029c-b62d-4ed1-aadb-32e364c4c654
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 0%

---

# Anpassad Java (HTTP Client Configuration)

Om programmet som kör SDK kräver en anpassad HTTP-klient måste `TargetClient` konfigureras med `ClientConfig.builder().httpClient()` för att funktioner som att konfigurera SSL eller lägga till standardhuvuden till begäranden ska kunna aktiveras:

## Grundläggande anpassad HTTP-klientkonfiguration

SDK stöder för närvarande HTTP-klienter som implementerar gränssnittet `org.apache.http.client.HttpClient`.

### Grundläggande implementering

```java {line-numbers="true"}
CloseableHttpClient httpClient = HttpClients.custom().build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Anpassad HTTP-klientkonfiguration med SSL-konfiguration

Här är ett exempel på hur du konfigurerar SSL i `TargetClient` genom att anpassa `HttpClient` som skickas till `ClientConfig`. Följande kodfragment använder klasser från paketet `org.apache.http.conn.ssl` för SSL-konfiguration.

### SSL-implementering

```java {line-numbers="true"}
SSLContext context = SSLContextBuilder.create().build();
SSLConnectionSocketFactory sslSocketFactory = new SSLConnectionSocketFactory(context);
CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(sslSocketFactory).build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```
