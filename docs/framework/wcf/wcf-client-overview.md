---
title: Visão geral do cliente WCF
description: Saiba mais sobre o que os aplicativos cliente fazem, como configurar, criar e usar um cliente WCF e como proteger aplicativos cliente.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- clients [WCF], architecture
ms.assetid: f60d9bc5-8ade-4471-8ecf-5a07a936c82d
ms.openlocfilehash: c66541f95d04373a9a29fafe58528872335936c4
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245902"
---
# <a name="wcf-client-overview"></a>Visão geral do cliente WCF

Esta seção descreve o que os aplicativos cliente fazem, como configurar, criar e usar um cliente Windows Communication Foundation (WCF) e como proteger aplicativos cliente.  
  
## <a name="using-wcf-client-objects"></a>Usando objetos de cliente WCF  
 Um aplicativo cliente é um aplicativo gerenciado que usa um cliente WCF para se comunicar com outro aplicativo. A criação de um aplicativo cliente para um serviço WCF requer as seguintes etapas:  
  
1. Obter o contrato de serviço, as associações e as informações de endereço de um ponto de extremidade de serviço.  
  
2. Crie um cliente WCF usando essas informações.  
  
3. Chamar as operações.  
  
4. Feche o objeto cliente do WCF.  
  
As seções a seguir abordam essas etapas e fazem breves introduções às seguintes questões:  
  
- Tratamento de erros.  
  
- Configurando e protegendo clientes.  
  
- Criando objetos de retorno de chamada para serviços duplex.  
  
- Chamando serviços de modo assíncrono.  
  
- Chamando serviços por meio de canais de cliente.  
  
## <a name="obtain-the-service-contract-bindings-and-addresses"></a>Obter o contrato de serviço, as associações e os endereços  
 No WCF, os serviços e os clientes modelam contratos usando atributos, interfaces e métodos gerenciados. Para se conectar a um serviço em um aplicativo cliente, é necessário obter as informações de tipo do contrato de serviço. Normalmente, você obtém informações de tipo para o contrato de serviço usando a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md). O utilitário baixa os metadados do serviço, converte-os em um arquivo de código-fonte gerenciado no idioma de sua escolha e cria um arquivo de configuração de aplicativo cliente que você pode usar para configurar o objeto do cliente WCF. Por exemplo, se você pretende criar um objeto de cliente WCF para invocar um `MyCalculatorService` , e souber que os metadados para esse serviço são publicados em `http://computerName/MyCalculatorService/Service.svc?wsdl` , o exemplo de código a seguir mostra como usar Svcutil.exe para obter um `ClientCode.vb` arquivo que contém o contrato de serviço em código gerenciado.  
  
```console  
svcutil /language:vb /out:ClientCode.vb /config:app.config http://computerName/MyCalculatorService/Service.svc?wsdl  
```  
  
 Você pode compilar esse código de contrato no aplicativo cliente ou em outro assembly que o aplicativo cliente pode usar para criar um objeto de cliente WCF. Você pode usar o arquivo de configuração para configurar o objeto de cliente para se conectar corretamente ao serviço.  
  
 Para obter um exemplo desse processo, consulte [como: criar um cliente](how-to-create-a-wcf-client.md). Para obter informações mais completas sobre contratos, consulte [contratos](./feature-details/contracts.md).  
  
## <a name="create-a-wcf-client-object"></a>Criar um novo objeto de cliente WCF  
 Um cliente WCF é um objeto local que representa um serviço WCF em um formulário que o cliente pode usar para se comunicar com o serviço remoto. Os tipos de cliente do WCF implementam o contrato de serviço de destino, portanto, ao criar um e configurá-lo, você pode usar o objeto de cliente diretamente para invocar operações de serviço. O tempo de execução do WCF converte as chamadas de método em mensagens, as envia para o serviço, escuta a resposta e retorna esses valores para o objeto cliente do WCF como valores de retorno ou `out` `ref` parâmetros.  
  
 Você também pode usar objetos de canal do cliente WCF para se conectar com o e usar os serviços do. Para obter detalhes, consulte [arquitetura de cliente WCF](./feature-details/client-architecture.md).  
  
#### <a name="creating-a-new-wcf-object"></a>Criando um novo objeto do WCF  
 Para ilustrar o uso de uma classe <xref:System.ServiceModel.ClientBase%601>, suponha que o seguinte contrato de serviço simples foi gerado em um aplicativo de serviço.  
  
> [!NOTE]
> Se você estiver usando o Visual Studio para criar seu cliente WCF, os objetos serão carregados automaticamente no Pesquisador de objetos quando você adicionar uma referência de serviço ao seu projeto.  
  
 [!code-csharp[C_GeneratedCodeFiles#12](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#12)]  
  
 Se você não estiver usando o Visual Studio, examine o código de contrato gerado para localizar o tipo que estende <xref:System.ServiceModel.ClientBase%601> e a interface do contrato de serviço `ISampleService` . Nesse caso, esse tipo terá a seguinte aparência:  
  
 [!code-csharp[C_GeneratedCodeFiles#14](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#14)]  
  
 Essa classe pode ser criada como um objeto local usando um dos construtores, configurada e, em seguida, usada para se conectar a um serviço do tipo `ISampleService`.  
  
 É recomendável que você crie primeiro seu objeto de cliente WCF e, em seguida, use-o e feche-o dentro de um único bloco try/catch. Não use a `using` instrução ( `Using` em Visual Basic) porque ela pode mascarar exceções em determinados modos de falha. Para obter mais informações, consulte as seções a seguir, bem como [usar fechar e anular para liberar os recursos do cliente do WCF](./samples/use-close-abort-release-wcf-client-resources.md).  
  
### <a name="contracts-bindings-and-addresses"></a>Contratos, associações e endereços  
 Antes de criar um objeto de cliente do WCF, você deve configurar o objeto de cliente. Especificamente, ele deve ter um *ponto de extremidade* de serviço para usar. O ponto de extremidade é a combinação de um contrato de serviço, uma associação e um endereço. (Para obter mais informações sobre pontos de extremidade, consulte [pontos de extremidade: endereços, associações e contratos](./feature-details/endpoints-addresses-bindings-and-contracts.md).) Normalmente, essas informações estão localizadas no [\<endpoint>](../configure-apps/file-schema/wcf/endpoint-of-client.md) elemento em um arquivo de configuração de aplicativo cliente, como a que a ferramenta de Svcutil.exe gera e é carregada automaticamente quando você cria o objeto de cliente. Ambos os tipos de cliente WCF também têm sobrecargas que permitem especificar essas informações programaticamente.  
  
 Por exemplo, um arquivo de configuração gerado de um `ISampleService` usado nos exemplos anteriores contém as seguintes informações de ponto de extremidade.  
  
 [!code-xml[C_GeneratedCodeFiles#19](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/common/client.exe.config#19)]  
  
 Esse arquivo de configuração especifica um ponto de extremidade de destino no elemento `<client>`. Para obter mais informações sobre como usar vários pontos de extremidade de destino, consulte os <xref:System.ServiceModel.ClientBase%601.%23ctor%2A> <xref:System.ServiceModel.ChannelFactory%601.%23ctor%2A> construtores ou.  
  
## <a name="calling-operations"></a>Chamando operações  
 Depois que você tiver um objeto cliente criado e configurado, crie um bloco try/catch, chame operações da mesma maneira que faria se o objeto fosse local e feche o objeto cliente do WCF. Quando o aplicativo cliente chama a primeira operação, o WCF abre automaticamente o canal subjacente e o canal subjacente é fechado quando o objeto é reciclado. (Como alternativa, você também pode abrir e fechar explicitamente o canal antes ou após chamar outras operações.)  
  
 Por exemplo, se você tiver o seguinte contrato de serviço:  
  
```csharp  
namespace Microsoft.ServiceModel.Samples  
{  
    using System;  
    using System.ServiceModel;  
  
    [ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples")]  
    public interface ICalculator  
   {  
        [OperationContract]  
        double Add(double n1, double n2);  
        [OperationContract]  
        double Subtract(double n1, double n2);  
        [OperationContract]  
        double Multiply(double n1, double n2);  
        [OperationContract]  
        double Divide(double n1, double n2);  
    }  
}  
```  
  
```vb  
Namespace Microsoft.ServiceModel.Samples  
  
    Imports System  
    Imports System.ServiceModel  
  
    <ServiceContract(Namespace:= _  
    "http://Microsoft.ServiceModel.Samples")> _
   Public Interface ICalculator  
        <OperationContract> _
        Function Add(n1 As Double, n2 As Double) As Double  
        <OperationContract> _
        Function Subtract(n1 As Double, n2 As Double) As Double  
        <OperationContract> _
        Function Multiply(n1 As Double, n2 As Double) As Double  
        <OperationContract> _
     Function Divide(n1 As Double, n2 As Double) As Double  
End Interface  
```  
  
 Você pode chamar operações criando um objeto de cliente WCF e chamando seus métodos, como demonstra o exemplo de código a seguir. A abertura, a chamada e o fechamento do objeto do cliente do WCF ocorre dentro de um único bloco try/catch. Para obter mais informações, consulte [acessando serviços usando um cliente WCF](./feature-details/accessing-services-using-a-client.md) e [usar fechar e anular para liberar recursos do cliente WCF](./samples/use-close-abort-release-wcf-client-resources.md).  
  
 [!code-csharp[C_GeneratedCodeFiles#20](../../../samples/snippets/csharp/VS_Snippets_CFX/c_generatedcodefiles/cs/proxycode.cs#20)]  
  
## <a name="handling-errors"></a>Tratando erros  
 Poderão ocorrer exceções em um aplicativo cliente quando o canal de cliente subjacente for aberto (explícita ou automaticamente, durante o chamamento de uma operação), o objeto de cliente ou de canal for usado para chamar operações, ou o canal de cliente subjacente for fechado. É recomendável que os aplicativos possam, pelo menos, manipular as exceções <xref:System.TimeoutException?displayProperty=nameWithType> e <xref:System.ServiceModel.CommunicationException?displayProperty=nameWithType> possíveis, além de qualquer objeto <xref:System.ServiceModel.FaultException?displayProperty=nameWithType> lançado como resultado de falhas de SOAP retornadas pelas operações. As falhas de SOAP especificadas no contrato de operação são geradas para aplicativos cliente como <xref:System.ServiceModel.FaultException%601?displayProperty=nameWithType>, em que o parâmetro de tipo é o tipo de detalhe da falha de SOAP. Para obter mais informações sobre como lidar com condições de erro em um aplicativo cliente, consulte [envio e recebimento de falhas](sending-and-receiving-faults.md). Para obter um exemplo completo, o mostra como tratar erros em um cliente, consulte [exceções esperadas](./samples/expected-exceptions.md).  
  
## <a name="configuring-and-securing-clients"></a>Configurando e protegendo clientes  
 A configuração de um cliente começa com o carregamento obrigatório das informações de ponto de extremidade de destino do objeto de cliente ou de canal, geralmente em um arquivo de configuração, embora você também pode carregar essas informações de modo programático, usando as propriedades e os construtores de cliente. No entanto, são necessárias etapas adicionais de configuração em vários cenários de segurança e para habilitar determinado comportamento de cliente.  
  
 Por exemplo, os requisitos de segurança dos contratos de serviço são declarados na interface do contrato de serviço e, se a ferramenta Svcutil.exe tiver criado um arquivo de configuração, este geralmente conterá uma associação capaz de oferecer suporte aos requisitos de segurança do serviço. Em alguns casos, no entanto, talvez seja necessário definir configurações de segurança adicional, como configurar credenciais do cliente. Para obter informações completas sobre a configuração de segurança para clientes WCF, consulte [protegendo clientes](securing-clients.md).  
  
 Além disso, algumas modificações personalizadas podem ser habilitadas em aplicativos cliente, como comportamentos personalizados de tempo de execução. Para obter mais informações sobre como configurar um comportamento de cliente personalizado, consulte [Configurando comportamentos do cliente](configuring-client-behaviors.md).  
  
## <a name="creating-callback-objects-for-duplex-services"></a>Criando objetos de retorno de chamada para serviços duplex  
 Os serviços duplex especificam um contrato de retorno de chamada que o aplicativo cliente deve implementar para fornecer um objeto de retorno de chamada que o serviço chamará de acordo com os requisitos do contrato. Embora os objetos de retorno de chamada não sejam serviços completos (por exemplo, você não pode iniciar um canal com um objeto de retorno de chamada), para fins de implementação e configuração, eles podem ser considerados um tipo de serviço.  
  
 Os clientes dos serviços duplex devem:  
  
- Implementar uma classe de contrato de retorno de chamada.  
  
- Crie uma instância da classe de implementação de contrato de retorno de chamada e use-a para criar o <xref:System.ServiceModel.InstanceContext?displayProperty=nameWithType> objeto que você passa para o construtor de cliente do WCF.  
  
- Chamar operações e manipular retornos de chamada de operação.  
  
 Os objetos de cliente do WCF duplex funcionam como suas contrapartes não duplex, com exceção de que eles expõem a funcionalidade necessária para dar suporte a retornos de chamada, incluindo a configuração do serviço de retorno de chamada.  
  
 Por exemplo, você pode controlar vários aspectos do comportamento de runtime do objeto de retorno de chamada usando as propriedades do atributo <xref:System.ServiceModel.CallbackBehaviorAttribute?displayProperty=nameWithType> da classe de retorno de chamada. Outro exemplo é o uso da classe <xref:System.ServiceModel.Description.CallbackDebugBehavior?displayProperty=nameWithType> para habilitar o retorno das informações de exceção para serviços que chamam o objeto de retorno de chamada. Para obter mais informações, consulte [duplex Services](./feature-details/duplex-services.md). Para obter um exemplo completo, consulte [duplex](./samples/duplex.md).  
  
 Em computadores com o Windows XP que executam os Serviços de Informações da Internet (IIS) 5.1, os clientes duplex devem especificar um endereço básico de cliente usando a classe <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType>; do contrário, uma exceção será lançada. O exemplo de código a seguir mostra como fazer isso no código.  
  
 [!code-csharp[S_DualHttp#8](../../../samples/snippets/csharp/VS_Snippets_CFX/s_dualhttp/cs/program.cs#8)]
 [!code-vb[S_DualHttp#8](../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_dualhttp/vb/module1.vb#8)]  
  
 O exemplo de código a seguir mostra como fazer isso em um arquivo de configuração  
  
 [!code-csharp[S_DualHttp#134](../../../samples/snippets/csharp/VS_Snippets_CFX/s_dualhttp/cs/program.cs#134)]  
  
## <a name="calling-services-asynchronously"></a>Chamando serviços de modo assíncrono  
 A maneira como as operações serão chamadas fica totalmente a cargo do desenvolvedor do cliente. Isso acontece porque as mensagens que compõem uma operação podem ser mapeadas para métodos síncronos ou assíncronos quando expressas em código gerenciado. Portanto, se você quiser compilar um cliente que chama operações de forma assíncrona, use o Svcutil.exe para gerar um código de cliente assíncrono usando a opção `/async`. Para obter mais informações, consulte [como: chamar operações de serviço de forma assíncrona](./feature-details/how-to-call-wcf-service-operations-asynchronously.md).  
  
## <a name="calling-services-using-wcf-client-channels"></a>Chamando serviços por meio de canais de cliente WCF  
 Os tipos de cliente do WCF se estendem <xref:System.ServiceModel.ClientBase%601> , que derivam de <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> interface para expor o sistema de canal subjacente. Você pode chamar serviços usando o contrato de serviço de destino com a classe <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType>. Para obter detalhes, consulte [arquitetura de cliente WCF](./feature-details/client-architecture.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType>
- <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType>
