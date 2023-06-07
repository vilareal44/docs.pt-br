---
title: Como fazer referência de forma consistente aos certificados X.509
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates [WCF], referencing X.509 certificates
ms.assetid: a6de1c63-e450-4640-ad08-ad7302dbfbfc
ms.openlocfilehash: c13dd5ebb18df62ce64fc74da53f3f5a2e8cadb7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599081"
---
# <a name="how-to-consistently-reference-x509-certificates"></a>Como fazer referência de forma consistente aos certificados X.509
Você pode identificar um certificado de várias maneiras: pelo hash do certificado, pelo emissor e número de série, ou pelo identificador de chave da entidade (esqui). A esqui fornece uma identificação exclusiva para a chave pública da entidade do certificado e é geralmente usada ao trabalhar com a assinatura digital XML. O valor de esqui geralmente faz parte do certificado X. 509 como uma *extensão de certificado x. 509*. Windows Communication Foundation (WCF) tem um *estilo de referência* padrão que usa o emissor e o número de série se a extensão de esqui está ausente do certificado. Se o certificado contiver a extensão de esqui, o estilo de referência padrão usará a esqui para apontar para o certificado. Se for o meio-caminho do desenvolvimento de um aplicativo, você alternar do uso de certificados que não usam a extensão de esqui para certificados que usam a extensão de esqui, o estilo de referência usado em mensagens geradas pelo WCF também é alterado.  
  
 Se um estilo de referência consistente for necessário independentemente da presença da extensão de esqui, será possível configurar o estilo de referência desejado, conforme mostrado no código a seguir.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um elemento de associação de segurança personalizado que usa um único estilo de referência consistente, o nome do emissor e o número de série.  
  
 [!code-csharp[c_ReferencingCertificatesConsistently#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_referencingcertificatesconsistently/cs/source.cs#1)]
 [!code-vb[c_ReferencingCertificatesConsistently#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_referencingcertificatesconsistently/vb/source.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
 Os namespaces a seguir são necessários para compilar o código:  
  
- <xref:System>  
  
- <xref:System.ServiceModel>  
  
- <xref:System.ServiceModel.Channels>  
  
- <xref:System.ServiceModel.Security.Tokens>  
  
## <a name="see-also"></a>Consulte também

- [Trabalhando com certificados](working-with-certificates.md)
