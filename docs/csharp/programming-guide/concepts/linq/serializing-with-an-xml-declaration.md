---
title: Serializando com uma declaração XML (C#)
description: Saiba mais sobre as configurações em que a serialização em C# gera uma declaração XML, incluindo a serialização para um arquivo, TextWriter e XmlWriter.
ms.date: 07/20/2015
ms.assetid: c237fa4a-a042-40fd-886f-17b54c66bb75
ms.openlocfilehash: 7e91b61f037d28149f7c2355f4233dc319b54627
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302354"
---
# <a name="serializing-with-an-xml-declaration-c"></a>Serializando com uma declaração XML (C#)
Este tópico descreve como controlar se a serialização gera uma declaração XML.  
  
## <a name="xml-declaration-generation"></a>Geração da declaração XML  
 Serializar para um <xref:System.IO.File> ou <xref:System.IO.TextWriter> usando o método <xref:System.Xml.Linq.XElement.Save%2A?displayProperty=nameWithType> ou o método <xref:System.Xml.Linq.XDocument.Save%2A?displayProperty=nameWithType> gera uma declaração XML. Quando você serializa para um <xref:System.Xml.XmlWriter>, as configurações do gravador (especificadas em um objeto <xref:System.Xml.XmlWriterSettings>) determinam se uma declaração XML é gerada ou não.  
  
 Se você estiver serialização para uma cadeia de caracteres usando o método `ToString`, o XML resultante não incluirá uma declaração XML.  
  
### <a name="serializing-with-an-xml-declaration"></a>Serializando com uma declaração XML  
 O exemplo a seguir cria um <xref:System.Xml.Linq.XElement>, salva o documento em um arquivo e imprime o arquivo no console:  
  
```csharp  
XElement root = new XElement("Root",  
    new XElement("Child", "child content")  
);  
root.Save("Root.xml");  
string str = File.ReadAllText("Root.xml");  
Console.WriteLine(str);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<Root>  
  <Child>child content</Child>  
</Root>  
```  
  
### <a name="serializing-without-an-xml-declaration"></a>Serializando sem uma declaração XML  
 O exemplo a seguir mostra como salvar um <xref:System.Xml.Linq.XElement> em um <xref:System.Xml.XmlWriter>.  
  
```csharp  
StringBuilder sb = new StringBuilder();  
XmlWriterSettings xws = new XmlWriterSettings();  
xws.OmitXmlDeclaration = true;  
  
using (XmlWriter xw = XmlWriter.Create(sb, xws)) {  
    XElement root = new XElement("Root",  
        new XElement("Child", "child content")  
    );  
    root.Save(xw);  
}  
Console.WriteLine(sb.ToString());  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root><Child>child content</Child></Root>  
```  
  
## <a name="see-also"></a>Veja também

- [Serializando árvores XML (C#)](serializing-to-files-textwriters-and-xmlwriters.md)
