---
ms.date: 06/12/2017
ms.topic: conceptual
keywords: DSC, powershell, configuratie, setup
title: DSC voor Linux nxUser Resource
ms.openlocfilehash: 222bd2191cf5c5f0a90ba947275ffde47d22ec86
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="dsc-for-linux-nxuser-resource"></a>DSC voor Linux nxUser Resource

De **nxUser** in PowerShell Desired State Configuration (DSC)-bron biedt een mechanisme op voor het beheren van lokale gebruikers op een Linux-knooppunt.

## <a name="syntax"></a>Syntaxis

```
nxUser <string> #ResourceName
{
    UserName = <string>
    [ Ensure = <string> { Absent | Present }  ]
    [ FullName = <string> ]
    [ Description = <string> ]
    [ Password = <string> ]
    [ Disabled = <bool> ]
    [ PasswordChangeRequired = <bool> ]
    [ HomeDirectory = <string> ]
    [ GroupID = <string> ]
    [ DependsOn = <string[]> ]

}
```

## <a name="properties"></a>Eigenschappen

|  Eigenschap |  Geeft de accountnaam waarvan u wilt om te controleren of een specifieke status. |
|---|---|
| UserName| Hiermee geeft u de locatie op waar u om te controleren of de status voor een bestand of map.|
| Zorg ervoor dat| Geeft aan of het account bestaat. Deze eigenschap instellen op 'Aanwezig' om ervoor te zorgen dat het account bestaat en stel deze in op 'Ontbreekt' om ervoor te zorgen dat het account niet bestaat.|
| FullName| Een tekenreeks met de volledige naam moet worden gebruikt voor het gebruikersaccount.|
| Beschrijving| De beschrijving voor het gebruikersaccount.|
| Wachtwoord| De hash van het wachtwoord van gebruikers in de juiste vorm voor de Linux-computer. Dit is meestal een gezouten SHA-256 of SHA-512 hash. Voor Debian en Ubuntu Linux, kan deze waarde met de opdracht mkpasswd worden gegenereerd. Voor andere Linux-distributies, kan de methode crypt van Python Crypt bibliotheek worden gebruikt voor het genereren van de hash.|
| Disabled| Hiermee wordt aangegeven of het account is ingeschakeld. Deze eigenschap instellen op **$true** om ervoor te zorgen dat dit account is uitgeschakeld en stel deze in op **$false** om ervoor te zorgen dat deze is ingeschakeld.|
| PasswordChangeRequired| Hiermee wordt aangegeven of de gebruiker het wachtwoord kunt wijzigen. Deze eigenschap instellen op **$true** om ervoor te zorgen dat de gebruiker kan het wachtwoord wijzigen en stel deze in op **$false** zodat de gebruiker het wachtwoord te wijzigen. De standaardwaarde is **$false**. Deze eigenschap wordt alleen beoordeeld als het gebruikersaccount niet aanwezig waren en wordt gemaakt.|
| HomeDirectory| De basismap voor de gebruiker.|
| Groeps-id| De primaire groeps-ID voor de gebruiker.|
| dependsOn | Hiermee wordt aangegeven dat de configuratie van een andere resource uitvoeren moet voordat deze bron is geconfigureerd. Bijvoorbeeld, als de ID van het scriptblok voor resource configuratie die u wilt uitvoeren eerst is 'ResourceName' en het type is 'ResourceType', de syntaxis voor het gebruik van deze eigenschap is `DependsOn = "[ResourceType]ResourceName"`.|

## <a name="example"></a>Voorbeeld

Het volgende voorbeeld zorgt ervoor dat de gebruiker "monuser" bestaat en lid van de groep 'DBusers is'.

```
Import-DSCResource -Module nx

Node $node {
nxUser UserExample{
   UserName = "monuser"
   Description = "Monitoring user"
   Password  =    '$6$fZAne/Qc$MZejMrOxDK0ogv9SLiBP5J5qZFBvXLnDu8HY1Oy7ycX.Y3C7mGPUfeQy3A82ev3zIabhDQnj2ayeuGn02CqE/0'
   Ensure = "Present"
   HomeDirectory = "/home/monuser"
}

nxGroup GroupExample{
   GroupName = "DBusers"
   Ensure = "Present"
   MembersToInclude = "monuser"
   DependsOn = "[nxUser]UserExample"
}
}
```