---
title: Como usar o ChannelFactory
description: Saiba como criar uma fábrica de canais para criar mais de um canal para acessar serviços usando um cliente WCF.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d48f01b5-582b-4c8b-b547-8adddae7e371
ms.openlocfilehash: 7c87026ca4cf7c739f4615da9bc7f0272a382392
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246656"
---
# <a name="how-to-use-the-channelfactory"></a>Como usar o ChannelFactory
A <xref:System.ServiceModel.ChannelFactory%601> classe genérica é usada em cenários avançados que exigem a criação de uma fábrica de canais que pode ser usada para criar mais de um canal.  
  
### <a name="to-create-and-use-the-channelfactory-class"></a>Para criar e usar a classe ChannelFactory  
  
1. Compilar e executar um serviço de Windows Communication Foundation (WCF). Para obter mais informações, consulte [projetando e implementando](../designing-and-implementing-services.md)serviços, [Configurando serviços](../configuring-services.md)e [hospedando serviços](../hosting-services.md).  
  
2. Use a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar o contrato (interface) para o cliente.  
  
3. No código do cliente, use a <xref:System.ServiceModel.ChannelFactory%601> classe para criar vários ouvintes de ponto de extremidade.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[c_HowToUseChannelFactory#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtousechannelfactory/cs/source.cs#1)]
 [!code-vb[c_HowToUseChannelFactory#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtousechannelfactory/vb/source.vb#1)]
