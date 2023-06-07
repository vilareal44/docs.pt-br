---
title: Usando associações para configurar serviços e clientes
description: As associações contêm informações de configuração usadas por clientes ou serviços do WFC. Saiba como definir associações e como especificar uma associação para um ponto de extremidade de serviço.
ms.date: 03/30/2017
helpviewer_keywords:
- bindings [WCF], using
ms.assetid: c39479c3-0766-4a17-ba4c-97a74607f392
ms.openlocfilehash: 60db37d4381191314e9d5588dd61015a7078e84d
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245928"
---
# <a name="using-bindings-to-configure-services-and-clients"></a>Usando associações para configurar serviços e clientes
Associações são objetos que especificam os detalhes de comunicação necessários para se conectar a um ponto de extremidade. Mais especificamente, as associações contêm informações de configuração usadas para criar o tempo de execução do cliente ou do serviço definindo as especificidades de transportes, formatos de transmissão (codificação de mensagem) e protocolos a serem usados para o respectivo ponto de extremidade ou canal de cliente. Para criar um serviço de Windows Communication Foundation funcional (WCF), cada ponto de extremidade no serviço requer uma associação. Este tópico explica o que são associações, como elas são definidas e como uma associação específica é especificada para um ponto de extremidade.  
  
## <a name="what-a-binding-defines"></a>O que uma associação define  
 As informações em uma associação podem ser muito básicas ou muito complexas. A associação mais básica especifica apenas o protocolo de transporte (como HTTP) que deve ser usado para se conectar ao ponto de extremidade. Em geral, as informações que uma associação contém sobre como se conectar a um ponto de extremidade se enquadram em uma das categorias na tabela a seguir.  
  
 Protocolos  
 Determina o mecanismo de segurança que está sendo usado, o recurso de mensagens confiável ou as configurações de fluxo de contexto de transação.  
  
 Transporte  
 Determina o protocolo de transporte subjacente a ser usado (por exemplo, TCP ou HTTP).  
  
 Codificação  
 Determina a codificação de mensagem, por exemplo, texto/XML, binário ou MTOM (mecanismo de otimização de transmissão de mensagens), que determina como as mensagens são representadas como fluxos de bytes na transmissão.  
  
## <a name="system-provided-bindings"></a>Associações fornecidas pelo sistema  
 O WCF inclui um conjunto de associações fornecidas pelo sistema que são projetadas para abranger a maioria dos requisitos e cenários de aplicativos. As classes a seguir representam alguns exemplos de associações fornecidas pelo sistema:  
  
- <xref:System.ServiceModel.BasicHttpBinding>: Uma associação de protocolo HTTP adequada para se conectar a serviços Web que está em conformidade com a especificação WS-I Basic Profile 1,1 (por exemplo, serviços da ASP.NET Web Services [ASMX]).  
  
- <xref:System.ServiceModel.WSHttpBinding>: Uma associação de protocolo HTTP adequada para se conectar a pontos de extremidade que estão em conformidade com os protocolos de especificações de serviços da Web.  
  
- <xref:System.ServiceModel.NetNamedPipeBinding>: Usa a codificação binária do .NET e as tecnologias de enquadramento em conjunto com o transporte de pipe nomeado do Windows para se conectar a outros pontos de extremidade do WCF no mesmo computador.  
  
- <xref:System.ServiceModel.NetMsmqBinding>: Usa a codificação binária do .NET e as tecnologias de enquadramento em conjunto com o enfileiramento de mensagens (também conhecido como MSMQ) para criar conexões de mensagens em fila com outros pontos de extremidade do WCF.  
  
 Para obter uma lista completa de associações fornecidas pelo sistema, com descrições, consulte [associações fornecidas pelo sistema](system-provided-bindings.md).  
  
## <a name="custom-bindings"></a>Associações personalizadas  
 Se a coleção de associação fornecida pelo sistema não tiver a combinação correta de recursos que um aplicativo de serviço requer, você poderá criar uma <xref:System.ServiceModel.Channels.CustomBinding> associação. Para obter mais informações sobre os elementos de uma <xref:System.ServiceModel.Channels.CustomBinding> associação, consulte [\<customBinding>](../configure-apps/file-schema/wcf/custombinding.md) e [associações personalizadas](./extending/custom-bindings.md).  
  
## <a name="using-bindings"></a>Usando associações  
 O uso de associações envolve duas etapas básicas:  
  
1. Selecione ou defina uma associação. O método mais fácil é escolher uma das associações fornecidas pelo sistema e usar suas configurações padrão. Você também pode escolher uma associação fornecida pelo sistema e redefinir seus valores de propriedade para atender às suas necessidades. Como alternativa, você pode criar uma associação personalizada e definir todas as propriedades conforme necessário.  
  
2. Crie um ponto de extremidade que use essa associação.  
  
## <a name="code-and-configuration"></a>Código e configuração  
 Você pode definir ou configurar associações por meio de código ou configuração. Essas duas abordagens são independentes do tipo de associação usado, por exemplo, se você estiver usando uma associação ou um fornecido pelo sistema <xref:System.ServiceModel.Channels.CustomBinding> . Em geral, o uso de código lhe dá controle total sobre a definição de uma associação quando você compila. Usar a configuração, por outro lado, permite que um administrador do sistema ou o usuário de um serviço ou cliente do WCF altere os parâmetros das associações. Essa flexibilidade geralmente é desejável porque não há como prever os requisitos de máquina específicos e as condições de rede nas quais um aplicativo WCF deve ser implantado. Separar as informações de associação (e endereçamento) do código permite que os administradores alterem os detalhes de associação sem precisar recompilar ou reimplantar o aplicativo. Observe que, se a associação for definida no código, ela substituirá as definições baseadas em configuração feitas no arquivo de configuração. Para obter exemplos dessas abordagens, consulte os seguintes tópicos:  
  
- [Como hospedar um serviço WCF em um aplicativo gerenciado](how-to-host-a-wcf-service-in-a-managed-application.md) fornece um exemplo de criação de uma associação no código.  
  
- [Tutorial: criar um cliente Windows Communication Foundation](how-to-create-a-wcf-client.md) fornece um exemplo de como criar um cliente usando a configuração do.  
  
## <a name="see-also"></a>Veja também

- [Visão geral de criação de ponto de extremidade](endpoint-creation-overview.md)
- [Como especificar uma associação de serviço na configuração](how-to-specify-a-service-binding-in-configuration.md)
- [Como especificar uma associação de serviço no código](how-to-specify-a-service-binding-in-code.md)
- [Como especificar uma associação de cliente na configuração](how-to-specify-a-client-binding-in-configuration.md)
- [Como especificar uma associação de cliente no código](how-to-specify-a-client-binding-in-code.md)
