---
title: Como criar um serviço Web HTTP WCF básico
description: Saiba como criar um serviço que expõe um ponto de extremidade da Web no WCF. Os pontos de extremidade da Web enviam dados usando XML ou JSON. Não há envelope SOAP.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 877662d3-d372-4e08-b417-51f66a0095cd
ms.openlocfilehash: 7481367f27d973ba809dff5ca1c4a4f168fbbb98
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247098"
---
# <a name="how-to-create-a-basic-wcf-web-http-service"></a>Como criar um serviço Web HTTP WCF básico

Windows Communication Foundation (WCF) permite que você crie um serviço que expõe um ponto de extremidade da Web. Os pontos de extremidade Web enviam dados por XML ou JSON; não há envelope SOAP. Este tópico demonstra como expor um ponto de extremidade desse tipo.

> [!NOTE]
> A única maneira de proteger um ponto de extremidade Web é exibi-lo via HTTPS, usando segurança de transporte. Quando a segurança baseada em mensagem é usada, as informações de segurança são geralmente colocadas em cabeçalhos SOAP e, como as mensagens enviadas a pontos de extremidade não SOAP não contêm envelopes SOAP, não há lugar para colocar informações de segurança. Assim, é necessário confiar na segurança de transporte.

## <a name="to-create-a-web-endpoint"></a>Para criar um ponto de extremidade Web

1. Defina um contrato de serviço usando uma interface marcada com os atributos <xref:System.ServiceModel.ServiceContractAttribute>, <xref:System.ServiceModel.Web.WebInvokeAttribute> e <xref:System.ServiceModel.Web.WebGetAttribute>.

     [!code-csharp[htBasicService#0](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#0)]
     [!code-vb[htBasicService#0](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#0)]

    > [!NOTE]
    > Por padrão, <xref:System.ServiceModel.Web.WebInvokeAttribute> mapeia chamadas POST para a operação. É possível, entretanto, especificar o método HTTP (por exemplo, HEAD, PUT ou DELETE) para mapear a operação especificando um parâmetro "method=". <xref:System.ServiceModel.Web.WebGetAttribute> não tem um parâmetro "method=" e só mapeia chamadas GET para a operação de serviço.

2. Implemente o contrato de serviço.

     [!code-csharp[htBasicService#1](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#1)]
     [!code-vb[htBasicService#1](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#1)]

## <a name="to-host-the-service"></a>Para hospedar o serviço

1. Crie um objeto <xref:System.ServiceModel.Web.WebServiceHost>.

     [!code-csharp[htBasicService#2](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#2)]
     [!code-vb[htBasicService#2](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#2)]

2. Adicione uma classe <xref:System.ServiceModel.Description.ServiceEndpoint> com <xref:System.ServiceModel.Description.WebHttpBehavior>.

     [!code-csharp[htBasicService#3](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#3)]
     [!code-vb[htBasicService#3](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#3)]

    > [!NOTE]
    > Se você não adicionar um ponto de extremidade, <xref:System.ServiceModel.Web.WebServiceHost> criará automaticamente um ponto de extremidade padrão. <xref:System.ServiceModel.Web.WebServiceHost> também adiciona <xref:System.ServiceModel.Description.WebHttpBehavior> e desabilita a página da ajuda HTTP e a funcionalidade GET da linguagem WSDL para que o ponto de extremidade de metadados não interfira no ponto de extremidade HTTP padrão.
    >
    >  A adição de um ponto de extremidade não SOAP com uma URL de "" gera um comportamento inesperado quando há uma tentativa de chamar uma operação no ponto de extremidade. O motivo disso é que o URI de escuta do ponto de extremidade é o mesmo que o URI da página de ajuda (a página que é exibida quando você navega até o endereço base de um serviço WCF).

     Você pode executar uma das seguintes ações para evitar que isso ocorra:

    - Especifique sempre um URI que não esteja em branco para um ponto de extremidade não SOAP.

    - Desative a página de ajuda. Isso pode ser feito com o seguinte código:

     [!code-csharp[htBasicService#4](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/snippets.cs#4)]
     [!code-vb[htBasicService#4](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/snippets.vb#4)]

3. Abra o host do serviço e aguarde até que o usuário pressione ENTER.

     [!code-csharp[htBasicService#5](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/snippets.cs#5)]
     [!code-vb[htBasicService#5](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/snippets.vb#5)]

     Este exemplo demonstra como hospedar um serviço estilo Web com um aplicativo de console. Também é possível hospedar esse tipo de serviço no IIS. Para fazer isso, especifique a classe <xref:System.ServiceModel.Activation.WebServiceHostFactory> em um arquivo .svc como demonstra o código a seguir.

    ```text
    <%ServiceHost
        language=c#
        Debug="true"
        Service="Microsoft.Samples.Service"
        Factory=System.ServiceModel.Activation.WebServiceHostFactory%>
    ```

## <a name="to-call-service-operations-mapped-to-get-in-internet-explorer"></a>Para chamar operações de serviço mapeadas para GET no Internet Explorer

1. Abra o Internet Explorer e digite " `http://localhost:8000/EchoWithGet?s=Hello, world!` " e pressione Enter. A URL contém o endereço base do serviço ( `http://localhost:8000/` ), o endereço relativo do ponto de extremidade (""), a operação de serviço a ser chamada ("EchoWithGet") e um ponto de interrogação seguido por uma lista de parâmetros nomeados separados por um e comercial (&).

## <a name="to-call-service-operations-in-code"></a>Para chamar operações de serviço no código

1. Crie uma instância de <xref:System.ServiceModel.ChannelFactory%601> em um bloco `using`.

     [!code-csharp[htBasicService#6](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#6)]
     [!code-vb[htBasicService#6](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#6)]

2. Adicione <xref:System.ServiceModel.Description.WebHttpBehavior> ao ponto de extremidade que <xref:System.ServiceModel.ChannelFactory%601> chama.

     [!code-csharp[htBasicService#7](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#7)]
     [!code-vb[htBasicService#7](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#7)]

3. Crie o canal e chame o serviço.

     [!code-csharp[htBasicService#8](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#8)]
     [!code-vb[htBasicService#8](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#8)]

4. Feche a classe <xref:System.ServiceModel.Web.WebServiceHost>.

     [!code-csharp[htBasicService#9](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#9)]
     [!code-vb[htBasicService#9](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#9)]

## <a name="example"></a>Exemplo

A seguir está a listagem completa de códigos deste exemplo.

[!code-csharp[htBasicService#10](~/samples/snippets/csharp/VS_Snippets_CFX/htbasicservice/cs/service.cs#10)]
[!code-vb[htBasicService#10](~/samples/snippets/visualbasic/VS_Snippets_CFX/htbasicservice/vb/service.vb#10)]

## <a name="compiling-the-code"></a>Compilando o código

Ao compilar Service.cs, faça referência à System.ServiceModel.dll e à System.ServiceModel.Web.dll.

## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
- <xref:System.ServiceModel.Web.WebInvokeAttribute>
- <xref:System.ServiceModel.Web.WebServiceHost>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- [Modelo de programação WCF Web HTTP](wcf-web-http-programming-model.md)
