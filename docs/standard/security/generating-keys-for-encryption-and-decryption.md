---
title: Gerando chaves para criptografia e descriptografia
description: Entenda como criar e gerenciar chaves simétricas e assimétricas para criptografia e descriptografia no .NET.
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- keys, encryption and decryption
- keys, asymmetric
- keys, symmetric
- encryption [.NET], keys
- symmetric keys
- asymmetric keys [.NET]
- cryptography [.NET], keys
ms.assetid: c197dfc9-a453-4226-898d-37a16638056e
ms.openlocfilehash: 7ce19dc465fb1fac22545398e0724e6b76dd7098
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556937"
---
# <a name="generating-keys-for-encryption-and-decryption"></a>Gerando chaves para criptografia e descriptografia
A criação e o gerenciamento de chaves é uma parte importante do processo criptográfico. Os algoritmos simétricos exigem a criação de uma chave e um vetor de inicialização (IV). A chave deve ser mantida em segredo de qualquer pessoa que não deva descriptografar seus dados. O IV não precisa ser secreto, mas deve ser alterado para cada sessão. Algoritmos assimétricos exigem a criação de uma chave pública e uma chave privada. A chave pública pode se tornar pública para qualquer pessoa, enquanto a chave privada deve ser conhecida somente pela parte que descriptografará os dados criptografados com a chave pública. Esta seção descreve como gerar e gerenciar chaves para algoritmos simétricos e assimétricos.  
  
## <a name="symmetric-keys"></a>Chaves simétricas  
 As classes de criptografia simétrica fornecidas pelo .NET exigem uma chave e um novo vetor de inicialização (IV) para criptografar e descriptografar dados. Sempre que você cria uma nova instância de uma das classes de criptografia simétricas gerenciadas usando o método sem parâmetros `Create()` , uma nova chave e IV são criadas automaticamente. Qualquer pessoa que você permita descriptografar seus dados deve ter a mesma chave e IV e usar o mesmo algoritmo. Em geral, uma nova chave e IV devem ser criadas para cada sessão e nem a chave nem o IV devem ser armazenados para uso em uma sessão posterior.  
  
 Para comunicar uma chave simétrica e um IV a uma parte remota, você normalmente criptografaria a chave simétrica usando a criptografia assimétrica. Enviar a chave por meio de uma rede insegura sem criptografá-la não é seguro, pois qualquer pessoa que intercepta a chave e o IV pode então descriptografar seus dados.  
  
 O exemplo a seguir mostra a criação de uma nova instância da classe de implementação padrão para o <xref:System.Security.Cryptography.Aes> algoritmo.  
  
```vb  
Dim aes As Aes = Aes.Create()  
```  
  
```csharp  
Aes aes = Aes.Create();  
```  
  
 Quando o código anterior é executado, uma nova chave e IV são gerados e colocados nas propriedades **Key** e **IV** , respectivamente.  
  
 Às vezes, talvez seja necessário gerar várias chaves. Nessa situação, você pode criar uma nova instância de uma classe que implementa um algoritmo simétrico e, em seguida, criar uma nova chave e IV chamando os métodos **GenerateKey** e **GenerateIV** . O exemplo de código a seguir ilustra como criar novas chaves e IVs após uma nova instância da classe de criptografia simétrica ter sido feita.  
  
```vb  
Dim aes As Aes = Aes.Create()  
aes.GenerateIV()  
aes.GenerateKey()  
```  
  
```csharp  
Aes aes = Aes.Create();  
aes.GenerateIV();  
aes.GenerateKey();  
```  
  
 Quando o código anterior é executado, uma chave e IV são gerados quando a nova instância do **AES** é feita. Outra chave e IV são criados quando os métodos **GenerateKey** e **GenerateIV** são chamados.
  
## <a name="asymmetric-keys"></a>Chaves assimétricas

 O .NET fornece a <xref:System.Security.Cryptography.RSA> classe para a criptografia assimétrica. Essa classe cria um par de chaves pública/privada quando você usa o método sem parâmetros `Create()` para criar uma nova instância. Chaves assimétricas podem ser armazenadas para uso em várias sessões ou geradas apenas para uma sessão. Embora a chave pública possa ser disponibilizada para o público geral, a chave privada deve ser bastante protegida.  
  
 Um par de chaves pública/privada é gerado sempre que uma nova instância de uma classe de algoritmo assimétrico é criada. Depois que uma nova instância da classe é criada, as informações de chave podem ser extraídas usando o <xref:System.Security.Cryptography.RSA.ExportParameters%2A> método, que retorna uma <xref:System.Security.Cryptography.RSAParameters> estrutura que contém as informações de chave. O método aceita um valor booliano que indica se deve retornar apenas as informações de chave pública ou para retornar as informações de chave pública e de chave privada.

Também há outros métodos que permitem extrair informações de chave, como:

* <xref:System.Security.Cryptography.RSA.ExportRSAPublicKey%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.RSA.ExportRSAPrivateKey%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.AsymmetricAlgorithm.ExportSubjectPublicKeyInfo%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.AsymmetricAlgorithm.ExportPkcs8PrivateKey%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.AsymmetricAlgorithm.ExportEncryptedPkcs8PrivateKey%2A?displayProperty=nameWithType>

Uma instância **RSA** pode ser inicializada para o valor de uma estrutura **RSAParameters** usando o <xref:System.Security.Cryptography.RSA.ImportParameters%2A> método. Ou crie uma nova instância usando o <xref:System.Security.Cryptography.RSA.Create(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType> método.  
  
 As chaves privadas assimétricas nunca devem ser armazenadas no formato textual nem como texto sem formatação no computador local. Se precisar armazenar uma chave privada, você deverá usar um contêiner de chave. Para obter mais informações sobre como armazenar uma chave privada em um contêiner de chave, consulte [como armazenar chaves assimétricas em um contêiner de chave](how-to-store-asymmetric-keys-in-a-key-container.md).  
  
 O exemplo de código a seguir cria uma nova instância da classe **RSA** , criando um par de chaves pública/privada e salva as informações de chave pública em uma estrutura **RSAParameters** .  
  
```vb  
'Generate a public/private key pair.  
Dim rsa as RSA = RSA.Create()  
'Save the public key information to an RSAParameters structure.  
Dim rsaKeyInfo As RSAParameters = rsa.ExportParameters(false)  
```  
  
```csharp  
//Generate a public/private key pair.  
RSA rsa = RSA.Create();  
//Save the public key information to an RSAParameters structure.  
RSAParameters rsaKeyInfo = rsa.ExportParameters(false);  
```  
  
## <a name="see-also"></a>Confira também

- [Criptografando dados](encrypting-data.md)
- [Descriptografando dados](decrypting-data.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Como: armazenar chaves assimétricas em um contêiner de chave](how-to-store-asymmetric-keys-in-a-key-container.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
