---
title: Como criar um contrato duplex
description: Saiba como criar um contrato duplex, que permite que clientes e servidores WCF se comuniquem entre si de forma independente. O pode iniciar chamadas para o outro.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- duplex contracts [WCF]
ms.assetid: 500a75b6-998a-47d5-8e3b-24e3aba2a434
ms.openlocfilehash: 9320e5b36b8faba3602fbe1df1b95c05dcc7fa7e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247085"
---
# <a name="how-to-create-a-duplex-contract"></a>Como criar um contrato duplex
Este tópico mostra as etapas básicas para criar métodos que usam um contrato duplex (bidirecional). Um contrato duplex permite que os clientes e os servidores se comuniquem entre si independentemente, de modo que qualquer um possa iniciar chamadas para o outro. O contrato duplex é um dos três padrões de mensagem disponíveis para os serviços Windows Communication Foundation (WCF). Os outros padrões de duas mensagens são unidirecionais e solicitação-resposta. Um contrato duplex consiste de dois contratos unidirecionais entre o cliente e o servidor e não exige que as chamadas de método sejam correlacionadas. Use esse tipo de contrato quando o serviço tiver que consultar o cliente para obter mais informações ou explicitamente gerar eventos no cliente. Para obter mais informações sobre como criar um aplicativo cliente para um contrato duplex, consulte [como acessar serviços com um contrato duplex](how-to-access-services-with-a-duplex-contract.md). Para obter um exemplo funcional, consulte o exemplo de [duplex](../samples/duplex.md) .  
  
### <a name="to-create-a-duplex-contract"></a>Para criar um contrato duplex  
  
1. Crie a interface que compõem o lado do servidor do contrato duplex.  
  
2. Aplique a classe <xref:System.ServiceModel.ServiceContractAttribute> à interface.  
  
     [!code-csharp[S_WS_DualHttp#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#3)]
     [!code-vb[S_WS_DualHttp#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#3)]  
  
3. Declare as assinaturas de método na interface.  
  
4. Aplique a classe <xref:System.ServiceModel.OperationContractAttribute> a cada assinatura de método que deve ser parte do contrato público.  
  
5. Crie a interface de retorno de chamada que define o conjunto de operações que o serviço pode chamar no cliente.  
  
     [!code-csharp[S_WS_DualHttp#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#4)]
     [!code-vb[S_WS_DualHttp#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#4)]  
  
6. Declare as assinaturas de método na interface de retorno de chamada.  
  
7. Aplique a classe <xref:System.ServiceModel.OperationContractAttribute> a cada assinatura de método que deve ser parte do contrato público.  
  
8. Vincular as duas interfaces em um contrato duplex definindo a propriedade <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A> na interface primária para o tipo da interface de retorno de chamada.  
  
### <a name="to-call-methods-on-the-client"></a>Para chamar métodos no cliente  
  
1. Na implementação de serviço do contrato primário, declare uma variável para a interface de retorno de chamada.  
  
2. Defina a variável para a referência de objeto retornada pelo método <xref:System.ServiceModel.OperationContext.GetCallbackChannel%2A> da classe <xref:System.ServiceModel.OperationContext>.  
  
     [!code-csharp[S_WS_DualHttp#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#1)]
     [!code-vb[S_WS_DualHttp#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#1)]  
  
     [!code-csharp[S_WS_DualHttp#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#2)]
     [!code-vb[S_WS_DualHttp#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#2)]  
  
3. Chame os métodos definidos pela interface de retorno de chamada.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra a comunicação duplex. O contrato do serviço contém operações de serviço para avançar e voltar. O contrato do cliente contém uma operação de serviço para relatar sua posição.  
  
 [!code-csharp[S_WS_DualHttp#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#5)]
 [!code-vb[S_WS_DualHttp#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#5)]  
  
- Aplicar os atributos <xref:System.ServiceModel.ServiceContractAttribute> e <xref:System.ServiceModel.OperationContractAttribute> permite a geração automática de definições do contrato de serviço na linguagem WSDL.  
  
- Use a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para recuperar o documento WSDL e o código (opcional) e a configuração de um cliente.  
  
- Os pontos de extremidade que expõem serviços duplex devem ser protegidos. Quando um serviço recebe uma mensagem duplex, ele olha o ReplyTo na mensagem de entrada para determinar para onde enviar a resposta. Se o canal não estiver protegido, um cliente não confiável poderá enviar uma mensagem mal-intencionada com um ReplyTo do computador de destino, resultando em uma negação de serviço do computador de destino. Com mensagens normais de solicitação-resposta, isso não é um problema, pois o ReplyTo é ignorado e a resposta é enviada no canal no qual a mensagem original chegou.  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.ServiceContractAttribute>
- <xref:System.ServiceModel.OperationContractAttribute>
- [Como acessar serviços com um contrato Duplex](how-to-access-services-with-a-duplex-contract.md)
- [Duplex](../samples/duplex.md)
- [Serviços de design e implantação](../designing-and-implementing-services.md)
- [Como definir um contrato de serviço](../how-to-define-a-wcf-service-contract.md)
- [Sessão](../samples/session.md)
