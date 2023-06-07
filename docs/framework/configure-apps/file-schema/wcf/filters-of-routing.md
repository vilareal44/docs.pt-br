---
title: <filters> de <routing>
ms.date: 03/30/2017
ms.assetid: 7993cf90-9afd-4c3c-9608-184d5da1105c
ms.openlocfilehash: c9bc3a2c379e14d8cf687676a3ec40702d150e1e
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855241"
---
# <a name="filters-of-routing"></a>\<filters> de \<routing>

Representa uma seção de configuração para definir um conjunto de filtros de roteamento, que determinam o tipo de Windows Communication Foundation (WCF) <xref:System.ServiceModel.Dispatcher.MessageFilter> a ser usado ao avaliar mensagens de entrada.

[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;[**\<routing>**](routing.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<filters>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <routing>
    <filters>
      <filter customType="String"
              filterData="String"
              filterType="Action/Address/AddressPrefix/And/Custom/Endpoint/MatchAll/XPath"
              name="String" />
    </filters>
  </routing>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.

### <a name="attributes"></a>Atributos

Nenhum

### <a name="child-elements"></a>Elementos filho

|     | Descrição |
| --- | ----------- |
| [**\<filter>**](filter.md) | Contém um filtro de roteamento que determina o tipo de Windows Communication Foundation (WCF) <xref:System.ServiceModel.Dispatcher.MessageFilter> será usado ao avaliar mensagens de entrada. |

### <a name="parent-elements"></a>Elementos pai

|     | Descrição |
| --- | ----------- |
| [**\<routing>**](routing.md) | Representa uma seção de configuração para definir um conjunto de filtros de roteamento, que determinam o tipo de Windows Communication Foundation (WCF) <xref:System.ServiceModel.Dispatcher.MessageFilter> a ser usado ao avaliar mensagens de entrada, bem como tabelas de roteamento que definem os pontos de extremidade de destino para envio de mensagens quando um filtro é correspondente. |

## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Routing.Configuration.FilterElement?displayProperty=nameWithType>
