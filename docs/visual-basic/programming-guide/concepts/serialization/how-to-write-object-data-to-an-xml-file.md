---
title: Como gravar dados de objeto em um arquivo XML
ms.date: 07/20/2015
ms.assetid: f7966480-5ed2-43ac-9894-33427436de2a
ms.openlocfilehash: 9608a48cb8b3fac1c71affa7a0a17e9789f94b18
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413148"
---
# <a name="how-to-write-object-data-to-an-xml-file-visual-basic"></a>Como gravar dados de objeto em um arquivo XML (Visual Basic)
Este exemplo grava o objeto de uma classe para um arquivo XML usando a classe <xref:System.Xml.Serialization.XmlSerializer>.  
  
## <a name="example"></a>Exemplo  
  
```vb  
Public Module XMLWrite  
  
    Sub Main()  
        WriteXML()  
    End Sub  
  
    Public Class Book  
        Public Title As String  
    End Class  
  
    Public Sub WriteXML()  
        Dim overview As New Book  
        overview.Title = "Serialization Overview"  
        Dim writer As New System.Xml.Serialization.XmlSerializer(GetType(Book))  
        Dim file As New System.IO.StreamWriter(  
            "c:\temp\SerializationOverview.xml")  
        writer.Serialize(file, overview)  
        file.Close()  
    End Sub  
End Module  
```  
  
## <a name="compile-the-code"></a>Compilar o código  
 A classe deve ter um construtor público sem parâmetros.  
  
## <a name="robust-programming"></a>Programação robusta  
 As seguintes condições podem causar uma exceção:  
  
- A classe que está sendo serializada não tem um construtor público sem parâmetros.  
  
- O arquivo existe e é somente leitura (<xref:System.IO.IOException>).  
  
- O caminho é muito longo (<xref:System.IO.PathTooLongException>).  
  
- O disco está cheio (<xref:System.IO.IOException>).  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Este exemplo cria um novo arquivo, se o arquivo ainda não existe. Se um aplicativo precisar criar um arquivo, ele precisará de acesso `Create` para a pasta. Se o arquivo já existe, o aplicativo precisa apenas de acesso `Write`, um privilégio menor. Sempre que possível, é mais seguro criar o arquivo durante a implantação e somente conceder acesso `Read` a um único arquivo, em vez de acesso `Create` a uma pasta.  
  
## <a name="see-also"></a>Confira também

- <xref:System.IO.StreamWriter>
- [Como ler dados de objeto de um arquivo XML (Visual Basic)](how-to-read-object-data-from-an-xml-file.md)
- [Serialização (Visual Basic)](index.md)
