---
author: normesta
ms.service: storage
ms.topic: include
ms.date: 09/28/2020
ms.author: normesta
ms.openlocfilehash: 2474b8920c5387c7896b413f229c2f5b06cdafb1
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011191"
---
| Egenskap | Beskrivning |
|:--- |:---|
|**tid** | UTC-tid (Universal Time Coordinated) när begäran togs emot av lagrings platsen. Till exempel: `2018/11/08 21:09:36.6900118`.|
|**resourceId** | Resurs-ID för lagrings kontot. Exempelvis: `/subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/`<br>`myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/storageAccounts/blobServices/default`|
|**kategori** | Kategorin för den begärda åtgärden. Till exempel: `StorageRead` , `StorageWrite` , eller `StorageDelete` .|
|**operationName** | Den typ av REST-åtgärd som utfördes. <br> En fullständig lista över åtgärder finns i [avsnittet Lagringsanalys loggade åtgärder och status meddelanden](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). |
|**operationVersion** | Den lagrings tjänst version som angavs när begäran gjordes. Detta motsvarar värdet för huvudet **x-MS-version** . Till exempel: `2017-04-17`.|
|**Schema** | Loggens schema version. Till exempel: `1.0`.|
|**statusCode** | HTTP-statuskod för begäran. Om begäran avbryts kan det här värdet vara inställt på `Unknown` . <br> Exempelvis: `206` |
|**Status text** | Status för den begärda åtgärden.  En fullständig lista över status meddelanden finns i [avsnittet Lagringsanalys loggade åtgärder och status meddelanden](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages). I version 2017-04-17 och senare används inte status meddelandet `ClientOtherError` . I stället innehåller det här fältet en felkod. Exempelvis: `SASSuccess`  |
|**. Durationms** | Den totala tiden, uttryckt i millisekunder, för att utföra den begärda åtgärden. Detta omfattar tiden att läsa den inkommande begäran och att skicka svaret till beställaren. Till exempel: `12`.|
|**callerIpAddress** | IP-adressen för beställaren, inklusive Port numret. Till exempel: `192.100.0.102:4362`. |
|**correlationId** | Det ID som används för att korrelera loggar över resurser. Till exempel: `b99ba45e-a01e-0042-4ea6-772bbb000000`. |
|**sökvägen** | Lagrings kontots plats. Till exempel: `North Europe`. |
|**protokollhanterare**|Det protokoll som används i åtgärden. Till exempel: `HTTP` , `HTTPS` , `SMB` eller `NFS`|
| **URI** | Enhetligt resurs-ID som begärs. |