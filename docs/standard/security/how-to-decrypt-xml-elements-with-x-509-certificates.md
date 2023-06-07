---
title: 'Como: descriptografar elementos XML com certificados X.509'
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- System.Security.Cryptography.EncryptedXml class
- XML encryption
- System.Security.Cryptography.X509Certificate2 class
- decryption
- X.509 certificates
- certificates, X.509 certificates
ms.assetid: bd015722-d88d-408d-8ca8-e4e475c441ed
ms.openlocfilehash: f60947a3b722f33c88b696d47c6a4000a1cb076b
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555728"
---
# <a name="how-to-decrypt-xml-elements-with-x509-certificates"></a>Como: descriptografar elementos XML com certificados X.509

Você pode usar as classes no <xref:System.Security.Cryptography.Xml> namespace para criptografar e descriptografar um elemento dentro de um documento XML.  A criptografia XML é uma maneira padrão de trocar ou armazenar dados XML criptografados, sem se preocupar com os dados que estão sendo facilmente lidos.  Para obter mais informações sobre o padrão de criptografia XML, consulte a especificação do World Wide Web Consortium (W3C) para criptografia XML localizada em <https://www.w3.org/TR/xmldsig-core/> .  
  
 Este exemplo descriptografa um elemento XML que foi criptografado usando os métodos descritos em: [como: Criptografar elementos XML com certificados X. 509](how-to-encrypt-xml-elements-with-x-509-certificates.md).  Ele encontra um `EncryptedData` elemento <>, descriptografa o elemento e, em seguida, substitui o elemento pelo elemento XML original de texto não criptografado.  
  
O exemplo de código neste procedimento descriptografa um elemento XML usando um certificado X. 509 do repositório de certificados local da conta de usuário atual.  O exemplo usa o <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> método para recuperar automaticamente o certificado X. 509 e descriptografar uma chave de sessão armazenada no `EncryptedKey` elemento <> do `EncryptedData` elemento <>.  O <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> método, então, usa automaticamente a chave de sessão para descriptografar o elemento XML.  
  
Este exemplo é apropriado para situações em que vários aplicativos precisam compartilhar dados criptografados ou onde um aplicativo precisa salvar dados criptografados entre os horários em que é executado.  
  
### <a name="to-decrypt-an-xml-element-with-an-x509-certificate"></a>Para descriptografar um elemento XML com um certificado X.509  
  
1. Crie um <xref:System.Xml.XmlDocument> objeto carregando um arquivo XML do disco.  O <xref:System.Xml.XmlDocument> objeto contém o elemento XML a ser descriptografado.  
  
     [!code-csharp[HowToDecryptXMLElementX509#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#2)]
     [!code-vb[HowToDecryptXMLElementX509#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#2)]  
  
2. Crie um novo <xref:System.Security.Cryptography.Xml.EncryptedXml> objeto passando o <xref:System.Xml.XmlDocument> objeto para o construtor.  
  
     [!code-csharp[HowToDecryptXMLElementX509#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#3)]
     [!code-vb[HowToDecryptXMLElementX509#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#3)]  
  
3. Descriptografe o documento XML usando o <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> método.  
  
     [!code-csharp[HowToDecryptXMLElementX509#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#4)]
     [!code-vb[HowToDecryptXMLElementX509#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#4)]  
  
4. Salve o <xref:System.Xml.XmlDocument> objeto.  
  
     [!code-csharp[HowToDecryptXMLElementX509#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#5)]
     [!code-vb[HowToDecryptXMLElementX509#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#5)]  
  
## <a name="example"></a>Exemplo

Este exemplo pressupõe que um arquivo chamado `"test.xml"` existe no mesmo diretório que o programa compilado.  Ele também pressupõe que `"test.xml"` contém um `"creditcard"` elemento.  Você pode inserir o XML a seguir em um arquivo chamado `test.xml` e usá-lo com este exemplo.  
  
```xml  
<root>  
    <creditcard>  
        <number>19834209</number>  
        <expiry>02/02/2002</expiry>  
    </creditcard>  
</root>  
```  
  
[!code-csharp[HowToDecryptXMLElementX509#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementX509/cs/sample.cs#1)]
[!code-vb[HowToDecryptXMLElementX509#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementX509/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Em um projeto que tem como destino .NET Framework, inclua uma referência a `System.Security.dll` .

- Em um projeto direcionado para .NET Core ou .NET 5, instale o pacote NuGet [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml).

- Inclua os seguintes namespaces: <xref:System.Xml> , <xref:System.Security.Cryptography> e <xref:System.Security.Cryptography.Xml> .  
  
## <a name="net-security"></a>Segurança do .NET

O certificado X. 509 usado neste exemplo é apenas para fins de teste.  Os aplicativos devem usar um certificado X. 509 gerado por uma autoridade de certificação confiável.  
  
## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.Xml>
- [Como: criptografar elementos XML com certificados X.509](how-to-encrypt-xml-elements-with-x-509-certificates.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
