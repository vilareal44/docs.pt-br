---
title: Como armazenar chaves assimétricas em um contêiner de chave
description: Saiba como armazenar chaves assimétricas em um contêiner de chave no .NET. Veja como criar uma chave assimétrica, salvá-la em um contêiner de chave e recuperar e excluir a chave.
ms.date: 05/26/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [.NET], asymmetric keys
- storing asymmetric keys
- keys, asymmetric
- encryption keys
- keys, storing in key containers
- asymmetric keys [.NET]
- encryption [.NET], asymmetric keys
- decryption keys
ms.assetid: 0dbcbd8d-0dcf-40e9-9f0c-e3f162d35ccc
ms.openlocfilehash: 9c04d1ea4d7e7ee46d875b3fa791f3eee2059e52
ms.sourcegitcommit: c37e8d4642fef647ebab0e1c618ecc29ddfe2a0f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854718"
---
# <a name="store-asymmetric-keys-in-a-key-container"></a>Armazenar chaves assimétricas em um contêiner de chave

As chaves privadas assimétricas nunca devem ser armazenadas no formato textual nem como texto sem formatação no computador local. Se você precisar armazenar uma chave privada, use um contêiner de chave. Para obter mais informações sobre contêineres de chave, consulte [noções básicas sobre contêineres de chave RSA no nível do computador e](https://docs.microsoft.com/previous-versions/aspnet/f5cs0acs(v=vs.100))no nível do usuário.

> [!NOTE]
> O código neste artigo aplica-se ao Windows e usa recursos não disponíveis no .NET Core 2,2 e em versões anteriores. Para obter mais informações, consulte [dotnet/tempo de execução # 23391](https://github.com/dotnet/runtime/issues/23391).

## <a name="create-an-asymmetric-key-and-save-it-in-a-key-container"></a>Criar uma chave assimétrica e salvá-la em um contêiner de chave

1. Crie uma nova instância de uma <xref:System.Security.Cryptography.CspParameters> classe e passe o nome que você deseja chamar o contêiner de chave para o <xref:System.Security.Cryptography.CspParameters.KeyContainerName?displayProperty=nameWithType> campo.

1. Crie uma nova instância de uma classe derivada da <xref:System.Security.Cryptography.AsymmetricAlgorithm> classe (geralmente <xref:System.Security.Cryptography.RSACryptoServiceProvider> ou <xref:System.Security.Cryptography.DSACryptoServiceProvider> ) e passe o objeto criado anteriormente `CspParameters` para seu construtor.

> [!NOTE]
> A criação e a recuperação de uma chave assimétrica são uma operação. Se uma chave ainda não estiver no contêiner, ela será criada antes de ser retornada.
>
> - <xref:System.Security.Cryptography.RSA.ToXmlString%2A?displayProperty=nameWithType>
> - <xref:System.Security.Cryptography.DSA.ToXmlString%2A?displayProperty=nameWithType>

## <a name="delete-the-key-from-the-key-container"></a>Excluir a chave do contêiner de chave

1. Crie uma nova instância de uma `CspParameters` classe e passe o nome que você deseja chamar o contêiner de chave para o <xref:System.Security.Cryptography.CspParameters.KeyContainerName?displayProperty=nameWithType> campo.

1. Crie uma nova instância de uma classe derivada da <xref:System.Security.Cryptography.AsymmetricAlgorithm> classe (geralmente `RSACryptoServiceProvider` ou `DSACryptoServiceProvider` ) e passe o objeto criado anteriormente `CspParameters` para seu construtor.

1. Defina a <xref:System.Security.Cryptography.RSACryptoServiceProvider.PersistKeyInCsp?displayProperty=nameWithType> propriedade ou <xref:System.Security.Cryptography.DSACryptoServiceProvider.PersistKeyInCsp?displayProperty=nameWithType> da classe que deriva de `AsymmetricAlgorithm` para `false` ( `False` em Visual Basic).

1. Chame o `Clear` método da classe derivada de `AsymmetricAlgorithm` . Esse método libera todos os recursos da classe e limpa o contêiner de chaves.

## <a name="example"></a>Exemplo

O exemplo a seguir mostra como criar uma chave assimétrica, salvá-la em um contêiner de chaves, recuperar a chave posteriormente e excluir a chave do contêiner.

Observe que o código no `GenKey_SaveInContainer` método e o `GetKeyFromContainer` método são semelhantes. Quando você especifica um nome de contêiner de chave para um <xref:System.Security.Cryptography.CspParameters> objeto e o passa para um <xref:System.Security.Cryptography.AsymmetricAlgorithm> objeto com a <xref:System.Security.Cryptography.RSACryptoServiceProvider.PersistKeyInCsp%2A> propriedade ou <xref:System.Security.Cryptography.DSACryptoServiceProvider.PersistKeyInCsp%2A> propriedade definida como `true` , o comportamento é o seguinte:

- se o contêiner de chaves com o nome especificado não existir, ele será criado e a chave persistirá.
- Se o contêiner de chaves com o nome especificado existir, a chave será carregada automaticamente no objeto <xref:System.Security.Cryptography.AsymmetricAlgorithm> atual.

Portanto, o código no `GenKey_SaveInContainer` método persiste a chave porque ela é executada primeiro, enquanto o código no `GetKeyFromContainer` método carrega a chave porque ela é executada em segundo lugar.

```vb
Imports System
Imports System.Security.Cryptography

Public Class StoreKey

    Public Shared Sub Main()
        Try
            ' Create a key and save it in a container.
            GenKey_SaveInContainer("MyKeyContainer")

            ' Retrieve the key from the container.
            GetKeyFromContainer("MyKeyContainer")

            ' Delete the key from the container.
            DeleteKeyFromContainer("MyKeyContainer")

            ' Create a key and save it in a container.
            GenKey_SaveInContainer("MyKeyContainer")

            ' Delete the key from the container.
            DeleteKeyFromContainer("MyKeyContainer")
        Catch e As CryptographicException
            Console.WriteLine(e.Message)
        End Try
    End Sub

    Private Shared Sub GenKey_SaveInContainer(ByVal ContainerName As String)
        ' Create the CspParameters object and set the key container
        ' name used to store the RSA key pair.
        Dim parameters As New CspParameters With {
            .KeyContainerName = ContainerName
        }

        ' Create a new instance of RSACryptoServiceProvider that accesses
        ' the key container MyKeyContainerName.
        Using rsa As New RSACryptoServiceProvider(parameters)
            ' Display the key information to the console.
            Console.WriteLine($"Key added to container:  {rsa.ToXmlString(True)}")
        End Using
    End Sub

    Private Shared Sub GetKeyFromContainer(ByVal ContainerName As String)
        ' Create the CspParameters object and set the key container
        '  name used to store the RSA key pair.
        Dim parameters As New CspParameters With {
            .KeyContainerName = ContainerName
        }

        ' Create a new instance of RSACryptoServiceProvider that accesses
        ' the key container MyKeyContainerName.
        Using rsa As New RSACryptoServiceProvider(parameters)
            ' Display the key information to the console.
            Console.WriteLine($"Key retrieved from container : {rsa.ToXmlString(True)}")
        End Using
    End Sub

    Private Shared Sub DeleteKeyFromContainer(ByVal ContainerName As String)
        ' Create the CspParameters object and set the key container
        '  name used to store the RSA key pair.
        Dim parameters As New CspParameters With {
            .KeyContainerName = ContainerName
        }

        ' Create a new instance of RSACryptoServiceProvider that accesses
        ' the key container.
        ' Delete the key entry in the container.
        Dim rsa As New RSACryptoServiceProvider(parameters) With {
            .PersistKeyInCsp = False
        }

        ' Call Clear to release resources and delete the key from the container.
        rsa.Clear()

        Console.WriteLine("Key deleted.")
    End Sub
End Class
```

```csharp
using System;
using System.Security.Cryptography;

public class StoreKey
{
    public static void Main()
    {
        try
        {
            // Create a key and save it in a container.
            GenKey_SaveInContainer("MyKeyContainer");

            // Retrieve the key from the container.
            GetKeyFromContainer("MyKeyContainer");

            // Delete the key from the container.
            DeleteKeyFromContainer("MyKeyContainer");

            // Create a key and save it in a container.
            GenKey_SaveInContainer("MyKeyContainer");

            // Delete the key from the container.
            DeleteKeyFromContainer("MyKeyContainer");
        }
        catch (CryptographicException e)
        {
            Console.WriteLine(e.Message);
        }
    }

    private static void GenKey_SaveInContainer(string containerName)
    {
        // Create the CspParameters object and set the key container
        // name used to store the RSA key pair.
        var parameters = new CspParameters
        {
            KeyContainerName = containerName
        };

        // Create a new instance of RSACryptoServiceProvider that accesses
        // the key container MyKeyContainerName.
        using var rsa = new RSACryptoServiceProvider(parameters);

        // Display the key information to the console.
        Console.WriteLine($"Key added to container: \n  {rsa.ToXmlString(true)}");
    }

    private static void GetKeyFromContainer(string containerName)
    {
        // Create the CspParameters object and set the key container
        // name used to store the RSA key pair.
        var parameters = new CspParameters
        {
            KeyContainerName = containerName
        };

        // Create a new instance of RSACryptoServiceProvider that accesses
        // the key container MyKeyContainerName.
        using var rsa = new RSACryptoServiceProvider(parameters);

        // Display the key information to the console.
        Console.WriteLine($"Key retrieved from container : \n {rsa.ToXmlString(true)}");
    }

    private static void DeleteKeyFromContainer(string containerName)
    {
        // Create the CspParameters object and set the key container
        // name used to store the RSA key pair.
        var parameters = new CspParameters
        {
            KeyContainerName = containerName
        };

        // Create a new instance of RSACryptoServiceProvider that accesses
        // the key container.
        using var rsa = new RSACryptoServiceProvider(parameters)
        {
            // Delete the key entry in the container.
            PersistKeyInCsp = false
        };

        // Call Clear to release resources and delete the key from the container.
        rsa.Clear();

        Console.WriteLine("Key deleted.");
    }
}
```

A saída é da seguinte maneira:

```console
Key added to container:
<RSAKeyValue> Key Information A</RSAKeyValue>
Key retrieved from container :
<RSAKeyValue> Key Information A</RSAKeyValue>
Key deleted.
Key added to container:
<RSAKeyValue> Key Information B</RSAKeyValue>
Key deleted.
```

## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Gerando chaves para criptografia e descriptografia](generating-keys-for-encryption-and-decryption.md)
- [Criptografando dados](encrypting-data.md)
- [Descriptografando dados](decrypting-data.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
