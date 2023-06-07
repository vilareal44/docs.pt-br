---
title: Selecionando um transporte
description: 'Saiba mais sobre os critérios para escolher entre os principais transportes que o WCF oferece: HTTP, TCP e pipes nomeados.'
ms.date: 03/30/2017
helpviewer_keywords:
- choosing transports [WCF]
ms.assetid: b169462b-f7b6-4cf4-9fca-d306909ee8bf
ms.openlocfilehash: e1a92203de25aa399316eea91a758802768442a0
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247488"
---
# <a name="choosing-a-transport"></a>Selecionando um transporte
Este tópico discute os critérios para escolher entre os três transportes principais que estão incluídos no Windows Communication Foundation (WCF): HTTP, TCP e pipes nomeados. O WCF também inclui um transporte de enfileiramento de mensagens (também conhecido como MSMQ), mas este documento não aborda o enfileiramento de mensagens.  
  
 O modelo de programação do WCF separa as operações de ponto de extremidade (como expressas em um contrato de serviço) do mecanismo de transporte que conecta dois pontos de extremidade. Isso lhe dá a flexibilidade de decidir como expor seus serviços para a rede.  
  
 No WCF, você especifica como transferir dados em uma rede entre pontos de extremidade usando uma *Associação*, que é composta de uma sequência de *elementos de associação*. Um transporte é representado por um elemento de associação de transporte, que faz parte da associação. Uma associação inclui elementos de associação de protocolo opcionais, como segurança, um elemento de associação de codificador de mensagem necessário e um elemento de associação de transporte necessário. Um transporte envia ou recebe a forma serializada de uma mensagem para ou de outro aplicativo.  
  
 Se você precisar se conectar a um cliente ou servidor existente, talvez não tenha a opção de usar um transporte específico. No entanto, os serviços WCF podem se tornar acessíveis por meio de vários pontos de extremidade, cada um com um transporte diferente. Quando um único transporte não abrange o público-alvo para seu serviço, considere expor o serviço em vários pontos de extremidade. Os aplicativos cliente podem usar o ponto de extremidade melhor para eles.  
  
 Depois de escolher um transporte, você deve selecionar uma associação que o utilize. Você pode escolher uma associação fornecida pelo sistema (consulte [associações fornecidas pelo sistema](../system-provided-bindings.md)) ou pode criar sua própria associação personalizada (consulte [associações personalizadas](../extending/custom-bindings.md)). Você também pode criar sua própria associação. Para obter mais informações, consulte [Criando associações definidas pelo usuário](../extending/creating-user-defined-bindings.md).  
  
## <a name="advantages-of-each-transport"></a>Vantagens de cada transporte  
 Esta seção descreve os principais motivos para escolher qualquer um dos três transportes principais, incluindo um gráfico de decisão detalhado para escolher entre eles.  
  
### <a name="when-to-use-http-transport"></a>Quando usar o transporte HTTP  
 HTTP é um protocolo de solicitação/resposta entre clientes e servidores. O aplicativo mais comum consiste em clientes de navegador da Web que se comunicam com um servidor Web. O cliente envia uma solicitação para um servidor, que escuta as mensagens de solicitação do cliente. Quando o servidor recebe uma solicitação, ele retorna uma resposta, que contém o status da solicitação. Se forem bem-sucedidos, os dados opcionais, como uma página da Web, uma mensagem de erro ou outras informações, serão retornados. Para obter mais informações sobre o protocolo HTTP, consulte [http-Hypertext Transfer Protocol](https://www.w3.org/Protocols/).  
  
 O protocolo HTTP não é baseado em conexão — depois que a resposta é enviada, nenhum estado é mantido. Para lidar com transações de várias páginas, o aplicativo deve manter qualquer Estado necessário.  
  
 No WCF, a associação de transporte HTTP é otimizada para interoperabilidade com sistemas não WCF herdados. Se todas as partes de comunicação estiverem usando o WCF, as associações baseadas em TCP ou em pipes nomeados serão mais rápidas. Para obter mais informações, consulte <xref:System.ServiceModel.NetTcpBinding> e <xref:System.ServiceModel.NetNamedPipeBinding>.  
  
### <a name="when-to-use-the-tcp-transport"></a>Quando usar o transporte TCP  
 O TCP é um serviço de entrega orientado por fluxo baseado em conexão com detecção e correção de erros de ponta a ponta. *Baseado em conexão* significa que uma sessão de comunicação entre hosts é estabelecida antes de trocar dados. Um host é qualquer dispositivo em uma rede TCP/IP identificada por um endereço IP lógico.  
  
 O TCP fornece entrega de dados confiável e facilidade de uso. Especificamente, o TCP notifica o remetente da entrega de pacotes, garante que os pacotes sejam entregues na mesma ordem em que são enviados, retransmite os pacotes perdidos e garante que os pacotes de dados não sejam duplicados. Observe que essa entrega confiável se aplica entre dois nós TCP/IP e não é a mesma coisa que o *WS-ReliableMessaging*, que se aplica entre pontos de extremidade, independentemente de quantos nós intermediários eles podem incluir.  
  
 O transporte TCP do WCF é otimizado para o cenário em que ambas as extremidades da comunicação estão usando o WCF. Essa associação é a associação WCF mais rápida para cenários que envolvem a comunicação entre diferentes máquinas. As trocas de mensagens usam o <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> para transferência de mensagens otimizadas. O TCP fornece comunicação duplex e, portanto, pode ser usado para implementar contratos duplex, mesmo que o cliente esteja atrás da NAT (conversão de endereços de rede).  
  
### <a name="when-to-use-the-named-pipe-transport"></a>Quando usar o transporte de pipe nomeado  
 Um pipe nomeado é um objeto no kernel do sistema operacional Windows, como uma seção de memória compartilhada que os processos podem usar para comunicação. Um pipe nomeado tem um nome e pode ser usado para comunicação unidirecional ou duplex entre os processos em um único computador.  
  
 Quando a comunicação é necessária entre diferentes aplicativos WCF em um único computador e você deseja evitar qualquer comunicação de outro computador, use o transporte de pipes nomeados. Uma restrição adicional é que os processos em execução do Windows Área de Trabalho Remota podem ser restritos à mesma sessão do Windows Área de Trabalho Remota, a menos que tenham privilégios elevados.  
  
> [!WARNING]
> Ao usar o transporte de pipe nomeado com uma reserva de URL de curinga fraco em vários sites hospedados no IIS, o seguinte erro pode ocorrer: ocorreu um erro no serviço de ativação ' NetPipeActivator ' do protocolo ' net. pipe ' ao tentar escutar o site ' 2 ', portanto, o protocolo está desabilitado para o site temporariamente. Consulte a mensagem de exceção para obter mais detalhes. URL: WeakWildcard: net. pipe:/ \<machine name> /status: ConflictingRegistration exceção: nome do processo: ID do processo SMSvcHost: 1076 \  
  
## <a name="decision-points-for-choosing-a-transport"></a>Pontos de decisão para escolher um transporte  
 A tabela a seguir descreve os pontos de decisão comuns usados para escolher um transporte. Você deve considerar todos os atributos e transportes adicionais que se aplicam ao seu aplicativo. Identifique os atributos que são importantes para seu aplicativo, identifique os transportes que se associam de forma favorável a cada um dos seus atributos e selecione os transportes que funcionam melhor com seu conjunto de atributos.  
  
|Atributo|Descrição|Transportes favorecedos|  
|---------------|-----------------|------------------------|  
|Diagnósticos|O diagnóstico permite detectar automaticamente problemas de conectividade de transporte. Todos os transportes dão suporte à capacidade de enviar informações de back-up que descrevem a conectividade. No entanto, o WCF não inclui ferramentas de diagnóstico para investigar problemas de rede.|Nenhum|  
|Hosting|Todos os pontos de extremidade do WCF devem ser hospedados dentro de um aplicativo. O IIS 6,0 e versões anteriores dão suporte apenas a aplicativos de hospedagem que usam o transporte HTTP. No Windows Vista, o suporte é adicionado para hospedar todos os transportes do WCF, incluindo TCP e pipes nomeados. Para obter mais informações, consulte [hospedagem em serviços de informações da Internet](hosting-in-internet-information-services.md) e [hospedagem no serviço de ativação de processos do Windows](hosting-in-windows-process-activation-service.md).|HTTP|  
|Inspeção|A inspeção é a capacidade de extrair e processar informações de mensagens durante a transmissão. O protocolo HTTP separa o roteamento e controla as informações dos dados, facilitando a criação de ferramentas que inspecionam e analisam mensagens. Os transportes que são fáceis de inspecionar também podem exigir menos capacidade de processamento em dispositivos de rede. O nível de segurança usado afeta se as mensagens podem ser inspecionadas.|HTTP|  
|Latency|Latência é a quantidade mínima de tempo necessária para concluir uma troca de mensagens. Todas as operações de rede têm mais ou menos latência, dependendo da escolha do transporte. Usar a comunicação duplex ou unidirecional com um transporte cujo padrão de troca de mensagens nativa é solicitação-resposta, como HTTP, pode causar latência adicional devido à correlação forçada de mensagens. Nessa situação, considere usar um transporte cujo padrão de troca de mensagens nativa seja duplex, como TCP.|TCP, nomeado<br /><br /> Pipe|  
|Reach|O alcance de um transporte reflete o quão capaz o transporte está na conexão com outros sistemas. O transporte de pipe nomeado tem muito pouco alcance; Ele só pode se conectar a serviços em execução no mesmo computador. Os transportes TCP e HTTP têm um alcance excelente e podem penetrar em algumas configurações de NAT e firewall. Para obter mais informações, consulte [trabalhando com NATs e firewalls](working-with-nats-and-firewalls.md).|HTTP, TCP|  
|Segurança|A segurança é a capacidade de proteger mensagens durante a transferência, fornecendo confidencialidade, integridade ou autenticação. A confidencialidade protege uma mensagem de ser examinada, a integridade protege a modificação de uma mensagem e a autenticação fornece garantias sobre o remetente ou destinatário da mensagem.<br /><br /> O WCF dá suporte à segurança de transferência no nível da mensagem e no nível de transporte. A segurança da mensagem comporá com um transporte se o transporte der suporte a um modo de transferência em buffer. O suporte para segurança de transporte varia dependendo do transporte escolhido. Os transportes HTTP, TCP e pipe nomeado têm paridade razoável em seu suporte para segurança de transporte.|Tudo|  
|Produtividade|A taxa de transferência mede a quantidade de dados que podem ser transmitidos e processados em um período de tempo especificado. Assim como a latência, o transporte escolhido pode afetar a taxa de transferência para operações de serviço. Maximizar a taxa de transferência para um transporte requer minimizar a sobrecarga de transmitir conteúdo, bem como minimizar o tempo gasto aguardando a conclusão das trocas de mensagens. Os transportes TCP e pipe nomeado adicionam pouca sobrecarga ao corpo da mensagem e dão suporte a uma forma duplex nativa que reduz a espera por respostas de mensagens.|TCP, pipe nomeado|  
|Ferramentas|As ferramentas representam o suporte a aplicativos de terceiros para um protocolo para desenvolvimento, diagnóstico, hospedagem e outras atividades. Desenvolver ferramentas e software para trabalhar com o protocolo HTTP significa um investimento particularmente grande.|HTTP|  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.BasicHttpBinding>
- <xref:System.ServiceModel.WSHttpBinding>
- <xref:System.ServiceModel.WSDualHttpBinding>
- <xref:System.ServiceModel.WSFederationHttpBinding>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.NetTcpBinding>
- <xref:System.ServiceModel.Channels.TcpTransportBindingElement>
- <xref:System.ServiceModel.NetNamedPipeBinding>
- <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>
- [Associações](bindings.md)
- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
- [Criando associações definidas pelo usuário](../extending/creating-user-defined-bindings.md)
