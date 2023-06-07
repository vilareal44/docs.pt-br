---
title: Escopo de namespace padrão em C#
description: Saiba como consultar em namespaces XML padrão em LINQ to XML em C#. Use uma variável XNamespace e o nome local para criar um nome qualificado para a consulta.
ms.date: 07/20/2015
ms.assetid: fe826236-830f-457a-9027-7ad62c909fae
ms.openlocfilehash: 912e47099f89daa9b80ac58b422d39d598509ac9
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87302393"
---
# <a name="scope-of-default-namespaces-in-c"></a>Escopo de namespace padrão em C\#
Namespaces padrões como representadas na árvore XML não estiver no escopo para consultas. Se você tiver XML que é em um namespace padrão, você ainda deve declarar uma variável de <xref:System.Xml.Linq.XNamespace> , e combina-o com o nome local para fazer um nome qualificado para ser usado na consulta.  
  
 Um dos problemas mais comuns para o consulte árvores XML é que se a árvore tem um namespace XML padrão, o desenvolvedor escreve às vezes a consulta como se o XML não estar em um namespace.  
  
 Definir primeiro exemplos neste tópico mostra uma maneira comum que XML em um namespace padrão é carregado, mas é visto de modo inadequado.  
  
 O segundo conjunto de exemplos a seguir mostra as correções necessárias para que você possa ver XML em um namespace.  
  
## <a name="example"></a>Exemplo  
 Este exemplo mostra como criar XML em um namespace, e uma consulta que retorna um conjunto de resultados vazia.  
  
### <a name="code"></a>Código  
  
```csharp  
XElement root = XElement.Parse(  
@"<Root xmlns='http://www.adventure-works.com'>  
    <Child>1</Child>  
    <Child>2</Child>  
    <Child>3</Child>  
    <AnotherChild>4</AnotherChild>  
    <AnotherChild>5</AnotherChild>  
    <AnotherChild>6</AnotherChild>  
</Root>");  
IEnumerable<XElement> c1 =  
    from el in root.Elements("Child")  
    select el;  
Console.WriteLine("Result set follows:");  
foreach (XElement el in c1)  
    Console.WriteLine((int)el);  
Console.WriteLine("End of result set");  
```  
  
### <a name="comments"></a>Comentários  
 Este exemplo gerencia o resultado seguinte:  
  
```output  
Result set follows:  
End of result set  
```  
  
## <a name="example"></a>Exemplo  
 Este exemplo mostra como criar XML em um namespace, e uma consulta que é codificado corretamente.  
  
 Em contraste com incorretamente codificado o exemplo anterior, a abordagem correta para usar C# é declarar e inicializar um objeto de <xref:System.Xml.Linq.XNamespace> , e usá-lo para especificar <xref:System.Xml.Linq.XName> objetos. Nesse caso, o argumento para o método de <xref:System.Xml.Linq.XContainer.Elements%2A> é um objeto de <xref:System.Xml.Linq.XName> .  
  
### <a name="code"></a>Código  
  
```csharp  
XElement root = XElement.Parse(  
@"<Root xmlns='http://www.adventure-works.com'>  
    <Child>1</Child>  
    <Child>2</Child>  
    <Child>3</Child>  
    <AnotherChild>4</AnotherChild>  
    <AnotherChild>5</AnotherChild>  
    <AnotherChild>6</AnotherChild>  
</Root>");  
XNamespace aw = "http://www.adventure-works.com";  
IEnumerable<XElement> c1 =  
    from el in root.Elements(aw + "Child")  
    select el;  
Console.WriteLine("Result set follows:");  
foreach (XElement el in c1)  
    Console.WriteLine((int)el);  
Console.WriteLine("End of result set");  
```  
  
### <a name="comments"></a>Comentários  
 Este exemplo gerencia o resultado seguinte:  
  
```output  
Result set follows:  
1  
2  
3  
End of result set  
```  
  
## <a name="see-also"></a>Veja também

- [Visão geral sobre namespaces (LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md)
