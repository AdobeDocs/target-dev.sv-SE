---
keywords: implementera, implementera, konfigurera, konfigurera, sidparametrar
description: Hämta data till [!DNL Target] med sidparametrar.
title: Hur får jag in data på [!DNL Target] Använda sidparametrar?
feature: Implementation
exl-id: 9bb7157e-a938-4150-8a15-c9bf0a0e2296
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 0%

---

# Sidparametrar

Sidparametrar (kallas även&quot;mbox parameters&quot;) är namn/värde-par som skickas direkt via sidkod och som inte lagras i besökarens profil för framtida bruk.

Sidparametrar är användbara när du vill skicka siddata till [!DNL Adobe Target] som inte behöver lagras med besökarens profil för framtida målinriktning. Dessa värden används i stället för att beskriva sidan eller den åtgärd som användaren utförde på den specifika sidan.

## Format

Sidparametrar skickas till [!DNL Target] via ett serveranrop som ett strängnamn/värde-par. Parameternamn och värden kan anpassas (även om det finns &quot;reserverade namn&quot; för vissa användningsområden).

Här är några exempel på sidparametrar

* `page=productPage`

* `categoryId=homeLoans`

## Exempel på användningsområden

* **Produktsidor**: Skicka information om vilken produkt som visas (den här metoden är hur Recommendations fungerar)
* **Beställningsinformation**: Skicka order-ID, orderTotal o.s.v. för ordersamling
* **Kategoritillhörighet**: Skicka kategorivisad information till [!DNL Target] för att skapa kunskap om användarens tillhörighet till särskilda webbplatskategorier
* **data från tredje part**: Skicka information från datakällor från tredje part, till exempel leverantörer av väderanpassning, kontodata (till exempel DemandBase), demografiska data (till exempel Experience) och mycket mer.

## Fördelar med metoden

Data skickas till [!DNL Target] i realtid och kan användas på samma server för att anropa de data som de finns på.

## Caveats

* Kräver uppdatering av sidkod (direkt eller via ett tagghanteringssystem).
* Om data måste användas för att rikta in sig på en efterföljande sida/server-anrop måste de översättas till ett profilskript.
* Frågesträngar får endast innehålla tecken enligt [IETF-standard (Internet Engineering Task Force)](https://www.ietf.org/rfc/rfc3986.txt) .

  Förutom de tecken som nämns på IETF:s webbplats, [!DNL Target] tillåter följande tecken i frågesträngar:

  ```< > # % " { } | \ ^ [ ] ` ``` {line-numbers=&quot;true&quot;}

  Allt annat måste vara url-kodat. Standarden anger följande format ( [https://www.ietf.org/rfc/rfc1738.txt](https://www.ietf.org/rfc/rfc1738.txt) ), enligt nedan:

  ![alt-bild](assets/ietf1.png)

  Eller hela listan för enkelhet:

  ![alt-bild](assets/ietf2.png)

## Exempel på koder

targetPageParamsAll (bifogar parametrarna till alla mbox-anrop på sidan):

`function targetPageParamsAll() { return "param1=value1&param2=value2&p3=hello%20world";`

targetPageParams (lägger till parametrarna i den globala mbox på sidan):

`function targetPageParams() { return "param1=value1&param2=value2&p3=hello%20world";`

## Länkar till relevant information

Recommendations: [Implementering enligt sidtyp](https://experienceleague.adobe.com/docs/target/using/recommendations/plan-implement.html)

Orderbekräftelse: [Spåra konverteringar](../../implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)

Kategoritillhörighet: [Kategoritillhörighet](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/category-affinity.html)
