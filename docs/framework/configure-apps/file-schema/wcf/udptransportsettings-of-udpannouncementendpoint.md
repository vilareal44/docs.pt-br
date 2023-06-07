---
title: <udpTransportSettings> de <udpAnnouncementEndpoint>
ms.date: 03/30/2017
ms.assetid: a7ddff1a-5eed-4bbc-8580-b95ef8890e1f
ms.openlocfilehash: b67bdf825948dffe18aabe91b0de236eb929bccc
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70854855"
---
# <a name="udptransportsettings-of-udpannouncementendpoint"></a>\<udpTransportSettings> de \<udpAnnouncementEndpoint>
Este elemento de configuração expõe as configurações de transporte UDP para [\<udpAnnouncementEndpoint>](udpannouncementendpoint.md) .  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<udpAnnouncementEndpoint>**](udpannouncementendpoint.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<updTransportSettings>**  

## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <udpAnnouncementEndpoint>
      <standardEndpoint>
        <updTransportSettings duplicateMessageHistoryLength="Integer"
                              maxBufferPoolSize="Integer"
                              maxMulticastRetransmitCount="Integer"
                              maxPendingMessageCount="Integer"
                              maxReceivedMessageSize="Integer"
                              maxUnicastRetransmitCount="Integer"
                              multicastInterfaceId="String"
                              socketReceiveBufferSize="Integer"
                              timeToLive="Integer" />
      </standardEndpoint>
    </udpAnnouncementEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|duplicateMessageHistoryLength|Um inteiro que especifica o número máximo de hashes de mensagem usados pelo transporte para identificar mensagens duplicadas.  A detecção de duplicidades será feita no nível de transporte. A definição dessa propriedade como 0 desabilita a detecção de duplicidades.<br /><br /> Esse atributo permite que administradores de sistema ou desenvolvedores desativem algoritmos de detecção de mensagens duplicadas. Isso pode ser desejável se você quiser implementar seu próprio algoritmo de detecção de duplicidades.<br /><br /> O padrão é 4112.|  
|maxBufferPoolSize|Um inteiro que especifica o tamanho máximo de qualquer pool de buffers usado pelo transporte.|  
|maxMulticastRetransmitCount|Um inteiro que especifica o número máximo de vezes que a mensagem deve ser retransmitida (além do primeiro envio).<br /><br /> O padrão é 2.|  
|maxPendingMessageCount|Um inteiro que especifica o número máximo de mensagens que foram recebidas, mas que ainda não foram removidas do InputQueue para uma instância de canal individual.  Se o InputQueue tiver atingido seu limite de contagem de mensagens pendentes, a mensagem será descartada.<br /><br /> O padrão é 32.|  
|maxReceivedMessageSize|Um inteiro que especifica o tamanho máximo de uma mensagem que pode ser processada pela associação.<br /><br /> O valor padrão é 65507.|  
|maxUnicastRetransmitCount|Um inteiro que especifica o número máximo de vezes que a mensagem deve ser retransmitida (além do primeiro envio).  Se a mensagem for enviada para um endereço unicast e uma mensagem de resposta for recebida com um cabeçalho RelatesTo correspondente, a retransmissão poderá ser encerrada antecipadamente (antes de retransmitir o número de vezes configurado).<br /><br /> O valor padrão é 1.|  
|multicastInterfaceId|Uma cadeia de caracteres que identifica exclusivamente o adaptador de rede que deve ser usado ao enviar e receber o tráfego de multicast em máquinas com hospedagem múltipla. Em tempo de execução, o transporte usará esse valor de atributo para pesquisar o índice de interface, que é usado para definir as `IP_MULTICAST_IF` `IPV6_MULTICAST_IF` Opções de soquete e.  O mesmo índice de interface será usado ao ingressar em um grupo de multicast, se aplicável.<br /><br /> O valor padrão é `null`.|  
|socketReceiveBufferSize|Um inteiro que especifica o tamanho do buffer de recebimento no soquete do WinSock subjacente.<br /><br /> Um usuário de um canal receptor pode usar esse atributo na associação para controlar como o sistema se comporta quando recebe dados.  Por exemplo, Considerando que um aplicativo que está consumindo mensagens WCF de entrada no limite máximo, usar um valor mais alto para esse atributo permitiria que as mensagens fossem empilhadas no buffer do WinSock enquanto aguardava que o aplicativo pudesse processá-las.  Usar um valor mais baixo na mesma situação resultaria em mensagens sendo descartadas. Esse atributo expõe a opção de `SO_RCVBUF` soquete WinSock subjacente. Esse valor de atributo deve ser pelo menos o tamanho de `maxReceivedMessageSize` .   Configurá-lo com um valor menor do que o `maxReceivedMessageSize` resultará em exceção de tempo de execução.<br /><br /> O valor padrão é 65536.|  
|timeToLive|Um inteiro que especifica o número de saltos de segmento de rede que um pacote de multicast pode atravessar.  Esse atributo expõe a funcionalidade associada às `IP_MULTICAST_TTL` Opções de `IP_TTL` soquete e.<br /><br /> O valor padrão é 1.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<udpAnnouncementEndpoint>](udpannouncementendpoint.md)|Um ponto de extremidade padrão que corrigiu o contrato de anúncio e a associação de transporte UDP.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Discovery.UdpTransportSettings>
