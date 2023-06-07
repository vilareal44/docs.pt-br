---
title: <backupLists>
ms.date: 03/30/2017
ms.assetid: 593b3390-f65b-4684-ad40-0596b62f0954
ms.openlocfilehash: 5f2bb030d13389e15cb44f1ddff3b8168b4f2140
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70850263"
---
# \<backupLists>
Representa uma seção de configuração para definir um conjunto de serviços de backup usados no tratamento de erros. Cada elemento filho é uma lista de backup que enumera um conjunto de pontos de extremidade que você deseja que o serviço de roteamento use caso o ponto de extremidade primário não possa ser alcançado. Se o primeiro ponto de extremidade na lista estiver inativo, o serviço de roteamento fará failover automaticamente para o próximo da lista.  Isso lhe dá uma maneira rápida de adicionar confiabilidade ao seu aplicativo sem ter que ensinar o aplicativo cliente a lidar com padrões complexos ou onde todos os seus serviços são implantados.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<routing>**](routing.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<backupLists>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<routing>
  <backupLists>
    <backupList name="String">
      <add endpointName="String" />
    </backupList>
  </backupLists>
</routing>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<filter>](filter.md)|Contém uma lista de pontos de extremidade que você deseja que o serviço de roteamento use caso o ponto de extremidade primário não possa ser acessado. .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<routing>](routing.md)|Representa uma seção de configuração para definir um conjunto de filtros de roteamento, que determinam o tipo de Windows Communication Foundation (WCF) <xref:System.ServiceModel.Dispatcher.MessageFilter> a ser usado ao avaliar mensagens de entrada, bem como tabelas de roteamento que definem os pontos de extremidade de destino para envio de mensagens quando um filtro é correspondente.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Routing.Configuration.BackupEndpointCollection?displayProperty=nameWithType>
