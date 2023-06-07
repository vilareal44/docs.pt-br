---
title: Modelo de criptografia .NET
description: Examine as implementações de algoritmos de criptografia usuais no .NET. Aprenda o modelo de criptografia extensível de herança de objeto, design de fluxo & configuração.
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [.NET], model
- encryption [.NET], model
ms.assetid: 12fecad4-fbab-432a-bade-2f05976a2971
ms.openlocfilehash: 0b3e07238bf0932572c222f7b947cfa7ae0221a9
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556989"
---
# <a name="net-cryptography-model"></a>Modelo de criptografia .NET

O .NET fornece implementações de muitos algoritmos criptográficos padrão, e o modelo de criptografia .NET é extensível.

## <a name="object-inheritance"></a>Herança de objetos

O sistema de criptografia .NET implementa um padrão extensível de herança de classe derivada. A hierarquia é a seguinte:

- Classe de tipo de algoritmo, como <xref:System.Security.Cryptography.SymmetricAlgorithm> , <xref:System.Security.Cryptography.AsymmetricAlgorithm> ou <xref:System.Security.Cryptography.HashAlgorithm> . Esse nível é abstrato.

- Classe de algoritmo que herda de uma classe de tipo de algoritmo; por exemplo, <xref:System.Security.Cryptography.Aes> , <xref:System.Security.Cryptography.RSA> ou <xref:System.Security.Cryptography.ECDiffieHellman> . Esse nível é abstrato.

- Implementação de uma classe de algoritmo que herda de uma classe de algoritmo; por exemplo, <xref:System.Security.Cryptography.AesManaged> , <xref:System.Security.Cryptography.RC2CryptoServiceProvider> ou <xref:System.Security.Cryptography.ECDiffieHellmanCng> . Esse nível é totalmente implementado.

Esse padrão de classes derivadas permite que você adicione um novo algoritmo ou uma nova implementação de um algoritmo existente. Por exemplo, para criar um novo algoritmo Public-Key, você herdaria da <xref:System.Security.Cryptography.AsymmetricAlgorithm> classe. Para criar uma nova implementação de um algoritmo específico, você deve criar uma classe derivada não abstrata desse algoritmo.

## <a name="how-algorithms-are-implemented-in-net"></a>Como os algoritmos são implementados no .NET

Como um exemplo das diferentes implementações disponíveis para um algoritmo, considere algoritmos simétricos. A base para todos os algoritmos simétricos é <xref:System.Security.Cryptography.SymmetricAlgorithm> herdada por <xref:System.Security.Cryptography.Aes> , <xref:System.Security.Cryptography.TripleDES> e outras que não são mais recomendadas.

<xref:System.Security.Cryptography.Aes>é herdado por <xref:System.Security.Cryptography.AesCryptoServiceProvider> , <xref:System.Security.Cryptography.AesCng> e <xref:System.Security.Cryptography.AesManaged> .

Em .NET Framework no Windows:

* `*CryptoServiceProvider`as classes de algoritmo, como <xref:System.Security.Cryptography.AesCryptoServiceProvider> , são wrappers em relação à implementação da capi (API de criptografia do Windows) de um algoritmo.
* `*Cng`classes de algoritmo, como <xref:System.Security.Cryptography.ECDiffieHellmanCng> os wrappers em relação à implementação da CNG (criptografia próxima geração) do Windows.
* `*Managed`classes, como <xref:System.Security.Cryptography.AesManaged> , são inteiramente escritas em código gerenciado. `*Managed`as implementações não são certificadas pelo FIPS (Federal Information Processing Standards) e podem ser mais lentas do que as `*CryptoServiceProvider` `*Cng` classes wrapper.

No .NET Core e no .NET 5 e versões posteriores, todas as classes de implementação ( `*CryptoServiceProvider` , `*Managed` e `*Cng` ) são wrappers para os algoritmos do sistema operacional (SO). Se os algoritmos do sistema operacional forem certificados pelo FIPS, o .NET usará algoritmos com certificação FIPS. Para obter mais informações, consulte [criptografia de plataforma cruzada](cross-platform-cryptography.md).

Na maioria dos casos, você não precisa fazer referência diretamente a uma classe de implementação de algoritmo, como `AesCryptoServiceProvider` . Os métodos e as propriedades que você normalmente precisa estão na classe de algoritmo base, como `Aes` . Crie uma instância de uma classe de implementação padrão usando um método de fábrica na classe de algoritmo base e consulte a classe de algoritmo base. Por exemplo, consulte a linha de código realçada no exemplo a seguir:

:::code language="csharp" source="snippets/encrypting-data/csharp/aes-encrypt.cs" highlight="16":::
:::code language="vb" source="snippets/encrypting-data/vb/aes-encrypt.vb" highlight="12":::

## <a name="cryptographic-configuration"></a>Configuração criptográfica

A configuração criptográfica permite que você resolva uma implementação específica de um algoritmo para um nome de algoritmo, permitindo a extensibilidade das classes de criptografia do .NET. Você pode adicionar sua própria implementação de hardware ou software de um algoritmo e mapear a implementação para o nome do algoritmo de sua escolha. Se um algoritmo não for especificado no arquivo de configuração, as configurações padrão serão usadas.

## <a name="choosing-an-algorithm"></a>Escolhendo um algoritmo

Você pode selecionar um algoritmo por diferentes motivos: por exemplo, para a integridade dos dados, para privacidade de dados ou para gerar uma chave. Algoritmos simétricos e de hash destinam-se à proteção de dados por motivos de integridade (proteção contra alterações) ou por motivos de privacidade (proteção contra a exibição). Algoritmos de hash são usados principalmente para integridade de dados.

Aqui está uma lista de algoritmos recomendados por aplicativo:

- Privacidade de dados:
  - <xref:System.Security.Cryptography.Aes>
- Integridade dos dados:
  - <xref:System.Security.Cryptography.HMACSHA256>
  - <xref:System.Security.Cryptography.HMACSHA512>
- Assinatura digital:
  - <xref:System.Security.Cryptography.ECDsa>
  - <xref:System.Security.Cryptography.RSA>
- Troca de chaves:
  - <xref:System.Security.Cryptography.ECDiffieHellman>
  - <xref:System.Security.Cryptography.RSA>
- Geração de número aleatório:
  - <xref:System.Security.Cryptography.RandomNumberGenerator.Create%2A?displayProperty=nameWithType>
- Gerando uma chave a partir de uma senha:
  - <xref:System.Security.Cryptography.Rfc2898DeriveBytes>

## <a name="see-also"></a>Confira também

- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
