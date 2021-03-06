---
ms.date: 06/12/2017
contributor: JKeithB
ms.topic: conceptual
keywords: Galerie, powershell, cmdlet, psgallery
title: psgallery_items_tab
ms.openlocfilehash: 5058253678a4f996b080e43820fee06b35b681f4
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="items-tab"></a>Tabblad items

De [tabblad Items](https://www.powershellgallery.com/items) alle beschikbare items worden weergegeven in de PowerShell-galerie.

Er zijn verschillende manieren filteren, sorteren en zoek de items.
Voor meer informatie over een bepaald item, klikt u op het item.

## <a name="filter-by"></a>Filteren op

De vervolgkeuzelijst onder 'Filteren op' kan gebruikers de resultaten door te filteren:
* Include Prerelease
* Alleen stabiele

Zie voor meer informatie over 'Prerelease' en 'Stabiele' [Prerelease versiebeheer toegevoegd aan PowerShellGet en PowerShell Gallery](https://blogs.msdn.microsoft.com/powershell/2017/12/05/prerelease-versioning-added-to-powershellget-and-powershell-gallery/) in de PowerShell-teamblog.

De selectievakjes uit onder de vervolgkeuzelijst kunnen gebruikers de resultaten door te filteren:
* Itemtypen
  - Module
  - Script
* Categorieën
  - De cmdlet
  - DSC-Resource
  - Functie
  - Mogelijkheid tot rol
  - Werkstroom

Als u wilt zien alleen modules in de PowerShell-galerie, Controleer Module in de itemtypen.
Controleer op dezelfde manier als u wilt zien alleen scripts die in de PowerShell-galerie, Script in de itemtypen.

> [!NOTE]
> Filters zijn inclusief.
> Voorbeeld: Een item met zowel cmdlets en functies wordt weergegeven als een Cmdlet of functie (of beide) worden gecontroleerd.
> Als geen van beide zijn geselecteerd, wordt het item wordt niet weergegeven.
> Alle categorieën zijn geselecteerd, wordt alleen items met een van deze categorieën die wordt weergegeven.
> **Items die niet bij een van deze categorieën horen worden niet weergegeven.**

## <a name="sort-by"></a>Sorteren op

De sorteren op vervolgkeuzelijst kan gebruikers de resultaten sorteren op:
* Populariteit - populariteit wordt bepaald door het aantal downloaden
* A-Z - alfabetisch op itemnaam
* Recente - Items worden weergegeven in de volgorde van de datum publiceren

## <a name="search-box"></a>Zoekvak

Het zoekvak kunnen gebruikers de items in de trefwoorden zoeken.
Zie voor meer informatie [galerie Zoeksyntaxis](psgallery_search_syntax.md).