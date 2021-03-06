---
title: Aktivera Azure Monitor för en hybrid miljö
description: I den här artikeln beskrivs hur du aktiverar Azure Monitor for VMs för en hybrid moln miljö som innehåller en eller flera virtuella datorer.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: d994df4d56f4958784256ff9cd92ce1e6f3b3e50
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88642171"
---
# <a name="enable-azure-monitor-for-vms-for-a-hybrid-virtual-machine"></a>Aktivera Azure Monitor for VMs för en hybrid virtuell dator
I den här artikeln beskrivs hur du aktiverar Azure Monitor for VMs för en virtuell dator utanför Azure, inklusive lokala miljöer och andra moln miljöer.

> [!IMPORTANT]
> Den rekommenderade metoden för att aktivera hybrid virtuella datorer är att först aktivera [Azure-båge för servrar](../../azure-arc/servers/overview.md) så att de virtuella datorerna kan aktive ras för Azure Monitor for VMS att använda processer som liknar virtuella Azure-datorer. I den här artikeln beskrivs hur du kan publicera virtuella datorer i hybrider om du väljer att inte använda Azure Arc.

## <a name="prerequisites"></a>Förutsättningar

- [Skapa och konfigurera en Log Analytics-arbetsyta](vminsights-configure-workspace.md).
- Se [operativ system som stöds](vminsights-enable-overview.md#supported-operating-systems) för att säkerställa att operativ systemet för den virtuella datorn eller skalnings uppsättningen för virtuella datorer som du aktiverar stöds. 


## <a name="overview"></a>Översikt
Virtuella datorer utanför Azure kräver samma Log Analytics agent och beroende agent som används för virtuella Azure-datorer. Eftersom du inte kan använda VM-tillägg för att installera agenterna, måste du installera dem manuellt i gäst operativ systemet eller låta dem installeras via någon annan metod. 

Mer information om hur du distribuerar Log Analytics-agenten finns i [ansluta Windows-datorer till Azure Monitor](../platform/agent-windows.md) eller [ansluta Linux-datorer till Azure Monitor](../platform/agent-linux.md) . Information om beroende agenten finns i den här artikeln. 

## <a name="firewall-requirements"></a>Brandväggsförutsättningar
Brand Väggs krav för Log Analytics agenten finns i [Översikt över Log Analytics-agenten](../platform/log-analytics-agent.md#network-requirements). Azure Monitor for VMs kartans beroende agent skickar inte några data och kräver inte några ändringar i brand väggar eller portar. Kartdata överförs alltid av Log Analytics agenten till tjänsten Azure Monitor, antingen direkt eller via [Operations Management Suite-gatewayen](../../azure-monitor/platform/gateway.md) om dina IT-säkerhetsprinciper inte tillåter att datorer i nätverket kan ansluta till Internet.


## <a name="dependency-agent"></a>Beroendeagent

>[!NOTE]
>Följande information som beskrivs i det här avsnittet gäller även för den [tjänstkarta lösningen](service-map.md).  

Du kan ladda ned beroende agenten från följande platser:

| Fil | Operativsystem | Version | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.10.5.10940 | C27A56D0BE9CF162DF73292DFBB2083F5FF749F2B80FCAD2545BC8B14B64A8D7  |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.10.5.10940 | 71B4E1DA5116E61E03317C49C6702B5069F01A0C9A7CB860F6ACFAF5C198740E |


## <a name="install-the-dependency-agent-on-windows"></a>Installera beroende agenten i Windows

Du kan installera beroende agenten manuellt på Windows-datorer genom att köra `InstallDependencyAgent-Windows.exe` . Om du kör den här körbara filen utan några alternativ startas en installations guide som du kan följa för att installera agenten interaktivt. Du måste ha *Administratörs* behörighet på gäst operativ systemet för att installera eller avinstallera agenten.

I följande tabell beskrivs de parametrar som stöds av installations programmet för agenten från kommando raden.

| Parameter | Beskrivning |
|:--|:--|
| /? | Returnerar en lista med kommando rads alternativen. |
| / S | Utför en tyst installation utan användar interaktion. |

Om du till exempel vill köra installations programmet med `/?` parametern anger du **InstallDependencyAgent-Windows.exe/?**.

Filer för Windows beroende Agent installeras i *C:\Program Files\Microsoft Dependency agent* som standard. Om beroende agenten inte startar efter att installationen har slutförts, kontrollerar du i loggarna om det finns detaljerad fel information. Logg katalogen är en *%program%\Microsoft-Agent\logs*.

### <a name="powershell-script"></a>PowerShell-skript
Använd följande exempel PowerShell-skript för att ladda ned och installera agenten:

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```


## <a name="install-the-dependency-agent-on-linux"></a>Installera beroende agenten på Linux

Beroende agenten är installerad på Linux-servrar från *InstallDependencyAgent-Linux64. bin*, ett gränssnitts skript med en självextraherande binärfil. Du kan köra filen genom att använda `sh` eller lägga till kör-behörigheter till själva filen.

>[!NOTE]
> Du måste ha rotbehörighet för att kunna installera eller konfigurera agenten.
>

| Parameter | Beskrivning |
|:--|:--|
| – hjälp | Hämta en lista med kommandoradsalternativ. |
| -s | Utför en tyst installation utan någon användarprompter. |
| --kontrol lera | Kontrol lera behörigheter och operativ systemet, men installera inte agenten. |

Om du till exempel vill köra installations programmet med `-help` -parametern anger du **InstallDependencyAgent-Linux64. bin – hjälp**. Installera Linux-beroende agenten som rot genom att köra kommandot `sh InstallDependencyAgent-Linux64.bin` .

Om det inte går att starta beroende agenten kontrollerar du om det finns detaljerad fel information i loggarna. I Linux-agenter är logg katalogen */var/opt/Microsoft/Dependency-Agent/log*.

Filer för beroende agenten placeras i följande kataloger:

| Files | Location |
|:--|:--|
| Kärnfiler | /opt/microsoft/dependency-agent |
| Loggfiler | /var/opt/microsoft/dependency-agent/log |
| Konfigurationsfiler | /etc/opt/microsoft/dependency-agent/config |
| Körbara tjänstfiler | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binära lagringsfiler | /var/opt/microsoft/dependency-agent/storage |

### <a name="shell-script"></a>Gränssnitts skript 
Använd följande exempel på ett shell-skript för att ladda ned och installera agenten:

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Önskad tillståndskonfiguration

Om du vill distribuera beroende agenten med önskad tillstånds konfiguration (DSC) kan du använda xPSDesiredStateConfiguration-modulen med följande exempel kod:

```powershell
configuration VMInsights {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```



## <a name="troubleshooting"></a>Felsökning

### <a name="vm-doesnt-appear-on-the-map"></a>Den virtuella datorn finns inte på kartan

Om beroende Agent installationen lyckades, men du inte ser datorn på kartan, kan du diagnostisera problemet genom att följa dessa steg.

1. Har Dependency Agent installerats på rätt sätt? Du kan validera detta genom att kontrollera om tjänsten är installerad och körs.

    **Windows**: Sök efter tjänsten med namnet "Microsoft Dependency agent".

    **Linux**: Sök efter den process som körs "Microsoft-Dependency-agent".

2. Är du på den [kostnads fria pris nivån av Log Analytics](./solutions.md)? Den kostnads fria planen tillåter upp till fem unika datorer. Eventuella efterföljande datorer visas inte på kartan, även om de fem föregående fem inte längre skickar data.

3. Skickar datorn logg-och perf-data till Azure Monitor loggar? Utför följande fråga för datorn:

    ```Kusto
    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType
    ```

    Returnerade du ett eller flera resultat? Är data aktuella? I så fall fungerar din Log Analytics-agenten som den ska och kommunicerar med tjänsten. Om inte, kontrol lera agenten på servern: [Log Analytics agent för Windows fel sökning](../platform/agent-windows-troubleshoot.md) eller [Log Analytics agent för Linux-felsökning](../platform/agent-linux-troubleshoot.md).

#### <a name="computer-appears-on-the-map-but-has-no-processes"></a>Datorn visas på kartan men saknar processer

Om du ser din server på kartan, men inte har någon process-eller anslutnings data, indikerar att beroende agenten är installerad och körs, men kernel-drivrutinen har inte lästs in.

Kontrollera filen C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log (Windows) eller filen /var/opt/microsoft/dependency-agent/log/service.log (Linux). De sista raderna i filen anger varför kerneln inte har lästs in. Till exempel kanske din kernel inte stöds i Linux om du har uppdaterat den.


## <a name="next-steps"></a>Nästa steg

Nu när övervakning är aktiverat för dina virtuella datorer är den här informationen tillgänglig för analys med Azure Monitor for VMs.

- Information om hur du visar identifierade program beroenden finns i [visa Azure Monitor for VMS karta](vminsights-maps.md).

- Information om hur du identifierar Flask halsar och övergripande användning med den virtuella datorns prestanda finns i [Visa prestanda för virtuella Azure-datorer](vminsights-performance.md).