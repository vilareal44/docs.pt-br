---
title: 'Como: acessar dispositivos de criptografia de hardware'
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- encryption
- key card
- cryptography
- hardware encryption
- CspParameters
ms.assetid: b0e734df-6eb4-4b16-b48c-6f0fe82d5f17
ms.openlocfilehash: 7cd3aab80a8388c1d4ce08e4ae94aae84cfff239
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557132"
---
# <a name="how-to-access-hardware-encryption-devices"></a>Como: acessar dispositivos de criptografia de hardware

> [!NOTE]
> Este artigo aplica-se ao Windows.

Você pode usar a <xref:System.Security.Cryptography.CspParameters> classe para acessar dispositivos de criptografia de hardware. Por exemplo, você pode usar essa classe para integrar seu aplicativo a um cartão inteligente, um gerador de números aleatórios de hardware ou uma implementação de hardware de um algoritmo criptográfico específico.  

A <xref:System.Security.Cryptography.CspParameters> classe cria um CSP (provedor de serviços de criptografia) que acessa um dispositivo de criptografia de hardware instalado corretamente.  Você pode verificar a disponibilidade de um CSP inspecionando a seguinte chave do registro usando o editor do registro (Regedit.exe): HKEY_LOCAL_MACHINE \Software\Microsoft\Cryptography\Defaults\Provider.  
  
### <a name="to-sign-data-using-a-key-card"></a>Para assinar dados usando um cartão-chave  
  
1. Crie uma nova instância da <xref:System.Security.Cryptography.CspParameters> classe, passando o tipo de provedor de inteiro e o nome do provedor para o construtor.  
  
2. Passe os sinalizadores apropriados para a <xref:System.Security.Cryptography.CspParameters.Flags%2A> Propriedade do <xref:System.Security.Cryptography.CspParameters> objeto recém-criado.  Por exemplo, passe o <xref:System.Security.Cryptography.CspProviderFlags.UseDefaultKeyContainer> sinalizador.  
  
3. Crie uma nova instância de uma <xref:System.Security.Cryptography.AsymmetricAlgorithm> classe (por exemplo, a <xref:System.Security.Cryptography.RSACryptoServiceProvider> classe), passando o <xref:System.Security.Cryptography.CspParameters> objeto para o construtor.  
  
4. Assine seus dados usando um dos `Sign` métodos e verifique seus dados usando um dos `Verify` métodos.  
  
### <a name="to-generate-a-random-number-using-a-hardware-random-number-generator"></a>Para gerar um número aleatório usando um gerador de número aleatório de hardware  
  
1. Crie uma nova instância da <xref:System.Security.Cryptography.CspParameters> classe, passando o tipo de provedor de inteiro e o nome do provedor para o construtor.  
  
2. Crie uma nova instância do <xref:System.Security.Cryptography.RNGCryptoServiceProvider> , passando o <xref:System.Security.Cryptography.CspParameters> objeto para o construtor.  
  
3. Crie um valor aleatório usando o <xref:System.Security.Cryptography.RNGCryptoServiceProvider.GetBytes%2A> <xref:System.Security.Cryptography.RNGCryptoServiceProvider.GetNonZeroBytes%2A> método ou.  
  
## <a name="example"></a>Exemplo

O exemplo de código a seguir demonstra como assinar dados usando um cartão inteligente.  O exemplo de código cria um <xref:System.Security.Cryptography.CspParameters> objeto que expõe um cartão inteligente e inicializa um <xref:System.Security.Cryptography.RSACryptoServiceProvider> objeto usando o CSP.  O exemplo de código, em seguida, assina e verifica alguns dados.  

Devido a problemas de colisão com SHA1, recomendamos SHA256 ou melhor.
  
[!code-cpp[Cryptography.SmartCardCSP#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Cryptography.SmartCardCSP/CPP/Cryptography.SmartCardCSP.cpp#1)]
[!code-csharp[Cryptography.SmartCardCSP#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Cryptography.SmartCardCSP/CS/example.cs#1)]
[!code-vb[Cryptography.SmartCardCSP#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Cryptography.SmartCardCSP/VB/example.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Inclua os <xref:System> <xref:System.Security.Cryptography> namespaces e.  
  
- Você deve ter um leitor de cartão inteligente e drivers instalados no seu computador.  
  
- Você deve inicializar o <xref:System.Security.Cryptography.CspParameters> objeto usando informações específicas para o leitor de cartão.  Para obter mais informações, consulte a documentação do leitor de cartão.

## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
