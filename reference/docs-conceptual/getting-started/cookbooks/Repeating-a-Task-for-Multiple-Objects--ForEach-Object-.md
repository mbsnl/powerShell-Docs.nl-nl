---
ms.date: 06/05/2017
keywords: PowerShell-cmdlet
title: Een taak te herhalen voor meerdere objecten ForEach-Object
ms.assetid: 6697a12d-2470-4ed6-b5bb-c35e5d525eb6
ms.openlocfilehash: 8b8002af3ade0905421760ce29cdc84b084236e9
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="repeating-a-task-for-multiple-objects-foreach-object"></a>Een taak herhalen voor meerdere objecten (ForEach-Object)

De **ForEach-Object** cmdlet maakt gebruik van scriptblokken en de $_ descriptor voor de huidige pipeline-object waarmee u een opdracht uitvoeren op elk object in de pijplijn. Dit kan worden gebruikt voor een aantal complexe taken uitvoeren.

Een situatie waarin dit kan handig zijn met het bewerken van de gegevens zodat nuttiger. De Win32_LogicalDisk-klasse van WMI kan bijvoorbeeld worden gebruikt om informatie van de vrije ruimte voor elke lokale schijf te retourneren. De gegevens worden geretourneerd in termen van bytes, waardoor het moeilijk te lezen:

```
PS> Get-WmiObject -Class Win32_LogicalDisk

DeviceID     : C:
DriveType    : 3
ProviderName :
FreeSpace    : 50665070592
Size         : 203912880128
VolumeName   : Local Disk
```

We kunnen de waarde FreeSpace omzetten in megabytes door elke waarde te delen door 1024 tweemaal te gebruiken. na de eerste deling worden de gegevens zich in kilobytes en na het tweede getal is in megabytes. U kunt dit doen in een scriptblok ForEach-Object te typen:

```
PS> Get-WmiObject -Class Win32_LogicalDisk | ForEach-Object -Process {($_.FreeSpace)/1024.0/1024.0}
48318.01171875
```

De uitvoer is nu gegevens zonder bijbehorende label. Omdat WMI-eigenschappen zoals deze alleen-lezen zijn, kunt u niet rechtstreeks FreeSpace converteren. Als u dit typt:

```powershell
Get-WmiObject -Class Win32_LogicalDisk | ForEach-Object -Process {$_.FreeSpace = ($_.FreeSpace)/1024.0/1024.0}
```

U er een foutbericht weergegeven:

```output
"FreeSpace" is a ReadOnly property.
At line:1 char:70
+ Get-WmiObject -Class Win32_LogicalDisk | ForEach-Object -Process {$_.F <<<< r
eeSpace = ($_.FreeSpace)/1024.0/1024.0}
```

U kunt de gegevens opnieuw indelen met behulp van sommige geavanceerde technieken, maar het eenvoudiger is voor het maken van een nieuw object met behulp van **Select-Object**.