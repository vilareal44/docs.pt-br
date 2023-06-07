---
title: 'Como: usar a proteção de dados'
description: Saiba como usar a proteção de dados acessando a API de proteção de dados (DPAPI) no .NET.
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DPAPI
- encryption [.NET], data protection API
- data [.NET], decryption
- ProtectedMemory class, about ProtectedMemory class
- ProtectedData class, about ProtectedData class
- cryptography [.NET], data protection API
- data protection API [.NET]
- decryption
- data [.NET], encryption
ms.assetid: 606698b0-cb1a-42ca-beeb-0bea34205d20
ms.openlocfilehash: 263a07ddf357734e819fffdd41cdff60657adf15
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557054"
---
# <a name="how-to-use-data-protection"></a>Como: usar a proteção de dados

> [!NOTE]
> Este artigo aplica-se ao Windows.
>
> Para obter informações sobre ASP.NET Core, consulte [proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction).

O .NET fornece acesso à DPAPI (API de proteção de dados), que permite que você criptografe os dados usando as informações da conta de usuário atual ou do computador.  Ao usar o DPAPI, você alivia o problema difícil de gerar e armazenar explicitamente uma chave criptográfica.  
  
Use a <xref:System.Security.Cryptography.ProtectedData> classe para criptografar uma cópia de uma matriz de bytes. Essa funcionalidade está disponível em .NET Framework, .NET Core e .NET 5.  Você pode especificar que os dados criptografados pela conta de usuário atual possam ser descriptografados somente pela mesma conta de usuário ou pode especificar que os dados criptografados pela conta de usuário atual possam ser descriptografados por qualquer conta no computador.  Consulte a <xref:System.Security.Cryptography.DataProtectionScope> enumeração para obter uma descrição detalhada das <xref:System.Security.Cryptography.ProtectedData> opções.  
  
### <a name="to-encrypt-data-to-a-file-or-stream-using-data-protection"></a>Para criptografar dados para um arquivo ou fluxo usando a proteção de dados  
  
1. Criar entropia aleatória.  
  
2. Chame o <xref:System.Security.Cryptography.ProtectedData.Protect%2A> método estático ao passar uma matriz de bytes para criptografar, a entropia e o escopo de proteção de dados.  
  
3. Grave os dados criptografados em um arquivo ou fluxo.  
  
### <a name="to-decrypt-data-from-a-file-or-stream-using-data-protection"></a>Para descriptografar dados de um arquivo ou fluxo usando a proteção de dados  
  
1. Ler os dados criptografados de um arquivo ou fluxo.  
  
2. Chame o <xref:System.Security.Cryptography.ProtectedData.Unprotect%2A> método estático ao passar uma matriz de bytes para descriptografar e o escopo de proteção de dados.  
  
## <a name="example"></a>Exemplo

O exemplo de código a seguir demonstra duas formas de criptografia e descriptografia.  Primeiro, o exemplo de código criptografa e, em seguida, descriptografa uma matriz de bytes na memória.  Em seguida, o exemplo de código criptografa uma cópia de uma matriz de bytes, salva-a em um arquivo, carrega os dados de volta do arquivo e, em seguida, descriptografa os dados.  O exemplo exibe os dados originais, os dados criptografados e os dados descriptografados.

[!code-csharp[DPAPI-HowTO#1](../../../samples/snippets/csharp/VS_Snippets_CLR/DPAPI-HowTO/cs/sample.cs#1)]
[!code-vb[DPAPI-HowTO#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DPAPI-HowTO/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  

Este exemplo compila e executa somente ao direcionar .NET Framework e em execução no Windows.

- Inclua uma referência para `System.Security.dll` .  
  
- Inclua o <xref:System> namespace,, <xref:System.IO> <xref:System.Security.Cryptography> e <xref:System.Text> .  
  
## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.ProtectedMemory>
- <xref:System.Security.Cryptography.ProtectedData>
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
