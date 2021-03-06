---
title: inkludera fil
description: inkludera fil
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 9/1/2020
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: d0754ea2d7e8f8f59ec475be8e27fcffd058c11f
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/05/2020
ms.locfileid: "91376884"
---
## <a name="prerequisites"></a>Förutsättningar
Innan du börjar ska du se till att:

- Skapa ett Azure-konto med en aktiv prenumeration. Mer information finns i [skapa ett konto kostnads fritt](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Installera [Node.js](https://nodejs.org/en/download/) Active LTS och underhåll LTS-versioner (8.11.1 och 10.14.1 rekommenderas).
- Skapa en Azure Communication Services-resurs. Mer information finns i [skapa en Azure Communication-resurs](../../create-communication-resource.md). Du måste registrera resurs **slut punkten** för den här snabb starten.
- En [åtkomsttoken för användare](../../access-tokens.md). Var noga med att ange omfånget till "chatt" och anteckna token-strängen och userId-strängen.

## <a name="setting-up"></a>Konfigurera

### <a name="create-a-new-web-application"></a>Skapa ett nytt webb program

Först öppnar du terminalen eller kommando fönstret för att skapa en ny katalog för din app och navigerar till den.

```console
mkdir chat-quickstart && cd chat-quickstart
```
   
Kör `npm init -y` för att skapa en **package.jspå** en fil med standardinställningar.

```console
npm init -y
```

Använd en text redigerare för att skapa en fil med namnet **start-chat.js** i projektets rot Katalog. Du kommer att lägga till alla käll koder för den här snabb starten till den här filen i följande avsnitt.

### <a name="install-the-packages"></a>Installera paketen

Använd `npm install` kommandot för att installera klient biblioteken nedan för kommunikations tjänster för Java Script.

```console
npm install @azure/communication-common --save

npm install @azure/communication-administration --save

npm install @azure/communication-signaling --save

npm install @azure/communication-chat --save

```

I det här `--save` alternativet visas biblioteket som ett beroende i **package.jsi** filen.

### <a name="set-up-the-app-framework"></a>Konfigurera app Framework

I den här snabb starten används WebPack för att paketera program till gångarna. Kör följande kommando för att installera WebPack-, WebPack-CLI-och WebPack-dev-Server NPM-paket och lista dem som utvecklings beroenden i din **package.jspå**:

```console
npm install webpack webpack-cli webpack-dev-server --save-dev
```

Skapa en **index.html** -fil i projektets rot Katalog. Vi använder den här filen som en mall för att lägga till chatt-funktioner med hjälp av klient biblioteket för Azure Communication Chat för Java Script.

Här är koden:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Communication Client - Chat Sample</title>
  </head>
  <body>
    <h4>Azure Communication Services</h4>
    <h1>Chat Quickstart</h1>
    <script src="./bundle.js"></script>
  </body>
</html>
```
Skapa en fil i projektets rot Katalog som kallas **client.js** som innehåller program logiken för den här snabb starten. 

### <a name="create-a-chat-client"></a>Skapa en Chat-klient

Om du vill skapa en chatt-klient i din webbapp använder du SIP-slutpunkten för kommunikation och den åtkomsttoken som genererades som en del av de nödvändiga stegen. Med token för användar åtkomst kan du skapa klient program som direkt autentiserar till Azure Communication Services. När du har genererat dessa token på servern skickar du tillbaka dem till en klient enhet. Du måste använda- `AzureCommunicationUserCredential` klassen från `Common client library` för att skicka token till din Chat-klient.

Skapa en **client.js** -fil i projektets rot Katalog. Vi använder den här filen för att lägga till chatt-funktioner med hjälp av klient biblioteket för Azure Communication Chat för Java Script.

```JavaScript

import { ChatClient } from '@azure/communication-chat';
import { AzureCommunicationUserCredential } from '@azure/communication-common';

// Your unique Azure Communication service endpoint
let endpointUrl = 'https://<RESOURCE_NAME>.communication.azure.com';
let userAccessToken = '<USER_ACCESS_TOKEN>';

let chatClient = new ChatClient(endpointUrl, new AzureCommunicationUserCredential(userAccessToken));
console.log('Azure Communication Chat client created!');
```
Ersätt **slut punkten** med den som skapades innan du bygger på dokumentationen om att [skapa en Azure Communication-resurs](../../create-communication-resource.md) .
Ersätt **USER_ACCESS_TOKEN** med en token som utfärdats baserat på dokumentationen för [användar åtkomst-token](../../access-tokens.md) .
Lägg till den här koden i **client.js** -filen


### <a name="run-the-code"></a>Kör koden
Använd `webpack-dev-server` för att skapa och köra din app. Kör följande kommando för att paketera program värden i på en lokal webserver:
```console
npx webpack-dev-server --entry ./client.js --output bundle.js --debug --devtool inline-source-map
```
Öppna webbläsaren och gå till http://localhost:8080/ .
I konsolen för utvecklarverktyg i webbläsaren bör du se följande:

```console
Azure Communication Chat client created!
```

## <a name="object-model"></a>Objekt modell 
Följande klasser och gränssnitt hanterar några av de viktigaste funktionerna i Azure Communication Servicess Chat-klient bibliotek för Java Script.

| Namn                                   | Beskrivning                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ChatClient | Den här klassen krävs för chatt-funktionen. Du instansierar den med din prenumerations information och använder den för att skapa, hämta och ta bort trådar. |
| ChatThreadClient | Den här klassen krävs för chatt-trådens funktion. Du får en instans via ChatClient och använder den för att skicka/ta emot/uppdatera/ta bort meddelanden, lägga till/ta bort/hämta användare, skicka meddelanden och läsa kvitton, prenumerera på chatt-händelser. |


## <a name="start-a-chat-thread"></a>Starta en chatt-tråd

Använd `createThread` metoden för att skapa en chatt-tråd.

`createThreadRequest` används för att beskriva tråd förfrågan:

- Används `topic` för att ge ett ämne till den här chatten. Ämnet kan uppdateras när chatt-tråden har skapats med hjälp av `UpdateThread` funktionen. 
- Används `members` för att visa en lista över medlemmar som ska läggas till i chatten.

När `createChatThread` metoden har lösts returnerar metoden `threadId` som används för att utföra åtgärder på den nya chatt-tråden som att lägga till medlemmar till chatt-tråden, skicka meddelanden, ta bort meddelanden och så vidare.

```Javascript
async function createChatThread() {
   let createThreadRequest = {
       topic: 'Preparation for London conference',
       members: [{
                   user: { communicationUserId: '<USER_ID_FOR_JACK>' },
                   displayName: 'Jack'
               }, {
                   user: { communicationUserId: '<USER_ID_FOR_GEETA>' },
                   displayName: 'Geeta'
               }]
   };
   let chatThreadClient= await chatClient.createChatThread(createThreadRequest);
   let threadId = chatThreadClient.threadId;
   return threadId;
}

createChatThread().then(async threadId => {
   console.log(`Thread created:${threadId}`);
   // PLACEHOLDERS
   // <CREATE CHAT THREAD CLIENT>
   // <RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>
   // <SEND MESSAGE TO A CHAT THREAD>
   // <LIST MESSAGES IN A CHAT THREAD>
   // <ADD NEW MEMBER TO THREAD>
   // <LIST MEMBERS IN A THREAD>
   // <REMOVE MEMBER FROM THREAD>
});
```

Ersätt **USER_ID_FOR_JACK** och **USER_ID_FOR_GEETA** med de användar-ID: n som hämtades från föregående steg (skapa användare och utfärda [token för användar åtkomst](../../access-tokens.md))

När du uppdaterar din webb läsar flik bör du se följande i-konsolen
```console
Thread created: <threadId>
```

## <a name="get-a-chat-thread-client"></a>Hämta en klient för chatt-tråd

`getChatThreadClient`Metoden returnerar en `chatThreadClient` för en tråd som redan finns. Den kan användas för att utföra åtgärder på den skapade tråden: Lägg till medlemmar, skicka meddelande, osv. threadId är det unika ID: t för den befintliga chatt tråden.

```JavaScript

let chatThreadClient = await chatClient.getChatThreadClient(threadId);
console.log(`Chat Thread client for threadId:${chatThreadClient.threadId}`);

```
Lägg till den här koden i stället för `<CREATE CHAT THREAD CLIENT>` kommentaren i **client.js**, uppdatera din webbläsare-flik och kontrol lera konsolen. du bör se:
```console
Chat Thread client for threadId: <threadId>
```

## <a name="send-a-message-to-a-chat-thread"></a>Skicka ett meddelande till en chatt-tråd

Använd `sendMessage` metoden för att skicka ett chatt meddelande till den tråd som du nyss skapade, som identifieras av threadId.

`sendMessageRequest` beskriver de obligatoriska fälten i begäran om chatt-meddelande:

- Används `content` för att tillhandahålla chatt-meddelandets innehåll.

`sendMessageOptions` beskriver valfria fält för begäran om chatt-meddelande:

- Används `priority` för att ange prioritets nivå för chatt meddelande, till exempel "normal" eller "hög"; den här egenskapen kan användas för att Visa användar gränssnitts indikatorn för mottagaren i appen för att uppmärksamma meddelandet eller köra anpassad affärs logik.   
- Används `senderDisplayName` för att ange visnings namnet på avsändaren.

Svaret `sendChatMessageResult` innehåller ett "ID", vilket är det unika ID: t för meddelandet.

```JavaScript

let sendMessageRequest =
{
    content: 'Hello Geeta! Can you share the deck for the conference?'
};
let sendMessageOptions =
{
    priority: 'Normal',
    senderDisplayName : 'Jack'
};
let sendChatMessageResult = await chatThreadClient.sendMessage(sendMessageRequest, sendMessageOptions);
let messageId = sendChatMessageResult.id;
console.log(`Message sent!, message id:${messageId}`);

```
Lägg till den här koden i stället för `<SEND MESSAGE TO A CHAT THREAD>` kommentaren i **client.js**, uppdatera din webbläsare-flik och kontrol lera konsolen.
```console
Message sent!, message id:<number>
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Ta emot Chat-meddelanden från en chatt-tråd

Med real tids signalering kan du prenumerera på att lyssna efter nya inkommande meddelanden och uppdatera aktuella meddelanden i minnet. Azure Communication Services har stöd för en [lista över händelser som du kan prenumerera på](../../../concepts/chat/concepts.md#real-time-signaling).

```JavaScript
// open notifications channel
await chatClient.startRealtimeNotifications();
// subscribe to new notification
chatClient.on("chatMessageReceived", (e) => {
    console.log("Notification chatMessageReceived!");
    // your code here
});

```
Lägg till den här koden i stället för `<RECEIVE A CHAT MESSAGE FROM A CHAT THREAD>` kommentaren i **client.js**.
Uppdatera din webbläsare-flik visas i konsolen ett meddelande `Notification chatMessageReceived` .

Du kan också hämta Chat-meddelanden genom att avsöka `listMessages` metoden vid angivna intervall. 

```JavaScript

let pagedAsyncIterableIterator = await chatThreadClient.listMessages();
let nextMessage = await pagedAsyncIterableIterator.next();
 while (!nextMessage.done) {
     let chatMessage = nextMessage.value;
     console.log(`Message :${chatMessage.content}`);
     // your code here
     nextMessage = await pagedAsyncIterableIterator.next();
 }

```
Lägg till den här koden i stället för `<LIST MESSAGES IN A CHAT THREAD>` kommentaren i **client.js**.
Uppdatera fliken i-konsolen bör du hitta en lista över meddelanden som skickas i den här chatt-tråden.


`listMessages` Returnerar den senaste versionen av meddelandet, inklusive eventuella ändringar eller borttagningar som hände i meddelandet med hjälp av `updateMessage` och `deleteMessage` .
För borttagna meddelanden `chatMessage.deletedOn` returnerar ett datetime-värde som anger när meddelandet togs bort. För redigerade meddelanden `chatMessage.editedOn` returnerar en datetime som anger när meddelandet redigerades. Det går att få åtkomst till den ursprungliga tiden för att skapa meddelanden med `chatMessage.createdOn` som kan användas för att ordna meddelandena.

`listMessages` returnerar olika typer av meddelanden som kan identifieras av `chatMessage.type` . Dessa typer är:

- `Text`: Vanligt chatt-meddelande som skickas av en tråd medlem.

- `ThreadActivity/TopicUpdate`: System meddelande som anger att ämnet har uppdaterats.

- `ThreadActivity/AddMember`: System meddelande som anger att en eller flera medlemmar har lagts till i chatt-tråden.

- `ThreadActivity/RemoveMember`: System meddelande som anger att en medlem har tagits bort från chatt-tråden.

Mer information finns i [meddelande typer](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-member-to-the-chat-thread"></a>Lägg till en användare som medlem i Chat-tråden

När en chatt-tråd har skapats kan du lägga till och ta bort användare från den. Genom att lägga till användare ger du dem åtkomst till att skicka meddelanden till chatt-tråden och lägga till/ta bort andra medlemmar. Innan du anropar `addMembers` -metoden kontrollerar du att du har skaffat en ny åtkomsttoken och identitet för användaren. Användaren måste ha denna åtkomsttoken för att kunna initiera sin Chat-klient.

`addMembersRequest` Beskriver objektet Request där `members` anger medlemmar som ska läggas till i chatten.
- `user`, krävs, är den kommunikations användare som ska läggas till i chatt-tråden.
- `displayName`, valfritt är visnings namnet för tråd medlemmen.
- `shareHistoryTime`, valfritt, är den tid som chatt-historiken delas med medlemmen. Om du vill dela historiken på grund av att chatten är i gång, anger du den här egenskapen till ett datum som är lika med eller mindre än tiden för tråd skapande. Om du inte vill dela någon historik tidigare när medlemmen lades till, ställer du in den på det aktuella datumet. Om du vill dela delar av historiken anger du det datum som du önskar.

```JavaScript

let addMembersRequest =
{
    members: [
        {
            user: { communicationUserId: '<NEW_MEMBER_USER_ID>' },
            displayName: 'Jane'
        }
    ]
};

await chatThreadClient.addMembers(addMembersRequest);

```
Ersätt **NEW_MEMBER_USER_ID** med ett [nytt användar-ID](../../access-tokens.md) Lägg till den här koden i stället för `<ADD NEW MEMBER TO THREAD>` kommentaren i **client.js**

## <a name="list-users-in-a-chat-thread"></a>Lista användare i en chatt-tråd
```JavaScript
async function listThreadMembers() {
   let pagedAsyncIterableIterator = await chatThreadClient.listMembers();
   let next = await pagedAsyncIterableIterator.next();
   while (!next.done) {
      let user = next.value;
      console.log(`User :${user.displayName}`);
      next = await pagedAsyncIterableIterator.next();
   }
}
await listThreadMembers();
```
Lägg till den här koden i stället för `<LIST MEMBERS IN A THREAD>` kommentaren i **client.js**, uppdatera din webbläsare-flik och kontrol lera konsolen. du bör se information om användare i en tråd.

## <a name="remove-user-from-a-chat-thread"></a>Ta bort användare från en chatt-tråd

På samma sätt som du lägger till en medlem kan du ta bort medlemmar från en chatt-tråd. För att kunna ta bort måste du spåra ID: n för de medlemmar som du har lagt till.

Använd `removeMember` metoden där `member` är den kommunikations användare som ska tas bort från tråden.

```JavaScript

await chatThreadClient.removeMember({ communicationUserId: <MEMBER_ID> });
await listThreadMembers();
```
Ersätt **MEMBER_ID** med ett användar-ID som användes i föregående steg (<NEW_MEMBER_USER_ID>).
Lägg till den här koden i stället för `<REMOVE MEMBER FROM THREAD>` kommentaren i **client.js**,
