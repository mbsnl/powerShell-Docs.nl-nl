---
ms.date: 02/02/2018
ms.topic: conceptual
keywords: DSC, powershell, configuratie, setup
title: DSC-pull-service
ms.openlocfilehash: 1547092d5ea6733296bf89f05dd96f70c0a000ac
ms.sourcegitcommit: cf195b090b3223fa4917206dfec7f0b603873cdf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2018
---
# <a name="desired-state-configuration-pull-service"></a>Pull-desired State Configuration-Service

> Van toepassing op: Windows PowerShell 5.0

Lokale Configuration Manager kunnen centraal worden beheerd door een oplossing voor Pull-Service.
Wanneer u deze methode gebruikt, wordt het knooppunt dat wordt beheerd geregistreerd bij een service en een configuratie in LCM instellingen toegewezen.
De configuratie en alle benodigde als afhankelijkheden voor de configuratie van de DSC-resources zijn gedownload naar de computer en gebruikt door LCM voor het beheren van de configuratie.
Informatie over de status van de machine wordt beheerd, is geüpload naar de service voor rapportage.
Dit concept wordt 'pull-service' genoemd.

De huidige opties voor het pull-service zijn onder andere:

- Azure Automation gewenst State Configuration-service
- Een pull-service op Windows Server
- Community-oplossingen voor open-source onderhouden
- Een SMB-share

**De aanbevolen oplossing**, en de optie met de meeste functies die beschikbaar is, is [Azure Automation DSC](https://docs.microsoft.com/en-us/azure/automation/automation-dsc-getting-started).

De Azure-service kunt knooppunten lokale in datacenters persoonlijke of openbare clouds, zoals Azure en AWS beheren.
Persoonlijke omgevingen waarin servers kunnen niet rechtstreeks verbinding met Internet maken, Beperk uitgaand verkeer naar de gepubliceerde Azure IP-bereik (Zie [Azure Datacenter IP-adresbereiken](https://www.microsoft.com/en-us/download/details.aspx?id=41653)).

Functies van de online service die momenteel niet beschikbaar in de pull-service op Windows Server zijn:
- Alle gegevens worden versleuteld in rust en onderweg
- Clientcertificaten worden gemaakt en automatisch beheerd
- Geheimen opslaan voor het centraal beheren van [wachtwoorden/referenties](https://docs.microsoft.com/en-us/azure/automation/automation-credentials), of [variabelen](https://docs.microsoft.com/en-us/azure/automation/automation-variables) zoals servernamen of verbindingsreeksen
- Centraal beheren knooppunt [LCM configuratie](metaConfig.md#basic-settings)
- Centraal toewijzen configuraties aan client-knooppunten
- Release-configuratie voor het testen alvorens productie gewijzigd in 'kokospalm groepen'
- Grafische rapportage
  - Details van de status op het niveau van de resource DSC van granulatie
  - Uitgebreide foutberichten van clientcomputers voor probleemoplossing
- [Integratie met Azure-logboekanalyse](https://docs.microsoft.com/en-us/azure/automation/automation-dsc-diagnostics) voor waarschuwingen, geautomatiseerde taken Android/iOS-app voor rapportage en waarschuwingen

## <a name="dsc-pull-service-in-windows-server"></a>DSC-pull-service in Windows Server

Het is mogelijk met het configureren van een pull-service uit te voeren op Windows Server.
Neem in kennis gesteld dat het pull-serviceoplossing opgenomen in Windows Server alleen mogelijkheden bevat van het opslaan van configuraties/modules voor het downloaden en vastleggen van gegevens in database.
Hierbij worden veel van de mogelijkheden die worden aangeboden door de service in Azure en is dus niet een goede hulpprogramma voor het evalueren van hoe de service moet worden gebruikt.

De pull-service wordt aangeboden op Windows Server is een webservice in IIS die gebruikmaakt van een OData-interface om DSC-configuratiebestanden te doelknooppunten wanneer die knooppunten van deze vragen.

Vereisten voor het gebruik van een pull-server:

- Een server met:
  - WMF/PowerShell 5.0 of hoger
  - IIS-serverrol
  - DSC Service
- In het ideale geval sommige betekent dat voor het genereren van een certificaat voor het beveiligen van referenties die zijn doorgegeven aan de lokale Configuration Manager (LCM) op de doelknooppunten

De beste manier om Windows Server configureren voor pull-hostservice is een DSC-configuratie te gebruiken.
Hieronder vindt u een voorbeeldscript.

### <a name="using-the-xdscwebservice-resource"></a>Met behulp van de resource xDSCWebService

De eenvoudigste manier voor het instellen van een pull-webserver is met de resource xWebService, opgenomen in de module xPSDesiredStateConfiguration.
De volgende stappen wordt uitgelegd hoe u de bron in een configuratie die de webservice heeft ingesteld.

1. Roep de [Install-Module](https://technet.microsoft.com/en-us/library/dn807162.aspx) cmdlet voor het installeren van de **xPSDesiredStateConfiguration** module. **Opmerking**: **Install-Module** is opgenomen in de **PowerShellGet** module die is opgenomen in PowerShell 5.0. U kunt downloaden via de **PowerShellGet** -module voor PowerShell 3.0 en 4.0 op [PackageManagement PowerShell-Modules Preview](https://www.microsoft.com/en-us/download/details.aspx?id=49186).
1. Een SSL-certificaat ophalen voor de DSC-Pull-server via een vertrouwde certificeringsinstantie zich binnen uw organisatie of een openbare CA. Het certificaat dat is ontvangen van de instantie is meestal de PFX-indeling. Installeer het certificaat op het knooppunt dat de DSC-Pull-server op de standaardlocatie CERT: \LocalMachine\My moet worden. Noteer de certificaatvingerafdruk van het.
1. Selecteer een GUID moet worden gebruikt als de registratiesleutel. Voor het genereren van een met behulp van PowerShell, typ het volgende achter de PS-prompt en druk op enter: '``` [guid]::newGuid()```'of'```New-Guid```'. Deze sleutel wordt door de knooppunten van de client worden gebruikt als een gedeelde sleutel om te verifiëren tijdens de registratie. Zie de sectie registratiesleutel hieronder voor meer informatie.
1. Start in de PowerShell ISE (F5) het volgende configuratiescript (opgenomen in de map voorbeeld van de **xPSDesiredStateConfiguration** module als Sample_xDscWebService.ps1). Dit script is ingesteld van de pull-server.

```powershell
    configuration Sample_xDscPullServer
    {
        param
        (
                [string[]]$NodeName = 'localhost',

                [ValidateNotNullOrEmpty()]
                [string] $certificateThumbPrint,

                [Parameter(Mandatory)]
                [ValidateNotNullOrEmpty()]
                [string] $RegistrationKey
         )

         Import-DSCResource -ModuleName xPSDesiredStateConfiguration
         Import-DSCResource –ModuleName PSDesiredStateConfiguration

         Node $NodeName
         {
             WindowsFeature DSCServiceFeature
             {
                 Ensure = 'Present'
                 Name   = 'DSC-Service'
             }

             xDscWebService PSDSCPullServer
             {
                 Ensure                   = 'Present'
                 EndpointName             = 'PSDSCPullServer'
                 Port                     = 8080
                 PhysicalPath             = "$env:SystemDrive\inetpub\PSDSCPullServer"
                 CertificateThumbPrint    = $certificateThumbPrint
                 ModulePath               = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules"
                 ConfigurationPath        = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration"
                 State                    = 'Started'
                 DependsOn                = '[WindowsFeature]DSCServiceFeature'
                 UseSecurityBestPractices = $false
             }

            File RegistrationKeyFile
            {
                Ensure          = 'Present'
                Type            = 'File'
                DestinationPath = "$env:ProgramFiles\WindowsPowerShell\DscService\RegistrationKeys.txt"
                Contents        = $RegistrationKey
            }
        }
    }

```

1. Voer de configuratie, voor het doorgeven van de vingerafdruk van het SSL-certificaat als de **certificateThumbPrint** parameter en de registratie van een GUID sleutel als de **RegistrationKey** parameter:

```powershell
    # To find the Thumbprint for an installed SSL certificate for use with the pull server list all certificates in your local store
    # and then copy the thumbprint for the appropriate certificate by reviewing the certificate subjects
    dir Cert:\LocalMachine\my

    # Then include this thumbprint when running the configuration
    Sample_xDSCPullServer -certificateThumbprint 'A7000024B753FA6FFF88E966FD6E19301FAE9CCC' -RegistrationKey '140a952b-b9d6-406b-b416-e0f759c9c0e4' -OutputPath c:\Configs\PullServer

    # Run the compiled configuration to make the target node a DSC Pull Server
    Start-DscConfiguration -Path c:\Configs\PullServer -Wait -Verbose

```

#### <a name="registration-key"></a>Registratiesleutel

Als u wilt toestaan client knooppunten om te registreren bij de server, zodat ze configuratienamen in plaats van een configuratie-ID gebruiken kunnen, een registratiesleutel die is gemaakt door de bovenstaande configuratie wordt opgeslagen in een bestand met de naam `RegistrationKeys.txt` in `C:\Program Files\WindowsPowerShell\DscService`. De registratiesleutel fungeert als een gedeeld geheim gebruikt tijdens de initiële registratie door de client met de pull-server. De client genereert een zelfondertekend certificaat dat is gebruikt voor een unieke verificatie naar de pull-server wanneer de registratie is voltooid. De vingerafdruk van dit certificaat wordt lokaal opgeslagen en die zijn gekoppeld aan de URL van de pull-server.
> **Opmerking**: registratiesleutels worden niet ondersteund in PowerShell 4.0.

Om te configureren van een knooppunt om te verifiëren met de pull-server van de registratie van de moet sleutel in de metaconfiguratie voor elk doelknooppunt die zal worden geregistreerd met deze pull-server. Houd er rekening mee dat de **RegistrationKey** in de onderstaande metaconfiguratie wordt verwijderd nadat de doelmachine heeft geregistreerd en de waarde '140a952b-b9d6-406b-b416-e0f759c9c0e4' moet overeenkomen met de waarde die is opgeslagen in de RegistrationKeys.txt-bestand op de pull-server. Altijd worden beschouwd door de waarde van de registratie-sleutel veilig, omdat elke doelcomputer registreren bij de pull-server geïnstalleerd is toegestaan.

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientConfigID
{
    Node localhost
    {
        Settings
        {
            RefreshMode          = 'Pull'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded   = $true
        }

        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL          = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey    = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
            ConfigurationNames = @('ClientConfig')
        }

        ReportServerWeb CONTOSO-PullSrv
        {
            ServerURL       = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
        }
    }
}

PullClientConfigID -OutputPath c:\Configs\TargetNodes


```

> **Opmerking**: de **ReportServerWeb** sectie kan worden verzonden naar de pull-server melden.

Het ontbreken van de **ConfigurationID** eigenschap in het bestand metaconfiguratie impliciet betekent dat pull-server ondersteunt de V2-versie van het protocol voor pull-server zodat een initiële registratie is vereist.
Als u daarentegen de aanwezigheid van een **ConfigurationID** betekent dat de V1-versie van het pull-protocol wordt gebruikt en er geen registratie-verwerking is.

>**Opmerking**: In een PUSH-scenario is een fout bestaat in de huidige officieel uitgegeven die het nodig zijn voor het definiëren van een eigenschap ConfigurationID in het bestand metaconfiguratie voor knooppunten die nooit zijn geregistreerd bij een pull-server maakt. Hiermee wordt het protocol V1-Pull-Server dwingen en registratie foutberichten voorkomen.

## <a name="placing-configurations-and-resources"></a>Configuraties en resources plaatsen

Nadat de pull server setup is voltooid, de mappen die zijn gedefinieerd door de **ConfigurationPath** en **ModulePath** eigenschappen in de configuratie van de pull-server zijn waar u modules en configuraties worden geplaatst die beschikbaar zal zijn voor de doelknooppunten om op te halen.
Deze bestanden moeten in een specifieke indeling om de pull-server ze correct te verwerken.

### <a name="dsc-resource-module-package-format"></a>DSC-resource module pakket-indeling

Elke resource module moet worden ingepakt en met de naam op basis van het volgende patroon volgen `{Module Name}_{Module Version}.zip`.
Bijvoorbeeld, een module met de naam xWebAdminstration met een moduleversie van 3.1.2.0 de naam 'xWebAdministration_3.2.1.0.zip'.
Elke versie van een module moet worden opgenomen in een enkel zip-bestand.
Omdat er slechts één versie van een resource in elke zip-bestand in WMF 5.0 met de indeling van de module hebt toegevoegd wordt ondersteuning voor meerdere moduleversies van de in een enkele map wordt niet ondersteund.
Dit betekent dat voordat verpakking van DSC-resource-modules voor gebruik met pull-server u moet een kleine wijziging aanbrengen in de mapstructuur.
De standaardindeling van modules met van DSC-resource in WMF 5.0 is ' {Modulemap}\{moduleversie} \DscResources\{DSC-Resourcemap}\'.
Pakketten voor de pull-server te verwijderen en daarna de **{moduleversie}** map zodat het pad ' {Modulemap} \DscResources\{DSC-Resourcemap}\'.
Zip de map met deze wijziging, zoals hierboven wordt beschreven op en plaats deze zip-bestanden in de **ModulePath** map.

Gebruik `new-dscchecksum {module zip file}` een controlesom-bestand voor de toegevoegde module te maken.

### <a name="configuration-mof-format"></a>Configuratie MOF-indeling

Een configuratie MOF-bestand moet worden gekoppeld aan een bestand controlesom zodat een LCM in een doelknooppunt kunt de configuratie valideren.
Aanroepen voor het maken van een controlesom, de [nieuw DSCCheckSum](https://technet.microsoft.com/en-us/library/dn521622.aspx) cmdlet.
De cmdlet heeft een **pad** parameter waarmee de map waarin de configuratie van de MOF zich bevindt.
De cmdlet maakt een controlesom-bestand met de naam `ConfigurationMOFName.mof.checksum`, waarbij `ConfigurationMOFName` is de naam van het mof-bestand voor configuratie.
Als er meer dan één configuratie MOF-bestanden in de opgegeven map, wordt een controlesom gemaakt voor elke configuratie in de map.
Plaats de MOF-bestanden en hun bijbehorende controlesom-bestanden in de **ConfigurationPath** map.

>**Opmerking**: als u het MOF-bestand van de configuratie op een manier wijzigt, moet u ook het bestand controlesom opnieuw.

### <a name="tooling"></a>Tooling

Om de instelling gezamenlijk valideren en het beheren van de eenvoudiger, pull-server de volgende hulpprogramma's zijn opgenomen als voorbeelden in de meest recente versie van de module xPSDesiredStateConfiguration:

1. Een module die u helpt met het verpakken van DSC-resource-modules en de configuratiebestanden voor gebruik op de pull-server. [PublishModulesAndMofsToPullServer.psm1](https://github.com/PowerShell/xPSDesiredStateConfiguration/blob/dev/DSCPullServerSetup/PublishModulesAndMofsToPullServer.psm1). De volgende voorbeelden:

    ```powershell
        # Example 1 - Package all versions of given modules installed locally and MOF files are in c:\LocalDepot
         $moduleList = @("xWebAdministration", "xPhp")
         Publish-DSCModuleAndMof -Source C:\LocalDepot -ModuleNameList $moduleList

         # Example 2 - Package modules and mof documents from c:\LocalDepot
         Publish-DSCModuleAndMof -Source C:\LocalDepot -Force
    ```

1. Een script dat de pull-server valideert is correct geconfigureerd. [PullServerSetupTests.ps1](https://github.com/PowerShell/xPSDesiredStateConfiguration/blob/dev/DSCPullServerSetup/PullServerDeploymentVerificationTest/PullServerSetupTests.ps1).

## <a name="community-solutions-for-pull-service"></a>Community-oplossingen voor Pull-Service

De DSC-community heeft meerdere oplossingen voor het implementeren van het protocol van de service pull geschreven.
Voor on-premises back-omgevingen die deze bieden mogelijkheden voor pull-service en een kans om bijdragen aan de community met incrementele verbeteringen.

- [Tug](https://github.com/powershellorg/tug)
- [DSC-TRÆK](https://github.com/powershellorg/dsc-traek)

## <a name="pull-client-configuration"></a>Pull-clientconfiguratie

De volgende onderwerpen wordt instellen van de pull-clients in detail beschreven:

- [Instellen van een DSC-pull-client met behulp van een configuratie-ID](pullClientConfigID.md)
- [Instellen van een DSC-pull-client met behulp van configuratienamen](pullClientConfigNames.md)
- [Gedeeltelijke configuraties](partialConfigs.md)

## <a name="see-also"></a>Zie ook

- [Windows PowerShell Desired State Configuration-overzicht](overview.md)
- [Configuraties doorvoeren](enactingConfigurations.md)
- [Een DSC-rapportserver gebruiken](reportServer.md)