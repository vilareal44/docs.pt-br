---
title: Como gravar dados de objeto em um arquivo XML (C#)
description: Este exemplo de C# grava o objeto de uma classe em um arquivo XML usando a classe XmlSerializer. Saiba como compilar o código.
ms.date: 07/20/2015
ms.assetid: 7681eb98-703d-4005-a369-26a7bca0f894
ms.openlocfilehash: c88a85f8bc65a195f404dab6aa39675bac7e4f84
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303121"
---
# <a name="how-to-write-object-data-to-an-xml-file-c"></a>Como gravar dados de objeto em um arquivo XML (C#)
Este exemplo grava o objeto de uma classe para um arquivo XML usando a classe <xref:System.Xml.Serialization.XmlSerializer>.  
  
## <a name="example"></a>Exemplo  
  
```csharp  
public class XMLWrite  
{  
  
   static void Main(string[] args)  
    {  
        WriteXML();  
    }  
  
    public class Book  
    {  
        public String title;
    }  
  
    public static void WriteXML()  
    {  
        Book overview = new Book();  
        overview.title = "Serialization Overview";  
        System.Xml.Serialization.XmlSerializer writer =
            new System.Xml.Serialization.XmlSerializer(typeof(Book));  
  
        var path = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments) + "//SerializationOverview.xml";  
        System.IO.FileStream file = System.IO.File.Create(path);  
  
        writer.Serialize(file, overview);  
        file.Close();  
    }  
}  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
 A classe precisa ter um construtor público sem parâmetros.  
  
## <a name="robust-programming"></a>Programação robusta  
 As seguintes condições podem causar uma exceção:  
  
- A classe que está sendo serializada não tem um construtor público sem parâmetros.  
  
- O arquivo existe e é somente leitura (<xref:System.IO.IOException>).  
  
- O caminho é muito longo (<xref:System.IO.PathTooLongException>).  
  
- O disco está cheio (<xref:System.IO.IOException>).  
  
## <a name="net-security"></a>Segurança do .NET  
 Este exemplo cria um novo arquivo, se o arquivo ainda não existe. Se um aplicativo precisar criar um arquivo, ele precisará de acesso `Create` para a pasta. Se o arquivo já existe, o aplicativo precisa apenas de acesso `Write`, um privilégio menor. Sempre que possível, é mais seguro criar o arquivo durante a implantação e somente conceder acesso `Read` a um único arquivo, em vez de acesso `Create` a uma pasta.  
  
## <a name="see-also"></a>Veja também

- <xref:System.IO.StreamWriter>
- [Como ler dados de objeto de um arquivo XML (C#)](./how-to-read-object-data-from-an-xml-file.md)
- [Serialização (C#)](./index.md)
