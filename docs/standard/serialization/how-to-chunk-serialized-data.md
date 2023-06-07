---
title: Como partir dados serializados
description: Você pode dividir os dados para evitar problemas com grandes conjuntos. Implemente a interface IXmlSerializable para controlar a serialização e a desserialização.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- chunking serialized data
- data chunking
- binary serialization, chunking data
- large data set chunking
- serialization, chunking data
- serialization, examples
- binary serialization, examples
ms.assetid: 22f1b818-7e0d-428a-8680-f17d6ebdd185
ms.openlocfilehash: 860fdcae0d1937f53ee964d9d4631ec812b3d379
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "83379136"
---
# <a name="how-to-chunk-serialized-data"></a>Como partir dados serializados

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

Dois problemas que ocorrem ao enviar grandes conjuntos de dados em mensagens de serviço Web são:  
  
1. Um grande conjunto de trabalho (memória) devido ao armazenamento em buffer pelo mecanismo de serialização.  
  
2. Consumo de largura de banda desordenado devido à inflação de 33 por cento após a codificação Base64.  
  
 Para resolver esses problemas, implemente a interface <xref:System.Xml.Serialization.IXmlSerializable> para controlar a serialização e a desserialização. Especificamente, implemente os métodos <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> e <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> para partir os dados.  
  
### <a name="to-implement-server-side-chunking"></a>Para implementar o agrupamento do lado do servidor  
  
1. Na música do servidor, o método da Web deve desativar o buffer do ASP.NET e retornar um tipo que implementa <xref:System.Xml.Serialization.IXmlSerializable>.  
  
2. O tipo que implementa <xref:System.Xml.Serialization.IXmlSerializable> partir os dados no método <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A>.  
  
### <a name="to-implement-client-side-processing"></a>Para implementar o processamento do lado do cliente  
  
1. Altere o método da Web no proxy do cliente para retornar o tipo que implementa <xref:System.Xml.Serialization.IXmlSerializable>. Você pode usar uma <xref:System.Xml.Serialization.Advanced.SchemaImporterExtension> para fazer isso automaticamente, mas isso não é mostrado aqui.  
  
2. Implemente o método <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> para ler o fluxo de dados em partes e gravar os bytes no disco. Essa implementação também gera eventos de progresso que podem ser usados por um controle de gráfico, como uma barra de progresso.  
  
## <a name="example"></a>Exemplo  
O exemplo de código a seguir mostra o método da Web no cliente que desativa o buffering do ASP.NET. Ele também mostra a implementação do lado do cliente da interface <xref:System.Xml.Serialization.IXmlSerializable> que parte os dados no método <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A>.  
  
[!code-csharp[HowToChunkSerializedData#1](../../../samples/snippets/csharp/VS_Snippets_Remoting/HowToChunkSerializedData/CS/SerializationChunk.cs#1)]
[!code-vb[HowToChunkSerializedData#1](../../../samples/snippets/visualbasic/VS_Snippets_Remoting/HowToChunkSerializedData/VB/SerializationChunk.vb#1)]  
[!code-csharp[HowToChunkSerializedData#2](../../../samples/snippets/csharp/VS_Snippets_Remoting/HowToChunkSerializedData/CS/SerializationChunk.cs#2)]
[!code-vb[HowToChunkSerializedData#2](../../../samples/snippets/visualbasic/VS_Snippets_Remoting/HowToChunkSerializedData/VB/SerializationChunk.vb#2)]  
[!code-csharp[HowToChunkSerializedData#3](../../../samples/snippets/csharp/VS_Snippets_Remoting/HowToChunkSerializedData/CS/SerializationChunk.cs#3)]
[!code-vb[HowToChunkSerializedData#3](../../../samples/snippets/visualbasic/VS_Snippets_Remoting/HowToChunkSerializedData/VB/SerializationChunk.vb#3)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- O código usa os seguintes namespaces: <xref:System>, <xref:System.Runtime.Serialization>, <xref:System.Web.Services>, <xref:System.Web.Services.Protocols>, <xref:System.Xml>, <xref:System.Xml.Serialization> e <xref:System.Xml.Schema>.  
  
## <a name="see-also"></a>Confira também

- [Serialização personalizada](custom-serialization.md)
