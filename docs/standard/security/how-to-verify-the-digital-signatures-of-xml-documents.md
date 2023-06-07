---
title: 'Como: verificar as assinaturas digitais de documentos XML'
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- System.Security.Cryptography.SignedXml class
- signatures, cryptographic
- System.Security.Cryptography.RSA class
- verifying signatures
- checking signatures
- XML digital signatures
- digital signatures, verifying
ms.assetid: a4d5ceb1-b9f5-47e8-9e4a-a2b39110002f
ms.openlocfilehash: b9b2dc6a558d1fd6acd2922a7c8ad82ce8776c26
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557041"
---
# <a name="how-to-verify-the-digital-signatures-of-xml-documents"></a>Como: verificar as assinaturas digitais de documentos XML

Você pode usar as classes no <xref:System.Security.Cryptography.Xml> namespace para verificar os dados XML assinados com uma assinatura digital. As assinaturas digitais XML (XMLDSIG) permitem que você verifique se os dados não foram alterados após sua assinatura. Para obter mais informações sobre o padrão XMLDSIG, consulte a especificação do World Wide Web Consortium (W3C) em <https://www.w3.org/TR/xmldsig-core/> .
  
> [!NOTE]
> O código neste artigo aplica-se ao Windows.

O exemplo de código neste procedimento demonstra como verificar uma assinatura digital XML contida em um elemento <`Signature`>.  O exemplo recupera uma chave pública RSA de um contêiner de chave e, em seguida, usa a chave para verificar a assinatura.  
  
Para obter informações sobre como criar uma assinatura digital que pode ser verificada usando essa técnica, consulte [como assinar documentos XML com assinaturas digitais](how-to-sign-xml-documents-with-digital-signatures.md).  
  
### <a name="to-verify-the-digital-signature-of-an-xml-document"></a>Para verificar a assinatura digital de um documento XML  
  
1. Para verificar o documento, você deve usar a mesma chave assimétrica que foi usada para assinatura.  Crie um <xref:System.Security.Cryptography.CspParameters> objeto e especifique o nome do contêiner de chave que foi usado para assinatura.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#2)]
     [!code-vb[HowToVerifyXMLDocumentRSA#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#2)]  
  
2. Recupere a chave pública usando a <xref:System.Security.Cryptography.RSACryptoServiceProvider> classe.  A chave é carregada automaticamente do contêiner de chave por nome quando você passa o <xref:System.Security.Cryptography.CspParameters> objeto para o construtor da <xref:System.Security.Cryptography.RSACryptoServiceProvider> classe.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#3)]
     [!code-vb[HowToVerifyXMLDocumentRSA#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#3)]  
  
3. Crie um <xref:System.Xml.XmlDocument> objeto carregando um arquivo XML do disco.  O <xref:System.Xml.XmlDocument> objeto contém o documento XML assinado para verificar.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#4)]
     [!code-vb[HowToVerifyXMLDocumentRSA#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#4)]  
  
4. Crie um novo <xref:System.Security.Cryptography.Xml.SignedXml> objeto e passe o <xref:System.Xml.XmlDocument> objeto para ele.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#5)]
     [!code-vb[HowToVerifyXMLDocumentRSA#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#5)]  
  
5. Localize o `signature` elemento <> e crie um novo <xref:System.Xml.XmlNodeList> objeto.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#6)]
     [!code-vb[HowToVerifyXMLDocumentRSA#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#6)]  
  
6. Carregue o XML do primeiro elemento <`signature`> no <xref:System.Security.Cryptography.Xml.SignedXml> objeto.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#7)]
     [!code-vb[HowToVerifyXMLDocumentRSA#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#7)]  
  
7. Verifique a assinatura usando o <xref:System.Security.Cryptography.Xml.SignedXml.CheckSignature%2A> método e a chave pública RSA.  Esse método retorna um valor booliano que indica êxito ou falha.  
  
     [!code-csharp[HowToVerifyXMLDocumentRSA#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#8)]
     [!code-vb[HowToVerifyXMLDocumentRSA#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#8)]  
  
## <a name="example"></a>Exemplo

Este exemplo pressupõe que um arquivo chamado `"test.xml"` existe no mesmo diretório que o programa compilado.  O `"test.xml"` arquivo deve ser assinado usando as técnicas descritas em [como assinar documentos XML com assinaturas digitais](how-to-sign-xml-documents-with-digital-signatures.md).  
  
[!code-csharp[HowToVerifyXMLDocumentRSA#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/cs/sample.cs#1)]
[!code-vb[HowToVerifyXMLDocumentRSA#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToVerifyXMLDocumentRSA/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Em um projeto que tem como destino .NET Framework, inclua uma referência a `System.Security.dll` .

- Em um projeto direcionado para .NET Core ou .NET 5, instale o pacote NuGet [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml).
  
- Inclua os seguintes namespaces: <xref:System.Xml> , <xref:System.Security.Cryptography> e <xref:System.Security.Cryptography.Xml> .  
  
## <a name="net-security"></a>Segurança do .NET

Nunca armazene ou transfira a chave privada de um par de chaves assimétricas em texto não criptografado.  Para obter mais informações sobre chaves de criptografia simétrica e assimétrica, consulte [gerando chaves para criptografia e descriptografia](generating-keys-for-encryption-and-decryption.md).  
  
Nunca incorpore uma chave privada diretamente em seu código-fonte.  Chaves inseridas podem ser facilmente lidas de um assembly usando o [Ildasm.exe (desmontador Il)](../../framework/tools/ildasm-exe-il-disassembler.md) ou abrindo o assembly em um editor de texto como o bloco de notas.  
  
## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.Xml>
- [Como: assinar documento XML com assinaturas digitais](how-to-sign-xml-documents-with-digital-signatures.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
