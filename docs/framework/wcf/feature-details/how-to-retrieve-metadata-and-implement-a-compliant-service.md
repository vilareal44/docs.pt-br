---
title: Como recuperar metadados e implementar um serviço compatível
ms.date: 03/30/2017
ms.assetid: f6f3a2b9-c8aa-4b0b-832c-ec2927bf1163
ms.openlocfilehash: 6bd67ec8dcc0e73d097796b44974dce00b9a4eb2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601212"
---
# <a name="how-to-retrieve-metadata-and-implement-a-compliant-service"></a>Como recuperar metadados e implementar um serviço compatível
Muitas vezes, a mesma pessoa não cria e implementa serviços. Em ambientes onde os aplicativos de interoperação são importantes, os contratos podem ser criados ou descritos no WSDL (Web Services Description Language) e um desenvolvedor deve implementar um serviço que esteja em conformidade com o contrato fornecido. Talvez você também queira migrar um serviço existente para Windows Communication Foundation (WCF), mas preservar o formato de conexão. Além disso, os contratos duplex exigem que os chamadores implementem um contrato de retorno de chamada também.  
  
 Nesses casos, você deve usar a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) (ou uma ferramenta equivalente) para gerar uma interface de contrato de serviço em uma linguagem gerenciada que pode ser implementada para atender aos requisitos do contrato. Normalmente, a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) é usada para adquirir um contrato de serviço que é usado com uma fábrica de canais ou um tipo de cliente WCF, bem como com um arquivo de configuração de cliente que define a ligação e o endereço corretos. Para usar o arquivo de configuração gerado, você deve alterá-lo em um arquivo de configuração de serviço. Talvez você também precise modificar o contrato de serviço.  
  
### <a name="to-retrieve-data-and-implement-a-compliant-service"></a>Para recuperar dados e implementar um serviço em conformidade  
  
1. Use a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) em arquivos de metadados ou um ponto de extremidade de metadados para gerar um arquivo de código.  
  
2. Pesquise a parte do arquivo de código de saída que contém a interface de interesse (caso haja mais de um) marcado com o <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType> atributo. O exemplo de código a seguir mostra as duas interfaces geradas pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). O primeiro ( `ISampleService` ) é a interface de contrato de serviço que você implementa para criar um serviço em conformidade. O segundo ( `ISampleServiceChannel` ) é uma interface auxiliar para uso do cliente que estende a interface do contrato de serviço e <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> e é para uso em um aplicativo cliente.  
  
     [!code-csharp[ClientProxyCodeSample#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/proxycode.cs#2)]  
  
3. Se o WSDL não especificar uma ação de resposta para todas as operações, os contratos de operação gerados poderão ter a <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> propriedade definida como o caractere curinga (*). Remova essa configuração de propriedade. Caso contrário, quando você implementar os metadados do contrato de serviço, os metadados não poderão ser exportados para essas operações.  
  
4. Implemente a interface em uma classe e hospede o serviço. Para obter um exemplo, consulte [como implementar um contrato de serviço](../how-to-implement-a-wcf-contract.md)ou ver uma implementação simples abaixo na seção de exemplo.  
  
5. No arquivo de configuração do cliente que a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) gera, altere a [\<client>](../../configure-apps/file-schema/wcf/client.md) seção de configuração para uma [\<services>](../../configure-apps/file-schema/wcf/services.md) seção de configuração. (Para obter um exemplo de um arquivo de configuração de aplicativo cliente gerado, consulte a seção "exemplo" a seguir.)  
  
6. Na [\<services>](../../configure-apps/file-schema/wcf/services.md) seção configuração, crie um `name` atributo na [\<services>](../../configure-apps/file-schema/wcf/services.md) seção configuração para sua implementação de serviço.  
  
7. Defina o atributo de serviço `name` como o nome de configuração para a implementação de seu serviço.  
  
8. Adicione os elementos de configuração do ponto de extremidade que usam o contrato de serviço implementado à seção de configuração do serviço.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra a maioria de um arquivo de código gerado pela execução da [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) em relação aos arquivos de metadados.  
  
 O código a seguir mostra:  
  
- A interface de contrato de serviço que, quando implementada, está em conformidade com os requisitos de contrato ( `ISampleService` ).  
  
- A interface auxiliar para o uso do cliente que estende a interface do contrato de serviço e <xref:System.ServiceModel.IClientChannel?displayProperty=nameWithType> e é para uso em um aplicativo cliente ( `ISampleServiceChannel` ).  
  
- A classe auxiliar que estende <xref:System.ServiceModel.ClientBase%601?displayProperty=nameWithType> e é para uso em um aplicativo cliente ( `SampleServiceClient` ).  
  
- O arquivo de configuração gerado a partir do serviço.  
  
- Uma `ISampleService` implementação de serviço simples.  
  
- Uma conversão do arquivo de configuração do lado do cliente para uma versão do lado do serviço.  
  
[!code-csharp[ClientProxyCodeSample#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/proxycode.cs#1)]

[!code-xml[ClientProxyCodeSample#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/client.exe.config#10)]

[!code-csharp[ClientProxyCodeSample#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/hostapplication.cs#5)]

[!code-xml[ClientProxyCodeSample#20](../../../../samples/snippets/csharp/VS_Snippets_CFX/clientproxycodesample/cs/hostapplication.exe.config#20)]
  
## <a name="see-also"></a>Consulte também

- [Ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md)
