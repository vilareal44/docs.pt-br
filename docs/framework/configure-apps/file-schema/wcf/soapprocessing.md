---
title: <soapProcessing>
ms.date: 03/30/2017
ms.assetid: e8707027-e6b8-4539-893d-3cd7c13fbc18
ms.openlocfilehash: 0728e22205d4ac2c7674f7690e142aed51d42440
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399534"
---
# \<soapProcessing>

Define o comportamento de ponto de extremidade de cliente usado para realizar marshaling de mensagens entre diferentes tipos de associação e versões de mensagem.

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<soapProcessing>**
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<soapProcessing processMessages="true|false" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
  
As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|                   | Descrição |
| ----------------- | ----------- |
| `processMessages` | Um valor booliano que especifica se as mensagens devem ser empacotadas entre as versões de mensagem SOAP. |

### <a name="child-elements"></a>Elementos filho

Nenhum

### <a name="parent-elements"></a>Elementos pai

|     | Descrição |
| --- | ----------- |
| [**\<behavior>**](behavior-of-endpointbehaviors.md) | Especifica um comportamento de ponto de extremidade. |

## <a name="remarks"></a>Comentários

O processamento de SOAP é o processo em que as mensagens são convertidas entre as versões de mensagem.

O serviço de roteamento do Windows Communication Foundation (WCF) pode converter mensagens de um protocolo para outro. Se as versões de mensagens de entrada e saída forem diferentes, uma nova mensagem da versão correta será criada. O processamento de mensagens de um <xref:System.ServiceModel.Channels.MessageVersion> para outro é realizado pela construção de uma nova mensagem do WCF que contém a parte do corpo e os cabeçalhos relevantes da mensagem de entrada do WCF. Os cabeçalhos que são específicos para endereçamento, ou que são compreendidos no nível do roteador, não são usados durante a construção da nova mensagem do WCF, pois esses cabeçalhos são de uma versão diferente (no caso de cabeçalhos de endereçamento) ou foram processados como parte da comunicação entre o cliente e o roteador.

Se um cabeçalho é colocado na mensagem de saída é determinado por se ele foi marcado como compreendido ou não como sendo passado pela camada de canal de entrada. Os cabeçalhos que não são compreendidos (como cabeçalhos personalizados) não são removidos e, portanto, passam pelo serviço de roteamento ao serem copiados para a mensagem de saída. O corpo da mensagem é copiado para a mensagem de saída. Em seguida, a mensagem envia o canal de saída, ponto em que todos os cabeçalhos e outros dados de envelope específicos ao protocolo de comunicação/transporte serão criados e adicionados.

Essas etapas de processamento ocorrem quando o comportamento de processamento de SOAP é especificado. Esse [\<soapProcessingExtension>](soapprocessing.md) comportamento é um comportamento de ponto de extremidade que é aplicado a todos os pontos de extremidades de cliente (enviados) quando o serviço de roteamento é iniciado. padrão, o [\<routing>](routing-of-servicebehavior.md) comportamento cria e anexa um novo [\<soapProcessingExtension>](soapprocessing.md) comportamento com `processMessages` definido como `true` para cada ponto de extremidade do cliente. Se você tiver um protocolo que o serviço de roteamento não entende ou deseja substituir o comportamento de processamento padrão, você poderá desabilitar o processamento SOAP para o serviço de roteamento inteiro ou apenas para pontos de extremidade específicos.  Para desabilitar o processamento de SOAP para todo o serviço de roteamento em todos os pontos de extremidade, defina o `soapProcessing` atributo do [\<routing>](routing-of-servicebehavior.md) comportamento como `false` . Para desativar o processamento de SOAP para um ponto de extremidade específico, use esse comportamento e defina seu `processMessages` atributo como `false` , em seguida, anexe esse comportamento ao ponto de extremidade no qual você não deseja que o código de processamento padrão seja executado.  Quando o [\<routing>](routing-of-servicebehavior.md) comportamento configura o serviço de roteamento, ele ignorará a reaplicação do comportamento do ponto de extremidade desde que já exista um.
