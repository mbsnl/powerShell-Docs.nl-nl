---
ms.date: 06/12/2017
ms.topic: conceptual
keywords: DSC, powershell, configuratie, setup
title: Groep DSC-Resource
ms.openlocfilehash: 6a4732439bb45e36fa9201975f12194442611002
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="dsc-group-resource"></a>Groep DSC-Resource

> Van toepassing op: Windows PowerShell 4.0, Windows PowerShell 5.0

De bron van de groep in Windows PowerShell Desired State Configuration (DSC) biedt een mechanisme voor het beheren van lokale groepen in het doelknooppunt.

## <a name="syntax"></a>Syntaxis

```
Group [string] #ResourceName
{
    GroupName          = [string]
    [ Credential       = [PSCredential] ]
    [ Description      = [string[]] ]
    [ Ensure           = [string] { Absent | Present }  ]
    [ Members          = [string[]] ]
    [ MembersToExclude = [string[]] ]
    [ MembersToInclude = [string[]] ]
    [ DependsOn        = [string[]] ]
}
```

## <a name="properties"></a>Eigenschappen

|  Eigenschap  |  Beschrijving   |
|---|---|
| GroupName| De naam van de groep waarvoor u om te controleren of een specifieke status.|
| referentie| De referenties die zijn vereist voor toegang tot externe bronnen. **Opmerking**: dit account de juiste Active Directory-machtigingen voor alle niet-lokale accounts toevoegen aan de groep moet hebben; anders wordt een fout optreedt wanneer de configuratie in het doelknooppunt wordt uitgevoerd.
| Beschrijving| De beschrijving van de groep.|
| Zorg ervoor dat| Hiermee wordt aangegeven of de groep bestaat. Deze eigenschap instellen op 'Ontbreekt' om ervoor te zorgen dat de groep niet bestaat. Instellen om "" (de standaardwaarde), zorgt u ervoor dat de groep bestaat.|
| Leden| Gebruik deze eigenschap het lidmaatschap van de huidige vervangt door de opgegeven leden. De waarde van deze eigenschap is een matrix met tekenreeksen van het formulier *domein*\\*gebruikersnaam*. Als u deze eigenschap in een configuratie hebt ingesteld, geen gebruik van ofwel de **MembersToExclude** of **MembersToInclude** eigenschap. In dat geval wordt een fout gegenereerd.|
| MembersToExclude| Gebruik deze eigenschap leden verwijderen uit het bestaande lidmaatschap van de groep. De waarde van deze eigenschap is een matrix met tekenreeksen van het formulier *domein*\\*gebruikersnaam*. Als u deze eigenschap in een configuratie instellen, gebruikt u niet de **leden** eigenschap. In dat geval wordt een fout gegenereerd.|
| MembersToInclude| Gebruik deze eigenschap leden toevoegen aan het bestaande lidmaatschap van de groep. De waarde van deze eigenschap is een matrix met tekenreeksen van het formulier *domein*\\*gebruikersnaam*. Als u deze eigenschap in een configuratie instellen, gebruikt u niet de **leden** eigenschap. Hierdoor wordt een fout gegenereerd.|
| dependsOn | Hiermee wordt aangegeven dat de configuratie van een andere resource uitvoeren moet voordat deze bron is geconfigureerd. Bijvoorbeeld, als de ID van de resourceconfiguratie scriptblok die u wilt uitvoeren eerst is __ResourceName__ en het type __ResourceType__, de syntaxis voor het gebruik van deze eigenschap is ' DependsOn = '[ ResourceType] ResourceName' ''.|

## <a name="example-1"></a>Voorbeeld 1

Het volgende voorbeeld laat zien hoe om ervoor te zorgen dat een groep genaamd 'Testgroep' ontbreekt.

```powershell
Group GroupExample
{
    # This removes TestGroup, if present
    # To create a new group, set Ensure to "Present“
    Ensure = "Absent"
    GroupName = "TestGroup"
}
```

## <a name="example-2"></a>Voorbeeld 2

Het volgende voorbeeld laat zien hoe een Active Directory-gebruiker toevoegen aan de lokale groep administrators als onderdeel van een testlab met meerdere Machine build als u al van een PSCredential voor het lokale Administrator-account gebruikmaakt.
Als dit wordt ook gebruikt voor het domein Admin-Account (na de promotie van domein), moeten we vervolgens deze bestaande PSCredential omzetten in een domein beschrijvende referentie.
Vervolgens kunt we een domeingebruiker toevoegen aan de lokale groep Administrators op de lidserver.

```powershell
@{
    AllNodes = @(
        @{
            NodeName = '*';
            DomainName = 'SubTest.contoso.com';
         }
        @{
            NodeName = 'Box2';
            AdminAccount = 'Admin-Dave_Alexanderson'
        }
    )
}

$domain = $node.DomainName.split('.')[0]
$DCredential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList ("$domain\$($credential.Username)", $Credential.Password)

Group AddADUserToLocalAdminGroup {
    GroupName='Administrators'
    Ensure= 'Present'
    MembersToInclude= "$domain\$($Node.AdminAccount)"
    Credential = $dCredential
    PsDscRunAsCredential = $DCredential
}
```

## <a name="example-3"></a>Voorbeeld 3

Het volgende voorbeeld laat zien hoe om te controleren of een lokale groep, TigerTeamAdmins, op de server TigerTeamSource.Contoso.Com bevat niet de account van een bepaald domein, Contoso\JerryG.

```powershell
Configuration SecureTigerTeamSrouce {
  Import-DscResource -ModuleName 'PSDesiredStateConfiguration'

  Node TigerTeamSource.Contoso.Com {
    Group TigerTeamAdmins {
       GroupName        = 'TigerTeamAdmins'
       Ensure           = 'Present'
       MembersToExclude = "Contoso\JerryG"
    }
  }
}
```