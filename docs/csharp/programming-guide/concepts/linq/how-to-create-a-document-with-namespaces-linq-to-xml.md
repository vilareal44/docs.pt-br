---
title: Como criar um documento com namespaces (C#) (LINQ to XML)
description: Saiba como criar um documento com um namespace em LINQ to XML em C# usando um objeto XNamespace para combinar o namespace com o nome local.
ms.date: 07/20/2015
ms.assetid: 37e63c57-f86d-47ac-88a7-2c2d107def30
ms.openlocfilehash: 6472baefc73285af1c6dca0bfe7d874003940fc4
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103395"
---
# <a name="how-to-create-a-document-with-namespaces-c-linq-to-xml"></a>Como criar um documento com namespaces (C#) (LINQ to XML)
Este tópico mostra como criar documentos com namespaces.  
  
## <a name="example"></a>Exemplo  
 Para criar um elemento ou um atributo que esteja em um namespace, você primeiro declara e inicializa um objeto <xref:System.Xml.Linq.XNamespace>. Em seguida, você usa a sobrecarga do operador de adição para combinar o namespace com o nome local, expresso como uma cadeia de caracteres.  
  
 O exemplo a seguir cria um documento com um namespace. Por padrão, o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] serializa esse documento com um namespace padrão.  
  
```csharp  
// Create an XML tree in a namespace.  
XNamespace aw = "http://www.adventure-works.com";  
XElement root = new XElement(aw + "Root",  
    new XElement(aw + "Child", "child content")  
);  
Console.WriteLine(root);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root xmlns="http://www.adventure-works.com">  
  <Child>child content</Child>  
</Root>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um documento com um namespace. Também cria um atributo que declara o namespace com um prefixo de namespace. Para criar um atributo que declare um namespace com um prefixo, você cria um atributo onde o namespace do nome do atributo seja o prefixo do namespace, e esse nome esteja no namespace <xref:System.Xml.Linq.XNamespace.Xmlns%2A>. O valor desse atributo é o URI do namespace.  
  
```csharp  
// Create an XML tree in a namespace, with a specified prefix  
XNamespace aw = "http://www.adventure-works.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com"),  
    new XElement(aw + "Child", "child content")  
);  
Console.WriteLine(root);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com">  
  <aw:Child>child content</aw:Child>  
</aw:Root>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra a criação de um documento que contém dois namespaces. Um é o namespace padrão. O outro é um namespace com um prefixo.  
  
 Com a inclusão de atributos de namespace no elemento raiz, os namespaces são serializados, de modo que `http://www.adventure-works.com` seja o namespace padrão e `www.fourthcoffee.com` seja serializado com o prefixo "fc". Para criar um atributo que declare um namespace padrão, você cria um atributo com o nome "xmlns", sem um namespace. O valor do atributo é o URI padrão do namespace.  
  
```csharp  
// The http://www.adventure-works.com namespace is forced to be the default namespace.  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace fc = "www.fourthcoffee.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute("xmlns", "http://www.adventure-works.com"),  
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
<Root xmlns="http://www.adventure-works.com" xmlns:fc="www.fourthcoffee.com">  
  <fc:Child>  
    <DifferentChild>other content</DifferentChild>  
  </fc:Child>  
  <Child2>c2 content</Child2>  
  <fc:Child3>c3 content</fc:Child3>  
</Root>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um documento que contém dois namespaces, ambos com prefixos de namespace.  
  
```csharp  
XNamespace aw = "http://www.adventure-works.com";  
XNamespace fc = "www.fourthcoffee.com";  
XElement root = new XElement(aw + "Root",  
    new XAttribute(XNamespace.Xmlns + "aw", aw.NamespaceName),  
    new XAttribute(XNamespace.Xmlns + "fc", fc.NamespaceName),  
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
  
## <a name="example"></a>Exemplo  
 Outra maneira de obter o mesmo resultado é usar nomes expandidos, em vez de declarar e criar um objeto <xref:System.Xml.Linq.XNamespace>.  
  
 Essa abordagem tem implicações de desempenho. Cada vez que você passa uma cadeia de caracteres que contém um nome expandido para o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] deve analisar o nome, localizar o namespace atomizado e localizar o nome atomizado. Esse processo utiliza tempo de CPU. Se o desempenho for importante, convém declarar e usar um objeto <xref:System.Xml.Linq.XNamespace> explicitamente.  
  
 Se o desempenho for uma questão importante, consulte [Pré-atomização de objetos XName (LINQ to XML) (C#)](./pre-atomization-of-xname-objects-linq-to-xml.md) para obter mais informações  
  
```csharp  
// Create an XML tree in a namespace, with a specified prefix  
XElement root = new XElement("{http://www.adventure-works.com}Root",  
    new XAttribute(XNamespace.Xmlns + "aw", "http://www.adventure-works.com"),  
    new XElement("{http://www.adventure-works.com}Child", "child content")  
);  
Console.WriteLine(root);  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com">  
  <aw:Child>child content</aw:Child>  
</aw:Root>  
```  
  
## <a name="see-also"></a>Confira também

- [Visão geral sobre namespaces (LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md)
