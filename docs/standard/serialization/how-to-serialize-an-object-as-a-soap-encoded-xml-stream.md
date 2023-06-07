---
title: Como serializar um objeto como um fluxo XML codificado para SOAP
description: Saiba como serializar um objeto como um fluxo XML codificado em SOAP. A classe XmlSerializer pode ser usada para serializar classes e gerar mensagens SOAP codificadas.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SOAP, XML serialization
- XML serialization, SOAP
- serialization, SOAP
ms.assetid: af406e0a-fa3a-46dd-a7ba-c80731eba3a0
ms.openlocfilehash: 1d38c4e334439ef41b4d4429e52cff04c6463573
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84291559"
---
# <a name="how-to-serialize-an-object-as-a-soap-encoded-xml-stream"></a>Como serializar um objeto como um fluxo XML codificado para SOAP
  
 Como a mensagem de SOAP é criada usando XML, a classe <xref:System.Xml.Serialization.XmlSerializer> pode ser usado para serializar classes e gerar mensagens SOAP codificadas. O XML resultante está em conformidade com a [seção 5 do documento "Protocolo SOAP 1.1" do World Wide Web Consortium](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/#_Toc478383512). Quando você está criando um serviço Web XML que se comunica por meio de mensagens SOAP, pode personalizar o fluxo XML aplicando um conjunto de atributos SOAP especiais para classes e membros de classes. Para obter mais informações, consulte [Atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md).  
  
### <a name="to-serialize-an-object-as-a-soap-encoded-xml-stream"></a>Para serializar um objeto como um fluxo XML codificado para SOAP  
  
1. Criar a classe usando a [Ferramenta de Definição de Esquema XML (Xsd.exe)](xml-schema-definition-tool-xsd-exe.md).  
  
2. Aplique um ou mais dos atributos especiais localizados em `System.Xml.Serialization`. Consulte a lista em "Atributos que controlam a serialização SOAP codificada".  
  
3. Crie um `XmlTypeMapping` criando um novo `SoapReflectionImporter` e invocando o método `ImportTypeMapping` com o tipo da classe serializada.  
  
     O exemplo de código a seguir chama o método `ImportTypeMapping` da classe `SoapReflectionImporter` para criar um `XmlTypeMapping`.  
  
    ```vb  
    ' Serializes a class named Group as a SOAP message.  
    Dim myTypeMapping As XmlTypeMapping =
        New SoapReflectionImporter().ImportTypeMapping(GetType(Group))  
    ```  
  
    ```csharp  
    // Serializes a class named Group as a SOAP message.  
    XmlTypeMapping myTypeMapping =
        new SoapReflectionImporter().ImportTypeMapping(typeof(Group));
    ```  
  
4. Cria uma instância da classe `XmlSerializer` passando o `XmlTypeMapping` para o construtor <xref:System.Xml.Serialization.XmlSerializer.%23ctor%28System.Xml.Serialization.XmlTypeMapping%29>.  
  
    ```vb  
    Dim mySerializer As XmlSerializer = New XmlSerializer(myTypeMapping)  
    ```  
  
    ```csharp  
    XmlSerializer mySerializer = new XmlSerializer(myTypeMapping);  
    ```  
  
5. Chame o método `Serialize` or `Deserialize`.  
  
## <a name="example"></a>Exemplo  
  
```vb  
' Serializes a class named Group as a SOAP message.  
Dim myTypeMapping As XmlTypeMapping =
    New SoapReflectionImporter().ImportTypeMapping(GetType(Group))
Dim mySerializer As XmlSerializer = New XmlSerializer(myTypeMapping)  
```  
  
```csharp  
// Serializes a class named Group as a SOAP message.  
XmlTypeMapping myTypeMapping =
    new SoapReflectionImporter().ImportTypeMapping(typeof(Group));
XmlSerializer mySerializer = new XmlSerializer(myTypeMapping);  
```  
  
## <a name="see-also"></a>Confira também

- [Serialização de XML e SOAP](xml-and-soap-serialization.md)
- [Atributos que controlam a serialização SOAP codificada](attributes-that-control-encoded-soap-serialization.md)
- [Serialização XML com serviços Web XML](xml-serialization-with-xml-web-services.md)
- [Como serializar um objeto](how-to-serialize-an-object.md)
- [Como desserializar um objeto](how-to-deserialize-an-object.md)
- [Como substituir a serialização XML de SOAP codificada](how-to-override-encoded-soap-xml-serialization.md)
