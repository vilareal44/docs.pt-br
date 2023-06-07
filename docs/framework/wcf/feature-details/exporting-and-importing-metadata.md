---
title: Exportando e importando metadados
ms.date: 03/30/2017
helpviewer_keywords:
- metadata [WCF], exporting and importing
ms.assetid: 614a75bb-e0b0-4c95-b6d8-02cb5e5ddb38
ms.openlocfilehash: f07a1a10529aa1615bb00a0f3faeca9cb249aa64
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595525"
---
# <a name="exporting-and-importing-metadata"></a>Exportando e importando metadados
No Windows Communication Foundation (WCF), exportar metadados é o processo de descrever os pontos de extremidade de serviço e projetar-los em uma representação paralela e padronizada que os clientes podem usar para entender como usar o serviço. Importar metadados de serviço é o processo de gerar <xref:System.ServiceModel.Description.ServiceEndpoint> instâncias ou partes de metadados de serviço.  
  
## <a name="exporting-metadata"></a>Exportando metadados  
 Para exportar metadados de <xref:System.ServiceModel.Description.ServiceEndpoint?displayProperty=nameWithType> instâncias, use uma implementação da <xref:System.ServiceModel.Description.MetadataExporter> classe abstract. O <xref:System.ServiceModel.Description.WsdlExporter> tipo é a implementação da <xref:System.ServiceModel.Description.MetadataExporter> classe abstrata incluída no WCF.  
  
 O <xref:System.ServiceModel.Description.WsdlExporter?displayProperty=nameWithType> tipo gera metadados WSDL (Web Services Description Language) com expressões de política anexadas encapsuladas em uma <xref:System.ServiceModel.Description.MetadataSet> instância do. Você pode usar uma <xref:System.ServiceModel.Description.WsdlExporter?displayProperty=nameWithType> instância para exportar de forma iterativa os metadados para <xref:System.ServiceModel.Description.ContractDescription> objetos e <xref:System.ServiceModel.Description.ServiceEndpoint> objetos. Você também pode exportar uma coleção de <xref:System.ServiceModel.Description.ServiceEndpoint> objetos e associá-los a um nome de serviço específico.  
  
> [!NOTE]
> Você só pode usar o `WsdlExporter` para exportar metadados de `ContractDescription` instâncias que contenham informações de tipo Common Language Runtime (CLR), como uma `ContractDescription` instância criada usando o `ContractDescription.GetContract` método ou criada como parte do `ServiceDescription` para uma `ServiceHost` instância do. Você não pode usar o `WsdlExporter` para exportar metadados de `ContractDescription` instâncias importadas de metadados de serviço ou construídos sem informações de tipo.  
  
## <a name="importing-metadata"></a>Importando metadados  
  
### <a name="importing-wsdl-documents"></a>Importando documentos WSDL  
 Para importar metadados de serviço no WCF, use uma implementação da <xref:System.ServiceModel.Description.MetadataImporter> classe abstrata. O <xref:System.ServiceModel.Description.WsdlImporter?displayProperty=nameWithType> tipo é a implementação da <xref:System.ServiceModel.Description.MetadataImporter> classe abstrata incluída no WCF. O <xref:System.ServiceModel.Description.WsdlImporter> tipo importa metadados WSDL com políticas anexadas agrupadas em um <xref:System.ServiceModel.Description.MetadataSet> objeto.  
  
 O <xref:System.ServiceModel.Description.WsdlImporter> tipo fornece controle sobre como importar os metadados. Você pode importar todos os pontos de extremidade, todas as associações ou todos os contratos. Você pode importar todos os pontos de extremidade associados a um serviço WSDL específico, associação ou tipo de porta. Você também pode importar o ponto de extremidade para uma porta WSDL específica, a associação para uma associação WSDL específica ou o contrato para um tipo de porta WSDL específico.  
  
 O <xref:System.ServiceModel.Description.WsdlImporter> também expõe uma <xref:System.ServiceModel.Description.MetadataImporter.KnownContracts%2A> propriedade que permite que você especifique um conjunto de contratos que não precisam ser importados. O <xref:System.ServiceModel.Description.WsdlImporter> usa os contratos na <xref:System.ServiceModel.Description.MetadataImporter.KnownContracts%2A> propriedade em vez de importar um contrato com o mesmo nome qualificado dos metadados.  
  
### <a name="importing-policies"></a>Importando políticas  
 O <xref:System.ServiceModel.Description.WsdlImporter> tipo coleta as expressões de política anexadas à mensagem, à operação e aos assuntos da política de ponto de extremidade e, em seguida, usa as <xref:System.ServiceModel.Description.IPolicyImportExtension> implementações na <xref:System.ServiceModel.Description.MetadataImporter.PolicyImportExtensions%2A> coleção para importar as expressões de política.  
  
 A lógica de importação de política manipula automaticamente as referências de política para expressões de política no mesmo documento WSDL e é identificada com um `wsu:Id` `xml:id` atributo ou. A lógica de importação de política protege aplicativos contra referências de política circulares limitando o tamanho de uma expressão de política a 4096 nós, em que um nó é um dos seguintes elementos: `wsp:Policy` ,, `wsp:All` `wsp:ExactlyOne` , `wsp:policyReference` .  
  
 A lógica de importação de política também normaliza automaticamente as expressões de política. As expressões de política aninhadas e o `wsp:Optional` atributo não são normalizados. A quantidade de processamento de normalização é limitada a 4096 etapas, em que cada etapa gera uma declaração de política ou um elemento filho de um `wsp:ExactlyOne` elemento.  
  
 O <xref:System.ServiceModel.Description.WsdlImporter> tipo tenta até 32 combinações de alternativas de política anexadas aos diferentes assuntos da política WSDL. Se nenhuma combinação for importada corretamente, a primeira combinação será usada para construir uma associação personalizada parcial.  
  
## <a name="error-handling"></a>Tratamento de erros  
 Os <xref:System.ServiceModel.Description.MetadataExporter> <xref:System.ServiceModel.Description.MetadataImporter> tipos e expõem uma `Errors` propriedade que pode conter uma coleção de mensagens de erro e de aviso encontradas durante os processos de exportação e importação, respectivamente, que podem ser usados durante a implementação de ferramentas.  
  
 O <xref:System.ServiceModel.Description.WsdlImporter> tipo geralmente gera uma exceção para uma exceção detectada durante o processo de importação e adiciona um erro correspondente à sua `Errors` propriedade. Os <xref:System.ServiceModel.Description.WsdlImporter.ImportAllContracts%2A> <xref:System.ServiceModel.Description.WsdlImporter.ImportAllBindings%2A> métodos,, <xref:System.ServiceModel.Description.WsdlImporter.ImportAllEndpoints%2A> e <xref:System.ServiceModel.Description.WsdlImporter.ImportEndpoints%2A> , no entanto, não lançam essas exceções, portanto, você deve verificar a `Errors` propriedade para determinar se ocorreram problemas ao chamar esses métodos.  
  
 O <xref:System.ServiceModel.Description.WsdlExporter> tipo gera novamente as exceções detectadas durante o processo de exportação. Essas exceções não são capturadas como erros na `Errors` propriedade. Depois que o <xref:System.ServiceModel.Description.WsdlExporter> gera uma exceção, ela está em um estado com falha e não pode ser reutilizada. O <xref:System.ServiceModel.Description.WsdlExporter> adiciona avisos à sua `Errors` propriedade quando uma operação não pode ser exportada porque usa ações curinga e quando são encontrados nomes de associação duplicados.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Como importar metadados para pontos de extremidade de serviço](how-to-import-metadata-into-service-endpoints.md)  
 Descreve como importar metadados baixados para objetos de descrição.  
  
 [Como exportar metadados para pontos de extremidade de serviço](how-to-export-metadata-from-service-endpoints.md)  
 Descreve como exportar objetos de descrição para metadados.  
  
 [ServiceDescription and WSDL Reference](servicedescription-and-wsdl-reference.md)  
 Descreve o mapeamento entre os objetos Description e WSDL.  
  
 [Como utilizar o Svcutil.exe para exportar metadados de código de serviço compilado](how-to-use-svcutil-exe-to-export-metadata-from-compiled-service-code.md)  
 Descreve o uso de svcutil. exe para exportar metadados para serviços, contratos e tipos de dados em assemblies compilados.  
  
 [Referência de esquema de contrato de dados](data-contract-schema-reference.md)  
 Descreve o subconjunto do esquema XML (XSD) usado pelo <xref:System.Runtime.Serialization.DataContractSerializer> para descrever tipos CLR (Common Language Runtime) para SERIALIZAÇÃO XML.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel.Description.WsdlExporter>  
  
 <xref:System.ServiceModel.Description.WsdlImporter>  
  
## <a name="see-also"></a>Consulte também

- [Exportando metadados personalizados para uma extensão do WCF](../extending/exporting-custom-metadata-for-a-wcf-extension.md)
- [Importando metadados personalizados para uma extensão do WCF](../extending/importing-custom-metadata-for-a-wcf-extension.md)
