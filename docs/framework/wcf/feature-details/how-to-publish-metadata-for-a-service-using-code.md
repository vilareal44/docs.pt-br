---
title: Como publicar metadados utilizando código para um serviço
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 51407e6d-4d87-42d5-be7c-9887b8652006
ms.openlocfilehash: 9239e8bd9b85986d41006c4b2a21b6f2304e8275
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601225"
---
# <a name="how-to-publish-metadata-for-a-service-using-code"></a>Como publicar metadados utilizando código para um serviço
Este é um dos dois tópicos de instruções que abordam a publicação de metadados para um serviço Windows Communication Foundation (WCF). Há duas maneiras de especificar como um serviço deve publicar metadados, usando um arquivo de configuração e usando código. Este tópico mostra como publicar metadados para um serviço usando um código.  
  
> [!CAUTION]
> Este tópico mostra como publicar metadados de maneira não segura. Qualquer cliente pode recuperar os metadados do serviço. Se você precisar que seu serviço publique metadados de maneira segura. consulte [ponto de extremidade de metadados seguro personalizado](../samples/custom-secure-metadata-endpoint.md).  
  
 Para obter mais informações sobre como publicar metadados em um arquivo de configuração, consulte [como: publicar metadados para um serviço usando um arquivo de configuração](how-to-publish-metadata-for-a-service-using-a-configuration-file.md). A publicação de metadados permite que os clientes recuperem os metadados usando uma solicitação WS-Transfer GET ou uma solicitação HTTP/GET usando a `?wsdl` cadeia de caracteres de consulta. Para ter certeza de que o código está funcionando, você deve criar um serviço WCF básico. Um serviço auto-hospedado básico é fornecido no código a seguir.  
  
 [!code-csharp[htPublishMetadataCode#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#0)]
 [!code-vb[htPublishMetadataCode#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#0)]  
  
### <a name="to-publish-metadata-in-code"></a>Para publicar metadados no código  
  
1. Dentro do método principal de um aplicativo de console, crie uma instância de um <xref:System.ServiceModel.ServiceHost> objeto passando o tipo de serviço e o endereço base.  
  
     [!code-csharp[htPublishMetadataCode#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#1)]
     [!code-vb[htPublishMetadataCode#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#1)]  
  
2. Crie um bloco try imediatamente abaixo do código da etapa 1. isso captura todas as exceções que são geradas durante a execução do serviço.  
  
     [!code-csharp[htPublishMetadataCode#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#2)]
     [!code-vb[htPublishMetadataCode#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#2)]  
  
     [!code-csharp[htPublishMetadataCode#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#3)]
     [!code-vb[htPublishMetadataCode#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#3)]  
  
3. Verifique se o host de serviço já contém um <xref:System.ServiceModel.Description.ServiceMetadataBehavior> , se não, criar uma nova <xref:System.ServiceModel.Description.ServiceMetadataBehavior> instância.  
  
     [!code-csharp[htPublishMetadataCode#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#4)]
     [!code-vb[htPublishMetadataCode#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#4)]  
  
4. Defina a propriedade <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled%2A> como `true.`  
  
     [!code-csharp[htPublishMetadataCode#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#5)]
     [!code-vb[htPublishMetadataCode#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#5)]  
  
5. O <xref:System.ServiceModel.Description.ServiceMetadataBehavior> contém uma <xref:System.ServiceModel.Description.MetadataExporter> propriedade. O <xref:System.ServiceModel.Description.MetadataExporter> contém uma <xref:System.ServiceModel.Description.MetadataExporter.PolicyVersion%2A> propriedade. Defina o valor da <xref:System.ServiceModel.Description.MetadataExporter.PolicyVersion%2A> propriedade como <xref:System.ServiceModel.Description.PolicyVersion.Policy15%2A> . A <xref:System.ServiceModel.Description.MetadataExporter.PolicyVersion%2A> propriedade também pode ser definida como <xref:System.ServiceModel.Description.PolicyVersion.Policy12%2A> . Quando definido como <xref:System.ServiceModel.Description.PolicyVersion.Policy15%2A> o exportador de metadados gera informações de política com os metadados que estão em conformidade com ws-policy 1,5. Quando definido como <xref:System.ServiceModel.Description.PolicyVersion.Policy12%2A> o exportador de metadados gera informações de política que estão em conformidade com ws-policy 1,2.  
  
     [!code-csharp[htPublishMetadataCode#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#6)]
     [!code-vb[htPublishMetadataCode#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#6)]  
  
6. Adicione a <xref:System.ServiceModel.Description.ServiceMetadataBehavior> instância à coleção de comportamentos do host de serviço.  
  
     [!code-csharp[htPublishMetadataCode#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#7)]
     [!code-vb[htPublishMetadataCode#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#7)]  
  
7. Adicione o ponto de extremidade de troca de metadados ao host de serviço.  
  
     [!code-csharp[htPublishMetadataCode#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#8)]
     [!code-vb[htPublishMetadataCode#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#8)]  
  
8. Adicione um ponto de extremidade de aplicativo ao host de serviço.  
  
     [!code-csharp[htPublishMetadataCode#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#9)]
     [!code-vb[htPublishMetadataCode#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#9)]  
  
    > [!NOTE]
    > Se você não adicionar nenhum ponto de extremidade ao serviço, o runtime adicionará pontos de extremidade padrão para você. Neste exemplo, como o serviço tem um <xref:System.ServiceModel.Description.ServiceMetadataBehavior> definido como `true` , o serviço tem metadados de publicação habilitados. Para obter mais informações sobre pontos de extremidade padrão, consulte [configuração simplificada](../simplified-configuration.md) e [configuração simplificada para serviços WCF](../samples/simplified-configuration-for-wcf-services.md).  
  
9. Abra o host de serviço e aguarde as chamadas de entrada. Quando o usuário pressionar ENTER, feche o host de serviço.  
  
     [!code-csharp[htPublishMetadataCode#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#10)]
     [!code-vb[htPublishMetadataCode#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#10)]  
  
10. Crie e execute o aplicativo de console.  
  
11. Use o Internet Explorer para navegar até o endereço base do serviço ( `http://localhost:8001/MetadataSample` neste exemplo) e verificar se a publicação de metadados está ativada. Você deve ver uma página da Web exibida dizendo "serviço simples" na parte superior e imediatamente abaixo de "você criou um serviço". Caso contrário, uma mensagem na parte superior da página resultante será exibida: "a publicação de metadados para este serviço está atualmente desabilitada".  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra a implementação de um serviço WCF básico que publica metadados para o serviço no código.  
  
 [!code-csharp[htPublishMetadataCode#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/htpublishmetadatacode/cs/program.cs#11)]
 [!code-vb[htPublishMetadataCode#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/htpublishmetadatacode/vb/program.vb#11)]  
  
## <a name="see-also"></a>Consulte também

- [Como hospedar um serviço do WCF em um aplicativo gerenciado](../how-to-host-a-wcf-service-in-a-managed-application.md)
- [Self-Host](../samples/self-host.md)
- [Visão geral da arquitetura de metadados](metadata-architecture-overview.md)
- [Utilizando metadados](using-metadata.md)
- [Como publicar metadados para um serviço usando um arquivo de configuração](how-to-publish-metadata-for-a-service-using-a-configuration-file.md)
