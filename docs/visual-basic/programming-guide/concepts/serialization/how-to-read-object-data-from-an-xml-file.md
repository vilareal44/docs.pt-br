---
title: Como ler dados de objeto a partir de um arquivo XML
ms.date: 07/20/2015
ms.assetid: 1e1423bf-74a4-4dde-a3bb-ae1bfc0a68ed
ms.openlocfilehash: 7097ec146987aea7855da40dd30f9cd3c17d8ce4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413161"
---
# <a name="how-to-read-object-data-from-an-xml-file-visual-basic"></a>Como ler dados de objeto de um arquivo XML (Visual Basic)
Este exemplo lê dados de objeto que foram previamente gravados em um arquivo XML usando a classe <xref:System.Xml.Serialization.XmlSerializer>.  
  
## <a name="example"></a>Exemplo  
  
```vb  
Public Class Book  
    Public Title As String  
End Class  
  
Public Sub ReadXML()  
    Dim reader As New System.Xml.Serialization.XmlSerializer(GetType(Book))  
    Dim file As New System.IO.StreamReader(  
        "c:\temp\SerializationOverview.xml")  
    Dim overview As Book  
    overview = CType(reader.Deserialize(file), Book)  
    Console.WriteLine(overview.Title)  
End Sub  
```  
  
## <a name="compile-the-code"></a>Compilar o código  
 Substitua o nome de arquivo "c:\temp\SerializationOverview.xml" pelo nome do arquivo que contém os dados serializados. Para obter mais informações sobre a serialização de dados, consulte [como: gravar dados de objeto em um arquivo XML (Visual Basic)](how-to-write-object-data-to-an-xml-file.md).  
  
 A classe deve ter um construtor público sem parâmetros.  
  
 Somente propriedades e campos públicos são desserializados.  
  
## <a name="robust-programming"></a>Programação robusta  
 As seguintes condições podem causar uma exceção:  
  
- A classe que está sendo serializada não tem um construtor público sem parâmetros.  
  
- Os dados no arquivo não representam dados da classe a ser desserializada.  
  
- O arquivo não existe (<xref:System.IO.IOException>).  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Sempre verifique as entradas e nunca desserialize dados de uma fonte não confiável. O objeto recriado é executado em um computador local com as permissões do código que o desserializou. Verifique todas as entradas antes de usar os dados no seu aplicativo.  
  
## <a name="see-also"></a>Confira também

- <xref:System.IO.StreamWriter>
- [Como gravar dados de objeto em um arquivo XML (Visual Basic)](how-to-write-object-data-to-an-xml-file.md)
- [Serialização (Visual Basic)](index.md)
- [Guia de programação do Visual Basic](../../index.md)
