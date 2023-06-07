---
title: 'Como: criar um documento com namespaces (LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: cc5b0d4d-360c-4ada-94fa-2d2916e989be
ms.openlocfilehash: a440c9d810798eb5ebd025a336cbb17fface23b4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414628"
---
# <a name="how-to-create-a-document-with-namespaces-linq-to-xml-visual-basic"></a>Como: Crie um documento com namespaces (LINQ to XML) (Visual Basic)
Este tópico mostra como criar um documento com namespaces no Visual Basic.  
  
 Ao usar literais XML no Visual Basic, os usuários podem definir um namespace XML global padrão. Este namespace é o namespace padrão para literais XML e propriedades XML. O namespace XML padrão pode ser definida no nível de projeto ou nível de arquivo. Se for definida em nível de arquivo, substitui o namespace padrão no nível do projeto.  
  
 Você também pode definir namespaces, e especifica os prefixos de namespace para esses namespaces.  
  
 Você define dois namespaces padrão e namespaces com um prefixo usando a palavra-chave `Imports` .  
  
 Para obter mais informações, consulte [introdução aos literais XML no Visual Basic](introduction-to-xml-literals.md).  
  
 Observe que o namespace XML padrão se aplica somente aos elementos e não a atributos. Atributos são sempre por padrão em qualquer namespace. No entanto, você pode usar um prefixo de namespace para colocar um atributo em um namespace.  
  
## <a name="example"></a>Exemplo  
 Este exemplo cria um documento que contém um namespace.  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
Module Module1  
    Sub Main()  
        Dim root As XElement = _  
            <aw:Root>  
                <aw:Child aw:Att="attvalue"/>  
            </aw:Root>  
        Console.WriteLine(root)  
    End Sub  
End Module  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<aw:Root xmlns:aw="http://www.adventure-works.com">  
  <aw:Child aw:Att="attvalue" />  
</aw:Root>  
```  
  
## <a name="example"></a>Exemplo  
 Este exemplo cria um documento que contém dois namespaces, uma que é o namespace padrão.  
  
```vb  
Imports <xmlns="http://www.adventure-works.com">  
Imports <xmlns:fc="www.fourthcoffee.com">  
  
Module Module1  
  
    Sub Main()  
        Dim root As XElement = _  
            <Root>  
                <Child Att="attvalue"/>  
                <fc:Child2>child2 content</fc:Child2>  
            </Root>  
        Console.WriteLine(root)  
    End Sub  
  
End Module  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<Root xmlns:fc="www.fourthcoffee.com" xmlns="http://www.adventure-works.com">  
  <Child Att="attvalue" />  
  <fc:Child2>child2 content</fc:Child2>  
</Root>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um documento que contém vários namespaces, tanto com prefixos de namespace.  
  
 Quando serializar uma árvore XML, [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] emite-se declarações namespace como necessário para que cada elemento está no namespace designada.  
  
```vb  
Imports <xmlns:aw="http://www.adventure-works.com">  
Imports <xmlns:fc="www.fourthcoffee.com">  
  
Module Module1  
  
    Sub Main()  
        Dim root As XElement = _  
            <aw:Root>  
                <fc:Child>  
                    <aw:DifferentChild>other content</aw:DifferentChild>  
                </fc:Child>  
                <aw:Child2>c2 content</aw:Child2>  
                <fc:Child3>c3 content</fc:Child3>  
            </aw:Root>  
        Console.WriteLine(root)  
    End Sub  
  
End Module  
```  
  
 Esse exemplo gera a saída a seguir:  
  
```xml  
<aw:Root xmlns:fc="www.fourthcoffee.com" xmlns:aw="http://www.adventure-works.com">  
  <fc:Child>  
    <aw:DifferentChild>other content</aw:DifferentChild>  
  </fc:Child>  
  <aw:Child2>c2 content</aw:Child2>  
  <fc:Child3>c3 content</fc:Child3>  
</aw:Root>  
```  
  
## <a name="see-also"></a>Confira também

- [Visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md)
