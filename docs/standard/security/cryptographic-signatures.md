---
title: Assinaturas criptográficas
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- digital signatures
- cryptography [.NET], signatures
- digital signatures, XML signing
- signatures, cryptographic
- digital signatures, generating
- verifying signatures
- generating signatures
- digital signatures, about
- encryption [.NET], signatures
- security [.NET], signatures
- XML signing
- digital signatures, verifying
- signing XML
ms.assetid: aa87cb7f-e608-4a81-948b-c9b8a1225783
ms.openlocfilehash: ce2be1d509da4e399bf87e1c8df7ba061fc2707c
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557002"
---
# <a name="cryptographic-signatures"></a>Assinaturas criptográficas

As assinaturas digitais de criptografia usam algoritmos de chave pública para fornecer integridade de dados. Quando você assina dados com uma assinatura digital, outra pessoa pode verificar a assinatura e pode provar que os dados foram originados de você e não foram alterados depois que você o assinou. Para obter mais informações sobre assinaturas digitais, consulte [serviços de criptografia](cryptographic-services.md).

Este tópico explica como gerar e verificar assinaturas digitais usando classes no <xref:System.Security.Cryptography> namespace.

## <a name="generating-signatures"></a>Gerando assinaturas

Geralmente, as assinaturas digitais são aplicadas a valores de hash que representam dados maiores. O exemplo a seguir aplica uma assinatura digital a um valor de hash. Primeiro, uma nova instância da <xref:System.Security.Cryptography.RSA> classe é criada para gerar um par de chaves pública/privada. Em seguida, o <xref:System.Security.Cryptography.RSA> é passado para uma nova instância da <xref:System.Security.Cryptography.RSAPKCS1SignatureFormatter> classe. Isso transfere a chave privada para o <xref:System.Security.Cryptography.RSAPKCS1SignatureFormatter> , que realmente executa a assinatura digital. Antes de poder assinar o código hash, você deve especificar um algoritmo de hash a ser usado. Este exemplo usa o algoritmo SHA1. Por fim, o <xref:System.Security.Cryptography.AsymmetricSignatureFormatter.CreateSignature%2A> método é chamado para executar a assinatura.

Devido a problemas de colisão com SHA1, recomendamos SHA256 ou melhor.

```vb
Imports System.Security.Cryptography

Module Module1
    Sub Main()
        'The hash value to sign.
        Dim hashValue As Byte() = {59, 4, 248, 102, 77, 97, 142, 201, 210, 12, 224, 93, 25, 41, 100, 197, 213, 134, 130, 135}

        'The value to hold the signed value.
        Dim signedHashValue() As Byte

        'Generate a public/private key pair.
        Dim rsa As RSA = RSA.Create()

        'Create an RSAPKCS1SignatureFormatter object and pass it
        'the RSA instance to transfer the private key.
        Dim rsaFormatter As New RSAPKCS1SignatureFormatter(rsa)

        'Set the hash algorithm to SHA1.
        rsaFormatter.SetHashAlgorithm("SHA1")

        'Create a signature for hashValue and assign it to
        'signedHashValue.
        signedHashValue = rsaFormatter.CreateSignature(hashValue)
    End Sub
End Module
```

```csharp
using System;
using System.Security.Cryptography;

class Class1
{
   static void Main()
   {
      //The hash value to sign.
      byte[] hashValue = {59,4,248,102,77,97,142,201,210,12,224,93,25,41,100,197,213,134,130,135};

      //The value to hold the signed value.
      byte[] signedHashValue;

      //Generate a public/private key pair.
      RSA rsa = RSA.Create();

      //Create an RSAPKCS1SignatureFormatter object and pass it the
      //RSA instance to transfer the private key.
      RSAPKCS1SignatureFormatter rsaFormatter = new RSAPKCS1SignatureFormatter(rsa);

      //Set the hash algorithm to SHA1.
      rsaFormatter.SetHashAlgorithm("SHA1");

      //Create a signature for hashValue and assign it to
      //signedHashValue.
      signedHashValue = rsaFormatter.CreateSignature(hashValue);
   }
}
```

## <a name="verifying-signatures"></a>Verificando assinaturas

Para verificar se os dados foram assinados por uma entidade específica, você deve ter as seguintes informações:

- A chave pública da entidade que assinou os dados.

- A assinatura digital.

- Os dados que receberam um sinal.

- O algoritmo de hash usado pelo assinante.

Para verificar uma assinatura assinada pela <xref:System.Security.Cryptography.RSAPKCS1SignatureFormatter> classe, use a <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter> classe. A <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter> classe deve ser fornecida à chave pública do Assinante. Para a RSA, você precisará dos valores do módulo e do expoente para especificar a chave pública. (A parte que gerou o par de chaves pública/privada deve fornecer esses valores.) Primeiro, crie um <xref:System.Security.Cryptography.RSA> objeto para manter a chave pública que verificará a assinatura e, em seguida, inicialize uma <xref:System.Security.Cryptography.RSAParameters> estrutura para os valores de módulo e expoente que especificam a chave pública.

O código a seguir mostra a criação de uma <xref:System.Security.Cryptography.RSAParameters> estrutura. A `Modulus` propriedade é definida como o valor de uma matriz de bytes chamada `modulusData` e a `Exponent` propriedade é definida como o valor de uma matriz de bytes chamada `exponentData` .

```vb
Dim rsaKeyInfo As RSAParameters
rsaKeyInfo.Modulus = modulusData
rsaKeyInfo.Exponent = exponentData
```

```csharp
RSAParameters rsaKeyInfo;
rsaKeyInfo.Modulus = modulusData;
rsaKeyInfo.Exponent = exponentData;
```

Depois de criar o <xref:System.Security.Cryptography.RSAParameters> objeto, você pode inicializar uma nova instância da classe de <xref:System.Security.Cryptography.RSA> implementação para os valores especificados em <xref:System.Security.Cryptography.RSAParameters> . A <xref:System.Security.Cryptography.RSA> instância é, por sua vez, passada para o construtor de um <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter> para transferir a chave.

O exemplo a seguir ilustra esse processo. Neste exemplo, `hashValue` e `signedHashValue` são matrizes de bytes fornecidos por uma parte remota. A parte remota assinou o `hashValue` usando o algoritmo SHA1, produzindo a assinatura digital `signedHashValue` . O <xref:System.Security.Cryptography.RSAPKCS1SignatureDeformatter.VerifySignature%2A?displayProperty=nameWithType> método verifica se a assinatura digital é válida e se foi usada para assinar o `hashValue` .

Devido a problemas de colisão com SHA1, recomendamos SHA256 ou melhor.  No entanto, se o SHA1 foi usado para criar a assinatura, você precisa usar o SHA1 para verificar a assinatura.

```vb
Dim rsa As RSA = RSA.Create()
rsa.ImportParameters(rsaKeyInfo)
Dim rsaDeformatter As New RSAPKCS1SignatureDeformatter(rsa)
rsaDeformatter.SetHashAlgorithm("SHA1")
If rsaDeformatter.VerifySignature(hashValue, signedHashValue) Then
   Console.WriteLine("The signature is valid.")
Else
   Console.WriteLine("The signature is not valid.")
End If
```

```csharp
RSA rsa = RSA.Create();
rsa.ImportParameters(rsaKeyInfo);
RSAPKCS1SignatureDeformatter rsaDeformatter = new RSAPKCS1SignatureDeformatter(rsa);
rsaDeformatter.SetHashAlgorithm("SHA1");
if(rsaDeformatter.VerifySignature(hashValue, signedHashValue))
{
   Console.WriteLine("The signature is valid.");
}
else
{
   Console.WriteLine("The signature is not valid.");
}
```

Esse fragmento de código exibirá " `The signature is valid` " se a assinatura for válida e " `The signature is not valid` " se não for.

## <a name="see-also"></a>Confira também

- [Serviços criptográficos](cryptographic-services.md)
- [Modelo de criptografia](cryptography-model.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
