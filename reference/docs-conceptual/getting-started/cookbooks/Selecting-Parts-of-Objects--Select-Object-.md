---
ms.date: 06/05/2017
keywords: PowerShell-cmdlet
title: Object selecteren selecteren delen van objecten
ms.assetid: 72e64b1a-d351-4500-9da3-24d8a71d7a92
ms.openlocfilehash: 323c57ba4462e20d9713fb74732989584f5a993f
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="selecting-parts-of-objects-select-object"></a>Delen van objecten (Select-Object) selecteren

U kunt de **Select-Object** cmdlet voor het maken van nieuwe, aangepaste Windows PowerShell-objecten die u hebt geselecteerd in de objecten die u gebruikt om ze te maken, eigenschappen bevatten. Typ de volgende opdracht voor het maken van een nieuw object met alleen de naam en FreeSpace eigenschappen van de Win32_LogicalDisk WMI-klasse:

```
PS> Get-WmiObject -Class Win32_LogicalDisk | Select-Object -Property Name,FreeSpace

Name                                    FreeSpace
----                                    ---------
C:                                      50664845312
```

Het type gegevens na het uitgeven van deze opdracht niet wordt weergegeven, maar als u het resultaat dat Get-lid na de Select-Object doorsluizen, kunt u zien dat er een nieuw type object, een PSCustomObject:

```
PS> Get-WmiObject -Class Win32_LogicalDisk | Select-Object -Property Name,FreeSpace| Get-Member

   TypeName: System.Management.Automation.PSCustomObject

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       System.Boolean Equals(Object obj)
GetHashCode Method       System.Int32 GetHashCode()
GetType     Method       System.Type GetType()
ToString    Method       System.String ToString()
FreeSpace   NoteProperty  FreeSpace=...
Name        NoteProperty System.String Name=C:
```

Select-Object heeft veel gebruikt. Een van beide is bezig met het repliceren van gegevens die u kunt wijzigen. We kan het probleem dat er op in de vorige sectie is nu verwerken. We kunnen de waarde van FreeSpace bijwerken in onze zojuist gemaakte objecten en de uitvoer bevat de beschrijvende label:

```
Get-WmiObject -Class Win32_LogicalDisk | Select-Object -Property Name,FreeSpace | ForEach-Object -Process {$_.FreeSpace = ($_.FreeSpace)/1024.0/1024.0; $_}
Name                                                                  FreeSpace
----                                                                  ---------
C:                                                                48317.7265625
```