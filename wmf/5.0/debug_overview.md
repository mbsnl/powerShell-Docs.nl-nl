---
ms.date: 06/12/2017
author: JKeithB
ms.topic: reference
keywords: wmf,powershell,installeren
ms.openlocfilehash: dee5e8206c61d79faadf8573a82c74d4ac0fb8e0
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="improvements-in-powershell-script-debugging"></a>Verbeteringen in foutopsporing voor PowerShell-scripts

Een aantal verbeteringen aangebracht in PowerShell 5.0 om de foutopsporing ervaring te verbeteren:

## <a name="break-all"></a>Alle opsplitsen

De PowerShell-console en de Windows PowerShell ISE kunt u nu onderbreken met foutopsporing voor het uitvoeren van scripts. Dit werkt in zowel lokale als externe sessies.

In de console, drukt u op **Ctrl + Break**.

In de ISE, drukt u op **Ctrl + B**, of gebruik de **Debug -> alle opsplitsen** menuopdracht.

## <a name="remote-debugging-and-remote-file-editing-in-windows-powershell-ise"></a>Foutopsporing op afstand en extern bestand bewerken in Windows PowerShell ISE

Windows PowerShell ISE nu kunt u bestanden openen en bewerken in een externe sessie met de opdracht PSEdit.
U kunt bijvoorbeeld een bestand vanaf de opdrachtregel bewerken in een externe sessie als volgt openen:

```powershell
[RemoteComputer1]: PS C:\> PSEdit C:\DebugDemoScripts\Test-GetMutex.ps1
```

Bovendien kunt u nu bewerken en opslaan van wijzigingen in een extern bestand dat automatisch wordt geopend in Windows PowerShell ISE wanneer u een onderbrekingspunt.
Nu kunt u fouten opsporen in een scriptbestand dat wordt uitgevoerd op een externe computer, het bestand bewerken als een fout corrigeren en voer het gewijzigde script.

## <a name="advanced-script-debugging"></a>Foutopsporing in geavanceerde scripts

Er zijn nieuwe, geavanceerde foutopsporing functies waarmee u kunnen koppelen aan een proces voor lokale computer die Windows PowerShell geladen en willekeurige runspaces in het proces voor foutopsporing.

### <a name="runspace-debugging"></a>Runspace foutopsporing

Nieuwe cmdlets er zijn toegevoegd waarmee u een lijst met huidige runspaces in een proces en de Windows PowerShell-console of de ISE-foutopsporingsprogramma koppelen aan die runspace voor foutopsporing in scripts kunt:

-   Get-Runspace
-   Debug-Runspace
-   Enable-RunspaceDebug
-   Disable-RunspaceDebug
-   Get-RunspaceDebug

### <a name="attach-to-process-hosting-powershell"></a>Koppelen aan een proces voor het hosten van PowerShell

U kunt nu koppelen aan het proces voor elke computer met Windows PowerShell geladen. U doen dit door te voeren in een interactieve sessie met het proces, net zoals bij hoe u in een interactieve externe sessie invoeren door de Enter-PSSession cmdlet:

-   Enter-PSHostProcess
-   Exit-PSHostProcess