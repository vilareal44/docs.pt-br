---
title: Microsoft.VisualStudio.Activities.Asr.ClientActivityBuilder..ctor
ms.date: 03/30/2017
ms.topic: reference
api_name:
- Microsoft.VisualStudio.Activities.Asr.ClientActivityBuilder..ctor
api_location:
- Microsoft.VisualStudio.Activities.dll
api_type:
- Assembly
ms.assetid: 6b44b13c-7a23-4df2-8f9f-45e2b1430002
ms.openlocfilehash: 99f2eb9447bdf43cb57cfe86f35d2c09044ed470
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "69947621"
---
# <a name="microsoftvisualstudioactivitiesasrclientactivitybuilderctor"></a>Microsoft.VisualStudio.Activities.Asr.ClientActivityBuilder..ctor
Cria uma instância da classe [Microsoft. VisualStudio. Activities. ASR. ClientActivityBuilder](microsoft-visualstudio-activities-asr-clientactivitybuilder.md) .  
  
## <a name="syntax"></a>Sintaxe  
  
```csharp  
public ClientActivityBuilder(OperationDescription operationDescription, string configurationName, string proxyNamespace);  
```  
  
## <a name="parameters"></a>Parâmetros  
  
## <a name="parameter-values"></a>Valores dos parâmetros  
 *operationDescription*  
  
 Descreve a operação a ser executada na atividade de fluxo de trabalho deve ser gerada, incluindo o nome da operação, tipo de retorno e informações de parâmetro. O valor desse parâmetro não deve ser **nulo**. Ele deve descrever uma operação síncrona que usa um contrato de mensagem e usa um argumento com uma mensagem. Se essas condições não forem atendidas, o resultado de runtime do uso do construtor e os outros métodos dessa classe é indefinido.  
  
 *Configuration*  
  
 Especifica o nome da configuração de ponto de extremidade. O valor desse parâmetro não deve ser **nulo** ou vazio. Se essas condições não forem atendidas, o resultado de runtime do uso do construtor e os outros métodos dessa classe é indefinido.  
  
 *proxyNamespace*  
  
 Especifica o namespace de serviço para a operação. O valor desse parâmetro não deve ser **nulo** ou vazio. Se essas condições não forem atendidas, o resultado de runtime do uso do construtor e os outros métodos dessa classe é indefinido.  
  
## <a name="see-also"></a>Confira também

- [Microsoft.VisualStudio.Activities.Asr.ClientActivityBuilder](microsoft-visualstudio-activities-asr-clientactivitybuilder.md)
