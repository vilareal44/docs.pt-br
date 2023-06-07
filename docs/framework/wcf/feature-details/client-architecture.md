---
title: Arquitetura do cliente
ms.date: 03/30/2017
ms.assetid: 02624403-0d77-41cb-9a86-ab55e98c7966
ms.openlocfilehash: c873368b82551312d203eb28d208eb6e3f50c89b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586995"
---
# <a name="client-architecture"></a>Arquitetura do cliente
Os aplicativos usam objetos de cliente Windows Communication Foundation (WCF) para invocar operações de serviço. Este tópico discute os objetos de cliente WCF, os canais de cliente WCF e suas relações com a arquitetura de canal subjacente. Para obter uma visão geral básica dos objetos de cliente WCF, consulte [visão geral do cliente WCF](../wcf-client-overview.md). Para obter mais informações sobre a camada de canal, consulte [estendendo a camada de canal](../extending/extending-the-channel-layer.md).  
  
## <a name="overview"></a>Visão geral  
 O tempo de execução do modelo de serviço cria clientes WCF, que são compostos do seguinte:  
  
- Uma implementação de cliente gerada automaticamente de um contrato de serviço, que transforma as chamadas do código do aplicativo em mensagens de saída e transforma as mensagens de resposta em parâmetros de saída e retorna valores que seu aplicativo pode recuperar.  
  
- Uma implementação de uma interface de controle ( <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> ) que agrupa várias interfaces e fornece acesso à funcionalidade de controle, mais notavelmente a capacidade de fechar a sessão do cliente e descartar o canal.  
  
- Um canal de cliente criado com base nas definições de configuração especificadas pela associação usada.  
  
 Os aplicativos podem criar tais clientes sob demanda, seja por meio de um <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType> ou criando uma instância de uma <xref:System.ServiceModel.ClientBase%601> classe derivada, pois ela é gerada pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Essas classes de cliente prontas encapsulam e delegam a uma implementação de canal de cliente criada dinamicamente por um <xref:System.ServiceModel.ChannelFactory> . Portanto, o canal do cliente e a fábrica de canais que os produzem são o ponto focal de interesse desta discussão.  
  
## <a name="client-objects-and-client-channels"></a>Objetos de cliente e canais de cliente  
 A interface base dos clientes WCF é a <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> interface, que expõe a funcionalidade principal do cliente, bem como a funcionalidade básica do objeto de comunicação do <xref:System.ServiceModel.ICommunicationObject?displayProperty=nameWithType> , a funcionalidade de contexto do <xref:System.ServiceModel.IContextChannel?displayProperty=nameWithType> e o comportamento extensível do <xref:System.ServiceModel.IExtensibleObject%601?displayProperty=nameWithType> .  
  
 A <xref:System.ServiceModel.IClientChannel> interface, no entanto, não define um contrato de serviço em si. Eles são declarados pela interface do contrato de serviço (normalmente gerada a partir de metadados de serviço usando uma ferramenta como a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)). Os tipos de cliente WCF estendem <xref:System.ServiceModel.IClientChannel> e a interface de contrato de serviço de destino para permitir que aplicativos chamem operações diretamente e também tenham acesso à funcionalidade de tempo de execução do lado do cliente. A criação de um cliente WCF fornece <xref:System.ServiceModel.ChannelFactory?displayProperty=nameWithType> aos objetos WCF as informações necessárias para criar um tempo de execução que pode se conectar e interagir com o ponto de extremidade de serviço configurado.  
  
 Conforme mencionado anteriormente, os dois tipos de cliente do WCF devem ser configurados antes que você possa usá-los. Os tipos de cliente mais simples do WCF são objetos que derivam de <xref:System.ServiceModel.ClientBase%601> (ou <xref:System.ServiceModel.DuplexClientBase%601> se o contrato de serviço é um contrato duplex). Você pode criar esses tipos usando um construtor, configurado programaticamente, ou usando um arquivo de configuração e, em seguida, chamado diretamente para invocar operações de serviço. Para obter uma visão geral básica dos <xref:System.ServiceModel.ClientBase%601> objetos, consulte [visão geral do cliente WCF](../wcf-client-overview.md).  
  
 O segundo tipo é gerado em tempo de execução de uma chamada para o <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A> método. Os aplicativos preocupados com o controle rígido das especificidades de comunicação normalmente usam esse tipo de cliente, chamado de *objeto de canal de cliente*, porque permite uma interação mais direta do que o sistema subjacente de tempo de execução do cliente e do canal.  
  
## <a name="channel-factories"></a>Fábricas de canal  
 A classe que é responsável pela criação do tempo de execução subjacente que dá suporte a invocações de cliente é a <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> classe. Os objetos cliente do WCF e os objetos de canal do cliente WCF usam um <xref:System.ServiceModel.ChannelFactory%601> objeto para criar instâncias; o <xref:System.ServiceModel.ClientBase%601> objeto cliente derivado encapsula a manipulação da fábrica de canais, mas, para vários cenários, é perfeitamente razoável usar a fábrica de canais diretamente. O cenário comum para isso é se você deseja criar repetidamente novos canais de cliente de uma fábrica existente. Se você estiver usando um objeto de cliente, poderá obter a fábrica de canais subjacente de um objeto de cliente do WCF chamando a <xref:System.ServiceModel.ClientBase%601.ChannelFactory%2A?displayProperty=nameWithType> propriedade.  
  
 O importante a ser lembrado sobre fábricas de canal é que elas criam novas instâncias de canais de cliente para a configuração fornecida a eles antes da chamada <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType> . Depois de chamar <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A> (ou <xref:System.ServiceModel.ClientBase%601.Open%2A?displayProperty=nameWithType> , <xref:System.ServiceModel.ClientBase%601.CreateChannel%2A?displayProperty=nameWithType> ou qualquer operação em um objeto de cliente WCF), você não poderá modificar a fábrica de canais e esperar obter canais para diferentes instâncias de serviço, mesmo se você estiver apenas alterando o endereço do ponto de extremidade de destino. Se você quiser criar um canal de cliente ou objeto de cliente com uma configuração diferente, deverá criar primeiro uma nova fábrica de canal.  
  
 Para obter mais informações sobre vários problemas usando objetos de cliente WCF e canais de cliente WCF, consulte [acessando serviços usando um cliente WCF](accessing-services-using-a-client.md).  
  
 As duas seções a seguir descrevem a criação e o uso de objetos de canal do cliente WCF.  
  
#### <a name="creating-a-new-wcf-client-channel-object"></a>Criando um novo objeto de canal do cliente WCF  
 Para ilustrar o uso de um canal de cliente, suponha que o contrato de serviço a seguir foi gerado.  
  
 [!code-csharp[C_GeneratedCodeFiles#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#12)]  
  
 Para se conectar a um `ISampleService` serviço, use a interface de contrato gerada diretamente com uma fábrica de canais ( <xref:System.ServiceModel.ChannelFactory%601> ). Depois de criar e configurar uma fábrica de canais para um contrato específico, você pode chamar o <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A> método para retornar os objetos de canal do cliente que você pode usar para se comunicar com um `ISampleService` serviço.  
  
 Ao usar a <xref:System.ServiceModel.ChannelFactory%601> classe com uma interface de contrato de serviço, você deve converter na <xref:System.ServiceModel.IClientChannel> interface para abrir, fechar ou abortar o canal explicitamente. Para facilitar o trabalho com o, a ferramenta svcutil. exe também gera uma interface auxiliar que implementa a interface do contrato de serviço e <xref:System.ServiceModel.IClientChannel> para permitir que você interaja com a infraestrutura de canal do cliente sem precisar fazer a conversão. O código a seguir mostra a definição de um canal cliente auxiliar que implementa o contrato de serviço anterior.  
  
 [!code-csharp[C_GeneratedCodeFiles#13](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#13)]  
  
#### <a name="creating-a-new-wcf-client-channel-object"></a>Criando um novo objeto de canal do cliente WCF  
 Para usar um canal de cliente para se conectar a um `ISampleService` serviço, use a interface de contrato gerada (ou a versão auxiliar) diretamente com uma fábrica de canais, passando o tipo da interface de contrato como o parâmetro de tipo. Depois que uma fábrica de canais para um determinado contrato tiver sido criada e configurada, você poderá chamar o <xref:System.ServiceModel.ChannelFactory%601.CreateChannel%2A?displayProperty=nameWithType> método para retornar os objetos de canal do cliente que você pode usar para se comunicar com um `ISampleService` serviço.  
  
 Quando criados, os objetos de canal do cliente são implementados <xref:System.ServiceModel.IClientChannel> e a interface do contrato. Portanto, você pode usá-los diretamente para chamar operações que interagem com um serviço que dá suporte a esse contrato.  
  
 A diferença entre usar objetos de cliente e objetos de canal de cliente é meramente um dos controles e facilidade de uso para os desenvolvedores. Muitos desenvolvedores que estão confortáveis trabalhando com classes e objetos preferem usar o objeto de cliente do WCF em vez do canal do cliente do WCF.  
  
 Para obter um exemplo, consulte [como: usar a ChannelFactory](how-to-use-the-channelfactory.md).
