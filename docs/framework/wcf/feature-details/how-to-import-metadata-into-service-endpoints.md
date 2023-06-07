---
title: Como importar metadados para pontos de extremidade de serviço
ms.date: 03/30/2017
ms.assetid: b69dbe20-92a1-4911-89d8-ffbc3dad4663
ms.openlocfilehash: 1de316b8e91739d5e3e24ff960e2cdfb33cc7fab
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597053"
---
# <a name="how-to-import-metadata-into-service-endpoints"></a>Como importar metadados para pontos de extremidade de serviço
Este tópico explica como importar metadados para uma coleção de pontos de extremidade de serviço e usar o serviço definido no [introdução](../samples/getting-started-sample.md). Este tópico mostra como criar um aplicativo cliente que importa os metadados do serviço e, em seguida, chama o `Add` método no serviço.  
  
### <a name="to-import-metadata-into-service-endpoints"></a>Para importar metadados em pontos de extremidade de serviço  
  
1. Declare um <xref:System.ServiceModel.EndpointAddress> objeto e inicialize-o com o Uniform Resource Identifier (URI) para o endereço de intercâmbio de metadados (MEX) do serviço.  
  
     [!code-csharp[UE_ImportMetadata#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/ue_importmetadata/cs/client.cs#0)]  
  
2. Crie um <xref:System.ServiceModel.Description.MetadataExchangeClient> , passando o endereço MEX e chame <xref:System.ServiceModel.Description.MetadataExchangeClient.GetMetadata%2A> . Isso recupera os metadados do serviço.  
  
     [!code-csharp[UE_ImportMetadata#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/ue_importmetadata/cs/client.cs#1)]  
  
3. Crie um <xref:System.ServiceModel.Description.WsdlImporter> , passando os metadados recuperados anteriormente e chame <xref:System.ServiceModel.Description.WsdlImporter.ImportAllContracts%2A> . Isso gera uma coleção de <xref:System.ServiceModel.Description.ContractDescription> objetos. Você também pode chamar <xref:System.ServiceModel.Description.WsdlImporter.ImportAllEndpoints%2A> ou <xref:System.ServiceModel.Description.WsdlImporter.ImportAllBindings%2A> , dependendo das suas necessidades.  
  
     [!code-csharp[UE_ImportMetadata#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/ue_importmetadata/cs/client.cs#2)]  
  
    > [!NOTE]
    > Depois de importar os metadados, você não poderá criar um canal de cliente nem exportar os metadados. Isso ocorre porque nenhuma informação de tipo está disponível neste ponto. As informações de tipo são necessárias para realmente interagir com o serviço ou exportar metadados. Para gerar as informações de tipo, você precisa gerar o código, mostrado nas etapas 4 e 5. Como alternativa, você pode usar a <xref:System.ServiceModel.Description.MetadataResolver> classe auxiliar. Para obter mais informações, consulte [como: usar MetadataResolver para obter metadados de associação dinamicamente](how-to-use-metadataresolver-to-obtain-binding-metadata-dynamically.md).  
  
4. Gerar informações de tipo para cada contrato.  
  
     [!code-csharp[UE_ImportMetadata#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/ue_importmetadata/cs/client.cs#3)]  
  
5. Agora você pode usar essas informações. O exemplo a seguir gera código-fonte C#.  
  
     [!code-csharp[UE_ImportMetadata#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/ue_importmetadata/cs/client.cs#4)]  
  
## <a name="see-also"></a>Consulte também

- [Metadados](metadata.md)
- [Introdução](../samples/getting-started-sample.md)
