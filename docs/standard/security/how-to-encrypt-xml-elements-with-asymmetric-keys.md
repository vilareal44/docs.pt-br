---
title: 'Como: criptografar elementos XML com chaves assimétricas'
ms.date: 07/14/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [.NET], asymmetric keys
- AES algorithm
- System.Security.Cryptography.RSA class
- asymmetric keys [.NET]
- System.Security.Cryptography.EncryptedXml class
- XML encryption
- key containers
- Advanced Encryption Standard algorithm
- encryption [.NET], asymmetric keys
ms.assetid: a164ba4f-e596-4bbe-a9ca-f214fe89ed48
ms.openlocfilehash: 1c824b00a1df920108cfcd8c4590b680020cdf3e
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555781"
---
# <a name="how-to-encrypt-xml-elements-with-asymmetric-keys"></a>Como: criptografar elementos XML com chaves assimétricas

Você pode usar as classes no <xref:System.Security.Cryptography.Xml> namespace para criptografar um elemento em um documento XML.  A criptografia XML é uma maneira padrão de trocar ou armazenar dados XML criptografados, sem se preocupar com os dados que estão sendo facilmente lidos.  Para obter mais informações sobre o padrão de criptografia XML, consulte a especificação do World Wide Web Consortium (W3C) para criptografia XML localizada em <https://www.w3.org/TR/xmldsig-core/> .  
  
 Você pode usar a criptografia XML para substituir qualquer elemento XML ou documento por um `EncryptedData` elemento <> que contenha os dados XML criptografados.  O `EncryptedData` elemento> de <também pode conter subelementos que incluem informações sobre as chaves e os processos usados durante a criptografia.  A criptografia XML permite que um documento contenha vários elementos criptografados e permite que um elemento seja criptografado várias vezes.  O exemplo de código neste procedimento mostra como criar um elemento <`EncryptedData`> juntamente com vários outros subelementos que você pode usar posteriormente durante a descriptografia.  
  
 Este exemplo criptografa um elemento XML usando duas chaves.  Ele gera um par de chaves pública/privada RSA e salva o par de chaves em um contêiner de chave segura.  Em seguida, o exemplo cria uma chave de sessão separada usando o algoritmo de criptografia AES (AES).  O exemplo usa a chave de sessão AES para criptografar o documento XML e, em seguida, usa a chave pública RSA para criptografar a chave de sessão AES.  Por fim, o exemplo salva a chave de sessão AES criptografada e os dados XML criptografados no documento XML em um novo `EncryptedData` elemento <>.  
  
 Para descriptografar o elemento XML, recupere a chave privada RSA do contêiner de chave, use-a para descriptografar a chave de sessão e, em seguida, use a chave de sessão para descriptografar o documento.  Para obter mais informações sobre como descriptografar um elemento XML que foi criptografado usando esse procedimento, consulte [como: descriptografar elementos XML com chaves assimétricas](how-to-decrypt-xml-elements-with-asymmetric-keys.md).  
  
 Este exemplo é apropriado para situações em que vários aplicativos precisam compartilhar dados criptografados ou onde um aplicativo precisa salvar dados criptografados entre os horários em que é executado.
  
### <a name="to-encrypt-an-xml-element-with-an-asymmetric-key"></a>Para criptografar um elemento XML com uma chave assimétrica  
  
1. Crie um <xref:System.Security.Cryptography.CspParameters> objeto e especifique o nome do contêiner de chave.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#2)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#2)]  
  
2. Gere uma chave simétrica usando a <xref:System.Security.Cryptography.RSACryptoServiceProvider> classe.  A chave é salva automaticamente no contêiner de chave quando você passa o <xref:System.Security.Cryptography.CspParameters> objeto para o construtor da <xref:System.Security.Cryptography.RSACryptoServiceProvider> classe.  Essa chave será usada para criptografar a chave de sessão AES e poderá ser recuperada posteriormente para descriptografá-la.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#3)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#3)]  
  
3. Crie um <xref:System.Xml.XmlDocument> objeto carregando um arquivo XML do disco.  O <xref:System.Xml.XmlDocument> objeto contém o elemento XML a ser criptografado.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#4)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#4)]  
  
4. Localize o elemento especificado no <xref:System.Xml.XmlDocument> objeto e crie um novo <xref:System.Xml.XmlElement> objeto para representar o elemento que você deseja criptografar. Neste exemplo, o `"creditcard"` elemento é criptografado.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#5)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#5)]  
  
5. Crie uma nova chave de sessão usando a <xref:System.Security.Cryptography.Aes> classe.  Essa chave irá criptografar o elemento XML e, em seguida, será criptografada e colocada no documento XML.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#6)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#6)]  
  
6. Crie uma nova instância da <xref:System.Security.Cryptography.Xml.EncryptedXml> classe e use-a para criptografar o elemento especificado usando a chave de sessão.  O <xref:System.Security.Cryptography.Xml.EncryptedXml.EncryptData%2A> método retorna o elemento Encrypted como uma matriz de bytes criptografados.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#7)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#7)]  
  
7. Construa um <xref:System.Security.Cryptography.Xml.EncryptedData> objeto e popule-o com o identificador de URL do elemento XML criptografado.  Esse identificador de URL permite que uma parte descriptografada saiba que o XML contém um elemento criptografado.  Você pode usar o <xref:System.Security.Cryptography.Xml.EncryptedXml.XmlEncElementUrl> campo para especificar o identificador de URL.  O elemento XML de texto não criptografado será substituído por um `EncryptedData` elemento <> encapsulado por esse <xref:System.Security.Cryptography.Xml.EncryptedData> objeto.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#8)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#8)]  
  
8. Crie um <xref:System.Security.Cryptography.Xml.EncryptionMethod> objeto que é inicializado para o identificador de URL do algoritmo criptográfico usado para gerar a chave de sessão.  Passe o <xref:System.Security.Cryptography.Xml.EncryptionMethod> objeto para a <xref:System.Security.Cryptography.Xml.EncryptedType.EncryptionMethod%2A> propriedade.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#9)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#9)]  
  
9. Crie um <xref:System.Security.Cryptography.Xml.EncryptedKey> objeto para conter a chave de sessão criptografada.  Criptografe a chave de sessão, adicione-a ao <xref:System.Security.Cryptography.Xml.EncryptedKey> objeto e insira um nome de chave de sessão e uma URL de identificador de chave.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#10)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#10)]  
  
10. Crie um novo <xref:System.Security.Cryptography.Xml.DataReference> objeto que mapeie os dados criptografados para uma chave de sessão específica.  Essa etapa opcional permite que você especifique facilmente que várias partes de um documento XML foram criptografadas por uma única chave.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#11)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#11)]  
  
11. Adicione a chave criptografada ao <xref:System.Security.Cryptography.Xml.EncryptedData> objeto.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#12)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#12)]  
  
12. Crie um novo <xref:System.Security.Cryptography.Xml.KeyInfo> objeto para especificar o nome da chave RSA.  Adicione-o ao <xref:System.Security.Cryptography.Xml.EncryptedData> objeto. Isso ajuda a parte descriptografada a identificar a chave assimétrica correta a ser usada ao descriptografar a chave de sessão.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#13)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#13)]  
  
13. Adicione os dados do elemento criptografado ao <xref:System.Security.Cryptography.Xml.EncryptedData> objeto.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#14](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#14)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#14)]  
  
14. Substitua o elemento do objeto original <xref:System.Xml.XmlDocument> pelo <xref:System.Security.Cryptography.Xml.EncryptedData> elemento.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#15](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#15)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#15)]  
  
15. Salve o <xref:System.Xml.XmlDocument> objeto.  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#16](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#16)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#16)]  
  
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
  
 [!code-csharp[HowToEncryptXMLElementAsymmetric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#1)]
 [!code-vb[HowToEncryptXMLElementAsymmetric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>Compilando o código  
  
- Em um projeto que tem como destino .NET Framework, inclua uma referência a `System.Security.dll` .

- Em um projeto direcionado para .NET Core ou .NET 5, instale o pacote NuGet [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml).
  
- Inclua os seguintes namespaces: <xref:System.Xml> , <xref:System.Security.Cryptography> e <xref:System.Security.Cryptography.Xml> .  
  
## <a name="net-security"></a>Segurança do .NET

Nunca armazene uma chave de criptografia simétrica em texto não criptografado ou transfira uma chave simétrica entre máquinas em texto não criptografado.  Além disso, nunca armazene ou transfira a chave privada de um par de chaves assimétricas em texto não criptografado.  Para obter mais informações sobre chaves de criptografia simétrica e assimétrica, consulte [gerando chaves para criptografia e descriptografia](generating-keys-for-encryption-and-decryption.md).  
  
Nunca incorpore uma chave diretamente no seu código-fonte.  Chaves inseridas podem ser facilmente lidas de um assembly usando o [Ildasm.exe (desmontador Il)](../../framework/tools/ildasm-exe-il-disassembler.md) ou abrindo o assembly em um editor de texto como o bloco de notas.  
  
Quando você terminar de usar uma chave criptográfica, limpe-a da memória definindo cada byte como zero ou chamando o <xref:System.Security.Cryptography.SymmetricAlgorithm.Clear%2A> método da classe de criptografia gerenciada.  Às vezes, as chaves criptográficas podem ser lidas da memória por um depurador ou lidas de um disco rígido se o local da memória for paginado no disco.  
  
## <a name="see-also"></a>Confira também

- [Modelo de criptografia](cryptography-model.md)
- [Serviços criptográficos](cryptographic-services.md)
- [Criptografia de plataforma cruzada](cross-platform-cryptography.md)- <xref:System.Security.Cryptography.Xml>
- [Como: descriptografar elementos XML com chaves assimétricas](how-to-decrypt-xml-elements-with-asymmetric-keys.md)
- [Proteção de dados do ASP.NET Core](/aspnet/core/security/data-protection/introduction)
