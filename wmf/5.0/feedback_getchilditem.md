---
ms.date: 06/12/2017
author: JKeithB
ms.topic: reference
keywords: wmf,powershell,installeren
ms.openlocfilehash: 62cccabd7c63c6ba928fc2bf8addd3d11483e90f
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="get-childitem-has--depth-parameter"></a>Get-ChildItem heeft - diepte-parameter
**Get-ChildItem** heeft nu een **– diepte** u met de parameter **– Recurse** de recursie beperken:

PS C:\\gebruikers\\slee\\downloadt\\voorbeeld&gt; Get-ChildItem-Recurse - diepte 0

Map: C:\\gebruikers\\slee\\downloadt\\voorbeeld

Modus LastWriteTime lengte naam

---- ------------- ------ ----

d---4-14-2015 17:36 uur Depth0

-een---4-14-2015 13:19 uur 0 Bestand1.txt

-een---4-14-2015 13:19 uur 0 bestand2.txt samengevoegd

-een---4-14-2015 13:19 uur 0 File3.txt

PS C:\\gebruikers\\slee\\downloadt\\voorbeeld&gt; Get-ChildItem-Recurse - diepte 1

Map: C:\\gebruikers\\slee\\downloadt\\voorbeeld

Modus LastWriteTime lengte naam

---- ------------- ------ ----

d---4-14-2015 17:36 uur Depth0

-een---4-14-2015 13:19 uur 0 Bestand1.txt

-een---4-14-2015 13:19 uur 0 bestand2.txt samengevoegd

-een---4-14-2015 13:19 uur 0 File3.txt

Map: C:\\gebruikers\\slee\\downloadt\\voorbeeld\\Depth0

Modus LastWriteTime lengte naam

---- ------------- ------ ----

d---4-14-2015 5:33 PM Depth1