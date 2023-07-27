---
source-git-commit: 5c66ee5c8dd5fe60eaeed10fdb9bb6dcee000c89
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---
# När ska enhetsspecifik eller kantavkänning användas

## Fundera på användningsfall när du beslutar om du ska använda enhetsbeslut

![alt-bild](assets/comparison.jpeg)

Huvudskillnaden mellan *på enheten* beslutsfattande och edge-beslut är att beslut på enheter verkställs lokalt på dina servrar, medan beslut om edge fattas i Adobe Target Edge-nätverk. Enhetsspecifika beslut bör användas för alla A/B- och XT-aktiviteter som behöver levereras på sidor med omfattande trafik, där prestanda påverkar nyckeltal som konvertering, intäkter och lojalitet i verksamheten avsevärt. Anta till exempel att ert marknadsföringsteam kör annonskampanjer för att locka potentiella kunder till er hemsida. Det krävs betalning för att köra annonskampanjer i utgivarnätverk, och alla potentiella kunder som finns på din hemsida blir därför en krona. Anta samtidigt att du kör A/B-experiment för att se vilken hjältebild som fångar upp kundens uppmärksamhet på bästa sätt. Om det tar ytterligare 2 sekunder att leverera A/B-experimenten är det stor sannolikhet att konsumenten blir otålig och studsar. Nu får ni marknadsföringsbudgeten och A/B-experiment! Att förlora denna intjänade potentiella kund är svårt, eftersom alla möjligheter att konvertera den här möjligheten till en lojal eller upprepad kund nu har gått förlorade. Om du kör en beslutsaktivitet på enheten för det här användningsfallet kan du undvika negativa effekter som fördröjning kan ge upphov till.

Å andra sidan kräver edge-beslut ett nätverks-blockerande anrop för att hämta en upplevelse, men det kan vara mycket fördelaktigt eftersom realtidsdata och ML kan användas för att göra slutanvändarens upplevelse mycket engagerande. Ett anrop om blockering av nätverk ger ytterligare fördröjning när upplevelsen levereras, men i vissa fall kan en sådan avaktivering vara vettig. Tänk dig till exempel ett scenario där en kund bläddrar genom din produktkatalog och antar att de navigerar till en produktinformationssida. Om den sidan innehåller en rekommenderad lista över produkter, tillsammans med den produkt kunden för närvarande besöker, kan detta öka engagemanget - och senare konverteringar och intäkter. Samtidigt som den rekommenderade listan med produkter visas på det här sättet skulle det krävas ett edge-beslut som påverkas av Adobe Target ML-algoritm, vilket innebär att det skulle finnas ytterligare latens, att den tillagda fördröjningen inte skulle vara tillräckligt stor för att slutanvändaren ska kunna studsa. Dessutom innebär en rekommenderad lista över produkter en högre konverteringsgrad. I det här fallet är det därför ett bra beslut som ger ditt företag det mest värde.

## Funktioner som stöds

Förutom att utvärdera dina användningsfall och verksamhetsmål kan du granska vilka funktioner som finns i enhetsbeslut [supports](../on-device-decisioning/supported-features.md) innan du bestämmer dig för om enhetsbeslut ska användas eller inte. För närvarande har edge-beslutsfunktioner stöd för alla aktivitetstyper, målgruppsanpassning och allokeringsmetoder.