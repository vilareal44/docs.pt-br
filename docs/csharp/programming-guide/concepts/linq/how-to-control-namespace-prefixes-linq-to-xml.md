---
title: Como controlar prefixos de namespace (C#) (LINQ to XML)
description: Saiba como controlar prefixos de namespace ao serializar uma árvore XML em LINQ to XML em C#. Algumas situações exigem o controle de prefixos de namespace.
ms.date: 07/20/2015
ms.assetid: 64de5186-b81a-4ddd-8327-8693df59a01b
ms.openlocfilehash: b0c5cbfa7488f3a7105595830ef6765e6bfb1f12
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87105311"
---
# <a name="how-to-control-namespace-prefixes-c-linq-to-xml"></a>Como controlar prefixos de namespace (C#) (LINQ to XML)
Este tópico descreve como você pode controlar prefixos de namespace ao serializar uma árvore XML.  
  
 Em muitas situações, não é necessário controlar prefixos de namespace.  
  
 No entanto, determinadas ferramentas de programação XML requerem controle específico de prefixos de namespace. Por exemplo, você pode manipular uma folha de estilos XSLT ou um documento XAML que contenha as expressões XPath inseridas que se referem a prefixos de namespace específicos. Nesse caso, é importante que o documento seja serializado com esses prefixos específicos.  
  
 Esse é o motivo mais comum para controlar prefixos de namespace.  
  
 Outra razão comum para controlar prefixos de namespace é que você deseja que os usuários editem manualmente o documento XML e deseja criar prefixos de namespace que sejam convenientes para o usuário digitar. Por exemplo, você pode estar gerando um documento XSD. As convenções de esquemas sugerem que você use `xs` ou `xsd` como o prefixo do namespace do esquema.  
  
 Para controlar prefixos de namespace, você insere os atributos que declaram os namespaces. Se você declarar namespaces com prefixos específicos, o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] tentará respeitar os prefixos de namespace ao serializar.  
  
 Para criar um atributo que declare um namespace com um prefixo, você cria um atributo onde o namespace do nome do atributo é <xref:System.Xml.Linq.XNamespace.Xmlns%2A>, e o nome do atributo é o prefixo do namespace. O valor do atributo é o URI do namespace.  
  
## <a name="example"></a>Exemplo  
 Esse exemplo declara dois namespaces. Ele especifica que o namespace `http://www.adventure-works.com` tem o prefixo `aw` e que o namespace `www.fourthcoffee.com` tem o prefixo `fc`.  
  
```csharp  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace fc = "www.fourthcoffee.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com"),  
    new XAttribute(XNamespace.Xmlns + "fc", "www.fourthcoffee.com"),  
    new XElement(fc + "Child",  
        new XElement(aw + "DifferentChild", "other content")  
    ),  
    new XElement(aw + "Child2", "c2 content"),  
    new XElement(fc + "Child3", "c3 content")  
);  
Console.WriteLine(root);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com" xmlns:fc="www.fourthcoffee.com">  
  <fc:Child>  
    <aw:DifferentChild>other content</aw:DifferentChild>  
  </fc:Child>  
  <aw:Child2>c2 content</aw:Child2>  
  <fc:Child3>c3 content</fc:Child3>  
</aw:Root>  
```  
  
## <a name="see-also"></a>Confira também

- [Visão geral sobre namespaces (LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md)
