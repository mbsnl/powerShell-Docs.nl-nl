---
ms.date: 2017-06-12
author: eslesar
ms.topic: conceptual
keywords: DSC, powershell, configuratie, setup
title: SendConfigurationApplyAsync-methode van de klasse MSFT_DSCLocalConfigurationManager
ms.openlocfilehash: e680bfd1c5b39d364c90cf5ef6b43d0a568af23a
ms.sourcegitcommit: 75f70c7df01eea5e7a2c16f9a3ab1dd437a1f8fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/12/2017
---
# <a name="sendconfigurationapplyasync-method-of-the-msftdsclocalconfigurationmanager-class"></a>SendConfigurationApplyAsync-methode van de klasse MSFT_DSCLocalConfigurationManager

Het configuratiebestand voor asynchroon verzendt naar het beheerde knooppunt en gebruik van de configuratie-Agent voor de configuratie.

<a name="syntax"></a>Syntaxis
------

```mof
uint32 SendConfigurationApplyAsync(
  [in] uint8   ConfigurationData[],
  [in] boolean force,
  [in] string  jobId
);
```

<a name="parameters"></a>Parameters
----------

*ConfigurationData* \[in\]  
De omgevingsgegevens voor de configuratie.

*Force* \[in\]  
**de waarde True** om af te dwingen van de configuratie te stoppen.

*jobId* \[in\]  
De ID van de taak voor voor het verzenden van de configuratie.

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


 

 


