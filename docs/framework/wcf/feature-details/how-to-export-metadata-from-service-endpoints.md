---
title: Como exportar metadados para pontos de extremidade de serviço
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b6c4dfd0-f270-43ec-961a-e16eb6af2f2c
ms.openlocfilehash: 58e86e5566775048e081bfb4ac217a7747b98a35
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579404"
---
# <a name="how-to-export-metadata-from-service-endpoints"></a>Como exportar metadados para pontos de extremidade de serviço
Este tópico explica como exportar metadados de pontos de extremidade de serviço.  
  
### <a name="to-export-metadata-from-service-endpoints"></a>Para exportar metadados de pontos de extremidade de serviço  
  
1. Crie um novo projeto de aplicativo de console do Visual Studio. Adicione o código mostrado nas etapas a seguir no arquivo Program.cs gerado dentro do método Main ().  
  
2. Criará um <xref:System.ServiceModel.Description.WsdlExporter>.  
  
     [!code-csharp[S_UEWsdlExporter#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#1)]
     [!code-vb[S_UEWsdlExporter#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#1)]  
  
3. Defina a <xref:System.ServiceModel.Description.MetadataExporter.PolicyVersion%2A> propriedade como um dos valores da <xref:System.ServiceModel.Description.PolicyVersion> enumeração. Este exemplo define o valor para o <xref:System.ServiceModel.Description.PolicyVersion.Policy15%2A> qual corresponde a WS-Policy 1,5.  
  
     [!code-csharp[S_UEWsdlExporter#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#2)]
     [!code-vb[S_UEWsdlExporter#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#2)]  
  
4. Crie uma matriz de <xref:System.ServiceModel.Description.ServiceEndpoint> objetos.  
  
     [!code-csharp[S_UEWsdlExporter#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#3)]
     [!code-vb[S_UEWsdlExporter#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#3)]  
  
5. Exportar metadados para cada ponto de extremidade de serviço.  
  
     [!code-csharp[S_UEWsdlExporter#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#4)]
     [!code-vb[S_UEWsdlExporter#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#4)]  
  
6. Verifique se não ocorreu nenhum erro durante o processo de exportação e recupere os metadados.  
  
     [!code-csharp[S_UEWsdlExporter#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#5)]
     [!code-vb[S_UEWsdlExporter#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#5)]  
  
7. Agora você pode usar os metadados, como gravá-los em um arquivo chamando o <xref:System.ServiceModel.Description.MetadataSet.WriteTo%28System.Xml.XmlWriter%29> método.  
  
## <a name="example"></a>Exemplo  
 A seguir está a listagem completa de códigos deste exemplo.  
  
 [!code-csharp[S_UEWsdlExporter#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_uewsdlexporter/cs/program.cs#0)]
 [!code-vb[S_UEWsdlExporter#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_uewsdlexporter/vb/program.vb#0)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Ao compilar o Program.cs Reference System. ServiceModel. dll.  
  
## <a name="see-also"></a>Consulte também

- [Visão geral da arquitetura de metadados](metadata-architecture-overview.md)
- [Utilizando metadados](using-metadata.md)
- [Pontos de extremidade: endereços, associações e contratos](endpoints-addresses-bindings-and-contracts.md)
