---
title: ta med fil
description: ta med fil
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/16/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e2a1eb8524c9d9a4b0ae603da3c05fbb20900111
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/25/2020
ms.locfileid: "95995510"
---
SMB Multichannel för Azure-filresurser har för närvarande följande begränsningar:
- Kan endast användas med lokalt redundanta FileStorage-konton.
- Stöds endast för Windows-klienter. 
- Maximalt antal kanaler är fyra.
- SMB Direct stöds inte.
- För lagrings konton med lokal Active Directory Domain Services (AD DS) eller Azure AD DS [Identity-baserad autentisering](../articles/storage/files/storage-files-active-directory-overview.md) aktive rad för Azure Files kan SMB-klienter inte använda Windows Utforskaren för att konfigurera NTFS-behörigheter för kataloger och filer.
    - Använd Windows [icacls](/windows-server/administration/windows-commands/icacls) -verktyget eller [set-ACL-](/powershell/module/microsoft.powershell.security/set-acl?view=powershell-7) kommandot i stället för att konfigurera behörigheter.

