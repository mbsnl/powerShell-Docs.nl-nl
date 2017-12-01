---
ms.date: 2017-06-12
author: eslesar
ms.topic: conceptual
keywords: DSC, powershell, configuratie, setup
title: ResourceGet-methode van de klasse MSFT_DSCLocalConfigurationManager
ms.openlocfilehash: 7d8b185c49778253dcb4e983ad948775c4cb0842
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/12/2017
---
# <a name="resourceget-method-of-the-msftdsclocalconfigurationmanager-class"></a>ResourceGet-methode van de klasse MSFT_DSCLocalConfigurationManager

Rechtstreeks roept de **ophalen** methode van een DSC-resource.

<a name="syntax"></a>Syntaxis
------

```mof
uint32 ResourceGet(
  [in]  string           ResourceType,
  [in]  string           ModuleName,
  [in]  uint8            resourceProperty[],
  [out] OMI_BaseResource configurations
);
```

<a name="parameters"></a>Parameters
----------

*ResourceType* \[in\]  
De naam van de bron aan te roepen.

*Modulenaam* \[in\]  
De naam van de module met de bron aan te roepen.

*resourceProperty* \[in\]  
Hiermee geeft u de naam van de resource-eigenschap en de waarde ervan in een hashtabel als de sleutel en waarde, respectievelijk. Gebruik de [Get-DscResource](https://technet.microsoft.com/en-us/library/dn521625.aspx) cmdlet voor het detecteren van de resource-eigenschappen en hun typen.

*configuraties* \[uit\]  
Bevat een ingesloten exemplaar van de configuraties op return.

## <a name="return-value"></a>Retourwaarde
------------

Retourneert nul geslaagd; Anders retourneert een foutcode.

## <a name="remarks"></a>Opmerkingen

Dit is een statische methode.

## <a name="requirements"></a>Vereisten
------------
>**MOF:** DscCore.mof

>**Namespace**: Root\Microsoft\Windows\DesiredStateConfiguration


## <a name="see-also"></a>Zie ook


[**MSFT_DSCLocalConfigurationManager**](msft-dsclocalconfigurationmanager.md)


 

 


