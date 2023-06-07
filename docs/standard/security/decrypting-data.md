---
title: Descriptografando dados
description: Saiba como descriptografar dados no .NET, usando um algoritmo simétrico ou um algoritmo assimétrico.
ms.date: 07/16/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data [.NET], decryption
- symmetric decryption
- asymmetric decryption
- decryption
ms.assetid: 9b266b6c-a9b2-4d20-afd8-b3a0d8fd48a0
ms.openlocfilehash: 2ba4c3ba43d688aeb66c67ec3f94f4a503d47892
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556976"
---
# <a name="decrypting-data"></a>Descriptografando dados

A descriptografia é a operação inversa da criptografia. Para criptografia de chave secreta, você deve saber a chave e o IV que foram usados para criptografar os dados. Para criptografia de chave pública, você deve saber a chave pública (se os dados foram criptografados usando a chave privada) ou a chave privada (se os dados foram criptografados usando a chave pública).

## <a name="symmetric-decryption"></a>Descriptografia simétrica

A descriptografia de dados criptografados com algoritmos simétricos é semelhante ao processo usado para criptografar dados com algoritmos simétricos. A <xref:System.Security.Cryptography.CryptoStream> classe é usada com classes de criptografia simétrica fornecidas pelo .net para descriptografar dados lidos de qualquer objeto de fluxo gerenciado.

O exemplo a seguir ilustra como criar uma nova instância da classe de implementação padrão para o <xref:System.Security.Cryptography.Aes> algoritmo. A instância é usada para executar a descriptografia em um <xref:System.Security.Cryptography.CryptoStream> objeto. Este exemplo primeiro cria uma nova instância da classe de implementação **AES** . Em seguida, ele cria um objeto **CryptoStream** e o inicializa para o valor de um fluxo gerenciado chamado `myStream` . Em seguida, o método **Createdecryptr** da classe **AES** é passado para a mesma chave e IV que foi usado para criptografia e, em seguida, é passado para o construtor **CryptoStream** .

```vb
Dim aes As Aes = Aes.Create()
Dim cryptStream As New CryptoStream(myStream, aes.CreateDecryptor(key, iv), CryptoStreamMode.Read)
```

```csharp
Aes aes = Aes.Create();
CryptoStream cryptStream = new CryptoStream(myStream, aes.CreateDecryptor(key, iv), CryptoStreamMode.Read);
```

O exemplo a seguir mostra todo o processo de criação de um fluxo, descriptografia do fluxo, leitura a partir do fluxo e fechamento dos fluxos. Um objeto de fluxo de arquivo é criado e lê um arquivo chamado *TestData.txt*. O fluxo de arquivo é então descriptografado usando a classe **CryptoStream** e a classe **AES** . Este exemplo especifica os valores de chave e IV que são usados no exemplo de criptografia simétrica para [criptografar dados](encrypting-data.md). Ele não mostra o código necessário para criptografar e transferir esses valores.

```vb
Imports System
Imports System.IO
Imports System.Security.Cryptography

Module Module1
    Sub Main()
            'The key and IV must be the same values that were used
            'to encrypt the stream.
            Dim key As Byte() = {&H1, &H2, &H3, &H4, &H5, &H6, &H7, &H8, &H9, &H10, &H11, &H12, &H13, &H14, &H15, &H16}
            Dim iv As Byte() = {&H1, &H2, &H3, &H4, &H5, &H6, &H7, &H8, &H9, &H10, &H11, &H12, &H13, &H14, &H15, &H16}
        Try
            'Create a file stream.
            Dim myStream As FileStream = new FileStream("TestData.txt", FileMode.Open)

            'Create a new instance of the default Aes implementation class
            'and decrypt the stream.
            Dim aes As Aes = Aes.Create()

            'Create an instance of the CryptoStream class, pass it the file stream, and decrypt
            'it with the Rijndael class using the key and IV.
            Dim cryptStream As New CryptoStream(myStream, aes.CreateDecryptor(key, iv), CryptoStreamMode.Read)

            'Read the stream.
            Dim sReader As New StreamReader(cryptStream)

            'Display the message.
            Console.WriteLine("The decrypted original message: {0}", sReader.ReadToEnd())

            'Close the streams.
            sReader.Close()
            myStream.Close()
            'Catch any exceptions.
        Catch
            Console.WriteLine("The decryption Failed.")
            Throw
        End Try
    End Sub
End Module
```

```csharp
using System;
using System.IO;
using System.Security.Cryptography;

class Class1
{
    static void Main(string[] args)
    {
        //The key and IV must be the same values that were used
        //to encrypt the stream.
        byte[] key = { 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x10, 0x11, 0x12, 0x13, 0x14, 0x15, 0x16 };
        byte[] iv = { 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x10, 0x11, 0x12, 0x13, 0x14, 0x15, 0x16 };
        try
        {
            //Create a file stream.
            FileStream myStream = new FileStream("TestData.txt", FileMode.Open);

            //Create a new instance of the default Aes implementation class
            Aes aes = Aes.Create();

            //Create a CryptoStream, pass it the file stream, and decrypt
            //it with the Aes class using the key and IV.
            CryptoStream cryptStream = new CryptoStream(
               myStream,
               aes.CreateDecryptor(key, iv),
               CryptoStreamMode.Read);

            //Read the stream.
            StreamReader sReader = new StreamReader(cryptStream);

            //Display the message.
            Console.WriteLine("The decrypted original message: {0}", sReader.ReadToEnd());

            //Close the streams.
            sReader.Close();
            myStream.Close();
        }
        //Catch any exceptions.
        catch
        {
            Console.WriteLine("The decryption failed.");
            throw;
        }
    }
}
```

O exemplo anterior usa a mesma chave, IV e algoritmo usado no exemplo de criptografia simétrica para [criptografar dados](encrypting-data.md). Ele descriptografa o arquivo de *TestData.txt* que é criado por esse exemplo e exibe o texto original no console.

## <a name="asymmetric-decryption"></a>Descriptografia assimétrica

Normalmente, uma entidade (parte A) gera uma chave pública e privada e armazena a chave na memória ou em um contêiner de chave de criptografia. Em seguida, a parte a envia a chave pública para outra parte (parte B). Usando a chave pública, a parte B criptografa dados e envia os dados de volta para a parte A. Depois de receber os dados, A parte A descriptografa usando a chave privada que corresponde. A descriptografia será bem-sucedida somente se a parte A usar a chave privada que corresponde à parte da chave pública B usada para criptografar os dados.

Para obter informações sobre como armazenar uma chave assimétrica no contêiner de chave criptográfica segura e como recuperar posteriormente a chave assimétrica, consulte [como armazenar chaves assimétricas em um contêiner de chave](how-to-store-asymmetric-keys-in-a-key-container.md).

O exemplo a seguir ilustra a descriptografia de duas matrizes de bytes que representam uma chave simétrica e um IV. Para obter informações sobre como extrair a chave pública assimétrica do <xref:System.Security.Cryptography.RSA> objeto em um formato que você pode enviar facilmente a terceiros, consulte [Criptografando dados](encrypting-data.md).

```vb
'Create a new instance of the RSA class.
Dim rsa As RSA = RSA.Create()

' Export the public key information and send it to a third party.
' Wait for the third party to encrypt some data and send it back.

'Decrypt the symmetric key and IV.
symmetricKey = rsa.Decrypt(encryptedSymmetricKey, RSAEncryptionPadding.Pkcs1)
symmetricIV = rsa.Decrypt(encryptedSymmetricIV, RSAEncryptionPadding.Pkcs1)
```

```csharp
//Create a new instance of the RSA class.
RSA rsa = RSA.Create();

// Export the public key information and send it to a third party.
// Wait for the third party to encrypt some data and send it back.

//Decrypt the symmetric key and IV.
symmetricKey = rsa.Decrypt(encryptedSymmetricKey, RSAEncryptionPadding.Pkcs1);
symmetricIV = rsa.Decrypt(encryptedSymmetricIV , RSAEncryptionPadding.Pkcs1);
```

## <a name="see-also"></a>Confira também

- [Gerando chaves para criptografia e descriptografia](generating-keys-for-encryption-and-decryption.md)
- [Criptografando dados](encrypting-data.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Modelo de criptografia](cryptography-model.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Vulnerabilidades de temporização com descriptografia simétrica no modo CBC usando preenchimento](vulnerabilities-cbc-mode.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
