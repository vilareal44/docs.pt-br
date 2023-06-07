---
title: Assegurando a integridade dos dados com códigos hash
description: Saiba como garantir a integridade dos dados usando códigos de hash no .NET. Um valor de hash é um valor numérico de um comprimento fixo que identifica dados exclusivamente.
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generating hash
- verifying hash codes
- cryptography [.NET], hash
- data integrity
- checking hash codes
- encryption [.NET], hash
- hash
ms.assetid: 33660f33-b70f-4dca-8c87-ab35cfc2961a
ms.openlocfilehash: 3205e37f283cb205f5edfc5948a9cb9f7256f752
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556950"
---
# <a name="ensuring-data-integrity-with-hash-codes"></a>Assegurando a integridade dos dados com códigos hash
Um valor de hash é um valor numérico de um comprimento fixo que identifica dados exclusivamente. Os valores de hash representam grandes quantidades de dados como valores numéricos muito menores, para que sejam usados com assinaturas digitais. Você pode assinar um valor de hash com mais eficiência do que assinar o valor maior. Os valores de hash também são úteis para verificar a integridade dos dados enviados por meio de canais inseguros. O valor de hash dos dados recebidos pode ser comparado ao valor de hash dos dados conforme eles foram enviados para determinar se os dados foram alterados.  
  
Este tópico descreve como gerar e verificar os códigos de hash usando as classes no <xref:System.Security.Cryptography> namespace.  
  
## <a name="generating-a-hash"></a>Gerando um hash

 As classes de hash gerenciadas podem aplicar hash a uma matriz de bytes ou a um objeto de fluxo gerenciado. O exemplo a seguir usa o algoritmo de hash SHA1 para criar um valor de hash para uma cadeia de caracteres. O exemplo usa a <xref:System.Text.UnicodeEncoding> classe para converter a cadeia de caracteres em uma matriz de bytes com hash usando a <xref:System.Security.Cryptography.SHA256> classe. O valor de hash é exibido para o console.  

 [!code-csharp[GeneratingAHash#1](../../../samples/snippets/csharp/VS_Snippets_CLR/generatingahash/cs/program.cs#1)]
 [!code-vb[GeneratingAHash#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/generatingahash/vb/program.vb#1)]  
  
 Esse código exibirá a seguinte cadeia de caracteres para o console:  
  
 `185 203 236 22 3 228 27 130 87 23 244 15 87 88 14 43 37 61 106 224 81 172 224 211 104 85 194 197 194 25 120 217`  
  
## <a name="verifying-a-hash"></a>Verificando um hash

 Os dados podem ser comparados a um valor de hash para determinar sua integridade. Normalmente, os dados são codificados em hash em um determinado momento e o valor de hash é protegido de alguma forma. Mais tarde, os dados podem ser codificados novamente e comparados com o valor protegido. Se os valores de hash forem correspondentes, os dados não foram alterados. Se os valores não corresponderem, os dados ficarão corrompidos. Para que esse sistema funcione, o hash protegido deve ser criptografado ou mantido em segredo de todas as partes não confiáveis.  
  
 O exemplo a seguir compara o valor de hash anterior de uma cadeia de caracteres com um novo valor de hash. Este exemplo percorre cada byte dos valores de hash e faz uma comparação.  
  
 [!code-csharp[VerifyingAHash#1](../../../samples/snippets/csharp/VS_Snippets_CLR/verifyingahash/cs/program.cs#1)]
 [!code-vb[VerifyingAHash#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/verifyingahash/vb/program.vb#1)]  
  
 Se os dois valores de hash corresponderem, esse código exibirá o seguinte no console:  
  
```console  
The hash codes match.  
```  
  
 Se eles não corresponderem, o código exibirá o seguinte:  
  
```console  
The hash codes do not match.  
```  
  
## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
