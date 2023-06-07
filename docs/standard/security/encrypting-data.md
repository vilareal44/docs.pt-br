---
title: Criptografando dados
description: Saiba como criptografar dados no .NET usando um algoritmo simétrico ou um algoritmo assimétrico.
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- encryption [.NET], symmetric
- symmetric encryption
- cryptography [.NET], asymmetric
- asymmetric encryption
ms.assetid: 7ecce51f-db5f-4bd4-9321-cceb6fcb2a77
ms.openlocfilehash: 8a8b5988a13ab571284b08c7aaece3542467aa71
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556963"
---
# <a name="encrypting-data"></a>Criptografando dados

A criptografia simétrica e a criptografia assimétrica são executadas usando processos diferentes. A criptografia simétrica é executada em fluxos e, portanto, é útil para criptografar grandes quantidades de dados. A criptografia assimétrica é executada em um pequeno número de bytes e, portanto, é útil apenas para pequenas quantidades de dados.  
  
## <a name="symmetric-encryption"></a>Criptografia simétrica  

As classes de criptografia simétrica gerenciadas são usadas com uma classe de fluxo especial chamada a <xref:System.Security.Cryptography.CryptoStream> que criptografa os dados lidos no fluxo. A classe **CryptoStream** é inicializada com uma classe de fluxo gerenciada, uma classe que implementa a <xref:System.Security.Cryptography.ICryptoTransform> interface (criada a partir de uma classe que implementa um algoritmo criptográfico) e uma <xref:System.Security.Cryptography.CryptoStreamMode> enumeração que descreve o tipo de acesso permitido para o **CryptoStream**. A classe **CryptoStream** pode ser inicializada usando qualquer classe derivada da <xref:System.IO.Stream> classe, incluindo <xref:System.IO.FileStream> , <xref:System.IO.MemoryStream> e <xref:System.Net.Sockets.NetworkStream> . Usando essas classes, você pode executar a criptografia simétrica em uma variedade de objetos Stream.  
  
O exemplo a seguir ilustra como criar uma nova instância da classe de implementação padrão para o <xref:System.Security.Cryptography.Aes> algoritmo. A instância é usada para executar a criptografia em uma classe **CryptoStream** . Neste exemplo, o **CryptoStream** é inicializado com um objeto de fluxo chamado `myStream` que pode ser qualquer tipo de fluxo gerenciado. O método **Createencryptr** da classe **AES** é passado à chave e ao IV que são usados para criptografia. Nesse caso, a chave padrão e o IV gerados de `aes` são usados.
  
```vb  
Dim aes As Aes = Aes.Create()  
Dim cryptStream As New CryptoStream(myStream, aes.CreateEncryptor(key, iv), CryptoStreamMode.Write)  
```  
  
```csharp  
Aes aes = Aes.Create();  
CryptoStream cryptStream = new CryptoStream(myStream, aes.CreateEncryptor(key, iv), CryptoStreamMode.Write);  
```  
  
Depois que esse código é executado, todos os dados gravados no objeto **CryptoStream** são criptografados usando o algoritmo AES.  
  
O exemplo a seguir mostra todo o processo de criação de um fluxo, criptografia do fluxo, gravação no fluxo e fechamento do fluxo. Este exemplo cria um fluxo de arquivos que é criptografado usando a classe **CryptoStream** e a classe **AES** . Uma mensagem é gravada no fluxo criptografado com a <xref:System.IO.StreamWriter> classe.
  
:::code language="csharp" source="snippets/encrypting-data/csharp/aes-encrypt.cs":::
:::code language="vb" source="snippets/encrypting-data/vb/aes-encrypt.vb":::

O código criptografa o fluxo usando o algoritmo simétrico AES e grava "Olá, Mundo!" para o fluxo. Se o código for bem-sucedido, ele criará um arquivo criptografado chamado *TestData.txt* e exibirá o seguinte texto no console:  
  
```console  
The text was encrypted.  
```  

Você pode descriptografar o arquivo usando o exemplo de descriptografia simétrica na [descriptografia de dados](decrypting-data.md). Esse exemplo e este exemplo especificam a mesma chave e IV.

No entanto, se uma exceção for gerada, o código exibirá o seguinte texto no console:  
  
```console  
The encryption failed.  
```

## <a name="asymmetric-encryption"></a>Criptografia assimétrica

Os algoritmos assimétrico geralmente são usados para criptografar pequenas quantidades de dados, como a criptografia de uma chave simétrica e IV. Normalmente, um indivíduo executando a criptografia assimétrica usa a chave pública gerada por outra entidade. A <xref:System.Security.Cryptography.RSA> classe é fornecida pelo .net para essa finalidade.  
  
O exemplo a seguir usa informações de chave pública para criptografar uma chave simétrica e um IV. Duas matrizes de bytes são inicializadas que representam a chave pública de terceiros. Um <xref:System.Security.Cryptography.RSAParameters> objeto é inicializado para esses valores. Em seguida, o objeto **RSAParameters** (junto com a chave pública que ele representa) é importado para uma instância **RSA** usando o <xref:System.Security.Cryptography.RSA.ImportParameters%2A?displayProperty=nameWithType> método. Por fim, a chave privada e o IV criados por uma <xref:System.Security.Cryptography.Aes> classe são criptografados. Este exemplo requer que os sistemas tenham a criptografia de 128 bits instalada.  
  
```vb  
Imports System
Imports System.Security.Cryptography

Module Module1

    Sub Main()
        'Initialize the byte arrays to the public key information.  
        Dim modulus As Byte() = {214, 46, 220, 83, 160, 73, 40, 39, 201, 155, 19, 202, 3, 11, 191, 178, 56, 74, 90, 36, 248, 103, 18, 144, 170, 163, 145, 87, 54, 61, 34, 220, 222, 207, 137, 149, 173, 14, 92, 120, 206, 222, 158, 28, 40, 24, 30, 16, 175, 108, 128, 35, 230, 118, 40, 121, 113, 125, 216, 130, 11, 24, 90, 48, 194, 240, 105, 44, 76, 34, 57, 249, 228, 125, 80, 38, 9, 136, 29, 117, 207, 139, 168, 181, 85, 137, 126, 10, 126, 242, 120, 247, 121, 8, 100, 12, 201, 171, 38, 226, 193, 180, 190, 117, 177, 87, 143, 242, 213, 11, 44, 180, 113, 93, 106, 99, 179, 68, 175, 211, 164, 116, 64, 148, 226, 254, 172, 147}

        Dim exponent As Byte() = {1, 0, 1}

        'Create values to store encrypted symmetric keys.  
        Dim encryptedSymmetricKey() As Byte
        Dim encryptedSymmetricIV() As Byte

        'Create a new instance of the default RSA implementation class.
        Dim rsa As RSA = RSA.Create()

        'Create a new instance of the RSAParameters structure.  
        Dim rsaKeyInfo As New RSAParameters()

        'Set rsaKeyInfo to the public key values.
        rsaKeyInfo.Modulus = modulus
        rsaKeyInfo.Exponent = exponent

        'Import key parameters into rsa
        rsa.ImportParameters(rsaKeyInfo)

        'Create a new instance of the default Aes implementation class.  
        Dim aes As Aes = Aes.Create()

        'Encrypt the symmetric key and IV.  
        encryptedSymmetricKey = rsa.Encrypt(aes.Key, RSAEncryptionPadding.Pkcs1)
        encryptedSymmetricIV = rsa.Encrypt(aes.IV, RSAEncryptionPadding.Pkcs1)
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
        //Initialize the byte arrays to the public key information.  
        byte[] modulus = {214,46,220,83,160,73,40,39,201,155,19,202,3,11,191,178,56,
            74,90,36,248,103,18,144,170,163,145,87,54,61,34,220,222,
            207,137,149,173,14,92,120,206,222,158,28,40,24,30,16,175,
            108,128,35,230,118,40,121,113,125,216,130,11,24,90,48,194,
            240,105,44,76,34,57,249,228,125,80,38,9,136,29,117,207,139,
            168,181,85,137,126,10,126,242,120,247,121,8,100,12,201,171,
            38,226,193,180,190,117,177,87,143,242,213,11,44,180,113,93,
            106,99,179,68,175,211,164,116,64,148,226,254,172,147};

        byte[] exponent = { 1, 0, 1 };

        //Create values to store encrypted symmetric keys.  
        byte[] encryptedSymmetricKey;
        byte[] encryptedSymmetricIV;

        //Create a new instance of the RSA class.  
        RSA rsa = RSA.Create();

        //Create a new instance of the RSAParameters structure.  
        RSAParameters rsaKeyInfo = new RSAParameters();

        //Set rsaKeyInfo to the public key values.
        rsaKeyInfo.Modulus = modulus;
        rsaKeyInfo.Exponent = exponent;

        //Import key parameters into rsa.  
        rsa.ImportParameters(rsaKeyInfo);

        //Create a new instance of the default Aes implementation class.  
        Aes aes = Aes.Create();

        //Encrypt the symmetric key and IV.  
        encryptedSymmetricKey = rsa.Encrypt(aes.Key, RSAEncryptionPadding.Pkcs1);
        encryptedSymmetricIV = rsa.Encrypt(aes.IV, RSAEncryptionPadding.Pkcs1);
    }
}
```  
  
## <a name="see-also"></a>Confira também

- [Gerando chaves para criptografia e descriptografia](generating-keys-for-encryption-and-decryption.md)
- [Descriptografando dados](decrypting-data.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Vulnerabilidades de temporização com descriptografia simétrica no modo CBC usando preenchimento](vulnerabilities-cbc-mode.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
