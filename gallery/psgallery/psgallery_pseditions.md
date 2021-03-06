---
ms.date: 06/12/2017
contributor: JKeithB
ms.topic: conceptual
keywords: Galerie, powershell, cmdlet, psgallery
title: psgallery_pseditions
ms.openlocfilehash: 0b30c1da53832a6b74be7aa14ed9331b1e9fe643
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="items-with-compatible-powershell-editions"></a>Items met compatibel PowerShell-edities
Vanaf versie 5.1 is PowerShell beschikbaar in verschillende edities die staan voor verschillende functies en platformcompatibiliteit.

- **Desktop-editie:** deze editie is gebaseerd op .NET Framework en biedt compatibiliteit met scripts en modules die zijn gericht op versies van PowerShell die worden uitgevoerd op edities van Windows met een volledige footprint zoals Server Core en Windows Desktop.
- **Core-editie:** deze editie is gebaseerd op .NET Framework en biedt compatibiliteit met scripts en modules die zijn gericht op versies van PowerShell die worden uitgevoerd op edities van Windows met een verminderde footprint zoals Nano Server en Windows IoT.

## <a name="powershell-gallery-extracts-supported-pseditions-metadata-and-allows-you-to-filters-the-items-compatible-for-specific-powershell-editions"></a>PowerShell Gallery extraheert ondersteunde PSEditions metagegevens en kunt u filters de items compatibel voor specifieke PowerShell-edities

Als een artikel heeft compatibel PSEditions opgegeven, worden deze weergegeven als onderdeel van PowerShell-edities in de pagina item weergeven en ook in de resultaten van de items.
![Pagina van de item weergeven met PSEditions](Images/ItemDisplayPageWithPSEditions.PNG)

## <a name="search-for-items-in-the-gallery-ui-which-works-on-powershellcore"></a>Zoeken naar items in de gebruikersinterface die op PowerShellCore werkt-galerie
Codes gebruiken: Tags en 'PSEdition_Desktop': 'PSEdition_Core' met filters voor de items in PowerShell Gallery.

### <a name="use-tagspseditioncore-to-search-items-compatible-with-powershell-core-edition"></a>Codes gebruiken: 'PSEdition_Core' om te zoeken items compatibel met PowerShell Core Edition.
![Zoekresultaten voor objecten die compatibel is met Core PSEdition](Images/SearchResultsWithPSEditions.PNG)

### <a name="use-tagspseditiondesktop-to-search-items-compatible-with-powershell-desktop-edition"></a>Codes gebruiken: 'PSEdition_Desktop' om te zoeken items die compatibel is met PowerShell Desktop Edition.
![Zoekresultaten voor objecten die compatibel is met het bureaublad PSEdition](Images/SearchResultsWithPSEdition_Desktop.PNG)

## <a name="more-details-on-authoring-and-finding-the-items-with-compatible-powershell-editions"></a>Meer informatie over het ontwerpen en zoeken van de items met compatibel PowerShell-edities
### <a name="modules-with-pseditionspsgetmodulemodulewithpseditionsupportmd"></a>[Modules met PSEditions](../psget/module/modulewithpseditionsupport.md)
### <a name="scripts-with-pseditionspsgetscriptscriptwithpseditionsupportmd"></a>[Scripts met PSEditions](../psget/script/scriptwithpseditionsupport.md)