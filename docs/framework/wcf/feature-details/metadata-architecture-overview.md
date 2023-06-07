---
title: Visão geral da arquitetura de metadados
ms.date: 03/30/2017
helpviewer_keywords:
- metadata [WCF], overview
ms.assetid: 1d37645e-086d-4d68-a358-f3c5b6e8205e
ms.openlocfilehash: a6bd41fa5aed4c2a22ee7c72087e2da0d7e4ea17
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598847"
---
# <a name="metadata-architecture-overview"></a>Visão geral da arquitetura de metadados
O Windows Communication Foundation (WCF) fornece uma infra-estrutura avançada para exportar, publicar, recuperar e importar metadados de serviço. Os serviços WCF usam metadados para descrever como interagir com os pontos de extremidade do serviço para que as ferramentas, como SvcUtil. exe, possam gerar automaticamente o código do cliente para acessar o serviço.  
  
 A maioria dos tipos que compõem a infraestrutura de metadados do WCF reside no <xref:System.ServiceModel.Description> namespace.  
  
 O WCF usa a <xref:System.ServiceModel.Description.ServiceEndpoint> classe para descrever os pontos de extremidade em um serviço. Você pode usar o WCF para gerar metadados para pontos de extremidade de serviço ou importar metadados de serviço para gerar <xref:System.ServiceModel.Description.ServiceEndpoint> instâncias.  
  
 O WCF representa os metadados de um serviço como uma instância do <xref:System.ServiceModel.Description.MetadataSet> tipo, a estrutura do que está fortemente ligada ao formato de serialização de metadados definido em WS-MetadataExchange. O <xref:System.ServiceModel.Description.MetadataSet> tipo agrupa os metadados de serviço reais, como documentos WSDL (Web Services Description Language), documentos de esquema XML ou expressões WS-Policy, como uma coleção de <xref:System.ServiceModel.Description.MetadataSection> instâncias. Cada <xref:System.ServiceModel.Description.MetadataSection?displayProperty=nameWithType> instância contém um dialeto de metadados específico e um identificador. Um <xref:System.ServiceModel.Description.MetadataSection?displayProperty=nameWithType> pode conter os seguintes itens em sua <xref:System.ServiceModel.Description.MetadataSection.Metadata%2A?displayProperty=nameWithType> Propriedade:  
  
- Metadados brutos.  
  
- Uma instância <xref:System.ServiceModel.Description.MetadataReference>.  
  
- Uma instância <xref:System.ServiceModel.Description.MetadataLocation>.  
  
 Uma <xref:System.ServiceModel.Description.MetadataReference?displayProperty=nameWithType> instância aponta para outro ponto de extremidade de intercâmbio de metadados (MEX) e <xref:System.ServiceModel.Description.MetadataLocation?displayProperty=nameWithType> instâncias apontam para um documento de metadados usando uma URL http. O WCF dá suporte ao uso de documentos WSDL para descrever pontos de extremidade de serviço, contratos de serviço, associações, padrões de troca de mensagens, mensagens e mensagens de falha implementadas por um serviço. Os tipos de dados usados pelo serviço são descritos em documentos WSDL usando o esquema XML. Para obter mais informações, consulte [importação e exportação de esquema](schema-import-and-export.md). Você pode usar o WCF para exportar e importar extensões WSDL para comportamento de serviço, comportamentos de contrato e elementos de associação que estendem a funcionalidade de um serviço. Para obter mais informações, consulte [exportando metadados personalizados para uma extensão WCF](../extending/exporting-custom-metadata-for-a-wcf-extension.md).  
  
## <a name="exporting-service-metadata"></a>Exportando metadados de serviço  
 No WCF, a *exportação de metadados* é o processo de descrever os pontos de extremidade de serviço e projetar-los em uma representação paralela e padronizada que os clientes podem usar para entender como usar o serviço. Para exportar metadados de <xref:System.ServiceModel.Description.ServiceEndpoint> instâncias, use uma implementação da <xref:System.ServiceModel.Description.MetadataExporter> classe abstract. Uma <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> implementação gera metadados que são encapsulados em uma <xref:System.ServiceModel.Description.MetadataSet> instância do.  
  
 A <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> classe fornece uma estrutura para gerar expressões de política que descrevem os recursos e requisitos de uma associação de ponto de extremidade e suas operações, mensagens e falhas associadas. Essas expressões de política são capturadas em uma <xref:System.ServiceModel.Description.PolicyConversionContext> instância do. Uma <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> implementação pode, então, anexar essas expressões de política aos metadados que ele gera.  
  
 As <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> chamadas para cada uma <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType> implementa a <xref:System.ServiceModel.Description.IPolicyExportExtension> interface na associação de a <xref:System.ServiceModel.Description.ServiceEndpoint> ao gerar um <xref:System.ServiceModel.Description.PolicyConversionContext> objeto para a <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> implementação a ser usada. Você pode exportar novas declarações de política implementando a <xref:System.ServiceModel.Description.IPolicyExportExtension> interface em suas implementações personalizadas do <xref:System.ServiceModel.Channels.BindingElement> tipo.  
  
 O <xref:System.ServiceModel.Description.WsdlExporter> tipo é a implementação da <xref:System.ServiceModel.Description.MetadataExporter?displayProperty=nameWithType> classe abstrata incluída no WCF. O <xref:System.ServiceModel.Description.WsdlExporter> tipo gera metadados WSDL com expressões de política anexadas.  
  
 Para exportar metadados WSDL personalizados ou extensões WSDL para comportamentos de ponto de extremidade, comportamentos de contrato ou elementos de associação em um ponto de extremidade de serviço, você pode implementar a <xref:System.ServiceModel.Description.IWsdlExportExtension> interface. O <xref:System.ServiceModel.Description.WsdlExporter> examina uma <xref:System.ServiceModel.Description.ServiceEndpoint> instância para elementos de associação, comportamentos de operação, comportamentos de contrato e comportamentos de ponto de extremidade que implementam a <xref:System.ServiceModel.Description.IWsdlExportExtension> interface ao gerar o documento WSDL.  
  
## <a name="publishing-service-metadata"></a>Publicando metadados do serviço  
 Os serviços WCF publicam metadados expondo um ou mais pontos de extremidade de metadados. A publicação de metadados de serviço torna os metadados de serviço disponíveis usando protocolos padronizados, como MEX e solicitações HTTP/GET. Os pontos de extremidade de metadados são semelhantes a outros pontos de extremidade de serviço, pois têm um endereço, uma associação e um contrato. Você pode adicionar pontos de extremidade de metadados a um host de serviço na configuração ou no código.  
  
 Para publicar pontos de extremidade de metadados para um serviço WCF, você deve primeiro adicionar uma instância do <xref:System.ServiceModel.Description.ServiceMetadataBehavior> comportamento de serviço ao serviço. A adição de uma <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> instância ao seu serviço aumenta seu serviço com a capacidade de publicar metadados expondo um ou mais pontos de extremidade de metadados. Depois de adicionar o <xref:System.ServiceModel.Description.ServiceMetadataBehavior?displayProperty=nameWithType> comportamento do serviço, você pode expor os pontos de extremidade de metadados que dão suporte ao protocolo Mex ou aos pontos de extremidade de metadados que respondem às solicitações HTTP/Get.  
  
 Para adicionar pontos de extremidade de metadados que usam o protocolo MEX, adicione pontos de extremidade de serviço ao host de serviço que usa o contrato de serviço denominado IMetadataExchange. o WCF define a <xref:System.ServiceModel.Description.IMetadataExchange> interface que tem esse nome de contrato de serviço. Os pontos de extremidade de WS-MetadataExchange, ou pontos de extremidade MEX, podem usar uma das quatro associações padrão expostas pelos métodos de fábrica estáticos na <xref:System.ServiceModel.Description.MetadataExchangeBindings> classe para corresponder às associações padrão usadas pelas ferramentas do WCF, como SvcUtil. exe. Você também pode configurar pontos de extremidade de metadados MEX usando uma associação personalizada.  
  
 O <xref:System.ServiceModel.Description.ServiceMetadataBehavior> usa um <xref:System.ServiceModel.Description.WsdlExporter?displayProperty=nameWithType> para exportar metadados para todos os pontos de extremidade de serviço em seu serviço. Para obter mais informações sobre como exportar metadados de um serviço, consulte Exportando [e importando metadados](exporting-and-importing-metadata.md).  
  
 O <xref:System.ServiceModel.Description.ServiceMetadataBehavior> aumenta seu host de serviço adicionando uma <xref:System.ServiceModel.Description.ServiceMetadataExtension> instância como uma extensão ao seu host de serviço. O <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> fornece a implementação para os protocolos de publicação de metadados. Você também pode usar o <xref:System.ServiceModel.Description.ServiceMetadataExtension?displayProperty=nameWithType> para obter os metadados do serviço em tempo de execução acessando a <xref:System.ServiceModel.Description.ServiceMetadataExtension.Metadata%2A> propriedade.  
  
> [!CAUTION]
> Se você adicionar um ponto de extremidade MEX em seu arquivo de configuração de aplicativo e, em seguida, tentar adicionar o <xref:System.ServiceModel.Description.ServiceMetadataBehavior> ao seu host de serviço no código, obterá a seguinte exceção:  
>
> System. InvalidOperationException: o nome do contrato ' IMetadataExchange ' não pôde ser encontrado na lista de contratos implementados pelo Service Service1. Adicione um ServiceMetadataBehavior ao arquivo de configuração ou ao ServiceHost diretamente para habilitar o suporte para este contrato.  
>
> Você pode contornar esse problema adicionando o <xref:System.ServiceModel.Description.ServiceMetadataBehavior> no arquivo de configuração ou adicionando o ponto de extremidade e <xref:System.ServiceModel.Description.ServiceMetadataBehavior> no código.  
>
> Para obter um exemplo de adição <xref:System.ServiceModel.Description.ServiceMetadataBehavior> em um arquivo de configuração de aplicativo, consulte a [introdução](../samples/getting-started-sample.md). Para obter um exemplo de adição <xref:System.ServiceModel.Description.ServiceMetadataBehavior> no código, consulte o exemplo de hospedagem [interna](../samples/self-host.md) .  

> [!CAUTION]
> Ao publicar metadados para um serviço que expõe dois contratos de serviço diferentes nos quais cada um contém uma operação do mesmo nome, uma exceção é lançada. Por exemplo, se você tiver um serviço que expõe um contrato de serviço chamado ICarService que tem uma operação get (carro c) e o mesmo serviço expõe um contrato de serviço chamado IBookService que tem uma operação get (livro b), uma exceção é lançada ou uma mensagem de erro é exibida ao gerar os metadados do serviço. Para contornar esse problema, faça um dos seguintes:  
>
> - Renomeie uma das operações.
> - Defina <xref:System.ServiceModel.OperationContractAttribute.Name%2A> como um nome diferente.  
> - Defina um dos namespaces de operações para um namespace diferente usando a <xref:System.ServiceModel.ServiceContractAttribute.Namespace%2A> propriedade.  
  
## <a name="retrieving-service-metadata"></a>Recuperando metadados de serviço  
 O WCF pode recuperar metadados de serviço usando protocolos padronizados, como WS-MetadataExchange e HTTP. Os dois protocolos têm suporte do <xref:System.ServiceModel.Description.MetadataExchangeClient> tipo. Você recupera os metadados de serviço usando o <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> tipo fornecendo um endereço e uma associação opcional. A associação usada por uma <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> instância pode ser uma das associações padrão da <xref:System.ServiceModel.Description.MetadataExchangeBindings> classe estática, uma associação fornecida pelo usuário ou uma associação carregada de uma configuração de ponto de extremidade para o `IMetadataExchange` contrato. O <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> também pode resolver referências de URL http para metadados usando o <xref:System.Net.HttpWebRequest> tipo.  
  
 Por padrão, uma <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> instância está vinculada a uma única <xref:System.ServiceModel.Channels.ChannelFactoryBase> instância. Você pode alterar ou substituir a <xref:System.ServiceModel.Channels.ChannelFactoryBase> instância usada por um <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> substituindo o <xref:System.ServiceModel.Description.MetadataExchangeClient.GetChannelFactory%2A> método virtual. Da mesma forma, você pode alterar ou substituir a <xref:System.Net.HttpWebRequest?displayProperty=nameWithType> instância usada por um <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> para fazer solicitações HTTP/Get substituindo o <xref:System.ServiceModel.Description.MetadataExchangeClient.GetWebRequest%2A?displayProperty=nameWithType> método virtual.  
  
 Você pode recuperar os metadados de serviço usando solicitações WS-MetadataExchange ou HTTP/GET usando a ferramenta svcutil. exe e passando a opção **/target: Metadata** e um endereço. Svcutil. exe baixa os metadados no endereço especificado e salva os arquivos em disco. Svcutil. exe usa uma <xref:System.ServiceModel.Description.MetadataExchangeClient?displayProperty=nameWithType> instância internamente e carrega uma configuração de ponto de extremidade MEX (do arquivo de configuração do aplicativo) cujo nome corresponde ao esquema do endereço passado para SvcUtil. exe, se houver. Caso contrário, svcutil. exe usa uma das associações definidas pelo <xref:System.ServiceModel.Description.MetadataExchangeBindings> tipo de alocador estático.  
  
## <a name="importing-service-metadata"></a>Importando metadados de serviço  
 No WCF, a importação de metadados é o processo de gerar uma representação abstrata de um serviço ou de suas partes de componente a partir de seus metadados. Por exemplo, o WCF pode importar <xref:System.ServiceModel.Description.ServiceEndpoint> instâncias, <xref:System.ServiceModel.Channels.Binding> instâncias ou instâncias <xref:System.ServiceModel.Description.ContractDescription> de um documento WSDL para um serviço. Para importar metadados de serviço no WCF, use uma implementação da <xref:System.ServiceModel.Description.MetadataImporter> classe abstrata. Os tipos que derivam da <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> classe implementam o suporte à importação de formatos de metadados que aproveitam a lógica de importação de WS-Policy no WCF.  
  
 Uma <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> implementação coleta as expressões de política anexadas aos metadados de serviço em um <xref:System.ServiceModel.Description.PolicyConversionContext> objeto. <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType>Em seguida, o processa as políticas como parte da importação dos metadados chamando as implementações da <xref:System.ServiceModel.Description.IPolicyImportExtension> interface na <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A> propriedade.  
  
 Você pode adicionar suporte para importar novas declarações de política para um <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> adicionando sua própria implementação da <xref:System.ServiceModel.Description.IPolicyImportExtension> interface à <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A> coleção em uma <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> instância. Como alternativa, você pode registrar sua extensão de importação de política no arquivo de configuração de aplicativo cliente.  
  
 O <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> tipo é a implementação da <xref:System.ServiceModel.Description.MetadataImporter?displayProperty=nameWithType> classe abstrata incluída no WCF. O <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> tipo importa metadados WSDL com políticas anexadas que são agrupadas em um <xref:System.ServiceModel.Description.MetadataSet> objeto.  
  
 Você pode adicionar suporte para importar extensões WSDL implementando a <xref:System.ServiceModel.Description.IWsdlImportExtension> interface e, em seguida, adicionando sua implementação à <xref:System.ServiceModel.Description.WsdlImporter.WsdlImportExtensions%2A> propriedade em sua <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> instância. O <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> também pode carregar implementações da <xref:System.ServiceModel.Description.IWsdlImportExtension?displayProperty=nameWithType> interface registrada no arquivo de configuração do aplicativo cliente.  
  
## <a name="dynamic-bindings"></a>Associações dinâmicas  
 Você pode atualizar dinamicamente a associação que você usa para criar um canal para um ponto de extremidade de serviço no caso em que a associação para o ponto de extremidade é alterada ou você deseja criar um canal para um ponto de extremidade que usa o mesmo contrato, mas tem uma associação diferente. Você pode usar a <xref:System.ServiceModel.Description.MetadataResolver> classe estática para recuperar e importar metadados em tempo de execução para pontos de extremidade de serviço que implementam um contrato específico. Você pode usar os objetos importados <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType> para criar uma fábrica de cliente ou de canal para o ponto de extremidade desejado.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Description>
- [Formatos de metadados](metadata-formats.md)
- [Exportando e importando metadados](exporting-and-importing-metadata.md)
- [Publicando metadados](publishing-metadata.md)
- [Recuperando metadados](retrieving-metadata.md)
- [Utilizando metadados](using-metadata.md)
- [Considerações de segurança com metadados](security-considerations-with-metadata.md)
- [Estendendo o sistema de metadados](../extending/extending-the-metadata-system.md)
