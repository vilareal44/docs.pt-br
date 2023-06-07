---
title: Considerações de segurança com metadados
ms.date: 03/30/2017
ms.assetid: e78ef8ab-4f63-4656-ab93-b1deab2666d5
ms.openlocfilehash: c41ebf5c39e74932a4bade610c4e236b28444327
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601017"
---
# <a name="security-considerations-with-metadata"></a>Considerações de segurança com metadados
Ao usar os recursos de metadados no Windows Communication Foundation (WCF), considere as implicações de segurança de publicação, recuperação e uso de metadados de serviço.  
  
## <a name="when-to-publish-metadata"></a>Quando publicar metadados  
 Os serviços WCF não publicam metadados por padrão. Para publicar metadados para um serviço WCF, você deve habilitar explicitamente a publicação de metadados adicionando pontos de extremidade de metadados ao seu serviço (consulte [publicando metadados](publishing-metadata.md)). Deixar a publicação de metadados desabilitada reduz a superfície de ataque para seu serviço e reduz o risco de divulgação involuntária de informações. Nem todos os serviços devem publicar metadados. Se você não precisar publicar metadados, considere deixá-los desativados. Observe que você ainda pode gerar metadados e código de cliente diretamente de seus assemblies de serviço usando a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Para obter mais informações sobre como usar svcutil. exe para exportar metadados, consulte [como: usar svcutil. exe para exportar metadados do código de serviço compilado](how-to-use-svcutil-exe-to-export-metadata-from-compiled-service-code.md).  
  
## <a name="publishing-metadata-using-a-secure-binding"></a>Publicando metadados usando uma associação segura  
 As associações de metadados padrão fornecidas pelo WCF não são seguras e permitem o acesso anônimo aos metadados. Os metadados de serviço que um serviço WCF publica contêm uma descrição detalhada sobre o serviço e podem, intencionalmente ou não, conter informações confidenciais. Por exemplo, os metadados de serviço podem conter informações sobre as operações de infraestrutura que não se destinam a serem transmitidas publicamente. Para proteger os metadados de serviço contra acesso não autorizado, você pode usar uma associação segura para o ponto de extremidade de metadados. Os pontos de extremidade de metadados respondem a solicitações HTTP/GET que podem usar protocolo SSL (SSL) para proteger os metadados. Para obter mais informações, consulte [como: proteger pontos de extremidade de metadados](how-to-secure-metadata-endpoints.md).  
  
 Proteger seus pontos de extremidade de metadados também fornece uma maneira para os solicitantes recuperarem com segurança os metadados de serviço sem o risco de adulteração ou falsificação.  
  
## <a name="using-only-trusted-metadata"></a>Usando apenas metadados confiáveis  
 Você pode usar metadados de serviço para construir automaticamente os componentes de tempo de execução necessários para chamar o serviço. Você também pode usar metadados em tempo de design para desenvolver um aplicativo cliente ou em tempo de execução para atualizar dinamicamente a associação que um cliente usa para chamar um serviço.  
  
 Os metadados de serviço podem ser adulterados ou falsificados quando recuperados de maneira não segura. Os metadados adulterados podem redirecionar o cliente para um serviço mal-intencionado, conter configurações de segurança comprometidas ou conter estruturas XML mal-intencionadas. Os documentos de metadados podem ser grandes e são salvos com frequência no sistema de arquivos. Para proteger contra violação e falsificação, use uma associação segura para solicitar metadados de serviço quando um estiver disponível.  
  
## <a name="using-safe-techniques-for-processing-metadata"></a>Usando técnicas seguras para processar metadados  
 Os metadados de serviço são recuperados frequentemente de um serviço em uma rede usando protocolos padronizados, como WS-MetadataExchange (MEX). Muitos formatos de metadados incluem mecanismos de referência para apontar para metadados adicionais. O <xref:System.ServiceModel.Description.MetadataExchangeClient> tipo processa automaticamente referências para você em documentos WSDL (Web Services Description Language), esquema XML e documentos Mex. O tamanho do <xref:System.ServiceModel.Description.MetadataSet> objeto criado a partir dos metadados recuperados é diretamente proporcional ao <xref:System.ServiceModel.Description.MetadataExchangeClient.MaximumResolvedReferences%2A> valor da <xref:System.ServiceModel.Description.MetadataExchangeClient> instância usada e ao `MaxReceivedMessageSize` valor da associação que está sendo usada por essa <xref:System.ServiceModel.Description.MetadataExchangeClient> instância. Defina essas cotas como valores apropriados, conforme determinado pelo seu cenário.  
  
 No WCF, os metadados de serviço são processados como XML. Ao processar documentos XML, os aplicativos devem se proteger contra estruturas XML mal-intencionadas. Use o <xref:System.Xml.XmlDictionaryReader> com cotas apropriadas ao processar XML e também defina a <xref:System.Xml.XmlTextReader.DtdProcessing%2A> propriedade como <xref:System.Xml.DtdProcessing.Prohibit> .  
  
 O sistema de metadados no WCF é extensível e as extensões de metadados podem ser registradas em seu arquivo de configuração de aplicativo (consulte [estendendo o sistema de metadados](../extending/extending-the-metadata-system.md)). As extensões de metadados podem executar código arbitrário, portanto, você deve proteger seu arquivo de configuração de aplicativo com ACLs (listas de controle de acesso) apropriadas e registrar somente as implementações de extensão de metadados confiáveis.  
  
## <a name="validating-generated-clients"></a>Validando clientes gerados  
 Ao gerar o código do cliente a partir de metadados recuperados de uma fonte que não é confiável, valide o código do cliente gerado para garantir que o cliente gerado esteja em conformidade com as políticas de segurança de aplicativos cliente. Você pode usar um comportamento de validação para verificar as configurações em sua associação de cliente ou inspecionar visualmente o código gerado por ferramentas. Para obter um exemplo de como implementar um cliente que valida comportamentos, consulte [validação do cliente](../samples/client-validation.md).  
  
## <a name="protecting-application-configuration-files"></a>Protegendo arquivos de configuração de aplicativo  
 O arquivo de configuração de aplicativo de um serviço pode controlar como e se os metadados são publicados. É uma boa ideia proteger o arquivo de configuração do aplicativo com ACLs (listas de controle de acesso) apropriadas para garantir que um invasor não possa modificar essas configurações.  
  
## <a name="see-also"></a>Consulte também

- [Como proteger pontos de extremidade de metadados](how-to-secure-metadata-endpoints.md)
- [Segurança](security.md)
