---
title: Cotas de transporte
ms.date: 03/30/2017
helpviewer_keywords:
- transport quotas [WCF]
ms.assetid: 3e71dd3d-f981-4d9c-9c06-ff8abb61b717
ms.openlocfilehash: fca5fbeffb560f848edda6421301785f02547d2c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585695"
---
# <a name="transport-quotas"></a>Cotas de transporte
As cotas de transporte são um mecanismo de política para decidir quando uma conexão está consumindo recursos excessivos. Uma cota é um limite rígido que impede o uso de recursos adicionais depois que o valor da cota é excedido. As cotas de transporte impedem ataques de negação de serviço maliciosos ou não intencionais.  
  
 Os transportes do Windows Communication Foundation (WCF) têm valores de cota padrão baseados em uma alocação conservadora de recursos. Esses valores padrão são adequados para ambientes de desenvolvimento e pequenos cenários de instalação. Os administradores de serviço devem revisar as cotas de transporte e ajustar valores de cota individuais se uma instalação estiver ficando sem recursos ou se as conexões estiverem sendo limitadas, apesar da disponibilidade de recursos adicionais.  
  
## <a name="types-of-transport-quotas"></a>Tipos de cotas de transporte  
 Os transportes do WCF têm três tipos de cotas:  
  
- Os *tempos limite* atenuam ataques de negação de serviço que dependem da vinculação de recursos por um longo período de tempo.  
  
- *Os limites de alocação de memória* impedem uma única conexão de esgotar a memória do sistema e negar o serviço a outras conexões.  
  
- *Limites de tamanho de coleção limitam* o consumo de recursos que alocam indiretamente a memória ou estão em um fornecimento limitado.  
  
## <a name="transport-quota-descriptions"></a>Descrições de cota de transporte  
 Esta seção descreve as cotas de transporte disponíveis para os transportes padrão do WCF: HTTP (S), TCP/IP e pipes nomeados. Transportes personalizados podem expor suas próprias cotas configuráveis não incluídas nesta lista. Consulte a documentação de um transporte personalizado para descobrir sobre suas cotas.  
  
 Cada configuração de cota tem um tipo, valor mínimo e valor padrão. O valor máximo de uma cota é limitado por seu tipo. Devido a limitações do computador, nem sempre é possível definir uma cota para seu valor máximo.  
  
|Nome|Type|Mín.<br /><br /> valor|Padrão<br /><br /> valor|Descrição|  
|----------|----------|--------------------|-----------------------|-----------------|  
|`ChannelInitializationTimeout`|TimeSpan|1 tique|5 segundos|Tempo máximo de espera por uma conexão para enviar o preâmbulo durante a leitura inicial. Esses dados são recebidos antes que a autenticação ocorra. Essa configuração geralmente é muito menor do que o `ReceiveTimeout` valor da cota.|  
|`CloseTimeout`|TimeSpan|0|1 minuto|Tempo máximo de espera para que uma conexão seja fechada antes de o transporte gerar uma exceção.|  
|`ConnectionBufferSize`|Inteiro|1|8 KB|Tamanho, em bytes, dos buffers de transmissão e recebimento do transporte subjacente. Aumentar o tamanho do buffer pode melhorar a taxa de transferência ao enviar mensagens grandes.|  
|`IdleTimeout`|TimeSpan|0|2 min|Tempo máximo que uma conexão em pool pode permanecer ociosa antes de ser fechada.<br /><br /> Essa configuração se aplica somente a conexões em pool.|  
|`LeaseTimeout`|TimeSpan|0|5 min|Tempo de vida máximo de uma conexão ativa em pool. Depois que o tempo especificado decorrer, a conexão será fechada depois que a solicitação atual for atendida.<br /><br /> Essa configuração se aplica somente a conexões em pool.|  
|`ListenBacklog`|Inteiro|1|10|Número máximo de conexões que o ouvinte pode ter não atendido antes que conexões adicionais para esse ponto de extremidade sejam negadas.|  
|`MaxBufferPoolSize`|long|0|512 KB|Memória máxima, em bytes, que o transporte deve votar no pool de buffers de mensagens reutilizáveis. Quando o pool não pode fornecer um buffer de mensagens, um novo buffer é alocado para uso temporário.<br /><br /> Instalações que criam várias fábricas de canal ou ouvintes podem alocar grandes quantidades de memória para pools de buffer. Reduzir esse tamanho de buffer pode reduzir muito o uso de memória nesse cenário.|  
|`MaxBufferSize`|Inteiro|1|64 KB|Tamanho máximo, em bytes, de um buffer usado para transmitir dados. Se essa cota de transporte não estiver definida ou o transporte não estiver usando streaming, o valor da cota será o mesmo que o menor do `MaxReceivedMessageSize` valor da cota e <xref:System.Int32.MaxValue> .|  
|`MaxOutboundConnectionsPerEndpoint`|Inteiro|1|10|Número máximo de conexões de saída que podem ser associadas a um ponto de extremidade específico.<br /><br /> Essa configuração se aplica somente a conexões em pool.|  
|`MaxOutputDelay`|TimeSpan|0|200 MS|Tempo máximo de espera após uma operação de envio para enviar mensagens adicionais em lote em uma única operação. As mensagens são enviadas antes se o buffer do transporte subjacente ficar cheio. O envio de mensagens adicionais não redefine o período de atraso.|  
|`MaxPendingAccepts`|Inteiro|1|1|Número máximo de aceitações para canais que o ouvinte pode ter aguardando.<br /><br /> Há um intervalo de tempo entre a conclusão da aceitação e a inicialização de uma nova aceitação. Aumentar esse tamanho de coleção pode impedir que os clientes que se conectam durante esse intervalo sejam descartados.|  
|`MaxPendingConnections`|Inteiro|1|10|Número máximo de conexões que o ouvinte pode ter aguardando para ser aceito pelo aplicativo. Quando esse valor de cota é excedido, novas conexões de entrada são removidas em vez de aguardar para serem aceitas.<br /><br /> Recursos de conexão, como segurança de mensagem, podem fazer com que um cliente Abra mais de uma conexão. Os administradores de serviço devem considerar essas conexões adicionais ao definir esse valor de cota.|  
|`MaxReceivedMessageSize`|long|1|64 KB|O tamanho máximo, em bytes, de uma mensagem recebida, incluindo os cabeçalhos, antes de o transporte gerar uma exceção.|  
|`OpenTimeout`|TimeSpan|0|1 minuto|Tempo máximo de espera para que uma conexão seja estabelecida antes de o transporte gerar uma exceção.|  
|`ReceiveTimeout`|TimeSpan|0|10 min|Tempo máximo de espera para a conclusão de uma operação de leitura antes de o transporte gerar uma exceção.|  
|`SendTimeout`|Timespan|0|1 minuto|Tempo máximo de espera para a conclusão de uma operação de gravação antes de o transporte gerar uma exceção.|  
  
 As cotas de transporte `MaxPendingConnections` e `MaxOutboundConnectionsPerEndpoint` são combinadas em uma única cota de transporte chamada `MaxConnections` quando definidas por meio da associação ou configuração. Somente o elemento Binding permite definir esses valores de cota individualmente. A `MaxConnections` cota de transporte tem os mesmos valores mínimo e padrão.  
  
## <a name="setting-transport-quotas"></a>Definindo cotas de transporte  
 As cotas de transporte são definidas por meio do elemento de associação de transporte, da Associação de transporte, da configuração do aplicativo ou da política de host. Este documento não aborda a definição de transportes por meio da política de host. Consulte a documentação do transporte subjacente para descobrir as configurações para cotas de política de host. O tópico [Configurando http e HTTPS](configuring-http-and-https.md) descreve as configurações de cota para o driver http. sys. Pesquise a base de dados de conhecimento Microsoft para obter mais informações sobre como configurar limites do Windows em conexões HTTP, TCP/IP e pipe nomeado.  
  
 Outros tipos de cotas se aplicam indiretamente a transportes. O codificador de mensagem que o transporte usa para transformar uma mensagem em bytes pode ter suas próprias configurações de cota. No entanto, essas cotas são independentes do tipo de transporte que está sendo usado.  
  
### <a name="controlling-transport-quotas-from-the-binding-element"></a>Controlando cotas de transporte do elemento de associação  
 A definição de cotas de transporte por meio do elemento de associação oferece a maior flexibilidade no controle do comportamento do transporte. Os tempos limite padrão para operações de fechamento, abertura, recebimento e envio são obtidos da associação quando um canal é compilado.  
  
|Nome|HTTP|TCP/IP|Pipe nomeado|  
|----------|----------|-------------|----------------|  
|`ChannelInitializationTimeout`||X|X|  
|`CloseTimeout`||||  
|`ConnectionBufferSize`||X|X|  
|`IdleTimeout`||X|X|  
|`LeaseTimeout`||X||  
|`ListenBacklog`||X||  
|`MaxBufferPoolSize`|X|X|X|  
|`MaxBufferSize`|X|X|X|  
|`MaxOutboundConnectionsPerEndpoint`||X|X|  
|`MaxOutputDelay`||X|X|  
|`MaxPendingAccepts`||X|X|  
|`MaxPendingConnections`||X|X|  
|`MaxReceivedMessageSize`|X|X|X|  
|`OpenTimeout`||||  
|`ReceiveTimeout`||||  
|`SendTimeout`||||  
  
### <a name="controlling-transport-quotas-from-the-binding"></a>Controlando cotas de transporte da Associação  
 Definir as cotas de transporte por meio da Associação oferece um conjunto simplificado de cotas para escolher enquanto ainda concede acesso aos valores de cota mais comuns.  
  
|Nome|HTTP|TCP/IP|Pipe nomeado|  
|----------|----------|-------------|----------------|  
|`ChannelInitializationTimeout`||||  
|`CloseTimeout`|X|X|X|  
|`ConnectionBufferSize`||||  
|`IdleTimeout`||||  
|`LeaseTimeout`||||  
|`ListenBacklog`||X||  
|`MaxBufferPoolSize`|X|X|X|  
|`MaxBufferSize`|1|X|X|  
|`MaxOutboundConnectionsPerEndpoint`||2|2|  
|`MaxOutputDelay`||||  
|`MaxPendingAccepts`||||  
|`MaxPendingConnections`||2|2|  
|`MaxReceivedMessageSize`|X|X|X|  
|`OpenTimeout`|X|X|X|  
|`ReceiveTimeout`|X|X|X|  
|`SendTimeout`|X|X|X|  
  
1. A `MaxBufferSize` cota de transporte só está disponível na `BasicHttp` associação. As `WSHttp` associações são para cenários que não dão suporte a modos de transporte transmitidos.  
  
2. As cotas de transporte `MaxPendingConnections` e `MaxOutboundConnectionsPerEndpoint` são combinadas em uma única cota de transporte chamada `MaxConnections` .  
  
### <a name="controlling-transport-quotas-from-configuration"></a>Controlando cotas de transporte da configuração  
 A configuração do aplicativo pode definir as mesmas cotas de transporte que acessam propriedades diretamente em uma associação. Em arquivos de configuração, o nome de uma cota de transporte sempre começa com uma letra minúscula. Por exemplo, a `CloseTimeout` propriedade em uma associação corresponde à `closeTimeout` configuração na configuração e a `MaxConnections` propriedade em uma associação corresponde à `maxConnections` configuração na configuração.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement>
- <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>
- <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
