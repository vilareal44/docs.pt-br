---
title: Usando um cliente para acessar um serviço
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c8329832-bf66-4064-9034-bf39f153fc2d
ms.openlocfilehash: 001f30d7a0dde952a7d18bfbc50f2c3622287406
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84576545"
---
# <a name="accessing-services-using-a-client"></a>Usando um cliente para acessar um serviço
Os aplicativos cliente devem criar, configurar e usar objetos de cliente ou de canal do WCF para se comunicar com os serviços. O tópico [visão geral do cliente WCF](../wcf-client-overview.md) fornece uma visão geral dos objetos e das etapas envolvidas na criação de objetos de cliente e de canal básicos e no uso deles.  
  
 Este tópico fornece informações detalhadas sobre alguns dos problemas com aplicativos cliente e objetos de cliente e de canal que podem ser úteis, dependendo do seu cenário.  
  
## <a name="overview"></a>Visão geral  
 Este tópico descreve o comportamento e os problemas relacionados a:  
  
- Tempos de vida de canal e sessão.  
  
- Tratamento de exceções.  
  
- Entendendo os problemas de bloqueio.  
  
- Inicializando canais interativamente.  
  
### <a name="channel-and-session-lifetimes"></a>Tempos de vida de canal e sessão  
 Os aplicativos Windows Communication Foundation (WCF) incluem duas categorias de canais, datagrama e sessão.  
  
 Um canal de *datagrama* é um canal no qual todas as mensagens não estão correlacionadas. Com um canal de datagrama, se uma operação de entrada ou saída falhar, a próxima operação normalmente não será afetada e o mesmo canal poderá ser reutilizado. Por isso, os canais de datagrama normalmente não têm falha.  
  
 Os canais de *sessão* , no entanto, são canais com uma conexão com o outro ponto de extremidade. As mensagens em uma sessão em um lado são sempre correlacionadas com a mesma sessão do outro lado. Além disso, ambos os participantes em uma sessão devem concordar que os requisitos de sua conversa foram atendidos para que a sessão seja considerada com êxito. Se eles não concordarem, o canal de sessão poderá falhar.  
  
 Abra clientes explicitamente ou implicitamente chamando a primeira operação.  
  
> [!NOTE]
> A tentativa de detectar explicitamente os canais com falha na sessão não é normalmente útil, porque quando você é notificado depende da implementação da sessão. Por exemplo, como o <xref:System.ServiceModel.NetTcpBinding?displayProperty=nameWithType> (com a sessão confiável desabilitada) superfícies a sessão da conexão TCP, se você ouvir o <xref:System.ServiceModel.ICommunicationObject.Faulted?displayProperty=nameWithType> evento no serviço ou o cliente provavelmente será notificado rapidamente no caso de uma falha de rede. Mas as sessões confiáveis (estabelecidas por associações em que o <xref:System.ServiceModel.Channels.ReliableSessionBindingElement?displayProperty=nameWithType> está habilitado) foram projetadas para isolar serviços de pequenas falhas de rede. Se a sessão puder ser restabelecida em um período de tempo razoável, a mesma ligação — configurada para sessões confiáveis — pode não falhar até que a interrupção continue por um período de tempo maior.  
  
 A maioria das associações fornecidas pelo sistema (que expõem canais à camada de aplicativo) usa sessões por padrão, mas o não <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType> faz isso. Para obter mais informações, consulte [usando sessões](../using-sessions.md).  
  
### <a name="the-proper-use-of-sessions"></a>O uso adequado de sessões  
 As sessões fornecem uma maneira de saber se a troca de mensagens inteira está concluída e se ambos os lados consideraram que ela foi bem-sucedida. É recomendável que um aplicativo de chamada Abra o canal, use-o e feche o canal dentro de um bloco try. Se um canal de sessão estiver aberto e o <xref:System.ServiceModel.ICommunicationObject.Close%2A?displayProperty=nameWithType> método for chamado uma vez e essa chamada retornar com êxito, a sessão foi bem-sucedida. O êxito nesse caso significa que toda a entrega garante que a associação especificada foi atendida e o outro lado não chamou <xref:System.ServiceModel.ICommunicationObject.Abort%2A?displayProperty=nameWithType> no canal antes de chamar <xref:System.ServiceModel.ICommunicationObject.Close%2A> .  
  
 A seção a seguir fornece um exemplo dessa abordagem de cliente.  
  
### <a name="handling-exceptions"></a>Tratando exceções  
 A manipulação de exceções em aplicativos cliente é simples. Se um canal for aberto, usado e fechado dentro de um bloco try, a conversa terá êxito, a menos que uma exceção seja gerada. Normalmente, se uma exceção for lançada, a conversa será anulada.  
  
> [!NOTE]
> O uso da `using` instrução ( `Using` em Visual Basic) não é recomendado. Isso ocorre porque o final da `using` instrução pode causar exceções que podem mascarar outras exceções sobre as quais você talvez precise saber. Para obter mais informações, consulte [usar fechar e anular para liberar recursos do cliente WCF](../samples/use-close-abort-release-wcf-client-resources.md).  
  
 O exemplo de código a seguir mostra o padrão de cliente recomendado usando um bloco try/catch e não a `using` instrução.  
  
 [!code-csharp[FaultContractAttribute#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/client.cs#3)]
 [!code-vb[FaultContractAttribute#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/client.vb#3)]  
  
> [!NOTE]
> Verificar o valor da <xref:System.ServiceModel.ICommunicationObject.State%2A?displayProperty=nameWithType> propriedade é uma condição de corrida e não é recomendável determinar se deve reutilizar ou fechar um canal.  
  
 Os canais de datagrama nunca falham, mesmo se as exceções ocorrerem quando forem fechadas. Além disso, os clientes não-duplex que não se autenticam usando uma conversa segura normalmente lançam um <xref:System.ServiceModel.Security.MessageSecurityException?displayProperty=nameWithType> . No entanto, se o cliente duplex que usa uma conversa segura falhar ao autenticar, o cliente receberá um <xref:System.TimeoutException?displayProperty=nameWithType> em vez disso.  
  
 Para obter informações mais completas sobre como trabalhar com informações de erro no nível do aplicativo, consulte [especificando e manipulando falhas em contratos e serviços](../specifying-and-handling-faults-in-contracts-and-services.md). As [exceções esperadas](../samples/expected-exceptions.md) descrevem as exceções esperadas e mostram como tratá-las. Para obter mais informações sobre como lidar com erros ao desenvolver canais, consulte [tratando exceções e falhas](../extending/handling-exceptions-and-faults.md).  
  
### <a name="client-blocking-and-performance"></a>Desempenho e bloqueio de cliente  
 Quando um aplicativo chama de forma síncrona uma operação de solicitação-resposta, o cliente é bloqueado até que um valor de retorno seja recebido ou uma exceção (como um <xref:System.TimeoutException?displayProperty=nameWithType> ) seja gerada. Esse comportamento é semelhante ao comportamento local. Quando um aplicativo invoca de forma síncrona uma operação em um canal ou objeto de cliente WCF, o cliente não retorna até que a camada de canal possa gravar os dados na rede ou até que uma exceção seja gerada. E, embora o padrão de troca de mensagens unidirecionais (especificado marcando uma operação com <xref:System.ServiceModel.OperationContractAttribute.IsOneWay%2A?displayProperty=nameWithType> definido como `true` ) possa tornar alguns clientes mais responsivos, as operações unidirecionais também podem bloquear, dependendo da associação e quais mensagens já foram enviadas. Operações unidirecionais são apenas sobre a troca de mensagens, não mais nem menos. Para obter mais informações, consulte [Serviços unidirecionais](one-way-services.md).  
  
 Partes de dados grandes podem reduzir o processamento do cliente independentemente do padrão de troca de mensagens. Para entender como lidar com esses problemas, consulte [grandes dados e streaming](large-data-and-streaming.md).  
  
 Se seu aplicativo precisar fazer mais trabalho enquanto uma operação for concluída, você deverá criar um par de métodos assíncronos na interface de contrato de serviço que seu cliente WCF implementa. A maneira mais fácil de fazer isso é usar a `/async` opção na ferramenta de [Utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Para obter um exemplo, consulte [como: chamar operações de serviço de forma assíncrona](how-to-call-wcf-service-operations-asynchronously.md).  
  
 Para obter mais informações sobre como aumentar o desempenho do cliente, consulte [aplicativos cliente de camada intermediária](middle-tier-client-applications.md).  
  
### <a name="enabling-the-user-to-select-credentials-dynamically"></a>Habilitando o usuário a selecionar as credenciais dinamicamente  
 A <xref:System.ServiceModel.Dispatcher.IInteractiveChannelInitializer> interface permite que os aplicativos exibam uma interface do usuário que permite ao usuário escolher credenciais com as quais um canal é criado antes do início dos temporizadores de tempo limite.  
  
 Os desenvolvedores de aplicativos podem fazer uso de um inserido de <xref:System.ServiceModel.Dispatcher.IInteractiveChannelInitializer> duas maneiras. O aplicativo cliente pode chamar <xref:System.ServiceModel.ClientBase%601.DisplayInitializationUI%2A?displayProperty=nameWithType> ou <xref:System.ServiceModel.IClientChannel.DisplayInitializationUI%2A?displayProperty=nameWithType> (ou uma versão assíncrona) antes de abrir o canal (a *abordagem explícita* ) ou chamar a primeira operação (a abordagem *implícita* ).  
  
 Se estiver usando a abordagem implícita, o aplicativo deverá chamar a primeira operação em <xref:System.ServiceModel.ClientBase%601> uma <xref:System.ServiceModel.IClientChannel> extensão ou. Se ele chamar algo diferente da primeira operação, uma exceção será lançada.  
  
 Se estiver usando a abordagem explícita, o aplicativo deverá executar as seguintes etapas na ordem:  
  
1. Chame uma <xref:System.ServiceModel.ClientBase%601.DisplayInitializationUI%2A?displayProperty=nameWithType> ou <xref:System.ServiceModel.IClientChannel.DisplayInitializationUI%2A?displayProperty=nameWithType> (ou uma versão assíncrona).  
  
2. Quando os inicializadores forem retornados, chame o <xref:System.ServiceModel.ICommunicationObject.Open%2A> método no <xref:System.ServiceModel.IClientChannel> objeto ou no <xref:System.ServiceModel.IClientChannel> objeto retornado da <xref:System.ServiceModel.ClientBase%601.InnerChannel%2A?displayProperty=nameWithType> propriedade.  
  
3. Chamar as operações.  
  
 É recomendável que os aplicativos de qualidade de produção controlem o processo de interface do usuário, adotando a abordagem explícita.  
  
 Os aplicativos que usam a abordagem implícita invocam os inicializadores de interface do usuário, mas se o usuário do aplicativo não responder dentro do período de tempo limite de envio da associação, uma exceção será lançada quando a interface do usuário retornar.  
  
## <a name="see-also"></a>Consulte também

- [Serviços duplex](duplex-services.md)
- [Como acessar os serviços com contratos de resposta/solicitação e unidirecionais](how-to-access-wcf-services-with-one-way-and-request-reply-contracts.md)
- [Como acessar serviços com um contrato Duplex](how-to-access-services-with-a-duplex-contract.md)
- [Como acessar um serviço WSE 3.0](how-to-access-a-wse-3-0-service-with-a-wcf-client.md)
- [Como usar o ChannelFactory](how-to-use-the-channelfactory.md)
- [Como chamar operações de serviço de maneira assíncrona](how-to-call-wcf-service-operations-asynchronously.md)
- [Aplicativos cliente de camada intermediária](middle-tier-client-applications.md)
