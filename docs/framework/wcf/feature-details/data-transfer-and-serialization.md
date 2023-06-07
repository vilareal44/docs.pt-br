---
title: Serialização e transferência de dados
ms.date: 03/30/2017
helpviewer_keywords:
- data serialization [WCF]
- data transfer [WCF]
ms.assetid: 0f03c635-f3e7-4c5c-9463-3cb0135e221e
ms.openlocfilehash: b07937b0a94c24a934b17d6cf21b726ee0d4362e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593474"
---
# <a name="data-transfer-and-serialization"></a>Serialização e transferência de dados
Em um sistema conectado, os serviços e os clientes dependem da troca de dados para realizar qualquer tarefa. Como desenvolvedor de um serviço ou cliente, você também deve entender como a Windows Communication Foundation (WCF) lida com dados e a serialização de dados para criar aplicativos que são eficientes e fáceis de manter.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Especificando transferência de dados em contratos de serviço](specifying-data-transfer-in-service-contracts.md)  
 Descreve os conceitos básicos de transferência de dados em serviços.  
  
 [Usando contratos de dados](using-data-contracts.md)  
 Descreve o que são contratos de dados e como criá-los e usá-los.  
  
 [Serializador de contrato de dados](data-contract-serializer.md)  
 Descreve como realizar a serialização de dados com a <xref:System.Runtime.Serialization.DataContractSerializer> classe ou qualquer extensão da <xref:System.Runtime.Serialization.XmlObjectSerializer> classe.  
  
 [Usando a classe XmlSerializer](using-the-xmlserializer-class.md)  
 Descreve como e por que usar a <xref:System.Xml.Serialization.XmlSerializer> classe, uma alternativa para a <xref:System.Runtime.Serialization.DataContractSerializer> classe.  
  
 [Utilizando contratos de mensagem](using-message-contracts.md)  
 Descreve como os contratos de mensagem permitem um controle fino sobre mensagens SOAP.  
  
 [Usando a classe de mensagens](using-the-message-class.md)  
 Descreve como usar recursos de classe de mensagem.  
  
 [Filtragem](filtering.md)  
 Descreve a filtragem, que habilita o pré-processamento de uma mensagem com base em vários critérios.  
  
 [Dados grandes e streaming](large-data-and-streaming.md)  
 Descreve como enviar um grande bloco de dados, como um arquivo binário.  
  
 [Considerações de segurança para dados](security-considerations-for-data.md)  
 Descreve os itens que estão cientes do ao programar a transferência e a serialização de dados.  
  
 [Visão geral da arquitetura de transferência de dados](data-transfer-architectural-overview.md)  
 Descreve uma exibição do design geral de transferência de dados no WCF.  
  
## <a name="reference"></a>Referência  
 <xref:System.ServiceModel>  
  
 <xref:System.Runtime.Serialization.DataContractSerializer>  
  
 <xref:System.Xml.Serialization.XmlSerializer>  
  
 <xref:System.Runtime.Serialization>  
  
 <xref:System.Xml.Serialization>  
  
## <a name="related-sections"></a>Seções relacionadas  
 [Estendendo codificadores e serializadores](../extending/extending-encoders-and-serializers.md)  
  
## <a name="see-also"></a>Consulte também

- [Práticas recomendadas: controle de versão de contrato de dados](../best-practices-data-contract-versioning.md)
- [Controle de versão de serviço](../service-versioning.md)
