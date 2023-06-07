---
title: Visão geral de associações do Windows Communication Foundation
description: Saiba mais sobre associações, que especificam como se conectar a um serviço WCF, incluindo elementos de uma associação e como especificar uma associação para um ponto de extremidade de serviço.
ms.date: 03/30/2017
helpviewer_keywords:
- bindings [WCF], overview
ms.assetid: cfb5842f-e0f9-4c56-a015-f2b33f258232
ms.openlocfilehash: da8050c4e9aeb111de3a54315b3650bcf09f23ed
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247708"
---
# <a name="windows-communication-foundation-bindings-overview"></a>Visão geral de associações do Windows Communication Foundation
Associações são objetos que são usados para especificar os detalhes de comunicação que são necessários para se conectar ao ponto de extremidade de um serviço de Windows Communication Foundation (WCF). Cada ponto de extremidade em um serviço WCF exige que uma associação seja bem especificada. Este tópico descreve os tipos de detalhes de comunicação que as associações definem, os elementos de uma associação, quais associações são incluídas no WCF e como uma associação pode ser especificada para um ponto de extremidade.  
  
## <a name="what-a-binding-defines"></a>O que uma associação define  
 As informações em uma associação podem ser muito básicas ou muito complexas. A associação mais básica especifica apenas o protocolo de transporte (como HTTP) que deve ser usado para se conectar ao ponto de extremidade. Em geral, as informações que uma associação contém sobre como se conectar a um ponto de extremidade se enquadram em uma das seguintes categorias:  
  
 **Protocolos**  
 Determina o mecanismo de segurança que está sendo usado: capacidade de mensagens confiável ou configurações de fluxo de contexto de transação.  
  
 **Codificação**  
 Determina a codificação de mensagem (por exemplo, texto ou binário).  
  
 **Transporte**  
 Determina o protocolo de transporte subjacente a ser usado (por exemplo, TCP ou HTTP).  
  
## <a name="the-elements-of-a-binding"></a>Os elementos de uma associação  
 Uma associação basicamente consiste em uma pilha ordenada de elementos de associação, cada um deles especifica parte das informações de comunicação necessárias para se conectar a um ponto de extremidade de serviço. As duas camadas mais baixas na pilha são obrigatórias. Na base da pilha está o elemento de ligação de transporte e logo acima é o elemento que contém as especificações de codificação de mensagem. Os elementos de associação opcionais que especificam os outros protocolos de comunicação são dispostos em camadas acima desses dois elementos necessários. Para obter mais informações sobre esses elementos de ligação e sua ordenação correta, consulte [associações personalizadas](./extending/custom-bindings.md).  
  
## <a name="system-provided-bindings"></a>Associações fornecidas pelo sistema  
 As informações em uma associação podem ser complexas e algumas configurações podem não ser compatíveis com outras. Por esse motivo, o WCF inclui um conjunto de associações fornecidas pelo sistema. Essas associações foram projetadas para abranger a maioria dos requisitos de aplicativo. As classes a seguir representam alguns exemplos de associações fornecidas pelo sistema:  
  
- <xref:System.ServiceModel.BasicHttpBinding>: Uma associação de protocolo HTTP adequada para se conectar a serviços Web que está em conformidade com a especificação de perfil básico WS-I (por exemplo, serviços baseados em serviços Web do ASP.NET).  
  
- <xref:System.ServiceModel.WSHttpBinding>: Uma associação interoperável adequada para se conectar a pontos de extremidade que estão em conformidade com os protocolos WS-*.  
  
- <xref:System.ServiceModel.NetNamedPipeBinding>: Usa o .NET Framework para se conectar a outros pontos de extremidade do WCF no mesmo computador.  
  
- <xref:System.ServiceModel.NetMsmqBinding>: Usa o .NET Framework para criar conexões de mensagens em fila com outros pontos de extremidade do WCF.  

- <xref:System.ServiceModel.NetTcpBinding>: Essa associação oferece um desempenho maior do que as associações HTTP e é ideal para uso em uma rede local.
  
 Para obter uma lista completa, com descrições de todas as associações fornecidas pelo WCF, consulte [associações fornecidas pelo sistema](system-provided-bindings.md).  
  
## <a name="using-your-own-bindings"></a>Usando suas próprias associações  
 Se nenhuma das associações fornecidas pelo sistema incluída tiver a combinação certa de recursos que um aplicativo de serviço requer, você poderá criar sua própria associação. Há duas maneiras de fazer isso. Você pode criar uma nova associação a partir de elementos de associação pré-existentes usando um <xref:System.ServiceModel.Channels.CustomBinding> objeto ou pode criar uma associação totalmente definida pelo usuário derivando da <xref:System.ServiceModel.Channels.Binding> associação. Para obter mais informações sobre como criar sua própria Associação usando essas duas abordagens, consulte [associações personalizadas](./extending/custom-bindings.md) e [Criando associações definidas pelo usuário](./extending/creating-user-defined-bindings.md).  
  
## <a name="using-bindings"></a>Usando associações  
 O uso de associações envolve duas etapas básicas:  
  
1. Selecione ou defina uma associação. O método mais fácil é escolher uma das associações fornecidas pelo sistema incluídas com o WCF e usá-la com suas configurações padrão. Você também pode escolher uma associação fornecida pelo sistema e redefinir seus valores de propriedade para atender às suas necessidades. Como alternativa, você pode criar uma associação personalizada ou uma associação definida pelo usuário para ter mais graus de controle e personalização.  
  
2. Crie um ponto de extremidade que usa a associação selecionada ou definida.  
  
## <a name="code-and-configuration"></a>Código e configuração  
 Você pode definir associações de duas maneiras: por meio de código ou por meio de configuração. Essas duas abordagens não dependem se você estiver usando uma associação fornecida pelo sistema ou uma associação personalizada. Em geral, o uso de código lhe dá controle total sobre a definição de uma associação em tempo de design. Usar a configuração, por outro lado, permite que um administrador do sistema ou o usuário de um serviço ou cliente do WCF altere os parâmetros de uma associação sem precisar recompilar o aplicativo de serviço. Essa flexibilidade geralmente é desejável porque não há como prever requisitos de máquina específicos nos quais um aplicativo WCF deve ser implantado. Manter a associação (e as informações de endereçamento) do código permite que elas sejam alteradas sem a necessidade de recompilação ou reimplantação do aplicativo. Observe que as associações definidas no código são criadas após associações especificadas na configuração, permitindo que as associações definidas pelo código substituam quaisquer associações definidas pela configuração.  
  
## <a name="see-also"></a>Veja também

- [Usando associações para configurar serviços e clientes](using-bindings-to-configure-services-and-clients.md)
